---
ms.assetid: ''
description: 이 문서에서는 MediaFrameReader 클래스와 함께 OpenCV (Open Source Computer Vision Library)를 사용 하는 방법을 보여 줍니다.
title: MediaFrameReader와 함께 OpenCV 사용
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, openCV
ms.localizationpriority: medium
ms.openlocfilehash: f78197b6108a81f3335dc202585127105bd94339
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363725"
---
# <a name="use-the-open-source-computer-vision-library-opencv-with-mediaframereader"></a>MediaFrameReader에서 OpenCV (Open Source Computer Vision Library) 사용

이 문서에서는 여러 소스 simulataneously에서 미디어 프레임을 읽을 수 있는 [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) 클래스를 사용 하 여 다양 한 이미지 처리 알고리즘을 제공 하는 네이티브 코드 라이브러리인 OpenCV (Open Source Computer Vision library)를 사용 하는 방법을 보여 줍니다. 이 문서의 예제 코드는 색 센서에서 프레임을 가져오고, OpenCV 라이브러리를 사용 하 여 각 프레임을 흐리게 하 고, 처리 된 이미지를 XAML **이미지** 컨트롤에 표시 하는 간단한 앱을 만드는 과정을 안내 합니다. 

>[!NOTE]
>OpenCV. 핵심 및 OpenCV는 정기적으로 업데이트 되지 않으며 저장소 규정 준수 검사를 통과 하지 않으므로 이러한 패키지는 실험 전용입니다.

이 문서는 두 개의 다른 아티클의 콘텐츠를 기반으로 합니다.

* [MediaFrameReader를 사용 하 여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md) -이 문서에서는 **MediaFrameReader** 를 사용 하 여 하나 이상의 미디어 프레임 소스에서 프레임을 가져오는 방법에 대 한 자세한 정보를 제공 하 고이 문서의 샘플 코드 대부분에 대해 자세히 설명 합니다. 특히 MediaFrameReader를 **사용 하 여 미디어 프레임 처리** 는 XAML **이미지** 요소의 미디어 프레임 프레젠테이션을 처리 하는 **FrameRenderer**도우미 클래스에 대 한 코드 목록을 제공 합니다. 이 문서의 샘플 코드에서는이 도우미 클래스도 사용 합니다.

* [Opencv를 사용 하 여 소프트웨어 비트맵 처리](process-software-bitmaps-with-opencv.md) -이 문서에서는 네이티브 코드 Windows 런타임 구성 요소인 **OpenCVBridge**를 만드는 과정을 안내 합니다 .이 **개체는** MediaFrameReader에서 사용 하는 **MediaFrameReader**와 OpenCV 라이브러리에서 사용 **되는 대/소문자 형식 간에** 변환 하는 데 도움이 됩니다. 이 문서의 샘플 코드에서는 **OpenCVBridge** 구성 요소를 UWP 앱 솔루션에 추가 하는 단계를 수행한 것으로 가정 합니다.

이러한 문서 외에도이 문서에 설명 된 시나리오의 전체 종단 간 작업 샘플을 보고 다운로드 하려면 Windows 유니버설 샘플 GitHub 리포지토리의 [카메라 프레임 + OpenCV 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV) 을 참조 하세요.

신속 하 게 개발을 시작 하려면 NuGet 패키지를 사용 하 여 UWP 앱 프로젝트에 OpenCV 라이브러리를 포함할 수 있지만, 이러한 패키지는 앱을 스토어에 제출할 때 앱 certficication 프로세스를 통과 하지 못할 수 있습니다. 따라서 응용 프로그램을 제출 하기 전에 OpenCV 라이브러리 소스 코드를 다운로드 하 고 이진 파일을 직접 빌드하는 것이 좋습니다. OpenCV로 개발 하는 정보는에서 찾을 수 있습니다. [https://opencv.org](https://opencv.org)


## <a name="implement-the-opencvhelper-native-windows-runtime-component"></a>OpenCVHelper 네이티브 Windows 런타임 구성 요소 구현
[Opencv를 사용 하 여 소프트웨어 비트맵 처리](process-software-bitmaps-with-opencv.md) 의 단계에 따라 opencv 도우미 Windows 런타임 구성 요소를 만들고 구성 요소 프로젝트에 대 한 참조를 UWP 앱 솔루션에 추가 합니다.

## <a name="find-available-frame-source-groups"></a>사용 가능한 프레임 소스 그룹 찾기
먼저 미디어 프레임을 가져올 미디어 프레임 원본 그룹을 찾아야 합니다. **[MediafFindAllAsync](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)** 를 호출 하 여 현재 장치에서 사용 가능한 원본 그룹 목록을 가져옵니다. 그런 다음 앱 시나리오에 필요한 센서 유형을 제공 하는 원본 그룹을 선택 합니다. 이 예에서는 RGB 카메라의 프레임을 제공 하는 소스 그룹만 있으면 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVFrameSourceGroups":::

## <a name="initialize-the-mediacapture-object"></a>MediaCapture 개체를 초기화 합니다.
다음으로, **MediaCaptureInitializationSettings**의 **[sourcegroup](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)** 속성을 설정 하 여 이전 단계에서 선택한 프레임 원본 그룹을 사용 하도록 **MediaCapture** 개체를 초기화 해야 합니다.

> [!NOTE] 
> OpenCVHelper 구성 요소에서 사용 하는 기술 ( [OpenCV를 사용 하 여 소프트웨어 비트맵 처리](process-software-bitmaps-with-opencv.md)에서 자세히 설명)에는 이미지 데이터가 GPU 메모리가 아닌 CPU 메모리에 있어야 합니다. 따라서 **MediaCaptureInitializationSettings**의 **[MemoryPreference](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.MemoryPreference)** 필드에 **MemoryPreference** 를 지정 해야 합니다.

**MediaCapture** 개체가가 초기화 된 후 **[FrameSources](/uwp/api/windows.media.capture.mediacapture.FrameSources)** 속성에 액세스 하 여 RGB 프레임 소스에 대 한 참조를 가져옵니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVInitMediaCapture":::

## <a name="initialize-the-mediaframereader"></a>MediaFrameReader 초기화
다음으로, 이전 단계에서 검색 한 RGB 프레임 소스에 대 한 [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) 를 만듭니다. 적절 한 프레임 속도로 유지 하기 위해 센서 해상도 보다 해상도가 낮은 프레임을 처리할 수 있습니다. 이 예제에서는 **[MediaCapture CreateFrameReaderAsync](/uwp/api/windows.media.capture.mediacapture.createframereaderasync)** 메서드에 선택적 **[BitmapSize](/uwp/api/windows.graphics.imaging.bitmapsize)** 인수를 제공 하 여 프레임 판독기에서 제공 하는 프레임의 크기를 640 x 480 픽셀로 조정 하도록 요청 합니다.

프레임 판독기를 만든 후 **[FrameArrived](/uwp/api/windows.media.capture.frames.mediaframereader.FrameArrived)** 이벤트에 대 한 처리기를 등록 합니다. 그런 다음 **FrameRenderer** helper 클래스에서 처리 된 이미지를 표시 하는 데 사용 하는 새 **[SoftwareBitmapSource](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource)** 개체를 만듭니다. 그런 다음 **FrameRenderer**에 대 한 생성자를 호출 합니다. OpenCVBridge Windows 런타임 구성 요소에 정의 된 **OpenCVHelper** 클래스의 인스턴스를 초기화 합니다. 이 도우미 클래스는 **FrameArrived** 처리기에서 각 프레임을 처리 하는 데 사용 됩니다. 마지막으로 **[StartAsync](/uwp/api/windows.media.capture.frames.mediaframereader.StartAsync)** 를 호출 하 여 프레임 판독기를 시작 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVFrameReader":::


## <a name="handle-the-framearrived-event"></a>FrameArrived 이벤트를 처리 합니다.
**FrameArrived** 이벤트는 프레임 판독기에서 새 프레임을 사용할 수 있을 때마다 발생 합니다. **[TryAcquireLatestFrame](/uwp/api/windows.media.capture.frames.mediaframereader.TryAcquireLatestFrame)** 를 호출 하 여 프레임 (있는 경우)을 가져옵니다. **[MediaFrameReference](/uwp/api/windows.media.capture.frames.mediaframereference)** 에서이 **비트맵** 을 가져옵니다. 이 예제에서 사용 된 **CVHelper** 클래스는 미리 증가 된 알파를 사용 하 여 BRGA8 픽셀 형식을 사용 해야 합니다. 이벤트로 전달 되는 프레임의 형식이 다른 경우에는 잘못 된 **형식으로 변환** 합니다. 다음으로, 흐림 작업의 대상으로 사용할는이 **비트맵** 을 만듭니다. 소스 이미지 속성은 형식이 일치 하는 비트맵을 만들기 위해 생성자에 대 한 인수로 사용 됩니다. 도우미 클래스 **흐림** 메서드를 호출 하 여 프레임을 처리 합니다. 마지막으로, 흐림 작업의 출력 이미지를 **PresentSoftwareBitmap**에 전달 합니다. **FrameRenderer** helper 클래스의 메서드는 초기화 된 XAML **이미지** 컨트롤의 이미지를 표시 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVFrameArrived":::

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [MediaFrameReader를 사용하여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md)
* [OpenCV를 사용 하 여 소프트웨어 비트맵 처리](process-software-bitmaps-with-opencv.md)
* [카메라 프레임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [카메라 프레임 + OpenCV 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV)
 

 
