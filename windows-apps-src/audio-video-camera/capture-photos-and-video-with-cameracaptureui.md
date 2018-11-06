---
author: drewbatgit
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: 이 문서에서는 CameraCaptureUI 클래스를 사용하여 Windows에 기본 제공된 카메라 UI로 사진 또는 동영상을 캡처하는 방법을 설명합니다.
title: Windows 기본 제공 카메라 UI를 사용하여 사진 및 비디오 캡처
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fd9a347904b83db4bb927a36e466153a9e24d8de
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6047142"
---
# <a name="capture-photos-and-video-with-windows-built-in-camera-ui"></a>Windows 기본 제공 카메라 UI를 사용하여 사진 및 비디오 캡처



이 문서에서는 CameraCaptureUI 클래스를 사용하여 Windows에 기본 제공된 카메라 UI로 사진 또는 동영상을 캡처하는 방법을 설명합니다. 이 기능은 사용하기 쉬우며 몇 줄의 코드로 앱이 사용자가 캡처한 사진 또는 동영상을 가져올 수 있도록 합니다.

자체 카메라 UI를 제공하려고 하거나 시나리오가 캡처 작업에 대해 좀 더 강력한 하위 수준의 제어를 요구하는 경우 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 개체를 사용하고 자체 캡처 환경을 구현해야 합니다. 자세한 내용은 [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)를 참조하세요.

> [!NOTE]
> 하지 앱만 CameraCaptureUI를 사용 하는 경우 앱 매니페스트 파일에 **웹캠** 또는 **마이크** 기능을 지정 해야 합니다. 그러면 디바이스의 카메라 개인 정보 설정에 앱이 표시되지만 사용자가 앱에 대한 카메라 액세스를 거부하더라도 CameraCaptureUI의 미디어 캡처가 차단되지 않습니다. Windows 기본 제공 카메라 앱은 사용자가 단추를 눌러서 사진, 오디오 및 비디오 캡처를 시작해야 하는 신뢰할 수 있는 자사 앱이기 때문입니다. 앱에만 사진 캡처 메커니즘으로 CameraCaptureUI 사용 시 웹캠 또는 마이크 기능을 지정 하는 경우 스토어에 제출 하는 경우 Windows 응용 프로그램 인증 키트 인증을 실패할 수 있습니다.
> MediaCapture를 사용하여 오디오, 사진 또는 비디오를 프로그래밍 방식으로 캡처하는 경우 앱 매니페스트 파일에 웹캠 또는 마이크 기능을 지정해야 합니다.

## <a name="capture-a-photo-with-cameracaptureui"></a>CameraCaptureUI로 사진 캡처

카메라 캡처 UI를 사용하려면 프로젝트에 [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) 네임스페이스를 포함합니다. 반환된 이미지 파일을 사용하여 파일 작업을 수행하려면 [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346)를 포함합니다.

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

사진을 캡처하려면 새 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 개체를 만듭니다. 개체의 [**PhotoSettings**](https://msdn.microsoft.com/library/windows/apps/br241058) 속성을 사용하여 사진의 이미지 형식과 같은 반환된 사진에 대한 속성을 지정할 수 있습니다. 기본적으로 이 카메라 캡처 UI는 [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042) 속성을 사용하여 해제할 수 있지만 반환되기 전에 사진을 자르는 데 사용할 수 있습니다. 이 예제에서는 [**CroppedSizeInPixels**](https://msdn.microsoft.com/library/windows/apps/br241044)를 설정하여 반환된 이미지가 200x200 픽셀 단위로 되도록 요청합니다.

> [!NOTE]
> **CameraCaptureUI**의 이미징 자르기는 모바일 디바이스 패밀리의 디바이스에 대해 지원되지 않습니다. [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042) 속성 값은 앱이 이러한 디바이스에서 실행되는 경우 무시됩니다.

[**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057)를 호출하고 [**CameraCaptureUIMode.Photo**](https://msdn.microsoft.com/library/windows/apps/br241040)를 지정하여 사진을 캡처하도록 지정할 수 있습니다. 이 메서드는 캡처가 성공적으로 수행되는 경우 이미지를 포함하는 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 인스턴스를 반환합니다. 사용자가 캡처를 취소하면 반환되는 개체는 null입니다.

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

캡처한 사진을 포함하고 있는 **StorageFile**은 동적으로 생성된 이름이 지정되고 앱의 로컬 폴더에 저장됩니다. 캡처한 사진을 더 잘 구성하기 위해 해당 파일을 다른 폴더로 이동하는 것이 좋습니다.

[!code-cs[CopyAndDeletePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCopyAndDeletePhoto)]

앱에서 사진을 사용하려면 여러 다양한 유니버설 Windows 앱 기능에 사용할 수 있는 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 개체를 만드는 것이 좋습니다.

먼저 프로젝트에 [**Windows.Graphics.Imaging**](https://msdn.microsoft.com/library/windows/apps/br226400) 네임스페이스를 포함해야 합니다.

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

[**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116)를 호출하여 이미지 파일에서 스트림을 가져옵니다. 그런 후 [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226182)를 호출하여 스트림에 대한 비트맵 디코더를 가져옵니다. 그런 다음 [**GetSoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887332)을 호출하여 이미지의 **SoftwareBitmap** 표현을 가져옵니다.

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

UI에 이미지를 표시하려면 XAML 페이지에서 [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) 컨트롤을 선언합니다.

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

XAML 페이지에서 소프트웨어 비트맵을 사용하려면 프로젝트에 [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258)네임스페이스 사용을 포함합니다.

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

**Image** 컨트롤을 사용하려면 이미지 소스가 사전 곱셈된 알파가 있거나 알파가 없는 BGRA8 형식이어야 합니다. 따라서 정적 메서드 [**SoftwareBitmap.Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362)를 호출하여 원하는 형식의 새 소프트웨어 비트맵을 만듭니다. 다음에는 새 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) 개체를 만들고 [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856)를 호출하여 소프트웨어 비트맵을 소스에 할당합니다. 마지막으로 **Image** 컨트롤의 [**Source**](https://msdn.microsoft.com/library/windows/apps/br242760) 속성을 설정하여 캡처한 사진을 UI에 표시합니다.

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## <a name="capture-a-video-with-cameracaptureui"></a>CameraCaptureUI로 비디오 캡처

비디오를 캡처하려면 새 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 개체를 만듭니다. 개체의 [**VideoSettings**](https://msdn.microsoft.com/library/windows/apps/br241059) 속성을 사용하여 비디오의 형식과 같은 반환된 비디오에 대한 속성을 지정할 수 있습니다.

[**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057)를 호출하고 [**Video**](https://msdn.microsoft.com/library/windows/apps/br241059)를 지정하여 비디오를 캡처하도록 지정할 수 있습니다. 이 메서드는 캡처가 성공적으로 수행되는 경우 비디오를 포함하는 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 인스턴스를 반환합니다. 사용자가 캡처를 취소하면 반환되는 개체는 null입니다.

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

캡처한 비디오 파일로 수행하는 작업은 앱에 대한 시나리오에 따라 달라집니다. 이 문서의 나머지 부분에서는 하나 이상의 캡처된 비디오에서 미디어 컴퍼지션을 신속하게 만든 후 UI에 표시하는 방법을 보여 줍니다.

먼저 비디오 컴퍼지션을 표시할 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 컨트롤을 XAML 페이지에 추가합니다.

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]


카메라 캡처 UI에서 반환된 동영상 파일을 사용하고 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)를 호출하여 새 **[CreateFromStorageFile](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile)** 을 만듭니다. **MediaPlayerElement**와 연결된 기본 **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** 의 **[Play](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play)** 메서드를 호출하여 비디오를 재생합니다.

[!code-cs[PlayVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetPlayVideo)]
 

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 
 

 




