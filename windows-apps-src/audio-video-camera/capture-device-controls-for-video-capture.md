---
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: 이 문서에서는 수동 디바이스 컨트롤을 사용하여 HDR 비디오, 노출 우선 순위 등의 향상된 비디오 캡처 시나리오를 가능하게 하는 방법을 보여 줍니다.
title: 비디오 캡처를 위한 수동 카메라 컨트롤
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f144ef398fc55e79d2f0190c61214cdf1aa93b68
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8195074"
---
# <a name="manual-camera-controls-for-video-capture"></a>비디오 캡처를 위한 수동 카메라 컨트롤



이 문서에서는 수동 디바이스 컨트롤을 사용하여 HDR 비디오, 노출 우선 순위 등의 향상된 비디오 캡처 시나리오를 가능하게 하는 방법을 보여 줍니다.

이 문서에서 설명하는 비디오 디바이스 컨트롤은 모두 동일한 패턴을 사용하여 앱에 추가됩니다. 먼저, 앱이 실행 중인 현재 디바이스에서 컨트롤이 지원되는지를 확인합니다. 컨트롤이 지원되는 경우 컨트롤에 대해 원하는 모드를 설정합니다. 일반적으로 특정 컨트롤이 현재 디바이스에서 지원되지 않으면 사용자가 기능을 사용하도록 설정할 수 있는 UI 요소를 사용하지 않도록 설정하거나 숨겨야 합니다.

이 문서에서 다루는 모든 API 디바이스 컨트롤은 [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902) 네임스페이스의 구성원입니다.

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

> [!NOTE] 
> 이 문서는 기본 사진 및 비디오 캡처 구현 단계를 설명하는 [MediaCapture를 사용한 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 설명된 개념 및 코드를 토대로 작성되었습니다. 보다 수준 높은 캡처 시나리오를 진행하기 전에 해당 문서의 기본적인 미디어 캡처 패턴을 파악하는 것이 좋습니다. 이 문서의 코드는 앱에 적절히 초기화된 MediaCapture의 인스턴스가 이미 있다고 가정합니다.

## <a name="hdr-video"></a>HDR 비디오

HDR(High Dynamic Range) 비디오 기능은 캡처 디바이스의 비디오 스트림에 HDR 처리를 적용합니다. [**HdrVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926682) 속성을 확인하여 HDR 비디오가 지원되는지 검토합니다.

HDR 비디오 컨트롤은 세 가지 모드인 켜짐, 꺼짐 및 자동을 지원합니다. 즉, 디바이스는 HDR 비디오 처리가 미디어 캡처를 향상시키는지를 동적으로 확인하고 향상시킬 경우 HDR 비디오를 사용하도록 설정합니다. 현재 디바이스에서 특정 모드가 지원되는지 확인하려면 [**HdrVideoControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926683) 컬렉션이 원하는 모드를 포함하는지 확인합니다.

[**HdrVideoControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926681)를 원하는 모드로 설정하여 HDR 비디오 처리를 사용하거나 사용하지 않도록 설정합니다.

[!code-cs[SetHdrVideoMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetHdrVideoMode)]

## <a name="exposure-priority"></a>노출 우선 순위

[**ExposurePriorityVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926644)을 사용할 경우 캡처 디바이스의 비디오 프레임을 평가하여 비디오가 낮은 조명 장면을 캡처하는지를 확인합니다. 따라서 이 컨트롤은 각 프레임의 노출 시간을 늘리고 캡처된 비디오의 시각적 품질을 개선하기 위해 캡처한 비디오의 프레임 속도를 낮춥니다.

[**ExposurePriorityVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926647) 속성을 확인하여 현재 디바이스에서 노출 우선 순위 컨트롤이 지원되는지를 확인합니다.

[**ExposurePriorityVideoControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926646)를 원하는 모드로 설정하여 노출 우선순위 컨트롤을 사용하거나 사용하지 않도록 설정합니다.

[!code-cs[EnableExposurePriority](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEnableExposurePriority)]

## <a name="temporal-denoising"></a>임시 디노이징
Windows 10, 버전 1803부터는 비디오를 지원하는 장치에서 비디오를 일시적으로 디노이징할 수 있습니다. 이 기능은 실시간으로 여러 인접 프레임의 이미지 데이터를 융합하여 시각적 노이즈가 적은 비디오 프레임을 생성합니다.

[**VideoTemporalDenoisingControl**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol)을 통해  앱은 현재 디바이스에서 임시 디노이징 기능이 지원되는지 확인할 수 있고 그럴 경우 지원되는 디노이징 모드를 알할 수 있습니다. 사용 가능한 디노이징 모드는 [**Off**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode), [**On**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode), [**Auto**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode)입니다. 디바이스에서 모든 모드를 지원하지 않을 수 있지만, 모든 디바이스는 **Auto** 또는 **On** 및 **Off**를 지원해야 합니다.

다음 예제에서는 간단한 UI를 사용하여 사용자가 다른 디노이징 모드로 전환할 수 있는 라디오 버튼을 제공합니다.

[!code-cs[SnippetDenoiseXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetDenoiseXAML)]

다음 메서드에서 [**VideoTemporalDenoisingControl.Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.supported) 속성을 확인하여 임시 디노이징이 현재 디바이스에서 지원되고 있는지 확인할 수 있습니다. 지원되는 경우, **Off** 및 **Auto** 또는 **On**이 지원되는지 확인하고 이 경우에 라디오 단추가 표시되는지도 확인합니다. 그 다음 이 메서드가 지원되면 **Auto** 및 **On** 단추가 표시되도록 합니다.

[!code-cs[SnippetUpdateDenoiseCapabilities](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetUpdateDenoiseCapabilities)]

라디오 단추의 **Checked** 이벤트 처리기에서 이 단추의 이름을 확인하고 [**VideoTemporalDenoisingControl.Mode**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.mode) 속성을 설정하여 해당 모드를 설정합니다.

[!code-cs[SnippetDenoiseButtonChecked](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseButtonChecked)]

### <a name="disabling-temporal-denoising-while-processing-frames"></a>프레임을 처리하는 동안 임시 디노이징 비활성화
임시 디노이징 기능을 사용하여 처리된 비디오는 인간의 눈에 더욱 만족스럽게 보일 수 있습니다. 그러나 임시 디노이징이 이미지 일관성에 영향을 미치고 프레임의 세부 사항을 감소시킬 수 있으므로 등록 또는 광학 문자 인식과 같이 프레임에서 이미지 처리를 수행하는 응용 프로그램은 이미지 처리가 활성화될 때 프로그래밍 방식으로 비노이징 기능을 비활성화할 수 있습니다.

다음 예제는 지원되는 디노이징 모드를 결정하고 일부 클래스 변수에 이 정보를 저장합니다.

[!code-cs[SnippetDenoiseFrameReaderVars](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseFrameReaderVars)]

[!code-cs[SnippetDenoiseCapabilitiesForFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseCapabilitiesForFrameProcessing)]

앱이 프레임 처리를 지원할 때 프레임 처리에서 디노이징되지 않은 원시 프레임을 사용할 수 있도록 디노이징 모드를 지원하면 앱은 디노이징 모드를 **Off**로 설정합니다.

[!code-cs[SnippetEnableFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEnableFrameProcessing)]

앱에서 프레임 처리를 비활성화하면 지원되는 모드에 따라 앱은 디노이징 모드를 **On** 또는 **Auto**로 설정합니다.

[!code-cs[SnippetDisableFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDisableFrameProcessing)]

이미지 처리를 위한 비디오 프레임을 가져오는 방법에 대한 자세한 내용은 [MediaFrameReader를 사용하여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md)를 참조하세요.

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [MediaFrameReader를 사용하여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md)
*  [**VideoTemporalDenoisingControl**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol)
 




