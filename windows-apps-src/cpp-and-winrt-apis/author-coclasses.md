---
description: C++/WinRT는 Windows 런타임 클래스를 작성하는 데 도움이 되는 것처럼 클래식 COM 구성 요소를 작성하는 데도 도움이 됩니다.
title: C++/WinRT를 통한 COM 구성 요소 작성
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 작성, COM, 구성 요소
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5ff3677c3624974759d1f6ff21d6e53cf9d33144
ms.sourcegitcommit: c5699e74b60c5c7a88658b4ebe30c1475eef5c27
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71344514"
---
# <a name="author-com-components-with-cwinrt"></a>C++/WinRT를 통한 COM 구성 요소 작성

[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)는 Windows 런타임 클래스를 작성하는 데 도움이 되는 것처럼 클래식 COM(구성 요소 개체 모델) 구성 요소를 작성하는 데도 도움이 됩니다. 이 항목에서는 다음 방법을 보여 줍니다.

## <a name="how-cwinrt-behaves-by-default-with-respect-to-com-interfaces"></a>기본적으로 COM 인터페이스와 관련하여 C++/WinRT가 동작하는 방법

C++/WinRT의 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 템플릿은 런타임 클래스 및 활성화 팩터리가 직접 또는 간접적으로 파생되는 기본 템플릿입니다.

기본적으로 **winrt::implements**는 [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable) 기반 인터페이스만 지원하며 클래식 COM 인터페이스를 자동으로 무시합니다. 따라서 클래식 COM 인터페이스에 대한 모든 **QueryInterface**(QI) 호출이 **E_NOINTERFACE**에서 실패합니다.

이 상황을 해결하는 방법을 곧 배우게 되겠지만 먼저 기본적으로 어떤 일이 발생하는지 설명하는 코드 예제를 살펴보겠습니다.

```idl
// Sample.idl
runtimeclass Sample
{
    Sample();
    void DoWork();
}

// Sample.h
#include "pch.h"
#include <shobjidl.h> // Needed only for this file.

namespace winrt::MyProject
{
    struct Sample : implements<Sample, IInitializeWithWindow>
    {
        IFACEMETHOD(Initialize)(HWND hwnd);
        void DoWork();
    }
}
```

또한 **Sample** 클래스를 사용하는 클라이언트 코드는 다음과 같습니다.

```cppwinrt
// Client.cpp
Sample sample; // Construct a Sample object via its projection.

// This next line crashes, because the QI for IInitializeWithWindow fails.
sample.as<IInitializeWithWindow>()->Initialize(hwnd); 
```

다행히도 **winrt::implements**에서 클래식 COM 인터페이스를 지원하도록 하려면 C++/WinRT 헤더를 포함하기 전에 `unknwn.h`를 포함시키면 됩니다.

`ole2.h`와 같은 다른 헤더 파일을 포함하여 명시적으로 또는 간접적으로 이 작업을 수행할 수 있습니다. 한 가지 권장되는 방법은 [Windows 구현 라이브러리(WIL)](https://github.com/Microsoft/wil)에 있는 `wil\cppwinrt.h` 헤더 파일을 포함하는 것입니다. `wil\cppwinrt.h` 헤더 파일은 `unknwn.h`가 `winrt/base.h` 앞에 확실하게 포함되도록 할 뿐 아니라 C++/WinRT 및 WIL에서 서로의 예외 및 오류 코드를 알 수 있도록 구성을 설정합니다.

## <a name="a-simple-example-of-a-com-component"></a>COM 구성 요소의 간단한 예제

다음은 C++/WinRT를 사용하여 작성된 COM 구성 요소의 간단한 예제입니다. 이는 미니 애플리케이션의 전체 목록이므로 코드를 새 **Windows 콘솔 애플리케이션(C++/WinRT)** 프로젝트의 `pch.h` 및 `main.cpp`에 붙여넣으면 해당 코드를 사용해 볼 수 있습니다.

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

[C++/WinRT를 통한 COM 구성 요소 사용](consume-com.md)을 참조하세요.

## <a name="a-more-realistic-and-interesting-example"></a>더 현실적이고 흥미로운 예제

이 항목의 나머지 부분에서는 C++/WinRT를 사용하여 기본 coclass(COM 구성 요소 또는 COM 클래스) 및 클래스 팩터리를 구현하는 최소한의 콘솔 애플리케이션 프로젝트를 만드는 방법을 안내합니다. 예제 애플리케이션은 콜백 단추를 사용하여 알림을 전달하는 방법을 보여 주며, **INotificationActivationCallback** COM 인터페이스를 구현하는 coclass를 사용하면 사용자가 알림에서 해당 단추를 클릭할 때 애플리케이션을 시작하고 콜백할 수 있습니다.

알림 기능 영역에 대한 더 많은 배경은 [로컬 알림 보내기](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)에서 확인할 수 있습니다. 문서의 해당 섹션에 있는 코드 예제는 C++/WinRT를 사용하지 않으므로 이 항목에 표시된 코드를 사용하는 것이 좋습니다.

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Windows 콘솔 애플리케이션 프로젝트(ToastAndCallback) 만들기

먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. **Windows 콘솔 애플리케이션(C++/WinRT)** 프로젝트를 만들고 이름을 *ToastAndCallback*으로 지정합니다.

`pch.h`를 열고 C++/WinRT 헤더의 includes 앞에 `#include <unknwn.h>`를 추가합니다. 결과적으로 `pch.h`의 콘텐츠를 이 목록으로 바꿀 수 있습니다.

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

`main.cpp`를 열고 프로젝트 템플릿이 생성하는 using 지시문을 제거합니다. 그 위치에 필요한 라이브러리, 헤더 및 형식 이름을 제공하는 다음 코드를 삽입합니다. 결과적으로 `main.cpp`의 콘텐츠를 이 목록으로 바꿀 수 있습니다(나중에 해당 함수를 바꿀 예정이므로 아래 목록의 `main`에서 코드도 제거함).

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

프로젝트가 아직 빌드되지 않았으며, 코드 추가를 마친 후에 빌드하고 실행하라는 메시지가 표시됩니다.

## <a name="implement-the-coclass-and-class-factory"></a>coclass 및 클래스 팩터리 구현

C++/WinRT에서는 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 기본 구조체에서 파생시키는 방식으로 coclass 및 클래스 팩터리를 구현합니다. 위에 표시된 3개의 using 지시문 바로 뒤에(및 `main` 앞에) 이 코드를 붙여넣어 알림 COM 활성자 구성 요소를 구현합니다.

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

위의 coclass 구현은 [C++/WinRT를 통한 API 작성](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class)에 설명된 동일한 패턴을 따릅니다. 따라서 동일한 기술을 사용하여 Windows 런타임 인터페이스뿐만 아니라 COM 인터페이스도 구현할 수 있습니다. COM 구성 요소 및 Windows 런타임 클래스는 인터페이스를 통해 해당 기능을 공개합니다. 모든 COM 인터페이스는 기본적으로 [**IUnknown 인터페이스**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown) 인터페이스에서 파생됩니다. Windows 런타임은 COM을 기반으로 합니다. 한 가지 예외는 Windows 런타임 인터페이스는 기본적으로 [**IInspectable 인터페이스**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)에서 파생되고 **IInspectable**은 **IUnknown**에서 파생된다는 것입니다.

위 코드의 coclass에서는 사용자가 알림에서 콜백 단추를 클릭할 때 호출되는 함수인 **INotificationActivationCallback::Activate** 메서드를 구현합니다. 그러나 이 함수를 호출하기 전에 **IClassFactory::CreateInstance** 함수를 사용하여 coclass의 인스턴스를 만들어야 합니다.

방금 구현한 coclass를 알림용 ‘COM 활성자’라고 하며, 위에서 확인한 **GUID** 형식의 `callback_guid` 식별자 형태로 클래스 ID(CLSID)를 포함합니다.  나중에 시작 메뉴 바로 가기 및 Windows 레지스트리 항목의 형태로 해당 식별자를 사용합니다. COM 활성자 CLSID 및 연결된 COM 서버 경로(여기서는 빌드하는 실행 파일 경로)는 알림 센터에서 알림을 클릭하는지 여부와 관계없이 콜백 단추를 클릭하는 경우의 인스턴스를 만드는 클래스를 알림 메시지에서 인식하는 데 사용되는 메커니즘입니다.

## <a name="best-practices-for-implementing-com-methods"></a>COM 메서드 구현 모범 사례

오류 처리 및 리소스 관리 기술은 밀접한 관련이 있습니다. 오류 코드보다는 예외를 사용하는 것이 더 편리하고 실용적입니다. RAII(Resource-Acquisition-Is-Initialization) 관용구를 사용하면 오류 코드를 명시적으로 확인한 후 리소스를 명시적으로 릴리스하지 않아도 됩니다. 이 명시적 확인은 코드를 필요한 것보다 더 복잡하게 만들고 숨겨야 하는 많은 곳에 버그를 제공합니다. RAII 및 throw/catch 예외를 대신 사용합니다. 이렇게 하면 리소스 할당은 예외로부터 안전하며 코드는 단순합니다.

그러나 예외가 COM 메서드 구현을 이스케이프하도록 허용하면 안 됩니다. COM 메서드에서 `noexcept` 지정자를 사용하여 확인할 수 있습니다. 메서드가 종료되기 전에 예외를 처리하는 경우에는 메서드의 호출 그래프에서 예외가 throw되어도 괜찮습니다. `noexcept`를 사용하지만 예외가 메서드를 이스케이프하도록 허용하면 애플리케이션이 종료됩니다.

## <a name="add-helper-types-and-functions"></a>도우미 형식 및 함수 추가

이 단계에서는 나머지 코드에서 사용할 일부 도우미 형식 및 함수를 추가합니다. 따라서 `main` 바로 앞에 다음을 추가합니다.

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

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>나머지 함수 및 wmain 진입점 함수 구현

`main` 함수를 삭제하며, coclass를 등록한 후 애플리케이션을 호출할 수 있는 알림을 전달하는 코드가 포함된 이 코드 목록을 해당 위치에 붙여넣습니다.

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

## <a name="how-to-test-the-example-application"></a>예제 애플리케이션을 테스트하는 방법

애플리케이션을 빌드한 후 관리자 권한으로 한 번 이상 실행하여 등록 및 기타 설정 코드가 실행되도록 합니다. 이 작업을 수행하는 한 가지 방법은 Visual Studio를 관리자 권한으로 실행한 후 Visual Studio에서 앱을 실행하는 것입니다. 작업 표시줄에서 Visual Studio를 마우스 오른쪽 단추로 클릭하여 점프 목록을 표시하고, 점프 목록에서 Visual Studio를 마우스 오른쪽 단추로 클릭한 다음, **관리자 권한으로 실행**을 클릭합니다. 프롬프트에 동의한 후 프로젝트를 엽니다. 애플리케이션을 실행할 때 애플리케이션이 관리자 권한으로 실행 중인지 여부를 나타내는 메시지가 표시됩니다. 메시지가 표시되지 않으면 등록 및 기타 설정이 실행되지 않습니다. 애플리케이션이 올바르게 작동하려면 해당 등록 및 기타 설정이 한 번 이상 실행되어야 합니다.

애플리케이션을 관리자 권한으로 실행 중인지 여부에 관계없이 ‘T’를 눌러 알림이 표시되도록 합니다. 그런 다음, 표시되는 알림에서 직접 또는 알림 센터에서 **ToastAndCallback 콜백** 단추를 클릭하면 애플리케이션이 시작되고, coclass가 인스턴스화되고, **INotificationActivationCallback::Activate** 메서드가 실행됩니다.

## <a name="in-process-com-server"></a>In-process COM 서버

위의 *ToastAndCallback* 예제 앱은 로컬(또는 Out of Process) COM 서버로 작동합니다. 이 서버는 해당 coclass의 CLSID를 등록하는 데 사용하는 [LocalServer32](/windows/desktop/com/localserver32) Windows 레지스트리 키로 표시됩니다. 로컬 COM 서버는 실행 가능한 이진 파일(`.exe`) 내부에 해당 coclass를 호스트합니다.

가능성이 더 높은 또 다른 방법으로, 동적 연결 라이브러리(`.dll`) 내부에 coclass를 호스트할 수 있습니다. DLL 형식의 COM 서버를 In-process COM 서버라고 하며 [InprocServer32](/windows/desktop/com/inprocserver32) Windows 레지스트리 키를 사용하여 등록되는 CLSID로 표시됩니다.

### <a name="create-a-dynamic-link-library-dll-project"></a>DLL(동적 연결 라이브러리) 프로젝트 만들기

Microsoft Visual Studio에서 새 프로젝트를 만들어 In-process COM 서버를 만드는 작업을 시작할 수 있습니다. **Visual C++**  > **Windows 데스크톱** > **DLL(동적 연결 라이브러리)** 프로젝트를 만듭니다.

새 프로젝트에 C++/WinRT 지원을 추가하려면 [Windows 데스크톱 애플리케이션 프로젝트를 수정하여 C++/WinRT 지원 추가](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support)에 설명된 단계를 따릅니다.

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>coclass, 클래스 팩터리 및 in-proc 서버 내보내기 구현

`dllmain.cpp`를 열고 아래에 표시된 코드 목록을 추가합니다.

C++/WinRT Windows 런타임 클래스를 구현하는 DLL이 이미 있는 경우 아래 표시된 **DllCanUnloadNow** 함수가 이미 있는 것입니다. coclass를 해당 DLL에 추가하려면 **DllGetClassObject** 함수를 추가하면 됩니다.

호환성을 유지하는 데 필요한 기존 [Windows 런타임 C++ 템플릿 라이브러리(WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 코드가 없는 경우 표시된 코드에서 WRL 파트를 제거할 수 있습니다.

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
            return winrt::make<MyCoclass>()->QueryInterface(riid, ppvObject);
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

### <a name="support-for-weak-references"></a>약한 참조 지원

[C++/WinRT의 약한 참조](weak-references.md#weak-references-in-cwinrt)를 참조하세요.

사용자가 사용하는 형식이 [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)(또는 **IInspectable**에서 파생되는 인터페이스)을 구현하는 경우 C++/WinRT(특히 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 기본 구조체 템플릿)는 [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource)를 구현합니다.

이는 **IWeakReferenceSource** 및 [**IWeakReference**](/windows/desktop/api/weakreference/nn-weakreference-iweakreference)가 Windows 런타임 형식용으로 디자인되었기 때문입니다. 따라서 **winrt::Windows::Foundation::IInspectable**(또는 **IInspectable**에서 파생된 인터페이스)을 구현에 추가하면 coclass의 약한 참조 지원을 간단히 켤 수 있습니다.

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
