---
description: PropertyPath 클래스 및 문자열 구문을 사용하여 XAML이나 코드에서 PropertyPath 값을 인스턴스화할 수 있습니다.
title: 속성 경로 구문
ms.assetid: FF3ECF47-D81F-46E3-BE01-C839E0398025
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f0f49792a92010f97c8388540fd63c38eed5f75e
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8199718"
---
# <a name="property-path-syntax"></a>속성 경로 구문


[**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) 클래스 및 문자열 구문을 사용하여 XAML이나 코드에서 **PropertyPath** 값을 인스턴스화할 수 있습니다. **PropertyPath** 값은 데이터 바인딩에 사용됩니다. 유사한 구문이 대상 스토리보드 애니메이션에 사용됩니다. 두 시나리오에서 속성 경로는 결국 단일 속성으로 확인되는 여러 개체-속성 관계의 통과를 설명합니다.

속성 경로 문자열을 XAML의 특성으로 직접 설정할 수 있습니다. 동일한 문자열 구문을 사용하면 코드에서 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)을 설정하는 [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)를 생성하거나 [**SetTargetProperty**](https://msdn.microsoft.com/library/windows/apps/br210503)를 사용하여 코드에서 애니메이션 대상을 설정할 수 있습니다. Windows 런타임에는 속성 경로를 사용하는 두 가지 고유한 기능 영역인 데이터 바인딩과 애니메이션 대상이 있습니다. 애니메이션 대상은 Windows 런타임 구현에서 기본 Property-path 구문 값을 만들지 않고 정보를 문자열로 유지하지만 개체-속성 통과의 개념이 매우 유사합니다. 데이터 바인딩과 애니메이션 대상은 각각 속성 경로를 약간 다르게 평가합니다. 따라서 각각의 경우에 대한 속성 경로 구문을 별도로 설명합니다.

## <a name="property-path-for-objects-in-data-binding"></a>데이터 바인딩의 개체에 대한 속성 경로

Windows 런타임에서는 종속성 속성의 대상 값에 바인딩할 수 있습니다. 데이터 바인딩의 원본 속성 값은 종속성 속성일 필요가 없으며 비즈니스 개체(예: Microsoft .NET 언어 또는 C++로 작성된 클래스)에 대한 속성일 수 있습니다. 또는 바인딩 값의 원본 개체는 앱에서 이미 정의한 기존 종속성 개체일 수 있습니다. 원본은 간단한 속성 이름이나 비즈니스 개체의 개체 그래프에서 개체-속성 관계의 통과로 참조할 수 있습니다.

개별 속성 값에 바인딩하거나 목록이나 컬렉션을 보유하는 대상 속성에 바인딩할 수 있습니다. 원본이 컬렉션이거나 경로에서 컬렉션 속성을 지정하는 경우 데이터 바인딩 엔진은 원본의 컬렉션 항목을 바인딩 대상과 일치시켜 해당 컬렉션의 특정 항목을 예상할 필요 없이 데이터 원본 컬렉션의 항목 목록으로 [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) 채우기와 같은 동작이 발생하도록 합니다.

### <a name="traversing-an-object-graph"></a>개체 그래프 트래버스

개체 그래프에서 개체-속성 관계의 통과를 나타내는 구문의 요소는 점(**.**) 문자입니다. 속성 경로 문자열의 각 점은 개체(점의 왼쪽)와 해당 개체의 속성(점의 오른쪽) 간 분할을 나타냅니다. 문자열은 왼쪽에서 오른쪽으로 평가되어 여러 개체-속성 관계를 단계별로 평가합니다. 예를 살펴보겠습니다.

``` syntax
"{Binding Path=Customer.Address.StreetAddress1}"
```

이 경로가 평가되는 방법은 다음과 같습니다.

1.  데이터 컨텍스트 개체(또는 동일한 [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)에서 지정된 [**Source**](https://msdn.microsoft.com/library/windows/apps/br209832))가 "Customer" 속성에 대해 검색됩니다.
2.  "Customer" 속성 값인 개체가 "Address" 속성에 대해 검색됩니다.
3.  "Address" 속성 값인 개체가 "StreetAddress1" 속성에 대해 검색됩니다.

이러한 각 단계에서 값은 개체로 처리됩니다. 결과의 유형은 바인딩이 특정 속성에 적용되는 경우에만 확인됩니다. "Address"가 단지 문자열 값이고 문자열에서 거리 주소 부분을 표시하지 않을 경우 이 예제는 실패합니다. 일반적으로 바인딩은 알려진 의도적 정보 구조를 갖는 비즈니스 개체의 특정 중첩 속성 값을 가리킵니다.

### <a name="rules-for-the-properties-in-a-data-binding-property-path"></a>데이터 바인딩 속성 경로의 속성에 대한 규칙

-   속성 경로에서 참조하는 모든 속성은 원본 비즈니스 개체에서 공용이어야 합니다.
-   끝 속성(경로에서 마지막으로 명명된 속성)은 공용이어야 하고 변경 가능해야 합니다. 정적 값에는 바인딩할 수 없습니다.
-   이 경로가 양방향 바인딩을 위한 [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) 정보로 사용되는 경우 끝 속성은 읽기/쓰기가 가능해야 합니다.

### <a name="indexers"></a>인덱서

데이터 바인딩의 속성 경로에는 인덱싱된 속성에 대한 참조가 포함될 수 있습니다. 따라서 순서가 지정된 목록/벡터 또는 사전/맵에 바인딩할 수 있습니다. 대괄호 "\[\]" 문자를 사용하여 인덱싱된 속성을 나타냅니다. 이러한 대괄호의 내용은 정수(순서가 지정된 목록의 경우)이거나 따옴표로 끝난 문자열(사전의 경우)일 수 있습니다. 키가 정수인 사전에 바인딩할 수도 있습니다. 동일한 경로에서 개체-속성을 구분하는 점을 사용하여 여러 가지 인덱싱된 속성을 사용할 수 있습니다.

예를 들어 "Teams" 목록(순서가 지정된 목록)이 있는 비즈니스 개체를 예로 들어 보겠습니다. 각 개체에는 "Players"라는 사전이 있으며 각 플레이어는 성별로 키가 지정되어 있습니다. 두 번째 팀의 특정 플레이어에 대한 예제 속성 경로는 "Teams\[1\].Players\[Smith\]"입니다. 목록의 인덱스가 0부터 시작되므로 1을 사용하여 "Teams"의 두 번째 항목을 나타냅니다.

**참고**c + + 데이터 원본에 대 한 인덱싱 지원은 제한 됩니다. [데이터 바인딩 심층 분석을](https://msdn.microsoft.com/library/windows/apps/mt210946)참조 하세요.

### <a name="attached-properties"></a>연결된 속성

속성 경로에는 연결된 속성에 대한 참조가 포함될 수 있습니다. 연결된 속성을 식별하는 이름에는 점이 이미 포함되어 있으므로 점이 개체-속성 단계로 처리되지 않도록 하려면 연결된 속성 이름을 괄호로 묶어야 합니다. 예를 들어 [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/hh759773)를 바인딩 경로로 사용하도록 지정하는 문자열은 "(Canvas.ZIndex)"입니다. 연결된 속성에 대한 자세한 내용은 [연결된 속성 개요](attached-properties-overview.md)를 참조하세요.

### <a name="combining-property-path-syntax"></a>속성 경로 구문 결합

속성 경로 구문의 다양한 요소를 단일 문자열로 결합할 수 있습니다. 예를 들어 데이터 원본에 인덱싱된 연결된 속성이 있는 경우 이러한 속성을 참조하는 속성 경로를 정의할 수 있습니다.

### <a name="debugging-a-binding-property-path"></a>바인딩 속성 경로 디버깅

속성 경로는 바인딩 엔진에서 해석되고 런타임에만 있을 수 있는 정보를 사용하므로 개발 도구에서 기본 디자인 타임 또는 컴파일 시간 지원을 사용할 수 없으면 바인딩의 속성 경로를 자주 디버그해야 합니다. 대부분의 경우 속성 경로를 확인하지 못하는 런타임 결과는 바인딩 확인의 의도된 폴백 동작이므로 오류가 없는 빈 값입니다. 하지만 Microsoft Visual Studio는 바인딩 소스를 지정하는 속성 경로에서 확인되지 못한 부분을 격리시킬 수 있는 디버그 출력 모드를 제공합니다. 이 개발 도구 기능을 사용하는 방법에 대한 자세한 내용은 [데이터 바인딩 심층 분석의 "디버깅" 섹션](../data-binding/data-binding-in-depth.md#debugging)을 참조하세요.

## <a name="property-path-for-animation-targeting"></a>애니메이션 대상에 대한 속성 경로

애니메이션은 애니메이션이 실행될 때 스토리보드 값이 적용되는 종속성 속성을 대상으로 합니다. 애니메이션할 속성이 있는 개체를 식별하기 위해 애니메이션은 이름([x:Name 특성](x-name-attribute.md))에 따라 요소를 대상으로 지정합니다. [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/hh759823)으로 식별되는 개체에서 시작하고 애니메이션이 적용되어야 하는 특정 종속성 속성 값으로 끝나는 속성 경로를 정의해야 하는 경우가 많습니다. 해당 속성 경로는 [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/hh759824)의 값으로 사용됩니다.

XAML에서 애니메이션을 정의하는 방법에 대한 자세한 내용은 [스토리보드 애니메이션](https://msdn.microsoft.com/library/windows/apps/mt187354)을 참조하세요.

## <a name="simple-targeting"></a>간단한 대상

대상 개체 자체에 있는 속성을 애니메이션하려는 경우 해당 속성의 유형에 속성 값의 하위 속성이 아닌 해당 속성에 직접 적용된 애니메이션이 있으면 추가 자격 부여 없이 애니메이션할 속성의 이름만 지정하면 됩니다. 예를 들어 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 같은 [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 하위 클래스를 대상으로 지정하고 애니메이션된 [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723)을 [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) 속성에 적용하려는 경우 속성 경로는 "Fill"이 될 수 있습니다.

## <a name="indirect-property-targeting"></a>간접 속성 대상

대상 개체의 하위 속성인 속성을 애니메이션할 수 있습니다. 즉, 대상 개체에 개체 자체인 속성이 있는 경우 해당 개체에 속성이 있으면 해당 개체-속성 관계를 단계별로 실행하는 방법을 설명하는 속성 경로를 정의해야 합니다. 하위 속성을 애니메이션하려는 개체를 지정할 때마다 속성 이름을 괄호로 묶고 *typename*.*propertyname* 형식으로 속성을 지정합니다. 예를 들어 대상 개체 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/br208980) 속성의 개체 값을 지정하려면 "(UIElement.RenderTransform)"을 속성 경로의 첫 번째 단계로 지정합니다. 아직 [**Transform**](https://msdn.microsoft.com/library/windows/apps/br243006) 값에 직접 적용할 수 있는 애니메이션이 없으므로 전체 경로가 아닙니다. 따라서 이 예제에서는 끝 속성이 **Double** 값 "(UIElement.RenderTransform).(CompositeTransform.TranslateX)"로 애니메이션할 수 있는 **Transform** 하위 클래스의 속성이 되도록 속성 경로를 완료합니다.

## <a name="specifying-a-particular-child-in-a-collection"></a>컬렉션에서 특정 자식 지정

컬렉션 속성에서 자식 항목을 지정하려면 숫자 인덱서를 사용할 수 있습니다. 정수 인덱스 값 주위에 대괄호("\[\]") 문자를 사용합니다. 사전이 아닌 순서가 지정된 목록만 참조할 수 있습니다. 컬렉션은 애니메이션할 수 있는 값이 아니므로 인덱서 사용은 속성 경로에서 끝 속성이 될 수 없습니다.

예를 들어 컨트롤의 [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) 속성에 적용되는 [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/br210108)에서 첫 번째 색 정지 색을 애니메이션하도록 지정하려는 경우 속성 경로는 "(Control.Background).(GradientBrush.GradientStops)\[0\].(GradientStop.Color)"입니다. 어떻게 인덱서가 경로의 마지막 단계가 될 수 없는지 확인하고 특히 마지막 단계에서는 컬렉션에 있는 항목 0의 [**GradientStop.Color**](https://msdn.microsoft.com/library/windows/apps/br210094) 속성을 참조하여 [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) 애니메이션 값을 적용해야 함을 확인합니다.

## <a name="animating-an-attached-property"></a>연결된 속성 애니메이션

일반적인 시나리오는 아니지만 연결된 속성에 애니메이션 유형과 일치하는 속성 값이 있으면 연결된 속성을 애니메이션할 수 있습니다. 연결된 속성을 식별하는 이름에는 점이 이미 포함되어 있으므로 점이 개체-속성 단계로 처리되지 않도록 하려면 연결된 속성 이름을 괄호로 묶어야 합니다. 예를 들어 개체의 [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795) 연결된 속성을 애니메이션하도록 지정하는 문자열은 속성 경로 "(Grid.Row)"를 사용합니다.

**참고**이 예제에서는 [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795) 의 값은 **Int32** 속성 형식. 따라서 **Double** 애니메이션으로 애니메이션할 수 없습니다. 대신 [**ObjectKeyFrame.Value**](https://msdn.microsoft.com/library/windows/apps/br210344)가 "0" 또는 "1" 같은 정수로 설정된 [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/br243132) 구성 요소가 있는 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320)를 정의했습니다.

## <a name="rules-for-the-properties-in-an-animation-targeting-property-path"></a>애니메이션 대상 속성 경로의 속성에 대한 규칙

-   속성 경로의 가정된 시작 지점은 [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/hh759823)으로 식별되는 개체입니다.
-   속성 경로를 따라 참조되는 모든 개체 및 속성은 공용이어야 합니다.
-   끝 속성(경로에서 마지막으로 명명된 속성)은 공용이어야 하며 읽기/쓰기 가능하고 종속성 속성이어야 합니다.
-   끝 속성에는 애니메이션 유형([**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) 애니메이션, **Double** 애니메이션, [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) 애니메이션, [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320))의 광범위한 클래스 중 하나로 애니메이션할 수 있는 속성 유형이 있어야 합니다.

## <a name="the-propertypath-class"></a>PropertyPath 클래스

[**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) 클래스는 바인딩 시나리오에 대한 [**Binding.Path**](https://msdn.microsoft.com/library/windows/apps/br209830)의 기본 속성 유형입니다.

대부분 코드를 전혀 사용하지 않고 XAML에서[**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)를 적용할 수 있습니다. 그러나 경우에 따라 코드를 사용하여 **PropertyPath** 개체를 정의하고 런타임에 속성에 할당해야 할 수 있습니다.

[**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)에는 [**PropertyPath(String)**](https://msdn.microsoft.com/library/windows/apps/br244261) 생성자가 있으며 기본 생성자는 없습니다. 이 생성자에 전달하는 문자열은 앞에서 설명한 대로 속성 경로 구문을 사용하여 정의된 문자열입니다. 또한 [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830)를 XAML 특성으로 할당하는 데 사용한 문자열과 동일한 문자열입니다. **PropertyPath** 클래스의 유일하게 다른 API는 읽기 전용인 [**Path**](https://msdn.microsoft.com/library/windows/apps/br244260) 속성입니다. 이 속성을 다른 **PropertyPath** 인스턴스의 생성 문자열로 사용할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [데이터 바인딩 심층 분석](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [스토리보드 애니메이션](https://msdn.microsoft.com/library/windows/apps/mt187354)
* [{Binding} 태그 확장](binding-markup-extension.md)
* [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)
* [**바인딩**](https://msdn.microsoft.com/library/windows/apps/br209820)
* [**바인딩 생성자**](https://msdn.microsoft.com/library/windows/apps/br209825)
* [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713)

