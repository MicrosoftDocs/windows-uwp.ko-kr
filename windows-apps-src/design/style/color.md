---
author: QuinnRadich
description: UWP 앱에서 테마 컬러 및 테마 사용 방법을 알아보세요.
title: UWP 앱의 컬러
ms.author: quradic
ms.date: 4/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
design-contact: karenmui
ms.localizationpriority: medium
ms.openlocfilehash: 19f4d9cde6ee2bc9615f044f18bc5e8828ca1985
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/19/2018
ms.locfileid: "4052785"
---
# <a name="color"></a>색상

![영웅 이미지](images/header-color.svg)

색은 앱의 사용자에게 정보를 전달하는 직관적인 방법입니다. 대화형 작업을 나타내거나 사용자 작업에 대한 피드백을 제공하고 인터페이스에 시각적 연속성을 부여하는 데 사용할 수 있습니다. 

UWP 앱에서 색은 주로 테마 컬러와 테마로 결정됩니다. 이 문서에서는 앱에서 색을 사용하는 방법과 테마 컬러 및 테마 리소스를 사용하여 UWP 앱을 모든 테마 컨텍스트에서 사용할 수 있도록 하는 방법에 대해 설명합니다. 

## <a name="color-principles"></a>색의 원칙

:::row:::
    :::column:::
        **의미에 따라 색을 사용 합니다.**
중요한 요소를 강조할 때 색을 간간히 사용하면 유연하고 직관적인 사용자 인터페이스를 구현할 수 있습니다.
    :::column-end:::
    :::column:::
        **색을 사용 하 여 대화형 작업을 나타냅니다.**
대화형 응용 프로그램 요소를 나타내는 한 가지 색을 선택하는 것이 좋습니다. 예를 들어 많은 웹 페이지는 하이퍼링크를 나타낼 때 파란색 텍스트를 사용합니다.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **색은 개인입니다.**
Windows에서 사용자는 테마 컬러와 밝거나 어두운 테마를 선택하여 사용 중인 전체 환경에 이것을 반영할 수 있습니다. 사용자의 테마 컬러와 테마를 응용 프로그램에 통합하는 방법을 선택하여 사용 중인 환경을 개인화할 수 있습니다.
    :::column-end:::
    :::column:::
        **색은 곧 문화입니다.**
사용하는 색상이 다른 문화권의 사람들에게는 어떻게 해석되는지를 고려해야 합니다. 예를 들어 일부 문화권에서는 파란색이 미덕 및 보호와 관련이 있는 반면, 다른 문화에서는 애도를 나타내기도 합니다.
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
        사용자 지정 컨트롤에 대 한 템플릿을 만들 때 하드 코드 색 값 보다는 테마 브러시를 사용 합니다. 이런 방식으로 앱은 모든 테마에 쉽게 적응할 수 있습니다.

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
        ![사용자가 선택한 테마 헤더](images/color/user-accent.svg) ![사용자가 선택한 테마 컬러](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
        ![사용자 지정 테마 헤더](images/color/custom-accent.svg) ![사용자 지정 브랜드 테마 컬러](images/color/brand-color.svg)
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

## <a name="usability"></a>사용 편의성

:::row:::
    :::column:::
        ![대비 그림](images/color/illo-contrast.svg)
    :::column-end:::
    ::: 열 범위 = "2"::: **대비**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessiblity should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![대비 그림](images/color/illo-lighting.svg)
    :::column-end:::
    ::: 열 범위 = "2"::: **조명**

        Be aware that variation in ambient lighting can affect the useability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![대비 그림](images/color/illo-colorblindness.svg)
    :::column-end:::
    ::: 열 범위 = "2"::: **색맹**

        Be aware of how colorblindness could affect the useability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>관련 문서

- [XAML 스타일](../controls-and-patterns/xaml-styles.md)
- [XAML 테마 리소스](../controls-and-patterns/xaml-theme-resources.md)