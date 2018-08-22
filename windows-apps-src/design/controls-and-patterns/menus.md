---
author: mijacobs
Description: A flyout is a lightweight popup that is used to temporarily show UI that is related to what the user is currently doing.
title: 메뉴 및 상황에 맞는 메뉴
label: Menus and context menus
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e38e9d61e8546d412cc30bad26680243f3a188e4
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/22/2018
ms.locfileid: "2792346"
---
# <a name="menus-and-context-menus"></a>메뉴 및 상황에 맞는 메뉴



메뉴 및 상황에 맞는 메뉴는 사용자 요청에 따라 명령 또는 옵션 목록을 표시합니다.

> **중요 API**: [MenuFlyout 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout), [ContextFlyout 속성](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx), [FlyoutBase.AttachedFlyout 속성](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx)

![일반적인 상황에 맞는 메뉴 예](images/contextmenu_rs2_icons.png)


## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?
메뉴 및 상황에 맞는 메뉴는 명령을 구성하고 사용자가 요청할 때까지 이를 숨겨 공간을 절약합니다. 특정 명령이 자주 사용되고 사용 가능한 공간이 있다면 사용자가 메뉴를 사용하지 않고도 이용할 수 있도록 메뉴 대신 해당 요소에 직접 배치할 수 있습니다.

메뉴 및 상황에 맞는 메뉴의 목적은 명령을 구성하는 것입니다. 알림 등의 임의 콘텐츠를 표시하거나 확인을 요청하려면 [대화 상자 또는 플라이아웃](dialogs.md)을 사용하세요.  

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

메뉴 및 상황에 맞는 메뉴는 모양과 포함할 수 있는 항목 면에서 동일합니다. 실제로 동일한 컨트롤인 [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030)을 사용하여 만들어집니다. 유일한 차이점은 사용자가 액세스하는 방법입니다.

메뉴 또는 상황에 맞는 메뉴는 언제 사용해야 하나요?
* 호스트 요소가 단추이거나 또는 주 역할이 추가 명령을 제공하는 일부 다른 명령 요소인 경우 메뉴를 사용합니다.
* 호스트 요소가 주요 목적과 형식이 다른 일부 요소를 사용하는 경우(예: 텍스트 또는 이미지 표시) 상황에 맞는 메뉴를 사용합니다.

예를 들어 탐색 창의 단추에 메뉴를 사용하여 추가 탐색 옵션을 제공합니다. 이 시나리오에서 단추 컨트롤의 주요 목적은 메뉴에 대한 액세스를 제공하는 것입니다.

![메일 메뉴의 예](images/Mail_Menu.png)

잘라내기, 복사 및 붙여넣기 등의 명령을 텍스트 요소에 추가하려면 메뉴 대신 상황에 맞는 메뉴를 사용합니다. 이 시나리오에서 텍스트 요소의 주 역할은 텍스트 표시 및 편집입니다. 잘라내기, 복사 및 붙여넣기 등의 추가 명령은 보조 역할이므로 상황에 맞는 메뉴에 포함됩니다.

![사진 갤러리의 컨텍스트 메뉴의 예](images/ContextMenu_example.png) 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>메뉴</b></p>
<ul>
<li>단일 진입점(예: 화면의 맨 위에 있는 파일 메뉴)이 항상 표시되도록 합니다.</li>
<li>일반적으로 단추 또는 부모 메뉴 항목에 연결됩니다.</li>
<li>마우스 왼쪽 단추로 클릭하거나 이와 동등한 작업(예: 손가락으로 탭)을 통해 호출됩니다.</li><li>해당 <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx">Flyout</a> 또는 <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx">FlyoutBase.AttachedFlyout</a> 속성을 통해 요소와 연결됩니다.</li>
</ul>
</div>
  <div class="side-by-side-content-right">
   <p><b>상황에 맞는 메뉴</b></p>

<ul>
<li>단일 요소에 연결되고 보조 명령을 표시합니다.</li>
<li>마우스 오른쪽 단추로 클릭하거나 이와 동등한 작업(예: 손가락으로 길게 누르기)을 통해 호출됩니다.</li><li>해당 <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx">ContextFlyout</a> 속성을 통해 요소와 연결됩니다.</li>
</ul>
  </div>
</div>
</div>

## <a name="icons"></a>아이콘

다음에 대한 메뉴 항목 아이콘 제공을 고려하세요.

<ul>
<li> 가장 일반적으로 사용되는 항목 </li>
<li> 아이콘이 표준이거나 잘 알려진 메뉴 항목 </li>
<li> 아이콘이 명령의 기능을 잘 설명하는 메뉴 항목 </li>
</ul>

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
> MenuFlyoutItems의 아이콘 크기는 16x16px입니다. SymbolIcon, FontIcon 또는 PathIcon을 사용하는 경우 화질 손실 없이 아이콘 배율이 올바른 크기로 자동 조정됩니다. BitmapIcon을 사용할 경우 자산이 16x16px인지 확인하세요.  

## <a name="create-a-menu-or-a-context-menu"></a>메뉴 또는 상황에 맞는 메뉴 만들기

메뉴 또는 상황에 맞는 메뉴를 만들려면 [MenuFlyout 클래스](https://msdn.microsoft.com/library/windows/apps/dn299030)를 사용합니다. [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx), [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) 및 [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) 개체를 MenuFlyout에 추가하여 메뉴 콘텐츠를 정의합니다. 이러한 개체는 다음 작업에 사용됩니다.
* [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx)—즉시 작업 수행
* [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx)—옵션 켜기 또는 끄기
* [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx)—메뉴 항목을 시각적으로 구분


이 예제에서는 [MenuFlyout 클래스](https://msdn.microsoft.com/library/windows/apps/dn299030)를 사용하고 대부분의 컨트롤에서 사용할 수 있는 [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 속성을 사용하여 [MenuFlyout 클래스](https://msdn.microsoft.com/library/windows/apps/dn299030)를 상황에 맞는 메뉴로 표시합니다.

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


> 메뉴, 상황에 맞는 메뉴 및 기타 플라이아웃 등의 빠른 해제 컨트롤은 해제될 때까지 키보드 및 게임 패드 포커스를 임시 UI 내에 트래핑합니다. 이 동작에 대한 시각 신호를 제공하기 위해 Xbox의 빠른 해제 컨트롤은 범위를 벗어난 UI를 흐리게 표시하는 오버레이를 그립니다. 이 동작은 [LightDismissOverlayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx) 속성으로 수정할 수 있습니다. 기본적으로 임시 UI는 Xbox(**자동**)에 빠른 해제 오버레이를 그리지만 다른 장치 패밀리에는 그리지 않습니다. 그러나 앱에서 오버레이가 항상 **설정** 또는 항상 **해제** 상태가 되도록 선택할 수 있습니다.

> ```xaml
> <MenuFlyout LightDismissOverlayMode="Off" />
> ```

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.
- [XAML 상황에 맞는 메뉴 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>관련 문서

- [MenuFlyout 클래스](https://msdn.microsoft.com/library/windows/apps/dn299030)
