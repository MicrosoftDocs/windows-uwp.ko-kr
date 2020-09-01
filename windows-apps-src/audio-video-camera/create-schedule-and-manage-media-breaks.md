---
ms.assetid: 0309c7a1-8e4c-4326-813a-cbd9f8b8300d
description: 이 문서에서는 미디어 재생 앱에 대 한 미디어 나누기를 만들고 예약 하 고 관리 하는 방법을 보여 줍니다.
title: 미디어 휴지 만들기, 예약 및 관리
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: df79bb7d705dbb7f26661a8977b9670aa2a76e02
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175717"
---
# <a name="create-schedule-and-manage-media-breaks"></a>미디어 휴지 만들기, 예약 및 관리

이 문서에서는 미디어 재생 앱에 대 한 미디어 나누기를 만들고 예약 하 고 관리 하는 방법을 보여 줍니다. 미디어 나누기는 일반적으로 미디어 콘텐츠에 오디오 또는 비디오 광고를 삽입 하는 데 사용 됩니다. Windows 10, 버전 1607부터 [**MediaBreakManager**](/uwp/api/Windows.Media.Playback.MediaBreakManager) 클래스를 사용 하 여 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer)로 재생 하는 모든 [**mediaplaybackitem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) 에 미디어 나누기를 빠르고 쉽게 추가할 수 있습니다.


하나 이상의 미디어 분리를 예약한 후 시스템은 재생 중 지정 된 시간에 미디어 콘텐츠를 자동으로 재생 합니다. **MediaBreakManager** 는 응용 프로그램이 미디어 중단의 시작, 종료 또는 사용자에 의해 생략 될 때 반응할 수 있도록 이벤트를 제공 합니다. 미디어 나누기에 대 한 [**Mediaplaybacksession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession) 에 액세스 하 여 진행률 업데이트 다운로드 및 버퍼링과 같은 이벤트를 모니터링할 수도 있습니다.

## <a name="schedule-media-breaks"></a>미디어 중단 예약
모든 **Mediaplaybackitem** 개체에는 항목이 재생 될 때 재생 되는 미디어 나누기를 구성 하는 데 사용 하는 자체 [**MediaBreakSchedule**](/uwp/api/Windows.Media.Playback.MediaBreakSchedule) 있습니다. 앱에서 미디어 나누기를 사용 하는 첫 번째 단계는 기본 재생 콘텐츠에 대 한 [**Mediaplaybackitem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) 을 만드는 것입니다. 

[!code-cs[MoviePlaybackItem](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMoviePlaybackItem)]

**Mediaplaybackitem**, [**mediaplaybackitem**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) 및 기타 기본 미디어 재생 api를 사용 하는 방법에 대 한 자세한 내용은 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)을 참조 하세요.

다음 예제에서는 **Mediaplaybackitem**에 미리 받기 나누기를 추가 하는 방법을 보여 줍니다. 즉, 중단 된 재생 항목을 재생 하기 전에 시스템에서 미디어 나누기를 재생 합니다. 먼저 새 [**MediaBreak**](/uwp/api/Windows.Media.Playback.MediaBreak) 개체가 인스턴스화됩니다. 이 예제에서는 [**MediaBreakInsertionMethod**](/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod)를 사용 하 여 생성자를 호출 합니다. 즉, 중단 콘텐츠가 재생 되는 동안 주 콘텐츠가 일시 중지 됩니다. 

다음으로, 중단 중에 재생되는 광고 등의 콘텐츠에 대한 새 **MediaPlaybackItem**이 만들어집니다. 이 재생 항목의 [**Canskip**](/uwp/api/windows.media.playback.mediaplaybackitem.canskip) 속성은 false로 설정 됩니다. 이는 사용자가 기본 제공 미디어 컨트롤을 사용 하 여 항목을 건너뛸 수 없음을 의미 합니다. 앱은 [**Skipcurrentbreak**](/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak)를 호출 하 여 프로그래밍 방식으로 추가를 건너뛰도록 선택할 수 있습니다. 

미디어 중단의 [**playbacklist**](/uwp/api/windows.media.playback.mediabreak.playbacklist) 속성은 여러 미디어 항목을 재생 목록으로 재생할 수 있는 [**mediaplaybacklist**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) 입니다. 미디어 중단의 재생 목록에 포함 하려면 목록 **항목** 컬렉션에서 하나 이상의 **Mediaplaybackitem** 개체를 추가 합니다.

마지막으로 주 콘텐츠 재생 항목의 기능 [**일정**](/uwp/api/windows.media.playback.mediaplaybackitem.breakschedule) 속성을 사용 하 여 미디어 중단을 예약 합니다. 일정 개체의 [**PrerollBreak**](/uwp/api/windows.media.playback.mediabreakschedule.prerollbreak) 속성에 할당 하 여 한도를 미리 받기로 지정 합니다.

[!code-cs[PreRollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPreRollBreak)]

이제 주 미디어 항목을 재생할 수 있으며, 만든 미디어 나누기는 주 콘텐츠 이전에 재생 됩니다. 새 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 개체를 만들고 필요에 [**따라 자동 재생 속성을**](/uwp/api/windows.media.playback.mediaplayer.autoplay) true로 설정 하 여 재생을 자동으로 시작 합니다. **MediaPlayer** 의 [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) 속성을 주 콘텐츠 재생 항목으로 설정 합니다. 필수는 아니지만 [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 에 **MediaPlayer** 를 할당 하 여 XAML 페이지에서 미디어를 렌더링할 수 있습니다. **Mediaplayer**를 사용 하는 방법에 대 한 자세한 내용은 mediaplayer를 사용 하 [여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)을 참조 하세요.

[!code-cs[Play](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPlay)]

**MediaBreak** 개체를 [**PostrollBreak**](/uwp/api/windows.media.playback.mediabreakschedule.postrollbreak) 속성에 할당 하는 것을 제외 하 고 미리 받기와 동일한 기술을 사용 하 여 주 콘텐츠를 포함 하는 **mediaplaybackitem** 재생이 완료 된 후에 재생 되는 postroll break를 추가 합니다.

[!code-cs[PostRollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPostRollBreak)]

주 콘텐츠를 재생 하는 동안 지정 된 시간에 재생 되는 하나 이상의 midroll 나누기를 예약할 수도 있습니다. 다음 예제에서는 [**MediaBreak**](/uwp/api/Windows.Media.Playback.MediaBreak) 이 **TimeSpan** 개체를 허용 하는 생성자 오버 로드를 사용 하 여 생성 됩니다 .이 경우 구분선이 재생 될 때 주 미디어 항목 재생 내의 시간을 지정 합니다. 다시, [**MediaBreakInsertionMethod**](/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod) 는 중단을 재생 하는 동안 주 콘텐츠의 재생이 일시 중지 됨을 나타내기 위해 지정 됩니다. [**InsertMidrollBreak**](/uwp/api/windows.media.playback.mediabreakschedule.insertmidrollbreak)를 호출 하 여 midroll break를 일정에 추가 합니다. [**MidrollBreaks**](/uwp/api/windows.media.playback.mediabreakschedule.midrollbreaks) 속성에 액세스 하 여 일정에서 현재 midroll 나누기의 읽기 전용 목록을 가져올 수 있습니다.

[!code-cs[MidrollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMidrollBreak)]

다음 midroll break 예제에서는 [**MediaBreakInsertionMethod**](/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod) 삽입 메서드를 사용 합니다. 즉, 중단을 재생 하는 동안 시스템에서 주 콘텐츠를 계속 처리 합니다. 이 옵션은 일반적으로 광고를 재생 하는 동안 콘텐츠를 일시 중지 하 고 라이브 스트림 뒤에 포함 하지 않으려는 라이브 스트리밍 미디어 앱에서 사용 됩니다. 

또한이 예제에서는 두 [**TimeSpan**](/uwp/api/Windows.Foundation.TimeSpan) 매개 변수를 허용 하는 [**Mediaplaybackitem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) 생성자의 오버 로드를 사용 합니다. 첫 번째 매개 변수는 재생이 시작 되는 미디어 중단 항목 내의 시작 지점을 지정 합니다. 두 번째 매개 변수는 미디어 중단 항목을 재생할 기간을 지정 합니다. 따라서 다음 예제에서 **MediaBreak** 는 주 콘텐츠로 20 분 내에 재생을 시작 합니다. 미디어 항목을 재생 하면 미디어 항목은 중단 미디어 항목의 처음부터 30 초 후에 시작 되며, 주 미디어 콘텐츠가 재생을 다시 시작 하기 전에 15 초 동안 재생 됩니다.

[!code-cs[MidrollBreak2](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMidrollBreak2)]

## <a name="skip-media-breaks"></a>미디어 나누기 건너뛰기
이 문서 앞부분에서 설명한 것 처럼 사용자가 기본 제공 컨트롤을 사용 하 여 콘텐츠를 건너뛰지 못하게 하려면 **Mediaplaybackitem** 의 [**canskip**](/uwp/api/windows.media.playback.mediaplaybackitem.canskip) 속성을 설정할 수 있습니다. 그러나 언제 든 지 코드에서 [**Skipcurrentbreak**](/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak) 를 호출 하 여 현재 중단을 건너뛸 수 있습니다.

[!code-cs[SkipButtonClick](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetSkipButtonClick)]

## <a name="handle-mediabreak-events"></a>MediaBreak 이벤트 처리

미디어 나누기의 변경 상태에 따라 작업을 수행 하기 위해 등록할 수 있는 미디어 나누기와 관련 된 몇 가지 이벤트가 있습니다.

[!code-cs[RegisterMediaBreakEvents](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetRegisterMediaBreakEvents)]

미디어 중단이 시작 되 면 [**시작**](/uwp/api/windows.media.playback.mediabreakmanager.breakstarted) 이 발생 합니다. 사용자가 미디어 중단 콘텐츠가 재생 되 고 있음을 사용자에 게 알릴 수 있도록 UI를 업데이트 하는 것이 좋습니다. 이 예제에서는 처리기에 전달 된 [**MediaBreakStartedEventArgs**](/uwp/api/Windows.Media.Playback.MediaBreakStartedEventArgs) 를 사용 하 여 시작 된 미디어 중단에 대 한 참조를 가져옵니다. 그런 다음 [**Currentitemindex**](/uwp/api/windows.media.playback.mediaplaybacklist.currentitemindex) 속성은 미디어 중단의 재생 목록에서 재생할 미디어 항목을 결정 하는 데 사용 됩니다. 그러면 UI가 업데이트 되어 사용자에 게 현재 ad 인덱스와 중단 된 광고 수가 표시 됩니다. UI 업데이트는 UI 스레드에서 수행해야 하므로 [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) 호출 내에서 호출해야 합니다. 

[!code-cs[BreakStarted](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakStarted)]

중단의 모든 미디어 항목 재생이 완료 되었거나 [**생략 된 경우**](/uwp/api/windows.media.playback.mediabreakmanager.breakended) 에 발생 합니다. 이 이벤트에 대 한 처리기를 사용 하 여 미디어 중단 콘텐츠가 더 이상 재생 되지 않음을 나타내는 UI를 업데이트할 수 있습니다.

[!code-cs[BreakEnded](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakEnded)]

사용자가 [**Canskip**](/uwp/api/windows.media.playback.mediaplaybackitem.canskip) 이 true 인 항목을 재생 하는 동안 기본 제공 UI에서 *다음* 단추를 누를 때 또는 [**skipcurrentbreak**](/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak)를 호출 하 여 코드의 중단을 건너뛰는 경우에는 **생략** 이벤트가 발생 합니다.

다음 예제에서는 **MediaPlayer** 의 [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) 속성을 사용 하 여 주 콘텐츠의 미디어 항목에 대 한 참조를 가져옵니다. 건너뛴 미디어 나누기는이 항목의 중단 일정에 속합니다. 그런 다음 코드는 건너뛴 미디어 나누기가 일정의 [**PrerollBreak**](/uwp/api/windows.media.playback.mediabreakschedule.prerollbreak) 속성으로 설정 된 미디어 나누기와 동일한 지 확인 합니다. 이 경우에는 미리 받기 중단이 건너뛴 중단 이었던 것입니다 .이 경우 새 midroll 바꿈이 만들어지고 주 콘텐츠로 10 분이 재생 되도록 예약 됩니다.

[!code-cs[BreakSkipped](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakSkipped)]

[**BreaksSeekedOver**](/uwp/api/windows.media.playback.mediabreakmanager.breaksseekedover) 는 주 미디어 항목의 재생 위치가 예약 된 시간 동안 하나 이상의 미디어 분할에 대해 전달 될 때 발생 합니다. 다음 예에서는 두 개 이상의 미디어 중단이 seeked 되었는지 확인 하 고, 재생 위치가 앞으로 이동 되었으며, 10 분 미만으로 이동 되었는지 확인 합니다. 그렇다면 이벤트 인수에 의해 노출 되는 [**SeekedOverBreaks**](/uwp/api/windows.media.playback.mediabreakseekedovereventargs.seekedoverbreaks) 컬렉션에서 가져온 첫 번째 중단은 **MediaPlayer Seeked Manager**의 [**playbreak**](/uwp/api/windows.media.playback.mediabreakmanager.playbreak) 메서드를 호출 하 여 즉시 재생 됩니다.

[!code-cs[BreakSeekedOver](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakSeekedOver)]


## <a name="access-the-current-playback-session"></a>현재 재생 세션에 액세스
[**Mediaplaybacksession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession) 개체는 **MediaPlayer** 클래스를 사용 하 여 현재 재생 중인 미디어 콘텐츠와 관련 된 데이터 및 이벤트를 제공 합니다. 또한 [**MediaBreakManager**](/uwp/api/Windows.Media.Playback.MediaBreakManager) 에는 재생할 미디어 중단 콘텐츠와 관련 된 데이터 및 이벤트를 가져오기 위해 액세스할 수 있는 **Mediaplaybacksession** 이 있습니다. 재생 세션에서 얻을 수 있는 정보에는 현재 재생 상태, 재생 중 또는 일시 중지 됨 및 콘텐츠 내의 현재 재생 위치가 포함 됩니다. [**NaturalVideoWidth**](/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideowidth) 및 [**NaturalVideoHeight**](/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideoheight) 속성 및 [**NaturalVideoSizeChanged**](/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideosizechanged) 를 사용 하 여 미디어 나누기 콘텐츠가 주 콘텐츠와 다른 가로 세로 비율을 사용 하는 경우 비디오 UI를 조정할 수 있습니다. 또한 응용 프로그램의 성능에 대 한 중요 한 원격 분석을 제공할 수 있는 [**Bufferingstarted**](/uwp/api/windows.media.playback.mediaplaybacksession.bufferingstarted), [**bufferingstarted**](/uwp/api/windows.media.playback.mediaplaybacksession.bufferingended)및 [**DownloadProgressChanged**](/uwp/api/windows.media.playback.mediaplaybacksession.downloadprogresschanged) 와 같은 이벤트를 받을 수 있습니다.

다음 예제에서는 **BufferingProgressChanged 이벤트**에 대 한 처리기를 등록 합니다. 이벤트 처리기에서 UI를 업데이트 하 여 현재 버퍼링 진행률을 표시 합니다.

[!code-cs[RegisterBufferingProgressChanged](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetRegisterBufferingProgressChanged)]

[!code-cs[BufferingProgressChanged](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBufferingProgressChanged)]

## <a name="related-topics"></a>관련 항목
* [미디어 재생](media-playback.md)
* [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)
* [시스템 미디어 전송 컨트롤의 수동 컨트롤](system-media-transport-controls.md)

 

 