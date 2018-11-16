---
author: jwmsft
description: XAML 구문 규칙에 대해 설명하고 XAML 구문에 사용 가능한 선택 사항 및 제한 사항을 나타내는 용어에 대해서 설명합니다.
title: XAML 구문 가이드
ms.assetid: A57FE7B4-9947-4AA0-BC99-5FE4686B611D
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1fe2460dfc5ab11a9168f1d1d87207d2b9490026
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6840257"
---
# <a name="xaml-syntax-guide"></a>XAML 구문 가이드


XAML 구문 규칙에 대해 설명하고 XAML 구문에 사용 가능한 선택 사항 및 제한 사항을 나타내는 용어에 대해서 설명합니다. 이 항목은 XAML 언어를 처음 사용하거나 용어나 구문의 일부를 다시 살펴보고 싶은 독자 또는 XAML 언어의 작동 방식을 알고 싶으며 자세한 배경과 상황을 원하는 독자에게 유용합니다.

## <a name="xaml-is-xml"></a>XAML은 XML이다

XAML(Extensible Application Markup Language)의 기본 구문은 XML을 바탕으로 만들어졌으며 정의상 유효한 XAML은 유효한 XML이어야 합니다. 그러나 XAML에는 XML을 확장하는 고유한 구문 개념도 있습니다. 지정된 XML 엔터티가 일반 XML에서 유효하지만 해당 구문을 XAML로 사용할 경우 전혀 다르고 보다 완전한 의미를 가질 수도 있습니다. 이 항목에서는 이러한 XAML 구문 개념에 대해 설명합니다.

## <a name="xaml-vocabularies"></a>XAML 어휘

XAML이 대부분의 XML 사용 시와 차이가 있는 한 가지 영역은, XAML은 일반적으로 XSD 파일과 같은 스키마가 적용되지 않는다는 점입니다. 이는 XAML은 확장 가능형이기 때문입니다(머리글자어 XAML의 "X"가 확장 가능함을 의미함). XAML이 구문 분석되면 XAML에서 참조하는 요소 및 특성이 일부 지원 코드 표현에서 Windows 런타임에 정의된 핵심 유형 또는 Windows 런타임 기반이거나 Windows 런타임에서 확장되는 유형으로 존재할 것으로 예상됩니다. SDK 설명서에서 Windows 런타임에 기본 제공되며 XAML에서 Windows 런타임용 *XAML 어휘*로 사용할 수 있는 유형을 참조하는 경우가 있습니다. Microsoft Visual Studio를 사용하면 이 XAML 어휘 내에서 유효한 태그를 생성할 수 있습니다. Visual Studio는 또한 사용자 지정 유형의 원본이 프로젝트에서 올바르게 참조되는 경우 이 유형을 XAML 용도로 포함할 수도 있습니다. XAML 및 사용자 지정 유형에 대한 자세한 내용은 [XAML 네임스페이스 및 네임스페이스 매핑](xaml-namespaces-and-namespace-mapping.md)을 참조하세요.

##  <a name="declaring-objects"></a>선언하는 개체

프로그래머는 흔히 개체 및 멤버의 측면에서 생각하지만 생성 언어는 요소와 특성으로 개념화됩니다. 가장 기본적인 의미에서, XAML 태그에 선언된 요소는 지원하는 런타임 개체 표현에서 개체가 됩니다. 앱에 대한 런타임 개체를 만들려면 XAML 태그에서 XAML 요소를 선언합니다. Windows 런타임에서 XAML을 로드할 때 개체가 만들어집니다.

XAML 파일에는 항상 루트 역할을 하는 요소가 정확히 하나가 있으며 응용 프로그램 전체 런타임 정의에 대한 개체 그래프 또는 페이지 같은 일부 프로그래밍 구조의 개념 루트가 될 개체를 선언합니다.

XAML 구문면에서 개체를 XAML에서 선언하는 방법은 다음 3가지입니다.

-   **개체 요소 구문을 사용하여 직접적으로:** 여는 태그와 닫는 태그를 사용하여 개체를 XML 형식 요소로 인스턴스화합니다. 이 구문을 사용하여 루트 개체를 선언하거나 속성 값을 설정하는 중첩 개체를 만들 수 있습니다.
-   **특성 구문을 사용하여 간접적으로:** 개체를 만드는 방법에 대한 지침이 있는 인라인 문자열 값을 사용합니다. XAML 파서는 이 문자열을 사용하여 속성 값을 새로 만든 참조 값으로 설정합니다. 이 방법은 특정 공용 개체와 속성에 대해서만 지원됩니다.
-   태그 확장 사용

XAML 용어 모음에서 개체 만들기에 필요한 모든 구문을 항상 선택할 수 있는 것은 아닙니다. 일부 개체는 개체 요소 구문을 사용해야만 만들 수 있습니다. 초기에 특성에서 설정해야 만들 수 있는 개체도 있습니다. 사실, XAML 용어 모음에서 개체 요소 또는 특성 구문으로 만들 수 있는 개체는 비교적 적습니다. 두 구문 형식이 모두 가능하지만 스타일 문제로 구문 중 하나가 더 많이 사용될 것입니다.
XAML에서 새 값을 만드는 대신 기존 개체를 참조하는 데 사용할 수 있는 기술도 있습니다. 기존 개체는 XAML의 다른 영역에서 정의되었거나 플랫폼과 해당 응용 프로그램 또는 프로그래밍 모델의 동작을 통해 암시적으로 존재할 수 있습니다.

### <a name="declaring-an-object-by-using-object-element-syntax"></a>개체 요소 구문을 사용하여 개체 선언

개체 요소 구문을 사용하여 개체를 선언하려면 `<objectName>  </objectName>`처럼 태그를 작성합니다. 여기서 *objectName*은 인스턴스화할 개체의 형식 이름입니다. 다음은 개체 요소를 사용하여 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 개체를 선언하는 방법입니다.

```xml
<Canvas>
</Canvas>
```

개체에 다른 개체가 포함되지 않은 경우 여는 태그/닫는 태그 쌍 대신 하나의 자체 닫는 태그를 사용하여 개체 요소를 선언할 수 있습니다. `<Canvas />`

### <a name="containers"></a>컨테이너

[**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)와 같이 UI 요소로 사용되는 많은 개체에는 다른 개체가 포함될 수 있습니다. 이러한 개체를 컨테이너라고 합니다. 다음 예제에서는 한 요소([**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle))가 포함된 **Canvas** 컨테이너를 보여 줍니다.

```xml
<Canvas>
  <Rectangle />
</Canvas>
```

### <a name="declaring-an-object-by-using-attribute-syntax"></a>특성 구문을 사용하여 개체 선언

이 동작은 속성 설정에 연결되어 있으므로 이후 섹션에서 자세히 설명하겠습니다.

### <a name="initialization-text"></a>초기화 텍스트

일부 개체의 경우 구성의 초기화 값으로 사용되는 내부 텍스트를 통해 새 값을 선언할 수 있습니다. XAML에서 이 방법과 구문을 *초기화 텍스트*라고 합니다. 개념상, 초기화 텍스트는 매개 변수가 있는 생성자 호출과 유사합니다. 초기화 텍스트는 특정 구조의 초기 값을 설정하는 데 유용합니다.

[**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)에 포함될 수 있도록 **x:Key**가 있는 구조 값을 원하는 경우 대체로 초기화 텍스트와 함께 개체 요소 구문을 사용합니다. 여러 대상 속성에서 해당 구조 값을 공유하는 경우 이 작업을 수행할 수 있습니다. 일부 구조의 경우 특성 구문을 사용하여 구조 값을 설정할 수 없습니다. 초기화 텍스트를 통해서만 유용하고 공유 가능한 [**CornerRadius**](https://msdn.microsoft.com/library/windows/apps/br242343), [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864), [**GridLength**](https://msdn.microsoft.com/library/windows/apps/br208754) 또는 [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) 리소스를 생성할 수 있습니다.

이 간략한 예에서는 초기화 텍스트를 사용하여 [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864)의 값을 지정합니다. 이 경우에는 **Left** 및 **Right**를 20으로 설정하고 **Top** 및 **Bottom**을 10으로 설정하는 값을 지정합니다. 이 예에서는 입력된 리소스로 만들어진 **Thickness**를 보여 준 다음 이 리소스에 대한 참조를 보여 줍니다. [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864) 초기화 텍스트에 대한 자세한 내용은 [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864)를 참조하세요.

```xml
<UserControl ...>
  <UserControl.Resources>
    <Thickness x:Key="TwentyTenThickness">20,10</Thickness>
    ....
  </UserControl.Resources>
  ...
  <Grid Margin="{StaticResource TwentyTenThickness}">
  ...
  </Grid>
</UserControl ...>
```

**참고**일부 구조는 개체 요소로 선언할 수 없습니다. 초기화 텍스트가 지원되지 않으며 리소스로 사용할 수 없습니다. XAML에서 속성을 이러한 값으로 설정하려면 특성 구문을 사용해야 합니다. 이러한 형식은 [**Duration**](https://msdn.microsoft.com/library/windows/apps/br242377), [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/br210411), [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870), [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994) 및 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)입니다.

## <a name="setting-properties"></a>속성 설정

개체 요소 구문을 사용하여 선언한 개체에 대해 속성을 설정할 수 있습니다. XAML에서 속성을 설정하는 방법은 여러 가지가 있습니다.

-   특성 구문 사용
-   속성 요소 구문 사용
-   콘텐츠(내부 텍스트 또는 자식 요소)가 개체의 XAML 콘텐츠 속성을 설정하는 요소 구문 사용
-   컬렉션 구문 사용(대개 암시적 컬렉션 구문).

개체 선언과 마찬가지로, 이 목록이 각 방법으로 모든 속성을 설정할 수 있다는 것을 의미하지는 않습니다. 일부 속성은 이러한 방법 중 하나만 지원합니다.
둘 이상의 형식을 지원하는 속성도 있습니다. 예를 들어 속성 요소 구문이나 특성 구문을 사용할 수 있는 속성이 있습니다. 가능한 방법은 속성과 속성이 사용하는 개체 형식에 따라 달라집니다. Windows 런타임 API 참조의 경우 **구문** 섹션에서 활용 가능한 XAML 사용을 확인할 수 있습니다. 적용되는 대체 사용이 있는 경우가 있지만 더 세부적입니다. XAML에서의 해당 속성 사용에 대한 모범 사례나 실제 시나리오를 표시하려고 노력하고 있으므로, 이런 세부적인 사용은 표시되지 않을 때가 있습니다. XAML 구문에 대한 지침은 XAML에서 설정할 수 있는 속성에 대한 참조 페이지의 **XAML 사용** 섹션에 제공됩니다.

XAML에서는 어떠한 방법으로도 설정할 수 없고 코드를 사용해서만 설정할 수 있는 개체의 속성도 있습니다. 일반적으로 이러한 속성은 XAML이 아니라 코드 숨김에서 작업하는 것이 더 좋습니다.

읽기 전용 속성은 XAML에서 설정할 수 없습니다. 코드에서라도 소유 형식이 생성자 오버로드, 도우미 메서드, 계산된 속성 지원 등 이러한 속성을 설정하는 다른 방법을 지원해야 합니다. 계산된 속성은 다른 설정 가능한 속성의 값과 기본 제공 처리가 있는 이벤트를 사용합니다. 이러한 기능은 종속성 속성 시스템에서 사용할 수 있습니다. 종속성 속성이 계산된 속성 지원에 유용한 방식에 대한 자세한 내용은 [종속성 속성 개요](dependency-properties-overview.md)를 참조하세요.

XAML의 컬렉션 구문은 읽기 전용 속성을 설정하는 것 같은 느낌을 주지만 실제로는 그렇지 않습니다. 이 항목의 뒷부분에 있는 "컬렉션 구문을 사용하여 속성 설정" 섹션을 참조하세요.

### <a name="setting-a-property-by-using-attribute-syntax"></a>특성 구문을 사용하여 속성 설정

특성 값 설정은 XML 또는 HTML과 같은 생성 언어에서 속성 값을 설정하는 일반적인 방법입니다. XAML 특성 설정은 XML에서 특성 값을 설정하는 방법과 유사합니다. 특성 이름은 요소 이름 뒤에 있는 태그 내 임의 지점에서 지정되며 하나 이상의 공백으로 요소 이름과 구분됩니다. 특성 이름 뒤에는 등호가 옵니다. 특성 이름은 한 쌍의 따옴표 안에 포함됩니다. 따옴표는 일치하고 값을 묶기만 한다면 큰따옴표를 사용하든, 작은따옴표를 사용하든 관계 없습니다. 특성 값 자체는 문자열로 표현될 수 있어야 합니다. 문자열에 숫자가 포함된 경우도 많지만, XAML에서는 XAML 파서가 사용되고 일부 기본 값 변환을 수행할 때까지 모든 특성 값은 문자열 값입니다.

이 예에서는 네 가지 특성의 특성 구문을 사용하여 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 개체의 [**Name**](https://msdn.microsoft.com/library/windows/apps/br208735), [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 및 [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) 속성을 설정합니다.

```xml
<Rectangle Name="rectangle1" Width="100" Height="100" Fill="Blue" />
```

### <a name="setting-a-property-by-using-property-element-syntax"></a>속성 요소 구문을 사용하여 속성 설정

개체의 많은 속성은 속성 요소 구문을 사용하여 설정될 수 있습니다. 속성 요소는 다음과 같이 표시됩니다. `<`*object*`.`*property*`>`.

속성 요소 구문을 사용하기 위해 설정할 속성에 대한 XAML 속성 요소를 만듭니다. 표준 XML에서 이 요소는 이름에 점이 있는 요소로 간주됩니다. 그러나 XAML에서 요소 이름의 점은 요소를 속성 요소로 식별합니다. 이때 *property*는 지원하는 개체 모델 구현에서 *object*의 멤버로 예상됩니다. 속성 요소 구문을 사용하려면 속성 요소 태그를 "채우기" 위해 개체 요소를 지정할 수 있어야 합니다. 속성 요소에는 항상 일부 콘텐츠(단일 요소, 여러 요소 또는 내부 텍스트)가 있습니다. 자체적으로 닫히는 속성 요소를 사용하는 것은 의미가 없습니다.

다음 문법에서 *property*는 설정할 속성의 이름이고 *propertyValueAsObjectElement*는 속성의 값 형식 요구 사항을 충족해야 하는 단일 개체 요소입니다.

`<`*object*`>`

`<`*object*`.`*property*`>`

*propertyValueAsObjectElement*

`</`*object*`.`*property*`>`

`</`*object*`>`

다음 예제에서는 속성 요소 구문을 사용하여 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)의 [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill)을 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) 개체 요소를 통해 설정합니다. **SolidColorBrush** 내에서 [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color)는 특성으로 설정됩니다. 이 XAML의 구문 분석된 결과는 특성 구문을 사용하여 **Fill**을 설정한 이전 XAML 예제와 동일합니다.

```xml
<Rectangle
  Name="rectangle1"
  Width="100" 
  Height="100"
> 
  <Rectangle.Fill> 
    <SolidColorBrush Color="Blue"/> 
  </Rectangle.Fill>
</Rectangle>
```

### <a name="xaml-vocabularies-and-object-oriented-programming"></a>XAML 용어 모음 및 개체 지향 프로그래밍

Windows 런타임 XAML 형식의 XAML 멤버로 표시되는 속성과 이벤트는 기본 형식에서 상속되는 경우가 많습니다. 다음 예제를 살펴보세요. `<Button Background="Blue" .../>`. [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) 속성은 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 클래스에서 직접 선언된 속성이 아닙니다. **Background**는 기본 [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) 클래스에서 상속됩니다. 실제로, **Button**에 대한 참조 항목을 살펴보면 [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736), [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390), [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706), [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911), [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 등 연속하는 기본 클래스로 구성된 각 체인에서 상속된 멤버가 멤버 목록에 하나 이상 있습니다. **속성** 목록에 있는 모든 읽기-쓰기 속성과 컬렉션 속성은 XAML 용어 모음 측면에서 상속됩니다. 이벤트(예: 다양한 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 이벤트)도 상속됩니다.

Windows 런타임 참조의 XAML 지침을 사용하는 경우 구문이나 예제 코드에 표시된 요소 이름은 원래 속성을 정의한 형식과 관련된 경우가 많은데, 이는 참조 항목이 기본 클래스에서 이 형식을 상속할 수 있는 모든 형식에서 공유되기 때문입니다. XML 편집기에서 Visual Studio의 IntelliSense for XAML을 사용하는 경우 XML 편집기, IntelliSense 및 드롭다운은 효율적으로 상속을 통합하고, 클래스 인스턴스에 대한 개체 요소로 시작한 후 설정에 사용할 수 있는 정확한 특성 목록을 제공합니다.

### <a name="xaml-content-properties"></a>XAML 콘텐츠 속성

일부 형식은 속성이 XAML 콘텐츠 구문을 가능하게 하도록 속성 중 하나를 정의합니다. 형식에 대한 XAML 콘텐츠 속성의 경우 XAML에서 속성을 지정할 때 해당 속성의 속성 요소를 생략할 수 있습니다. 또는 소유하는 형식의 개체 요소 태그 내에 내부 텍스트를 직접 제공하여 내부 텍스트 값으로 속성을 설정할 수 있습니다. XAML 콘텐츠 속성은 해당 속성에 대해 간단한 태그 구문을 지원하며 중첩을 줄여 XAML을 사람이 읽기 쉬운 형식으로 만듭니다.

XAML 콘텐츠 구문을 사용할 수 있는 경우 해당 구문이 Windows 런타임 참조 설명서에서 속성에 대한 **구문**의 "XAML" 섹션에 표시됩니다. 예를 들어 [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250)에 대한 [**Child**](https://msdn.microsoft.com/library/windows/apps/br209258) 속성 페이지에서는 **Border**의 단일 개체 **Border.Child** 값을 설정하는 속성 요소 구문 대신 다음과 같이 XAML 콘텐츠 구문을 보여 줍니다.

```xml
<Border>
  <Button .../>
</Border>
```

XAML 콘텐츠 속성으로 선언된 속성이 **Object** 형식이거나 **String** 형식인 경우 XAML 콘텐츠 구문은 기본적으로 XML 문서 모델에서 내부 텍스트인 부분(여는 개체 태그와 닫는 개체 태그 사이의 문자열)을 지원합니다. 예를 들어 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)에 대한 [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) 속성 페이지는 **Text**를 설정하는 내부 텍스트 값이 있는 XAML 콘텐츠 구문을 보여 주지만 "Text" 문자열은 태그에 표시되지 않습니다. 사용 예제는 다음과 같습니다.

```xml
<TextBlock>Hello!</TextBlock>
```

클래스에 대해 XAML 콘텐츠 속성이 있는 경우 "특성" 섹션에서 클래스에 대한 참조 항목을 참조하세요. [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011)의 값을 찾습니다. 이 특성은 명명된 "Name" 필드를 사용합니다. "Name" 값은 XAML 콘텐츠 속성인 해당 클래스의 속성 이름입니다. 예를 들어 [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) 참조 페이지에는 다음 항목이 표시됩니다. ContentProperty("Name=Child").

한 가지 중요한 XAML 구문 규칙은 요소에 설정한 다른 속성 요소와 XAML 콘텐츠 속성을 함께 사용할 수 없다는 것입니다. XAML 콘텐츠 속성을 전체 속성 요소 앞이나 뒤에 설정해야 합니다. 예를 들어 다음은 잘못된 XAML입니다.

``` syntax
<StackPanel>
  <Button>This example</Button>
  <StackPanel.Resources>
    <SolidColorBrush x:Key="BlueBrush" Color="Blue"/>
  </StackPanel.Resources>
  <Button>... is illegal XAML</Button>
</StackPanel>
```

## <a name="collection-syntax"></a>컬렉션 구문

지금까지 보여 준 구문은 모두 속성을 단일 개체로 설정합니다. 그러나 많은 UI 시나리오에서는 지정된 부모 요소에 여러 자식 요소가 있을 수 있어야 합니다. 예를 들어 입력 양식의 UI에 몇 가지 입력란 요소, 레이블 및 "제출" 단추가 필요합니다. 하지만 프로그래밍 개체 모델을 사용하여 이러한 여러 요소에 액세스할 경우 이러한 요소는 각 항목이 서로 다른 속성의 값이 되는 것이 아니라 대개 단일 컬렉션 속성의 항목이 됩니다. XAML은 컬렉션 형식을 사용하는 속성을 암시적으로 처리하고 컬렉션 형식의 자식 요소를 특수 처리하여 일반적인 지원 컬렉션 모델을 지원할 뿐만 아니라 여러 자식 요소를 지원합니다.

많은 컬렉션 속성은 클래스에 대한 XAML 콘텐츠 속성으로도 식별됩니다. 암시적 컬렉션 처리 및 XAML 콘텐츠 구문의 조합은 패널, 뷰, 항목 컨트롤 등 컨트롤 합치기에 사용되는 형식에서 자주 나타납니다. 예를 들어 다음 예제에서는 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635) 내에서 두 개의 피어 UI 요소를 합치기 위한 가장 간단한 XAML을 보여 줍니다.

```xml
<StackPanel>
  <TextBlock>Hello</TextBlock>
  <TextBlock>World</TextBlock>
</StackPanel>
```

### <a name="the-mechanism-of-xaml-collection-syntax"></a>XAML 컬렉션 구문의 메커니즘

처음에는 XAML이 읽기 전용 컬렉션 속성의 "설정"을 가능하게 하는 것처럼 보일 수 있습니다. 실제로 XAML이 여기에서 가능하게 하는 것은 기존 컬렉션에 항목을 추가하는 것입니다. XAML 지원을 구현하는 XAML 언어와 XAML 프로세서는 지원하는 컬렉션 형식의 규칙을 사용하여 이 구문을 사용하도록 설정합니다. 일반적으로 인덱서 또는 컬렉션의 특정 항목을 참조하는 **Items** 속성과 같은 지원 속성이 있습니다. 대개 이 속성은 XAML 구문에서 명시적이 아닙니다. 컬렉션의 경우 XAML 구문 분석의 기본 메커니즘은 속성이 아니라 메서드입니다. 특히 대부분의 경우 **Add** 메서드입니다. XAML 처리기가 XAML 컬렉션 구문 내에서 하나 이상의 개체 요소를 발견하면 이러한 각 개체가 요소에서 먼저 만들어진 다음 컬렉션의 **Add** 메서드를 호출하여 각 새 개체가 포함하는 컬렉션에 순서대로 추가됩니다.

XAML 파서가 항목을 컬렉션에 추가할 때 **Add** 메서드의 논리에 의해 주어진 XAML 요소가 컬렉션 개체의 허용 가능한 항목 자식인지 여부가 확인됩니다. 많은 컬렉션 형식이 지원하는 구현에 의해 강력하게 형식화되므로 **Add**의 입력 매개 변수는 전달되는 모든 항목의 형식이 **Add** 매개 변수 형식과 일치해야 한다고 기대합니다.

컬렉션 속성의 경우 컬렉션을 개체 요소로 명시적으로 지정할 때 주의해야 합니다. XAML 파서는 개체 요소를 발견할 때마다 새 개체를 만듭니다. 사용하려는 컬렉션 속성이 읽기 전용인 경우 이로 인해 XAML 구문 분석 예외가 발생할 수 있습니다. 암시적 컬렉션 구문을 사용하면 이러한 예외가 표시되지 않습니다.

## <a name="when-to-use-attribute-or-property-element-syntax"></a>특성 또는 속성 요소 구문을 사용하는 경우

XAML에서 지원이 설정되는 모든 속성은 직접 값 설정을 위해 특성 또는 속성 요소 구문을 지원하지만 두 구문을 서로 바꿔서 지원하지는 않습니다. 일부 속성은 두 구문 중 하나를 지원하며, XAML 콘텐츠 속성 등의 추가 구문 옵션을 지원하는 속성도 있습니다. 속성이 지원하는 XAML 구문의 형식은 속성이 속성 형식으로 사용하는 개체의 형식에 따라 달라집니다. 속성 형식이 double(float 또는 decimal), integer, Boolean 또는 string과 같은 기본 형식인 경우 속성은 항상 특성 구문을 지원합니다.

속성을 설정하는 데 사용하는 개체 형식이 문자열을 처리하여 만들어질 수 있는 경우 특성 구문을 사용하여 해당 속성을 설정할 수도 있습니다. primitive의 경우 항상 형식 변환이 파서에 기본 제공됩니다. 그러나 다른 특정 개체 형식은 속성 요소 내의 개체 요소가 아니라 특성 값으로 지정된 문자열을 사용하여 만들 수도 있습니다. 이렇게 하려면 특정 속성에서 지원되거나 일반적으로 해당 속성 형식을 사용하는 모든 값에 대해 지원되는 기본 형식 변환이 있어야 합니다. 특성의 문자열 값은 새 개체 값의 초기화에 중요한 속성을 설정하는 데 사용됩니다. 특정 형식 변환기가 문자열의 정보를 어떻게 고유하게 처리하는지에 따라 일반 속성 형식의 여러 가지 서브클래스도 만들 수 있습니다. 이 동작을 지원하는 개체 형식에는 참조 설명서의 구문 섹션에 나열된 특수 문법이 있습니다. 예를 들어 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)에 대한 XAML 구문은 특성 구문을 사용하여 **Brush** 형식의 임의 속성에 대해 새 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) 값을 만들 수 있는 방법을 보여 주며, Windows 런타임 XAML에는 많은 **Brush** 속성이 있습니다.

## <a name="xaml-parsing-logic-and-rules"></a>XAML 구문 분석 논리 및 규칙

XAML 파서에서 읽어야 하는 방법과 유사하게 XAML을 선형 순서로 발생한 문자열 토큰 집합으로 읽는 것이 유용한 경우도 있습니다. XAML 파서는 XAML 작동 방법에 대한 정의에 포함된 규칙 집합을 통해 이러한 토큰을 해석해야 합니다.

특성 값 설정은 XML 또는 HTML과 같은 생성 언어에서 속성 값을 설정하는 일반적인 방법입니다. 다음 구문에서 *objectName*은 인스턴스화할 개체이고, *propertyName*은 해당 개체에 대해 설정할 속성의 이름이며, *propertyValue*는 설정할 값입니다.

```xml
<objectName propertyName="propertyValue" .../>

-or-

<objectName propertyName="propertyValue">

...<!--element children -->

</objectName>
```

두 구문을 사용하여 개체를 선언하고 해당 개체에 대해 속성을 설정할 수 있습니다. 첫 번째 예가 태그의 단일 요소이지만 실제로는 XAML 처리기가 이 태그를 구문 분석하는 방법과 관련하여 몇 단계가 존재합니다.

먼저, 개체 요소가 있으면 새 *objectName* 개체가 인스턴스화되어야 함을 나타냅니다. 이러한 인스턴스가 있어야만 인스턴스 속성 *propertyName*이 해당 인스턴스에 대해 설정될 수 있습니다.

또 다른 XAML 규칙은 요소의 특성을 임의 순서로 설정할 수 있어야 한다는 것입니다. 예를 들어 `<Rectangle Height="50" Width="100" />`과 `<Rectangle Width="100"  Height="50" />`은 차이가 없습니다. 사용하는 순서는 스타일의 문제일 뿐입니다.

**참고**XML 편집기 이외의 디자인 화면을 사용 하지만 특성을 다시 정렬 하거나 새 도입할 나중에 해당 XAML을 자유롭게 편집 하는 경우 XAML 디자이너 순서 지정 규칙을 종종 홍보 합니다.

## <a name="attached-properties"></a>연결된 속성

XAML은 *연결된 속성*으로 알려진 구문 요소를 추가하여 XML을 확장합니다. 속성 요소 구문과 유사한 연결된 속성 구문에는 점이 포함되며 이러한 점은 XAML 구문 분석에 특별한 의미를 가지고 있습니다. 특히 점은 연결된 속성의 소유자 공급자 및 속성 이름을 구분합니다.

XAML에서 *AttachedPropertyProvider*.*PropertyName* 구문을 사용하여 연결된 속성을 설정합니다. 다음은 XAML에서 연결된 속성 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771)를 설정할 수 있는 방법에 대한 예입니다.

```xml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

지원 형식에 해당 이름의 속성이 없는 요소에 대해 연결된 속성을 설정할 수 있습니다. 이렇게 하면 해당 속성은 글로벌 속성과 유사하게 작동하거나 **xml:space** 특성처럼 다른 XML 네임스페이스에서 정의된 특성과 유사하게 작동합니다.

Windows 런타임 XAML에서는 다음 시나리오를 지원하는 연결된 속성이 표시됩니다.

-   자식 요소가 부모 컨테이너 패널에 레이아웃에서의 동작 방법을 알릴 수 있음: [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267), [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704), [**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227651)
-   컨트롤 사용 시 컨트롤 템플릿에서 가져온 중요한 컨트롤 부분의 동작에 영향을 줄 수 있음: [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527), [**VirtualizingStackPanel**](https://msdn.microsoft.com/library/windows/apps/br227689)
-   관련된 클래스에서 사용할 수 있는 서비스를 사용하며, 여기서 서비스 및 이 서비스를 사용하는 클래스가 상속을 공유하지 않음: [**Typography**](https://msdn.microsoft.com/library/windows/apps/hh702143), [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/br209021), [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/br209081), [**ToolTipService**](https://msdn.microsoft.com/library/windows/apps/br227609).
-   애니메이션 대상: [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490)

자세한 내용은 [연결된 속성 개요](attached-properties-overview.md)를 참조하세요.

## <a name="literal--values"></a>리터럴 "{" 값

여는 중괄호(\{)는 태그 확장 순서를 여는 것이므로 이스케이프 시퀀스를 사용하여 "\{"로 시작하는 리터럴 문자열 값을 지정합니다. 이스케이프 시퀀스는 "\{\}"입니다. 예를 들어 하나의 여는 중괄호를 문자열 값으로 지정하려면 특성 값을 "\{\}\{"로 지정합니다. 따옴표를 대신 사용하여(예: **""** 로 구분된 특성 값 내의 **'**) "\{" 값을 문자열로 제공할 수도 있습니다.

**참고**"\\}"는 따옴표가 붙은 특성 하는 경우에 작동 합니다.
 
## <a name="enumeration-values"></a>열거형 값

Windows 런타임 API의 많은 속성은 열거형을 값으로 사용합니다. 멤버가 읽기-쓰기 속성인 경우 특성 값을 제공하여 해당 속성을 설정할 수 있습니다. 상수 이름의 비정규화된 이름을 통해 속성 값으로 사용할 열거형 값을 식별합니다. 예를 들어 다음은 XAML에서 [**UIElement.Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992)를 설정하는 방법입니다. `<Button Visibility="Visible"/>`. 여기에서 문자열로 "표시"는 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br209006) 열거형, **Visible**의 명명된 상수에 직접 매핑됩니다.

-   정규화된 형식은 작동하지 않으므로 사용하지 마세요. 예를 들어 다음은 잘못된 XAML입니다. `<Button Visibility="Visibility.Visible"/>`.
-   상수의 값을 사용하지 마세요. 다시 말해서 열거형이 정의된 방식에 의해 명시적으로 또는 암시적으로 영향을 받는 열거형의 정수 값을 사용하지 마세요. 작동하는 것처럼 보일지도 모르지만, 일시적인 구현 정보에 의존하게 되므로 XAML 또는 코드에서 사용하면 좋지 않습니다. 예를 들어 다음과 같이 사용하지 마세요.`<Button Visibility="1"/>`.

**참고**을 XAML을 사용 하는 열거형 Api에 대 한 참조 항목에서 **구문의** **속성 값** 섹션에는 열거형 형식의 링크를 클릭 합니다. 그러면 해당 열거형의 명명된 상수를 찾을 수 있는 열거형 페이지로 연결됩니다.

열거형은 깃발 모양이 될 수 있습니다. 즉, **FlagsAttribute**를 사용하여 특성이 지정됩니다. 플래그 수준의 열거형에 대한 값의 조합을 XAML 특성 값으로 지정해야 할 경우 각 열거형 상수 이름을 쉼표(,)로 구분하고 공백 문자 없이 사용할 수 있습니다. Windows 런타임 XAML 어휘에서는 플래그 수준의 특성이 일반적이지 않지만, [**ManipulationModes**](https://msdn.microsoft.com/library/windows/apps/br227934)는 XAML에서 플래그 수준의 열거형 값 설정이 지원되는 예입니다.

## <a name="interfaces-in-xaml"></a>XAML의 인터페이스

드문 경우이긴 하지만 속성의 형식이 인터페이스인 XAML 구문을 볼 수 있습니다. XAML 형식 시스템에서 이 인터페이스를 구현하는 형식은 구문 분석될 때 값으로 허용됩니다. 값으로 제공할 수 있는 해당 형식의 인스턴스가 생성되어 있어야 합니다. [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736)의 [**Command**](https://msdn.microsoft.com/library/windows/apps/br227740) 및 [**CommandParameter**](https://msdn.microsoft.com/library/windows/apps/br227741) 속성에 대한 XAML 구문에서 형식으로 사용된 인터페이스를 볼 수 있습니다. 이러한 속성은 **ICommand** 인터페이스가 보기 및 모델이 상호 작용하는 방식에 대한 계약이 되는 MVVM(Model-View-ViewModel) 디자인 패턴을 지원합니다.

## <a name="xaml-placeholder-conventions-in-windows-runtime-reference"></a>Windows 런타임 참조의 XAML 자리 표시자 규칙

XAML을 사용할 수 있는 Windows 런타임 API에 대한 참조 항목의 **구문** 섹션을 살펴본 경우 구문에 여러 자리 표시자가 포함된 것을 본 적이 있을 것입니다. XAML 구문은 다른 C#, Microsoft Visual Basic 또는 VisualC + + 구성 요소 확장 (C + + CX) 구문을 XAML 구문은 사용 구문 이므로 합니다. 이 구문은 자체 XAML 파일에서의 최종 사용과 비슷하지만 사용할 수 있는 값에 대해서는 지나치게 지시적이지 않습니다. 따라서 일반적으로 리터럴과 자리 표시자를 혼합하는 문법 형식을 설명하고 **XAML 값** 섹션에 있는 일부 자리 표시자를 정의합니다.

속성에 대한 XAML 구문에 형식 이름/요소 이름이 표시된 경우 표시된 이름은 원래 속성을 정의하는 형식에 대한 이름입니다. 그러나 Windows 런타임 XAML은 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 기반 클래스의 클래스 상속 모델을 지원합니다. 따라서 문자 그대로 정의 클래스가 아니며 대신 속성/특성을 처음에 정의한 클래스에서 파생되는 클래스에서 종종 특성을 사용할 수 있습니다. 예를 들어 전체 상속을 사용하여 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 파생 클래스에서 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992)를 특성으로 설정할 수 있습니다. 예를 들면 `<Button Visibility="Visible" />`와 같습니다. 따라서 XAML 사용 구문에 표시되는 요소 이름을 너무 문자 그대로 받아들이지 마세요. 이 구문은 해당 클래스를 나타내는 요소 및 파생 클래스를 나타내는 요소에 대해 가능할 수 있습니다. 정의 요소로 표시된 형식이 실제 사용에 있을 가능성이 거의 없거나 불가능한 경우 해당 형식 이름은 구문에서 고의로 소문자로 표시합니다. 예를 들어 **UIElement.Visibility**의 구문은 다음과 같이 표시됩니다.

``` syntax
<uiElement Visibility="Visible"/>
-or-
<uiElement Visibility="Collapsed"/>
```

많은 XAML 구문 섹션의 "사용"에는 자리 표시자가 포함되어 있으며 그런 다음 **구문** 섹션 바로 아래에 있는 **XAML 값** 섹션에 정의됩니다.

XAML 사용 섹션은 또한 다양한 범용 자리 표시자를 사용합니다. 이러한 자리 표시자는 나타내는 것을 추측하거나 결국 알 수 있기 때문에 **XAML 값**에서 매번 다시 정의되지 않습니다. 독자가 **XAML 값**에서 이러한 자리 표시자를 반복해서 보는 것을 지루해할 것으로 여기고 정의를 표시하지 않았습니다. 참조를 위해 다음은 일부 이러한 자리 표시자와 일반적으로 의미하는 바를 나타내는 목록입니다.

-   *object*: 이론적으로는 개체 값이지만, 사실상 문자열 또는 개체 선택 등의 특정 개체 형식으로 제한됩니다. 자세한 내용은 참조 페이지의 "설명"에서 확인할 수 있습니다.
-   *object**property*: *object**property*는 결합하여 표시할 구문이 여러 속성의 특성 값으로 사용할 수 있는 형식의 구문인 경우 사용됩니다. 예를 들어 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)에 대해 표시된 **XAML 특성 사용**에는 &lt;*object* *property*="*predefinedColorName*"/&gt;이 포함됩니다.
-   *eventhandler*: 이벤트 특성에 대해 표시된 모든 XAML 구문의 특성 값으로 표시됩니다. 여기에서 제공하는 것은 이벤트 처리기 함수의 함수 이름입니다. 이 함수는 XAML 페이지의 코드 숨김에서 정의되어야 합니다. 프로그래밍 수준에서 이 함수는 처리 중인 이벤트의 대리자 서명과 일치해야 합니다. 그렇지 않으면 앱 코드가 컴파일되지 않습니다. 그러나 이는 프로그래밍과 관련된 고려 사항이며, XAML과 관련된 고려 사항이 아니므로 XAML 구문에서 대리자 형식에 대한 암시를 주기 위해 노력하지 않습니다. 이벤트에 대해 구현해야 할 대리자에 대해 알고 싶으면 해당 이벤트에 대한 참조 항목의 **이벤트 정보** 섹션에서 **대리자**라는 레이블의 표 행을 확인할 수 있습니다.
-   *enumMemberName*: 모든 열거형에 대한 특성 구문에 표시됩니다. 열거형 값을 사용하는 속성에 대한 유사한 자리 표시자기 있지만 일반적으로 열거형의 이름을 암시하는 자리 표시자의 앞에 표시됩니다. 예를 들어 [**FrameworkElement.FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716)에 대해 표시된 구문은 <*frameworkElement***FlowDirection**="* flowDirectionMemberName*"/>입니다. 이러한 속성 참조 페이지 중 하나를 보고 있는 경우 **속성 값** 섹션에서 **형식:** 텍스트 옆에 나타나는 열거형 형식의 링크를 클릭하세요. 해당 열거형을 사용하는 속성의 특성 값을 보려면 **멤버** 목록의 **멤버** 열에 나열된 문자열을 사용할 수 있습니다.
-   *double*, *int*, *string*, *bool*: XAML 언어에 알려진 기본 형식입니다. C# 또는 Visual Basic을 사용하여 프로그래밍하는 경우 이러한 형식은 Microsoft .NET의 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Int32**](https://msdn.microsoft.com/library/windows/apps/xaml/system.int32.aspx), [**String**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.aspx) 및 [**Boolean**](https://msdn.microsoft.com/library/windows/apps/xaml/system.boolean.aspx) 등의 형식에 투영됩니다. .NET 코드 숨김에서 XAML로 정의된 값으로 작업할 경우 이러한 .NET 형식의 멤버를 사용할 수 있습니다. C++/CX를 사용하여 프로그래밍할 경우 C++ 기본 형식을 사용하지만 [**Platform**](https://msdn.microsoft.com/library/windows/apps/xaml/hh710417.aspx) 네임스페이스에 의해 정의된 동일한 형식을 고려할 수 있습니다(예: [**Platform::String**](https://msdn.microsoft.com/library/windows/apps/xaml/hh755812.aspx)). 경우에 따라 특정 속성에 대해 추가 값 제한 사항이 있을 수 있습니다. 그러나 이러한 제한 사항은 코드 사용 및 XAML 사용에 모두 적용되므로 XAML 섹션이 아닌 **속성 값** 섹션 또는 '설명' 섹션에 이러한 내용이 표시됩니다.

## <a name="tips-and-tricks-notes-on-style"></a>스타일에 대한 유용한 정보와 팁, 참고 사항

-   태그 확장은 일반적으로 기본 [XAML 개요](xaml-overview.md)에서 설명합니다. 이 항목에 제시된 지침에 가장 큰 영향을 미치는 마크업 확장은 [StaticResource](staticresource-markup-extension.md) 마크업 확장(및 관련 [ThemeResource](themeresource-markup-extension.md))입니다. StaticResource 태그 확장의 기능은 XAML [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)에서 가져온 재사용 가능 리소스에 대한 XAML 팩터링을 사용하는 것입니다. 거의 항상 **ResourceDictionary**에서 컨트롤 템플릿 및 관련 스타일을 정의합니다. **ResourceDictionary**에서 컨트롤 템플릿 정의의 작은 부분이나 앱 특정 스타일을 정의하는 경우도 많습니다(예: 앱이 UI의 다양한 부분에 대해 여러 번 사용하는 색의 경우 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) 정의). StaticResource를 사용하면 속성 요소를 사용해야 하는 속성을 특성 구문에서 설정할 수 있습니다. 하지만 다시 사용하기 위한 XAML 팩터링의 이점은 페이지 수준 구문을 단순화하는 것 이상입니다. 자세한 내용은 [ResourceDictionary 및 XAML 리소스 참조](https://msdn.microsoft.com/library/windows/apps/mt187273)를 확인하세요.
-   XAML 예제에서 공백과 줄 바꿈이 적용된 방법에 대한 다양한 규칙이 표시됩니다. 특히, 서로 다른 많은 특성 집합이 있는 개체 요소를 분리하는 방법에 대해 여러 가지 규칙이 있습니다. 이것은 단지 스타일의 문제일 뿐입니다. XAML을 편집할 때 Visual Studio XML 편집기는 일부 기본 스타일 규칙을 적용하지만 설정에서 이러한 규칙을 변경할 수 있습니다. XAML 파일의 공백이 의미 있는 것으로 간주되는 경우도 간혹 있습니다. 자세한 내용은 [XAML 및 공백](xaml-and-whitespace.md)을 참조하세요.

## <a name="related-topics"></a>관련 항목

* [XAML 개요](xaml-overview.md)
* [XAML 네임스페이스 및 네임스페이스 매핑](xaml-namespaces-and-namespace-mapping.md)
* [ResourceDictionary 및 XAML 리소스 참조](https://msdn.microsoft.com/library/windows/apps/mt187273)
 

