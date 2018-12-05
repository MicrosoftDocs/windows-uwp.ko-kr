---
description: 스타일을 사용하면 컨트롤 속성을 설정하고 이 설정을 재사용하여 여러 컨트롤에서 일관된 모양을 얻을 수 있습니다.
MS-HAID: dev\_ctrl\_layout\_txt.styling\_controls
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: XAML 스타일
ms.assetid: AB469A46-FAF5-42D0-9340-948D0EDF4150
label: XAML styles
template: detail.hbs
ms.localizationpriority: medium
ms.openlocfilehash: d9f77d92e3c80e4a0d4e0808289f032b4a1125a5
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8692335"
---
# <a name="xaml-styles"></a>XAML 스타일





XAML 프레임워크를 사용하여 다양한 방법으로 앱 모양을 사용자 지정할 수 있습니다. 스타일을 사용하면 컨트롤 속성을 설정하고 이 설정을 재사용하여 여러 컨트롤에서 일관된 모양을 얻을 수 있습니다.

## <a name="style-basics"></a>스타일 기본

스타일을 사용하여 시각적 속성 설정을 재사용 가능한 리소스로 추출합니다. 다음은 [BorderBrush](https://msdn.microsoft.com/library/windows/apps/br209397), [BorderThickness](https://msdn.microsoft.com/library/windows/apps/br209399) 및 [Foreground](https://msdn.microsoft.com/library/windows/apps/br209414) 속성을 설정하는 스타일을 사용한 3개의 단추를 보여줍니다. 스타일을 적용하면 각 컨트롤에서 이러한 속성을 별도로 설정하지 않고도 컨트롤이 동일하게 표시되도록 만들 수 있습니다.

![스타일이 적용된 단추](images/styles-rainbow-buttons.png)

컨트롤에 XAML로 스타일을 인라인으로 정의하거나 재사용 가능한 리소스로 정의할 수 있습니다. 개별 페이지의 XAML 파일인 App.xaml 파일 또는 별도의 리소스 사전 XAML 파일에서 리소스를 정의합니다. 앱 간에 리소스 사전 XAML 파일을 공유할 수 있으며, 여러 리소스 사전을 하나의 앱으로 병합할 수 있습니다. 리소스가 정의되는 위치에 따라 리소스를 사용할 수 있는 범위가 결정됩니다. 페이지 수준별 리소스는 해당 리소스가 정의된 페이지에서만 사용할 수 있습니다. App.xaml과 페이지에 모두 동일한 키를 사용하여 리소스가 정의되어 있는 경우 페이지의 리소스는 App.xaml의 리소스보다 우선합니다. 리소스가 별도 리소스 사전 파일에서 정의된 경우 리소스 사전이 참조된 위치에 따라 범위가 결정됩니다.

[Style](https://msdn.microsoft.com/library/windows/apps/br208849) 정의에서는 [TargetType](https://msdn.microsoft.com/library/windows/apps/br208857) 특성과 하나 이상의 [Setter](https://msdn.microsoft.com/library/windows/apps/br208817) 요소 컬렉션이 필요합니다. **TargetType** 특성은 스타일을 적용하는 [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/br208706) 형식을 지정하는 문자열입니다. **TargetType** 값은 참조되는 어셈블리에서 사용할 수 있는 사용자 지정 형식 또는 Windows 런타임에서 정의되는 **FrameworkElement** 파생 형식을 지정해야 합니다. 컨트롤에 스타일을 적용하려고 하는데 컨트롤의 형식이 적용하려는 스타일의 **TargetType** 특성과 일치하지 않으면 예외가 발생합니다.

각 [Setter](https://msdn.microsoft.com/library/windows/apps/br208817) 요소에는 [Property](https://msdn.microsoft.com/library/windows/apps/br208836) 및 [Value](https://msdn.microsoft.com/library/windows/apps/br208838)가 필요합니다. 이러한 속성 설정은 설정이 적용되는 컨트롤 속성과 해당 속성에 설정할 값을 지정합니다. 특성 또는 속성 요소 구문으로 **Setter.Value**를 설정할 수 있습니다. 다음 XAML은 앞에 표시된 단추에 적용되는 스타일을 보여 줍니다. 이 XAML에서 처음 두 **Setter** 요소는 특성 구문을 사용하지만, 마지막 **Setter**는 [BorderBrush](https://msdn.microsoft.com/library/windows/apps/br209397) 속성에 대해 속성 요소 구문을 사용합니다. 이 예제에서는 [x:Key 특성](../../xaml-platform/x-key-attribute.md) 특성을 사용하지 않으므로 스타일이 단추에 암시적으로 적용됩니다. 스타일의 암시적 또는 명시적 적용은 다음 섹션에서 설명합니다.

```XAML
<Page.Resources>
    <Style TargetType="Button">
        <Setter Property="BorderThickness" Value="5" />
        <Setter Property="Foreground" Value="Black" />
        <Setter Property="BorderBrush" >
            <Setter.Value>
                <LinearGradientBrush StartPoint="0.5,0" EndPoint="0.5,1">
                    <GradientStop Color="Yellow" Offset="0.0" />
                    <GradientStop Color="Red" Offset="0.25" />
                    <GradientStop Color="Blue" Offset="0.75" />
                    <GradientStop Color="LimeGreen" Offset="1.0" />
                </LinearGradientBrush>
            </Setter.Value>
        </Setter>
    </Style>
</Page.Resources>

<StackPanel Orientation="Horizontal">
    <Button Content="Button"/>
    <Button Content="Button"/>
    <Button Content="Button"/>
</StackPanel>
```

## <a name="apply-an-implicit-or-explicit-style"></a>암시적 또는 명시적 스타일 적용

스타일을 리소스로 정의할 경우 다음 두 가지 방법으로 스타일을 컨트롤에 적용할 수 있습니다.

-   암시적 적용 - [Style](https://msdn.microsoft.com/library/windows/apps/br208857)에 대하여 [TargetType](https://msdn.microsoft.com/library/windows/apps/br208849)만 지정합니다.
-   명시적 적용 - [Style](https://msdn.microsoft.com/library/windows/apps/br208849)에 대해 [TargetType](https://msdn.microsoft.com/library/windows/apps/br208857) 및 [x:Key 특성](../../xaml-platform/x-key-attribute.md) 특성을 지정한 후 명시적 키를 사용하는 [{StaticResource} 태그 확장](https://msdn.microsoft.com/library/windows/apps/mt185588) 참조로 대상 컨트롤의 [Style](https://msdn.microsoft.com/library/windows/apps/br208743) 속성을 설정합니다.

스타일에 [x:Key 특성](../../xaml-platform/x-key-attribute.md)이 포함된 경우, 컨트롤의 [Style](https://msdn.microsoft.com/library/windows/apps/br208743) 속성을 키 입력 스타일로 설정하여 이 특성을 컨트롤에만 적용할 수 있습니다. 이와 달리, x:Key 특성이 없는 스타일은 이 경우 외에는 명시적 스타일 설정이 없는 대상 유형의 모든 컨트롤에 자동으로 적용됩니다.

다음은 암시적 및 명시적 스타일을 보여주는 두 개의 단추입니다.

![암시적 및 명시적 스타일의 단추](images/styles-buttons-implicit-explicit.png)

이 예제에서는 첫 번째 스타일에 [x:Key 특성](../../xaml-platform/x-key-attribute.md)이 있고 대상 유형은 [Button](https://msdn.microsoft.com/library/windows/apps/br209265)입니다. 첫 번째 단추의 [Style](https://msdn.microsoft.com/library/windows/apps/br208743) 속성이 이 키로 설정되므로 이 스타일이 명시적으로 적용됩니다. 두 번째 단추는 대상 유형이 **Button**이고 스타일에 x:Key 특성이 없으므로 두 번째 스타일이 암시적으로 적용됩니다.

```XAML
<Page.Resources>
    <Style x:Key="PurpleStyle" TargetType="Button">
        <Setter Property="FontFamily" Value="Segoe UI"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Foreground" Value="Purple"/>
    </Style>

    <Style TargetType="Button">
        <Setter Property="FontFamily" Value="Segoe UI"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="RenderTransform">
            <Setter.Value>
                <RotateTransform Angle="25"/>
            </Setter.Value>
        </Setter>
        <Setter Property="BorderBrush" Value="Green"/>
        <Setter Property="BorderThickness" Value="2"/>
        <Setter Property="Foreground" Value="Green"/>
    </Style>
</Page.Resources>

<Grid x:Name="LayoutRoot">
    <Button Content="Button" Style="{StaticResource PurpleStyle}"/>
    <Button Content="Button"/>
</Grid>
```

## <a name="use-based-on-styles"></a>파생 스타일 사용

스타일을 더 쉽게 유지 관리하고 스타일 재사용을 최적화하기 위해 다른 스타일에서 상속받는 스타일을 만들 수 있습니다. [BasedOn](https://msdn.microsoft.com/library/windows/apps/br208852) 속성을 사용하여 상속받은 스타일을 만들 수 있습니다. 다른 스타일에서 상속받은 스타일은 동일한 유형의 컨트롤 또는 기본 스타일이 대상으로 하는 유형에서 파생된 컨트롤을 대상으로 지정해야 합니다. 예를 들어, 기본 스타일이 [ContentControl](https://msdn.microsoft.com/library/windows/apps/br209365)을 대상으로 지정할 경우 이 스타일을 기반으로 하는 파생 스타일은 **ContentControl**, 또는 [Button](https://msdn.microsoft.com/library/windows/apps/br209265) 및 [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/br209527)와 같은 **ContentControl**에서 파생된 유형을 대상으로 지정할 수 있습니다. 파생 스타일에 값이 설정되어 있지 않으면 기본 스타일에서 상속받습니다. 기본 스타일의 값을 변경하면 파생 스타일이 이 값보다 우선합니다. 다음 예제는 동일한 기본 스타일에서 상속받은 스타일이 적용된 **Button** 및 [CheckBox](https://msdn.microsoft.com/library/windows/apps/br209316)를 보여 줍니다.

![파생 스타일을 사용한 스타일 적용 단추](images/styles-buttons-based-on.png)

기본 스타일이 [ContentControl](https://msdn.microsoft.com/library/windows/apps/br209365)을 대상으로 지정하고 [Height](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 및 [Width](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 속성을 설정합니다. 이 스타일을 기반으로 하는 스타일은 **ContentControl**에서 파생되는 [CheckBox](https://msdn.microsoft.com/library/windows/apps/br209316) 및 [Button](https://msdn.microsoft.com/library/windows/apps/br209265)를 대상으로 지정합니다. 파생 스타일은 [BorderBrush](https://msdn.microsoft.com/library/windows/apps/br209397) 및 [Foreground](https://msdn.microsoft.com/library/windows/apps/br209414) 속성에 대해 서로 다른 색상을 설정합니다. (일반적으로 **CheckBox** 주위에 테두리를 배치하지 않습니다. 여기서 스타일의 효과를 표시하기 위해 배치합니다.)

```XAML
<Page.Resources>
    <Style x:Key="BasicStyle" TargetType="ContentControl">
        <Setter Property="Width" Value="130" />
        <Setter Property="Height" Value="30" />
    </Style>

    <Style x:Key="ButtonStyle" TargetType="Button"
           BasedOn="{StaticResource BasicStyle}">
        <Setter Property="BorderBrush" Value="Orange" />
        <Setter Property="BorderThickness" Value="2" />
        <Setter Property="Foreground" Value="Red" />
    </Style>

    <Style x:Key="CheckBoxStyle" TargetType="CheckBox"
           BasedOn="{StaticResource BasicStyle}">
        <Setter Property="BorderBrush" Value="Blue" />
        <Setter Property="BorderThickness" Value="2" />
        <Setter Property="Foreground" Value="Green" />
    </Style>
</Page.Resources>

<StackPanel>
    <Button Content="Button" Style="{StaticResource ButtonStyle}" Margin="0,10"/>
    <CheckBox Content="CheckBox" Style="{StaticResource CheckBoxStyle}"/>
</StackPanel>
```

## <a name="use-tools-to-work-with-styles-easily"></a>스타일 작업을 쉽게 할 수 있는 도구 사용

스타일을 컨트롤에 적용하는 가장 빠른 방법은 Microsoft Visual Studio XAML 또는 디자인 화면에서 컨트롤을 마우스 오른쪽 단추로 클릭하고 **스타일 편집** 또는 **템플릿 편집**을 선택하는 것입니다(마우스 오른쪽 단추로 클릭하는 컨트롤에 따라 다름). 그런 다음 **리소스 적용**을 선택하여 기존 스타일을 적용하거나 **빈 스타일 만들기**를 선택하여 새로운 스타일을 정의할 수 있습니다. 빈 스타일을 만드는 경우 페이지, App.xaml 파일 또는 별도의 리소스 사전에서 스타일을 정의할 수 있는 옵션이 제공됩니다.

## <a name="lightweight-styling"></a>경량 스타일 지정

시스템 브러시를 재정의하는 작업은 일반적으로 앱 또는 페이지 수준에서 수행되며, 두 경우 모두 해당 브러시를 참조하는 모든 컨트롤에 색상 재지정이 적용됩니다. 또한 XAML에서 많은 컨트롤은 동일한 시스템 브러시를 참조할 수 있습니다.

![스타일이 적용된 단추](images/LightweightStyling_ButtonStatesExample.png)

```XAML
<Page.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Light">
                 <SolidColorBrush x:Key="ButtonBackground" Color="Transparent"/>
                 <SolidColorBrush x:Key="ButtonForeground" Color="MediumSlateBlue"/>
                 <SolidColorBrush x:Key="ButtonBorderBrush" Color="MediumSlateBlue"/>
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Page.Resources>
```

PointerOver(단추 위로 마우스를 가져감), **PointerPressed**(단추가 호출됨) 또는 Disabled(단추가 조작 가능하지 않음)와 같은 상태에 해당합니다. **ButtonBackgroundPointerOver**, **ButtonForegroundPointerPressed**, **ButtonBorderBrushDisabled** 등의 끝이 원래의 경량 스타일 이름에 추가됩니다. 해당 브러시도 수정하면 컨트롤 색상이 앱 테마와 일관되게 지정됩니다.

이러한 브러시 재정의를 **App.Resources** 수준에 배치하면 단일 페이지가 아닌 전체 앱 내의 모든 단추가 변경됩니다.

### <a name="per-control-styling"></a>컨트롤 기준 스타일 지정

특정 컨트롤의 다른 버전은 변경하지 않으면서 한 페이지에서만 특정 방식으로 보이도록 해당 컨트롤을 변경해야 하는 경우가 있을 수 있습니다.

![스타일이 적용된 단추](images/LightweightStyling_CheckboxExample.png)

```XAML
<CheckBox Content="Normal CheckBox" Margin="5"/>
    <CheckBox Content="Special CheckBox" Margin="5">
        <CheckBox.Resources>
            <ResourceDictionary>
                <ResourceDictionary.ThemeDictionaries>
                    <ResourceDictionary x:Key="Light">
                        <SolidColorBrush x:Key="CheckBoxForegroundUnchecked"
                            Color="Purple"/>
                        <SolidColorBrush x:Key="CheckBoxForegroundChecked"
                            Color="Purple"/>
                        <SolidColorBrush x:Key="CheckBoxCheckGlyphForegroundChecked"
                            Color="White"/>
                        <SolidColorBrush x:Key="CheckBoxCheckBackgroundStrokeChecked"  
                            Color="Purple"/>
                        <SolidColorBrush x:Key="CheckBoxCheckBackgroundFillChecked"
                            Color="Purple"/>
                    </ResourceDictionary>
                </ResourceDictionary.ThemeDictionaries>
            </ResourceDictionary>
        </CheckBox.Resources>
    </CheckBox>
<CheckBox Content="Normal CheckBox" Margin="5"/>
```

이것은 해당 컨트롤이 있는 페이지의 단일 "특수 확인란"에만 영향을 미칩니다.

## <a name="modify-the-default-system-styles"></a>기본 시스템 스타일 수정

가능한 경우 Windows 런타임 기본 XAML 리소스에서 가져온 스타일을 사용해야 합니다. 고유한 스타일을 정의해야 할 경우 가능하면 기본 스타일을 기반으로 합니다(이전에 설명한 대로 파생 스타일을 사용하거나 원래 기본 스타일의 복사본을 편집하여).

## <a name="the-template-property"></a>Template 속성

스타일 setter는 Control의 [[Template](https://msdn.microsoft.com/library/windows/apps/br209390)](https://msdn.microsoft.com/library/windows/apps/br209465) 속성에 사용할 수 있으며 사실 대부분의 일반적인 XAML 스타일과 앱의 XAML 리소스를 구성합니다. 이 스타일 setter는 [컨트롤 템플릿](control-templates.md)에서 더 자세히 알아봅니다.
