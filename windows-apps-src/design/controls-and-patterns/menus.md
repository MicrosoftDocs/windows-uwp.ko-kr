---
Description: 메뉴 및 상황에 맞는 메뉴는 사용자 요청에 따라 명령 또는 옵션 목록을 표시합니다.
title: 메뉴 및 상황에 맞는 메뉴
label: Menus and context menus
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
ms.custom: RS5, 19H1
keywords: windows 10, uwp
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e44da32b5eec603ed9a334b4b870ca1176047a97
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218656"
---
# <a name="menus-and-context-menus"></a>메뉴 및 상황에 맞는 메뉴

메뉴 및 상황에 맞는 메뉴는 사용자 요청에 따라 명령 또는 옵션 목록을 표시합니다. 메뉴 플라이아웃을 사용하여 단일 인라인 메뉴를 표시합니다. 메뉴 막대를 사용하여 일반적으로 앱 창의 맨 위에 있는 가로 행에 메뉴 세트를 표시합니다. 각 메뉴에는 메뉴 항목과 하위 메뉴가 있을 수 있습니다.

![일반적인 상황에 맞는 메뉴 예제](images/contextmenu_rs2_icons.png)

**Windows UI 라이브러리 가져오기**

|  |  |
| - | - |
| ![WinUI 로고](images/winui-logo-64x64.png) | **MenuBar** 컨트롤은 Windows 앱용 새 컨트롤과 UI 기능을 포함하는 NuGet 패키지인 Windows UI 라이브러리의 일부로 포함되어 있습니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리 개요](/uwp/toolkits/winui/)를 참조하세요. |

> **Windows UI 라이브러리 API:** [MenuBar 클래스](/uwp/api/microsoft.ui.xaml.controls.menubar)
>
> **플랫폼 API:** [MenuFlyout 클래스](/uwp/api/windows.ui.xaml.controls.menuflyout), [MenuBar 클래스](/uwp/api/windows.ui.xaml.controls.menubar), [ContextFlyout 속성](/uwp/api/windows.ui.xaml.uielement.contextflyout), [FlyoutBase.AttachedFlyout 속성](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase#xaml-attached-properties)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

메뉴 및 상황에 맞는 메뉴는 명령을 구성하고 사용자가 요청할 때까지 이를 숨겨 공간을 절약합니다. 특정 명령이 자주 사용되고 사용 가능한 공간이 있다면 사용자가 메뉴를 사용하지 않고도 이용할 수 있도록 메뉴 대신 해당 요소에 직접 배치할 수 있습니다.

메뉴와 상황에 맞는 메뉴는 명령을 구성하기 위한 것입니다. 알림, 확인 요청 등의 임의 콘텐츠를 표시하려면 [대화 상자 또는 플라이아웃](./dialogs-and-flyouts/index.md)을 사용하세요.

### <a name="menubar-vs-menuflyout"></a>MenuBar와 MenuFlyout 비교

캔버스 UI 요소에 연결된 메뉴를 플라이아웃에 표시하려면 MenuFlyout 컨트롤을 사용하여 메뉴 항목을 호스트합니다. 메뉴 플라이아웃을 일반 메뉴 또는 상황에 맞는 메뉴로 호출할 수 있습니다. 메뉴 플라이아웃은 단일 최상위 메뉴(및 선택적 하위 메뉴)를 호스트합니다.

여러 개의 최상위 메뉴 세트를 가로 행에 표시하려면 메뉴 모음을 사용합니다. 일반적으로 메뉴 모음은 앱 창의 맨 위에 배치합니다.

### <a name="menubar-vs-commandbar"></a>MenuBar와 CommandBar

MenuBar와 CommandBar는 모두 사용자에게 명령을 노출하는 데 사용할 수 있는 표면을 나타냅니다. MenuBar는 CommandBar에서 허용되는 것보다 많은 조직 또는 그룹화가 필요할 수 있는 앱용 명령 세트를 노출하는 빠르고 간단한 방법을 제공합니다.

CommandBar와 MenuBar를 함께 사용할 수도 있습니다. MenuBar를 사용하여 대량 명령을 제공하고, CommandBar를 사용하여 가장 많이 사용하는 명령을 강조 표시합니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/MenuFlyout">앱을 열고 MenuFlyout의 기능을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>메뉴 및 상황에 맞는 메뉴

메뉴 및 상황에 맞는 메뉴는 모양과 포함할 수 있는 항목이 유사합니다. 실제로 동일한 컨트롤인 [MenuFlyout](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout)을 사용하여 만들 수 있습니다. 차이점은 사용자가 액세스하는 방법입니다.

메뉴 또는 상황에 맞는 메뉴는 언제 사용해야 하나요?

- 호스트 요소가 단추 또는 주 역할이 추가 명령을 제공하는 것인 다른 명령 요소인 경우 메뉴를 사용합니다.
- 호스트 요소가 주요 목적과 형식이 다른 일부 요소를 사용하는 경우(예: 텍스트 또는 이미지 표시) 상황에 맞는 메뉴를 사용합니다.

예를 들어 단추의 메뉴를 사용하여 목록에 대한 필터링 및 정렬 옵션을 제공합니다. 이 시나리오에서 단추 컨트롤의 주요 목적은 메뉴에 대한 액세스를 제공하는 것입니다.

![메일 메뉴 예제](images/Mail_Menu.png)

잘라내기, 복사 및 붙여넣기 등의 명령을 텍스트 요소에 추가하려면 메뉴 대신 상황에 맞는 메뉴를 사용합니다. 이 시나리오에서 텍스트 요소의 주 역할은 텍스트 표시 및 편집입니다. 잘라내기, 복사 및 붙여넣기 등의 추가 명령은 보조 역할이므로 상황에 맞는 메뉴에 포함됩니다.

![사진 갤러리의 상황에 맞는 메뉴 예제](images/ContextMenu_example.png)

### <a name="menus"></a>메뉴

- 단일 진입점(예: 화면의 맨 위에 있는 파일 메뉴)이 항상 표시되도록 합니다.
- 일반적으로 단추 또는 부모 메뉴 항목에 연결됩니다.
- 마우스 왼쪽 단추로 클릭하거나 이와 동등한 작업(예: 손가락으로 탭)을 통해 호출됩니다.
- 해당 [Flyout](/uwp/api/windows.ui.xaml.controls.button.flyout) 또는 [FlyoutBase.AttachedFlyout](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase#xaml-attached-properties) 속성을 통해 요소와 연결되거나, 앱 창의 맨 위에 있는 메뉴 막대에 그룹화됩니다.

### <a name="context-menus"></a>상황에 맞는 메뉴

- 단일 요소에 연결되고 보조 명령을 표시합니다.
- 마우스 오른쪽 단추를 클릭하거나 이와 동등한 작업(예: 손가락으로 길게 누르기)을 통해 호출됩니다.
- 해당 [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) 속성을 통해 요소와 연결됩니다.

## <a name="icons"></a>아이콘

다음에 대해 메뉴 항목 아이콘을 제공하는 것이 좋습니다.

- 가장 일반적으로 사용되는 항목
- 아이콘이 표준이거나 잘 알려진 메뉴 항목
- 아이콘이 명령의 기능을 잘 설명하는 메뉴 항목

표준 시각화가 없는 명령에 대해 반드시 아이콘을 제공해야 하는 것은 아닙니다. 모호한 아이콘은 도움이 되지 않으며, 화면을 복잡하게 만들고, 사용자가 중요한 메뉴 항목에 집중할 수 없게 합니다.

![아이콘이 있는 상황에 맞는 메뉴 예제](images/contextmenu_rs2_icons.png)

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
> MenuFlyoutItem의 아이콘 크기는16x16px입니다. SymbolIcon, FontIcon 또는 PathIcon을 사용하는 경우 화질 손실 없이 아이콘이 올바른 크기로 자동 조정됩니다. BitmapIcon을 사용하는 경우 자산이 16x16px인지 확인합니다.  

## <a name="create-a-menu-flyout-or-a-context-menu"></a>메뉴 플라이아웃 또는 상황에 맞는 메뉴 만들기

메뉴 플라이아웃 또는 상황에 맞는 메뉴를 만들려면 [MenuFlyout 클래스](/uwp/api/windows.ui.xaml.controls.menuflyout)를 사용합니다. [MenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.menuflyoutitem), [MenuFlyoutSubItem](/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem), [ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), [RadioMenuFlyoutItem](/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem) 및 [MenuFlyoutSeparator](/uwp/api/windows.ui.xaml.controls.menuflyoutseparator) 개체를 MenuFlyout에 추가하여 메뉴 콘텐츠를 정의합니다.

이러한 개체는 다음 작업에 사용됩니다.

- [MenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.menuflyoutitem)—즉시 작업 수행
- [MenuFlyoutSubItem](/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem) - 메뉴 항목의 계단식 목록 포함
- [ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)—옵션 켜기 또는 끄기
- [RadioMenuFlyoutItem](/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem) - 함께 사용할 수 없는 메뉴 항목 간에 전환
- [MenuFlyoutSeparator](/uwp/api/windows.ui.xaml.controls.menuflyoutseparator)—메뉴 항목을 시각적으로 구분

이 예제에서는 [MenuFlyout](/uwp/api/windows.ui.xaml.controls.menuflyout)을 만들고, 대부분의 컨트롤에서 사용할 수 있는 [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) 속성을 사용하여 MenuFlyout을 상황에 맞는 메뉴로 표시합니다.

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

다음 예제는 거의 동일하지만 [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) 속성을 사용하여 [MenuFlyout 클래스](/uwp/api/windows.ui.xaml.controls.menuflyout)를 상황에 맞는 메뉴로 표시하는 대신 [FlyoutBase.ShowAttachedFlyout](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) 속성을 사용하여 메뉴로 표시합니다.

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

메뉴, 상황에 맞는 메뉴 및 기타 플라이아웃 등의 빠른 해제 컨트롤은 해제될 때까지 키보드 및 게임 패드 포커스를 임시 UI 내에 트래핑합니다. 이 동작에 대한 시각 신호를 제공하기 위해 Xbox의 빠른 해제 컨트롤은 범위를 벗어난 UI를 흐리게 표시하는 오버레이를 그립니다. 이 동작은 [LightDismissOverlayMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode) 속성으로 수정할 수 있습니다. 기본적으로 임시 UI는 Xbox(**자동**)에 빠른 해제 오버레이를 그리지만 다른 디바이스 패밀리에는 그리지 않습니다. 오버레이를 항상 **켜짐** 또는 항상 **꺼짐**으로 강제 적용할 수 있습니다.

```xaml
<MenuFlyout LightDismissOverlayMode="Off" />
```

## <a name="create-a-menu-bar"></a>메뉴 모음 만들기

> [!IMPORTANT]
> MenuBar에는 Windows 10, 버전 1809([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 이상 또는 [Windows UI Library](/uwp/toolkits/winui/)가 필요합니다.

메뉴 플라이아웃과 동일한 요소를 사용하여 메뉴 모음에 메뉴를 만듭니다. 그러나 MenuFlyout에 MenuFlyoutItem 개체를 그룹화하는 대신 MenuBarItem 요소에 그룹화합니다. 각 MenuBarItem은 최상위 메뉴로 MenuBar에 추가됩니다.

![메뉴 모음 예제](images/menu-bar-submenu.png)

> [!NOTE]
> 이 예제에서는 UI 구조를 만드는 방법만 보여 주고, 명령 구현은 표시하지 않습니다.

```xaml
<muxc:MenuBar>
    <muxc:MenuBarItem Title="File">
        <MenuFlyoutSubItem Text="New">
            <MenuFlyoutItem Text="Plain Text Document"/>
            <MenuFlyoutItem Text="Rich Text Document"/>
            <MenuFlyoutItem Text="Other Formats..."/>
        </MenuFlyoutSubItem>
        <MenuFlyoutItem Text="Open..."/>
        <MenuFlyoutItem Text="Save"/>
        <MenuFlyoutSeparator />
        <MenuFlyoutItem Text="Exit"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="Edit">
        <MenuFlyoutItem Text="Undo"/>
        <MenuFlyoutItem Text="Cut"/>
        <MenuFlyoutItem Text="Copy"/>
        <MenuFlyoutItem Text="Paste"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="View">
        <MenuFlyoutItem Text="Output"/>
        <MenuFlyoutSeparator/>
        <muxc:RadioMenuFlyoutItem Text="Landscape" GroupName="OrientationGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Portrait" GroupName="OrientationGroup" IsChecked="True"/>
        <MenuFlyoutSeparator/>
        <muxc:RadioMenuFlyoutItem Text="Small icons" GroupName="SizeGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Medium icons" IsChecked="True" GroupName="SizeGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Large icons" GroupName="SizeGroup"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="Help">
        <MenuFlyoutItem Text="About"/>
    </muxc:MenuBarItem>
</muxc:MenuBar>
```

## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여 줍니다.
- [XAML 상황에 맞는 메뉴 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>관련된 문서

- [MenuFlyout 클래스](/uwp/api/windows.ui.xaml.controls.menuflyout)
- [MenuBar 클래스](/uwp/api/microsoft.ui.xaml.controls.menubar)
