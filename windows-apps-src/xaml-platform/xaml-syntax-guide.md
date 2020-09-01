---
description: Xaml 구문 규칙 및 XAML 구문에 사용할 수 있는 제한 사항에 대해 설명 하는 용어에 대해 설명 합니다.
title: XAML 구문 가이드
ms.assetid: A57FE7B4-9947-4AA0-BC99-5FE4686B611D
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e27fb3edbcf3a6ffcd1e1a175e63c1a7090f5719
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164097"
---
# <a name="xaml-syntax-guide"></a>XAML 구문 가이드


Xaml 구문 규칙 및 XAML 구문에 사용할 수 있는 제한 사항에 대해 설명 하는 용어에 대해 설명 합니다. 이 항목은 XAML 언어를 처음 사용 하거나, 용어 또는 구문의 일부에 대 한 리프레셔를 사용 하 고, XAML 언어가 작동 하 고 더 많은 배경 및 컨텍스트를 원하는 경우에 유용 합니다.

## <a name="xaml-is-xml"></a>XAML은 XML입니다.

Extensible Application Markup Language (XAML)에는 XML을 기반으로 하는 기본 구문이 있으며, 정의에 따라 유효한 XAML은 유효한 XML 이어야 합니다. 그러나 XAML은 XML을 확장 하는 자체 구문 개념을 포함 하 고 있습니다. 지정 된 XML 엔터티는 일반 XML에서 유효 하지만이 구문은 XAML과는 완전히 다른 의미를 가질 수 있습니다. 이 항목에서는 이러한 XAML 구문 개념에 대해 설명 합니다.

## <a name="xaml-vocabularies"></a>XAML 어휘

XAML이 대부분의 XML 사용과 다른 한 가지 영역은 일반적으로 XSD 파일 등의 스키마를 사용 하 여 XAML이 적용 되지 않는다는 것입니다. XAML은 확장 가능 하기 때문 이며, 머리글자어 XAML의 "X"는이를 의미 합니다. XAML이 구문 분석 되 면 XAML에서 참조 하는 요소 및 특성이 Windows 런타임에서 정의 된 핵심 형식이 나 Windows 런타임를 기반으로 하거나 확장 하는 형식에서 일부 지원 코드 표시에 있어야 합니다. SDK 설명서는 Windows 런타임에 이미 기본 제공 되 고 Windows 런타임 xaml *어휘* 로 xaml에서 사용할 수 있는 형식을 참조 하는 경우도 있습니다. Microsoft Visual Studio를 사용 하 여이 XAML 어휘 내에서 유효한 태그를 생성할 수 있습니다. Visual Studio는 프로젝트에서 이러한 형식의 소스가 올바르게 참조 되는 한 XAML 사용에 대 한 사용자 지정 형식을 포함할 수도 있습니다. XAML 및 사용자 지정 형식에 대 한 자세한 내용은 [xaml 네임 스페이스 및 네임 스페이스 매핑](xaml-namespaces-and-namespace-mapping.md)을 참조 하세요.

##  <a name="declaring-objects"></a>개체 선언

프로그래머는 종종 개체와 멤버를 기준으로 생각 하는 반면, 태그 언어는 요소 및 특성으로 개념화 됩니다. 가장 기본적인 의미에서 XAML 태그에 선언 하는 요소는 지원 런타임 개체 표현의 개체가 됩니다. 앱에 대 한 런타임 개체를 만들려면 xaml 태그에서 XAML 요소를 선언 합니다. 개체는 Windows 런타임가 XAML을 로드할 때 생성 됩니다.

XAML 파일에는 항상 루트 역할을 하는 요소가 하나 있습니다 .이는 페이지와 같은 일부 프로그래밍 구조의 개념적 루트가 될 개체를 선언 하거나 응용 프로그램 전체 런타임 정의의 개체 그래프를 선언 합니다.

XAML 구문 측면에서 개체를 XAML로 선언 하는 방법에는 세 가지가 있습니다.

-   **개체 요소 구문을 사용 하 여 직접:** 이는 여는 태그와 닫는 태그를 사용 하 여 개체를 XML 형식 요소로 인스턴스화합니다. 이 구문을 사용 하 여 루트 개체를 선언 하거나 속성 값을 설정 하는 중첩 된 개체를 만들 수 있습니다.
-   **간접적으로 특성 구문을 사용 합니다.** 개체를 만드는 방법에 대 한 지침을 포함 하는 인라인 문자열 값을 사용 합니다. XAML 파서는이 문자열을 사용 하 여 속성 값을 새로 만든 참조 값으로 설정 합니다. 이에 대 한 지원은 특정 공통 개체 및 속성으로 제한 됩니다.
-   태그 확장 사용.

이는 항상 XAML 어휘에서 개체를 만드는 데 필요한 모든 구문을 선택 하는 것을 의미 하지는 않습니다. 일부 개체는 개체 요소 구문을 사용 해야만 만들 수 있습니다. 일부 개체는 처음에 특성에 설정 된 경우에만 만들 수 있습니다. 실제로 개체 요소나 특성 구문을 사용 하 여 만들 수 있는 개체는 XAML 어휘에서 비교적 드물게 발생 합니다. 두 구문 형식이 모두 가능 하더라도 구문 중 하나가 스타일의 문제로 더 일반적입니다.
새 값을 만드는 대신 XAML에서 기존 개체를 참조 하는 데 사용할 수 있는 기술도 있습니다. 기존 개체는 XAML의 다른 영역에서 정의 되거나 플랫폼 및 해당 응용 프로그램 또는 프로그래밍 모델의 일부 동작을 통해 암시적으로 존재할 수 있습니다.

### <a name="declaring-an-object-by-using-object-element-syntax"></a>개체 요소 구문을 사용 하 여 개체 선언

개체 요소 구문을 사용 하 여 개체를 선언 하려면 다음과 같은 태그를 작성 `<objectName>  </objectName>` 합니다. 여기서 *objectName* 은 인스턴스화할 개체의 형식 이름입니다. [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) 개체를 선언 하는 데 사용 되는 개체 요소는 다음과 같습니다.

```xml
<Canvas>
</Canvas>
```

개체에 다른 개체가 포함 되어 있지 않은 경우에는 여는/닫는 쌍 대신 자체 닫는 태그 하나를 사용 하 여 개체 요소를 선언할 수 있습니다. `<Canvas />`

### <a name="containers"></a>컨테이너

[**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas)와 같은 UI 요소로 사용 되는 많은 개체는 다른 개체를 포함할 수 있습니다. 이러한 경우를 컨테이너 라고도 합니다. 다음 예제에서는 하나의 요소인 [**사각형**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)을 포함 하는 **Canvas** 컨테이너를 보여 줍니다.

```xml
<Canvas>
  <Rectangle />
</Canvas>
```

### <a name="declaring-an-object-by-using-attribute-syntax"></a>특성 구문을 사용 하 여 개체 선언

이 동작은 속성 설정에 연결 되므로 이후 섹션에서이에 대해 자세히 설명 합니다.

### <a name="initialization-text"></a>초기화 텍스트

일부 개체의 경우 생성 시 초기화 값으로 사용 되는 내부 텍스트를 사용 하 여 새 값을 선언할 수 있습니다. XAML에서이 기술 및 구문을 *초기화 텍스트*라고 합니다. 개념적으로 초기화 텍스트는 매개 변수가 있는 생성자를 호출 하는 것과 비슷합니다. 초기화 텍스트는 특정 구조체의 초기 값을 설정 하는 데 유용 합니다.

**X:Key**로 구조체 값을 사용할 수 있도록 개체 요소 구문을 초기화 텍스트와 함께 사용 [**하는 경우가 많습니다.**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 여러 대상 속성에서 해당 구조 값을 공유 하는 경우이 작업을 수행할 수 있습니다. 일부 구조에서는 특성 구문을 사용 하 여 구조체의 값을 설정할 수 없습니다. 초기화 텍스트는 유용 하 고 공유 가능한 [**CornerRadius**](/uwp/api/Windows.UI.Xaml.CornerRadius), [**두께**](/uwp/api/Windows.UI.Xaml.Thickness), [**gridlength**](/uwp/api/Windows.UI.Xaml.GridLength) 또는 [**Color**](/uwp/api/Windows.UI.Color) 리소스를 생성 하는 유일한 방법입니다.

이 간략 한 예제에서는 초기화 텍스트를 사용 하 여 [**두께**](/uwp/api/Windows.UI.Xaml.Thickness)값을 지정 합니다 .이 경우에는 **왼쪽과** **오른쪽** 모두 20으로 설정 되 고, **위쪽** 및 **아래쪽** 모두 10으로 설정 된 값을 지정 합니다. 이 예제에서는 키가 지정 된 리소스로 만들어진 **두께** 를 표시 한 다음 해당 리소스에 대 한 참조를 표시 합니다. [**두께**](/uwp/api/Windows.UI.Xaml.Thickness) 초기화 텍스트에 대 한 자세한 내용은 [**두께**](/uwp/api/Windows.UI.Xaml.Thickness)를 참조 하세요.

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

**참고**    일부 구조는 개체 요소로 선언할 수 없습니다. 초기화 텍스트는 지원 되지 않으며 리소스로 사용할 수 없습니다. XAML에서 속성을 이러한 값으로 설정 하려면 특성 구문을 사용 해야 합니다. 이러한 형식은 [**Duration**](/uwp/api/Windows.UI.Xaml.Duration), [**RepeatBehavior**](/uwp/api/Windows.UI.Xaml.Media.Animation.RepeatBehavior), [**Point**](/uwp/api/Windows.Foundation.Point), [**Rect**](/uwp/api/Windows.Foundation.Rect) 및 [**Size**](/uwp/api/Windows.Foundation.Size)입니다.

## <a name="setting-properties"></a>속성 설정

개체 요소 구문을 사용 하 여 선언 된 개체에 대 한 속성을 설정할 수 있습니다. XAML에서 속성을 설정 하는 방법에는 여러 가지가 있습니다.

-   특성 구문을 사용 합니다.
-   속성 요소 구문을 사용 합니다.
-   콘텐츠 (내부 텍스트 또는 자식 요소)가 개체의 XAML 콘텐츠 속성을 설정 하는 요소 구문을 사용 합니다.
-   컬렉션 구문 (일반적으로 암시적 컬렉션 구문)을 사용 합니다.

개체 선언과 마찬가지로이 목록에서는 각 기술을 사용 하 여 모든 속성을 설정할 수 있다는 의미는 아닙니다. 일부 속성은 기술 중 하나만 지원 합니다.
일부 속성은 두 개 이상의 폼을 지원 합니다. 예를 들어 속성 요소 구문이 나 특성 구문을 사용할 수 있는 속성이 있습니다. 속성 및 속성에서 사용 하는 개체 형식에 따라 가능 합니다. Windows 런타임 API 참조에는 **구문** 섹션에서 사용할 수 있는 XAML 사용이 표시 됩니다. 다른 방법으로도 작동 하지만 더 자세한 정보를 사용 하는 경우도 있습니다. 이러한 자세한 정보 사용은 XAML에서 해당 속성을 사용 하기 위한 모범 사례 또는 실제 시나리오를 표시 하려고 하기 때문에 항상 표시 되지 않습니다. Xaml 구문에 대 한 지침은 xaml로 설정할 수 있는 속성에 대 한 참조 페이지의 **Xaml 사용** 섹션에 제공 됩니다.

개체의 일부 속성은 어떤 방법으로도 XAML로 설정할 수 없으며 코드를 사용 해야만 설정할 수 있습니다. 일반적으로 이러한 속성은 XAML이 아닌 코드 숨김으로 작업 하는 데 더 적합 한 속성입니다.

읽기 전용 속성은 XAML에서 설정할 수 없습니다. 코드 에서도 소유 하는 형식은 생성자 오버 로드, 도우미 메서드 또는 계산 된 속성 지원과 같이 다른 방법으로 설정 하는 다른 방법을 지원 해야 합니다. 계산 된 속성은 설정 가능한 다른 속성의 값을 사용 하 고 때로는 기본 제공 처리를 사용 하는 이벤트를 사용 합니다. 이러한 기능은 종속성 속성 시스템에서 사용할 수 있습니다. 계산 된 속성 지원에 종속성 속성을 활용 하는 방법에 대 한 자세한 내용은 [종속성 속성 개요](dependency-properties-overview.md)를 참조 하세요.

XAML의 컬렉션 구문은 읽기 전용 속성을 설정 하는 모양을 제공 하지만 실제로는 그렇지 않습니다. 이 항목의 뒷부분에 나오는 "[컬렉션 구문](#collection-syntax)"을 참조 하십시오.

### <a name="setting-a-property-by-using-attribute-syntax"></a>특성 구문을 사용 하 여 속성 설정

특성 값을 설정 하는 일반적인 방법은 태그 언어에서 속성 값을 설정 하는 것입니다 (예: XML 또는 HTML). XAML 특성을 설정 하는 방법은 XML에서 특성 값을 설정 하는 방법과 비슷합니다. 특성 이름은 요소 이름 다음에 있는 태그 내에서 하나 이상의 공백으로 구분 되는 요소 이름 뒤에 지정 됩니다. 특성 이름 뒤에 등호를 입력 합니다. 특성 값은 따옴표 쌍 안에 포함 됩니다. 따옴표는 큰따옴표 또는 작은따옴표가 일치 하 고 값을 묶을 수 있습니다. 특성 값 자체는 문자열로 표현할 수 있어야 합니다. 문자열은 종종 숫자를 포함 하지만 xaml에 대해 XAML 파서가 포함 될 때까지 모든 특성 값은 문자열 값 이며 일부 기본적인 값 변환을 수행 합니다.

이 예제에서는 네 가지 특성에 대해 특성 구문을 사용 하 여 [**사각형**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 개체의 [**이름**](/uwp/api/windows.ui.xaml.frameworkelement.name), [**너비**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), [**높이**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height)및 [**채우기**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) 속성을 설정 합니다.

```xml
<Rectangle Name="rectangle1" Width="100" Height="100" Fill="Blue" />
```

### <a name="setting-a-property-by-using-property-element-syntax"></a>속성 요소 구문을 사용 하 여 속성 설정

속성 요소 구문을 사용 하 여 개체의 많은 속성을 설정할 수 있습니다. 속성 요소는 다음과 같습니다. `<` *object* `.` *속성* `>`

속성 요소 구문을 사용 하려면 설정 하려는 속성에 대 한 XAML 속성 요소를 만듭니다. 표준 XML에서이 요소는 이름에 점이 있는 요소로만 간주 됩니다. 그러나 XAML에서 요소 이름의 점은 요소를 속성 요소로 식별 하 고, *속성* 은 지원 개체 모델 구현에서 *개체* 의 멤버가 되어야 합니다. 속성 요소 구문을 사용 하려면 속성 요소 태그를 "채우기" 위해 개체 요소를 지정할 수 있어야 합니다. 속성 요소는 항상 일부 콘텐츠 (단일 요소, 여러 요소 또는 내부 텍스트)를 포함 합니다. 자체 닫기 속성 요소를 가질 필요가 없습니다.

다음 문법에서 *속성* 은 설정 하려는 속성의 이름이 고 *Propertyvalueasobjectelement* 는 단일 개체 요소입니다 .이는 속성의 값 형식 요구 사항을 충족 해야 합니다.

`<`*object*`>`

`<`*개체* `.` *속성*`>`

*propertyValueAsObjectElement*

`</`*개체* `.` *속성*`>`

`</`*object*`>`

다음 예제에서는 속성 요소 구문을 사용 하 여 [**system.windows.media.solidcolorbrush>**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) object 요소를 사용 하는 [**사각형**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 의 [**채우기를**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) 설정 합니다. **System.windows.media.solidcolorbrush>** 내에서 [**색**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 은 특성으로 설정 됩니다. 이 XAML의 구문 분석 된 결과는 특성 구문을 사용 하 여 **Fill** 을 설정 하는 이전 XAML 예제와 동일 합니다.

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

### <a name="xaml-vocabularies-and-object-oriented-programming"></a>XAML 어휘 및 개체 지향 프로그래밍

Windows 런타임 XAML 형식의 XAML 멤버로 표시 되는 속성 및 이벤트는 기본 형식에서 상속 되는 경우가 많습니다. 이 예를 살펴보겠습니다 `<Button Background="Blue" .../>` . [**Background**](/uwp/api/windows.ui.xaml.controls.control.background) 속성은 [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) 클래스에서 즉시 선언 된 속성이 아닙니다. 대신, **배경은** 기본 [**컨트롤**](/uwp/api/Windows.UI.Xaml.Controls.Control) 클래스에서 상속 됩니다. 실제로 **단추** 에 대 한 참조 항목을 살펴보면 멤버 목록에 연속 된 기본 클래스의 각 체인에 있는 하나 이상의 상속 된 멤버 ( [**buttonbase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase), [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control), [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement), [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement), [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject))가 포함 되어 있는 것을 볼 수 있습니다. **속성** 목록에서 모든 읽기/쓰기 속성 및 컬렉션 속성은 XAML 어휘 의미에서 상속 됩니다. 이벤트 (예: 다양 한 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 이벤트)도 상속 됩니다.

XAML 지침에 대 한 Windows 런타임 참조를 사용 하는 경우 해당 참조 항목이 기본 클래스에서 상속 하는 모든 가능한 형식에서 공유 되기 때문에 원래 속성을 정의 하는 형식에 대해 구문 또는 예제 코드에 표시 되는 요소 이름이 될 수도 있습니다. XML 편집기에서 XAML 용 Visual Studio의 IntelliSense를 사용 하는 경우 IntelliSense와 해당 드롭다운은 상속을 결합 하 고 클래스 인스턴스의 개체 요소를 사용 하 여 시작 하면 설정에 사용할 수 있는 정확한 특성 목록을 제공 하는 데 큰 도움이 됩니다.

### <a name="xaml-content-properties"></a>XAML 콘텐츠 속성

일부 형식은 속성에서 XAML 콘텐츠 구문을 사용 하도록 설정 하는 속성 중 하나를 정의 합니다. 형식의 XAML 콘텐츠 속성에 대해 XAML에서 지정 하는 경우 해당 속성에 대 한 속성 요소를 생략할 수 있습니다. 또는 소유 하는 형식의 개체 요소 태그 내에서 직접 내부 텍스트를 제공 하 여 속성을 내부 텍스트 값으로 설정할 수 있습니다. XAML 콘텐츠 속성은 해당 속성에 대 한 간단한 태그 구문을 지원 하며 중첩을 줄여 XAML을 사용자가 더 쉽게 읽을 수 있도록 합니다.

XAML 콘텐츠 구문을 사용할 수 있는 경우 Windows 런타임 참조 설명서에서 해당 속성에 대 한 **구문의** "XAML" 섹션에 해당 구문이 표시 됩니다. 예를 들어 [**border**](/uwp/api/Windows.UI.Xaml.Controls.Border) 의 [**자식**](/uwp/api/windows.ui.xaml.controls.border.child) 속성 페이지는 단일 개체 테두리를 설정 하는 속성 요소 구문 대신 XAML 콘텐츠 구문을 표시 합니다. 예를 들어 다음과 같이 **테두리**의 자식 값을 설정 합니다 **.**

```xml
<Border>
  <Button .../>
</Border>
```

XAML 콘텐츠 속성으로 선언 된 속성이 **개체** 형식 이거나 **문자열**형식인 경우 xaml 콘텐츠 구문은 XML 문서 모델에서 기본적으로 내부 텍스트를 지원 합니다. 즉, 여는 개체 태그와 닫는 개체 태그 사이의 문자열을 지원 합니다. 예를 들어 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 의 [**text**](/uwp/api/windows.ui.xaml.controls.textblock.text) 속성 페이지에는 **텍스트**를 설정 하기 위한 내부 텍스트 값이 있는 XAML 콘텐츠 구문이 표시 되지만 문자열 "텍스트"는 태그에 표시 되지 않습니다. 사용법은 다음과 같습니다.

```xml
<TextBlock>Hello!</TextBlock>
```

클래스에 대 한 XAML 콘텐츠 속성이 있는 경우 클래스에 대 한 참조 항목의 "Attributes" 섹션에서 표시 됩니다. [**ContentPropertyAttribute**](/uwp/api/Windows.UI.Xaml.Markup.ContentPropertyAttribute)의 값을 찾습니다. 이 특성은 명명 된 필드 "Name"을 사용 합니다. "Name" 값은 XAML 콘텐츠 속성인 해당 클래스의 속성 이름입니다. 예를 들어 [**테두리**](/uwp/api/Windows.UI.Xaml.Controls.Border) 참조 페이지에 contentproperty ("Name = Child")가 표시 됩니다.

한 가지 중요 한 XAML 구문 규칙은 XAML 콘텐츠 속성과 요소에 대해 설정 하는 기타 속성 요소를 섞 수 없다는 것입니다. XAML 콘텐츠 속성은 속성 요소 앞 이나 완전히 뒤에 설정 되어야 합니다. 예를 들어 다음은 잘못 된 XAML입니다.

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

지금까지 표시 된 모든 구문은 속성을 단일 개체로 설정 합니다. 하지만 대부분의 UI 시나리오에는 지정 된 부모 요소에 자식 요소가 여러 개 있을 수 있어야 합니다. 예를 들어 입력 양식에 대 한 UI에는 여러 개의 텍스트 상자 요소, 일부 레이블 및 "제출" 단추가 필요 합니다. 그러나 프로그래밍 개체 모델을 사용 하 여 이러한 여러 요소에 액세스 하는 경우에는 일반적으로 각 항목이 서로 다른 속성의 값이 아니라 단일 컬렉션 속성에 있는 항목입니다. XAML은 컬렉션 형식을 암시적으로 사용 하는 속성을 처리 하 고 컬렉션 형식의 모든 자식 요소에 대해 특별 한 처리를 수행 하 여 일반적인 지원 컬렉션 모델을 지원할 뿐만 아니라 여러 자식 요소를 지원 합니다.

많은 컬렉션 속성은 클래스에 대 한 XAML 콘텐츠 속성으로도 식별 됩니다. 컨트롤 합성에 사용 되는 형식 (예: 패널, 뷰 또는 항목 컨트롤)에서는 암시적 컬렉션 처리 및 XAML 콘텐츠 구문의 조합이 자주 나타납니다. 예를 들어 다음 예제에서는 [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel)내에서 두 개의 피어 UI 요소를 합성 하는 데 사용할 수 있는 가장 간단한 XAML을 보여 줍니다.

```xml
<StackPanel>
  <TextBlock>Hello</TextBlock>
  <TextBlock>World</TextBlock>
</StackPanel>
```

### <a name="the-mechanism-of-xaml-collection-syntax"></a>XAML 컬렉션 구문의 메커니즘

처음에는 XAML에서 읽기 전용 컬렉션 속성의 "설정"을 사용 하도록 설정 하는 것 처럼 보일 수 있습니다. 실제로 여기서는 기존 컬렉션에 항목을 추가 하는 XAML을 사용 합니다. Xaml 지원을 구현 하는 xaml 언어 및 XAML 프로세서는이 구문을 사용 하기 위해 컬렉션 형식 지원의 규칙에 의존 합니다. 일반적으로 컬렉션의 특정 항목을 참조 하는 인덱서 또는 **항목** 속성과 같은 지원 속성이 있습니다. 일반적으로이 속성은 XAML 구문에서 명시적이 지 않습니다. 컬렉션의 경우 XAML 구문 분석을 위한 기본 메커니즘은 속성이 아니라 메서드 (특히, 대부분의 경우 **Add** 메서드)입니다. XAML 프로세서는 XAML 컬렉션 구문 내에서 하나 이상의 개체 요소를 발견할 경우 각 개체를 먼저 요소에서 만든 다음 컬렉션의 **Add** 메서드를 호출 하 여 포함 하는 컬렉션에 새 개체를 각각 추가 합니다.

XAML 파서가 컬렉션에 항목을 추가 하는 경우이는 지정 된 XAML 요소가 컬렉션 개체의 허용 가능한 항목 자식 인지 여부를 확인 하는 **Add** 메서드의 논리입니다. 많은 컬렉션 형식은 지원 구현에 의해 강력 하 게 형식화 됩니다. 즉, **추가** 의 입력 매개 변수는 전달 된 항목이 **add** 매개 변수 형식과 일치 하는 형식 이어야 합니다.

컬렉션 속성의 경우 컬렉션을 개체 요소로 명시적으로 지정 하려고 하면 주의 해야 합니다. XAML 파서는 개체 요소를 발견할 때마다 새 개체를 만듭니다. 사용 하려는 컬렉션 속성이 읽기 전용 이면 XAML 구문 분석 예외가 throw 될 수 있습니다. 암시적 컬렉션 구문을 사용하면 이러한 예외가 표시되지 않습니다.

## <a name="when-to-use-attribute-or-property-element-syntax"></a>특성 또는 속성 요소 구문을 사용 하는 경우

XAML에서 설정 하는 것을 지 원하는 모든 속성은 직접 값 설정에 대 한 특성 또는 속성 요소 구문을 지원 하지만 두 구문을 서로 바꿔 사용할 수 있습니다. 일부 속성은 두 구문을 모두 지원 하며, 일부 속성은 XAML 콘텐츠 속성과 같은 추가 구문 옵션을 지원 합니다. 속성에서 지 원하는 XAML 구문의 형식은 속성이 속성 형식으로 사용 하는 개체의 형식에 따라 달라 집니다. 속성 형식이 double (float 또는 decimal), 정수, 부울 또는 문자열과 같은 기본 형식인 경우 속성은 항상 특성 구문을 지원 합니다.

문자열을 처리 하 여 속성을 설정 하는 데 사용 하는 개체 형식을 만들 수 있는 경우 특성 구문을 사용 하 여 속성을 설정할 수도 있습니다. 기본 형식에 대해 항상이 경우 형식 변환이 파서에 기본 제공 됩니다. 그러나 다른 특정 개체 형식은 속성 요소 내의 개체 요소가 아니라 특성 값으로 지정 된 문자열을 사용 하 여 만들 수도 있습니다. 이 작업을 수행 하려면 특정 속성에 의해 지원 되거나 해당 속성 형식을 사용 하는 모든 값에 대해 일반적으로 지원 되는 기본 형식 변환이 있어야 합니다. 특성의 문자열 값은 새 개체 값을 초기화 하는 데 중요 한 속성을 설정 하는 데 사용 됩니다. 잠재적으로 특정 형식 변환기는 문자열의 정보를 고유 하 게 처리 하는 방법에 따라 공용 속성 형식의 다른 서브 클래스를 만들 수도 있습니다. 이 동작을 지원하는 개체 형식에는 참조 설명서의 구문 섹션에 나열된 특수 문법이 있습니다. 예를 들어 [**브러시**](/uwp/api/Windows.UI.Xaml.Media.Brush) 의 xaml 구문은 특성 구문을 사용 하 여 **brush** 형식의 모든 속성에 대 한 새 [**system.windows.media.solidcolorbrush>**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) 값을 만드는 방법을 보여 줍니다 .이 경우 xaml Windows 런타임의 **브러시** 속성은 여러 가지가 있습니다.

## <a name="xaml-parsing-logic-and-rules"></a>XAML 구문 분석 논리 및 규칙

Xaml 파서가이를 읽는 방법과 유사한 방식으로 XAML을 읽는 방법에 대 한 정보를 제공 합니다 .이는 선형 순서에서 발생 한 문자열 토큰 집합입니다. Xaml 파서는 XAML 작동 방식에 대 한 정의의 일부인 규칙 집합에서 이러한 토큰을 해석 해야 합니다.

특성 값을 설정 하는 일반적인 방법은 태그 언어에서 속성 값을 설정 하는 것입니다 (예: XML 또는 HTML). 다음 구문에서 *objectName* 은 인스턴스화할 개체이 고, *propertyName* 은 해당 개체에 대해 설정 하려는 속성의 이름이 며, *propertyValue* 는 설정할 값입니다.

```xml
<objectName propertyName="propertyValue" .../>

-or-

<objectName propertyName="propertyValue">

...<!--element children -->

</objectName>
```

두 구문을 사용 하 여 개체를 선언 하 고 해당 개체에 대 한 속성을 설정할 수 있습니다. 첫 번째 예제는 태그의 단일 요소 이지만 XAML 프로세서에서이 태그를 구문 분석 하는 방법과 관련 하 여 여기에는 실제로 불연속 단계가 있습니다.

첫째, object 요소가 있으면 새 *objectName* 개체가 인스턴스화 되어야 함을 나타냅니다. 이러한 인스턴스가 있는 후에만 인스턴스 속성 *propertyName* 을 설정할 수 있습니다.

XAML의 또 다른 규칙은 요소의 특성을 순서에 관계 없이 설정할 수 있어야 한다는 것입니다. 예를 들어와 사이에는 차이가 `<Rectangle Height="50" Width="100" />` 없습니다 `<Rectangle Width="100"  Height="50" />` . 사용 하는 순서는 스타일의 문제입니다.

**참고**    XML 편집기를 사용 하지 않는 디자인 화면을 사용 하는 경우 XAML 디자이너는 일반적으로 정렬 규칙을 승격 하지만 나중에이 XAML을 자유롭게 편집 하 여 특성의 순서를 변경 하거나 새 특성을 도입할 수 있습니다.

## <a name="attached-properties"></a>연결된 속성

XAML은 *연결 된 속성*이라는 구문 요소를 추가 하 여 XML을 확장 합니다. 속성 요소 구문과 마찬가지로 연결 된 속성 구문에는 점이 포함 되 고, 점은 XAML 구문 분석에 특별 한 의미를 갖습니다. 특히이 점이 연결 된 속성의 공급자와 속성 이름을 구분 합니다.

XAML에서 *AttachedPropertyProvider*구문을 사용 하 여 연결 된 속성을 설정 합니다. *PropertyName* 다음은 연결 된 속성 캔버스를 XAML로 남겨 둘 수 있는 방법의 예입니다 [**.**](/dotnet/api/system.windows.controls.canvas.left)

```xml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

지원 형식에 해당 이름의 속성이 없는 요소에 대해 연결 된 속성을 설정할 수 있으며, 이러한 방식으로 전역 속성 또는 **xml: space** 특성과 같은 다른 xml 네임 스페이스에 정의 된 특성과 유사 하 게 작동 합니다.

Windows 런타임 XAML에서 이러한 시나리오를 지 원하는 연결 된 속성이 표시 됩니다.

-   자식 요소는 부모 컨테이너 패널의 레이아웃에서 동작 하는 방법을 알릴 수 있습니다. [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas), [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid), [**VariableSizedWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid).
-   컨트롤 사용은 컨트롤 템플릿에서 제공 되는 중요 한 컨트롤 파트의 동작 ( [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer), [**VirtualizingStackPanel**](/uwp/api/Windows.UI.Xaml.Controls.VirtualizingStackPanel))에 영향을 줄 수 있습니다.
-   서비스 및이를 사용 하는 클래스에서 사용할 수 있는 관련 클래스에서 사용할 수 있는 서비스를 사용 하는 경우에는 [**입력 체계**](/uwp/api/Windows.UI.Xaml.Documents.Typography), [**r**](/uwp/api/Windows.UI.Xaml.VisualStateManager), [**automationproperties**](/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties), [**tooltipservice.tooltip**](/uwp/api/Windows.UI.Xaml.Controls.ToolTipService)가 상속 되지 않습니다.
-   애니메이션 대상 지정: [**스토리 보드**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard).

자세한 내용은 [연결 된 속성 개요](attached-properties-overview.md)를 참조 하세요.

## <a name="literal--values"></a>리터럴 "{" 값

여는 중괄호 기호는 \{ 태그 확장 시퀀스의 여는 것 이므로 이스케이프 시퀀스를 사용 하 여 ""로 시작 하는 리터럴 문자열 값을 지정 \{ 합니다. 이스케이프 시퀀스는 " \{ \} "입니다. 예를 들어 한 여는 중괄호 인 문자열 값을 지정 하려면 특성 값을 ""로 지정 \{ \} \{ 합니다. 대체 따옴표 (예: **""** 로 구분 된 특성 값 내에서 **'** )를 사용 하 여 "" 값을 문자열로 제공할 수도 있습니다 \{ .

**참고**    " \\ }" 는 따옴표로 묶인 특성 내에 있는 경우에도 작동 합니다.
 
## <a name="enumeration-values"></a>열거형 값

Windows 런타임 API의 많은 속성은 열거형을 값으로 사용 합니다. 멤버가 읽기/쓰기 속성인 경우 특성 값을 제공 하 여 이러한 속성을 설정할 수 있습니다. 상수 이름의 정규화 되지 않은 이름을 사용 하 여 속성의 값으로 사용할 열거형 값을 식별 합니다. 예를 들어 다음은 XAML에서 [**UIElement**](/uwp/api/windows.ui.xaml.uielement.visibility) 를 설정 하는 방법 `<Button Visibility="Visible"/>` 입니다. 여기서 "표시"는 문자열을 표시 [**유형**](/uwp/api/Windows.UI.Xaml.Visibility) 열거형의 명명 된 상수에 직접 **매핑합니다.**

-   정규화 된 형태를 사용 하지 않으면 작동 하지 않습니다. 예를 들어이는 잘못 된 XAML `<Button Visibility="Visibility.Visible"/>` 입니다.
-   상수 값을 사용 하지 마세요. 즉, 열거형의 정의 방법에 따라 명시적 또는 암시적으로 열거의 정수 값을 사용 하지 마세요. 작동 하는 것 처럼 보일 수 있지만, 일시적 구현 세부 정보를 사용할 수 있는 항목에 의존 하기 때문에 XAML 또는 코드에서 잘못 된 습관입니다. 예를 들어 다음을 수행 하지 마세요. `<Button Visibility="1"/>` .

**참고**    XAML을 사용 하 고 열거형을 사용 하는 Api에 대 한 참조 항목에서 **구문의** **속성 값** 섹션에서 열거형 형식에 대 한 링크를 클릭 합니다. 이 열거형에 대 한 명명 된 상수를 검색할 수 있는 열거형 페이지에 연결 됩니다.

열거형은 플래그 수 있습니다. 즉, **FlagsAttribute**로 특성이 지정 됩니다. 플래그 열거의 값 조합을 XAML 특성 값으로 지정 해야 하는 경우 각 열거형 상수의 이름을 사용 하 고 각 이름 사이에 쉼표 (,)를 사용 하 고 공백 문자를 사용 하지 않습니다. 플래그 특성은 Windows 런타임 XAML 어휘에서 일반적이 지 않지만 [**ManipulationModes**](/uwp/api/Windows.UI.Xaml.Input.ManipulationModes) 는 xaml에서 플래그 열거형 값을 설정 하는 예제입니다.

## <a name="interfaces-in-xaml"></a>XAML의 인터페이스

드문 경우 지만 속성의 형식이 인터페이스인 XAML 구문이 표시 됩니다. XAML 형식 시스템에서 해당 인터페이스를 구현 하는 형식은 구문 분석할 때 값으로 허용 됩니다. 값으로 사용할 수 있는 이러한 형식의 인스턴스가 생성 되어야 합니다. [**Buttonbase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase)의 [**Command**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command) 및 [**COMMANDPARAMETER**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.commandparameter) 속성에 대 한 XAML 구문에서 형식으로 사용 되는 인터페이스가 표시 됩니다. 이러한 속성은 MVVM (모델-뷰-ViewModel) 디자인 패턴을 지원 합니다. 여기서 **ICommand** 인터페이스는 뷰와 모델이 상호 작용 하는 방식에 대 한 계약입니다.

## <a name="xaml-placeholder-conventions-in-windows-runtime-reference"></a>Windows 런타임 참조의 XAML 자리 표시자 규칙

XAML을 사용할 수 있는 Windows 런타임 Api에 대 한 참조 항목의 **구문** 섹션을 검토 한 경우 구문에 몇 가지 자리 표시 자가 포함 되어 있는 것을 볼 수 있습니다. Xaml 구문은 c #, Microsoft Visual Basic 또는 Visual C++ 구성 요소 확장 (c + +/CX) 구문과 다르므로 XAML 구문은 사용 구문입니다. 사용자 고유의 XAML 파일에서 최종 사용을 힌트 하지만 사용할 수 있는 값에 대 한 자세한 내용은 안내 하지 않습니다. 따라서 일반적으로 사용법은 리터럴과 자리 표시자를 혼합 하는 문법 유형을 설명 하 고 **XAML 값** 섹션에서 일부 자리 표시자를 정의 합니다.

속성의 XAML 구문에 형식 이름/요소 이름이 표시 되 면 원래 속성을 정의 하는 형식에 대 한 이름이 표시 됩니다. 그러나 Windows 런타임 XAML은 [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject)기반 클래스에 대 한 클래스 상속 모델을 지원 합니다. 따라서 종종 클래스를 정의 하는 클래스가 아닌 클래스에서 특성을 사용할 수 있지만, 대신 속성/특성을 먼저 정의한 클래스에서 파생 됩니다. 예를 들어 심층 상속을 사용 하 여 모든 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 파생 클래스에서 [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) 를 특성으로 설정할 수 있습니다. 예: `<Button Visibility="Visible" />` 따라서 XAML 사용 구문에 표시 되는 요소 이름을 너무 문자 그대로 사용 하지 마십시오. 구문은 해당 클래스를 나타내는 요소 및 파생 클래스를 나타내는 요소에 대해 실행 될 수 있습니다. 정의 요소로 표시 되는 형식이 실제 사용에 있을 때 거의 또는 불가능 한 경우 해당 형식 이름이 구문에서 의도적으로 lowercased 됩니다. 예를 들어, UIElement에 대해 표시 되는 구문은 다음과 같습니다 **.**

``` syntax
<uiElement Visibility="Visible"/>
-or-
<uiElement Visibility="Collapsed"/>
```

대부분의 XAML 구문 섹션에는 **구문** 섹션 바로 아래에 있는 **xaml 값** 섹션에서 정의 되는 "사용"의 자리 표시 자가 포함 되어 있습니다.

XAML 사용 섹션은 또한 다양 한 일반화 된 자리 표시자를 사용 합니다. 이러한 자리 표시자는 **XAML 값**에서 매번 다시 정의 되지 않습니다. 대부분의 판독기는 **XAML 값** 에 다시 표시 되지 않으므로 정의에서 제외 하는 것으로 간주 됩니다. 참조를 위해 다음은 이러한 자리 표시자의 목록과 일반적인 의미를 의미 합니다.

-   *object*: 이론적으로 모든 개체 값 이지만 일반적으로 문자열 또는 개체 선택 등의 특정 개체 유형으로 제한 되며, 참조 페이지의 설명에서 자세한 정보를 확인 해야 합니다.
-   *개체* *속성*: 조합 된 *개체* *속성* 은 표시 되는 구문이 여러 속성의 특성 값으로 사용할 수 있는 형식에 대 한 구문 인 경우에 사용 됩니다. 예를 들어 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)에 대해 표시된 **XAML 특성 사용**에는 &lt;*object* *property*="*predefinedColorName*"/&gt;이 포함됩니다.
-   *eventhandler*: 이벤트 특성에 대해 표시 되는 모든 XAML 구문에 대 한 특성 값으로 나타납니다. 여기에서 제공 하는 내용은 이벤트 처리기 함수의 함수 이름입니다. XAML 페이지의 코드 숨김으로 해당 함수를 정의 해야 합니다. 프로그래밍 수준에서 해당 함수는 처리 중인 이벤트의 대리자 시그니처와 일치 해야 합니다. 그렇지 않으면 앱 코드가 컴파일되지 않습니다. 그러나이는 XAML을 고려 하지 않는 프로그래밍 고려 사항 이므로 XAML 구문에서 대리자 형식에 대 한 힌트를 제공 하지 않습니다. 이벤트에 대해 구현 해야 하는 대리자를 확인 하려는 경우 해당 이벤트에 대 한 참조 항목의 **이벤트 정보** 섹션에 있는 **대리자**라는 레이블이 지정 된 테이블 행에 있습니다.
-   *enumMemberName*: 모든 열거형의 특성 구문에 표시 됩니다. 열거형 값을 사용 하는 속성에는 비슷한 자리 표시 자가 있지만 일반적으로 자리 표시자의 이름을 열거형 이름 힌트와 접두사로 사용 합니다. 예를 들어 [**frameworkelement**](/uwp/api/windows.ui.xaml.frameworkelement.flowdirection) 에 대해 표시 된 구문은 <*Frameworkelement ***flowdirection**= "* flowDirectionMemberName *"/>입니다. 이러한 속성 참조 페이지 중 하나에 있는 경우 텍스트 **형식**옆에 있는 **속성 값** 섹션에 표시 되는 열거형 형식에 대 한 링크를 클릭 합니다. 해당 열거형을 사용 하는 속성의 특성 값에 대해 **멤버** 목록의 **멤버** 열에 나열 된 모든 문자열을 사용할 수 있습니다.
-   *double*, *int*, *string*, *bool*: XAML 언어에 알려진 기본 형식입니다. C # 또는 Visual Basic를 사용 하 여 프로그래밍 하는 경우 이러한 형식은 [**Double**](/dotnet/api/system.double), [**Int32**](/dotnet/api/system.int32), [**String**](/dotnet/api/system.string) 및 [**Boolean**](/dotnet/api/system.boolean)과 같은 동등한 형식 Microsoft .NET 프로젝션 되며 .net 코드 숨김으로 XAML 정의 값으로 작업할 때 해당 .net 형식의 멤버를 사용할 수 있습니다. C + +/CX를 사용 하 여 프로그래밍 하는 경우 c + + 기본 형식을 사용 하지만 platform [**:: String**](/cpp/cppcx/platform-string-class)과 같은 [**플랫폼**](/cpp/cppcx/platform-namespace-c-cx) 네임 스페이스에 정의 된 형식에 해당 하는 것을 고려할 수도 있습니다. 특정 속성에 대 한 추가 값 제한이 있을 수 있습니다. 그러나 이러한 제한 사항은 코드 사용 및 XAML 사용에 모두 적용 되기 때문에 이러한 제한 사항은 일반적으로 **속성 값** 섹션 또는 설명 섹션에 표시 되 고 xaml 섹션에는 표시 되지 않습니다.

## <a name="tips-and-tricks-notes-on-style"></a>팁과 요령, 스타일에 대 한 참고 사항

-   일반적으로 태그 확장은 기본 [XAML 개요](xaml-overview.md)에 설명 되어 있습니다. 그러나이 항목에서 제공 하는 지침에 가장 많은 영향을 주는 태그 확장은 [StaticResource](staticresource-markup-extension.md) 태그 확장 ( [및 관련 된](themeresource-markup-extension.md)항목)입니다. StaticResource 태그 확장의 함수는 xaml을 XAML [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)에서 제공 되는 재사용 가능한 리소스로 팩터링 하도록 설정 하는 것입니다. **ResourceDictionary**에서 컨트롤 템플릿 및 관련 스타일을 거의 항상 정의 합니다. 응용 프로그램에서 UI의 다른 부분에 대해 두 번 이상 사용 하는 색에 대 한 [**system.windows.media.solidcolorbrush>**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) 같이 **ResourceDictionary** 에서 컨트롤 템플릿 정의 또는 앱 별 스타일의 더 작은 부분을 정의 하는 경우가 많습니다. StaticResource를 사용 하 여 속성 요소 사용을 설정 해야 하는 속성은 이제 특성 구문에서 설정할 수 있습니다. 그러나 XAML을 다시 사용 하는 경우에는 페이지 수준 구문을 간소화 하는 것 외에도 XAML을 팩터링 하는 이점이 있습니다. 자세한 내용은 [ResourceDictionary 및 XAML 리소스 참조](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)를 확인하세요.
-   XAML 예제에서 공백과 줄 바꿈이 적용 되는 방법에 대 한 몇 가지 다른 규칙이 표시 됩니다. 특히 여러 특성이 설정 된 개체 요소를 분리 하는 방법에 대 한 규칙은 서로 다릅니다. 이것은 스타일의 문제입니다. Visual Studio XML 편집기는 XAML을 편집할 때 몇 가지 기본 스타일 규칙을 적용 하지만 설정에서 변경할 수 있습니다. XAML 파일의 공백이 중요 한 것으로 간주 되는 적은 수의 사례가 있습니다. 자세한 내용은 [XAML 및 공백](xaml-and-whitespace.md)을 참조 하십시오.

## <a name="related-topics"></a>관련 항목

* [XAML 개요](xaml-overview.md)
* [XAML 네임스페이스 및 네임스페이스 매핑](xaml-namespaces-and-namespace-mapping.md)
* [ResourceDictionary 및 XAML 리소스 참조](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)
 