---
title: XDK에서 UWP로 Xbox Live 코드 포팅
author: KevinAsgari
description: Xbox Live 코드 Xbox 개발 키트 (XDK) 플랫폼에서 유니버설 Windows 플랫폼 (UWP)에 포트 하는 방법을 알아봅니다.
ms.assetid: 69939f95-44ad-4ffd-851f-59b0745907c8
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, xdk, 포팅
ms.localizationpriority: medium
ms.openlocfilehash: 9278ee433852bf3ef1eec2570340ef9cb7d64c92
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4611595"
---
# <a name="porting-xbox-live-code-from-the-xbox-developer-kit-xdk-to-universal-windows-platform-uwp"></a>Xbox Live 코드 Xbox 개발자 키트 (XDK)에서 유니버설 Windows 플랫폼 (UWP)로 포팅

## <a name="introduction"></a>소개

이 문서를 사용 하는 Xbox One XDK 자신의 Xbox Live 코드를 Windows 10 유니버설 Windows 플랫폼 (UWP) 마이그레이션을 시작 하는 개발자를 위해 제공 됩니다.

이 마이그레이션 기능도 포함 됩니다 XSAPI 1.0 (Xbox Live 서비스 API, Xbox One XDK 통해 2015 년 8 월에에서 포함)에서 전환 XSAPI 2.0 (2015 년 11 월부터 Xbox One XDK에 포함 하 고 또한 Xbox Live SDK에서 사용할 수 있습니다. 이러한 Api의 기능 거의 동일 하지만 몇 가지 중요 한 구현 차이가 있습니다.

이 문서에서 다루는 다른 항목 포함 Windows 개발 컴퓨터를 준비 하 고 클라우드 기반 관리에 대 한 연결 된 저장소 API 뿐만 아니라 보안 소켓 API와 같은 Xbox Live 서비스를 사용할 때 필요한 다른 Api를 일반적으로 설치 게임 저장합니다.

<a name="_Setting_up_and"></a>

## <a name="setting-up-and-configuring-your-project-in-dev-center-and-xdp"></a>설정 및 개발자 센터 및 XDP 프로젝트 구성

Xbox Live 서비스를 사용 하는 UWP 제목 [Windows 개발자 센터](https://dev.windows.com/en-us) 또는 [Xbox 개발자 포털 (XDP)](https://xdp.xboxlive.com/)에서 구성 해야 합니다. 최신 정보를 Xbox Live 프로그래밍 가이드의 [Xbox Live SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx)에 포함 된 [새로운 또는 기존 UWP 프로젝트에 Xbox Live 추가](../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) 참조 하세요.

타이틀에 Xbox Live 서비스를 사용 하 여 이러한 단계를 포함 하는 해당 페이지에 대 한 항목:

-   Windows 개발자 센터에서 UWP 앱 프로젝트를 만듭니다.

-   XDP를 사용 하 여 Xbox Live 사용에 대 한 프로젝트를 설정 합니다.

-   개발자 센터 제품 XDP 제품에 연결 합니다.

-   XDP (샌드박스에 Xbox Live 타이틀을 실행 하는 경우 필요 함)에서 개발자 계정을 만듭니다.

제목에 멀티 플레이 지원 하는 경우 몇 가지 추가 설정에서 멀티 플레이 세션 템플릿 필요할 수 있습니다. Xbox Live 멀티 플레이어를 사용 하 고는 MPSD (멀티 플레이 세션 문서)를 작성 하는 모든 Windows 10 타이틀이 세션 템플릿에 "기능" 목록에 새 필드가 필요: ```userAuthorizationStyle: true```.

### <a name="enabling-cross-play"></a>크로스 플레이 사용 하도록 설정

"크로스 플레이" (공유 Xbox Live 구성을 허용 장치 간 멀티 플레이어 게임을 Xbox One 및 PC 게임 사이)을 지원할 경우 세션 템플릿에이 기능을 추가 해야도: **crossPlay: true**.

크로스 플레이 및 구성 요구 사항에 XDP 지원에 대 한 자세한 내용은 "Ingesting XDK 및 UWP 크로스 플레이 타이틀에서 XDP" Xbox Live 프로그래밍 가이드에서를 참조 하세요.

또한 일부 프로그래밍 고려 사항에 대 한 [지원 멀티 플레이 크로스 플레이 Xbox One 및 PC 간에](#_Supporting_multiplayer_cross-play)이후 섹션을 참조 하세요.

## <a name="setting-up-your-windows-development-environment"></a>Windows 개발 환경 설정

1.  [최신 **Xbox Live SDK** 를 다운로드](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx) 하 고 로컬로 추출 합니다.

2.  UWP에 대 한 보안 소켓 API 및/또는 게임 저장 API (일명 연결 저장소) 해야 하는 경우 [ **Xbox Live 플랫폼 확장 SDK** 를 설치](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx) 합니다.

3.  Visual Studio에서 유니버설 Windows 앱 프로젝트에 Xbox Live 지원 추가. 전체 소스를 추가 하거나 Visual Studio 프로젝트에 NuGet 패키지를 설치 하 여 이진 파일을 참조할 수 있습니다. 패키지는 c + + 및 WinRT 모두 사용할 수 있습니다. 자세한 내용은 참조 [새로운 또는 기존 UWP 프로젝트에 Xbox Live 추가](../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)

4.  에 샌드박스를 사용 하 여 개발 컴퓨터를 구성 합니다. 관리자 권한 명령 프롬프트에서 사용할 수 있는 Xbox Live SDK의 도구 디렉터리에 명령줄 스크립트는 (예: SwitchSandbox.cmd XDKS.1).

  **참고** 소매 샌드박스도 다시 전환 하려면 레지스트리 키를 수정 하는 스크립트 또는 소매 라는 샌드박스를 전환할 수 있습니다 하거나 삭제할 수 있습니다.

1.  개발 컴퓨터에 개발자 계정을 추가 합니다. 개발자 계정을 XDP에서 만든가 할당 된 샌드박스에 개발 하거나 샘플을 실행 하는 경우 런타임에 Xbox Live 서비스와 상호 작용 해야 합니다. 에 하나 이상의 계정을 Windows에 추가 합니다.

    1.  **설정** 을 엽니다 (바로 가기: Windows 키 + I).

    2.  **계정**을 엽니다.

    3.  **계정** 탭에서 **Microsoft 계정 추가**클릭 합니다.

    4.  개발자 계정 메일 및 암호를 입력 합니다.

### <a name="appxmanifest-changes"></a>AppxManifest 변경

Xbox 및 UWP 버전 appxmanifest.xml 파일의 가장 일반적인 변경 다음과 같습니다.

1. 패키지 Id UWP에서 개발 하는 동안에 중요합니다. Id 이름 및 게시자 모두 *일치 해야* UWP 앱에 대 한 개발자 센터에 정의 된 것입니다.

1. 패키지 종속성 섹션은 필요 합니다. 예를 들면 다음과 같습니다.

```xml
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Universal" MinVersion="10.0.10240.0" MaxVersionTested="10.0.10240.0" />
  </Dependencies>
```

1.  예제 UWP 응용 프로그램 매니페스트 (예를 들어, Xbox Live SDK에 포함 된 UWP 샘플 또는 Visual Studio에서 만든 기본 유니버설 Windows 앱 프로젝트 중 하나)에 대 한 특정 요구 사항이 응용 프로그램 매니페스트의 다른 섹션에 대 한 참조 UWP, 같은 ```<VisualElements>```.

1.  제목 및 서비스 안내 xboxservices.config 파일에 정의 된 ( [다음 섹션](#_Define_your_title)을 참조 하세요.) 대신 "xbox.live" 확장 범주에 있습니다.

1.  "Xbox.system.resources" 확장 범주 필요 하지 않습니다.

1.  보안 소켓 networkmanifest.xml 파일에 정의 된 ( [Secure sockets](#_Secure_sockets)참조) 대신 "windows.xbox.networking" 범주에 있습니다.

1.  UWP 타이틀에 Xbox Live 초대를 받으려면 "windows.protocol" 확장 범주를 정의 해야 합니다 ( [보내기 및 받기 초대](#_Sending_and_receiving)참조).

1.  내 마이크 장치 기능을 추가 하려는 GameChat API를 사용 하는 경우는 ```<Capabilities>``` 요소입니다. 예를 들면 다음과 같습니다.

  ```<DeviceCapability Name="microphone">```

<a name="_Define_your_title"></a>

### <a name="define-your-title-and-scid-for-the-xbox-live-sdk-in-a-config-file"></a>구성 파일에서 Xbox Live SDK에 대 한 제목 및 서비스 안내 정의

Xbox Live SDK는 더 이상 UWP 제목에 대 한 appxmanifest.xml에 포함 된 ID 및 서비스 안내, 타이틀 알아야 합니다. 대신 프로젝트 루트 디렉터리에서 **xboxservices.config** 라는 텍스트 파일을 추가 하는 다음 필드를 값 타이틀에 대 한 정보로 바꿉니다.

```
{
  "TitleId": 123456789,
  "PrimaryServiceConfigId": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
}
```

> [!NOTE]
> Xboxservices.config 내 모든 값은 대/소문자 구분 합니다.

빌드 출력에서 사용할 수 있도록 프로젝트의 콘텐츠로이 구성 파일을 포함 합니다.

**참고** 이러한 값은 다음 API를 사용 하 여 타이틀 내에서 프로그래밍 방식으로 제공 됩니다.

```cpp
Microsoft::Xbox::Services::XboxLiveAppConfiguration^ xblConfig = xblContext->AppConfig;
unsigned int titleId = xblConfig->TitleId;
Platform::String^ scid = xblConfig->ServiceConfigurationId;
```

### <a name="api-namespace-mapping"></a>API 네임 스페이스 매핑

표 1. XDK에서 UWP로 매핑을 Namespace 합니다.

<table>
  <tr>
    <td></td>
    <td><b>Xbox One XDK</b></td><td><b>UWP</b></td>
    <td><b>API는 사용할 수 있습니다.</b></td>
  </tr>
  <tr>
    <td>Xbox 서비스 API (XSAPI)</td>
    <td>Microsoft::Xbox::Services</td>
    <td>Microsoft::Xbox::Services (<i>변경 없음</i>)</td>
    <td>Xbox Live SDK (소스 또는 NuGet 이진 사용)</td>
  </tr>
  <tr>
    <td>GameChat</td>
    <td>
Microsoft::Xbox::GameChat Windows::Xbox::Chat </td>
    <td>
Microsoft::Xbox::GameChat (*변경 없음*) Microsoft::Xbox::ChatAudio </td>
    <td>
Xbox Live SDK (이진 NuGet 사용 하는 경우) </td>
  </tr>
  <tr>
    <td>SecureSockets</td>
    <td>Windows::Xbox::Networking</td>
    <td>Windows::Networking::XboxLive</td>
    <td>Xbox Live 플랫폼 확장 SDK</td>
  </tr>
  <tr>
    <td>연결된 저장소</td>
    <td>Windows::Xbox::Storage</td>
    <td>Windows::Gaming::XboxLive::Storage</td>
    <td>Xbox Live 플랫폼 확장 SDK</td>
  </tr>
</table>

### <a name="multiplayer-subscriptions-and-event-handling"></a>멀티 플레이 구독 및 이벤트 처리

가장 멀티 플레이 제목을 포함 되어 있는 XSAPI 2.0 XSAPI 1.0의 주요 변경 하나가 **MultiplayerService** **RealTimeActivityService** 에서 여러 메서드 및 이벤트의 이동 합니다.

예를 들면 다음과 같습니다.

-   **EnableMultiplayerSubscriptions () \ *** 메서드

-   **DisableMultiplayerSubscriptions()** 메서드

-   **MultiplayerSessionChanged** 이벤트

-   **MultiplayerSubscriptionLost** 이벤트

-   **MultiplayerSubscriptionsEnabled** 속성

**중요 한 구현 참고** 사용할 수 있는 하지 명시적으로 **RealTimeActivityService** 의 다른 항목과 **MultiplayerService**를 통해 이러한 메서드와 이벤트를 이동한 후에 여전히 호출 해야 **xblContext-&gt;RealTimeActivityService&gt;Activate()** 멀티 플레이 구독 RTA 서비스 해야 하기 때문에 **EnableMultiplayerSubscriptions()** 를 호출 하기 전에 합니다.

## <a name="whats-handled-differently-in-uwp"></a>UWP에서 다르게 처리 하는 기능

다음은 새 [NetRumble 샘플](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx) (XDK와 UWP 버전 포함)는 발견 될 가능성이 XDK 및 UWP, 간의 차이점 않은 코드의 섹션의 매우 높은 수준의 목록.

-   제목 ID 및 서비스 안내 정보에 액세스

-   사전 실행 활성화 (새 UWP 용)

-   일시 중단/다시 시작 PLM 처리

-   확장된 실행 (새 UWP 용)

-   Xbox **사용자** 개체 및 사용자 처리 차이

    -   로그인 및 로그 아웃 처리

    -   컨트롤러 (Xbox에서 처리)만 페어링

    -   게임 패드 처리

-   멀티 플레이 권한 확인

-   Xbox One 및 PC 간에 멀티 플레이 크로스 플레이 지원

-   게임 초대 보내기

    -   게임-UWP에서 해당 없음에서에서 타사 앱을 열 수

    -   게임-UWP에서 해당 없음에서에서 타사 구성원을 열거 하는 기능

-   게이머 프로필을 보여 주는

-   보안 소켓 API 서피스 변경 내용

-   QoS 측정 시작 및 결과 처리

-   게임 이벤트 작성

-   GameChat: 이벤트, 설정 및 ChatUser 개체

-   저장소 API 서피스 변경 연결

-   PIX 이벤트 (Xbox; 에서만에서 다루지 않는이 백서)

-   일부 렌더링 차이점

다음 섹션에서는 이러한 차이점은 대부분에 자세히로 이동 합니다.

### <a name="accessing-title-id-and-scid-info"></a>제목 ID 및 서비스 안내 정보에 액세스

UWP, 제목 ID 및 서비스 구성 ID는 **XboxLiveContext**인스턴스에 AppConfig 속성을 통해 액세스 됩니다.

```cpp
Microsoft::Xbox::Services::XboxLiveAppConfiguration^ xblConfig = xblContext->AppConfig;
unsigned int titleId = xblConfig->TitleId;
Platform::String^ scid = xblConfig->ServiceConfigurationId;
```

**참고** XDK, **Windows::Xbox::Services::XboxLiveConfiguration**에서 이러한 새 속성 또는 이전 정적 속성을 사용 하 여 이러한 Id를 얻을 수 있습니다.

### <a name="prelaunch-activation"></a>사전 실행 활성화

사용자가 로그인 할 때 Windows 10에서 자주 사용 되는 타이틀 사전 실행 되지 않을 수 있습니다. 이 처리 하려면 타이틀 **PreLaunchActivated**에 대 한 시작 인수를 확인 하는 코드가 있어야 합니다. 예를 들어 아마도 하지 않으려면이 유형의 정품 인증 하는 동안 모든 리소스를 로드 합니다. 자세한 내용은 [앱 사전 실행 처리](https://msdn.microsoft.com/library/windows/apps/mt593297.ASPx)MSDN 문서를 참조 하세요.

### <a name="suspendresume-plm-handling"></a>일시 중단/다시 시작 PLM 처리

일시 중단 및 다시 시작 하 고 일반적으로 PLM; Xbox One의 작동 방식으로 유니버설 Windows 앱에서 비슷하게 작동 그러나 염두에 중요 한 차이점이 몇 가지 있습니다.

-   PC에서 **제한** 상태가 없습니다-는 Xbox One 단독 개념입니다.

-   일시 중단 시작 즉시 제목 최소화 됩니다. 이 방식으로 [확장 실행](#_Extended_execution)섹션을 참조 하세요.

-   타이밍은 다른: 콘솔에서 1 초 대신 PC에서 일시 중단 하는 데 5 초 해야 합니다.

또 다른 중요 한 고려 사항은 연결 된 저장소를 사용 하는 경우이 API의 UWP 버전에서 새 **ContainersChangedSinceLastSync** 속성입니다. 다시 시작 이벤트를 처리할 때 모든 컨테이너 변경 된 경우 클라우드에 타이틀 일시 중단 된 동안 확인 하려면이 속성을 확인할 수 있습니다. 이 플레이어가 한 대의 PC에서 게임을 일시 중단, 다른 위치에 재생 및 첫 번째 PC에 반환 하는 경우 발생할 수 있습니다. 하면 중단 되기 전에 데이터 이러한 컨테이너에서 메모리로 읽어온 적, 아마도 하려는 경우 읽기 다시 표시 되도록 변경 내용 및 적절 하 게 변경 내용을 처리 합니다.

Windows 10의 UWP 앱에서 PLM을 처리 하는 방법에 대 한 자세한 내용은 MSDN 문서를 참조 하세요 [실행, 다시 시작 및 백그라운드 작업](https://msdn.microsoft.com/library/windows/apps/xaml/mt227652.aspx).

유용할 수 있습니다 또한 [Xbox One 용 PLM](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx) 백서 GDN에서 고려 하는 게임을 사용 하 여 작성 된 있어 앱 수명 주기를 처리 하기 위한 개념의 대부분은 PC에서 계속 적용 합니다.

<a name="_Extended_execution"></a>

### <a name="extended-execution"></a>확장된 실행

UWP 최소화 PC에서 앱 일반적으로 발생 즉시 터 일시 중단 합니다. 확장된 실행을 사용 하 여이 프로세스를 지연 시킬 수가 있습니다. 예제 구현을:

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

**ExtensionRevokedHandler** 호출 된 후 새 확장 향후 잠재적인 보류에 대 한 요청 해야 합니다. **ExtensionRevokedHandler** 메모리 압력 시스템에서는 10 분이 지난 없거나 최소화 된 게임은 사용자가 게임을 다시 전환할 때 호출 됩니다. 따라서 해당이 시간에 될 가능성이 **RequestExtension()** 를 호출 해야 합니다.

-   동안 시작 합니다.

-   **ExtensionRevokedHandler** 에서 때 인수-&gt;이유 재개 (10 분 타이머가 만료 되기 전에 게임 최소화 하는 동안에 다시 탭 사용자) = = 합니다.

-   **OnResuming** 처리기 (제목 메모리 압력 또는 10 분 타이머 인해 일시 중단 된) 경우.

### <a name="handling-users-and-controllers"></a>사용자 및 컨트롤러 처리

Windows에서는 한 번에 하나의 로그인 사용자를 사용 하 여 작업할 수 있습니다. 먼저 **XboxLiveUser** 개체를 만들고 Xbox Live SDK에서 Xbox Live에 로그인 한 다음이 사용자의 **XboxLiveContext** 개체를 만듭니다.

이전에 Xbox One XDK에서:

1.  (예: 게임 패드 조작)에서 사용자를 획득 합니다.
2.  해당 사용자에서는 **XboxLiveContext** 를 만듭니다.
  ```
  ref new Microsoft::Xbox::Services::XboxLiveContext( Windows::Xbox::System::User^ user )
  ```
1.  **아웃** 이벤트를 처리 합니다.
  ```
  Windows::Xbox::System::User::SignOutStarted
  ```
1.  게임 패드/컨트롤러를 사용 하 여 페어링을 처리 합니다.
  ```
  Windows::Xbox::Input::Controller::ControllerRemoved
  Windows::Xbox::Input::Controller::ControllerPairingChanged
  ```

이제 UWP/Xbox Live SDK에 대 한:

1.  **XboxLiveUser**프로그램 만듭니다.

  ```
  auto xblUser = ref new Microsoft::Xbox::Services::System::XboxLiveUser();
  ```

1.  UI를 사용 하 여 이러한 귀찮게 하지 않고 사용 마지막 Microsoft 계정을 사용 하 여 로그인 하려고 합니다.

  ```
  xblUser->SignInSilentlyAsync();
  ```

1.  이 비동기 작업의 결과에서 **SignInResult::Success** 를 얻은 경우 **XboxLiveContext**만들고 지역 /:

  ```
  auto xblContext = ref new Microsoft::Xbox::Services::XboxLiveContext( xblUser );
  ```

1.  에 경우 대신 **SignInResult::UserInteractionRequired**대화형 로그인 메서드를 호출 하면 시스템 UI 해야 합니다.

  ```
  xblUser->SignInAsync();
  ```

1.  여기에서 발생할 수 있습니다 **SignInResult::UserCancel**, 사용자 로그인 하지 않은 경우 하 고 다시 로그인 하도록 메뉴 옵션을 제공 하는 것이 좋습니다.

  **참고** 메뉴 옵션을 제공할 때 다른 Microsoft 계정으로 전환할 수 있는 옵션을 제공 하는 것이 좋습니다.

  ```
  xblUser->SwitchAccountAsync( nullptr );
  ```

1.  로그인 사용자를 클릭 한 후 로그 아웃 사용자에 게 반응할 수 있도록 **XboxLiveUser::SignOutCompleted** 이벤트에 후크 하고자 할 수 있습니다.

  ```
  xblUser->SignOutCompleted += ref new Windows::Foundation::EventHandler<Microsoft::Xbox::Services::System::SignOutCompletedEventArgs^>( &OnSignOutCompleted );
  ```

1.  Windows 10에서 처리할 페어링할 컨트롤러인 있습니다.

이 c + +에 대 한 간단한 예 / WinRT 합니다. 자세한 예제에서는 Xbox Live 프로그래밍 가이드의 "Windows 10에서에서 라이브 인증 Xbox"를 참조 하세요. "새 UWP 프로젝트에 Xbox Live 추가"에 더 광범위 한 예제 찾을 수도 있습니다 유용 합니다.

### <a name="checking-multiplayer-privileges"></a>멀티 플레이 권한 확인

해당 하는 **CheckPrivilegeAsync()** 는 아직 Xbox Live SDK에서 사용할 수 있습니다. 지금은 **XboxLiveUser**프로그램 대 한 **권한** 을 속성에 의해 반환 되는 문자열 목록에서 필요한 사용 권한을 검색 해야 합니다. 예를 들어 멀티 플레이 권한을 확인 하려면 찾습니다 권한 "254." XDK 설명서를 사용 하는 모든 Xbox Live 권한 목록을 **Windows::Xbox::ApplicationModel::Store::KnownPrivileges** 열거형에 찾을 수 있습니다.

이 항목에서 논의 [xsapi 및 사용자 권한](https://forums.xboxlive.com/questions/48513/xsapi-user-privileges.html)을 게시 포럼을 참조 하세요.

<a name="_Supporting_multiplayer_cross-play"></a>

### <a name="supporting-multiplayer-cross-play-between-xbox-one-and-pc-uwp"></a>Xbox One 및 PC UWP 멀티 플레이 크로스 플레이 지원

XDP에서 새 세션 템플릿 요구 사항 외에도 ( [개발자 센터 및 XDP 프로젝트 구성 및 설정](#_Setting_up_and)참조)을 크로스 플레이 세션 가입 기능에 대 한 새 제한이 함께 제공 됩니다. "None" 세션 가입 제한으로 더 이상 사용할 수 없습니다. "열어" 또는 "로컬" (기본이 "로컬") 중 하나를 사용 해야 합니다.

또한 가입 및 읽기 제한 기본값 "로컬" 필수 **userAuthorizationStyle** 기능으로 인해 Windows 10 멀티 플레이어 합니다.

[공용 멀티 플레이 세션을 만들 수는](https://forums.xboxlive.com/questions/46781/is-it-possible-to-create-public-multiplayer-sessio.html),이 포럼 문서 유용한 추가 정보를 포함합니다.

자세한 내용 및 예제는 업데이트 된 멀티 플레이 개발자 순서도 교차 플레이 활성화 멀티 플레이 샘플 NetRumble, 또는 사용자 개발자 계정 관리자 (댐을)에서 찾을 수 있습니다.

<a name="_Sending_and_receiving"></a>

### <a name="sending-and-receiving-invites"></a>초대를 주고받기

초대 보내기에 대 한 UI를 불러오는 데 API는 이제 **Microsoft:: Xbox::Services::System::TitleCallableUI::ShowGameInviteUIAsync()** 합니다. 세션의 전달&gt; 사용자 활동 세션 (일반적으로 경우 로비)에서 **SessionReference** 개체입니다. 필요에 따라 참조 사용자 지정 초대 XDP에서 서비스 구성에 정의 된 된 문자열 ID는 두 번째 매개 변수로 전달할 수 있습니다. 여기에 정의 된 문자열 초대 받은 플레이어에 게 전송 알림 메시지에 표시 됩니다. Note는 어떤에서 전달 하는이 메서드에 매개 변수가 ID 번호와 서비스에 대 한 제대로 포맷 해야 합니다. 예를 들어, 문자열 ID "1"로 전달 되어야 합니다에서 "/ / / 1".

멀티 플레이 서비스를 사용 하 여 직접 초대 보내기 하려는 경우 (즉, 없이 보여 주는 모든 UI), 다른 초대 메서드에서 사용자의 **Microsoft:: Xbox::Services::Multiplayer::MultiplayerService::SendInvitesAsync()** 계속 사용할 수 있습니다 **XboxLiveContext**합니다.

Windows에 들어오는 초대에 대 한 프로토콜 활성화 타이틀을 허용 하려면이 확장을 추가 해야 합니다 ** &lt;응용 프로그램&gt; ** appxmanifest에서 요소:

```xml
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-xbl-multiplayer" />
  </uap:Extension>
</Extensions>
```

그런 다음 하기 전에 Xbox One에서 때 처럼에 **CoreApplication** **활성화** 이벤트를 가져오고 활성화 종류는 **ActivationKind::Protocol**는 초대를 처리할 수 있습니다.

### <a name="showing-the-gamer-profile-card"></a>게이머 프로필 카드가 표시

UWP에서 게이머 프로필 카드, 팝업 **Microsoft:: Xbox::Services::System::TitleCallableUI::ShowProfileCardUIAsync()**, 대상 사용자의 XUID 전달 하 여 사용 합니다.

<a name="_Secure_sockets"></a>

### <a name="secure-sockets"></a>보안 소켓

보안 소켓 API는 별도 [Xbox Live 플랫폼 확장 SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx)에 포함 됩니다.

이 포럼 게시물 API 사용에 대 한 참조: [플랫폼 간 SecureDeviceAssociation에 대 한 설정](https://forums.xboxlive.com/answers/45722/view.html)합니다.

**참고** UWP 용 **SocketDescriptions** 섹션 appxmanifest를 자체 [networkmanifest.xml](https://forums.xboxlive.com/storage/attachments/410-networkmanifestxml.txt)이동 했습니다. 내부의 형식을 합니다 &lt;SocketDescriptions&gt; 요소는 방금 없이 거의 동일 합니다 **mx:** 접두사입니다.

Xbox 및 Windows 10 간의 크로스 플레이, *있는지* 모든 것이 정의 된 *동일한* 두 개의 다른 종류의 매니페스트 (Package.appxmanifest Xbox One 용) 및 Windows 10 용 networkmanifest.xml 간에 이어야 합니다. 소켓 이름, 프로토콜 등 *정확히*일치 해야 합니다.

또한 크로스 플레이 해야 안에 다음 네 가지 SDA 사용법을 정의 하는 ```<AllowedUsages>``` *모두* Xbox One Package.appxmanifest에서 요소 및 Windows 10 networkmanifest.xml:

```xml
<SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
<SecureDeviceAssociationUsage Type="AcceptOnMicrosoftConsole" />
<SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
<SecureDeviceAssociationUsage Type="AcceptOnWindowsDesktop" />
```

### <a name="multiplayer-qos-measurements"></a>멀티 플레이어 QoS 측정

보안 소켓 API에서 네임 스페이스 변경 하는 것 외에도 개체 이름과 값의 일부 변경, 너무 합니다. 다음 표에서에서 일반적으로 사용 되는 측정 상태에 대 한 매핑을 발견 됩니다.

표 2. 측정 상태 매핑 주로 사용 됩니다.

| XDK (Windows::Xbox::Networking::QualityOfServiceMeasurementStatus)  | UWP (Windows::Networking::XboxLive::XboxLiveQualityOfServiceMeasurementStatus)  |
|------------------------------------|--------------------------------------------|
| HostUnreachable                    | NoCompatibleNetworkPaths                   |
| MeasurementTimedOut                | 시간이 초과                                   |
| PartialResults                     | InProgressWithProvisionalResults           |
| 성공                            | 성공                                  |

QoS (서비스 품질) *측정* 단계 및 *결과 처리* 는 원칙적에서 동일 API의 XDK 및 UWP 버전 비교 하는 경우. 그러나 이름 변경 하 고 몇 가지 변경 사항으로 인해 결과 코드는 다르게 일부 위치에서.

**XDK**용는 QoS을 측정 하려면 보안 장치 주소의 컬렉션 및 메트릭 컬렉션이 생성 및 이러한 **MeasureQualityOfServiceAsync()** 메서드로 전달 합니다.

**UWP**에 대 한 QoS는 측정, 새 **XboxLiveQualityOfServiceMeasurement()** 개체를 만들고를 호출할 때 **Append()** 은 **메트릭** 및 **DeviceAddresses** 속성에 개체의 **MeasureAsync()** 호출 메서드입니다.

예를 들면 다음과 같습니다.

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

더 많은 예제 NetRumble 샘플에서 **MatchmakingSession::MeasureQualityOfService()** 및 **MatchmakingSession::ProcessQosMeasurements()** 함수를 참조 하세요.

### <a name="writing-game-events"></a>게임 이벤트 작성

UWP의 다른 API가 타이틀의 서비스 구성에서 구성 된 게임 이벤트를 전송 합니다. Xbox Live SDK **EventsService** 및 속성 모음 모델을 사용합니다.

예를 들면 다음과 같습니다.

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

자세한 내용은 Xbox Live SDK 설명서를 참조 하세요.

**팁** .H 헤더 파일에 XDP에서 다운로드 한 events.man 파일을 변환 (도구 디렉터리에 있는) Xbox Live SDK와 함께 제공 되는 **xcetool.exe** 사용할 수 있습니다. 사용 하는 '-x' 새 v2 속성 모음 스키마를 사용 하 여이 c + + 헤더를 생성 하는 옵션입니다. 이 헤더에 구성 된 이벤트의 모든 호출할 수 있는 c + + 함수를 포함 되어 있습니다. 예를 들어, **EventWriteMultiplayerRoundStart()** 합니다. WinRT 인터페이스를 사용 하려는 경우 각 이벤트에 대 한 속성 및 측정을 구성 하는 방법을 보려면이 헤더 파일을 참조할 수 있습니다.

### <a name="game-chat"></a>게임 채팅

UWP에서 GameChat 이진 NuGet 패키지로 Xbox Live SDK와 함께 포함 됩니다. 프로젝트에이 NuGet 패키지를 추가 하는 방법에 대 한 Xbox Live 프로그래밍 가이드의 지침을 참조 하세요.

기본 사용법의 XDK 및 UWP 버전 간에 거의 동일합니다. API의 몇 가지 차이점은 다음과 같습니다.

1.  **User::AudioDeviceAdded** 이벤트는 UWP의 제목 듣지 필요가 없습니다. 기본 채팅 라이브러리 핸들 장치 추가 및 제거 합니다.

2.  **ChatUser** 이제 **GameChatUser**라고 합니다.

3.  **Microsoft::Xbox::GameChat** 네임 스페이스는 동일 하 게 유지 하지만 **Windows::Xbox::Chat** 네임 스페이스 **Microsoft::Xbox::ChatAudio**되었습니다.

4.  XUID a 나 a **AddLocalUserToChatChannelAsync()** 걸리는 **ChatAudio::IChatUser ^** 는 **XboxUser**대신 합니다.

5.  **RemoveLocalUserFromChatChannelAsync()** 필요는 **ChatAudio::IChatUser ^** 는 **XboxUser**대신 합니다. **IChatUser** **GameChatUser**에서 가져올 수 있습니다-&gt;**사용자**.

### <a name="connected-storage"></a>연결 된 저장소

연결 된 저장소 API는 별도 [Xbox Live 플랫폼 확장 SDK](https://developer.xboxlive.com/en-us/live/development/Pages/Downloads.aspx)에서 제공 됩니다. 설명서는 Xbox Live SDK 문서에 포함 됩니다.

전체 흐름와 동일 Xbox One에서 UWP 버전에서 **ContainersChangedSinceLastSync** 속성을 추가 합니다. 타이틀 변경 내용을 확인 컨테이너 클라우드에 타이틀 일시 중단 된 동안 다시 **GetForUserAsync()** 를 호출한 후 다시 시작 이벤트를 처리 하는 경우이 속성을 검사 해야 합니다. 변경 된 컨테이너 중 하나에서 메모리에 로드 하는 데이터에 있는 경우 다시 참조 하 고 변경 그에 따라 변경 처리에 데이터를 읽고 하려고 하는 것입니다.

UWP 버전의 다른 주요 차이점은 다음과 같습니다.

1.  Namespace 변경은 **Windows::Xbox::Storage** **Windows::Gaming::XboxLive::Storage**수 있습니다.

2.  **ConnectedStorageSpace** 이름이 **GameSaveProvider**합니다.

3.  **Windows::System::User** **XboxUser**대신 **GetForUserAsync()** 사용 되 고는 서비스 안내는 이제 필요 합니다.

4.  로컬 없음 "컴퓨터" 저장소 (즉, **GetForMachineAsync()** 제거 되었습니다). 에 비 로밍, 로컬 데이터 저장에 대 한 **Windows::Storage::ApplicationData** 를 대신 사용 하는 것이 좋습니다.

5.  예외 즈 프리 \*Result-type 개체 (예를 들어 **GameSaveProviderGetResult**); 비동기 결과가 반환 됩니다. 이 **Status** 속성을 확인 수 있으며 오류가 없는 경우 **Value** 속성에서 반환 된 개체를 읽을 수 있습니다.

6.  **ConnectedStorageErrorStatus 열거형** 은 **GameSaveErrorStatus** 바뀌고 결과의 **Status** 속성에서 반환 됩니다. 모든 이전 값 존재 하 고 몇 가지 새로 추가 되었습니다.

-   Abort

-   ObjectExpired

-   확인

-   UserHasNoXboxLiveInfo

GameSave 샘플 또는 참조 NetRumble 샘플 예를 사용 합니다.

**참고** Gamesaveutil.exe는 xbstorage.exe (명령줄 개발자는 XDK에 포함 된 유틸리티)을 같습니다. Xbox Live 플랫폼 확장 SDK를 설치한 후이 유틸리티를 볼 수 있습니다: C:\\Program Files (x86) \\Windows Kits\\10\\Extension SDKs\\XboxLive\\1.0\\Bin\\x64

## <a name="summary"></a>요약

API 변경 내용 및이 백서에 설명 된 새 요구 하는 것을 새 UWP Xbox One XDK에서 기존 게임 코드를 포팅할 때 발생할 수 있습니다. 특정 강조 응용 프로그램 및 환경 설정 뿐만 아니라 멀티 플레이어 및 연결 된 저장소 등의 Xbox Live 서비스와 관련 된 기능 영역을가지고 있습니다. 자세한 내용은 다음을 참조를이 문서에서는 전체에서 제공 된 링크를 따라 및 더 많은 도움말, 응답 및 뉴스에 대 한 [개발자 포럼](https://forums.xboxlive.com) 의 "Windows 10" 섹션을 참조 하세요.

## <a name="references"></a>참조

-   [Xbox One에서에서 Windows 10로 포팅](https://developer.xboxlive.com/en-us/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx)

-   [Xbox One 백서](https://developer.xboxlive.com/en-us/platform/development/education/Pages/WhitePapers.aspx)

-   [샘플](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx)
