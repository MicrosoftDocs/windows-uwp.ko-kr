---
author: mijacobs
description: "표시는 앱의 대화형 요소에 깊이와 포커스를 더해주는 조명 효과입니다."
title: "강조 표시"
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: high
ms.openlocfilehash: 8ba0d9939d7ab1d9826ed2848e476499f09c628f
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2018
---
# <a name="reveal-highlight"></a>강조 표시

표시는 앱의 대화형 요소에 깊이와 포커스를 더해주는 조명 효과입니다.

> **중요 API**: [RevealBrush 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush), [RevealBackgroundBrush 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush), [RevealBorderBrush 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush), [RevealBrushHelper 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper), [VisualState 클래스](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

표시 동작은 포인터가 근처에 있을 때 클릭할 수 있는 콘텐츠의 컨테이너를 표시하여 이 작업을 수행합니다.

![Visual 표시](images/Nav_Reveal_Animation.gif)

표시는 개체 주변의 숨겨진 테두리를 노출하여 사용자가 상호 작용하는 공간을 이해하고 사용 가능한 작업을 이해하는 데 도움을 줍니다. 이는 목록 컨트롤과 버튼 그룹에 특히 중요합니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/Reveal">앱을 열고 작동 중인 표시를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>비디오 요약

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev013/player]

## <a name="reveal-and-the-fluent-design-system"></a>표시 및 Fluent 디자인 시스템

 Fluent 디자인 시스템을 사용하면 조명, 깊이, 움직임, 재질 및 배율이 통합된 선명한 현대식 UI를 만들 수 있습니다. 표시는 앱에 조명을 추가하는 Fluent 디자인 시스템 구성 요소입니다. 자세히 알아보려면 [UWP용 Fluent 디자인 개요](../fluent-design-system/index.md)를 확인하십시오.

## <a name="how-to-use-it"></a>사용 방법

일부 컨트롤에 대해 자동으로 표시됩니다. 다른 컨트롤에 대해 컨트롤에 특별한 스타일을 지정하여 표시를 사용할 수 있습니다.

## <a name="controls-that-automatically-use-reveal"></a>자동으로 표시를 사용하는 컨트롤

- [**ListView**](../controls-and-patterns/lists.md)
- [**GridView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**AutosuggestBox**](../controls-and-patterns/auto-suggest-box.md)
- [**MediaTransportControl**](../controls-and-patterns/media-playback.md)
- [**CommandBar**](../controls-and-patterns/app-bars.md)
- [**ComboBox**](../controls-and-patterns/lists.md)

이 그림은 여러 가지 컨트롤에 대한 표시 영향을 보여줍니다.

![표시 예제](images/RevealExamples_Collage.png)

## <a name="enabling-reveal-on-other-common-controls"></a>다른 일반 컨트롤에서 표시를 사용하도록 설정

표시를 적용해야 하는 시나리오가 있는 경우(이러한 컨트롤은 주 콘텐츠이며/이거나 목록 또는 컬렉션 방향에 사용됨) 이러한 상황에 표시를 사용할 수 있는 옵트인(opt in) 리소스 스타일이 제공됩니다.

이러한 컨트롤은 일반적으로 응용 프로그램 주요 포커스 지점의 도우미 컨트롤인 작은 컨트롤이기 때문에 기본적으로 표시가 없지만, 똑같은 앱은 하나도 없으며 이러한 컨트롤이 앱에서 가장 많이 사용되는 경우를 대비하여 일부 스타일이 제공되었습니다.

| 컨트롤 이름   | 리소스 이름 |
|----------|:-------------:|
| 단추 |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| SemanticZoom | SemanticZoomRevealStyle |

이러한 스타일을 적용하려면 스타일 속성을 다음과 같이 업데이트합니다.

```XAML
<Button Content="Button Content" Style="{StaticResource ButtonRevealStyle}"/>
```

## <a name="enabling-reveal-on-custom-controls"></a>사용자 지정 컨트롤에서 표시를 사용하도록 설정

귀하의 사용자 지정 컨트롤이 표시를 사용할지 여부를 결정할 때 고려할 일반적인 규칙은 앱에서 수행하고자 하는 중요한 기능 또는 동작과 모두 관련된 대화형 요소의 그룹이 있어야 한다는 것입니다.

예를 들어 NavigationView의 항목은 페이지 탐색과 관련이 있습니다. CommandBar의 버튼은 메뉴 작업이나 페이지 기능 작업과 관련됩니다. 아래에 있는 MediaTransportControl의 버튼은 모두 재생 중인 미디어와 관련이 있습니다.

표시되는 컨트롤은 서로 관련되지 않아도 되며 고밀도 영역에 있고 보다 큰 목적으로 사용해야 합니다.

사용자 지정 컨트롤 또는 다시 템플릿으로 지정된 컨트롤에서 표시를 사용하려면 해당 컨트롤의 시각적 상태에서 해당 컨트롤의 스타일로 이동하고 루트 그리드에서 표시를 지정합니다.

```xaml
<VisualState x:Name="PointerOver">
    <VisualState.Setters>
        <Setter Target="RootGrid.(RevealBrush.State)" Value="PointerOver" />
        <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}" />
        <Setter Target="ContentPresenter.BorderBrush" Value="Transparent"/>
        <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}" />
    </VisualState.Setters>
</VisualState>
```

표시가 완벽하게 작동하려면 시각적 상태의 브러시와 setter 모두 필요합니다. 컨트롤의 브러시를 표시 브러시 리소스 중 하나에 설정하기만 하는 것은 해당 컨트롤의 표시를 활성화하지 않습니다. 반대로 표시 브러시가 되는 값 없이 대상 또는 설정만 있어도 표시를 활성화하지 않습니다.

UI를 사용자 지정하는 데 사용할 수 있는 시스템 표시 브러시 세트를 만들었습니다. 예를 들어 **ButtonRevealBackground** 브러시를 사용하여 사용자 지정 버튼 배경을 만들거나 사용자 지정 목록에 **ListViewItemRevealBackground** 브러시를 사용할 수 있습니다.

(XAMl에서 리소스가 작동하는 방법에 대해 알아보려면 [Xaml 리소스 사전](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md) 문서를 확인하세요.)

### <a name="reveal-on-listview-controls-with-nested-buttons"></a>중첩된 단추로 ListView 컨트롤에 표시

[ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)가 있고 버튼 또는 [ListViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem) 요소 내에 중첩된 호출할 수 있는 콘텐츠가 있는 경우 중첩된 항목에 대해 표시를 사용해야 합니다.

버튼 또는 ListViewItem에서 버튼과 같은 컨트롤의 경우 컨트롤의 [스타일](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_Style) 속성을 **ButtonRevealStyle** 정적 리소스로 설정하기만 하면 됩니다. 

![중첩된 표시](images/NestedListContent.png)

이 예에서는 ListViewItem 내 여러 버튼에 표시를 활성화합니다. 

```XAML
<ListViewItem>
    <StackPanel Orientation="Horizontal">
        <TextBlock Margin="5">Test Text: lorem ipsum.</TextBlock>
        <StackPanel Orientation="Horizontal">
            <Button Content="&#xE71B;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
            <Button Content="&#xE728;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
            <Button Content="&#xE74D;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
         </StackPanel>
    </StackPanel>
</ListViewItem>
```

### <a name="listviewitempresenter-with-reveal"></a>표시가 있는 ListViewItemPresenter

XAML에서 목록은 약간 특별하며 표시의 경우 ListViewItemPresenter 내부에서만 표시에 대한 Visual State Manager를 정의해야 합니다.

```XAML
<ListViewItemPresenter>
<!-- ContentTransitions, SelectedForeground, etc. properties -->
RevealBackground="{ThemeResource ListViewItemRevealBackground}"
RevealBorderThickness="{ThemeResource ListViewItemRevealBorderThemeThickness}"
RevealBorderBrush="{ThemeResource ListViewItemRevealBorderBrush}">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
        <VisualState x:Name="Normal" />
        <VisualState x:Name="Selected" />
        <VisualState x:Name="PointerOver">
            <VisualState.Setters>
                <Setter Target="Root.(RevealBrush.State)" Value="PointerOver" />
            </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PointerOverSelected">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="PointerOver" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PointerOverPressed">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PressedSelected">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            </VisualStateGroup>
                <VisualStateGroup x:Name="EnabledGroup">
                    <VisualState x:Name="Enabled" />
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                             <Setter Target="Root.RevealBorderThickness" Value="0"/>
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</ListViewItemPresenter>
```

즉, Visual State setter 특정 표시가 있는 ListViewItemPresenter 내 속성 컬렉션의 끝에 추가함을 의미합니다.

### <a name="full-template-example"></a>전체 템플릿 예제

다음은 표시 버튼이 나타날 전체 템플릿입니다.

```XAML
<Style TargetType="Button" x:Key="ButtonStyle1">
    <Setter Property="Background" Value="{ThemeResource ButtonRevealBackground}" />
    <Setter Property="Foreground" Value="{ThemeResource ButtonForeground}" />
    <Setter Property="BorderBrush" Value="{ThemeResource ButtonRevealBorderBrush}" />
    <Setter Property="BorderThickness" Value="2" />
    <Setter Property="Padding" Value="8,4,8,4" />
    <Setter Property="HorizontalAlignment" Value="Left" />
    <Setter Property="VerticalAlignment" Value="Center" />
    <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}" />
    <Setter Property="FontWeight" Value="Normal" />
    <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}" />
    <Setter Property="UseSystemFocusVisuals" Value="True" />
    <Setter Property="FocusVisualMargin" Value="-3" />
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="Button">
                <Grid x:Name="RootGrid" Background="{TemplateBinding Background}">

                    <VisualStateManager.VisualStateGroups>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal">

                                <Storyboard>
                                    <PointerUpThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="PointerOver">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.(RevealBrush.State)" Value="PointerOver" />
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="Transparent"/>
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}" />
                                </VisualState.Setters>

                                <Storyboard>
                                    <PointerUpThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="Pressed">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.(RevealBrush.State)" Value="Pressed" />
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPressed}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBackgroundPressed}" />
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPressed}" />
                                </VisualState.Setters>

                                <Storyboard>
                                    <PointerDownThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="Disabled">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundDisabled}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushDisabled}" />
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundDisabled}" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>

              </VisualStateManager.VisualStateGroups>
                    <ContentPresenter x:Name="ContentPresenter"
                    BorderBrush="{TemplateBinding BorderBrush}"
                    BorderThickness="{TemplateBinding BorderThickness}"
                    Content="{TemplateBinding Content}"
                    ContentTransitions="{TemplateBinding ContentTransitions}"
                    ContentTemplate="{TemplateBinding ContentTemplate}"
                    Padding="{TemplateBinding Padding}"
                    HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}"
                    VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"
                    AutomationProperties.AccessibilityView="Raw" />
                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항
- 사용자가 조치를 취할 수 있는 요소에 표시 사용(단추, 선택)
- 기본적으로 시각적 구분 기호가 없는 대화형 요소를 그룹화하는 데 표시를 사용하지 않음(목록, 명령 모음)
- 대화형 요소의 밀도가 높은 영역에 표시 사용
- 정적 콘텐츠 에서 표시 사용 안 함(배경, 텍스트)
- 격리된 일회성 상황에는 표시를 사용하지 말 것
- 매우 큰 항목에 표시 사용 안 함(500epx 이상)
- 사용자에게 제공해야 하는 메시지로부터 사용자의 주의를 뺏을 수 있으므로 보안 결정에는 표시를 사용하지 말 것

## <a name="how-we-designed-reveal"></a>표시 설계 방식

표시에는 두 개의 주요 시각적 구성 요소가 있는데, 하나는 **가리켜서 표시** 동작이고 다른 하나는 **테두리 표시** 동작입니다.

![계층 표시](images/RevealLayers.png)

가리켜서 표시는 포인터 또는 포커스 입력을 통해 가리키는 콘텐츠에 직접 연결되며, 가리킨 항목 또는 포커스가 설정된 항목 주변에 은은한 후광 형상을 적용합니다.

테두리 표시는 포커스가 설정된 항목과 그 주변 항목에 적용됩니다. 이를 통해 주변 개체가 현재 포커스가 설정된 작업과 비슷한 작업을 수행할 수 있음을 보여 줍니다.

표시 제작법 분류는 다음과 같습니다.

- 테두리 표시는 모든 콘텐츠 위에 있지만 지정된 가장자리 위에 있습니다.
- 텍스트와 콘텐츠는 테두리 표시 바로 아래에 표시됩니다.
- 가리켜서 표시는 콘텐츠 및 텍스트 아래에 있습니다.
- 백플레이트(가리켜서 표시를 켜고 사용하도록 설정)
- 배경(컨트롤의 배경)

<!--
<div class=”microsoft-internal-note”>
To create your own Reveal lighting effect for static comps or prototype purposes, see the full [uni design guidance](http://uni/DesignDepot.FrontEnd/#/ProductNav/3020/1/dv/?t=Resources%7CToolkit%7CReveal&f=Neon) for this effect in illustrator.
</div>
-->  

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서

- [RevealBrush 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [아크릴](acrylic.md)
- [컴퍼지션 효과](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [UWP용 Fluent 디자인](../fluent-design-system/index.md)
- [시스템의 과학: Fluent 디자인 및 깊이](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [시스템의 과학: Fluent 디자인 및 빛](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
