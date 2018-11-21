---
author: jwmsft
description: XAML의 연결된 속성에 대한 개념을 설명하고 몇 가지 예를 제공합니다.
title: 연결된 속성 개요
ms.assetid: 098C1DE0-D640-48B1-9961-D0ADF33266E2
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: a0950bfc9d90ba893be8ca52cc295b38b142798e
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7433609"
---
# <a name="attached-properties-overview"></a>연결된 속성 개요

*연결된 속성*은 XAML 개념입니다. 연결된 속성을 통해 추가 속성/값 쌍이 개체에 설정될 수 있지만 속성은 원본 개체 정의의 일부가 아닙니다. 일반적으로 연결된 속성은 소유자 형식의 개체 모델에 기존의 속성 래퍼가 없는 특수한 형태의 종속성 속성으로 정의됩니다.

## <a name="prerequisites"></a>사전 요구 사항

종속성 속성의 기본 개념을 이해하고 있고 [종속성 속성 개요](dependency-properties-overview.md)를 읽었다고 간주합니다.

## <a name="attached-properties-in-xaml"></a>XAML의 연결된 속성

XAML에서는 _AttachedPropertyProvider.PropertyName_ 구문을 사용하여 연결된 속성을 설정합니다. 다음은 XAML에서 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771)를 설정할 수 있는 방법을 보여 주는 예입니다.

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

> [!NOTE]
> 사용 이유를 자세히 설명 하지 않고 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) 예제에서는 연결 된 속성으로 사용 하면 하겠습니다. **Canvas.Left**의 용도와 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)에서 해당 레이아웃 자식을 처리하는 방법에 대해 자세히 알아보려면 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 참조 항목이나 [XAML을 사용하여 레이아웃 정의](https://msdn.microsoft.com/library/windows/apps/mt228350)를 참조하세요.

## <a name="why-use-attached-properties"></a>연결된 속성을 사용하는 이유

연결된 속성은 관계된 여러 개체가 런타임에 서로 정보를 전달할 수 없도록 하는 코딩 규칙을 이스케이프하는 방법입니다. 각 개체가 해당 속성만 가져오고 설정할 수 있도록 공통 기본 클래스에 속성을 배치할 수 있습니다. 그러나 이 작업을 수행하는 시나리오 수가 증가할수록 결국 기본 클래스가 공유 가능한 속성들로 꽉 차게 됩니다. 수백 개의 하위 항목 중 두 개만 속성을 사용하는 경우가 발생할 수도 있습니다. 이것은 바람직한 클래스 디자인이 아닙니다. 이 문제를 해결하려면 연결된 속성 개념을 통해 개체가 해당 클래스 구조에서 정의되지 않은 속성에 대해 값을 할당할 수 있도록 합니다. 개체 트리에서 여러 개체를 만든 후에는 정의 클래스가 런타임에 자식 개체에서 값을 읽을 수 있습니다.

예를 들어 자식 요소는 연결된 속성을 사용하여 해당 부모 요소에 UI에 표시되는 방법을 알릴 수 있습니다. [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) 연결된 속성이 이 경우에 해당됩니다. **Canvas.Left**는 **Canvas** 자체가 아니라 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 요소에 포함된 요소에 대해 설정되기 때문에 연결된 속성으로 만들어집니다. 모든 가능한 자식 요소는 **Canvas.Left** 및 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772)을 사용하여 **Canvas** 레이아웃 컨테이너 부모 내에서 해당 레이아웃 오프셋을 지정합니다. 연결된 속성을 사용하면 많은 가능한 레이아웃 컨테이너 중 하나에만 각각 적용되는 다양한 속성으로 기본 요소 개체 모델을 복잡하게 만들지 않고 이 시나리오가 작동할 수 있습니다. 대신 많은 레이아웃 컨테이너가 연결된 속성 집합을 자체적으로 구현합니다.

연결된 속성을 구현하기 위해 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 클래스는 정적 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 필드 [**Canvas.LeftProperty**](https://msdn.microsoft.com/library/windows/apps/br209272)를 정의합니다. 그런 다음, **Canvas**는 [**SetLeft**](https://msdn.microsoft.com/library/windows/apps/br209273) 및 [**GetLeft**](https://msdn.microsoft.com/library/windows/apps/br209269) 메서드를 연결된 속성의 공개 접근자로 제공하여 XAML 설정 및 런타임 값 액세스를 모두 가능하게 합니다. XAML과 종속성 속성 시스템의 경우 이 API 집합은 연결된 속성의 특정 XAML 구문을 가능하게 하고 종속성 속성 저장소에 값을 저장하는 패턴을 충족합니다.

## <a name="how-the-owning-type-uses-attached-properties"></a>소유 형식에서 연결된 속성을 사용하는 방법

모든 XAML 요소(또는 기본 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356))에 대해 연결된 속성을 설정할 수 있지만 이것이 속성을 설정하면 가시적인 결과가 발생하거나 속성 값이 액세스됨을 자동으로 의미하지는 않습니다. 연결된 속성을 정의하는 형식은 대개 다음 시나리오 중 하나를 따릅니다.

- 연결된 속성을 정의하는 형식이 다른 개체의 관계에서 부모입니다. 자식 개체가 연결된 속성의 값을 설정합니다. 연결된 속성 소유자 형식에 자식 요소를 반복하고 값을 가져온 다음 개체 수명의 일정 시점에서 해당 값에 대해 작업하는 몇 가지 고유 동작(레이아웃 작업, [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br208742) 등)이 있습니다.
- 연결된 속성을 정의하는 형식이 다양한 범위의 가능한 부모 요소와 콘텐츠 모델의 자식 요소로 사용되지만 정보가 반드시 레이아웃 정보인 것은 아닙니다.
- 연결된 속성이 다른 UI 요소가 아니라 서비스에 정보를 보고합니다.

이러한 시나리오 및 소유 형식에 대한 자세한 내용은 [사용자 지정 연결된 속성](custom-attached-properties.md)의 "Canvas.Left에 대한 자세한 정보" 섹션을 참조하세요.

## <a name="attached-properties-in-code"></a>코드의 연결된 속성

연결된 속성은 다른 종속성 속성과 달리 쉬운 get 및 set 액세스를 위해 일반적인 속성 래퍼를 갖고 있지 않습니다. 이는 연결된 속성이 자신이 설정되는 인스턴스에 대한 코드 중심 개체 모델의 일부일 필요는 없기 때문입니다. 다른 형식이 자신에 대해 설정할 수 있는 연결된 속성이기도 하고 소유 형식에 기존의 속성 사용법도 적용되는 속성을 정의하는 것이 일반적이지는 않지만 허용됩니다.

속성 시스템 API를 사용하거나 XAML 패턴 접근자를 사용하는 방법으로 코드에서 연결된 속성을 설정할 수 있습니다. 두 방법은 최종 결과 측면에서 거의 동일하므로 어느 방법을 사용할지는 주로 코딩 스타일의 문제입니다.

### <a name="using-the-property-system"></a>속성 시스템 사용

Windows 런타임의 연결된 속성은 종속성 속성으로 구현되므로 값은 속성 시스템에 의해 공유 종속성 속성 저장소에 저장될 수 있습니다. 따라서 연결된 속성은 소유 클래스에서 종속성 속성 식별자를 노출합니다.

코드에서 연결된 속성을 설정하려면 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 메서드를 호출하고 연결된 속성의 식별자 역할을 하는 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 필드를 전달합니다. 설정할 값을 전달하기도 했습니다.

코드에서 연결된 속성의 값을 가져오려면 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 메서드를 호출하고 식별자 역할을 하는 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 필드를 다시 전달합니다.

### <a name="using-the-xaml-accessor-pattern"></a>XAML 접근자 패턴 사용

XAML 프로세서는 XAML이 개체 트리로 구문 분석될 때 연결된 속성 값을 설정할 수 있어야 합니다. 연결 된 속성의 소유자 형식 형태로 명명 된 전용된 접근자 메서드를 구현 해야 **가져오기 * PropertyName* 및 **설정 * * * PropertyName*합니다. 이러한 전용 접근자 메서드는 코드에서 연결된 속성을 가져오거나 설정하는 방법이기도 합니다. 코드 관점에서 연결된 속성은 속성 접근자 대신 메서드 접근자가 있는 보조 필드와 유사하며, 이 보조 필드는 특정하게 정의될 필요 없이 어떠한 개체에서도 존재할 수 있습니다.

다음 예에서는 XAML 접근자 API를 통해 코드에서 연결된 속성을 설정하는 방법을 보여 줍니다. 이 예에서 `myCheckBox`는 [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) 클래스의 인스턴스입니다. 마지막 행은 실제로 값을 설정하는 코드이고 그전의 여러 행에서는 인스턴스와 인스턴스의 부모-자식 관계를 설정합니다. 주석으로 처리되지 않은 마지막 행은 속성 시스템을 사용하는 경우 구문입니다. 주석으로 처리된 마지막 행은 XAML 접근자 패턴을 사용하는 경우 구문입니다.

```csharp
    Canvas myC = new Canvas();
    CheckBox myCheckBox = new CheckBox();
    myCheckBox.Content = "Hello";
    myC.Children.Add(myCheckBox);
    myCheckBox.SetValue(Canvas.TopProperty,75);
    //Canvas.SetTop(myCheckBox, 75);
```

```vb
    Dim myC As Canvas = New Canvas()
    Dim myCheckBox As CheckBox= New CheckBox()
    myCheckBox.Content = "Hello"
    myC.Children.Add(myCheckBox)
    myCheckBox.SetValue(Canvas.TopProperty,75)
    ' Canvas.SetTop(myCheckBox, 75)
```

```cppwinrt
Canvas myC;
CheckBox myCheckBox;
myCheckBox.Content(winrt::box_value(L"Hello"));
myC.Children().Append(myCheckBox);
myCheckBox.SetValue(Canvas::TopProperty(), winrt::box_value(75));
// Canvas::SetTop(myCheckBox, 75);
```

```cpp
    Canvas^ myC = ref new Canvas();
    CheckBox^ myCheckBox = ref new CheckBox();
    myCheckBox->Content="Hello";
    myC->Children->Append(myCheckBox);
    myCheckBox->SetValue(Canvas::TopProperty,75);
    // Canvas::SetTop(myCheckBox, 75);
```

## <a name="custom-attached-properties"></a>사용자 지정 연결된 속성

사용자 지정 연결된 속성을 정의하는 방법에 대한 코드 예와 연결된 속성을 사용하기 위한 시나리오에 대해 자세히 보려면 [사용자 지정 연결된 속성](custom-attached-properties.md)을 참조하세요.

## <a name="special-syntax-for-attached-property-references"></a>연결된 속성 참조를 위한 특수 구문

연결된 속성 이름의 점은 식별 패턴의 핵심 부분입니다. 구문이나 상황에 따라 점이 다른 의미로 처리되는 경우 모호함이 발생하기도 합니다. 예를 들어 점은 바인딩 경로의 개체 모델 통과로 처리됩니다. 이러한 모호함과 관련된 경우는 대부분 내부 점이 연결된 속성의 _owner_**.**_property_ 구분 기호로 구문 분석될 수 있도록 하는 연결된 속성의 특수 구문이 있습니다.

- 연결된 속성을 애니메이션에 대한 대상 경로의 일부로 지정하려면 연결된 속성 이름을 괄호("()") 예를 들어 "(Canvas.Left)"처럼 연결된 속성 이름을 닫습니다. 자세한 내용은 [속성 경로 구문](property-path-syntax.md)을 참조하세요.

> [!WARNING]
> Windows 런타임 XAML 구현의 기존 제한 점은 사용자 지정 연결 된 속성을 애니메이션할 수입니다.

- 연결된 속성을 리소스 파일에서 **x:Uid**로의 리소스 참조를 위한 대상 속성으로 지정하려면 코드 스타일로 정규화된 **using:** 선언을 대괄호("\[\]") 안에 주입한 특수 구문을 사용하여 의도적인 범위 분할을 만듭니다. 예를 들어 요소가 있다고 가정할 경우는 `<TextBlock x:Uid="Title" />`, 해당 인스턴스에서 **Canvas.Top** 값을 대상으로 하는 리소스 파일에서 리소스 키는 "Title.\[using:Windows.UI.Xaml.Controls\]Canvas.Top"입니다. 리소스 파일 및 XAML에 대한 자세한 내용은 [빠른 시작: UI 리소스 변환](https://msdn.microsoft.com/library/windows/apps/xaml/hh965329)을 참조하세요.

## <a name="related-topics"></a>관련 항목

- [사용자 지정 연결된 속성](custom-attached-properties.md)
- [종속성 속성 개요](dependency-properties-overview.md)
- [XAML을 사용하여 레이아웃 정의](https://msdn.microsoft.com/library/windows/apps/mt228350)
- [빠른 시작: UI 리소스 번역](https://msdn.microsoft.com/library/windows/apps/hh943060)
- [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)
- [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359)
