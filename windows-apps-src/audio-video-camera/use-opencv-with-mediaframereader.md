---
ms.assetid: ''
description: 이 문서에서는 MediaFrameReader 클래스와 함께 OpenCV(오픈 소스 컴퓨터 비전 라이브러리)를 사용하는 방법을 설명합니다.
title: MediaFrameReader와 함께 OpenCV 사용
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, openCV
ms.localizationpriority: medium
ms.openlocfilehash: a603899776879cb7c8dc2439c3c22906db0b8038
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8477041"
---
# <a name="use-the-open-source-computer-vision-library-opencv-with-mediaframereader"></a>MediaFrameReader와 OpenCV(오픈 소스 컴퓨터 비전 라이브러리) 사용

이 문서에서는 여러 소스에서 미디어 프레임을 동시에 읽을 수 있는 [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) 클래스와 함께 다양한 이미지 처리 알고리즘을 제공하는 네이티브 코드 라이브러리인 OpenCV(오픈 소스 컴퓨터 비전 라이브러리)를 사용하는 방법을 설명합니다. 이 문서의 예제 코드는 컬러 센서에서 프레임을 가져와서 OpenCV 라이브러리를 이용해 각 프레임을 흐리게 처리한 다음, 처리된 이미지를 XAML **이미지** 컨트롤로 표시하는 간단한 앱을 만드는 과정을 안내합니다. 

>[!NOTE]
>OpenCV.Win.Core 및 OpenCV.Win.ImgProc는 정기적으로 업데이트되지 않지만 이 페이지의 설명대로 OpenCVHelper를 만드는 데 계속 권장됩니다.

이 문서는 다음 두 문서의 콘텐츠를 기반으로 합니다.

* [MediaFrameReader를 사용하여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md) - 이 문서는 **MediaFrameReader**를 사용하여 하나 이상의 미디어 프레임 소스에서 프레임을 얻는 방법을 자세히 안내하고, 이 문서에 나오는 대부분의 샘플 코드를 자세히 설명합니다. 특히 **MediaFrameReader를 사용하여 미디어 프레임 처리**에는 XAML **이미지** 요소로 미디어 프레임의 제공을 처리하는 도우미 클래스 **FrameRenderer**의 코드 목록이 수록되어 있습니다. 이 문서의 샘플 코드는 이 도우미 클래스도 사용합니다.

* [OpenCV를 사용하여 소프트웨어 비트맵 처리](process-software-bitmaps-with-opencv.md) - 이 문서는 **MediaFrameReader**에서 사용하는 **SoftwareBitmap** 개체와 OpenCV 라이브러리에서 사용하는 **Mat** 유형 간 변환에 도움을 주는, 네이티브 코드 Windows 런타임 구성 요소 **OpenCVBridge**를 만드는 방법을 안내합니다. 이 문서의 샘플 코드는 UWP 앱 솔루션에 **OpenCVBridge** 구성 요소를 추가하는 단계를 따른 것으로 가정합니다.

이 문서 외에, 여기에서 설명하는 시나리오의 종단 간 작업 샘플 전체를 확인하고 다운로드하려면, Windows 유니버설 샘플 GitHub 리포의 [카메라 프레임 + OpenCV 샘플](https://go.microsoft.com/fwlink/?linkid=854003)을 참조하세요.

개발을 빠르게 시작 하려면 포함할 수 OpenCV 라이브러리를 UWP 앱 프로젝트에서 NuGet 패키지를 사용 하 여 있지만 하므로 OpenCV 다운로드 하는 것이 좋습니다. 스토어에 앱을 제출할 때 이러한 패키지 앱 certficication 프로세스를 전달 하지 않을 수 있습니다. 라이브러리는 소스 코드 및 앱을 제출 하기 전에 사용자가 직접 바이너리를 빌드합니다. OpenCV에 관한 최신 소식은 [http://opencv.org](http://opencv.org)에서 확인할 수 있습니다.


## <a name="implement-the-opencvhelper-native-windows-runtime-component"></a>OpenCVHelper 네이티브 Windows 런타임 구성 요소 구현
[OpenCV를 사용하여 소프트웨어 비트맵 처리](process-software-bitmaps-with-opencv.md)에 제시된 단계에 따라 OpenCV 도우미 Windows 런타임 구성 요소를 만들고 구성 요소 프로젝트에 대한 참조를 UWP 앱 솔루션에 추가합니다.

## <a name="find-available-frame-source-groups"></a>사용 가능한 프레임 소스 그룹 찾기
먼저 미디어 프레임을 가져올 미디어 프레임 소스 그룹을 찾아야 합니다. **[MediaFrameSourceGroup.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)** 를 호출하여 현재 장치에서 사용할 수 있는 소스 그룹 목록을 가져옵니다. 그런 다음, 앱 시나리오에 필요한 센서 유형을 제공하는 소스 그룹을 선택합니다. 이 예제에서는 RGB 카메라의 프레임을 제공하는 소스 그룹만 있으면 됩니다.

[!code-cs[OpenCVFrameSourceGroups](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameSourceGroups)]

## <a name="initialize-the-mediacapture-object"></a>MediaCapture 개체 초기화
그런 다음, 이전 단계에서 **MediaCaptureInitializationSettings**의 **[SourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)** 속성을 설정하여 선택한 프레임 소스 그룹을 사용하려면 **MediaCapture** 개체를 초기화해야 합니다.

> [!NOTE] 
> [OpenCV를 사용하여 소프트웨어 비트맵 처리](process-software-bitmaps-with-opencv.md)에서 자세히 설명한 OpenCVHelper 구성 요소에 사용되는 기술은 GPU 메모리가 아닌 CPU 메모리에 이미지 데이터가 있어야 합니다. 따라서 **MediaCaptureInitializationSettings**의 **[MemoryPreference](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.MemoryPreference)** 필드에 **MemoryPreference.CPU**를 지정해야 합니다.

**MediaCapture** 개체가 초기화된 후 **[MediaCapture.FrameSources](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.FrameSources)** 속성에 액세스하여 RGB 프레임 소스에 대한 참조를 가져옵니다.

[!code-cs[OpenCVInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVInitMediaCapture)]

## <a name="initialize-the-mediaframereader"></a>MediaFrameReader 초기화
그런 다음, 이전 단계에서 검색한 RGB 프레임 소스에 대한 [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader)를 생성합니다. 효과적인 프레임 속도를 유지하려면 센서의 해상도보다 낮은 해상도의 프레임을 처리해야 할 수 있습니다. 이 예제는 **[MediaCapture.CreateFrameReaderAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync)** 메서드에 옵션 **[BitmapSize](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapsize)** 인수를 제공하여, 프레임 읽기 프로그램이 제공하는 프레임 크기를 640 x 480픽셀로 조정할 것을 요청합니다.

프레임 읽기 프로그램을 만든 후 **[FrameArrived](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.FrameArrived)** 이벤트에 처리기를 등록합니다. 그런 다음, **FrameRenderer** 도우미 클래스가 처리된 이미지를 제공하는 데 사용할 새 **[SoftwareBitmapSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource)** 개체를 만듭니다. 그리고 **FrameRenderer**에 대한 생성자를 호출합니다. OpenCVBridge Windows 런타임 구성 요소에 정의된 **OpenCVHelper** 클래스의 인스턴스를 초기화합니다. 이 도우미 클래스는 각 프레임을 처리할 **FrameArrived** 처리기에 사용됩니다. 마지막으로, **[StartAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.StartAsync)** 를 호출하여 프레임 읽기 프로그램을 시작합니다.

[!code-cs[OpenCVFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameReader)]


## <a name="handle-the-framearrived-event"></a>FrameArrived 이벤트 처리
프레임 읽기 프로그램에서 새 프레임을 사용할 수 있을 때마다 **FrameArrived** 이벤트가 발생합니다. **[TryAcquireLatestFrame](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.TryAcquireLatestFrame)** 을 호출하여, 프레임이 있을 경우 가져옵니다. **[MediaFrameReference](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference)** 에서 **SoftwareBitmap**을 가져옵니다. 이 예제에 사용된 **CVHelper** 클래스의 이미지는 프리멀티플라이된 알파가 있는 BRGA8 픽셀 형식을 사용해야 합니다. 이벤트로 전달된 프레임의 형식이 다른 경우 **SoftwareBitmap**을 올바른 형식으로 변환합니다. 그런 다음, 흐림 작업의 대상으로 사용할 **SoftwareBitmap**을 생성합니다. 소스 이미지 속성이 생성자에 대한 인수로 사용되어, 해당하는 형식으로 비트맵을 만듭니다. 도우미 클래스 **Blur** 메서드를 호출하여 프레임을 처리합니다. 마지막으로, 초기화에 사용된 XAML **Image** 컨트롤로 이미지를 표시하는 **FrameRenderer** 도우미 클래스의 메서드 **PresentSoftwareBitmap**으로 흐림 작업의 출력 이미지를 전달합니다.

[!code-cs[OpenCVFrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameArrived)]

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [MediaFrameReader를 사용하여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md)
* [OpenCV로 소프트웨어 비트맵 처리](process-software-bitmaps-with-opencv.md)
* [카메라 프레임 샘플](http://go.microsoft.com/fwlink/?LinkId=823230)
* [카메라 프레임 + OpenCV 샘플](https://go.microsoft.com/fwlink/?linkid=854003)
 

 




