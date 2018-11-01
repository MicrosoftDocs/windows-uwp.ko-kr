---
author: drewbatgit
ms.assetid: 831123A7-1F40-4B74-AE9F-69AC9883B4AD
description: 이 문서에서는 수동 디바이스 컨트롤을 사용하여 광학 이미지 손떨림 보정, 부드러운 줌 등의 향상된 사진 및 비디오 캡처 시나리오를 가능하게 하는 방법을 보여 줍니다.
title: 사진 및 비디오 캡처를 위한 수동 카메라 컨트롤
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7475910adffd24e4484b539f65633dfb8fc054a8
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5923096"
---
# <a name="manual-camera-controls-for-photo-and-video-capture"></a>사진 및 비디오 캡처를 위한 수동 카메라 컨트롤



이 문서에서는 수동 디바이스 컨트롤을 사용하여 광학 이미지 손떨림 보정, 부드러운 줌 등의 향상된 사진 및 비디오 캡처 시나리오를 가능하게 하는 방법을 보여 줍니다.

이 문서에서 설명하는 컨트롤은 모두 동일한 패턴을 사용하여 앱에 추가됩니다. 먼저, 앱이 실행 중인 현재 디바이스에서 컨트롤이 지원되는지를 확인합니다. 컨트롤이 지원되는 경우 컨트롤에 대해 원하는 모드를 설정합니다. 일반적으로 특정 컨트롤이 현재 디바이스에서 지원되지 않으면 사용자가 기능을 사용하도록 설정할 수 있는 UI 요소를 사용하지 않도록 설정하거나 숨겨야 합니다.

이 문서의 코드는 [카메라 수동 컨트롤 SDK 샘플](https://go.microsoft.com/fwlink/?linkid=845228)에서 조정되었습니다. 샘플을 다운로드하여 상황에 맞게 사용되는 코드를 참조하거나 자체 앱을 처음 빌드하기 시작할 때 샘플을 사용할 수 있습니다.

> [!NOTE]
> 이 문서는 기본 사진 및 비디오 캡처 구현 단계를 설명하는 [MediaCapture를 사용한 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 설명된 개념 및 코드를 토대로 작성되었습니다. 보다 수준 높은 캡처 시나리오를 진행하기 전에 해당 문서의 기본적인 미디어 캡처 패턴을 파악하는 것이 좋습니다. 이 문서의 코드는 앱에 적절히 초기화된 MediaCapture의 인스턴스가 이미 있다고 가정합니다.

이 문서에서 다루는 모든 API 디바이스 컨트롤은 [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902) 네임스페이스의 멤버입니다.

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

## <a name="exposure"></a>Exposure

[**ExposureControl**](https://msdn.microsoft.com/library/windows/apps/dn278910)을 사용하면 사진 또는 비디오 캡처 중에 사용되는 셔터 속도를 설정할 수 있습니다.

이 예제에서는 [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) 컨트롤을 사용하여 현재 노출 값과 자동 노출 조정을 전환하는 확인란을 조정합니다.

[!code-xml[ExposureXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetExposureXAML)]

[**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297710) 속성을 확인하여 현재 캡처 디바이스가 **ExposureControl**을 지원하는지 확인합니다. 컨트롤이 지원되는 경우 이 기능에 대한 UI를 표시하고 사용하도록 설정할 수 있습니다. 자동 노출 조정이 현재 활성화되었는지 여부를 나타내기 위해 확인란의 선택 상태를 [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn278911) 속성의 값으로 설정합니다.

노출 값은 디바이스에서 지원되는 범위 내에 있어야 하며 지원되는 단계 크기의 증분이어야 합니다. 슬라이더 컨트롤의 해당 속성을 설정하는 데 사용되는 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn278919), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn278914) 및 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn278930) 속성을 확인하여 현재 디바이스에 대해 지원되는 값을 가져옵니다.

값이 설정되었을 때 이벤트가 트리거되지 않도록 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 이벤트 처리기를 등록 취소한 후 슬라이더 컨트롤 값을 **ExposureControl**의 현재 값으로 설정합니다.

[!code-cs[ExposureControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureControl)]

**ValueChanged** 이벤트 처리기에서 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278927)를 호출하여 컨트롤의 현재 값을 가져와서 노출 값을 설정합니다.

[!code-cs[ExposureSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureSlider)]

자동 노출 확인란의 **CheckedChanged** 이벤트 처리기에서 [**SetAutoAsync**](https://msdn.microsoft.com/library/windows/apps/dn278920)를 호출하고 부울 값을 전달하여 자동 노출 조정을 켜거나 끕니다.

[!code-cs[ExposureCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureCheckBox)]

> [!IMPORTANT]
> 자동 노출 모드는 미리 보기 스트림이 실행되는 동안만 지원됩니다. 자동 노출을 켜기 전에 미리 보기 스트림이 실행되고 있는지 확인합니다.

## <a name="exposure-compensation"></a>노출 보정

[**ExposureCompensationControl**](https://msdn.microsoft.com/library/windows/apps/dn278897)을 사용하면 사진 또는 비디오 캡처 중에 사용되는 노출 보정을 설정할 수 있습니다.

이 예제에서는 [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) 컨트롤을 사용하여 현재 노출 보정 값을 조정합니다.

[!code-xml[EvXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetEvXAML)]

[지원됨](supported-codecs.md) 속성을 확인하여 현재 캡처 디바이스가 **ExposureCompensationControl**을 지원하는지 확인합니다. 컨트롤이 지원되는 경우 이 기능에 대한 UI를 표시하고 사용하도록 설정할 수 있습니다.

노출 보정 값은 디바이스에서 지원되는 범위 내에 있어야 하고 지원되는 단계 크기의 증분이어야 합니다. 슬라이더 컨트롤의 해당 속성을 설정하는 데 사용되는 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn278901), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn278899) 및 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn278904) 속성을 확인하여 현재 디바이스에 대해 지원되는 값을 가져옵니다.

값이 설정되었을 때 이벤트가 트리거되지 않도록 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 이벤트 처리기를 등록 취소한 후 슬라이더 컨트롤 값을 **ExposureCompensationControl**의 현재 값으로 설정합니다.

[!code-cs[EvControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEvControl)]

**ValueChanged** 이벤트 처리기에서 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278903)를 호출하여 컨트롤의 현재 값을 가져와서 노출 값을 설정합니다.

[!code-cs[EvValueChanged](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEvValueChanged)]

## <a name="flash"></a>Flash

[**FlashControl**](https://msdn.microsoft.com/library/windows/apps/dn297725)을 사용하면 플래시를 사용하거나 사용하지 않도록 설정하거나 자동 플래시를 사용하도록 설정하여 시스템이 동적으로 플래시 사용 여부를 결정할 수 있습니다. 또한 이 컨트롤을 사용하면 자동 적목 현상 감소를 지원하는 디바이스에서 이를 사용하도록 설정할 수 있습니다. 이러한 설정은 모두 사진 캡처에 적용됩니다. [**TorchControl**](https://msdn.microsoft.com/library/windows/apps/dn279077)은 비디오 캡처를 위해 토치를 켜거나 끄기 위한 별도의 컨트롤입니다.

이 예제에서는 라디오 단추 집합을 사용하여 사용자가 켜기, 끄기 및 자동 플래시 설정 간에 전환할 수 있습니다. 또한 적목 현상 감소와 비디오 토치를 전환할 수 있도록 확인란이 제공됩니다.

[!code-xml[FlashXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetFlashXAML)]

[**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297837) 속성을 확인하여 현재 캡처 디바이스가 **FlashControl**을 지원하는지 확인합니다. 컨트롤이 지원되는 경우 이 기능에 대한 UI를 표시하고 사용하도록 설정할 수 있습니다. **FlashControl**이 지원되는 경우 자동 적목 현상 감소가 지원되거나 지원되지 않을 수도 있으므로 [**RedEyeReductionSupported**](https://msdn.microsoft.com/library/windows/apps/dn297766) 속성을 확인한 다음 UI를 사용하도록 설정합니다. **TorchControl**은 플래시 컨트롤과 분리되어 있으므로 해당 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn279081) 속성을 확인한 다음 사용해야 합니다.

각 플래시 라디오 단추에 대한 [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796) 이벤트 처리기에서 적절한 해당 플래시 설정을 사용하거나 사용하지 않도록 설정합니다. 플래시를 항상 사용하도록 설정하려면 [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn297733) 속성을 true로 설정하고 [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn297728) 속성을 false로 설정해야 합니다.

[!code-cs[FlashControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFlashControl)]

[!code-cs[FlashRadioButtons](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFlashRadioButtons)]

적목 현상 감소 확인란에 대한 처리기에서 [**RedEyeReduction**](https://msdn.microsoft.com/library/windows/apps/dn297758) 속성을 적절한 값으로 설정합니다.

[!code-cs[RedEye](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetRedEye)]

마지막으로, 비디오 토치 확인란에 대한 처리기에서 [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn279078) 속성을 적절한 값으로 설정합니다.

[!code-cs[Torch](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTorch)]

> [!NOTE] 
>  [**TorchControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn279078)가 true로 설정되어도 장치에 실행 중인 미리 보기 스트림이 없어서 적극적으로 비디오를 캡처하지 않는 경우 일부 장치에서는 토치가 빛을 발하지 않습니다. 권장되는 작업 순서는 비디오 미리 보기를 켠 다음 **Enabled**를 true로 설정하고 비디오 캡처를 시작하는 것입니다. 일부 디바이스에서는 미리 보기가 시작된 다음 토치가 밝게 표시됩니다. 다른 디바이스에서는 비디오 캡처가 시작될 때까지 토치가 밝게 표시되지 않을 수 있습니다.

## <a name="focus"></a>초점

카메라의 초점을 조정하기 위해 [**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788) 개체에서 일반적으로 사용하는 세 가지 다른 방법은 연속 자동 초점, 탭하여 초점 맞추기 및 수동 초점입니다. 카메라 앱이 이러한 세 가지 메서드를 모두 지원할 수 있지만 읽기 쉽게 하기 위해 이 문서에서는 각 기술을 개별적으로 다룹니다. 또한 이 섹션에서는 초점 도우미 광원을 사용하도록 설정하는 방법도 설명합니다.

### <a name="continuous-autofocus"></a>연속 자동 초점

연속 자동 초점을 사용하도록 설정하면 사진 또는 비디오 대상의 초점이 맞춰지도록 카메라에게 초점을 동적으로 조정하라는 지시를 내립니다.  이 예제에서는 라디오 단추를 사용하여 연속 자동 초점을 켜고 끄는 것을 전환합니다.

[!code-xml[CAFXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetCAFXAML)]

[**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785) 속성을 확인하여 현재 캡처 디바이스가 **FocusControl**을 지원하는지 확인합니다. 그런 다음 [**FocusMode.Continuous**](https://msdn.microsoft.com/library/windows/apps/dn608084) 값이 들어 있는지 보기 위해 [**SupportedFocusModes**](https://msdn.microsoft.com/library/windows/apps/dn608079) 목록을 확인하여 연속 자동 초점이 지원되는지 보고, 지원되는 경우 연속 자동 초점 라디오 단추를 표시합니다.

[!code-cs[CAF](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetCAF)]

연속 자동 초점 라디오 단추의 [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796) 이벤트 처리기에서 [**VideoDeviceController.FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn279091) 속성을 사용하여 컨트롤의 인스턴스를 가져옵니다. 앱이 이전에 [**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075)를 호출하여 다른 초점 모드 중 하나를 사용하도록 설정한 경우 [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081)를 호출하여 컨트롤의 잠금을 해제합니다.

새 [**FocusSettings**](https://msdn.microsoft.com/library/windows/apps/dn608085) 개체를 만들고 [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn608090) 속성을 **Continuous**로 가져옵니다. [**AutoFocusRange**](https://msdn.microsoft.com/library/windows/apps/dn608086) 속성을 앱 시나리오에 적절한 값으로 설정하거나 UI에서 사용자가 선택한 값으로 설정합니다. **FocusSettings** 개체를 [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067) 메서드로 전달한 다음 [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794)를 호출하여 연속 자동 초점을 시작합니다.

[!code-cs[CafFocusRadioButton](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetCafFocusRadioButton)]

> [!IMPORTANT]
> 자동 초점 모드는 미리 보기 스트림이 실행되는 동안만 지원됩니다. 연속 자동 초점을 켜기 전에 미리 보기 스트림이 실행되고 있는지 확인합니다.

### <a name="tap-to-focus"></a>탭하여 초점 맞추기

탭하여 초점 맞추기 기술은 [**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788) 및 [**RegionsOfInterestControl**](https://msdn.microsoft.com/library/windows/apps/dn279064)을 사용하여 캡처 디바이스가 초점을 맞추어야 하는 캡처 프레임의 하위 영역을 지정합니다. 초점 영역은 미리 보기 스트림을 표시하는 화면을 사용자가 탭하면 결정됩니다.

이 예제에서는 라디오 단추를 사용하여 탭하여 초점 맞추기 모드를 사용하거나 사용하지 않도록 설정합니다.

[!code-xml[TapFocusXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetTapFocusXAML)]

[**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785) 속성을 확인하여 현재 캡처 디바이스가 **FocusControl**을 지원하는지 확인합니다. 이 기술을 사용하려면 **RegionsOfInterestControl**이 지원되어야 하며 이 컨트롤에서 하나 이상의 영역을 지원해야 합니다. [**AutoFocusSupported**](https://msdn.microsoft.com/library/windows/apps/dn279066) 및 [**MaxRegions**](https://msdn.microsoft.com/library/windows/apps/dn279069) 속성을 확인하여 탭하여 초점 맞추기에 대한 라디오 단추를 표시하거나 숨길지 여부를 결정합니다.

[!code-cs[TapFocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocus)]

탭하여 초점 맞추기 라디오 단추에 대한 [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796) 이벤트 처리기에서 [**VideoDeviceController.FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn279091) 속성을 사용하여 컨트롤의 인스턴스를 가져옵니다. 앱이 이전에 연속 자동 초점을 사용하도록 [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081)를 호출한 경우 [**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075)를 호출하여 컨트롤을 잠근 다음 사용자가 화면을 탭하여 포커스를 변경할 때까지 기다립니다.

[!code-cs[TapFocusRadioButton](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocusRadioButton)]

이 예제에서는 토글처럼 사용자가 화면을 탭할 때 영역에 초점을 맞추고 사용자가 다시 탭할 때 해당 영역에서 초점을 제거합니다. 현재 전환된 상태를 추적하려면 부울 변수를 사용합니다.

[!code-cs[IsFocused](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsFocused)]

다음 단계는 캡처 미리 보기 스트림을 현재 표시하는 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278)의 [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985) 이벤트를 처리하여 사용자가 화면을 탭할 때 이벤트를 수신 대기하는 것입니다. 카메라가 현재 미리 보고 있는 중이 아니거나 탭하여 초점 맞추기 모드가 사용되지 않는 경우에는 아무 작업도 수행하지 않고 처리기에서 반환합니다.

추적 변수 *\_isFocused*가 false로 전환되고 카메라가 현재 초점 프로세스([**FocusState**](https://msdn.microsoft.com/library/windows/apps/dn608074)의 **FocusControl** 속성에 의해 결정)에 있지 않은 경우 탭하여 초점 맞추기 프로세스를 시작합니다. 사용자의 탭 위치를 이벤트 인수에서 처리기로 전달합니다. 이 예제에서는 이 기회를 사용해 초점을 맞출 영역의 크기를 선택합니다. 이 경우 크기는 가장 작은 캡처 요소 치수의 1/4입니다. 탭 위치 및 영역 크기를 다음 섹션에서 정의되는 **TapToFocus** 도우미 메서드로 전달합니다.

*\_isFocused* 토글이 true로 설정된 경우 사용자가 탭하면 이전 영역의 초점이 지워집니다. 이 작업은 아래 나와 있는 **TapUnfocus** 도우미 메서드에서 수행됩니다.

[!code-cs[TapFocusPreviewControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocusPreviewControl)]

**TapToFocus** 도우미 메서드에서 먼저 *\_isFocused* 토글을 true로 설정하여 다음 화면 탭이 탭한 영역의 초점을 해제하도록 합니다.

이 도우미 메서드의 다음 작업은 초점 컨트롤에 할당될 미리 보기 스트림 내의 사각형을 결정하는 것입니다. 이 작업은 두 단계가 필요합니다. 첫 번째 단계는 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 컨트롤 내에서 미리 보기 스트림이 차지하는 사각형을 결정하는 것입니다. 이는 미리 보기 스트림의 크기 및 디바이스 방향에 따라 다릅니다. 이 섹션의 끝에 나와 있는 도우미 메서드 **GetPreviewStreamRectInControl**이 이 작업을 수행하고 미리 보기 스트림이 포함된 사각형을 반환합니다.

**TapToFocus**의 다음 작업은 탭 위치와 원하는 초점 사각형 크기를 **CaptureElement.Tapped** 이벤트 처리기 내에서 결정된 캡처 스트림 내의 좌표로 변환하는 것입니다. 이 섹션의 뒷부분에 나와 있는 **ConvertUiTapToPreviewRect** 도우미 메서드는 초점을 요청할 캡처 스트림 좌표에서 이 변환을 수행하고 사각형을 반환합니다.

이제 대상 사각형을 가져왔으므로 새 [**RegionOfInterest**](https://msdn.microsoft.com/library/windows/apps/dn279058) 개체를 만들고 [**Bounds**](https://msdn.microsoft.com/library/windows/apps/dn279062) 속성을 이전 단계에서 가져온 대상 사각형으로 설정합니다.

캡처 디바이스의 [**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788)을 가져옵니다. 새 [**FocusSettings**](https://msdn.microsoft.com/library/windows/apps/dn608085) 개체를 만들고 [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn608076) 및 [**AutoFocusRange**](https://msdn.microsoft.com/library/windows/apps/dn608086)를 원하는 값으로 설정합니다. 그전에 **FocusControl**에서 이러한 기능이 지원되는지 확인합니다. **FocusControl**에서 [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067)을 호출하여 설정을 활성화하고 지정된 영역에서 초점 맞추기를 시작하라는 신호를 디바이스에 보냅니다.

그런 다음 캡처 디바이스의 [**RegionsOfInterestControl**](https://msdn.microsoft.com/library/windows/apps/dn279064)을 가져오고 [**SetRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279070)를 호출하여 활성 영역을 설정합니다. 여러 영역을 지원하는 디바이스에서 관심 있는 여러 영역을 설정할 수 있지만 이 예제에서는 단일 영역만 설정합니다.

마지막으로 **FocusControl**에서 [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794)를 호출하여 초점 지정을 시작합니다.

> [!IMPORTANT]
> 초점에 대한 탭을 구현할 때 작업 순서가 중요합니다. 다음 순서로 이러한 API를 호출해야 합니다.
>
> 1. [**FocusControl.Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067)
> 2. [**RegionsOfInterestControl.SetRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279070)
> 3. [**FocusControl.FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794)

[!code-cs[TapToFocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapToFocus)]

**TapUnfocus** 도우미 메서드에서 **RegionsOfInterestControl**을 가져오고 [**ClearRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279068)를 호출하여 **TapToFocus** 도우미 메서드에서 컨트롤에 등록된 영역을 지웁니다. 그런 다음 디바이스가 관심 영역 없이 다시 초점을 맞추도록 **FocusControl**을 가져오고 [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794)를 호출합니다.

[!code-cs[TapUnfocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapUnfocus)]

**GetPreviewStreamRectInControl** 도우미 메서드에서는 미리 보기 스트림의 해상도 및 디바이스의 방향을 사용하여 미리 보기 스트림이 포함된 미리 보기 요소 내의 사각형을 결정한 다음 컨트롤이 스트림의 가로 세로 비율을 유지하기 위해 제공할 수 있는 레터박스 안쪽 여백을 잘라냅니다. 이 메서드는 [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)에 있는 기본 미디어 캡처 예제 코드에서 정의된 클래스 멤버 변수를 사용합니다.

[!code-cs[GetPreviewStreamRectInControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetGetPreviewStreamRectInControl)]

**ConvertUiTapToPreviewRect** 도우미 메서드는 **GetPreviewStreamRectInControl** 도우미 메서드에서 가져온 미리 보기 스트림이 포함된 사각형, 원하는 초점 영역의 크기, 탭 이벤트의 위치를 인수로 사용합니다. 이 메서드는 이러한 값 및 디바이스의 현재 방향을 사용하여 원하는 영역이 포함된 미리 보기 스트림 내에서 사각형을 계산합니다. 다시 한 번 이 방법은 [MediaCapture를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-mediacapture.md)에 있는 기본 미디어 캡처 예제 코드에서 정의된 클래스 멤버 변수를 사용합니다.

[!code-cs[ConvertUiTapToPreviewRect](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetConvertUiTapToPreviewRect)]

### <a name="manual-focus"></a>수동 초점

수동 초점 기술은 **Slider** 컨트롤을 사용하여 캡처 디바이스의 현재 초점 깊이를 설정합니다. 라디오 단추는 수동 초점 켜기/끄기를 전환하는 데 사용됩니다.

[!code-xml[ManualFocusXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetManualFocusXAML)]

[**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297837) 속성을 확인하여 현재 캡처 디바이스가 **FocusControl**을 지원하는지 확인합니다. 컨트롤이 지원되는 경우 이 기능에 대한 UI를 표시하고 사용하도록 설정할 수 있습니다.

초점 값은 디바이스에서 지원되는 범위 내에 있어야 하고 지원되는 단계 크기의 증분이어야 합니다. 슬라이더 컨트롤의 해당 속성을 설정하는 데 사용되는 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn297808), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn297802) 및 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn297833) 속성을 확인하여 현재 디바이스에 대해 지원되는 값을 가져옵니다.

값이 설정되었을 때 이벤트가 트리거되지 않도록 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 이벤트 처리기를 등록 취소한 후 슬라이더 컨트롤 값을 **FocusControl**의 현재 값으로 설정합니다.

[!code-cs[Focus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocus)]

앱에서 이전에 [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081)에 대한 호출을 사용하여 초점을 잠금 해제한 경우 수동 포커스 라디오 단추의 **Checked** 이벤트 처리기에서 **FocusControl** 개체를 가져와서 [**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075)를 호출합니다.

[!code-cs[ManualFocusChecked](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetManualFocusChecked)]

수동 초점 슬라이더의 **ValueChanged** 이벤트 처리기에서 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn297828)를 호출하여 컨트롤의 현재 값을 가져와서 초점 값을 설정합니다.

[!code-cs[FocusSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusSlider)]

### <a name="enable-the-focus-light"></a>초점 광원 사용

이 옵션을 지원하는 디바이스에서 디바이스 초점을 맞추기 위해 초점 도우미 광원을 사용하도록 설정할 수 있습니다. 이 예제에서는 확인란을 사용하여 초점 도우미 광원을 사용하거나 사용하지 않도록 설정합니다.

[!code-xml[FocusLightXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetFocusLightXAML)]

[**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785) 속성을 확인하여 현재 캡처 디바이스가 **FlashControl**을 지원하는지 확인합니다. 또한 [**AssistantLightSupported**](https://msdn.microsoft.com/library/windows/apps/dn608066)를 확인하여 도우미 광원이 지원되는지 확인합니다. 이들이 둘 다 지원되는 경우 이 기능에 대한 UI를 표시하고 사용하도록 설정할 수 있습니다.

[!code-cs[FocusLight](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusLight)]

**CheckedChanged** 이벤트 처리기에서 캡처 디바이스 [**FlashControl**](https://msdn.microsoft.com/library/windows/apps/dn297725) 개체를 가져옵니다. [**AssistantLightEnabled**](https://msdn.microsoft.com/library/windows/apps/dn608065) 속성을 설정하여 포커스 광원을 사용하거나 사용하지 않도록 설정합니다.

[!code-cs[FocusLightCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusLightCheckBox)]

## <a name="iso-speed"></a>ISO 감도

[**IsoSpeedControl**](https://msdn.microsoft.com/library/windows/apps/dn297850)을 사용하여 사진 또는 비디오 캡처 중에 사용되는 ISO 감도를 설정할 수 있습니다.

이 예제에서는 [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) 컨트롤을 사용하여 현재 노출 보정 값 및 자동 ISO 감도 조정을 전환하는 확인란을 조정합니다.

[!code-xml[IsoXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetIsoXAML)]

[**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297869) 속성을 확인하여 현재 캡처 디바이스가 **IsoSpeedControl**을 지원하는지 확인합니다. 컨트롤이 지원되는 경우 이 기능에 대한 UI를 표시하고 사용하도록 설정할 수 있습니다. 자동 ISO 감도 조정이 현재 활성화되었는지 여부를 나타내기 위해 확인란의 선택 상태를 [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn608093) 속성의 값으로 설정합니다.

ISO 감도 값은 디바이스에서 지원되는 범위 내에 있어야 하고 지원되는 단계 크기의 증분이어야 합니다. 슬라이더 컨트롤의 해당 속성을 설정하는 데 사용되는 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn608095), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn608094) 및 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn608129) 속성을 확인하여 현재 디바이스에 대해 지원되는 값을 가져옵니다.

값이 설정되었을 때 이벤트가 트리거되지 않도록 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 이벤트 처리기를 등록 취소한 후 슬라이더 컨트롤 값을 **IsoSpeedControl**의 현재 값으로 설정합니다.

[!code-cs[IsoControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoControl)]

**ValueChanged** 이벤트 처리기에서 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn608128)를 호출하여 컨트롤의 현재 값을 가져와서 ISO 감도 값을 설정합니다.

[!code-cs[IsoSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoSlider)]

자동 ISO 감도 확인란의 **CheckedChanged** 이벤트 처리기에서 [**SetAutoAsync**](https://msdn.microsoft.com/library/windows/apps/dn608127)를 호출하여 자동 ISO 감도 조정을 켭니다. [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn608128)를 호출하고 슬라이더 컨트롤의 현재 값을 전달하여 자동 ISO 감도 조정을 끕니다.

[!code-cs[IsoCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoCheckBox)]

## <a name="optical-image-stabilization"></a>광학 이미지 손떨림 보정

OIS(광학 이미지 손떨림 보정)는 하드웨어 캡처 디바이스를 기계적으로 조작하여 캡처된 비디오 스트림을 안정화합니다. 이 방식은 디지털 손떨림 보정보다 뛰어난 결과를 제공할 수 있습니다. OIS를 지원하지 않는 디바이스에서는 VideoStabilizationEffect를 사용하여 캡처된 동영상에 대해 디지털 손떨림 보정을 수행할 수 있습니다. 자세한 내용은 [비디오 캡처 효과](effects-for-video-capture.md)를 참조하세요.

[**OpticalImageStabilizationControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926689) 속성을 확인하여 현재 디바이스에서 OIS가 지원되는지 확인합니다.

OIS 컨트롤은 세 가지 모드인 켜짐, 꺼짐 및 자동을 지원합니다. 즉, 디바이스는 OIS가 미디어 캡처를 향상시키는지를 동적으로 확인하고 향상시킬 경우 OIS를 사용하도록 설정합니다. 디바이스에서 특정 모드가 지원되는지 확인하려면 [**OpticalImageStabilizationControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926690) 컬렉션이 원하는 모드를 포함하는지 확인합니다.

[**OpticalImageStabilizationControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926691)를 원하는 모드로 설정하여 OIS를 사용하거나 사용하지 않도록 설정합니다.

[!code-cs[SetOpticalImageStabilizationMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetOpticalImageStabilizationMode)]

## <a name="powerline-frequency"></a>Powerline 주파수
일부 카메라 디바이스는 현재 환경에서 powerline의 AC 주파수를 아는 데 따른 깜박임 방지 처리를 지원합니다. 일부 디바이스는 powerline 주파수의 자동 결정을 지원하는 반면 다른 디바이스는 주파수를 수동으로 설정해야 합니다. 다음 코드 예제에서는 디바이스에서 powerline 주파수 지원을 결정하는 방법 및 필요에 따라 주파수를 수동으로 설정하는 방법을 보여 줍니다. 

먼저 **VideoDeviceController** 메서드 [**TryGetPowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/br206898)를 호출하여 [**PowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.PowerlineFrequency) 형식의 출력 매개 변수를 전달합니다. 이 호출이 실패할 경우 powerline 주파수 컨트롤은 현재 디바이스에서 지원되지 않습니다. 기능이 지원되는 경우 자동 모드 설정을 시도하여 디바이스에서 자동 모드를 사용할 수 있는지 확인할 수 있습니다. [**TrySetPowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/br206899) 호출 및 **자동**값을 전달 하 여이 작업을 수행 합니다. 호출에 성공 하면 자동 powerline 주파수를 지원 하는지 의미 합니다. powerline 주파수 컨트롤러는 디바이스에서 지원되고 자동 주파수 감지는 지원되지 않는 경우 **TrySetPowerlineFrequency**를 사용하여 수동으로 주파수를 설정할 수 있습니다. 이 예제에서 **MyCustomFrequencyLookup**은 디바이스의 현재 위치에 대한 올바른 주파수를 결정하기 위해 구현하는 사용자 지정 메서드입니다. 

[!code-cs[PowerlineFrequency](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetPowerlineFrequency)]

## <a name="white-balance"></a>화이트 밸런스

[**WhiteBalanceControl**](https://msdn.microsoft.com/library/windows/apps/dn279104)을 사용하면 사진 또는 비디오 캡처 중에 사용되는 화이트 밸런스를 설정할 수 있습니다.

이 예제에서는 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348) 컨트롤을 사용하여 기본 제공 색 온도 미리 설정에서 선택하고 수동 화이트 밸런스 조정을 위해 [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) 컨트롤을 사용합니다.

[!code-xml[WhiteBalanceXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetWhiteBalanceXAML)]

[**Supported**](https://msdn.microsoft.com/library/windows/apps/dn279120) 속성을 확인하여 현재 캡처 디바이스가 **WhiteBalanceControl**을 지원하는지 확인합니다. 컨트롤이 지원되는 경우 이 기능에 대한 UI를 표시하고 사용하도록 설정할 수 있습니다. 콤보 상자 항목을 [**ColorTemperaturePreset**](https://msdn.microsoft.com/library/windows/apps/dn278894) 열거의 값으로 설정합니다. 선택한 항목을 [**Preset**](https://msdn.microsoft.com/library/windows/apps/dn279110) 속성의 현재 값으로 설정합니다.

수동 컨트롤의 경우 화이트 밸런스 값은 디바이스에서 지원되는 범위 내에 있어야 하고 지원되는 단계 크기의 증분이어야 합니다. 슬라이더 컨트롤의 해당 속성을 설정하는 데 사용되는 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn279109), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn279107) 및 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn279119) 속성을 확인하여 현재 디바이스에 대해 지원되는 값을 가져옵니다. 수동 컨트롤을 사용하도록 설정하기 전에 지원되는 최소값과 최대값 사이의 범위가 단계 크기보다 큰지 확인합니다. 그렇지 않은 경우 수동 컨트롤이 현재 디바이스에서 지원되지 않습니다.

값이 설정되었을 때 이벤트가 트리거되지 않도록 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 이벤트 처리기를 등록 취소한 후 슬라이더 컨트롤 값을 **WhiteBalanceControl**의 현재 값으로 설정합니다.

[!code-cs[WhiteBalance](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalance)]

색 온도 미리 설정 콤보 상자의 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) 이벤트 처리기에서 [**SetPresetAsync**](https://msdn.microsoft.com/library/windows/apps/dn279113)를 호출하여 현재 선택한 미리 설정을 가져오고 컨트롤의 값을 설정합니다. 선택한 미리 설정 값이 **Manual**이 아닌 경우 수동 화이트 밸런스 슬라이더를 사용하지 않도록 설정합니다.

[!code-cs[WhiteBalanceComboBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalanceComboBox)]

**ValueChanged** 이벤트 처리기에서 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278927)를 호출하여 컨트롤의 현재 값을 가져와서 화이트 밸런스 값을 설정합니다.

[!code-cs[WhiteBalanceSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalanceSlider)]

> [!IMPORTANT]
> 화이트 밸런스 조정은 미리 보기 스트림이 실행되는 동안만 지원됩니다. 화이트 밸런스 값 또는 미리 설정을 설정하기 전에 미리 보기 스트림이 실행되고 있는지 확인합니다.

> [!IMPORTANT]
> **ColorTemperaturePreset.Auto** 미리 설정 값이 시스템에 화이트 밸런스 수준을 자동으로 조정하도록 지시합니다. 일부 시나리오의 경우 각 프레임에 대해 화이트 밸런스 수준이 같아야 하는 사진 시퀀스 캡처와 같이 컨트롤을 현재 자동 값으로 잠글 수 있습니다. 그러려면 [**SetPresetAsync**](https://msdn.microsoft.com/library/windows/apps/dn279113)를 호출하고, **Manual** 미리 설정을 지정하고, [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn279114)를 사용하는 컨트롤에 대한 값은 설정하지 않습니다. 그러면 디바이스가 현재 값을 잠그게 됩니다. 이 값은 올바르다고 보장되지 않으므로 현재 컨트롤 값을 읽고 반환된 값을 **SetValueAsync**로 전달하려고 시도하지 마세요.

## <a name="zoom"></a>줌

[**ZoomControl**](https://msdn.microsoft.com/library/windows/apps/dn608149)을 통해 사진 또는 비디오 캡처 중에 사용되는 확대/축소 수준을 설정할 수 있습니다.

이 예제에서는 [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) 컨트롤을 사용하여 현재 확대/축소 수준을 조정합니다. 다음 섹션은 화면에서 축소 제스처에 따라 확대/축소를 조정하는 방법을 보여 줍니다.

[!code-xml[ZoomXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetZoomXAML)]

[**Supported**](https://msdn.microsoft.com/library/windows/apps/dn633819) 속성을 확인하여 현재 캡처 디바이스가 **ZoomControl**을 지원하는지 확인합니다. 컨트롤이 지원되는 경우 이 기능에 대한 UI를 표시하고 사용하도록 설정할 수 있습니다.

확대/축소 수준 값은 디바이스에서 지원되는 범위 내에 있어야 하고 지원되는 단계 크기의 증분이어야 합니다. 슬라이더 컨트롤의 해당 속성을 설정하는 데 사용되는 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn633817), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn608150) 및 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn633818) 속성을 확인하여 현재 디바이스에 대해 지원되는 값을 가져옵니다.

값이 설정되었을 때 이벤트가 트리거되지 않도록 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 이벤트 처리기를 등록 취소한 후 슬라이더 컨트롤 값을 **ZoomControl**의 현재 값으로 설정합니다.

[!code-cs[ZoomControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetZoomControl)]

**ValueChanged** 이벤트 처리기에서 [**ZoomSettings**](https://msdn.microsoft.com/library/windows/apps/dn926722) 클래스의 새 인스턴스를 만들어 [**Value**](https://msdn.microsoft.com/library/windows/apps/dn926724) 속성을 확대/축소 슬라이더 컨트롤의 현재 값으로 설정합니다. **ZoomControl**의 [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926721) 속성이 [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726)를 포함하는 경우 이는 디바이스가 확대/축소 수준에서 부드러운 전환을 지원한다는 것입니다. 이 모드는 더 나은 사용자 환경을 제공하기 때문에 일반적으로 **ZoomSettings** 개체의 [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn926723) 속성에 이 값을 사용하고 싶을 수 있습니다.

마지막으로 **ZoomSettings** 개체를 **ZoomControl** 개체의 [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn926719) 메서드로 전달하여 현재 확대/축소 설정을 변경합니다.

[!code-cs[ZoomSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetZoomSlider)]

### <a name="smooth-zoom-using-pinch-gesture"></a>축소 제스처를 사용하는 부드러운 줌

이전 섹션에서 설명했듯이 부드러운 줌을 지원하는 디바이스에서 이 모드를 사용하면 캡처 디바이스가 디지털 확대/축소 수준 간을 매끄럽게 전환할 수 있으므로 사용자는 캡처 작업 동안 전환을 방해하지 않고 연속적으로 진행하면서 확대/축소 수준을 동적으로 조정할 수 있습니다. 이 섹션에서는 축소 제스처에 대한 응답으로 확대/축소 수준을 조정하는 방법에 대해 설명합니다.

먼저 [**ZoomControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn633819) 속성을 확인하여 현재 디바이스에서 디지털 확대/축소 컨트롤이 지원되는지를 확인합니다. 다음으로 [**ZoomControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926721)가 [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726) 값을 포함하는지 확인하여 부드러운 줌 모드를 사용할 수 있는지 검토합니다.

[!code-cs[IsSmoothZoomSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsSmoothZoomSupported)]

멀티 터치 사용 디바이스에서 일반적인 시나리오는 두 손가락 모으기 제스처에 따라 확대/축소 배율을 조정하는 것입니다. [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 컨트롤의 [**ManipulationMode**](https://msdn.microsoft.com/library/windows/apps/br208948) 속성을 [**ManipulationModes.Scale**](https://msdn.microsoft.com/library/windows/apps/br227934)로 설정하여 손가락 모으기 제스처를 사용하도록 설정합니다. 그런 다음 손가락 모으기 제스처 크기가 변경될 때 발생하는 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 이벤트에 등록합니다.

[!code-cs[RegisterPinchGestureHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterPinchGestureHandler)]

**ManipulationDelta** 이벤트에 대한 처리기에서 사용자의 손가락 모으기 제스처의 변화에 따라 확대/축소 배율을 업데이트합니다. [**ManipulationDelta.Scale**](https://msdn.microsoft.com/library/windows/apps/br242016) 값은 손가락 모으기 크기가 약간 증가하면 1.0보다 약간 더 큰 숫자가 되고 손가락 모으기 크기가 약간 감소하면 1.0보다 약간 더 작은 숫자가 되도록 손가락 모으기 제스처의 크기 변경을 나타냅니다. 이 예제에서는 확대/축소 컨트롤의 현재 값에 배율 델타를 곱합니다.

확대/축소 배율을 설정하기 전에 해당 값이 디바이스에서 지원되는 최소값([**ZoomControl.Min**](https://msdn.microsoft.com/library/windows/apps/dn633817) 속성으로 지정)보다 작지 않은지 확인해야 합니다. 또한 해당 값은 [**ZoomControl.Max**](https://msdn.microsoft.com/library/windows/apps/dn608150) 값보다 작거나 같아야 합니다. 마지막으로 확대/축소 [**단계**](https://msdn.microsoft.com/library/windows/apps/dn633818) 속성에 의해 표시 된 대로 장치에서 지원 하 고 확대/축소 단계 크기의 배수가 있는지 확인 해야 합니다. 확대/축소 배율이 이러한 요구를 충족하지 못하면 캡처 디바이스에서 확대/축소 수준을 설정하려고 할 때 예외가 발생됩니다.

새 [**ZoomSettings**](https://msdn.microsoft.com/library/windows/apps/dn926722) 개체를 만들어 캡처 디바이스의 확대/축소 수준을 설정합니다. [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn926723) 속성을 [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726)로 설정한 다음 [**Value**](https://msdn.microsoft.com/library/windows/apps/dn926724) 속성을 원하는 확대/축소 배율로 설정합니다. 마지막으로 [**ZoomControl.Configure**](https://msdn.microsoft.com/library/windows/apps/dn926719)를 호출하여 디바이스에 대해 새 확대/축소 값을 설정합니다. 디바이스는 새 확대/축소 값으로 원활하게 전환됩니다.

[!code-cs[ManipulationDelta](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
