---
author: stevewhims
description: C + + WinRT 하는 데 유용한 클래식 COM 구성 요소를 작성 것 처럼 Windows 런타임 클래스를 작성 하는 데 도움이 됩니다.
title: 작성 COM 구성 요소 C + + WinRT
ms.author: stwhi
ms.date: 09/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c + +, cpp, winrt, 프로젝션, 작성, COM, 구성 요소
ms.localizationpriority: medium
ms.openlocfilehash: 729cfae39f302ae6b5bae275d9e28a39f3d9503b
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4126524"
---
# <a name="author-com-components-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>COM 구성 요소를 작성 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

C + + WinRT 하면 Windows 런타임 클래스를 작성 하는 것 처럼 클래식 구성 요소 개체 모델 (COM) 구성 요소 (또는 coclass)를 작성 하는 데 도움이 됩니다. 에 붙여넣을 경우 테스트할 수 있는 매우 간단한 그림은 다음과 같습니다은 `main.cpp` 새 **Windows 콘솔 응용 프로그램 (C + + WinRT)** 프로젝트.

```cppwinrt
// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;

int main()
{
    init_apartment();

    struct MyCoclass : winrt::implements<MyCoclass, IPersist>
    {
        HRESULT STDMETHODCALLTYPE GetClassID(CLSID* id) noexcept override
        {
            *id = IID_IPersist; // Doesn't matter what we return, for this example.
            return S_OK;
        }
    };

    auto mycoclass_instance{ winrt::make<MyCoclass>() };
    CLSID id{};
    winrt::check_hresult(mycoclass_instance->GetClassID(&id));
}
```

참고 [소비 COM 구성 요소 C + + WinRT](consume-com.md).

## <a name="a-more-realistic-and-interesting-example"></a>더 현실적인 하 고 흥미로운 예제

이 항목의 나머지 부분에서는 C + 최소한의 콘솔 응용 프로그램 프로젝트를 만드는 방법을 안내 + 기본 coclass 및 클래스 팩터리를 구현 하는 WinRT 합니다. 예제에서는 응용 프로그램에서는 콜백 단추를 사용 하 여 알림 메시지를 제공 하는 방법을 보여 주며 coclass ( **INotificationActivationCallback** COM 인터페이스를 구현 하는)는 응용 프로그램을 시작 하 라는 때 사용자 알림에 해당 단추를 클릭합니다.

[로컬 알림 메시지 보내기](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)에서 알림 메시지 알림 기능 영역에 대 한 추가적인 배경 정보를 확인할 수 있습니다. None 설명서의이 섹션의 코드 예제에 사용 하 여 C + + /winrt 하지만 하므로 좋습니다이 항목에 표시 된 코드를 원하는 합니다.

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Windows 콘솔 응용 프로그램 프로젝트 (ToastAndCallback) 만들기

먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. **Visual c + +** 만들기 > **Windows 데스크톱** > **Windows 콘솔 응용 프로그램 (C + + WinRT)** 프로젝트를 만들어서 *ToastAndCallback*이름을 지정 합니다.

열기 `main.cpp`, 및 제거를 사용 하 여-지시문 프로젝트 템플릿을 생성 합니다. 그 대신에서 (라이브러리 머리글과 필요 하다는 형식 이름을 구할)는 다음 코드를 붙여 넣습니다.

```cppwinrt
#pragma comment(lib, "shell32")

#include <iomanip>
#include <iostream>
#include <notificationactivationcallback.h>
#include <propkey.h>
#include <propvarutil.h>
#include <shlobj.h>
#include <winrt/Windows.UI.Notifications.h>
#include <winrt/Windows.Data.Xml.Dom.h>

using namespace winrt;
using namespace Windows::Data::Xml::Dom;
using namespace Windows::UI::Notifications;
```

## <a name="implement-the-coclass-and-class-factory"></a>Coclass 및 클래스 팩터리 구현

C + + /winrt에 구현 coclass 및 클래스 팩터리 [**winrt:: implements**](/uwp/cpp-ref-for-winrt/implements) 기본 구조체에서 파생 시켜 합니다. 세 가지를 사용 하 여-지시문 위에 표시 된 직후 (하기 전에 `main`), 알림 COM 알림 활성 자 구성 요소를 구현 하는이 코드를 붙여 넣습니다.

```cppwinrt
static constexpr GUID callback_guid // BAF2FA85-E121-4CC9-A942-CE335B6F917F
{
    0xBAF2FA85, 0xE121, 0x4CC9, {0xA9, 0x42, 0xCE, 0x33, 0x5B, 0x6F, 0x91, 0x7F}
};

std::wstring const this_app_name{ L"ToastAndCallback" };

struct callback : winrt::implements<callback, INotificationActivationCallback>
{
    HRESULT __stdcall Activate(
        LPCWSTR app,
        LPCWSTR args,
        [[maybe_unused]] NOTIFICATION_USER_INPUT_DATA const* data,
        [[maybe_unused]] ULONG count) noexcept final
    {
        try
        {
            std::wcout << this_app_name << L" has been called back from a notification." << std::endl;
            std::wcout << L"Value of the 'app' parameter is '" << app << L"'." << std::endl;
            std::wcout << L"Value of the 'args' parameter is '" << args << L"'." << std::endl;
            return S_OK;
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }
};

struct callback_factory : implements<callback_factory, IClassFactory>
{
    HRESULT __stdcall CreateInstance(
        IUnknown* outer,
        GUID const& iid,
        void** result) noexcept final
    {
        *result = nullptr;

        if (outer)
        {
            return CLASS_E_NOAGGREGATION;
        }

        return make<callback>()->QueryInterface(iid, result);
    }

    HRESULT __stdcall LockServer(BOOL) noexcept final
    {
        return S_OK;
    }
};
```

위의 coclass의 구현에 설명 된 동일한 패턴을 따르는 [작성자 Api C + + WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class). 이 기술은 Windows 런타임 인터페이스 (모든 인터페이스 궁극적으로 [**IInspectable**](https://msdn.microsoft.com/library/br205821)에서 파생 되는)에 대 한 하지만 COM 인터페이스 (궁극적으로 [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)에서 파생 되는 모든 인터페이스)를 구현도 사용할 수 있는 확인 합니다.

위의 코드에서 coclass에서 알림 메시지에서 콜백 단추를 클릭할 때 호출 되는 함수는 **INotificationActivationCallback::Activate** 메서드를 구현 합니다. 하지만 그 함수를 호출할 수 coclass의 인스턴스를 만들 수 있어야 하 고 **IClassFactory::CreateInstance** 함수는 작업입니다.

구현 하는 coclass 알림에 대 한 *COM 활성 자* 라고 하며 해당 클래스 id (CLSID)의 형태로 합니다 `callback_guid` 위에 표시 하는 식별자 ( **GUID**형식)입니다. 사용할 식별자에 나중에 시작 메뉴 바로 가기와 Windows 레지스트리 항목의 숫자 형태로 합니다. COM 활성 자 CLSID 및 경로 (즉, 여기서를 구축 하 고 실행 파일에 대 한 경로) 연결된 된 COM 서버를 사용할 경우 알림 기울기 콜백 단추를 클릭할 때의 인스턴스를 만드는 클래스 무엇을 알고 메커니즘 (여부는 알림을 클릭할 알림 센터에서 여부).

## <a name="best-practices-for-implementing-com-methods"></a>COM 메서드를 구현 하기 위한 모범 사례

오류 처리 및 리소스 관리에 대 한 기술 손에서 직접 이동할 수 있습니다. 것 보다 편리 하 고 오류 코드 보다는 예외를 사용 하기에 적합 합니다. 및 리소스 취득-는-초기화 (RAII) 방법을 사용 하면 다음 않아도 명시적으로 오류 코드를 확인 하 고 리소스를 명시적으로 해제 합니다. 이러한 명시적 검사 필요한 경우 보다 더 난해해 코드 하 고 다양 한 위치를 숨기려면 버그를 제공 합니다. 대신, RAII, 사용 및 예외를 throw/catch 합니다. 이렇게 하면 리소스 할당은 예외 로부터 안전 하 고 코드는 간단 합니다.

그러나 이스케이프 COM 메서드 구현에 대 한 예외를 허용할 돼 있습니다. 사용 하 여를 확인 하는 합니다 `noexcept` COM 메서드에서 지정자 합니다. 메서드에 끝나기 전에 처리 하는 있기만 것 메서드를 호출 그래프의 아무 곳 이나 예외가 예외에 대 한 확인 합니다. 사용 하는 경우 `noexcept`, 하지만 다음 메서드를 이스케이프 하 예외를 허용 하 고 응용 프로그램을 종료 합니다.

## <a name="add-helper-types-and-functions"></a>도우미 형식과 함수를 추가 합니다.

이 단계에서의 나머지 코드는 몇 가지 도우미 형식 및 함수 사용 추가 하겠습니다. 따라서 하기 전에 `main`, 다음 코드를 추가 합니다.

```cppwinrt
struct prop_variant : PROPVARIANT
{
    prop_variant() noexcept : PROPVARIANT{}
    {
    }

    ~prop_variant() noexcept
    {
        clear();
    }

    void clear() noexcept
    {
        WINRT_VERIFY_(S_OK, ::PropVariantClear(this));
    }
};

struct registry_traits
{
    using type = HKEY;

    static void close(type value) noexcept
    {
        WINRT_VERIFY_(ERROR_SUCCESS, ::RegCloseKey(value));
    }

    static constexpr type invalid() noexcept
    {
        return nullptr;
    }
};

using registry_key = winrt::handle_type<registry_traits>;

std::wstring get_module_path()
{
    std::wstring path(100, L'?');
    uint32_t path_size{};
    DWORD actual_size{};

    do
    {
        path_size = static_cast<uint32_t>(path.size());
        actual_size = ::GetModuleFileName(nullptr, path.data(), path_size);

        if (actual_size + 1 > path_size)
        {
            path.resize(path_size * 2, L'?');
        }
    } while (actual_size + 1 > path_size);

    path.resize(actual_size);
    return path;
}

std::wstring get_shortcut_path()
{
    std::wstring format{ LR"(%ProgramData%\Microsoft\Windows\Start Menu\Programs\)" };
    format += (this_app_name + L".lnk");

    auto required{ ::ExpandEnvironmentStrings(format.c_str(), nullptr, 0) };
    std::wstring path(required - 1, L'?');
    ::ExpandEnvironmentStrings(format.c_str(), path.data(), required);
    return path;
}
```

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>나머지 함수 및 진입점 wmain 함수를 구현 합니다.

프로젝트 템플릿은 생성 한 `main` 함수를 합니다. 삭제 하는 `main` 함수를 찾아서 제자리에 coclass에 등록 하는 코드를 포함 하는 목록에이 코드를 붙여 차례로 응용 프로그램을 호출할 수 있는 알림 제공 합니다.

```cppwinrt
void register_callback()
{
    DWORD registration{};

    winrt::check_hresult(::CoRegisterClassObject(
        callback_guid,
        make<callback_factory>().get(),
        CLSCTX_LOCAL_SERVER,
        REGCLS_SINGLEUSE,
        &registration));
}

void create_shortcut()
{
    auto link{ winrt::create_instance<IShellLink>(CLSID_ShellLink) };
    std::wstring module_path{ get_module_path() };
    winrt::check_hresult(link->SetPath(module_path.c_str()));

    auto store = link.as<IPropertyStore>();
    prop_variant value;
    winrt::check_hresult(::InitPropVariantFromString(this_app_name.c_str(), &value));
    winrt::check_hresult(store->SetValue(PKEY_AppUserModel_ID, value));
    value.clear();
    winrt::check_hresult(::InitPropVariantFromCLSID(callback_guid, &value));
    winrt::check_hresult(store->SetValue(PKEY_AppUserModel_ToastActivatorCLSID, value));

    auto file{ store.as<IPersistFile>() };
    std::wstring shortcut_path{ get_shortcut_path() };
    winrt::check_hresult(file->Save(shortcut_path.c_str(), TRUE));

    std::wcout << L"In " << shortcut_path << L", created a shortcut to " << module_path << std::endl;
}

void update_registry()
{
    std::wstring key_path{ LR"(SOFTWARE\Classes\CLSID\{????????-????-????-????-????????????})" };
    ::StringFromGUID2(callback_guid, key_path.data() + 23, 39);
    key_path += LR"(\LocalServer32)";
    registry_key key;

    winrt::check_win32(::RegCreateKeyEx(
        HKEY_CURRENT_USER,
        key_path.c_str(),
        0,
        nullptr,
        0,
        KEY_WRITE,
        nullptr,
        key.put(),
        nullptr));
    ::RegDeleteValue(key.get(), nullptr);

    std::wstring path{ get_module_path() };

    winrt::check_win32(::RegSetValueEx(
        key.get(),
        nullptr,
        0,
        REG_SZ,
        reinterpret_cast<BYTE const*>(path.c_str()),
        static_cast<uint32_t>((path.size() + 1) * sizeof(wchar_t))));

    std::wcout << L"In " << key_path << L", registered local server at " << path << std::endl;
}

void create_toast()
{
    XmlDocument xml;

    std::wstring toastPayload
    {
        LR"(
<toast>
  <visual>
    <binding template='ToastGeneric'>
      <text>)"
    };
    toastPayload += this_app_name;
    toastPayload += LR"(
      </text>
    </binding>
  </visual>
  <actions>
    <action content='Call back )";
    toastPayload += this_app_name;
    toastPayload += LR"(
' arguments='the_args' activationKind='Foreground' />
  </actions>
</toast>)";
    xml.LoadXml(toastPayload);

    ToastNotification toast{ xml };
    ToastNotifier notifier{ ToastNotificationManager::CreateToastNotifier(this_app_name) };
    notifier.Show(toast);
}

void LaunchedNormally(HANDLE, INPUT_RECORD &, DWORD &);
void LaunchedFromNotification(HANDLE, INPUT_RECORD &, DWORD &);

int wmain(int argc, wchar_t * argv[], wchar_t * /* envp */[])
{
    init_apartment();

    register_callback();

    HANDLE consoleHandle{ ::GetStdHandle(STD_INPUT_HANDLE) };
    INPUT_RECORD buffer{};
    DWORD events{};
    ::FlushConsoleInputBuffer(consoleHandle);

    if (argc == 1)
    {
        LaunchedNormally(consoleHandle, buffer, events);
    }
    else if (argc == 2 && wcscmp(argv[1], L"-Embedding") == 0)
    {
        LaunchedFromNotification(consoleHandle, buffer, events);
    }
}

void LaunchedNormally(HANDLE consoleHandle, INPUT_RECORD & buffer, DWORD & events)
{
    try
    {
        bool runningAsAdmin{ ::IsUserAnAdmin() == TRUE };
        std::wcout << this_app_name << L" is running" << (runningAsAdmin ? L" (Administrator)." : L".") << std::endl;

        if (runningAsAdmin)
        {
            create_shortcut();
            update_registry();
        }

        std::wcout << std::endl << L"Press 'T' to display a toast notification (press any other key to exit)." << std::endl;

        ::ReadConsoleInput(consoleHandle, &buffer, 1, &events);
        if (towupper(buffer.Event.KeyEvent.uChar.UnicodeChar) == L'T')
        {
            create_toast();
        }
    }
    catch (winrt::hresult_error const& e)
    {
        std::wcout << L"Error: " << e.message().c_str() << L" (" << std::hex << std::showbase << std::setw(8) << static_cast<uint32_t>(e.code()) << L")" << std::endl;
    }
}

void LaunchedFromNotification(HANDLE consoleHandle, INPUT_RECORD & buffer, DWORD & events)
{
    ::Sleep(50); // Give the callback chance to display its message.
    std::wcout << std::endl << L"Press any key to exit." << std::endl;
    ::ReadConsoleInput(consoleHandle, &buffer, 1, &events);
}
```

## <a name="how-to-test-the-example-application"></a>예제에서는 응용 프로그램을 테스트 하는 방법

응용 프로그램을 빌드하고 등록, 및 기타 설정, 코드를 실행 하려면 관리자 권한으로 한 번 이상 실행 합니다. 관리자 권한으로 실행 중인 다음 ' T 키를 눌러 여부 ' 알림 표시 되도록 합니다. Pop 위쪽 또는 알림 센터와 응용 프로그램에서 실행 알림, 인스턴스화된 coclass 및 INotificationActivationCallback **에서 직접 **ToastAndCallback를 호출할** 단추를 클릭 수 있습니다. :: 활성화** 메서드가 실행 됩니다.

## <a name="important-apis"></a>중요 API
* [IInspectable 인터페이스](https://msdn.microsoft.com/library/br205821)
* [IUnknown 인터페이스](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [winrt::implements 구조체 템플릿](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT를 통한 API 작성](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [소비 COM 구성 요소 C + + WinRT](consume-com.md)
* [로컬 알림 메시지 보내기](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
