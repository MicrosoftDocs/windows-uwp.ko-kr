---
Description: XAML 프레임워크에서 컨트롤 템플릿을 만들어 컨트롤의 시각적 구조와 동작을 사용자 지정할 수 있습니다.
MS-HAID: dev\_ctrl\_layout\_txt.control\_templates
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 컨트롤 템플릿
ms.assetid: 6E642626-A1D6-482F-9F7E-DBBA7A071DAD
label: Control templates
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d8968104ec7b28a6b4a59eb6a83c422807282a7c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362988"
---
# <a name="control-templates"></a>컨트롤 템플릿

XAML 프레임워크에서 컨트롤 템플릿을 만들어 컨트롤의 시각적 구조와 동작을 사용자 지정할 수 있습니다. 컨트롤에는 [**Background**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.background), [**Foreground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.foreground) 및 [**FontFamily**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.fontfamily) 등 다양한 속성이 있으며, 이러한 속성을 설정하여 컨트롤 모양의 다양한 측면을 지정할 수 있습니다. 하지만 이러한 속성을 설정하여 수행할 수 있는 변경은 제한되어 있습니다. [  **ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 클래스를 사용하는 템플릿을 만들어 추가로 사용자 지정할 수 있습니다. 여기서는 [**CheckBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 컨트롤의 모양을 사용자 지정하기 위해 **ControlTemplate**을 만드는 방법을 보여 줍니다.

> **중요 API**: [**ControlTemplate 클래스**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)하십시오 [ **Control.Template 속성**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.template)

## <a name="custom-control-template-example"></a>사용자 지정 컨트롤 템플릿 예

기본적으로 [**CheckBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 컨트롤은 콘텐츠(문자열 또는 **CheckBox** 옆의 개체)를 선택 상자의 오른쪽에 놓습니다. 확인 표시는 사용자가 **CheckBox**를 선택했다는 것을 의미합니다. 이러한 특성은 **CheckBox**의 시각적 동작 및 시각적 구조를 나타냅니다.

다음은 `Unchecked`, `Checked` 및 `Indeterminate` 상태로 표시된 기본 [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox)을 사용하는 [**확인란**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)입니다.

![기본 CheckBox 템플릿](images/templates-checkbox-states-default.png)

[  **CheckBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)용 [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox)을 만들어 이러한 특성을 변경할 수 있습니다. 예를 들어 확인란의 콘텐츠를 선택 상자 아래에 놓고 **X**를 사용하여 사용자가 확인란을 선택했음을 표시하려고 합니다. **CheckBox**의 **ControlTemplate**에서 이러한 특성을 지정할 수 있습니다.

컨트롤이 있는 사용자 지정 템플릿을 사용하려면 [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)을 컨트롤의 [**Template**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.template) 속성에 할당합니다. 다음은 `CheckBoxTemplate1`이라는 **ControlTemplate**을 사용한 [**CheckBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox)입니다. 다음 섹션에서는 **ControlTemplate**용 XAML(Extensible Application Markup Language)을 보여 줍니다.

```XAML
<CheckBox Content="CheckBox" Template="{StaticResource CheckBoxTemplate1}" IsThreeState="True" Margin="20"/>
```

다음은 템플릿을 적용한 후 이 [**CheckBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox)가 `Unchecked`, `Checked` 및 `Indeterminate` 상태로 나타나는 모양입니다.

![CheckBox 템플릿 사용자 지정](images/templates-checkbox-states.png)

## <a name="specify-the-visual-structure-of-a-control"></a>컨트롤의 시각적 구조 지정

[  **ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)을 만들 때 [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 개체를 조합하여 단일 컨트롤을 구축합니다. **ControlTemplate**은 루트 요소로 오직 하나의 **FrameworkElement**만 사용해야 합니다. 루트 요소는 일반적으로 다른 **FrameworkElement** 개체를 포함합니다. 개체의 조합은 컨트롤의 시각적 구조를 구성합니다.

다음 XAML은 컨트롤의 콘텐츠가 선택 상자 아래에 있도록 지정하는 [**CheckBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)용 [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox)을 만듭니다. 루트 요소는 [**Border**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border)입니다. 이 예에서는 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)를 지정하여 사용자가 **CheckBox**를 선택했음을 나타내는 **X**를 만들고 확정되지 않은 상태를 나타내는 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)를 만듭니다. [  **Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)가 **Path**와 **Ellipse** 양쪽 모두에서 0으로 설정되어 있으므로 기본적으로 어느 쪽에도 나타나지 않습니다.

[TemplateBinding](../../xaml-platform/templatebinding-markup-extension.md)은 컨트롤 템플릿의 속성 값을 템플릿 기반 컨트롤에서 노출되는 몇몇 다른 속성 값에 연결하는 특별 바인딩입니다. TemplateBinding은 XAML의 ControlTemplate 정의 내에서만 사용할 수 있습니다. 자세한 내용은 [TemplateBinding 태그 확장](../../xaml-platform/templatebinding-markup-extension.md)을 참조하세요.

> [!NOTE]
> Windows 10 버전 1809부터 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk))를 사용할 수 있습니다 [ **X:bind** ](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 사용 하는 위치에서 태그 확장 [TemplateBinding](../../xaml-platform/templatebinding-markup-extension.md) . 자세한 내용은 [TemplateBinding 태그 확장](../../xaml-platform/templatebinding-markup-extension.md)을 참조하세요.

```XAML
<ControlTemplate x:Key="CheckBoxTemplate1" TargetType="CheckBox">
    <Border BorderBrush="{TemplateBinding BorderBrush}"
            BorderThickness="{TemplateBinding BorderThickness}"
            Background="{TemplateBinding Background}">
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="25"/>
            </Grid.RowDefinitions>
            <Rectangle x:Name="NormalRectangle" Fill="Transparent" Height="20" Width="20"
                       Stroke="{ThemeResource SystemControlForegroundBaseMediumHighBrush}"
                       StrokeThickness="{ThemeResource CheckBoxBorderThemeThickness}"
                       UseLayoutRounding="False"/>
            <!-- Create an X to indicate that the CheckBox is selected. -->
            <Path x:Name="CheckGlyph"
                  Data="M103,240 L111,240 119,248 127,240 135,240 123,252 135,264 127,264 119,257 111,264 103,264 114,252 z"
                  Fill="{ThemeResource CheckBoxForegroundThemeBrush}"
                  FlowDirection="LeftToRight"
                  Height="14" Width="16" Opacity="0" Stretch="Fill"/>
            <Ellipse x:Name="IndeterminateGlyph"
                     Fill="{ThemeResource CheckBoxForegroundThemeBrush}"
                     Height="8" Width="8" Opacity="0" UseLayoutRounding="False" />
            <ContentPresenter x:Name="ContentPresenter"
                              ContentTemplate="{TemplateBinding ContentTemplate}"
                              Content="{TemplateBinding Content}"
                              Margin="{TemplateBinding Padding}" Grid.Row="1"
                              HorizontalAlignment="Center"
                              VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
        </Grid>
    </Border>
</ControlTemplate>
```

## <a name="specify-the-visual-behavior-of-a-control"></a>컨트롤의 시각적 동작 지정

시각적 동작은 특정 상태의 컨트롤 모양을 지정합니다. [  **CheckBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 컨트롤에는 `Checked`, `Unchecked`, `Indeterminate`의 3가지 표시 상태가 있습니다. [  **IsChecked**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked) 속성의 값은 **CheckBox**의 상태를 결정하고 그 상태는 확인란 상자에 나타나는 모양을 결정합니다.

아래 표에 [**IsChecked**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked)의 가능한 값, [**CheckBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox)의 해당 상태 및 **CheckBox**의 모양이 나와 있습니다.

|                     |                    |                         |
|---------------------|--------------------|-------------------------|
| **IsChecked** 값 | **CheckBox** 상태 | **CheckBox** 모양 |
| **true**            | `Checked`          | "X" 포함        |
| **false**           | `Unchecked`        | 비어 있음                  |
| **null**            | `Indeterminate`    | 원을 포함합니다.      |


[  **VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) 개체를 사용하여 특정 상태일 때 나타나는 컨트롤의 모양을 지정합니다. **VisualState**에는 [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter)에 있는 요소의 모양을 변경하는 [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.BeginStoryboard) 또는 [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)가 있습니다. 컨트롤이 [**VisualState.Name**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstate.name) 속성이 지정한 상태가 되면 **Setter** 또는 [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)의 속성 변경 내용이 적용됩니다. 컨트롤이 상태에서 나가면 변경 내용이 제거됩니다. **VisualState** 개체를 [**VisualStateGroup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualStateGroup) 개체에 추가합니다. **VisualStateGroup** 개체를 **ControlTemplate**의 루트 [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement)에 설정한 [**VisualStateManager.VisualStateGroups**](https://docs.microsoft.com/dotnet/api/system.windows.visualstatemanager?view=netframework-4.8) 연결된 속성에 추가합니다.

이 XAML은 `Checked`, `Unchecked` 및 `Indeterminate` 상태의 [**VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) 개체를 보여 줍니다. 이 예제에서는 [**Border**](https://docs.microsoft.com/dotnet/api/system.windows.visualstatemanager?view=netframework-4.8)에 [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border)의 루트 요소인 [**VisualStateManager.VisualStateGroups**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 연결된 속성을 설정합니다. `Checked` **VisualState** 지정 된 [ **불투명도** ](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 의 [ **경로** ](/uwp/api/Windows.UI.Xaml.Shapes.Path) 라는 `CheckGlyph` (이전 예제에 표시)는 1입니다. `Indeterminate` **VisualState** 지정 합니다 **불투명도** 의 [ **타원** ](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 라는 `IndeterminateGlyph` 는 1입니다. 합니다 `Unchecked` **VisualState** 되지 [ **Setter** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter) 하거나 [ **스토리 보드**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)이므로 합니다 [ **CheckBox** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 기본 모양에 반환 합니다.

```XAML
<ControlTemplate x:Key="CheckBoxTemplate1" TargetType="CheckBox">
    <Border BorderBrush="{TemplateBinding BorderBrush}"
            BorderThickness="{TemplateBinding BorderThickness}"
            Background="{TemplateBinding Background}">

        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CheckStates">
                <VisualState x:Name="Checked">
                    <VisualState.Setters>
                        <Setter Target="CheckGlyph.Opacity" Value="1"/>
                    </VisualState.Setters>
                    <!-- This Storyboard is equivalent to the Setter. -->
                    <!--<Storyboard>
                        <DoubleAnimation Duration="0" To="1"
                         Storyboard.TargetName="CheckGlyph" Storyboard.TargetProperty="Opacity"/>
                    </Storyboard>-->
                </VisualState>
                <VisualState x:Name="Unchecked"/>
                <VisualState x:Name="Indeterminate">
                    <VisualState.Setters>
                        <Setter Target="IndeterminateGlyph.Opacity" Value="1"/>
                    </VisualState.Setters>
                    <!-- This Storyboard is equivalent to the Setter. -->
                    <!--<Storyboard>
                        <DoubleAnimation Duration="0" To="1"
                         Storyboard.TargetName="IndeterminateGlyph" Storyboard.TargetProperty="Opacity"/>
                    </Storyboard>-->
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="25"/>
            </Grid.RowDefinitions>
            <Rectangle x:Name="NormalRectangle" Fill="Transparent" Height="20" Width="20"
                       Stroke="{ThemeResource SystemControlForegroundBaseMediumHighBrush}"
                       StrokeThickness="{ThemeResource CheckBoxBorderThemeThickness}"
                       UseLayoutRounding="False"/>
            <!-- Create an X to indicate that the CheckBox is selected. -->
            <Path x:Name="CheckGlyph"
                  Data="M103,240 L111,240 119,248 127,240 135,240 123,252 135,264 127,264 119,257 111,264 103,264 114,252 z"
                  Fill="{ThemeResource CheckBoxForegroundThemeBrush}"
                  FlowDirection="LeftToRight"
                  Height="14" Width="16" Opacity="0" Stretch="Fill"/>
            <Ellipse x:Name="IndeterminateGlyph"
                     Fill="{ThemeResource CheckBoxForegroundThemeBrush}"
                     Height="8" Width="8" Opacity="0" UseLayoutRounding="False" />
            <ContentPresenter x:Name="ContentPresenter"
                              ContentTemplate="{TemplateBinding ContentTemplate}"
                              Content="{TemplateBinding Content}"
                              Margin="{TemplateBinding Padding}" Grid.Row="1"
                              HorizontalAlignment="Center"
                              VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
        </Grid>
    </Border>
</ControlTemplate>
```

[  **VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) 개체가 작동하는 방식을 더 잘 이해하려면, [**CheckBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox)가 `Unchecked` 상태에서 `Checked` 상태로 변할 때, `Indeterminate` 상태로 변한 후 `Unchecked` 상태로 다시 돌아올 때 어떤 현상이 일어나는지 살펴보세요. 다음은 전환에 대한 내용입니다.

|                                      |                                                                                                                                                                                                                                                                                                                                                |                                                   |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| 상태 전환                     | 발생하는 동작                                                                                                                                                                                                                                                                                                                                   | 전환이 완료된 후 CheckBox 모양 |
| `Unchecked`에서 `Checked`로       | [ **Setter** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter) 값을 `Checked` [ **VisualState** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) 적용 된 하므로 [ **불투명도**  ](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 의 `CheckGlyph` 1입니다.                                                                                                                                                         | X가 표시됩니다.                                |
| `Checked`에서 `Indeterminate`로   | [ **Setter** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter) 값을 `Indeterminate` [ **VisualState** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) 적용 된 하므로 [ **불투명도**  ](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 의 `IndeterminateGlyph` 1입니다. **Setter** 값을 `Checked` **VisualState** 제거 되 하므로 [ **불투명도** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.brush.opacity) 의 `CheckGlyph` 은 0입니다. | 원이 표시됩니다.                            |
| `Indeterminate`에서 `Unchecked`로 | [ **Setter** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter) 의 값을 `Indeterminate` [ **VisualState** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) 제거 되 면 하므로 [ **불투명도**  ](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 의 `IndeterminateGlyph` 은 0입니다.                                                                                                                                           | 아무것도 표시되지 않습니다.                             |

 
컨트롤의 시각적 상태를 만드는 방법 및 특히 [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) 클래스 및 애니메이션 형식을 사용하는 방법에 대한 자세한 내용은 [시각적 상태에 대한 스토리보드 애니메이션](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10))을 참조하세요.

## <a name="use-tools-to-work-with-themes-easily"></a>테마 작업을 쉽게 할 수 있는 도구 사용

테마를 컨트롤에 적용하는 가장 빠른 방법은 Microsoft Visual Studio **문서 개요**에서 컨트롤을 마우스 오른쪽 단추로 클릭하고 **테마 편집** 또는 **스타일 편집**을 선택하는 것입니다(마우스 오른쪽 단추로 클릭하는 컨트롤에 따라 다름). 그런 다음 **리소스 적용**을 선택하여 기존 테마를 적용하거나 **빈 테마 만들기**를 선택하여 새로운 테마를 정의할 수 있습니다.

## <a name="controls-and-accessibility"></a>컨트롤 및 접근성

컨트롤에 대한 새 템플릿을 만들 때 컨트롤의 동작 및 시각적 모양을 변경하는 것 외에 컨트롤이 접근성 프레임워크에 표시되는 방법도 변경할 수 있습니다. UWP(유니버설 Windows 플랫폼)는 접근성을 위한 Microsoft UI 자동화 프레임워크를 지원합니다. 모든 기본 컨트롤과 해당 템플릿은 컨트롤의 용도 및 기능에 적합한 일반 UI 자동화 컨트롤 유형 및 패턴을 지원합니다. 이러한 컨트롤 유형 및 패턴은 보조 기술 등의 UI 자동화 클라이언트에 의해 해석되며 이를 통해 액세스 가능한 더 큰 앱 UI의 일부로 컨트롤에 액세스할 수 있습니다.

기본 컨트롤 논리를 구분하고 또한 UI 자동화의 일부 아키텍처 요구 사항을 충족시키기 위해 컨트롤 클래스에는 별도의 클래스인 자동화 피어에 대한 접근성 지원이 포함되어 있습니다. 보조 기술이 단추의 작업을 호출하도록 하는 등의 기능이 가능하도록 자동화 피어는 일부 명명된 부분이 템플릿에 있을 것으로 기대하므로 때때로 컨트롤 템플릿과 상호 작용합니다.

완전히 새로운 사용자 지정 컨트롤을 만들 때 경우에 따라 이 컨트롤과 함께 새 자동화 피어를 만들 수도 있습니다. 자세한 내용은 [사용자 지정 자동화 피어](../accessibility/custom-automation-peers.md)를 참조하세요.

## <a name="learn-more-about-a-controls-default-template"></a>컨트롤의 기본 서식 파일에 대한 자세한 정보

XAML 컨트롤의 스타일과 템플릿을 문서화하는 항목은 앞에서 설명한 **테마 편집** 또는 **스타일 편집** 기술을 사용한 경우 표시되는 것과 동일한 시작 XAML의 일부를 보여 줍니다. 각 항목은 시각적 상태의 이름, 사용된 테마 리소스 및 템플릿이 포함된 스타일의 전체 XAML을 나열합니다. 템플릿 수정을 이미 시작했으며 원본 템플릿의 모양을 확인하거나 새 템플릿에 명명된 필수 시각적 상태가 모두 있는지 확인하려는 경우 이러한 항목은 유용한 지침이 될 수 있습니다.

## <a name="theme-resources-in-control-templates"></a>컨트롤 템플릿의 테마 리소스

XAML 예제의 일부 속성에서 [{ThemeResource} 태그 확장](../../xaml-platform/themeresource-markup-extension.md)을 사용하는 리소스 참조를 봤을 것입니다. 이 기술은 단일 컨트롤 템플릿이 현재 활성화된 테마에 따라 값이 달라질 수 있는 리소스를 사용할 수 있도록 합니다. 테마의 주요 목적은 사용자가 시스템 전체에 어둡거나 밝거나 고대비 테마를 적용할 것인지 선택할 수 있게 하는 것이므로 이 기술은 특히 브러시와 색에 중요합니다. XAML 리소스 시스템을 사용하는 앱은 앱 UI의 테마 선택 항목이 사용자 시스템의 테마 선택을 반영하도록 해당 테마에 적합한 리소스 집합을 사용할 수 있습니다.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

* [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)
* [사용자 지정 텍스트 편집 컨트롤 샘플](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CustomEditControl)
