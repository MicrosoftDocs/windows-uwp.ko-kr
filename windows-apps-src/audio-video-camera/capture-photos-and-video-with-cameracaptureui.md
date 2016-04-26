---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: 이 문서에서는 CameraCaptureUI 클래스를 사용하여 Windows에 기본 제공된 카메라 UI로 사진 또는 동영상을 캡처하는 방법을 설명합니다.
title: CameraCaptureUI를 사용하여 사진 및 비디오 캡처
---

# CameraCaptureUI를 사용하여 사진 및 비디오 캡처

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


이 문서에서는 CameraCaptureUI 클래스를 사용하여 Windows에 기본 제공된 카메라 UI로 사진 또는 동영상을 캡처하는 방법을 설명합니다. 이 기능은 사용하기 쉬우며 몇 줄의 코드로 앱이 사용자가 캡처한 사진 또는 동영상을 가져올 수 있도록 합니다.

시나리오가 캡처 작업에 대해 좀 더 강력한 하위 수준의 제어를 요구하는 경우 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 개체를 사용하고 자체 캡처 환경을 구현해야 합니다. 자세한 내용은 [MediaCapture를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-mediacapture.md)를 참조하세요.

## CameraCaptureUI로 사진 캡처

카메라 캡처 UI를 사용하려면 프로젝트에 [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) 네임스페이스를 포함합니다. 반환된 이미지 파일을 사용하여 파일 작업을 수행하려면 [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346)를 포함합니다.

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

사진을 캡처하려면 새 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 개체를 만듭니다. 개체의 [**PhotoSettings**](https://msdn.microsoft.com/library/windows/apps/br241058) 속성을 사용하여 사진의 이미지 형식과 같은 반환된 사진에 대한 속성을 지정할 수 있습니다. 기본적으로 이 카메라 캡처 UI는 [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042) 속성을 사용하여 해제할 수 있지만 반환되기 전에 사진을 자르는 데 사용할 수 있습니다. 이 예제에서는 [**CroppedSizeInPixels**](https://msdn.microsoft.com/library/windows/apps/br241044)를 설정하여 반환된 이미지가 200x200 픽셀 단위로 되도록 요청합니다.

**참고** CameraCaptureUI의 이미징 자르기는 모바일 디바이스 패밀리의 디바이스에 대해 지원되지 않습니다. [
            **AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042) 속성 값은 앱이 이러한 장치에서 실행되는 경우 무시됩니다.

[
            **CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057)를 호출하고 [**CameraCaptureUIMode.Photo**](https://msdn.microsoft.com/library/windows/apps/br241040)를 지정하여 사진을 캡처하도록 지정할 수 있습니다. 이 메서드는 캡처가 성공적으로 수행되는 경우 이미지를 포함하는 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 인스턴스를 반환합니다. 사용자가 캡처를 취소하면 반환되는 개체는 null입니다.

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

캡처된 사진이 들어 있는 **StorageFile**이 있으면 여러 다른 유니버설 Windows 앱 기능에서 사용할 수 있는 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 개체를 만들 수 있습니다.

먼저 프로젝트에 [**Windows.Graphics.Imaging**](https://msdn.microsoft.com/library/windows/apps/br226400) 네임스페이스를 포함해야 합니다.

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

[
            **OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116)를 호출하여 이미지 파일에서 스트림을 가져옵니다. 그런 후 [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226182)를 호출하여 스트림에 대한 비트맵 디코더를 가져옵니다. 그런 다음 [**GetSoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887332)을 호출하여 이미지의 **SoftwareBitmap** 표현을 가져옵니다.

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

UI에 이미지를 표시하려면 XAML 페이지에서 [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) 컨트롤을 선언합니다.

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

XAML 페이지에서 소프트웨어 비트맵을 사용하려면 프로젝트에 using [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258)네임스페이스를 포함합니다.

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

**Image** 컨트롤을 사용하려면 이미지 소스가 사전 곱셈된 알파가 있거나 알파가 없는 BGRA8 형식이어야 합니다. 따라서 정적 메서드 [**SoftwareBitmap.Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362)를 호출하여 원하는 형식의 새 소프트웨어 비트맵을 만듭니다. 다음에는 새 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) 개체를 만들고 [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856)를 호출하여 소프트웨어 비트맵을 소스에 할당합니다. 마지막으로 **Image** 컨트롤의 [**Source**](https://msdn.microsoft.com/library/windows/apps/br242760) 속성을 설정하여 캡처한 사진을 UI에 표시합니다.

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## CameraCaptureUI로 비디오 캡처

비디오를 캡처하려면 새 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 개체를 만듭니다. 개체의 [**VideoSettings**](https://msdn.microsoft.com/library/windows/apps/br241059) 속성을 사용하여 비디오의 형식과 같은 반환된 비디오에 대한 속성을 지정할 수 있습니다.

[
            **CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057)를 호출하고 [**Video**](https://msdn.microsoft.com/library/windows/apps/br241059)를 지정하여 비디오를 캡처하도록 지정할 수 있습니다. 이 메서드는 캡처가 성공적으로 수행되는 경우 비디오를 포함하는 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 인스턴스를 반환합니다. 사용자가 캡처를 취소하면 반환되는 개체는 null입니다.

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

캡처한 비디오 파일로 수행하는 작업은 앱에 대한 시나리오에 따라 달라집니다. 이 문서의 나머지 부분에서는 하나 이상의 캡처된 비디오에서 미디어 컴퍼지션을 신속하게 만든 후 UI에 표시하는 방법을 보여 줍니다.

먼저 비디오 컴퍼지션을 표시할 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 컨트롤을 XAML 페이지에 추가합니다.

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]

프로젝트에 [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565) 및 [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) 네임스페이스를 추가합니다.


[!code-cs[UsingMediaComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingMediaComposition)]

페이지의 수명 범위 내에 유지하려는 [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn282716) 및 [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) 개체에 대해 멤버 변수를 선언합니다.

[!code-cs[DeclareMediaComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

일단 모든 비디오를 캡처하기 전에 **MediaComposition** 클래스의 새 인스턴스를 만들어야 합니다.

[!code-cs[InitComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetInitComposition)]

카메라 캡처 UI에서 반환된 동영상 파일을 사용하고 [**MediaClip.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607)를 호출하여 새 [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596)을 만듭니다. 미디어 클립을 컴퍼지션의 [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648) 컬렉션에 추가합니다.

[
            **GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674)를 호출하여 컴퍼지션에서 **MediaStreamSource** 개체를 만듭니다.

[!code-cs[AddToComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetAddToComposition)]

마지막으로, 스트림 소스를 미디어 요소의 [**SetMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn299029) 메서드를 사용하도록 설정하여 컴퍼지션을 UI에 표시합니다.

[!code-cs[SetMediaElementSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetMediaElementSource)]

비디오 클립을 캡처한 후 컴퍼지션에 계속 추가할 수 있습니다. 미디어 컴퍼지션에 대한 자세한 내용은 [미디어 컴퍼지션 및 편집](media-compositions-and-editing.md)을 참조하세요.

**참고**  
이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.

 

## 관련 항목

* [MediaCapture를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-mediacapture.md)
* [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030)
 

 






<!--HONumber=Mar16_HO1-->


