---
ms.assetid: 84729E44-10E9-4D7D-8575-6A9D97467ECD
description: 이 항목에서는 FaceDetector를 사용하여 이미지로 얼굴을 감지하는 방법을 보여 줍니다. FaceTracker는 비디오 프레임의 시퀀스에서 시간에 따라 얼굴을 추적하도록 최적화되어 있습니다.
title: 이미지 또는 동영상에서 얼굴 감지
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 970fbcef984f28e779ea7c133de95f7f7f99be8d
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8214484"
---
# <a name="detect-faces-in-images-or-videos"></a>이미지 또는 동영상에서 얼굴 감지



\[일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.\]

이 항목에서는 [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129)를 사용하여 이미지로 얼굴을 감지하는 방법을 보여 줍니다. [**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150)는 비디오 프레임의 시퀀스에서 시간에 따라 얼굴을 추적하도록 최적화되어 있습니다.

[**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/dn948776)를 사용하여 얼굴을 추적하는 다른 방법은 [미디어 캡처의 장면 분석](scene-analysis-for-media-capture.md)을 참조하세요.

이 문서의 코드는 [기본 얼굴 감지](http://go.microsoft.com/fwlink/p/?LinkId=620512&clcid=0x409) 및 [기본 얼굴 추적](http://go.microsoft.com/fwlink/p/?LinkId=620513&clcid=0x409) 샘플에서 가져왔습니다. 이러한 샘플을 다운로드하여 코드가 사용되는 컨텍스트를 확인하거나 직접 앱을 만들 때 샘플을 기준으로 삼을 수 있습니다.

## <a name="detect-faces-in-a-single-image"></a>이미지 하나로 얼굴 감지

[**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) 클래스를 사용하면 정지 이미지에서 하나 이상의 얼굴을 감지할 수 있습니다.

이 예제에서는 다음 네임스페이스에서 API를 사용합니다.

[!code-cs[FaceDetectionUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceDetectionUsing)]

이미지에서 감지되는 [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) 개체 및 [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) 개체의 목록에 대한 클래스 멤버 변수를 선언합니다.

[!code-cs[ClassVariables1](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables1)]

얼굴 감지는 다양한 방법으로 만들 수 있는 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 개체에서 작동합니다. 이 예제에서는 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)를 사용하여 사용자가 얼굴을 감지할 이미지 파일을 선택할 수 있도록 합니다. 소프트웨어 비트맵 사용에 대한 자세한 내용은 [이미징](imaging.md)을 참조하세요.

[!code-cs[Picker](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetPicker)]

[**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) 클래스를 사용하면 이미지 파일을 **SoftwareBitmap**으로 디코드할 수 있습니다. 이미지가 작으면 얼굴 감지 프로세스가 더 빨라지므로 원본 이미지를 더 작은 크기로 축소할 수 있습니다. 이를 위해 디코딩 동안 [**BitmapTransform**](https://msdn.microsoft.com/library/windows/apps/br226254) 개체를 만들고 [**ScaledWidth**](https://msdn.microsoft.com/library/windows/apps/br226261) 및 [**ScaledHeight**](https://msdn.microsoft.com/library/windows/apps/br226260) 속성을 설정하여 [**GetSoftwareBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn887332) 호출에 전달합니다. 그러면 디코드되고 크기 조정된 **SoftwareBitmap**이 반환됩니다.

[!code-cs[Decode](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDecode)]

현재 버전에서 **FaceDetector** 클래스는 Gray8 또는 Nv12의 이미지만 지원합니다. **SoftwareBitmap** 클래스는 비트맵의 형식을 변환하는 [**Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362) 메서드를 제공합니다. 이 예제에서는 원본 이미지가 Gray8 픽셀 형식이 아닌 경우 이 형식으로 변환합니다. 지원되는 형식 집합이 이후 버전에서 확장되는 경우를 위해 원하는 경우 [**GetSupportedBitmapPixelFormats**](https://msdn.microsoft.com/library/windows/apps/dn974140) 및 [**IsBitmapPixelFormatSupported**](https://msdn.microsoft.com/library/windows/apps/dn974142) 메서드를 사용하여 런타임에 지원되는 픽셀 형식인지 확인할 수 있습니다.

[!code-cs[Format](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFormat)]

[**CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn974132)를 호출하여 **FaceDetector** 개체를 인스턴스화한 다음 [**DetectFacesAsync**](https://msdn.microsoft.com/library/windows/apps/dn974134)를 호출하여 적절한 크기로 조정되고 지원되는 픽셀 형식으로 변환된 비트맵을 전달합니다. 이 메서드는 [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) 개체의 목록을 반환합니다. **ShowDetectedFaces**는 아래에 표시된 대로 이미지의 얼굴 주위에 사각형을 그리는 도우미 메서드입니다.

[!code-cs[Detect](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDetect)]

얼굴 감지 프로세스 중 생성된 개체를 삭제해야 합니다.

[!code-cs[Dispose](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDispose)]

이미지를 표시하고 감지된 얼굴 주위에 상자를 그리려면 XAML 페이지에 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 요소를 추가합니다.

[!code-xml[Canvas](./code/FaceDetection_Win10/cs/MainPage.xaml#SnippetCanvas)]

그리는 사각형의 스타일을 지정하려면 몇 가지 멤버 변수를 정의합니다.

[!code-cs[ClassVariables2](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables2)]

**ShowDetectedFaces** 도우미 메서드에서 새 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101)가 생성되고 원본이 원본 이미지를 나타내는 **SoftwareBitmap**에서 생성된 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854)로 설정됩니다. XAML **Canvas** 컨트롤의 배경이 이미지 브러시로 설정됩니다.

도우미 메서드로 전달된 얼굴 목록이 비어 있지 않은 경우 목록의 각 얼굴을 반복하고 [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123)의 [**FaceBox**](https://msdn.microsoft.com/library/windows/apps/dn974126) 속성을 사용하여 얼굴이 포함된 이미지 내에 있는 사각형의 위치와 크기를 결정합니다. **Canvas** 컨트롤은 원본 이미지와 다른 크기일 가능성이 크므로 **FaceBox**의 X 및 Y 좌표와 너비 및 높이 모두에 원본 이미지 크기와 **Canvas** 컨트롤 실제 크기의 비율인 배율 값을 곱해야 합니다.

[!code-cs[ShowDetectedFaces](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetShowDetectedFaces)]

## <a name="track-faces-in-a-sequence-of-frames"></a>프레임 시퀀스에서 얼굴 추적

동영상에서 얼굴을 감지하려면 구현 단계는 아주 유사하지만 [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) 클래스보다 [**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) 클래스를 사용하는 것이 더 효율적입니다. **FaceTracker**는 이전에 처리된 프레임에 대한 정보를 사용하여 감지 프로세스를 최적화합니다.

[!code-cs[FaceTrackingUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceTrackingUsing)]

**FaceTracker** 개체에 대한 클래스 변수를 선언합니다. 이 예제에서는 [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/br230587)를 사용하여 정의된 간격으로 얼굴 추적을 시작합니다. [SemaphoreSlim](https://msdn.microsoft.com/library/system.threading.semaphoreslim.aspx)은 얼굴 추적 작업이 한 번에 하나만 실행 중인지 확인하는 데 사용됩니다.

[!code-cs[ClassVariables3](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables3)]

얼굴 추적 작업을 초기화하려면 [**CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn974151)를 호출하여 새 **FaceTracker** 개체를 만듭니다. 원하는 타이머 간격을 초기화한 다음 타이머를 만듭니다. **ProcessCurrentVideoFrame** 도우미 메서드는 지정된 간격이 경과할 때마다 호출됩니다.

[!code-cs[TrackingInit](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetTrackingInit)]

**ProcessCurrentVideoFrame** 도우미는 타이머에 의해 비동기적으로 호출되므로 메서드는 먼저 세마포의 **Wait** 메서드를 호출하여 추적 작업이 진행 중인지 확인하고 그런 경우 메서드는 얼굴을 감지하지 않고 반환됩니다. 이 메서드의 끝 부분에 세마포의 **Release** 메서드가 호출되므로 이후 **ProcessCurrentVideoFrame**을 계속 호출할 수 있습니다.

[**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) 클래스는 [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) 개체에서 작동합니다. **VideoFrame**을 가져올 수 있는 방법은 실행 중인 [MediaCapture](capture-photos-and-video-with-mediacapture.md) 개체에서 미리 보기 프레임을 캡처하거나 [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788)의 [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764784) 메서드를 구현하는 것을 비롯해 여러 가지가 있습니다. 이 예제에서는 비디오 프레임을 반환하는 정의되지 않은 도우미 메서드 **GetLatestFrame**을 이 작업에 대한 자리 표시자로 사용합니다. 실행 중인 미디어 캡처 디바이스의 미리 보기 스트림에서 비디오 프레임을 가져오는 방법에 대한 정보는 [미리 보기 프레임 가져오기](get-a-preview-frame.md)를 참조하세요.

**FaceDetector**와 마찬가지로 **FaceTracker**는 제한된 픽셀 형식 집합을 지원합니다. 이 예제에서는 제공된 프레임이 Nv12 형식이 아닌 경우 얼굴 감지를 중단합니다.

[**ProcessNextFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn974157)을 호출하여 프레임에 있는 얼굴을 나타내는 [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) 개체의 목록을 검색합니다. 얼굴 목록이 있으면 얼굴 감지를 위해 위에 설명된 동일한 방식으로 얼굴을 표시할 수 있습니다. 얼굴 추적 도우미 메서드는 UI 스레드에서 호출되지 않으므로 모든 UI 업데이트는 [**CoredDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 호출 내에서 해야 합니다.

[!code-cs[ProcessCurrentVideoFrame](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetProcessCurrentVideoFrame)]

## <a name="related-topics"></a>관련 항목

* [미디어 캡처의 장면 분석](scene-analysis-for-media-capture.md)
* [기본 얼굴 감지 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620512&clcid=0x409)
* [기본 얼굴 추적 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620513&clcid=0x409)
* [Camera](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [미디어 재생](media-playback.md)
