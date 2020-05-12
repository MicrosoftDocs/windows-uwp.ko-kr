---
description: 표시는 앱의 대화형 요소에 깊이와 포커스를 더해주는 조명 효과입니다.
title: 강조 표시
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 26e756b52d4faf18eff2fc684c7db94bca058642
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82971078"
---
# <a name="reveal-highlight"></a>강조 표시

![영웅 이미지](images/header-reveal-highlight.svg)

강조 표시는 사용자가 포인터 명령 모음과 같은 대화형 요소로 움직였을 때 이를 강조 표시하는 조명 효과입니다. 

> **중요 API**: [RevealBrush 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush), [RevealBackgroundBrush 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush), [RevealBorderBrush 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush), [RevealBrushHelper 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper), [VisualState 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState)

## <a name="how-it-works"></a>작동 방식
강조 표시는 이 그림에 나타난 것처럼 포인터가 근처에 있을 때 요소의 컨테이너를 표시하여 대화형 요소를 더욱 잘 보이게 합니다.

![표시 화면 효과](images/Nav_Reveal_Animation.gif)

표시는 개체 주변의 숨겨진 테두리를 노출하여 사용자가 조작하는 공간을 파악하고 사용 가능한 작업을 이해하는 데 도움을 줍니다. 목록 컨트롤과 단추 그룹화에 특히 중요합니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/Reveal">앱을 열고 표시의 기능을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>동영상 요약

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev013/player]

## <a name="how-to-use-it"></a>사용 방법

일부 컨트롤에 대해 자동으로 표시됩니다. 다른 컨트롤의 경우 이 문서의 [다른 컨트롤에서 표시 사용](#enabling-reveal-on-other-controls) 및 [사용자 지정 컨트롤에서 표시 사용](#enabling-reveal-on-custom-controls) 섹션에 설명된 것처럼 컨트롤에 특별한 스타일을 할당하여 표시를 사용할 수 있습니다.

## <a name="controls-that-automatically-use-reveal"></a>자동으로 표시를 사용하는 컨트롤

- [**ListView**](../controls-and-patterns/lists.md)
- [**GridView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**MediaTransportControl**](../controls-and-patterns/media-playback.md)
- [**CommandBar**](../controls-and-patterns/app-bars.md)

이 그림은 여러 가지 컨트롤의 강조 표시를 보여 줍니다.

![표시 예제](images/RevealExamples_Collage.png)


## <a name="enabling-reveal-on-other-controls"></a>다른 컨트롤에서 표시 사용

표시를 적용해야 하는 시나리오가 있는 경우(이러한 컨트롤이 주 콘텐츠이거나 목록 또는 컬렉션 방향에서 사용됨), 이러한 상황에 표시를 사용하도록 설정할 수 있는 옵트인 리소스 스타일이 제공됩니다.

이 컨트롤은 일반적으로 애플리케이션 주요 포커스 지점의 도우미 컨트롤인 작은 컨트롤이기 때문에 기본적으로 표시가 없지만, 똑같은 앱은 하나도 없으며 이 컨트롤이 앱에서 가장 많이 사용되는 경우를 대비하여 일부 스타일이 제공되었습니다.

| 컨트롤 이름   | 리소스 이름 |
|----------|:-------------:|
| 단추 |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| AppBarToggleButton | AppBarToggleButtonRevealStyle |
| GridViewItem(콘텐츠 위에 표시) | GridViewItemRevealBackgroundShowsAboveContentStyle |

이 스타일을 적용하려면 컨트롤의 [Style](/uwp/api/Windows.UI.Xaml.Style) 속성을 설정하면 됩니다.

```xaml
<Button Content="Button Content" Style="{ThemeResource ButtonRevealStyle}"/>
```

### <a name="reveal-in-themes"></a>테마에 표시

표시는 컨트롤, 앱 또는 사용자 설정의 요청된 테마에 따라 약간씩 내용이 달라집니다. 어두운 테마에서 표시의 테두리 및 가리키기 조명은 흰색이지만 밝은 테마에서는 어두운 테두리만 밝은 회색으로 변합니다.

![어두운 표시 및 밝은 표시](images/Dark_vs_LightReveal.png)

밝은 테마에서 흰 테두리를 사용하려면 컨트롤에 요청된 테마를 [어둡게]로 설정하면 됩니다.

```xaml
<Grid RequestedTheme="Dark">
    <Button Content="Button" Click="Button_Click" Style="{ThemeResource ButtonRevealStyle}"/>
</Grid>
```

또는 RevealBorderBrush의 TargetTheme을 [어둡게]로 변경합니다. 기억해 두세요. TargetTheme이 [어둡게]로 설정되면 표시는 흰색이 되지만 [밝게]로 설정되면 표시의 테두리는 회색이 됩니다.

```xaml
 <RevealBorderBrush x:Key="MyLightBorderBrush" TargetTheme="Dark" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}" />
```

## <a name="enabling-reveal-on-custom-controls"></a>사용자 지정 컨트롤에서 표시 사용

사용자 지정 컨트롤에 표시를 추가할 수 있습니다. 시작하기 전에 표시 효과의 작동 방식을 좀 더 자세히 알아두는 것이 도움이 됩니다. 표시는 **테두리 표시**와 **가리키기 표시**라는 두 개의 개별 효과로 구성됩니다.

- **테두리**는 포인터가 근처에 있을 때 대화형 요소의 테두리를 표시합니다. 이 효과는 주변 개체가 현재 포커스가 설정된 작업과 비슷한 작업을 수행할 수 있음을 보여 줍니다.
- **가리키기**는 가리킨 항목 또는 포커스가 설정된 항목 주변에 은은한 후광 형상을 적용하며, 클릭 시 애니메이션이 재생됩니다. 

![계층 표시](images/RevealLayers.png)

<!-- The Reveal recipe breakdown is:

- Border reveal will be on top of all content but on the designated edges
- Text and content will be displayed directly under Border Reveal
- Hover reveal will be beneath content and text
- The backplate (that turns on and enables Hover Reveal)
- The background (background of control) -->


이 효과는 다음 두 개의 브러시로 정의됩니다. 
* 테두리 표시는 **RevealBorderBrush**로 정의됩니다.
* 가리켜서 표시는 **RevealBackgroundBrush**로 정의됩니다.

```xaml
<RevealBorderBrush x:Key="MyRevealBorderBrush" TargetTheme="Light" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}"/>
<RevealBackgroundBrush x:Key="MyRevealBackgroundBrush" TargetTheme="Light" Color="{StaticResource SystemAccentColor}" FallbackColor="{StaticResource SystemAccentColor}" />
```
대부분의 경우 특정 컨트롤에 대해 자동으로 표시를 켜서 두 표시의 사용을 모두 처리합니다. 그러나 기타 컨트롤은 스타일을 적용하거나 직접 템플릿을 변경하여 사용하도록 설정해야 합니다.

### <a name="when-to-add-reveal"></a>표시를 추가하는 경우
사용자 지정 컨트롤에 표시를 추가할 수 있습니다. 하지만 추가하기 전에 컨트롤의 유형과 작동 방식을 고려해야 합니다. 
* 사용자 지정 컨트롤이 단일 대화형 요소이며 공간을 공유하는 유사한 컨트롤이 없는 경우(예: 메뉴에 있는 메뉴 항목), 사용자 지정 컨트롤에 표시가 필요하지 않을 가능성이 높습니다.  
* 관련 대화형 콘텐츠 또는 요소의 그룹화가 있는 경우 앱의 해당 영역에 표시가 필요할 가능성이 높습니다. 이 영역을 일반적으로 [명령](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/collection-commanding) 표면이라고 합니다.

예를 들어 단추 자체는 표시를 사용하면 안 되지만, 명령 표시줄에 있는 단추 세트는 표시를 사용해야 합니다.

<!-- For example, NavigationView's items are related to page navigation. CommandBar's buttons relate to menu actions or page feature actions. MediaTransportControl's buttons beneath all relate to the media being played. -->

### <a name="using-the-control-template-to-add-reveal"></a>컨트롤 템플릿을 사용하여 표시 추가 
사용자 지정 컨트롤 또는 템플릿 재작성 컨트롤에서 표시를 사용하려면 컨트롤의 컨트롤 템플릿을 수정합니다. 대부분 컨트롤 템플릿은 루트에 그리드가 있습니다. 해당 루트 그리드의 [VisualState](/uwp/api/windows.ui.xaml.visualstate)를 업데이트하여 표시를 사용합니다.

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

표시가 제대로 작동하려면 시각적 상태의 브러시와 setter가 모두 필요합니다. 컨트롤의 브러시를 표시 브러시 리소스 중 하나로 설정하는 것만으로는 해당 컨트롤에 표시를 사용할 수 없습니다. 반대로, 표시 브러시가 되는 값 없이 대상 또는 설정만 사용하는 경우에도 표시를 사용할 수 없습니다.

컨트롤 템플릿 수정 방법에 대한 자세한 내용은 [XAML 컨트롤 템플릿](../controls-and-patterns/control-templates.md) 문서를 참조하세요.

템플릿을 사용자 지정하는 데 사용할 수 있는 시스템 표시 브러시 세트를 만들었습니다. 예를 들어 **ButtonRevealBackground** 브러시를 사용하여 사용자 지정 단추 배경을 만들거나, 사용자 지정 목록에 **ListViewItemRevealBackground** 브러시를 사용할 수 있습니다. XAML에서 리소스 작동 방식을 알아보려면 [XAML 리소스 사전](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md) 문서를 확인하세요.

### <a name="full-template-example"></a>전체 템플릿 예제

다음은 표시 단추의 모양을 위한 전체 템플릿입니다.

```xaml
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

### <a name="fine-tuning-the-reveal-effect-on-a-custom-control"></a>사용자 지정 컨트롤의 표시 효과 미세 조정 

사용자 지정 컨트롤, 템플릿 재작성 컨트롤 또는 사용자 지정 명령 표면에 표시를 사용하는 경우 다음 팁을 활용하여 효과를 최적화할 수 있습니다.
 
* 특히 목록에서 높이 또는 너비가 맞지 않는 크기의 인접 항목: 테두리 접근 동작을 제거하고 가리킬 때만 테두리가 표시되도록 유지합니다.
* 사용 안 함 상태가 자주 설정/해제되는 명령 항목: 요소의 백플레이트 및 해당 테두리에 테두리 접근 브러시를 설정하여 상태를 강조합니다.
* 너무 가까워서 서로 닿아 있는 인접한 명령 요소: 두 요소 사이에 1px 여백을 추가합니다. 

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항
### <a name="do"></a>권장 사항:
- 사용자가 여러 조치를 취할 수 있는 요소에 표시 사용(CommandBars, 탐색 메뉴)
- 기본적으로 시각적 구분 기호가 없는 대화형 요소를 그룹화하는 데 표시 사용(목록, 리본)
- 대화형 요소의 밀도가 높은 영역에 표시 사용(명령 시나리오)
- 표시 항목 사이에 1px 여백 공간 배치

### <a name="dont"></a>안 함
- 정적 콘텐츠에 표시를 사용하지 않음(배경, 텍스트)
- 팝업, 드롭다운 또는 플라이아웃에 표시를 사용하지 않음
- 격리된 일회성 상황에 표시를 사용하지 않음
- 매우 큰 항목에 표시를 사용하지 않음(500epx 초과)
- 사용자에게 제공해야 하는 메시지로부터 주의를 분산시킬 수 있으므로 보안 결정에 표시를 사용하지 않음


## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여줍니다.

## <a name="reveal-and-the-fluent-design-system"></a>표시 및 흐름 디자인 시스템

 흐름 디자인 시스템을 사용하면 조명, 깊이, 움직임, 재질, 배율이 통합된 선명한 현대식 UI를 만들 수 있습니다. 표시는 앱에 조명을 추가하는 흐름 디자인 시스템의 구성 요소입니다. 자세한 내용은 [Fluent Design 개요](/windows/apps/fluent-design-system)를 참조하세요.

## <a name="related-articles"></a>관련된 문서

- [RevealBrush 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [아크릴](acrylic.md)
- [컴퍼지션 효과](https://docs.microsoft.com/windows/uwp/graphics/composition-effects)
- [UWP용 흐름 디자인](/windows/apps/fluent-design-system)
- [시스템의 과학: 흐름 디자인 및 깊이](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [시스템의 과학: 흐름 디자인 및 조명](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
