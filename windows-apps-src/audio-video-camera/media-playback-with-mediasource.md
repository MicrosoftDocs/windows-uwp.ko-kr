---
ms.assetid: C5623861-6280-4352-8F22-80EB009D662C
description: 이 문서에서는 로컬 또는 원격 파일과 같은 여러 원본의 미디어를 참조 하 고 재생 하는 일반적인 방법을 제공 하 고, 기본 미디어 형식에 관계 없이 미디어 데이터에 액세스 하기 위한 일반적인 모델을 제공 하는 MediaSource을 사용 하는 방법을 보여 줍니다.
title: 미디어 항목, 재생 목록 및 트랙
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f1f428bb8beb4bb933387a77e5a74819016a4c64
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363886"
---
# <a name="media-items-playlists-and-tracks"></a>미디어 항목, 재생 목록 및 트랙


 이 문서에서는 [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) 클래스를 사용 하는 방법을 보여 줍니다 .이 클래스는 로컬 또는 원격 파일과 같은 여러 원본의 미디어를 참조 하 고 재생 하는 일반적인 방법을 제공 하 고, 기본 미디어 형식에 관계 없이 미디어 데이터에 액세스 하기 위한 공통 모델을 노출 합니다. [**Mediaplaybackitem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) 클래스는 **MediaSource**의 기능을 확장 하 여 미디어 항목에 포함 된 여러 오디오, 비디오 및 메타 데이터 트랙에서 관리 하 고 선택할 수 있습니다. [**Mediaplaybacklist**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) 를 사용 하면 하나 이상의 미디어 재생 항목에서 재생 목록을 만들 수 있습니다.


## <a name="create-and-play-a-mediasource"></a>MediaSource 만들기 및 재생

클래스에 의해 노출 되는 팩터리 메서드 중 하나를 호출 하 여 **MediaSource** 의 새 인스턴스를 만듭니다.

-   [**CreateFromAdaptiveMediaSource**](/uwp/api/windows.media.core.mediasource.createfromadaptivemediasource)
-   [**CreateFromIMediaSource**](/uwp/api/windows.media.core.mediasource.createfromimediasource)
-   [**CreateFromMediaStreamSource**](/uwp/api/windows.media.core.mediasource.createfrommediastreamsource)
-   [**CreateFromMseStreamSource**](/uwp/api/windows.media.core.mediasource.createfrommsestreamsource)
-   [**CreateFromStorageFile**](/uwp/api/windows.media.core.mediasource.createfromstoragefile)
-   [**CreateFromStream**](/uwp/api/windows.media.core.mediasource.createfromstream)
-   [**CreateFromStreamReference**](/uwp/api/windows.media.core.mediasource.createfromstreamreference)
-   [**CreateFromUri**](/uwp/api/windows.media.core.mediasource.createfromuri)
-   [**CreateFromDownloadOperation**](/uwp/api/windows.media.core.mediasource.createfromdownloadoperation)

**MediaSource** 를 만든 후에는 [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) 속성을 설정 하 여 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 로 재생할 수 있습니다. Windows 10, 버전 1607부터, XAML 페이지에서 미디어 플레이어 콘텐츠를 렌더링 하기 위해 [**Setmediaplayer**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.setmediaplayer) 를 호출 하 여 **MediaPlayer** 를 [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 에 할당할 수 있습니다. 이것이 기본적인 **MediaElement** 사용 방법입니다. **Mediaplayer**를 사용 하는 방법에 대 한 자세한 내용은 [**mediaplayer를 사용 하 여 오디오 및 비디오 재생**](play-audio-and-video-with-mediaplayer.md)을 참조 하세요.

다음 예제에서는 **MediaSource**를 사용 하 여 **MediaPlayer** 에서 사용자가 선택한 미디어 파일을 재생 하는 방법을 보여 줍니다.

이 시나리오를 완료 하려면 [**Windows media**](/uwp/api/Windows.Media.Core) To the windows [**. Media to**](/uwp/api/Windows.Media.Playback) the windows를 포함 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetUsing":::

**MediaSource**형식의 변수를 선언 합니다. 이 문서의 예제에서는 여러 위치에서 액세스할 수 있도록 미디어 소스가 클래스 멤버로 선언 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaSource":::

**MediaPlayer** 개체를 저장 하는 변수를 선언 하 고, XAML에서 미디어 콘텐츠를 렌더링 하려면 **MediaPlayerElement** 컨트롤을 페이지에 추가 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaPlayer":::

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetMediaPlayerElement":::

사용자가 재생할 미디어 파일을 선택할 수 있게 하려면 [**Fileopenpicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)를 사용 합니다. Picker의 [**PickSingleFileAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) 메서드에서 반환 된 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 개체를 사용 하 여 [**MediaSource CreateFromStorageFile**](/uwp/api/windows.media.core.mediasource.createfromstoragefile)를 호출 하 여 새 mediaobject를 초기화 합니다. 마지막으로, [**Setplaybacksource**](/uwp/api/windows.ui.xaml.controls.mediaelement.setplaybacksource) 메서드를 호출 하 여 미디어 원본을 **MediaElement** 의 재생 소스로 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlayMediaSource":::

기본적으로 **MediaPlayer** 는 미디어 원본이 설정 될 때 자동으로 재생 되기 시작 하지 않습니다. [**Play**](/uwp/api/windows.media.playback.mediaplayer.play)를 호출 하 여 수동으로 재생을 시작할 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlay":::

또한 **MediaPlayer** 의 [**자동 실행**](/uwp/api/windows.media.playback.mediaplayer.autoplay) 속성을 true로 설정 하 여 미디어 원본이 설정 되는 즉시 플레이어에 게 재생을 시작할 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAutoPlay":::

### <a name="create-a-mediasource-from-a-downloadoperation"></a>DownloadOperation에서 MediaSource 만들기
Windows, 버전 1803부터 **Downloadoperation**에서 **MediaSource** 개체를 만들 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetCreateMediaSourceFromDownload":::

다운로드를 시작 하지 않고 다운로드에서 **MediaSource** 를 만들거나 **IsRandomAccessRequired** 속성을 true로 설정 하는 동안에는 **MediaSource** 를 **MediaPlayer** 또는 **MediaPlayerElement** 에 연결 하기 전에 두 가지 작업을 모두 수행 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetStartDownload":::


## <a name="handle-multiple-audio-video-and-metadata-tracks-with-mediaplaybackitem"></a>MediaPlaybackItem을 사용 하 여 여러 오디오, 비디오 및 메타 데이터 트랙 처리

재생에 [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) 를 사용 하는 것은 다양 한 종류의 원본에서 미디어를 재생 하는 일반적인 방법을 제공 하기 때문에 편리 하지만 **MediaSource**에서 [**mediaplaybackitem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) 을 만들어 고급 동작에 액세스할 수 있습니다. 여기에는 미디어 항목에 대 한 여러 오디오, 비디오 및 데이터 트랙을 액세스 하 고 관리 하는 기능이 포함 됩니다.

**Mediaplaybackitem**을 저장할 변수를 선언 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaPlaybackItem":::

생성자를 호출 하 고 초기화 된 **MediaSource** 개체를 전달 하 여 **Mediaplaybackitem** 을 만듭니다.

앱이 미디어 재생 항목에서 여러 오디오, 비디오 또는 데이터 트랙을 지 원하는 경우에는 [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.timedmetadatatrackschanged) 이벤트에 대 한 이벤트 [**처리기를**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged)등록 [**VideoTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.videotrackschanged)합니다.

마지막으로 **MediaElement** 또는 **MediaPlayer** 의 재생 원본을 **mediaplaybackitem**으로 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlayMediaPlaybackItem":::

> [!NOTE] 
> **MediaSource** 는 단일 **Mediaplaybackitem**에만 연결할 수 있습니다. 원본에서 **Mediaplaybackitem** 을 만든 후 동일한 원본에서 다른 재생 항목을 만들려고 하면 오류가 발생 합니다. 또한 미디어 원본에서 **Mediaplaybackitem** 을 만든 후에는 **MediaSource** 개체를 **MediaPlayer** 의 원본으로 직접 설정할 수 없지만 대신 **mediaplaybackitem**을 사용 해야 합니다.

[**Videochanges Schanged**](/uwp/api/windows.media.playback.mediaplaybackitem.videotrackschanged) 이벤트는 여러 비디오 트랙이 포함 된 **mediaplaybackitem** 이 재생 소스로 할당 된 후 발생 하며, 비디오 목록에서 항목 변경 내용에 대 한 변경 내용을 추적 하는 경우 다시 발생할 수 있습니다. 이 이벤트에 대 한 처리기를 통해 사용자가 사용 가능한 트랙 간을 전환할 수 있도록 UI를 업데이트할 수 있습니다. 이 예제에서는 [**콤보 상자**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 를 사용 하 여 사용 가능한 비디오 트랙을 표시 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetVideoComboBox":::

비디오 기능 **변경** 처리기에서 재생 항목의 [**video트랙**](/uwp/api/windows.media.playback.mediaplaybackitem.videotracks) 목록에 있는 모든 트랙을 반복 합니다. 각 트랙에 대해 새 [**ComboBoxItem**](/uwp/api/Windows.UI.Xaml.Controls.ComboBoxItem) 이 만들어집니다. 트랙에 아직 레이블이 없으면 트랙 인덱스에서 레이블이 생성 됩니다. 콤보 상자 항목의 [**Tag**](/uwp/api/windows.ui.xaml.frameworkelement.tag) 속성은 나중에 확인할 수 있도록 트랙 인덱스에 설정 됩니다. 마지막으로 항목이 콤보 상자에 추가 됩니다. 이러한 작업은 ui 스레드에서 모든 UI를 변경 해야 하며이 이벤트가 다른 스레드에서 발생 하기 때문에 CoreDispatcher 호출 내에서 수행 됩니다 [**.**](/uwp/api/windows.ui.core.coredispatcher.runasync)

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetVideoTracksChanged":::

콤보 상자의 [**Selectionchanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 처리기에서는 선택한 항목의 **Tag** 속성에서 트랙 인덱스를 검색 합니다. 미디어 재생 항목의 [**비디오 트랙**](/uwp/api/windows.media.playback.mediaplaybackitem.videotracks) 목록에서 [**SelectedIndex**](/uwp/api/windows.media.playback.mediaplaybackvideotracklist.selectedindex) 속성을 설정 하면 **MediaElement** 또는 **MediaPlayer** 가 활성 비디오 트랙을 지정 된 인덱스로 전환 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetVideoTracksSelectionChanged":::

여러 오디오 트랙을 사용 하 여 미디어 항목을 관리 하는 것은 비디오 트랙과 똑같이 동일 하 게 작동 합니다. 재생 항목의 오디오 트랙 목록에 있는 오디오 트랙으로 UI를 업데이트 하도록 [**변경**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged) 된 [**오디오 트랙을**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotracks) 처리 합니다. 사용자가 오디오 트랙을 선택 하는 경우에는 **MediaElement** 또는 **MediaPlayer** 가 활성 오디오 트랙을 지정 된 인덱스로 전환 하도록 오디오 **트랙** 목록의 [**SelectedIndex**](/uwp/api/windows.media.playback.mediaplaybackaudiotracklist.selectedindex) 속성을 설정 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetAudioComboBox":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAudioTracksChanged":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAudioTracksSelectionChanged":::

오디오 및 비디오 외에도 **Mediaplaybackitem** 개체는 0 개 이상의 [**TimedMetadataTrack**](/uwp/api/Windows.Media.Core.TimedMetadataTrack) 개체를 포함할 수 있습니다. 시간이 지정 된 메타 데이터 트랙에는 부제 또는 캡션 텍스트가 포함 될 수 있으며, 앱을 소유 하는 사용자 지정 데이터가 포함 될 수도 있습니다. 시간이 지정 된 메타 데이터 트랙에는 [**datacue**](/uwp/api/Windows.Media.Core.DataCue) 또는 [**Timedtextcue**](/uwp/api/Windows.Media.Core.TimedTextCue)와 같이 [**imediacue**](/uwp/api/Windows.Media.Core.IMediaCue)에서 상속 하는 개체로 표시 되는 큐 목록이 포함 됩니다. 각 큐에는 시작 시간 및 큐가 활성화 되는 시간 및 기간을 결정 하는 기간이 있습니다.

오디오 트랙과 비디오 트랙과 마찬가지로 미디어 항목에 대 한 시간 제한 된 메타 데이터 트랙은 **Mediaplaybackitem**의 [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.timedmetadatatrackschanged) 이벤트를 처리 하 여 검색할 수 있습니다. 그러나 시간이 지정 된 메타 데이터 트랙에서는 사용자가 한 번에 둘 이상의 메타 데이터 트랙을 사용 하도록 설정할 수 있습니다. 또한 응용 프로그램 시나리오에 따라 사용자 개입 없이 메타 데이터 트랙을 자동으로 사용 하거나 사용 하지 않도록 설정할 수 있습니다. 설명을 위해이 예제에서는 사용자가 추적을 사용 하거나 사용 하지 않도록 설정할 수 있도록 미디어 항목의 각 메타 데이터 트랙에 대해 [**ToggleButton**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton) 을 추가 합니다. 각 단추의 **Tag** 속성은 연결 된 메타 데이터 트랙의 인덱스로 설정 되므로 단추가 설정/해제 될 때이를 식별할 수 있습니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetMetaStackPanel":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedMetadataTrackschanged":::

메타 데이터 트랙은 한 번에 하나씩만 활성화 될 수 있으므로 메타 데이터 트랙 목록에 대해 활성 인덱스를 설정 하지 않습니다. 대신, **Mediaplaybackitem** 개체의 [**SetPresentationMode**](/previous-versions/windows/dn986977(v=win.10)) 메서드를 호출 하 여 토글할 트랙의 인덱스를 전달 하 고 [**TimedMetadataTrackPresentationMode**](/uwp/api/Windows.Media.Playback.TimedMetadataTrackPresentationMode) 열거형의 값을 제공 합니다. 선택 하는 프레젠테이션 모드는 앱의 구현에 따라 달라 집니다. 이 예제에서 메타 데이터 트랙은 사용 하도록 설정 될 때 **Platformpresented** 로 설정 됩니다. 텍스트 기반 트랙의 경우 시스템에서 트랙의 텍스트 큐를 자동으로 표시 함을 의미 합니다. 설정/해제 단추가 해제 되어 있으면 프레젠테이션 모드는 **사용 안 함**으로 설정 됩니다. 즉, 텍스트가 표시 되지 않고 큐 이벤트가 발생 하지 않습니다. 큐 이벤트는이 문서의 뒷부분에서 설명 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetToggleChecked":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetToggleUnchecked":::

메타 데이터 트랙을 처리할 때 [**큐**](/uwp/api/windows.media.core.timedmetadatatrack.cues) 또는 [**ActiveCues**](/uwp/api/windows.media.core.timedmetadatatrack.activecues) 속성에 액세스 하 여 트랙 내의 큐 집합에 액세스할 수 있습니다. 미디어 항목의 큐 위치를 표시 하도록 UI를 업데이트 하기 위해이 작업을 수행할 수 있습니다.

## <a name="handle-unsupported-codecs-and-unknown-errors-when-opening-media-items"></a>미디어 항목을 열 때 지원 되지 않는 코덱 및 알 수 없는 오류 처리
Windows 10, 버전 1607부터 미디어 항목을 재생 하는 데 필요한 코덱이 앱이 실행 되는 장치에서 지원 되는지 또는 부분적으로 지원 되는지 확인할 수 있습니다. **Mediaplaybackitem** 트랙-변경 된 이벤트 (예: [**오디오**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged)간 변경)에 대 한 이벤트 처리기에서 먼저 추적 변경이 새 트랙을 삽입 하 고 있는지 확인 합니다. 이 경우 **IVectorChangedEventArgs** 매개 변수에 전달 된 인덱스를 사용 하 여 삽입 되는 트랙에 대 한 참조를 가져올 수 있습니다 .이 매개 변수는 **Mediaplaybackitem** 매개 변수 (예: 나머지 track [**컬렉션)**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotracks) 와 함께 사용할 수 있습니다.

삽입 된 트랙에 대 한 참조가 있으면 트랙의 [**Supportinfo**](/uwp/api/windows.media.core.audiotrack.supportinfo) 속성의 [**DecoderStatus**](/uwp/api/windows.media.core.audiotracksupportinfo.decoderstatus) 를 확인 합니다. 값이 [**FullySupported**](/uwp/api/Windows.Media.Core.MediaDecoderStatus)이면 트랙을 재생 하는 데 필요한 적절 한 코덱이 장치에 표시 됩니다. 값이 [**저하**](/uwp/api/Windows.Media.Core.MediaDecoderStatus)된 경우 시스템에서 트랙을 재생할 수 있지만 어떤 식으로든 재생 속도가 저하 됩니다. 예를 들어 5.1 오디오 트랙은 대신 2 채널 스테레오로 재생할 수 있습니다. 이 경우 사용자에 게 성능 저하를 경고 하기 위해 UI를 업데이트 해야 할 수 있습니다. 값이 [**Unsupportedsubtype 유형**](/uwp/api/Windows.Media.Core.MediaDecoderStatus) 또는 [**UnsupportedEncoderProperties**](/uwp/api/Windows.Media.Core.MediaDecoderStatus)인 경우 장치에서 현재 코덱으로 트랙을 재생할 수 없습니다. 사용자에 게 경고 하 고 항목의 재생을 건너뛰거나 사용자가 올바른 코덱을 다운로드할 수 있도록 UI를 구현할 수 있습니다. 트랙의 [**GetEncodingProperties**](/uwp/api/windows.media.core.audiotrack.getencodingproperties) 메서드를 사용 하 여 재생에 필요한 코덱을 결정할 수 있습니다.

마지막으로 트랙의 [**Openfailed**](/uwp/api/windows.media.core.audiotrack.openfailed) 이벤트를 등록할 수 있습니다 .이 이벤트는 트랙이 장치에서 지원 되지만 파이프라인에서 알 수 없는 오류로 인해 열리지 못한 경우에 발생 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAudioTracksChanged_CodecCheck":::

[**Openfailed**](/uwp/api/windows.media.core.audiotrack.openfailed) 이벤트 처리기에서 **MediaSource** 상태를 알 수 없는지 확인할 수 있습니다 .이 경우에는 프로그래밍 방식으로 다른 트랙을 선택 하 여 사용자가 다른 트랙을 선택 하거나 재생을 중단할 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetOpenFailed":::

## <a name="set-display-properties-used-by-the-system-media-transport-controls"></a>시스템 미디어 전송 컨트롤에서 사용 하는 표시 속성 설정
Windows 10 버전 1607부터 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 에서 재생 된 미디어는 기본적으로 시스템 미디어 전송 컨트롤 (smtc)에 자동으로 통합 됩니다. **Mediaplaybackitem**에 대 한 표시 속성을 업데이트 하 여 smtc에서 표시 되는 메타 데이터를 지정할 수 있습니다. [**Getdisplayproperties**](/uwp/api/windows.media.playback.mediaplaybackitem.getdisplayproperties)를 호출 하 여 항목의 표시 속성을 나타내는 개체를 가져옵니다. [**유형**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.type) 속성을 설정 하 여 재생 항목이 음악과 비디오 인지 여부를 설정 합니다. 그런 다음 개체의 [**videoproperties**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.videoproperties) 또는 [**MusicProperties**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.musicproperties)의 속성을 설정 합니다. [**Applydisplayproperties**](/uwp/api/windows.media.playback.mediaplaybackitem.applydisplayproperties) 를 호출 하 여 항목의 속성을 입력 한 값으로 업데이트 합니다. 일반적으로 앱은 웹 서비스에서 표시 값을 동적으로 검색 하지만 다음 예제에서는 하드 코드 된 값을 사용 하는이 프로세스를 보여 줍니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetSetVideoProperties":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetSetMusicProperties":::

## <a name="add-external-timed-text-with-timedtextsource"></a>TimedTextSource를 사용 하 여 외부 시간 제한 텍스트 추가

일부 시나리오의 경우 미디어 항목과 관련 된 시간이 지정 된 텍스트가 포함 된 외부 파일이 있을 수 있습니다. 예를 들어 다른 로캘에 대 한 자막이 포함 된 별도의 파일을 사용할 수 있습니다. [**Timedtextsource**](/uwp/api/Windows.Media.Core.TimedTextSource) 클래스를 사용 하 여 스트림 또는 URI에서 외부에서 시간이 지정 된 텍스트 파일을 로드 합니다.

이 예제에서는 **사전** 컬렉션을 사용 하 여 원본 URI와 **timedtextsource** 개체를 사용 하 여 미디어 항목에 대 한 시간 제한 텍스트 목록을 저장 한 후 트랙을 식별 하기 위해 키/값 쌍으로 저장 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedTextSourceMap":::

[**Createfromuri**](/uwp/api/windows.media.core.mediasource.createfromuri)를 호출 하 여 각 외부 시간 제한 텍스트 파일에 대해 새 **Timedtextsource** 를 만듭니다. 시간이 지정 된 텍스트 소스에 대 한 항목을 **사전** 에 추가 합니다. 항목을 로드 하지 못한 경우 처리 하거나 항목이 성공적으로 로드 된 후 추가 속성을 설정 하기 위해 [**Timedtextsource. 해결**](/uwp/api/windows.media.core.timedtextsource.resolved) 된 이벤트에 대 한 처리기를 추가 합니다.

[**Externaltimedtextsources**](/uwp/api/windows.media.core.mediasource.externaltimedtextsources) 컬렉션에 **MediaSource** 를 추가 하 여 모든 **timedtextsource** 개체를 등록 합니다. 외부의 시간 제한 텍스트 원본은 원본에서 만든 **Mediaplaybackitem** 이 아닌 **MediaSource** 직접 추가 됩니다. 외부 텍스트 트랙을 반영 하도록 UI를 업데이트 하려면이 문서의 앞부분에서 설명한 대로 **TimedMetadataTracksChanged** 이벤트를 등록 하 고 처리 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedTextSource":::

[**Timedtextsource. 해결**](/uwp/api/windows.media.core.timedtextsource.resolved) 된 이벤트에 대 한 처리기에서 처리기에 전달 된 [**TimedTextSourceResolveResultEventArgs**](/uwp/api/Windows.Media.Core.TimedTextSourceResolveResultEventArgs) 의 **error** 속성을 확인 하 여 시간이 지정 된 텍스트 데이터를 로드 하는 동안 오류가 발생 했는지 확인 합니다. 항목이 성공적으로 확인 되 면이 처리기를 사용 하 여 확인 된 트랙의 추가 속성을 업데이트할 수 있습니다. 이 예에서는 이전에 **사전**에 저장 한 URI를 기준으로 각 트랙에 대 한 레이블을 추가 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedTextSourceResolved":::

## <a name="add-additional-metadata-tracks"></a>메타 데이터 트랙 추가

코드에서 사용자 지정 메타 데이터 트랙을 동적으로 만들고 미디어 소스와 연결할 수 있습니다. 만든 트랙에는 부제 또는 캡션 텍스트가 포함 될 수 있으며, 사용자는 소유 앱 데이터를 포함할 수 있습니다.

생성자를 호출 하 고 ID, 언어 식별자 및 [**TimedMetadataKind**](/uwp/api/Windows.Media.Core.TimedMetadataKind) 열거형의 값을 지정 하 여 새 [**TimedMetadataTrack**](/uwp/api/Windows.Media.Core.TimedMetadataTrack) 를 만듭니다. [**CueEntered**](/uwp/api/windows.media.core.timedmetadatatrack.cueentered) 및 [**CueExited**](/uwp/api/windows.media.core.timedmetadatatrack.cueexited) 이벤트에 대 한 처리기를 등록 합니다. 이러한 이벤트는 큐에 대 한 시작 시간에 도달 하 고 각각의 큐 기간이 만료 된 경우에 발생 합니다.

만든 메타 데이터 트랙의 형식에 적합 한 새 큐 개체를 만들고 해당 트랙에 대 한 ID, 시작 시간 및 기간을 설정 합니다. 이 예에서는 데이터 트랙을 만들어 [**Datacue**](/uwp/api/Windows.Media.Core.DataCue) 개체 집합이 생성 되 고 각 큐에 대해 앱 별 데이터가 포함 된 버퍼가 제공 됩니다. 새 트랙을 등록 하려면 **MediaSource** 개체의 [**ExternalTimedMetadataTracks**](/uwp/api/windows.media.core.mediasource.externaltimedmetadatatracks) 컬렉션에 추가 합니다.

Windows 10 버전 1703부터 **Datacue** 속성은 **CueEntered** 및 **CueExited** 이벤트에서 검색할 수 있는 키/데이터 쌍으로 사용자 지정 속성을 저장 하는 데 사용할 수 있는 [**PropertySet**](/uwp/api/windows.foundation.collections.propertyset) 을 노출 합니다.  

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAddDataTrack":::

**CueEntered** 이벤트는 연결 된 트랙에 **applicationpresented**, **Hidden**또는 **platformpresented** 의 프레젠테이션 모드가 있으면 큐의 시작 시간에 도달 하면 발생 합니다. 트랙의 프레젠테이션 모드가 **비활성화**되어 있는 동안에는 메타 데이터 트랙에 대해 큐 이벤트가 발생 하지 않습니다. 이 예제에서는 단순히 큐와 연결 된 사용자 지정 데이터를 디버그 창에 출력 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDataCueEntered":::

이 예에서는 추적을 만들 때 **TimedMetadataKind** 를 지정 하 고 [**Timedtextcue**](/uwp/api/Windows.Media.Core.TimedTextCue) 개체를 사용 하 여 트랙에 큐를 추가 하는 방법으로 사용자 지정 텍스트 트랙을 추가 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAddTextTrack":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTextCueEntered":::

## <a name="play-a-list-of-media-items-with-mediaplaybacklist"></a>MediaPlaybackList를 사용 하 여 미디어 항목 목록 재생

[**Mediaplaybacklist**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) 를 사용 하 여 **Mediaplaybacklist** 개체로 표시 되는 미디어 항목의 재생 목록을 만들 수 있습니다.

**참고**    [**Mediaplaybacklist**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) 의 항목은 gapless 재생을 사용 하 여 렌더링 됩니다. 시스템은 제공 된 메타 데이터를 MP3 또는 AAC 인코드된 파일로 사용 하 여 gapless 재생에 필요한 지연 또는 패딩 보정을 결정 합니다. MP3 또는 AAC 인코드된 파일이이 메타 데이터를 제공 하지 않으면 시스템은 지연 또는 패딩 발견적를 확인 합니다. PCM, FLAC 또는 ALAC와 같은 무손실 형식의 경우 이러한 인코더는 지연 또는 패딩을 도입 하지 않기 때문에 시스템은 아무런 작업도 수행 하지 않습니다.

시작 하려면 **Mediaplaybacklist**를 저장할 변수를 선언 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaPlaybackList":::

이 문서의 앞에서 설명한 것과 동일한 절차를 사용 하 여 목록에 추가 하려는 각 미디어 항목에 대해 **Mediaplaybackitem** 을 만듭니다. **Mediaplaybacklist** 개체를 초기화 하 고 미디어 재생 항목을 여기에 추가 합니다. [**Currentitemchanged**](/uwp/api/windows.media.playback.mediaplaybacklist.currentitemchanged) 이벤트에 대 한 처리기를 등록 합니다. 이 이벤트를 사용 하면 현재 재생 중인 미디어 항목을 반영 하도록 UI를 업데이트할 수 있습니다. 목록의 항목이 성공적으로 열리면 [Itemopened](/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemOpened) 이벤트를 등록 하 고, 목록에서 항목을 열 수 없을 때 발생 하는 [itemopened](/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemFailed) 이벤트를 등록할 수도 있습니다.

Windows 10, 버전 1703부터 [MaxPlayedItemsToKeepOpen](/uwp/api/Windows.Media.Playback.MediaPlaybackList.MaxPlayedItemsToKeepOpen) 속성을 설정 하 여 시스템을 재생 한 후 시스템에서 계속 열어 두 **는** mediaplaybackitem 개체의 최대 수를 지정할 수 있습니다. **MediaPlaybackItem** **Mediaplaybackitem** 을 열린 상태로 유지 하는 경우 항목을 다시 로드 하지 않아도 되기 때문에 사용자가 해당 항목으로 전환할 때 항목 재생이 즉시 시작 될 수 있습니다. 그러나 항목을 열어 두면 앱의 메모리 소비가 증가 하므로이 값을 설정할 때 응답성과 메모리 사용량 간의 균형을 고려해 야 합니다. 

목록의 재생을 사용 하도록 설정 하려면 **MediaPlayer** 의 재생 원본을 **Mediaplaybacklist**로 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlayMediaPlaybackList":::

**Currentitemchanged** 이벤트 처리기에서 현재 재생 중인 항목을 반영 하도록 UI를 업데이트 합니다 .이 항목은 이벤트로 전달 되는 [**CurrentMediaPlaybackItemChangedEventArgs**](/uwp/api/Windows.Media.Playback.CurrentMediaPlaybackItemChangedEventArgs) 개체의 [**NewItem**](/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.newitem) 속성을 사용 하 여 검색할 수 있습니다. 이 이벤트에서 UI를 업데이트 하는 경우 [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher.runasync) 에 대 한 호출 내에서 ui 스레드에 업데이트를 적용 하도록이 작업을 수행 해야 합니다.

Windows 10, 버전 1703부터 [CurrentMediaPlaybackItemChangedEventArgs](/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.Reason) 속성을 확인 하 여 항목이 변경 된 이유 (예: 프로그래밍 방식으로 앱을 전환 하는 경우, 이전에 재생 한 항목이 끝에 도달 했거나 오류가 발생 한 경우)를 나타내는 값을 가져올 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetMediaPlaybackListItemChanged":::


[**MovePrevious**](/uwp/api/windows.media.playback.mediaplaybacklist.moveprevious) 또는 [**MoveNext**](/uwp/api/windows.media.playback.mediaplaybacklist.movenext) 를 호출 하 여 Media Player가 **mediaplaybacklist**의 이전 또는 다음 항목을 재생 하도록 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPrevButton":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetNextButton":::

[**ShuffleEnabled**](/uwp/api/windows.media.playback.mediaplaybacklist.shuffleenabled) 속성을 설정 하 여 media player가 목록의 항목을 임의 순서로 재생할지 여부를 지정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetShuffleButton":::

[**AutoRepeatEnabled**](/uwp/api/windows.media.playback.mediaplaybacklist.autorepeatenabled) 속성을 설정 하 여 미디어 플레이어가 목록의 재생을 반복 해야 하는지 여부를 지정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetRepeatButton":::


### <a name="handle-the-failure-of-media-items-in-a-playback-list"></a>재생 목록에서 미디어 항목의 실패를 처리 합니다.
목록의 항목이 열리지 않으면 [**Itemfailed**](/uwp/api/windows.media.playback.mediaplaybacklist.itemfailed) 이벤트가 발생 합니다. 처리기로 전달 되는 [**Mediaplaybackitemerror**](/uwp/api/Windows.Media.Playback.MediaPlaybackItemError) 개체의 [**ErrorCode**](/uwp/api/windows.media.playback.mediaplaybackitemerror.errorcode) 속성은 가능한 경우 네트워크 오류, 디코딩 오류 또는 암호화 오류 등의 특정 실패 원인을 열거 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetItemFailed":::

### <a name="disable-playback-of-items-in-a-playback-list"></a>재생 목록에서 항목 재생 사용 안 함
Windows 10 버전 1703부터 [Mediaplaybackitem](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) 의 [IsDisabledInPlaybackList](/uwp/api/Windows.Media.Playback.MediaPlaybackItem.IsDisabledInPlaybackList) 속성을 false로 설정 하 여 **mediaplaybackitemlist** 에서 하나 이상의 항목에 대 한 재생을 사용 하지 않도록 설정할 수 있습니다. 

이 기능에 대 한 일반적인 시나리오는 인터넷에서 스트리밍된 음악을 재생 하는 앱에 대 한 것입니다. 앱은 장치의 네트워크 연결 상태에 대 한 변경 내용을 수신 대기 하 고 완전히 다운로드 되지 않은 항목의 재생을 사용 하지 않도록 설정할 수 있습니다. 다음 예제에서는 [system.net.networkinformation](/uwp/api/Windows.Networking.Connectivity.NetworkInformation.NetworkStatusChanged) 이벤트에 대 한 처리기가 등록 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterNetworkStatusChanged":::

**Networkstatuschanged 처리기 변경 됨**에서 [Getinternetconnectionprofile](/uwp/api/Windows.Networking.Connectivity.NetworkInformation.GetInternetConnectionProfile) 이 null을 반환 하는지 확인 합니다 .이는 네트워크가 연결 되지 않았음을 나타냅니다. 이 경우 재생 목록의 모든 항목을 반복 하 고 항목에 대 한 [Totaldownloadprogress](/uwp/api/windows.media.playback.mediaplaybackitem.TotalDownloadProgress) 가 1 보다 작은 경우 항목이 완전히 다운로드 되지 않은 것입니다. 즉, 항목을 사용 하지 않도록 설정 합니다. 네트워크 연결을 사용 하는 경우 재생 목록의 모든 항목을 반복 하 고 각 항목을 사용 하도록 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetNetworkStatusChanged":::

### <a name="defer-binding-of-media-content-for-items-in-a-playback-list-by-using-mediabinder"></a>MediaBinder를 사용 하 여 재생 목록의 항목에 대 한 미디어 콘텐츠 바인딩 지연
이전 예제에서 **MediaSource** 는 파일, URL 또는 스트림에서 생성 된 후 **Mediaplaybackitem** 이 만들어지고 **mediaplaybackitem**에 추가 됩니다. 사용자가 콘텐츠 보기에 대 한 요금이 부과 되는 경우와 같은 일부 시나리오의 경우 재생 목록의 항목을 실제로 재생할 준비가 될 때까지 **MediaSource** 콘텐츠의 검색을 연기할 수 있습니다. 이 시나리오를 구현 하려면 [**MediaBinder**](/uwp/api/Windows.Media.Core.MediaBinder) 클래스의 인스턴스를 만듭니다. [**토큰**](/uwp/api/Windows.Media.Core.MediaBinder.Token) 속성을 검색을 연기할 콘텐츠를 식별 하는 앱 정의 문자열로 설정 하 고 [**바인딩**](/uwp/api/Windows.Media.Core.MediaBinder.Binding) 이벤트에 대 한 처리기를 등록 합니다. 다음으로 [**MediaSource. CreateFromMediaBinder**](/uwp/api/windows.media.core.mediasource.createfrommediabinder)를 호출 하 여 **바인더** 에서 **MediaSource** 을 만듭니다. 그런 다음 **MediaSource** 에서 **Mediaplaybackitem** 을 만들고 평소와 같이 재생 목록에 추가 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetInitMediaBinder":::

시스템에서 **MediaBinder** 와 연결 된 콘텐츠를 검색 해야 하는 것으로 확인 되 면 **바인딩** 이벤트를 발생 시킵니다. 이 이벤트에 대 한 처리기에서 이벤트에 전달 된 [**Mediabindingeventargs**](/uwp/api/windows.media.core.mediabindingeventargs) 에서 **MediaBinder** 인스턴스를 검색할 수 있습니다. **토큰** 속성에 대해 지정한 문자열을 검색 하 여 검색할 콘텐츠를 결정 하는 데 사용 합니다. **Mediabindingeventargs** 는 [**SetStorageFile**](/uwp/api/windows.media.core.mediabindingeventargs.setstoragefile), [**setstream**](/uwp/api/windows.media.core.mediabindingeventargs.setstream), [**setstreamreference**](/uwp/api/windows.media.core.mediabindingeventargs.setstreamreference)및 [**setstream**](/uwp/api/windows.media.core.mediabindingeventargs.seturi)를 비롯 한 여러 다른 표현으로 바인딩된 콘텐츠를 설정 하는 메서드를 제공 합니다. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetBinderBinding":::

**바인딩** 이벤트 처리기에서 웹 요청과 같은 비동기 작업을 수행 하는 경우에는 [**GetDeferral**](/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) 메서드를 호출 하 여 작업이 완료 될 때까지 대기 하도록 시스템에 지시 해야 합니다. 작업이 완료 되 면 지연을 호출 하 여 시스템을 계속 하도록 지시 합니다 [**.**](/uwp/api/windows.foundation.deferral.Complete)

Windows 10, 버전 1703부터 [**SetAdaptiveMediaSource**](/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)를 호출 하 여 [**AdaptiveMediaSource**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) 를 바인딩된 콘텐츠로 제공할 수 있습니다. 앱에서 적응 스트리밍을 사용 하는 방법에 대 한 자세한 내용은 [적응 스트리밍](adaptive-streaming.md)을 참조 하세요.



## <a name="related-topics"></a>관련 항목
* [미디어 재생](media-playback.md)
* [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)
* [을 (를) 사용 하 여 시스템과 통합](integrate-with-systemmediatransportcontrols.md)
* [백그라운드에서 미디어 재생](background-audio.md)
