---
author: Jwmsft
Description: "탭 및 피벗을 사용하면 사용자가 자주 액세스하는 콘텐츠 간에 쉽게 이동할 수 있습니다."
title: "탭 및 피벗"
ms.assetid: 556BC70D-CF5D-4295-A655-D58163CC1824
label: Tabs and pivots
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 93df9051bc0ae724e2c47f5ce5f73da3910c30ce
ms.lasthandoff: 02/07/2017

---
# <a name="pivot-and-tabs"></a>피벗 및 탭

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

피벗 컨트롤과 관련 탭 패턴은 자주 액세스되며 뚜렷이 다른 콘텐츠 범주를 탐색하는 데 사용됩니다. 피벗을 사용하면 둘 이상의 콘텐츠 창 간에 탐색하고 텍스트 헤더를 사용하여 다양한 콘텐츠 섹션을 명확히 표현할 수 있습니다.

![탭 예제](images/pivot_Hero_main.png)

탭은 아이콘 및 텍스트 조합을 사용하거나 아이콘만 사용하여 섹션 콘텐츠를 명확히 표현하는 피벗의 시각적 변형입니다. 탭은 [**피벗**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx) 컨트롤을 사용하여 빌드됩니다. [**피벗 샘플**](http://go.microsoft.com/fwlink/p/?LinkId=619903)은 피벗 컨트롤을 탭 패턴으로 사용자 지정하는 방법을 보여 줍니다.

<div class="important-apis" >
<b>중요 API</b><br/>
<ul>
<li>[**Pivot 클래스**](https://msdn.microsoft.com/library/windows/apps/dn608241)</li>
</ul>
</div>


## <a name="the-pivot-pattern"></a>피벗 패턴

피벗을 사용하여 앱을 빌드할 경우 고려할 주요 디자인 변수가 몇 가지 있습니다.

- **헤더 레이블.**  헤더에는 텍스트가 있는 아이콘이 있거나 아이콘만 있거나 텍스트만 있을 수 있습니다.
- **헤더 맞춤.**  헤더에는 왼쪽 맞춤 또는 가운데 맞춤을 적용할 수 있습니다.
- **최상위 또는 하위 수준 탐색.**  피벗은 두 탐색 수준에 사용할 수 있습니다. 필요에 따라 피벗이 보조 역할을 하면서 [탐색 창](nav-pane.md)은 기본 수준 역할을 할 수 있습니다.
- **터치 제스처 지원.**  터치 제스처를 지원하는 디바이스의 경우 다음 두 조작 집합 중 하나를 사용하여 콘텐츠 범주 간에 탐색할 수 있습니다.
    1. 해당 범주를 탐색하려면 탭/피벗 헤더를 탭합니다.
    2. 인접한 범주를 탐색하려면 콘텐츠 영역을 왼쪽 또는 오른쪽으로 살짝 밉니다.

## <a name="examples"></a>예제

전화의 Pivot 컨트롤입니다.

![피벗의 예](images/pivot_example.png)

알람 및 시계 앱에서 탭 패턴입니다.

![알람 및 시계에서 탭 패턴의 예](images/tabs_alarms-and-clock.png)

## <a name="create-a-pivot-control"></a>피벗 컨트롤 만들기

[**피벗**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx) 컨트롤은 이 섹션에서 설명한 기본 기능과 함께 제공됩니다.

이 XAML은 3개의 콘텐츠 섹션이 있는 기본 피벗 컨트롤을 만듭니다.

```xaml
<Pivot x:Name="rootPivot" Title="Pivot Title">
    <PivotItem Header="Pivot Item 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 1."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 2."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 3."/>
    </PivotItem>
</Pivot>
```

### <a name="pivot-items"></a>피벗 항목

피벗은 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx)이므로 모든 유형의 항목 컬렉션을 포함할 수 있습니다. 피벗에 추가한 항목 중 명시적으로 [**PivotItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx)이 아닌 항목은 모두 PivotItem에 암시적으로 래핑됩니다. 피벗은 콘텐츠 페이지 간 탐색에 사용되는 경우가 많기 때문에 [**Items**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) 컬렉션에 XAML UI 요소를 직접 채우는 것이 일반적입니다. 또는 데이터 원본에 [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 속성을 설정할 수 있습니다. ItemsSource에 바인딩된 항목은 임의 유형일 수 있지만 명시적으로 PivotItems가 아닌 경우 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 및 [**HeaderTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx)을 정의하여 항목의 표시 방법을 지정해야 합니다.

[**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) 속성을 사용하여 피벗의 활성 항목을 가져오거나 설정할 수 있습니다. 활성 항목의 인덱스를 설정하려면 [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) 속성을 사용합니다.

### <a name="pivot-headers"></a>피벗 헤더

[**LeftHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) 및 [**RightHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) 속성을 사용하여 피벗 헤더에 다른 컨트롤을 추가할 수 있습니다.

### <a name="pivot-interaction"></a>피벗 조작

컨트롤을 통해 다음 터치 제스처 조작 기능을 사용할 수 있습니다.

-   피벗 컨트롤 헤더를 탭하면 해당 헤더의 섹션 콘텐츠로 이동합니다.
-   피벗 항목 헤더를 왼쪽 또는 오른쪽으로 살짝 밀면 인접한 섹션으로 이동합니다.
-   섹션 콘텐츠에서 왼쪽 또는 오른쪽으로 살짝 밀면 인접한 섹션으로 이동합니다.
![섹션 콘텐츠에서 왼쪽으로 살짝 밀기 예](images/pivot_w_hand.png)

컨트롤은 다음 두 모드를 제공합니다.

**고정**

-   모든 피벗 헤더가 허용되는 공간 내에 맞는 경우 피벗은 고정됩니다.
-   피벗 레이블을 탭하면 피벗 자체는 움직이지 않고 해당 페이지로 이동합니다. 활성 피벗이 강조 표시됩니다.

<div class="microsoft-internal-note">
3m 환경에서는 항목이 회전하지 않도록 하는 것이 좋습니다. Xbox에서 실행되는 앱일 경우 새로운 `IsHeaderItemsCarouselEnabled` 속성을 False로 설정합니다.
</div>

**회전**

-   모든 피벗 헤더가 허용되는 공간 내에 맞지 않는 경우 피벗이 회전합니다.
-   피벗 레이블을 탭하면 해당 페이지로 이동하며 활성 피벗 레이블이 첫 번째 위치로 회전합니다.
-   회전 모드에서 피벗 항목은 마지막에서부터 첫 번째 피벗 섹션까지 루핑합니다.

<div class="microsoft-internal-note">
### 피벗 포커스

기본적으로 피벗 헤더에 대한 키보드 포커스는 밑줄로 표현됩니다.

![기본 포커스는 선택된 헤더에 밑줄로 표시됩니다.](images/pivot_focus_selectedHeader.png)

사용자 지정된 피벗이 있고 헤더 선택 시각적 개체에 밑줄을 적용하는 앱은 새로운 `HeaderFocusVisualPlacement` 속성을 사용하여 기본값을 변경할 수 있습니다. `HeaderFocusVisualPlacement=\"ItemHeaders\"`일 경우 전체 헤더 패널 주변으로 포커스가 그려집니다.

![ItemsHeader 옵션은 모든 피벗 헤더 주변에 포커스를 그립니다.](images/pivot_focus_headers.png)
</div>

## <a name="recommendations"></a>권장 사항

-   탭/피벗 헤더의 맞춤은 화면 크기를 기준으로 합니다. 일반적으로 화면 너비가 720epx 이하인 경우 가운데 맞춤이 효과적이며 대부분의 경우 화면 너비가 720epx 이상인 경우 왼쪽 맞춤이 좋습니다.
-   회전(왕복) 모드를 사용할 경우 5개를 초과하는 헤더를 사용하면 혼란이 발생할 수 있으므로 5개를 초과하는 헤더를 루핑하지 않도록 합니다.
-   피벗 항목에 고유한 아이콘이 있는 경우에는 탭 패턴만 사용합니다.
-   사용자가 각 피벗 섹션의 의미를 이해할 수 있도록 피벗 항목 헤더에 텍스트를 포함합니다. 일부 사용자에게는 아이콘 외에 별도의 설명이 필요할 수도 있습니다.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드
- [피벗 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619903)<br/>
    Pivot 컨트롤을 탭 패턴으로 사용자 지정하는 방법을 보여 줍니다.
- [XAML UI 기본 사항 샘플](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    대화형 형식의 모든 XAML 컨트롤을 보여 줍니다.

## <a name="related-topics"></a>관련 항목
- [탐색 디자인 기본 사항](../layout/navigation-basics.md)
- [**피벗 샘플**](http://go.microsoft.com/fwlink/p/?LinkId=619903)

