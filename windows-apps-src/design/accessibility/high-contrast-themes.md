---
author: Xansky
description: 고대비 테마가 활성 상태일 때 UWP(유니버설 Windows 플랫폼) 앱을 사용하는 데 필요한 단계에 대해 설명합니다.
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: 고대비 테마
template: detail.hbs
ms.author: mhopkins
ms.date: 09/28/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7cf8b634cfc7ba66cde107150b54ecec76b2861d
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/16/2018
ms.locfileid: "6995796"
---
# <a name="high-contrast-themes"></a>고대비 테마  

Windows에서는 OS 및 앱에 대해 고대비 테마가 지원되며, 사용자가 사용하도록 설정할 수 있습니다. 고대비 테마는 인터페이스를 보기 쉽게 만드는 대비색의 작은 색상표를 사용합니다.

![밝은 테마 및 고대비 검정 테마로 표시된 계산기](images/high-contrast-calculators.png)

*밝은 테마 및 고대비 검정 테마로 표시된 계산기*

*설정 &gt; 접근성 &gt; 고대비*를 사용하여 고대비 테마로 전환할 수 있습니다.

> [!NOTE]
> 고대비로 간주되지 않는 훨씬 큰 색상표를 허용하는 밝은 테마 및 어두운 테마와 고대비 테마를 혼동하지 마세요. 밝은 테마 및 어두운 테마를 더 많이 보려면 [색](../style/color.md)과 관련된 문서를 참조하세요.

공용 컨트롤에는 전체 고대비 지원이 무료로 제공되지만 UI를 사용자 지정하는 동안 주의해야 합니다. 가장 일반적인 고대비 버그는 컨트롤에 색을 인라인으로 하드 코딩하여 발생합니다.

```xaml
<!-- Don't do this! -->
<Grid Background="#E6E6E6">

<!-- Instead, create BrandedPageBackgroundBrush and do this. -->
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

첫 번째 예제에서 인라인으로 `#E6E6E6` 색을 설정하면 모든 테마에서 그리드에 해당 배경색이 유지됩니다. 사용자가 고대비 검정 테마로 전환할 경우 앱에 검은색 배경이 표시될 것으로 기대합니다. `#E6E6E6`은 거의 흰색이므로 일부 사용자는 앱을 조작하지 못할 수도 있습니다.

두 번째 예제에서는 [**{ThemeResource} 태그 확장**](../../xaml-platform/themeresource-markup-extension.md)을 사용하여 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 요소의 전용 속성인 [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.resourcedictionary.themedictionaries.aspx) 컬렉션의 색을 참조합니다. **ThemeDictionaries**를 사용하면 XAML에서 사용자의 현재 테마에 따라 자동으로 색을 바꿀 수 있습니다.

## <a name="theme-dictionaries"></a>테마 사전

시스템 기본값에서 색을 변경해야 하는 경우 앱에 대한 ThemeDictionaries 컬렉션을 만듭니다.

1. 이 컬렉션이 없는 경우 적절한 연결을 만들어 시작합니다. App.xaml에서 최소한 **Default** 및 **HighContrast**를 포함하여 **ThemeDictionaries** 컬렉션을 만듭니다.
2. **Default**에서 필요한 유형의 [Brush](http://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.aspx)를 만듭니다. 일반적으로 **SolidColorBrush**입니다. 이 항목에 용도와 관련된 *x:Key* 이름을 지정합니다.
3. 이 항목에 원하는 **Color**를 할당합니다.
4. 이 **Brush**를 **HighContrast**에 복사합니다.

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called
            out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- Optional, Light is used in light theme.
            If included, Default will be used for Dark theme -->
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- HighContrast is used in all high contrast themes -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

마지막 단계는 고대비에서 사용할 색을 결정하는 것으로, 다음 섹션에서 설명합니다.

> [!NOTE]
> **HighContrast**는 사용할 수 있는 유일한 키 이름이 아닙니다. **HighContrastBlack**, **HighContrastWhite**, **HighContrastCustom**도 있습니다. 대부분의 경우 **HighContrast**만 있으면 됩니다.

## <a name="high-contrast-colors"></a>고대비 색

*설정 > 접근성 > 고대비* 페이지에는 기본값으로 4가지 고대비 테마가 있습니다. 


![고대비 설정](images/high-contrast-settings.png)  

*사용자가 옵션을 선택하면 페이지에 미리 보기가 표시됩니다.*  

![고대비 리소스](images/high-contrast-resources.png)  

*미리 보기에서 각 색 견본을 클릭하면 값을 변경할 수 있습니다. 또한 각 견본은 XAML 색 리소스에 직접 매핑됩니다.*  

각 **SytemColor*Color **리소스는 사용자가 고대비 테마를 전환할 때 자동으로 색을 업데이트하는 변수입니다. 다음은 각 리소스를 사용할 위치 및 시기에 대한 지침입니다.

리소스 | 사용 |
|--------|-------|
**SystemColorWindowTextColor** | 본문 복사, 제목, 목록, 조작할 수 없는 모든 텍스트 |
| **SystemColorHotlightColor** | 하이퍼링크 |
| **SystemColorGrayTextColor** | 사용할 수 없는 UI |
| **SystemColorHighlightTextColor** | 진행 중이거나 선택되었거나 현재 조작 중인 텍스트 또는 UI의 전경색 |
| **SystemColorHighlightColor** | 진행 중이거나 선택되었거나 현재 조작 중인 텍스트 또는 UI의 배경색 |
| **SystemColorButtonTextColor** | 단추, 조작할 수 있는 모든 UI의 전경색 |
| **SystemColorButtonFaceColor** | 단추, 조작할 수 있는 모든 UI의 배경색 |
| **SystemColorWindowColor** | 페이지, 창, 팝업, 막대의 배경 |

기존 앱, 시작 또는 공용 컨트롤을 살펴보고 다른 개발자가 유사한 고대비 디자인 문제를 어떻게 해결했는지 확인하면 도움이 되는 경우가 많습니다.

**권장 사항**

* 가능하면 배경/전경 쌍을 준수합니다.
* 앱이 실행되는 동안 4개의 고대비 테마에서 모두 테스트합니다. 사용자가 테마를 전환할 때 앱을 다시 시작할 필요가 없어야 합니다.
* 일관성을 유지합니다.

**금지 사항**

* **HighContrast** 테마에 색을 하드 코딩하지 않습니다. **SysemColor*Color**리소스를 사용합니다.
* 미학적으로 색 리소스를 선택하지 않습니다. 테마에 따라 변경된다는 것을 명심하세요.
* 2차적이거나 힌트 역할을 하는 본문 복사에는 **SystemColorGrayTextColor**를 사용하지 않습니다.


앞의 예제를 계속하려면 **BrandedPageBackgroundBrush**에 대한 리소스를 선택해야 합니다. 이름에서 배경색에 사용됨을 나타내므로 **SystemColorWindowColor**를 선택하는 것이 좋습니다.

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called
            out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- Optional, Light is used in light theme.
            If included, Default will be used for Dark theme -->
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- HighContrast is used in all high contrast themes -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

나중에 해당 앱에서 배경을 설정할 수 있습니다.

```xaml
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

참고: **\{ThemeResource\}** 를 두 번 사용하는 방법(한 번은 **SystemColorWindowColor** 참조에, 그리고 다시 **BrandedPageBackgroundBrush** 참조에). 앱이 런타임에 테마를 올바르게 적용하려면 둘 다 필요합니다. 이때 앱의 기능을 테스트하는 것이 좋습니다. 고대비 테마로 전환하면 그리드의 배경이 자동으로 업데이트됩니다. 다른 고대비 테마로 전환할 때도 업데이트됩니다.

## <a name="when-to-use-borders"></a>테두리를 사용하는 경우

페이지, 창, 팝업 및 막대는 모두 고대비에서 배경에 **SystemColorWindowColor**를 사용해야 합니다. UI에서 중요한 경계를 유지하기 위해 필요한 경우 고대비 전용 테두리를 추가합니다.

![페이지의 나머지 부분과 분리된 탐색 창](images/high-contrast-actions-content.png)  

*탐색 창과 페이지는 고대비에서 동일한 배경색을 공유합니다. 따라서 두 항목을 구분하는 고대비 전용 테두리가 필요합니다.*


## <a name="list-items"></a>목록 항목

고대비에서 [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 항목의 배경은 마우스로 가리키거나 누르거나 선택할 때 **SystemColorHighlightColor**로 설정됩니다. 복잡한 목록 항목에는 일반적으로 항목을 마우스로 가리키거나 누르거나 선택할 때 목록 항목 콘텐츠의 색이 반전되지 않는 버그가 있습니다. 이 때문에 항목을 읽을 수 없게 됩니다.

![밝은 테마 및 고대비 검정 테마의 간단한 목록](images/high-contrast-list1.png)

*밝은 테마(왼쪽) 및 고대비 검정 테마(오른쪽)의 간단한 목록. 두 번째 항목이 선택되어 있습니다. 고대비에서 해당 텍스트 색이 어떻게 반전되는지 확인합니다.*


### <a name="list-items-with-colored-text"></a>색이 지정된 텍스트가 있는 목록 항목

한 가지 원인은 ListView의 [DataTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx)에서 TextBlock.Foreground를 설정했기 때문입니다. 일반적으로 이 작업은 시각적 계층 구조를 설정하기 위해 수행됩니다. Foreground 속성은 [ListViewItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewitem.aspx)에서 설정되며, 항목을 마우스로 가리키거나 누르거나 선택할 때 DataTemplate의 TextBlocks가 올바른 Foreground 색을 상속합니다. 그러나 Foreground를 설정하면 상속이 끊어집니다.

![밝은 테마 및 고대비 검정 테마의 복잡한 목록](images/high-contrast-list2.png)

*밝은 테마(왼쪽) 및 고대비 검정 테마(오른쪽)의 복잡한 목록. 고대비에서 선택한 항목의 두 번째 줄은 반전되지 않습니다.*  

**ThemeDictionaries** 컬렉션에 있는 Style을 통해 조건부로 Foreground를 설정하면 이 문제를 해결할 수 있습니다. **HighContrast**에서 **SecondaryBodyTextBlockStyle**에 의해 **Foreground**가 설정되지 않았으므로 해당 색이 올바르게 반전됩니다.

```xaml
<!-- In App.xaml... -->
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}">
            <Setter Property="Foreground" Value="{StaticResource SystemControlForegroundBaseMediumBrush}" />
        </Style>
    </ResourceDictionary>

    <ResourceDictionary x:Key="Light">
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}">
            <Setter Property="Foreground" Value="{StaticResource SystemControlForegroundBaseMediumBrush}" />
        </Style>
    </ResourceDictionary>

    <ResourceDictionary x:Key="HighContrast">
        <!-- The Foreground Setter is omitted in HighContrast -->
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}" />
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>

<!-- Usage in your DataTemplate... -->
<DataTemplate>
    <StackPanel>
        <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Double line list item" />

        <!-- Note how ThemeResource is used to reference the Style -->
        <TextBlock Style="{ThemeResource SecondaryBodyTextBlockStyle}" Text="Second line of text" />
    </StackPanel>
</DataTemplate>
```


## <a name="detecting-high-contrast"></a>고대비 검색

[**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) 클래스의 멤버를 사용하여 프로그래밍 방식으로 현재 테마가 고대비 테마인지 확인할 수 있습니다.

> [!NOTE]
> 앱이 초기화되어 이미 콘텐츠를 표시하고 있는 범위에서 **AccessibilitySettings** 생성자를 호출해야 합니다.

## <a name="related-topics"></a>관련 항목  
* [접근성](accessibility.md)
* [UI 대비 및 설정 샘플](http://go.microsoft.com/fwlink/p/?linkid=231539)
* [XAML 접근성 샘플](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [XAML 고대비 샘플](http://go.microsoft.com/fwlink/p/?linkid=254993)
* [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)
