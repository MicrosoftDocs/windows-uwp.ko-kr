---
author: Jwmsft
description: UWP 앱에서 테마 컬러 및 테마 사용 방법을 알아보세요.
title: UWP 앱의 컬러
ms.author: jimwalk
ms.date: 4/7/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: karenmui
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: aebfb7dc55ef6f633e0afce5b1d0f562ded6663e
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7299178"
---
# <a name="color"></a>색상

![영웅 이미지](images/header-color.svg)

색은 앱의 사용자에게 정보를 전달하는 직관적인 방법입니다. 대화형 작업을 나타내거나 사용자 작업에 대한 피드백을 제공하고 인터페이스에 시각적 연속성을 부여하는 데 사용할 수 있습니다. 

UWP 앱에서 색은 주로 테마 컬러와 테마로 결정됩니다. 이 문서에서는 앱에서 색을 사용하는 방법과 테마 컬러 및 테마 리소스를 사용하여 UWP 앱을 모든 테마 컨텍스트에서 사용할 수 있도록 하는 방법에 대해 설명합니다. 

## <a name="color-principles"></a>색의 원칙

:::row:::
    :::column:::
        **Use color meaningfully.**
        When color is used sparingly to highlight important elements, it can help create a user interface that is fluid and intuitive.
    :::column-end:::
    :::column:::
        **Use color to indicate interactivity.**
        It's a good idea to choose one color to indicate elements of your application that are interactive. For example, many web pages use blue text to denote a hyperlink.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Color is personal.**
        In Windows, users can choose an accent color and a light or dark theme, which are reflected throughout their experience. You can choose how to incorporate the user's accent color and theme into your application, personalizing their experience.
    :::column-end:::
    :::column:::
        **Color is cultural.**
        Consider how the colors you use will be interpreted by people from different cultures. For example, in some cultures the color blue is associated with virtue and protection, while in others it represents mourning.
    :::column-end:::
:::row-end:::

## <a name="themes"></a>테마

UWP 앱은 밝거나 어두운 응용 프로그램 테마를 사용할 수 있습니다. 이 테마는 앱의 백그라운드,텍스트, 아이콘, [공용 컨트롤](../controls-and-patterns/index.md)에 영향을 줍니다.

### <a name="light-theme"></a>밝은 테마

![밝은 테마](images/color/light-theme.svg)

### <a name="dark-theme"></a>어두운 테마

![어두운 테마](images/color/dark-theme.svg)

기본적으로 UWP 앱의 테마는 Windows 설정에서 사용자의 테마 기본 설정이거나 디바이스의 기본 테마(예: XBox의 어두운 색)입니다. 그렇지만 UWP 앱에 대한 테마를 설정할 수 있습니다. 

### <a name="changing-the-theme"></a>테마 변경

`App.xaml` 파일에서 **RequestedTheme** 속성을 변경하여 테마를 변경할 수 있습니다.

```XAML
<Application
    x:Class="App9.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App9"
    RequestedTheme="Dark">
</Application>
```

**RequestedTheme** 속성을 제거하면 응용 프로그램이 사용자의 시스템 설정을 사용할 수 있습니다.

또한 사용자는 인터페이스를 쉽게 확인할 수 있도록 대비색으로 구성된 작은 팔레트를 사용하는 고대비 테마를 선택할 수 있습니다. 이 경우에 시스템은 RequestedTheme을 재정의합니다.

### <a name="testing-themes"></a>테마 테스트

앱의 테마를 요청하지 않은 경우 밝은 테마와 어두운 테마로 앱을 테스트하여 모든 조건에서 앱을 명확하게 읽을 수 있는지 확인해야 합니다.

**참고**: Visual Studio에서 기본 RequestedTheme은 밝으므로, RequestedTheme을 변경하여 모두 테스트해야 합니다.

## <a name="theme-brushes"></a>테마 브러시

공용 컨트롤은 [테마 브러시](../controls-and-patterns/xaml-theme-resources.md#the-xaml-color-ramp-and-theme-dependent-brushes)를 자동으로 사용하여 밝은 테마와 어두운 테마의 대비를 조절합니다.

그 예로서 다음은 [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md)가 테마 브러시를 어떻게 사용하는지 보여주는 설명입니다.

![테마 브러시 컨트롤 예](images/color/theme-brushes.svg)

테마 브러시의 사용 용도는 다음과 같습니다.

- **Base**는 텍스트용입니다.
- **Alt**는 Base의 반전입니다.
- **Chrome**은 탐색 창이나 명령 모음과 같은 최상위 요소입니다.
- **List** 목록 컨트롤용입니다.

**Low**/**Medium**/**High**는 색상의 강도를 말합니다.

### <a name="using-theme-brushes"></a>테마 브러시 사용

:::row:::
    :::column:::
        When creating templates for custom controls, use theme brushes rather than hard code color values. This way, your app can easily adapt to any theme.

        For example, these [item templates for ListView](../controls-and-patterns/item-templates-listview.md) demonstrate how to use theme brushes in a custom template.
    :::column-end:::
    :::column:::
         ![double line list item with icon example](images/color/list-view.svg)
    :::column-end:::
:::row-end:::

```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="DoubleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Height="64" AutomationProperties.Name="{x:Bind CompositionName}">
                <Ellipse Height="48" Width="48" VerticalAlignment="Center">
                    <Ellipse.Fill>
                        <ImageBrush ImageSource="Placeholder.png"/>
                    </Ellipse.Fill>
                </Ellipse>
                <StackPanel Orientation="Vertical" VerticalAlignment="Center" Margin="12,0,0,0">
                    <TextBlock Text="{x:Bind CompositionName}"  Style="{ThemeResource BaseTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseHighBrush}" />
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource BodyTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

앱에서 테마 브러시를 사용하는 방법에 대한 자세한 내용은 [테마 리소스](../controls-and-patterns/xaml-theme-resources.md)를 참조하세요.

## <a name="accent-color"></a>테마 컬러

공용 컨트롤은 테마 컬러를 사용하여 상태 정보를 전달할 수 있습니다. 기본적으로 테마 컬러는 설정에서 사용자가 선택하는 `SystemAccentColor`입니다. 그러나 브랜드를 반영하기 위해 앱의 테마 컬러를 사용자 지정할 수 있습니다.

![Windows 컨트롤](images/color/windows-controls.svg)

:::row:::
    :::column:::
        ![user-selected accent header](images/color/user-accent.svg)
        ![user-selected accent color](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
        ![custom accent header](images/color/custom-accent.svg)
        ![custom brand accent color](images/color/brand-color.svg)
    :::column-end:::
:::row-end:::

### <a name="overriding-the-accent-color"></a>테마 컬러를 재정의

앱의 테마 컬러를 변경하려면 `app.xaml`에 다음 코드를 배치합니다.

```xaml
<Application.Resources>
    <ResourceDictionary>
        <Color x:Key="SystemAccentColor">#107C10</Color>
    </ResourceDictionary>
</Application.Resources>
```

### <a name="choosing-an-accent-color"></a>테마 컬러 선택

앱의 사용자 지정 테마 컬러를 선택할 경우 최적의 가독성을 위해 테마 컬러를 사용하는 텍스트와 백그라운드 대비가 충분한지 확인하세요. 대비를 테스트할 때에는 Windows 설정에서 색 선택 도구를 사용하거나 이 [온라인 대비 도구](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources)를 사용할 수 있습니다.

![Windows 설정 사용자 지정 테마 색 선택](images/color/color-picker.svg)

## <a name="accent-color-palette"></a>테마 색상표

Windows 셸의 테마 컬러 알고리즘은 테마 컬러의 밝은 음영과 어두운 음영을 생성합니다.

![테마 색상표](images/color/accent-color-palette.svg)

이 음영은 [테마 리소스 ](../controls-and-patterns/xaml-theme-resources.md)로서 액세스할 수 있습니다.

- `SystemAccentColorLight3`
- `SystemAccentColorLight2`
- `SystemAccentColorLight1`
- `SystemAccentColorDark1`
- `SystemAccentColorDark2`
- `SystemAccentColorDark3`

<!-- check this is true -->
[**UISettings.GetColorValue**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UISettings#Windows_UI_ViewManagement_UISettings_GetColorValue_Windows_UI_ViewManagement_UIColorType_) 메서드 및 [**UIColorType**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType) 열거형을 통해 테마 색상표를 프로그래밍 방식으로 액세스할 수도 있습니다.

앱에서 색 테마 지정을 위해 테마 컬러를 사용할 수 있습니다. 다음은 단추에서 테마 색상표를 사용하는 방법을 보여주는 예입니다.

![단추에 적용된 테마 색상표](images/color/color-theme-button.svg)

```xaml
<Page.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="ButtonBackground" Color="{ThemeResource SystemAccentColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver" Color="{ThemeResource SystemAccentColorLight1}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed" Color="{ThemeResource SystemAccentColorDark1}"/>
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Page.Resources>

<Button Content="Button"></Button>
```

컬러 백그라운드에서 컬러 텍스트를 사용할 때 텍스트와 백그라운드의 대비가 충분한지 확인하세요. 기본적으로 하이퍼링크나 하이퍼텍스트는 테마 컬러를 사용합니다. 백그라운드에 테마 컬러 변형을 적용하는 경우 원래 테마 컬러의 변형을 사용하여 컬러 백그라운드에서 컬러 텍스트의 대비를 최적화해야 합니다.

아래 차트는 테마 컬러의 밝거나 어두운 다양한 명암의 예와 컬러 표면에 컬러 유형을 적용하는 방법을 보여줍니다.

![color-on-color](images/color/color-on-color.svg)

스타일 컨트롤에 대한 자세한 내용은 [XAML 스타일](../controls-and-patterns/xaml-styles.md)을 참조하세요.

## <a name="color-api"></a>Color API

응용 프로그램에 색을 추가하는 데 사용할 수 있는 몇 가지 API가 있습니다. 첫째 [**Colors**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colors) 클래스는 대규모 사전 정의된 색 목록을 구현합니다. XAML 속성을 통해 여기에 자동으로 액세스할 수 있습니다. 아래의 예에서 단추를 만들고 배경색과 전경색 속성을 **Colors** 클래스의 구성원으로 설정합니다.

```xaml
<Button Background="MediumSlateBlue" Foreground="White">Button text</Button>
```

XAML에서 [**Color**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.color) 구조를 사용하여 RGB 또는 16진수 값에서 자체 색을 만들 수 있습니다.

```xaml
<Color x:Key="LightBlue">#FF36C0FF</Color>
```

**FromArgb** 메서드를 사용하여 코드에 동일한 색상을 만들 수도 있습니다.

```csharp
Color LightBlue = Color.FromArgb(255,54,192,255);
```

문자 "Argb"는 알파(불투명도), 적색, 녹색, 청색을 나타내며 네 가지 색 성분입니다. 각 인수 범위는 0~255입니다. 기본 불투명도 255 또는 100% 불투명도를 적용하는 첫 번째 값을 생략할 수 있습니다.

> [!Note]
> C++를 사용하는 경우 [**ColorHelper**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colorhelper) 클래스를 사용하여 색상을 만들어야 합니다.

**Color**의 가장 일반적인 용도는 [**SolidColorBrush**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.solidcolorbrush)용 인수이며, 이것을 사용하여 단색 UI 요소를 그릴 수 있습니다. 이 브러시는 [**ResourceDictionary**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.ResourceDictionary)에 일반적으로 정의되어 있으므로 여러 요소에서 재사용할 수 있습니다.

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="ButtonBackgroundBrush" Color="#FFFF4F67"/>
    <SolidColorBrush x:Key="ButtonForegroundBrush" Color="White"/>
</ResourceDictionary>
```

브러시를 사용하는 방법에 대한 자세한 내용은 [XAML 브러시](brushes.md)를 참조하세요.

## <a name="scoping-system-colors"></a>시스템 색 범위 지정

앱에서 자체 색을 정의 하는 것 외에도 **ColorSchemeResources** 태그를 사용 하 여 앱 전체에서 원하는 영역에이 systematized 색을 또한 범위 수 있습니다. 이 API 뿐만 아니라 색을 지정 하 고 테마 많은 컨트롤 그룹을 한 번에 몇 가지 속성, 뿐만 아니라 제공 하면 다른 많은 시스템 혜택을 설정 하 여 사용자 지정 색을 수동으로 정의 게 일반적으로 허용 합니다.

- **ColorSchemeResources** 를 사용 하 여 설정 하는 모든 색상 고대비 영향을 주지 않습니다.
  * 앱을 의미 추가 디자인 이나 개발자 비용 없이도 더 많은 사용자가 액세스할 수 있습니다.
- 쉽게 색을 설정할 수 밝거나 어두운 널리 퍼져 두 테마에서 API에 대 한 속성을 설정 하 여
- **ColorSchemeResources** 에 설정 된 색도 해당 시스템 색을 사용 하는 모든 유사한 컨트롤이 나타날 때까지 아래로 계단식 배열 됩니다.
  * 이렇게 하면 앱에서 브랜드의 모양을 유지 하면서 일관 된 색 스토리 하 게 됩니다는
- 템플릿을 다시 만들 필요 없이 모든 시각적 상태, 애니메이션 및 불투명도 변형 효과

### <a name="how-to-use-colorschemeresources"></a>ColorSchemeResources를 사용 하는 방법

ColorSchemeResources는 시스템 리소스 되는 범위 위치를 알려 주는 API입니다. ColorSchemeResources는 [X:key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute)를 세 가지 중 하나일 수 있는 수행 해야 합니다.
- Default
  * [밝게](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme) 및 [어둡게](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme) 둘 다 테마 색 변경 내용을 표시 됩니다.
- Light
  * 색 변경을 [밝은 테마](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme) 에 표시 됩니다. 
- 어둡게
  * 색 변경을 [어두운 테마](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme) 에 표시 됩니다.

X:key를 설정 하면 시스템 또는 앱 테마 색 적절 하 게 변경 두 테마에서 서로 다른 사용자 지정 모양을 선택 해야 합니다.

### <a name="how-to-apply-scoped-colors"></a>범위가 지정 된 색을 적용 하는 방법

범위는 **ColorSchemeResources** API XAML에서 리소스를 사용 하면 모든 시스템 색 또는 [테마 리소스](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-theme-resources) 라이브러리에 있고 페이지 또는 컨테이너의 범위 내에서를 재정의 하는 브러시를 수행할 수 있습니다.

예를 들어, 두 개의 시스템 색- **SystemBaseLowColor** 및 grid에서 내부 **SystemBaseMediumLowColor** 정의 하 고 다음 페이지에 두 개의 단추를 배치 하는 경우: 해당 그리드 내 1과 하나의 외부:

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorSchemeResources x:Key="Default" 
        SystemBaseLowColor="LightGreen" 
        SystemBaseMediumLowColor="DarkCyan"/>
    </Grid.Resources>

    <Buton Content="Button_A"/>
</Grid>
<Buton Content="Button_B"/>
```

적용 된 새 색과 **Button_A** 얻게 미치며 **Button_B** 는 우리의 시스템 기본 단추 처럼 보이는:

![범위가 지정 된 시스템 색 단추](images/color/scopedcolors_cyan_button.png)

그러나 우리 모든 시스템 색 너무 다른 컨트롤 나타날 때까지 아래로 계단식, 이후 **SystemBaseLowColor** 및 **SystemBaseMediumLowColor** 설정 이상의 단추 적용 됩니다. 이 경우 제어 **ToggleButton**, **라디오 단추** 및 **슬라이더** 는 또한 영향을 받을 이러한 시스템 색 변경 처럼 해당 컨트롤에 저장 되어야 exampl 그리드 범위 위에 합니다.
시스템 색 변경에 *에 단일 컨트롤만* 범위를 지정 하려는 경우 **ColorSchemeResources** 해당 컨트롤의 리소스 내에서 정의 하 여이 수행할 수 있습니다.

```xaml
<Grid x:Name="Grid_A">
    <Button Content="Button_A">
        <Button.Resources>
            <ColorSchemeResources x:Key="Default" 
                SystemBaseLowColor="LightGreen" 
                SystemBaseMediumLowColor="DarkCyan"/>
        </Button.Resources>
    </Button>
</Grid>
<Button Content="Button_B"/>
```
기본적으로, 앞으로 정확히 동일한 기능 했지만 이제 눈금 목록에 추가 하는 다른 모든 컨트롤 색 변경을 반영 되지 않습니다. 이러한 시스템 색 **Button_A** 로 범위가 때문입니다.

### <a name="nesting-scoped-resources"></a>범위가 지정 된 중첩 리소스

시스템 색 중첩 수도 하 고 앱 레이아웃의 태그 내에 중첩 된 요소가 리소스에 **ColorSchemeResources** 전환 하 여 수행 됩니다.

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorSchemeResources x:Key="Default"
            SystemBaseLowColor="LightGreen"
            SystemBaseMediumLowColor="DarkCyan"/>
    </Grid.Resources>

    <Button Content="Button_A"/>
    <Grid x:Name="Grid_B">
        <Grid.Resources>
            <ColorSchemeResources x:Key="Default"
                SystemBaseLowColor="Goldenrod"
                SystemBaseMediumLowColor="DarkGoldenrod"/>
        </Grid.Resources>

        <Button Content="Nested Button"/>
    </Grid>
</Grid>
```

이 예제에서는 **Button_A** 상속 하는 색 **Grid_A**의 리소스에서 정의 하 고 **중첩 된 단추** 는 **Grid_B**의 리소스에서 색을 상속 합니다. 에 정의 된 마지막 항목이 없는 경우이 기본 색을 적용 하 고 더 나아가이 따라서 **Grid_B** 내 다른 컨트롤에 배치 하는 확인 하거나 확인 하거나 **Grid_A**의 리소스를 적용 하기 전에 먼저 **Grid_B**의 리소스를 적용 합니다 페이지 또는 앱 수준입니다.

이 임의 개수의 리소스가 색상 정의 하는 중첩 된 요소에 대해 작동 합니다.

### <a name="scoping-with-a-resourcedictionary"></a>ResourceDictionary를 사용 하 여 범위 지정

컨테이너 또는 페이지의 리소스에 제한 되지 및 ResourceDictionary 다음 병합 될 수 있는 모든 범위에서 일반적으로 사전 병합는 방식으로 이러한 시스템 색을 정의할 수 있습니다.

#### <a name="mycustomthemexaml"></a>MyCustomTheme.xaml

첫째, ResourceDictionary 만들게 됩니다. 다음은 ThemeDictionaries 내 **ColorSchemeResources** 배치 하 고 원하는 시스템 색을 재정의:

```xaml
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TestApp">

    <ResourceDictionary.ThemeDictionaries>

        <ColorSchemeResources x:Key="Default"
            SystemBaseLowColor="LightGreen"
            SystemBaseMediumLowColor="DarkCyan"/>
        
    </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

#### <a name="mainpagexaml"></a>MainPage.xaml

레이아웃에 포함 된 페이지에서 원하는 범위에서 해당 사전을 병합 하기만 하면 됩니다.

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <ResourceDictionary Source="MyCustomTheme.xaml"/>
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
    </Grid.Resources>
             
    <Button Content="Button_A"/>
</Grid>
```

이제 모든 리소스, 테마, 및 색상을 사용자 지정 단일 **MyCustomTheme** 리소스 사전에 배치 되어 레이아웃 태그의 추가 혼란에 걱정할 필요 없이 필요한 경우 범위.

### <a name="other-ways-to-define-color-resources"></a>색 리소스를 정의 하는 다른 방법

ColorSchemeResources에 배치할 시스템 색 및 줄에 있는 것이 아니라 한 래퍼에 직접 정의 대 한 수도 있습니다.

``` xaml
<ColorSchemeResources x:Key="Dark">
    <Color x:Key="SystemBaseLowColor">Goldenrod</Color>
</ColorSchemeResources>
```

## <a name="usability"></a>사용 편의성

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-contrast.svg)
    :::column-end:::
    :::column span="2":::
        **Contrast**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessiblity should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-lighting.svg)
    :::column-end:::
    :::column span="2":::
        **Lighting**

        Be aware that variation in ambient lighting can affect the useability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-colorblindness.svg)
    :::column-end:::
    :::column span="2":::
        **Colorblindness**

        Be aware of how colorblindness could affect the useability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>관련 문서

- [XAML 스타일](../controls-and-patterns/xaml-styles.md)
- [XAML 테마 리소스](../controls-and-patterns/xaml-theme-resources.md)
