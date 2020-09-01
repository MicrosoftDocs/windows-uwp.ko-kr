---
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: SystemMediaTransportControls 클래스를 사용하면 Windows에서 기본 제공되는 앱에서 시스템 미디어 전송 컨트롤을 사용하고 앱이 현재 재생 중인 미디어에 대해 컨트롤이 표시하는 메타데이터를 업데이트할 수 있습니다.
title: 시스템 미디어 전송 컨트롤의 수동 컨트롤
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8650ed0d7dc5caceca5d58de61f48f254a571914
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175687"
---
# <a name="manual-control-of-the-system-media-transport-controls"></a>시스템 미디어 전송 컨트롤의 수동 컨트롤


Windows 10 버전 1607부터 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 클래스를 사용 하 여 미디어를 재생 하는 UWP 앱은 기본적으로 시스템 미디어 전송 컨트롤 (smtc)과 자동으로 통합 됩니다. 이 방법은 대부분의 시나리오에서 SMTC와 상호 작용 하는 데 권장 되는 방법입니다. **MediaPlayer**와 함께 smtc의 기본 통합을 사용자 지정 하는 방법에 대 한 자세한 내용은 [시스템 미디어 전송 컨트롤과의 통합](integrate-with-systemmediatransportcontrols.md)을 참조 하세요.

SMTC의 수동 제어를 구현 해야 하는 몇 가지 시나리오가 있습니다. [**MediaTimelineController**](/uwp/api/Windows.Media.MediaTimelineController) 를 사용 하 여 하나 이상의 미디어 플레이어 재생을 제어 하는 경우에 이러한 작업이 포함 됩니다. 또는 여러 미디어 플레이어를 사용 하 고 응용 프로그램에 대해 하나의 SMTC 인스턴스만 사용 하려는 경우. [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 를 사용 하 여 미디어를 재생 하는 경우 smtc를 수동으로 제어 해야 합니다.

## <a name="set-up-transport-controls"></a>전송 제어 설정
**MediaPlayer** 를 사용 하 여 미디어를 재생 하는 경우 [**MediaPlayer.SystemMediaTransportControls**](/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols) 속성에 액세스 하 여 [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) 클래스의 인스턴스를 가져올 수 있습니다. SMTC를 수동으로 제어 하려면 [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) 속성을 false로 설정 하 여 **MediaPlayer** 에서 제공 하는 자동 통합을 사용 하지 않도록 설정 해야 합니다.

> [!NOTE] 
> [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) 를 false로 설정 하 여 [**Mediaplayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 의 [**mediaplaybackcommandmanager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) 를 사용 하지 않도록 설정 하는 경우 **MediaPlayerElement**에서 제공 하는 **mediaplayer** [**컨트롤**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) 사이의 연결을 중단 하므로 기본 제공 전송 컨트롤은 플레이어의 재생을 더 이상 자동으로 제어 하지 않습니다. 대신 사용자 고유의 컨트롤을 구현 하 여 **MediaPlayer**를 제어 해야 합니다.

[!code-cs[InitSMTCMediaPlayer](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetInitSMTCMediaPlayer)]

[**GetForCurrentView**](/uwp/api/windows.media.systemmediatransportcontrols.getforcurrentview)를 호출 하 여 [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) 인스턴스를 가져올 수도 있습니다. **MediaElement** 를 사용 하 여 미디어를 재생 하는 경우이 메서드를 사용 하 여 개체를 가져와야 합니다.

[!code-cs[InitSMTCMediaElement](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetInitSMTCMediaElement)]

[**Isplayenabled**](/uwp/api/windows.media.systemmediatransportcontrols.isplayenabled), [**IsPauseEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.ispauseenabled), [**Isplayenabled**](/uwp/api/windows.media.systemmediatransportcontrols.isnextenabled)및 [**IsPreviousEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.ispreviousenabled)와 같은 **SystemMediaTransportControls** 개체의 해당 "is enabled" 속성을 설정 하 여 앱에서 사용 하는 단추를 사용 하도록 설정 합니다. 사용 가능한 컨트롤의 전체 목록은 **SystemMediaTransportControls** 참조 설명서를 참조 하세요.

[!code-cs[EnableContols](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetEnableContols)]

사용자가 단추를 누를 때 알림을 받도록 [**Buttonpressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) 이벤트에 대 한 처리기를 등록 합니다.

[!code-cs[RegisterButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterButtonPressed)]

## <a name="handle-system-media-transport-controls-button-presses"></a>시스템 미디어 전송 컨트롤 단추 누름 처리

[**Buttonpressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) 이벤트는 사용 단추 중 하나를 누를 때 시스템 전송 컨트롤에 의해 발생 합니다. 이벤트 처리기에 전달 되는 [**SystemMediaTransportControlsButtonPressedEventArgs**](/uwp/api/Windows.Media.SystemMediaTransportControlsButtonPressedEventArgs) 의 [**Button**](/uwp/api/windows.media.systemmediatransportcontrolsbuttonpressedeventargs.button) 속성은 사용 하도록 설정 된 단추를 나타내는 [**SystemMediaTransportControlsButton**](/uwp/api/Windows.Media.SystemMediaTransportControlsButton) 열거형의 멤버입니다.

[**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 개체와 같은 [**buttonpressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) 이벤트 처리기에서 UI 스레드의 개체를 업데이트 하려면 [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)를 통해 호출을 마샬링해야 합니다. 이는 **Buttonpressed** 이벤트 처리기가 ui 스레드에서 호출 되지 않기 때문 이며, 따라서 ui를 직접 수정 하려고 하면 예외가 throw 됩니다.

[!code-cs[SystemMediaTransportControlsButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsButtonPressed)]

## <a name="update-the-system-media-transport-controls-with-the-current-media-status"></a>현재 미디어 상태를 사용 하 여 시스템 미디어 전송 컨트롤 업데이트

시스템이 현재 상태를 반영 하도록 컨트롤을 업데이트할 수 있도록 미디어의 상태가 변경 되 면 [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) 에 게 알려야 합니다. 이렇게 하려면 [**Playbackstatus**](/uwp/api/windows.media.systemmediatransportcontrols.playbackstatus) 속성을 [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement)의 [**CurrentStateChanged**](/uwp/api/windows.ui.xaml.controls.mediaelement.currentstatechanged) 이벤트 내에서 적절 한 [**mediaplaybackstatus**](/uwp/api/Windows.Media.MediaPlaybackStatus) 값으로 설정 합니다 .이 값은 미디어 상태가 변경 될 때 발생 합니다.

[!code-cs[SystemMediaTransportControlsStateChange](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsStateChange)]

## <a name="update-the-system-media-transport-controls-with-media-info-and-thumbnails"></a>미디어 정보 및 미리 보기를 사용 하 여 시스템 미디어 전송 컨트롤 업데이트

[**SystemMediaTransportControlsDisplayUpdater**](/uwp/api/Windows.Media.SystemMediaTransportControlsDisplayUpdater) 클래스를 사용 하 여 현재 재생 중인 미디어 항목의 노래 제목 또는 앨범 아트와 같은 전송 컨트롤에 표시 되는 미디어 정보를 업데이트 합니다. [**SystemMediaTransportControls**](/uwp/api/windows.media.systemmediatransportcontrols.displayupdater) 속성을 사용 하 여이 클래스의 인스턴스를 가져옵니다. 일반적인 시나리오의 경우 메타 데이터를 전달 하는 권장 방법은 현재 재생 중인 미디어 파일을 전달 하 여 [**CopyFromFileAsync**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.copyfromfileasync)를 호출 하는 것입니다. 표시 업데이트 프로그램이 파일에서 메타 데이터 및 미리 보기 이미지를 자동으로 추출 합니다.

[**업데이트**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.update) 를 호출 하 여 시스템 미디어 전송 컨트롤이 새 메타 데이터 및 미리 보기를 사용 하 여 UI를 업데이트 하도록 합니다.

[!code-cs[SystemMediaTransportControlsUpdater](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsUpdater)]

시나리오에 필요한 경우 [**displayupdater**](/uwp/api/windows.media.systemmediatransportcontrols.displayupdater) 클래스에서 노출 하는 [**MusicProperties**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.musicproperties), [**imageproperties**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.imageproperties)또는 [**videoproperties**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.videoproperties) 개체의 값을 설정 하 여 시스템 미디어 전송 컨트롤에 의해 표시 되는 메타 데이터를 수동으로 업데이트할 수 있습니다.

[!code-cs[SystemMediaTransportControlsUpdaterManual](./code/SMTCWin10/cs/MainPage.xaml.cs#SystemMediaTransportControlsUpdaterManual)]

> [!Note]
> 앱은 시스템 미디어 전송 컨트롤에 의해 표시 되는 다른 미디어 메타 데이터를 제공 하지 않는 경우에도 [SystemMediaTransportControlsDisplayUpdater](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.type#Windows_Media_SystemMediaTransportControlsDisplayUpdater_Type
) 속성에 대 한 값을 설정 해야 합니다. 이 값을 사용 하면 재생 하는 동안 화면 보호기를 활성화 하는 것을 방지 하는 등 시스템에서 미디어 콘텐츠를 올바르게 처리할 수 있습니다.


## <a name="update-the-system-media-transport-controls-timeline-properties"></a>시스템 미디어 전송 컨트롤 타임 라인 속성 업데이트

시스템 전송 컨트롤은 현재 재생 위치, 시작 시간 및 미디어 항목의 종료 시간을 포함 하 여 현재 재생 중인 미디어 항목의 타임 라인에 대 한 정보를 표시 합니다. 시스템 전송 컨트롤 타임 라인 속성을 업데이트 하려면 새 [**SystemMediaTransportControlsTimelineProperties**](/uwp/api/Windows.Media.SystemMediaTransportControlsTimelineProperties) 개체를 만듭니다. 재생 중인 미디어 항목의 현재 상태를 반영 하도록 개체의 속성을 설정 합니다. SystemMediaTransportControls를 호출 하 여 컨트롤이 타임 라인을 업데이트 하도록 합니다 [**.**](/uwp/api/windows.media.systemmediatransportcontrols.updatetimelineproperties)

[!code-cs[UpdateTimelineProperties](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetUpdateTimelineProperties)]

-   시스템 컨트롤이 재생 항목에 대 한 타임 라인을 표시 하려면 [**StartTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.starttime), [**EndTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.endtime) 및 [**Position**](/uwp/api/windows.media.systemmediatransportcontrols.playbackpositionchangerequested) 의 값을 제공 해야 합니다.

-   [**MinSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.minseektime) 및 [**MaxSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.maxseektime) 를 사용 하면 사용자가 검색할 수 있는 타임 라인 내 범위를 지정할 수 있습니다. 이에 대 한 일반적인 시나리오는 콘텐츠 공급자가 해당 미디어에 광고 나누기를 포함 하도록 허용 하는 것입니다.

    [**Positionchangerequest**](/uwp/api/windows.media.systemmediatransportcontrols.playbackpositionchangerequested) 를 발생 시키려면 [**MinSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.minseektime) 및 [**MaxSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.maxseektime) 를 설정 해야 합니다.

-   재생 하는 동안에는 약 5 초 마다 이러한 속성을 업데이트 하 고 일시 중지 하거나 새 위치로 이동 하는 등의 재생 상태를 변경 하 여 시스템 컨트롤을 미디어 재생과 동기화 된 상태로 유지 하는 것이 좋습니다.

## <a name="respond-to-player-property-changes"></a>플레이어 속성 변경 내용에 응답

미디어 플레이어 자체의 현재 상태와 관련 된 시스템 전송 컨트롤 속성 집합이 있습니다. 미디어 항목 재생 중 상태는 아닙니다. 이러한 각 속성은 사용자가 연결 된 컨트롤을 조정할 때 발생 하는 이벤트와 일치 합니다. 이러한 속성 및 이벤트는 다음과 같습니다.

| 속성                                                                  | 이벤트                                                                                                   |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [**AutoRepeatMode**](/uwp/api/windows.media.systemmediatransportcontrols.autorepeatmode) | [**AutoRepeatModeChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.autorepeatmodechangerequested) |
| [**PlaybackRate**](/uwp/api/windows.media.systemmediatransportcontrols.playbackrate)     | [**PlaybackRateChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.playbackratechangerequested)     |
| [**ShuffleEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.shuffleenabled) | [**ShuffleEnabledChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.shuffleenabledchangerequested) |

 
이러한 컨트롤 중 하나를 사용 하 여 사용자 상호 작용을 처리 하려면 먼저 연결 된 이벤트에 대 한 처리기를 등록 합니다.

[!code-cs[RegisterPlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterPlaybackChangedHandler)]

이벤트 처리기에서 먼저 요청 된 값이 유효 하 고 예상 범위 내에 있는지 확인 합니다. 인 경우 [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 에서 해당 속성을 설정 하 고 [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) 개체에서 해당 속성을 설정 합니다.

[!code-cs[PlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetPlaybackChangedHandler)]

-   이러한 플레이어 속성 이벤트 중 하나가 발생 하려면 속성의 초기 값을 설정 해야 합니다. 예를 들어 [**Playbackrate**](/uwp/api/windows.media.systemmediatransportcontrols.playbackrate) 속성의 값을 한 번 이상 설정한 후에 야 [**PlaybackRateChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.playbackratechangerequested) 이 발생 합니다.

## <a name="use-the-system-media-transport-controls-for-background-audio"></a>배경 오디오에 시스템 미디어 전송 컨트롤 사용

**MediaPlayer** 에서 제공 하는 자동 smtc 통합을 사용 하지 않는 경우에는 수동으로 smtc와 통합 하 여 배경 오디오를 사용 하도록 설정 해야 합니다. 최소한 앱에서 [**Isplayenabled**](/uwp/api/windows.media.systemmediatransportcontrols.isplayenabled) 및 [**IsPauseEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.ispauseenabled) 를 true로 설정 하 여 재생 및 일시 중지 단추를 사용 하도록 설정 해야 합니다. 앱은 [**Buttonpressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) 이벤트도 처리 해야 합니다. 앱이 이러한 요구 사항을 충족 하지 않는 경우 앱이 백그라운드로 이동할 때 오디오 재생이 중지 됩니다.

백그라운드 오디오에 대해 새로운 단일 프로세스 모델을 사용 하는 앱은 [**GetForCurrentView**](/uwp/api/windows.media.systemmediatransportcontrols.getforcurrentview)를 호출 하 여 [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) 인스턴스를 가져와야 합니다. 백그라운드 오디오에 대해 레거시 2 프로세스 모델을 사용 하는 앱은 [**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols) 를 사용 하 여 백그라운드 프로세스에서 smtc에 액세스할 수 있어야 합니다.

배경에서 오디오를 재생 하는 방법에 대 한 자세한 내용은 [백그라운드에서 미디어 재생](background-audio.md)을 참조 하세요.

## <a name="related-topics"></a>관련 항목
* [미디어 재생](media-playback.md)
* [시스템 미디어 전송 컨트롤과 통합](integrate-with-systemmediatransportcontrols.md) 
* [시스템 미디어 및 포트 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls) 

 