---
description: '이 항목에서는 UI에 대 한 XAML 정의와 함께 c + +, c # 또는 Visual Basic를 사용 하 여 Windows 런타임 앱을 작성할 때 사용할 수 있는 종속성 속성 시스템에 대해 설명 합니다.'
title: 종속성 속성 개요
ms.assetid: AD649E66-F71C-4DAA-9994-617C886FDA7E
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: db9d47fdef12e5d838c919b2b5b653ea00c1196d
ms.sourcegitcommit: f44f94c2ef41b33c1a9719fa7b303ec525d479b5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85382713"
---
# <a name="dependency-properties-overview"></a>종속성 속성 개요

이 항목에서는 UI에 대 한 XAML 정의와 함께 c + +, c # 또는 Visual Basic를 사용 하 여 Windows 런타임 앱을 작성할 때 사용할 수 있는 종속성 속성 시스템에 대해 설명 합니다.

## <a name="what-is-a-dependency-property"></a>종속성 속성 이란?

종속성 속성은 속성의 특수 한 형식입니다. 특히 속성의 값을 추적 하 고 Windows 런타임의 일부인 전용 속성 시스템의 영향을 받는 속성입니다.

종속성 속성을 지원 하기 위해 속성을 정의 하는 개체는 [**dependencyobject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) 여야 합니다. 즉, 해당 상속의 어딘가에 **dependencyobject** 기본 클래스가 있는 클래스입니다. XAML을 사용 하는 UWP 앱에 대 한 UI 정의에 사용 하는 대부분의 형식은 **DependencyObject** 하위 클래스가 되며 종속성 속성을 지원 합니다. 그러나 이름에 "XAML"이 없는 Windows 런타임 네임 스페이스에서 제공 되는 모든 형식은 종속성 속성을 지원 하지 않습니다. 이러한 형식의 속성은 속성 시스템의 종속성 동작이 없는 일반 속성입니다.

종속성 속성의 용도는 다른 입력 (다른 속성, 응용 프로그램을 실행 하는 동안 응용 프로그램 내에서 발생 하는 상태)에 따라 속성의 값을 계산 하는 시스템 방법을 제공 하는 것입니다. 이러한 다른 입력에는 다음이 포함 될 수 있습니다.

- 사용자 기본 설정과 같은 외부 입력
- 데이터 바인딩, 애니메이션 및 storyboard와 같은 just-in-time 속성 결정 메커니즘
- 리소스 및 스타일과 같은 여러 사용 템플릿 패턴
- 개체 트리의 다른 요소와 부모-자식 관계를 통해 알려진 값

종속성 속성은 UI 및 c #에 대 한 XAML을 사용 하 여 Windows 런타임 앱을 정의 하는 프로그래밍 모델의 특정 기능을 나타내거나 지원 합니다. 코드에 대해 Microsoft Visual Basic 또는 Visual C++ 구성 요소 확장 (c + +/CX)을 지원 합니다. 이러한 기능에는 다음이 포함됩니다.

- 데이터 바인딩
- 스타일
- 스토리보드 애니메이션
- "PropertyChanged" 동작; 종속성 속성을 구현 하 여 다른 종속성 속성에 변경 내용을 전파할 수 있는 콜백을 제공할 수 있습니다.
- 속성 메타 데이터에서 제공 하는 기본값 사용
- [**Clearvalue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.clearvalue) 및 metadata lookup과 같은 일반 속성 시스템 유틸리티

## <a name="dependency-properties-and-windows-runtime-properties"></a>종속성 속성 및 Windows 런타임 속성

종속성 속성은 런타임에 응용 프로그램의 모든 종속성 속성을 백업 하는 전역 내부 속성 저장소를 제공 하 여 기본 Windows 런타임 속성 기능을 확장 합니다. 이는 속성 정의 클래스에서 private 인 전용 필드를 사용 하 여 속성을 백업 하는 표준 패턴에 대 한 대안입니다. 이 내부 속성 저장소는 특정 개체에 대해 존재 하는 속성 식별자 및 값 집합 ( [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)인 경우) 이라고 생각할 수 있습니다. 이름으로 식별 되는 대신 저장소의 각 속성은 [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) 인스턴스로 식별 됩니다. 그러나 속성 시스템은 대개 이 구현 정보를 숨기므로, 사용자는 일반적으로 간단한 이름(사용하는 코드 언어의 프로그래밍 속성 이름 또는 XAML 작성 시의 특성 이름)을 사용하여 종속성 속성에 액세스할 수 있습니다.

종속성 속성 시스템의 기초가 되를 제공 하는 기본 형식은 [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)입니다. **Dependencyobject** 는 종속성 속성에 액세스할 수 있는 메서드를 정의 하 고, **dependencyobject** 파생 클래스의 인스턴스는 앞에서 언급 한 속성 저장소 개념을 내부적으로 지원 합니다.

종속성 속성을 설명할 때 설명서에서 사용 하는 용어의 합계는 다음과 같습니다.

| 용어 | Description |
|------|-------------|
| 종속성 속성 | [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) 식별자에 있는 속성 (아래 참조) 일반적으로이 식별자는 **DependencyObject** 파생 클래스를 정의 하는 정적 멤버로 사용할 수 있습니다. |
| 종속성 속성 식별자 | 속성을 식별 하는 상수 값입니다. 일반적으로 public이 고 읽기 전용입니다. |
| 속성 래퍼 | Windows 런타임 속성에 대 한 호출 가능 **get** 및 **set** 구현입니다. 또는 원래 정의에 대 한 언어별 프로젝션을 지정 합니다. **Get** 속성 래퍼 구현은 해당 종속성 속성 식별자를 전달 하 여 [**GetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getvalue)를 호출 합니다. |

속성 래퍼는 호출자가 편리 하 게 사용할 수 있는 것이 아니라 속성에 대 한 Windows 런타임 정의를 사용 하는 모든 프로세스, 도구 또는 프로젝션에도 종속성 속성을 노출 합니다.

다음 예제에서는 c #에 대해 정의 된 사용자 지정 종속성 속성을 정의 하 고 속성 래퍼에 대 한 종속성 속성 식별자의 관계를 보여 줍니다.

```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(string),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null)
);


public string Label
{
    get { return (string)GetValue(LabelProperty); }
    set { SetValue(LabelProperty, value); }
}
```

> [!NOTE]
> 앞의 예제는 사용자 지정 종속성 속성을 만드는 방법에 대 한 전체 예제로 제공 되지 않습니다. 코드를 통한 학습 개념을 선호 하는 모든 사용자에 대 한 종속성 속성 개념을 보여 주기 위한 것입니다. 이 예제에 대 한 자세한 설명은 [사용자 지정 종속성 속성](custom-dependency-properties.md)을 참조 하세요.

## <a name="dependency-property-value-precedence"></a>종속성 속성 값 우선 순위

종속성 속성의 값을 가져오면 Windows 런타임 속성 시스템에 참여 하는 입력 중 하나를 통해 해당 속성에 대해 결정 된 값을 가져옵니다. 종속성 속성 값 우선 순위는 Windows 런타임 속성 시스템에서 예측 가능한 방식으로 값을 계산할 수 있도록 하기 때문에 기본 우선 순위에 대해서도 잘 알고 있어야 합니다. 그렇지 않으면 한 수준 우선 순위로 속성을 설정 하려고 하지만 다른 수준에서 속성을 설정 하는 경우, 사용 되는 속성 값과 해당 값이 제공 되는 위치를 파악 하는 데 문제가 있는 경우를 생각해 볼 수 있을 것입니다.

예를 들어 스타일 및 템플릿은 속성 값을 설정 하 고 컨트롤의 모양을 공유 하기 위한 공유 시작점입니다. 그러나 특정 컨트롤 인스턴스에서 해당 값을 변경 하는 것이 좋습니다. 예를 들어 해당 값을 다른 배경색이 나 다른 텍스트 문자열을 콘텐츠로 지정 하는 등의 일반적인 템플릿 기반 값을 변경할 수 있습니다. Windows 런타임 속성 시스템은 스타일 및 템플릿에서 제공 하는 값 보다 우선 순위가 높은 로컬 값을 고려 합니다. 이를 통해 앱 별 값이 템플릿을 덮어써서 응용 프로그램 UI에서 컨트롤을 사용 하는 데 유용 하 게 사용할 수 있습니다.

### <a name="dependency-property-precedence-list"></a>종속성 속성 우선 순위 목록

다음은 속성 시스템에서 종속성 속성에 대 한 런타임 값을 할당할 때 사용 하는 결정적인 순서입니다. 가장 높은 우선 순위가 가장 먼저 나열됩니다. 이 목록 바로 뒤에 자세한 설명이 나와 있습니다.

1. **애니메이션 된 값:** [**HoldEnd**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FillBehavior) 동작을 사용 하는 활성 애니메이션, 시각적 상태 애니메이션 또는 애니메이션입니다. 실제적인 효과를 얻으려면 속성에 적용 된 애니메이션이 해당 값이 로컬로 설정 된 경우에도 기본 (애니메이션 되지 않은) 값 보다 우선 합니다.
1. **로컬 값:** 속성 래퍼를 편리 하 게 사용 하 여 로컬 값을 설정할 수 있습니다 .이는 XAML의 특성 또는 속성 요소로 설정 하거나 특정 인스턴스의 속성을 사용 하 여 [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue) 메서드를 호출 하는 것과 같습니다. 바인딩 또는 정적 리소스를 사용 하 여 로컬 값을 설정 하는 경우 이러한 각 값은 로컬 값이 설정 된 것 처럼 우선 순위에 따라 동작 하 고 새 로컬 값이 설정 된 경우 바인딩 또는 리소스 참조가 지워집니다.
1. **템플릿 속성:** [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 또는 [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate)에서 템플릿의 일부로 생성 된 요소는 이러한 요소를 포함 합니다.
1. **스타일 setter:** 페이지 또는 응용 프로그램 리소스의 스타일 내에 있는 [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter) 의 값입니다.
1. **기본값:** 종속성 속성은 메타 데이터의 일부로 기본값을 가질 수 있습니다.

### <a name="templated-properties"></a>템플릿 속성

템플릿 속성은 우선 순위 항목으로, XAML 페이지 태그에서 직접 선언 하는 요소의 속성에는 적용 되지 않습니다. 템플릿 속성 개념은 Windows 런타임가 XAML 템플릿을 UI 요소에 적용 하 여 해당 시각적 개체를 정의 하는 경우 생성 되는 개체에 대해서만 존재 합니다.

컨트롤 템플릿에서 설정 된 모든 속성에는 일종의 값이 있습니다. 이러한 값은 컨트롤의 확장 된 기본값 집합과 거의 비슷하며 속성 값을 직접 설정 하 여 나중에 다시 설정할 수 있는 값과 연결 되는 경우가 많습니다. 따라서 템플릿 집합 값은 실제 로컬 값과 구별 되어야 하므로 새 로컬 값을 덮어쓸 수 있습니다.

> [!NOTE]
> 경우에 따라 템플릿에서 설정할 수 있는 속성에 대해 [{TemplateBinding} 태그 확장](templatebinding-markup-extension.md) 참조를 노출 하지 못한 경우 템플릿이 로컬 값을 재정의할 수 있습니다. 이는 일반적으로 속성을 인스턴스에 설정 하지 않으려는 경우에만 수행 됩니다. 예를 들어 시각적 개체 및 템플릿 동작에만 관련 되 고 템플릿을 사용 하는 컨트롤의 의도 된 함수 또는 런타임 논리가 아닌 경우에만 수행 됩니다.

### <a name="bindings-and-precedence"></a>바인딩 및 우선 순위

바인딩 작업은 사용 되는 모든 범위에 대해 적절 한 우선 순위를 갖습니다. 예를 들어 로컬 값에 적용 되는 [{Binding}](binding-markup-extension.md) 은 로컬 값으로 작동 하며, 속성 setter의 [{TemplateBinding} 태그 확장이](templatebinding-markup-extension.md) 스타일 setter로 적용 됩니다. 바인딩은 데이터 원본에서 값을 가져올 때까지 기다려야 하므로 속성의 속성 값 우선 순위를 결정 하는 프로세스는 런타임에도 확장 됩니다.

바인딩 작업은 로컬 값과 동일한 우선 순위로 작동 하는 것이 아니라 로컬 값입니다. 여기서 바인딩은 지연 된 값의 자리 표시자입니다. 속성 값에 대 한 바인딩이 있는 경우 런타임에 바인딩을 완전히 대체 하는 로컬 값을 런타임에 설정 합니다. 마찬가지로, [**Setbinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.setbinding) 을 호출 하 여 런타임에만 존재 하는 바인딩을 정의 하는 경우 XAML 또는 이전에 실행 된 코드에 적용 했을 수 있는 모든 로컬 값을 바꿉니다.

### <a name="storyboarded-animations-and-base-value"></a>Storyboarded 애니메이션 및 기준 값

Storyboarded 애니메이션은 *기본 값*의 개념에 대해 작동 합니다. 기준 값은 해당 우선 순위를 사용 하 여 속성 시스템에 의해 결정 되는 값 이지만 애니메이션 검색의 마지막 단계를 생략 합니다. 예를 들어, 기본 값은 컨트롤의 템플릿에서 제공 될 수도 있고 컨트롤의 인스턴스에 대 한 로컬 값을 설정 하 여 가져올 수도 있습니다. 어떤 경우 든 애니메이션을 적용 하면이 기준 값이 덮어쓰므로 애니메이션이 계속 실행 되는 동안에는 애니메이션이 적용 된 값이 적용 됩니다.

애니메이션이 적용 된 속성의 경우 애니메이션의 동작에 영향을 미칠 수 있습니다. 해당 애니메이션 **에서** 및 **를**둘 다 명시적으로 지정 하지 않으면이 고, 애니메이션이 완료 되 면 속성을 기준 값으로 되돌리는 경우입니다. 이러한 경우 애니메이션이 더 이상 실행 되지 않으면 우선 순위가 다시 사용 됩니다.

그러나 [**HoldEnd**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FillBehavior) 동작을 사용 하 여 **를** 지정 하는 애니메이션은 애니메이션이 중지 된 것 처럼 보이는 경우에도 애니메이션이 제거 될 때까지 로컬 값을 재정의할 수 있습니다. 개념적으로이는 UI에 시각적 애니메이션이 없더라도 영구적으로 실행 되는 애니메이션 처럼 보입니다.

단일 속성에 여러 애니메이션을 적용할 수 있습니다. 이러한 각 애니메이션은 값 우선 순위의 다른 점에서 제공 된 기준 값을 대체 하도록 정의 되어 있을 수 있습니다. 그러나 이러한 애니메이션은 모두 런타임에 동시에 실행 되며 각 애니메이션이 값에 동일한 영향을 주므로 해당 값을 결합 해야 합니다. 결과는 애니메이션을 정의하는 정확한 방식과 애니메이션되는 값의 유형에 따라 달라집니다.

자세한 내용은 [Storyboarded 애니메이션](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations)(영문)을 참조 하세요.

### <a name="default-values"></a>기본값

[**PropertyMetadata**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyMetadata) 값을 사용 하 여 종속성 속성의 기본값을 설정 하는 방법은 [사용자 지정 종속성 속성](custom-dependency-properties.md) 항목에 자세히 설명 되어 있습니다.

종속성 속성에는 해당 기본값이 해당 속성의 메타 데이터에 명시적으로 정의 되지 않은 경우에도 기본값도 있습니다. 메타 데이터에 의해 변경 된 경우를 제외 하 고 Windows 런타임 종속성 속성의 기본값은 일반적으로 다음 중 하나입니다.

- 런타임 개체 또는 기본 **개체** 형식 ( *참조 형식*)을 사용 하는 속성의 기본값은 **null**입니다. 예를 들어, [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 는 의도적으로 설정 되거나 상속 될 때까지 **null** 입니다.
- 숫자 또는 부울 값 ( *값 형식*)과 같은 기본 값을 사용 하는 속성은 해당 값에 대해 예상 되는 기본값을 사용 합니다. 예를 들어 정수 및 부동 소수점 숫자의 경우 0, 부울의 경우 **false** 입니다.
- Windows 런타임 구조체를 사용 하는 속성에는 해당 구조체의 암시적 기본 생성자를 호출 하 여 얻은 기본값이 있습니다. 이 생성자는 구조체의 각 기본 값 필드에 대 한 기본값을 사용 합니다. 예를 들어 [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) 값의 기본값은 **X** 및 **Y** 값이 0으로 초기화 됩니다.
- 열거형을 사용 하는 속성에는 해당 열거형에 정의 된 첫 번째 멤버의 기본값이 있습니다. 특정 열거형의 참조를 확인 하 여 기본값이 무엇 인지 확인 합니다.
- 문자열 (.NET의 경우[**system.string**](https://docs.microsoft.com/dotnet/api/system.string) , [**Platform:: String**](https://docs.microsoft.com/cpp/cppcx/platform-string-class) for c + +/cx)을 사용 하는 속성의 기본값은 빈 문자열 (**""**)입니다.
- 컬렉션 속성은 일반적으로이 항목의 자세히 설명 된 이유로 종속성 속성으로 구현 되지 않습니다. 그러나 사용자 지정 컬렉션 속성을 구현 하 고 해당 속성을 종속성 속성으로 사용 하려면 [사용자 지정 종속성 속성](custom-dependency-properties.md)의 끝 부분에 설명 된 대로 의도 하지 않은 *singleton* 을 사용 하지 않도록 해야 합니다.

## <a name="property-functionality-provided-by-a-dependency-property"></a>종속성 속성에서 제공하는 속성 기능

### <a name="data-binding"></a>데이터 바인딩

종속성 속성의 값은 데이터 바인딩을 적용 하 여 설정할 수 있습니다. 데이터 바인딩은 XAML의 [{binding} 태그 확장](binding-markup-extension.md) 구문, [{x:bind} 태그 확장](x-bind-markup-extension.md) 또는 코드의 [**바인딩**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 클래스를 사용 합니다. 데이터 바인딩된 속성의 경우 최종 속성 값 결정은 런타임까지 지연 됩니다. 이때 데이터 원본에서 값을 가져옵니다. 종속성 속성 시스템에서 수행 하는 역할은 값이 아직 알려지지 않은 경우 XAML 로드와 같은 작업에 대 한 자리 표시자 동작을 활성화 한 다음 Windows 런타임 데이터 바인딩 엔진과 상호 작용 하 여 런타임에 값을 제공 하는 것입니다.

다음 예제에서는 XAML의 바인딩을 사용 하 여 [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 요소의 [**텍스트**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text) 값을 설정 합니다. 바인딩은 상속 된 데이터 컨텍스트와 개체 데이터 소스를 사용 합니다. 이러한 두 가지 방법은 축약 되지 않은 예제에 나와 있습니다. 컨텍스트 및 소스가 표시 되는 전체 샘플은 [심층 데이터 바인딩](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)을 참조 하세요.

```xaml
<Canvas>
  <TextBlock Text="{Binding Team.TeamName}"/>
</Canvas>
```

XAML이 아닌 코드를 사용 하 여 바인딩을 설정할 수도 있습니다. [**Setbinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.setbinding)을 참조 하십시오.

> [!NOTE]
> 이와 같은 바인딩은 종속성 속성 값 우선 순위를 위해 로컬 값으로 처리 됩니다. 원래 [**바인딩**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 값을 보유 한 속성에 대해 다른 로컬 값을 설정 하는 경우 바인딩의 런타임 값 뿐만 아니라 바인딩을 완전히 덮어씁니다. X:Bind 바인딩은 속성의 로컬 값을 설정 하는 생성 된 코드를 사용 하 여 구현 됩니다. {X:Bind}를 사용 하는 속성에 대 한 로컬 값을 설정 하면 해당 값이 원본 개체의 속성 변경 내용을 준수 하는 경우와 같이 다음에 바인딩이 평가 될 때 대체 됩니다.

### <a name="binding-sources-binding-targets-the-role-of-frameworkelement"></a>바인딩 소스, 바인딩 대상, FrameworkElement의 역할

바인딩의 소스인 경우 속성은 종속성 속성이 될 필요가 없습니다. 일반적으로 모든 속성을 바인딩 소스로 사용할 수 있습니다. 하지만이는 프로그래밍 언어에 따라 다르며 각에는 특정에 지 사례가 있습니다. 그러나 [{binding} 태그 확장](binding-markup-extension.md) 또는 [**바인딩의**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)대상이 되려면 해당 속성은 종속성 속성 이어야 합니다. {x:Bind}에서는 생성 된 코드를 사용 하 여 바인딩 값을 적용할 때이 요구 사항이 없습니다.

코드에서 바인딩을 만드는 경우 [**setbinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.setbinding) API는 [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement)에 대해서만 정의 됩니다. 그러나 대신 [**Bindingoperations**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindingOperations) 를 사용 하 여 바인딩 정의를 만들 수 있으므로 모든 [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) 속성을 참조할 수 있습니다.

코드 또는 XAML의 경우 [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 는 [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 속성입니다. 부모-자식 속성 상속의 형태 (일반적으로 XAML 태그에서 설정 됨)를 사용 하 여 바인딩 시스템은 부모 요소에 있는 **DataContext** 를 확인할 수 있습니다. 이 상속은 대상 속성이 있는 자식 개체가 **FrameworkElement** 가 아니므로 자체 **DataContext** 값을 포함 하지 않는 경우에도 평가할 수 있습니다. 그러나 상속 되는 부모 요소는 **DataContext**를 설정 하 고 유지 하기 위해 **FrameworkElement** 여야 합니다. 또는 **DataContext**에 대해 **null** 값을 사용 하 여 작동할 수 있도록 바인딩을 정의 해야 합니다.

바인딩 연결은 대부분의 데이터 바인딩 시나리오에 필요 하지 않습니다. 단방향 또는 양방향 바인딩을 적용 하려면 소스 속성이 바인딩 시스템에 전파 되는 변경 알림을 지원 해야 합니다. 사용자 지정 바인딩 원본의 경우 속성이 종속성 속성 이거나 개체가 [**INotifyPropertyChanged**](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged)을 지원 해야 함을 의미 합니다. 컬렉션은 [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged)를 지원 해야 합니다. 특정 클래스는 구현에서 이러한 인터페이스를 지원 하므로 데이터 바인딩 시나리오에 대 한 기본 클래스로 유용 하 게 사용할 수 있습니다. 이러한 클래스의 예로는 [**System.collections.objectmodel.observablecollection &lt; T &gt; **](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1)가 있습니다. 데이터 바인딩에 대 한 자세한 내용 및 데이터 바인딩이 속성 시스템과 관련 되는 방식에 대 한 자세한 내용은 [데이터 바인딩 심층](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)분석을 참조 하세요.

> [!NOTE]
> 여기에 나열 된 형식은 Microsoft .NET 데이터 소스를 지원 합니다. C + +/CX 데이터 소스는 다른 인터페이스를 사용 하 여 변경 알림이나 관찰 가능 동작을 사용 합니다. 자세한 내용은 [데이터 바인딩](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)을 참조 하세요.

### <a name="styles-and-templates"></a>스타일 및 템플릿

스타일 및 템플릿은 종속성 속성으로 정의 되는 속성에 대 한 두 가지 시나리오입니다. 스타일은 응용 프로그램의 UI를 정의 하는 속성을 설정 하는 데 유용 합니다. 스타일은 [**리소스**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources) 컬렉션의 항목 또는 테마 리소스 사전과 같은 별도의 xaml 파일에서 xaml의 리소스로 정의 됩니다. 스타일은 속성에 대 한 setter를 포함 하므로 속성 시스템과 상호 작용 합니다. 여기서 가장 중요 한 속성 [**은 컨트롤의**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) [**컨트롤 템플릿**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.template) 속성입니다 **. 컨트롤에**대 한 시각적 모양 및 시각적 상태를 대부분 정의 합니다. 스타일에 대 한 자세한 내용 및 [**스타일**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) 을 정의 하 고 setter를 사용 하는 몇 가지 예제 XAML은 [컨트롤 스타일](https://docs.microsoft.com/windows/uwp/controls-and-patterns/styling-controls)지정을 참조 하세요.

스타일 또는 템플릿에서 제공 되는 값은 바인딩과 유사 하 게 지연 된 값입니다. 이는 컨트롤 사용자가 템플릿 컨트롤을 다시 정의 하거나 스타일을 재정의할 수 있도록 하기 위한 것입니다. 이러한 이유로 스타일의 속성 setter는 일반 속성이 아닌 종속성 속성 에서만 작동할 수 있습니다.

### <a name="storyboarded-animations"></a>스토리보드 애니메이션

Storyboarded 애니메이션을 사용 하 여 종속성 속성의 값에 애니메이션 효과를 줄 수 있습니다. Windows 런타임 Storyboarded 애니메이션은 단지 시각적 장식이 아닌 것입니다. 애니메이션은 개별 속성이 나 컨트롤의 모든 속성 및 시각적 개체의 값을 설정 하 고 시간에 따라 이러한 값을 변경할 수 있는 상태 시스템 기술 이라고 생각 하는 것이 더 유용 합니다.

애니메이션을 적용 하려면 애니메이션의 대상 속성이 종속성 속성 이어야 합니다. 또한 애니메이션을 적용 하려면 대상 속성의 값 형식이 기존 [**타임 라인**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline)파생 애니메이션 형식 중 하나에서 지원 되어야 합니다. 보간 또는 키 프레임 기법을 사용 하 여 [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color), [**Double**](https://docs.microsoft.com/dotnet/api/system.double) 및 [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) 의 값에 애니메이션 효과를 적용할 수 있습니다. 불연속 **개체** 키 프레임을 사용 하 여 대부분의 다른 값에 애니메이션 효과를 적용할 수 있습니다.

애니메이션을 적용 하 고 실행 하면 애니메이션이 적용 된 값은 속성에 포함 된 값 (예: 로컬 값) 보다 높은 우선 순위로 작동 합니다. 애니메이션에는 애니메이션이 시각적으로 중지 된 것 처럼 보이는 경우에도 애니메이션이 속성 값에 적용 될 수 있는 선택적 [**HoldEnd**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FillBehavior) 동작도 있습니다.

상태 시스템 원칙은 컨트롤에 대한 [**VisualStateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualStateManager) 상태 모델의 일부로 스토리보드 애니메이션을 사용하여 구현됩니다. Storyboarded 애니메이션에 대 한 자세한 내용은 [storyboarded 애니메이션](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations)을 참조 하세요. 컨트롤의 시각적 상태를 **r** 하 고 정의 하는 방법에 대 한 자세한 내용은 [시각적 상태의 Storyboarded 애니메이션](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)) 또는 [컨트롤 템플릿](../design/controls-and-patterns/control-templates.md)을 참조 하세요.

### <a name="property-changed-behavior"></a>속성 변경 동작

속성 변경 동작은 종속성 속성 용어의 "종속성" 부분의 원점입니다. 다른 속성이 첫 번째 속성의 값에 영향을 줄 수 있는 경우 속성의 유효한 값을 유지 하는 것은 많은 프레임 워크에서 어려운 개발 문제입니다. Windows 런타임 속성 시스템에서 각 종속성 속성은 속성 값이 변경 될 때마다 호출 되는 콜백을 지정할 수 있습니다. 이 콜백은 일반적으로 동기 방식으로 관련 속성 값을 알리거나 변경 하는 데 사용할 수 있습니다. 기존의 많은 종속성 속성은 속성 변경 동작을 포함 합니다. 또한 사용자 지정 종속성 속성에 비슷한 콜백 동작을 추가 하 고 고유한 속성 변경 콜백을 구현할 수 있습니다. 예제는 [사용자 지정 종속성 속성](custom-dependency-properties.md) 을 참조 하세요.

Windows 10에서는 [**Registerpropertychangedcallback**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.registerpropertychangedcallback) 메서드를 소개 합니다. 이렇게 하면 [**DependencyObject**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject)인스턴스에서 지정 된 종속성 속성이 변경 될 때 응용 프로그램 코드에서 변경 알림을 등록할 수 있습니다.

### <a name="default-value-and-clearvalue"></a>Default value 및 **Clearvalue**

종속성 속성에는 속성 메타 데이터의 일부로 정의 된 기본값이 있을 수 있습니다. 종속성 속성의 경우 속성이 처음으로 설정 된 후에는 해당 기본값이 관련이 없는 상태가 됩니다. 기본값은 값 우선 순위의 다른 행렬식이 사라질 때마다 런타임에 다시 적용 될 수 있습니다. 종속성 속성 값 우선 순위는 다음 섹션에서 설명 합니다. 예를 들어 속성에 적용 되는 스타일 값 또는 애니메이션을 의도적으로 제거할 수 있지만 이렇게 한 후에는 값을 적절 한 기본값으로 지정할 수 있습니다. 종속성 속성 기본값은 특정 속성의 값을 추가 단계로 설정 하지 않고도이 값을 제공할 수 있습니다.

로컬 값을 사용 하 여 속성을 이미 설정한 후에도 속성을 기본값으로 설정할 수 있습니다. 값을 기본값으로 다시 설정 하 고 기본값을 재정의할 수 있는 다른 참여자를 우선 순위에 따라 설정 하려면 (메서드 매개 변수로 지울 속성 참조) [**clearvalue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.clearvalue) 메서드를 호출 합니다. 속성은 항상 기본값을 그대로 사용 하는 것을 원하지는 않지만 로컬 값을 지우고 기본값으로 되돌리면 우선 순위를 지정할 다른 항목 (예: 컨트롤 템플릿의 스타일 setter에서 가져온 값 사용)을 사용할 수 있습니다.

## <a name="dependencyobject-and-threading"></a>**DependencyObject** 및 스레딩

모든 [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) 인스턴스는 Windows 런타임 앱에 표시 되는 현재 [**창과**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) 연결 된 UI 스레드에서 만들어야 합니다. 각 **DependencyObject** 를 주 UI 스레드에 만들어야 하지만 [**디스패처**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) 속성에 액세스 하 여 다른 스레드의 디스패처 참조를 사용 하 여 개체에 액세스할 수 있습니다. 그런 다음 [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) 개체의 [**runasync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) 와 같은 메서드를 호출 하 고 UI 스레드에 대 한 스레드 제한 규칙 내에서 코드를 실행할 수 있습니다.

[**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) 의 스레딩 측면은 일반적으로 UI 스레드에서 실행 되는 코드만 변경 하거나 종속성 속성의 값을 읽을 수 있다는 것을 의미 하기 때문에 관련이 있습니다. 스레드 문제는 일반적으로 **비동기** 패턴 및 백그라운드 작업자 스레드를 올바르게 사용 하는 일반적인 UI 코드에서 피할 수 있습니다. 일반적으로 사용자 고유의 **dependencyobject** 형식을 정의 하 고이를 데이터 원본 또는 **dependencyobject** 가 반드시 적절 하지 않아도 되는 다른 시나리오에 사용 하려는 경우에만 **dependencyobject**관련 스레딩 문제를 실행 합니다.

## <a name="related-topics"></a>관련 항목

### <a name="conceptual-material"></a>개념 자료

- [사용자 지정 종속성 속성](custom-dependency-properties.md)
- [연결된 속성 개요](attached-properties-overview.md)
- [데이터 바인딩 심층 분석](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)
- [스토리보드 애니메이션](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations)
- [Windows 런타임 구성 요소 만들기](https://docs.microsoft.com/previous-versions/windows/apps/hh441572(v=vs.140))
- [XAML 사용자 및 사용자 지정 컨트롤 샘플](https://code.msdn.microsoft.com/windowsapps/XAML-user-and-custom-a8a9505e)

## <a name="apis-related-to-dependency-properties"></a>종속성 속성과 관련 된 Api

- [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)
- [**에서만**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty)

