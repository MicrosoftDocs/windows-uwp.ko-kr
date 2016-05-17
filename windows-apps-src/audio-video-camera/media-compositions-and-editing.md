---
author: drewbatgit
ms.assetid: C4DB495D-1F91-40EF-A55C-5CABBF3269A2
description: Windows.Media.Editing 네임스페이스의 API를 사용하면 사용자가 오디오 및 비디오 소스 파일에서 미디어 컴퍼지션을 만들 수 있게 허용하는 앱을 신속하게 개발할 수 있습니다.
title: 미디어 컴퍼지션 및 편집
---

# 미디어 컴퍼지션 및 편집

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


이 문서에서는 [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565) 네임스페이스의 API를 사용하여 사용자가 오디오 및 비디오 소스 파일에서 미디어 컴퍼지션을 만들 수 있게 허용하는 앱을 신속하게 개발하는 방법에 대해 설명합니다. 이 프레임워크의 기능에는 여러 비디오 클립을 프로그래밍 방식으로 함께 추가하는 기능과 비디오 및 이미지 오버레이 추가, 백그라운드 오디오 추가, 오디오 효과와 비디오 효과 적용 등의 기능이 포함됩니다. 생성된 미디어 컴퍼지션은 재생 또는 공유를 위한 플랫 미디어 파일로 렌더링할 수 있습니다. 그러나 컴퍼지션을 디스크로 직렬화하거나 디스크에서 역직렬화함으로써 사용자가 이전에 만든 컴퍼지션을 로드하고 수정할 수 있게 할 수도 있습니다. 이 모든 기능은 낮은 수준의 [Microsoft 미디어 파운데이션](https://msdn.microsoft.com/library/windows/desktop/ms694197) API와 비교해 볼 때 이러한 작업을 수행하는 데 필요한 코드의 양과 복잡성이 상당히 줄어드는 편리한 Windows 런타임 인터페이스에서 제공됩니다.

## 새 미디어 컴퍼지션 만들기

[
            **MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) 클래스는 컴퍼지션을 구성하는 모든 미디어 클립이 들어 있는 컨테이너로서, 최종 컴퍼지션을 렌더링하여 컴퍼지션을 디스크에 로드 및 저장하고 사용자가 UI에서 볼 수 있도록 컴퍼지션의 미리 보기 스트림을 제공합니다. 앱에 **MediaComposition**을 사용하려면 [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565) 네임스페이스뿐만 아니라 필요한 관련 API를 제공하는 [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) 네임스페이스도 포함합니다.

[!code-cs[Namespace1](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace1)]

**MediaComposition** 개체는 코드의 다양한 지점에서 액세스하므로, 일반적으로 이 개체를 저장할 멤버 변수를 선언합니다.

[!code-cs[DeclareMediaComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

**MediaComposition**에 대한 생성자에는 인수가 사용되지 않습니다.

[!code-cs[MediaCompositionConstructor](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetMediaCompositionConstructor)]

## 컴퍼지션에 미디어 클립 추가

미디어 컴퍼지션에는 일반적으로 하나 이상의 비디오 클립이 포함됩니다. [
            **FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/hh738369)를 사용하면 사용자가 비디오 파일을 선택할 수 있게 만들 수 있습니다. 파일을 선택했으면 [**MediaClip.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607)를 호출하여 비디오 클립을 포함할 새 [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596) 개체를 만듭니다. 그런 다음, **MediaComposition** 개체의 [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648) 목록에 클립을 추가합니다.

[!code-cs[PickFileAndAddClip](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetPickFileAndAddClip)]

-   미디어 클립이 [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648) 목록에 나타나는 것과 동일한 순서로 **MediaComposition**에 나타납니다.

-   **MediaClip**은 컴퍼지션에 한 번만 포함할 수 있습니다. 이미 컴퍼지션에서 사용 중인 **MediaClip**을 추가하려고 하면 오류가 발생합니다. 비디오 클립을 컴퍼지션에서 여러 번 다시 사용하려면 [**Clone**](https://msdn.microsoft.com/library/windows/apps/dn652599)을 호출하여 컴퍼지션에 추가할 수 있는 새 **MediaClip** 개체를 만듭니다.

-   유니버설 Windows 앱에는 전체 파일 시스템에 액세스할 수 있는 권한이 없습니다. [
            **StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456) 클래스의 [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) 속성은 앱이 사용자가 선택한 파일의 레코드를 저장할 수 있게 하여 이 파일에 대한 액세스 권한을 보유할 수 있습니다. **FutureAccessList**에 허용되는 항목 수는 최대 1000개이므로, 앱은 가득 차지 않도록 이 목록을 관리해야 합니다. 이 작업은 이전에 만든 컴퍼지션에 대한 로드 및 수정을 지원하려는 경우에 특히 중요합니다.

-   **MediaComposition**은 MP4 형식의 비디오 클립을 지원합니다.

-   비디오 파일에 포함된 오디오 트랙이 여러 개 있는 경우 [**SelectedEmbeddedAudioTrackIndex**](https://msdn.microsoft.com/library/windows/apps/dn652627) 속성을 설정하여 컴퍼지션에 사용될 오디오 트랙을 선택할 수 있습니다.

-   [
            **CreateFromColor**](https://msdn.microsoft.com/library/windows/apps/dn652605)를 호출하고 클립에 대해 색과 기간을 지정하여 단일 색으로 전체 프레임을 채운 **MediaClip**을 만듭니다.

-   [
            **CreateFromImageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652610)를 호출하고 클립에 대해 이미지 파일과 기간을 지정하여 이미지 파일에서 **MediaClip**을 만듭니다.

-   [
            **CreateFromSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505)를 호출하고 클립에서 화면과 기간을 지정하여 [**IDirect3DSurface**](https://msdn.microsoft.com/library/dn764774)에서 **MediaClip**을 만듭니다.

## MediaElement에서 컴퍼지션 미리 보기

사용자가 미디어 컴퍼지션을 볼 수 있게 하려면 UI를 정의하는 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)를 XAML 파일에 추가합니다.

[!code-xml[MediaElement](./code/MediaEditing/cs/MainPage.xaml#SnippetMediaElement)]

[
            **MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn282716) 형식의 멤버 변수를 선언합니다.


[!code-cs[DeclareMediaStreamSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaStreamSource)]

**MediaComposition** 개체의 [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674) 메서드를 호출하여 컴퍼지션에 대한 **MediaStreamSource**를 만든 다음 **MediaElement**의 [**SetMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn299029) 메서드를 호출합니다. 이제 이 컴퍼지션을 UI에서 볼 수 있습니다.


[!code-cs[UpdateMediaElementSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetUpdateMediaElementSource)]

-   [
            **GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674)를 호출하기 전에 **MediaComposition**에 하나 이상의 미디어 클립이 있어야 하며, 그렇지 않은 경우 반환되는 개체는 null입니다.

-   **MediaElement** 타임라인은 컴퍼지션의 변경 사항을 반영하도록 자동으로 업데이트되지 않습니다. 컴퍼지션에 일련의 변경을 수행하여 UI를 업데이트하려는 경우에는 그때마다 **GeneratePreviewMediaStreamSource**와 **SetMediaStreamSource**를 모두 호출하는 것이 좋습니다.

사용자가 관련 리소스를 해제하기 위해 페이지에서 벗어나 이동할 때는 **MediaStreamSource** 개체 및 **MediaElement**의 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 속성을 null로 설정하는 것이 좋습니다.

[!code-cs[OnNavigatedFrom](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

## 컴퍼지션을 비디오 파일로 렌더링

미디어 컴퍼지션을 다른 디바이스에서 공유 및 볼 수 있도록 플랫 비디오 파일로 렌더링하려면 [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105) 네임스페이스의 API를 사용해야 합니다. 비동기 작업의 진행률에 따라 UI를 업데이트하려면 [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383) 네임스페이스의 API도 필요합니다.

[!code-cs[Namespace2](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace2)]

사용자가 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)로 출력 파일을 선택할 수 있도록 허용하려면 **MediaComposition** 개체의 [**RenderToFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652690)를 호출하여 컴퍼지션을 선택된 파일로 렌더링합니다. 다음 예제에 나온 코드의 나머지 부분은 단순히 [**AsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/desktop/br205807) 처리 패턴을 따릅니다.

[!code-cs[RenderCompositionToFile](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetRenderCompositionToFile)]

-   [
            **MediaTrimmingPreference**](https://msdn.microsoft.com/library/windows/apps/dn640561)를 사용하면 코드 변환 작업의 속도와 인접 미디어 클립 자르기의 정밀도 중 우선 순위를 지정할 수 있습니다. **Fast**를 사용하면 자르기 정밀도가 낮아지는 동시에 코드 변환 속도가 높아지며, **Precise**를 사용하면 자르기 정밀도가 높아지지만 코드 변환 속도는 떨어집니다.

## 비디오 클립 자르기

[
            **MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596) 개체 [**TrimTimeFromStart**](https://msdn.microsoft.com/library/windows/apps/dn652637) 속성이나 [**TrimTimeFromEnd**](https://msdn.microsoft.com/library/windows/apps/dn652634) 속성 또는 둘 다를 설정하여 컴퍼지션에서 비디오 클립의 지속 시간을 자릅니다.

[!code-cs[TrimClipBeforeCurrentPosition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetTrimClipBeforeCurrentPosition)]

-   원하는 임의의 UI를 사용하여 사용자가 트리밍 시작 값과 트리밍 종료 값을 지정할 수 있게 할 수 있습니다. 위의 예제에서는 **MediaElement**의 [**Position**](https://msdn.microsoft.com/library/windows/apps/br227407) 속성을 사용하여 먼저 컴퍼지션의 현재 위치에서 재생 중인 MediaClip을 확인합니다. 이때 [**StartTimeInComposition**](https://msdn.microsoft.com/library/windows/apps/dn652629) 및 [**EndTimeInComposition**](https://msdn.microsoft.com/library/windows/apps/dn652618)을 확인하면 됩니다. 그런 다음, **Position** 및 **StartTimeInComposition** 속성을 다시 사용하여 클립의 시작 부분 이후 자를 시간의 양을 계산합니다. **FirstOrDefault** 메서드는 목록에서 항목을 선택하기 위한 코드를 간소화하는 **System.Linq** 네임스페이스의 확장 메서드입니다.
-   **MediaClip** 개체의 [**OriginalDuration**](https://msdn.microsoft.com/library/windows/apps/dn652625) 속성은 클리핑을 적용하지 않고도 미디어 클립의 지속 시간을 알 수 있게 합니다.
-   [
            **TrimmedDuration**](https://msdn.microsoft.com/library/windows/apps/dn652631) 속성을 통해 자르기를 적용한 후의 미디어 클립 지속 시간을 알 수 있습니다.
-   클립의 원래 지속 시간보다 큰 자르기 값을 지정해도 오류가 발생하지 않습니다. 그러나 컴퍼지션에 단일 클립만이 있고 큰 자르기 값을 지정하여 이 클립이 길이 0으로 잘린 경우에는 후속 [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674) 호출에서 마치 컴퍼지션에 클립이 없는 것처럼 null이 반환됩니다.

## 컴퍼지션에 백그라운드 오디오 트랙 추가

컴퍼지션에 백그라운드 트랙을 추가하려면 오디오 파일을 로드한 다음 팩터리 메서드 [**BackgroundAudioTrack.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652561)를 호출하여 [**BackgroundAudioTrack**](https://msdn.microsoft.com/library/windows/apps/dn652544) 개체를 만듭니다. 그런 다음, 컴퍼지션의 [**BackgroundAudioTracks**](https://msdn.microsoft.com/library/windows/apps/dn652647) 속성에 **BackgroundAudioTrack**을 추가합니다.

[!code-cs[AddBackgroundAudioTrack](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddBackgroundAudioTrack)]

-   **MediaComposition**은 MP3, WAV, FLAC 형식의 백그라운드 오디오 트랙을 지원합니다.

-   백그라운드 오디오 트랙

-   비디오 파일의 경우와 마찬가지로, 컴퍼지션의 파일에 대한 액세스를 유지하는 [**StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456) 클래스를 사용해야 합니다.

-   **MediaClip**의 경우와 마찬가지로, **BackgroundAudioTrack**은 컴퍼지션에 한 번만 포함할 수 있습니다. 이미 컴퍼지션에서 사용 중인 **BackgroundAudioTrack**을 추가하려고 하면 오류가 발생합니다. 오디오 트랙을 컴퍼지션에서 여러 번 다시 사용하려면 [**Clone**](https://msdn.microsoft.com/library/windows/apps/dn652599)을 호출하여 컴퍼지션에 추가할 수 있는 새 **MediaClip** 개체를 만듭니다.

-   기본적으로 백그라운드 오디오 트랙은 컴퍼지션 시작 부분에서 재생되기 시작합니다. 여러 백그라운드 트랙이 있는 경우에는 컴퍼지션의 시작 부분에서 모든 트랙이 재생되기 시작합니다. 다른 시간에 백그라운드 오디오 트랙이 재생되기 시작하도록 만들려면 [**Delay**](https://msdn.microsoft.com/library/windows/apps/dn652563) 속성을 원하는 시간 오프셋으로 설정합니다.

## 컴퍼지션에 오버레이 추가

오버레이를 사용하면 컴퍼지션에서 여러 비디오 계층을 서로 쌓아 올릴 수 있습니다. 컴퍼지션은 여러 개의 오버레이 계층을 포함할 수 있으며, 각 계층은 여러 오버레이를 포함할 수 있습니다. 해당 생성자에 **MediaClip**을 전달하여 [**MediaOverlay**](https://msdn.microsoft.com/library/windows/apps/dn764793) 개체를 만듭니다. 오버레이의 위치와 불투명도를 설정한 다음, 새 [**MediaOverlayLayer**](https://msdn.microsoft.com/library/windows/apps/dn764795)를 만들고 해당 [**Overlays**](https://msdn.microsoft.com/library/windows/desktop/dn280411) 목록에 **MediaOverlay**를 추가합니다. 마지막으로, 컴퍼지션의 [**OverlayLayers**](https://msdn.microsoft.com/library/windows/apps/dn764791) 목록에 **MediaOverlayLayer**를 추가합니다.

[!code-cs[AddOverlay](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddOverlay)]

-   계층 내의 오버레이는 포함 계층의 **Overlays** 목록에 나오는 순서에 따라 z-순서로 배열됩니다. 목록 내에서 더 높은 인덱스가 더 낮은 인덱스 위에 렌더링됩니다. 컴퍼지션 내의 오버레이 계층의 경우도 마찬가지입니다. 컴퍼지션의 **OverlayLayers** 목록에서 인덱스가 더 높은 계층이 더 낮은 인덱스 위에 렌더링됩니다.

-   오버레이는 순차적으로 재생되지 않고 서로 위에 쌓이기 때문에, 모든 오버레이는 기본적으로 컴퍼지션의 시작 부분에서 재생되기 시작합니다. 다른 시간에 오버레이가 재생되기 시작하도록 만들려면 [**Delay**](https://msdn.microsoft.com/library/windows/apps/dn764810) 속성을 원하는 시간 오프셋으로 설정합니다.

## 미디어 클립에 효과 추가

컴퍼지션의 각 **MediaClip**에는 여러 효과를 추가할 수 있는 오디오 및 비디오 효과 목록이 있습니다. 이러한 효과는 각각 [**IAudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608044)과 [**IVideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608047)을 구현해야 합니다. 다음 예제에서는 현재 미디어 요소 위치를 사용하여 현재 표시되는 **MediaClip**을 선택한 다음 새 [**VideoStabilizationEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn926762) 인스턴스를 만들어 이 인스턴스를 미디어 클립의 [**VideoEffectDefinitions**](https://msdn.microsoft.com/library/windows/apps/dn652643) 목록에 추가합니다.

[!code-cs[AddVideoEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddVideoEffect)]

## 파일에 컴퍼지션 저장

미디어 컴퍼지션은 나중에 수정할 수 있도록 파일로 직렬화할 수 있습니다. 출력 파일을 선택한 다음, [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) 메서드 [**SaveAsync**](https://msdn.microsoft.com/library/windows/apps/dn640554)를 호출하여 컴퍼지션을 저장합니다.

[!code-cs[SaveComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetSaveComposition)]

## 파일에서 컴퍼지션 로드

사용자가 컴퍼지션을 보고 수정할 수 있도록 파일에서 미디어 컴퍼지션을 역직렬화할 수 있습니다. 구성 파일을 선택한 다음 [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) 메서드 [**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/dn652684)를 호출하여 컴퍼지션을 로드합니다.

[!code-cs[OpenComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOpenComposition)]

-   컴퍼지션의 미디어 파일이 앱에서 액세스할 수 있는 위치에 없으며 앱에 대한 [**StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456) 클래스의 [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) 속성에 없는 경우 컴퍼지션을 로드할 때 오류가 발생합니다.

 

 






<!--HONumber=May16_HO2-->


