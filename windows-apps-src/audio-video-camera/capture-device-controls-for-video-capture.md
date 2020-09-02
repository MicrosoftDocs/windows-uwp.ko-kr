---
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: 이 문서에서는 수동 장치 컨트롤을 사용 하 여 HDR 비디오 및 노출 우선 순위를 비롯 한 향상 된 비디오 캡처 시나리오를 사용 하는 방법을 보여 줍니다.
title: 비디오 캡처를 위한 수동 카메라 컨트롤
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ac3a286a8e3961b66a8fd0e4cf20fa7665b6b081
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364016"
---
# <a name="manual-camera-controls-for-video-capture"></a>비디오 캡처를 위한 수동 카메라 컨트롤



이 문서에서는 수동 장치 컨트롤을 사용 하 여 HDR 비디오 및 노출 우선 순위를 비롯 한 향상 된 비디오 캡처 시나리오를 사용 하는 방법을 보여 줍니다.

이 문서에서 설명 하는 비디오 장치 컨트롤은 모두 동일한 패턴을 사용 하 여 앱에 추가 됩니다. 먼저 앱이 실행 되는 현재 장치에서 컨트롤이 지원 되는지 확인 합니다. 컨트롤이 지원 되는 경우 컨트롤에 대 한 원하는 모드를 설정 합니다. 일반적으로 현재 장치에서 특정 컨트롤이 지원 되지 않는 경우 사용자가 기능을 사용할 수 있도록 하는 UI 요소를 사용 하지 않도록 설정 하거나 숨깁니다.

이 문서에서 설명 하는 모든 장치 컨트롤 Api는 [**Windows. Media. Devices**](/uwp/api/Windows.Media.Devices) 네임 스페이스의 멤버입니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetVideoControllersUsing":::

> [!NOTE] 
> 이 문서는 기본 사진 및 비디오 캡처를 구현 하는 단계를 설명 하는 [MediaCapture을 사용 하 여 기본 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 설명 된 개념 및 코드를 기반으로 합니다. 고급 캡처 시나리오로 전환 하기 전에 해당 문서의 기본적인 미디어 캡처 패턴을 숙지 하는 것이 좋습니다. 이 문서의 코드는 앱에 이미 올바르게 초기화 된 MediaCapture 인스턴스가 있다고 가정 합니다.

## <a name="hdr-video"></a>HDR 비디오

HDR (high dynamic range) 비디오 기능은 캡처 장치의 비디오 스트림에 HDR 처리를 적용 합니다. [**HdrVideoControl**](/uwp/api/windows.media.devices.hdrvideocontrol.supported) 속성을 선택 하 여 HDR 비디오가 지원 되는지 확인 합니다.

HDR 비디오 컨트롤은 on, off 및 automatic의 세 가지 모드를 지원 합니다. 즉, 장치는 HDR 비디오 처리로 인해 미디어 캡처가 향상 되는지 여부를 동적으로 결정 하 고, 그럴 경우 HDR 비디오를 사용 하도록 설정 합니다. 특정 모드가 현재 장치에서 지원 되는지 확인 하려면 [**HdrVideoControl**](/uwp/api/windows.media.devices.hdrvideocontrol.supportedmodes) 컬렉션에 원하는 모드가 포함 되어 있는지 확인 하세요.

[**HdrVideoControl**](/uwp/api/windows.media.devices.hdrvideocontrol.mode) 을 원하는 모드로 설정 하 여 HDR 비디오 처리를 사용 하거나 사용 하지 않도록 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSetHdrVideoMode":::

## <a name="exposure-priority"></a>노출 우선 순위

[**ExposurePriorityVideoControl**](/uwp/api/Windows.Media.Devices.ExposurePriorityVideoControl)사용 하도록 설정 되 면 캡처 장치에서 비디오 프레임을 평가 하 여 비디오가 낮은 밝은 장면을 캡처 하 고 있는지 확인 합니다. 그렇다면 각 프레임에 대 한 노출 시간을 늘리고 캡처한 비디오의 시각적 품질을 향상 시키기 위해 캡처된 비디오의 프레임 속도로 줄어듭니다.

[**ExposurePriorityVideoControl**](/uwp/api/windows.media.devices.exposurepriorityvideocontrol.supported) 속성을 확인 하 여 현재 장치에서 노출 우선 순위 컨트롤이 지원 되는지 여부를 확인 합니다.

[**ExposurePriorityVideoControl**](/uwp/api/windows.media.devices.exposurepriorityvideocontrol.enabled) 을 원하는 모드로 설정 하 여 노출 우선 순위 컨트롤을 사용 하거나 사용 하지 않도록 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetEnableExposurePriority":::

## <a name="temporal-denoising"></a>임시 denoising
Windows 10 버전 1803부터 비디오를 지 원하는 장치에서 비디오에 대 한 임시 denoising를 사용 하도록 설정할 수 있습니다. 이 기능은 인접 한 여러 프레임의 이미지 데이터를 실시간으로 결합 시각적 노이즈가 떨어지는 비디오 프레임을 생성 합니다.

앱은 [**VideoTemporalDenoisingControl**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol) 를 사용 하 여 현재 장치에서 임시 denoising이 지원 되는지 여부를 확인 하 고, 있는 경우 지원 되는 denoising 모드를 확인할 수 있습니다. 사용 가능한 denoising 모드는 [**Off**](/uwp/api/windows.media.devices.videotemporaldenoisingmode), [**On**](/uwp/api/windows.media.devices.videotemporaldenoisingmode)및 [**Auto**](/uwp/api/windows.media.devices.videotemporaldenoisingmode)입니다. 장치에서 일부 모드를 지원 하지 않을 수 있지만 모든 장치에서 **자동** 또는 **켜기** 및 **끄기**를 지원 해야 합니다.

다음 예제에서는 간단한 UI를 사용 하 여 사용자가 denoising 모드 간을 전환할 수 있는 라디오 단추를 제공 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetDenoiseXAML":::

다음 방법에서는 [**VideoTemporalDenoisingControl**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.supported) 속성을 확인 하 여 현재 장치에서 임시 denoising가 모두 지원 되는지 확인 합니다. 그렇다면 **Off** 및 **Auto** 또는 **On** 이 지원 되는지 확인 합니다 .이 경우 라디오 단추가 표시 됩니다. 그런 다음 이러한 메서드가 지원 되는 경우 **자동** 및 **켜기** 단추가 표시 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetUpdateDenoiseCapabilities":::

라디오 단추에 대해 **checked** 이벤트 처리기에서 단추의 이름을 선택 하 고 [**VideoTemporalDenoisingControl**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.mode) 속성을 설정 하 여 해당 모드를 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetDenoiseButtonChecked":::

### <a name="disabling-temporal-denoising-while-processing-frames"></a>프레임을 처리 하는 동안 임시 denoising 사용 안 함
임시 denoising를 사용 하 여 처리 된 비디오는 인간 눈에 더 보기 편 수 있습니다. 그러나 temporal denoising는 이미지 일관성에 영향을 줄 수 있고 프레임의 세부 정보의 양을 낮출 수 있으므로, 이미지 처리를 사용 하도록 설정 하면 프레임에서 이미지 처리를 수행 하는 앱 (예: 등록 또는 광학 인식)에서 프로그래밍 방식으로 denoising을 사용 하지 않도록 설정할 수 있습니다.

다음 예에서는 지원 되는 denoising 모드를 확인 하 고 일부 클래스 변수에이 정보를 저장 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetDenoiseFrameReaderVars":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetDenoiseCapabilitiesForFrameProcessing":::

앱에서 프레임 처리를 사용 하는 경우 프레임 처리가 denoised 되지 않은 원시 프레임을 사용할 수 있도록 해당 모드가 지원 되는 경우 denoising 모드를 **Off** 로 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetEnableFrameProcessing":::

앱에서 프레임 prcessing를 사용 하지 않도록 설정 하면 지원 되는 모드에 따라 denoising 모드가 **켜기** 또는 **자동**으로 설정 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetDisableFrameProcessing":::

이미지 처리를 위한 비디오 프레임을 얻는 방법에 대 한 자세한 내용은 [MediaFrameReader를 사용 하 여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md)를 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [MediaFrameReader를 사용하여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md)
*  [**VideoTemporalDenoisingControl**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol)
 
