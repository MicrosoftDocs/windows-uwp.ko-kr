---
ms.assetid: eb690f2b-3bf8-4a65-99a4-2a3a8c7760b7
description: 이 문서에서는 시스템 미디어 전송 컨트롤과 상호 작용 하는 방법을 보여 줍니다.
title: 시스템 미디어 전송 컨트롤과 통합
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 449c8b445e70ffb68d0bc95f96e2b33c57e3b38f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163937"
---
# <a name="integrate-with-the-system-media-transport-controls"></a>시스템 미디어 전송 컨트롤과 통합

이 문서에서는 SMTC (시스템 미디어 전송 컨트롤)와 상호 작용 하는 방법을 보여 줍니다. SMTC는 모든 Windows 10 장치에 공통적으로 적용 되는 컨트롤 집합으로, 사용자가 재생을 위해 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 를 사용 하는 실행 중인 모든 앱의 미디어 재생을 제어할 수 있는 일관 된 방법을 제공 합니다.

미디어 응용 프로그램 개발자는 시스템 미디어 전송 컨트롤을 사용 하 여 기본 제공 시스템 UI와 통합 하 여 음악가, 앨범 제목, 장 제목 등의 미디어 메타 데이터를 표시할 수 있습니다. 시스템 전송 제어를 사용 하면 사용자가 재생을 일시 중지 하 고 재생 목록에서 앞뒤로 건너뛰는 등 기본 제공 시스템 UI를 사용 하 여 미디어 앱의 재생을 제어할 수도 있습니다.

<img alt="System Media Transtport Controls" src="images/smtc.png" />


SMTC와의 통합을 보여 주는 전체 샘플은 [github의 시스템 미디어 및 포트 컨트롤 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)을 참조 하세요.
                    
## <a name="automatic-integration-with-smtc"></a>SMTC와 자동 통합
Windows 10 버전 1607부터 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 클래스를 사용 하 여 미디어를 재생 하는 UWP 앱은 기본적으로 smtc와 자동으로 통합 됩니다. 기본적으로 **MediaPlayer** 의 새 인스턴스를 인스턴스화하고 [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource), [**Mediaplaybackitem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem)또는 [**Mediaplaybackitem**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) 를 플레이어의 [**원본**](/uwp/api/windows.media.playback.mediaplayer.source) 속성에 할당 하면 사용자는 smtc에서 앱 이름을 확인 하 고 smtc 컨트롤을 사용 하 여 재생 목록을 재생 하 고 일시 중지 하 고 이동할 수 있습니다. 

앱에서 한 번에 여러 **MediaPlayer** 개체를 만들고 사용할 수 있습니다. 앱의 각 활성 **MediaPlayer** 인스턴스에 대해 smtc에 별도의 탭이 만들어지므로 사용자가 활성 미디어 플레이어와 실행 중인 다른 앱의 플레이어 간을 전환할 수 있습니다. 현재 SMTC에서 선택 된 미디어 플레이어는 컨트롤에 영향을 줄 것입니다.

응용 프로그램에서 **mediaplayer** 를 사용 하는 방법에 대 한 자세한 내용은 XAML 페이지에서 [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 에 바인딩 포함을 참조 하세요. mediaplayer를 [사용 하 여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)을 참조 하세요. 

**MediaSource**, **mediaplaybackitem**및 **mediaplaybackitem**로 작업 하는 방법에 대 한 자세한 내용은 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)을 참조 하세요.

## <a name="add-metadata-to-be-displayed-by-the-smtc"></a>SMTC에서 표시할 메타 데이터를 추가 합니다.
SMTC의 미디어 항목에 대해 표시 되는 메타 데이터 (예: 비디오 또는 노래 제목)를 추가 하거나 수정 하려면 미디어 항목을 나타내는 **Mediaplaybackitem** 의 표시 속성을 업데이트 해야 합니다. 먼저 [**Getdisplayproperties**](/uwp/api/windows.media.playback.mediaplaybackitem.getdisplayproperties)를 호출 하 여 [**mediaitemdisplayproperties**](/uwp/api/Windows.Media.Playback.MediaItemDisplayProperties) 개체에 대 한 참조를 가져옵니다. 그런 다음 [**유형**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.type) 속성을 사용 하 여 항목의 미디어, 음악 또는 비디오 유형을 설정 합니다. 그런 다음 지정한 미디어 유형에 따라 [**MusicProperties**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.musicproperties) 또는 [**videoproperties**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.videoproperties)의 필드를 채울 수 있습니다. 마지막으로 [**Applydisplayproperties**](/uwp/api/windows.media.playback.mediaplaybackitem.applydisplayproperties)를 호출 하 여 미디어 항목에 대 한 메타 데이터를 업데이트 합니다.

[!code-cs[SetVideoProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetVideoProperties)]

[!code-cs[SetMusicProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetMusicProperties)]


> [!Note]
> 앱은 시스템 미디어 전송 컨트롤에 의해 표시 되는 다른 미디어 메타 데이터를 제공 하지 않는 경우에도 [**Type**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.type) 속성에 대 한 값을 설정 해야 합니다. 이 값을 사용 하면 재생 하는 동안 화면 보호기를 활성화 하는 것을 방지 하는 등 시스템에서 미디어 콘텐츠를 올바르게 처리할 수 있습니다.


## <a name="use-commandmanager-to-modify-or-override-the-default-smtc-commands"></a>CommandManager를 사용 하 여 기본 SMTC 명령을 수정 하거나 재정의 합니다.
앱은 [**Mediaplaybackcommandmanager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) 클래스를 사용 하 여 smtc 컨트롤의 동작을 수정 하거나 완전히 재정의할 수 있습니다. [**Commandmanager**](/uwp/api/windows.media.playback.mediaplayer.commandmanager) 속성에 액세스 하 여 **MediaPlayer** 클래스의 각 인스턴스에 대해 명령 관리자 인스턴스를 가져올 수 있습니다.

기본적으로 **Mediaplaybacklist**의 다음 항목으로 건너뛰는 *다음* 명령과 같은 모든 명령에 대해 명령 관리자는 [**nextreceived**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.nextreceived)와 같은 received 이벤트와 [**nextreceived**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.nextbehavior)와 같이 명령의 동작을 관리 하는 개체를 노출 합니다. 

다음 예제에서는 **Nextreceived** 이벤트에 대해 처리기를 등록 하 고 **Nextreceived**의 [**IsEnabledChanged**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.isenabledchanged) 이벤트에 대 한 처리기를 등록 합니다.

[!code-cs[AddNextHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddNextHandler)]

다음 예제에서는 사용자가 재생 목록에서 5 개 항목을 클릭 한 후 *다음* 명령을 사용 하지 않도록 설정 하 여 콘텐츠 재생을 계속 하기 전에 사용자 조작이 필요한 시나리오를 보여 줍니다. 각 # # **Nextreceived** 이벤트가 발생 하 고 카운터가 증가 합니다. 카운터가 목표 수에 도달 하면 *다음* 명령의 [**EnablingRule**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.enablingrule) 는 [**Never**](/uwp/api/Windows.Media.Playback.MediaCommandEnablingRule)로 설정 되며이는 명령을 사용 하지 않도록 설정 합니다. 

[!code-cs[NextReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetNextReceived)]

명령을 **항상**로 설정할 수도 있습니다. 즉, *다음* 명령 예제에서는 재생 목록에 항목이 더 이상 없는 경우에도 명령이 항상 사용 되도록 설정 됩니다. 또는 명령을 **자동**으로 설정할 수 있습니다. 여기서 시스템은 재생 중인 현재 콘텐츠를 기준으로 명령을 사용 하도록 설정할지 여부를 결정 합니다.

위에서 설명한 시나리오에서 특정 시점에 앱은 *다음* 명령을 다시 사용할 수 있도록 하 고 **EnablingRule** 를 **Auto**로 설정 하 여이를 수행 합니다.

[!code-cs[EnableNextButton](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetEnableNextButton)]

응용 프로그램에는 전경에 있는 동안 재생을 제어 하는 자체 UI가 있을 수 있으므로 [**IsEnabledChanged**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.isenabledchanged) 이벤트를 사용 하 여 처리기에 전달 된 [**MediaPlaybackCommandManagerCommandBehavior**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior) 의 [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.isenabled) 에 액세스 하 여 명령을 사용 하거나 사용 하지 않도록 설정할 수 있습니다.

[!code-cs[IsEnabledChanged](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetIsEnabledChanged)]

SMTC 명령의 동작을 완전히 무시 하는 경우도 있습니다. 아래 예제에서는 앱에서 현재 재생 목록의 트랙 간을 건너뛰지 않고 *다음* 및 *이전* 명령을 사용 하 여 인터넷 라디오 방송국 간을 전환 하는 시나리오를 보여 줍니다. 이전 예제와 같이 명령이 수신 될 때 처리기가 등록 됩니다 .이 경우에는 [**PreviousReceived**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.previousreceived) 이벤트입니다.

[!code-cs[AddPreviousHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddPreviousHandler)]

**PreviousReceived** 처리기에서 먼저 처리기로 전달 된 [**MediaPlaybackCommandManagerPreviousReceivedEventArgs**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManagerPreviousReceivedEventArgs) 의 [**GetDeferral**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagerpreviousreceivedeventargs.getdeferral) 를 호출 하 여 [**지연을**](/uwp/api/Windows.Foundation.Deferral) 가져옵니다. 이는 명령을 실행 하기 전에 deferall 완료 될 때까지 대기 하도록 시스템에 지시 합니다. 이는 처리기에서 비동기 호출을 수행 하는 경우 매우 중요 합니다. 이 시점에서 예제는 이전 라디오 방송국을 나타내는 **Mediaplaybackitem** 을 반환 하는 사용자 지정 메서드를 호출 합니다.

그런 다음 [**처리**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagerpreviousreceivedeventargs.handled) 된 속성을 확인 하 여 다른 처리기에서 이벤트를 아직 처리 하지 않았는지 확인 합니다. 그렇지 않으면 **처리** 된 속성이 true로 설정 됩니다. 이를 통해 SMTC 및 기타 구독 된 처리기는 이미 처리 되었으므로이 명령을 실행 하는 작업을 수행 하지 않아야 함을 알 수 있습니다. 그런 다음이 코드는 미디어 플레이어의 새 원본을 설정 하 고 플레이어를 시작 합니다.

마지막으로, 지연 개체에서 [**Complete**](/uwp/api/windows.foundation.deferral.complete) 를 호출 하 여 사용자가 명령 처리를 완료 하는 것을 시스템에 알릴 수 있습니다.

[!code-cs[PreviousReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetPreviousReceived)]
                 
## <a name="manual-control-of-the-smtc"></a>SMTC의 수동 제어
이 문서 앞부분에서 설명한 것 처럼 SMTC는 앱이 만드는 모든 **MediaPlayer** 인스턴스에 대 한 정보를 자동으로 검색 하 고 표시 합니다. **MediaPlayer** 의 여러 인스턴스를 사용 하지만 smtc에서 앱에 대 한 단일 항목을 제공 하려는 경우 자동 통합에 의존 하는 대신 smtc의 동작을 수동으로 제어 해야 합니다. 또한 [**MediaTimelineController**](/uwp/api/Windows.Media.MediaTimelineController) 를 사용 하 여 미디어 플레이어를 하나 이상 제어 하려면 수동 smtc 통합을 사용 해야 합니다. 또한 앱에서 **MediaPlayer**이외의 API (예: [**오디오 그래프**](/uwp/api/Windows.Media.Audio.AudioGraph) 클래스)를 사용 하 여 미디어를 재생 하는 경우에는 사용자가 smtc를 사용 하 여 앱을 제어 하도록 수동으로 smtc 통합을 구현 해야 합니다. SMTC를 수동으로 제어 하는 방법에 대 한 자세한 내용은 [시스템 미디어 전송 컨트롤의 수동 제어](system-media-transport-controls.md)를 참조 하세요.



## <a name="related-topics"></a>관련 항목
* [미디어 재생](media-playback.md)
* [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)
* [시스템 미디어 전송 컨트롤의 수동 컨트롤](system-media-transport-controls.md)
* [Github의 시스템 미디어/포트 컨트롤 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)
 

 