---
author: drewbatgit
ms.assetid: 370f2c14-4f1e-47b3-9197-24205ab255a3
description: 이 문서에는 UWP 앱에 사용할 수 있는 카메라 기능 및 사용 방법을 보여 주는 방법 문서의 링크가 나와 있습니다.
title: 카메라
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d330a762844e4889d0da9ae5457acc8d9f0c2b91
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6986709"
---
# <a name="camera"></a>카메라

이 섹션에서는 카메라 또는 마이크를 사용하여 사진, 비디오 또는 오디오를 캡처하는 UWP(유니버설 Windows 플랫폼) 앱을 만들기 위한 지침을 제공합니다.

## <a name="use-the-windows-built-in-camera-ui"></a>Windows 기본 제공 카메라 UI 사용

| 항목 | 설명 |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Windows 기본 제공 카메라 UI를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-cameracaptureui.md) | [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CameraCaptureUI) 클래스를 사용하여 Windows에 기본 제공된 카메라 UI로 사진 또는 비디오를 캡처하는 방법을 보여 줍니다. 단순히 사용자가 사진이나 비디오를 캡처하고 결과를 앱에 반환할 수 있게 하려는 경우에 가장 빠르고 편리한 방법입니다.  |

## <a name="basic-mediacapture-tasks"></a>기본 MediaCapture 작업

| 항목 | 설명 |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [카메라 미리 보기 표시](simple-camera-preview-access.md) | UWP 앱에서 XAML 페이지 내의 카메라 미리 보기 스트림을 빠르게 표시하는 방법을 보여 줍니다. |
| [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md) | [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) 클래스를 사용하여 사진과 비디오를 캡처하는 가장 간단한 방법을 보여 줍니다. **MediaCapture** 클래스는 캡처 파이프라인에 대한 하위 수준 제어를 제공하고 고급 캡처 시나리오를 가능하게 하는 강력한 API 집합을 표시하지만, 이 문서는 기본 미디어 캡처를 쉽고 빠르게 앱에 추가할 수 있도록 돕기 위한 것입니다. |
| [모바일 디바이스용 카메라 UI 기능](camera-ui-features-for-mobile-devices.md) | 모바일 디바이스에만 존재하는 특수한 카메라 UI 기능을 활용하는 방법을 보여 줍니다.  |
                                                                                                               
## <a name="advanced-mediacapture-tasks"></a>고급 MediaCapture 작업   
                                                                                                               
| 항목                                                                                             | 설명                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [MediaCapture를 사용하여 디바이스 및 화면 방향 처리](handle-device-orientation-with-mediacapture.md) | 도우미 클래스를 사용하여 사진 및 비디오를 캡처할 때 디바이스 방향을 처리하는 방법을 보여 줍니다. | 
| [카메라 프로필을 사용하여 카메라 기능 검색 및 선택](camera-profiles.md) | 카메라 프로필을 사용하여 여러 비디오 캡처 디바이스의 기능을 검색 및 관리하는 방법을 보여 줍니다. 특정 해상도 또는 프레임 속도를 지원하는 프로필, 여러 카메라에 대한 동시 액세스를 지원하는 프로필, HDR을 지원하는 프로필 선택 등의 작업이 포함됩니다. |
| [MediaCapture용 형식, 해상도 및 프레임 속도 선택](set-media-encoding-properties.md) | [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) 인터페이스를 사용하여 카메라 미리 보기 스트림과 캡처한 사진 및 비디오의 해상도와 프레임 속도를 설정하는 방법을 보여 줍니다. 또한 미리 보기 스트림의 가로 세로 비율이 캡처한 미디어의 가로 세로 비율과 일치하는지 확인하는 방법을 보여 줍니다. |
| [HDR 및 저조도 사진 캡처](high-dynamic-range-hdr-photo-capture.md) | [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture) 클래스를 사용하여 HDR(High Dynamic Range) 및 낮은 조명 사진을 캡처하는 방법을 보여 줍니다. |
| [사진 및 비디오 캡처를 위한 수동 카메라 컨트롤](capture-device-controls-for-photo-and-video-capture.md) | 수동 디바이스 컨트롤을 사용하여 광학 이미지 손떨림 보정, 부드러운 줌 등의 향상된 사진 및 비디오 캡처 시나리오를 가능하게 하는 방법을 보여 줍니다. |
| [비디오 캡처를 위한 수동 카메라 컨트롤](capture-device-controls-for-video-capture.md) | 수동 디바이스 컨트롤을 사용하여 HDR 비디오, 노출 우선 순위 등의 향상된 비디오 캡처 시나리오를 가능하게 하는 방법을 보여 줍니다.  |
| [비디오 캡처를 위한 동영상 손떨림 보정 효과](effects-for-video-capture.md) | 동영상 손떨림 보정 효과를 사용하는 방법을 보여 줍니다.  |
| [MediaCapture의 장면 분석](scene-analysis-for-media-capture.md) | [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.SceneAnalysisEffect) 및 [**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.FaceDetectionEffect)를 사용하여 미디어 캡처 미리 보기 스트림의 콘텐츠를 분석하는 방법을 보여 줍니다.  |
| [VariablePhotoSequence를 사용하여 사진 시퀀스 캡처](variable-photo-sequence.md) | 가변 사진 시퀀스를 캡처하여 여러 이미지 프레임을 빠른 속도로 연속으로 캡처하고 서로 다른 초점, 플래시, ISO, 노출 및 노출 보정 설정을 사용하도록 각 프레임을 구성할 수 있도록 하는 방법을 보여 줍니다.  |
| [MediaFrameReader를 사용하여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md) | [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader)와 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture)를 함께 사용하여 색, 깊이, 적외선 카메라, 오디오 디바이스, 사용자 지정 프레임 원본(예: 골격 추적 프레임을 생성하는 프레임 원본) 등 하나 이상의 사용 가능한 원본에서 미디어 프레임을 가져오는 방법을 보여 줍니다. 이 기능은 증강 현실 및 깊이 인식 카메라 앱과 같이 미디어 프레임의 실시간 처리를 수행하는 앱에 사용되도록 설계되었습니다.  |
| [미리 보기 프레임 가져오기](get-a-preview-frame.md) | 미디어 캡처 미리 보기 스트림에서 단일 미리 보기 프레임을 가져오는 방법을 보여 줍니다.  |                                                                                                   


## <a name="uwp-app-samples-for-camera"></a>카메라용 UWP 앱 샘플

* [카메라 얼굴 감지 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619486&clcid=0x409)
* [카메라 미리 보기 프레임 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620516&clcid=0x409)
* [카메라 HDR 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620517&clcid=0x409)
* [카메라 수동 컨트롤 샘플](http://go.microsoft.com/fwlink/p/?LinkID=627611&clcid=0x409)
* [카메라 프로필 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620518&clcid=0x409)
* [카메라 해상도 샘플](http://go.microsoft.com/fwlink/p/?LinkID=624252&clcid=0x409)
* [카메라 시작 키트](http://go.microsoft.com/fwlink/p/?LinkID=619479&clcid=0x409)
* [카메라 동영상 손떨림 보정 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620519&clcid=0x409)

## <a name="related-topics"></a>관련 항목

* [오디오 비디오 및 카메라](index.md)
 

 




