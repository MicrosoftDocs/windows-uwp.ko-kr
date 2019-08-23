---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: 이 문서에서는 Windows에 기본 제공 되는 카메라 UI를 사용 하 여 사진이 나 비디오를 캡처하는 CameraCaptureUI 클래스를 사용 하는 방법을 설명 합니다.
title: Windows 기본 제공 카메라 UI를 사용 하 여 사진 및 비디오 캡처
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d582d4815b4fb2168b187a1efff3795cc98aca02
ms.sourcegitcommit: 99595e4938213aafdb49635d684d8ba8eb3f697a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487802"
---
# <a name="capture-photos-and-video-with-the-windows-built-in-camera-ui"></a>Windows 기본 제공 카메라 UI를 사용 하 여 사진 및 비디오 캡처



이 문서에서는 Windows에 기본 제공 되는 카메라 UI를 사용 하 여 사진이 나 비디오를 캡처하는 CameraCaptureUI 클래스를 사용 하는 방법을 설명 합니다. 이 기능을 사용 하는 것이 쉽습니다. 이를 통해 앱은 코드를 몇 줄만 사용 하 여 사용자가 캡처한 사진 또는 비디오를 가져올 수 있습니다.

자체 카메라 UI를 제공하려고 하거나 시나리오가 캡처 작업에 대해 좀 더 강력한 하위 수준의 제어를 요구하는 경우 [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) 개체를 사용하고 자체 캡처 환경을 구현해야 합니다. 자세한 내용은 [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)를 참조하세요.

> [!NOTE]
> 앱에서 CameraCaptureUI만 사용 하는 경우 앱 매니페스트 파일에서 **웹캠** 또는 **마이크** 기능을 지정 하지 않아야 합니다. 이렇게 하면 앱이 장치의 카메라 개인 정보 설정에 표시 되지만 사용자가 앱에 대 한 카메라 액세스를 거부 하는 경우에도 CameraCaptureUI 미디어를 캡처하지 못하게 됩니다. <p>Windows 기본 제공 카메라 앱은 사용자가 단추를 눌러서 사진, 오디오 및 비디오 캡처를 시작해야 하는 신뢰할 수 있는 자사 앱이기 때문입니다. CameraCaptureUI를 유일한 사진 캡처 메커니즘으로 사용할 때 웹캠 또는 마이크 기능을 지정 하는 경우 앱에서 Windows 응용 프로그램 인증 키트 인증 Microsoft Store에 실패할 수 있습니다.<p>
MediaCapture를 사용 하 여 오디오, 사진 또는 비디오를 프로그래밍 방식으로 캡처하는 경우 앱 매니페스트 파일에서 웹캠 또는 마이크 기능을 지정 해야 합니다.

## <a name="capture-a-photo-with-cameracaptureui"></a>CameraCaptureUI로 사진 캡처

카메라 캡처 UI를 사용하려면 프로젝트에 [**Windows.Media.Capture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture) 네임스페이스를 포함합니다. 반환된 이미지 파일을 사용하여 파일 작업을 수행하려면 [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage)를 포함합니다.

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

사진을 캡처하려면 새 [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 개체를 만듭니다. 개체의 사진 [**설정**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.photosettings) 속성을 사용 하 여 사진의 이미지 형식과 같은 반환 된 사진의 속성을 지정할 수 있습니다. 기본적으로 카메라 캡처 UI는 반환 되기 전에 사진 잘라내기를 지원 합니다. 이는 [**Allowcropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) 속성으로 사용 하지 않도록 설정할 수 있습니다. 이 예제에서는 [**CroppedSizeInPixels**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.croppedsizeinpixels)를 설정하여 반환된 이미지가 200x200 픽셀 단위로 되도록 요청합니다.

> [!NOTE]
> 모바일 장치 제품군의 장치에 대해서는 **CameraCaptureUI** 의 이미지 자르기가 지원 되지 않습니다. [  **AllowCropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) 속성 값은 앱이 이러한 디바이스에서 실행되는 경우 무시됩니다.

[  **CaptureFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.capturefileasync)를 호출하고 [**CameraCaptureUIMode.Photo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUIMode)를 지정하여 사진을 캡처하도록 지정할 수 있습니다. 이 메서드는 캡처가 성공적으로 수행되는 경우 이미지를 포함하는 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 인스턴스를 반환합니다. 사용자가 캡처를 취소하면 반환되는 개체는 null입니다.

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

캡처한 사진을 포함하고 있는 **StorageFile**은 동적으로 생성된 이름이 지정되고 앱의 로컬 폴더에 저장됩니다. 캡처한 사진을 보다 잘 구성 하기 위해 파일을 다른 폴더로 이동할 수 있습니다.

[!code-cs[CopyAndDeletePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCopyAndDeletePhoto)]

앱에서 사진을 사용하려면 여러 다양한 유니버설 Windows 앱 기능에 사용할 수 있는 [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 개체를 만드는 것이 좋습니다.

먼저 프로젝트에 [**Windows. Graphics. Imaging**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging) 네임 스페이스를 포함 합니다.

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

[  **OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync)를 호출하여 이미지 파일에서 스트림을 가져옵니다. 그런 후 [**BitmapDecoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync)를 호출하여 스트림에 대한 비트맵 디코더를 가져옵니다. 그런 다음 [**Getsoftwarebitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) 을 호출 하 여 이미지의 **\bitmap** 표현을 가져옵니다.

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

UI에 이미지를 표시하려면 XAML 페이지에서 [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) 컨트롤을 선언합니다.

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

XAML 페이지에서 소프트웨어 비트맵을 사용하려면 프로젝트에 using [**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging)네임스페이스를 포함합니다.

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

**이미지** 컨트롤을 사용 하려면 이미지 원본이 미리 증가 된 알파를 사용 하거나 알파를 사용 하지 않는 BGRA8 형식 이어야 합니다. 정적 메서드를 호출 하 여 원하는 형식의 새 소프트웨어 비트맵을 만듭니다 [ **.** ](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) 그런 다음 새 [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) 개체를 만들고 [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) 를 호출 하 여 소프트웨어 비트맵을 원본에 할당 합니다. 마지막으로 **Image** 컨트롤의 [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) 속성을 설정하여 캡처한 사진을 UI에 표시합니다.

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## <a name="capture-a-video-with-cameracaptureui"></a>CameraCaptureUI로 비디오 캡처

비디오를 캡처하려면 새 [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 개체를 만듭니다. 개체의 [**Videosettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings) 속성을 사용 하 여 비디오 형식과 같이 반환 된 비디오에 대 한 속성을 지정할 수 있습니다.

[**CaptureFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) 를 호출 하고 [**비디오**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings)를 지정 하 여 비디오를 캡처합니다. 이 메서드는 캡처가 성공적으로 수행되는 경우 비디오를 포함하는 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 인스턴스를 반환합니다. 캡처를 취소 하면 반환 되는 개체는 null입니다.

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

캡처한 비디오 파일로 수행하는 작업은 앱에 대한 시나리오에 따라 달라집니다. 이 문서의 나머지 부분에서는 하나 이상의 캡처된 비디오에서 미디어 컴퍼지션을 신속하게 만든 후 UI에 표시하는 방법을 보여 줍니다.

먼저 XAML 페이지에 비디오 컴포지션이 표시 되는 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 컨트롤을 추가 합니다.

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]


비디오 파일이 카메라 캡처 UI에서 반환 되는 경우 **[CreateFromStorageFile](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile)** 를 호출 하 여 새 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 를 만듭니다. **MediaPlayerElement**와 연결된 기본 **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** 의 **[Play](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play)** 메서드를 호출하여 비디오를 재생합니다.

[!code-cs[PlayVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetPlayVideo)]
 

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용 하는 기본 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 
 

 




