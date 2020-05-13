---
Description: Win32 c + + WRL apps에서 로컬 알림 메시지를 보내고 알림 메시지를 클릭 하 여 사용자를 처리 하는 방법을 알아봅니다.
title: 데스크톱 c + + WRL apps에서 로컬 알림 메시지 보내기
label: Send a local toast notification from desktop C++ WRL apps
template: detail.hbs
ms.date: 03/07/2018
ms.topic: article
keywords: windows 10, uwp, win32, 데스크톱, 알림 메시지 보내기, 알림 보내기, 바탕 화면 브리지, msix, 스파스 패키지, c + +, cpp, cplusplus, WRL
ms.localizationpriority: medium
ms.openlocfilehash: 3e103c41de7bf169629085fd259e23e17804360d
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234664"
---
# <a name="send-a-local-toast-notification-from-desktop-c-wrl-apps"></a>데스크톱 c + + WRL apps에서 로컬 알림 메시지 보내기

데스크톱 앱 (패키지 된 [Msix](https://docs.microsoft.com/windows/msix/desktop/source-code-overview) 앱, 패키지 id를 얻기 위해 [스파스 패키지](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) 를 사용 하는 앱 및 클래식 패키지 되지 않은 Win32 앱 포함)은 Windows 앱과 마찬가지로 대화형 알림 메시지를 보낼 수 있습니다. 그러나 MSIX 또는 스파스 패키지를 사용 하지 않는 경우 다양 한 활성화 체계와 패키지 id의 잠재적 부족으로 인해 데스크톱 앱에 대 한 몇 가지 특별 한 단계가 있습니다.

> [!IMPORTANT]
> UWP 앱을 작성 하는 경우 [uwp 설명서](send-local-toast.md)를 참조 하세요. 다른 데스크톱 언어는 [데스크톱 c #](send-local-toast-desktop.md)을 참조 하세요.


## <a name="step-1-enable-the-windows-10-sdk"></a>1 단계: Windows 10 SDK 사용

Win32 앱에 대해 Windows 10 SDK를 사용 하도록 설정 하지 않은 경우이를 먼저 수행 해야 합니다. 몇 가지 주요 단계가 있습니다.

1. 추가 `runtimeobject.lib` **종속성** 에 추가
2. Windows 10 SDK를 대상으로 합니다.

프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다.

위의 **구성** 메뉴에서 **모든 구성** 을 선택 하 여 다음 변경 내용이 디버그와 릴리스에 모두 적용 되도록 합니다.

**링커-> 입력**에서 추가 `runtimeobject.lib` **종속성**에 추가 합니다.

그런 다음 **일반**에서 **Windows SDK 버전이** Windows 8.1 아닌 10.0 이상으로 설정 되어 있는지 확인 합니다.


## <a name="step-2-copy-compat-library-code"></a>2 단계: 호환 라이브러리 코드 복사

GitHub의 [desktopnotificationmanagercompat .h](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.h) 및 [desktopnotificationmanagercompat .cpp](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.cpp) 파일을 프로젝트에 복사 합니다. 호환성 라이브러리는 데스크톱 알림의 복잡성을 대부분 추상화 합니다. 다음 지침에는 호환 라이브러리가 필요 합니다.

미리 컴파일된 헤더를 사용 하는 경우에는를 `#include "stdafx.h"` DesktopNotificationManagerCompat .cpp 파일의 첫 번째 줄로 지정 해야 합니다.


## <a name="step-3-include-the-header-files-and-namespaces"></a>3 단계: 헤더 파일 및 네임 스페이스 포함

호환 되는 라이브러리 헤더 파일과 Windows 알림 Api 사용과 관련 된 헤더 파일 및 네임 스페이스를 포함 합니다.

```cpp
#include "DesktopNotificationManagerCompat.h"
#include <NotificationActivationCallback.h>
#include <windows.ui.notifications.h>

using namespace ABI::Windows::Data::Xml::Dom;
using namespace ABI::Windows::UI::Notifications;
using namespace Microsoft::WRL;
```


## <a name="step-4-implement-the-activator"></a>4 단계: 활성기 구현

사용자가 알림을 클릭할 때 앱에서 어떤 작업을 수행할 수 있도록 알림 활성화에 대 한 처리기를 구현 해야 합니다. 알림 메시지는 나중에 앱을 닫을 때 며칠을 클릭할 수 있으므로 알림 메시지를 알림 센터에 보관 하는 데 필요 합니다. 이 클래스는 프로젝트의 어느 위치에 나 배치할 수 있습니다.

UUID를 포함 하 여 아래에 표시 된 대로 **INotificationActivationCallback** 인터페이스를 구현 하 고 **Cocreatableclass** 를 호출 하 여 클래스에 COM 생성 가능으로 플래그를 지정 합니다. UUID의 경우 여러 온라인 GUID 생성기 중 하나를 사용 하 여 고유한 GUID를 만듭니다. 이 GUID CLSID (클래스 식별자)는 Action Center가 COM에서 활성화할 클래스를 인식 하는 방법입니다.

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


## <a name="step-5-register-with-notification-platform"></a>5 단계: 알림 플랫폼에 등록

그런 다음 알림 플랫폼에 등록 해야 합니다. MSIX/sparse 패키지를 사용 하는지 아니면 클래식 Win32를 사용 하는지에 따라 다른 단계가 있습니다. 두 단계를 모두 지 원하는 경우에는 두 단계를 모두 수행 해야 합니다 (그러나 코드를 분기할 필요는 없으며 라이브러리에서 사용자를 위해 처리 함).


### <a name="msixsparse-package"></a>MSIX/sparse 패키지

[Msix](https://docs.microsoft.com/windows/msix/desktop/source-code-overview) 또는 [스파스 패키지](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) 를 사용 하는 경우 (또는 둘 다를 지 원하는 경우) appxmanifest.xml에서 다음을 추가 **합니다**.

1. **Xmlns: com** 에 대 한 선언
2. **Xmlns: desktop** 에 대 한 선언
3. **IgnorableNamespaces** 특성, **com** 및 **desktop**
4. **com:** #4 단계에서 GUID를 사용 하 여 com 활성기에 대 한 확장입니다. `Arguments="-ToastActivated"`알림 메시지의 시작을 알 수 있도록를 포함 해야 합니다.
5. **desktop:** TOAST 활성기 CLSID (#4의 GUID)를 선언 하는 **windows. toastNotificationActivation** 용 확장입니다.

**Appxmanifest.xml**

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

클래식 Win32를 사용 하는 경우 (또는 둘 다를 지 원하는 경우) 시작의 앱 바로 가기에서 응용 프로그램 사용자 모델 ID (AUMID) 및 toast 활성기 CLSID (#4의 GUID)를 선언 해야 합니다.

Win32 앱을 식별 하는 고유한 AUMID를 선택 합니다. 이는 일반적으로 [CompanyName] 형식입니다. [AppName] 이지만 모든 앱에서 고유한 지 확인 하는 것이 좋습니다 (끝에 일부 숫자를 추가 하는 데 사용 가능).

#### <a name="step-51-wix-installer"></a>5.1 단계: WiX 설치 관리자

설치 관리자에 대해 WiX를 사용 하는 경우 다음에 표시 된 것 처럼 **Product. wxs** 파일을 편집 하 여 두 개의 바로 가기 속성을 시작 메뉴 바로 가기에 추가 합니다. 아래와 같이 #4 단계에서 GUID가로 묶여 있는지 확인 `{}` 합니다.

**Product. wxs**

```xml
<Shortcut Id="ApplicationStartMenuShortcut" Name="Wix Sample" Description="Wix Sample" Target="[INSTALLFOLDER]WixSample.exe" WorkingDirectory="INSTALLFOLDER">
                    
    <!--AUMID-->
    <ShortcutProperty Key="System.AppUserModel.ID" Value="YourCompany.YourApp"/>
    
    <!--COM CLSID-->
    <ShortcutProperty Key="System.AppUserModel.ToastActivatorCLSID" Value="{replaced-with-your-guid-C173E6ADF0C3}"/>
    
</Shortcut>
```

> [!IMPORTANT]
> 실제로 알림을 사용 하려면 AUMID 및 CLSID를 사용 하 여 바로 가기 키가 표시 되도록 정상적으로 디버깅 하기 전에 설치 관리자를 통해 앱을 설치 해야 합니다. 시작 바로 가기가 있는 후 Visual Studio에서 F5 키를 사용 하 여 디버그할 수 있습니다.


#### <a name="step-52-register-aumid-and-com-server"></a>5.2 단계: AUMID 및 COM 서버 등록

그런 다음, 응용 프로그램의 시작 코드 (알림 Api를 호출 하기 전에)에서 설치 관리자에 관계 없이 **RegisterAumidAndComServer** 메서드를 호출 하 여 #4 단계에서 알림 활성기 클래스를 지정 하 고 위에서 사용 했던 AUMID를 지정 합니다.

```cpp
// Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"YourCompany.YourApp", __uuidof(NotificationActivator));
```

MSIX/sparse 패키지와 클래식 Win32를 모두 지 원하는 경우에는이 메서드를 자유롭게 호출할 수 있습니다. MSIX 또는 스파스 패키지에서 실행 하는 경우이 메서드는 단순히를 즉시 반환 합니다. 코드를 분기 하지 않아도 됩니다.

이 방법을 사용 하면 AUMID를 지속적으로 제공 하지 않고도 호환성 Api를 호출 하 여 알림을 보내고 관리할 수 있습니다. 그리고 COM 서버에 대 한 LocalServer32 레지스트리 키를 삽입 합니다.


## <a name="step-6-register-com-activator"></a>6 단계: COM 활성기 등록

MSIX/sparse 패키지와 클래식 Win32 앱 모두에 대해 알림 활성화를 처리할 수 있도록 알림 활성기 유형을 등록 해야 합니다.

앱의 시작 코드에서 다음 **Registeractivator** 메서드를 호출 합니다. 이는 알림 활성화를 수신 하기 위해 호출 해야 합니다.

```cpp
// Register activator type
hr = DesktopNotificationManagerCompat::RegisterActivator();
```


## <a name="step-7-send-a-notification"></a>7 단계: 알림 보내기

Notification notification manager **호환성** 을 사용 하 여 **to **notification을 만드는 경우를 제외 하 고는 UWP 앱과 동일 합니다. 호환 라이브러리는 MSIX/sparse 패키지와 클래식 Win32의 차이를 자동으로 처리 하므로 코드를 포크 하지 않아도 됩니다. 클래식 Win32의 경우 호환성 라이브러리는 **RegisterAumidAndComServer** 를 호출할 때 제공한 AUMID를 캐시 하므로 AUMID를 제공 하거나 제공 하지 않을 시기를 걱정 하지 않아도 됩니다.

레거시 Windows 8.1 알림 템플릿이 #4 단계에서 만든 COM 알림 활성기를 활성화 하지 않으므로 아래에 표시 된 대로 **To generic** 바인딩을 사용 해야 합니다.

> [!IMPORTANT]
> Http 이미지는 자신의 매니페스트에 인터넷 기능이 있는 MSIX/sparse 패키지 앱 에서만 지원 됩니다. 클래식 Win32 앱은 http 이미지를 지원 하지 않습니다. 로컬 앱 데이터에 이미지를 다운로드 하 고 로컬에서 참조 해야 합니다.

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
        hr = DesktopNotificationManagerCompat::CreateToastNotification(doc.Get(), &toast);
        if (SUCCEEDED(hr))
        {
            // And show it!
            hr = notifier->Show(toast.Get());
        }
    }
}
```

> [!IMPORTANT]
> 클래식 Win32 앱은 레거시 알림 템플릿 (예: ToastText02)을 사용할 수 없습니다. COM CLSID를 지정 하면 레거시 템플릿의 활성화가 실패 합니다. 위에 표시 된 대로 Windows 10 To Generic 템플릿을 사용 해야 합니다.


## <a name="step-8-handling-activation"></a>8 단계: 활성화 처리

사용자가 알림 또는 toast의 단추를 클릭 하면 **Notificationactivator** 클래스의 **Activate** 메서드가 호출 됩니다.

Activate 메서드 내에서 알림 메시지에 지정 된 args를 구문 분석 하 고 사용자가 입력 하거나 선택한 사용자 입력을 가져온 다음 적절 하 게 앱을 활성화할 수 있습니다.

> [!NOTE]
> **Activate** 메서드는 주 스레드의 separte 스레드에서 호출 됩니다.

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

앱이 닫혀 있는 동안 정상적으로 시작을 지원 하려면 WinMain 함수에서, 알림에서 시작 하 고 있는지 여부를 확인 하는 것이 좋습니다. 알림에서 시작 하는 경우 "-ToastActivated 됨"의 시작 인수가 있습니다. 이 메시지가 표시 되 면 일반적인 시작 활성화 코드의 실행을 중지 하 고, 필요한 경우 **Notificationactivator** 에서 windows 시작을 처리 하도록 허용 해야 합니다.

```cpp
// Main function
int WINAPI wWinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE, _In_ LPWSTR cmdLineArgs, _In_ int)
{
    RoInitializeWrapper winRtInitializer(RO_INIT_MULTITHREADED);

    HRESULT hr = winRtInitializer;
    if (SUCCEEDED(hr))
    {
        // Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
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


### <a name="activation-sequence-of-events"></a>이벤트 활성화 시퀀스

활성화 시퀀스는 다음과 같습니다.

앱이 이미 실행 중인 경우:

1. **Notificationactivator** 에서 **활성화** 를 호출 합니다.

앱이 실행 되 고 있지 않은 경우:

1. 앱이 EXE를 실행 하 여 "-ToastActivated 됨"의 명령줄 인수를 가져옵니다.
2. **Notificationactivator** 에서 **활성화** 를 호출 합니다.


### <a name="foreground-vs-background-activation"></a>포그라운드 vs 백그라운드 활성화
데스크톱 앱의 경우 포그라운드 및 백그라운드 활성화는 동일 하 게 처리 됩니다. COM 활성기가 호출 됩니다. 창 표시 여부를 결정 하는 응용 프로그램의 코드는 단순히 작업을 수행 하 고 종료 하는 방법을 결정 하는 것입니다. 따라서 알림 콘텐츠에서 배경 **activationType** 지정 **background** 하면 동작이 변경 되지 않습니다.


## <a name="step-9-remove-and-manage-notifications"></a>9 단계: 알림 제거 및 관리

알림 제거 및 관리는 UWP 앱과 동일 합니다. 그러나 클래식 Win32를 사용 하는 경우 AUMID를 제공 하는 것에 대해 걱정 하지 않아도 되는 호환 라이브러리를 사용 하 여 **DesktopNotificationHistoryCompat** 를 얻는 것이 좋습니다.

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


## <a name="step-10-deploying-and-debugging"></a>10 단계: 배포 및 디버깅

MSIX/sparse 패키지 앱을 배포 하 고 디버그 하려면 패키지 [된 데스크톱 앱 실행, 디버그 및 테스트](/windows/uwp/porting/desktop-to-uwp-debug)를 참조 하세요.

클래식 Win32 앱을 배포 하 고 디버그 하려면 AUMID 및 CLSID의 시작 바로 가기가 존재 하도록 정상적으로 디버깅 하기 전에 설치 관리자를 통해 앱을 설치 해야 합니다. 시작 바로 가기가 있는 후 Visual Studio에서 F5 키를 사용 하 여 디버그할 수 있습니다.

알림이 클래식 Win32 앱에 표시 되지 않고 예외가 throw 되지 않는 경우에는 바로 가기 키가 없거나 (설치 관리자를 통해 앱을 설치 하는 경우), 코드에서 사용 된 AUMID 시작 바로 가기의 AUMID 일치 하지 않을 수 있습니다.

알림이 표시 되지만 동작 센터 (popup이 해제 된 후 사라짐)에서 지속 되지 않는 경우 COM 활성기를 올바르게 구현 하지 않은 것입니다.

MSIX/sparse 패키지와 클래식 Win32 앱을 모두 설치한 경우 알림 활성화를 처리할 때 MSIX/sparse 패키지 앱이 클래식 Win32 앱을 대체 합니다. 즉, 클래식 Win32 앱의 알림을 클릭 하면 MSIX/sparse 패키지 앱을 계속 해 서 시작 합니다. MSIX/sparse 패키지 앱을 제거 하면 정품 인증을 다시 클래식 Win32 앱으로 되돌립니다.

를 수신 하 `HRESULT 0x800401f0 CoInitialize has not been called.` `CoInitialize(nullptr)` 는 경우 api를 호출 하기 전에 앱에서를 호출 해야 합니다.

호환성 Api를 호출 하는 동안 수신 하는 경우 `HRESULT 0x8000000e A method was called at an unexpected time.` 필요한 Register 메서드를 호출 하지 못했음을 의미 합니다. 또는 MSIX/sparse 패키지 앱의 경우 현재 앱을 MSIX/스파스 컨텍스트에서 실행 하 고 있지 않습니다.

많은 `unresolved external symbol` 컴파일 오류가 발생 하면 `runtimeobject.lib` #1 단계에서 추가 **종속성** 에 추가 하지 않을 수 있습니다 (또는 디버그 구성에만 추가 하 고 릴리스 구성은 추가 하지 않음).


## <a name="handling-older-versions-of-windows"></a>이전 버전의 Windows 처리

Windows 8.1를 지원 하는 경우에는 **Desktopnotificationmanagercompat** api를 호출 하거나 To generic 알림을을 보내기 전에 Windows 10에서 실행 중인지 여부를 런타임에 확인 합니다.

Windows 8은 알림 메시지를 도입 했지만 ToastText01와 같은 [레거시 알림 템플릿을](https://docs.microsoft.com/previous-versions/windows/apps/hh761494(v=win.10))사용 했습니다. 알림을는 유지 되지 않는 간단한 팝업만 이므로 **Toastnotification** 클래스의 메모리 내 **활성화** 이벤트에 의해 활성화가 처리 되었습니다. Windows 10은 [대화형 To generic 알림을](adaptive-interactive-toasts.md)를 도입 했으며, 알림이 며칠 동안 지속 되는 작업 센터도 도입 했습니다. 알림 센터의 도입으로 COM 활성기를 도입 했으므로 알림을 만든 후 며칠 동안 활성화할 수 있습니다.

| OS | To Generic | COM 활성기 | 레거시 알림 템플릿 |
| -- | ------------ | ------------- | ---------------------- |
| Windows 10 | 지원됨 | 지원됨 | 지원 됨 (COM 서버를 활성화 하지 않음) |
| Windows 8.1/8 | N/A | 해당 없음 | 지원 여부 |
| Windows 7 및 낮음 | N/A | N/A | 해당 없음 |

Windows 10에서 실행 되 고 있는지 확인 하려면 헤더를 포함 하 `<VersionHelpers.h>` 고 **IsWindows10OrGreater** 메서드를 확인 합니다. True가 반환 되 면이 설명서에 설명 된 모든 메서드를 계속 호출 합니다. 

```cpp
#include <VersionHelpers.h>

if (IsWindows10OrGreater())
{
    // Running Windows 10, continue with sending Windows 10 toasts!
}
```


## <a name="known-issues"></a>알려진 문제

**수정 됨: 알림 클릭 후 앱이 집중 되지 않습니다**. 빌드 15063 이전 버전에서는 COM 서버를 활성화할 때 포그라운드 권한이 응용 프로그램에 전송 되지 않습니다. 따라서 앱은 포그라운드로 이동 하려고 할 때만 깜박입니다. 이 문제에 대 한 해결 방법이 없습니다. 빌드 16299 이상에서이 문제를 해결 했습니다.


## <a name="resources"></a>리소스

* [GitHub의 전체 코드 샘플](https://github.com/WindowsNotifications/desktop-toasts)
* [데스크톱 앱의 알림 메시지](toast-desktop-apps.md)
* [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)
