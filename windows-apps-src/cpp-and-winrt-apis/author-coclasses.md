---
description: C++/WinRT는 Windows 런타임 클래스를 작성하는 데 도움이 되는 것처럼 클래식 COM 구성 요소를 작성하는 데도 도움이 됩니다.
title: C++/WinRT으로 COM 구성 요소 작성
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, standard, c + +, cpp, winrt, 프로젝션, 작성자, COM, 구성 요소
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3badcd59155bc4bb5ef8d9e29271b853c245c24e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360320"
---
# <a name="author-com-components-with-cwinrt"></a>C++/WinRT으로 COM 구성 요소 작성

[C++/ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Windows 런타임 클래스를 작성 하는 데 도움이 하는 것 처럼 클래식 COM 구성 요소 개체 모델 () 구성 요소 (또는 coclass)를 작성 하는 데 도움이 됩니다. 다음은 코드를 붙여 넣는 경우 테스트할 수 있는 간단한 보여 줍니다는 `pch.h` 하 고 `main.cpp` 새 **Windows 콘솔 응용 프로그램 (C++/WinRT)** 프로젝트.

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

struct __declspec(uuid("ddc36e02-18ac-47c4-ae17-d420eece2281")) IMyComInterface : ::IUnknown
{
    virtual HRESULT __stdcall Call() = 0;
};

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();

    struct MyCoclass : winrt::implements<MyCoclass, IPersist, IStringable, IMyComInterface>
    {
        HRESULT __stdcall Call() noexcept override
        {
            return S_OK;
        }

        HRESULT __stdcall GetClassID(CLSID* id) noexcept override
        {
            *id = IID_IPersist; // Doesn't matter what we return, for this example.
            return S_OK;
        }

        winrt::hstring ToString()
        {
            return L"MyCoclass as a string";
        }
    };

    auto mycoclass_instance{ winrt::make<MyCoclass>() };
    CLSID id{};
    winrt::check_hresult(mycoclass_instance->GetClassID(&id));
    winrt::check_hresult(mycoclass_instance.as<IMyComInterface>()->Call());
}
```

도 참조 하세요 [사용 하 여 사용 하는 COM 구성 요소 C++/WinRT](consume-com.md)합니다.

## <a name="a-more-realistic-and-interesting-example"></a>더 현실적이 고 흥미로운 예제

이 항목의 나머지 부분에서는 C +를 사용 하는 최소한의 콘솔 응용 프로그램 프로젝트를 만드는 방법을 안내 + WinRT 클래스 팩터리를 기본 coclass (COM 구성 요소 또는 COM 클래스)를 구현 합니다. 예제 응용 프로그램에서 콜백 단추와 coclass 알림 메시지를 전달 하는 방법을 보여 줍니다 (구현 하는 **INotificationActivationCallback** COM 인터페이스)는 응용 프로그램이 시작 되 고 호출 허용 사용자가 알림에서 해당 단추를 클릭 하면 백업 합니다.

토스트 알림 기능 영역에 대 한 자세한 배경 정보를 찾을 수 있습니다 [로컬 알림 메시지를 보낼](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)합니다. 설명서의이 섹션의 코드 예제 중 C++/WinRT, 그러나 따라서 좋습니다이 항목에 표시 된 코드를 사용 하 합니다.

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Windows 콘솔 응용 프로그램 프로젝트 (ToastAndCallback) 만들기

먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. 만들기는 **Windows 콘솔 응용 프로그램 (C++/WinRT)** 프로젝트를 만들고 이름을 *ToastAndCallback*합니다.

열기 `pch.h`를 추가한 `#include <unknwn.h>` 하기 전에에 대 한 포함 되어 있습니다 C++/WinRT 헤더입니다. 다음은 결과가 됩니다. 콘텐츠를 바꿀 수 있습니다 프로그램 `pch.h` 이 목록을 사용 하 여 합니다.

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

열기 `main.cpp`, 및를 사용 하 여 지시문 프로젝트 템플릿을 생성 하는 제거 합니다. 그 자리에 다음 코드 (라이브러리, 헤더 및 해야 하는 형식 이름 제공)를 삽입 합니다. 다음은 결과가 됩니다. 콘텐츠를 바꿀 수 있습니다 하 `main.cpp` 이 목록을 사용 하 여 (에서 코드를 제거 했습니다 `main` 아래 목록에서에서는에서는 대체 함수를 나중에 있으므로).

```cppwinrt
// main.cpp : Defines the entry point for the console application.

#include "pch.h"

#pragma comment(lib, "advapi32")
#pragma comment(lib, "ole32")
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

int main() { }
```

프로젝트는 아직 빌드하지 않습니다. 코드 추가 완료 했습니다 후 빌드 및 실행을 묻는 메시지가 나타납니다.

## <a name="implement-the-coclass-and-class-factory"></a>Coclass 및 클래스 팩터리를 구현 합니다.

C++WinRT, 구현, coclass 및 클래스 팩터리에서 파생 하 여/합니다 [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) 기본 구조체입니다. 세 가지를 사용 하 여-지시문은 위에 표시 된 직후 (전과 `main`), 토스트 알림 COM 활성기 구성 요소를 구현 하는이 코드를 붙여 넣습니다.

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

위의 coclass의 구현에 설명 된 동일한 패턴을 따릅니다 [사용 하 여 만든 Api C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class)합니다. 따라서 Windows 런타임 인터페이스 뿐만 아니라 COM 인터페이스를 구현 하는 동일한 기법을 사용할 수 있습니다. COM 구성 요소 및 Windows 런타임 클래스 인터페이스를 통해 해당 기능을 노출합니다. 모든 COM 인터페이스에서 궁극적으로 파생 되는 [ **IUnknown 인터페이스** ](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown) 인터페이스입니다. COM 기반 Windows 런타임&mdash;하나의 구분 되는 궁극적으로 Windows 런타임 인터페이스에서 파생 된 [ **IInspectable 인터페이스** ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (및 **IInspectable**  에서 파생 되 **IUnknown**).

구현에서는 위의 코드에서 coclass를는 **INotificationActivationCallback::Activate** 메서드는 알림 메시지 통지 콜백 단추를 클릭할 때 호출 되는 함수입니다. 하지만 해당 함수를 호출할 수 있습니다, 전에 coclass 인스턴스를 만들 수 해야 하는 경우의 작업이 그 해답이 며 합니다 **IClassFactory::CreateInstance** 함수입니다.

만 구현 하는 coclass 라고 합니다 *COM 활성기* 에 대 한 알림 및 해당 컨트롤의 클래스 id (CLSID) 형식의 합니다 `callback_guid` 식별자 (형식의 **GUID**) 위에 표시 되는. 사용 해당 식별자 나중에 시작 메뉴 바로 가기 및 Windows 레지스트리 항목의 형식에서입니다. COM 활성기 CLSID, 및 경로를 해당 연결 된 COM 서버 (즉, 여기를 빌드하는 하는 실행 파일 경로)는 알림 메시지는 해당 콜백 단추를 클릭할 때의 인스턴스를 만드는 클래스 무엇을 알고 메커니즘 (여부는 알림을 클릭할 관리 센터에서 여부).

## <a name="best-practices-for-implementing-com-methods"></a>COM 메서드를 구현 하기 위한 모범 사례

리소스 관리 및 오류 처리에 대 한 기술 관련 직접 이동할 수 있습니다. 것이 더 편리 하 고 오류 코드 보다 예외를 사용 하는 데 적합 합니다. 그리고 리소스 취득-는-초기화 (RAII) 관용구를 사용 하면 다음 오류 코드에 대 한 명시적으로 확인 하 고 리소스를 명시적으로 해제 합니다. 명시적 검사가 필요한 경우 보다 더 복잡 하 여 코드를 확인 하 고 다양 한 위치를 숨기려면 버그를 제공. 대신, RAII을 throw/catch 예외입니다. 이렇게 하면 리소스 할당은 예외 로부터 안전한 및 코드는 간단 합니다.

그러나 COM 메서드 구현을 이스케이프 하는 예외를 허용할 돼 있습니다. 사용 하 여 되도록 하는 `noexcept` 에 COM 메서드에 지정자입니다. 메서드 종료 되기 전에 처리 하는 만큼 메서드의 호출 그래프에서 아무 곳 이나 throw 되는 예외에 대 한 해도 됩니다. 사용 하는 경우 `noexcept`, 않지만 끼운 예외가 메서드를 이스케이프 하 고 응용 프로그램이 종료 됩니다.

## <a name="add-helper-types-and-functions"></a>도우미 형식 및 함수를 추가 합니다.

이 단계에서는 코드의 나머지는 몇 가지 도우미 형식 및 함수 사용 추가 합니다. 따라서 직전 `main`, 다음을 추가 합니다.

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

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>나머지 함수 및 wmain 진입점 함수를 구현 합니다.

삭제에 `main` 함수를 해당 위치에이 코드를 붙여넣은 다음이 목록에 coclass를 등록 하는 코드를 포함 하는 응용 프로그램을 다시 호출할 수 있는 알림을 배달 하는 고 합니다.

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
    ::Sleep(50); // Give the callback chance to display.
}

void LaunchedNormally(HANDLE, INPUT_RECORD &, DWORD &);
void LaunchedFromNotification(HANDLE, INPUT_RECORD &, DWORD &);

int wmain(int argc, wchar_t * argv[], wchar_t * /* envp */[])
{
    winrt::init_apartment();

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
        std::wcout << this_app_name << L" is running" << (runningAsAdmin ? L" (administrator)." : L" (NOT as administrator).") << std::endl;

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

## <a name="how-to-test-the-example-application"></a>예제 응용 프로그램을 테스트 하는 방법

응용 프로그램을 빌드하고, 등록 및 다른 설치 프로그램을 실행 하는 코드를 관리자 권한으로 최소 한 번 실행 합니다. 작업을 수행 하는 한 가지 방법은 관리자로 서 Visual Studio를 실행 한 후 Visual Studio에서 앱을 실행 하는 경우 작업 표시줄 점프 목록 표시, 점프 목록에서 Visual Studio를 마우스 오른쪽 단추로 클릭 하 고 클릭을에서 Visual Studio를 마우스 오른쪽 단추로 클릭 **관리자 권한으로 실행**합니다. 프롬프트에 동의 하 고 프로젝트를 엽니다. 응용 프로그램을 실행 하는 경우 응용 프로그램을 관리자 권한으로 실행 되 고 있는지 여부를 나타내는 메시지가 표시 됩니다. 그렇지 않으면 등록 및 다른 설치가 실행 되지 않습니다. 해당 등록 및 기타 설치 응용 프로그램이 올바르게 작동 하기 위해 한 번 이상 실행 해야 합니다.

표시할지, 아니면 관리자 권한으로 응용 프로그램을 실행 하 고, 키를 눌러 ' T '를 표시할 알림. 클릭할 수 있습니다 합니다 **ToastAndCallback 콜백할** 단추에 표시 되는 알림 메시지에서 직접 또는 관리 센터 및 응용 프로그램에서 시작 됩니다, 인스턴스화된 coclass 및  **INotificationActivationCallback::Activate** 메서드를 실행 합니다.

## <a name="in-process-com-server"></a>In-process COM 서버

합니다 *ToastAndCallback* 위 예제 앱에 로컬 같거나 out-of-process COM 서버로 작동 합니다. 이 표시 됩니다는 [LocalServer32](/windows/desktop/com/localserver32) Windows 레지스트리 키를 사용 하 여 해당 coclass의 CLSID를 등록 합니다. 로컬 COM 서버를 호스팅하는 실행 가능 바이너리 내에서 해당 coclass(es) (프로그램 `.exe`).

또는 (및 가능성이 아마도), 동적 연결 라이브러리 내부에 coclass(es)를 호스트 하도록 선택할 수 있습니다 (한 `.dll`). DLL 형식의 COM 서버를 in-process COM 서버 이라고 하며 사용 하 여 등록할 Clsid로 표시 됩니다는 [InprocServer32](/windows/desktop/com/inprocserver32) Windows 레지스트리 키입니다.

### <a name="create-a-dynamic-link-library-dll-project"></a>동적 연결 라이브러리 (DLL) 프로젝트 만들기

Microsoft Visual Studio에서 새 프로젝트를 만들어는 in-process COM 서버를 만드는 작업을 시작할 수 있습니다. 만들기는 **시각적 C++**   >  **Windows 바탕 화면** > **동적 연결 라이브러리 (DLL)** 프로젝트입니다.

추가할 C++에 설명 된 단계를 수행 하는 새 프로젝트에 WinRT 지원/ [추가할 Windows 데스크톱 응용 프로그램 프로젝트를 수정 C++/WinRT 지원](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support)합니다.

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>Coclass, 클래스 팩터리 및 프로시저에서 서버 내보내기 구현

열기 `dllmain.cpp`, 아래에 표시 된 코드를 추가 합니다.

C +를 구현 하는 DLL이 이미 있는 경우 + WinRT Windows 런타임 클래스를 이미 해야 합니다 **DllCanUnloadNow** 아래 표시 된 함수입니다. 해당 DLL에 대 한 coclass를 추가 하려는 경우 추가할 수 있습니다 합니다 **DllGetClassObject** 함수입니다.

하는 경우 기존 없는 [Windows 런타임 C++ 템플릿 라이브러리 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 보여드린 코드의 부분 코드는 호환을 유지 하려는 다음 WRL를 제거할 수 있습니다.

```cppwinrt
// dllmain.cpp

struct MyCoclass : winrt::implements<MyCoclass, IPersist>
{
    HRESULT STDMETHODCALLTYPE GetClassID(CLSID* id) noexcept override
    {
        *id = IID_IPersist; // Doesn't matter what we return, for this example.
        return S_OK;
    }
};

struct __declspec(uuid("85d6672d-0606-4389-a50a-356ce7bded09"))
    MyCoclassFactory : winrt::implements<MyCoclassFactory, IClassFactory>
{
    HRESULT STDMETHODCALLTYPE CreateInstance(IUnknown *pUnkOuter, REFIID riid, void **ppvObject) noexcept override
    {
        try
        {
            *ppvObject = winrt::make<MyCoclass>().get();
            return S_OK;
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }

    HRESULT STDMETHODCALLTYPE LockServer(BOOL fLock) noexcept override
    {
        // ...
        return S_OK;
    }

    // ...
};

HRESULT __stdcall DllCanUnloadNow()
{
#ifdef _WRL_MODULE_H_
    if (!::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().Terminate())
    {
        return S_FALSE;
    }
#endif

    if (winrt::get_module_lock())
    {
        return S_FALSE;
    }

    winrt::clear_factory_cache();
    return S_OK;
}

HRESULT __stdcall DllGetClassObject(GUID const& clsid, GUID const& iid, void** result)
{
    try
    {
        *result = nullptr;

        if (clsid == __uuidof(MyCoclassFactory))
        {
            return winrt::make<MyCoclassFactory>()->QueryInterface(iid, result);
        }

#ifdef _WRL_MODULE_H_
        return ::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().GetClassObject(clsid, iid, result);
#else
        return winrt::hresult_class_not_available().to_abi();
#endif
    }
    catch (...)
    {
        return winrt::to_hresult();
    }
}
```

### <a name="support-for-weak-references"></a>약한 참조에 대 한 지원

도 참조 하세요 [약한 참조 C++/WinRT](weak-references.md#weak-references-in-cwinrt)합니다.

C++/ WinRT (특히 합니다 [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) 구조체를 기본 템플릿) 구현 [ **IWeakReferenceSource** ](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) 를 경우에 구현 형식 [ **IInspectable** ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (또는에서 파생 되는 모든 인터페이스 **IInspectable**).

왜냐하면 **IWeakReferenceSource** 하 고 [ **IWeakReference** ](/windows/desktop/api/weakreference/nn-weakreference-iweakreference) Windows 런타임 형식에 대 한 설계 되었습니다. 따라서 켤 수 있습니다 약한 참조 지원을 coclass에 대 한 추가 하기만 **winrt::Windows::Foundation::IInspectable** (또는 인터페이스에서 파생 되는 **IInspectable**) 구현 합니다.

```cppwinrt
struct MyCoclass : winrt::implements<MyCoclass, IMyComInterface, winrt::Windows::Foundation::IInspectable>
{
    //  ...
};
```

## <a name="important-apis"></a>중요 API
* [IInspectable 인터페이스](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [IUnknown 인터페이스](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)
* [winrt::implements 구조체 템플릿](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT를 통한 API 작성](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [C++/WinRT를 통한 COM 구성 요소 사용](consume-com.md)
* [로컬 알림 메시지 보내기](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
