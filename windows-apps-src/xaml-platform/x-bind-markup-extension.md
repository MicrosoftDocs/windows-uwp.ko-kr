---
author: jwmsft
description: "xBind 태그 확장은 Binding 대신 사용됩니다. xBind에는 Binding의 일부 기능이 없지만 Binding보다 적은 메모리로 빠르게 실행되며 향상된 디버깅을 지원합니다."
title: "xBind 태그 확장"
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
translationtype: Human Translation
ms.sourcegitcommit: 2dc706412684ded7fe4e98c6d01fb75ae65abd5f
ms.openlocfilehash: b7d1dd183ba35e4b694a80d3e43628c928b218e5

---

# {x:Bind} 태그 확장

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

**참고** 앱에서 **{x:Bind}**와 함께 데이터 바인딩을 사용하는 방법 및 **{x:Bind}**와 **{Binding}** 간 비교에 대한 일반 정보는 [데이터 바인딩 심층 분석](https://msdn.microsoft.com/library/windows/apps/mt210946)을 참조하세요.

Windows 10의 새로운 기능인 **{x:Bind}** 태그 확장은 **{Binding}** 대신 사용됩니다. **{x:Bind}**에는 **{Binding}**의 일부 기능이 없지만 **{Binding}**보다 적은 메모리로 빠르게 실행되며 향상된 디버깅을 지원합니다.

XAML 컴파일 시간에 **{x:Bind}**는 데이터 원본에 대한 속성에서 값을 가져오는 코드로 변환되고 태그에 지정된 속성에서 이를 설정합니다. 필요한 경우 데이터 원본 속성의 값 변경을 관찰하고 해당 변경 내용에 따라 자체적으로 새로 고치도록 바인딩 개체를 구성할 수 있습니다. 또한 필요한 경우 고유한 값 변경을 소스 속성에 다시 적용하도록 구성할 수도 있습니다. **{x:Bind}** 및 **{Binding}**에서 만든 바인딩 개체는 기능적으로 거의 동일합니다. 그러나 **{x:Bind}**는 컴파일 타임에 생성되는 특수 코드이며, **{Binding}**은 범용 런타임 개체 검사를 사용합니다. 따라서 **{x:Bind}** 바인딩(컴파일된 바인딩이라고도 함)은 성능이 뛰어나고, 바인딩 식에 대한 컴파일 타임 유효성 검사를 제공하며, 페이지의 partial 클래스로 생성되는 코드 파일에 중단점을 설정할 수 있도록 하여 디버깅을 지원합니다. 이러한 파일은 `obj` 폴더(C#의 경우 이름이 `<view name>.g.cs`와 같음)에서 찾을 수 있습니다.

**{x:Bind}를 보여 주는 샘플 앱**

-   [{x:Bind} 샘플](http://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)
-   [XAML UI 기본 사항 샘플](http://go.microsoft.com/fwlink/p/?linkid=619992)

## XAML 특성 사용

``` syntax
<object property="{x:Bind}" .../>
-or-
<object property="{x:Bind propertyPath}" .../>
-or-
<object property="{x:Bind bindingProperties}" .../>
-or-
<object property="{x:Bind propertyPath, bindingProperties}" .../>
```

| 용어 | 설명 |
|------|-------------|
| _propertyPath_ | 바인딩의 속성 경로를 지정하는 문자열. 자세한 내용은 [속성 경로](#property-path) 섹션을 참조하세요. |
| _bindingProperties_ |
| _propName_=_value_\[, _propName_=_value_\]* | 이름/값 쌍 구문을 사용하여 지정된 하나 이상의 바인딩 속성. |
| _propName_ | 바인딩 개체에 설정할 속성의 문자열 이름. 예: "Converter" |
| _value_ | 속성을 설정할 값. 인수 구문은 설정할 속성에 따라 다릅니다. 다음은 값 자체가 태그 확장인 _propName_=_value_ 사용법의 예입니다. `Converter={StaticResource myConverterClass}`. 자세한 내용은 아래의 [{x:Bind}로 설정할 수 있는 속성](#properties-you-can-set)을 참조하세요. | 

## 속성 경로

*PropertyPath*는 **{x:Bind}** 식에 대한 **Path**를 설정합니다. **Path**는 소스에 바인딩할 속성, 하위 속성, 필드 또는 메서드의 값을 지정하는 속성 경로입니다. **Path** 속성 이름을 명시적으로 지정하거나(`{Binding Path=...}`) 생략할 수 있습니다(`{Binding ...}`).

### 속성 경로 확인

**{x:Bind}**는 **DataContext**를 기본 소스로 사용하지 않습니다. 대신 페이지 또는 사용자 컨트롤 자체를 사용합니다. 따라서 속성, 필드 및 메서드에 대한 페이지 또는 사용자 컨트롤의 코드 숨김으로 표시됩니다. 뷰 모델을 **{x:Bind}**에 노출하려면 일반적으로 페이지 또는 사용자 컨트롤의 코드 숨김에 새 필드 또는 속성을 추가하면 됩니다. 속성 경로의 단계는 점(.)으로 구분되므로 연속적인 하위 속성을 트래버스하기 위해 여러 구분 기호를 포함할 수 있습니다. 바인딩할 개체를 구현하는 데 사용되는 프로그래밍 언어에 관계없이 점 구분 기호를 사용합니다.

예를 들어 페이지에서 **Text="{x:Bind Employee.FirstName}"**은 **Employee** 멤버를 찾은 다음 **Employee**에서 반환되는 개체에서 **FirstName** 멤버를 찾습니다. 직원의 부양가족을 포함하는 속성에 항목 컨트롤을 바인딩하는 경우 속성 경로는 "Employee.Dependents"일 수 있으며 항목 컨트롤의 항목 템플릿은 "Dependents" 항목의 표시를 담당합니다.

C++/CX의 경우 **{x:Bind}**는 페이지 또는 데이터 모델의 전용 필드 및 속성에 바인딩할 수 없습니다. 바인딩하려면 공용 속성이 있어야 합니다. 관련 메타데이터를 가져올 수 있도록 바인딩 노출 영역을 CX 클래스/인터페이스로 노출해야 합니다. **\[Bindable\]** 특성은 필요하지 않습니다.

**x:Bind**를 사용하면 **ElementName=xxx**를 바인딩 식의 일부로 사용할 필요가 없습니다. **x:Bind**를 사용하면 요소의 이름을 바인딩 경로의 첫 부분으로 사용할 수 있습니다. 명명된 요소는 루트 바인딩 소스를 나타내는 페이지 또는 사용자 컨트롤 내의 필드가 되기 때문입니다.

### 컬렉션

데이터 원본이 컬렉션인 경우 속성 패치가 컬렉션에서 위치 또는 색인별로 항목을 지정할 수 있습니다. 예를 들어 "Teams\[0\].Players"에서 리터럴 "\[\]"는 색인되지 않은 컬렉션의 첫 번째 항목을 요청하는 "0"을 묶습니다.

인덱서를 사용하려면 메서드에서 인덱싱할 속성 형식에 대해 **IList&lt;T&gt;** 또는 **IVector&lt;T&gt;**를 구현해야 합니다. 인덱싱된 속성 형식이 **INotifyCollectionChanged** 또는 **IObservableVector**를 지원하는 경우 바인딩이 OneWay 또는 TwoWay이면 해당 인터페이스에 대한 변경 알림을 등록하고 수신합니다. 변경 내용 검색 논리는 인덱싱된 특정 값에 영향을 주지 않는 경우에도 모든 컬렉션 변경 내용에 따라 업데이트됩니다.습니다 하는 경우에 모든 컬렉션 변경 내용에 따라 합니다. 이는 수신 대기 논리가 컬렉션의 모든 인스턴스에서 공통적이기 때문입니다.

데이터 원본이 사전 또는 맵인 경우 문자열 이름을 기준으로 속성 경로에서 컬렉션의 항목을 지정할 수 있습니다. 예를 들어 **&lt;TextBlock Text="{x:Bind Players\['John Smith'\]" /&gt;**는 "John Smith"라는 이름의 사전에서 항목을 찾습니다. 이름은 따옴표로 묶어야 하며 작은따옴표 또는 큰따옴표를 사용할 수 있습니다. 문자열에서 따옴표를 이스케이프할 때는 캐럿(^)을 사용할 수 있습니다. 일반적으로 XAML 특성에 대해 사용하지 않는 따옴표를 사용하는 것이 가장 쉽습니다.

문자열 인덱서를 사용하려면 모델에서 인덱싱할 속성 형식에 대해 **IDictionary&lt;string, T&gt;** 또는 **IMap&lt;string, T&gt;**를 구현해야 합니다. 인덱싱된 속성 형식이 **IObservableMap**을 지원하는 경우 바인딩이 OneWay 또는 TwoWay이면 해당 인터페이스에 대한 변경 알림을 등록하고 수신합니다. 변경 내용 검색 논리는 인덱싱된 특정 값에 영향을 주지 않는 경우에도 모든 컬렉션 변경 내용에 따라 업데이트됩니다.습니다 하는 경우에 모든 컬렉션 변경 내용에 따라 합니다. 이는 수신 대기 논리가 컬렉션의 모든 인스턴스에서 공통적이기 때문입니다.

### 연결된 속성

연결된 속성에 바인딩하려면 괄호 안의 점 뒤에 클래스 및 속성 이름을 입력해야 합니다. 예를 들면 **Text="{x:Bind Button22.(Grid.Row)}"**와 같습니다. Xaml 네임스페이스에서 속성이 선언되지 않은 경우 문서 헤드의 코드 네임스페이스에 매핑해야 하는 xml 네임스페이스를 접두사로 사용해야 합니다.

### 캐스팅

컴파일된 바인딩이 강력한 형식이며 경로의 각 단계에 대한 형식을 확인합니다. 반환된 형식에 멤버가 없으면 컴파일 타임에 실패합니다. 개체의 실제 형식을 바인딩하도록 명령하는 캐스트를 지정할 수 있습니다. 다음 사례에서 **obj**는 형식 개체의 속성이지만 입력란을 포함하므로 **Text="{x:Bind ((TextBox)obj).Text}"** 또는 **Text="{x:Bind obj.(TextBox.Text)}"**를 사용할 수 있습니다.
**Text="{x:Bind ((data:SampleDataGroup)groups3\[0\]).Title}"**의 **groups3** 필드는 개체의 사전이므로 **data:SampleDataGroup**에 캐스트해야 합니다. xml **data:** 네임스페이스 접두사는 기본 XAML 네임스페이스의 일부가 아닌 코드 네임스페이스에 개체 형식을 매핑하는 데 사용됩니다.

_참고: C# 스타일 캐스트 구문은 연결된 속성 구문보다 더 유연한 방법이므로 권장됩니다._

## 바인딩 경로의 함수

Windows10 버전 1607부터 **{x:Bind}**는 함수를 바인딩 경로의 리프 단계로 사용할 수 있습니다. 그러면 다음과 같은 이점이 있습니다.
- 간단한 값 변환 방법
- 둘 이상의 매개 변수를 사용하는 바인딩 방식

> [!NOTE]
> **{x:Bind}**와 함께 함수를 사용하려면 앱의 최소 대상 SDK 버전이 14393 이상이어야 합니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 함수를 사용할 수 없습니다. 대상 버전에 대한 자세한 내용은 [버전 적응 코드](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)를 참조하세요.

다음 예제에서는 항목의 배경과 전경이 색 매개 변수에 따라 변환을 수행하는 함수에 바인딩됩니다.

``` Xamlmarkup
<DataTemplate x:DataType="local:ColorEntry">
    <Grid Background="{x:Bind Brushify(Color)}" Width="240">
        <TextBlock Text="{x:Bind ColorName}" Foreground="{x:Bind TextColor(Color)}" Margin="10,5" />
    </Grid>
</DataTemplate>
```
``` C#
class ColorEntry
{
    public string ColorName { get; set; }
    public Color Color { get; set; }

    public static SolidColorBrush Brushify(Color c)
    {
        return new SolidColorBrush(c);
    }

    public static SolidColorBrush TextColor(Color c)
    {
        return new SolidColorBrush(((c.R * 0.299 + c.G * 0.587 + c.B * 0.114) > 150) ? Colors.Black : Colors.White);
    }
}

```
### 함수 구문
``` Syntax
Text="{x:Bind MyModel.Order.CalculateShipping(MyModel.Order.Weight, MyModel.Order.ShipAddr.Zip, 'Contoso'), Mode=OneTime}"
             |      Path to function         |    Path argument   |       Path argument       | Const arg |  Bind Props
```

### 함수 경로
함수 경로는 다른 속성 경로처럼 지정되며 점(.), 인덱서 또는 함수를 찾을 캐스트를 포함할 수 있습니다.

정적 함수는 XMLNamespace:ClassName.MethodName 구문을 사용하여 지정할 수 있습니다. 예를 들어 페이지 맨 위에서 **xmlns:sys="using:System"**을 지정한 경우 **&lt;CalendarDatePicker Date="\{x:Bind sys:DateTime.Parse(TextBlock1.Text)\}" /&gt;**가 DateTime.Parse 함수에 매핑됩니다.

모드가 OneWay/TwoWay인 경우 함수 경로에 수행된 변경 검색이 포함되고 이러한 개체가 변경된 경우 바인딩이 다시 평가됩니다.

바인딩할 함수는 다음을 수행해야 합니다.
- 코드 및 메타데이터에 액세스할 수 있어야 합니다. 그러므로 internal/private은 C#에서 작업하지만 C++/CX에는 public WinRT 메서드가 필요합니다.
- 오버로드는 형식이 아닌 인수 개수를 기반으로 하며 해당 개수의 인수를 사용하는 첫 번째 오버로드와 일치시키려고 합니다.
- 인수 형식은 전달 중인 데이터와 일치해야 합니다. 변환을 축소하지 않습니다.
- 함수의 반환 형식은 바인딩을 사용 중인 속성의 형식과 일치해야 합니다.


### 함수 인수
함수 인수를 여러 개 지정할 경우 쉼표(,)를 사용하여 구분합니다.
- 바인딩 경로 – 해당 개체에 직접 바인딩할 때와 동일한 구문입니다.
  - 모드가 OneWay/TwoWay인 경우 변경 검색이 수행되고 개체가 변경될 때 바인딩이 다시 평가됩니다.
- 따옴표로 묶인 상수 문자열 – 문자열로 지정하려면 따옴표가 필요합니다. 문자열에서 따옴표를 이스케이프할 때는 캐럿(^)을 사용합니다.
- 상수 - 예를 들어 123.456입니다.
- 부울 – "x:True" 또는 "x:False"로 지정합니다.

### 양방향 함수 바인딩
양방향 바인딩 시나리오에서는 두 번째 함수를 바인딩의 반대 방향으로 지정해야 합니다. 이 작업은 **BindBack** 바인딩 속성을 사용하여 수행할 수 있습니다(예: **Text="\{x:Bind a.MyFunc(b), BindBack=a.MyFunc2\}"**). 이 함수는 모델로 다시 푸시해야 하는 값을 인수로 사용합니다.

## 이벤트 바인딩

이벤트 바인딩은 컴파일된 바인딩에 대해 고유한 기능입니다. 이를 통해 코드 숨김에 대한 메서드 없이도 바인딩을 사용하여 이벤트 처리기를 지정할 수 있습니다. 예를 들면 **Click="{x:Bind rootFrame.GoForward}"**와 같습니다.

이벤트의 경우 대상 메서드는 오버로드되어서는 안 되며, 다음을 충족해야 합니다.

-   이벤트의 서명과 일치해야 합니다.
-   또는 매개 변수가 없어야 합니다.
-   또는 이벤트 매개 변수 형식에서 할당 가능한 동일한 개수의 매개 변수 형식이 있어야 합니다.

생성된 코드 숨김에서 컴파일된 바인딩은 이벤트를 처리하여 모델의 메서드로 라우트합니다. 이를 통해 이벤트가 발생한 경우 바인딩 식의 경로를 평가합니다. 즉, 속성 바인딩과 달리 모델의 변경 내용을 추적하지 않습니다.

속성 경로의 문자열 구문에 대한 자세한 내용은 [Property-path 구문](property-path-syntax.md)을 참조하세요(**{x:Bind}**에 대해 여기에 설명된 차이점에 유의).

##  {x:Bind}로 설정할 수 있는 속성


태그 확장에서 설정될 수 있는 읽기/쓰기 속성이 여러 개이므로 **{x:Bind}**은 *bindingProperties* 자리 표시자 구문을 통해 설명됩니다. 쉼표로 구분된 *propName*=*value* 쌍으로 순서에 상관없이 속성을 설정할 수 있습니다. 바인딩 식에는 줄바꿈을 포함할 수 없습니다. 일부 속성에는 형식 변환이 없는 형식이 필요하므로 해당 태그 확장이 **{x:Bind}**에 중첩되어야 합니다.

이러한 속성은 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 클래스의 속성과 거의 동일한 방식으로 작동합니다.

| 속성 | 설명 |
|----------|-------------|
| **경로** | 위의 [속성 경로](#property-path) 섹션을 참조하세요. |
| **변환기** | 바인딩 엔진이 호출하는 변환기 개체를 지정합니다. 변환기는 XAML에서 설정할 수 있지만 리소스 사전의 해당 개체에 대한 [{StaticResource} 태그 확장](staticresource-markup-extension.md) 참조에서 할당한 개체 인스턴스를 참조하는 경우에만 설정할 수 있습니다. |
| **ConverterLanguage** | 변환기가 사용할 문화권을 지정합니다. **ConverterLanguage**를 설정하는 경우 **Converter**도 설정해야 합니다. 문화권은 표준 기반 식별자로 설정됩니다. 자세한 내용은 [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880)를 참조하세요. |
| **ConverterParameter** | 변환기 논리에서 사용될 수 있는 변환기 매개 변수를 지정합니다. **ConverterParameter**를 설정하는 경우 **Converter**도 설정해야 합니다. 대부분의 변환기는 전달된 값에서 변환에 필요한 모든 정보를 가져오는 간단한 논리를 사용하므로 **ConverterParameter** 값이 필요하지 않습니다. **ConverterParameter** 매개 변수는 **ConverterParameter**에서 전달되는 내용을 수용하는 여러 논리가 있는 고급 변환기를 구현하기 위한 것입니다. 문자열이 아닌 값을 사용하는 변환기를 작성할 수도 있지만 일반적이지 않습니다. 자세한 내용은 [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827)에서 설명을 참조하세요. |
| **FallbackValue** | 원본 또는 경로를 확인할 수 없을 때 표시할 값을 지정합니다. |
| **모드** | “OneTime”, “OneWay” 또는 “TwoWay” 문자열 중 하나로 바인딩 모드를 지정합니다. 기본값은 "OneTime"입니다. 이 값은 **{Binding}**에 대한 기본값(대부분의 경우 "OneWay"임)과 다릅니다. |
| **TargetNullValue** | 원본 값이 확인되지만 명시적으로 **null**이 아닌 경우 표시할 값을 지정합니다. |
| **BindBack** | 양방향 바인딩의 반대 방향으로 사용할 함수를 지정합니다. | 

**참고** **{Binding}**에서 **{x:Bind}**로 태그를 변환하는 경우 **Mode** 속성에 대한 기본값의 차이에 주의하세요.
 
## 설명

**{x:Bind}**는 생성된 코드를 사용하여 이점을 제공하기 때문에 컴파일 타임에 형식 정보가 필요합니다. 즉, 사전에 형식을 모르는 속성에 바인딩할 수 없습니다. 따라서 **Object** 형식이고 런타임에 변경될 수 있는 **DataContext** 속성에서 **{x:Bind}**를 사용할 수 없습니다.

데이터 템플릿에서 **{x:Bind}**를 사용할 때는 아래 예제와 같이 **x:DataType** 값을 설정하여 바인딩할 형식을 나타내야 합니다. 형식을 인터페이스 또는 기본 클래스 형식으로 설정한 다음 필요한 경우 캐스트를 사용하여 전체 식을 구성할 수 있습니다.

컴파일된 바인딩은 코드 생성에 따라 다릅니다. 따라서 리소스 사전의 **{x:Bind}**를 사용하는 경우 리소스 사전에는 코드 숨김 클래스가 있어야 합니다. 코드 예제에 대해서는 [{x:Bind}를 사용하는 리소스 사전](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind)을 참조하세요.

컴파일된 바인딩이 포함된 페이지 및 사용자 컨트롤의 생성된 코드에는 "Bindings" 속성이 포함됩니다. 여기에는 다음 메서드가 포함됩니다.
- **Update()** - 컴파일된 모든 바인딩의 값을 업데이트합니다. 단방향/양방향 바인딩에는 변경 사항을 검색하기 위해 연결된 리스너가 있습니다.
- **Initialize()** - 바인딩이 아직 초기화되지 않은 경우 Update()를 호출하여 바인딩을 초기화합니다.
- **StopTracking()** - 단방향 및 양방향 바인딩을 위해 만들어진 모든 리스너의 연결이 해제됩니다. Update() 메서드를 사용하여 다시 초기화할 수 있습니다.

> [!NOTE]
> Windows10 버전 1607부터 XAML 프레임워크는 기본 제공 부울-표시 변환기를 제공합니다. 변환기는 **Visible** 열거형 값에 **true**를, **Collapsed**에 **false**를 매핑하므로 변환기를 만들지 않고 Visibility 속성을 부울에 바인딩할 수 있습니다. 기본 제공 변환기를 사용하려면 앱의 최소 대상 SDK 버전이 14393 이상이어야 합니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 기본 제공 변환기를 사용할 수 없습니다. 대상 버전에 대한 자세한 내용은 [버전 적응 코드](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)를 참조하세요.

**팁** [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) 또는 [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827)의 경우처럼 값에 단일 중괄호를 지정해야 하는 경우 중괄호 앞에 백슬래시를 넣습니다. `\{`. 또는 보조 따옴표 집합에서 이스케이프해야 하는 괄호가 포함된 전체 문자열을 다음과 같이 묶습니다. `ConverterParameter='{Mix}'`.

[**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826), [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) 및 **ConverterLanguage**는 모두 바인딩 원본의 값 또는 형식을 바인딩 대상 속성과 호환되는 형식 또는 값으로 변환하는 시나리오와 관련이 있습니다. 자세한 내용과 예제는 [데이터 바인딩 심층 분석](https://msdn.microsoft.com/library/windows/apps/mt210946)의 "데이터 변환" 섹션을 참조하세요.

**{x:Bind}**는 태그 확장일 뿐이므로 이러한 바인딩을 프로그래밍 방식으로 만들거나 조작할 방법이 없습니다. 태그 확장에 대한 자세한 내용은 [XAML 개요](xaml-overview.md)를 참조하세요.

## 예제

```XML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

이 예제 XAML에서는 **ListView.ItemTemplate** 속성에서 **{x:Bind}**를 사용합니다. **x:DataType** 값의 선언에 유의하세요.

```XML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```



<!--HONumber=Nov16_HO1-->


