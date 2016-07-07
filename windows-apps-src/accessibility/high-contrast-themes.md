---
author: Xansky
Description: "고대비 테마가 활성 상태일 때 UWP(유니버설 Windows 플랫폼) 앱을 사용하는 데 필요한 단계에 대해 설명합니다."
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: "고대비 테마"
label: High-contrast themes
template: detail.hbs
ms.sourcegitcommit: 50c37d71d3455fc2417d70f04e08a9daff2e881e
ms.openlocfilehash: 4201f5a0b08f1fc8d691218da0803ee04ab2c86a

---

# 고대비 테마  

고대비 테마가 활성 상태일 때 UWP(유니버설 Windows 플랫폼 앱을 사용할 수 있도록 하는 데 필요한 단계에 대해 설명합니다.

UWP 앱은 기본적으로 고대비 테마를 지원합니다. 사용자가 시스템에서 시스템 설정 또는 접근성 도구의 고대비 테마를 사용하도록 선택한 경우 이 프레임워크에서는 UI의 컨트롤 및 구성 요소에 대해 고대비 레이아웃 및 렌더링을 생성하는 색상 및 스타일 설정을 자동으로 사용합니다.

이 기본 기원은 기본 테마 및 템플릿 사용을 기반으로 합니다. 이러한 테마 및 템플릿은 시스템 색상을 리소스 정의로 참조하므로 시스템에서 고대비 모드를 사용하는 경우 리소스 원본이 자동으로 변경됩니다. 그러나 사용자 지정 템플릿, 테마 및 스타일을 컨트롤에 사용하는 경우 고대비에 대한 기본 제공 지원을 사용하도록 해야 합니다. 스타일 지정에 Microsoft Visual Studio용 XAML 디자이너를 사용하는 경우 디자이너에서는 기본 템플릿과 현저하게 다른 템플릿을 정의할 때마다 기본 테마와 함께 별도의 고대비 테마를 생성합니다. 별도의 테마 사전은 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 요소의 전용 속성인 [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.resourcedictionary.themedictionaries.aspx) 컬렉션으로 이동합니다.

테마 및 컨트롤 템플릿에 대한 자세한 내용은 [빠른 시작: 컨트롤 템플릿](https://msdn.microsoft.com/library/windows/apps/xaml/Hh465374)을 참조하세요. 특정 컨트롤에 대한 XAML 리소스 사전 및 테마를 확인하고 테마가 구성되는 방법 및 가능한 각 고대비 설정에 대해 유사하지만 다른 리소스를 참조하는 방법을 참조하면 매우 도움이 될 수 있습니다.

## 테마 사전

시스템 기본값에서 색을 변경해야 하거나 배경 이미지 등의 이미지를 장식으로 추가해야 하는 경우 앱에 대한 **ThemeDictionaries** 컬렉션을 만듭니다.

* 이 컬렉션이 없는 경우 적절한 연결을 만들어 시작합니다. App.xaml에서 **ThemeDictionaries** 컬렉션을 만듭니다.

``` xaml
 <Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">

            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">

            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources
```

* **HighContrast**는 사용할 수 있는 유일한 키 이름이 아닙니다. **HighContrastBlack**, **HighContrastWhite** 및 **HighContrastCustom**도 있습니다. 대부분의 경우 **HighContrast**만 있으면 됩니다.
* **Default**에서 필요한 [**Brush**](http://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.aspx)의 유형을 만듭니다. 일반적으로 **SolidColorBrush**입니다. 이 항목에 용도와 관련된 **x:Key** 이름을 제공합니다.<br/>
    `<SolidColorBrush x:Key="BrandedPageBackground" />`
* 이 항목에 원하는 **Color**를 할당합니다.<br/>
    `<SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />`
* 이 **Brush**를 **HighContrast**에 복사합니다.

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

* **Brush**의 색을 결정하고 **HighContrast**에서 수정합니다.

고대비 색을 결정하려면 관련 내용을 어느 정도 알아야 합니다. 위에서 만든 연결 덕분에 쉽게 업데이트할 수 있습니다.

## 고대비 색

설정 페이지를 사용하여 고대비로 전환할 수 있습니다. 기본적으로 4가지 고대비 테마가 있습니다. 사용자가 옵션을 선택하면 페이지에 앱의 모양을 보여 주는 미리 보기가 표시됩니다.

![고대비 설정](images/high-contrast-settings.png)<br/>
_고대비 설정_

 미리 보기에서 각 사각형을 클릭하여 값을 변경할 수 있습니다. 또한 각 사각형은 시스템 리소스에 직접 매핑됩니다.

![고대비 리소스](images/high-contrast-resources.png)<br/>
_고대비 리소스_

위에 나와 있는 이름의 접두사를 _SystemColor_로 지정하고 접미사를 _Color_로 지정하는 경우(예: **SystemColorWindowTextColor**) 이러한 항목이 사용자가 지정한 것과 일치하도록 동적으로 업데이트됩니다. 이에 따라 고대비에 대한 특정 색을 선택할 필요가 없습니다. 대신 색이 사용될 대상에 해당하는 시스템 리소스를 선택하면 됩니다. 위의 예에서는 페이지 배경색을 **SolidColorBrushBrandedPageBackground**로 지정했습니다. 이 항목은 배경에 사용되므로 고대비에서 **SystemColorWindowColor**에 매핑될 수 있습니다.

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

8가지 고대비 색의 색상표를 고수하는 경우 추가 고대비 **ResourceDictionaries**를 만들 필요가 없습니다. 이 제한된 색상표는 복잡한 시각적 상태를 나타낼 때 어려운 문제가 되는 경우가 많습니다. 흔히 고대비에서 영역에 테두리만 추가하면 상황을 명확하게 만드는 데 도움이 될 수 있습니다.

### 권장 사항 및 금지 사항

* 고대비 모드에서 조기에 자주 테스트를 수행합니다.
* 원하는 용도에 대한 명명된 색을 사용합니다.
* **ThemeDictionaries** 내에 **Color**, **Brush** 및 **Thickness**와 같은 기본 요소를 배치합니다. **Style** 요소와 같은 더 복잡한 리소스를 배치하지 않습니다. 다음 예는 원활하게 작동합니다.

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>

        <Style x:Key="MyButtonStyle" TargetType="Button">
            <Setter Property="Foreground" Value="{ThemeResource BrandedPageBackground}" />
        </Style>
    </ResourceDictionary>
</Application.Resources>

...

<Button Style="{StaticResource MyButtonStyle}" />
```

* 전경 UI 요소에 고대비 전경색을 사용합니다.
* 고대비 색을 정의된 색 쌍과 함께 사용합니다. 예를 들어 특히 전경/배경 상황에서 **BUTTONTEXT**를 항상 **BUTTONFACE**와 함께 사용합니다.
* 필요한 14:1 대비 비율이 충족되도록 특정 UI 요소에 권장되는 고대비 색 쌍을 사용합니다.
* 고대비 색 쌍을 나누거나 임의로 고대비 색을 혼합하여 사용하지 않습니다. 이렇게 하지 않으면 미리 설치된 고대비 테마 중 하나 이상에 대해 보이지 않는 UI가 만들어집니다.
* **ThemeDictionaries** 컬렉션 외부에 **Brush** 개체를 배치하지 않습니다.
* **StaticResource**를 사용하여 **ThemeDictionaries** 컬렉션의 리소스를 참조하지 않습니다. 이렇게 하지 않으면 앱이 실행되는 동안 사용자가 테마를 변경할 때까지는 작동하는 것처럼 보입니다. 대신 **ThemeResource**를 사용합니다.
* 하드 코드된 색 값을 사용하지 않습니다.
* 좋아한다는 이유만으로 색을 사용하지 않습니다.

자세한 내용은 [XAML 테마 리소스](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/xaml-theme-resources)를 참조하세요.

## 테두리를 사용하는 경우
고대비 모드에서 항목에 인식 가능한 경계 모양을 유지해야 하는 UI 요소에 테두리를 추가합니다. 탐색의 콘텐츠 영역, 작업 및 콘텐츠를 구분하려면 테두리를 사용합니다.

![페이지의 나머지 부분과 분리된 탐색 창](images/high-contrast-actions-content.png)<br/>
_페이지의 나머지 부분과 분리된 탐색 창_

UI 요소에 기본적으로 테두리나 배경이 _없는_ 경우 고대비 모드에서 테두리나 배경을 기본 상태에 추가하지 마세요.

UI 요소에 기본적으로 테두리가 _있는_ 경우에는 고대비 모드에서 테두리를 유지합니다.

겹치는 색이나 인접한 색은 서로 구별 가능해야 하지만 14:1의 색상 대비 비율을 반드시 충족할 필요는 없습니다. 그러나 이러한 유형의 시나리오에는 3:1 대비 비율을 사용하는 것이 가장 좋습니다.

고대비 배경색이 겹치는 UI 요소를 구분하는 데 사용되는 경우 이러한 요소 간의 대비를 보장하는 유일한 방법은 테두리를 적용하는 것입니다.

## 고대비 테마가 사용되는 경우 탐색  
[**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) 클래스의 멤버를 사용하여 고대비 테마의 현재 설정을 검색할 수 있습니다. [**HighContrast**](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.accessibilitysettings.highcontrast) 속성은 고대비 테마가 현재 선택되어 있는지 여부를 확인합니다. **HighContrast**가 **true**로 설정되어 있으며 다음 단계에서 [**HighContrastScheme**](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.accessibilitysettings.highcontrastscheme) 속성의 값을 확인하여 사용되는 고대비 테마의 이름을 가져옵니다. "고대비 흰색" 및 "고대비 검정"은 일반적으로 코드에서 응답해야 하는 **HighContrastScheme**에 대한 값입니다. XAML 정의 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 키에는 공백이 없으므로 리소스 사전에서 이러한 테마에 해당되는 키는 각각 "HighContrastWhite" 및 "HighContrastBlack"입니다. 값이 몇 가지 다른 문자열인 경우 기본 고대비 테마에 대한 폴백 논리도 있어야 합니다. [XAML 고대비 샘플](http://go.microsoft.com/fwlink/p/?linkid=254993)에서는 이에 대한 논리를 보여 줍니다.

> [!NOTE]
> 앱이 초기화되어 이미 콘텐츠를 표시하고 있는 범위에서 [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) 생성기를 호출해야 합니다.

앱은 실행하는 동안 고대비 리소스 값을 사용하는 것으로 전환할 수 있습니다. 이 작업은 스타일 또는 템플릿 XAML에서 [{ThemeResource}](https://msdn.microsoft.com/library/windows/apps/Mt185591)을 사용하여 리소스를 요청한 경우에만 작동합니다. 기본 테마(generic.xaml)는 모두 이 {ThemeResource} 태그 확장 기술을 사용하므로, 기본 컨트롤 테마를 사용하는 경우 이 동작을 구현할 수 있습니다. 이는 사용자 지정 컨트롤 또는 사용자 지정 컨트롤 스타일에서 수행할 수 있습니다. 단, 사용자 지정 템플릿 및 스타일에서도 이 {ThemeResource} 태그 확장 리소스 기술을 사용해야 합니다.

## 관련 항목  
* [접근성](accessibility.md)
* [UI 대비 및 설정 샘플](http://go.microsoft.com/fwlink/p/?linkid=231539)
* [XAML 접근성 샘플](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [XAML 고대비 샘플](http://go.microsoft.com/fwlink/p/?linkid=254993)
* [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)



<!--HONumber=Jun16_HO4-->


