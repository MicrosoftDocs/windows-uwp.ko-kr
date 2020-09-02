---
ms.assetid: 831123A7-1F40-4B74-AE9F-69AC9883B4AD
description: 이 문서에서는 수동 장치 컨트롤을 사용 하 여 광학 이미지 안정화 및 부드러운 확대/축소를 비롯 한 향상 된 사진 및 비디오 캡처 시나리오를 사용 하는 방법을 보여 줍니다.
title: 사진 및 비디오 캡처를 위한 수동 카메라 컨트롤
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d7248b4f3fe515a164410305bac074f44cae53e6
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362866"
---
# <a name="manual-camera-controls-for-photo-and-video-capture"></a>사진 및 비디오 캡처를 위한 수동 카메라 컨트롤



이 문서에서는 수동 장치 컨트롤을 사용 하 여 광학 이미지 안정화 및 부드러운 확대/축소를 비롯 한 향상 된 사진 및 비디오 캡처 시나리오를 사용 하는 방법을 보여 줍니다.

이 문서에서 설명 하는 컨트롤은 모두 동일한 패턴을 사용 하 여 앱에 추가 됩니다. 먼저 앱이 실행 되는 현재 장치에서 컨트롤이 지원 되는지 확인 합니다. 컨트롤이 지원 되는 경우 컨트롤에 대 한 원하는 모드를 설정 합니다. 일반적으로 현재 장치에서 특정 컨트롤이 지원 되지 않는 경우 사용자가 기능을 사용할 수 있도록 하는 UI 요소를 사용 하지 않도록 설정 하거나 숨깁니다.

이 문서의 코드는 [카메라 수동 컨트롤 SDK 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraManualControls)에서 도입 되었습니다. 샘플을 다운로드 하 여 컨텍스트에서 사용 되는 코드를 확인 하거나 사용자 고유의 앱에 대 한 시작 지점으로 샘플을 사용할 수 있습니다.

> [!NOTE]
> 이 문서는 기본 사진 및 비디오 캡처를 구현 하는 단계를 설명 하는 [MediaCapture을 사용 하 여 기본 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 설명 된 개념 및 코드를 기반으로 합니다. 고급 캡처 시나리오로 전환 하기 전에 해당 문서의 기본적인 미디어 캡처 패턴을 숙지 하는 것이 좋습니다. 이 문서의 코드는 앱에 이미 올바르게 초기화 된 MediaCapture 인스턴스가 있다고 가정 합니다.

이 문서에서 설명 하는 모든 장치 컨트롤 Api는 [**Windows. Media. Devices**](/uwp/api/Windows.Media.Devices) 네임 스페이스의 멤버입니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetVideoControllersUsing":::

## <a name="exposure"></a>노출을

[**ExposureControl**](/uwp/api/Windows.Media.Devices.ExposureControl) 를 사용 하면 사진 또는 비디오 캡처 중에 사용 되는 셔터 속도를 설정할 수 있습니다.

이 예제에서는 [**슬라이더**](/uwp/api/Windows.UI.Xaml.Controls.Slider) 컨트롤을 사용 하 여 현재 노출 값을 조정 하 고 확인란을 사용 하 여 자동 노출 조정을 설정/해제 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetExposureXAML":::

[**지원 되**](/uwp/api/windows.media.devices.exposurecontrol.supported) 는 속성을 확인 하 여 현재 캡처 장치가 **ExposureControl** 을 지원 하는지 확인 합니다. 컨트롤이 지원 되는 경우이 기능에 대 한 UI를 표시 하 고 사용 하도록 설정할 수 있습니다. 자동 노출 조정이 현재 [**Auto**](/uwp/api/windows.media.devices.exposurecontrol.auto) 속성의 값에 대해 활성 상태 인지 여부를 나타내도록 확인란의 선택 상태를 설정 합니다.

노출 값은 장치에서 지 원하는 범위 내에 있어야 하며, 지원 되는 단계 크기의 증가값 이어야 합니다. 슬라이더 컨트롤의 해당 속성을 설정 하는 데 사용 되는 [**Min**](/uwp/api/windows.media.devices.exposurecontrol.min), [**Max**](/uwp/api/windows.media.devices.exposurecontrol.max)및 [**Step**](/uwp/api/windows.media.devices.exposurecontrol.step) 속성을 확인 하 여 현재 장치에 대해 지원 되는 값을 가져옵니다.

[**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) 이벤트 처리기의 등록을 취소 한 후 slider 컨트롤의 값을 **ExposureControl** 의 현재 값으로 설정 하 여 값이 설정 되 면 이벤트가 트리거되지 않도록 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetExposureControl":::

**ValueChanged** 이벤트 처리기에서 컨트롤의 현재 값을 가져오고 [**SetValueAsync**](/uwp/api/windows.media.devices.exposurecontrol.setvalueasync)를 호출 하 여 노출 값을 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetExposureSlider":::

자동 노출 확인란의 **CheckedChanged** 이벤트 처리기에서 [**setautoasync**](/uwp/api/windows.media.devices.exposurecontrol.setautoasync) 를 호출 하 고 부울 값을 전달 하 여 자동 노출 조정을 설정 하거나 해제 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetExposureCheckBox":::

> [!IMPORTANT]
> 자동 노출 모드는 미리 보기 스트림이 실행 되는 동안에만 지원 됩니다. 자동 노출을 설정 하기 전에 미리 보기 스트림이 실행 중인지 확인 합니다.

## <a name="exposure-compensation"></a>노출 보정

[**ExposureCompensationControl**](/uwp/api/Windows.Media.Devices.ExposureCompensationControl) 를 사용 하면 사진 또는 비디오 캡처 중에 사용 되는 노출 보정을 설정할 수 있습니다.

이 예제에서는 [**슬라이더**](/uwp/api/Windows.UI.Xaml.Controls.Slider) 컨트롤을 사용 하 여 현재 노출 보정 값을 조정 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetEvXAML":::

[지원 되](supported-codecs.md) 는 속성을 확인 하 여 현재 캡처 장치가 **ExposureCompensationControl** 을 지원 하는지 확인 합니다. 컨트롤이 지원 되는 경우이 기능에 대 한 UI를 표시 하 고 사용 하도록 설정할 수 있습니다.

노출 보정 값은 장치에서 지 원하는 범위 내에 있어야 하며, 지원 되는 단계 크기의 증가값 이어야 합니다. 슬라이더 컨트롤의 해당 속성을 설정 하는 데 사용 되는 [**Min**](/uwp/api/windows.media.devices.exposurecompensationcontrol.min), [**Max**](/uwp/api/windows.media.devices.exposurecompensationcontrol.max)및 [**Step**](/uwp/api/windows.media.devices.exposurecompensationcontrol.step) 속성을 확인 하 여 현재 장치에 대해 지원 되는 값을 가져옵니다.

[**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) 이벤트 처리기의 등록을 취소 한 후 slider 컨트롤의 값을 **ExposureCompensationControl** 의 현재 값으로 설정 하 여 값이 설정 되 면 이벤트가 트리거되지 않도록 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetEvControl":::

**ValueChanged** 이벤트 처리기에서 컨트롤의 현재 값을 가져오고 [**SetValueAsync**](/uwp/api/windows.media.devices.exposurecompensationcontrol.setvalueasync)를 호출 하 여 노출 값을 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetEvValueChanged":::

## <a name="flash"></a>깜박임

[**FlashControl**](/uwp/api/Windows.Media.Devices.FlashControl) 를 사용 하면 플래시를 사용 하거나 사용 하지 않도록 설정할 수 있으며, 시스템에서 플래시를 사용할지 여부를 동적으로 결정 하는 자동 플래시를 사용 하도록 설정할 수 있습니다. 이 컨트롤을 사용 하면 장치를 지 원하는 장치에서 자동 적목 저하를 사용할 수도 있습니다. 이러한 설정은 모두 사진 캡처에 적용 됩니다. [**Torchcontrol**](/uwp/api/Windows.Media.Devices.TorchControl) 은 비디오 캡처를 위해 torch를 켜고 끌 수 있는 별도의 컨트롤입니다.

이 예제에서는 사용자가 설정, 해제 및 자동 플래시 설정 간을 전환할 수 있도록 하는 라디오 단추 집합을 사용 합니다. 빨간색 눈을 줄이고 비디오 torch를 전환할 수 있는 checkbox도 제공 됩니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetFlashXAML":::

[**지원 되**](/uwp/api/windows.media.devices.focuscontrol.supported) 는 속성을 확인 하 여 현재 캡처 장치가 **FlashControl** 을 지원 하는지 확인 합니다. 컨트롤이 지원 되는 경우이 기능에 대 한 UI를 표시 하 고 사용 하도록 설정할 수 있습니다. **FlashControl** 이 지원 되는 경우 자동 적목 현상이 지원 될 수도 있고 지원 되지 않을 수도 있으므로 UI를 사용 하도록 설정 하기 전에 [**RedEyeReductionSupported**](/uwp/api/windows.media.devices.flashcontrol.redeyereductionsupported) 속성을 확인 합니다. **Torchcontrol** 은 flash 컨트롤과 별개 이므로 [**지원 되**](/uwp/api/windows.media.devices.torchcontrol.supported) 는 속성을 사용 하기 전에 확인 해야 합니다.

각 flash 라디오 단추에 대해 [**확인**](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) 된 이벤트 처리기에서 적절 한 해당 플래시 설정을 사용 하거나 사용 하지 않도록 설정 합니다. 플래시를 항상 사용 하도록 설정 하려면 [**Enabled**](/uwp/api/windows.media.devices.flashcontrol.enabled) 속성을 true로 설정 하 고 [**Auto**](/uwp/api/windows.media.devices.flashcontrol.auto) 속성을 false로 설정 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFlashControl":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFlashRadioButtons":::

Red eye reduction에 대 한 처리기 확인란에서 [**RedEyeReduction**](/uwp/api/windows.media.devices.flashcontrol.redeyereduction) 속성을 적절 한 값으로 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetRedEye":::

마지막으로 video torch에 대 한 처리기 확인란에서 [**Enabled**](/uwp/api/windows.media.devices.torchcontrol.enabled) 속성을 적절 한 값으로 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTorch":::

> [!NOTE] 
>  일부 장치에서는 torch가 true로 설정 된 경우에도, 장치에 미리 보기 스트림이 실행 중이 고 비디오를 캡처하는 경우를 제외 하 고는 빛을 내보내지 않습니다 [**.**](/uwp/api/windows.media.devices.torchcontrol.enabled) 권장 되는 작업 순서는 video preview를 켜고 **torch를 true로 설정한** 다음 비디오 캡처를 시작 하는 것입니다. 일부 장치에서는 미리 보기가 시작 된 후 torch가 켜 집니다. 다른 장치에서 torch는 비디오 캡처가 시작 될 때까지 작동 하지 않을 수 있습니다.

## <a name="focus"></a>포커스

카메라의 포커스를 조정 하는 데 일반적으로 사용 되는 세 가지 방법은 [**FocusControl**](/uwp/api/Windows.Media.Devices.FocusControl) 개체, 연속 autofocus, 포커스를 위한 탭 및 수동 포커스에 의해 지원 됩니다. 카메라 앱은 이러한 세 가지 방법을 모두 지원할 수 있지만 가독성을 높이기 위해이 문서에서는 각 기술에 대해 개별적으로 설명 합니다. 이 섹션에서는 포커스 지원 조명을 사용 하도록 설정 하는 방법에 대해서도 설명 합니다.

### <a name="continuous-autofocus"></a>연속 autofocus

연속 autofocus를 사용 하도록 설정 하면 카메라에서 포커스를 동적으로 조정 하 여 사진 또는 비디오의 주제를 집중적으로 유지 합니다. 이 예제에서는 라디오 단추를 사용 하 여 연속 autofocus를 설정/해제 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetCAFXAML":::

[**지원 되**](/uwp/api/windows.media.devices.flashcontrol.supported) 는 속성을 확인 하 여 현재 캡처 장치가 **FocusControl** 을 지원 하는지 확인 합니다. 그런 다음 [**SupportedFocusModes**](/uwp/api/windows.media.devices.focuscontrol.supportedfocusmodes) 목록을 확인 하 여 연속 autofocus가 지원 되는지 확인 하 여 [**FocusMode**](/uwp/api/Windows.Media.Devices.FocusMode)값이 포함 되어 있는지 확인 하 고, 그렇다면 연속 autofocus 라디오 단추를 표시 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetCAF":::

연속 autofocus 라디오 단추에 대해 [**Checked**](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) 이벤트 처리기에서 [**VideoDeviceController**](/uwp/api/windows.media.devices.videodevicecontroller.focuscontrol) 속성을 사용 하 여 컨트롤의 인스턴스를 가져옵니다. 앱이 이전에 [**Lockasync**](/uwp/api/windows.media.devices.focuscontrol.lockasync) 를 호출 하 여 다른 포커스 모드 중 하나를 사용 하도록 설정한 경우에는 [**unlockasync**](/uwp/api/windows.media.devices.focuscontrol.unlockasync) 를 호출 하 여 컨트롤의 잠금을 해제 합니다.

새 [**FocusSettings**](/uwp/api/Windows.Media.Devices.FocusSettings) 개체를 만들고 [**Mode**](/uwp/api/windows.media.devices.focussettings.mode) 속성을 **Continuous**로 가져옵니다. [**AutoFocusRange**](/uwp/api/windows.media.devices.focussettings.autofocusrange) 속성을 앱 시나리오에 적합 한 값으로 설정 하거나 UI에서 사용자가 선택 합니다. **FocusSettings** 개체를 [**Configure**](/uwp/api/windows.media.devices.focuscontrol.configure) 메서드에 전달 하 고 [**FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync) 를 호출 하 여 연속 autofocus를 시작 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetCafFocusRadioButton":::

> [!IMPORTANT]
> 자동 포커스 모드는 미리 보기 스트림이 실행 되는 동안에만 지원 됩니다. 연속 autofocus를 설정 하기 전에 미리 보기 스트림이 실행 중인지 확인 합니다.

### <a name="tap-to-focus"></a>포커스를 누르기

[**FocusControl**](/uwp/api/Windows.Media.Devices.FocusControl) 및 [**RegionsOfInterestControl**](/uwp/api/Windows.Media.Devices.RegionsOfInterestControl) 를 사용 하 여 캡처 장치에서 포커스를 지정 하는 캡처 프레임 영역을 지정 합니다. 포커스 영역은 미리 보기 스트림을 표시 하는 화면을 누르는 사용자에 의해 결정 됩니다.

이 예제에서는 라디오 단추를 사용 하 여 포커스 이동 모드를 사용 하거나 사용 하지 않도록 설정 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetTapFocusXAML":::

[**지원 되**](/uwp/api/windows.media.devices.flashcontrol.supported) 는 속성을 확인 하 여 현재 캡처 장치가 **FocusControl** 을 지원 하는지 확인 합니다. 이 기술을 사용하려면 **RegionsOfInterestControl**이 지원되어야 하며 이 컨트롤에서 하나 이상의 영역을 지원해야 합니다. [**AutoFocusSupported**](/uwp/api/windows.media.devices.regionsofinterestcontrol.autofocussupported) 및 [**maxregions**](/uwp/api/windows.media.devices.regionsofinterestcontrol.maxregions) 속성을 확인 하 여 눌러서 포커스에 대 한 라디오 단추를 표시 하거나 숨길지 여부를 결정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapFocus":::

포커스를 VideoDeviceController 라디오 단추에 대해 [**확인**](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) 된 이벤트 처리기에서 [**FocusControl**](/uwp/api/windows.media.devices.videodevicecontroller.focuscontrol) 속성을 사용 하 여 컨트롤의 인스턴스를 가져옵니다. 앱이 이전에 [**Unlockasync**](/uwp/api/windows.media.devices.focuscontrol.unlockasync) 를 호출 하 여 연속 autofocus를 사용 하도록 설정한 경우에는 [**lockasync**](/uwp/api/windows.media.devices.focuscontrol.lockasync) 를 호출 하 여 컨트롤을 잠그고, 사용자가 화면을 눌러 포커스를 변경할 때까지 기다립니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapFocusRadioButton":::

이 예에서는 사용자가 화면을 탭 할 때 영역에 초점을 맞춘 다음 사용자가 다시 탭 하 여 전환 하는 경우와 같이 해당 지역에서 포커스를 제거 합니다. 부울 변수를 사용 하 여 현재 전환 된 상태를 추적 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsFocused":::

다음 단계는 사용자가 현재 캡처 미리 보기 스트림을 표시 하는 [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) 의 [**탭**](/uwp/api/windows.ui.xaml.uielement.tapped) 이벤트를 처리 하 여 화면을 누를 때 이벤트를 수신 하는 것입니다. 카메라에서 현재 미리 보기를 수행 하지 않는 경우 또는 탭 대 포커스 모드를 사용 하지 않도록 설정한 경우에는 아무것도 수행 하지 않고 처리기에서 반환 합니다.

추적 변수 * \_ isfocused* 가 false로 전환 되 고 카메라가 현재 포커스 ( **FocusControl**의 [**FocusState**](/uwp/api/windows.media.devices.focuscontrol.focusstate) 속성으로 결정 됨)에 있지 않은 경우에는 포커스 이동 프로세스를 시작 합니다. 처리기로 전달 되는 이벤트 인수에서 사용자 탭의 위치를 가져옵니다. 또한이 예제에서는이 기회를 사용 하 여 집중 될 영역의 크기를 선택 합니다. 이 경우 크기는 캡처 요소의 가장 작은 차원의 1/4입니다. 다음 섹션에 정의 된 **TapToFocus** 도우미 메서드에 탭 위치 및 지역 크기를 전달 합니다.

* \_ Isfocused* 토글이 true로 설정 된 경우 사용자 탭은 이전 지역에서 포커스를 지워야 합니다. 이 작업은 아래에 표시 된 **TapUnfocus** 도우미 메서드에서 수행 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapFocusPreviewControl":::

**TapToFocus** helper 메서드에서 먼저 * \_ isfocused* 토글을 true로 설정 하 여 다음 화면 탭이 탭 영역에서 포커스를 해제 하도록 합니다.

이 도우미 메서드의 다음 작업은 포커스 컨트롤에 할당 될 미리 보기 스트림 내에서 사각형을 결정 하는 것입니다. 이 작업에는 두 단계가 필요 합니다. 첫 번째 단계는 미리 보기 스트림이 [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) 컨트롤 내에서 차지 하는 사각형을 결정 하는 것입니다. 이는 미리 보기 스트림의 차원과 장치의 방향에 따라 달라 집니다. 이 단원의 끝에 표시 되는 도우미 메서드 **GetPreviewStreamRectInControl**는이 작업을 수행 하 고 미리 보기 스트림을 포함 하는 사각형을 반환 합니다.

**TapToFocus** 의 다음 작업은 **CaptureElement** 이벤트 처리기 내에서 확인 된 탭 위치 및 원하는 포커스 사각형 크기를 캡처 스트림 내 좌표로 변환 하는 것입니다. 이 단원의 뒷부분에 나오는 **ConvertUiTapToPreviewRect** 도우미 메서드는이 변환을 수행 하 고 캡처 스트림 좌표에서 포커스가 요청 되는 사각형을 반환 합니다.

이제 대상 사각형을 얻 [**었으 며**](/uwp/api/Windows.Media.Devices.RegionOfInterest) , 경계 속성을 이전 단계에서 얻은 대상 사각형으로 설정 하 여 새 [**영역**](/uwp/api/windows.media.devices.regionofinterest.bounds) 을 만듭니다.

캡처 장치의 [**FocusControl**](/uwp/api/Windows.Media.Devices.FocusControl)를 가져옵니다. **FocusControl**에서 지원 되는지 확인 한 후 새 [**FocusSettings**](/uwp/api/Windows.Media.Devices.FocusSettings) 개체를 만들고 [**모드**](/uwp/api/windows.media.devices.focuscontrol.mode) 와 [**AutoFocusRange**](/uwp/api/windows.media.devices.focussettings.autofocusrange) 를 원하는 값으로 설정 합니다. **FocusControl** 에 대해 [**Configure**](/uwp/api/windows.media.devices.focuscontrol.configure) 를 호출 하 여 설정을 활성 상태로 만들고 장치에 지정 된 지역에 집중 하도록 신호를 보낼 수 있습니다.

다음으로, 캡처 장치의 [**RegionsOfInterestControl**](/uwp/api/Windows.Media.Devices.RegionsOfInterestControl) 를 가져오고 [**SetRegionsAsync**](/uwp/api/windows.media.devices.regionsofinterestcontrol.setregionsasync) 를 호출 하 여 활성 영역을 설정 합니다. 여러 관심 영역을 지 원하는 장치에서 설정할 수 있지만,이 예제에서는 단일 영역만 설정 합니다.

마지막으로 **FocusControl** 에서 [**FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync) 를 호출 하 여 포커스를 시작 합니다.

> [!IMPORTANT]
> 포커스를 누르기를 구현할 때 작업 순서는 중요 합니다. 다음 순서에 따라 이러한 Api를 호출 해야 합니다.
>
> 1. [**FocusControl.Config.**](/uwp/api/windows.media.devices.focuscontrol.configure)
> 2. [**RegionsOfInterestControl.SetRegionsAsync**](/uwp/api/windows.media.devices.regionsofinterestcontrol.setregionsasync)
> 3. [**FocusControl.FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync)

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapToFocus":::

**TapUnfocus** Helper 메서드에서 **RegionsOfInterestControl** 를 가져오고 [**ClearRegionsAsync**](/uwp/api/windows.media.devices.regionsofinterestcontrol.clearregionsasync) 를 호출 하 여 **TapToFocus** helper 메서드 내에서 컨트롤에 등록 된 영역을 지웁니다. 그런 다음 **FocusControl** 를 가져오고 [**FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync) 를 호출 하 여 관심 영역 없이 장치를 refocus 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapUnfocus":::

**GetPreviewStreamRectInControl** 도우미 메서드는 미리 보기 스트림의 해상도와 장치의 방향을 사용 하 여 미리 보기 스트림을 포함 하는 미리 보기 요소 내에서 사각형을 확인 하 고, 해당 컨트롤이 스트림의 가로 세로 비율을 유지 하기 위해 제공할 수 있는 letterboxed 안쪽 여백을 자릅니다. 이 메서드는 [기본 사진, 비디오 및 MediaCapture를 사용한 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 있는 기본 미디어 캡처 예제 코드에 정의 된 클래스 멤버 변수를 사용 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetGetPreviewStreamRectInControl":::

**ConvertUiTapToPreviewRect** 도우미 메서드는 탭 이벤트의 위치, 원하는 포커스 영역 크기 및 **GetPreviewStreamRectInControl** helper 메서드에서 가져온 미리 보기 스트림을 포함 하는 사각형을 인수로 사용 합니다. 이 메서드는 이러한 값과 장치의 현재 방향을 사용 하 여 원하는 지역이 포함 된 미리 보기 스트림 내에서 사각형을 계산 합니다. 다시 한 번이 메서드는 [사진 캡처 및 MediaCapture로 비디오](./index.md)에 있는 기본 미디어 캡처 예제 코드에 정의 된 클래스 멤버 변수를 사용 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetConvertUiTapToPreviewRect":::

### <a name="manual-focus"></a>수동 포커스

수동 포커스 기술은 **슬라이더** 컨트롤을 사용 하 여 캡처 장치의 현재 포커스 깊이를 설정 합니다. 라디오 단추는 수동 포커스를 설정/해제 하는 데 사용 됩니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetManualFocusXAML":::

[**지원 되**](/uwp/api/windows.media.devices.focuscontrol.supported) 는 속성을 확인 하 여 현재 캡처 장치가 **FocusControl** 을 지원 하는지 확인 합니다. 컨트롤이 지원 되는 경우이 기능에 대 한 UI를 표시 하 고 사용 하도록 설정할 수 있습니다.

포커스 값은 장치에서 지 원하는 범위 내에 있어야 하며, 지원 되는 단계 크기의 증가값 이어야 합니다. 슬라이더 컨트롤의 해당 속성을 설정 하는 데 사용 되는 [**Min**](/uwp/api/windows.media.devices.focuscontrol.min), [**Max**](/uwp/api/windows.media.devices.focuscontrol.max)및 [**Step**](/uwp/api/windows.media.devices.focuscontrol.step) 속성을 확인 하 여 현재 장치에 대해 지원 되는 값을 가져옵니다.

[**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) 이벤트 처리기의 등록을 취소 한 후 slider 컨트롤의 값을 **FocusControl** 의 현재 값으로 설정 하 여 값이 설정 되 면 이벤트가 트리거되지 않도록 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocus":::

응용 프로그램에서 이전에 [**Unlockasync**](/uwp/api/windows.media.devices.focuscontrol.unlockasync)호출을 통해 포커스를 잠금 해제 한 경우에는 수동 포커스 라디오 단추에 대해 **Checked** 이벤트 처리기에서 **FocusControl** 개체를 가져오고 [**lockasync**](/uwp/api/windows.media.devices.focuscontrol.lockasync) 를 호출 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetManualFocusChecked":::

수동 포커스 슬라이더의 **ValueChanged** 이벤트 처리기에서 컨트롤의 현재 값을 가져오고 [**SetValueAsync**](/uwp/api/windows.media.devices.focuscontrol.setvalueasync)를 호출 하 여 포커스 값을 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocusSlider":::

### <a name="enable-the-focus-light"></a>포커스 조명 사용

이를 지 원하는 장치에서 포커스 지원 조명을 사용 하도록 설정 하 여 장치에 초점을 맞출 수 있습니다. 이 예제에서는 checkbox를 사용 하 여 focus 지원 조명을 활성화 하거나 비활성화 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetFocusLightXAML":::

[**지원 되**](/uwp/api/windows.media.devices.flashcontrol.supported) 는 속성을 확인 하 여 현재 캡처 장치가 **FlashControl** 을 지원 하는지 확인 합니다. 또한 [**AssistantLightSupported**](/uwp/api/windows.media.devices.flashcontrol.assistantlightsupported) 를 확인 하 여 지원 표시등이 지원 되는지 확인 합니다. 둘 다 지원 되는 경우이 기능에 대 한 UI를 표시 하 고 사용 하도록 설정할 수 있습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocusLight":::

**CheckedChanged** 이벤트 처리기에서 캡처 장치 [**FlashControl**](/uwp/api/Windows.Media.Devices.FlashControl) 개체를 가져옵니다. [**AssistantLightEnabled**](/uwp/api/windows.media.devices.flashcontrol.assistantlightenabled) 속성을 설정 하 여 포커스 조명을 사용 하거나 사용 하지 않도록 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocusLightCheckBox":::

## <a name="iso-speed"></a>ISO 속도

[**IsoSpeedControl**](/uwp/api/Windows.Media.Devices.IsoSpeedControl) 를 사용 하면 사진 또는 비디오 캡처 중에 사용 되는 ISO 속도를 설정할 수 있습니다.

이 예제에서는 [**슬라이더**](/uwp/api/Windows.UI.Xaml.Controls.Slider) 컨트롤을 사용 하 여 현재 노출 보정 값을 조정 하 고 확인란을 사용 하 여 자동 ISO 속도 조정을 설정/해제 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetIsoXAML":::

[**지원 되**](/uwp/api/windows.media.devices.isospeedcontrol.supported) 는 속성을 확인 하 여 현재 캡처 장치가 **IsoSpeedControl** 을 지원 하는지 확인 합니다. 컨트롤이 지원 되는 경우이 기능에 대 한 UI를 표시 하 고 사용 하도록 설정할 수 있습니다. 자동 ISO 속도 조정이 현재 [**Auto**](/uwp/api/windows.media.devices.isospeedcontrol.auto) 속성의 값에 대해 활성 상태 인지 여부를 나타내도록 확인란의 선택 된 상태를 설정 합니다.

ISO 속도 값은 장치에서 지 원하는 범위 내에 있어야 하며, 지원 되는 단계 크기의 증가값 이어야 합니다. 슬라이더 컨트롤의 해당 속성을 설정 하는 데 사용 되는 [**Min**](/uwp/api/windows.media.devices.isospeedcontrol.min), [**Max**](/uwp/api/windows.media.devices.isospeedcontrol.max)및 [**Step**](/uwp/api/windows.media.devices.isospeedcontrol.step) 속성을 확인 하 여 현재 장치에 대해 지원 되는 값을 가져옵니다.

[**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) 이벤트 처리기의 등록을 취소 한 후 slider 컨트롤의 값을 **IsoSpeedControl** 의 현재 값으로 설정 하 여 값이 설정 되 면 이벤트가 트리거되지 않도록 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsoControl":::

**ValueChanged** 이벤트 처리기에서 컨트롤의 현재 값을 가져오고 [**SetValueAsync**](/uwp/api/windows.media.devices.isospeedcontrol.setvalueasync)를 호출 하 여 ISO 속도 값을 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsoSlider":::

자동 ISO 속도 확인란의 **CheckedChanged** 이벤트 처리기에서 [**setautoasync**](/uwp/api/windows.media.devices.isospeedcontrol.setautoasync)를 호출 하 여 자동 iso 속도 조정을 설정 합니다. [**SetValueAsync**](/uwp/api/windows.media.devices.isospeedcontrol.setvalueasync) 를 호출 하 고 슬라이더 컨트롤의 현재 값을 전달 하 여 자동 ISO 속도 조정을 해제 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsoCheckBox":::

## <a name="optical-image-stabilization"></a>광학 이미지 안정화

OIS (광학 이미지 안정화)는 하드웨어 캡처 장치를 기계적으로 조작 하 여 캡처된 비디오 스트림을 안정화 합니다 .이는 디지털 안정화 보다 뛰어난 결과를 제공할 수 있습니다. OIS을 지원 하지 않는 장치에서는 VideoStabilizationEffect을 사용 하 여 캡처된 vide에서 디지털 안정화를 수행할 수 있습니다. 자세한 내용은 [비디오 캡처에 대 한 효과](effects-for-video-capture.md)를 참조 하세요.

[**OpticalImageStabilizationControl**](/uwp/api/windows.media.devices.opticalimagestabilizationcontrol.supported) 속성을 확인 하 여 현재 장치에서 OIS이 지원 되는지 확인 합니다.

OIS 컨트롤은 on, off 및 automatic의 세 가지 모드를 지원 합니다. 즉, 장치가 미디어 캡처를 개선 하는지 여부를 동적으로 결정 하 고, 그럴 경우 OIS를 사용 하도록 설정 합니다. 특정 모드가 장치에서 지원 되는지 확인 하려면 [**OpticalImageStabilizationControl**](/uwp/api/windows.media.devices.opticalimagestabilizationcontrol.supportedmodes) 컬렉션에 원하는 모드가 포함 되어 있는지 확인 하세요.

[**OpticalImageStabilizationControl**](/uwp/api/Windows.Media.Devices.OpticalImageStabilizationMode) 을 원하는 모드로 설정 하 여 OIS를 사용 하거나 사용 하지 않도록 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSetOpticalImageStabilizationMode":::

## <a name="powerline-frequency"></a>Powerline 빈도
일부 카메라 장치는 현재 환경에서 전원 라인의 AC 주파수를 파악 하는 것에 의존 하는 깜박임 방지 처리를 지원 합니다. 일부 장치는 powerline 빈도의 자동 결정을 지원 하지만, 다른 장치는 빈도를 수동으로 설정 해야 합니다. 다음 코드 예제에서는 장치에서 powerline frequency 지원을 확인 하 고, 필요한 경우 빈도를 수동으로 설정 하는 방법을 보여 줍니다. 

먼저 **VideoDeviceController** 메서드 [**TryGetPowerlineFrequency**](/uwp/api/windows.media.devices.videodevicecontroller.trygetpowerlinefrequency)를 호출 하 고 [**powerlinefrequency**](/uwp/api/Windows.Media.Capture.PowerlineFrequency)형식의 출력 매개 변수를 전달 합니다. 이 호출이 실패 하면 현재 장치에서 powerline frequency 컨트롤이 지원 되지 않습니다. 기능이 지원 되는 경우 자동 모드를 설정 하 여 장치에서 자동 모드를 사용할 수 있는지 확인할 수 있습니다. 이렇게 하려면 [**Trysetpowerlinefrequency**](/uwp/api/windows.media.devices.videodevicecontroller.trysetpowerlinefrequency) 를 호출 하 고 값 **Auto**를 전달 합니다. 호출이 성공 하는 경우 자동 powerline 빈도가 지원 됨을 의미 합니다. 장치에서 powerline frequency 컨트롤러를 지원 하지만 자동 빈도 검색을 지원 하지 않는 경우에는 해당 빈도를 수동으로 설정 **하 여 해당**빈도를 설정할 수 있습니다. 이 예제에서 **MyCustomFrequencyLookup** 는 장치의 현재 위치에 대 한 올바른 빈도를 결정 하기 위해 구현 하는 사용자 지정 메서드입니다. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetPowerlineFrequency":::

## <a name="white-balance"></a>백색 잔액

[**WhiteBalanceControl**](/uwp/api/windows.media.devices.videodevicecontroller.whitebalancecontrol) 를 사용 하면 사진 또는 비디오 캡처 중에 사용 되는 잔액을 설정할 수 있습니다.

이 예제에서는 [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 컨트롤을 사용 하 여 기본 제공 색 온도 미리 설정에서 선택 하 고 수동 흰색 잔액 조정을 위한 [**슬라이더**](/uwp/api/Windows.UI.Xaml.Controls.Slider) 컨트롤을 사용 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetWhiteBalanceXAML":::

[**지원 되**](/uwp/api/windows.media.devices.whitebalancecontrol.supported) 는 속성을 확인 하 여 현재 캡처 장치가 **WhiteBalanceControl** 을 지원 하는지 확인 합니다. 컨트롤이 지원 되는 경우이 기능에 대 한 UI를 표시 하 고 사용 하도록 설정할 수 있습니다. 콤보 상자의 항목을 [**ColorTemperaturePreset**](/uwp/api/Windows.Media.Devices.ColorTemperaturePreset) 열거형의 값으로 설정 합니다. 선택한 항목을 [**미리**](/uwp/api/windows.media.devices.whitebalancecontrol.preset) 설정 된 속성의 현재 값으로 설정 합니다.

수동 제어의 경우 백색 잔액 값은 장치에서 지 원하는 범위 내에 있어야 하며, 지원 되는 단계 크기의 증가값 이어야 합니다. 슬라이더 컨트롤의 해당 속성을 설정 하는 데 사용 되는 [**Min**](/uwp/api/windows.media.devices.whitebalancecontrol.min), [**Max**](/uwp/api/windows.media.devices.whitebalancecontrol.max)및 [**Step**](/uwp/api/windows.media.devices.whitebalancecontrol.step) 속성을 확인 하 여 현재 장치에 대해 지원 되는 값을 가져옵니다. 수동 제어를 사용 하도록 설정 하기 전에 지원 되는 최소 값과 최대 값 사이의 범위가 단계 크기 보다 큰지 확인 합니다. 그렇지 않은 경우에는 현재 장치에서 수동 제어가 지원 되지 않습니다.

[**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) 이벤트 처리기의 등록을 취소 한 후 slider 컨트롤의 값을 **WhiteBalanceControl** 의 현재 값으로 설정 하 여 값이 설정 되 면 이벤트가 트리거되지 않도록 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetWhiteBalance":::

색 온도 미리 설정 콤보 상자의 [**Selectionchanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 이벤트 처리기에서 현재 선택 된 기본 설정을 가져오고 [**SetPresetAsync**](/uwp/api/windows.media.devices.whitebalancecontrol.setpresetasync)를 호출 하 여 컨트롤의 값을 설정 합니다. 선택 된 기본 설정 값이 **수동**이 아닌 경우 수동 흰색 잔액 슬라이더를 사용 하지 않도록 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetWhiteBalanceComboBox":::

**ValueChanged** 이벤트 처리기에서 컨트롤의 현재 값을 가져오고 [**SetValueAsync**](/uwp/api/windows.media.devices.exposurecontrol.setvalueasync)를 호출 하 여 백색 잔액 값을 설정 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetWhiteBalanceSlider":::

> [!IMPORTANT]
> 흰색 잔액 조정은 미리 보기 스트림이 실행 되는 동안에만 지원 됩니다. 흰색 잔액 값 또는 미리 설정을 설정 하기 전에 미리 보기 스트림이 실행 중인지 확인 합니다.

> [!IMPORTANT]
> **ColorTemperaturePreset 자동** 설정 값은 자동으로 흰색 잔액 수준을 조정 하도록 시스템에 지시 합니다. 각 프레임에 대해 흰색 잔액 수준을 동일 하 게 유지 해야 하는 사진 시퀀스를 캡처하는 등의 일부 시나리오에서는 컨트롤을 현재 자동 값으로 잠글 수 있습니다. 이렇게 하려면 [**SetPresetAsync**](/uwp/api/windows.media.devices.whitebalancecontrol.setpresetasync) 를 호출 하 고 **수동** 사전 설정을 지정 하 고 [**SetValueAsync**](/uwp/api/windows.media.devices.whitebalancecontrol.setvalueasync)를 사용 하 여 컨트롤에 값을 설정 하지 마십시오. 이렇게 하면 장치에서 현재 값을 잠급니다. 이 값은 올바르다고 보장되지 않으므로 현재 컨트롤 값을 읽고 반환된 값을 **SetValueAsync**로 전달하려고 시도하지 마세요.

## <a name="zoom"></a>Zoom

확대/축소 [**컨트롤**](/uwp/api/Windows.Media.Devices.ZoomControl) 을 사용 하 여 사진 또는 비디오 캡처 중에 사용 되는 확대/축소 수준을 설정할 수 있습니다.

이 예제에서는 [**슬라이더**](/uwp/api/Windows.UI.Xaml.Controls.Slider) 컨트롤을 사용 하 여 현재 확대/축소 수준을 조정 합니다. 다음 섹션에서는 화면에서 손가락을 사용 하 여 확대/축소를 조정 하는 방법을 보여 줍니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetZoomXAML":::

현재 캡처 장치에서 [**지원 되**](/uwp/api/windows.media.devices.zoomcontrol.supported) 는 속성을 확인 하 여 확대/선택 **컨트롤이** 지원 되는지 확인 합니다. 컨트롤이 지원 되는 경우이 기능에 대 한 UI를 표시 하 고 사용 하도록 설정할 수 있습니다.

확대/축소 수준 값은 장치에서 지 원하는 범위 내에 있어야 하며, 지원 되는 단계 크기의 증가값 이어야 합니다. 슬라이더 컨트롤의 해당 속성을 설정 하는 데 사용 되는 [**Min**](/uwp/api/windows.media.devices.zoomcontrol.min), [**Max**](/uwp/api/windows.media.devices.zoomcontrol.max)및 [**Step**](/uwp/api/windows.media.devices.zoomcontrol.step) 속성을 확인 하 여 현재 장치에 대해 지원 되는 값을 가져옵니다.

[**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) 이벤트 처리기의 등록을 취소 한 후 슬라이더 컨트롤의 값을 확대/확대 **컨트롤** 의 현재 값으로 설정 하 여 값이 설정 되 면 이벤트가 트리거되지 않도록 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetZoomControl":::

**ValueChanged** 이벤트 처리기에서 [**값**](/uwp/api/windows.media.devices.zoomsettings.value) 속성을 확대/축소 슬라이더 컨트롤의 현재 값으로 설정 하 여 확대/축소 [**설정**](/uwp/api/Windows.Media.Devices.ZoomSettings) 클래스의 새 인스턴스를 만듭니다. 확대/축소 **컨트롤** 의 [**Supportedmodes**](/uwp/api/windows.media.devices.zoomcontrol.supportedmodes) 속성이 [**ZoomTransitionMode**](/uwp/api/Windows.Media.Devices.ZoomTransitionMode)를 포함 하는 경우이는 장치에서 확대/축소 수준 간의 부드러운 전환을 지원 함을 의미 합니다. 이 모드는 더 나은 사용자 환경을 제공 하므로 일반적으로 확대/ **설정** 개체의 [**Mode**](/uwp/api/windows.media.devices.zoomsettings.mode) 속성에이 값을 사용 하는 것이 좋습니다.

마지막으로, 확대/축소 **설정 개체를** 확대/축소 **컨트롤** 개체의 [**Configure**](/uwp/api/windows.media.devices.zoomcontrol.configure) 메서드에 전달 하 여 현재 확대/축소 설정을 변경 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetZoomSlider":::

### <a name="smooth-zoom-using-pinch-gesture"></a>손가락을 사용한 부드러운 확대/축소 제스처

이전 섹션에서 설명한 것 처럼이를 지 원하는 장치에서 부드러운 확대/축소 모드를 사용 하면 캡처 장치가 디지털 확대/축소 수준 간을 원활 하 게 전환할 수 있으므로 사용자는 불연속 및 jarring 전환 없이 캡처 작업 중 확대/축소 수준을 동적으로 조정할 수 있습니다. 이 섹션에서는 손가락 제스처에 대 한 응답으로 확대/축소 수준을 조정 하는 방법을 설명 합니다.

먼저, 확대/축소 컨트롤을 확인 하 여 현재 장치에서 디지털 확대/축소 컨트롤이 지원 되는지 확인 합니다 [**.**](/uwp/api/windows.media.devices.zoomcontrol.supported) 다음으로, [**ZoomTransitionMode**](/uwp/api/Windows.Media.Devices.ZoomTransitionMode)값이 포함 되어 있는지 확인 하려면 확대/축소 모드를 [**선택 하 여**](/uwp/api/windows.media.devices.zoomcontrol.supportedmodes) 부드러운 확대/축소 모드를 사용할 수 있는지 확인 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetIsSmoothZoomSupported":::

다중 터치를 사용 하는 장치에서 일반적인 시나리오는 두 손가락 손가락을 기준으로 확대/축소 비율을 조정 하는 것입니다. [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) 컨트롤의 [**ManipulationMode**](/uwp/api/windows.ui.xaml.uielement.manipulationmode) 속성을 [**ManipulationModes**](/uwp/api/Windows.UI.Xaml.Input.ManipulationModes) 로 설정 하 여 손가락 제스처를 사용 하도록 설정 합니다. 그런 다음 손가락을 [**system.windows.uielement.manipulationdelta>**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) 이벤트를 등록 하 여 손가락으로 크기를 변경 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetRegisterPinchGestureHandler":::

**System.windows.uielement.manipulationdelta>** 이벤트에 대 한 처리기에서 사용자의 손가락으로 변경 된 동작에 따라 확대/축소 비율을 업데이트 합니다. [**System.windows.uielement.manipulationdelta>**](/uwp/api/Windows.UI.Input.ManipulationDelta) 값은 작은 쪽의 작은 값이 1.0 보다 약간 큰 숫자이 고, 여는 크기의 작은 감소는 1.0 보다 약간 작은 수로 증가 하는 감소 하는 제스처의 크기 변화를 나타냅니다. 이 예제에서는 확대/축소 컨트롤의 현재 값에 배율 델타를 곱합니다.

확대/축소 비율을 설정 하기 전에 값이 확대/축소를 설정 [**하는 것**](/uwp/api/windows.media.devices.zoomcontrol.min) 과 같이 장치에서 지 원하는 최소값 보다 작지 않은지 확인 해야 합니다. 또한 값이 확대/확대/ [**최소**](/uwp/api/windows.media.devices.zoomcontrol.max) 값 보다 작거나 같은지 확인 합니다. 마지막으로 확대/축소 비율이 [**단계**](/uwp/api/windows.media.devices.zoomcontrol.step) 속성에 표시 된 대로 장치에서 지 원하는 확대/축소 단계 크기의 배수 인지 확인 해야 합니다. 확대/축소 비율이 이러한 요구 사항을 충족 하지 않는 경우 캡처 장치에서 확대/축소 수준을 설정 하려고 하면 예외가 throw 됩니다.

새 확대/축소 [**설정**](/uwp/api/Windows.Media.Devices.ZoomSettings) 개체를 만들어 캡처 장치에서 확대/축소 수준을 설정 합니다. [**Mode**](/uwp/api/windows.media.devices.zoomsettings.mode) 속성을 [**ZoomTransitionMode**](/uwp/api/Windows.Media.Devices.ZoomTransitionMode) 로 설정한 다음 [**Value**](/uwp/api/windows.media.devices.zoomsettings.value) 속성을 원하는 확대/축소 비율로 설정 합니다. 마지막으로 [**ZoomControl.Config**](/uwp/api/windows.media.devices.zoomcontrol.configure) 를 호출 하 여 장치에서 새로운 확대/축소 값을 설정 합니다. 장치가 새 확대/축소 값으로 원활 하 게 전환 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetManipulationDelta":::

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
