---
author: drewbatgit
ms.assetid: B5D915E4-4280-422C-BA0E-D574C534410B
description: 이 문서에서는 SceneAnalysisEffect 및 FaceDetectionEffect를 사용하여 미디어 캡처 미리 보기 스트림의 콘텐츠를 분석하는 방법을 설명합니다.
title: 카메라 프레임 분석 효과
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 3fc55919942c1edc82f7c2e5da2608b5f1b1445b
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.locfileid: "219115"
---
# <a name="effects-for-analyzing-camera-frames"></a>카메라 프레임 분석 효과

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


이 문서에서는 [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902) 및 [**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/dn948776)를 사용하여 미디어 캡처 미리 보기 스트림의 콘텐츠를 분석하는 방법을 설명합니다.

## <a name="scene-analysis-effect"></a>장면 분석 효과

[**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902)는 미디어 캡처 미리 보기 스트림의 비디오 프레임을 분석하고 캡처 결과를 개선하는 처리 옵션을 권장합니다. 현재 효과는 HDR(High Dynamic Range) 처리를 사용하여 캡처를 향상할 수 있는지 여부를 감지할 수 있도록 지원합니다.

효과가 HDR 사용을 권장하는 경우 다음과 같은 방식으로 사용할 수 있습니다.

-   [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 클래스를 사용하여 Windows 기본 제공 HDR 처리 알고리즘을 사용해 사진을 캡처합니다. 자세한 내용은 [HDR(High Dynamic Range) 사진 캡처](high-dynamic-range-hdr-photo-capture.md)를 참조하세요.

-   [**HdrVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926680)을 사용하여 Windows 기본 제공 HDR 처리 알고리즘을 사용해 동영상을 캡처합니다. 자세한 내용은 [비디오 캡처를 위한 캡처 디바이스 컨트롤](capture-device-controls-for-video-capture.md)을 참조하세요.

-   [**VariablePhotoSequenceControl**](https://msdn.microsoft.com/library/windows/apps/dn640573)을 사용하여 프레임 시퀀스를 캡처하며 그런 다음 사용자 지정 HDR 구현을 사용하여 복합적으로 구성할 수 있습니다. 자세한 내용은 [가변 사진 시퀀스](variable-photo-sequence.md)를 참조하세요.

### <a name="scene-analysis-namespaces"></a>장면 분석 네임스페이스

장면 분석을 사용하려면 앱에 기본적인 미디어 캡처에 필요한 네임스페이스 외에도 다음 네임스페이스가 있어야 합니다.

[!code-cs[SceneAnalysisUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSceneAnalysisUsing)]

### <a name="initialize-the-scene-analysis-effect-and-add-it-to-the-preview-stream"></a>장면 분석 효과를 초기화하고 미리 보기 스트림에 추가

비디오 효과는 두 개의 API, 효과 정의(캡처 장치가 효과를 초기화하는 데 필요한 설정을 제공함), 효과 인스턴스(효과를 제어하는 데 사용할 수 있음)를 사용하여 구현됩니다. 코드 내의 여러 위치에서 효과 인스턴스에 액세스해야 할 수도 있으므로, 일반적으로 개체를 보유하도록 멤버 변수를 선언해야 합니다.

[!code-cs[DeclareSceneAnalysisEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareSceneAnalysisEffect)]

앱에서 **MediaCapture** 개체를 초기화한 후에 새 [**SceneAnalysisEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn948903) 인스턴스를 만듭니다.

**MediaCapture** 개체에 대해 [**AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035)를 호출하여 캡처 디바이스에 효과를 등록하고, **SceneAnalysisEffectDefinition**을 제공하고 [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640)를 지정하여 캡처 스트림이 아니라 동영상 미리 보기 스트림에 효과를 적용해야 한다는 것을 나타냅니다. **AddVideoEffectAsync**는 추가된 효과의 인스턴스를 반환합니다. 이 메서드는 여러 효과 유형에 사용할 수 있으므로, 반환된 인스턴스를 [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902) 개체에 캐스팅해야 합니다.

장면 분석의 결과를 수신하려면 [**SceneAnalyzed**](https://msdn.microsoft.com/library/windows/apps/dn948920) 이벤트에 대한 처리기를 등록해야 합니다.

현재 장면 분석 효과에는 High Dynamic Range 분석기만 포함됩니다. 효과의 [**HighDynamicRangeControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn948827)를 true로 설정하여 HDR 분석을 사용하도록 설정합니다.

[!code-cs[CreateSceneAnalysisEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateSceneAnalysisEffectAsync)]

### <a name="implement-the-sceneanalyzed-event-handler"></a>SceneAnalyzed 이벤트 처리기 구현

장면 분석의 결과는 **SceneAnalyzed** 이벤트 처리기에 반환됩니다. 처리기에 전달된 [**SceneAnalyzedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn948922) 개체에는 [**HighDynamicRangeOutput**](https://msdn.microsoft.com/library/windows/apps/dn948830) 개체가 포함된 [**SceneAnalysisEffectFrame**](https://msdn.microsoft.com/library/windows/apps/dn948907) 개체가 있습니다. High Dynamic Range 출력의 [**Certainty**](https://msdn.microsoft.com/library/windows/apps/dn948833) 속성은 0과 1.0 사이의 값을 제공합니다(0은 HDR 처리가 캡처 결과 개선에 도움이 되지 않음을 나타내고 1.0은 HDR 처리가 도움이 된다는 것을 나타냄). HDR을 사용하거나 결과를 사용자에게 표시하고 사용자가 결정하도록 할 임계값 포인트를 결정할 수 있습니다.

[!code-cs[SceneAnalyzed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSceneAnalyzed)]

처리기에 전달된 [**HighDynamicRangeOutput**](https://msdn.microsoft.com/library/windows/apps/dn948830) 개체에는 HDR 처리를 위해 가변 사진 시퀀스를 캡처하는 데 권장되는 프레임 컨트롤러가 포함된 [**FrameControllers**](https://msdn.microsoft.com/library/windows/apps/dn948834) 속성도 있습니다. 자세한 내용은 [가변 사진 시퀀스](variable-photo-sequence.md)를 참조하세요.

### <a name="clean-up-the-scene-analysis-effect"></a>장면 분석 효과 정리

앱이 캡처를 완료하면 **MediaCapture** 개체를 처분하기 전에 먼저 효과의 [**HighDynamicRangeAnalyzer.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn948827) 속성을 false로 설정하고 [**SceneAnalyzed**](https://msdn.microsoft.com/library/windows/apps/dn948920) 이벤트 처리기를 등록 취소하여 장면 분석 효과를 사용하지 않도록 설정해야 합니다. [**MediaCapture.ClearEffectsAsync**](https://msdn.microsoft.com/library/windows/apps/br226592)를 호출하고 효과가 추가된 비디오 미리 보기 스트림을 지정합니다. 마지막으로, 멤버 변수를 null로 설정합니다.

[!code-cs[CleanUpSceneAnalysisEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpSceneAnalysisEffectAsync)]

## <a name="face-detection-effect"></a>얼굴 감지 효과

[**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/dn948776)는 미디어 캡처 미리 보기 스트림 내에서 얼굴의 위치를 식별합니다. 이 효과는 미리 보기 스트림에서 얼굴이 감지될 때마다 알림을 받을 수 있게 하며 미리 보기 프레임 내에서 감지된 각 얼굴에 대해 경계 상자를 제공합니다. 지원되는 장치에서 얼굴 감지 효과를 사용하면 장면에서 가장 중요한 얼굴에 대한 노출과 포커스가 향상됩니다.

### <a name="face-detection-namespaces"></a>얼굴 감지 네임스페이스

얼굴 감지를 사용하려면 앱에 기본적인 미디어 캡처에 필요한 네임스페이스 외에도 다음 네임스페이스가 있어야 합니다.

[!code-cs[FaceDetectionUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFaceDetectionUsing)]

### <a name="initialize-the-face-detection-effect-and-add-it-to-the-preview-stream"></a>얼굴 감지 효과를 초기화하고 미리 보기 스트림에 추가

비디오 효과는 두 개의 API, 효과 정의(캡처 장치가 효과를 초기화하는 데 필요한 설정을 제공함), 효과 인스턴스(효과를 제어하는 데 사용할 수 있음)를 사용하여 구현됩니다. 코드 내의 여러 위치에서 효과 인스턴스에 액세스해야 할 수도 있으므로, 일반적으로 개체를 보유하도록 멤버 변수를 선언해야 합니다.

[!code-cs[DeclareFaceDetectionEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareFaceDetectionEffect)]

앱에서 **MediaCapture** 개체를 초기화한 후에 새 [**FaceDetectionEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn948778) 인스턴스를 만듭니다. [**DetectionMode**](https://msdn.microsoft.com/library/windows/apps/dn948781) 속성을 설정하여 얼굴 감지 속도와 얼굴 감지의 정확성 중에서 우선 순위를 지정합니다. 미리 보기 경험이 뚝뚝 끊길 수 있으므로 들어오는 프레임이 얼굴 감지 완료까지 대기하느라 지연되지 않도록 지정하려면 [**SynchronousDetectionEnabled**](https://msdn.microsoft.com/library/windows/apps/dn948786)를 설정합니다.

**MediaCapture** 개체에 대해 [**AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035)를 호출하여 캡처 디바이스에 효과를 등록하고, **FaceDetectionEffectDefinition**을 제공하고 [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640)를 지정하여 캡처 스트림이 아니라 동영상 미리 보기 스트림에 효과를 적용해야 한다는 것을 나타냅니다. **AddVideoEffectAsync**는 추가된 효과의 인스턴스를 반환합니다. 이 메서드는 여러 효과 유형에 사용할 수 있으므로, 반환된 인스턴스를 [**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/dn948776) 개체에 캐스팅해야 합니다.

[**FaceDetectionEffect.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn948818) 속성을 설정하여 효과를 사용하거나 사용하지 않도록 설정합니다. [**FaceDetectionEffect.DesiredDetectionInterval**](https://msdn.microsoft.com/library/windows/apps/dn948814) 속성을 설정하여 효과가 프레임을 분석하는 빈도를 조정합니다. 미디어 캡처가 진행 중인 동안 이 두 속성을 모두 조정할 수 있습니다.

[!code-cs[CreateFaceDetectionEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateFaceDetectionEffectAsync)]

### <a name="receive-notifications-when-faces-are-detected"></a>얼굴이 감지되면 알림 수신

얼굴 감지 시 비디오 미리 보기에서 감지된 얼굴 주변에 상자를 그리는 것과 같은 일부 작업이 수행되도록 하려면 [**FaceDetected**](https://msdn.microsoft.com/library/windows/apps/dn948820) 이벤트에 대해 등록할 수 있습니다.

[!code-cs[RegisterFaceDetectionHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterFaceDetectionHandler)]

이벤트에 대한 처리기에서 [**FaceDetectedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn948774)의 [**FaceDetectionEffectFrame.DetectedFaces**](https://msdn.microsoft.com/library/windows/apps/dn948792) 속성에 액세스하여 프레임에서 감지된 모든 얼굴의 목록을 가져올 수 있습니다. [**FaceBox**](https://msdn.microsoft.com/library/windows/apps/dn974126) 속성은 감지된 얼굴이 포함된 사각형을 미리 보기 스트림 크기에 대해 상대적인 단위로 설명하는 [**BitmapBounds**](https://msdn.microsoft.com/library/windows/apps/br226169) 구조입니다. 미리 보기 스트림 좌표를 화면 좌표로 변환하는 샘플 코드를 보려면 [얼굴 감지 UWP 샘플](http://go.microsoft.com/fwlink/?LinkId=619486)을 참조하세요.

[!code-cs[FaceDetected](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFaceDetected)]

### <a name="clean-up-the-face-detection-effect"></a>얼굴 감지 효과 정리

앱이 캡처를 완료하면 **MediaCapture** 개체를 처분하기 전에 먼저 효과의 [**FaceDetectionEffect.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn948818)를 사용하여 얼굴 감지 효과를 사용하지 않도록 설정하고 이전에 등록해 둔 [**FaceDetected**](https://msdn.microsoft.com/library/windows/apps/dn948820) 이벤트 처리기를 등록 취소해야 합니다. [**MediaCapture.ClearEffectsAsync**](https://msdn.microsoft.com/library/windows/apps/br226592)를 호출하고 효과가 추가된 비디오 미리 보기 스트림을 지정합니다. 마지막으로, 멤버 변수를 null로 설정합니다.

[!code-cs[CleanUpFaceDetectionEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpFaceDetectionEffectAsync)]

### <a name="check-for-focus-and-exposure-support-for-detected-faces"></a>감지된 얼굴에 대한 포커스 및 노출 지원 확인

일부 장치에는 감지된 얼굴을 기반으로 포커스 및 노출을 조정할 수 있는 캡처 장치가 없습니다. 얼굴 감지에는 장치 리소스가 사용되기 때문에, 이 기능을 사용하여 캡처를 향상할 수 있는 장치에서만 얼굴 감지를 사용하도록 설정해야 할 수 있습니다. 얼굴 기반 캡처 최적화를 사용할 수 있는지 여부를 알아보려면 초기화된 [MediaCapture](capture-photos-and-video-with-mediacapture.md)에 대한 [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825)를 가져온 다음 비디오 디바이스 컨트롤러의 [**RegionsOfInterestControl**](https://msdn.microsoft.com/library/windows/apps/dn279064)을 가져옵니다. [**MaxRegions**](https://msdn.microsoft.com/library/windows/apps/dn279069)가 하나 이상의 영역을 지원하는지 확인합니다. 그런 다음, [**AutoExposureSupported**](https://msdn.microsoft.com/library/windows/apps/dn279065) 또는 [**AutoFocusSupported**](https://msdn.microsoft.com/library/windows/apps/dn279066)가 true인지 여부를 확인합니다. 이러한 조건이 충족되는 경우 디바이스는 얼굴 감지를 활용하여 캡처를 향상할 수 있습니다.

[!code-cs[AreFaceFocusAndExposureSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAreFaceFocusAndExposureSupported)]

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




