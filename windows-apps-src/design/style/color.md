---
description: UWP(유니버설 Windows 플랫폼) 앱에서 강조 색 및 테마 리소스를 조작하여 색을 효과적으로 사용하는 방법을 알아봅니다.
title: Windows 앱의 색
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
design-contact: karenmui
ms.localizationpriority: medium
ms.custom: RS5
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: f7035017b1b1bfe878e4bd390fcefbe2bf3a6be5
ms.sourcegitcommit: 6cb20dca1cb60b4f6b894b95dcc2cc3a166165ad
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91636563"
---
# <a name="color"></a>색

![영웅 이미지](images/header-color.svg)

색은 앱의 사용자에게 정보를 전달하는 직관적인 방법입니다. 대화형 작업을 나타내거나 사용자 작업에 대한 피드백을 제공하고 인터페이스에 시각적 연속성을 부여하는 데 사용할 수 있습니다.

Windows 앱에서 색은 주로 테마 컬러와 테마로 결정됩니다. 이 문서에서는 앱에서 색을 사용하는 방법과 테마 컬러 및 테마 리소스를 통해 모든 테마 컨텍스트에서 Windows 앱을 사용할 수 있도록 하는 방법을 설명합니다.

## <a name="color-principles"></a>색의 원칙

:::row:::
    :::column:::
**색을 의미 있게 사용합니다.**
색을 조금만 사용하여 중요한 요소를 강조하면 유동적이고 직관적인 사용자 인터페이스를 만들 수 있습니다.
    :::column-end:::
    :::column:::
**색을 사용하여 대화형 작업을 나타냅니다.**
한 가지 색을 선택하여 대화형 애플리케이션의 요소를 나타내는 것이 좋습니다. 예를 들어 대부분의 웹 페이지에서는 파란색 텍스트를 사용하여 하이퍼링크를 나타냅니다.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
**색은 맞춤입니다.**
Windows에서 사용자는 자신의 환경 전체에 반영되는 테마 컬러와 밝거나 어두운 테마를 선택할 수 있습니다. 사용자의 테마 컬러와 테마를 애플리케이션에 통합하여 자신의 환경을 요구 사항에 맞게 설정하는 방법을 선택할 수 있습니다.
    :::column-end:::
    :::column:::
**색은 문화입니다.**
다른 문화권의 사용자가 사용된 색을 해석하는 방법을 고려합니다. 예를 들어 일부 문화권에서는 파란색이 미덕과 보호를 나타내지만, 다른 문화권에서는 애도를 나타냅니다.
    :::column-end:::
:::row-end:::

## <a name="themes"></a>테마

Windows 앱은 밝거나 어두운 애플리케이션 테마를 사용할 수 있습니다. 이 테마는 앱의 배경, 텍스트, 아이콘, [공용 컨트롤](../controls-and-patterns/index.md)에 영향을 줍니다.

### <a name="light-theme"></a>밝은 테마

![밝은 테마](images/color/light-theme.svg)

### <a name="dark-theme"></a>어두운 테마

![어두운 테마](images/color/dark-theme.svg)

기본적으로 Windows 앱의 테마는 Windows 설정의 사용자 테마 기본 설정이거나 디바이스의 기본 테마(예: XBox의 어두운 색)입니다. 하지만 사용자가 Windows 앱의 테마를 설정할 수 있습니다.

### <a name="changing-the-theme"></a>테마 변경

`App.xaml` 파일에서 **RequestedTheme** 속성을 변경하면 테마를 변경할 수 있습니다.

```XAML
<Application
    x:Class="App9.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App9"
    RequestedTheme="Dark">
</Application>
```

**RequestedTheme** 속성을 제거하면 애플리케이션이 사용자의 시스템 설정을 사용할 수 있습니다.

사용자가 인터페이스를 보기 쉽도록 대비색으로 구성된 작은 팔레트를 사용하는 고대비 테마를 선택할 수도 있습니다. 이 경우, 시스템이 RequestedTheme을 재정의합니다.

### <a name="testing-themes"></a>테마 테스트

앱의 테마를 요청하지 않는 경우, 밝은 테마와 어두운 테마로 앱을 테스트하여 모든 조건에서 앱을 읽을 수 있는지 확인해야 합니다.

**참고**: Visual Studio에서 기본 RequestedTheme은 밝게이므로, RequestedTheme을 변경해서 둘 다 테스트해야 합니다.

## <a name="theme-brushes"></a>테마 브러시

공용 컨트롤은 자동으로 [테마 브러시](../controls-and-patterns/xaml-theme-resources.md#the-xaml-color-ramp-and-theme-dependent-brushes)를 사용하여 밝은 테마와 어두운 테마의 대비를 조절합니다.

예를 들어 [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md)에서 테마 브러시를 사용하는 방법을 보여 주는 그림은 다음과 같습니다.

![테마 브러시 컨트롤 예제](images/color/theme-brushes.svg)

테마 브러시는 다음과 같은 용도로 사용됩니다.

- **Base**는 텍스트에 사용합니다.
- **Alt**는 Base의 반전입니다.
- **Chrome**은 탐색 창이나 명령 모음과 같은 최상위 요소에 사용합니다.
- **List**는 목록 컨트롤에 사용합니다.

**Low**/**Medium**/**High**는 색의 강도를 가리킵니다.

### <a name="using-theme-brushes"></a>테마 브러시 사용

:::row:::
    :::column:::
사용자 지정 컨트롤용 템플릿을 만드는 경우 하드 코드 색 값 대신 테마 브러시를 사용합니다. 앱에서는 이러한 방식으로 모든 테마에 쉽게 적용할 수 있습니다.

예를 들어 이러한 [ListView의 항목 템플릿](../controls-and-patterns/item-templates-listview.md)은 사용자 지정 템플릿에서 테마 브러시를 사용하는 방법을 보여 줍니다.
    :::column-end:::
    :::column:::
 ![아이콘 예제가 있는 두 줄 목록 항목](images/color/list-view.svg)
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

## <a name="accent-color"></a>강조 색

공용 컨트롤은 테마 컬러를 사용하여 상태 정보를 전달할 수 있습니다. 기본적으로 테마 컬러는 설정에서 사용자가 선택하는 `SystemAccentColor`입니다. 그러나 브랜드를 반영하기 위해 앱의 테마 컬러를 사용자 지정할 수 있습니다.

![Windows 컨트롤](images/color/windows-controls.svg)

:::row:::
    :::column:::
![사용자가 선택한 테마 헤더](images/color/user-accent.svg)
![사용자가 선택한 테마 컬러](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
![사용자 지정 테마 헤더](images/color/custom-accent.svg)
![사용자 지정 브랜드 테마 컬러](images/color/brand-color.svg)
    :::column-end:::
:::row-end:::

### <a name="overriding-the-accent-color"></a>테마 컬러 재정의

앱의 테마 컬러를 변경하려면 `app.xaml`에 다음 코드를 배치합니다.

```xaml
<Application.Resources>
    <ResourceDictionary>
        <Color x:Key="SystemAccentColor">#107C10</Color>
    </ResourceDictionary>
</Application.Resources>
```

### <a name="choosing-an-accent-color"></a>테마 컬러 선택

앱의 사용자 지정 테마 컬러를 선택하는 경우 최적의 가독성을 위해 테마 컬러를 사용하는 텍스트와 배경의 대비가 충분한지 확인합니다. 대비를 테스트하기 위해 Windows 설정의 색 선택 도구를 사용하거나, 이 [온라인 대비 도구](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources)를 사용할 수 있습니다.

![Windows 설정 사용자 지정 테마 컬러 선택](images/color/color-picker.svg)

## <a name="accent-color-palette"></a>테마 컬러 색상표

Windows 셸의 테마 컬러 알고리즘은 테마 컬러의 밝은 음영과 어두운 음영을 생성합니다.

![테마 컬러 색상표](images/color/accent-color-palette.svg)

[테마 리소스](../controls-and-patterns/xaml-theme-resources.md)로 이러한 음영에 액세스할 수 있습니다.

- `SystemAccentColorLight3`
- `SystemAccentColorLight2`
- `SystemAccentColorLight1`
- `SystemAccentColorDark1`
- `SystemAccentColorDark2`
- `SystemAccentColorDark3`

<!-- check this is true -->
[**UISettings.GetColorValue**](/uwp/api/Windows.UI.ViewManagement.UISettings#Windows_UI_ViewManagement_UISettings_GetColorValue_Windows_UI_ViewManagement_UIColorType_) 메서드 및 [**UIColorType**](/uwp/api/Windows.UI.ViewManagement.UIColorType) 열거형을 통해 테마 컬러 색상표를 프로그래밍 방식으로 액세스할 수도 있습니다.

앱에서 색 테마 지정을 위해 테마 컬러 색상표를 사용할 수 있습니다. 다음은 단추에 테마 컬러 색상표를 사용하는 방법을 보여 주는 예제입니다.

![단추에 적용된 테마 컬러 색상표](images/color/color-theme-button.svg)

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

컬러 배경에서 컬러 텍스트를 사용하는 경우 텍스트와 배경의 대비가 충분한지 확인합니다. 기본적으로 하이퍼링크나 하이퍼텍스트는 테마 컬러를 사용합니다. 배경에 테마 컬러 변형을 적용하는 경우 원래 테마 컬러의 변형을 사용하여 컬러 배경에서 컬러 텍스트의 대비를 최적화해야 합니다.

아래 차트는 테마 컬러의 다양한 밝은/어두운 음영 예와 컬러 표면에 컬러 유형을 적용하는 방법을 보여 줍니다.

![위쪽의 연한 파란색에서 아래쪽의 진한 파란색으로 변하는 색 그라데이션을 보여주는 Color on Color 차트의 스크린샷.](images/color/color-on-color.png)

컨트롤에 스타일을 지정하는 방법에 대한 자세한 내용은 [XAML 스타일](../controls-and-patterns/xaml-styles.md)을 참조하세요.

## <a name="color-api"></a>색 API

애플리케이션에 색을 추가하는 데 사용할 수 있는 몇 가지 API가 있습니다. 첫째로, [**Colors**](/uwp/api/windows.ui.colors) 클래스는 미리 정의된 색의 큰 목록을 구현합니다. XAML 속성을 통해 자동으로 액세스할 수 있습니다. 아래 예제에서는 단추를 만들고 배경색과 전경색 속성을 **Colors** 클래스의 멤버로 설정합니다.

```xaml
<Button Background="MediumSlateBlue" Foreground="White">Button text</Button>
```

XAML에서 [**Color**](/uwp/api/windows.ui.color) 구조체를 사용하여 RGB 또는 16진수 값으로 고유한 색을 만들 수 있습니다.

```xaml
<Color x:Key="LightBlue">#FF36C0FF</Color>
```

**FromArgb** 메서드를 사용하여 코드에서 동일한 색을 만들 수도 있습니다.

```csharp
Color LightBlue = Color.FromArgb(255,54,192,255);
```
```cppwinrt
Windows::UI::Color LightBlue = Windows::UI::ColorHelper::FromArgb(255,54,192,255);
```

문자 “Argb”는 네 가지 색 구성 요소인 알파(불투명도), 빨간색, 녹색, 파란색을 나타냅니다. 각 인수의 범위는 0~255입니다. 기본 불투명도 255(100% 불투명)를 제공하는 첫 번째 값을 생략할 수 있습니다.

> [!Note]
> C++를 사용하는 경우 [**ColorHelper**](/uwp/api/windows.ui.colorhelper) 클래스를 통해 색을 만들어야 합니다.

단색 UI 요소를 그리는 데 사용할 수 있는 [**SolidColorBrush**](/uwp/api/windows.ui.xaml.media.solidcolorbrush)의 인수로 **Color**를 사용하는 경우가 가장 일반적입니다. 이 브러시는 일반적으로 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)에 정의되어 있으므로 여러 요소에서 재사용할 수 있습니다.

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="ButtonBackgroundBrush" Color="#FFFF4F67"/>
    <SolidColorBrush x:Key="ButtonForegroundBrush" Color="White"/>
</ResourceDictionary>
```

브러시를 사용하는 방법에 대한 자세한 내용은 [XAML 브러시](brushes.md)를 참조하세요.

## <a name="scoping-system-colors"></a>시스템 색 범위 지정

앱에서 고유한 색을 정의하는 것은 물론, **ColorPaletteResources** 태그를 사용하여 체계적인 색의 범위를 앱 전체의 원하는 영역으로 지정할 수도 있습니다. 이 API를 사용하면 몇 가지 속성을 설정하여 큰 컨트롤 그룹의 색과 테마를 한 번에 지정할 수 있을 뿐 아니라 수동으로 사용자 지정 색을 정의할 때는 불가능한 다른 많은 시스템 이점이 있습니다.

- **ColorPaletteResources**를 사용하여 설정된 색은 고대비에 영향을 주지 않음
  * 추가 디자인 또는 개발 비용 없이도 더 많은 사용자가 앱에 액세스할 수 있음
- API에서 하나의 속성을 설정하여 두 테마의 색을 모두 밝게, 어둡게 또는 보급형으로 쉽게 설정할 수 있음
- **ColorPaletteResources**에 설정된 색은 해당 시스템 색을 사용하는 유사한 모든 컨트롤에 계단식으로 적용됨
  * 브랜드 모양을 유지하면서 앱 전체에 일관성 있는 색 스토리 적용
- 템플릿을 다시 작성하지 않고도 모든 시각적 상태, 애니메이션 및 불투명도 변형 적용

### <a name="how-to-use-colorpaletteresources"></a>ColorPaletteResources 사용 방법

ColorPaletteResources는 어디에서, 어떤 리소스로 범위가 지정되는지를 시스템에 알려 주는 API입니다. ColorPaletteResources는 [x:Key](../../xaml-platform/x-key-attribute.md)를 사용해야 하며, 다음 세 가지 선택 항목 중 하나일 수 있습니다.
- Default
  * [밝은](#light-theme) 테마와 [어두운](#dark-theme) 테마에서 모두, 색 변경이 표시됩니다.
- Light
  * [밝은 테마](#light-theme)에서만 색 변경이 표시됩니다.
- Dark
  * [어두운 테마](#dark-theme)에서만 색 변경이 표시됩니다.

테마 중 하나에서 다른 사용자 지정 모양을 원하는 경우 x:Key를 설정하면 시스템 또는 앱 테마에 맞게 색이 변경됩니다.

### <a name="how-to-apply-scoped-colors"></a>범위가 지정된 색을 적용하는 방법

XAML에서 **ColorPaletteResources** API를 통해 리소스의 범위를 지정하면 [테마 리소스](../controls-and-patterns/xaml-theme-resources.md) 라이브러리에 있는 모든 시스템 색이나 브러시를 가져와 페이지 또는 컨테이너 범위 내에서 다시 정의할 수 있습니다.

예를 들어 **BaseLow** 및 **BaseMediumLow**라는 두 개의 시스템 색을 그리드에 정의하고 해당 그리드 내부와 외부에 각각 하나씩, 두 개의 단추를 페이지에 배치한다고 가정합니다.

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorPaletteResources x:Key="Default"
        BaseLow="LightGreen"
        BaseMediumLow="DarkCyan"/>
    </Grid.Resources>

    <Buton Content="Button_A"/>
</Grid>
<Buton Content="Button_B"/>
```

그러면 **Button_A**에는 새로운 색이 적용되고, **Button_B**는 시스템 기본 단추와 같은 모양으로 유지됩니다.

![범위가 지정된 단추의 시스템 색](images/color/scopedcolors_cyan_button.png)

그러나 모든 시스템 색은 다른 컨트롤에도 계단식으로 적용되므로 **BaseLow** 및 **BaseMediumLow**를 설정하면 단추 이상의 영역에 영향을 줍니다. 여기서는 **ToggleButton**, **RadioButton** 및 **Slider**와 같은 컨트롤도 위의 예제 그리드 범위에 있을 경우 이러한 시스템 색 변경의 영향을 받습니다.
시스템 색 변경의 범위를 ‘단일 컨트롤만’으로 지정하려는 경우 해당 컨트롤의 리소스 내에서 **ColorPaletteResources**를 정의하면 됩니다. 

```xaml
<Grid x:Name="Grid_A">
    <Button Content="Button_A">
        <Button.Resources>
            <ColorPaletteResources x:Key="Default"
                BaseLow="LightGreen"
                BaseMediumLow="DarkCyan"/>
        </Button.Resources>
    </Button>
</Grid>
<Button Content="Button_B"/>
```
근본적으로 컨트롤은 이전과 동일하지만, 이제 그리드에 추가한 다른 컨트롤에 색 변경이 적용되지 않습니다. 해당 시스템 색의 범위가 **Button_A**만으로 지정되었기 때문입니다.

### <a name="nesting-scoped-resources"></a>범위가 지정된 리소스 중첩

시스템 색을 중첩할 수도 있으며, 앱 레이아웃 태그에 있는 중첩된 요소의 리소스에 **ColorPaletteResources**를 배치하면 됩니다.

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorPaletteResources x:Key="Default"
            BaseLow="LightGreen"
            BaseMediumLow="DarkCyan"/>
    </Grid.Resources>

    <Button Content="Button_A"/>
    <Grid x:Name="Grid_B">
        <Grid.Resources>
            <ColorPaletteResources x:Key="Default"
                BaseLow="Goldenrod"
                BaseMediumLow="DarkGoldenrod"/>
        </Grid.Resources>

        <Button Content="Nested Button"/>
    </Grid>
</Grid>
```

이 예제에서 **Button_A**는 **Grid_A**의 리소스에 정의된 색을 상속받고, **Nested Button**은 **Grid_B**의 리소스에서 색을 상속받습니다. 더 나아가, **Grid_B**에 배치된 다른 모든 컨트롤은 **Grid_B**의 리소스를 먼저 확인하거나 적용한 후 **Grid_A**의 리소스를 확인하거나 적용하고, 마지막으로 페이지 또는 앱 수준에서 정의된 색이 없을 경우 기본 색을 적용합니다.

이 기능은 리소스에 색 정의가 있는 모든 개수의 중첩된 요소에서 작동합니다.

### <a name="scoping-with-a-resourcedictionary"></a>ResourceDictionary로 범위 지정

컨테이너 또는 페이지의 리소스에만 국한되지 않고 ResourceDictionary에서 이러한 시스템 색을 정의한 다음, 일반적으로 사전을 병합하는 방법으로 임의 범위에서 색을 병합할 수도 있습니다.

#### <a name="mycustomthemexaml"></a>MyCustomTheme.xaml

먼저 ResourceDictionary를 만듭니다. ThemeDictionaries에 **ColorPaletteResources**를 배치하고 원하는 시스템 색을 재정의합니다.

```xaml
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TestApp">

    <ResourceDictionary.ThemeDictionaries>
        <ResourceDictionary x:Key="Default">
            <ResourceDictionary.MergedDictionaries>

                <ColorPaletteResources x:Key="Default"
                    Accent="#FF0073CF"
                    AltHigh="#FF000000"
                    AltLow="#FF000000"/>

            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>        
    </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

#### <a name="mainpagexaml"></a>MainPage.xaml

레이아웃이 포함된 페이지에서 해당 사전을 원하는 범위에 병합합니다.

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

이제 모든 리소스, 테마, 사용자 지정 색을 단일 **MyCustomTheme** 리소스 사전에 배치한 다음, 레이아웃 태그를 수정하지 않고도 필요에 따라 범위를 지정할 수 있습니다.

### <a name="other-ways-to-define-color-resources"></a>색 리소스를 정의하는 기타 방법

ColorPaletteResources를 사용하면 인라인 대신 내부에서 래퍼로 직접 시스템 색을 배치하고 정의할 수도 있습니다.

``` xaml
<ColorPaletteResources x:Key="Dark">
    <Color x:Key="SystemBaseLowColor">Goldenrod</Color>
</ColorPaletteResources>
```

## <a name="usability"></a>사용 편의성

:::row:::
    :::column:::
![대비 그림](images/color/illo-contrast.svg)
    :::column-end:::
    :::column span="2":::
**대비**

요소와 이미지에는 테마 컬러 또는 테마에 관계없이 서로 구분할 수 있을 만큼 충분한 대비가 있어야 합니다.

애플리케이션에서 사용할 색을 고려하는 경우 접근성이 가장 중요합니다. 아래 지침을 사용하여 가능한 한 많은 사용자가 애플리케이션에 액세스할 수 있도록 합니다.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
![대비 그림](images/color/illo-lighting.svg)
    :::column-end:::
    :::column span="2":::
**조명**

주변 조명의 변화는 앱의 유용성에 영향을 줄 수 있습니다. 예를 들어 검은색 배경의 페이지는 화면 눈부심으로 인해 외부에서 읽을 수 없는 반면, 흰색 배경의 페이지는 어두운 공간에서 보는 것이 어려울 수 있습니다.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
![대비 그림](images/color/illo-colorblindness.svg)
    :::column-end:::
    :::column span="2":::
**색맹**

색맹은 앱의 유용성에 영향을 줄 수 있습니다. 예를 들어 적록 색맹인 사용자는 빨간색과 녹색 요소를 구분하는 데 어려움이 있습니다. **남성의 약 8%** 및 **여성의 약 0.5%** 는 적록 색맹이므로 이러한 색 조합을 애플리케이션 요소 간의 유일한 구별자로 사용하지 않도록 합니다.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>관련된 문서

- [XAML 스타일](../controls-and-patterns/xaml-styles.md)
- [XAML 테마 리소스](../controls-and-patterns/xaml-theme-resources.md)
