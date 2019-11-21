---
ms.assetid: 370f2c14-4f1e-47b3-9197-24205ab255a3
description: 이 문서에는 UWP 앱에 사용할 수 있는 카메라 기능 및 사용 방법을 보여 주는 방법 문서의 링크가 나와 있습니다.
title: 카메라
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d7d7fcdeb3740ac4c6851170796243392676d1d1
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254296"
---
# <a name="camera"></a>카메라

이 섹션에서는 카메라 또는 마이크를 사용하여 사진, 비디오 또는 오디오를 캡처하는 UWP(유니버설 Windows 플랫폼) 앱을 만들기 위한 지침을 제공합니다.

## <a name="use-the-windows-built-in-camera-ui"></a>Windows 기본 제공 카메라 UI 사용

| 항목 | 설명 |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Capture photos and video with Windows built-in camera UI](capture-photos-and-video-with-cameracaptureui.md) | [  **CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 클래스를 사용하여 Windows에 기본 제공된 카메라 UI로 사진 또는 비디오를 캡처하는 방법을 보여 줍니다. 단순히 사용자가 사진이나 비디오를 캡처하고 결과를 앱에 반환할 수 있게 하려는 경우에 가장 빠르고 편리한 방법입니다.  |

## <a name="basic-mediacapture-tasks"></a>기본 MediaCapture 작업

| 항목 | 설명 |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Display the camera preview](simple-camera-preview-access.md) | UWP 앱에서 XAML 페이지 내의 카메라 미리 보기 스트림을 빠르게 표시하는 방법을 보여 줍니다. |
| [Basic photo, video, and audio capture with MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md) | [  **MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) 클래스를 사용하여 사진과 비디오를 캡처하는 가장 간단한 방법을 보여 줍니다. **MediaCapture** 클래스는 캡처 파이프라인에 대한 하위 수준 제어를 제공하고 고급 캡처 시나리오를 가능하게 하는 강력한 API 집합을 표시하지만, 이 문서는 기본 미디어 캡처를 쉽고 빠르게 앱에 추가할 수 있도록 돕기 위한 것입니다. |
| [Camera UI features for mobile devices](camera-ui-features-for-mobile-devices.md) | 모바일 디바이스에만 존재하는 특수한 카메라 UI 기능을 활용하는 방법을 보여 줍니다.  |
                                                                                                               
## <a name="advanced-mediacapture-tasks"></a>고급 MediaCapture 작업   
                                                                                                               
| 항목                                                                                             | 설명                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Handle device and screen orientation with MediaCapture](handle-device-orientation-with-mediacapture.md) | 도우미 클래스를 사용하여 사진 및 비디오를 캡처할 때 디바이스 방향을 처리하는 방법을 보여 줍니다. | 
| [Discover and select camera capabilities with camera profiles](camera-profiles.md) | 카메라 프로필을 사용하여 여러 비디오 캡처 디바이스의 기능을 검색 및 관리하는 방법을 보여 줍니다. 특정 해상도 또는 프레임 속도를 지원하는 프로필, 여러 카메라에 대한 동시 액세스를 지원하는 프로필, HDR을 지원하는 프로필 선택 등의 작업이 포함됩니다. |
| [Set format, resolution, and frame rate for MediaCapture](set-media-encoding-properties.md) | [  **IMediaEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.IMediaEncodingProperties) 인터페이스를 사용하여 카메라 미리 보기 스트림과 캡처한 사진 및 비디오의 해상도와 프레임 속도를 설정하는 방법을 보여 줍니다. 또한 미리 보기 스트림의 가로 세로 비율이 캡처한 미디어의 가로 세로 비율과 일치하는지 확인하는 방법을 보여 줍니다. |
| [HDR and low-light photo capture](high-dynamic-range-hdr-photo-capture.md) | [  **AdvancedPhotoCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) 클래스를 사용하여 HDR(High Dynamic Range) 및 낮은 조명 사진을 캡처하는 방법을 보여 줍니다. |
| [Manual camera controls for photo and video capture](capture-device-controls-for-photo-and-video-capture.md) | 수동 디바이스 컨트롤을 사용하여 광학 이미지 손떨림 보정, 부드러운 줌 등의 향상된 사진 및 비디오 캡처 시나리오를 가능하게 하는 방법을 보여 줍니다. |
| [Manual camera controls for video capture](capture-device-controls-for-video-capture.md) | 수동 디바이스 컨트롤을 사용하여 HDR 비디오, 노출 우선 순위 등의 향상된 비디오 캡처 시나리오를 가능하게 하는 방법을 보여 줍니다.  |
| [Video stabilization effect for video capture](effects-for-video-capture.md) | 동영상 손떨림 보정 효과를 사용하는 방법을 보여 줍니다.  |
| [Scene anlysis for MediaCapture](scene-analysis-for-media-capture.md) | [  **SceneAnalysisEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffect) 및 [**FaceDetectionEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffect)를 사용하여 미디어 캡처 미리 보기 스트림의 콘텐츠를 분석하는 방법을 보여 줍니다.  |
| [Capture a photo sequence with VariablePhotoSequence](variable-photo-sequence.md) | 가변 사진 시퀀스를 캡처하여 여러 이미지 프레임을 빠른 속도로 연속으로 캡처하고 서로 다른 초점, 플래시, ISO, 노출 및 노출 보정 설정을 사용하도록 각 프레임을 구성할 수 있도록 하는 방법을 보여 줍니다.  |
| [Process media frames with MediaFrameReader](process-media-frames-with-mediaframereader.md) | [  **MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader)와 [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)를 함께 사용하여 색, 깊이, 적외선 카메라, 오디오 디바이스, 사용자 지정 프레임 원본(예: 골격 추적 프레임을 생성하는 프레임 원본) 등 하나 이상의 사용 가능한 원본에서 미디어 프레임을 가져오는 방법을 보여 줍니다. 이 기능은 증강 현실 및 깊이 인식 카메라 앱과 같이 미디어 프레임의 실시간 처리를 수행하는 앱에 사용되도록 설계되었습니다.  |
| [Get a preview frame](get-a-preview-frame.md) | 미디어 캡처 미리 보기 스트림에서 단일 미리 보기 프레임을 가져오는 방법을 보여 줍니다.  |                                                                                                   


## <a name="uwp-app-samples-for-camera"></a>카메라용 UWP 앱 샘플

* [Camera face detection sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFaceDetection)
* [Camera preview frame sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraGetPreviewFrame)
* [Camera HDR sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraAdvancedCapture)
* [Camera manual controls sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraManualControls)
* [Camera profile sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraProfile)
* [Camera resolution sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraResolution)
* [Camera starter kit](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraStarterKit)
* [Camera video stabilization sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraVideoStabilization)

## <a name="related-topics"></a>관련 항목

* [오디오 비디오 및 카메라](index.md)
 

 




