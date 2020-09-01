---
Description: 이 항목에서는 터치 대상 지정을 위한 연락처 기 하 도형 사용에 대해 설명 하 고 Windows 런타임 apps에서 대상 지정을 위한 모범 사례를 제공 합니다.
title: 대상 지정
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8608c1ff607c76c3f121fe5ed5fded9098911c9d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172437"
---
# <a name="guidelines-for-touch-targets"></a>터치 대상에 대한 지침

장치 형식 또는 입력 방법에 관계 없이 사용자가 정확 하 게 액세스 하 고 사용할 수 있도록 Windows 응용 프로그램의 모든 대화형 UI 요소는 커야 합니다.

터치 입력을 지원 하 고, 터치 접촉 영역의 상대적으로 부정확 한 특성을 지원 하려면 터치 디지타이저에서 보고 하는 더 복잡 한 입력 데이터 집합이 사용자의 의도 (또는 가장 가능성 있는 대상)를 결정 하는 데 사용 됩니다.

모든 UWP 컨트롤은 기본 터치 대상 크기 및 레이아웃을 사용 하 여 디자인 되었습니다 .이를 통해 편안 하 고 편리 하며 영감 확신 있는 시각적으로 균형을 유지 하 고 매력적인 앱을 빌드할 수 있습니다.

이 항목에서는 이러한 기본 동작을 설명 하므로 플랫폼 컨트롤과 사용자 지정 컨트롤 (앱에 필요한 경우)을 모두 사용 하 여 최대의 유용성을 위해 앱을 디자인할 수 있습니다.

> **중요 한 api**: [**windows**](/uwp/api/Windows.UI.Core). ui&gt. [**input**](/uwp/api/Windows.UI.Input), [**windows. .xaml. input**](/uwp/api/Windows.UI.Xaml.Input)

## <a name="fluent-standard-sizing"></a>Fluent 표준 크기

*Fluent 표준 크기*는 정보 밀도와 사용자 편의 간에 균형을 유지하기 위해 개발되었습니다. 화면에 있는 모든 항목을 40x40 유효 픽셀(epx) 대상에 맞추기 때문에 UI 요소를 그리드에 맞추고 시스템 수준 스케일링에 따라 적절하게 크기를 조정할 수 있습니다.

> [!NOTE]
> 유효 픽셀과 크기 조정에 대한 자세한 내용은 [Windows 앱 디자인 소개](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)를 참조하세요.
>
> 시스템 수준 크기 조정에 대한 자세한 내용은 [맞춤, 여백, 안쪽 여백](../layout/alignment-margin-padding.md)을 참조하세요.

## <a name="fluent-compact-sizing"></a>Fluent 컴팩트 크기

응용 프로그램은 *흐름 Compact 크기 조정*으로 높은 수준의 정보 밀도를 표시할 수 있습니다. 압축 크기 조정은 UI 요소를 32x32 window.epx.codesnippet 대상에 맞추고,이를 통해 UI 요소를 보다 엄격한 모눈에 맞추고 시스템 수준 크기 조정에 따라 적절 하 게 확장할 수 있습니다.

### <a name="examples"></a>예

컴팩트 크기 조정은 페이지 또는 그리드 수준에서 적용할 수 있습니다.

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

## <a name="target-size"></a>대상 크기

일반적으로 터치 대상 크기를 7.5 mm 정사각형 범위로 설정 합니다 (1.0 x 크기 조정 정체 되기에서 135 PPI 픽셀 디스플레이의 40x40 픽셀). 일반적으로 UWP 컨트롤은 7.5 mm touch target에 맞게 정렬 됩니다 .이는 특정 컨트롤 및 일반적인 사용 패턴에 따라 달라질 수 있습니다. 자세한 내용은 [크기 및 밀도 제어](../style/spacing.md) 를 참조 하세요.

이러한 대상 크기 권장 사항은 특정 시나리오에 필요한 대로 조정할 수 있습니다. 다음과 같은 몇 가지 사항을 고려해야 합니다.

- 접촉 빈도-최소 크기 보다 반복적 이거나 자주 눌러져 있는 대상을 지정 하는 것이 좋습니다.
- 오류 결과-오류가 발생 한 경우 심각한 결과가 포함 된 대상은 채우기가 더 크고 콘텐츠 영역의 가장자리에서 더 배치 되어야 합니다. 자주 수행 되는 대상의 경우에는 특히 그렇습니다.
- 콘텐츠 영역에서의 위치입니다.
- 폼 팩터 및 화면 크기.
- 핑거 상태.
- 터치 시각화.

## <a name="related-articles"></a>관련된 문서

- [Windows 앱 디자인 소개](../basics/design-and-ui-intro.md)
- [컨트롤 크기 및 밀도](../style/spacing.md)
- [맞춤, 여백, 안쪽 여백](../layout/alignment-margin-padding.md)

### <a name="samples"></a>샘플

- [기본 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [짧은 대기 시간 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [사용자 상호 작용 모드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [포커스 화면 효과 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>보관 샘플

- [Input: XAML 사용자 입력 이벤트 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [입력: 장치 기능 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [입력: 터치 적중 테스트 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [XAML 스크롤, 패닝 및 확대/축소 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [입력: 간소화 된 잉크 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [입력: Windows 8 제스처 샘플](/samples/browse/?redirectedfrom=MSDN-samples)
* [Input: 조작 및 제스처 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [DirectX touch 입력 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))