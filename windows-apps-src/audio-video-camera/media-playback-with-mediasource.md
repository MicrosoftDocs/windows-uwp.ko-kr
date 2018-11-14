---
author: drewbatgit
ms.assetid: C5623861-6280-4352-8F22-80EB009D662C
description: 이 문서에서는 MediaSource 사용 방법을 보여 줍니다. 이 클래스는 로컬 또는 원격 파일과 같은 여러 원본에서 미디어를 참조하고 재생하는 일반적인 방법을 제공하며 기본 미디어 형식에 상관없이 미디어 데이터에 액세스하기 위한 공통 모델을 공개합니다.
title: 미디어 항목, 재생 목록 및 트랙
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 73b6a19e2385f1a9b8afa4672df50d17ac16ec97
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "6451195"
---
# <a name="media-items-playlists-and-tracks"></a>미디어 항목, 재생 목록 및 트랙


 이 문서에서는 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) 클래스 사용 방법을 보여 줍니다. 이 클래스는 로컬 또는 원격 파일과 같은 여러 원본에서 미디어를 참조하고 재생하는 일반적인 방법을 제공하며 기본 미디어 형식에 상관없이 미디어 데이터에 액세스하기 위한 공통 모델을 공개합니다. [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) 클래스는 **MediaSource**의 기능을 확장하여 미디어 항목에 포함된 여러 오디오, 비디오 및 메타데이터 트랙에서 관리하고 선택할 수 있습니다. [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955)를 사용하면 하나 이상의 미디어 재생 항목에서 재생 목록을 만들 수 있습니다.


## <a name="create-and-play-a-mediasource"></a>MediaSource 만들기 및 재생

클래스에서 제공하는 팩터리 메서드 중 하나를 호출하여 **MediaSource**의 새 인스턴스를 만듭니다.

-   [**CreateFromAdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930906)
-   [**CreateFromIMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn965527)
-   [**CreateFromMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930907)
-   [**CreateFromMseStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930908)
-   [**CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909)
-   [**CreateFromStream**](https://msdn.microsoft.com/library/windows/apps/dn930910)
-   [**CreateFromStreamReference**](https://msdn.microsoft.com/library/windows/apps/dn930911)
-   [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912)
-   [**CreateFromDownloadOperation**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromdownloadoperation)

**MediaSource**를 만든 후에는 [**Source**](https://msdn.microsoft.com/library/windows/apps/dn987010) 속성을 설정하여 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652535)로 재생할 수 있습니다. Windows10 버전 1607부터는 XAML 페이지의 미디어 플레이어 콘텐츠를 렌더링하기 위해 [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764)를 호출하여 **MediaPlayer**를 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement)에 할당할 수 있습니다. 이것이 기본적인 **MediaElement** 사용 방법입니다. **MediaPlayer** 사용 방법은 [**MediaPlayer를 사용하여 오디오 및 비디오 재생**](play-audio-and-video-with-mediaplayer.md)을 참조하세요.

다음 예제에서는 **MediaSource**를 사용하여 사용자가 선택한 **MediaPlayer**의 미디어 파일을 재생하는 방법을 보여 줍니다.

이 시나리오를 완료하기 위해서는 [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) 및 [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) 네임스페이스를 포함해야 합니다.

[!code-cs[Using](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetUsing)]

형식 **MediaSource**의 변수를 선언합니다. 이 문서의 예제에서는 미디어 원본이 클래스 멤버로 선언되므로 여러 위치에서 액세스할 수 있습니다.

[!code-cs[DeclareMediaSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaSource)]

변수를 선언하여 **MediaPlayer** 개체를 저장하고 XAML에서 미디어 콘텐츠를 렌더링하려는 경우 **MediaPlayerElement** 컨트롤을 페이지에 추가합니다.

[!code-cs[DeclareMediaPlayer](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-xml[MediaPlayerElement](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetMediaPlayerElement)]

사용자가 재생할 미디어 파일을 선택할 수 있도록 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)를 사용합니다. 선택기의 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) 메서드에서 반환된 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 개체를 통해 [**MediaSource.CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909)을 호출하여 새 MediaObject를 초기화합니다. 마지막으로 [**SetPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/dn899085) 메서드를 호출하여 미디어 원본을 **MediaElement**에 대한 재생 원본으로 설정합니다.

[!code-cs[PlayMediaSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaSource)]

기본적으로 미디어 원본을 설정해도 **MediaPlayer**가 자동으로 재생되지 않습니다. [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play)를 호출하여 수동으로 재생을 시작할 수 있습니다.

[!code-cs[Play](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlay)]

미디어 원본이 설정되자마자 플레이어가 재생을 시작할 수 있도록 **MediaPlayer**의 [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) 속성을 true로 설정할 수도 있습니다.

[!code-cs[AutoPlay](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAutoPlay)]

### <a name="create-a-mediasource-from-a-downloadoperation"></a>Create a MediaSource from a DownloadOperation
Windows, 버전 1803부터 **DownloadOperation**의 **MediaSource** 개체를 만들 수 있습니다.

[!code-cs[CreateMediaSourceFromDownload](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetCreateMediaSourceFromDownload)]

**MediaSource**을 시작하지 않거나 그 **IsRandomAccessRequired** 속성을 true로 설정하지 않고 다운로드하여 만들 수 있지만, **MediaPlayer** 또는 **MediaPlayerElement**에 **MediaSource**을 첨부하여 재생하기 전에 이 두 가지 작업을 모두 수행해야 합니다.

[!code-cs[StartDownload](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetStartDownload)]


## <a name="handle-multiple-audio-video-and-metadata-tracks-with-mediaplaybackitem"></a>MediaPlaybackItem을 사용하여 여러 오디오, 비디오 및 메타데이터 트랙 처리

여러 종류의 원본에서 미디어를 재생하는 것은 일반적인 방법이므로 재생하기 위해 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930905)를 편하게 사용할 수 있지만 고급 동작에 액세스하려면 **MediaSource**에서 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939)을 만들어야 할 수 있습니다. 미디어 항목에 대한 여러 오디오, 비디오 및 데이터 트랙을 액세스하고 관리하는 기능이 포함됩니다.

**MediaPlaybackItem**을 저장하기 위한 변수를 선언합니다.

[!code-cs[DeclareMediaPlaybackItem](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackItem)]

초기화된 **MediaSource** 개체에서 생성자를 호출하고 전달하여 **MediaPlaybackItem**을 만듭니다.

앱이 미디어 재생 항목의 여러 오디오, 비디오 또는 데이터 트랙을 지원하는 경우 [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948), [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) 또는 [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952) 이벤트에 대한 이벤트 처리기를 등록합니다.

마지막으로 **MediaElement** 또는 **MediaPlayer**의 재생 원본을 **MediaPlaybackItem**으로 설정합니다.

[!code-cs[PlayMediaPlaybackItem](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackItem)]

> [!NOTE] 
> **MediaSource**는 단일 **MediaPlaybackItem**에만 연결될 수 있습니다. 원본에서 **MediaPlaybackItem**을 만든 후에 동일한 원본에서 다른 재생 항목 만들려고 하면 오류가 발생합니다. 또한 미디어 원본에서 **MediaPlaybackItem**을 만든 후에는 **MediaSource** 개체를 **MediaPlayer**에 대한 원본으로 직접 설정할 수 없지만 **MediaPlaybackItem**을 대신 사용해야 합니다.

[**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) 이벤트는 여러 비디오 트랙을 포함하는 **MediaPlaybackItem**이 임의 재생 원본에 할당된 후 발생하며 비디오 트랙 목록이 항목 변경으로 인해 변경되면 다시 발생할 수 있습니다. 이 이벤트에 대한 처리기를 사용하면 사용자가 사용 가능한 트랙 간에 전환할 수 있도록 UI를 업데이트할 수 있습니다. 이 예제에서는 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348)를 사용하여 사용 가능한 비디오 트랙을 표시합니다.

[!code-xml[VideoComboBox](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetVideoComboBox)]

**VideoTracksChanged** 처리기에서 재생 항목의 **[VideoTracks](https://msdn.microsoft.com/library/windows/apps/dn930953)** 목록에 있는 모든 트랙을 루핑합니다. 각 트랙에 대해 새 [**ComboBoxItem**](https://msdn.microsoft.com/library/windows/apps/br209349)이 만들어집니다. 트랙에 레이블이 아직 없는 경우 트랙 인덱스에서 레이블을 생성합니다. 콤보 상자 항목의 [**Tag**](https://msdn.microsoft.com/library/windows/apps/br208745) 속성은 트랙 인덱스로 설정되므로 나중에 식별할 수 있습니다. 마지막으로 콤보 상자에 항목이 추가됩니다. 모든 UI 변경 사항이 UI 스레드에서 만들어져야 하며 이 이벤트가 다른 스레드에서 발생하므로 이러한 작업은 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 호출 내에서 수행됩니다.

[!code-cs[VideoTracksChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetVideoTracksChanged)]

콤보 상자에 대한 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) 처리기에서 트랙 인덱스가 선택한 항목의 **Tag** 속성에서 검색됩니다. 미디어 재생 항목 [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) 목록의 [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn956634) 속성을 설정하면 **MediaElement** 또는 **MediaPlayer**가 활성 비디오 트랙을 지정한 인덱스로 전환합니다.

[!code-cs[VideoTracksSelectionChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetVideoTracksSelectionChanged)]

여러 오디오 트랙을 사용한 미디어 항목 관리는 비디오 트랙과 정확히 동일하게 작동합니다. [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948)를 처리하여 재생 항목의 [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/dn930947) 목록에 있는 오디오 트랙으로 UI를 업데이트합니다. 사용자가 오디오 트랙을 선택하는 경우 **AudioTracks** 목록의 [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn930937) 속성을 설정하여 **MediaElement** 또는 **MediaPlayer**가 활성 오디오 트랙을 지정한 인덱스로 전환하도록 합니다.

[!code-xml[AudioComboBox](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetAudioComboBox)]

[!code-cs[AudioTracksChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksChanged)]

[!code-cs[AudioTracksSelectionChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksSelectionChanged)]

오디오 및 비디오 이외에 **MediaPlaybackItem** 개체에는 0개 이상의 [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580) 개체가 포함될 수 있습니다. 시간이 제한된 메타데이터 트랙에는 자막 또는 자막 텍스트가 포함되거나 앱의 소유에 속하는 사용자 지정 데이터가 포함될 수 있습니다. 시간이 제한된 메타데이터 트랙에는 [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) 또는 [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655)와 같은 [**IMediaCue**](https://msdn.microsoft.com/library/windows/apps/dn930899)에서 상속되는 개체가 나타내는 신호 목록이 포함됩니다. 각 신호의 시작 시간 및 기간은 신호가 언제 활성화되고 얼마나 오래 활성화되는지를 결정합니다.

오디오 트랙 및 비디오 트랙과 유사하게 미디어 항목에 대한 시간이 제한된 메타데이터 트랙은 **MediaPlaybackItem**의 [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952) 이벤트를 처리하여 검색될 수 있습니다. 그러나 시간이 제한된 메타데이터 트랙을 사용하여 사용자가 한 번에 둘 이상의 메타데이터 트랙을 사용하려고 할 수 있습니다. 또한 앱 시나리오에 따라 사용자의 개입 없이 메타데이터 트랙을 자동으로 활성화하거나 비활성화하려고 할 수 있습니다. 설명하자면 이 예제에서는 미디어 항목에서 각 메타데이터 트랙에 [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/br209795)을 추가하여 사용자는 이 추적을 활성화 및 비활성화할 수 있습니다. 각 단추의 **Tag** 속성은 관련 메타데이터 트랙의 인덱스로 설정되어 단추를 켜거나 끌 때 식별할 수 있습니다.

[!code-xml[MetaStackPanel](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetMetaStackPanel)]

[!code-cs[TimedMetadataTrackschanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedMetadataTrackschanged)]

둘 이상의 메타데이터 트랙이 한 번에 활성화될 수 있으므로 메타데이터 트랙 목록에 대한 활성 인덱스를 간단히 설정하지 않습니다. 대신 **MediaPlaybackItem** 개체의 [**SetPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn986977) 메서드를 호출하여 전환하려는 트랙의 인덱스를 전달한 다음 [**TimedMetadataTrackPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn987016) 열거에서 값을 제공합니다. 선택하는 프레젠테이션 모드는 앱의 구현에 따라 다릅니다. 이 예제에서는 메타데이터 트랙이 활성화될 때 **PlatformPresented**로 설정됩니다. 텍스트 기반 트랙의 경우 시스템이 자동으로 트랙에 텍스트 신호를 표시합니다. 토글 단추가 꺼지면 프레젠테이션 모드가 **Disabled**로 설정됩니다. 즉, 텍스트가 표시되지 않고 신호 이벤트가 발생하지 않습니다. 이 문서의 뒷부분에서 신호 이벤트를 설명합니다.

[!code-cs[ToggleChecked](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetToggleChecked)]

[!code-cs[ToggleUnchecked](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetToggleUnchecked)]

메타데이터 트랙을 처리할 때 [**Cues**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.TimedMetadataTrack.Cues) 또는 [**ActiveCues**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.TimedMetadataTrack.ActiveCues) 속성에 액세스하여 트랙 내의 신호 집합에 액세스할 수 있습니다. 이렇게 하면 미디어 항목의 신호 위치가 표시되도록 UI를 업데이트할 수 있습니다.

## <a name="handle-unsupported-codecs-and-unknown-errors-when-opening-media-items"></a>미디어 항목을 열 때 지원되지 않는 코덱 및 알 수 없는 오류 처리
Windows10 버전 1607부터 앱이 실행 중인 디바이스에서 미디어 항목 재생에 필요한 코덱이 전체 또는 부분적으로 지원되는지 확인할 수 있습니다. [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.AudioTracksChanged)와 같은 **MediaPlaybackItem** 트랙 변경 이벤트에 대한 이벤트 처리기에서 먼저 트랙 변경이 새 트랙에 삽입되었는지 확인해야 합니다. 그럴 경우 [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.AudioTracks) 컬렉션과 같은 **MediaPlaybackItem** 매개 변수의 적절한 트랙 컬렉션과 함께 **IVectorChangedEventArgs.Index** 매개 변수에서 전달된 인덱스를 사용하여 삽입할 트랙에 대한 참조를 가져올 수 있습니다.

삽입된 트랙에 대한 참조가 있으면 트랙 [**SupportInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.SupportInfo) 속성의 [**DecoderStatus**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrackSupportInfo.DecoderStatus)를 확인합니다. 값이 [**FullySupported**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus)인 경우 트랙 재생에 필요한 적합한 코덱이 디바이스에 있는 것입니다. 값이 [**Degraded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus)인 경우 시스템에서 트랙을 재생할 수 있지만 재생 성능이 약간 저하됩니다. 예를 들어 5.1 오디오 트랙이 2채널 스테레오로 재생될 수 있습니다. 이 경우 사용자에게 성능 저하에 대해 알리도록 UI를 업데이트할 수 있습니다. 값이 [**UnsupportedSubtype**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus) 또는 [**UnsupportedEncoderProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus)인 경우 디바이스의 현재 코덱으로 트랙을 재생할 수 없습니다. 사용자에게 알리고 항목 재생을 건너뛰거나 사용자가 올바른 코덱을 다운로드할 수 있도록 UI를 구현할 수 있습니다. 트랙의 [**GetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.GetEncodingProperties) 메서드를 사용하여 재생에 필요한 코덱을 결정할 수 있습니다.

마지막으로, 트랙이 디바이스에서 지원되지만 파이프라인의 알 수 없는 오류로 인해 열 수 없는 경우 발생되는 트랙의 [**OpenFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.OpenFailed) 이벤트를 등록할 수 있습니다.

[!code-cs[AudioTracksChanged_CodecCheck](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksChanged_CodecCheck)]

[**OpenFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.OpenFailed) 이벤트 처리기에서 **MediaSource** 상태가 알 수 없음인지 확인하고, 알 수 없음인 경우 프로그래밍 방식으로 다른 재생 트랙을 선택하거나, 사용자에게 다른 트랙을 선택하도록 하거나, 재생을 취소할 수 있습니다.

[!code-cs[OpenFailed](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetOpenFailed)]

## <a name="set-display-properties-used-by-the-system-media-transport-controls"></a>시스템 미디어 전송 컨트롤에서 사용되는 디스플레이 속성 설정
Windows10 버전 1607부터 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer)에서 재생되는 미디어는 기본적으로 SMTC(시스템 미디어 전송 컨트롤)와 자동으로 통합됩니다. **MediaPlaybackItem**의 재생 속성을 업데이트하여 SMTC에 의해 재생되는 메타데이터를 지정할 수 있습니다. [**GetDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.GetDisplayProperties)를 호출하여 항목에 대한 디스플레이 속성을 나타내는 개체를 가져옵니다. [**Type**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.Type) 속성을 설정하여 재생 항목이 음악인지 또는 비디오인지 설정합니다. 그런 다음 개체의 [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.VideoProperties) 또는 [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.MusicProperties) 속성을 설정합니다. [**ApplyDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/mt489923)를 호출하여 항목의 속성을 제공된 값으로 업데이트합니다. 일반적으로 앱은 웹 서비스에서 디스플레이 값을 동적으로 검색하지만 다음 예제에서는 이 프로세스를 하드코드된 값을 사 표시합니다.

[!code-cs[SetVideoProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetVideoProperties)]

[!code-cs[SetMusicProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetMusicProperties)]

## <a name="add-external-timed-text-with-timedtextsource"></a>TimedTextSource를 사용하여 시간이 제한된 외부 텍스트 추가

일부 시나리오의 경우에는 여러 로캘의 자막이 있는 별도 파일과 같이 미디어 항목과 관련된 시간이 지정된 텍스트가 있는 외부 파일이 있을 수 있습니다. [**TimedTextSource**](https://msdn.microsoft.com/library/windows/apps/dn956679) 클래스를 사용하여 스트림이나 URI에서 시간이 지정된 외부 텍스트 파일을 로드합니다.

이 예제에서는 트랙을 확인한 후 식별하기 위해 원본 URI 및 **TimedTextSource** 개체를 키/값 쌍으로 사용하는 미디어 항목에 대해 시간이 지정된 텍스트 원본 목록을 저장하도록 **Dictionary** 컬렉션을 사용합니다.

[!code-cs[TimedTextSourceMap](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSourceMap)]

[**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn708190)를 호출하여 시간이 제한된 각 외부 텍스트 파일에 대해 새 **TimedTextSource**를 만듭니다. 시간이 지정된 텍스트 원본에 대한 **Dictionary**에 항목을 추가합니다. 항목을 로드하지 못하거나 해당 항목이 성공적으로 로드된 후에 추가 속성을 설정하는 데 실패한 경우 처리할 [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540) 이벤트에 대한 처리기를 추가합니다.

모든 **TimedTextSource** 개체를 [**ExternalTimedTextSources**](https://msdn.microsoft.com/library/windows/apps/dn930916) 컬렉션에 추가하여 **MediaSource**에 등록합니다. 시간이 지정된 외부 텍스트 원본을 해당 원본에서 만든 **MediaPlaybackItem**이 아니라 **MediaSource**에 직접 추가합니다. UI를 업데이트하여 외부 텍스트 트랙을 반영하려면 이 문서의 앞부분에서 설명한 대로 **TimedMetadataTracksChanged** 이벤트를 등록하고 처리합니다.

[!code-cs[TimedTextSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSource)]

[**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540) 이벤트에 대한 처리기에서 처리기에 전달된 [**TimedTextSourceResolveResultEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn965537)의 **Error** 속성을 확인하여 시간이 지정된 텍스트 데이터 로드를 시도하는 동안 오류가 발생했는지 확인합니다. 항목이 성공적으로 해결된 경우 이 처리기를 사용하여 해결된 트랙의 추가 속성을 업데이트할 수 있습니다. 이 예제는 이전에 **Dictionary**에 저장된 URI를 기반으로 각 트랙에 레이블을 추가합니다.

[!code-cs[TimedTextSourceResolved](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSourceResolved)]

## <a name="add-additional-metadata-tracks"></a>추가 메타데이터 트랙 추가

동적으로 코드에서 사용자 지정 메타데이터 트랙을 만들고 미디어 원본에 연결할 수 있습니다. 만들어진 트랙에는 자막 또는 자막 텍스트가 포함되거나 독점 앱 데이터가 포함될 수 있습니다.

생성자를 호출하고 ID, 언어 식별자 및 [**TimedMetadataKind**](https://msdn.microsoft.com/library/windows/apps/dn956578) 열거의 값을 지정하여 새 [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580)을 만듭니다. [**CueEntered**](https://msdn.microsoft.com/library/windows/apps/dn956583) 및 [**CueExited**](https://msdn.microsoft.com/library/windows/apps/dn956584) 이벤트에 대한 처리기를 등록합니다. 이러한 이벤트는 신호 시작 시간에 도달할 때 및 신호 기간이 만료될 때 각각 발생합니다.

생성한 메타데이터 트랙 유형에 적합한 새 신호 개체를 만들고 트랙의 ID, 시작 시간, 지속 시간을 설정합니다. 이 예제에서는 데이터 트랙을 만들어서 [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) 개체 세트를 생성하고 각 신호별로 앱별 데이터를 포함하는 버퍼를 제공합니다. 새 트랙을 등록하려면 **MediaSource** 개체의 [**ExternalTimedMetadataTracks**](https://msdn.microsoft.com/library/windows/apps/dn930915) 컬렉션에 추가합니다.

Windows 10 버전 1703부터 **DataCue.Properties** 속성은 **CueEntered** 및 **CueExited** 이벤트에서 검색할 수 있는 키/데이터 쌍으로 사용자 지정 속성을 저장하는 데 사용할 수 있는 [**PropertySet**](https://docs.microsoft.com/uwp/api/windows.foundation.collections.propertyset)를 노출합니다.  

[!code-cs[AddDataTrack](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAddDataTrack)]

**CueEntered** 이벤트는 관련된 트랙에 **ApplicationPresented**, **Hidden** 또는 **PlatformPresented**의 프레젠테이션 모드가 있는 한 신호 시작 시간에 도달할 때 발생합니다. 트랙에 대한 프레젠테이션 모드가 **Disabled**인 동안에는 신호 이벤트는 메타데이터 트랙에 대해 발생하지 않습니다. 이 예제에서는 디버그 창에 신호와 관련된 사용자 지정 데이터를 단순히 출력합니다.

[!code-cs[DataCueEntered](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDataCueEntered)]

이 예제에서는 트랙을 만들고 [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655) 개체를 사용하여 트랙에 신호를 추가할 때 **TimedMetadataKind.Caption**을 지정하여 사용자 지정 텍스트를 추가합니다.

[!code-cs[AddTextTrack](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAddTextTrack)]

[!code-cs[TextCueEntered](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTextCueEntered)]

## <a name="play-a-list-of-media-items-with-mediaplaybacklist"></a>MediaPlaybackList를 사용하여 미디어 항목 목록 재생

[**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955)를 사용하면 **MediaPlaybackItem** 개체가 표시하는 미디어 항목의 재생 목록을 만들 수 있습니다.

**참고** [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) 의 항목이 매끄러운 재생을 사용 하 여 렌더링 됩니다. 시스템은 MP3 또는 AAC로 인코딩된 파일에 제공된 메타데이터를 사용하여 매끄러운 재생에 필요한 지연 또는 패딩 보정을 결정합니다. MP3 또는 AAC로 인코딩된 파일이 이 메타데이터를 제공하지 않는 경우 시스템에서 지연이나 패딩을 스스로 결정합니다. 이러한 인코더는 지연 또는 패딩을 도입하지 않으므로 PCM, FLAC, ALAC 등 무손실 형식에 대해 시스템은 어떠한 조치도 취하지 않습니다.

시작하려면 변수를 선언하여 **MediaPlaybackList**를 저장합니다.

[!code-cs[DeclareMediaPlaybackList](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackList)]

이 문서의 앞부분에서 설명한 동일한 절차를 사용하여 목록에 추가하려는 각 미디어 항목에 대한 **MediaPlaybackItem**을 만듭니다. **MediaPlaybackList** 개체를 초기화하고 이 개체에 미디어 재생 항목을 추가합니다. [**CurrentItemChanged**](https://msdn.microsoft.com/library/windows/apps/dn930957) 이벤트에 대한 처리기를 등록합니다. 이 이벤트를 사용하면 UI를 업데이트하여 현재 재생 중인 미디어 항목을 반영할 수 있습니다. 또한 목록에서 항목을 성공적으로 열 때 발생하는 [ItemOpened](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemOpened) 이벤트 및 목록에서 항목을 열 수 없는 경우 발생하는 [ItemFailed](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemFailed) 이벤트를 등록할 수 있습니다.

Windows 10 버전 1703부터 [MaxPlayedItemsToKeepOpen](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.MaxPlayedItemsToKeepOpen) 속성을 설정하여 재생한 후 시스템에서 계속 열려 있는 **MediaPlaybackList**의 최대 **MediaPlaybackItem** 개체 수를 지정할 수 있습니다. **MediaPlaybackItem**이 계속해서 열려 있는 경우 항목을 다시 로드하지 않아도 되기 때문에 사용자가 해당 항목으로 전환할 때 즉시 항목이 재생됩니다. 하지만 계속해서 항목을 열어두면 앱의 메모리 소비가 증가하기 때문에 이 값을 설정할 때 응답성 및 메모리 사용량 간의 균형을 고려해야 합니다. 

목록의 재생을 활성화하려면 **MediaPlayer**의 재생 소스를 사용자의 **MediaPlaybackList**로 설정합니다.

[!code-cs[PlayMediaPlaybackList](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackList)]

**CurrentItemChanged** 이벤트 처리기에서 UI를 업데이트하여 이벤트로 전달된 [**CurrentMediaPlaybackItemChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn930929) 개체의 [**NewItem**](https://msdn.microsoft.com/library/windows/apps/dn930930) 속성을 사용하여 검색될 수 있는 현재 재생 중인 항목을 반영합니다. 이 이벤트에서 UI를 업데이트하는 경우 UI 스레드에서 업데이트되도록 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317)에 대한 호출 안에서 수행해야 합니다.

Windows 10 버전 1703부터 [CurrentMediaPlaybackItemChangedEventArgs.Reason](https://docs.microsoft.com/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.Reason) 속성을 확인하여 프로그래밍 방식의 앱 전환 항목, 이전에 재생된 항목의 종료, 또는 오류 발생 등 항목이 변경된 이유를 나타내는 값을 가져올 수 있습니다.

[!code-cs[MediaPlaybackListItemChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetMediaPlaybackListItemChanged)]


[**MovePrevious**](https://msdn.microsoft.com/library/windows/apps/mt146455) 또는 [**MoveNext**](https://msdn.microsoft.com/library/windows/apps/mt146454)를 호출하여 미디어 플레이어가 **MediaPlaybackList**에서 이전 또는 이후 항목을 재생하도록 합니다.

[!code-cs[PrevButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPrevButton)]

[!code-cs[NextButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetNextButton)]

[**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146457) 속성을 설정하여 미디어 플레이어가 임의 순서로 목록의 항목을 재생해야 하는지 여부를 지정합니다.

[!code-cs[ShuffleButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetShuffleButton)]

[**AutoRepeatEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146452) 속성을 설정하여 미디어 플레이어가 목록을 반복 재생해야 하는지 여부를 지정합니다.

[!code-cs[RepeatButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetRepeatButton)]


### <a name="handle-the-failure-of-media-items-in-a-playback-list"></a>재생 목록에 있는 미디어 항목의 오류 처리
목록의 항목을 열 수 없는 경우 [**ItemFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList.ItemFailed) 이벤트가 발생합니다. 가능한 경우 처리기에 전달된 [**MediaPlaybackItemError**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItemError) 개체의 [**ErrorCode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItemError.ErrorCode) 속성에 네트워크 오류, 디코딩 오류 또는 암호화 오류 등 실패의 특정 원인이 열거됩니다.

[!code-cs[ItemFailed](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetItemFailed)]

### <a name="disable-playback-of-items-in-a-playback-list"></a>재생 목록에 있는 항목의 재생 사용 안 함
Windows 10 버전 1703부터 [MediaPlaybackItem](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem)의 [IsDisabledInPlaybackList](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem.IsDisabledInPlaybackList) 속성을 false로 설정하여 **MediaPlaybackItemList**에 있는 하나 이상의 항목의 재생을 사용 안 함으로 설정할 수 있습니다. 

이 기능에 대한 일반적인 시나리오는 인터넷에서 스트리밍되는 음악을 재생하는 앱입니다. 앱은 디바이스의 네트워크 연결 상태 변경 내용을 수신하고 완전히 다운로드되지 않은 항목의 재생을 사용하지 않도록 설정할 수 있습니다. 다음 예제에서 처리기는 [NetworkInformation.NetworkStatusChanged](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation.NetworkStatusChanged) 이벤트에 대해 등록됩니다.

[!code-cs[RegisterNetworkStatusChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetRegisterNetworkStatusChanged)]

**NetworkStatusChanged**에 대한 처리기에서 [GetInternetConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation.GetInternetConnectionProfile)이 네트워크에 연결되어 있지 않음을 나타내는 null을 반환하는지 확인합니다. 이러한 경우, 재생 목록에서 항목을 모두 반복하고 항목에 대한 [TotalDownloadProgress](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TotalDownloadProgress)가 1보다 작은 경우, 즉, 항목이 완전히 다운로드되지 않은 경우 항목을 사용하지 않도록 설정합니다. 네트워크 연결을 사용하는 경우 재생 목록의 항목을 모두 반복하고 각 항목을 사용하도록 설정합니다.

[!code-cs[NetworkStatusChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetNetworkStatusChanged)]

### <a name="defer-binding-of-media-content-for-items-in-a-playback-list-by-using-mediabinder"></a>MediaBinder를 사용하여 재생 목록의 항목에 대한 미디어 콘텐츠 바인딩 연기
위 예제에서 **MediaSource**는 파일, URL 또는 스트림에서 생성되며 이후 **MediaPlaybackItem**을 만들고 **MediaPlaybackList**에 추가합니다. 사용자에게 콘텐츠 열람에 대해 비용이 청구되는 등 일부 시나리오의 경우 재생 목록의 항목이 실제로 재생할 준비가 될 때까지 **MediaSource**의 콘텐츠 검색을 연기할 수 있습니다. 이 시나리오를 구현하려면 [**MediaBinder**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder) 클래스의 인스턴스를 만듭니다. [**토큰**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Token) 속성을 검색을 연기하고 [**바인딩**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Binding) 이벤트에 대한 처리기를 등록하고자 하는 콘텐츠를 식별하는 앱 정의 문자열로 설정합니다. 그 다음 [**MediaSource.CreateFromMediaBinder**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediabinder)를 호출하여 **바인더**에서 **MediaSource**를 만듭니다. 그런 다음 평소와 같이 **MediaSource**에서 **MediaPlaybackItem**을 만들고 재생 목록에 추가합니다.

[!code-cs[InitMediaBinder](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetInitMediaBinder)]

시스템에서 **MediaBinder**와 관련된 콘텐츠를 검색하도록 결정한 경우 **바인딩** 이벤트가 발생합니다. 이 이벤트에 대한 처리기에서 이벤트에 전달된 [**MediaBindingEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs)로부터 **MediaBinder** 인스턴스를 검색할 수 있습니다. **토큰** 속성에 대해 지정한 문자열을 검색하고 이를 사용하여 검색해야 하는 콘텐츠 확인할 수 있습니다. **MediaBindingEventArgs**는 [**SetStorageFile**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstoragefile), [**SetStream**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstream), [**SetStreamReference**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstreamreference) 및 [**SetUri**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.seturi) 등 여러 가지 표현에 바인딩된 콘텐츠를 설정하는 메서드를 제공합니다. 

[!code-cs[BinderBinding](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetBinderBinding)]

웹 요청 등 비동기 작업을 수행하는 경우 **바인딩** 이벤트 처리기에서 [**MediaBindingEventArgs.GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) 메서드를 호출하여 작업을 진행하기 전에 시스템이 사용자 작업이 완료될 때까지 대기하도록 지시할 수 있습니다. 작업이 완료된 후 시스템에 계속하도록 지시하기 위해 [**Deferral.Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.Complete)를 호출합니다.

Windows 10 버전 1703부터 [**SetAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)를 호출하여 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource)를 바인딩된 콘텐츠로 제공할 수 있습니다. 앱에서 적응 스트리밍을 사용하는 것에 대한 자세한 내용은 [적응 스트리밍](adaptive-streaming.md)을 참조하세요.



## <a name="related-topics"></a>관련 항목
* [미디어 재생](media-playback.md)
* [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)
* [시스템 미디어 전송 컨트롤과 통합](integrate-with-systemmediatransportcontrols.md)
* [백그라운드에서 미디어 재생](background-audio.md)

