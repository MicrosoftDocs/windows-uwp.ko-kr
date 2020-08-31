---
description: PropertyPath 클래스 및 문자열 구문을 사용 하 여 XAML 또는 코드에서 PropertyPath 값을 인스턴스화할 수 있습니다.
title: 속성 경로 구문
ms.assetid: FF3ECF47-D81F-46E3-BE01-C839E0398025
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5a3cac608bbc985bb8c0db0e0cece219b3c566a8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155037"
---
# <a name="property-path-syntax"></a>속성 경로 구문


[**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) 클래스 및 문자열 구문을 사용 하 여 XAML 또는 코드에서 **PropertyPath** 값을 인스턴스화할 수 있습니다. **PropertyPath** 값은 데이터 바인딩에서 사용 됩니다. Storyboarded 애니메이션을 대상으로 지정 하는 데 비슷한 구문이 사용 됩니다. 두 시나리오 모두에 대해 속성 경로는 궁극적으로 단일 속성으로 확인 되는 하나 이상의 개체 속성 관계 트래버스를 설명 합니다.

속성 경로 문자열을 XAML의 특성에 직접 설정할 수 있습니다. 동일한 문자열 구문을 사용 하 여 코드에서 [**바인딩을**](/uwp/api/Windows.UI.Xaml.Data.Binding) 설정 하는 [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) 을 생성 하거나 [**SetTargetProperty**](/uwp/api/windows.ui.xaml.media.animation.storyboard.settargetproperty)를 사용 하 여 코드에서 애니메이션 대상을 설정할 수 있습니다. 속성 경로를 사용 하는 Windows 런타임에는 데이터 바인딩 및 애니메이션 대상 지정의 두 가지 고유한 기능 영역이 있습니다. 애니메이션 대상 지정은 Windows 런타임 구현에서 기본 속성 경로 구문 값을 만들지 않지만 정보를 문자열로 유지 하지만 개체 속성 순회의 개념은 매우 유사 합니다. 데이터 바인딩 및 애니메이션 대상 지정은 각각 속성 경로를 약간 다르게 평가 하므로 각각에 대해 개별적으로 속성 경로 구문을 설명 합니다.

## <a name="property-path-for-objects-in-data-binding"></a>데이터 바인딩에서 개체의 속성 경로

Windows 런타임에서 종속성 속성의 대상 값에 바인딩할 수 있습니다. 데이터 바인딩의 원본 속성 값은 종속성 속성이 아니어도 됩니다. 비즈니스 개체의 속성 (예: Microsoft .NET 언어 또는 c + +로 작성 된 클래스) 일 수 있습니다. 또는 바인딩 값의 원본 개체는 이미 앱에서 정의한 기존 종속성 개체 일 수 있습니다. 소스는 간단한 속성 이름 또는 비즈니스 개체의 개체 그래프에 있는 개체 속성 관계의 순회를 통해 참조할 수 있습니다.

개별 속성 값에 바인딩하거나 목록 또는 컬렉션을 보유 하는 대상 속성에 바인딩할 수 있습니다. 소스가 컬렉션인 경우 또는 경로에서 컬렉션 속성을 지정 하는 경우 데이터 바인딩 엔진은 소스의 컬렉션 항목을 바인딩 대상에 일치 시킵니다. 그러면 해당 컬렉션의 특정 항목을 예상할 필요 없이 데이터 원본 컬렉션의 항목 목록으로 [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) 를 채우는 것과 같은 동작이 발생 합니다.

### <a name="traversing-an-object-graph"></a>개체 그래프 트래버스

개체 그래프에서 개체 속성 관계의 순회를 나타내는 구문의 요소는 점 (**.**) 문자입니다. 속성 경로 문자열의 각 점은 개체 (점 왼쪽)와 해당 개체의 속성 (점 오른쪽) 간의 나누기를 나타냅니다. 문자열은 왼쪽에서 오른쪽으로 계산 되어 여러 개체 속성 관계를 단계별로 실행할 수 있습니다. 예를 살펴보겠습니다.

``` syntax
"{Binding Path=Customer.Address.StreetAddress1}"
```

이 경로를 평가 하는 방법은 다음과 같습니다.

1.  데이터 컨텍스트 개체 (또는 같은 [**바인딩에서**](/uwp/api/Windows.UI.Xaml.Data.Binding)지정한 [**원본**](/uwp/api/windows.ui.xaml.data.binding.source) )가 "Customer" 라는 속성을 검색 합니다.
2.  "Customer" 속성의 값인 개체는 "Address" 라는 속성을 검색 합니다.
3.  "Address" 속성의 값인 개체는 "StreetAddress1" 라는 속성을 검색 합니다.

이러한 각 단계에서 값은 개체로 처리 됩니다. 바인딩이 특정 속성에 적용 되는 경우에만 결과 형식이 확인 됩니다. 이 예에서는 "주소"가 주소에 대 한 문자열의 부분을 노출 하지 않는 문자열 값일 경우 실패 합니다. 일반적으로 바인딩은 알려져 있고 의도적인 정보 구조가 있는 비즈니스 개체의 중첩 된 특정 속성 값을 가리킵니다.

### <a name="rules-for-the-properties-in-a-data-binding-property-path"></a>데이터 바인딩 속성 경로에 있는 속성에 대 한 규칙

-   속성 경로에서 참조 하는 모든 속성은 원본 비즈니스 개체에서 public 이어야 합니다.
-   End 속성 (경로의 마지막으로 명명 된 속성)은 public 이어야 하며 변경할 수 있어야 합니다. 정적 값에 바인딩할 수 없습니다.
-   이 경로가 양방향 바인딩의 [**경로**](/uwp/api/windows.ui.xaml.data.binding.path) 정보로 사용 되는 경우 end 속성은 읽기/쓰기가 가능 해야 합니다.

### <a name="indexers"></a>인덱서

데이터 바인딩의 속성 경로에는 인덱싱된 속성에 대 한 참조가 포함 될 수 있습니다. 이렇게 하면 정렬 된 목록/벡터 또는 사전/지도에 바인딩할 수 있습니다. 대괄호 " \[ \] " 문자를 사용 하 여 인덱싱된 속성을 표시 합니다. 이러한 대괄호의 내용은 정수 (순서 있는 목록) 또는 따옴표 붙지 않은 문자열 (사전) 중 하나일 수 있습니다. 키가 정수인 사전에 바인딩할 수도 있습니다. 개체 속성을 구분 하는 점을 사용 하 여 동일한 경로에 서로 다른 인덱싱된 속성을 사용할 수 있습니다.

예를 들어, 각 플레이어가 성에 따라 키가 지정 된 "팀" (주문 된 목록) 목록이 있는 비즈니스 개체를 생각해 보겠습니다. 두 번째 팀의 특정 플레이어에 대 한 속성 경로 예는 "팀 \[ 1" \] 입니다. 플레이어 \[ Smith \] ". 1은 "팀"에서 두 번째 항목을 나타내는 데 사용 됩니다. 목록은 0으로 인덱싱됩니다.

**참고**    C + + 데이터 소스에 대 한 인덱싱 지원은 제한 됩니다. 자세한 내용은 [데이터 바인딩을](../data-binding/data-binding-in-depth.md)참조 하세요.

### <a name="attached-properties"></a>연결된 속성

속성 경로에는 연결 된 속성에 대 한 참조가 포함 될 수 있습니다. 연결 된 속성의 식별 이름에는 이미 점이 포함 되어 있으므로 점이 개체 속성 단계로 처리 되지 않도록 괄호 안에 연결 된 속성 이름을 묶어야 합니다. 예를 들어 [**ZIndex**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) 를 바인딩 경로로 사용 하도록 지정 하는 문자열은 "(Canvas. ZIndex)"입니다. 연결 된 속성에 대 한 자세한 내용은 [연결 된 속성 개요](attached-properties-overview.md)를 참조 하세요.

### <a name="combining-property-path-syntax"></a>속성 경로 구문 조합

속성 경로 구문의 여러 요소를 단일 문자열로 결합할 수 있습니다. 예를 들어 데이터 원본에 이러한 속성이 있는 경우 인덱싱된 연결 속성을 참조 하는 속성 경로를 정의할 수 있습니다.

### <a name="debugging-a-binding-property-path"></a>바인딩 속성 경로 디버깅

속성 경로는 바인딩 엔진에서 해석 되 고 런타임에만 나타날 수 있는 정보를 사용 하기 때문에 개발 도구에서 기본 디자인 타임 또는 컴파일 시간 지원을 사용 하지 않고도 바인딩의 속성 경로를 디버깅 해야 하는 경우가 많습니다. 대부분의 경우 속성 경로를 확인 하지 못하는 런타임 결과는 바인딩 확인의 디자인 대체 동작 이므로 오류가 없는 빈 값입니다. 다행히 Microsoft Visual Studio는 바인딩 소스를 지정 하는 속성 경로의 일부를 확인 하지 못한 부분을 격리할 수 있는 디버그 출력 모드를 제공 합니다. 이 개발 도구 기능 사용에 대 한 자세한 내용은 [데이터 바인딩의 "디버깅" 섹션을 자세히](../data-binding/data-binding-in-depth.md#debugging)참조 하세요.

## <a name="property-path-for-animation-targeting"></a>애니메이션 대상 지정을 위한 속성 경로

애니메이션은 애니메이션이 실행 될 때 storyboarded 값이 적용 되는 종속성 속성을 대상으로 지정 합니다. 애니메이션 효과가 적용 될 속성이 있는 개체를 식별 하기 위해 애니메이션은 이름 ([x:Name 특성](x-name-attribute.md))에 따라 요소를 대상으로 지정 합니다. 일반적으로 [**Storyboard. TargetName**](/dotnet/api/system.windows.media.animation.storyboard.targetname)으로 식별 되는 개체에서 시작 하 여 애니메이션이 적용 되는 특정 종속성 속성 값으로 끝나는 속성 경로를 정의 해야 합니다. 해당 속성 경로는 [**system.windows.media.animation.storyboard.targetproperty**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms616983(v=vs.95))의 값으로 사용 됩니다.

XAML에서 애니메이션을 정의 하는 방법에 대 한 자세한 내용은 참조 [Storyboarded 애니메이션](../design/motion/storyboarded-animations.md)합니다.

## <a name="simple-targeting"></a>단순 대상 지정

대상 개체 자체에 있는 속성에 애니메이션 효과를 적용 하 고 해당 속성의 형식이 속성 값의 하위 속성이 아닌 해당 속성에 직접 애니메이션을 적용할 수 있는 경우에는 추가 한정자 없이 속성의 이름을 지정할 수 있습니다. 예를 들어 [**사각형과**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)같은 [**셰이프**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 하위 클래스를 대상으로 하 고 [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) 속성에 애니메이션 [**색**](/uwp/api/Windows.UI.Color) 을 적용 하는 경우 속성 경로는 "fill"이 될 수 있습니다.

## <a name="indirect-property-targeting"></a>간접 속성 대상 지정

대상 개체의 하위 속성인 속성에 애니메이션 효과를 적용할 수 있습니다. 즉, 개체 자체에 해당 하는 대상 개체의 속성이 있고 해당 개체에 속성이 있는 경우 해당 개체 속성 관계를 단계별로 실행 하는 방법을 설명 하는 속성 경로를 정의 해야 합니다. 하위 속성에 애니메이션을 적용할 개체를 지정할 때마다 속성 이름을 괄호로 묶고 *typename*으로 속성을 지정 합니다. *propertyname* 형식입니다. 예를 들어 대상 개체의 [**rendertransform**](/uwp/api/windows.ui.xaml.uielement.rendertransform) 속성의 개체 값을 지정 하려면 속성 경로의 첫 번째 단계로 "(UIElement rendertransform)"을 지정 합니다. [**변환**](/uwp/api/Windows.UI.Xaml.Media.Transform) 값에 직접 적용할 수 있는 애니메이션이 없기 때문에이는 아직 전체 경로가 아닙니다. 이 예에서는 이제 속성 경로를 완료 하 여 end 속성이 **Double** 값으로 애니메이션을 적용할 수 있는 **Transform** 하위 클래스의 속성 인 "(UIElement rendertransform)을 완료 합니다. (CompositeTransform.TranslateX)"

## <a name="specifying-a-particular-child-in-a-collection"></a>컬렉션에서 특정 자식 지정

컬렉션 속성에서 자식 항목을 지정 하려면 숫자 인덱서를 사용할 수 있습니다. \[ \] 정수 인덱스 값 주위에 대괄호 "" 문자를 사용 합니다. 사전이 아닌 정렬 된 목록만 참조할 수 있습니다. 컬렉션은 애니메이션이 적용 될 수 있는 값이 아니기 때문에 인덱서 사용은 속성 경로에서 끝 속성 일 수 없습니다.

예를 들어, 컨트롤의 [**배경**](/uwp/api/windows.ui.xaml.controls.control.background) 속성에 적용 되는 [**system.windows.media.lineargradientbrush.startpoint**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) 의 첫 번째 색 중지 색에 애니메이션 효과를 적용 하도록 지정 하려면 속성 경로: "(control. Background). (Gradientbrush가 아니며. GradientStops) \[ 0 \] . ( GradientStop) ". 인덱서의 마지막 단계가 아니라 마지막 단계에서 컬렉션에 있는 항목 0의 [**GradientStop**](/uwp/api/windows.ui.xaml.media.gradientstop.color) 속성을 참조 하 여 [**색**](/uwp/api/Windows.UI.Color) 에 애니메이션 효과를 적용 해야 하는지 확인 합니다.

## <a name="animating-an-attached-property"></a>연결 된 속성에 애니메이션 적용

일반적인 시나리오는 아니지만 연결 된 속성에 애니메이션 형식과 일치 하는 속성 값이 있는 한 연결 된 속성에는 애니메이션 효과를 적용할 수 있습니다. 연결 된 속성의 식별 이름에는 이미 점이 포함 되어 있으므로 점이 개체 속성 단계로 처리 되지 않도록 괄호 안에 연결 된 속성 이름을 묶어야 합니다. 예를 들어, 개체에 대 한 연결 된 [**행**](/dotnet/api/system.windows.controls.grid.row) 속성에 애니메이션 효과를 주려면 속성 경로 "(Grid. row)"를 사용 하는 문자열을 지정할 수 있습니다.

**참고**    이 예제에서 [**Grid. Row**](/dotnet/api/system.windows.controls.grid.row) 값은 **Int32** 속성 형식입니다. 따라서 **이중** 애니메이션으로 애니메이션 효과를 적용할 수 없습니다. 대신 [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame) 구성 요소를 포함 하는 [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) 를 정의 합니다. 여기서 [**objectkeyframe. 값**](/uwp/api/windows.ui.xaml.media.animation.objectkeyframe.value) 은 "0" 또는 "1"과 같은 정수로 설정 됩니다.

## <a name="rules-for-the-properties-in-an-animation-targeting-property-path"></a>애니메이션 대상 속성 경로의 속성에 대 한 규칙

-   속성 경로의 시작 지점은 [**Storyboard. TargetName**](/dotnet/api/system.windows.media.animation.storyboard.targetname)에 의해 식별 되는 개체입니다.
-   속성 경로를 따라 참조 되는 모든 개체와 속성은 public 이어야 합니다.
-   End 속성 (경로의 마지막으로 명명 된 속성)은 public 이어야 하 고, 읽기/쓰기가 가능 하 고, 종속성 속성 이어야 합니다.
-   End 속성에는 애니메이션 형식의 광범위 한 클래스 ([**색**](/uwp/api/Windows.UI.Color) 애니메이션, **이중** 애니메이션, [**점**](/uwp/api/Windows.Foundation.Point) 애니메이션, [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames)) 중 하나에서 애니메이션을 적용할 수 있는 속성 형식이 있어야 합니다.

## <a name="the-propertypath-class"></a>PropertyPath 클래스

[**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) 클래스는 바인딩의 기본 속성 형식입니다. 바인딩 시나리오에 대 한 [**경로입니다.**](/uwp/api/windows.ui.xaml.data.binding.path)

대부분의 경우 코드를 전혀 사용 하지 않고 XAML에서 [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) 을 적용할 수 있습니다. 그러나 경우에 따라 코드를 사용 하 여 **PropertyPath** 개체를 정의 하 고 런타임에이 개체를 속성에 할당할 수 있습니다.

[**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) 에는 [**PropertyPath (String)**](/uwp/api/windows.ui.xaml.propertypath.-ctor) 생성자가 있고 기본 생성자가 없습니다. 이 생성자에 전달 하는 문자열은 앞에서 설명한 대로 속성 경로 구문을 사용 하 여 정의 된 문자열입니다. 이는 또한 [**경로**](/uwp/api/windows.ui.xaml.data.binding.path) 를 XAML 특성으로 할당 하는 데 사용 하는 것과 동일한 문자열입니다. **PropertyPath** 클래스의 유일한 다른 API는 읽기 전용 [**경로**](/uwp/api/windows.ui.xaml.propertypath.path) 속성입니다. 이 속성은 다른 **PropertyPath** 인스턴스에 대 한 생성 문자열로 사용할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [데이터 바인딩 심층 분석](../data-binding/data-binding-in-depth.md)
* [스토리보드 애니메이션](../design/motion/storyboarded-animations.md)
* [{Binding} 태그 확장](binding-markup-extension.md)
* [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath)
* [**바인딩되**](/uwp/api/Windows.UI.Xaml.Data.Binding)
* [**바인딩 생성자**](/uwp/api/windows.ui.xaml.data.binding.-ctor)
* [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext)