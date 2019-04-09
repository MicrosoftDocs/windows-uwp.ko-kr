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
ms.openlocfilehash: 5c05b6686d31606a9510b1433339dc8829a52893
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59247181"
---
# <a name="guidelines-for-touch-targets"></a>터치 대상에 대 한 지침

유니버설 Windows 플랫폼 (UWP) 응용 프로그램에서 모든 대화형 UI 요소는 정확 하 게 액세스 하 고 장치 유형 또는 입력 방법에 관계 없이 사용 하 여 사용자가 충분히 있어야 합니다.

터치식 디지타이저에서 보고 하는 입력된 데이터의 큰 규모의 더 복잡 한 집합을 결정 하는 대상 크기와 컨트롤 레이아웃 관련 하 여 추가적인 최적화 필요 터치 입력 (및 터치 연락처 영역의 비교적 정확 하지 않은 특성)를 지원 합니다 사용자의 의도 한 (또는 가장 가능성이 높은) 대상입니다.

모든 UWP 컨트롤 기본 터치 대상 크기 및 레이아웃 편안 하 고 편리 하 게 사용할 수 있는 시각적으로 부하가 분산 되 고 매력적인 앱을 빌드할 수 있도록 설계 되었습니다 및 두려움을 없애고 자신감 합니다.

이 항목에서는 (해야 앱 해야) 플랫폼 컨트롤 및 사용자 지정 컨트롤을 모두 사용 하는 최대 유용성에 대 한 앱을 디자인할 수 있도록 이러한 기본 동작 설명 합니다.

> **중요 API**: [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383), [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)

## <a name="fluent-standard-sizing"></a>Fluent 표준 크기 조정

*Fluent 표준 크기 조정* 정보 밀도 및 사용자와 쾌적 간의 균형을 이룰 위해 만들어졌습니다. 효과적으로 화면에 있는 모든 항목 UI 요소 표 형식으로 정렬 하 고 적절 하 게 확장할 수 있습니다. 시스템 수준 크기 조정에 기반 하는 40 x 40 효과적인 픽셀 (epx) 대상에 맞춥니다.

> [!NOTE]
>효과적인 픽셀 및 크기 조정에 대 한 자세한 내용은 참조 하세요. [UWP 앱 디자인 소개](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> 시스템 수준 크기 조정에 대 한 자세한 내용은 참조 하세요. [맞춤, 여백, 안쪽 여백](../layout/alignment-margin-padding.md)합니다.

## <a name="fluent-compact-sizing"></a>Fluent Compact 크기 조정

응용 프로그램에 더 높은 수준의 정보 밀도 사용 하 여 표시할 수 있습니다 *Fluent Compact 크기 조정*합니다. Compact 크기 조정 긴밀 한 그리드를 적절 하 게 시스템 수준 크기 조정 기준 크기 조정에 맞게 UI 요소 수는 32 x 32 epx 대상으로 UI 요소를 맞춥니다.

### <a name="examples"></a>예

Compact 크기 조정 페이지 또는 표 수준에서 적용할 수 있습니다.

### <a name="page-level"></a>페이지 수준

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

### <a name="grid-level"></a>표 수준

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="target-size"></a>대상 크기

일반적으로 터치 대상 크기 7.5 mm 사각형 범위 (135 PPI 화면 고 원으로 확장 x 1.0에서 40 x 40 픽셀)를 설정 합니다. 일반적으로 UWP 컨트롤 (이에 따라 달라질 수 특정 제어 및 모든 일반적인 사용 패턴) 7.5 mm 터치 대상에 맞춰집니다. 참조 [크기와 밀도 제어](../style/spacing.md) 자세한 세부 정보에 대 한 합니다.

이러한 권장 대상 크기는 특정 시나리오에 따라 조정될 수 있습니다. 고려할 사항은 다음과 같습니다.

- 터치-빈도 반복적으로 또는 자주 누르는 최소 크기 보다 큰 목표를 수행 하는 것이 좋습니다.
- 오류 결과-오류에서 작업 하는 경우 심각한 문제가 있는 대상이 큰 안쪽 여백을 콘텐츠 영역의 가장자리 멀리 배치 됩니다. 이는 자주 터치되는 대상의 경우 더 합니다.
- 콘텐츠 영역에 위치 합니다.
- 비율 및 화면 크기를 형성 합니다.
- 손가락 상태입니다.
- 시각화를 터치 합니다.

## <a name="related-articles"></a>관련 문서

- [UWP 앱 디자인 소개](../basics/design-and-ui-intro.md)
- [컨트롤 크기와 밀도](../style/spacing.md)
- [맞춤, 여백, 안쪽 여백](../layout/alignment-margin-padding.md)

### <a name="samples"></a>샘플

- [기본 입력 샘플](https://go.microsoft.com/fwlink/p/?LinkID=620302)
- [짧은 대기 시간 입력 샘플](https://go.microsoft.com/fwlink/p/?LinkID=620304)
- [사용자 조작 모드 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619894)
- [포커스 화면 효과 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619895)

### <a name="archive-samples"></a>보관 샘플

- [입력: XAML 사용자 입력된 이벤트 예제](https://go.microsoft.com/fwlink/p/?linkid=226855)
- [입력: 장치 기능 샘플](https://go.microsoft.com/fwlink/p/?linkid=231530)
- [입력: 터치 적중된 테스트 샘플](https://go.microsoft.com/fwlink/p/?linkid=231590)
- [XAML 스크롤, 이동 및 확대/축소 샘플](https://go.microsoft.com/fwlink/p/?linkid=251717)
- [입력: 간소화 된 잉크 샘플](https://go.microsoft.com/fwlink/p/?linkid=246570)
- [입력: Windows 8 제스처 샘플](https://go.microsoft.com/fwlink/p/?LinkId=264995)
- [입력: 조작 및 제스처 (C++) 샘플](https://go.microsoft.com/fwlink/p/?linkid=231605)
- [DirectX 터치 입력 샘플](https://go.microsoft.com/fwlink/p/?LinkID=231627)
