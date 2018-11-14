---
author: drewbatgit
ms.assetid: 0309c7a1-8e4c-4326-813a-cbd9f8b8300d
description: 이 문서에서는 미디어 재생 앱에서 미디어 중단을 만들고, 예약하고, 관리하는 방법을 보여 줍니다.
title: 미디어 휴지 만들기, 예약 및 관리
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0feb7f6771254bf500e4b64fd0e632daad9817e4
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6187413"
---
# <a name="create-schedule-and-manage-media-breaks"></a>미디어 휴지 만들기, 예약 및 관리

이 문서에서는 미디어 재생 앱에서 미디어 중단을 만들고, 예약하고, 관리하는 방법을 보여 줍니다. 미디어 중단은 일반적으로 미디어 콘텐츠에 오디오 또는 비디오 광고를 추가하는 데 사용됩니다. Windows 10 버전 1607부터 [**MediaBreakManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager) 클래스를 사용하여 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer)에서 재생하는 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem)에 미디어 중단을 쉽고 빠르게 추가할 수 있습니다.


하나 이상의 미디어 중단을 예약하면 재생 중 지정된 시간에 미디어 콘텐츠가 자동으로 재생됩니다. **MediaBreakManager**는 미디어 중단이 시작되거나 종료될 때 또는 사용자가 건너뛸 때 앱에서 반응할 수 있도록 이벤트를 제공합니다. 미디어 중단에 대한 [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession)에 액세스하여 다운로드 및 버퍼링 진행 업데이트와 같은 이벤트를 모니터링할 수도 있습니다.

## <a name="schedule-media-breaks"></a>미디어 중단 예약
모든 **MediaPlaybackItem** 개체에는 항목을 재생할 때 재생되는 미디어 중단을 구성하는 데 사용하는 해당 [**MediaBreakSchedule**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSchedule)이 있습니다. 앱에서 미디어 중단을 사용하기 위한 첫 번째 단계는 기본 재생 콘텐츠에 대한 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem)을 만드는 것입니다. 

[!code-cs[MoviePlaybackItem](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMoviePlaybackItem)]

**MediaPlaybackItem**, [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList) 및 다른 기본 미디어 재생 API 작업에 대한 자세한 내용은 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)을 참조하세요.

다음 예제에서는 중단이 속하는 재생 항목을 재생하기 전에 미디어 중단이 재생되는 미리 받기 중단을 **MediaPlaybackItem**에 추가하는 방법을 보여 줍니다. 먼저 새 [**MediaBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreak) 개체가 인스턴스화됩니다. 이 예제에서는 [**MediaBreakInsertionMethod.Interrupt**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakInsertionMethod)를 사용하여 생성자가 호출되므로 중단 콘텐츠를 재생하는 동안 기본 콘텐츠는 일시 중지됩니다. 

다음으로, 중단 중에 재생되는 광고 등의 콘텐츠에 대한 새 **MediaPlaybackItem**이 만들어집니다. 이 재생 항목의 [**CanSkip**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.CanSkip) 속성은 false로 설정됩니다. 즉, 사용자가 기본 제공 미디어 컨트롤을 사용하여 항목을 건너뛸 수 없습니다. 하지만 앱에서 [**SkipCurrentBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.SkipCurrentBreak)를 호출하여 프로그래밍 방식으로 추가를 건너뛸 수 있습니다. 

미디어 중단의 [**PlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreak.PlaybackList) 속성은 여러 미디어 항목을 하나의 재생 목록으로 재생할 수 있도록 하는 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList)입니다. 목록의 **항목** 컬렉션에서 **MediaPlaybackItem** 개체를 하나 이상 추가하여 미디어 중단 재생 목록에 포함합니다.

마지막으로, 기본 콘텐츠 재생 항목의 [**BreakSchedule**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.BreakSchedule) 속성을 사용하여 미디어 중단을 예약합니다. 일정 개체의 [**PrerollBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSchedule.PrerollBreak) 속성에 할당하여 중단을 미리 받기 중단으로 지정합니다.

[!code-cs[PreRollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPreRollBreak)]

이제 기본 미디어 항목을 재생할 수 있으며, 만든 미디어 중단이 기본 콘텐츠보다 먼저 재생됩니다. 새 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 개체를 만들고 필요에 따라 [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) 속성을 true로 설정하여 자동으로 재생을 시작합니다. **MediaPlayer**의 [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) 속성을 기본 콘텐츠 재생 항목으로 설정합니다. 필수는 아니지만 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement)에 **MediaPlayer**를 할당하여 XAML 페이지에서 미디어를 렌더링할 수 있습니다. **MediaPlayer** 사용 방법에 대한 자세한 내용은 [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)을 참조하세요.

[!code-cs[Play](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPlay)]

[**PostrollBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSchedule.PostrollBreak) 속성에 **MediaBreak** 개체를 할당한다는 점을 제외하고 미리 받기 중단과 동일한 기술을 사용하여 기본 콘텐츠가 포함된 **MediaPlaybackItem**의 재생이 완료된 후 재생되는 사후 받기 중단을 추가합니다.

[!code-cs[PostRollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPostRollBreak)]

기본 콘텐츠 재생 내의 지정된 시간에 재생되는 중간 받기 중단을 하나 이상 예약할 수도 있습니다. 다음 예제에서는 중단이 재생되는 기본 미디어 항목 재생 내의 시간을 지정하는 **TimeSpan** 개체를 받는 생성자 오버로드를 사용하여 [**MediaBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreak)를 만듭니다. 마찬가지로, 중단이 재생되는 동안 기본 콘텐츠 재생이 일시 중지됨을 나타내기 위해 [**MediaBreakInsertionMethod.Interrupt**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakInsertionMethod)가 지정됩니다. [**InsertMidrollBreak**](https://msdn.microsoft.com/library/windows/apps/mt670692)를 호출하여 일정에 중간 받기 중단이 추가됩니다. [**MidrollBreaks**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSchedule.MidrollBreaks) 속성에 액세스하여 일정에 있는 현재 중간 받기 중단의 읽기 전용 목록을 가져올 수 있습니다.

[!code-cs[MidrollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMidrollBreak)]

아래 표시된 중간 받기 중단 예제에서는 [**MediaBreakInsertionMethod.Replace**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakInsertionMethod) 삽입 메서드를 사용하므로 중단이 재생되는 동안 시스템에서 기본 콘텐츠를 계속 처리합니다. 이 옵션은 광고가 재생되는 동안 콘텐츠가 일시 중지되어 라이브 스트림보다 지연되는 것을 원하지 않는 라이브 스트리밍 미디어 앱에서 주로 사용됩니다. 

또한 이 예제에서는 두 개의 [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.TimeSpan) 매개 변수를 받는 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) 생성자의 오버로드를 사용합니다. 첫 번째 매개 변수는 재생이 시작되는 미디어 중단 항목 내의 시작점을 지정합니다. 두 번째 매개 변수는 미디어 중단 항목이 재생되는 기간을 지정합니다. 따라서 다음 예제에서 **MediaBreak**는 기본 콘텐츠가 20분 동안 재생된 후 재생되기 시작합니다. 재생 시 미디어 항목은 중단 미디어 항목의 시작 부분에서 30초부터 시작하고 기본 미디어 콘텐츠 재생이 다시 시작되기 전에 15초 동안 재생됩니다.

[!code-cs[MidrollBreak2](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMidrollBreak2)]

## <a name="skip-media-breaks"></a>미디어 중단 건너뛰기
이 문서의 앞부분에서 설명한 것처럼 사용자가 기본 제공 컨트롤을 사용하여 콘텐츠를 건너뛰지 못하도록 **MediaPlaybackItem**의 [**CanSkip**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.CanSkip) 속성을 설정할 수 있습니다. 하지만 언제든지 코드에서 [**SkipCurrentBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.SkipCurrentBreak)를 호출하여 현재 중단을 건너뛸 수 있습니다.

[!code-cs[SkipButtonClick](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetSkipButtonClick)]

## <a name="handle-mediabreak-events"></a>MediaBreak 이벤트 처리

미디어 중단 상태 변경에 따라 작업을 수행하기 위해 등록할 수 있는 여러 개의 미디어 중단 관련 이벤트가 있습니다.

[!code-cs[RegisterMediaBreakEvents](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetRegisterMediaBreakEvents)]

[**BreakStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.BreakStarted)는 미디어 중단이 시작될 때 발생합니다. 미디어 중단 콘텐츠가 재생되고 있음을 사용자에게 알리기 위해 UI를 업데이트하는 것이 좋습니다. 이 예제에서는 처리기에 전달된 [**MediaBreakStartedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakStartedEventArgs)를 사용하여 시작된 미디어 중단에 대한 참조를 가져옵니다. 그런 다음 [**CurrentItemIndex**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList.CurrentItemIndex) 속성을 사용하여 재생 중인 미디어 중단 재생 목록의 미디어 항목을 확인합니다. 사용자에게 현재 광고 인덱스 및 중단에 남아 있는 광고 수를 표시하기 위해 UI가 업데이트됩니다. UI 업데이트는 UI 스레드에서 수행해야 하므로 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 호출 내에서 호출해야 합니다. 

[!code-cs[BreakStarted](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakStarted)]

[**BreakEnded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.BreakEnded)는 중단에 포함된 모든 미디어 항목의 재생이 완료되거나 모두 건너뛰었을 때 발생합니다. 이 이벤트 처리기를 통해 UI를 업데이트하여 미디어 중단 콘텐츠가 더 이상 재생되지 않음을 나타낼 수 있습니다.

[!code-cs[BreakEnded](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakEnded)]

**BreakSkipped** 이벤트는 [**CanSkip**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.CanSkip)이 true인 항목을 재생하는 동안 사용자가 기본 제공 UI에서 *다음* 단추를 누르거나 [**SkipCurrentBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.SkipCurrentBreak)를 호출하여 코드에서 중단을 건너뛸 때 발생합니다.

다음 예제에서는 **MediaPlayer**의 [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) 속성을 사용하여 기본 콘텐츠의 미디어 항목에 대한 참조를 가져옵니다. 건너뛴 미디어 중단은 이 항목의 중단 일정에 속합니다. 코드에서 건너뛴 중단이 일정의 [**PrerollBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSchedule.PrerollBreak) 속성에 설정된 미디어 중단과 같은지 확인합니다. 같으면 미리 받기 중단을 건너뛴 것이며, 이 경우 새 중간 받기 중단이 생성되고 기본 콘텐츠가 10분 동안 재생된 후 재생되도록 예약됩니다.

[!code-cs[BreakSkipped](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakSkipped)]

[**BreaksSeekedOver**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.BreaksSeekedOver)는 기본 미디어 항목의 재생 위치가 하나 이상의 미디어 중단에 예약된 시간을 지났을 때 발생합니다. 다음 예제에서는 둘 이상의 미디어 중단이 검색되었는지, 재생 위치가 앞으로 이동되었는지, 10분 미만으로 앞으로 이동되었는지 확인합니다. 맞으면 이벤트 인수에 의해 노출된 [**SeekedOverBreaks**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSeekedOverEventArgs.SeekedOverBreaks) 컬렉션에서 가져온 검색된 첫 번째 중단이 **MediaPlayer.BreakManager**의 [**PlayBreak**](https://msdn.microsoft.com/library/windows/apps/mt670689) 메서드를 호출하여 즉시 재생됩니다.

[!code-cs[BreakSeekedOver](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakSeekedOver)]


## <a name="access-the-current-playback-session"></a>현재 재생 세션 액세스
[**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession) 개체는 **MediaPlayer** 클래스를 사용하여 현재 재생 중인 미디어 콘텐츠와 관련된 데이터 및 이벤트를 제공합니다. [**MediaBreakManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager)에는 특히 재생 중인 미디어 중단 콘텐츠와 관련된 데이터 및 이벤트를 가져오기 위해 액세스할 수 있는 **MediaPlaybackSession**도 있습니다. 재생 세션에서 가져올 수 있는 정보에는 현재 재생 상태(재생 중 또는 일시 중지됨), 콘텐츠 내의 현재 재생 위치 등이 포함됩니다. 미디어 중단 콘텐츠의 가로 세로 비율이 기본 콘텐츠와 다른 경우 [**NaturalVideoWidth**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NaturalVideoWidth) 및 [**NaturalVideoHeight**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NaturalVideoHeight) 속성과 [**NaturalVideoSizeChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NaturalVideoSizeChanged)를 사용하여 비디오 UI를 조정할 수 있습니다. 앱 성능에 대한 중요한 원격 분석을 제공할 수 있는 [**BufferingStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.BufferingStarted), [**BufferingEnded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.BufferingEnded), [**DownloadProgressChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.DownloadProgressChanged) 등의 이벤트를 받을 수도 있습니다.

다음 예제에서는 **BufferingProgressChanged 이벤트**의 처리기를 등록합니다. 이벤트 처리기에서 UI를 업데이트하여 현재 버퍼링 진행 상황을 표시합니다.

[!code-cs[RegisterBufferingProgressChanged](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetRegisterBufferingProgressChanged)]

[!code-cs[BufferingProgressChanged](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBufferingProgressChanged)]

## <a name="related-topics"></a>관련 항목
* [미디어 재생](media-playback.md)
* [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)
* [시스템 미디어 전송 컨트롤의 수동 컨트롤](system-media-transport-controls.md)

 

 




