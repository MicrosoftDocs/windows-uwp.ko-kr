---
description: XAML에서 연결 된 속성의 개념을 설명 하 고 몇 가지 예를 제공 합니다.
title: 연결된 속성 개요
ms.assetid: 098C1DE0-D640-48B1-9961-D0ADF33266E2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: cc1a879944054ce8fd2b2ea8a2f53aba4d202358
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155117"
---
# <a name="attached-properties-overview"></a>연결된 속성 개요

*연결 된 속성* 은 XAML 개념입니다. 연결 된 속성을 사용 하면 개체에 대해 추가 속성/값 쌍을 설정할 수 있지만 속성은 원래 개체 정의의 일부가 아닙니다. 일반적으로 연결 된 속성은 소유자 형식의 개체 모델에 기존 속성 래퍼가 없는 종속성 속성의 특수 한 형식으로 정의 됩니다.

## <a name="prerequisites"></a>필수 구성 요소

종속성 속성의 기본 개념을 이해 하 고 [종속성 속성 개요](dependency-properties-overview.md)를 읽는 것으로 가정 합니다.

## <a name="attached-properties-in-xaml"></a>XAML의 연결 된 속성

XAML에서 _AttachedPropertyProvider_구문을 사용 하 여 연결 된 속성을 설정 합니다. 다음은 XAML에서 Canvas를 설정 하는 방법의 예입니다 [**.**](/dotnet/api/system.windows.controls.canvas.left)

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

> [!NOTE]
> 사용 해야 하는 이유를 완전히 설명 하지 않고 [**Canvas를**](/dotnet/api/system.windows.controls.canvas.left) 연결 된 속성 예제로 사용 하는 것입니다. **캔버스** 에 대해 자세히 알아보려면 캔버스는 무엇 [**이며 캔버스는 레이아웃 자식을 처리 하**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) 는 방법에 대해 자세히 알아보려면 [**canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) 참조 항목 또는 [XAML로 레이아웃 정의](../design/layout/layouts-with-xaml.md)를 참조 하세요.

## <a name="why-use-attached-properties"></a>연결 된 속성을 사용 하는 이유

연결 된 속성은 관계의 여러 개체가 런타임에 정보를 서로 통신 하는 것을 방해할 수 있는 코딩 규칙을 이스케이프 하는 방법입니다. 각 개체가 해당 속성을 가져오고 설정할 수 있도록 공용 기본 클래스에 속성을 배치할 수 있습니다. 그러나이 작업을 수행 하려는 경우에는 공유 가능한 속성을 사용 하 여 기본 클래스를 더 많이 사용할 수 있습니다. 속성을 사용 하려는 경우 두 개의 하위 항목이 있을 수 있는 경우에도 발생할 수 있습니다. 이것은 좋은 클래스 디자인이 아닙니다. 이를 해결 하기 위해 연결 된 속성 개념은 개체가 자체 클래스 구조에서 정의 하지 않는 속성에 대 한 값을 할당할 수 있도록 합니다. 정의 클래스는 개체 트리에서 다양 한 개체가 만들어진 후 런타임에 자식 개체에서 값을 읽을 수 있습니다.

예를 들어 자식 요소는 연결 된 속성을 사용 하 여 UI에 표시 되는 방식에 대 한 부모 요소를 알릴 수 있습니다. 이는 [**Canvas. 왼쪽**](/dotnet/api/system.windows.controls.canvas.left) 에 연결 된 속성을 사용 하는 경우입니다. **Canvas** [**는 캔버스 자체가 아니라 canvas 요소 내**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) 에 포함 된 요소에 대해 설정 되기 때문에 연결 된 속성으로 만들어집니다 **.** 그러면 가능한 모든 자식 요소가 **canvas. Left** 및 [**canvas**](/dotnet/api/system.windows.controls.canvas.top) 를 사용 하 여 **캔버스** 레이아웃 컨테이너 부모 내에서 해당 레이아웃 오프셋을 지정 합니다. 연결 된 속성을 사용 하면 가능한 여러 레이아웃 컨테이너 중 하나에만 적용 되는 다양 한 속성을 사용 하 여 기본 요소의 개체 모델을 복잡 하 게 만들지 않고도이 작업을 수행할 수 있습니다. 대신, 대부분의 레이아웃 컨테이너는 자체 연결 된 속성 집합을 구현 합니다.

연결 된 속성을 구현 하기 위해 [**canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) 클래스는 [**Canvas. 왼쪽 속성**](/uwp/api/windows.ui.xaml.controls.canvas.leftproperty)이라는 정적 [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) 필드를 정의 합니다. 그런 다음, **Canvas** 는 [**Setleft**](/uwp/api/windows.ui.xaml.controls.canvas.setleft) 및 [**GetLeft**](/uwp/api/windows.ui.xaml.controls.canvas.getleft) 메서드를 연결 된 속성에 대 한 PUBLIC 접근자로 제공 하 여 XAML 설정 및 런타임 값 액세스를 모두 사용 하도록 설정 합니다. XAML 및 종속성 속성 시스템의 경우이 Api 집합은 연결 된 속성에 대해 특정 XAML 구문을 사용할 수 있도록 하는 패턴을 충족 하 고 종속성 속성 저장소에 값을 저장 합니다.

## <a name="how-the-owning-type-uses-attached-properties"></a>소유 하는 형식에서 연결 된 속성을 사용 하는 방법

연결 된 속성은 모든 XAML 요소 (또는 기본 [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject))에서 설정할 수 있지만, 속성 설정이 결과를 생성 하거나 값에 액세스 하는 것을 자동으로 의미 하지 않습니다. 연결 된 속성을 정의 하는 형식은 일반적으로 다음 시나리오 중 하나를 따릅니다.

- 연결 된 속성을 정의 하는 형식은 다른 개체와의 관계에서 부모입니다. 자식 개체는 연결 된 속성에 대 한 값을 설정 합니다. 연결 된 속성 소유자 형식에는 자식 요소를 반복 하 고, 값을 가져오고, 개체 수명의 특정 시점 (레이아웃 작업, [**SizeChanged**](/uwp/api/windows.ui.xaml.frameworkelement.sizechanged)등)에서 해당 값에 대해 동작 하는 일부 innate 동작이 있습니다.
- 연결 된 속성을 정의 하는 형식은 다양 한 부모 요소 및 콘텐츠 모델에 대 한 자식 요소로 사용 되지만 정보는 레이아웃 정보가 아닐 수 있습니다.
- 연결 된 속성은 다른 UI 요소가 아닌 서비스에 정보를 보고 합니다.

이러한 시나리오와 소유 형식에 대 한 자세한 내용은 [사용자 지정 연결 된 속성](custom-attached-properties.md)의 "Canvas에 대 한 자세한 정보" 섹션을 참조 하세요.

## <a name="attached-properties-in-code"></a>코드의 연결 된 속성

연결 된 속성에는 다른 종속성 속성과 같이 쉽게 get 및 set 액세스를 위한 일반적인 속성 래퍼가 없습니다. 이는 연결 된 속성이 속성이 설정 된 인스턴스에 대 한 코드 중심 개체 모델에 포함 될 필요가 없기 때문입니다. (일반적이 지 않은 경우 다른 형식에서 자체적으로 설정할 수 있는 연결 된 속성인 속성을 정의 하 고 소유 하는 형식에 대 한 기존 속성 사용도 있는 경우에는 허용 되지 않습니다.)

코드에서 연결 된 속성을 설정 하는 방법에는 속성 시스템 Api를 사용 하거나 XAML 패턴 접근자를 사용 하는 두 가지 방법이 있습니다. 이러한 기술은 최종 결과와 거의 동일 하므로 사용 해야 하는 것은 대부분 코딩 스타일의 문제입니다.

### <a name="using-the-property-system"></a>속성 시스템 사용

Windows 런타임에 대 한 연결 된 속성은 종속성 속성으로 구현 되므로 속성 시스템에서 공유 종속성 속성 저장소에 값을 저장할 수 있습니다. 따라서 연결 된 속성은 소유 하는 클래스에서 종속성 속성 식별자를 노출 합니다.

코드에서 연결 된 속성을 설정 하려면 [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) 메서드를 호출 하 고이 연결 된 속성의 식별자 역할을 하는 [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) 필드를 전달 합니다. 설정 하려면 값도 전달 합니다.

코드에서 연결 된 속성의 값을 가져오려면 [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) 메서드를 호출 하 여 식별자 역할을 하는 [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) 필드를 다시 전달 합니다.

### <a name="using-the-xaml-accessor-pattern"></a>XAML 접근자 패턴 사용

Xaml 프로세서는 XAML을 개체 트리로 구문 분석할 때 연결 된 속성 값을 설정할 수 있어야 합니다. 연결 된 속성의 소유자 형식은 **Get**_propertyname_ 및 **Set**_propertyname_형식의 이라는 전용 접근자 메서드를 구현 해야 합니다. 이러한 전용 접근자 메서드는 코드에서 연결 된 속성을 가져오거나 설정 하는 한 가지 방법 이기도 합니다. 코드 관점에서 연결 된 속성은 속성 접근자 대신 메서드 접근자가 있는 지원 필드와 유사 하며, 해당 지원 필드는 구체적으로 정의 해야 하는 것이 아니라 모든 개체에 있을 수 있습니다.

다음 예에서는 XAML 접근자 API를 통해 코드에서 연결된 속성을 설정하는 방법을 보여 줍니다. 이 예제에서 `myCheckBox` 는 [**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 클래스의 인스턴스입니다. 마지막 줄은 실제로 값을 설정 하는 코드입니다. 앞의 줄은 인스턴스와 부모-자식 관계만 설정 합니다. 주석 처리가 제거 마지막 줄은 속성 시스템을 사용 하는 경우의 구문입니다. 주석 처리 된 마지막 줄은 XAML 접근자 패턴을 사용 하는 경우 구문입니다.

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

사용자 지정 연결 된 속성을 정의 하는 방법에 대 한 코드 예제 및 연결 된 속성을 사용 하기 위한 시나리오에 대 한 자세한 내용은 [사용자 지정 연결 된 속성](custom-attached-properties.md)을 참조 하세요.

## <a name="special-syntax-for-attached-property-references"></a>연결 된 속성 참조에 대 한 특수 구문

연결 된 속성 이름의 점은 식별 패턴의 핵심 부분입니다. 구문이 나 상황에서 다른 의미를 갖는 점을 처리 하는 경우 모호성이 있을 수 있습니다. 예를 들어 점이 바인딩 경로에 대 한 개체 모델 순회로 처리 됩니다. 이러한 모호성을 포함 하는 대부분의 경우 연결 된 속성에 대 한 특수 구문이 있습니다 .이를 통해 내부 점이 _소유자_로 구문 분석 될 수 있습니다 **.** 연결 된 속성의 _속성_ 구분 기호입니다.

- 애니메이션에 대 한 대상 경로의 일부로 연결 된 속성을 지정 하려면 연결 된 속성 이름을 괄호 ("()")로 묶습니다 (예: "(Canvas; Left)"). 자세한 내용은 [속성 경로 구문](property-path-syntax.md)을 참조하세요.

> [!WARNING]
> Windows 런타임 XAML 구현의 기존 제한 사항은 사용자 지정 연결 된 속성에 애니메이션 효과를 적용할 수 없다는 것입니다.

- 리소스 파일에서 **x:Uid**로 리소스 참조에 대 한 대상 속성으로 연결 된 속성을 지정 하려면 대괄호 ("") 내에서 선언을 **사용 하 여** 정규화 된 코드 스타일을 삽입 하는 특수 구문을 사용 하 여 \[ \] 의도적인 범위 나누기를 만듭니다. 예를 들어, 요소 `<TextBlock x:Uid="Title" />` , 즉 캔버스를 대상으로 하는 리소스 파일의 리소스 키가 있다고 가정 **합니다.** 해당 인스턴스의 Top 값은 "Title입니다. \[ : Windows. .Xaml. Controls \] Canvas. Top "을 사용 합니다. 리소스 파일 및 XAML에 대한 자세한 내용은 [빠른 시작: UI 리소스 변환](/previous-versions/windows/apps/hh965329(v=win.10))을 참조하세요.

## <a name="related-topics"></a>관련 항목

- [사용자 지정 연결된 속성](custom-attached-properties.md)
- [종속성 속성 개요](dependency-properties-overview.md)
- [XAML을 사용 하 여 레이아웃 정의](../design/layout/layouts-with-xaml.md)
- [빠른 시작: UI 리소스 변환](/previous-versions/windows/apps/hh943060(v=win.10))
- [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue)
- [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue)