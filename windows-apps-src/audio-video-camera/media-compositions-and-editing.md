---
ms.assetid: C4DB495D-1F91-40EF-A55C-5CABBF3269A2
description: Windows의 Api를 사용 하 여 사용자가 오디오 및 비디오 원본 파일에서 미디어 컴퍼지션을 만들 수 있는 앱을 신속 하 게 개발할 수 있습니다.
title: 미디어 컴퍼지션 및 편집
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b1ae11d8f065cf72202365c36a9d69164fc2daa3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163877"
---
# <a name="media-compositions-and-editing"></a>미디어 컴퍼지션 및 편집



이 문서에서는 [**Windows**](/uwp/api/Windows.Media.Editing) 의 api를 사용 하 여 사용자가 오디오 및 비디오 원본 파일에서 미디어 컴퍼지션을 만들 수 있도록 하는 앱을 신속 하 게 개발 하는 방법을 보여 줍니다. 프레임 워크의 기능에는 프로그래밍 방식으로 여러 비디오 클립을 추가 하 고, 비디오 및 이미지 오버레이를 추가 하 고, 배경 오디오를 추가 하 고, 오디오 및 비디오 효과를 적용 하는 기능이 포함 됩니다. 미디어 컴퍼지션을 만든 후에는 재생 또는 공유를 위해 플랫 미디어 파일로 렌더링할 수 있지만, 컴퍼지션을 디스크에서 serialize 및 deserialize 할 수 있으므로 사용자가 이전에 만든 컴퍼지션을 로드 하 고 수정할 수 있습니다. 이 기능은 모두 사용 하기 쉬운 Windows 런타임 인터페이스로 제공 되므로 하위 수준 [Microsoft 미디어 파운데이션](/windows/desktop/medfound/microsoft-media-foundation-sdk) API와 비교할 때 이러한 작업을 수행 하는 데 필요한 코드의 양과 복잡성을 대폭 줄일 수 있습니다.

## <a name="create-a-new-media-composition"></a>새 미디어 컴퍼지션 만들기

[**Mediacomposition**](/uwp/api/Windows.Media.Editing.MediaComposition) 클래스는 컴퍼지션을 구성 하는 모든 미디어 클립의 컨테이너 이며 최종 컴퍼지션을 렌더링 하 고, 디스크에 컴퍼지션을 로드 및 저장 하 고, 사용자가 UI에서 볼 수 있도록 컴포지션의 미리 보기 스트림을 제공 합니다. 앱에서 **Mediacomposition** 을 사용 하려면 필요에 따라 관련 api를 제공 하는 windows [**. media. 네임 스페이스와**](/uwp/api/Windows.Media.Editing) [**windows**](/uwp/api/Windows.Media.Core) 네임 스페이스를 포함 합니다.

[!code-cs[Namespace1](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace1)]


**Mediacomposition** 개체는 코드의 여러 지점에서 액세스할 수 있으므로 일반적으로이 개체를 저장할 멤버 변수를 선언 합니다.

[!code-cs[DeclareMediaComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

**Mediacomposition** 의 생성자는 인수를 사용 하지 않습니다.

[!code-cs[MediaCompositionConstructor](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetMediaCompositionConstructor)]

## <a name="add-media-clips-to-a-composition"></a>컴포지션에 미디어 클립 추가

미디어 컴포지션에는 일반적으로 하나 이상의 비디오 클립이 포함 됩니다. [**Fileopenpicker**](/uwp/schemas/appxpackage/appxmanifestschema/element-fileopenpicker) 를 사용 하 여 사용자가 비디오 파일을 선택할 수 있습니다. 파일이 선택 되 면 [**Mediaclip**](/uwp/api/windows.media.editing.mediaclip.createfromfileasync)를 호출 하 여 비디오 클립을 포함 하는 새 [**mediaclip**](/uwp/api/Windows.Media.Editing.MediaClip) 개체를 만듭니다. 그런 다음 클립을 **Mediacomposition** 개체의 [**클립**](/uwp/api/windows.media.editing.mediacomposition.clips) 목록에 추가 합니다.

[!code-cs[PickFileAndAddClip](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetPickFileAndAddClip)]

-   미디어 클립은 [**클립**](/uwp/api/windows.media.editing.mediacomposition.clips) 목록에 표시 되는 것과 동일한 순서로 **Mediacomposition** 에 표시 됩니다.

-   **Mediaclip** 는 한 번에 하나의 컴포지션에만 포함할 수 있습니다. 이미 컴퍼지션에서 사용 중인 **Mediaclip** 를 추가 하려고 하면 오류가 발생 합니다. 컴포지션에서 비디오 클립을 여러 번 다시 사용 하려면 [**Clone**](/uwp/api/windows.media.editing.mediaclip.clone) 을 호출 하 여 새 **mediaclip** 개체를 만든 다음이 개체를 컴퍼지션에 추가할 수 있습니다.

-   유니버설 Windows 앱은 전체 파일 시스템에 액세스할 수 있는 권한이 없습니다. [**Storageapplicationpermissions**](/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) 클래스의 [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) 속성을 사용 하면 응용 프로그램에서 사용자가 선택한 파일의 레코드를 저장 하 여 파일에 대 한 액세스 권한을 유지할 수 있습니다. **FutureAccessList** 에는 최대 기간의 1000 항목이 있으므로 앱이 꽉 찰 수 없도록 목록을 관리 해야 합니다. 이전에 만든 컴포지션을 로드 하 고 수정 하는 것을 지원 하려는 경우에 특히 중요 합니다.

-   **Mediacomposition** 은 MP4 형식의 비디오 클립을 지원 합니다.

-   비디오 파일에 포함 된 오디오 트랙이 여러 개 있는 경우 [**SelectedEmbeddedAudioTrackIndex**](/uwp/api/windows.media.editing.mediaclip.selectedembeddedaudiotrackindex) 속성을 설정 하 여 컴포지션에서 사용 되는 오디오 트랙을 선택할 수 있습니다.

-   [**Createfromcolor**](/uwp/api/windows.media.editing.mediaclip.createfromcolor) 를 호출 하 고 클립에 대 한 색과 기간을 지정 하 여 전체 프레임을 채우는 단일 색을 사용 하 여 **mediaclip** 를 만듭니다.

-   [**CreateFromImageFileAsync**](/uwp/api/windows.media.editing.mediaclip.createfromimagefileasync) 를 호출 하 고 이미지 파일 및 클립 기간을 지정 하 여 이미지 파일에서 **mediaclip** 를 만듭니다.

-   [**Createfromsurface**](/uwp/api/windows.media.editing.mediaclip.createfromsurface) 를 호출 하 고 클립에서 서피스와 기간을 지정 하 여 [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) 에서 **mediaclip** 를 만듭니다.

## <a name="preview-the-composition-in-a-mediaelement"></a>MediaElement에서 컴퍼지션을 미리 봅니다.

사용자가 미디어 컴퍼지션을 볼 수 있도록 하려면 UI를 정의 하는 XAML 파일에 [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 를 추가 합니다.

[!code-xml[MediaElement](./code/MediaEditing/cs/MainPage.xaml#SnippetMediaElement)]

[**Mediastreamsource**](/uwp/api/Windows.Media.Core.MediaStreamSource)형식의 멤버 변수를 선언 합니다.


[!code-cs[DeclareMediaStreamSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaStreamSource)]

**Mediacomposition** 개체의 [**GeneratePreviewMediaStreamSource**](/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource) 메서드를 호출 하 여 컴포지션의 **mediastreamsource** 를 만듭니다. 팩터리 메서드 [**Createfrommediastreamsource**](/uwp/api/windows.media.core.mediasource.createfrommediastreamsource) 를 호출 하 여 [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) 개체를 만들고 **MediaPlayerElement**의 [**Source**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) 속성에 할당 합니다. 이제 UI에서 컴퍼지션을 볼 수 있습니다.


[!code-cs[UpdateMediaElementSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetUpdateMediaElementSource)]

-   **Mediacomposition** 은 [**GeneratePreviewMediaStreamSource**](/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource)를 호출 하기 전에 미디어 클립을 하나 이상 포함 해야 합니다. 그렇지 않으면 반환 된 개체가 null이 됩니다.

-   **MediaElement** 타임 라인은 컴퍼지션의 변경 내용을 반영 하도록 자동으로 업데이트 되지 않습니다. 컴퍼지션에 일련의 변경을 수행하여 UI를 업데이트하려는 경우에는 그때마다 **GeneratePreviewMediaStreamSource**를 호출하고 **MediaPlayerElement** **소스** 속성을 호출하는 것이 좋습니다.

사용자가 연결 된 리소스를 해제 하기 위해 페이지에서 벗어날 때 **Mediastreamsource** 개체와 **MediaPlayerElement** 의 [**source**](/uwp/api/windows.ui.xaml.controls.mediaelement.source) 속성을 null로 설정 하는 것이 좋습니다.

[!code-cs[OnNavigatedFrom](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

## <a name="render-the-composition-to-a-video-file"></a>비디오 파일에 컴퍼지션 렌더링

미디어 컴퍼지션을 다른 장치에서 공유 하 고 볼 수 있도록 미디어 컴퍼지션을 플랫 비디오 파일에 렌더링 하려면 Windows. d a. [**트랜스 코딩**](/uwp/api/Windows.Media.Transcoding) 네임 스페이스에서 api를 사용 해야 합니다. 비동기 작업의 진행률에 대 한 UI를 업데이트 하려면 [**Windows**](/uwp/api/Windows.UI.Core) 네임 스페이스의 api도 필요 합니다.

[!code-cs[Namespace2](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace2)]

사용자가 [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker)를 사용 하 여 출력 파일을 선택할 수 있게 한 후에는 **Mediacomposition** 개체의 [**RenderToFileAsync**](/uwp/api/windows.media.editing.mediacomposition.rendertofileasync)를 호출 하 여 컴퍼지션을 선택 된 파일로 렌더링 합니다. 다음 예제의 나머지 코드는 단순히 [**AsyncOperationWithProgress**](/previous-versions/br205807(v=vs.85))를 처리 하는 패턴을 따릅니다.

[!code-cs[RenderCompositionToFile](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetRenderCompositionToFile)]

-   [**MediaTrimmingPreference**](/uwp/api/Windows.Media.Editing.MediaTrimmingPreference) 를 사용 하면 트랜스 코딩 작업의 속도와 인접 한 미디어 클립의 트리밍 전체 자릿수를 우선 순위를 정할 수 있습니다. **빠른** 코드 변환이 낮은 정밀도 트리밍을 사용 하 여 더 빠르게 작동 하 고, **정확** 하 게 트랜스 코딩 속도가 느려지고, 보다 정확한 트리밍을 사용 합니다.

## <a name="trim-a-video-clip"></a>비디오 클립 자르기

[**Mediaclip**](/uwp/api/Windows.Media.Editing.MediaClip) 개체 [**TrimTimeFromStart**](/uwp/api/windows.media.editing.mediaclip.trimtimefromstart) 속성, [**TrimTimeFromEnd**](/uwp/api/windows.media.editing.mediaclip.trimtimefromend) 속성 또는 둘 다를 설정 하 여 컴포지션의 비디오 클립 기간을 자릅니다.

[!code-cs[TrimClipBeforeCurrentPosition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetTrimClipBeforeCurrentPosition)]

-   사용자가 시작 및 종료 트리밍 값을 지정할 수 있도록 하려는 모든 UI를 사용할 수 있습니다. 위의 예제에서는 **MediaPlayerElement** 와 연결 된 [**Mediaplaybacksession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession) 의 [**Position**](/uwp/api/windows.media.playback.mediaplaybacksession.position) 속성을 사용 하 여 [**Starttimeincomposition**](/uwp/api/windows.media.editing.mediaclip.starttimeincomposition) 및 [**endtimeincomposition**](/uwp/api/windows.media.editing.mediaclip.endtimeincomposition)확인 하 여 컴포지션의 현재 위치에서 재생 중인 **mediaclip** 를 먼저 확인 합니다. 그런 다음 **Position** 및 **Starttimeincomposition** 속성을 다시 사용 하 여 클립 시작 부분에서 잘라내는 시간을 계산 합니다. **FirstOrDefault** 메서드는 목록에서 항목을 선택 하는 코드를 간소화 하는 **system.string 네임 스페이스의 확장** 메서드입니다.
-   **Mediaclip** 개체의 [**originalduration**](/uwp/api/windows.media.editing.mediaclip.originalduration) 속성을 사용 하면 클리핑을 적용 하지 않고 미디어 클립의 기간을 알 수 있습니다.
-   [**TrimmedDuration**](/uwp/api/windows.media.editing.mediaclip.trimmedduration) 속성을 사용 하면 트리밍 적용 후 미디어 클립의 기간을 알 수 있습니다.
-   클립의 원래 지속 시간 보다 큰 트리밍 값을 지정 하면 오류가 발생 하지 않습니다. 그러나 컴퍼지션에 단일 클립만 포함 되 고, 대량 트리밍 값을 지정 하 여 길이가 0으로 잘린 경우 컴퍼지션에 클립이 없는 것 처럼 [**GeneratePreviewMediaStreamSource**](/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource) 에 대 한 후속 호출에서 null을 반환 합니다.

## <a name="add-a-background-audio-track-to-a-composition"></a>컴포지션에 배경 오디오 트랙 추가

컴포지션에 배경 트랙을 추가 하려면 오디오 파일을 로드 한 다음 [**BackgroundAudioTrack**](/uwp/api/windows.media.editing.backgroundaudiotrack.createfromfileasync)팩터리 메서드를 호출 하 여 [**BackgroundAudioTrack**](/uwp/api/Windows.Media.Editing.BackgroundAudioTrack) 개체를 만듭니다. 그런 다음 **BackgroundAudioTrack** 를 컴포지션의 [**BackgroundAudioTracks**](/uwp/api/windows.media.editing.mediacomposition.backgroundaudiotracks) 속성에 추가 합니다.

[!code-cs[AddBackgroundAudioTrack](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddBackgroundAudioTrack)]

-   **Mediacomposition** 은 MP3, WAV, FLAC 형식의 배경 오디오 트랙을 지원 합니다.

-   배경 오디오 트랙

-   비디오 파일의 경우와 마찬가지로, [**Storageapplicationpermissions**](/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) 클래스를 사용 하 여 컴퍼지션의 파일에 대 한 액세스를 유지 해야 합니다.

-   **Mediaclip**와 마찬가지로 **BackgroundAudioTrack** 는 한 번에 하나의 컴포지션에만 포함 될 수 있습니다. 컴퍼지션에서 이미 사용 중인 **BackgroundAudioTrack** 를 추가 하려고 하면 오류가 발생 합니다. 컴퍼지션에서 오디오 트랙을 여러 번 다시 사용 하려면 [**Clone**](/uwp/api/windows.media.editing.mediaclip.clone) 을 호출 하 여 새 **mediaclip** 개체를 만든 다음이 개체를 컴퍼지션에 추가할 수 있습니다.

-   기본적으로 배경 오디오 트랙은 컴퍼지션 시작 시 재생을 시작 합니다. 여러 백그라운드 트랙이 있는 경우 모든 트랙은 컴퍼지션 시작 시 재생을 시작 합니다. 배경 오디오 트랙이 다른 시간에 재생을 시작 하 게 하려면 [**Delay**](/uwp/api/windows.media.editing.backgroundaudiotrack.delay) 속성을 원하는 시간 오프셋으로 설정 합니다.

## <a name="add-an-overlay-to-a-composition"></a>컴퍼지션에 오버레이 추가

오버레이를 사용 하면 여러 계층의 비디오를 컴퍼지션에서 서로 위에 쌓을 수 있습니다. 컴포지션에는 여러 오버레이 계층이 포함 될 수 있으며, 각 계층에는 여러 오버레이가 포함 될 수 있습니다. **Mediaclip** 를 해당 생성자에 전달 하 여 [**mediaoverlay**](/uwp/api/Windows.Media.Editing.MediaOverlay) 개체를 만듭니다. 오버레이의 위치와 불투명도를 설정 하 고 새 [**MediaOverlayLayer**](/uwp/api/Windows.Media.Editing.MediaOverlayLayer) 를 만든 후 해당 [**층**](/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgioutput2-supportsoverlays) **을 오버레이 목록에 추가** 합니다. 마지막으로 컴퍼지션의 [**OverlayLayers**](/uwp/api/windows.media.editing.mediacomposition.overlaylayers) 목록에 **MediaOverlayLayer** 를 추가 합니다.

[!code-cs[AddOverlay](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddOverlay)]

-   계층 내 오버레이는 포함 하는 레이어의 **오버레이** 목록에서 순서에 따라 z 순서로 정렬 됩니다. 목록 내에서 더 높은 인덱스는 하위 인덱스 위에 렌더링 됩니다. 컴퍼지션 내 오버레이 계층의 경우에도 마찬가지입니다. 컴퍼지션의 **OverlayLayers** 목록에서 인덱스가 더 높은 계층이 더 낮은 인덱스 위에 렌더링됩니다.

-   오버레이는 순차적으로 재생 되는 것이 아니라 서로 위에 쌓입니다. 모든 오버레이는 기본적으로 컴퍼지션의 시작 부분에서 재생을 시작 합니다. 오버레이를 다른 시간에 재생 하기 시작 하려면 [**Delay**](/uwp/api/windows.media.editing.mediaoverlay.delay) 속성을 원하는 시간 오프셋으로 설정 합니다.

## <a name="add-effects-to-a-media-clip"></a>미디어 클립에 효과 추가

컴포지션의 각 **Mediaclip** 에는 여러 효과를 추가할 수 있는 오디오 및 비디오 효과 목록이 있습니다. 효과는 각각 [**IAudioEffectDefinition**](/uwp/api/Windows.Media.Effects.IAudioEffectDefinition) 및 [**IVideoEffectDefinition**](/uwp/api/Windows.Media.Effects.IVideoEffectDefinition) 를 구현 해야 합니다. 다음 예제에서는 현재 **MediaPlayerElement** 위치를 사용 하 여 현재 표시 된 **mediaclip** 를 선택한 다음 [**VideoStabilizationEffectDefinition**](/uwp/api/Windows.Media.Core.VideoStabilizationEffectDefinition) 의 새 인스턴스를 만들어 미디어 클립의 [**VideoEffectDefinitions**](/uwp/api/windows.media.editing.mediaclip.videoeffectdefinitions) 목록에 추가 합니다.

[!code-cs[AddVideoEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddVideoEffect)]

## <a name="save-a-composition-to-a-file"></a>파일에 컴퍼지션 저장

나중에 수정할 파일에 미디어 컴포지션을 직렬화 할 수 있습니다. 출력 파일을 선택한 다음 [**Mediacomposition**](/uwp/api/Windows.Media.Editing.MediaComposition) 메서드 [**SaveAsync**](/uwp/api/windows.media.editing.mediacomposition.saveasync) 를 호출 하 여 컴퍼지션을 저장 합니다.

[!code-cs[SaveComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetSaveComposition)]

## <a name="load-a-composition-from-a-file"></a>파일에서 컴퍼지션 로드

사용자가 컴퍼지션을 보거나 수정할 수 있도록 파일에서 미디어 컴퍼지션을 deserialize 할 수 있습니다. 컴퍼지션 파일을 선택한 다음 [**Mediacomposition**](/uwp/api/Windows.Media.Editing.MediaComposition) 메서드 [**LoadAsync**](/uwp/api/windows.media.editing.mediacomposition.loadasync) 를 호출 하 여 컴퍼지션을 로드 합니다.

[!code-cs[OpenComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOpenComposition)]

-   컴퍼지션의 미디어 파일이 앱에서 액세스할 수 있는 위치에 있지 않고 앱에 대 한 [**Storageapplicationpermissions**](/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) 클래스의 [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) 속성에 없는 경우 컴퍼지션을 로드 하면 오류가 throw 됩니다.

 

 