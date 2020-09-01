---
ms.assetid: 370f2c14-4f1e-47b3-9197-24205ab255a3
description: 이 문서에서는 UWP 앱에 사용할 수 있는 카메라 기능을 나열 하 고 사용 방법을 보여 주는 방법 문서에 대 한 링크를 제공 합니다.
title: 카메라
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e5b78364f5d59889f249d6d23d414528461368b4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161017"
---
# <a name="camera"></a>카메라

이 섹션에서는 카메라 또는 마이크를 사용 하 여 사진, 동영상 또는 오디오를 캡처하는 유니버설 Windows 플랫폼 (UWP) 앱을 만드는 방법에 대 한 지침을 제공 합니다.

## <a name="use-the-windows-built-in-camera-ui"></a>Windows 기본 제공 카메라 UI 사용

| 항목 | Description |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Windows 기본 카메라 UI를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-cameracaptureui.md) | [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) 클래스를 사용 하 여 Windows에 기본 제공 되는 카메라 UI를 사용 하 여 사진이 나 비디오를 캡처하는 방법을 보여 줍니다. 사용자가 사진 또는 비디오를 캡처하여 앱에 결과를 반환할 수 있도록 하려면이 작업을 수행 하는 가장 빠르고 쉬운 방법입니다.  |

## <a name="basic-mediacapture-tasks"></a>기본 MediaCapture 작업

| 항목 | Description |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [카메라 미리 보기 표시](simple-camera-preview-access.md) | UWP 앱의 XAML 페이지 내에서 카메라 미리 보기 스트림을 신속 하 게 표시 하는 방법을 보여 줍니다. |
| [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md) | [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) 클래스를 사용 하 여 사진과 비디오를 캡처하는 가장 간단한 방법을 보여 줍니다. **MediaCapture** 클래스는 캡처 파이프라인에 대 한 하위 수준 제어를 제공 하 고 고급 캡처 시나리오를 사용 하는 강력한 api 집합을 제공 하지만이 문서는 기본 미디어 캡처를 앱에 빠르고 쉽게 추가 하는 데 도움이 됩니다. |
| [모바일 디바이스용 카메라 UI 기능](camera-ui-features-for-mobile-devices.md) | 모바일 장치에만 있는 특수 카메라 UI 기능을 활용 하는 방법을 보여 줍니다.  |
                                                                                                               
## <a name="advanced-mediacapture-tasks"></a>Advanced MediaCapture 작업   
                                                                                                               
| 항목                                                                                             | Description                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [MediaCapture를 사용하여 디바이스 및 화면 방향 처리](handle-device-orientation-with-mediacapture.md) | 도우미 클래스를 사용 하 여 사진 및 비디오를 캡처할 때 장치 방향을 처리 하는 방법을 보여 줍니다. | 
| [카메라 프로필을 사용하여 카메라 기능 검색 및 선택](camera-profiles.md) | 카메라 프로필을 사용 하 여 다양 한 비디오 캡처 장치의 기능을 검색 하 고 관리 하는 방법을 보여 줍니다. 여기에는 특정 해상도 또는 프레임 속도를 지 원하는 프로필 선택, 여러 카메라에 대 한 동시 액세스를 지 원하는 프로필 및 HDR를 지 원하는 프로필 등의 작업이 포함 됩니다. |
| [MediaCapture용 형식, 해상도 및 프레임 속도 선택](set-media-encoding-properties.md) | [**IMediaEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.IMediaEncodingProperties) 인터페이스를 사용 하 여 카메라 미리 보기 스트림과 캡처한 사진 및 비디오의 해상도 및 프레임 주기를 설정 하는 방법을 보여 줍니다. 또한 미리 보기 스트림의 가로 세로 비율이 캡처된 미디어와 일치 하는지 확인 하는 방법도 보여 줍니다. |
| [HDR 및 저조도 사진 캡처](high-dynamic-range-hdr-photo-capture.md) | [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) 클래스를 사용 하 여 HDR (High Dynamic Range) 및 낮은 밝은 사진을 캡처하는 방법을 보여 줍니다. |
| [사진 및 비디오 캡처를 위한 수동 카메라 컨트롤](capture-device-controls-for-photo-and-video-capture.md) | 수동 장치 컨트롤을 사용 하 여 광학 이미지 안정화 및 부드러운 확대/축소를 비롯 한 향상 된 사진 및 비디오 캡처 시나리오를 사용 하는 방법을 보여 줍니다. |
| [비디오 캡처를 위한 수동 카메라 컨트롤](capture-device-controls-for-video-capture.md) | 수동 장치 컨트롤을 사용 하 여 HDR 비디오 및 노출 우선 순위를 비롯 한 향상 된 비디오 캡처 시나리오를 사용 하는 방법을 보여 줍니다.  |
| [비디오 캡처를 위한 동영상 손떨림 보정 효과](effects-for-video-capture.md) | 비디오 안정화 효과를 사용 하는 방법을 보여 줍니다.  |
| [MediaCapture에 대 한 장면 analysis](scene-analysis-for-media-capture.md) | [**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect) 및 [**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) 를 사용 하 여 미디어 캡처 미리 보기 스트림의 콘텐츠를 분석 하는 방법을 보여 줍니다.  |
| [VariablePhotoSequence를 사용하여 사진 시퀀스 캡처](variable-photo-sequence.md) | 여러 이미지 프레임을 신속 하 게 캡처하고 각 프레임에서 다른 포커스, 플래시, ISO, 노출 및 노출 보정 설정을 사용 하도록 구성할 수 있는 변수 사진 시퀀스를 캡처하는 방법을 보여 줍니다.  |
| [MediaFrameReader를 사용하여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md) | [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) 와 함께 [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) 를 사용 하 여 색, 깊이, 적외선 카메라, 오디오 장치 또는 사용자 지정 프레임 원본 (예: 골격 추적 프레임을 생성 하는)을 비롯 한 하나 이상의 사용 가능한 원본에서 미디어 프레임을 가져오는 방법을 보여 줍니다. 이 기능은 확대된 현실 및 깊이 인식 카메라 앱과 같이 미디어 프레임의 실시간 처리를 수행하는 앱에 사용되도록 설계되었습니다.  |
| [미리 보기 프레임 가져오기](get-a-preview-frame.md) | 미디어 캡처 미리 보기 스트림에서 단일 미리 보기 프레임을 가져오는 방법을 보여 줍니다.  |                                                                                                   


## <a name="uwp-app-samples-for-camera"></a>카메라에 대 한 UWP 앱 샘플

* [카메라 얼굴 검색 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFaceDetection)
* [카메라 미리 보기 프레임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraGetPreviewFrame)
* [카메라 HDR 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraAdvancedCapture)
* [카메라 수동 컨트롤 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraManualControls)
* [카메라 프로필 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraProfile)
* [카메라 해상도 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraResolution)
* [카메라 시작 키트](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraStarterKit)
* [카메라 비디오 안정화 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraVideoStabilization)

## <a name="related-topics"></a>관련 항목

* [오디오 비디오 및 카메라](index.md)
 

 