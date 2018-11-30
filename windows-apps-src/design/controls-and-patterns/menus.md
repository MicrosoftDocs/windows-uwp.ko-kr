---
Description: Menus and context menus display a list of commands or options when the user requests them.
title: 메뉴 및 상황에 맞는 메뉴
label: Menus and context menus
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, uwp
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 9edf7bcb2ad76ed02887dfffc3e72d0d47f5aa1a
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8201307"
---
# <a name="menus-and-context-menus"></a>메뉴 및 상황에 맞는 메뉴

메뉴 및 상황에 맞는 메뉴는 사용자 요청에 따라 명령 또는 옵션 목록을 표시합니다. 메뉴 플라이 아웃을 사용 하 여 단일, 인라인 메뉴를 표시 합니다. 메뉴 모음을 사용 하 여 앱 창의 위쪽에 일반적으로 가로 행에서 일련의 메뉴를 표시 합니다. 각 메뉴 메뉴 항목 및 하위 메뉴에 있을 수 있습니다.

![일반적인 상황에 맞는 메뉴 예](images/contextmenu_rs2_icons.png)

| **Windows UI 라이브러리 가져오기** |
| - |
| 이 컨트롤은 Windows UI 라이브러리 새 컨트롤 및 UWP 앱에 대 한 UI 기능을 포함 하는 NuGet 패키지의 일부로 포함 합니다. 설치 지침을 비롯 한 자세한 내용은 [Windows UI 라이브러리 개요](https://docs.microsoft.com/uwp/toolkits/winui/)를 참조 하세요. |

| **플랫폼 Api** | **Windows UI 라이브러리 Api** |
| - | - |
| [MenuFlyout 클래스](/uwp/api/windows.ui.xaml.controls.menuflyout), [메뉴 모음 클래스](/uwp/api/windows.ui.xaml.controls.menubar) [ContextFlyout 속성](/uwp/api/windows.ui.xaml.uielement.contextflyout), [FlyoutBase.AttachedFlyout 속성](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout) | [메뉴 모음 클래스](/uwp/api/microsoft.ui.xaml.controls.menubar) |

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

메뉴 및 상황에 맞는 메뉴는 명령을 구성하고 사용자가 요청할 때까지 이를 숨겨 공간을 절약합니다. 특정 명령이 자주 사용되고 사용 가능한 공간이 있다면 사용자가 메뉴를 사용하지 않고도 이용할 수 있도록 메뉴 대신 해당 요소에 직접 배치할 수 있습니다.

메뉴 및 상황에 맞는 메뉴는 명령을 구성 알림 또는 확인 요청 등의 임의 콘텐츠를 표시 하는 [대화 상자 또는 플라이 아웃을](dialogs.md)사용 합니다.

### <a name="menubar-vs-menuflyout"></a>메뉴 모음 및 MenuFlyout

캔버스에서 UI 요소에 연결 된 플라이 아웃에 메뉴를 표시 하려면 호스트 메뉴 항목을 MenuFlyout 컨트롤을 사용 합니다. 일반 메뉴 또는 상황에 맞는 메뉴 메뉴 플라이 아웃을 호출할 수 있습니다. 메뉴 플라이 아웃 단일 최상위 메뉴 (및 선택적 하위 메뉴)를 호스트합니다.

가로 행에 일련의 여러 최상위 메뉴를 표시 하려면 메뉴 모음을 사용 합니다. 일반적으로 앱 창의 맨 위에 있는 메뉴 모음을 배치 합니다.

### <a name="menubar-vs-commandbar"></a>CommandBar 및 메뉴 모음

메뉴 모음과 CommandBar 둘 다 사용자에 게 명령을 노출 하는 데 사용할 수 있는 표면을 나타냅니다. 메뉴 모음에는 더 많은 조직 또는 허용 되는 CommandBar 보다 그룹화 할 수 있는 앱에 대 한 명령 집합을 노출 하는 빠르고 간단한 방법을 제공 합니다.

메뉴 모음 CommandBar와 함께에서 사용할 수도 있습니다. 자주 사용 되는 명령을 강조 표시 하는 명령을과 CommandBar 제공 하는 메뉴 모음을 사용 합니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/MenuFlyout">앱을 열고 작동 중인 MenuFlyout을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>메뉴 및 상황에 맞는 메뉴

메뉴 및 상황에 맞는 메뉴는 모양과 포함할 수 있는 항목 및 유사 합니다. 실제로 동일한 컨트롤인 [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030)을 만들기 위해 사용할 수 있습니다. 차이점은 사용자가 액세스할 수 있도록 하는 방법입니다.

메뉴 또는 상황에 맞는 메뉴는 언제 사용해야 하나요?

- 호스트 요소가 단추이거나 또는 주 역할이 추가 명령을 제공하는 일부 다른 명령 요소인 경우 메뉴를 사용합니다.
- 호스트 요소가 주요 목적과 형식이 다른 일부 요소를 사용하는 경우(예: 텍스트 또는 이미지 표시) 상황에 맞는 메뉴를 사용합니다.

예를 들어, 필터링 및 정렬 목록에 대 한 옵션을 제공 하기 위해 단추에 메뉴를 사용 합니다. 이 시나리오에서 단추 컨트롤의 주요 목적은 메뉴에 대한 액세스를 제공하는 것입니다.

![메일 메뉴의 예](images/Mail_Menu.png)

잘라내기, 복사 및 붙여넣기 등의 명령을 텍스트 요소에 추가하려면 메뉴 대신 상황에 맞는 메뉴를 사용합니다. 이 시나리오에서 텍스트 요소의 주 역할은 텍스트 표시 및 편집입니다. 잘라내기, 복사 및 붙여넣기 등의 추가 명령은 보조 역할이므로 상황에 맞는 메뉴에 포함됩니다.

![사진 갤러리의 컨텍스트 메뉴의 예](images/ContextMenu_example.png)

### <a name="menus"></a>메뉴

- 단일 진입점(예: 화면의 맨 위에 있는 파일 메뉴)이 항상 표시되도록 합니다.
- 일반적으로 단추 또는 부모 메뉴 항목에 연결됩니다.
- 마우스 왼쪽 단추로 클릭하거나 이와 동등한 작업(예: 손가락으로 탭)을 통해 호출됩니다.
- [플라이 아웃](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) 또는 [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) 속성을 통해 요소와 관련 된 또는 앱 창의 맨 위에 있는 메뉴 모음에서 그룹화 됩니다.

### <a name="context-menus"></a>상황에 맞는 메뉴

- 단일 요소에 연결되고 보조 명령을 표시합니다.
- 마우스 오른쪽 단추로 클릭하거나 이와 동등한 작업(예: 손가락으로 길게 누르기)을 통해 호출됩니다.
- 해당 [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 속성을 통해 요소와 연결됩니다.

## <a name="icons"></a>아이콘

다음에 대한 메뉴 항목 아이콘 제공을 고려하세요.

- 가장은 일반적으로 사용 되는 항목입니다.
- 아이콘이 표준 이거나 잘 알려진은 메뉴 항목입니다.
- 메뉴 항목 아이콘이 잘 명령에 대해 설명 합니다.

반드시 표준 시각화가 없는 명령에 대해 아이콘을 제공할 필요는 없습니다. 모호한 아이콘은 도움이 되지 않으며, 화면을 복잡하게 만들고, 사용자가 중요한 메뉴 항목에 집중하지 못하게 합니다.

![아이콘이 있는 상황에 맞는 메뉴의 예](images/contextmenu_rs2_icons.png)

````xaml
<MenuFlyout>
  <MenuFlyoutItem Text="Share" >
    <MenuFlyoutItem.Icon>
      <FontIcon Glyph="&#xE72D;" />
    </MenuFlyoutItem.Icon>
  </MenuFlyoutItem>
  <MenuFlyoutItem Text="Copy" Icon="Copy" />
  <MenuFlyoutItem Text="Delete" Icon="Delete" />
  <MenuFlyoutSeparator />
  <MenuFlyoutItem Text="Rename" />
  <MenuFlyoutItem Text="Select" />
</MenuFlyout>
````

> [!TIP]
> MenuFlyoutItem의 아이콘 크기는 16x16px입니다. SymbolIcon, FontIcon 또는 PathIcon을 사용 하는 경우 아이콘 화질 손실 없이 배율이 올바른 크기로 자동으로 조정 됩니다. BitmapIcon을 사용할 경우 자산이 16x16px인지 확인하세요.  

## <a name="create-a-menu-flyout-or-a-context-menu"></a>메뉴 플라이 아웃 이나 상황에 맞는 메뉴 만들기

메뉴 플라이 아웃 이나 상황에 맞는 메뉴를 만들려면 [MenuFlyout 클래스](https://msdn.microsoft.com/library/windows/apps/dn299030)를 사용 합니다. [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx), [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) 및 [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) 개체를 MenuFlyout에 추가하여 메뉴 콘텐츠를 정의합니다.

이러한 개체는 다음 작업에 사용됩니다.

- [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx)—즉시 작업 수행
- [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx)—옵션 켜기 또는 끄기
- [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx)—메뉴 항목을 시각적으로 구분

이 예제에서는 [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) 만들고 대부분의 컨트롤에서 사용할 수 있는 [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 속성을 사용 하 여 MenuFlyout 상황에 맞는 메뉴로 표시 합니다.

````xaml
<Rectangle
  Height="100" Width="100">
  <Rectangle.ContextFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </Rectangle.ContextFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red.
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````

다음 예제는 거의 동일하지만 [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 속성을 사용하여 [MenuFlyout 클래스](https://msdn.microsoft.com/library/windows/apps/dn299030)를 상황에 맞는 메뉴로 표시하는 대신 [FlyoutBase.ShowAttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) 속성을 사용하여 메뉴로 표시합니다.

````xaml
<Rectangle
  Height="100" Width="100"
  Tapped="Rectangle_Tapped">
  <FlyoutBase.AttachedFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </FlyoutBase.AttachedFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void Rectangle_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}

private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red.
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````

### <a name="light-dismiss"></a>빠른 해제

메뉴, 상황에 맞는 메뉴 및 기타 플라이아웃 등의 빠른 해제 컨트롤은 해제될 때까지 키보드 및 게임 패드 포커스를 임시 UI 내에 트래핑합니다. 이 동작에 대한 시각 신호를 제공하기 위해 Xbox의 빠른 해제 컨트롤은 범위를 벗어난 UI를 흐리게 표시하는 오버레이를 그립니다. 이 동작은 [LightDismissOverlayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx) 속성으로 수정할 수 있습니다. 기본적으로 임시 UI는 Xbox(**자동**)에 빠른 해제 오버레이를 그리지만 다른 장치 패밀리에는 그리지 않습니다. 그러나 앱에서 오버레이가 항상 **설정** 또는 항상 **해제** 상태가 되도록 선택할 수 있습니다.

```xaml
<MenuFlyout LightDismissOverlayMode="Off" />
```

## <a name="create-a-menu-bar"></a>메뉴 모음 만들기

> **미리 보기**: 메뉴 모음에 [최신 Windows 10 Insider Preview 빌드 및 SDK](https://insider.windows.com/for-developers/) 또는 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)필요 합니다.

동일한 요소를 사용 하 여 같이 메뉴 플라이 아웃 메뉴 모음에서 메뉴를 만들 수 있습니다. 그러나 MenuFlyoutItem 개체를 MenuFlyout에 그룹화 하는 대신 그룹화 있습니다 MenuBarItem 요소에서. 각 MenuBarItem 최상위 메뉴의 메뉴 모음에 추가 됩니다.

![메뉴 모음의 예](images/menu-bar-submenu.png)

> [!NOTE]
> 이 예제에서는 UI 구조를 만드는 방법에 대해서만 표시 하지만 구현의 명령 중 하나는 표시 되지 않습니다.

```xaml
<MenuBar>
    <MenuBarItem Title="File">
        <MenuFlyoutSubItem Text="New">
            <MenuFlyoutItem Text="Plain Text Document"/>
            <MenuFlyoutItem Text="Rich Text Document"/>
            <MenuFlyoutItem Text="Other Formats..."/>
        </MenuFlyoutSubItem>
        <MenuFlyoutItem Text="Open..."/>
        <MenuFlyoutItem Text="Save">
        <MenuFlyoutSeparator />
        <MenuFlyoutItem Text="Exit"/>
    </MenuBarItem>

    <MenuBarItem Title="Edit">
        <MenuFlyoutItem Text="Undo"/>
        <MenuFlyoutItem Text="Cut"/>
        <MenuFlyoutItem Text="Copy"/>
        <MenuFlyoutItem Text="Paste"/>
    </MenuBarItem>

    <MenuBarItem Title="Help">
        <MenuFlyoutItem Text="About"/>
    </MenuBarItem>
</MenuBar>
```

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.
- [XAML 상황에 맞는 메뉴 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>관련 문서

- [MenuFlyout 클래스](https://msdn.microsoft.com/library/windows/apps/dn299030)
- [메뉴 모음 클래스](/uwp/api/Windows.UI.Xaml.Controls.MenuBar)
