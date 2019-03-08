---
title: Xbox Live 코드 XDK에서 UWP에 이식
description: Xbox Live 코드 Xbox 개발 키트 (XDK) 플랫폼에서 유니버설 Windows 플랫폼 (UWP)에 이식 하는 방법에 알아봅니다.
ms.assetid: 69939f95-44ad-4ffd-851f-59b0745907c8
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, xdk에서 이식
ms.localizationpriority: medium
ms.openlocfilehash: c6e8a6ebe716f1e062940066184e9f734441371b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590818"
---
# <a name="porting-xbox-live-code-from-the-xbox-developer-kit-xdk-to-universal-windows-platform-uwp"></a>Xbox Live 코드 이식 Xbox 개발자 키트 (XDK)에서 유니버설 Windows 플랫폼 (UWP)

## <a name="introduction"></a>소개

이 문서에서는 개발자가 Xbox One XDK Xbox Live 코드를 Windows 10 유니버설 Windows 플랫폼 (UWP) 마이그레이션 시작에 사용한 적이 있는 것입니다.

이 마이그레이션의 일부로 XSAPI 1.0 (Xbox Live Services API를 통해 2015 년 8 월 Xbox One XDK 포함)에서 전환 하기 위해 포함 XSAPI 2.0 (2015 년 11 월부터 Xbox One XDK 포함 및 Xbox Live SDK 에서도 사용 가능 합니다. 이러한 Api의 기능을 거의 동일 하지만 몇 가지 중요 한 구현의 차이점이 있습니다.

등의 Windows 개발 컴퓨터를 준비 하 고이 문서에서 다룰 주제 및 Xbox Live 서비스는 Secure Sockets API와 같은 클라우드 기반 관리에 대 한 연결 된 저장소 API를 사용 하는 경우 필요한 기타 Api를 일반적으로 설치 게임을 저장합니다.

<a name="_Setting_up_and"></a>

## <a name="setting-up-and-configuring-your-project-in-partner-center-and-xdp"></a>설정 및 파트너 센터 및 XDP 프로젝트 구성

Xbox Live를 사용 하는 UWP 제목을 서비스에서 구성 해야 [파트너 센터](https://partner.microsoft.com/dashboard)합니다. 최신 정보를 참조 하세요 [신규 또는 기존 UWP 프로젝트에 추가 Xbox Live](../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) Xbox Live 프로그래밍 가이드에 포함 된 합니다 [Xbox Live SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx)합니다.

해당 페이지에 대 한 항목을 제목에서 Xbox Live 서비스를 사용 하기 위한이 단계를 같습니다.

-   파트너 센터에서 UWP 앱 프로젝트를 만듭니다.

-   Xbox Live 사용량에 대 한 프로젝트를 설정 하려면 XDP를 사용 합니다.

-   파트너 센터 제품 XDP 제품에 연결 합니다.

-   XDP (샌드박스에 Xbox Live 타이틀을 실행 하는 경우 필요 함)에서 개발자 계정을 만듭니다.

제목을 멀티 플레이 지 원하는 경우 멀티 플레이 세션 템플릿에서 몇 가지 추가 설정이 필요할 수 있습니다. MPSD (멀티 플레이 세션 문서)를 쓸 및 Xbox Live 멀티 플레이어를 사용 하는 모든 Windows 10 제목을 세션 템플릿을 "기능" 목록에서이 새 필드를 필요: ```userAuthorizationStyle: true```합니다.

### <a name="enabling-cross-play"></a>교차 플레이 사용 하도록 설정

세션 템플릿을에이 기능을 추가할 필요는 "간 플레이" (공유 Xbox Live 구성 Xbox One 및 PC 게임, 장치 간 멀티 플레이 게임 허용 사이)를 지원할 경우: **crossPlay: true**합니다.

XDP에서 간 play 및 구성 요구 사항을 지원에 대 한 자세한 내용은 "수집 XDK 및 UWP 간 Play 제목에서 XDP" Xbox Live Programming guide에서를 참조 하세요.

또한 프로그래밍 방식으로 몇 가지 고려 사항에 대 한 이후 섹션을 참조 [Xbox One 및 PC 간에 멀티 플레이어 게임 간 실행을 지 원하는](#_Supporting_multiplayer_cross-play)합니다.

## <a name="setting-up-your-windows-development-environment"></a>Windows 개발 환경 설정

1.  [최신 다운로드 **Xbox Live SDK** ](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx) 로컬로 추출 합니다.

2.  [설치를 **Xbox Live 플랫폼 확장 SDK** ](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx) UWP에 대 한 Secure Sockets API 및/또는 게임 저장 API (즉, 연결 저장소)를 해야 합니다.

3.  Visual Studio에서 유니버설 Windows 앱 프로젝트에 지원을 Xbox Live를 추가 합니다. 전체 소스를 추가 하거나 Visual Studio 프로젝트에 NuGet 패키지를 설치 하 여 이진 파일을 참조할 수 있습니다. 패키지는 c + + 및 WinRT 모두 사용할 수 있습니다. 자세한 내용을 참조 하세요. [신규 또는 기존 UWP 프로젝트에 추가 Xbox Live](../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)

4.  샌드박스에 사용 하도록 개발 컴퓨터를 구성 합니다. 관리자 명령 프롬프트에서 사용할 수 있는 Xbox Live SDK의 Tools 디렉터리에는 명령줄 스크립트 (예: SwitchSandbox.cmd XDKS.1)입니다.

  **참고** 소매 샌드박스도 다시 전환 하거나 스크립트를 수정 하는 레지스트리 키를 삭제할 수 있습니다 또는 소매를 호출 하는 샌드박스를 전환할 수 있습니다.

1.  개발 컴퓨터에 개발자 계정을 추가 합니다. XDP에서 만든 개발자 계정에 할당 된 샌드박스 개발 하거나 샘플을 실행 하는 경우 런타임 시 Xbox Live 서비스와 상호 작용 하는 데 필요한 경우 Windows에 하나 이상의 계정을 추가 합니다.

    1.  오픈 **설정을** (바로 가기: Windows 키 + I)입니다.

    2.  오픈 **계정**합니다.

    3.  에 **계정의** 탭을 클릭 **Microsoft 계정 추가**합니다.

    4.  개발자 계정 전자 메일 및 암호를 입력 합니다.

### <a name="appxmanifest-changes"></a>AppxManifest 변경

Appxmanifest.xml 파일의 Xbox 및 UWP 버전 간의 가장 일반적인 변경은 다음과 같습니다.

1. 패키지 Id의 UWP 개발 중에 중요합니다. Id 이름 및 게시자 둘 다 *일치 해야* UWP 앱에 대 한 파트너 센터에 정의 된 것입니다.

1. 패키지 종속성 섹션을 반드시 입력 해야 합니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

```xml
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Universal" MinVersion="10.0.10240.0" MaxVersionTested="10.0.10240.0" />
  </Dependencies>
```

1.  예제에서는 UWP 응용 프로그램 매니페스트 (예를 들어, Xbox Live SDK에 포함 된 UWP 샘플의 Visual Studio에서 만든 기본 유니버설 Windows 앱 프로젝트는 하나)에 대 한 특정 요구 사항이 있는 응용 프로그램 매니페스트의 다른 섹션에 대 한 참조 UWP 같은 ```<VisualElements>```합니다.

1.  제목 및 서비스 안내 xboxservices.config 파일에 정의 된 (참조를 [다음 섹션](#_Define_your_title)) 대신 "xbox.live" 확장 범주에 있습니다.

1.  "Xbox.system.resources" 확장 범주 필요 하지 않습니다.

1.  보안 소켓 networkmanifest.xml 파일에 정의 됩니다 (참조 [Secure sockets](#_Secure_sockets)) 대신 "windows.xbox.networking" 범주에 있습니다.

1.  UWP 제목에 Xbox Live 초대를 받으려면 "windows.protocol" 확장 범주를 정의 해야 합니다 (참조 [초대 받고 보내기](#_Sending_and_receiving)).

1.  내부 마이크 장치 기능을 추가 해야 GameChat API를 사용 하는 경우는 ```<Capabilities>``` 요소입니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

  ```<DeviceCapability Name="microphone">```

<a name="_Define_your_title"></a>

### <a name="define-your-title-and-scid-for-the-xbox-live-sdk-in-a-config-file"></a>보내 주신 제목과 서비스 안내 구성 파일에서 Xbox Live SDK에 대 한 정의

Xbox Live SDK는 UWP 타이틀에 대 한 appxmanifest.xml에 더 이상 포함 된 ID 및 서비스를 안내를 제목 알아야 합니다. 라는 텍스트 파일을 만드는 대신 **xboxservices.config** 프로젝트의 루트 디렉터리 및 바꿀 값을 제목에 대 한 정보를 사용 하 여 다음 필드를 추가 합니다.

```
{
  "TitleId": 123456789,
  "PrimaryServiceConfigId": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
}
```

> [!NOTE]
> Xboxservices.config 내 모든 값은 대/소문자 구분입니다.

빌드 출력에 사용할 수 있도록 프로젝트의 내용으로이 구성 파일을 포함 합니다.

**참고** 다음 API를 사용 하 여 이러한 값을 제목 내에서 프로그래밍 방식으로 제공 됩니다.

```cpp
Microsoft::Xbox::Services::XboxLiveAppConfiguration^ xblConfig = xblContext->AppConfig;
unsigned int titleId = xblConfig->TitleId;
Platform::String^ scid = xblConfig->ServiceConfigurationId;
```

### <a name="api-namespace-mapping"></a>API 네임 스페이스 매핑

표 1. Namespace 매핑은 XDK에서 UWP로 합니다.

<table>
  <tr>
    <td></td>
    <td><b>Xbox One XDK</b></td><td><b>UWP</b></td>
    <td><b>API는 사용할 수 있습니다...</b></td>
  </tr>
  <tr>
    <td>Xbox 서비스 API (XSAPI)</td>
    <td>Microsoft::Xbox::Services</td>
    <td>Microsoft::Xbox::Services (<i>변하지</i>)</td>
    <td>Xbox Live SDK (사용 하 여 NuGet 이진 또는 소스)</td>
  </tr>
  <tr>
    <td>GameChat</td>
    <td>
Microsoft::Xbox::GameChat Windows::Xbox::Chat </td>
    <td>
Microsoft::Xbox::GameChat (*변하지*) Microsoft::Xbox::ChatAudio </td>
    <td>
Xbox Live SDK (이진 NuGet 사용 하는 경우) </td>
  </tr>
  <tr>
    <td>SecureSockets</td>
    <td>Windows::Xbox::Networking</td>
    <td>Windows::Networking::XboxLive</td>
    <td>Xbox Live SDK 플랫폼 확장</td>
  </tr>
  <tr>
    <td>연결된 저장소</td>
    <td>Windows::Xbox::Storage</td>
    <td>Windows::Gaming::XboxLive::Storage</td>
    <td>Xbox Live SDK 플랫폼 확장</td>
  </tr>
</table>

### <a name="multiplayer-subscriptions-and-event-handling"></a>멀티 플레이 구독 및 이벤트 처리

대부분의 멀티 플레이어 제목 발생 하는 XSAPI 2.0 XSAPI 1.0의 주요 변경 내용 중 하나는 여러 메서드 및 이벤트를 이동 하는 합니다 **RealTimeActivityService** 에 **MultiplayerService**.

예를 들어 다음과 같은 가치를 제공해야 합니다.

-   **EnableMultiplayerSubscriptions()\*** method

-   **DisableMultiplayerSubscriptions()** method

-   **MultiplayerSessionChanged** 이벤트

-   **MultiplayerSubscriptionLost** 이벤트

-   **MultiplayerSubscriptionsEnabled** 속성

**중요 한 구현** 하지 명시적으로 사용 하 고 어떤 항목도 있지만 합니다 **RealTimeActivityService** 를 이러한 이벤트와 메서드를 이동한 후를 **MultiplayerService**를 계속 호출 해야 합니다 **xblContext-&gt;RealTimeActivityService-&gt;Activate()** 호출 하기 전에 **EnableMultiplayerSubscriptions()** 멀티 플레이 구독에는 RTA 서비스가 필요 때문에 합니다.

## <a name="whats-handled-differently-in-uwp"></a>UWP에서 다르게 처리 하는 항목

다음은 새 발생 가능성이 XDK와 UWP의 차이점에 갖게 되는 코드 섹션의 매우 높은 수준의 목록 [NetRumble 샘플](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx) (XDK 및 UWP 버전 포함):

-   제목 ID 및 서비스 안내 정보 액세스

-   사전 실행 활성화 (신규 UWP 용)

-   PLM 처리를 일시 중단/재개

-   확장된 실행 (새 UWP 용)

-   Xbox **사용자** 개체 및 사용자 처리 차이점

    -   로그인 및 로그 아웃 처리

    -   컨트롤러 (Xbox에서 처리)만 페어링

    -   Gamepad 처리

-   멀티 플레이 권한 확인

-   Xbox One 및 PC 간에 멀티 플레이어 게임 간 실행 지원

-   게임 초대 보내기

    -   게임-UWP의 n/a에서에서 타사 앱을 열 수

    -   게임-UWP의 n/a에서에서 파티 멤버를 열거 하는 기능

-   게이머 프로필 표시

-   보안 소켓 API 화면이 변경

-   QoS 관리 측정 및 결과 처리

-   게임 이벤트를 작성합니다.

-   GameChat: 이벤트, 설정 및 ChatUser 개체

-   저장소 API 화면이 변경 연결

-   PIX 이벤트 (Xbox; 에서만 다루지 않음이 백서의 내용)

-   렌더링 차이점

다음 섹션에서는 이러한 차이점 중 상당수에 더 자세히로 이동합니다.

### <a name="accessing-title-id-and-scid-info"></a>제목 ID 및 서비스 안내 정보 액세스

인스턴스의 AppConfig 속성을 통해 제목 ID와 서비스 구성 ID를 UWP에 액세스를 **XboxLiveContext**합니다.

```cpp
Microsoft::Xbox::Services::XboxLiveAppConfiguration^ xblConfig = xblContext->AppConfig;
unsigned int titleId = xblConfig->TitleId;
Platform::String^ scid = xblConfig->ServiceConfigurationId;
```

**참고** XDK를 가져올 수 있습니다 이러한 Id를 사용 하 여 이러한 새 속성의 이전 정적 속성 **Windows::Xbox::Services::XboxLiveConfiguration**합니다.

### <a name="prelaunch-activation"></a>사전 실행 활성화

Windows 10에서 자주 사용 하는 타이틀의 사전 사용자가 로그인 할 때 실행 되었다는 지 뜻 될 수 있습니다. 이 처리 하기에 제목에 대 한 시작 인수를 확인 하는 코드 있어야 **PreLaunchActivated**합니다. 예를 들어, 하려는 경우 이러한 종류의 정품 인증 하는 동안 모든 리소스를 로드 합니다. 자세한 내용은 MSDN 문서를 참조 하세요 [핸들 앱 사전 실행](https://msdn.microsoft.com/library/windows/apps/mt593297.ASPx)합니다.

### <a name="suspendresume-plm-handling"></a>PLM 처리를 일시 중단/재개

일시 중단 및 다시 시작 하 고 일반적으로 PLM Xbox One;에서 작동 하는 방식과 유니버설 Windows 앱에서 비슷하게 작동 그러나 가지 염두에 몇 가지 중요 한 차이점이 있습니다.

-   방법이 없는 **Constrained** PC에서 상태-이 Xbox One 단독 개념입니다.

-   제목; 최소화 되는 즉시 시작 일시 중단 이 문제를 해결 하는 방식으로 섹션을 참조 하세요 [확장 실행](#_Extended_execution)합니다.

-   타이밍 다릅니다: 5 초에서 pc 대신 콘솔에 1 초 일시 중지 해야 합니다.

연결 된 저장소를 사용 하는 경우 또 다른 중요 한 고려 사항은 새 **ContainersChangedSinceLastSync** UWP 버전이 API의 속성입니다. 다시 시작 이벤트를 처리할 때이 속성을 제목 일시 중단 된 동안 클라우드에서 컨테이너를 모두 변경 여부를 확인할 수 있습니다. 이 플레이어 한 대의 PC에서 게임을 일시 중단, 다른 곳에서 재생 및 첫 번째 PC를 반환한 경우 발생할 수 있습니다. 했습니다를 읽는 경우 데이터이 컨테이너에서 메모리로 일시 중단 했습니다 전에 아마도 읽기를 다시 변경 내용과 그에 따라 변경 내용을 처리 하려고 합니다.

Windows 10에서 UWP 앱에서 PLM를 처리 하는 방법에 대 한 자세한 내용은 MSDN 문서를 참조 하세요 [Launching, resuming, 백그라운드 작업 및](https://msdn.microsoft.com/library/windows/apps/xaml/mt227652.aspx)합니다.

찾을 수 있습니다를 [Xbox One에 대 한 PLM](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx) 백서 GDN 게임을 염두에 두고 작성 되었습니다 것 이므로 대부분 앱 수명 주기를 처리 하는 것에 대 한 개념의 PC에 여전히 유용 합니다.

<a name="_Extended_execution"></a>

### <a name="extended-execution"></a>확장된 실행

UWP 최소화 PC에서 앱 일반적으로 결과에 즉시 일시 중단 하기 시작 합니다. 확장된 실행을 사용 하 여이 프로세스를 지연 시킬 수가 있습니다. 구현 예제:

```cpp
using namespace Windows::ApplicationModel::ExtendedExecution;
//If this goes out of scope the request is nullified
ExtendedExecutionSession^ session;
void App::RequestExtension()
{
       if (!session)
       {
              session = ref new ExtendedExecutionSession();
       }
       session->Reason = ExtendedExecutionReason::Unspecified;
       session->Description = "foo";
       session->Revoked += ref new TypedEventHandler<Platform::Object^, ExtendedExecutionRevokedEventArgs^>(this, &App::ExtensionRevokedHandler);
       IAsyncOperation<ExtendedExecutionResult>^ request = session->RequestExtensionAsync();
       //At this point the request has been made. When the IAsyncOperation request completes, verify that the ExtendedExecutionResult == Allowed and you will not suspend for up to 10 minutes while minimized.
}

void App::ExtensionRevokedHandler(Platform::Object^ obj, ExtendedExecutionRevokedEventArgs^ args)
{
       if (args->Reason == Windows::ApplicationModel::ExtendedExecutionRevokedReason::Resumed)
       {
              //Request the extension again in preparation for the next suspend.
RequestExtension();
       }
       //The app will either complete suspending if the extension was revoked by system policy or resume running if the user has switched back to the app.
}

```

후 합니다 **ExtensionRevokedHandler** 되었습니다 향후의 잠재적인 일시 중단에 대 한 요청 수 해야 새 확장을 호출 합니다. 합니다 **ExtensionRevokedHandler** 시스템에 메모리 부족, 10 분 경과 하거나 게임 최소화 하는 동안 사용자가 게임으로 다시 전환할 때 호출 됩니다. 따라서 **RequestExtension()** 가능성이 다음이 시간에 호출 해야 합니다.

-   중 시작 합니다.

-   에 **ExtensionRevokedHandler** 때 args-&gt;이유 재개 (10 분 타이머가 만료 되기 전에 게임 최소화 된 상태의 때 탭 사용자) = =.

-   에 **OnResuming** 처리기 (제목 메모리 압력 또는 10 분짜리 타이머 인해 일시 중단 된) 경우입니다.

### <a name="handling-users-and-controllers"></a>사용자 및 컨트롤러를 처리합니다.

Windows를에서 한 번에 하나에서 로그인 한 사용자를 사용 하 여 작동 있습니다. Xbox Live SDK를 먼저 만듭니다는 **XboxLiveUser** 개체, Xbox Live에 로그인 하 고 만든 **XboxLiveContext** 이 사용자의 개체입니다.

이전에 Xbox One XDK:

1.  사용자 (예를 들어 gamepad 상호 작용을)를 획득 합니다.
2.  만들기는 **XboxLiveContext** 해당 사용자에서:
  ```
  ref new Microsoft::Xbox::Services::XboxLiveContext( Windows::Xbox::System::User^ user )
  ```
1.  처리를 **SignOut** 이벤트:
  ```
  Windows::Xbox::System::User::SignOutStarted
  ```
1.  Gamepad/컨트롤러를 사용 하 여 연결을 처리 합니다.
  ```
  Windows::Xbox::Input::Controller::ControllerRemoved
  Windows::Xbox::Input::Controller::ControllerPairingChanged
  ```

이제는 UWP/Xbox 용 Live SDK:

1.  만들기는 **XboxLiveUser**:

  ```
  auto xblUser = ref new Microsoft::Xbox::Services::System::XboxLiveUser();
  ```

1.  UI를 사용 하 여 해당의 도움 없이 사용 하는 마지막 Microsoft 계정을 사용 하 여 로그인 하려고 합니다.

  ```
  xblUser->SignInSilentlyAsync();
  ```

1.  받게 되 면 **SignInResult::Success** 이 비동기 작업의 결과 만듭니다를 **XboxLiveContext**, 완료 하 합니다.

  ```
  auto xblContext = ref new Microsoft::Xbox::Services::XboxLiveContext( xblUser );
  ```

1.  발생 하는 대신 **SignInResult::UserInteractionRequired**, 대화형 로그인 메서드를 호출한 시스템 UI 표시 해야 합니다.

  ```
  xblUser->SignInAsync();
  ```

1.  여기에서 발생할 수 있습니다 **SignInResult::UserCancel**에서 로그인 한 사용자가 없는 경우 고 다시 로그인 하기 위해 메뉴 옵션을 제공 하는 것이 좋습니다.

  **참고** 메뉴 옵션을 제공 하는 경우 다른 Microsoft 계정으로 전환 하는 옵션 제공는 것이 좋습니다.

  ```
  xblUser->SwitchAccountAsync( nullptr );
  ```

1.  연결 하려는 로그인 사용자를 만든 후는 **XboxLiveUser::SignOutCompleted** 이벤트 로그 아웃 하는 사용자에 대응할 수 있도록 합니다.

  ```
  xblUser->SignOutCompleted += ref new Windows::Foundation::EventHandler<Microsoft::Xbox::Services::System::SignOutCompletedEventArgs^>( &OnSignOutCompleted );
  ```

1.  Windows 10에 처리할 페어링 컨트롤러인 있습니다.

이것은 c + +에 대 한 간단한 예 / WinRT 합니다. 더 자세한 예제를 Xbox Live 프로그래밍 가이드의 "Windows 10에서에서 라이브 인증 Xbox"를 참조 하세요. "새 UWP 프로젝트에 추가 Xbox Live"에서 더 광범위 한 예제를 찾아볼 수도 있습니다 유용 합니다.

### <a name="checking-multiplayer-privileges"></a>멀티 플레이 권한 확인

에 해당 **CheckPrivilegeAsync()** 아직 Xbox Live SDK에서 지원 되지 않습니다. 지금은 반환 된 문자열 목록에 필요한 권한에 대 한 검색 해야 합니다 **권한** 속성에 대 한는 **XboxLiveUser**합니다. 예를 들어, 다중 접속 권한을 확인 하려면 검색할 권한 "254." XDK 설명서 사용 권한의 모든는 Xbox Live의 목록을 찾을 수 있습니다 합니다 **Windows::Xbox::ApplicationModel::Store::KnownPrivileges** 열거형입니다.

이 항목의 내용은 포럼 게시물을 참조 하세요 [xsapi 및 사용자 권한을](https://forums.xboxlive.com/questions/48513/xsapi-user-privileges.html)합니다.

<a name="_Supporting_multiplayer_cross-play"></a>

### <a name="supporting-multiplayer-cross-play-between-xbox-one-and-pc-uwp"></a>Xbox One 및 PC UWP 사이의 멀티 플레이어 게임 간 실행 지원

XDP에서 새 세션 템플릿 요구 사항 외에도 (참조 [설정 및 파트너 센터 및 XDP 프로젝트를 구성](#_Setting_up_and)), 간 play 세션 조인 기능에 대 한 새 제한 함께 제공 됩니다. "None" 세션 조인 제한으로 더 이상 사용할 수 없습니다. "팔" 또는 "Local" (기본 제한 "Local"을은)를 사용 해야 합니다.

또한 조인 및 읽기 제한 기본적으로 "Local" 필요한 때문 **userAuthorizationStyle** 멀티 플레이어 게임 Windows 10 기능입니다.

포럼 기사의 [공용 멀티 플레이 세션을 만들 수는](https://forums.xboxlive.com/questions/46781/is-it-possible-to-create-public-multiplayer-sessio.html), 추가 정보를 포함 합니다.

추가 정보 및 예제는 업데이트 된 멀티 플레이어 개발자 순서도, 간 재생 가능한 멀티 플레이 샘플 NetRumble, 또는 개발자 계정 관리자 (파악)에서 찾을 수 있습니다.

<a name="_Sending_and_receiving"></a>

### <a name="sending-and-receiving-invites"></a>초대를 보내고

초대 보내기 위한 UI를 표시 하는 API는 이제 **Microsoft:: Xbox::Services::System::TitleCallableUI::ShowGameInviteUIAsync()** 합니다. 세션-전달&gt; **SessionReference** 활동 세션 (일반적으로 경우 로비)에서 개체입니다. 필요에 따라 참조를 사용자 지정 초대 XDP에서 서비스 구성에 정의 된 된 문자열 ID는 두 번째 매개 변수에 전달할 수 있습니다. 있습니다 정의한 문자열 초대 된 플레이어를 보낼 알림 메시지에에서 표시 됩니다. Note는 어떤에서 전달 하는이 메서드는 매개 변수는 ID 번호와 서비스에 대해 올바르게 포맷 되어야 합니다. 예를 들어, "1" 문자열 ID로 전달 되어야 합니다에 "/ / / 1".

멀티 플레이 서비스를 사용 하 여 직접 초대를 전송 하려는 경우 (즉, 없이 UI를 표시), 다른 초대 메서드를 사용할 수 있습니다 **Microsoft::Xbox::Services::Multiplayer::MultiplayerService::SendInvitesAsync()** 는 사용자 로부터 **XboxLiveContext**합니다.

Windows로 들어오는 초대에 대 한 프로토콜 활성화을 제목을 허용 하려면이 확장을 추가 합니다 **&lt;응용 프로그램&gt;** appxmanifest 요소:

```xml
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-xbl-multiplayer" />
  </uap:Extension>
</Extensions>
```

Xbox One에서 이전과 마찬가지로 초대를 후 처리할 수 있습니다 때 하 **CoreApplication** 가져옵니다는 **Activated** 이벤트 및 종류가 활성화는 **ActivationKind::Protocol**.

### <a name="showing-the-gamer-profile-card"></a>게이머 프로필 카드를 표시합니다.

UWP의 게이머 프로필 카드를 팝업을 사용 하 여 **Microsoft:: Xbox::Services::System::TitleCallableUI::ShowProfileCardUIAsync()** 대상 사용자에 대 한 XUID 전달 합니다.

<a name="_Secure_sockets"></a>

### <a name="secure-sockets"></a>보안 소켓

보안 소켓 API를 별도의 있기 [Xbox Live 플랫폼 확장 SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx)합니다.

이 포럼 API 사용에 대 한 게시물을 참조 하세요. [크로스 플랫폼에 대 한 SecureDeviceAssociation 설정](https://forums.xboxlive.com/answers/45722/view.html)합니다.

**참고** UWP 용 합니다 **SocketDescriptions** appxmanifest를 고유한 섹션 움직인 [networkmanifest.xml](https://forums.xboxlive.com/storage/attachments/410-networkmanifestxml.txt)합니다. 내부의 형식을 합니다 &lt;SocketDescriptions&gt; 요소는 제외 됩니다 거의 동일 합니다 **mx:** 접두사입니다.

크로스-play Xbox 및 Windows 10 사이 여야 *있는지* 정의 된 모든 *동일 하 게* 매니페스트 (Xbox One 및 networkmanifest.xml Package.appxmanifest의 두 가지 종류 사이 Windows 10). 소켓 이름, 프로토콜 등 일치 해야 합니다 *정확 하 게*합니다.

또한 교차-재생 해야 내에서 다음 네 가지 SDA 용도 정의 하는 ```<AllowedUsages>``` 요소에 *둘 다* Xbox 하나 Package.appxmanifest 및 Windows 10 networkmanifest.xml:

```xml
<SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
<SecureDeviceAssociationUsage Type="AcceptOnMicrosoftConsole" />
<SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
<SecureDeviceAssociationUsage Type="AcceptOnWindowsDesktop" />
```

### <a name="multiplayer-qos-measurements"></a>다중 접속 QoS 측정

보안 소켓 API의 네임 스페이스 변경, 외에도 일부 개체 이름 및 값의 변경 너무 합니다. 다음 표에서 일반적으로 사용 되는 측정 상태에 대 한 매핑은 있습니다.

표 2. 일반적으로 측정 상태 매핑을 사용 합니다.

| XDK (Windows::Xbox::Networking::QualityOfServiceMeasurementStatus)  | UWP (Windows::Networking::XboxLive::XboxLiveQualityOfServiceMeasurementStatus)  |
|------------------------------------|--------------------------------------------|
| HostUnreachable                    | NoCompatibleNetworkPaths                   |
| MeasurementTimedOut                | TimedOut                                   |
| PartialResults                     | InProgressWithProvisionalResults           |
| 성공                            | 성공                                  |

맵에서 *측정* QoS (서비스 품질) 및 *결과 처리* 동일 원칙적에서 XDK 및 UWP 버전의 API 비교 하는 경우. 그러나 이름 변경 및 몇 가지 디자인 변경으로 인해 코드도 다르게 일부에서입니다.

에 대 한 QoS를 측정 하는 **XDK**에 안전한 장치 주소의 컬렉션 및 메트릭 컬렉션을 만들고이 필드를 전달 합니다 **MeasureQualityOfServiceAsync()** 메서드.

측정에 대 한 QoS **UWP**, 만든 새 **XboxLiveQualityOfServiceMeasurement()** 개체를 호출 **Append()** 에 해당 **메트릭** 및 **DeviceAddresses** 속성과 호출 개체의 **MeasureAsync()** 메서드.

예를 들어 다음과 같은 가치를 제공해야 합니다.

```cpp
auto qosMeasurement = ref new Windows::Networking::XboxLive::XboxLiveQualityOfServiceMeasurement();
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageInboundBitsPerSecond);
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageOutboundBitsPerSecond);
qosMeasurement->Metrics->Append(Windows::Networking::XboxLive::XboxLiveQualityOfServiceMetric::AverageLatencyInMilliseconds);
qosMeasurement->NumberOfProbesToAttempt = myDefaultQosProbeCount;
qosMeasurement->TimeoutInMilliseconds = myDefaultQosMeasurementTimeout;

// Add secure addresses for each session member
for (const auto& member : session->GetMembers())
{
    if (!member->IsCurrentUser)
    {
        auto sda = member->SecureDeviceAddressBase64;

        if (!sda->IsEmpty())
        {
qosMeasurement->DeviceAddresses->Append(Windows::Networking::XboxLive::XboxLiveDeviceAddress::CreateFromSnapshotBase64(sda));
        }
    }
}

if (qosMeasurement->DeviceAddresses->Size > 0)
{
    qosMeasurement->MeasureAsync();
}

```

더 많은 예제를 참조 하세요. 합니다 **MatchmakingSession::MeasureQualityOfService()** 하 고 **MatchmakingSession::ProcessQosMeasurements()** NetRumble 예제의 함수입니다.

### <a name="writing-game-events"></a>게임 이벤트를 작성합니다.

다른 API를 UWP에 타이틀의 서비스 구성에서 구성 된 게임 이벤트를 전송 합니다. Xbox Live SDK를 사용 하 여 **EventsService** 및 속성 모음 모델.

예를 들어 다음과 같은 가치를 제공해야 합니다.

```cpp
auto properties = ref new Windows::Foundation::Collections::PropertySet();
properties->Insert("RoundId", m_roundId);
properties->Insert("SectionId", safe_cast<Platform::Object^>(0));
properties->Insert("MultiplayerCorrelationId", m_multiplayerCorrelationId);
properties->Insert("GameplayModeId", safe_cast<Platform::Object^>(0));
properties->Insert("MatchTypeId", safe_cast<Platform::Object^>(0));
properties->Insert("DifficultyLevelId", safe_cast<Platform::Object^>(0));

auto measurements = ref new Windows::Foundation::Collections::PropertySet();

xblContext->EventsService->WriteInGameEvent("MultiplayerRoundStart", properties, measurements);

```

자세한 내용은 Xbox Live SDK 설명서를 참조 하십시오.

**팁** 사용할 수는 **xcetool.exe** .h 헤더 파일에 XDP에서 다운로드 한 events.man 파일 변환 하려면 Xbox Live SDK (Tools 디렉터리에 있음)를 사용 하 여 제공 합니다. 사용 된 '-x' 새 v2 속성 모음 스키마를 사용 하 여이 c + + 헤더를 생성 하는 옵션입니다. 이 헤더에는 모든 구성된에 이벤트를 호출할 수 있는 c + + 함수가 포함 되어 있습니다. 예를 들어 **EventWriteMultiplayerRoundStart()** 합니다. WinRT 인터페이스를 사용 하려는 경우 각 이벤트에 대 한 속성 및 측정 생성 되는 방법을 확인 하려면이 헤더 파일을 참조할 수 있습니다.

### <a name="game-chat"></a>게임 채팅

UWP의 GameChat 이진 NuGet 패키지로 Xbox Live SDK와 함께 포함 되어 있습니다. 이 NuGet 패키지를 프로젝트에 추가 하는 방법에 대 한 Xbox Live 프로그래밍 가이드의 지침을 참조 하세요.

기본 사용법을 XDK 및 UWP 버전 간에 거의 동일합니다. API의 몇 가지 차이점은 다음과 같습니다.

1.  합니다 **User::AudioDeviceAdded** 이벤트 UWP 제목별 후크 될 필요가 없습니다. 기본 채팅 라이브러리 핸들 장치 추가 및 제거 합니다.

2.  **ChatUser** 라고 **GameChatUser**합니다.

3.  **Microsoft::Xbox::GameChat** 네임 스페이스 동일 하지만 **Windows::Xbox::Chat** 네임 스페이스가 **Microsoft::Xbox::ChatAudio**합니다.

4.  **AddLocalUserToChatChannelAsync()** XUID a 또는 a 걸립니다 **ChatAudio::IChatUser ^** 대신는 **XboxUser**합니다.

5.  **RemoveLocalUserFromChatChannelAsync()** 필요를 **ChatAudio::IChatUser ^** 대신는 **XboxUser**합니다. 가져올 수 있습니다는 **IChatUser** 에서 **GameChatUser**-&gt;**사용자**합니다.

### <a name="connected-storage"></a>연결 된 저장소

별도의 연결 된 저장소 API가 제공 [Xbox Live 플랫폼 확장 SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx)합니다. 설명서는 Xbox Live SDK 문서에 포함 됩니다.

전반적인 흐름은 동일 하 게 Xbox One을 추가 합니다 **ContainersChangedSinceLastSync** UWP 버전에서 속성입니다. 제목에 호출한 후 다시 시작 이벤트를 처리 하는 경우이 속성을 확인할 **GetForUserAsync()** 다시을 제목 하는 동안 클라우드에서 컨테이너의 변경 내용을 확인 하려면 일시 중단 되었습니다. 데이터를 변경 하는 컨테이너 중 하나에서 메모리에 로드 된 경우 아마도를 다시 변경 내용과 그에 따라 변경 내용을 처리할 데이터를 읽을 하려고 합니다.

UWP 버전의 다른 주요 차이점은 다음과 같습니다.

1.  변경 Namespace **Windows::Xbox::Storage** 하 **Windows::Gaming::XboxLive::Storage**합니다.

2.  **ConnectedStorageSpace** 바뀝니다 **GameSaveProvider**합니다.

3.  **Windows::System::User** 는 **GetForUserAsync()** 대신는 **XboxUser**, 서비스를 안내 이제 이며 필요 합니다.

4.  "컴퓨터" 로컬 저장소 (즉, **GetForMachineAsync()** 제거 되었습니다). 사용 하는 것이 좋습니다 **Windows::Storage::ApplicationData** 대신에 비-로밍에 대 한 로컬 데이터를 저장 합니다.

5.  비동기 결과가 반환 될 예외-무료 \*결과-형식 개체 (예를 들어 **GameSaveProviderGetResult**);에서 확인할 수 있습니다 합니다 **상태** 속성 오류가 없는 경우 반환 된 개체를 읽을 합니다 **값** 속성입니다.

6.  **ConnectedStorageErrorStatus 열거형** 이름이 **GameSaveErrorStatus** 반환 되는 **상태** 결과의 속성입니다. 이전 값을 모두 있으며 몇 새로 추가 되었습니다.

-   중단

-   ObjectExpired

-   확인

-   UserHasNoXboxLiveInfo

GameSave 샘플 또는 참조 NetRumble 샘플 예를 들어 사용 합니다.

**참고** Gamesaveutil.exe xbstorage.exe (XDK는 포함 된 명령줄 개발자 유틸리티)에 해당 됩니다. Xbox Live 플랫폼 확장 SDK를 설치한 후 여기이 유틸리티를 찾을 수 있습니다. C:\\프로그램 파일 (x86)\\Windows 키트\\10\\확장명 Sdk\\XboxLive\\1.0\\Bin\\x64

## <a name="summary"></a>요약

이 백서에 설명 된 새 요구 사항을 확인 하 고 API 변경 내용은 새 uwp Xbox One XDK 기존 게임 코드를 이식 하는 경우 발생할 가능성이 있는입니다. 특정 강조 멀티 플레이어와 연결 된 저장소와 같은 Xbox Live 서비스와 관련 된 기능 영역 뿐만 아니라 응용 프로그램 및 환경 설치를 지정 했습니다. 이 문서 전체에 다음 참조를 제공 된 링크를 따르고의 "Windows 10" 섹션을 방문 해야 합니다. 자세한 내용은 합니다 [개발자 포럼](https://forums.xboxlive.com) 자세한 도움말, 답변 및 뉴스에 대 한 합니다.

## <a name="references"></a>참조

-   [Xbox One에서에서 Windows 10에 이식](https://developer.xboxlive.com/en-us/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx)

-   [Xbox One 백서](https://developer.xboxlive.com/en-us/platform/development/education/Pages/WhitePapers.aspx)

-   [샘플](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx)
