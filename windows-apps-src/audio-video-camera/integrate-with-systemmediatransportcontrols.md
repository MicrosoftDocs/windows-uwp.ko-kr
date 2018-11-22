---
author: drewbatgit
ms.assetid: eb690f2b-3bf8-4a65-99a4-2a3a8c7760b7
description: 이 문서에서는 시스템 미디어 전송 컨트롤을 조작하는 방법을 보여 줍니다.
title: 시스템 미디어 전송 컨트롤과 통합
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 15d7e3b3e822081a751aa791a76e674b07af8d9d
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7573307"
---
# <a name="integrate-with-the-system-media-transport-controls"></a>시스템 미디어 전송 컨트롤과 통합

이 문서에서는 SMTC(시스템 미디어 전송 컨트롤)를 조작하는 방법을 보여 줍니다. SMTC는 모든 Windows 10 디바이스에 공통적으로 적용되고 사용자가 재생을 위해 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer)를 사용하는 실행 중인 모든 앱의 미디어 재생을 제어할 수 있는 일관된 방법을 제공하는 컨트롤 집합입니다.

SMTC와 통합을 보여 주는 전체 샘플을 보려면 [github의 시스템 미디어 전송 컨트롤 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)을 참조하세요.
                    
## <a name="automatic-integration-with-smtc"></a>SMTC와 자동 통합
Windows 10 버전 1607부터 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 클래스를 사용하여 미디어를 재생하는 UWP 앱은 기본적으로 SMTC와 자동으로 통합됩니다. **MediaPlayer**의 새 인스턴스를 인스턴스화하고 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource), [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) 또는 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList)를 플레이어의 [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) 속성에 할당하기만 하면 사용자가 SMTC에서 앱 이름을 보고 SMTC 컨트롤을 사용하여 재생, 일시 중지 및 재생 목록을 이동할 수 있습니다. 

앱에서 여러 개의 **MediaPlayer** 개체를 만들고 동시에 사용할 수 있습니다. 앱의 각 활성 **MediaPlayer** 인스턴스에 대해 별도의 탭이 SMTC에서 생성되므로 사용자가 활성 미디어 플레이어와 다른 실행 중인 앱의 활성 미디어 플레이어 간에 전환할 수 있습니다. SMTC에서 현재 선택된 미디어 플레이어에 컨트롤이 적용됩니다.

XAML 페이지에서 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement)에 바인딩 등 앱에서 **MediaPlayer**를 사용하는 방법에 대한 자세한 내용은 [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)을 참조하세요. 

**MediaSource**, **MediaPlaybackItem**, **MediaPlaybackList** 작업 방법에 대한 자세한 내용은 [미디어 항목, 재생 목록 및 트랙](media-playback-with-mediasource.md)을 참조하세요.

## <a name="add-metadata-to-be-displayed-by-the-smtc"></a>SMTC에서 표시할 메타데이터 추가
SMTC에서 비디오 또는 노래 제목과 같은 미디어 항목에 표시되는 메타데이터를 수정하거나 추가하려는 경우 미디어 항목을 나타내는 **MediaPlaybackItem**의 표시 속성을 업데이트해야 합니다. 먼저 [**GetDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.GetDisplayProperties)를 호출하여 [**MediaItemDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties) 개체에 대한 참조를 가져옵니다. [**Type**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.Type) 속성을 사용하여 항목에 대한 미디어 유형(음악 또는 비디오)을 설정합니다. 지정한 미디어 유형에 따라 [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.MusicProperties) 또는 [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.VideoProperties) 필드를 채웁니다. 마지막으로, [**ApplyDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/mt489923)를 호출하여 미디어 항목에 대한 메타데이터를 업데이트합니다.

[!code-cs[SetVideoProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetVideoProperties)]

[!code-cs[SetMusicProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetMusicProperties)]

## <a name="use-commandmanager-to-modify-or-override-the-default-smtc-commands"></a>CommandManager를 사용하여 기본 SMTC 명령 수정 또는 재정의
앱에서 [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) 클래스를 사용하여 SMTC 컨트롤의 동작을 수정하거나 완전히 재정의할 수 있습니다. [**CommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.CommandManager) 속성에 액세스하여 각 **MediaPlayer** 클래스 인스턴스에 대한 명령 관리자 인스턴스를 가져올 수 있습니다.

기본적으로 **MediaPlaybackList**의 다음 항목으로 건너뛰는 *Next* 명령 등의 모든 명령에 대해 명령 관리자는 [**NextReceived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.NextReceived) 등의 수신된 이벤트와 [**NextBehavior**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.NextBehavior) 등의 명령 동작을 관리하는 개체를 표시합니다. 

다음 예제에서는 **NextReceived** 이벤트 및 **NextBehavior**의 [**IsEnabledChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior.IsEnabledChanged) 이벤트에 대한 처리기를 등록합니다.

[!code-cs[AddNextHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddNextHandler)]

다음 예제에서는 앱에서 사용자가 재생 목록의 5개 항목을 클릭한 후 *Next* 명령을 사용할 수 없게 설정하여 콘텐츠를 계속 재생하기 전에 일부 사용자 조작을 요구하려는 시나리오를 보여 줍니다. **NextReceived** 이벤트가 발생하는 ##마다 카운터가 증가합니다. 카운터가 대상 개수에 도달하면 *Next* 명령에 대한 [**EnablingRule**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior.EnablingRule)이 [**Never**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaCommandEnablingRule)로 설정되고 명령을 사용할 수 없게 됩니다. 

[!code-cs[NextReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetNextReceived)]

명령을 **Always**로 설정할 수도 있으며, 이 경우 *Next* 명령 예제에서 재생 목록에 항목이 더 이상 없어도 항상 명령을 사용할 수 있습니다. 또는 명령을 **Auto**로 설정하여 시스템에서 현재 재생 중인 콘텐츠에 따라 명령을 사용할 수 있도록 설정할지 여부를 결정하게 할 수 있습니다.

위에서 설명한 시나리오의 경우 일정 시점에 앱이 *Next* 명령을 다시 사용할 수 있게 하며 이 작업을 위해 **EnablingRule**을 **Auto**로 설정합니다.

[!code-cs[EnableNextButton](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetEnableNextButton)]

앱이 포그라운드에 있는 동안 재생을 제어하기 위한 고유한 UI가 있을 수 있으므로 처리기에 전달된 [**MediaPlaybackCommandManagerCommandBehavior**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior)의 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior.IsEnabled)에 액세스하여 명령이 사용할 수 있거나 사용할 수 없도록 설정될 때 [**IsEnabledChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior.IsEnabledChanged) 이벤트를 사용하여 해당 UI를 SMTC에 맞게 업데이트할 수 있습니다.

[!code-cs[IsEnabledChanged](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetIsEnabledChanged)]

SMTC 명령의 동작을 완전히 재정의하려는 경우도 있습니다. 아래 예제에서는 앱이 *Next* 및 *Previous* 명령을 사용하여 현재 재생 목록의 트랙 간에 건너뛰는 대신 인터넷 라디오 방송국 간에 전환하는 시나리오를 보여 줍니다. 앞의 예제와 마찬가지로, 명령이 수신되면 해당 처리기가 등록됩니다. 이 경우 [**PreviousReceived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.PreviousReceived) 이벤트입니다.

[!code-cs[AddPreviousHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddPreviousHandler)]

**PreviousReceived** 처리기에서 먼저 처리기에 전달된 [**MediaPlaybackCommandManagerPreviousReceivedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerPreviousReceivedEventArgs)의 [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerPreviousReceivedEventArgs.GetDeferral)을 호출하여 [**Deferral**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.Deferral)을 가져옵니다. 그러면 시스템에서 명령을 실행하기 전에 지연이 완료될 때까지 기다립니다. 처리기에서 비동기 호출을 수행하려는 경우 이 작업이 매우 중요합니다. 이때 예제에서는 이전 라디오 방송국을 나타내는 **MediaPlaybackItem**을 반환하는 사용자 지정 메서드를 호출합니다.

[**Handled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerPreviousReceivedEventArgs.Handled) 속성을 검사하여 이벤트가 다른 처리기에서 처리되지 않았는지 확인합니다. 처리되지 않은 경우 **Handled** 속성을 true로 설정합니다. 그러면 SMTC 및 구독한 다른 처리기에서 이 명령이 이미 처리되었으므로 명령을 실행하기 위한 작업을 수행하면 안 된다는 것을 알 수 있습니다. 그런 다음 코드에서 미디어 플레이어에 대한 새 원본을 설정하고 플레이어를 시작합니다.

마지막으로, 지연 개체에서 [**Complete**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.Deferral.Complete)를 호출하여 명령 처리가 완료되었음을 시스템에 알립니다.

[!code-cs[PreviousReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetPreviousReceived)]
                
## <a name="manual-control-of-the-smtc"></a>SMTC 수동 제어
이 문서의 앞부분에서 설명했듯이, SMTC는 앱에서 만드는 모든 **MediaPlayer** 인스턴스에 대한 정보를 자동으로 검색하고 표시합니다. 여러 개의 **MediaPlayer** 인스턴스를 사용하지만 SMTC에서 앱에 대한 단일 항목을 제공하려는 경우 자동 통합을 사용하는 대신 SMTC의 동작을 수동으로 제어해야 합니다. 또한 [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController)를 사용하여 하나 이상의 미디어 플레이어를 제어하는 경우 수동 SMTC 통합을 사용해야 합니다. 또한 앱에서 **MediaPlayer** 이외의 API(예: [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph) 클래스)를 사용하여 미디어를 재생하는 경우 사용자가 SMTC를 사용하여 앱을 제어하려면 수동 SMTC 통합을 구현해야 합니다. SMTC를 수동으로 제어하는 방법에 대한 자세한 내용은 [시스템 미디어 전송 컨트롤의 수동 제어](system-media-transport-controls.md)를 참조하세요.



## <a name="related-topics"></a>관련 항목
* [미디어 재생](media-playback.md)
* [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)
* [시스템 미디어 전송 컨트롤의 수동 컨트롤](system-media-transport-controls.md)
* [github의 시스템 미디어 전송 컨트롤 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)
 

 




