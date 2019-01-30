---
Description: The Pivot control enables touch-swiping between a small set of content sections.
title: 피벗
template: detail.hbs
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 56079bc51d3efa8f7ecaaee21379a6e9caf7d440
ms.sourcegitcommit: a60ab85e9f2f9690e0141050ec3aa51f18ec61ec
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "9036895"
---
# <a name="pivot"></a>피벗

[피벗](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) 컨트롤 콘텐츠 섹션의 작은 집합 간의 터치 살짝 수 있습니다.

> **중요 Api**: [Pivot 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot), [NavigationView 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치 된 경우 여기를 클릭 하 <a href="xamlcontrolsgallery:/item/Pivot">여 앱을 열고 작동 중인 피벗 컨트롤을 참조 하세요</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

[NavigationView](navigationview.md)마찬가지로 피벗 컨트롤에 선택한 항목에 밑줄을 표시 합니다.

![기본 포커스는 선택된 헤더에 밑줄로 표시됩니다.](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

일반적인 상단 탐색 및 탭 패턴을 구현 하려면 [NavigationView](navigationview.md)를 자동으로 다른 화면 크기에 맞게 조정할 수 있도록 하는 사용자 지정에 대 한를 사용 하는 것이 좋습니다.

그러나 탐색을 위해 터치 살짝 밀기, 경우 피벗을 사용 하는 것이 좋습니다.

NavigationView 및 피벗 컨트롤 간의 주요 차이점 기본 오버플로 동작 및 탐색 API는:

- 피벗 항목을 NavigationView의 드롭다운 메뉴를 사용 하는 동안 사용자가 모든 항목을 볼 수 있도록 오버플로 컨베이어 벨트 오버플로.
- 피벗 콘텐츠 섹션을 더 많이 제어할 탐색 동작을 사용 하면 NavigationView 간의 탐색을 처리 합니다.

## <a name="use-navigationview-instead-of-pivot"></a>NavigationView를 사용 하 여 피벗 대신

앱의 UI가 피벗 컨트롤을 사용 하는 경우 다음을 변환할 수 있습니다 피벗 NavigationView 아래 코드를 사용 하 여 합니다.

이 XAML은 3 개의 [만들기 피벗 컨트롤](#create-a-pivot-control)피벗 예제와 마찬가지로 콘텐츠 섹션이 있는 NavigationView를 만듭니다.

```xaml
<NavigationView x:Name="rootNavigationView" Header="Category Title"
 ItemInvoked="NavView_ItemInvoked">
    <NavigationView.MenuItems>
        <NavigationViewItem Content="Section 1" x:Name="Section1Content" />
        <NavigationViewItem Content="Section 2" x:Name="Section2Content" />
        <NavigationViewItem Content="Section 3" x:Name="Section3Content" />
    </NavigationView.MenuItems>
</NavigationView>

<Page x:Class="AppName.Section1Page">
    <TextBlock Text="Content of section 1."/>
</Page>

<Page x:Class="AppName.Section2Page">
    <TextBlock Text="Content of section 2."/>
</Page>

<Page x:Class="AppName.Section3Page">
    <TextBlock Text="Content of section 3."/>
</Page>
```

NavigationView는 더 많은 탐색 사용자 지정 컨트롤을 제공 하 고 해당 코드 숨김 필요 합니다. 위의 XAML 권의 다음 코드 숨김을 사용:

```csharp
private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{
    FrameNavigationOptions navOptions = new FrameNavigationOptions();
    navOptions.TransitionInfoOverride = new SlideNavigationTransitionInfo() {
         SlideNavigationTransitionDirection=args.RecommendedNavigationTransitionInfo
    };

    navOptions.IsNavigationStackEnabled = False;

    switch (item.Name)
    {
        case "Section1Content":
            ContentFrame.NavigateToType(typeof(Section1Page), null, navOptions);
            break;

        case "Section2Content":
            ContentFrame.NavigateToType(typeof(Section2Page), null, navOptions);
            break;

        case "Section3Content":
            ContentFrame.NavigateToType(typeof(Section3Page), null, navOptions);
            break;
    }  
}
```

이 코드는 콘텐츠 섹션 간에 터치 살짝 경험-피벗 컨트롤의 기본 제공 탐색 경험을 모방합니다. 그러나 알 수 있듯이 사용자 지정할 수 있습니다 애니메이션된 전환, 탐색 매개 변수 및 스택 기능을 포함 하는 몇 가지 요소입니다.

## <a name="create-a-pivot-control"></a>피벗 컨트롤 만들기

이 코드는 3 개의 콘텐츠 섹션이 있는 기본 피벗 컨트롤을 만듭니다.

```xaml
<Pivot x:Name="rootPivot" Title="Category Title">
    <PivotItem Header="Section 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 1."/>
    </PivotItem>
    <PivotItem Header="Section 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 2."/>
    </PivotItem>
    <PivotItem Header="Section 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 3."/>
    </PivotItem>
</Pivot>
```

### <a name="pivot-items"></a>피벗 항목

피벗은 [ItemsControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx)이므로 모든 유형의 항목 컬렉션을 포함할 수 있습니다. 피벗에 추가한 항목 중 명시적으로 [PivotItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx)이 아닌 항목은 모두 PivotItem에 암시적으로 래핑됩니다. 피벗은 콘텐츠 페이지 간 탐색에 사용되는 경우가 많기 때문에 [Items](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) 컬렉션에 XAML UI 요소를 직접 채우는 것이 일반적입니다. 또는 데이터 원본에 [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 속성을 설정할 수 있습니다. ItemsSource에 바인딩된 항목은 임의 유형일 수 있지만 명시적으로 PivotItems가 아닌 경우 [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 및 [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx)을 정의하여 항목의 표시 방법을 지정해야 합니다.

[SelectedItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) 속성을 사용하여 피벗의 활성 항목을 가져오거나 설정할 수 있습니다. 활성 항목의 인덱스를 설정하려면 [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) 속성을 사용합니다.

### <a name="pivot-headers"></a>피벗 헤더

[LeftHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) 및 [RightHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) 속성을 사용하여 피벗 헤더에 다른 컨트롤을 추가할 수 있습니다.

예를 들어 피벗의 RightHeader에 [CommandBar](https://docs.microsoft.com/en-us/windows/uwp/controls-and-patterns/app-bars)를 추가할 수 있습니다.

```xaml
<Pivot>
    <Pivot.RightHeader>
        <CommandBar>
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit"/>
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
        </CommandBar>
    </Pivot.RightHeader>
</Pivot>
```

### <a name="pivot-interaction"></a>피벗 조작

컨트롤을 통해 다음 터치 제스처 조작 기능을 사용할 수 있습니다.

- 피벗 컨트롤 헤더를 탭하면 해당 헤더의 섹션 콘텐츠로 이동합니다.
- 피벗 항목 헤더를 왼쪽 또는 오른쪽으로 살짝 밀면 인접한 섹션으로 이동합니다.
- 섹션 콘텐츠에서 왼쪽 또는 오른쪽으로 살짝 밀면 인접한 섹션으로 이동합니다.

컨트롤은 다음 두 모드를 제공합니다.

**고정**

- 모든 피벗 헤더가 허용되는 공간 내에 맞는 경우 피벗은 고정됩니다.
- 피벗 레이블을 탭하면 피벗 자체는 움직이지 않고 해당 페이지로 이동합니다. 활성 피벗이 강조 표시됩니다.

**회전**

- 모든 피벗 헤더가 허용되는 공간 내에 맞지 않는 경우 피벗이 회전합니다.
- 피벗 레이블을 탭하면 해당 페이지로 이동하며 활성 피벗 레이블이 첫 번째 위치로 회전합니다.
- 회전 모드에서 피벗 항목은 마지막에서부터 첫 번째 피벗 섹션까지 루핑합니다.

> **참고** [3m 환경](../devices/designing-for-tv.md)에서는 피벗 헤더가 회전하면 안 됩니다. Xbox에서 실행되는 앱일 경우 [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) 속성을 **false**로 설정합니다.

## <a name="recommendations"></a>권장 사항

- 회전(왕복) 모드를 사용할 경우 5개를 초과하는 헤더를 사용하면 혼란이 발생할 수 있으므로 5개를 초과하는 헤더를 루핑하지 않도록 합니다.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-topics"></a>관련 항목

- [피벗 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [탐색 디자인 기본 사항](../basics/navigation-basics.md)