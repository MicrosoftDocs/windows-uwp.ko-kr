---
Description: 이 항목에서는 터치 타기팅을 위한 접촉 기하 사용에 대해 설명하고 Windows 런타임 앱에서 타기팅에 대한 모범 사례를 제공합니다.
title: 대상 지정
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b1cac04405f18aaf3c8f39f9bfce2b965577807
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257936"
---
# <a name="guidelines-for-touch-targets"></a>Guidelines for touch targets

All interactive UI elements in your Universal Windows Platform (UWP) application must be large enough for users to accurately access and use, regardless of device type or input method.

Supporting touch input (and the relatively imprecise nature of the touch contact area) requires further optimization with respect to target size and control layout as the larger, more complex set of input data reported by the touch digitizer is used to determine the user's intended (or most likely) target.

All UWP controls have been designed with default touch target sizes and layouts that enable you to build visually balanced and appealing apps that are comfortable, easy to use, and inspire confidence.

In this topic, we describe these default behaviors so you can design your app for maximum usability using both platform controls and custom controls (should your app require them).

> **중요 API**: [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="fluent-standard-sizing"></a>Fluent 표준 크기

*Fluent 표준 크기*는 정보 밀도와 사용자 편의 간에 균형을 유지하기 위해 개발되었습니다. 화면에 있는 모든 항목을 40x40 유효 픽셀(epx) 대상에 맞추기 때문에 UI 요소를 그리드에 맞추고 시스템 수준 스케일링에 따라 적절하게 크기를 조정할 수 있습니다.

> [!NOTE]
>유효 픽셀과 크기 조정에 대한 자세한 내용은 [UWP 앱 디자인 소개](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)를 참조하세요.
>
> 시스템 수준 크기 조정에 대한 자세한 내용은 [맞춤, 여백, 안쪽 여백](../layout/alignment-margin-padding.md)을 참조하세요.

## <a name="fluent-compact-sizing"></a>Fluent 컴팩트 크기

Applications can display a higher level of information density with *Fluent Compact sizing*. Compact sizing aligns UI elements to a 32x32 epx target, which lets UI elements to align to a tighter grid and scale appropriately based on system level scaling.

### <a name="examples"></a>예

Compact sizing can be applied at the page or grid level.

### <a name="page-level"></a>페이지 수준

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

### <a name="grid-level"></a>그리드 수준

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="target-size"></a>Target size

In general, set your touch target size to 7.5mm square range (40x40 pixels on a 135 PPI display at a 1.0x scaling plateau). Typically, UWP controls align with 7.5mm touch target (this can vary based on the specific control and any common usage patterns). See [Control size and density](../style/spacing.md) for more detail.

이러한 권장 대상 크기는 특정 시나리오에 따라 조정될 수 있습니다. Here are some things to consider:

- Frequency of Touches - consider making targets that are repeatedly or frequently pressed larger than the minimum size.
- Error Consequence - targets that have severe consequences if touched in error should have greater padding and be placed further from the edge of the content area. 이는 자주 터치되는 대상의 경우 더 합니다.
- Position in the content area.
- Form factor and screen size.
- Finger posture.
- Touch visualizations.

## <a name="related-articles"></a>관련 문서

- [UWP 앱 디자인 소개](../basics/design-and-ui-intro.md)
- [Control size and density](../style/spacing.md)
- [맞춤, 여백, 안쪽 여백](../layout/alignment-margin-padding.md)

### <a name="samples"></a>샘플

- [Basic input sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Low latency input sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [User interaction mode sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [포커스 화면 효과 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>보관 샘플

- [Input: XAML user input events sample](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
- [Input: Device capabilities sample](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
- [Input: Touch hit testing sample](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
- [XAML scrolling, panning, and zooming sample](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
- [Input: Simplified ink sample](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
- [Input: Windows 8 gestures sample](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
- [Input: Manipulations and gestures (C++) sample](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
- [DirectX touch input sample](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
