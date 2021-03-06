---
description: 플라이아웃은 사용자가 요청할 경우나 알림 또는 승인이 필요한 문제가 발생할 경우 나타나는 임시 UI 요소를 표시합니다.
title: 플라이아웃 컨트롤
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: cfc4dbc0bb52e6ba3f932d791c1b751d15626bef
ms.sourcegitcommit: 40b890c7b862f333879887cc22faff560c49eae6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97598774"
---
# <a name="flyouts"></a>플라이아웃

플라이아웃은 빠른 해제 컨테이너로 임의의 UI를 내용으로 표시할 수 있습니다. 플라이아웃에 중첩된 환경을 만들기 위한 다른 플라이아웃이나 상황에 맞는 메뉴가 포함될 수 있습니다.

![플라이아웃 내에 중첩된 상황에 맞는 메뉴](../images/flyout-nested.png)

**Windows UI 라이브러리 가져오기**

:::row:::
   :::column:::
      ![WinUI 로고](../images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Windows UI 라이브러리 2.2 이상에는 둥근 모서리를 사용하는 이 컨트롤의 새 템플릿이 포함되어 있습니다. 자세한 내용은 [모서리 반경](../../style/rounded-corner.md)을 참조하세요. WinUI는 Windows 앱에 대한 새 컨트롤 및 UI 기능이 포함된 NuGet 패키지입니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리](/uwp/toolkits/winui/)를 참조하세요.
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **플랫폼 API:** [Flyout 클래스](/uwp/api/Windows.UI.Xaml.Controls.Flyout)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

* [도구 설명](../tooltips.md) 또는 [상황에 맞는 메뉴](../menus.md) 대신에 플라이아웃을 사용하지 마세요. 도구 설명을 사용하여 지정된 시간 후에 숨겨지는 간단한 설명을 표시합니다. 복사 및 붙여넣기 등 UI 요소에 관련된 상황별 작업을 위한 상황에 맞는 메뉴를 사용합니다.

플라이아웃을 사용해야 할 경우와 대화 상자(비슷한 컨트롤)를 사용해야 할 경우에 대한 권장 사항을 보려면 [대화 상자 및 플라이아웃](index.md)을 참조하세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="../images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML Controls Gallery</strong> 앱이 설치되어 있는 경우 여기를 클릭하여 앱을 열고 <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> 또는 <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a>이 작동되는지 확인합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

##  <a name="how-to-create-a-flyout"></a>플라이아웃을 만드는 방법


플라이아웃은 특정 컨트롤에 연결됩니다. [Placement](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.Placement) 속성을 사용하여 플라이아웃이 표시되는 위치(위쪽, 왼쪽, 아래쪽, 오른쪽 또는 전체)를 지정할 수 있습니다. [전체 배치 모드](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode)를 선택하면 앱은 플라이아웃을 확대하고 앱 창 내에서 가운데로 지정합니다. [Button](/uwp/api/Windows.UI.Xaml.Controls.Button)과 같은 일부 컨트롤은 플라이아웃이나 [상황에 맞는 메뉴](../menus.md)를 연결하는 데 사용할 수 있는 [Flyout](/uwp/api/Windows.UI.Xaml.Controls.Button.Flyout) 속성을 제공합니다.

이 예제에서는 단추를 누를 때 일부 텍스트를 표시하는 간단한 플라이아웃을 만듭니다.
````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="This is a flyout!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````

컨트롤에 플라이아웃 속성이 없으면 대신 [FlyoutBase.AttachedFlyout](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.AttachedFlyoutProperty) 관련 속성을 사용할 수 있습니다. 이 속성을 사용할 경우 [FlyoutBase.ShowAttachedFlyout](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase#Windows_UI_Xaml_Controls_Primitives_FlyoutBase_ShowAttachedFlyout_Windows_UI_Xaml_FrameworkElement_) 메서드도 호출해야 플라이아웃을 표시할 수 있습니다.

이 예제에서는 이미지에 간단한 플라이아웃을 추가합니다. 사용자가 이미지를 탭하면 앱이 플라이아웃을 표시합니다.

````xaml
<Image Source="Assets/cliff.jpg" Width="50" Height="50"
  Margin="10" Tapped="Image_Tapped">
  <FlyoutBase.AttachedFlyout>
    <Flyout>
      <TextBlock Text="This is some text in a flyout."  />
    </Flyout>
  </FlyoutBase.AttachedFlyout>
</Image>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}
````

앞의 예제에서 해당 플라이아웃 인라인이 정의되었습니다. 고정 리소스로 플라이아웃을 정의한 다음 여러 요소와 함께 사용할 수도 있습니다. 이 예제에서는 해당 미리 보기를 탭하는 경우 더 큰 버전의 이미지를 표시하는 보다 복잡한 플라이아웃을 만듭니다.

````xaml
<!-- Declare the shared flyout as a resource. -->
<Page.Resources>
    <Flyout x:Key="ImagePreviewFlyout" Placement="Right">
        <!-- The flyout's DataContext must be the Image Source
             of the image the flyout is attached to. -->
        <Image Source="{Binding Path=Source}"
            MaxHeight="400" MaxWidth="400" Stretch="Uniform"/>
    </Flyout>
</Page.Resources>
````

````xaml
<!-- Assign the flyout to each element that shares it. -->
<StackPanel>
    <Image Source="Assets/cliff.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/grapes.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/rainier.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
</StackPanel>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}
````

## <a name="style-a-flyout"></a>플라이아웃 스타일 지정
플라이아웃의 스타일을 지정하려면 해당 [FlyoutPresenterStyle](/uwp/api/Windows.UI.Xaml.Controls.Flyout.FlyoutPresenterStyle)을 수정합니다. 이 예제에서는 줄 바꿈 단락을 보여 주고, 화면 읽기 프로그램에서 텍스트 블록에 액세스하도록 설정합니다.

![텍스트 줄 바꿈을 포함한 액세스 가능한 플라이아웃](../images/flyout-wrapping-text.png)

````xaml
<Flyout>
  <Flyout.FlyoutPresenterStyle>
    <Style TargetType="FlyoutPresenter">
      <Setter Property="ScrollViewer.HorizontalScrollMode"
          Value="Disabled"/>
      <Setter Property="ScrollViewer.HorizontalScrollBarVisibility" Value="Disabled"/>
      <Setter Property="IsTabStop" Value="True"/>
      <Setter Property="TabNavigation" Value="Cycle"/>
    </Style>
  </Flyout.FlyoutPresenterStyle>
  <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."/>
</Flyout>
````

## <a name="styling-flyouts-for-10-foot-experiences"></a>3m 환경을 위한 플라이아웃 스타일 지정

플라이아웃과 같은 빠른 해제 컨트롤은 해제될 때까지 키보드 및 게임 패드 포커스를 임시 UI 내에 트래핑합니다. 이 동작에 대한 시각 신호를 제공하기 위해 Xbox의 빠른 해제 컨트롤은 범위를 벗어난 UI의 대비와 가시성을 줄이는 오버레이를 그립니다. 이 동작은 [`LightDismissOverlayMode`](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.LightDismissOverlayMode) 속성으로 수정할 수 있습니다. 기본적으로 플라이아웃은 Xbox에 빠른 해제 오버레이를 그리지만 다른 디바이스 패밀리에는 그리지 않습니다. 그러나 앱에서 오버레이가 항상 **설정** 또는 항상 **해제** 상태가 되도록 선택할 수 있습니다.

![흐리게 표시 오버레이를 포함한 플라이아웃](../images/flyout-smoke.png)

```xaml
<MenuFlyout LightDismissOverlayMode="On">
```

## <a name="light-dismiss-behavior"></a>빠른 해제 동작
다음을 포함한 빠른 해제 동작으로 플라이아웃을 닫을 수 있습니다.
-    플라이아웃 밖 탭
-    Esc 키보드 키 누르기
-    하드웨어 또는 소프트웨어 시스템의 뒤로 단추 누르기
-    게임 패드 B 단추 누르기

탭으로 해제 시 이 제스처는 대개 포함되고 UI 아래에 전달되지 않습니다. 예를 들어, 열려 있는 플라이아웃 뒤에 단추가 표시된 경우 사용자의 첫 번째 탭으로 플라이아웃이 해제되지만 이 단추가 활성화되지는 않습니다. 단추를 누르려면 두 번째 탭이 필요합니다.

플라이아웃에 대한 입력 통과 요소로 단추를 지정하여 이 동작을 변경할 수 있습니다. 플라이아웃이 위에 설명된 빠른 해제 작업의 결과로 닫히고 탭 이벤트를 지정된 `OverlayInputPassThroughElement`에 전달합니다. 이러한 동작을 채택하여 기능적으로 비슷한 항목에 대한 사용자 상호 작용 속도를 높일 수 있습니다. 앱에 즐겨찾기 컬렉션이 있고 컬렉션의 각 항목에 연결된 플라이아웃이 포함된 경우 사용자가 빠르게 여러 플라이아웃과 연달아 상호 작용할 것으로 예상할 수 있습니다.

[!NOTE] 파괴적 작업을 유발하는 오버레이 입력 통과 요소를 지정하지 않도록 주의하세요. 사용자는 기본 UI를 활성화하지 않는 빠른 해제 작업에 익숙해졌습니다. 예기치 않은 파괴적 동작을 피하기 위해 닫기, 삭제 또는 비슷하게 파괴적인 단추가 빠른 해제 시 활성화되면 안 됩니다.

다음 예에서는 FavoritesBar 내의 3가지 단추 모두 첫 번째 탭에서 활성화됩니다.

````xaml
<Page>
    <Page.Resources>
        <Flyout x:Name="TravelFlyout" x:Key="TravelFlyout"
                OverlayInputPassThroughElement="{x:Bind FavoritesBar}">
            <StackPanel>
                <HyperlinkButton Content="Washington Trails Association"/>
                <HyperlinkButton Content="Washington Cascades - Go Northwest! A Travel Guide"/>
            </StackPanel>
        </Flyout>
    </Page.Resources>

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="FavoritesBar" Orientation="Horizontal">
            <HyperlinkButton x:Name="PageLinkBtn">Bing</HyperlinkButton>
            <Button x:Name="Folder1" Content="Travel" Flyout="{StaticResource TravelFlyout}"/>
            <Button x:Name="Folder2" Content="Entertainment" Click="Folder2_Click"/>
        </StackPanel>
        <ScrollViewer Grid.Row="1">
            <WebView x:Name="WebContent"/>
        </ScrollViewer>
    </Grid>
</Page>
````
````csharp
private void Folder2_Click(object sender, RoutedEventArgs e)
{
     Flyout flyout = new Flyout();
     flyout.OverlayInputPassThroughElement = FavoritesBar;
     ...
     flyout.ShowAt(sender as FrameworkElement);
{
````

## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련된 문서
- [도구 설명](../tooltips.md)
- [메뉴 및 상황에 맞는 메뉴](../menus.md)
- [Flyout 클래스](/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [ContentDialog 클래스](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)
