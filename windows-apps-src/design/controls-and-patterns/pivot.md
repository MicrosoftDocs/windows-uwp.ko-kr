---
Description: Pivot 컨트롤을 통해 작은 콘텐츠 섹션 세트에서 터치하여 살짝 밀기를 사용할 수 있습니다.
title: Pivot
template: detail.hbs
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6d55c24dbf84e16e0bb4dedc9eaf2b89982e2993
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80081616"
---
# <a name="pivot"></a>Pivot

[Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) 컨트롤을 통해 작은 콘텐츠 섹션 세트에서 터치하여 살짝 밀기를 사용할 수 있습니다.

![기본 포커스는 선택된 헤더에 밑줄로 표시됩니다.](images/pivot_focus_selectedHeader.png)

**Windows UI 라이브러리 가져오기**

|  |  |
| - | - |
| ![WinUI 로고](images/winui-logo-64x64.png) | Windows UI 라이브러리 2.2 이상에는 둥근 모서리를 사용하는 이 컨트롤의 새 템플릿이 포함되어 있습니다. 자세한 내용은 [모서리 반경](/windows/uwp/design/style/rounded-corner)을 참조하세요. WinUI는 UWP 앱에 대한 새 컨트롤 및 UI 기능이 포함된 NuGet 패키지입니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)를 참조하세요. |

> **플랫폼 API**: [Pivot 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot), [NavigationView 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/Pivot">앱을 열고 Pivot 컨트롤의 기능을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

[NavigationView](navigationview.md)와 마찬가지로, Pivot 컨트롤은 선택한 항목에 밑줄을 표시합니다.

![기본 포커스는 선택된 헤더에 밑줄로 표시됩니다.](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

일반적인 최상위 탐색 및 탭 패턴을 얻으려면 다양한 화면 크기에 맞게 자동으로 조정되고 더 많은 사용자 지정을 허용하는 [NavigationView](navigationview.md)를 사용하는 것이 좋습니다.

그러나 탐색에 터치하여 살짝 밀기가 필요한 경우 Pivot을 사용하는 것이 좋습니다.

NavigationView 및 Pivot 컨트롤 간의 다른 주요 차이점은 기본 오버플로 동작과 탐색 API입니다.

- NavigationView는 사용자가 모든 항목을 볼 수 있도록 메뉴 드롭다운 오버플로를 사용하는 반면, Pivot은 오버플로 항목을 캐러셀로 표시합니다.
- NavigationView를 사용하면 탐색 동작을 더 완벽하게 제어할 수 있는 반면, Pivot은 콘텐츠 섹션 간의 탐색을 처리합니다.

## <a name="use-navigationview-instead-of-pivot"></a>Pivot 대신 NavigationView 사용

앱의 UI가 Pivot 컨트롤을 사용하는 경우, 아래 코드를 사용하여 Pivot을 NavigationView로 변환할 수 있습니다.

이 XAML은 [ 컨트롤 만들기](#create-a-pivot-control)의 예제 Pivot과 같이 콘텐츠 섹션 3개가 있는 NavigationView를 만듭니다.

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

NavigationView를 사용하면 탐색 사용자 지정을 더 완벽하게 제어할 수 있으며, 해당 코드 숨김이 필요합니다. 위 XAML의 경우 다음 코드 숨김을 사용합니다.

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

이 코드는 콘텐츠 섹션 간의 터치하여 살짝 밀기 환경을 제외하고 Pivot 컨트롤의 기본 제공 탐색 환경을 모방합니다. 그러나 애니메이션 효과를 준 전환, 탐색 매개 변수, 스택 기능을 포함하여 여러 가지 사항을 사용자 지정할 수도 있습니다.

## <a name="create-a-pivot-control"></a>피벗 컨트롤 만들기

이 코드는 콘텐츠 섹션 3개가 있는 기본 Pivot 컨트롤을 만듭니다.

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

피벗은 [ItemsControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl)이므로 모든 유형의 항목 컬렉션을 포함할 수 있습니다. 피벗에 추가한 항목 중 명시적으로 [PivotItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PivotItem)이 아닌 항목은 모두 PivotItem에 암시적으로 래핑됩니다. 피벗은 콘텐츠 페이지 간 탐색에 사용되는 경우가 많기 때문에 [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 컬렉션에 XAML UI 요소를 직접 채우는 것이 일반적입니다. 또는 데이터 원본에 [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 속성을 설정할 수 있습니다. ItemsSource에 바인딩된 항목은 임의 유형일 수 있지만 명시적으로 PivotItems가 아닌 경우 [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 및 [HeaderTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.headertemplate)을 정의하여 항목의 표시 방법을 지정해야 합니다.

[SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.selecteditem) 속성을 사용하여 피벗의 활성 항목을 가져오거나 설정할 수 있습니다. 활성 항목의 인덱스를 설정하려면 [SelectedIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.selectedindex) 속성을 사용합니다.

### <a name="pivot-headers"></a>피벗 헤더

[LeftHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.leftheader) 및 [RightHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.rightheader) 속성을 사용하여 피벗 헤더에 다른 컨트롤을 추가할 수 있습니다.

예를 들어 피벗의 RightHeader에 [CommandBar](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/app-bars)를 추가할 수 있습니다.

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

**캐러셀**

- 모든 피벗 헤더가 허용되는 공간 내에 맞지 않는 경우 피벗이 회전합니다.
- 피벗 레이블을 탭하면 해당 페이지로 이동하며 활성 피벗 레이블이 첫 번째 위치로 회전합니다.
- 회전 모드에서 피벗 항목은 마지막에서부터 첫 번째 피벗 섹션까지 루핑합니다.

> **참고**[3m 환경](../devices/designing-for-tv.md)에서는 Pivot 헤더가 회전하면 안 됩니다. 앱이 Xbox에서 실행되는 경우 [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) 속성을 **false**로 설정합니다.

## <a name="recommendations"></a>권장 사항

- 회전(왕복) 모드를 사용할 경우 5개를 초과하는 헤더를 사용하면 혼란이 발생할 수 있으므로 5개를 초과하는 헤더를 루핑하지 않도록 합니다.

## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-topics"></a>관련 항목

- [Pivot 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [탐색 디자인 기본 사항](../basics/navigation-basics.md)