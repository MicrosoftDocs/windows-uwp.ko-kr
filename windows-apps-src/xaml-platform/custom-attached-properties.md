---
author: jwmsft
description: XAML 연결된 속성을 종속성 속성으로 구현하는 방법 및 연결된 속성을 XAML에서 사용 가능하게 하는 데 필요한 접근자 규칙을 정의하는 방법에 대해 설명합니다.
title: 사용자 지정 연결된 속성
ms.assetid: E9C0C57E-6098-4875-AA3E-9D7B36E160E0
ms.author: jimwalk
ms.date: 07/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: ce26242f1f5093afcbfb652a7d1736897975cb3a
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4128248"
---
# <a name="custom-attached-properties"></a>사용자 지정 연결된 속성

*연결된 속성*은 XAML 개념입니다. 연결된 속성은 일반적으로 특수 형태의 종속성 속성으로 정의됩니다. 이 항목에서는 연결된 속성을 종속성 속성으로 구현하는 방법 및 연결된 속성을 XAML에서 사용 가능하게 하는 데 필요한 접근자 규칙을 정의하는 방법에 대해 설명합니다.

## <a name="prerequisites"></a>필수 조건

기존 종속성 속성의 소비자 관점에서 종속성 속성을 이해하고 [종속성 속성 개요](dependency-properties-overview.md)를 읽은 것으로 가정합니다. [연결된 속성 개요](attached-properties-overview.md)도 읽어야 합니다. 이 항목에 있는 예를 이해하려면 XAML과 C++, C# 또는 Visual Basic을 사용하여 기본 Windows 런타임 앱을 작성하는 방법도 알고 있어야 합니다.

## <a name="scenarios-for-attached-properties"></a>연결된 속성 시나리오

정의 클래스가 아닌 클래스에 사용할 수 있는 속성 설정 메커니즘이 있어야 하는 이유가 있는 경우 연결된 속성을 만들 수 있습니다. 이러한 경우에 대한 가장 일반적인 시나리오는 레이아웃 및 서비스 지원입니다. 기존 레이아웃 속성에 대한 예로는 [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/hh759773) 및 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772)이 있습니다. 레이아웃 시나리오에서는 레이아웃 제어 요소의 자식 요소로 존재하는 요소가 해당 부모 요소에 대한 레이아웃 요구 사항을 개별적으로 표시하고 각각 해당 부모가 연결된 속성으로 정의하는 속성 값을 설정할 수 있습니다. Windows 런타임 API의 서비스 지원 시나리오 예로는 [**ScrollViewer.IsZoomChainingEnabled**](https://msdn.microsoft.com/library/windows/apps/br209561) 같은 [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527)의 연결된 속성 집합이 있습니다.

> [!WARNING]
> Windows 런타임 XAML 구현의 기존 제한 점은 사용자 지정 연결 된 속성을 애니메이션할 수 없다는 됩니다.

## <a name="registering-a-custom-attached-property"></a>사용자 지정 연결된 속성 등록

엄격하게 기타 형식에서만 사용하도록 연결된 속성을 정의하는 경우 속성이 등록된 클래스가 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)에서 파생할 필요는 없습니다. 그러나 연결된 속성이 종속성 속성이기도 한 일반 모델을 따를 경우 백업 속성 저장소를 사용할 수 있도록 접근자의 대상 매개 변수에 **DependencyObject**가 사용되게 해야 합니다.

[**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 형식의 **public** **static** **readonly** 속성을 선언하여 연결된 속성을 종속성 속성으로 정의합니다. 이 속성은 [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833) 메서드의 반환 값을 사용하여 정의합니다. 속성 이름은 **RegisterAttached** *name* 매개 변수로 지정하는 연결된 속성 이름과 일치해야 하며 문자열 "Property"가 끝에 추가됩니다. 표시하는 속성과 관련하여 종속성 속성 식별자를 명명하기 위해 설정된 규칙입니다.

사용자 지정 연결된 속성 정의가 사용자 지정 종속성 속성과 가장 다른 부분은 접근자 또는 래퍼 정의 방식입니다. [사용자 지정 종속성 속성](custom-dependency-properties.md)에 설명 된 래퍼 기술을 사용 하는 대신도 제공 해야 정적 **가져오기 * * * PropertyName* 및 **설정 * * * PropertyName* 메서드도 접근자로 연결 된 속성에 대 한 합니다. 비XAML 시나리오에서는 다른 호출자도 접근자를 사용하여 값을 설정할 수 있으나 접근자는 주로 XAML 파서에서 사용합니다.

> [!IMPORTANT]
> 접근자를 올바르게 정의 하지 않는 경우 XAML 프로세서가 연결 된 속성에 액세스할 수 없습니다 및 사용 하려는 사람은 아 XAML 파서 오류가 발생 합니다. 디자인 및 코딩 도구도 참조된 어셈블리에서 사용자 지정 종속성 속성이 발견되는 경우 "\*Property" 규칙에 따라 식별자를 명명하는 경우가 많습니다.

## <a name="accessors"></a>접근자

**Get**_PropertyName_ 접근자의 서명은 다음이어야 합니다.

`public static` _valueType_ **Get**_PropertyName_ `(DependencyObject target)`

Microsoft Visual Basic의 경우 다음과 같습니다.

`Public Shared Function Get`_PropertyName_`(ByVal target As DependencyObject) As `_valueType_`)`

*target* 개체의 형식은 구현에서 더 구체적일 수 있으며 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)에서 파생해야 합니다. *valueType* 반환 값의 형식도 구현에서 더 구체적일 수 있습니다. 기본 **Object** 형식을 사용할 수 있으나 연결된 속성의 형식 안전성을 강화하려는 경우가 많습니다. 형식 안전성을 강화하는 방법으로 getter 및 setter 시그니처 입력을 사용하는 것이 좋습니다.

서명은 **설정 * * * PropertyName* 접근자는 다음과 같아야 합니다.

`public static void Set`_PropertyName_` (DependencyObject target , `_valueType_` value)`

Visual Basic의 경우 다음과 같습니다.

`Public Shared Sub Set`_PropertyName_` (ByVal target As DependencyObject, ByVal value As `_valueType_`)`

*target* 개체의 형식은 구현에서 더 구체적일 수 있으며 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)에서 파생해야 합니다. *value* 개체 및 해당 *valueType*의 형식도 구현에서 더 구체적일 수 있습니다. 이 메서드의 값은 태그에서 연결된 속성을 발견하는 경우 XAML 프로세서에서 제공하는 입력입니다. 특성 값(최종적으로는 문자열임)으로 적절한 형식을 만들 수 있으려면 사용하는 형식에 대한 형식 변환 또는 기존 태그 확장 지원이 있어야 합니다. 기본 **Object** 형식을 사용할 수 있으나 형식 안전성을 강화하려는 경우가 많습니다. 이 경우 접근자에 형식 적용을 넣으세요.

> [!NOTE]
> 속성 요소 구문을 통해 용도 연결된 된 속성을 정의 하는 것도 가능 합니다. 이 경우 값에 형식 변환은 필요하지 않지만 의도한 값을 XAML에서 생성할 수 있는지 확인해야 합니다. [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/hh738505)는 속성 요소 사용만 지원하는 기존 연결된 속성의 예입니다.

## <a name="code-example"></a>코드 예제

다음 예에서는 종속성 속성 등록([**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833) 메서드 사용)을 보여 주며 사용자 지정 연결된 속성의 경우 **Get** 및 **Set** 접근자도 보여 줍니다. 이 예에서 연결된 속성 이름은 `IsMovable`입니다. 따라서 접근자는 `GetIsMovable` 및 `SetIsMovable`로 명명되어야 합니다. 연결된 속성의 소유자는 고유 UI가 없는 `GameService`라는 서비스 클래스이며, 이 클래스는 **GameService.IsMovable** 연결된 속성이 사용되는 경우 연결된 속성 서비스를 제공하는 것만을 목적으로 합니다.

연결 된 속성 정의 하는 C + + /CX는 약간 더 복잡 합니다. 헤더 및 코드 파일 사이에서 팩터링하는 방법을 결정해야 합니다. 또한 [사용자 지정 종속성 속성](custom-dependency-properties.md)에 설명된 이유로 인해 **get** 접근자만 있는 속성으로 식별자를 노출해야 합니다. C + +이 속성-필드 관계를 정의 해야 CX 명시적으로.NET **readonly** 키워 및 암시적 의존 하는 대신 간단한 속성의 백업 합니다. 또한, 앱이 처음 시작되고 연결된 속성이 필요한 XAML 페이지가 로드되기 전에 도우미 함수 내에서 연결된 속성을 등록해야 합니다. 이 함수는 한 번만 실행됩니다. 일부 및 전체 종속성 속성이나 연결된 속성에 대해 속성 등록 도우미 함수를 호출하는 일반적인 위치는 app.xaml 파일의 코드 안에 있는 **App** / [**Application**](https://msdn.microsoft.com/library/windows/apps/br242325) 생성자입니다.

```csharp
public class GameService : DependencyObject
{
    public static readonly DependencyProperty IsMovableProperty = 
    DependencyProperty.RegisterAttached(
      "IsMovable",
      typeof(Boolean),
      typeof(GameService),
      new PropertyMetadata(false)
    );
    public static void SetIsMovable(UIElement element, Boolean value)
    {
        element.SetValue(IsMovableProperty, value);
    }
    public static Boolean GetIsMovable(UIElement element)
    {
        return (Boolean)element.GetValue(IsMovableProperty);
    }
}
```

```vb
Public Class GameService
    Inherits DependencyObject

    Public Shared ReadOnly IsMovableProperty As DependencyProperty = 
        DependencyProperty.RegisterAttached("IsMovable",  
        GetType(Boolean), 
        GetType(GameService), 
        New PropertyMetadata(False))

    Public Shared Sub SetIsMovable(ByRef element As UIElement, value As Boolean)
        element.SetValue(IsMovableProperty, value)
    End Sub

    Public Shared Function GetIsMovable(ByRef element As UIElement) As Boolean
        GetIsMovable = CBool(element.GetValue(IsMovableProperty))
    End Function
End Class
```

```cppwinrt
// GameService.idl
namespace UserAndCustomControls
{
    runtimeclass GameService : Windows.UI.Xaml.DependencyObject
    {
        GameService();
        static Windows.UI.Xaml.DependencyProperty IsMovableProperty{ get; };
        Boolean IsMovable;
    }
}

// GameService.h
...
    bool IsMovable(){ return winrt::unbox_value<bool>(GetValue(m_IsMovableProperty)); }
    void IsMovable(bool value){ SetValue(m_IsMovableProperty, winrt::box_value(value)); }
    Windows::UI::Xaml::DependencyProperty IsMovableProperty(){ return m_IsMovableProperty; }

private:
    static Windows::UI::Xaml::DependencyProperty m_IsMovableProperty;
...

// GameService.cpp
...
Windows::UI::Xaml::DependencyProperty GameService::m_IsMovableProperty =
    Windows::UI::Xaml::DependencyProperty::RegisterAttached(
        L"IsMovable",
        winrt::xaml_typename<bool>(),
        winrt::xaml_typename<UserAndCustomControls::GameService>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(false) }
);
...
```

```cpp
// GameService.h
#pragma once

#include "pch.h"
//namespace WUX = Windows::UI::Xaml;

namespace UserAndCustomControls {
    public ref class GameService sealed : public WUX::DependencyObject {
    private:
        static WUX::DependencyProperty^ _IsMovableProperty;
    public:
        GameService::GameService();
        void GameService::RegisterDependencyProperties();
        static property WUX::DependencyProperty^ IsMovableProperty
        {
            WUX::DependencyProperty^ get() {
                return _IsMovableProperty;
            }
        };
        static bool GameService::GetIsMovable(WUX::UIElement^ element) {
            return (bool)element->GetValue(_IsMovableProperty);
        };
        static void GameService::SetIsMovable(WUX::UIElement^ element, bool value) {
            element->SetValue(_IsMovableProperty,value);
        }
    };
}

// GameService.cpp
#include "pch.h"
#include "GameService.h"

using namespace UserAndCustomControls;

using namespace Platform;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Data;
using namespace Windows::UI::Xaml::Documents;
using namespace Windows::UI::Xaml::Input;
using namespace Windows::UI::Xaml::Interop;
using namespace Windows::UI::Xaml::Media;

GameService::GameService() {};

GameService::RegisterDependencyProperties() {
    DependencyProperty^ GameService::_IsMovableProperty = DependencyProperty::RegisterAttached(
         "IsMovable", Platform::Boolean::typeid, GameService::typeid, ref new PropertyMetadata(false));
}
```

## <a name="using-your-custom-attached-property-in-xaml"></a>XAML에서 사용자 지정 연결된 속성 사용

연결된 속성을 정의하고 해당 지원 멤버를 사용자 지정 형식의 일부로 포함한 후에는 XAML 사용에서 정의를 사용할 수 있게 해야 합니다. 이렇게 하려면 관련 클래스가 있는 코드 네임스페이스를 참조할 XAML 네임스페이스를 매핑해야 합니다. 연결된 속성을 라이브러리의 일부로 정의한 경우에는 해당 라이브러리를 앱의 앱 패키지 일부로 포함해야 합니다.

XAML에 대한 XML 네임스페이스 매핑은 일반적으로 XAML 페이지의 루트 요소에 지정됩니다. 예를 들어 앞의 코드 조각에 표시된 연결된 속성 정의가 있는 네임스페이스 `UserAndCustomControls`의 `GameService` 클래스의 경우 매핑은 다음과 유사합니다.

```xaml
<UserControl
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
  xmlns:uc="using:UserAndCustomControls"
  ... >
```

이 매핑을 사용하여 Windows 런타임이 정의하는 기존 형식을 비롯한 대상 정의와 일치하는 모든 요소에 대해 `GameService.IsMovable` 연결된 속성을 설정할 수 있습니다.

```xaml
<Image uc:GameService.IsMovable="True" .../>
```

매핑된 동일한 XML 네임스페이스 내에도 있는 요소에 대해 속성을 설정하는 경우에도 연결된 속성 이름에 접두사를 포함해야 합니다. 접두사가 소유자 형식을 규정하기 때문입니다. 일반 XML 규칙에 따라 특성이 요소에서 네임스페이스를 상속할 수 있는 경우에도 연결된 속성의 특성이 동일한 XML 네임스페이스 내에 특성이 포함되어 있는 요소로 있다고 가정할 수 없습니다. 예를 들어 `ImageWithLabelControl`(정의가 표시되지 않음)의 사용자 지정 형식에 `GameService.IsMovable`를 설정하는 경우 둘 다 동일한 접두어로 매핑되는 동일한 코드 네임스페이스에 정의되어 있어도 XAML은 다음과 같습니다.

```xaml
<uc:ImageWithLabelControl uc:GameService.IsMovable="True" .../>
```

> [!NOTE]
> C + +로 XAML UI를 작성 하는 경우 해당 형식을 XAML 페이지에서는 언제 든 지 연결 된 속성을 정의 하는 사용자 지정 형식에 대 한 헤더 포함 해야 합니다. 각 XAML 페이지에는 관련된 .xaml.h 코드 숨김 헤더가 있습니다. 여기에 연결된 속성의 소유자 형식 정의에 대한 헤더를 포함해야 합니다(**\#include** 사용).

## <a name="value-type-of-a-custom-attached-property"></a>사용자 지정 연결된 속성의 값 형식

사용자 지정 연결된 속성의 값 형식으로 사용되는 형식은 사용이나 정의 또는 사용과 정의 둘 다에 영향을 줍니다. 연결된 속성의 값 형식은 **Get** 및 **Set** 접근자 메서드의 시그니처, [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833) 호출의 *propertyType* 매개 변수 등 여러 위치에서 선언됩니다.

연결된 속성(사용자 지정 또는 기타)의 가장 일반적인 값 형식은 단순 문자열입니다. 연결된 속성은 일반적으로 XAML 특성 사용을 위한 것이며 값 형식으로 문자열을 사용하면 속성이 경량으로 유지되기 때문입니다. 정수, double, 열거형 값 등 문자열 메서드로 원시 변환되는 다른 primitive도 연결된 속성의 일반적인 값 형식입니다. 다른 값 형식(원시 문자열 변환을 지원하지 않는 값 형식)을 연결된 속성 값으로 사용할 수 있습니다. 그러나 이 경우 다음과 같이 사용 또는 구현에 대한 선택이 수반됩니다.

- 연결된 속성을 그대로 둘 수 있으나 연결된 속성이 속성 요소인 경우에만 연결된 속성이 사용을 지원할 수 있고 해당 값은 개체 요소로 선언됩니다. 이 경우 속성 형식이 XAML 사용을 개체 요소로 지원해야 합니다. 기존 Windows 런타임 참조 클래스의 경우 XAML 구문에서 해당 형식이 XAML 개체 요소 사용을 지원하는지 확인합니다.
- 연결된 속성을 그대로 둘 수 있으나 문자열로 표시 가능한 **Binding** 또는 **StaticResource** 같은 XAML 참조 기술을 통한 특성 사용에서만 연결된 속성을 사용합니다.

## <a name="more-about-the-canvasleft-example"></a>**Canvas.Left** 예제에 대한 자세한 정보

연결된 속성 사용의 이전 예제에서는 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) 연결된 속성을 설정하는 다양한 방법을 보여 주었습니다. 그러나 연결된 속성에 의해 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)와 개체의 상호 작용 방법은 어떻게 변경되고 언제 변경될까요? 연결된 속성을 구현하면 일반적인 연결된 속성 소유자 클래스가 다른 개체에서 발견할 경우 연결된 속성 값에 대해 다른 어떤 작업을 수행하는지 확인하는 것도 흥미로울 수 있으므로 이 특정 예제를 좀더 살펴보겠습니다.

[**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)의 주요 기능은 UI의 절대 위치 레이아웃 컨테이너입니다. **Canvas**의 자식은 기본 클래스 정의 속성인 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514)에 저장됩니다. 모든 패널 중에서 **Canvas**만 절대 위치를 사용합니다. **UIElement**의 자식 요소인 특정 **UIElement** 경우와 **Canvas**에만 관련이 있을 수 있는 속성을 추가한다면 공용 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 형식의 개체 모델이 너무 커질 것입니다. **Canvas**의 레이아웃 제어 속성을 모든 **UIElement**가 사용할 수 있는 연결된 속성으로 정의하면 개체 모델이 깔끔하게 유지됩니다.

실용적인 패널이 되도록 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)에는 프레임워크 수준의 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 및 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 메서드를 재정의하는 동작이 있습니다. **Canvas**는 실제로 여기서 자식의 연결된 속성 값을 확인합니다. **Measure** 및 **Arrange** 패턴 둘 다에 모든 콘텐츠를 반복하는 루프가 있으며, 패널에는 패널의 자식으로 간주되어야 하는 항목을 명시적으로 지정하는 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 속성이 있습니다. 따라서 **Canvas** 레이아웃 동작은 이러한 자식을 반복하고 각 자식에서 정적 [**Canvas.GetLeft**](https://msdn.microsoft.com/library/windows/apps/br209269) 및 [**Canvas.GetTop**](https://msdn.microsoft.com/library/windows/apps/br209270) 호출을 수행하여 연결된 속성에 기본값이 아닌 값이 있는지 확인합니다(기본값은 0임). 그런 다음 이 값은 각 자식이 제공하고 **Arrange**를 통해 커밋한 특정 값에 따라 사용 가능한 **Canvas** 레이아웃 공간에서 각 자식을 절대 위치에 배치하는 데 사용됩니다.

코드는이 의사 코드 처럼 표시 됩니다.

```syntax
protected override Size ArrangeOverride(Size finalSize)
{
    foreach (UIElement child in Children)
    {
        double x = (double) Canvas.GetLeft(child);
        double y = (double) Canvas.GetTop(child);
        child.Arrange(new Rect(new Point(x, y), child.DesiredSize));
    }
    return base.ArrangeOverride(finalSize); 
    // real Canvas has more sophisticated sizing
}
```

> [!NOTE]
> 패널 작동 방법에 대 한 자세한 내용은 [XAML 사용자 지정 패널 개요](https://msdn.microsoft.com/library/windows/apps/mt228351)를 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833)
* [연결된 속성 개요](attached-properties-overview.md)
* [사용자 지정 종속성 속성](custom-dependency-properties.md)
* [XAML 개요](xaml-overview.md)
