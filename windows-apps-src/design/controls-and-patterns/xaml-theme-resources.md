---
Description: XAML의 테마 리소스는 활성 상태인 시스템 테마에 따라 다른 값을 적용하는 리소스 집합입니다.
MS-HAID: dev\_ctrl\_layout\_txt.xaml\_theme\_resources
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: XAML 테마 리소스
ms.assetid: 41B87DBF-E7A2-44E9-BEBA-AF6EEBABB81B
label: XAML theme resources
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f2097a35d87594251ed2c0a04be06ccdb705902f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80249857"
---
# <a name="xaml-theme-resources"></a>XAML 테마 리소스

XAML의 테마 리소스는 활성 상태인 시스템 테마에 따라 다른 값을 적용하는 리소스 집합입니다. XAML 프레임워크에서 지원하는 테마는 “밝게”, “어둡게”, “고대비”의 세 가지입니다.

**필수 조건**: 이 항목에서는 [ResourceDictionary 및 XAML 리소스 참조](resourcedictionary-and-xaml-resource-references.md)를 이미 읽었다고 가정합니다.

## <a name="theme-resources-v-static-resources"></a>테마 리소스와 고정 리소스 비교

기존 XAML 리소스 사전에서 XAML 리소스를 참조할 수 있는 두 가지 XAML 태그 확장 즉, [{StaticResource} 태그 확장](../../xaml-platform/staticresource-markup-extension.md) 및 [{ThemeResource} 태그 확장](../../xaml-platform/themeresource-markup-extension.md)이 있습니다.

앱이 로드될 때 및 이후 런타임에 테마가 변경될 때마다 [{ThemeResource} 태그 확장](../../xaml-platform/themeresource-markup-extension.md)이 평가됩니다. 이것은 일반적으로 사용자가 디바이스 설정을 변경하거나 앱 내의 프로그래밍 방식이 변경되어 현재 테마가 바뀔 때 발생합니다.

반면에 [{StaticResource} 태그 확장](../../xaml-platform/staticresource-markup-extension.md)의 경우 XAML이 앱에서 처음 로드될 때만 평가됩니다. 업데이트는 수행되지 않습니다. 이 작업은 앱 시작 시, XAML에서 찾기가 수행된 후 실제 런타임 값으로 바뀌는 것과 비슷합니다.

## <a name="theme-resources-in-the-resource-dictionary-structure"></a>리소스 사전 구조의 테마 리소스

각 테마 리소스는 XAML 파일 themeresources.xaml의 일부입니다. 디자인 목적으로, themeresources.xaml은 Windows SDK(소프트웨어 개발 키트) 설치의 \\(Program Files)\\Windows Kits\\10\\DesignTime\\CommonConfiguration\\Neutral\\UAP\\&lt;SDK version&gt;\\Generic 폴더에 제공됩니다. 또한 themeresources.xaml의 리소스 사전은 동일한 디렉터리의 generic.xaml에서 재현됩니다.

Windows 런타임은 런타임 조회를 위해 이러한 물리적 파일을 사용하지 않습니다. 따라서 이러한 물리적 파일은 특별히 DesignTime 폴더에 있으며, 기본적으로 앱에 복사되지 않습니다. 대신 이러한 리소스 사전은 Windows 런타임 자체의 일부로 메모리에 존재하고, 여기서 테마 리소스 또는 시스템 리소스에 대한 앱의 XAML 리소스 참조를 런타임에 확인합니다.

## <a name="guidelines-for-custom-theme-resources"></a>사용자 지정 테마 리소스에 대한 지침

고유한 사용자 지정 테마 리소스를 정의하여 사용하는 경우 다음 지침을 따르세요.

- "고대비" 사전 외에 "밝게" 및 "어둡게" 둘 다에 대해서도 테마 사전을 지정하세요. "기본값"을 키로 사용하여 [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary)를 만들 수도 있지만 명시적으로 "밝게", "어둡게" 및 "고대비"를 대신 사용하는 것이 좋습니다.

- 스타일, setter, 컨트롤 템플릿, 속성 setter 및 애니메이션에서 [{ThemeResource} 태그 확장](../../xaml-platform/themeresource-markup-extension.md)을 사용하세요.

- [ThemeDictionaries](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries) 내의 리소스 정의에는 [{ThemeResource} 태그 확장](../../xaml-platform/themeresource-markup-extension.md)을 사용하지 마세요. 대신 [{StaticResource} 태그 확장](../../xaml-platform/staticresource-markup-extension.md)을 사용하세요.

    예외: [{ThemeResource} 태그 확장](../../xaml-platform/themeresource-markup-extension.md)을 사용하여 [ThemeDictionaries](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries)의 앱 테마에 관계없이 리소스를 참조할 수 있습니다. 이러한 리소스의 예로는 `SystemAccentColor`와 같은 테마 컬러 리소스 또는 일반적으로 "SystemColor"로 접두사가 지정된 시스템 색 리소스(예: `SystemColorButtonFaceColor`)가 있습니다.

> [!CAUTION]
> 다음 지침을 따르지 않으면 앱에서 테마와 관련된 예기치 않은 동작이 발생할 수 있습니다. 자세한 내용은 [테마 리소스 문제 해결](#troubleshooting-theme-resources) 섹션을 참조하세요.

## <a name="the-xaml-color-ramp-and-theme-dependent-brushes"></a>XAML 색 램프 및 테마 종속 브러시

"밝게", "어둡게" 및 "고대비" 테마에 대한 결합된 색 집합이 XAML의 *Windows 색 램프*를 구성합니다. 시스템 테마를 수정하거나 고유한 XAML 요소에 시스템 테마를 적용하려면 색 리소스의 구성 방식을 이해해야 합니다.

UWP 앱에 색을 적용하는 방법에 대한 자세한 내용은 [UWP 앱의 색](../style/color.md)을 참조하세요.

### <a name="light-and-dark-theme-colors"></a>밝게 및 어둡게 테마 색

XAML 프레임워크는 "밝게" 및 "어둡게" 테마에 맞게 조정된 값을 갖는 [Color](/uwp/api/Windows.UI.Color) 리소스 집합을 제공합니다. 이러한 리소스를 참조하는 데 사용하는 키는 다음과 같은 이름 지정 형식을 따릅니다. `System[Simple Light/Dark Name]Color`.

다음 표에는 XAML 프레임워크에서 제공하는 “밝게” 및 “어둡게” 리소스에 대한 키, 간단한 이름, 색의 문자열 표현(\#aarrggbb 형식 사용)이 나와 있습니다. 이 키는 앱에서 리소스를 참조하는 데 사용됩니다. "간단한 밝게/어둡게 이름"은 뒷부분에서 설명되는 브러시 이름 지정 규칙의 일부로 사용됩니다.

| 키                             | 간단한 밝게/어둡게 이름 | Light      | Dark       |
|---------------------------------|------------------------|------------|------------|
| SystemAltHighColor              | AltHigh                | \#FFFFFFFF | \#FF000000 |
| SystemAltLowColor               | AltLow                 | \#33FFFFFF | \#33000000 |
| SystemAltMediumColor            | AltMedium              | \#99FFFFFF | \#99000000 |
| SystemAltMediumHighColor        | AltMediumHigh          | \#CCFFFFFF | \#CC000000 |
| SystemAltMediumLowColor         | AltMediumLow           | \#66FFFFFF | \#66000000 |
| SystemBaseHighColor             | BaseHigh               | \#FF000000 | \#FFFFFFFF |
| SystemBaseLowColor              | BaseLow                | \#33000000 | \#33FFFFFF |
| SystemBaseMediumColor           | BaseMedium             | \#99000000 | \#99FFFFFF |
| SystemBaseMediumHighColor       | BaseMediumHigh         | \#CC000000 | \#CCFFFFFF |
| SystemBaseMediumLowColor        | BaseMediumLow          | \#66000000 | \#66FFFFFF |
| SystemChromeAltLowColor         | ChromeAltLow           | \#FF171717 | \#FFF2F2F2 |
| SystemChromeBlackHighColor      | ChromeBlackHigh        | \#FF000000 | \#FF000000 |
| SystemChromeBlackLowColor       | ChromeBlackLow         | \#33000000 | \#33000000 |
| SystemChromeBlackMediumLowColor | ChromeBlackMediumLow   | \#66000000 | \#66000000 |
| SystemChromeBlackMediumColor    | ChromeBlackMedium      | \#CC000000 | \#CC000000 |
| SystemChromeDisabledHighColor   | ChromeDisabledHigh     | \#FFCCCCCC | \#FF333333 |
| SystemChromeDisabledLowColor    | ChromeDisabledLow      | \#FF7A7A7A | \#FF858585 |
| SystemChromeHighColor           | ChromeHigh             | \#FFCCCCCC | \#FF767676 |
| SystemChromeLowColor            | ChromeLow              | \#FFF2F2F2 | \#FF171717 |
| SystemChromeMediumColor         | ChromeMedium           | \#FFE6E6E6 | \#FF1F1F1F |
| SystemChromeMediumLowColor      | ChromeMediumLow        | \#FFF2F2F2 | \#FF2B2B2B |
| SystemChromeWhiteColor          | ChromeWhite            | \#FFFFFFFF | \#FFFFFFFF |
| SystemListLowColor              | ListLow                | \#19000000 | \#19FFFFFF |
| SystemListMediumColor           | ListMedium             | \#33000000 | \#33FFFFFF |

:::row:::
    :::column:::
        #### <a name="light-theme"></a>밝은 테마
    :::column-end:::
    :::column:::
        #### <a name="dark-theme"></a>어두운 테마
    :::column-end:::
:::row-end:::

#### <a name="base"></a>기준

:::row:::
    :::column:::
        ![기본 밝은 테마](images/themes/light-base.png)
    :::column-end:::
    :::column:::
        ![기본 어두운 테마](images/themes/dark-base.png)
    :::column-end:::
:::row-end:::

#### <a name="alt"></a>Alt

:::row:::
    :::column:::
        ![대체 밝은 테마](images/themes/light-alt.png)
    :::column-end:::
    :::column:::
        ![대체 어두운 테마](images/themes/dark-alt.png)
    :::column-end:::
:::row-end:::

#### <a name="list"></a>목록

:::row:::
    :::column:::
        ![목록 밝은 테마](images/themes/light-list.png)
    :::column-end:::
    :::column:::
        ![목록 어두운 테마](images/themes/dark-list.png)
    :::column-end:::
:::row-end:::

#### <a name="chrome"></a>크롬

:::row:::
    :::column:::
        ![크롬 밝은 테마](images/themes/light-chrome.png)
    :::column-end:::
    :::column:::
        ![크롬 어두운 테마](images/themes/dark-chrome.png)
    :::column-end:::
:::row-end:::

### <a name="windows-system-high-contrast-colors"></a>Windows 시스템 고대비 색

XAML 프레임워크에서 제공하는 리소스 집합 외에, Windows 시스템 팔레트에서 가져온 색 값 집합이 있습니다. 이러한 색은 Windows 런타임 또는 UWP(유니버설 Windows 플랫폼) 앱에만 국한되지 않습니다. 그러나 색상 시스템이 "고대비" 테마를 사용하여 작동되고 앱이 실행될 때 많은 XAML [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush) 리소스가 이러한 색을 사용합니다. XAML 프레임워크는 이러한 시스템 수준 색을 키 입력 리소스로 제공합니다. 키는 다음과 같은 이름 지정 형식을 따릅니다. `SystemColor[name]Color`

다음 표에서는 XAML이 제공하는 시스템 수준 색을 Windows 시스템 팔레트에서 가져온 리소스 개체로 표시합니다. "접근성 이름" 열에는 Windows 설정 UI에서 색 레이블이 지정되는 방식이 표시됩니다. "간단한 고대비 이름" 열은 일반적인 XAML 컨트롤에서 색이 적용되는 방식을 한 단어로 나타냅니다. 이 설명은 뒷부분에서 설명되는 브러시 이름 지정 규칙의 일부로 사용됩니다. "초기 기본값" 열에는 시스템이 고대비에서 실행 중이 아닐 경우 얻게 되는 값이 표시됩니다.

| 키                           | 접근성 이름            | 단순한 고대비 이름 | 초기 기본값 |
|-------------------------------|--------------------------------|--------------------------|-----------------|
| SystemColorButtonFaceColor    | **단추 텍스트**(배경)   | 배경               | \#FFF0F0F0      |
| SystemColorButtonTextColor    | **단추 텍스트**(전경)   | 전경               | \#FF000000      |
| SystemColorGrayTextColor      | **사용할 수 없는 텍스트**              | 사용 안 함                 | \#FF6D6D6D      |
| SystemColorHighlightColor     | **선택한 텍스트**(배경) | 강조 표시                | \#FF3399FF      |
| SystemColorHighlightTextColor | **선택한 텍스트**(전경) | HighlightAlt             | \#FFFFFFFF      |
| SystemColorHotlightColor      | **하이퍼링크**                 | Hyperlink                | \#FF0066CC      |
| SystemColorWindowColor        | **배경**                 | PageBackground           | \#FFFFFFFF      |
| SystemColorWindowTextColor    | **텍스트**                       | PageText                 | \#FF000000      |

Windows는 다양한 고대비 테마를 제공하고 다음과 같이 사용자가 접근성 센터를 통해 고대비 설정에 특정 색을 설정할 수 있도록 합니다. 따라서 고대비 색 값의 확정된 목록을 제공할 수는 없습니다.

![Windows 고대비 설정 UI](images/high-contrast-settings.png)

고대비 테마 지원에 대한 자세한 내용은 [고대비 테마](https://docs.microsoft.com/windows/uwp/accessibility/high-contrast-themes)를 참조하세요.

### <a name="system-accent-color"></a>시스템 테마 컬러

시스템 고대비 테마 색 외에도 `SystemAccentColor` 키를 사용하는 특수한 색 리소스로 시스템 테마 컬러가 제공됩니다. 런타임에 이 리소스는 사용자가 Windows 개인 설정에서 테마 컬러로 지정한 색을 가져옵니다.

> [!NOTE]
> 시스템 색 리소스를 재정의할 수도 있지만, 특히 고대비 설정에서는 사용자의 색 선택을 적용하는 것이 가장 좋습니다.

### <a name="theme-dependent-brushes"></a>테마 종속 브러시

위 섹션에 표시된 색 리소스는 시스템 테마 리소스 사전에서 [SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) 리소스의 [Color](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 속성을 설정하는 데 사용됩니다. 브러시 리소스를 사용하여 XAML 요소에 색을 적용합니다. 브러시 리소스에 대한 키는 다음 이름 지정 형식을 따릅니다. `SystemControl[Simple HighContrast name][Simple light/dark name]Brush`. 정의합니다(예: `SystemControlBackgroundAltHighBrush`).

런타임에 이 브러시에 대한 색 값이 결정되는 방식을 알아보겠습니다. "밝게" 및 "어둡게" 리소스 사전에서 이 브러시는 다음과 같이 정의됩니다.

`<SolidColorBrush x:Key="SystemControlBackgroundAltHighBrush" Color="{StaticResource SystemAltHighColor}"/>`

"고대비" 리소스 사전에서 이 브러시는 다음과 같이 정의됩니다.

`<SolidColorBrush x:Key="SystemControlBackgroundAltHighBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>`

이 브러시가 XAML 요소에 적용되면 이 표에 표시된 대로, 런타임에 현재 테마에 따라 색이 결정됩니다.

| 테마        | 간단한 색 이름 | 색 리소스             | 런타임 값                                              |
|--------------|-------------------|----------------------------|------------------------------------------------------------|
| Light        | AltHigh           | SystemAltHighColor         | \#FFFFFFFF                                                 |
| Dark         | AltHigh           | SystemAltHighColor         | \#FF000000                                                 |
| 고대비 | 배경        | SystemColorButtonFaceColor | 단추 배경에 대한 설정에 지정된 색입니다. |

`SystemControl[Simple HighContrast name][Simple light/dark name]Brush` 이름 지정 체계를 사용하여 고유한 XAML 요소에 적용할 브러시를 확인할 수 있습니다.

<!--
For many examples of how the brushes are used in the XAML control templates, see the [Default control styles and templates](default-control-styles-and-templates.md).
-->

> [!NOTE]
> \[‘단순한 고대비 이름’\]\[’단순한 밝게/어둡게 이름’\]의 모든 조합이 브러시 리소스로 제공되는 것은 아닙니다.  

## <a name="the-xaml-type-ramp"></a>XAML 유형 램프

themeresources.xaml 파일은 UI의 텍스트 컨테이너, 특히 [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 또는 [RichTextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)에 적용할 수 있는 [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style)을 정의하는 몇 가지 리소스를 정의합니다. 이것은 기본 암시적 스타일이 아닙니다. [글꼴에 대한 지침](../style/typography.md)에 지정된 *Windows 유형 램프*와 일치하는 XAML UI 정의를 좀 더 쉽게 만들 수 있도록 하기 위해 제공됩니다.

이런 스타일은 전체 텍스트 컨테이너에 적용하려는 텍스트 특성과 관련됩니다. 텍스트의 일부에만 스타일을 적용하려는 경우에는 컨테이너 내부 텍스트 요소에, 예를 들어 [TextBlock.Inlines](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.inlines)의 [Run](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Run) 또는 [RichTextBlock.Blocks](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblock.blocks)의 [Paragraph](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Paragraph)에 대해 특성을 설정합니다.

스타일은 [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)에 적용될 때 다음과 같이 표시됩니다.

![텍스트 블록 스타일](../style/images/type/text-block-type-ramp.svg)

```XAML
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>
```

앱에서 UWP 유형 램프를 사용하는 방법에 대한 자세한 내용은 [UWP 앱의 입력 체계](../style/typography.md)를 참조하세요.

### <a name="basetextblockstyle"></a>BaseTextBlockStyle

**TargetType**: [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

다른 모든 [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 컨테이너 스타일에 대한 일반 속성을 제공합니다.

```XAML
<!-- Usage -->
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="BaseTextBlockStyle" TargetType="TextBlock">
    <Setter Property="FontFamily" Value="Segoe UI"/>
    <Setter Property="FontWeight" Value="SemiBold"/>
    <Setter Property="FontSize" Value="14"/>
    <Setter Property="TextTrimming" Value="None"/>
    <Setter Property="TextWrapping" Value="Wrap"/>
    <Setter Property="LineStackingStrategy" Value="MaxHeight"/>
    <Setter Property="TextLineBounds" Value="Full"/>
</Style>
```

### <a name="headertextblockstyle"></a>HeaderTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="HeaderTextBlockStyle" TargetType="TextBlock"
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="46"/>
    <Setter Property="FontWeight" Value="Light"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="subheadertextblockstyle"></a>SubheaderTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="SubheaderTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="34"/>
    <Setter Property="FontWeight" Value="Light"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="titletextblockstyle"></a>TitleTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="TitleTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="SemiLight"/>
    <Setter Property="FontSize" Value="24"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="subtitletextblockstyle"></a>SubtitleTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="SubtitleTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
    <Setter Property="FontSize" Value="20"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="bodytextblockstyle"></a>BodyTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="BodyTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
    <Setter Property="FontSize" Value="14"/>
</Style>
```

### <a name="captiontextblockstyle"></a>CaptionTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="CaptionTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="12"/>
    <Setter Property="FontWeight" Value="Normal"/>
</Style>
```

### <a name="baserichtextblockstyle"></a>BaseRichTextBlockStyle

**TargetType**: [RichTextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)

다른 모든 [RichTextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) 컨테이너 스타일에 대한 일반 속성을 제공합니다.

```XAML
<!-- Usage -->
<RichTextBlock Style="{StaticResource BaseRichTextBlockStyle}">
    <Paragraph>Rich text.</Paragraph>
</RichTextBlock>

<!-- Style definition -->
<Style x:Key="BaseRichTextBlockStyle" TargetType="RichTextBlock">
    <Setter Property="FontFamily" Value="Segoe UI"/>
    <Setter Property="FontWeight" Value="SemiBold"/>
    <Setter Property="FontSize" Value="14"/>
    <Setter Property="TextTrimming" Value="None"/>
    <Setter Property="TextWrapping" Value="Wrap"/>
    <Setter Property="LineStackingStrategy" Value="MaxHeight"/>
    <Setter Property="TextLineBounds" Value="Full"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="bodyrichtextblockstyle"></a>BodyRichTextBlockStyle

```XAML
<!-- Usage -->
<RichTextBlock Style="{StaticResource BodyRichTextBlockStyle}">
    <Paragraph>Rich text.</Paragraph>
</RichTextBlock>

<!-- Style definition -->
<Style x:Key="BodyRichTextBlockStyle" TargetType="RichTextBlock" BasedOn="{StaticResource BaseRichTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
</Style>
```

**참고**:  [RichTextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) 스타일에는 [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)에 있는 텍스트 램프 스타일 중 일부가 없습니다. **RichTextBlock**의 블록 기반 문서 개체 모델을 사용하면 개별 텍스트 요소의 특성을 더 쉽게 설정할 수 있기 때문입니다. 또한 XAML 콘텐츠 속성을 사용하여 [TextBlock.Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text)를 설정하면 스타일 지정할 텍스트 요소가 없어서 컨테이너의 스타일을 지정해야 하는 상황이 발생합니다. **RichTextBlock**의 경우에는 문제가 되지 않습니다. 해당 텍스트 콘텐츠가 페이지 머리글, 페이지 하위 머리글, 유사한 텍스트 램프 정의에 대해 XAML 스타일을 적용하는 [Paragraph](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Paragraph) 같은 특정 텍스트 요소에 항상 있어야 하기 때문입니다.

## <a name="miscellaneous-named-styles"></a>기타 명명된 스타일

기본 암시적 스타일과는 다르게 [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) 스타일을 지정하기 위해 적용할 수 있는 추가적인 키 입력 [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) 정의 집합이 있습니다.

### <a name="textblockbuttonstyle"></a>TextBlockButtonStyle

**TargetType**: [ButtonBase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase)

작업을 수행하기 위해 클릭할 수 있는 텍스트를 표시해야 할 경우 이 스타일을 [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)에 적용합니다. 텍스트는 대화형으로 구분하기 위해 현재 테마 컬러를 사용하여 스타일이 지정되며, 텍스트에 잘 맞는 초점 사각형을 포함합니다. [HyperlinkButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton)의 암시적 스타일과 달리 **TextBlockButtonStyle**에서는 텍스트에 밑줄을 표시하지 않습니다.

이 템플릿은 또한 표현된 텍스트가 **SystemControlHyperlinkBaseMediumBrush**("PointerOver" 상태의 경우), **SystemControlHighlightBaseMediumLowBrush**("Pressed" 상태의 경우) 및 **SystemControlDisabledBaseLowBrush**("Disabled" 상태의 경우)를 사용하도록 스타일 지정합니다.

다음은 **TextBlockButtonStyle** 리소스가 적용된 [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)입니다.

```XAML
<Button Content="Clickable text" Style="{StaticResource TextBlockButtonStyle}"
        Click="Button_Click"/>
```

다음과 같이 표시됩니다.

![텍스트처럼 스타일이 지정된 단추](images/styles-textblock-button-style.png)

### <a name="navigationbackbuttonnormalstyle"></a>NavigationBackButtonNormalStyle

**TargetType**: [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)

이 [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style)은 탐색 앱을 위한 탐색 뒤로 단추가 될 수 있는 [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)용 전체 템플릿을 제공합니다. 기본 크기는 40 x 40 픽셀입니다. 스타일을 조정하기 위해 **Button**에 대해 [Height](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height), [Width](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), [FontSize](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.fontsize) 및 다른 속성을 명시적으로 설정하거나, [BasedOn](https://docs.microsoft.com/uwp/api/windows.ui.xaml.style.basedon)을 사용하여 파생 스타일을 만들 수 있습니다.

다음은 **NavigationBackButtonNormalStyle** 리소스가 적용된 [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)입니다.

```XAML
<Button Style="{StaticResource NavigationBackButtonNormalStyle}" />
```

다음과 같이 표시됩니다.

![뒤로 단추로 스타일이 지정된 단추](images/styles-back-button-normal.png)

### <a name="navigationbackbuttonsmallstyle"></a>NavigationBackButtonSmallStyle

**TargetType**: [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)

이 [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style)은 탐색 앱을 위한 탐색 뒤로 단추가 될 수 있는 [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)용 전체 템플릿을 제공합니다. **NavigationBackButtonNormalStyle**과 비슷하지만, 치수가 30x30 픽셀입니다.

다음은 **NavigationBackButtonSmallStyle** 리소스가 적용된 [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)입니다.

```XAML
<Button Style="{StaticResource NavigationBackButtonSmallStyle}" />
```

## <a name="troubleshooting-theme-resources"></a>테마 리소스 문제 해결

[테마 리소스 사용 지침](#guidelines-for-custom-theme-resources)을 따르지 않으면 앱에서 테마와 관련된 예기치 않은 동작이 발생할 수 있습니다.

예를 들어 밝은 테마 플라이아웃을 열면 어두운 테마 앱 부분이 마치 원래 밝은 테마였던 것처럼 변경됩니다. 또는 밝은 테마 페이지로 이동했다가 되돌아오면 원래 어두운 테마 페이지(또는 일부)가 맑은 테마인 것처럼 보입니다.

일반적으로 이러한 유형의 문제는 고대비 시나리오를 제공하기 위해 "기본" 테마 및 "고대비" 테마를 제공한 다음 앱의 다른 부분에서 "밝게" 및 "어둡게" 테마를 모두 사용하는 경우에 발생합니다.

예를 들어 다음 테마 사전 정의를 고려해보세요.

```XAML
<!-- DO NOT USE. THIS XAML DEMONSTRATES AN ERROR. -->
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

간단히 말해서 이 정의는 올바른 것처럼 보입니다. 고대비 상태에서는 `myBrush`가 가리키는 색으로 변경하지만, 고대비 상태가 아닐 때는 [{ThemeResource} 태그 확장](../../xaml-platform/themeresource-markup-extension.md)을 사용하여 `myBrush`가 테마에 적절한 색을 가리키도록 합니다. 앱에서 해당 시각적 트리 내의 요소에 [FrameworkElement.RequestedTheme](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.requestedtheme)가 설정되지 않은 경우 일반적으로 예상 대로 작동합니다. 그러나 시각적 트리의 다른 부분에 대해 테마를 다시 시작하려고 하는 즉시, 앱에 문제가 발생합니다.

이 문제는 다른 대부분의 XAML 형식과 달리 브러시는 공유 리소스이기 때문입니다. XAML 하위 트리에 동일한 브러시 리소스를 참조하지만 테마는 다른 2개의 요소가 있는 경우 프레임워크가 각 하위 트리를 따라 이동하면서 해당 [{ThemeResource} 태그 확장](../../xaml-platform/themeresource-markup-extension.md) 식을 업데이트할 때 공유 브러시 리소스의 변경 내용이 의도한 결과와 달리 다른 하위 트리에도 반영됩니다.

이 문제를 해결하려면 "고대비" 외에, "밝게" 및 "어둡게" 둘 다에 대해 "기본" 사전을 별도의 테마 사전으로 바꾸세요.

```XAML
<!-- DO NOT USE. THIS XAML DEMONSTRATES AN ERROR. -->
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Light">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="Dark">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

그러나 이러한 리소스가 [Foreground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.foreground)와 같은 상속된 속성에서 참조되는 경우에 여전히 문제가 발생합니다. 사용자 지정 컨트롤 템플릿은 [{ThemeResource} 태그 확장](../../xaml-platform/themeresource-markup-extension.md)을 사용하여 요소의 전경색을 지정할 수 있지만 프레임워크는 상속된 값을 자식 요소에 전파할 때 {ThemeResource} 태그 확장 식에서 확인된 리소스에 대한 직접 참조를 제공합니다. 이로 인해 프레임워크가 컨트롤의 시각적 트리를 따라 테마 변경 내용을 처리할 때 문제가 발생합니다. 프레임워크는 {ThemeResource} 태그 확장 식을 다시 평가하여 새 브러시 리소스를 가져오지만, 아직 컨트롤의 자식으로 이 참조를 전파하지는 않습니다. 이 작업은 나중에, 예를 들면 다음 측정 단계 도중에 발생합니다.

결과적으로 프레임워크는 테마 변경에 대한 응답으로 컨트롤의 시각적 트리를 따라 자식 요소로 이동한 후 해당 요소에 설정된 [{ThemeResource} 태그 확장](../../xaml-platform/themeresource-markup-extension.md) 식 또는 속성에 대해 설정된 개체를 업데이트합니다. 여기에서 문제가 발생합니다. 프레임워크는 브러시 리소스를 따라 이동하며, {ThemeResource} 태그 확장을 사용하여 색을 지정하므로 다시 평가됩니다.

이제 프레임워크는 사전의 리소스가 다른 사전에서 설정된 색을 포함하게 되므로 테마 사전이 혼용된 것처럼 보입니다.

이 문제를 해결하려면 [{ThemeResource} 태그 확장](../../xaml-platform/staticresource-markup-extension.md) 대신 [{StaticResource} 태그 확장](../../xaml-platform/themeresource-markup-extension.md)을 사용하세요. 적용된 지침에 따라, 테마 사전 모양은 다음과 같습니다.

```XAML
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Light">
      <SolidColorBrush x:Key="myBrush" Color="{StaticResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="Dark">
      <SolidColorBrush x:Key="myBrush" Color="{StaticResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

[{ThemeResource} 태그 확장](../../xaml-platform/themeresource-markup-extension.md)은 "고대비" 사전에서 여전히 [{StaticResource} 태그 확장](../../xaml-platform/staticresource-markup-extension.md) 대신 사용됩니다. 이러한 상황은 지침 앞부분에 제공된 예외에 해당합니다. "고대비" 테마에 사용되는 대부분의 브러시 값은 시스템에서 포괄적으로 제어되는 색 선택을 사용하지만 XAML에는 특수하게 명명된 리소스(이름 맨 앞에 ‘System Color’가 붙은 리소스)로 노출됩니다. 시스템은 사용자가 접근성 센터를 통해 고대비 설정에 사용하는 특정 색을 설정할 수 있도록 합니다. 이러한 색 선택 항목은 특별히 명명된 리소스에 적용됩니다. 또한 XAML 프레임워크는 동일한 테마 변경 이벤트를 사용하여 시스템 수준에서 변경된 것이 확인될 때 이러한 브러시를 업데이트합니다. 이 때문에 여기에서 {ThemeResource} 태그 확장이 사용됩니다.
