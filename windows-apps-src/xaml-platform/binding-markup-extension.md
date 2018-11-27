---
description: Binding 태그 확장은 XAML 로드 시 Binding 클래스의 인스턴스로 변환됩니다.
title: Binding 태그 확장
ms.assetid: 3BAFE7B5-AF33-487F-9AD5-BEAFD65D04C3
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b197ea668ec73711b7a9c63e516b4ec9a5f54d62
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7713989"
---
# <a name="binding-markup-extension"></a>{Binding} 태그 확장


**참고**새 바인딩 메커니즘은 성능 및 개발자 생산성에 최적화 된 Windows10 사용할 수 있습니다. [{x:Bind} 태그 확장](x-bind-markup-extension.md)을 참조하세요.

**참고**바인딩 **{Binding}** 를 사용 하 여 (및는 비교 **{x: Bind}** **{Binding}** 사이 대 한) 앱에서 데이터를 사용 하는 방법에 대 한 일반 정보에 대 한 [데이터 바인딩 심층 분석을](https://msdn.microsoft.com/library/windows/apps/mt210946)참조 하세요.

**{Binding}** 태그 확장은 코드 등 데이터 원본에서 가져온 값에는 컨트롤에 데이터 바인딩 속성에 사용 됩니다. **{Binding}** 태그 확장은 XAML 로드 시 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 클래스의 인스턴스로 변환됩니다. 이 바인딩 개체는 데이터 원본의 속성에서 값을 가져와서 컨트롤의 속성으로 푸시합니다. 필요한 경우 데이터 원본 속성의 값 변경을 관찰하고 해당 변경 내용에 따라 자체적으로 업데이트하도록 바인딩 개체를 구성할 수 있습니다. 또한 필요한 경우 제어 값 변경을 원본 속성에 다시 적용하도록 구성할 수도 있습니다. 데이터 바인딩의 대상인 속성은 종속성 속성이어야 합니다. 자세한 내용은 [종속성 속성 개요](dependency-properties-overview.md)를 참조하세요.

**{Binding}** 은 로컬 값과 동일한 종속성 속성 우선 순위를 가지며, 명령적 코드에서 로컬 값을 설정할 경우 태그에 설정된 **{Binding}** 의 효과가 제거됩니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용


``` syntax
<object property="{Binding}" .../>
-or-
<object property="{Binding propertyPath}" .../>
-or-
<object property="{Binding bindingProperties}" .../>
-or-
<object property="{Binding propertyPath, bindingProperties}" .../>
```

| 용어 | 설명 |
|------|-------------|
| *propertyPath* | 바인딩의 속성 경로를 지정하는 문자열. 자세한 내용은 [속성 경로](#property-path) 섹션을 참조하세요. |
| *bindingProperties* | *propName*=*value*\[, *propName*=*value*\]*<br/>이름/값 쌍 구문을 사용하여 지정된 하나 이상의 바인딩 속성. |
| *propName* | [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 개체에 설정할 속성의 문자열 이름. 예: "Converter" |
| *value* | 속성을 설정할 값. 인수 구문은 아래 [{Binding}으로 설정할 수 있는 Binding 클래스의 속성](#properties-of-the-binding-class-that-can-be-set-with-binding) 섹션의 속성에 따라 달라집니다. |

## <a name="property-path"></a>속성 경로

[**Path**](https://msdn.microsoft.com/library/windows/apps/br209830)는 소스 속성에 바인딩할 속성에 대해 설명합니다. 경로는 위치 매개 변수로, 매개 변수 이름을 명시적으로 사용하거나(`{Binding Path=EmployeeID}`), 이름 없는 첫 번째 매개 변수로 지정할 수 있습니다(`{Binding EmployeeID}`).

[**Path**](https://msdn.microsoft.com/library/windows/apps/br209830)의 형식은 속성 경로, 즉 사용자 지정 형식 또는 프레임워크 형식의 속성 또는 하위 속성으로 평가되는 문자열입니다. 형식은 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)일 수 있지만 반드시 그래야 하는 것은 아닙니다. 속성 경로의 단계는 점(.)으로 구분되므로 연속적인 하위 속성을 트래버스하기 위해 여러 구분 기호를 포함할 수 있습니다. 바인딩할 개체를 구현하는 데 사용되는 프로그래밍 언어에 관계없이 점 구분 기호를 사용합니다.

예를 들어 직원 개체의 이름 속성에 UI를 바인딩하려면 속성 경로가 "Employee.FirstName"일 수 있습니다. 직원의 부양가족을 포함하는 속성에 항목 컨트롤을 바인딩하는 경우 속성 경로는 "Employee.Dependents"일 수 있으며 항목 컨트롤의 항목 템플릿은 "Dependents" 항목의 표시를 담당합니다.

데이터 원본이 컬렉션인 경우 속성 패치가 컬렉션에서 위치 또는 색인별로 항목을 지정할 수 있습니다. 예를 들어 "Teams\[0\].Players"에서 리터럴 "\[\]"는 컬렉션의 첫 번째 항목을 지정하는 "0"을 묶습니다.

기존 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)에 대한 [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) 바인딩을 사용하는 경우 연결된 속성을 속성 경로의 일부로 사용할 수 있습니다. 연결된 속성 이름의 중간 점이 속성 경로의 단계로 간주되지 않도록 연결된 속성을 구분하려면 소유자가 한정한 연결된 속성 이름을 괄호로 묶습니다(예: `(AutomationProperties.Name)`).

속성 경로 중간 개체는 런타임 표현에서 [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) 개체로 저장되지만 대부분의 시나리오는 코드에서 **PropertyPath** 개체를 조작할 필요가 없습니다. 일반적으로 XAML을 사용하여 필요한 바인딩 정보를 지정할 수 있습니다.

속성 경로에 대한 문자열 구문, 애니메이션 기능 영역의 속성 경로 및 [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) 개체 구성에 대한 자세한 내용은 [속성 경로 구문](property-path-syntax.md)을 참조하세요.

## <a name="properties-of-the-binding-class-that-can-be-set-with-binding"></a>{Binding}으로 설정할 수 있는 Binding 클래스의 속성


태그 확장에서 설정될 수 있는 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)의 읽기/쓰기 속성이 여러 개이기 때문에 **{Binding}** 은 *bindingProperties* 자리 표시자 구문을 통해 설명됩니다. 쉼표로 구분된 *propName*=*value* 쌍으로 순서에 상관없이 속성을 설정할 수 있습니다. 일부 속성에는 형식 변환이 없는 형식이 필요하므로 해당 태그 확장이 **{Binding}** 에 중첩되어야 합니다.

| 속성 | 설명 |
|----------|-------------|
| [**경로**](https://msdn.microsoft.com/library/windows/apps/br209830) | 위의 [속성 경로](#property-path) 섹션을 참조하세요. |
| [**변환기**](https://msdn.microsoft.com/library/windows/apps/br209826) | 바인딩 엔진이 호출하는 변환기 개체를 지정합니다. 변환기는 [{StaticResource} 태그 확장](staticresource-markup-extension.md)을 사용하여 리소스 사전의 해당 개체에 대한 참조로 태그에 설정될 수 있습니다. |
| [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) | 변환기가 사용할 문화권을 지정합니다. ([**변환기**](https://msdn.microsoft.com/library/windows/apps/br209826)를 설정하는 경우) 문화권은 표준 기반 식별자로 설정됩니다. 자세한 내용은 [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880)를 참조하세요. |
| [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827) | 변환기 논리에서 사용될 수 있는 변환기 매개 변수를 지정합니다. [**변환기**](https://msdn.microsoft.com/library/windows/apps/br209826)를 설정하는 경우에는 대부분의 변환기가 전달된 값에서 변환을 위해 필요한 모든 정보를 얻는 간단한 논리를 사용하며 **ConverterParameter** 값이 필요하지 않습니다. **ConverterParameter** 매개 변수는 **ConverterParameter**에서 전달되는 내용을 수용하는 조건부 논리가 있는 보다 복잡한 변환기를 구현하기 위한 것입니다. 문자열이 아닌 값을 사용하는 변환기를 작성할 수도 있지만 일반적이지 않습니다. 자세한 내용은 **ConverterParameter**에서 설명을 참조하세요. |
| [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) | **Name** 속성 또는 [x:Name](x-name-attribute.md) 특성이 포함된 동일한 XAML 구성에서 다른 요소를 참조하여 데이터 원본을 지정합니다. 이 방법은 관련 값을 공유하거나 한 UI 요소의 하위 속성을 사용하여 다른 요소에 대한 특정 값을 제공하는 데 사용되기도 합니다(예: XAML 컨트롤 템플릿). |
| [**FallbackValue**](https://msdn.microsoft.com/library/windows/apps/dn279345) | 원본 또는 경로를 확인할 수 없을 때 표시할 값을 지정합니다. |
| [**모드**](https://msdn.microsoft.com/library/windows/apps/br209829) | "OneTime", "OneWay" 또는 "TwoWay" 값 중 하나로 바인딩 모드를 지정합니다. 이러한 문자열은 [**BindingMode**](https://msdn.microsoft.com/library/windows/apps/br209822) 열거형의 상수 이름에 해당합니다. 기본값은 "OneWay"입니다. 이 값은 **{x:Bind}** 에 대한 기본값("OneTime")과 다릅니다. | 
| [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) | 바인딩 대상의 위치에 상대적인 바인딩 소스의 위치를 설명하여 데이터 원본을 지정합니다. 이는 XAML 컨트롤 템플릿 내에서 바인딩에 가장 자주 사용됩니다. [{RelativeSource} 태그 확장](relativesource-markup-extension.md) 설정 |
| [**소스**](https://msdn.microsoft.com/library/windows/apps/br209832) | 개체 데이터 원본을 지정합니다. **Binding** 태그 확장 내에서 [**Source**](https://msdn.microsoft.com/library/windows/apps/br209832) 속성에는 [{StaticResource} 태그 확장](staticresource-markup-extension.md) 참조와 같은 개체 참조가 필요합니다. 이 속성이 지정되지 않으면 작동하는 데이터 컨텍스트가 원본을 지정합니다. 개별 바인딩에서 Source 값을 지정하지 않고 여러 바인딩에 대해 공유된 **DataContext**를 사용하는 것이 더 일반적인 방법입니다. 자세한 내용은 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.datacontext.aspx) 또는 [데이터 바인딩 심층 분석](https://msdn.microsoft.com/library/windows/apps/mt210946)을 참조하세요. |
| [**TargetNullValue**](https://msdn.microsoft.com/library/windows/apps/dn279347) | 원본 값이 확인되지만 명시적으로 **null**이 아닌 경우 표시할 값을 지정합니다. |
| [**UpdateSourceTrigger**](https://msdn.microsoft.com/library/windows/apps/dn279350) | 바인딩 소스 업데이트의 타이밍을 지정합니다. 지정하지 않을 경우 기본값은 **Default**입니다. |

**참고**차이점 **모드** 속성에 대 한 기본 값으로 변환 하는 태그에서 **{x: Bind}** **{Binding}** 다음의 주의 합니다.

[**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826), [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) 및 **ConverterLanguage**는 모두 바인딩 원본의 값 또는 형식을 바인딩 대상 속성과 호환되는 형식 또는 값으로 변환하는 시나리오와 관련이 있습니다. 자세한 내용과 예제는 [데이터 바인딩 심층 분석](https://msdn.microsoft.com/library/windows/apps/mt210946)의 "데이터 변환" 섹션을 참조하세요.

> [!NOTE]
> Windows10 버전 1607부터 XAML 프레임워크는 기본 제공 부울-표시 변환기를 제공합니다. 변환기는 **Visible** 열거형 값에 **true**를, **Collapsed**에 **false**를 매핑하므로 변환기를 만들지 않고 Visibility 속성을 부울에 바인딩할 수 있습니다. 기본 제공 변환기를 사용하려면 앱의 최소 대상 SDK 버전이 14393 이상이어야 합니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 기본 제공 변환기를 사용할 수 없습니다. 대상 버전에 대한 자세한 내용은 [버전 적응 코드](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)를 참조하세요.

[**Source**](https://msdn.microsoft.com/library/windows/apps/br209832), [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) 및 [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828)은 바인딩 소스를 지정하므로 상호 배타적입니다.

**팁**값에 단일 중괄호를 지정 해야 하는 경우 [**경로**](https://msdn.microsoft.com/library/windows/apps/br209830) 또는 [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827)같이 다음 앞에 백슬래시: `\{`. 또는 보조 따옴표 집합에서 이스케이프해야 하는 괄호가 포함된 전체 문자열을 다음과 같이 묶습니다. `ConverterParameter='{Mix}'`.

## <a name="examples"></a>예제

```XML
<!-- binding a UI element to a view model -->    
<Page ... >
    <Page.DataContext>
        <local:BookstoreViewModel/>
    </Page.DataContext>

    <GridView ItemsSource="{Binding BookSkus}" SelectedItem="{Binding SelectedBookSku, Mode=TwoWay}" ... />
</Page>
```

```XML
<!-- binding a UI element to another UI element -->
<Page ... >
    <Page.Resources>
        <local:S2Formatter x:Key="GradeConverter"/>
    </Page.Resources>

    <Slider x:Name="sliderValueConverter" ... />
    <TextBox Text="{Binding Path=Value, ElementName=sliderValueConverter,
        Mode=OneWay,
        Converter={StaticResource GradeConverter}}"/>
</Page>
```

두 번째 예제에서는 4가지 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 속성 집합([**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828), [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830), [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209829) 및 [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826))을 설정합니다. 이 경우 **Path**는 명시적으로 **Binding** 속성으로 명명되어 표시됩니다. **Path**는 동일한 런타임 개체 트리에 있는 다른 [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) 개체(`sliderValueConverter`)인 데이터 바인딩 소스로 평가됩니다.

[**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) 속성 값은 다른 태그 확장인 [{StaticResource} 태그 확장](staticresource-markup-extension.md)을 사용하므로 여기서 중첩된 태그 확장 두 개가 사용됩니다. 안쪽 태그 확장이 먼저 평가되므로 리소스를 획득한 후에는 바인딩에서 사용할 수 있는 실용적인 [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903)(리소스에서 `local:S2Formatter` 요소로 인스턴스화되는 사용자 지정 클래스)가 있습니다.

## <a name="tools-support"></a>도구 지원

Microsoft Visual Studio의 Microsoft IntelliSense는 XAML 태그 편집기에서 **{Binding}** 을 작성하는 동안 데이터 컨텍스트의 속성을 표시합니다. "{Binding"을 입력하는 즉시 [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830)에 적합한 데이터 컨텍스트 속성이 드롭다운에 표시됩니다. IntelliSense는 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)의 다른 속성에도 도움이 됩니다. 이렇게 하려면 데이터 컨텍스트 또는 디자인 타임 데이터 컨텍스트를 태그 페이지에 설정해야 합니다. **정의로 이동**(F12)도 **{Binding}** 과 함께 작동합니다. 또는 데이터 바인딩 대화 상자를 사용할 수 있습니다.

 
