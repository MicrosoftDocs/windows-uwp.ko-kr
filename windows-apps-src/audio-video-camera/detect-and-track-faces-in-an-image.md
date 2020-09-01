---
ms.assetid: 84729E44-10E9-4D7D-8575-6A9D97467ECD
description: 이 항목에서는 FaceDetector를 사용 하 여 이미지에서 얼굴을 감지 하는 방법을 보여 줍니다. FaceTracker는 비디오 프레임 시퀀스에서 시간 경과에 따라 얼굴을 추적 하는 데 최적화 되어 있습니다.
title: 이미지 또는 동영상에서 얼굴 감지
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b6b2cf937646f41a2e51e6a109d272965817665f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160887"
---
# <a name="detect-faces-in-images-or-videos"></a>이미지 또는 동영상에서 얼굴 감지



\[일부 정보는 상업적으로 출시 되기 전에 대폭 수정 될 수 있는 미리 릴리스된 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 어떠한 명시적이거나 묵시적인 보증도 하지 않습니다.\]

이 항목에서는 [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) 를 사용 하 여 이미지에서 얼굴을 감지 하는 방법을 보여 줍니다. [**FaceTracker**](/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) 는 비디오 프레임 시퀀스에서 시간 경과에 따라 얼굴을 추적 하는 데 최적화 되어 있습니다.

[**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect)를 사용 하 여 얼굴을 추적 하는 다른 방법은 [미디어 캡처에 대 한 장면 분석](scene-analysis-for-media-capture.md)을 참조 하세요.

이 문서의 코드는 [기본 얼굴 감지](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceDetection) 및 [기본 얼굴 추적](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceTracking) 샘플에서 도입 되었습니다. 이러한 샘플을 다운로드 하 여 컨텍스트에서 사용 되는 코드를 확인 하거나 사용자 고유의 앱에 대 한 시작 지점으로 샘플을 사용할 수 있습니다.

## <a name="detect-faces-in-a-single-image"></a>단일 이미지에서 얼굴 감지

[**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) 클래스를 사용 하 여 스틸 이미지에서 하나 이상의 얼굴을 검색할 수 있습니다.

이 예제에서는 다음 네임 스페이스의 Api를 사용 합니다.

[!code-cs[FaceDetectionUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceDetectionUsing)]

[**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) 개체 및 이미지에서 검색 되는 [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace) 개체의 목록에 대 한 클래스 멤버 변수를 선언 합니다.

[!code-cs[ClassVariables1](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables1)]

얼굴 검색은 다양 한 방식으로 만들 수 있는 [**\Bitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 개체에서 작동 합니다. 이 예제에서는 [**Fileopenpicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 를 사용 하 여 사용자가 얼굴을 검색할 이미지 파일을 선택할 수 있게 합니다. 소프트웨어 비트맵을 사용 하는 방법에 대 한 자세한 내용은 [Imaging](imaging.md)을 참조 하세요.

[!code-cs[Picker](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetPicker)]

[**Bitmapdecoder에서**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) 클래스를 사용 하 여 이미지 파일을 파일 **비트맵**으로 디코딩합니다. 얼굴 검색 프로세스는 더 작은 이미지를 사용 하는 속도가 더 빠르지만 원본 이미지를 더 작은 크기로 크기를 조정 하는 것이 좋습니다. 이 작업은 [**BitmapTransform**](/uwp/api/Windows.Graphics.Imaging.BitmapTransform) 개체를 만들고 [**ScaledWidth**](/uwp/api/windows.graphics.imaging.bitmaptransform.scaledwidth) 및 [**ScaledHeight**](/uwp/api/windows.graphics.imaging.bitmaptransform.scaledheight) 속성을 설정한 다음 [**GetSoftwareBitmapAsync**](/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync)에 대 한 호출에 전달 하 여 디코딩 중에 수행할 수 있습니다. 그러면 디코딩된 및 배율이 조정 된 개체 **비트맵이**반환 됩니다.

[!code-cs[Decode](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDecode)]

현재 버전에서 **FaceDetector** 클래스는 Gray8 또는 Nv12의 이미지만 지원 합니다. **\Bitmap** 클래스는 비트맵을 한 형식에서 다른 형식으로 변환 하는 [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) 메서드를 제공 합니다. 이 예제에서는 원본 이미지를 Gray8 픽셀 형식으로 변환 합니다 (해당 형식이 아직 없는 경우). 필요에 따라 [**GetSupportedBitmapPixelFormats**](/uwp/api/windows.media.faceanalysis.facedetector.getsupportedbitmappixelformats) 및 [**IsBitmapPixelFormatSupported**](/uwp/api/windows.media.faceanalysis.facedetector.isbitmappixelformatsupported) 메서드를 사용 하 여 지원 되는 형식 집합이 이후 버전에서 확장 되는 경우에 픽셀 형식이 지원 되는지 여부를 결정할 수 있습니다.

[!code-cs[Format](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFormat)]

[**Createasync**](/uwp/api/windows.media.faceanalysis.facedetector.createasync) 를 호출 하 고 [**DetectFacesAsync**](/uwp/api/windows.media.faceanalysis.facedetector.detectfacesasync)를 호출 하 여 적절 한 크기로 확장 되 고 지원 되는 픽셀 형식으로 변환 된 비트맵을 전달 하 여 **FaceDetector** 개체를 인스턴스화합니다. 이 메서드는 [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace) 개체의 목록을 반환 합니다. **ShowDetectedFaces** 는 아래에 표시 된 도우미 메서드로, 이미지의 얼굴 주위에 사각형을 그립니다.

[!code-cs[Detect](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDetect)]

얼굴 검색 프로세스 중에 생성 된 개체를 삭제 해야 합니다.

[!code-cs[Dispose](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDispose)]

검색 된 얼굴 주위에 이미지를 표시 하 고 상자를 그리려면 XAML 페이지에 [**캔버스**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) 요소를 추가 합니다.

[!code-xml[Canvas](./code/FaceDetection_Win10/cs/MainPage.xaml#SnippetCanvas)]

일부 멤버 변수를 정의 하 여 그릴 사각형의 스타일을 지정 합니다.

[!code-cs[ClassVariables2](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables2)]

**ShowDetectedFaces** helper 메서드에서는 새 ImageBrush를 만들고 소스가 원본 이미지를 나타내는 [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) **비트맵** 에서 만든 [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) 로 설정 됩니다. XAML **Canvas** 컨트롤의 배경은 이미지 브러시로 설정 됩니다.

도우미 메서드에 전달 된 얼굴 목록이 비어 있지 않은 경우 목록에서 각 얼굴을 반복 하 고 [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace) 클래스의 [**FaceBox**](/uwp/api/windows.media.faceanalysis.detectedface.facebox) 속성을 사용 하 여 얼굴을 포함 하는 이미지 내에서 사각형의 위치와 크기를 확인 합니다. **Canvas** 컨트롤이 원본 이미지와 크기가 다를 수 있기 때문에 X 및 Y 좌표와 **FaceBox** 의 너비와 높이를 모두 **캔버스** 컨트롤의 실제 크기에 대 한 크기 조정 값으로 곱합니다.

[!code-cs[ShowDetectedFaces](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetShowDetectedFaces)]

## <a name="track-faces-in-a-sequence-of-frames"></a>프레임 시퀀스의 얼굴 추적

비디오에서 얼굴을 감지 하려면 구현 단계는 매우 유사 하지만 [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) 클래스 대신 [**FaceTracker**](/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) 클래스를 사용 하는 것이 더 효율적입니다. **FaceTracker** 는 이전에 처리 된 프레임에 대 한 정보를 사용 하 여 검색 프로세스를 최적화 합니다.

[!code-cs[FaceTrackingUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceTrackingUsing)]

**FaceTracker** 개체에 대 한 클래스 변수를 선언 합니다. 이 예에서는 [**windows.system.threading.threadpooltimer**](/uwp/api/Windows.System.Threading.ThreadPoolTimer) 를 사용 하 여 정의 된 간격으로 얼굴 추적을 시작 합니다. [SemaphoreSlim](/dotnet/api/system.threading.semaphoreslim) 는 한 번에 하나의 얼굴 추적 작업만 실행 되는지 확인 하는 데 사용 됩니다.

[!code-cs[ClassVariables3](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables3)]

얼굴 추적 작업을 초기화 하려면 [**Createasync**](/uwp/api/windows.media.faceanalysis.facetracker.createasync)를 호출 하 여 새 **FaceTracker** 개체를 만듭니다. 원하는 타이머 간격을 초기화 하 고 타이머를 만듭니다. **ProcessCurrentVideoFrame** 도우미 메서드는 지정 된 간격이 경과할 때마다 호출 됩니다.

[!code-cs[TrackingInit](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetTrackingInit)]

**ProcessCurrentVideoFrame** 도우미는 타이머에 의해 비동기적으로 호출되므로 메서드는 먼저 세마포의 **Wait** 메서드를 호출하여 추적 작업이 진행 중인지 확인하고 그런 경우 메서드는 얼굴을 감지하지 않고 반환됩니다. 이 메서드가 끝날 때 세마포의 **릴리스** 메서드가 호출 되어 **ProcessCurrentVideoFrame** 에 대 한 후속 호출을 계속할 수 있습니다.

[**FaceTracker**](/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) 클래스는 [**videoframe**](/uwp/api/Windows.Media.VideoFrame) 개체에서 작동 합니다. 여러 가지 방법으로 실행 중인 [MediaCapture](./index.md) 개체에서 미리 보기 프레임을 캡처하거나 [**Ibasicvideoeffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect)의 [**processframe**](/uwp/api/windows.media.effects.ibasicaudioeffect.processframe) 메서드를 구현 하 여 **비디오 프레임** 을 가져올 수 있습니다. 이 예제에서는 비디오 프레임을 반환하는 정의되지 않은 도우미 메서드 **GetLatestFrame**을 이 작업에 대한 자리 표시자로 사용합니다. 실행 중인 미디어 캡처 장치의 미리 보기 스트림에서 비디오 프레임을 가져오는 방법에 대 한 자세한 내용은 [미리 보기 프레임 가져오기](get-a-preview-frame.md)를 참조 하세요.

**FaceDetector**와 마찬가지로 **FaceTracker** 는 제한 된 픽셀 형식 집합을 지원 합니다. 이 예에서는 제공 된 프레임이 Nv12 형식이 아닌 경우 얼굴 검색을 무시 합니다.

[**ProcessNextFrameAsync**](/uwp/api/windows.media.faceanalysis.facetracker.processnextframeasync) 를 호출 하 여 프레임의 얼굴을 나타내는 [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace) 개체의 목록을 검색 합니다. 얼굴 목록이 표시 된 후 얼굴 감지를 위해 위에서 설명한 것과 동일한 방식으로 표시 될 수 있습니다. 얼굴 추적 도우미 메서드는 UI 스레드에서 호출 되지 않으므로 [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher.runasync)호출 내에서 ui 업데이트를 수행 해야 합니다.

[!code-cs[ProcessCurrentVideoFrame](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetProcessCurrentVideoFrame)]

## <a name="related-topics"></a>관련 항목

* [미디어 캡처의 장면 분석](scene-analysis-for-media-capture.md)
* [Basic 얼굴 감지 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceDetection)
* [기본 얼굴 추적 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceTracking)
* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [미디어 재생](media-playback.md)