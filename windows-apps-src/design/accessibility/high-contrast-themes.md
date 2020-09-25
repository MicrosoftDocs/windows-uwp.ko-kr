---
description: 고대비 테마가 활성화 되어 있을 때 Windows 앱을 사용할 수 있는지 확인 하는 데 필요한 단계를 설명 합니다.
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: 고대비 테마
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b5e9b823ca335370f6cde22ef6417a6823851c18
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219876"
---
# <a name="high-contrast-themes"></a>고대비 테마  

Windows는 사용자가 사용 하도록 선택할 수 있는 OS 및 앱에 대해 고대비 테마를 지원 합니다. 고대비 테마는 인터페이스를 더 쉽게 볼 수 있도록 하는 대비 색의 작은 색상표를 사용 합니다.

![밝은 테마와 고대비 검정색 테마에 표시 되는 계산기입니다.](images/high-contrast-calculators.png)

*밝은 테마와 고대비 검정색 테마에 표시 되는 계산기입니다.*

고대비를 > 하는 *설정 > 간편한 액세스*를 사용 하 여 고대비 테마로 전환할 수 있습니다.

> [!NOTE]
> 고대비 테마는 고대비를 사용 하는 것으로 간주 되지 않는 매우 큰 색상표를 허용 하는 밝은 테마와 어두운 테마를 혼동 하지 마세요. 더 밝은 테마 및 어두운 테마는 [색](../style/color.md)에 대 한 문서를 참조 하세요.

일반 컨트롤은 전체 고대비 지원을 무료로 제공 하지만 UI를 사용자 지정 하는 동안 주의 해야 합니다. 가장 일반적인 고대비 버그는 컨트롤의 색을 인라인으로 하드 코딩 하 여 발생 합니다.

```xaml
<!-- Don't do this! -->
<Grid Background="#E6E6E6">

<!-- Instead, create BrandedPageBackgroundBrush and do this. -->
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

`#E6E6E6`첫 번째 예제에서 색을 인라인으로 설정 하면 그리드는 모든 테마에서 해당 배경색을 유지 합니다. 사용자가 고대비 블랙 테마로 전환 하면 앱이 검정색 배경으로 표시 될 것입니다. `#E6E6E6`는 거의 흰색 이므로 일부 사용자가 앱과 상호 작용 하지 못할 수 있습니다.

두 번째 예제에서는 [**{Themeresource} 태그 확장**](../../xaml-platform/themeresource-markup-extension.md) 을 사용 하 여 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 요소의 전용 속성인 [**ThemeDictionaries**](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries) collection의 색을 참조 합니다. **ThemeDictionaries** 를 사용 하면 XAML에서 사용자의 현재 테마에 따라 색을 자동으로 바꿀 수 있습니다.

## <a name="theme-dictionaries"></a>테마 사전

시스템 기본값에서 색을 변경 해야 하는 경우 앱에 대 한 ThemeDictionaries 컬렉션을 만듭니다.

1. 아직 존재 하지 않는 경우 적절 한 통로를 만들어 시작 합니다. App.xaml에서 **기본** 및 **system.windows.forms.systeminformation.highcontrast** 를 포함 하 여 **ThemeDictionaries** 컬렉션을 최소한으로 만듭니다.
2. **기본적**으로 필요한 [브러시](/uwp/api/Windows.UI.Xaml.Media.Brush) 유형 (일반적으로 **system.windows.media.solidcolorbrush>**)을 만듭니다. 사용 되는 것과 관련 된 *x:Key* 이름을 지정 합니다.
3. 원하는 **색** 을 할당 합니다.
4. **System.windows.forms.systeminformation.highcontrast**에 해당 **브러시** 를 복사 합니다.

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

마지막 단계는 고대비에서 사용할 색을 결정 하는 것입니다 .이에 대해서는 다음 섹션에서 설명 합니다.

> [!NOTE]
> **System.windows.forms.systeminformation.highcontrast** 은 유일 하 게 사용할 수 있는 키 이름입니다. **HighContrastBlack**, **HighContrastWhite**및 **HighContrastCustom**도 있습니다. 대부분의 경우 **system.windows.forms.systeminformation.highcontrast** 가 필요 합니다.

## <a name="high-contrast-colors"></a>고대비 색

*설정 > 접근성 > 고대비* 페이지에는 기본적으로 4 개의 고대비 테마가 있습니다. 


![고대비 설정](images/high-contrast-settings.png)  

*사용자가 옵션을 선택 하면 페이지에 미리 보기가 표시 됩니다.*  

![고대비 리소스](images/high-contrast-resources.png)  

*미리 보기의 모든 색 견본을 클릭 하 여 해당 값을 변경할 수 있습니다. 모든 견본은 XAML 색 리소스에 직접 매핑됩니다.*  

각 **systemcolor * 색** 리소스는 사용자가 고대비 테마를 전환할 때 색을 자동으로 업데이트 하는 변수입니다. 다음은 각 리소스를 사용 하는 위치 및 시기에 대 한 지침입니다.

리소스 | 사용량 |
|--------|-------|
**SystemColorWindowTextColor** | 본문 복사, 머리글, 목록 상호 작용할 수 없는 모든 텍스트 |
| **SystemColorHotlightColor** | 하이퍼링크 |
| **SystemColorGrayTextColor** | 사용 안 함 UI |
| **SystemColorHighlightTextColor** | 진행 중이거나 선택 되었거나 현재 상호 작용 중인 텍스트 또는 UI의 전경색입니다. |
| **SystemColorHighlightColor** | 진행 중이거나 선택 되었거나 현재 상호 작용 중인 텍스트 또는 UI의 배경색입니다. |
| **SystemColorButtonTextColor** | 단추의 전경색 ( 상호 작용할 수 있는 모든 UI |
| **SystemColorButtonFaceColor** | 단추의 배경색 상호 작용할 수 있는 모든 UI |
| **SystemColorWindowColor** | 페이지, 창, 팝업 및 막대의 배경 |

기존 앱, 시작 또는 공용 컨트롤을 확인 하 여 다른 사용자가 자신의 자체와 유사한 고대비 디자인 문제를 해결 하는 방법을 확인 하는 것이 유용한 경우가 많습니다.

**해야 할 일**

* 가능 하면 배경/전경 쌍을 고려 합니다.
* 앱이 실행 되는 동안 모든 4 개의 고대비 테마에서 테스트 합니다. 사용자는 테마를 전환할 때 앱을 다시 시작 하지 않아도 됩니다.
* 일관 됩니다.

**안 함**

* **System.windows.forms.systeminformation.highcontrast** 테마에서 색을 하드 코딩 합니다. **systemcolor * 색** 리소스를 사용 합니다.
* 미관에 대 한 색 리소스를 선택 합니다. 테마를 사용 하 여 변경 합니다.
* 보조 이거나 힌트로 사용 되는 본문 복사에 **SystemColorGrayTextColor** 를 사용 하지 마세요.


이전 예제를 계속 하려면 **BrandedPageBackgroundBrush**에 대 한 리소스를 선택 해야 합니다. 이 이름은 배경에 사용 됨을 나타내므로 **Systemcolorwindowcolor** 를 선택 하는 것이 좋습니다.

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

이제 앱에서 나중에 배경을 설정할 수 있습니다.

```xaml
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

BrandedPageBackgroundBrush을 참조 하기 위해 두 번 사용 되는 것은 **Systemcolorwindowcolor** 를 두 번 사용 **BrandedPageBackgroundBrush**하는 것입니다. ** \{ \} ** 응용 프로그램을 런타임에 올바르게 테마에 맞게 입력 해야 합니다. 앱에서 기능을 테스트 하는 것이 좋습니다. 고대비 테마를 전환 하면 표 배경이 자동으로 업데이트 됩니다. 또한 서로 다른 고대비 테마 간에 전환할 때 업데이트 됩니다.

## <a name="when-to-use-borders"></a>테두리를 사용 하는 경우

페이지, 창, 팝업 및 막대는 모두 높은 대비의 배경에 대해 **Systemcolorwindowcolor** 를 사용 해야 합니다. UI에서 중요 한 경계를 유지 하기 위해 필요한 경우 고대비 전용 테두리를 추가 합니다.

![페이지의 나머지 부분에서 분리 된 탐색 창](images/high-contrast-actions-content.png)  

*탐색 창과 페이지는 모두 고대비에서 동일한 배경색을 공유 합니다. 고대비 전용 테두리가 매우 중요 합니다.*


## <a name="list-items"></a>항목 나열

반면, [ListView](/uwp/api/windows.ui.xaml.controls.listview) 의 항목은 가리킨 다음, 누르거나, 선택 하는 경우 **SystemColorHighlightColor** 로 설정 된 배경에 있습니다. 복합 목록 항목에는 일반적으로 항목을 가리킨 다음 누르거나 선택할 때 목록 항목의 내용이 색을 반전 하지 못하는 버그가 있습니다. 이렇게 하면 항목을 읽을 수 없습니다.

![밝은 테마의 간단한 목록과 고대비 검정 테마](images/high-contrast-list1.png)

*밝은 테마의 간단한 목록 (왼쪽) 및 고대비 검정 테마 (오른쪽)입니다. 두 번째 항목이 선택 됩니다. 텍스트 색이 고대비에서 반전 되는 방식을 확인 합니다.*


### <a name="list-items-with-colored-text"></a>색이 지정 된 텍스트가 있는 목록 항목

한 가지 이유는 ListView의 [DataTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)에서 TextBlock을 설정 하는 것입니다. 일반적으로 비주얼 계층 구조를 설정 하기 위해 수행 됩니다. 전경 속성은 [ListViewItem](/uwp/api/windows.ui.xaml.controls.listviewitem)에 대해 설정 되 고, DataTemplate의 textblock는 항목을 가리킴, 누르거나, 선택할 때 올바른 전경색을 상속 합니다. 그러나 전경을 설정 하면 상속이 중단 됩니다.

![밝은 테마의 복합 목록과 고대비 검정 테마](images/high-contrast-list2.png)

*밝은 테마의 복합 목록 (왼쪽) 및 고대비 검정 테마 (오른쪽)입니다. 고대비에서 선택한 항목의 두 번째 줄을 반전 하지 못했습니다.*  

**ThemeDictionaries** collection에 있는 스타일을 통해 전경을 조건부로 설정 하 여이 문제를 해결할 수 있습니다. **System.windows.forms.systeminformation.highcontrast**의 **SecondaryBodyTextBlockStyle** 에서 **전경을** 설정 하지 않았으므로 색이 올바르게 반전 됩니다.

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

[**AccessibilitySettings**](/uwp/api/Windows.UI.ViewManagement.AccessibilitySettings) 클래스의 멤버를 사용 하 여 현재 테마가 고대비 테마 인지를 프로그래밍 방식으로 확인할 수 있습니다.

> [!NOTE]
> 앱이 초기화 되 고 이미 콘텐츠를 표시 하는 범위에서 **AccessibilitySettings** 생성자를 호출 해야 합니다.

## <a name="related-topics"></a>관련 항목  
* [접근성](accessibility.md)
* [UI 대비 및 설정 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20high%20contrast%20style%20sample%20(Windows%208))
* [XAML 접근성 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
* [XAML 고대비 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20high%20contrast%20style%20sample%20(Windows%208))
* [**AccessibilitySettings**](/uwp/api/Windows.UI.ViewManagement.AccessibilitySettings)
