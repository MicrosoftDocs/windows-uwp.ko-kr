---
author: andrewleader
Description: Learn how Win32 C++ WRL apps can send local toast notifications and handle the user clicking the toast.
title: 데스크톱 C++ WRL 앱에서 로컬 알림 메시지 보내기
label: Send a local toast notification from desktop C++ WRL apps
template: detail.hbs
ms.author: mijacobs
ms.date: 03/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, win32, 데스크톱, 알림 메시지, 알림 보내기, 로컬 알림 보내기, 데스크톱 브리지, C++, cpp, cplusplus, WRL
ms.localizationpriority: medium
ms.openlocfilehash: a6134e65a27563f96c03880dea026bed11f46641
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4319257"
---
# <a name="send-a-local-toast-notification-from-desktop-c-wrl-apps"></a>데스크톱 C++ WRL 앱에서 로컬 알림 메시지 보내기

데스크톱 앱(데스크톱 브리지 및 클래식 Win32)은 UWP(유니버설 Windows 플랫폼) 앱과 마찬가지로 대화형 알림 메시지를 보낼 수 있습니다. 그러나 데스크톱 브리지를 사용하지 않는 경우 활성화 스키마가 다르고 패키지 ID가 부족할 수 있기 때문에 데스크톱 앱에 대한 몇 가지 특수 단계가 있습니다.

> [!IMPORTANT]
> UWP 앱을 작성하는 경우 [UWP 설명서](send-local-toast.md)를 참조하세요. 다른 데스크톱 언어는 [Desktop C#](send-local-toast-desktop.md)을 참조하세요.


## <a name="step-1-enable-the-windows-10-sdk"></a>1단계: Windows 10 SDK 활성화

Win32 앱용 Windows 10 SDK를 활성화하지 않았다면 먼저 활성화 해야 합니다. 몇 가지 주요 단계가 있습니다.

1. **추가 종속성**에 `runtimeobject.lib` 추가
2. Windows 10 SDK 대상 지정

프로젝트를 마우스 오른쪽 단추로 클릭한 다음 **속성**을 선택합니다.

위쪽 **구성** 메뉴에서 **모든 구성**을 선택하여 다음 변경 사항을 디버그와 릴리스에 적용합니다.

**링커-> 입력** 아래에서 `runtimeobject.lib`를 **추가 종속성**에 추가합니다.

그런 다음 **일반**에서 **Windows SDK 버전**이 10.0 이상(Windows 8.1. 아님)으로 설정되어 있는지 확인합니다.


## <a name="step-2-copy-compat-library-code"></a>2단계: compat 라이브러리 코드 복사

[DesktopNotificationManagerCompat.h](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.h)와 [DesktopNotificationManagerCompat.cpp](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.cpp) 파일을 GitHub에서 프로젝트로 복사합니다. 호환되는 라이브러리는 데스크톱 알림의 복잡성을 대부분 추상화합니다. 다음 지침은 호환되는 라이브러리가 필요합니다.

미리 컴파일된 헤더를 사용하는 경우 `#include "stdafx.h"`를 DesktopNotificationManagerCompat.cpp 파일의 첫 번째 행으로 지정합니다.


## <a name="step-3-include-the-header-files-and-namespaces"></a>3단계: 헤더 파일 및 네임스페이스 포함

compat 라이브러리 헤더 파일을 포함하고 UWP 알림 API 사용과 관련된 헤더 파일과 네임스페이스를 포함합니다.

```cpp
#include "DesktopNotificationManagerCompat.h"
#include "NotificationActivationCallback.h"
#include <windows.ui.notifications.h>

using namespace ABI::Windows::Data::Xml::Dom;
using namespace ABI::Windows::UI::Notifications;
using namespace Microsoft::WRL;
```


## <a name="step-4-implement-the-activator"></a>4단계: 활성자 구현

알림 활성화를 위해 처리기를 구현해야하므로 사용자가 알림을 클릭할 때 앱에서 작업을 수행할 수 있습니다. 이때 알림을 알림 센터에서 지속해야 합니다(앱을 종료하고 나서 며칠 후 알림을 클릭할 수 있기 때문). 이 클래스는 프로젝트의 어느 위치에나 배치할 수 있습니다.

UUID를 포함하여 아래에 표시된 대로 **INotificationActivationCallback** 인터페이스를 구현하고, **CoCreatableClass**를 호출하여 COM 생성 가능 클래스로 플래그를 지정합니다. UUID의 경우 많은 온라인 GUID 생성기 중 하나를 사용하여 고유한 GUID를 만듭니다. 이 GUID CLSID(클래스 식별자)는 알림 센터가 COM에 활성화해야 하는 COM에 대한 클래스를 인식하는 방법입니다.

```cpp
// The UUID CLSID must be unique to your app. Create a new GUID if copying this code.
class DECLSPEC_UUID("replaced-with-your-guid-C173E6ADF0C3") NotificationActivator WrlSealed WrlFinal
    : public RuntimeClass<RuntimeClassFlags<ClassicCom>, INotificationActivationCallback>
{
public:
    virtual HRESULT STDMETHODCALLTYPE Activate(
        _In_ LPCWSTR appUserModelId,
        _In_ LPCWSTR invokedArgs,
        _In_reads_(dataCount) const NOTIFICATION_USER_INPUT_DATA* data,
        ULONG dataCount) override
    {
        // TODO: Handle activation
    }
};

// Flag class as COM creatable
CoCreatableClass(NotificationActivator);
```


## <a name="step-5-register-with-notification-platform"></a>5단계: 알림 플랫폼에 등록

그런 다음 알림 플랫폼에 등록해야 합니다. 데스크톱 브리지 또는 클래식 Win32 중 어느 것을 사용하는지에 따라 단계가 달라집니다. 이 둘을 모두 지원하는 경우 두 단계를 수행해야 합니다(하지만, 코드를 분기하지 않아도 되며 라이브러리가 이것을 자동으로 처리합니다!)


### <a name="desktop-bridge"></a>데스크톱 브리지

**Package.appxmanifest**에서 데스크톱 브리지(또는 이 두 가지 모두를 지원하는 경우)를 사용하는 경우 다음을 추가합니다.

1. **xmlns:com** 선언
2. **xmlns:desktop** 선언
3. **IgnorableNamespaces** 특성에서 **com**과 **desktop**
4. 4단계에서 GUID를 사용하여 COM 활성자에서 **com:Extension**. 알림에서 실행을 알 수 있도록 `Arguments="-ToastActivated"`를 포함해야 합니다.
5. **windows.toastNotificationActivation**에 대한 **desktop:Extension**는 알림 활성자 CLSID(4단계의 GUID)를 선언합니다.

**Package.appxmanifest**

```xml
<Package
  ...
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="... com desktop">
  ...
  <Applications>
    <Application>
      ...
      <Extensions>

        <!--Register COM CLSID LocalServer32 registry key-->
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="YourProject\YourProject.exe" Arguments="-ToastActivated" DisplayName="Toast activator">
              <com:Class Id="replaced-with-your-guid-C173E6ADF0C3" DisplayName="Toast activator"/>
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>

        <!--Specify which CLSID to activate when toast clicked-->
        <desktop:Extension Category="windows.toastNotificationActivation">
          <desktop:ToastNotificationActivation ToastActivatorCLSID="replaced-with-your-guid-C173E6ADF0C3" /> 
        </desktop:Extension>

      </Extensions>
    </Application>
  </Applications>
 </Package>
```


### <a name="classic-win32"></a>클래식 Win32

클래식 Win32(또는 두 가지를 모두 지원하는 경우)를 사용하는 경우 시작 중 앱의 바로 가기에서 응용 프로그램 사용자 모델 ID(AUMID)와 알림 활성자 CLSID(4단계의 GUID)를 선언해야 합니다.

Win32 앱을 식별하는 고유한 AUMID를 선택합니다. 이것은 일반적으로 [CompanyName].[AppName]의 형태지만, 모든 앱에서 고유해야 합니다(끝 부분에 임의의 숫자를 자유롭게 추가할 수 있음).

#### <a name="step-51-wix-installer"></a>5.1단계: WiX 설치 관리자

설치 프로그램에 WiX를 사용하는 경우 **Product.wxs** 파일을 편집하여 두 개의 바로 가기 속성을 아래 보이는 시작 메뉴 바로 가기에 추가합니다. 4단계에서 GUID가 아래와 같이 `{}`로 묶여 있는지 확인해야 합니다.

**Product.wxs**

```xml
<Shortcut Id="ApplicationStartMenuShortcut" Name="Wix Sample" Description="Wix Sample" Target="[INSTALLFOLDER]WixSample.exe" WorkingDirectory="INSTALLFOLDER">
                    
    <!--AUMID-->
    <ShortcutProperty Key="System.AppUserModel.ID" Value="YourCompany.YourApp"/>
    
    <!--COM CLSID-->
    <ShortcutProperty Key="System.AppUserModel.ToastActivatorCLSID" Value="{replaced-with-your-guid-C173E6ADF0C3}"/>
    
</Shortcut>
```

> [!IMPORTANT]
> 알림을 실제로 사용하려면 정상적으로 디버깅하기 전에 한 번 설치 프로그램을 통해 앱을 설치해야 AUMID 및 CLSID로 시작 바로 가기가 표시됩니다. 시작 바로 가기가 나타나면 Visual Studio에서 F5를 사용하여 디버깅할 수 있습니다.


#### <a name="step-52-register-aumid-and-com-server"></a>5.2단계: AUMID 및 COM 서버 등록

그런 다음 설치 프로그램과 관계없이 앱의 시작 코드(알림 API를 호출하기 전)에서 4단계의 알림 활성자 클래스와 위에 사용된 AUMID를 지정하여 **RegisterAumidAndComServer** 메서드를 호출합니다.

```cpp
// Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"YourCompany.YourApp", __uuidof(NotificationActivator));
```

데스크톱 브리지와 클래식 Win32를 모두 지원하는 이 메서드를 자유롭게 호출합니다. 데스크톱 브리지에서 실행 중인 경우 이 메서드는 즉시 반환됩니다. 코드를 분기할 필요가 없습니다.

이 메서드를 사용하면 AUMID를 지속적으로 제공하지 않고도 compat API를 호출하여 알림을 보내고 관리할 수 있습니다. 또한 이 메서드는 COM 서버에 대해 LocalServer32 레지스트리 키를 삽입합니다.


## <a name="step-6-register-com-activator"></a>6단계: COM 활성자 등록

데스크톱 브리지 및 클래식 Win32 앱 모두에서 알림 활성화를 처리할 수 있도록 알림 활성자 유형을 등록해야 합니다.

앱의 시작 코드에서 다음 **RegisterActivator** 메소드를 호출합니다. 이것은 알림 활성화를 받을 수 있도록 호출되어야 합니다.

```cpp
// Register activator type
hr = DesktopNotificationManagerCompat::RegisterActivator();
```


## <a name="step-7-send-a-notification"></a>7단계: 알림 보내기

알림 보내기는 **DesktopNotificationManagerCompat**을 사용하여 **ToastNotifier**를 만든다는 점만 제외하고 UWP 앱과 동일합니다. compat 라이브러리는 데스크톱 브리지와 클래식 Win32의 차이를 자동으로 처리하므로 코드를 분기하지 않아도 됩니다. 클래식 Win32의 경우 compat 라이브러리가 **RegisterAumidAndComServer**를 호출할 때 제공한 AUMID를 캐싱하므로 AUMID 제공 여부 시기를 걱정하지 않아도 됩니다.

레거시 Windows 8.1 알림 메시지 템플릿은 4단계에서 생성한 COM 알림 활성자를 활성화하지 않기 때문에 아래에 나오는 **ToastGeneric** 바인딩을 사용해야 합니다.

> [!IMPORTANT]
> Http 이미지는 매니페스트에 인터넷 기능이 있는 데스크톱 브리지 앱에서만 지원됩니다. 클래식 Win32 앱은 http 이미지를 지원하지 않습니다. 따라서 로컬 앱 데이터에 이미지를 다운로드하고 로컬로 이것을 참조해야 합니다.

```cpp
// Construct XML
ComPtr<IXmlDocument> doc;
hr = DesktopNotificationManagerCompat::CreateXmlDocumentFromString(
    L"<toast><visual><binding template='ToastGeneric'><text>Hello world</text></binding></visual></toast>",
    &doc);
if (SUCCEEDED(hr))
{
    // See full code sample to learn how to inject dynamic text, buttons, and more

    // Create the notifier
    // Classic Win32 apps MUST use the compat method to create the notifier
    ComPtr<IToastNotifier> notifier;
    hr = DesktopNotificationManagerCompat::CreateToastNotifier(&notifier);
    if (SUCCEEDED(hr))
    {
        // Create the notification itself (using helper method from compat library)
        ComPtr<IToastNotification> toast;
        hr = DesktopNotificationManagerCompat::CreateToastNotification(doc, &toast);
        if (SUCCEEDED(hr))
        {
            // And show it!
            hr = notifier->Show(toast.Get());
        }
    }
}
```

> [!IMPORTANT]
> 클래식 Win32 앱은 레거시 알림 템플릿(예: ToastText02)을 사용할 수 없습니다. COM CLSID가 지정되었을 때 레거시 템플릿의 활성화는 실패합니다. 위에서 설명한 대로 Windows 10 ToastGeneric 템플릿을 사용해야 합니다.


## <a name="step-8-handling-activation"></a>8단계: 활성화 처리

사용자가 알림이나 알림의 단추를 클릭할 때 **NotificationActivator** 클래스의 **Activate** 메서드가 호출됩니다.

Activate 메서드 내에서 알림에 지정된 인수를 구문 분석하고 사용자가 입력하거나 선택한 사용자 입력을 가져온 다음 그에 따라 앱을 활성화합니다.

> [!NOTE]
> **Activate** 메서드는 주 스레드의 별도의 스레드에서 호출됩니다.

```cpp
// The GUID must be unique to your app. Create a new GUID if copying this code.
class DECLSPEC_UUID("replaced-with-your-guid-C173E6ADF0C3") NotificationActivator WrlSealed WrlFinal
    : public RuntimeClass<RuntimeClassFlags<ClassicCom>, INotificationActivationCallback>
{
public: 
    virtual HRESULT STDMETHODCALLTYPE Activate(
        _In_ LPCWSTR appUserModelId,
        _In_ LPCWSTR invokedArgs,
        _In_reads_(dataCount) const NOTIFICATION_USER_INPUT_DATA* data,
        ULONG dataCount) override
    {
        std::wstring arguments(invokedArgs);
        HRESULT hr = S_OK;

        // Background: Quick reply to the conversation
        if (arguments.find(L"action=reply") == 0)
        {
            // Get the response user typed.
            // We know this is first and only user input since our toasts only have one input
            LPCWSTR response = data[0].Value;

            hr = DesktopToastsApp::SendResponse(response);
        }

        else
        {
            // The remaining scenarios are foreground activations,
            // so we first make sure we have a window open and in foreground
            hr = DesktopToastsApp::GetInstance()->OpenWindowIfNeeded();
            if (SUCCEEDED(hr))
            {
                // Open the image
                if (arguments.find(L"action=viewImage") == 0)
                {
                    hr = DesktopToastsApp::GetInstance()->OpenImage();
                }

                // Open the app itself
                // User might have clicked on app title in Action Center which launches with empty args
                else
                {
                    // Nothing to do, already launched
                }
            }
        }

        if (FAILED(hr))
        {
            // Log failed HRESULT
        }

        return S_OK;
    }

    ~NotificationActivator()
    {
        // If we don't have window open
        if (!DesktopToastsApp::GetInstance()->HasWindow())
        {
            // Exit (this is for background activation scenarios)
            exit(0);
        }
    }
};

// Flag class as COM creatable
CoCreatableClass(NotificationActivator);
```

WinMain 기능에서 앱이 종료된 상태로 제대로 실행되도록 지원하려면 알림에서 실행할지를 결정해야 합니다. 알림에서 시작하는 경우 "-ToastActivated"라는 시작 인수가 있습니다. 이것이 표시되면 정상적인 시작 활성화 코드 수행을 중단하고 필요에 따라 **NotificationActivator**가 창 실행을 처리하도록 허용해야 합니다.

```cpp
// Main function
int WINAPI wWinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE, _In_ LPWSTR cmdLineArgs, _In_ int)
{
    RoInitializeWrapper winRtInitializer(RO_INIT_MULTITHREADED);

    HRESULT hr = winRtInitializer;
    if (SUCCEEDED(hr))
    {
        // Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
        hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"WindowsNotifications.DesktopToastsCpp", __uuidof(NotificationActivator));
        if (SUCCEEDED(hr))
        {
            // Register activator type
            hr = DesktopNotificationManagerCompat::RegisterActivator();
            if (SUCCEEDED(hr))
            {
                DesktopToastsApp app;
                app.SetHInstance(hInstance);

                std::wstring cmdLineArgsStr(cmdLineArgs);

                // If launched from toast
                if (cmdLineArgsStr.find(TOAST_ACTIVATED_LAUNCH_ARG) != std::string::npos)
                {
                    // Let our NotificationActivator handle activation
                }

                else
                {
                    // Otherwise launch like normal
                    app.Initialize(hInstance);
                }

                app.RunMessageLoop();
            }
        }
    }

    return SUCCEEDED(hr);
}
```


### <a name="activation-sequence-of-events"></a>이벤트의 활성화 시퀀스

활성화 시퀀스는 다음과 같습니다.

앱이 이미 실행되고 있는 경우:

1. **NotificationActivator**에서 **Activate**가 호출됨

앱이 실행되지 않는 경우:

1. 앱이 시작 EXE로 실행되면 명령줄 인수 "-ToastActivated"를 받게 됨
2. **NotificationActivator**에서 **Activate**가 호출됨


### <a name="foreground-vs-background-activation"></a>포그라운드 및 백그라운드 활성화
데스크톱 앱의 경우 전경 및 배경 활성화가 동일하게 처리되어 COM 활성화가 호출됩니다. 창을 표시할지 아니면 단순히 작업을 수행한 다음 종료할지 결정하는 것은 앱의 코드에 따라 달라집니다. 따라서 알림 콘텐츠에 **background**의 **activationType**을 지정해도 동작이 바뀌지 않습니다.


## <a name="step-9-remove-and-manage-notifications"></a>9단계: 알림 제거 및 관리

알림 제거 및 관리는 UWP 앱과 동일합니다. 그렇지만 compat 라이브러리를 사용하여 **DesktopNotificationHistoryCompat**을 가져오는 것이 좋습니다. 따라서 클래식 Win32를 사용하는 경우 AUMID 제공에 대해 걱정하지 않아도 됩니다.

```cpp
std::unique_ptr<DesktopNotificationHistoryCompat> history;
auto hr = DesktopNotificationManagerCompat::get_History(&history);
if (SUCCEEDED(hr))
{
    // Remove a specific toast
    hr = history->Remove(L"Message2");

    // Clear all toasts
    hr = history->Clear();
}
```


## <a name="step-10-deploying-and-debugging"></a>10단계: 배포 및 디버깅

데스크톱 브리지 앱을 배포하고 디버깅하려면 [패키지로 만든 데스크톱 앱 실행, 디버깅, 테스트](/porting/desktop-to-uwp-debug.md)를 참조하세요.

클래식 Win32 앱을 배포하고 디버깅하려면, 정상적으로 디버깅하기 전에 한 번 설치 프로그램을 통해 앱을 설치해야 해야만 AUMID 및 CLSID로 시작 바로 가기가 표시됩니다. 시작 바로 가기가 나타나면 Visual Studio에서 F5를 사용하여 디버깅할 수 있습니다.

클래식 Win32 앱에 알림이 표시되지 않고 예외가 발생하지 않으면 시작 바로 가기가 표시되지 않거나(설치 프로그램을 통해 앱 설치) 코드에 사용한 AUMID가 시작 바로 가기의 AUMID와 일치하지 않습니다.

알림이 나타나지만 알림 센터에 계속 표지되지 않으면(팝업이 해제된 후 사라짐) COM 활성자를 제대로 구현한 것이 아닙니다.

데스크톱 브리지와 클래식 Win32 앱을 모두 설치한 경우 알림 활성화를 처리할 때에는 데스크톱 브리지 앱이 클래식 Win32 앱보다 우선합니다. 즉, 클래식 Win32 앱의 알림은 클릭하면 계속해서 데스크톱 브리지 앱을 시작합니다. 데스크톱 브리지 앱을 제거하면 활성화가 다시 클래식 Win32 앱으로 되돌아갑니다.

`HRESULT 0x800401f0 CoInitialize has not been called.`를 받으면 API를 호출하기 전에 앱에서 `CoInitialize(nullptr)`를 호출해야 합니다.

Compat API를 호출하는 동안 `HRESULT 0x8000000e A method was called at an unexpected time.`를 받으면 이는 필요한 Register 메서드를 호출하지 못했을 가능성이나 데스크톱 브리지 앱의 경우 데스크톱 브리지 컨텍스트에서 현재 앱을 실행하지 않음을 의미합니다.

`unresolved external symbol` 컴파일 오류가 여러 개 발생하는 경우 1단계에서 `runtimeobject.lib`를 **추가 종속성**에 추가하는 것을 잊었거나 디버그 구성에만 추가하고 릴리스 구성에는 추가하지 않았을 가능성이 있습니다.


## <a name="handling-older-versions-of-windows"></a>이전 버전의 Windows 처리

Windows 8.1 이하를 지원하는 경우 **DesktopNotificationManagerCompat** API를 호출하거나 ToastGeneric 알림을 보내기 전에 Windows 10에서 실행 중인지 런타임에서 확인해야 합니다.

Windows 8은 알림 메시지를 도입했지만, ToastText01과 같은 [레거시 알림 템플릿](https://docs.microsoft.com/en-us/previous-versions/windows/apps/hh761494(v=win.10))을 사용했습니다. 활성화는 지속되지 않은 간단한 팝업이었기 때문에 활성화는 **ToastNotification** 클래스에서 메모리 내 **활성화됨** 이벤트를 통해 처리되었습니다. Windows 10은 [대화형 ToastGeneric 알림](adaptive-interactive-toasts.md)을 도입했으며 수일 동안 알림이 지속되는 알림 센터를 도입했습니다. 알림 센터를 도입하기 위해 COM 활성자의 도입이 필요했으므로 알림을 만든 날짜 이후에도 알림을 활성화할 수 있습니다.

| OS | ToastGeneric | COM 활성자 | 레거시 알림 템플릿 |
| -- | ------------ | ------------- | ---------------------- |
| Windows10 | 지원 | 지원 | 지원(하지만 COM 서버 활성화 제외) |
| Windows 8.1/8 | 해당 없음 | 해당 없음 | 지원 |
| Windows 7 이하 | 해당 없음 | 해당 없음 | 해당 없음 |

Windows 10을 실행하고 있는지 확인하려면, `<VersionHelpers.h>` 헤더를 포함하고 **IsWindows10OrGreater** 메서드를 확인합니다. true를 반환하면 계속해서 이 설명서 나오는 모든 메서드를 호출합니다! 

```cpp
#include <VersionHelpers.h>

if (IsWindows10OrGreater())
{
    // Running Windows 10, continue with sending Windows 10 toasts!
}
```


## <a name="known-issues"></a>알려진 문제

**수정된 내용: 알림 클릭 후 앱의 초점이 맞춰지지 않음**: 빌드 15063 및 이전 버전에서는 전경 권한이 COM 서버를 활성화할 때 응용 프로그램으로 전달되지 않았습니다. 따라서 앱을 전경으로 옮기려고 하면 앱이 깜박입니다. 이 문제에 대한 해결 방법이 없었습니다. 빌드 16299 이상에서 이 문제를 해결했습니다.


## <a name="resources"></a>리소스

* [GitHub의 전체 코드 샘플](https://github.com/WindowsNotifications/desktop-toasts)
* [데스크톱 앱에서 알림 메시지](toast-desktop-apps.md)
* [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)