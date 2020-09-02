---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: 이 문서에서는 Windows에 기본 제공 되는 카메라 UI를 사용 하 여 사진이 나 비디오를 캡처하는 [**CameraCaptureUI**](/uwp/api/windows.media.capture.cameracaptureui) 클래스를 사용 하는 방법을 설명 합니다.
title: Windows 기본 카메라 UI를 사용하여 사진 및 비디오 캡처
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 5b5e1369e37fc683a3a09c8f404b1998ee06bdab
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364006"
---
# <a name="capture-photos-and-video-with-the-windows-built-in-camera-ui"></a>Windows 기본 카메라 UI를 사용하여 사진 및 비디오 캡처

이 문서에서는 Windows에 기본 제공 되는 카메라 UI를 사용 하 여 사진이 나 비디오를 캡처하는 [**CameraCaptureUI**](/uwp/api/windows.media.capture.cameracaptureui) 클래스를 사용 하는 방법을 설명 합니다. 이 기능을 사용 하는 것이 쉽습니다. 이를 통해 앱은 코드를 몇 줄만 사용 하 여 사용자가 캡처한 사진 또는 비디오를 가져올 수 있습니다.

사용자 고유의 카메라 UI를 제공 하거나 시나리오에서 캡처 작업에 대 한 보다 강력 하 고 낮은 수준의 제어를 요구 하는 경우 [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) 클래스를 사용 하 여 사용자 고유의 캡처 환경을 구현 해야 합니다. 자세한 내용은 [MediaCapture를 사용 하 여 기본 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)를 참조 하세요.

> [!NOTE]
> 앱에서 **CameraCaptureUI**만 사용 하는 경우 앱 매니페스트 파일에서 **웹캠** 및 **마이크** 기능을 지정 하면 안 됩니다. 이렇게 하면 앱이 장치의 카메라 개인 정보 설정에 표시 되지만 사용자가 앱에 대 한 카메라 액세스를 거부 하는 경우에도 **CameraCaptureUI** 미디어를 캡처하지 못하게 됩니다. <p>Windows 기본 제공 카메라 앱은 사용자가 단추를 눌러 사진, 오디오 및 비디오 캡처를 시작 해야 하는 신뢰할 수 있는 자사 앱 이기 때문입니다. **CameraCaptureUI** 를 유일한 사진 캡처 메커니즘으로 사용할 때 웹캠 또는 마이크 기능을 지정 하는 경우 앱에서 Windows 응용 프로그램 인증 키트 인증 Microsoft Store에 실패할 수 있습니다.<p>
**MediaCapture** 를 사용 하 여 오디오, 사진 또는 비디오를 프로그래밍 방식으로 캡처하는 경우 앱 매니페스트 파일에서 **웹캠** 또는 **마이크** 기능을 지정 해야 합니다.

## <a name="capture-a-photo-with-cameracaptureui"></a>CameraCaptureUI를 사용 하 여 사진 캡처

카메라 캡처 UI를 사용하려면 프로젝트에 [**Windows.Media.Capture**](/uwp/api/Windows.Media.Capture) 네임스페이스를 포함합니다. 반환 된 이미지 파일을 사용 하 여 파일 작업을 수행 하려면 [**Windows 저장소**](/uwp/api/Windows.Storage)를 포함 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingCaptureUI":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingCaptureUI":::

사진을 캡처하려면 새 [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) 개체를 만듭니다. 개체의 사진 [**설정**](/uwp/api/windows.media.capture.cameracaptureui.photosettings) 속성을 사용 하 여 사진의 이미지 형식과 같은 반환 된 사진의 속성을 지정할 수 있습니다. 기본적으로 카메라 캡처 UI는 반환 되기 전에 사진 잘라내기를 지원 합니다. 이는 [**Allowcropping**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) 속성으로 사용 하지 않도록 설정할 수 있습니다. 이 예제에서는 반환 된 이미지를 200 x 200 (픽셀)로 요청 하도록 [**CroppedSizeInPixels**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.croppedsizeinpixels) 를 설정 합니다.

> [!NOTE]
> 모바일 장치 제품군의 장치에 대해서는 **CameraCaptureUI** 의 이미지 자르기가 지원 되지 않습니다. 이러한 장치에서 앱이 실행 되는 경우 [**Allowcropping**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) 속성의 값이 무시 됩니다.

[**CaptureFileAsync**](/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) 를 호출 하 고 [**CameraCaptureUIMode**](/uwp/api/Windows.Media.Capture.CameraCaptureUIMode) 를 지정 하 여 사진을 캡처해야 하도록 지정 합니다. 이 메서드는 캡처가 성공한 경우 이미지를 포함 하는 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 인스턴스를 반환 합니다. 사용자가 캡처를 취소 하면 반환 되는 개체는 null입니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCapturePhoto":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCapturePhoto":::

캡처된 사진을 포함 하는 **StorageFile** 에는 동적으로 생성 된 이름이 지정 되 고 앱의 로컬 폴더에 저장 됩니다. 캡처한 사진을 보다 잘 구성 하기 위해 파일을 다른 폴더로 이동할 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCopyAndDeletePhoto":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCopyAndDeletePhoto":::

앱에서 사진을 사용 하려면 여러 가지 유니버설 Windows 앱 기능에서 사용할 수 있는 기능 [**비트맵**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 개체를 만들 수 있습니다.

먼저 프로젝트에 [**Windows. Graphics. Imaging**](/uwp/api/Windows.Graphics.Imaging) 네임 스페이스를 포함 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingSoftwareBitmap":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingSoftwareBitmap":::

[**Openasync**](/uwp/api/windows.storage.istoragefile.openasync) 를 호출 하 여 이미지 파일에서 스트림을 가져옵니다. [**Bitmapdecoder에서**](/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) 를 호출 하 여 스트림에 대 한 비트맵 디코더를 가져옵니다. 그런 다음 [**Getsoftwarebitmap**](/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) 을 호출 하 여 이미지의 **\bitmap** 표현을 가져옵니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetSoftwareBitmap":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetSoftwareBitmap":::

UI에 이미지를 표시 하려면 XAML 페이지에서 [**이미지**](/uwp/api/Windows.UI.Xaml.Controls.Image) 컨트롤을 선언 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml" id="SnippetImageControl":::
:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.xaml" id="SnippetImageControl":::

XAML 페이지에서 소프트웨어 비트맵을 사용 하려면 프로젝트에 사용 하는 [**Windows.**](/uwp/api/Windows.UI.Xaml.Media.Imaging) x m l. x m l. x m l 네임 스페이스를 포함 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingSoftwareBitmapSource":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingSoftwareBitmapSource":::

**이미지** 컨트롤을 사용 하려면 이미지 원본이 미리 증가 된 알파를 사용 하거나 알파를 사용 하지 않는 BGRA8 형식 이어야 합니다. 정적 메서드를 호출 하 여 원하는 형식의 새 소프트웨어 비트맵을 만듭니다 [**.**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) 그런 다음 새 [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) 개체를 만들고 [**SetBitmapAsync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) 를 호출 하 여 소프트웨어 비트맵을 원본에 할당 합니다. 마지막으로, **이미지** 컨트롤의 [**Source**](/uwp/api/windows.ui.xaml.controls.image.source) 속성을 설정 하 여 캡처된 사진을 UI에 표시 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetSetImageSource":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetSetImageSource":::

## <a name="capture-a-video-with-cameracaptureui"></a>CameraCaptureUI를 사용 하 여 비디오 캡처

비디오를 캡처하려면 새 [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) 개체를 만듭니다. 개체의 [**Videosettings**](/uwp/api/windows.media.capture.cameracaptureui.videosettings) 속성을 사용 하 여 비디오 형식과 같이 반환 된 비디오에 대 한 속성을 지정할 수 있습니다.

[**CaptureFileAsync**](/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) 를 호출 하 [**고 비디오를 지정 하**](/uwp/api/windows.media.capture.cameracaptureui.videosettings) 여 비디오를 캡처합니다. 이 메서드는 캡처가 성공한 경우 비디오를 포함 하는 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 인스턴스를 반환 합니다. 캡처를 취소 하면 반환 되는 개체는 null입니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCaptureVideo":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCaptureVideo":::

캡처한 비디오 파일을 사용 하 여 수행 하는 작업은 앱의 시나리오에 따라 달라 집니다. 이 문서의 나머지 부분에서는 캡처된 비디오 하나 이상에서 미디어 컴퍼지션을 빠르게 만들고 UI에 표시 하는 방법을 보여 줍니다.

먼저 XAML 페이지에 비디오 컴포지션이 표시 되는 [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 컨트롤을 추가 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml" id="SnippetMediaElement":::

비디오 파일이 카메라 캡처 UI에서 반환 되는 경우 **[CreateFromStorageFile](/uwp/api/windows.media.core.mediasource.createfromstoragefile)** 를 호출 하 여 새 [**MediaSource**](/uwp/api/windows.media.core.mediasource) 를 만듭니다. **MediaPlayerElement** 와 연결 된 기본 **[MediaPlayer](/uwp/api/windows.media.playback.mediaplayer)** 의 **[play](/uwp/api/windows.media.playback.mediaplayer.Play)** 메서드를 호출 하 여 비디오를 재생 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetPlayVideo":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetPlayVideo":::

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [CameraCaptureUI](/uwp/api/Windows.Media.Capture.CameraCaptureUI)
