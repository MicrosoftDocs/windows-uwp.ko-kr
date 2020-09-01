---
description: 바인딩 태그 확장은 XAML 로드 시 바인딩 클래스의 인스턴스로 변환 됩니다.
title: 바인딩 태그 확장 '
ms.assetid: 3BAFE7B5-AF33-487F-9AD5-BEAFD65D04C3
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 618c59858a4a5b48194644ce992bc246bfcdce71
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174227"
---
# <a name="binding-markup-extension"></a>{Binding} 태그 확장


> [!NOTE]
> 새 바인딩 메커니즘은 성능 및 개발자 생산성을 위해 최적화 된 Windows 10에서 사용할 수 있습니다. [{X:bind} 태그 확장](x-bind-markup-extension.md)을 참조 하세요.

> [!NOTE]
> **{Binding}** 을 사용 하 여 앱에서 데이터 바인딩을 사용 하는 방법과 **{X:bind}** 및 **{binding}** 사이의 전체 비교를 사용 하는 방법에 대 한 일반적인 정보는 [데이터 바인딩 심층](../data-binding/data-binding-in-depth.md)분석을 참조 하세요.

**{Binding}** 태그 확장은 코드와 같은 데이터 소스에서 가져온 값에 컨트롤의 속성을 데이터 바인딩하는 데 사용 됩니다. **{Binding}** 태그 확장은 XAML 로드 시 [**바인딩**](/uwp/api/Windows.UI.Xaml.Data.Binding) 클래스의 인스턴스로 변환 됩니다. 이 바인딩 개체는 데이터 소스의 속성에서 값을 가져와 컨트롤의 속성에 푸시합니다. 필요에 따라 바인딩 개체를 구성 하 여 데이터 원본 속성 값의 변경 내용을 관찰 하 고 해당 변경 내용에 따라 자체를 업데이트할 수 있습니다. 또한 필요에 따라 컨트롤 값에 대 한 변경 내용을 원본 속성에 다시 푸시하는 구성도 가능 합니다. 데이터 바인딩의 대상인 속성은 종속성 속성 이어야 합니다. 자세한 내용은 [종속성 속성 개요](dependency-properties-overview.md)를 참조 하세요.

**{Binding}** 은 (는) 로컬 값과 동일한 종속성 속성 우선 순위를 가지 며, 명령적 코드에서 로컬 값을 설정 하면 태그에 설정 된 **{Binding}** 의 영향이 제거 됩니다.

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

| 용어 | Description |
|------|-------------|
| *propertyPath* | 바인딩의 속성 경로를 지정하는 문자열. 자세한 내용은 [속성 경로](#property-path) 섹션을 참조하세요. |
| *bindingProperties* | *속성 이름* = *value* \[ , *속성 이름* = *값*\]*<br/>이름/값 쌍 구문을 사용하여 지정된 하나 이상의 바인딩 속성. |
| *propName* | [**바인딩**](/uwp/api/Windows.UI.Xaml.Data.Binding) 개체에 설정할 속성의 문자열 이름입니다. 예: "Converter" |
| *value* | 속성을 설정할 값. 인수의 구문은 아래 [{binding} 섹션을 사용 하 여 설정할 수 있는 바인딩 클래스 속성](#properties-of-the-binding-class-that-can-be-set-with-binding) 의 속성에 따라 달라 집니다. |

## <a name="property-path"></a>속성 경로

[**경로**](/uwp/api/windows.ui.xaml.data.binding.path) 바인딩할 속성 (원본 속성)을 설명 합니다. Path는 위치 매개 변수입니다. 즉, 매개 변수 이름을 명시적으로 사용 `{Binding Path=EmployeeID}` 하거나 (), 명명 되지 않은 첫 번째 매개 변수 ()로 지정할 수 있습니다 `{Binding EmployeeID}` .

[**경로**](/uwp/api/windows.ui.xaml.data.binding.path) 형식은 사용자 지정 형식 또는 프레임 워크 형식의 속성 또는 하위 속성으로 계산 되는 문자열인 속성 경로입니다. 이 형식은 [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject)가 될 수 있지만이 될 필요는 없습니다. 속성 경로의 단계는 점(.)으로 구분되므로 연속적인 하위 속성을 트래버스하기 위해 여러 구분 기호를 포함할 수 있습니다. 바인딩할 개체를 구현하는 데 사용되는 프로그래밍 언어에 관계없이 점 구분 기호를 사용합니다.

예를 들어, UI를 employee 개체의 first name 속성에 바인딩하려면 속성 경로가 "Employee. FirstName" 일 수 있습니다. 직원의 부양가족을 포함하는 속성에 항목 컨트롤을 바인딩하는 경우 속성 경로는 "Employee.Dependents"일 수 있으며 항목 컨트롤의 항목 템플릿은 "Dependents" 항목의 표시를 담당합니다.

데이터 원본이 컬렉션인 경우 속성 패치가 컬렉션에서 위치 또는 색인별로 항목을 지정할 수 있습니다. 예를 들면 "팀 \[ 0" \] 입니다. "" 리터럴 " \[ \] "는 컬렉션의 첫 번째 항목을 지정 하는 "0"을 포함 합니다.

기존 [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject)에 [**ElementName**](/uwp/api/windows.ui.xaml.data.binding.elementname) 바인딩을 사용 하는 경우 연결 된 속성을 속성 경로의 일부로 사용할 수 있습니다. 연결 된 속성 이름의 중간 점이 속성 경로에 대 한 한 단계로 간주 되지 않도록 연결 된 속성을 명확 하 게 구분 하려면 소유자 한정 연결 된 속성 이름 앞뒤에 괄호를 추가 합니다. 예를 들면 `(AutomationProperties.Name)` 입니다.

속성 경로 중간 개체는 런타임 표현에서 [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) 개체로 저장 되지만 대부분의 시나리오는 코드에서 **PropertyPath** 개체와 상호 작용할 필요가 없습니다. 일반적으로 XAML을 사용 하 여 필요한 바인딩 정보를 지정할 수 있습니다.

속성 경로에 대 한 문자열 구문, 애니메이션 기능 영역의 속성 경로 및 [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) 개체를 생성 하는 방법에 대 한 자세한 내용은 [속성 경로 구문](property-path-syntax.md)을 참조 하세요.

## <a name="properties-of-the-binding-class-that-can-be-set-with-binding"></a>{Binding}을 사용 하 여 설정할 수 있는 바인딩 클래스의 속성


태그 확장에서 설정할 수 있는 [**바인딩의**](/uwp/api/Windows.UI.Xaml.Data.Binding) 읽기/쓰기 속성이 여러 개 있으므로 **{Binding}** 은 (는) *bindingproperties* 자리 표시자 구문을 사용 하 여 보여 줍니다. 속성은 *쉼표로 구분 된 속성* = *값* 쌍을 사용 하 여 순서에 관계 없이 설정할 수 있습니다. 일부 속성에는 형식 변환이 없는 형식이 필요 하므로 이러한 속성에는 **{Binding}** 내에 중첩 된 태그 확장이 필요 합니다.

| 속성 | Description |
|----------|-------------|
| [**경로**](/uwp/api/windows.ui.xaml.data.binding.path) | 위의 [속성 경로](#property-path) 섹션을 참조하세요. |
| [**변환기**](/uwp/api/windows.ui.xaml.data.binding.converter) | 바인딩 엔진에서 호출 하는 변환기 개체를 지정 합니다. 태그에서 [{StaticResource} 태그 확장](staticresource-markup-extension.md) 을 사용 하 여 리소스 사전에서 해당 개체에 대 한 참조로 변환기를 설정할 수 있습니다. |
| [**ConverterLanguage**](/uwp/api/windows.ui.xaml.data.binding.converterlanguage) | 변환기가 사용할 문화권을 지정합니다. ( [**변환기**](/uwp/api/windows.ui.xaml.data.binding.converter)를 설정 하는 경우) 문화권이 표준 기반 식별자로 설정 됩니다. 자세한 내용은 ConverterLanguage를 참조 하세요. [ **ConverterLanguage**](/uwp/api/windows.ui.xaml.data.binding.converterlanguage) |
| [**ConverterParameter**](/uwp/api/windows.ui.xaml.data.binding.converterparameter) | 변환기 논리에 사용할 수 있는 변환기 매개 변수를 지정 합니다. ( [**변환기**](/uwp/api/windows.ui.xaml.data.binding.converter)를 설정 하는 경우) 대부분의 변환기는 전달 된 값에서 변환 하는 데 필요한 모든 정보를 가져오는 간단한 논리를 사용 하며 **ConverterParameter** 값이 필요 하지 않습니다. **ConverterParameter** 매개 변수는 **ConverterParameter**에서 전달 되는 항목을 키로 하는 조건부 논리를 포함 하는 더 복잡 한 변환기 구현을 위한 것입니다. 문자열이 아닌 값을 사용하는 변환기를 작성할 수도 있지만 일반적이지 않습니다. 자세한 내용은 **ConverterParameter**에서 설명을 참조하세요. |
| [**ElementName**](/uwp/api/windows.ui.xaml.data.binding.elementname) | **이름** 속성 또는 [x:Name 특성이](x-name-attribute.md)있는 동일한 XAML 구문에서 다른 요소를 참조 하 여 데이터 소스를 지정 합니다. 이는 종종를 사용 하 여 관련 값을 공유 하거나 한 UI 요소의 하위 속성을 사용 하 여 다른 요소에 대 한 특정 값을 제공 합니다 (예: XAML 컨트롤 템플릿). |
| [**FallbackValue**](/uwp/api/windows.ui.xaml.data.binding.fallbackvalue) | 원본 또는 경로를 확인할 수 없을 때 표시할 값을 지정합니다. |
| [**Mode**](/uwp/api/windows.ui.xaml.data.binding.mode) | 바인딩 모드를 "OneTime", "OneWay" 또는 "TwoWay" 값 중 하나로 지정 합니다. 이러한 이름은 [**Bindingmode**](/uwp/api/Windows.UI.Xaml.Data.BindingMode) 열거형의 상수 이름에 해당 합니다. 기본값은 "OneWay"입니다. 이는 "OneTime" 인 **{X:bind}** 의 기본값과 다릅니다. | 
| [**RelativeSource**](/uwp/api/windows.ui.xaml.data.binding.relativesource) | 바인딩 대상의 위치를 기준으로 바인딩 소스 위치를 설명 하 여 데이터 소스를 지정 합니다. 이는 XAML 컨트롤 템플릿 내의 바인딩에서 주로 사용 됩니다. [{RelativeSource} 태그 확장](relativesource-markup-extension.md)을 설정 합니다. |
| [**원본**](/uwp/api/windows.ui.xaml.data.binding.source) | 개체 데이터 소스를 지정 합니다. **바인딩** 태그 확장 내에서 [**Source**](/uwp/api/windows.ui.xaml.data.binding.source) 속성에는 [{StaticResource} 태그 확장](staticresource-markup-extension.md) 참조와 같은 개체 참조가 필요 합니다. 이 속성이 지정 되지 않은 경우 작동 하는 데이터 컨텍스트는 소스를 지정 합니다. 개별 바인딩에 원본 값을 지정 하는 대신 여러 바인딩에 대해 공유 된 **DataContext** 를 사용 하는 것이 더 일반적입니다. 자세한 내용은 [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 또는 [데이터 바인딩 심층](../data-binding/data-binding-in-depth.md)분석을 참조 하세요. |
| [**TargetNullValue**](/uwp/api/windows.ui.xaml.data.binding.targetnullvalue) | 원본 값이 확인되지만 명시적으로 **null**이 아닌 경우 표시할 값을 지정합니다. |
| [**UpdateSourceTrigger**](/uwp/api/windows.ui.xaml.data.binding.updatesourcetrigger) | 바인딩 소스 업데이트의 타이밍을 지정 합니다. 지정 하지 않으면 기본값은 **default**입니다. |

**참고**    **{X:bind}** 에서 **{Binding}**(으)로 태그를 변환 하는 경우 **Mode** 속성의 기본값 차이를 알고 있어야 합니다.

[**Converter**](/uwp/api/windows.ui.xaml.data.binding.converter), [**ConverterLanguage**](/uwp/api/windows.ui.xaml.data.binding.converterlanguage)와 **ConverterLanguage**는 모두 바인딩 소스에서 바인딩 대상 속성과 호환 되는 형식 또는 값으로 값 또는 형식을 변환 하는 시나리오와 관련이 있습니다. 자세한 내용과 예제는 [데이터 바인딩 심층 분석](../data-binding/data-binding-in-depth.md)의 "데이터 변환" 섹션을 참조하세요.

> [!NOTE]
> Windows 10 버전 1607부터 XAML 프레임워크는 기본 제공 부울-표시 변환기를 제공합니다. 변환기는 **표시 되** 는 열거형 값에 **True** 를 매핑하고 축소 되도록 **false** 를 **축소** 하므로 변환기를 만들지 않고도 표시 유형 속성을 부울에 바인딩할 수 있습니다. 기본 제공 변환기를 사용하려면 앱의 최소 대상 SDK 버전이 14393 이상이어야 합니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 기본 제공 변환기를 사용할 수 없습니다. 대상 버전에 대한 자세한 내용은 [버전 적응 코드](../debug-test-perf/version-adaptive-code.md)를 참조하세요.

[**Source**](/uwp/api/windows.ui.xaml.data.binding.source), [**RelativeSource**](/uwp/api/windows.ui.xaml.data.binding.relativesource)및 [**ElementName**](/uwp/api/windows.ui.xaml.data.binding.elementname) 은 바인딩 소스를 지정 하므로 함께 사용할 수 없습니다.

**팁**    [**Path**](/uwp/api/windows.ui.xaml.data.binding.path) 또는 [**ConverterParameter**](/uwp/api/windows.ui.xaml.data.binding.converterparameter)와 같은 값에 대해 단일 중괄호를 지정 해야 하는 경우 백슬래시를 사용 하 여 앞에를 `\{` 붙입니다. 또는 보조 따옴표 집합에서 이스케이프해야 하는 괄호가 포함된 전체 문자열을 다음과 같이 묶습니다. `ConverterParameter='{Mix}'`.

## <a name="examples"></a>예

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

두 번째 예제는 [**ElementName**](/uwp/api/windows.ui.xaml.data.binding.elementname), [**경로**](/uwp/api/windows.ui.xaml.data.binding.path), [**모드**](/uwp/api/windows.ui.xaml.data.binding.mode) 및 [**변환기**](/uwp/api/windows.ui.xaml.data.binding.converter)의 네 가지 [**바인딩**](/uwp/api/Windows.UI.Xaml.Data.Binding) 속성을 설정 합니다. 이 경우의 **경로** 는 **바인딩** 속성으로 명시적으로 명명 되어 표시 됩니다. **경로** 는 이름이 인 [**슬라이더**](/uwp/api/Windows.UI.Xaml.Controls.Slider) 와 동일한 런타임 개체 트리의 다른 개체인 데이터 바인딩 원본으로 평가 됩니다 `sliderValueConverter` .

[**변환기**](/uwp/api/windows.ui.xaml.data.binding.converter) 속성 값에서 다른 태그 확장명 [{StaticResource} 태그](staticresource-markup-extension.md)확장을 사용 하는 방법에는 여기에 두 개의 중첩 된 태그 확장 용도가 있습니다. 내부는 먼저 계산 되므로 리소스를 가져온 후에는 바인딩에서 사용할 수 있는 실용적인 [**ivalueconverter.convert**](/uwp/api/Windows.UI.Xaml.Data.IValueConverter) (리소스의 요소에 의해 인스턴스화된 사용자 지정 클래스)가 발생 `local:S2Formatter` 합니다.

## <a name="tools-support"></a>도구 지원

Microsoft Visual Studio의 Microsoft IntelliSense는 XAML 태그 편집기에서 **{Binding}** 을 제작 하는 동안 데이터 컨텍스트의 속성을 표시 합니다. "{Binding"을 입력 하는 즉시 [**경로**](/uwp/api/windows.ui.xaml.data.binding.path) 에 적합 한 데이터 컨텍스트 속성이 드롭다운에서 표시 됩니다. IntelliSense는 [**바인딩의**](/uwp/api/Windows.UI.Xaml.Data.Binding)다른 속성을 사용 하는 데도 도움이 됩니다. 이 작업을 수행 하려면 마크업 페이지에서 데이터 컨텍스트 또는 디자인 타임 데이터 컨텍스트를 설정 해야 합니다. **정의로 이동** (F12)은 **{Binding}** 에서도 작동 합니다. 또는 데이터 바인딩 대화 상자를 사용할 수 있습니다.

 