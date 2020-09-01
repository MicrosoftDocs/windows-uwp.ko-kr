---
ms.assetid: 58af5e9d-37a1-4f42-909c-db7cb02a0d12
description: 이 문서에서는 MediaPlayer를 사용 하 여 유니버설 Windows 앱에서 미디어를 재생 하는 방법을 보여 줍니다.
title: MediaPlayer를 사용하여 오디오 및 비디오 재생
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e29862160adc3a78bd1b83d6869db47903c638b7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163747"
---
# <a name="play-audio-and-video-with-mediaplayer"></a>MediaPlayer를 사용하여 오디오 및 비디오 재생

이 문서에서는  [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 클래스를 사용 하 여 유니버설 Windows 앱에서 미디어를 재생 하는 방법을 보여 줍니다. Windows 10, 버전 1607을 사용 하면 배경 오디오를 위한 간소화 된 단일 프로세스 디자인, SMTC (시스템 미디어 전송 컨트롤)와의 자동 통합, 여러 미디어 플레이어를 동기화 하는 기능, 비디오 프레임을 창에 렌더링 하는 기능 및 콘텐츠에서 미디어 나누기를 만들고 예약 하기 위한 간편한 인터페이스를 포함 하 여 미디어 재생 Api에 대 한 상당한 기능이 향상 되었습니다. 이러한 향상 된 기능을 활용 하려면 미디어 재생을 위해 **MediaElement** 대신 **MediaPlayer** 클래스를 사용 하는 것이 좋습니다. 경량 XAML 컨트롤인 [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)는 xaml 페이지에서 미디어 콘텐츠를 렌더링할 수 있도록 하기 위해 도입 되었습니다. 이제 **MediaElement** 에서 제공 되는 대부분의 재생 제어 및 상태 api를 새 [**Mediaplaybacksession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession) 개체를 통해 사용할 수 있습니다. **MediaElement** 는 이전 버전과의 호환성을 지원 하기 위해 계속 작동 하지만이 클래스에 추가 기능이 추가 되지 않습니다.

이 문서에서는 일반적인 미디어 재생 앱에서 사용할 **MediaPlayer** 기능을 안내 합니다. **MediaPlayer** 는 [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) 클래스를 모든 미디어 항목의 컨테이너로 사용 합니다. 이 클래스를 사용 하면 동일한 인터페이스를 사용 하 여 로컬 파일, 메모리 스트림 및 네트워크 원본을 비롯 한 다양 한 원본에서 미디어를 로드 하 고 재생할 수 있습니다. 또한 [**Mediaplaybackitem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) 및 [**Mediaplaybackitem**](/uwp/api/Windows.Media.Playback.MediaPlaybackList)와 같이 **MediaSource**에서 작동 하는 더 높은 수준의 클래스가 있습니다 .이 클래스는 재생 목록과 같은 고급 기능을 제공 하 고 여러 오디오, 비디오 및 메타 데이터 트랙으로 미디어 원본을 관리 하는 기능을 제공 합니다. **MediaSource** 및 관련 api에 대 한 자세한 내용은 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)을 참조 하세요.

> [!NOTE] 
> Windows 10 N 및 Windows 10 KN 버전에는 재생을 위해 **MediaPlayer** 를 사용 하는 데 필요한 미디어 기능이 포함 되어 있지 않습니다. 이러한 기능은 수동으로 설치할 수 있습니다. 자세한 내용은 [windows 10 N 및 windows 10 kn 버전용 미디어 기능 팩](https://support.microsoft.com/help/3010081/media-feature-pack-for-windows-10-n-and-windows-10-kn-editions)을 참조 하세요.

## <a name="play-a-media-file-with-mediaplayer"></a>MediaPlayer를 사용 하 여 미디어 파일 재생  
**MediaPlayer** 로 기본 미디어를 재생 하는 것은 매우 간단 하 게 구현할 수 있습니다. 먼저 **MediaPlayer** 클래스의 새 인스턴스를 만듭니다. 앱에서 한 번에 여러 개의 **MediaPlayer** 인스턴스를 활성화할 수 있습니다. 그런 다음, 플레이어의 [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) 속성을 [**imediaplaybacksource**](/uwp/api/Windows.Media.Playback.IMediaPlaybackSource)(예: [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource), [**Mediaplaybackitem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem)또는 [**mediaplaybacklist**](/uwp/api/Windows.Media.Playback.MediaPlaybackList))를 구현 하는 개체로 설정 합니다. 이 예제에서는 앱의 로컬 저장소에 있는 파일에서 **MediaSource** 을 만든 다음, 해당 원본에서 **Mediaplaybackitem** 을 만든 다음 플레이어의 **source** 속성에 할당 합니다.

**MediaElement**와 달리 **MediaPlayer** 는 기본적으로 재생을 자동으로 시작 하지 않습니다. [**Play**](/uwp/api/windows.media.playback.mediaplayer.play)를 호출 하거나, [**자동**](/uwp/api/windows.media.playback.mediaplayer.autoplay) 재생 속성을 true로 설정 하거나, 사용자가 기본 제공 미디어 컨트롤로 재생을 시작할 때까지 대기 하 여 재생을 시작할 수 있습니다.

[!code-cs[SimpleFilePlayback](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSimpleFilePlayback)]

**MediaPlayer**를 사용 하 여 앱을 실행 하는 경우에는 [**Close**](/uwp/api/windows.media.playback.mediaplayer.close) 메서드 (c #에서는 **Dispose** 로 프로젝션 됨)를 호출 하 여 플레이어에서 사용 하는 리소스를 정리 해야 합니다.

[!code-cs[CloseMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCloseMediaPlayer)]

## <a name="use-mediaplayerelement-to-render-video-in-xaml"></a>MediaPlayerElement를 사용 하 여 XAML로 비디오 렌더링
XML에 표시 하지 않고 **MediaPlayer** 에서 미디어를 재생할 수 있지만 많은 미디어 재생 앱이 미디어를 xaml 페이지에서 렌더링 하려고 합니다. 이렇게 하려면 경량 [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 컨트롤을 사용 합니다. **MediaElement**와 마찬가지로 **MediaPlayerElement** 를 사용 하면 기본 제공 전송 컨트롤을 표시할지 여부를 지정할 수 있습니다.

[!code-xml[MediaPlayerElementXAML](./code/MediaPlayer_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

[**Setmediaplayer**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.setmediaplayer)를 호출 하 여 요소가 바인딩되는 **MediaPlayer** 인스턴스를 설정할 수 있습니다.

[!code-cs[SetMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetMediaPlayer)]

**MediaPlayerElement** 에 재생 원본을 설정할 수도 있습니다. 그러면 요소는 [**mediaplayer**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer) 속성을 사용 하 여 액세스할 수 있는 새 **MediaPlayer** 인스턴스를 자동으로 만듭니다.

[!code-cs[GetPlayerFromElement](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetGetPlayerFromElement)]

> [!NOTE] 
> [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) 를 false로 설정 하 여 [**Mediaplayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 의 [**mediaplaybackcommandmanager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) 를 사용 하지 않도록 설정 하는 경우 **MediaPlayerElement**에서 제공 하는 **mediaplayer** [**컨트롤**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) 사이의 연결을 중단 하므로 기본 제공 전송 컨트롤은 플레이어의 재생을 더 이상 자동으로 제어 하지 않습니다. 대신 사용자 고유의 컨트롤을 구현 하 여 **MediaPlayer**를 제어 해야 합니다.

## <a name="common-mediaplayer-tasks"></a>일반 MediaPlayer 작업
이 섹션에서는 **MediaPlayer**의 일부 기능을 사용 하는 방법을 보여 줍니다.

### <a name="set-the-audio-category"></a>오디오 범주 설정
**MediaPlayer** 의 [**AudioCategory**](/uwp/api/windows.media.playback.mediaplayer.audiocategory) 속성을 [**MediaPlayerAudioCategory**](/uwp/api/Windows.Media.Playback.MediaPlayerAudioCategory) 열거형 값 중 하나로 설정 하 여 시스템에서 재생 중인 미디어의 종류를 알 수 있도록 합니다. 게임은 다른 응용 프로그램이 백그라운드에서 음악을 재생 하는 경우 게임 음악이 자동으로 mutes 하기 위해 음악 스트림을 **GameMedia** 로 분류 해야 합니다. 음악과 비디오 응용 프로그램은 스트림을 **미디어** 또는 **동영상** 으로 분류 하 여 **GameMedia** 스트림 보다 우선 순위가 높은 것을 고려해 야 합니다.

[!code-cs[SetAudioCategory](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioCategory)]

### <a name="output-to-a-specific-audio-endpoint"></a>특정 오디오 끝점으로 출력
기본적으로 **mediaplayer** 의 오디오 출력은 시스템의 기본 오디오 끝점으로 라우팅됩니다. 그러나 **mediaplayer** 가 출력에 사용 해야 하는 특정 오디오 끝점을 지정할 수 있습니다. 아래 예제에서 [**GetAudioRenderSelector**](/uwp/api/windows.media.devices.mediadevice.getaudiorenderselector) 는 장치의 오디오 렌더링 범주를 고유 하 게 idenfies 하는 문자열을 반환 합니다. 그런 다음 [**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 메서드 [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 를 호출 하 여 선택한 유형의 사용 가능한 모든 장치 목록을 가져옵니다. 사용자가 장치를 선택할 수 있도록, 사용 하려는 장치를 프로그래밍 방식으로 확인 하거나 [**콤보 상자**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 에 반환 된 장치를 추가할 수 있습니다.

[!code-cs[SetAudioEndpointEnumerate](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpointEnumerate)]

장치 콤보 상자의 [**Selectionchanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 이벤트에서 **MediaPlayer** 의 [**오디오 장치**](/uwp/api/windows.media.playback.mediaplayer.audiodevice) 속성은 **ComboBoxItem**의 [**Tag**](/uwp/api/windows.ui.xaml.frameworkelement.tag) 속성에 저장 된 선택 된 장치로 설정 됩니다.

[!code-cs[SetAudioEndpontSelectionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpontSelectionChanged)]

### <a name="playback-session"></a>재생 세션
이 문서 앞부분에서 설명한 것 처럼 **MediaElement** 클래스에 의해 노출 되는 대부분의 함수는 [**Mediaplaybacksession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession) 클래스로 이동 되었습니다. 여기에는 플레이어의 재생 상태에 대 한 정보 (예: 현재 재생 위치, 플레이어의 일시 중지 여부와 재생 속도 및 현재 재생 속도)가 포함 됩니다. 또한 **Mediaplaybacksession** 은 재생 중인 콘텐츠의 현재 버퍼링 및 다운로드 상태와 현재 재생 중인 비디오 콘텐츠의 기본 크기 및 가로 세로 비율을 비롯 하 여 상태가 변경 될 때 사용자에 게 알리는 여러 이벤트를 제공 합니다.

다음 예제에서는 콘텐츠에서 10 초 앞으로 건너뛰는 단추 클릭 처리기를 구현 하는 방법을 보여 줍니다. 먼저 선수의 **mediaplaybacksession** 개체가 [**playbacksession**](/uwp/api/windows.media.playback.mediaplayer.playbacksession) 속성을 사용 하 여 검색 됩니다. 그런 다음 [**Position**](/uwp/api/windows.media.playback.mediaplaybacksession.position) 속성은 현재 재생 위치에 10 초를 더한 값으로 설정 됩니다.

[!code-cs[SkipForwardClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSkipForwardClick)]

다음 예에서는 토글 단추를 사용 하 여 세션의 [**Playbackrate**](/uwp/api/windows.media.playback.mediaplaybacksession.playbackrate) 속성을 설정 하 여 일반 재생 속도와 2 배 속도 사이를 전환 하는 방법을 보여 줍니다.

[!code-cs[SpeedChecked](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSpeedChecked)]

Windows 10, 버전 1803부터 **MediaPlayer** 에서 90도 단위로 증가 하는 비디오가 표시 되는 회전을 설정할 수 있습니다.

[!code-cs[SetRotation](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetRotation)]

### <a name="detect-expected-and-unexpected-buffering"></a>예상 및 예기치 않은 버퍼링 검색
이전 섹션에서 설명 하는 **Mediaplaybacksession** 개체는 현재 재생 중인 미디어 파일이 시작 되 고 버퍼링 되는 시기, **[bufferingstarted](/uwp/api/windows.media.playback.mediaplaybacksession.BufferingStarted)** 및 **[bufferingstarted](/uwp/api/windows.media.playback.mediaplaybacksession.BufferingEnded)** 를 검색 하는 두 가지 이벤트를 제공 합니다. 그러면 버퍼링이 발생 하는 사용자를 표시 하는 UI를 업데이트할 수 있습니다. 초기 버퍼링은 미디어 파일을 처음 열 때 또는 사용자가 재생 목록의 새 항목으로 전환 하는 경우에 필요 합니다. 네트워크 속도가 떨어지거나 콘텐츠를 제공 하는 콘텐츠 관리 시스템에서 기술적인 문제가 발생 하면 예기치 않은 버퍼링이 발생할 수 있습니다. RS3부터 **Bufferingstarted** 이벤트를 사용 하 여 버퍼링 이벤트가 예상 되는지 또는 예기치 않은 경우 재생을 중단 하는지 확인할 수 있습니다. 앱 또는 미디어 배달 서비스에 대 한 원격 분석 데이터로이 정보를 사용할 수 있습니다. 

버퍼링 상태 알림을 받기 위해 **Bufferingstarted** 및 **bufferingstarted** 이벤트에 대 한 처리기를 등록 합니다.

[!code-cs[RegisterBufferingHandlers](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterBufferingHandlers)]

**Bufferingstarted** 이벤트 처리기에서 이벤트에 전달 된 이벤트 인수를 **[MediaPlaybackSessionBufferingStartedEventArgs](/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs)** 개체로 캐스팅 하 고 **[isplaybackinterruption](/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs.IsPlaybackInterruption)** 속성을 선택 합니다. 이 값이 true 이면 이벤트를 트리거한 버퍼링이 예기치 않은 것 이며 재생이 중단 됩니다. 그렇지 않으면 초기 버퍼링이 예상 됩니다. 

[!code-cs[BufferingHandlers](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetBufferingHandlers)]


### <a name="pinch-and-zoom-video"></a>비디오 확대 및 축소
**MediaPlayer** 를 사용 하면 비디오 콘텐츠 내에서 렌더링 되어야 하는 원본 사각형을 지정 하 여 비디오를 확대할 수 있습니다. 지정 하는 사각형은 정규화 된 사각형 (0, 0, 1, 1)에 상대적입니다. 여기서 0은 프레임 왼쪽 상단이 고 1은 프레임의 전체 너비와 높이를 지정 합니다. 예를 들어 비디오의 오른쪽 위 사분면이 렌더링 되도록 확대/축소 사각형을 설정 하려면 사각형 (. 5, 0, 5, 5)을 지정 합니다.  원본 사각형이 (0, 0, 1, 1) 정규화 된 사각형 내에 있는지 확인 하기 위해 값을 확인 하는 것이 중요 합니다. 이 범위를 벗어나는 값을 설정하려고 하면 예외가 발생됩니다.

다중 터치 제스처를 사용 하 여 손가락 및 확대/축소를 구현 하려면 먼저 지원 하려는 제스처를 지정 해야 합니다. 이 예제에서는 크기 조정 및 변환 제스처를 요청 합니다. [**System.windows.uielement.manipulationdelta>**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) 이벤트는 구독 하는 제스처 중 하나가 발생할 때 발생 합니다. [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped) 이벤트는 전체 프레임에 대 한 확대/축소를 다시 설정 하는 데 사용 됩니다. 

[!code-cs[RegisterPinchZoomEvents](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPinchZoomEvents)]

다음에는 현재 확대/축소 소스 사각형을 저장할 **Rect** 개체를 선언 합니다.

[!code-cs[DeclareSourceRect](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareSourceRect)]

**System.windows.uielement.manipulationdelta>** 처리기는 확대/축소 사각형의 배율 또는 변환을 조정 합니다. 델타 배율 값이 1이 아닌 경우 사용자가 손가락을 손가락으로 사용 했음을 의미 합니다. 값이 1 보다 크면 소스 사각형을 더 작게 만들어 콘텐츠를 확대 합니다. 값이 1 보다 작은 경우에는 소스 사각형을 더 크게 축소 해야 합니다. 새 크기 조정 값을 설정 하기 전에 결과 사각형이 완전히 (0, 0, 1, 1) 제한 내에 있는지 확인 합니다.

배율 값이 1 이면 변환 제스처가 처리 됩니다. 사각형은 제스처의 픽셀 수를 컨트롤의 너비와 높이로 나눈 값으로 변환 됩니다. 그 결과 사각형은 (0, 0, 1, 1) 범위 내에 있는지 확인 하기 위해 확인 됩니다.

마지막으로, **Mediaplaybacksession** 의 [**NormalizedSourceRect**](/uwp/api/windows.media.playback.mediaplaybacksession.normalizedsourcerect) 은 새로 조정 된 사각형으로 설정 되어 렌더링 해야 하는 비디오 프레임 내의 영역을 지정 합니다.

[!code-cs[ManipulationDelta](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

[**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped) 이벤트 처리기에서 원본 사각형은 (0, 0, 1, 1)로 다시 설정 되어 전체 비디오 프레임이 렌더링 됩니다.

[!code-cs[DoubleTapped](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDoubleTapped)]

**참고** 이 섹션에서는 터치식 입력에 대해 설명 합니다. 터치 패드는 포인터 이벤트를 보내고 조작 이벤트를 보내지 않습니다.

### <a name="handling-policy-based-playback-degradation"></a>정책 기반 재생 저하 처리

경우에 따라 시스템은 성능 문제가 아닌 정책에 따라 해상도 (constriction)를 줄이는 등 미디어 항목의 재생을 저하 시킬 수 있습니다. 예를 들어, 서명 되지 않은 비디오 드라이버를 사용 하 여 비디오를 재생 하는 경우에는 시스템에 의해 성능이 저하 될 수 있습니다. [**Mediaplaybacksession**](/uwp/api/windows.media.playback.mediaplaybacksession.getoutputdegradationpolicystate#Windows_Media_Playback_MediaPlaybackSession_GetOutputDegradationPolicyState) 를 호출 하 여이 정책 기반 저하 발생 여부 및 이유를 확인 하 고 사용자에 게 경고 하거나 원격 분석 목적에 대 한 이유를 기록할 수 있습니다.

다음 예제에서는 플레이어가 새 미디어 항목을 열 때 발생 하는 **MediaPlayer** 이벤트 처리기의 구현을 보여 줍니다. **GetOutputDegradationPolicyState** 는 처리기에 전달 된 **MediaPlayer** 에서 호출 됩니다. [**VideoConstrictionReason**](/uwp/api/windows.media.playback.mediaplaybacksessionoutputdegradationpolicystate.videoconstrictionreason#Windows_Media_Playback_MediaPlaybackSessionOutputDegradationPolicyState_VideoConstrictionReason) 값은 비디오가 constricted 된 정책 이유를 나타냅니다. 값이 **None**이 아닌 경우이 예에서는 원격 분석을 위해 성능 저하 이유를 기록 합니다. 또한이 예제에서는 현재 재생 중인 **AdaptiveMediaSource** 의 비트 전송률을 가장 낮은 대역폭으로 설정 하 여 데이터 사용량을 저장 하는 방법을 보여 줍니다 .이는 비디오가 constricted이 고 고해상도에서 표시 되지 않기 때문입니다. **AdaptiveMediaSource**사용에 대 한 자세한 내용은 [적응 스트리밍](adaptive-streaming.md)을 참조 하세요.

[!code-cs[PolicyDegradation](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPolicyDegradation)]
        
## <a name="use-mediaplayersurface-to-render-video-to-a-windowsuicomposition-surface"></a>MediaPlayerSurface를 사용 하 여 비디오를 화면에 렌더링 합니다.
Windows 10, 버전 1607부터 **MediaPlayer** 를 사용 하 여 비디오를에 렌더링 하 여 비디오를 [**ICompositionSurface**](/uwp/api/Windows.UI.Composition.ICompositionSurface)에 렌더링할 수 있습니다. 그러면 플레이어는 [**windows**](/uwp/api/Windows.UI.Composition) 의 api와 상호 운용할 수 있습니다. 컴퍼지션 프레임 워크를 사용 하면 XAML과 하위 수준 DirectX 그래픽 Api 사이에서 시각적 계층의 그래픽으로 작업할 수 있습니다. 이렇게 하면 비디오를 XAML 컨트롤에 렌더링 하는 등의 시나리오를 사용할 수 있습니다. 컴퍼지션 Api를 사용 하는 방법에 대 한 자세한 내용은 [시각적 계층](../composition/visual-layer.md)을 참조 하세요.

다음 예제에서는 비디오 플레이어 콘텐츠를 [**캔버스**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) 컨트롤에 렌더링 하는 방법을 보여 줍니다. 이 예제의 미디어 플레이어 관련 호출은 [**SetSurfaceSize**](/uwp/api/windows.media.playback.mediaplayer.setsurfacesize) 및 [**getsurface**](/uwp/api/windows.media.playback.mediaplayer.getsurface)입니다. **SetSurfaceSize** 는 콘텐츠를 렌더링 하기 위해 할당 해야 하는 버퍼의 크기를 시스템에 알려 줍니다. **Getsurface** 는 [**Compositor**](/uwp/api/Windows.UI.Composition.Compositor) 을 arguemnt로 사용 하 고 [**MediaPlayerSurface**](/uwp/api/Windows.Media.Playback.MediaPlayerSurface) 클래스의 인스턴스를 검색 합니다. 이 클래스는 화면을 만드는 데 사용 되는 **MediaPlayer** 및 **Compositor** 에 대 한 액세스를 제공 하 고 [**CompositionSurface**](/uwp/api/windows.media.playback.mediaplayersurface.compositionsurface) 속성을 통해 화면 자체를 노출 합니다.

이 예제의 나머지 코드는 비디오가 렌더링 되는 [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) 을 만들고 크기를 시각적 개체를 표시 하는 canvas 요소의 크기를 설정 합니다. 다음으로 [**CompositionBrush**](/uwp/api/Windows.UI.Composition.CompositionBrush) 는 [**MediaPlayerSurface**](/uwp/api/Windows.Media.Playback.MediaPlayerSurface) 에서 만들어지고 시각적 개체의 [**Brush**](/uwp/api/windows.ui.composition.spritevisual.brush) 속성에 할당 됩니다. 그런 다음 [**system.windows.media.containervisual>**](/uwp/api/Windows.UI.Composition.ContainerVisual) 가 만들어지고 시각적 트리 맨 위에 **SpriteVisual** 가 삽입 됩니다. 마지막으로, [**Setelementchildvisual 개체**](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual) 는 **캔버스**에 컨테이너 시각적 개체를 할당 하기 위해 호출 됩니다.

[!code-cs[Compositor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCompositor)]
        
## <a name="use-mediatimelinecontroller-to-synchronize-content-across-multiple-players"></a>MediaTimelineController를 사용 하 여 여러 플레이어에서 콘텐츠를 동기화 합니다.
이 문서 앞부분에서 설명한 것 처럼 앱은 한 번에 여러 개의 **MediaPlayer** 개체를 활성화할 수 있습니다. 기본적으로 사용자가 만드는 각 **MediaPlayer** 는 독립적으로 작동 합니다. 설명 트랙과 비디오를 동기화 하는 등의 일부 시나리오에서는 플레이어 상태, 재생 위치 및 여러 플레이어의 재생 속도를 동기화 할 수 있습니다. Windows 10 버전 1607부터 [**MediaTimelineController**](/uwp/api/Windows.Media.MediaTimelineController) 클래스를 사용 하 여이 동작을 구현할 수 있습니다.

### <a name="implement-playback-controls"></a>재생 컨트롤 구현
다음 예에서는 **MediaTimelineController** 를 사용 하 여 **MediaPlayer**의 두 인스턴스를 제어 하는 방법을 보여 줍니다. 먼저 **MediaPlayer** 의 각 인스턴스가 인스턴스화되고 **소스가** 미디어 파일로 설정 됩니다. 다음으로 새 **MediaTimelineController** 이 만들어집니다. 각 **MediaPlayer**에 대해 [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) 속성을 false로 설정 하 여 각 플레이어와 연결 된 [**mediaplaybackcommandmanager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) 를 사용 하지 않도록 설정할 수 있습니다. 그런 다음 [**TimelineController**](/uwp/api/windows.media.playback.mediaplayer.timelinecontroller) 속성은 타임 라인 컨트롤러 개체로 설정 됩니다.

[!code-cs[DeclareMediaTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaTimelineController)]

[!code-cs[SetTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetTimelineController)]

**주의** [**Mediaplaybackcommandmanager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) 는 **MediaPlayer** 와 시스템 미디어 전송 컨트롤 (smtc) 간의 자동 통합을 제공 하지만, **MediaTimelineController**로 제어 되는 미디어 플레이어에는이 자동 통합을 사용할 수 없습니다. 따라서 플레이어의 타임 라인 컨트롤러를 설정 하기 전에 media player에 대 한 명령 관리자를 사용 하지 않도록 설정 해야 합니다. 이렇게 하지 않으면 다음 메시지와 함께 예외가 throw 됩니다. "개체의 현재 상태로 인해 미디어 타임 라인 컨트롤러를 연결 하지 못했습니다." SMTC와 미디어 플레이어를 통합 하는 방법에 대 한 자세한 내용은 [시스템 미디어 전송 컨트롤과 통합](integrate-with-systemmediatransportcontrols.md)을 참조 하세요. **MediaTimelineController** 를 사용 하는 경우에도 smtc를 수동으로 제어할 수 있습니다. 자세한 내용은 [시스템 미디어 전송 컨트롤의 수동 제어](system-media-transport-controls.md)를 참조 하세요.

하나 이상의 미디어 플레이어에 **MediaTimelineController** 를 연결한 후에는 컨트롤러에서 노출 하는 메서드를 사용 하 여 재생 상태를 제어할 수 있습니다. 다음 예에서는 [**Start**](/uwp/api/windows.media.mediatimelinecontroller.start) 를 호출 하 여 미디어의 시작 부분에 있는 모든 연결 된 미디어 플레이어의 재생을 시작 합니다.

[!code-cs[PlayButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPlayButtonClick)]

이 예제에서는 연결 된 미디어 플레이어를 모두 일시 중지 하 고 다시 시작 하는 방법을 보여 줍니다.

[!code-cs[PauseButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPauseButtonClick)]

모든 연결 된 미디어 플레이어를 빠르게 전달 하려면 재생 속도를 1 보다 큰 값으로 설정 합니다.

[!code-cs[FastForwardButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFastForwardButtonClick)]

다음 예제에서는 **슬라이더** 컨트롤을 사용 하 여 연결 된 미디어 플레이어 중 하나의 콘텐츠 기간을 기준으로 타임 라인 컨트롤러의 현재 재생 위치를 표시 하는 방법을 보여 줍니다. 먼저 새 **MediaSource** 가 만들어지고 미디어 원본의 [**openoperationcompleted**](/uwp/api/windows.media.core.mediasource.openoperationcompleted) 에 대 한 처리기가 등록 됩니다. 

[!code-cs[CreateSourceWithOpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCreateSourceWithOpenCompleted)]

**Openoperationcompleted** 처리기는 미디어 원본 콘텐츠의 기간을 검색할 수 있는 기회로 사용 됩니다. 기간이 결정 되 면 **슬라이더** 컨트롤의 최대값이 미디어 항목의 총 시간 (초)으로 설정 됩니다. 값은 [**Runasync**](/uwp/api/windows.ui.core.coredispatcher.runasync) 호출 내에서 설정 되어 UI 스레드에서 실행 되는지 확인 합니다.

[!code-cs[DeclareDuration](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareDuration)]

[!code-cs[OpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenCompleted)]

그런 다음, 타임 라인 컨트롤러의  [**Positionchanged**](/uwp/api/windows.media.mediatimelinecontroller.positionchanged) 이벤트에 대 한 처리기가 등록 됩니다. 이는 초당 약 4 번 시스템에 의해 주기적으로 호출 됩니다.

[!code-cs[RegisterPositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPositionChanged)]

**Positionchanged**처리기에서 슬라이더 값은 타임 라인 컨트롤러의 현재 위치를 반영 하도록 업데이트 됩니다.

[!code-cs[PositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPositionChanged)]

### <a name="offset-the-playback-position-from-the-timeline-position"></a>타임 라인 위치에서 재생 위치 오프셋
경우에 따라 타임 라인 컨트롤러와 연결 된 하나 이상의 미디어 플레이어의 재생 위치를 다른 플레이어에서 오프셋할 수 있습니다. 오프셋할 **MediaPlayer** 개체의 [**TimelineControllerPositionOffset**](/uwp/api/windows.media.playback.mediaplayer.timelinecontrollerpositionoffset) 속성을 설정 하 여이 작업을 수행할 수 있습니다. 다음 예제에서는 두 개의 media 선수 콘텐츠의 기간을 사용 하 여 두 slider 컨트롤의 최소값과 최대값을 더하기 및 빼기 항목 길이를 뺀 값으로 설정 합니다.  

[!code-cs[OffsetSliders](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOffsetSliders)]

각 슬라이더의 [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) 이벤트에서 각 플레이어의 **TimelineControllerPositionOffset** 는 해당 값으로 설정 됩니다.

[!code-cs[TimelineOffset](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetTimelineOffset)]

플레이어의 오프셋 값이 네거티브 재생 위치에 매핑되면 오프셋이 0에 도달할 때까지 클립이 일시 중지 된 상태로 유지 되 고 재생이 시작 됩니다. 마찬가지로, 오프셋 값이 미디어 항목 지속 시간 보다 큰 재생 위치에 매핑되면 최종 프레임은 단일 미디어 플레이어가 콘텐츠의 끝에 도달 했을 때와 마찬가지로 표시 됩니다.

## <a name="play-spherical-video-with-mediaplayer"></a>MediaPlayer를 사용 하 여 구형 비디오 재생
Windows 10 버전 1703부터 **MediaPlayer** 는 구형 비디오 재생에 대 한 등 장방형 프로젝션을 지원 합니다. 구형 비디오 콘텐츠는 비디오 인코딩이 지원 되는 경우 해당 **MediaPlayer** 가 비디오를 렌더링 한다는 일반적인 플랫 비디오와는 다릅니다. 비디오에서 등 장방형 프로젝션을 사용 하도록 지정 하는 메타 데이터 태그를 포함 하는 구면 비디오의 경우 **MediaPlayer** 는 지정 된 뷰 필드 및 뷰 방향을 사용 하 여 비디오를 렌더링할 수 있습니다. 이렇게 하면 헤드를 탑재 한 디스플레이를 사용 하 여 가상 현실 비디오 재생 등의 시나리오를 가능 하 게 하거나 마우스나 키보드 입력을 사용 하 여 사용자가 구형 비디오 콘텐츠 내에서 이동할 수 있습니다.

구면 비디오를 재생 하려면이 문서의 앞부분에서 설명한 비디오 콘텐츠 재생 단계를 사용 합니다. 추가 단계 중 하나는 [**MediaPlayer에서 연**](/uwp/api/Windows.Media.Playback.MediaPlayer#Windows_Media_Playback_MediaPlayer_MediaOpened) 이벤트에 대 한 처리기를 등록 하는 것입니다. 이 이벤트는 구형 비디오 재생 매개 변수를 설정 하 고 제어할 수 있는 기회를 제공 합니다.

[!code-cs[OpenSphericalVideo](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenSphericalVideo)]

**Mediaopened** 처리기에서 먼저 [**playbacksession**](/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.FrameFormat) 속성을 확인 하 여 새로 열린 미디어 항목의 프레임 형식을 확인 합니다. 이 값이 [**SphericaVideoFrameFormat**](/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat)인 경우 시스템은 비디오 콘텐츠를 자동으로 프로젝션 할 수 있습니다. 먼저 [**Playbacksession IsEnabled**](/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.IsEnabled) 속성을 **true**로 설정 합니다. 미디어 플레이어가 비디오 콘텐츠를 프로젝션 하는 데 사용할 보기 방향 및 보기 필드와 같은 속성을 조정할 수도 있습니다. 이 예에서는 [**HorizontalFieldOfViewInDegrees**](/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.HorizontalFieldOfViewInDegrees) 속성을 설정 하 여 뷰의 필드가 와이드 값 120도로 설정 됩니다.

비디오 콘텐츠가 구형 이지만 등 장방형 이외의 형식에 있는 경우 media player의 프레임 서버 모드를 사용 하 여 개별 프레임을 수신 및 처리 하는 고유한 프로젝션 알고리즘을 구현할 수 있습니다.

[!code-cs[SphericalMediaOpened](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalMediaOpened)]

다음 예제 코드에서는 왼쪽 및 오른쪽 화살표 키를 사용 하 여 구면 비디오 보기 방향을 조정 하는 방법을 보여 줍니다.

[!code-cs[SphericalOnKeyDown](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalOnKeyDown)]

앱에서 비디오 재생 목록을 지 원하는 경우 UI에 구면 비디오가 포함 된 재생 항목을 식별할 수 있습니다. 미디어 재생 목록은 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)문서에 자세히 설명 되어 있습니다. 다음 예제에서는 새 재생 목록을 만들고, 항목을 추가 하 고, Mediaplaybackitem에 대 한 처리기를 등록 하는 방법을 보여 줍니다. 비디오 항목에 대 한 비디오 트랙이 해결 될 때 발생 하는 [**VideoTracksChanged 이벤트입니다.**](/uwp/api/windows.media.playback.mediaplaybackitem.VideoTracksChanged)

[!code-cs[SphericalList](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalList)]

**Video트랙볼 Schanged** 이벤트 처리기에서 [**GetEncodingProperties**](/uwp/api/windows.media.core.videotrack.GetEncodingProperties)를 호출 하 여 추가 된 비디오 트랙의 인코딩 속성을 가져옵니다. Encoding 속성의 [**SphericalVideoFrameFormat**](/uwp/api/windows.media.mediaproperties.videoencodingproperties.SphericalVideoFrameFormat) 속성이 [**SphericaVideoFrameFormat**](/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat)이외의 값 이면 비디오 트랙에 구면 비디오가 포함 되 고, 선택 하는 경우 UI를 적절 하 게 업데이트할 수 있습니다.

[!code-cs[SphericalTracksChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalTracksChanged)]

## <a name="use-mediaplayer-in-frame-server-mode"></a>프레임 서버 모드에서 MediaPlayer 사용
Windows 10 버전 1703부터 프레임 서버 모드에서 **MediaPlayer** 를 사용할 수 있습니다. 이 모드에서 **MediaPlayer** 는 연결 된 **MediaPlayerElement**프레임을 자동으로 렌더링 하지 않습니다. 대신 앱은 **MediaPlayer** 의 현재 프레임을 [**IDirect3DSurface**](/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface)를 구현 하는 개체로 복사 합니다. 이 기능의 기본 시나리오는 픽셀 셰이더를 사용 하 여 **MediaPlayer**에서 제공 하는 비디오 프레임을 처리 하는 것입니다. 앱은 XAML [**이미지**](/uwp/api/windows.ui.xaml.controls.image) 컨트롤에서 프레임을 표시 하는 등 처리 후 각 프레임을 표시 하는 작업을 담당 합니다.

다음 예에서는 새 **MediaPlayer** 를 초기화 하 고 비디오 콘텐츠를 로드 합니다. 그런 다음 [**사용할 수 있는 Video프레임**](/uwp/api/windows.media.playback.mediaplayer.VideoFrameAvailable) 처리기가 등록 됩니다. **MediaPlayer** 개체의 [**IsVideoFrameServerEnabled**](/uwp/api/windows.media.playback.mediaplayer.IsVideoFrameServerEnabled) 속성을 **true**로 설정 하 여 프레임 서버 모드를 사용 하도록 설정 합니다. 마지막으로 [**재생**](/uwp/api/windows.media.playback.mediaplayer.Play)을 호출 하 여 미디어 재생이 시작 됩니다.

[!code-cs[FrameServerInit](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFrameServerInit)]

다음 예제에서는 [Win2D](https://github.com/Microsoft/Win2D) 를 사용 하 여 비디오의 각 프레임에 단순한 흐림 효과를 추가한 다음 처리 된 프레임을 XAML [이미지](/uwp/api/windows.ui.xaml.controls.image) 컨트롤에 표시 하는 **video프레임 사용** 에 대 한 처리기를 보여 줍니다.

**Video프레임 사용 가능** 처리기가 호출 될 때마다 [**CopyFrameToVideoSurface**](/uwp/api/windows.media.playback.mediaplayer.copyframetovideosurface) 메서드를 사용 하 여 프레임의 내용을 [**IDirect3DSurface**](/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface)에 복사 합니다. [**CopyFrameToStereoscopicVideoSurfaces**](/uwp/api/windows.media.playback.mediaplayer.copyframetostereoscopicvideosurfaces) 를 사용 하 여 왼쪽 눈동자를 개별적으로 처리 하기 위해 3d 콘텐츠를 두 개의 화면으로 복사할 수도 있습니다. **IDirect3DSurface** 를 구현 하는 개체를 가져오기 위해이 예제에서는 Win2D [**비트맵**](/uwp/api/windows.graphics.imaging.softwarebitmap) 을 만든 다음 해당 개체를 사용 하 여 필요한 인터페이스를 구현 하는 **CanvasBitmap**를 만듭니다. **CanvasImageSource** 는 **이미지** 컨트롤의 원본으로 사용할 수 있는 Win2D 개체 이므로 새 항목을 만들고 콘텐츠가 표시 되는 **이미지** 의 원본으로 설정 합니다. 그런 다음 **CanvasDrawingSession** 를 만듭니다. 이는 Win2D에서 흐리게 효과를 렌더링 하는 데 사용 됩니다.

필요한 모든 개체가 인스턴스화된 후에는 **CopyFrameToVideoSurface** 가 호출 되어 **MediaPlayer** 의 현재 프레임을 **CanvasBitmap**로 복사 합니다. 그런 다음 Win2D **GaussianBlurEffect** 이 만들어지고,이 작업의 소스로 설정 된 **CanvasBitmap** 합니다. 마지막으로, CanvasDrawingSession는 **이미지** 컨트롤과 연결 된 **CanvasImageSource** 에 흐리게 효과를 적용 하 여 소스 이미지를 그리기 위해 호출 되어 UI에 그려집니다 **.**

[!code-cs[VideoFrameAvailable](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetVideoFrameAvailable)]

Win2D에 대 한 자세한 내용은 [Win2D GitHub 리포지토리](https://github.com/Microsoft/Win2D)를 참조 하세요. 위에 표시 된 샘플 코드를 사용 하려면 다음 지침을 사용 하 여 Win2D NuGet 패키지를 프로젝트에 추가 해야 합니다.

**Win2D NuGet 패키지를 효과 프로젝트에 추가 하려면**

1.  **솔루션 탐색기**에서 마우스 오른쪽 단추로 프로젝트를 클릭하고, **NuGet 패키지 관리**를 선택합니다.
2.  창 위쪽에서 **찾아보기** 탭을 선택 합니다.
3.  검색 상자에 **Win2D**를 입력 합니다.
4.  **Win2D**를 선택한 다음 오른쪽 창에서 **설치** 를 선택 합니다.
5.  **변경 내용 검토** 대화 상자에 설치할 패키지를 표시 합니다. **확인**을 클릭합니다.
6.  패키지 라이선스에 동의 합니다.

## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>시스템의 오디오 수준 변경 내용 검색 및 응답
Windows 10 버전 1803부터 응용 프로그램은 시스템이 현재 재생 하는 **MediaPlayer**의 오디오 수준을 mutes 하거나 해당 시스템에서 사용 하는 경우를 감지할 수 있습니다. 예를 들어, 경보가 울림 될 때 시스템은 오디오 재생 수준을 낮출 수 있습니다. 앱이 앱 매니페스트에서 *backgroundMediaPlayback* 기능을 선언 하지 않은 경우 시스템은 백그라운드에 들어가면 앱을 음소거 합니다. [**AudioStateMonitor**](./uwp/api/windows.media.audio.audiostatemonitor) 클래스를 사용 하면 시스템에서 오디오 스트림의 볼륨을 수정할 때 이벤트를 수신 하도록 등록할 수 있습니다. **Mediaplayer** 의 **AudioStateMonitor** 속성에 액세스 하 고 시스템에서 해당 **mediaplayer** 의 오디오 수준을 변경할 때 알리도록 [**SoundLevelChanged**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) 이벤트에 대 한 처리기를 등록 합니다.

[!code-cs[RegisterAudioStateMonitor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterAudioStateMonitor)]

**SoundLevelChanged** 이벤트를 처리할 때 재생 중인 콘텐츠 형식에 따라 다른 작업을 수행할 수 있습니다. 현재 음악을 재생 하는 경우 볼륨을 ducked 하는 동안 음악이 계속 재생 되도록 할 수 있습니다. 그러나 포드캐스트를 재생 하는 경우 오디오가 ducked 때 재생을 일시 중지 하 여 사용자가 콘텐츠를 누락 하지 않을 수 있습니다.

이 예에서는 현재 재생 중인 콘텐츠가 포드캐스트 인지 여부를 추적 하는 변수를 선언 합니다 .이는 **MediaPlayer**의 콘텐츠를 선택할 때이 값을 적절 한 값으로 설정 하는 것으로 가정 합니다. 또한 오디오 수준이 변경 될 때 프로그래밍 방식으로 재생을 일시 중지 하는 경우 추적할 클래스 변수를 만듭니다.

[!code-cs[AudioStateVars](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

**SoundLevelChanged** 이벤트 처리기에서 **AudioStateMonitor** 발신자의 [**SoundLevel**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) 속성을 확인 하 여 새 사운드 수준을 결정 합니다. 이 예에서는 새 소리 수준이 전체 볼륨 인지 여부를 확인 합니다. 즉, 시스템에서 음소거를 중지 했거나 볼륨을 ducking 소리 수준이 내려간 경우에는 비상업용 콘텐츠가 아닌 콘텐츠를 재생 합니다. 이러한 중 하나가 true이 고 콘텐츠가 이전에 프로그래밍 방식으로 일시 중지 된 경우 재생이 다시 시작 됩니다. 새 소리 수준이 음소거 상태 이거나 현재 콘텐츠가 포드캐스트이 고 소리 수준이 낮으면 재생이 일시 중지 되 고 변수가 프로그래밍 방식으로 시작 되었음을 추적 하도록 설정 됩니다.

[!code-cs[SoundLevelChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSoundLevelChanged)]

사용자는 오디오를 시스템에서 ducked 하는 경우에도 일시 중지 하거나 계속 재생 하려고 한다고 결정할 수 있습니다. 이 예제에서는 재생 및 일시 중지 단추에 대 한 이벤트 처리기를 보여 줍니다. 일시 중지 단추 클릭 처리기가 일시 중지 되었습니다. 프로그래밍 방식으로 재생을 이미 일시 중지 한 경우 사용자가 콘텐츠를 일시 중지 했음을 나타내는 변수를 업데이트 합니다. 재생 단추 클릭 처리기에서 재생을 재개 하 고 추적 변수를 지웁니다.

[!code-cs[ButtonUserClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetButtonUserClick)]

## <a name="related-topics"></a>관련 항목
* [미디어 재생](media-playback.md)
* [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)
* [을 (를) 사용 하 여 시스템과 통합](integrate-with-systemmediatransportcontrols.md)
* [미디어 휴지 만들기, 예약 및 관리](create-schedule-and-manage-media-breaks.md)
* [백그라운드에서 미디어 재생](background-audio.md)





 

 