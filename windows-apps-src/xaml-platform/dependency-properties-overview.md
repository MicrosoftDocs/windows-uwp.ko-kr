---
이 항목에서는 UI의 XAML 정의와 함께 C++, C# 또는 Visual Basic을 사용하여 Windows 런타임 앱을 작성할 때 사용할 수 있는 종속성 속성 시스템에 대해 설명합니다.
종속성 속성 개요
ms.assetid: AD649E66-F71C-4DAA-9994-617C886FDA7E
---

# 종속성 속성 개요

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 항목에서는 UI의 XAML 정의와 함께 C++, C# 또는 Visual Basic을 사용하여 Windows 런타임 앱을 작성할 때 사용할 수 있는 종속성 속성 시스템에 대해 설명합니다.

## 종속성 속성이란?

종속성 속성은 특수한 유형의 속성입니다. 구체적으로 말해, 속성의 값이 Windows 런타임의 일부인 전용 속성 시스템에 의해 추적되고 영향을 받는 속성입니다.

종속성 속성을 지원하기 위해 이 속성을 정의하는 개체는 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)(즉, **DependencyObject** 기본 클래스가 어디서든 상속된 클래스)여야 합니다. XAML을 사용한 Windows 스토어 앱의 UI 정의에 사용하는 유형 중 상당수는 **DependencyObject** 하위 클래스이며 종속성 속성을 지원합니다. 하지만 이름에 "XAML"이 포함되지 않은 Windows 런타임 네임스페이스에서 가져온 유형은 종속성 속성을 지원하지 않습니다. 이 유형의 속성은 속성 시스템의 종속성 동작을 가지지 않는 일반적인 속성입니다.

종속성 속성의 목적은 다른 입력(실행 중에 앱 내에서 발생하는 다른 속성, 이벤트 및 상태)을 기반으로 속성 값을 계산하는 체계적인 방법을 제공하는 것입니다. 이러한 다른 입력으로는 다음을 들 수 있습니다.

-   사용자 기본 설정 같은 외부 입력
-   데이터 바인딩, 애니메이션, 스토리보드 같은 Just-In-Time 속성 결정 메커니즘
-   리소스 및 스타일 같은 다중 사용 템플릿 지정 패턴
-   개체 트리의 다른 요소와 부모-자식 관계를 맺음으로써 알게 된 값

종속성 속성은 UI에 XAML을 사용하고 코드에 C#, Microsoft Visual Basic 또는 Visual C++ 확장 구성 요소(C++/CX)로 작성된 Windows 런타임 앱을 정의하기 위한 프로그래밍 모델의 특정 기능을 나타내거나 지원합니다. 이러한 기능은 다음과 같습니다.

-   데이터 바인딩
-   스타일
-   스토리보드 애니메이션
-   "PropertyChanged" 동작(종속성 속성을 구현하여 다른 종속성 속성에 변경 내용을 전파할 수 있는 콜백을 제공할 수 있음)
-   속성 메타데이터에서 가져온 기본값 사용
-   [
            **ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) 및 메타데이터 조회와 같은 일반적인 속성 시스템 유틸리티

## 종속성 속성 및 Windows 런타임 속성

종속성 속성은 런타임에 앱의 모든 종속성 속성을 지원하는 전역 내부 속성 저장소를 제공함으로써 기본 Windows 런타임 속성 기능을 확장합니다. 속성 정의 클래스에서 private인 개인 필드로 속성을 지원하는 표준 패턴의 대안으로 사용됩니다. 이 내부 속성 저장소는 특정 개체에 대해 존재하는 속성 식별자와 값의 집합으로 간주될 수 있습니다([**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)인 경우). 저장소의 각 속성은 이름으로 식별되는 대신 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 인스턴스로 식별됩니다. 그러나 속성 시스템은 대개 이 구현 정보를 숨기므로, 사용자는 일반적으로 간단한 이름(사용하는 코드 언어의 프로그래밍 속성 이름 또는 XAML 작성 시의 특성 이름)을 사용하여 종속성 속성에 액세스할 수 있습니다.

종속성 속성 시스템의 기초를 제공하는 기본 형식은 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)입니다. **DependencyObject**는 종속성 속성에 액세스할 수 있는 메서드를 정의하고, **DependencyObject** 파생 클래스의 인스턴스는 이전에 언급한 속성 저장소 개념을 내부적으로 지원합니다.

종속성 속성에 대해 설명할 때 이 설명서에서 사용되는 용어를 요약하면 다음과 같습니다.

| 용어 | 설명 |
|------|-------------|
| 종속성 속성 | [
            **DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 식별자에 존재하는 속성(아래 참조)입니다. 일반적으로 이 식별자는 정의하는 **DependencyObject** 파생 클래스의 정적 멤버로 사용할 수 있습니다. |
| 종속성 속성 식별자 | [
            **SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361), 이 때문에 종속성 속성 식별자가 읽기 전용인 경우에도 일반적으로 공개 식별자입니다. |
| 속성 래퍼 | Windows 런타임 속성의 호출 가능한 **get** 및 **set** 구현. 또는 원래 정의의 언어별 프로젝션. **get** 속성 래퍼 구현은 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)를 호출하여 관련 종속성 속성 식별자를 첫 번째 입력으로 전달하고 설정할 값을 두 번째 입력으로 전달합니다. | 

속성 래퍼는 호출자에게 편의를 제공할 뿐만 아니라 속성에 대한 Windows 런타임 정의를 사용하는 모든 프로세스, 도구 또는 프로젝션에 종속성 속성을 노출합니다.

다음 예에서는 C#의 정의대로 사용자 지정 "IsSpinning" 종속성 속성을 정의하고 종속성 속성 식별자와 속성 래퍼의 관계를 보여 줍니다.

```csharp
// IsSpinningProperty is the dependency property identifier
// no need for info in the last PropertyMetadata parameter, so we pass null
public static readonly DependencyProperty IsSpinningProperty = 
    DependencyProperty.Register(
        "IsSpinning", typeof(Boolean),
        typeof(ExampleClass), null
    );
// The property wrapper, so that callers can use this property through a simple ExampleClassInstance.IsSpinning usage rather than requiring property system APIs
public bool IsSpinning
{
    get { return (bool)GetValue(IsSpinningProperty); }
    set { SetValue(IsSpinningProperty, value); }
}
```

**참고** 위의 예는 사용자 지정 종속성 속성을 만드는 방법에 대한 완전한 예가 아니며, 코드를 통한 개념 학습을 선호하는 사용자를 위해 종속성 속성 개념을 표시하기 위한 것입니다. 자세한 예를 보려면 [사용자 지정 종속성 속성](custom-dependency-properties.md)을 참조하세요.

## 종속성 속성 값 우선 순위

종속성 속성의 값을 가져오는 경우 Windows 런타임 속성 시스템에 참가하는 입력 중 하나를 통해 해당 속성에 결정된 값을 가져옵니다. Windows 런타임 속성 시스템에서 예측 가능한 방식으로 값을 계산할 수 있도록 종속성 속성 값 우선 순위가 있으며, 사용자는 기본적인 우선 순위 순서를 잘 알고 있어야 합니다. 그렇지 않으면 사용자가 특정 수준의 우선 순위로 속성을 설정하려고 하는데 다른 주체(시스템, 타사 호출자, 고유 코드의 일부)는 다른 수준으로 속성을 설정하는 상황이 발생할 수 있으며, 이런 경우 사용되는 속성 값 및 이 값의 출처를 알아내야 하는 번거로움을 겪게 됩니다.

예를 들어 스타일과 템플릿은 속성 값과 컨트롤의 모양을 설정하기 위한 공유 시작 지점이 될 수 있습니다. 그러나 특정 컨트롤 인스턴스에서 해당 값과 공통 템플릿 값을 변경하여 해당 컨트롤에 다른 배경색이나 다른 텍스트 문자열을 콘텐츠로 제공하려고 할 수 있습니다. Windows 런타임 속성 시스템은 스타일과 템플릿에서 제공되는 값보다 높은 우선 순위로 로컬 값을 고려합니다. 그러면 앱 특정 값을 사용하는 시나리오에서 템플릿을 덮어써서 앱 UI에서 컨트롤을 직접 사용하는 데 유용하도록 만들 수 있습니다.

### 종속성 속성 우선 순위 목록

속성 시스템에서 종속성 속성의 런타임 값을 할당할 때 사용하는 정의된 순서는 다음과 같습니다. 가장 높은 우선 순위가 가장 먼저 나와 있습니다. 이 목록 바로 뒤에서 더 자세한 설명을 찾을 수 있습니다.

1.  **애니메이션 효과를 준 값:** 활성 애니메이션, 시각적 상태 애니메이션 또는 [**HoldEnd**](https://msdn.microsoft.com/library/windows/apps/br210306) 동작이 있는 애니메이션. 실제적인 효과를 주려면 속성에 적용된 애니메이션이 기준(애니메이션 효과를 주지 않은) 값보다 우선해야 합니다. 이는 해당 값이 로컬로 설정된 경우에도 해당합니다.
2.  **로컬 값:** 로컬 값은 편리한 속성 래퍼를 통해 설정될 수 있습니다. 이는 XAML에서 특성 또는 속성 요소로 설정하거나 특정 인스턴스의 속성을 사용하여 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 메서드를 호출하는 것과도 같습니다. 바인딩이나 고정 리소스를 사용하여 로컬 값을 설정하는 경우 이러한 항목이 각각 로컬 값이 설정된 것처럼 우선 순위대로 작동하고 바인딩 또는 리소스 참조는 새 로컬 값이 설정되면 지워집니다.
3.  **템플릿 기반 속성:** 요소가 템플릿([**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) 또는 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348))의 일부로 만들어진 경우 요소에 템플릿 기반 속성이 있습니다.
4.  **스타일 setter:** 페이지 또는 응용 프로그램 리소스의 스타일에 있는 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817)의 값입니다.
5.  **기본값:** 종속성 속성에는 메타데이터의 일부로 기본값이 있을 수 있습니다.

### 템플릿 기반 속성

우선 순위 항목으로서의 템플릿 기반 속성은 XAML 페이지 태그에서 직접 선언하는 요소의 속성에 적용되지 않습니다. 템플릿 기반 속성 개념은 Windows 런타임이 XAML 템플릿을 UI 요소에 적용하여 시각 효과를 정의할 때 만들어지는 개체에 대해서만 존재합니다.

컨트롤 템플릿에서 설정된 모든 속성에는 일부 종류의 값이 있습니다. 이 값은 컨트롤의 기본값이 확장된 값 집합과 거의 같으며 나중에 직접 속성 값을 설정함으로써 다시 설정할 수 있는 값과 연결되는 경우가 많습니다. 따라서, 템플릿에서 설정된 값은 새 로컬 값에 의해 덮어쓸 수 있도록 실제 로컬 값과 구별할 수 있어야 합니다.

**참고**  
일부 경우 인스턴스에 대해 설정할 수 있어야 하는 속성에 대한 [{TemplateBinding} 태그 확장](templatebinding-markup-extension.md) 참조를 템플릿이 표시하지 못하면 템플릿이 로컬 값까지도 재정의할 수 있습니다. 이런 작업은 대체로 속성이 인스턴스에 대해 실제로 설정할 용도가 아닌 경우(예: 시각 효과 및 템플릿 동작과 관련 있으며 템플릿을 사용하는 컨트롤의 의도한 기능 또는 런타임 논리와 관련 없는 경우)에만 이루어집니다.

###  바인딩 및 우선 순위

바인딩 작업에는 사용되는 모든 범위에 대해 적절한 우선 순위가 있습니다. 예를 들어 로컬 값에 적용되는 바인딩은 로컬 값으로 작동하고 속성 setter에 대한 바인딩([{TemplateBinding} 태그 확장](templatebinding-markup-extension.md))은 스타일 setter가 적용될 때 적용됩니다. 바인딩은 런타임 시 데이터 원본에서 값을 가져올 때까지 기다려야 하기 때문에, 속성에 대한 속성 값 우선 순위를 결정하는 프로세스는 런타임으로도 확장됩니다.

바인딩은 로컬 값과 같은 우선 순위로 작동할 뿐만 아니라 실제로 로컬 값이며, 이 경우 바인딩은 지연된 값의 개체 틀입니다. 속성 값에 대한 바인딩이 기존에 있으며 이에 대해 런타임에 로컬 값을 설정하는 경우 바인딩이 완전히 대체됩니다. 마찬가지로, [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257)을 호출하여 런타임에만 존재하는 바인딩을 정의하는 경우 XAML에서 또는 이전에 실행한 코드를 사용하여 적용한 로컬 값이 대체됩니다.

### 스토리보드 애니메이션 및 기준 값

스토리보드 애니메이션은 *기준 값* 개념에 적용됩니다. 기준 값은 속성 시스템에서 우선 순위를 사용하고 애니메이션을 찾는 마지막 단계를 생략하여 결정하는 값입니다. 예를 들어, 기준 값은 컨트롤의 템플릿에서 가져올 수도 있고 컨트롤 인스턴스에 대한 로컬 값 설정에서 가져올 수도 있습니다. 어느 경우이든, 애니메이션을 적용하면 애니메이션이 계속 실행되는 한 이 기준 값을 덮어쓰고 애니메이션 효과를 준 값이 적용됩니다.

애니메이션 속성의 경우, 애니메이션이 **From** 및 **To**를 명시적으로 지정하지 않거나 완료 시 애니메이션이 속성을 기준 값으로 되돌리면 기준 값이 여전히 애니메이션의 동작에 영향을 줄 수 있습니다. 이런 경우 애니메이션의 실행이 중지되면 나머지 우선 순위가 다시 사용됩니다.

하지만 [**HoldEnd**](https://msdn.microsoft.com/library/windows/apps/br210306) 동작과 함께 **To**를 지정하는 애니메이션은 시각적으로 중지된 것처럼 보이는 경우에도 애니메이션이 제거될 때까지 로컬 값을 재정의할 수 있습니다. 이것은 개념적으로 UI에 시각적 애니메이션이 없는 경우에도 영원히 실행되는 애니메이션과 같습니다.

단일 속성에 여러 애니메이션을 적용할 수 있습니다. 이 애니메이션 각각은 서로 다른 값 우선 순위 지점에서 나온 기준 값을 대체하도록 정의되었을 수 있습니다. 하지만 이 애니메이션은 모두 런타임에 동시에 실행되며, 이는 종종 각 애니메이션이 값에 대해 가지는 영향이 동일하기 때문에 해당 값을 결합해야 한다는 것을 의미합니다. 이는 애니메이션의 정의 방법과 애니메이션 효과가 적용되는 값의 형식에 따라 달라집니다.

자세한 내용은 [스토리보드 애니메이션](https://msdn.microsoft.com/library/windows/apps/mt187354)을 참조하세요.

### 기본값

[
            **PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 값을 사용하여 종속성 속성의 기본값을 설정하는 작업은 [사용자 지정 종속성 속성](custom-dependency-properties.md) 항목에서 자세히 설명합니다.

종속성 속성은 속성 메타데이터에서 기본값이 명시적으로 정의되지 않은 경우에도 기본값을 가집니다. 메타데이터에서 변경되지 않은 경우 Windows 런타임 종속성 속성의 기본값은 일반적으로 다음 중 하나입니다.

-   런타임 개체나 기본 **Object** 형식(*참조 형식*)을 사용하는 속성의 기본값은 **null**입니다. 예를 들어 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713)는 의도적으로 설정되거나 상속될 때까지는 **null**입니다.
-   숫자 또는 부울 값과 같은 기준 값(*값 형식*)을 사용하는 속성은 해당 값에 대해 예상되는 기본값을 사용합니다. 예를 들어 정수 및 부동 소수점 숫자의 경우 0이고, 부울의 경우 **false**입니다.
-   Windows 런타임 구조를 사용하는 속성의 기본값은 해당 구조의 암시적 기본 생성자를 호출하여 가져옵니다. 이 생성자는 구조의 각 기준 값 필드에 대한 기본값을 사용합니다. 예를 들어 [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) 값의 기본값은 **X** 및 **Y** 값을 0으로 하여 초기화됩니다.
-   열거형을 사용하는 속성은 해당 열거형에 정의된 첫 번째 멤버의 기본값을 가집니다. 특정 열거형의 참조에서 기본값을 확인할 수 있습니다.
-   문자열(.NET의 경우 [**System.String**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.aspx), C++/CX의 경우 [**Platform::String**](https://msdn.microsoft.com/library/windows/apps/xaml/hh755812.aspx))을 사용하는 속성은 빈 문자열(**""**)의 기본값을 가집니다.
-   컬렉션 속성은 일반적으로 종속성 속성으로 구현되지 않으며, 그 이유에 대해서는 이 항목 후반에서 설명합니다. 하지만 사용자 지정 컬렉션 속성을 구현하는 경우 이 속성을 종속성 속성으로 만들려면 [사용자 지정 종속성 속성](custom-dependency-properties.md)의 끝부분에 나오는 설명대로 *의도하지 않은 단일 패턴*을 방지해야 합니다.

## 종속성 속성이 제공하는 속성 기능

### 데이터 바인딩

종속성 속성은 데이터 바인딩 적용을 통해 값을 설정할 수 있습니다. 데이터 바인딩은 XAML의 [{Binding} 태그 확장](binding-markup-extension.md) 구문이나 코드의 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 클래스를 사용합니다. 데이터 바인딩된 속성의 경우 최종 속성 값 결정은 런타임까지 지연됩니다. 런타임에 데이터 원본에서 값을 가져옵니다. 종속성 속성 시스템이 여기서 수행하는 역할은 값을 아직 모르는 경우 XAML 로딩과 같은 작업이 가능하도록 개체 틀 동작을 활성화한 다음 런타임에 Windows 런타임 데이터 바인딩 엔진과 상호 작용하여 값을 제공하는 것입니다.

다음 예에서는 XAML에서 바인딩을 사용하여 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 요소의 [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) 값을 설정합니다. 바인딩은 상속된 데이터 컨텍스트와 개체 데이터 원본을 사용합니다. 간략한 예에는 컨텍스트와 원본에 대한 내용이 나와 있지 않습니다. 컨텍스트와 원본을 보여 주는 보다 완전한 샘플을 보려면 [데이터 바인딩 심층 분석](https://msdn.microsoft.com/library/windows/apps/mt210946)을 참조하세요.

```XAML
<Canvas>
  <TextBlock Text="{Binding Team.TeamName}"/>
</Canvas>
```

XAML 대신 코드를 사용하여 바인딩을 설정할 수도 있습니다. [
            **SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257)을 참조하세요.

**참고** 이와 같은 바인딩은 종속성 속성 값 우선 순위에 따라 로컬 값으로 처리됩니다. 원래 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) 값이 포함된 속성에 대해 다른 로컬 값을 설정하면 바인딩의 런타임 값뿐만 아니라 바인딩을 완전히 덮어씁니다.

### 바인딩 소스, 바인딩 대상, FrameworkElement의 역할

바인딩의 소스가 되기 위해 속성이 종속성 속성일 필요는 없습니다. 일반적으로 어떠한 속성이든 바인딩 소스로 사용할 수 있습니다. 단, 사용된 프로그래밍 언어에 따라 다르며 개별적으로 특정 사례가 있습니다. 그러나 바인딩의 대상이 되려면 속성이 종속성 속성이어야 합니다.

코드에서 바인딩을 만드는 경우 [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257) API가 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)에 대해서만 정의됩니다. 그러나 [**BindingOperations**](https://msdn.microsoft.com/library/windows/apps/br209823)를 대신 사용하여 바인딩 정의를 만들 수 있으므로 어떠한 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 속성이든 참조할 수 있습니다.

코드나 XAML에서 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713)는 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 속성입니다. 바인딩 시스템은 부모-자식 속성 상속(일반적으로 XAML 태그에서 설정됨)의 형태를 사용하여 부모 요소에 존재하는 **DataContext**를 확인할 수 있습니다. 이 상속은 대상 속성을 가진 자식 개체가 **FrameworkElement**가 아니므로 고유 **DataContext** 값을 보유하지 않는 경우에도 평가할 수 있습니다. 그러나 상속되는 부모 요소는 **DataContext**를 설정하고 보유할 수 있으려면 **FrameworkElement**이어야 합니다. 또는 **DataContext**의 **null** 값을 사용하여 작동할 수 있도록 바인딩을 정의해야 합니다.

바인딩 연결은 대부분의 데이터 바인딩 시나리오에 필요한 여러 요소 중 하나입니다. 단방향 또는 양방향 바인딩이 적용되려면 원본 속성이 바인딩 시스템과 대상으로 전파되는 변경 알림을 지원해야 합니다. 사용자 지정 바인딩 원본의 경우 이는 속성이 [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx)를 지원해야 함을 의미합니다. 컬렉션은 [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx)를 지원해야 합니다. 특정 클래스는 해당 구현에서 이러한 인터페이스를 지원하므로 데이터 바인딩 시나리오를 위한 기본 클래스로 유용합니다. 이러한 클래스의 예는 [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx)입니다. 데이터 바인딩 및 데이터 바인딩과 속성 시스템의 관계에 대한 자세한 내용은 [데이터 바인딩 심층 분석](https://msdn.microsoft.com/library/windows/apps/mt210946)을 참조하세요.

**참고** 여기 표시된 형식은 Microsoft .NET 데이터 원본을 지원합니다. C++/CX 데이터 원본은 변경 알림 또는 식별 가능한 동작에 대해 다른 인터페이스를 사용합니다. [데이터 바인딩 심층 분석](https://msdn.microsoft.com/library/windows/apps/mt210946)을 참조하세요.

### 스타일 및 템플릿

스타일과 템플릿은 속성이 종속성 속성으로 정의되는 시나리오 중 두 가지입니다. 스타일은 앱 UI를 정의하는 속성을 설정하는 데 유용합니다. 스타일은 XAML에서 리소스([**Resources**](https://msdn.microsoft.com/library/windows/apps/br208740) 컬렉션 또는 테마 리소스 사전 같은 별도 XAML 파일의 항목)로 정의됩니다. 스타일과 속성 시스템에는 속성에 대한 setter가 포함되어 있으므로 스타일이 속성 시스템과 상호 작용합니다. 여기서 가장 중요한 속성은 [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390)의 [**Control.Template**](https://msdn.microsoft.com/library/windows/apps/br209465) 속성입니다. 이 속성은 **Control**의 시각적 모양과 시각적 상태를 대부분 정의합니다. 스타일에 대한 자세한 내용과 [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849)을 정의하고 setter를 사용하는 몇 가지 예제 XAML을 보려면 [컨트롤 스타일 지정](https://msdn.microsoft.com/library/windows/apps/mt210950)을 참조하세요.

스타일 또는 템플릿에서 가져온 값은 바인딩과 유사하게 지연된 값입니다. 이에 따라 컨트롤 사용자가 컨트롤의 템플릿을 다시 만들거나 스타일을 다시 정의할 수 있습니다. 이 때문에 스타일의 속성 setter는 일반 속성이 아니라 종속성 속성에만 작용될 수 있습니다.

### 스토리보드 애니메이션

스토리보드 애니메이션을 사용하여 종속성 속성 값에 애니메이션 효과를 줄 수 있습니다. Windows 런타임의 스토리보드 애니메이션은 단순히 시각적 데코레이션이 아닙니다. 애니메이션을 개별 속성이나 모든 속성의 값 및 컨트롤의 시각 표현을 설정하고 시간이 지남에 따라 이 값을 변경할 수 있는 상태 시스템 기법이라고 생각하면 더 편리합니다.

애니메이션 효과를 주려면 애니메이션의 대상 속성이 종속성 속성이어야 하고, 대상 속성의 값 형식이 기존 [**Timeline**](https://msdn.microsoft.com/library/windows/apps/br210517) 파생 애니메이션 형식 중 하나에 의해 지원되어야 합니다. [
            **Color**](https://msdn.microsoft.com/library/windows/apps/hh673723), [**Double**](T:System.Double) 및 [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)의 값은 보간 또는 키 프레임 기술을 사용하여 애니메이션 효과를 줄 수 있습니다. 대부분의 다른 값은 개별 **Object** 키 프레임을 사용하여 애니메이션 효과를 줄 수 있습니다.

애니메이션이 적용되고 실행되면 애니메이션 값이 속성이 가진 다른 값(로컬 값 등)보다 높은 우선 순위로 작동합니다. 애니메이션에는 애니메이션이 시각적으로 중지된 것처럼 보이는 경우에도 애니메이션이 속성 값에 적용되도록 하는 선택적 [**HoldEnd**](https://msdn.microsoft.com/library/windows/apps/br210306) 동작도 있습니다.

상태 시스템 원칙은 컨트롤에 대한 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/br209021) 상태 모델의 일부로 스토리보드 애니메이션을 사용하여 구현됩니다. 스토리보드 애니메이션에 대한 자세한 내용은 [스토리보드 애니메이션](https://msdn.microsoft.com/library/windows/apps/mt187354)을 참조하세요. **VisualStateManager** 및 컨트롤의 시각적 상태 정의에 대한 자세한 내용은 [시각적 상태에 대한 스토리보드 애니메이션](https://msdn.microsoft.com/library/windows/apps/xaml/jj819808) 또는 [빠른 시작: 컨트롤 템플릿](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)을 참조하세요.

### 속성 변경 동작

속성 변경 동작은 종속성 속성 용어의 "종속성" 부분에 대한 출처입니다. 다른 속성이 첫 번째 속성의 값에 영향을 미칠 수 있는 경우 유효한 속성 값을 유지하는 것은 많은 프레임워크에서 어려운 개발 문제입니다. Windows 런타임 속성 시스템에서 각 종속성 속성은 속성 값이 변경될 때마다 호출되는 콜백을 지정할 수 있습니다. 이 콜백을 사용하여 일반적으로 동기식으로 관련 속성 값을 알리거나 변경할 수 있습니다. 많은 기존 종속성 속성에는 속성 변경 동작이 있습니다. 유사한 콜백 동작을 사용자 지정 종속성 속성에 추가하고 고유한 속성 변경 콜백을 구현할 수도 있습니다. 예는 [사용자 지정 종속성 속성](custom-dependency-properties.md)을 참조하세요.

### 기본값 및 **ClearValue**

종속성 속성에는 속성 메타데이터의 일부로 정의된 기본값이 있을 수 있습니다. 종속성 속성의 경우 기본값은 속성을 처음 설정한 후에는 관련성이 없어집니다. 값 우선 순위에서 다른 결정자가 사라질 때마다 런타임에 기본값을 다시 적용할 수 있습니다. (종속성 속성 값 우선 순위에 대해서는 다음 섹션에서 설명합니다.) 예를 들어 속성에 적용되는 스타일 값이나 애니메이션을 의도적으로 제거한 후 값이 적당한 기본값이 되기를 원할 수 있습니다. 종속성 속성 기본값은 각 속성의 값을 추가 단계로 특정하게 지정하지 않고도 이 값을 제공할 수 있습니다.

이미 로컬 값을 사용하여 속성을 설정한 후에도 속성을 의도적으로 기본값으로 설정할 수 있습니다. 값을 기본값으로 다시 설정할 뿐 아니라 우선 순위에서 기본값을 재정의할 수 있지만 로컬 값이 아닌 다른 값을 사용하도록 설정하려면 [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) 메서드를 호출합니다(메서드 매개 변수로서 해당 속성 참조 지우기). 속성에서 기본값을 문자 그대로 사용하지 않게 설정하려는 경우도 있지만, 로컬 값을 지우고 기본값으로 되돌리면 컨트롤 템플릿의 스타일 setter에서 가져온 값을 사용하는 등 현재 적용하려는 다른 우선 순위 항목이 활성화될 수 있습니다.

## **DependencyObject** 및 스레딩

모든 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 인스턴스는 Windows 런타임 앱에 표시되는 현재 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)와 연결된 UI 스레드에 만들어야 합니다. 각 **DependencyObject**는 주 UI 스레드에 만들어야 하지만 개체는 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br230616)에 액세스하여 다른 스레드의 디스패처 참조를 사용하여 액세스할 수 있습니다. 그런 다음, [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 개체에서 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317)와 같은 메서드를 호출하고 UI 스레드에 대한 스레드 제한 사항의 규칙 내에서 코드를 실행할 수 있습니다.

[
            **DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)의 스레딩 측면은 일반적으로 UI 스레드에서 실행되는 코드만 종속성 속성의 값을 변경하거나 읽을 수 있다는 의미이므로 관련이 있습니다. 스레딩 문제는 보통 **async** 패턴 및 백그라운드 작업자 스레드를 올바르게 사용하는 일반적인 UI 코드로 방지할 수 있습니다. 일반적으로 직접 **DependencyObject** 유형을 정의하여 **DependencyObject**가 적합하지 않을 수 있는 데이터 원본이나 다른 시나리오에 사용하려고 하는 경우에만 **DependencyObject** 관련 스레딩 문제가 발생합니다.

## 관련 항목

**개념 자료**
* [사용자 지정 종속성 속성](custom-dependency-properties.md)
* [연결된 속성 개요](attached-properties-overview.md)
* [데이터 바인딩 심층 분석](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [스토리보드 애니메이션](https://msdn.microsoft.com/library/windows/apps/mt187354)
* [Windows 런타임 구성 요소 만들기](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [XAML 사용자 및 사용자 지정 컨트롤 샘플](http://go.microsoft.com/fwlink/p/?linkid=238581)
**종속성 속성 관련 API**
* [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)
* [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362)



<!--HONumber=Mar16_HO1-->


