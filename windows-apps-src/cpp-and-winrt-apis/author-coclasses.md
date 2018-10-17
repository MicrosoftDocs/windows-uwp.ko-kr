---
author: stevewhims
description: C + + WinRT 하는 데 유용한 클래식 COM 구성 요소를 작성 하면 Windows 런타임 클래스를 작성 하는 것 처럼 합니다.
title: C++/WinRT으로 COM 구성 요소 작성
ms.author: stwhi
ms.date: 09/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c + +, cpp, winrt, 프로젝션, 작성, COM, 구성 요소
ms.localizationpriority: medium
ms.openlocfilehash: 94f59833f4c657445b7135b1158974d8a553813f
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4747064"
---
# <a name="author-com-components-with-cwinrt"></a>C++/WinRT으로 COM 구성 요소 작성

[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 하면 Windows 런타임 클래스를 작성 하는 것 처럼 클래식 구성 요소 개체 모델 (COM) 구성 요소 (또는 coclass)를 작성 하는 데 도움이 됩니다. 다음은 코드를 붙여 넣는 경우 테스트할 수 있는 간단한 일러스트레이션, 합니다 `pch.h` 및 `main.cpp` 의 새 **Windows 콘솔 응용 프로그램 (C + + WinRT)** 프로젝트.

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

참고 [소비 COM 구성 요소 C + + WinRT](consume-com.md).

## <a name="a-more-realistic-and-interesting-example"></a>더 현실적인 하 고 흥미로운 예제

이 항목의 나머지 부분에서는 C + 최소한의 콘솔 응용 프로그램 프로젝트를 만드는 방법을 안내 + 클래스 공장 기본 coclass (COM 구성 요소 또는 COM 클래스)을 구현 하는 WinRT 합니다. 예제에서는 응용 프로그램에서는 콜백 단추를 사용 하 여 알림 메시지를 제공 하는 방법을 보여 주며 coclass ( **INotificationActivationCallback** COM 인터페이스를 구현)는 응용 프로그램을 시작 하 라는 때 사용자 알림에 해당 단추를 클릭합니다.

[로컬 알림 메시지 보내기](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)에서 알림 알림 기능 영역에 대 한 추가적인 배경 정보를 확인할 수 있습니다. 설명서의이 섹션의 코드 예제 none 사용 하 여 C + + /winrt에서는 하지만 하므로 권장이 항목에 표시 된 코드를 원하는 합니다.

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Windows 콘솔 응용 프로그램 프로젝트 (ToastAndCallback) 만들기

먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. **Visual c + +** 만들기 > **Windows 데스크톱** > **Windows 콘솔 응용 프로그램 (C + + WinRT)** 프로젝트를 만들어서 이름을 *ToastAndCallback*합니다.

열기 `pch.h`를 추가 `#include <unknwn.h>` 하기 전에 포함 된 C + + /winrt 헤더.

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

열기 `main.cpp`를 사용 하 여-지시문 프로젝트 템플릿을 생성 하는 제거 합니다. 그 대신에서 (라이브러리, 머리글 및 필요 하다는 형식 이름을 구할)는 다음 코드를 붙여 넣습니다.

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

C + + /winrt에 구현 coclass 및 클래스 팩터리 [**winrt:: implements**](/uwp/cpp-ref-for-winrt/implements) 기본 구조체에서 파생 시켜 합니다. 세 가지를 사용 하 여-지시문 위에 표시 된 직후 (및 하기 전에 `main`), 알림 COM 알림 활성 자 구성 요소를 구현 하는이 코드를 붙여 넣습니다.

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

위의 coclass의 구현에 설명 된 동일한 패턴을 따르는 [작성자 Api C + + WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class). 따라서 Windows 런타임 인터페이스 뿐 아니라 COM 인터페이스를 구현 하려면 같은 기술을 사용할 수 있습니다. COM 구성 요소와 Windows 런타임 클래스 인터페이스를 통해 해당 기능을 노출합니다. 모든 COM 인터페이스는 궁극적으로 [**IUnknown 인터페이스**](https://msdn.microsoft.com/library/windows/desktop/ms680509) 인터페이스에서 파생 됩니다. Windows 런타임은 COM 기반&mdash;중 하나를 구별 하는 Windows 런타임 인터페이스는 궁극적으로 [**IInspectable 인터페이스**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) 에서 파생 ( **IInspectable** **IUnknown**에서 파생).

위의 코드에서 coclass에서 알림 메시지에서 콜백 단추를 클릭할 때 호출 되는 함수인 **INotificationActivationCallback::Activate** 메서드를 구현 합니다. 하지만 해당 함수를 호출할 수, coclass의 인스턴스를 작성 해야 하 고 **IClassFactory::CreateInstance** 함수는 작업입니다.

구현 하는 coclass 알림에 대 한 *COM 활성 자* 라고 하며 해당 클래스 id (CLSID)의 형태로 합니다 `callback_guid` 위에 표시 되는 식별자 ( **GUID**형식의). 사용할 것 해당 식별자 나중에 시작 메뉴 바로 가기 및 Windows 레지스트리 항목의 형식에서입니다. COM 활성 자 CLSID 및 연결된 된 COM 서버 (즉, 여기에 빌드하는 것을 실행 파일의 경로)의 경로 알림 기울기 해당 콜백 단추를 클릭할 때의 인스턴스를 만드는 클래스 무엇을 알고 메커니즘 (여부는 알림을 클릭할 알림 센터에서 여부).

## <a name="best-practices-for-implementing-com-methods"></a>COM 메서드를 구현 하기 위한 모범 사례

오류 처리 및 리소스 관리에 대 한 기술 손에서 직접 이동할 수 있습니다. 것 보다 편리 하 고 오류 코드 보다는 예외를 사용 하기에 적합 합니다. 및 리소스 취득-는-초기화 (RAII) 방법을 사용 하면 다음 방지할 수 있습니다 오류 코드에 대 한 명시적으로 확인 하 고 리소스를 명시적으로 해제 합니다. 이러한 명시적 검사 필요한 경우 보다 더 난해해 코드를 확인 하 고 다양 한 위치를 숨기려면 버그 제공. 대신 RAII를 사용 하 고 예외를 throw/catch 합니다. 이런 식으로 리소스 할당은 예외 로부터 안전 하 고 코드는 간단 합니다.

그러나 이스케이프 COM 메서드 구현에 대 한 예외를 허용할 돼 있습니다. 사용 하 여 확인 수는 `noexcept` COM 메서드에서 지정자 합니다. 메서드에 끝나기 전에 처리 하는 있기만 것이 메서드를 호출 그래프의 아무 곳 이나 예외가 예외에 대 한 확인 합니다. 사용 하는 경우 `noexcept`, 하지만 다음 메서드를 이스케이프 하 예외를 허용 하면 다음 응용 프로그램을 종료 합니다.

## <a name="add-helper-types-and-functions"></a>도우미 형식과 함수를 추가 합니다.

이 단계에서는 코드의 나머지 부분에는 몇 가지 도우미 형식 및 함수를 사용 추가 하겠습니다. 그렇다면 하기 전에 `main`, 다음 추가 합니다.

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

프로젝트 템플릿은 생성 한 `main` 함수를 합니다. 삭제 하는 `main` 함수를 찾아서 제자리에 coclass에 등록 하는 코드를 포함 하는 목록에이 코드를 붙여 넣습니다. 알림 응용 프로그램을 다시 호출 수를 제공 하는 차례로 합니다.

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

응용 프로그램을 빌드하고 등록 및 기타 설정, 코드를 실행 하려면 관리자 권한으로 한 번 이상 실행 합니다. 관리자 권한으로 실행 중인 다음 ' T 키를 눌러 여부 ' 알림 표시 되도록 합니다. Pop 위쪽 또는 알림 센터와 응용 프로그램에서이 실행 될 알림, 인스턴스화된 coclass 및 INotificationActivationCallback **에서 직접 **ToastAndCallback 다시 호출** 단추를 클릭 한 다음 수 있습니다. :: 활성화** 메서드가 실행 됩니다.

## <a name="in-process-com-server"></a>In-process COM 서버

위의 *ToastAndCallback* 예제 앱 로컬 (또는 out of process) COM 서버로 작동합니다. 해당 coclass의 CLSID를 등록 하는 데 사용할 수 있는 [LocalServer32](/windows/desktop/com/localserver32) Windows 레지스트리 키에 의해 표시 됩니다. 로컬 COM 서버를 호스팅하는 실행 가능 이진 파일 내에서 해당 coclass(es) (프로그램 `.exe`).

또는 (및 틀림 없이 가능성이), 동적 연결 라이브러리 내에 coclass(es) 호스트 하도록 선택할 수 있습니다 (한 `.dll`). DLL의 숫자 형태로 COM 서버는 in-process COM 서버 라고 하며 Clsid [InprocServer32](/windows/desktop/com/inprocserver32) Windows 레지스트리 키를 사용 하 여 등록 하 여 표시 됩니다.

### <a name="create-a-dynamic-link-library-dll-project"></a>동적 연결 라이브러리 (DLL) 프로젝트 만들기

Microsoft Visual Studio에서 새 프로젝트를 만들어서는 in-process COM 서버를 만드는 작업을 시작할 수 있습니다. **Visual c + +** 만들기 > **Windows 데스크톱** > **동적 연결 라이브러리 (DLL)** 프로젝트.

### <a name="set-project-properties"></a>프로젝트 속성 설정

프로젝트 속성 **일반**로 이동 \> **Windows SDK 버전**및 선택 **모든 구성** 및 **모든 플랫폼**입니다. **Windows SDK 버전** 을 *10.0.17134.0 (Windows 10, 버전 1803)* 이상 설정 합니다.

C +에 대 한 Visual Studio 지원을 추가 하 + /winrt 프로젝트를 편집 하 `.vcxproj` 파일을 찾고 `<PropertyGroup Label="Globals">` 해당 속성 그룹 내에서 속성을 설정 및 `<CppWinRTEnabled>true</CppWinRTEnabled>`합니다.

때문에 C + + WinRT는 c++17 표준의 기능을 사용, **C/c + +** 프로젝트 속성을 설정 > **언어** > **c + + 언어 표준** *ISO c++17 표준 (/ (/std:c++17 + + 17)*.

### <a name="the-precompiled-header"></a>미리 컴파일된 헤더

이름 바꾸기에 `stdafx.h` 및 `stdafx.cpp` 를 `pch.h` 및 `pch.cpp`각각 합니다. **C/c + +** 프로젝트 속성을 설정 > **미리 컴파일된 헤더** >  *pch.h*에**미리 컴파일된 헤더 파일** .

모든 replace `#include "stdafx.h"` 와 `#include "pch.h"`.

`pch.h`을 포함 `winrt/base.h`.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

사용자는 영향을 받지 않도록 확인 [새 프로젝트 컴파일되지 않습니다 이유?](/windows/uwp/cpp-and-winrt-apis/faq)합니다.

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>Coclass, 클래스 팩터리 및 프로세서에서 서버 내보내기를 구현합니다

열기 `dllmain.cpp`, 아래 표시 된 코드를 추가 합니다.

C + 구현 하는 DLL 이미 있는 경우 + WinRT Windows 런타임 클래스, 아래 표시 된 **DllCanUnloadNow** 함수를 이미 해야 합니다. 해당 DLL에 coclass 추가 하려는 경우 **DllGetClassObject** 함수를 추가할 수 있습니다.

경우 호환을 유지 하고자 하는 기존 [Windows 런타임 c + + 템플릿 라이브러리 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 코드가 없는 다음 표시 된 코드에서 WRL 부분을 제거할 수 있습니다.

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

참고 [약한 참조 C + + WinRT](weak-references.md#weak-references-in-cwinrt).

C + + 형식 [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (또는 **IInspectable**에서 파생 되는 모든 인터페이스)를 구현 하는 경우 WinRT (특히 [**winrt:: implements**](/uwp/cpp-ref-for-winrt/implements) 기본 구조체 템플릿인) 한 [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) 을 구현 합니다.

Windows 런타임 형식에 대 한 **IWeakReferenceSource** 및 [**IWeakReference**](/windows/desktop/api/weakreference/nn-weakreference-iweakreference) 되도록 설계 되기 때문입니다. 따라서 켤 수 있습니다 약한 참조 지원은 coclass에 대 한 구현에 **winrt::Windows::Foundation::IInspectable** (또는 **IInspectable**에서 파생 되는 인터페이스)를 추가 하 여 간단 하 게 합니다.

```cppwinrt
struct MyCoclass : winrt::implements<MyCoclass, IMyComInterface, winrt::Windows::Foundation::IInspectable>
{
    //  ...
};
```

## <a name="important-apis"></a>중요 API
* [IInspectable 인터페이스](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [IUnknown 인터페이스](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [winrt::implements 구조체 템플릿](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT를 통한 API 작성](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [C++/WinRT로 작성된 COM 구성 요소 사용](consume-com.md)
* [로컬 알림 메시지 보내기](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
