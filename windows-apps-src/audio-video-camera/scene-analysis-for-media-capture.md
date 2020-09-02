---
ms.assetid: B5D915E4-4280-422C-BA0E-D574C534410B
description: 이 문서에서는 SceneAnalysisEffect 및 FaceDetectionEffect를 사용 하 여 미디어 캡처 미리 보기 스트림의 콘텐츠를 분석 하는 방법을 설명 합니다.
title: 카메라 프레임 분석 효과
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 56f828268980eb7ccc63a84729365bb5d17a609c
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363756"
---
# <a name="effects-for-analyzing-camera-frames"></a>카메라 프레임 분석 효과



이 문서에서는 [**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect) 및 [**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) 를 사용 하 여 미디어 캡처 미리 보기 스트림의 콘텐츠를 분석 하는 방법을 설명 합니다.

## <a name="scene-analysis-effect"></a>장면 분석 효과

[**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect) 는 미디어 캡처 미리 보기 스트림의 비디오 프레임을 분석 하 고 캡처 결과를 개선 하는 처리 옵션을 권장 합니다. 현재이 효과는 HDR (High Dynamic Range) 처리를 사용 하 여 캡처를 향상할 수 있는지 검색 하는 것을 지원 합니다.

HDR 사용을 권장 하는 경우 다음과 같은 방법으로이 작업을 수행할 수 있습니다.

-   [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) 클래스를 사용 하 여 Windows 기본 제공 HDR 처리 알고리즘을 사용 하 여 사진을 캡처할 수 있습니다. 자세한 내용은 [HDR (High Dynamic Range) 사진 캡처](high-dynamic-range-hdr-photo-capture.md)를 참조 하세요.

-   [**HdrVideoControl**](/uwp/api/Windows.Media.Devices.HdrVideoControl) 를 사용 하 여 Windows 기본 제공 HDR 처리 알고리즘을 통해 비디오를 캡처합니다. 자세한 내용은 [비디오 캡처를 위한 장치 컨트롤 캡처](capture-device-controls-for-video-capture.md)를 참조 하세요.

-   [**VariablePhotoSequenceControl**](/uwp/api/Windows.Media.Devices.Core.VariablePhotoSequenceController) 를 사용 하 여 사용자 지정 HDR 구현을 사용 하 여 합성할 수 있는 프레임 시퀀스를 캡처할 수 있습니다. 자세한 내용은 [변수 사진 시퀀스](variable-photo-sequence.md)를 참조 하세요.

### <a name="scene-analysis-namespaces"></a>장면 분석 네임 스페이스

장면 분석을 사용 하려면 앱이 기본 미디어 캡처에 필요한 네임 스페이스 외에도 다음 네임 스페이스를 포함 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSceneAnalysisUsing":::

### <a name="initialize-the-scene-analysis-effect-and-add-it-to-the-preview-stream"></a>장면 분석 효과를 초기화 하 고 미리 보기 스트림에 추가 합니다.

비디오 효과는 캡처 장치에서 효과를 초기화 하는 데 필요한 설정과 효과를 제어 하는 데 사용할 수 있는 효과 인스턴스를 제공 하는 효과 정의 인 두 개의 Api를 사용 하 여 구현 됩니다. 코드 내의 여러 위치에서 효과 인스턴스에 액세스할 수 있으므로 일반적으로 개체를 보유 하는 멤버 변수를 선언 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetDeclareSceneAnalysisEffect":::

앱에서 **MediaCapture** 개체를 초기화 한 후 [**SceneAnalysisEffectDefinition**](/uwp/api/Windows.Media.Core.SceneAnalysisEffectDefinition)의 새 인스턴스를 만듭니다.

**MediaCapture** 개체에서 [**AddVideoEffectAsync**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) 를 호출 하 여 캡처 장치에 효과를 등록 하 고, **SceneAnalysisEffectDefinition** 를 제공 하 고, [**mediastreamtype VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) 을 지정 하 여 효과를 캡처 스트림과 달리 비디오 미리 보기 스트림에 적용 해야 함을 표시 합니다. **AddVideoEffectAsync** 는 추가 된 효과의 인스턴스를 반환 합니다. 이 메서드는 여러 효과 형식에 사용할 수 있으므로 반환 된 인스턴스를 [**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect) 개체로 캐스팅 해야 합니다.

장면 분석의 결과를 수신 하려면 [**SceneAnalyzed**](/uwp/api/windows.media.core.sceneanalysiseffect.sceneanalyzed) 이벤트에 대 한 처리기를 등록 해야 합니다.

현재 장면 분석 효과는 높은 동적 범위 분석기만 포함 합니다. 효과의 HighDynamicRangeControl를 true로 설정 하 여 HDR 분석을 사용 하도록 설정 [**합니다.**](/uwp/api/windows.media.core.highdynamicrangecontrol.enabled)

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCreateSceneAnalysisEffectAsync":::

### <a name="implement-the-sceneanalyzed-event-handler"></a>SceneAnalyzed 이벤트 처리기 구현

장면 분석의 결과는 **SceneAnalyzed** 이벤트 처리기에서 반환 됩니다. 처리기에 전달 된 [**SceneAnalyzedEventArgs**](/uwp/api/Windows.Media.Core.SceneAnalyzedEventArgs) 개체에는 [**Highdynamicrangeoutput**](/uwp/api/Windows.Media.Core.HighDynamicRangeOutput) 개체가 있는 [**SceneAnalysisEffectFrame**](/uwp/api/Windows.Media.Core.SceneAnalysisEffectFrame) 개체가 있습니다. 높은 동적 범위 출력의 [**확신**](/uwp/api/windows.media.core.highdynamicrangeoutput.certainty) 속성은 0에서 1.0 사이의 값을 제공 합니다. 여기서 0은 hdr 처리가 캡처 결과를 개선 하는 데 도움이 되지 않으며 1.0은 hdr 처리가 도움이 됨을 나타냅니다. 에서는 HDR를 사용 하거나 사용자에 게 결과를 표시 하 고 사용자가 결정 하도록 하는 임계값 지점을 결정할 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSceneAnalyzed":::

또한 처리기로 전달 되는 [**Highdynamicrangeoutput**](/uwp/api/Windows.Media.Core.HighDynamicRangeOutput) 개체에는 HDR 처리를 위한 변수 사진 시퀀스를 캡처하기 위한 제안 프레임 컨트롤러를 포함 하는 [**FrameControllers**](/uwp/api/windows.media.core.highdynamicrangeoutput.framecontrollers) 속성이 있습니다. 자세한 내용은 [변수 사진 시퀀스](variable-photo-sequence.md)를 참조 하세요.

### <a name="clean-up-the-scene-analysis-effect"></a>장면 분석 효과 정리

앱이 캡처를 완료 한 후에는 **MediaCapture** 개체를 삭제 하기 전에 효과의 [**Highdynamicrangeanalyzer**](/uwp/api/windows.media.core.highdynamicrangecontrol.enabled) 를 false로 설정 하 고 [**SceneAnalyzed**](/uwp/api/windows.media.core.sceneanalysiseffect.sceneanalyzed) 이벤트 처리기를 등록 취소 하 여 장면 분석 효과를 사용 하지 않도록 설정 해야 합니다. MediaCapture를 호출 하 여 효과가 추가 된 스트림 이후 비디오 미리 보기 스트림을 지정 합니다 [**.**](/uwp/api/windows.media.capture.mediacapture.cleareffectsasync) 마지막으로 멤버 변수를 null로 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpSceneAnalysisEffectAsync":::

## <a name="face-detection-effect"></a>얼굴 검색 효과

[**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) 는 미디어 캡처 미리 보기 스트림 내에서 얼굴의 위치를 식별 합니다. 이 효과를 사용 하면 미리 보기 스트림에서 얼굴이 검색 될 때마다 알림을 받을 수 있으며 미리 보기 프레임 내에서 검색 된 각 면에 대 한 경계 상자를 제공할 수 있습니다. 지원 되는 장치에서 얼굴 검색 효과는 향상 된 노출을 제공 하며 장면의 가장 중요 한 면에 집중 합니다.

### <a name="face-detection-namespaces"></a>얼굴 감지 네임 스페이스

얼굴 감지를 사용 하려면 앱이 기본 미디어 캡처에 필요한 네임 스페이스 외에도 다음 네임 스페이스를 포함 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFaceDetectionUsing":::

### <a name="initialize-the-face-detection-effect-and-add-it-to-the-preview-stream"></a>얼굴 검색 효과를 초기화 하 고 미리 보기 스트림에 추가 합니다.

비디오 효과는 캡처 장치에서 효과를 초기화 하는 데 필요한 설정과 효과를 제어 하는 데 사용할 수 있는 효과 인스턴스를 제공 하는 효과 정의 인 두 개의 Api를 사용 하 여 구현 됩니다. 코드 내의 여러 위치에서 효과 인스턴스에 액세스할 수 있으므로 일반적으로 개체를 보유 하는 멤버 변수를 선언 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetDeclareFaceDetectionEffect":::

앱에서 **MediaCapture** 개체를 초기화 한 후 [**FaceDetectionEffectDefinition**](/uwp/api/Windows.Media.Core.FaceDetectionEffectDefinition)의 새 인스턴스를 만듭니다. [**DetectionMode**](/uwp/api/windows.media.core.facedetectioneffectdefinition.detectionmode) 속성을 설정 하 여 더 빠른 얼굴 검색 또는 더 정확한 얼굴 검색의 우선 순위를 지정 합니다. [**SynchronousDetectionEnabled**](/uwp/api/windows.media.core.facedetectioneffectdefinition.synchronousdetectionenabled) 를 설정 하 여이 작업을 수행 하면 미리 보기 환경이 고르지 않을 수 있으므로 들어오는 프레임에서 얼굴 감지를 완료할 때까지 지연 되지 않도록 지정 합니다.

**MediaCapture** 개체에서 [**AddVideoEffectAsync**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) 를 호출 하 여 캡처 장치에 효과를 등록 하 고, **FaceDetectionEffectDefinition** 를 제공 하 고, [**mediastreamtype VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) 을 지정 하 여 효과를 캡처 스트림과 달리 비디오 미리 보기 스트림에 적용 해야 함을 표시 합니다. **AddVideoEffectAsync** 는 추가 된 효과의 인스턴스를 반환 합니다. 이 메서드는 여러 효과 형식에 사용할 수 있으므로 반환 된 인스턴스를 [**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) 개체로 캐스팅 해야 합니다.

[**FaceDetectionEffect**](/uwp/api/windows.media.core.facedetectioneffect.enabled) 속성을 설정 하 여 효과를 사용 하거나 사용 하지 않도록 설정 합니다. [**FaceDetectionEffect DesiredDetectionInterval**](/uwp/api/windows.media.core.facedetectioneffect.desireddetectioninterval) 속성을 설정 하 여 효과에서 프레임을 분석 하는 빈도를 조정 합니다. 미디어 캡처가 진행 중일 때 이러한 속성을 모두 조정할 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCreateFaceDetectionEffectAsync":::

### <a name="receive-notifications-when-faces-are-detected"></a>얼굴이 감지 되 면 알림 받기

비디오 미리 보기에서 검색 된 얼굴 주위의 상자를 그리는 등 얼굴 감지 시 일부 작업을 수행 하려는 경우 [**FaceDetected**](/uwp/api/windows.media.core.facedetectioneffect.facedetected) 이벤트를 등록할 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetRegisterFaceDetectionHandler":::

이벤트에 대 한 처리기에서 [**FaceDetectedEventArgs**](/uwp/api/Windows.Media.Core.FaceDetectedEventArgs)의 [**FaceDetectionEffectFrame DetectedFaces**](/uwp/api/windows.media.core.facedetectioneffectframe.detectedfaces) 속성에 액세스 하 여 프레임에서 검색 된 모든 얼굴의 목록을 가져올 수 있습니다. [**FaceBox**](/uwp/api/windows.media.faceanalysis.detectedface.facebox) 속성은 미리 보기 스트림 차원과 관련 하 여 검색 된 얼굴을 포함 하는 사각형을 설명 하는 [**BitmapBounds**](/uwp/api/Windows.Graphics.Imaging.BitmapBounds) 구조체입니다. 미리 보기 스트림 좌표를 화면 좌표로 변환 하는 샘플 코드를 보려면 [face 검색 UWP 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFaceDetection)을 참조 하세요.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFaceDetected":::

### <a name="clean-up-the-face-detection-effect"></a>얼굴 검색 효과 정리

앱이 캡처를 완료 한 후 **MediaCapture** 개체를 삭제 하기 전에 [**FaceDetectionEffect**](/uwp/api/windows.media.core.facedetectioneffect.enabled) 를 사용 하 여 얼굴 검색 효과를 사용 하지 않도록 설정 하 고 이전에 등록 한 경우 [**FaceDetected**](/uwp/api/windows.media.core.facedetectioneffect.facedetected) 이벤트 처리기를 등록 취소 해야 합니다. MediaCapture를 호출 하 여 효과가 추가 된 스트림 이후 비디오 미리 보기 스트림을 지정 합니다 [**.**](/uwp/api/windows.media.capture.mediacapture.cleareffectsasync) 마지막으로 멤버 변수를 null로 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpFaceDetectionEffectAsync":::

### <a name="check-for-focus-and-exposure-support-for-detected-faces"></a>검색 된 면에 대 한 포커스 및 노출이 지원 되는지 확인 합니다.

모든 장치에는 검색 된 면에 따라 포커스 및 노출을 조정할 수 있는 캡처 장치가 없습니다. 얼굴 검색은 장치 리소스를 사용 하므로,이 기능을 사용 하 여 캡처를 향상 시킬 수 있는 장치 에서만 얼굴 감지를 사용 하도록 설정할 수 있습니다. 얼굴 기반 캡처 최적화를 사용할 수 있는지 확인 하려면 초기화 된 [MediaCapture](./index.md) 에 대 한 [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) 을 가져온 다음 비디오 장치 컨트롤러의 [**RegionsOfInterestControl**](/uwp/api/Windows.Media.Devices.RegionsOfInterestControl)를 가져옵니다. [**Maxregions**](/uwp/api/windows.media.devices.regionsofinterestcontrol.maxregions) 하나 이상의 지역을 지원 하는지 확인 하세요. 그런 다음 [**AutoExposureSupported**](/uwp/api/windows.media.devices.regionsofinterestcontrol.autoexposuresupported) 또는 [**AutoFocusSupported**](/uwp/api/windows.media.devices.regionsofinterestcontrol.autofocussupported) 가 true 인지 확인 합니다. 이러한 조건이 충족 되 면 장치에서 얼굴 감지를 활용 하 여 캡처를 향상 시킬 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetAreFaceFocusAndExposureSupported":::

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
