---
description: XAML 연결 된 속성을 종속성 속성으로 구현 하는 방법 및 연결 된 속성을 XAML에서 사용할 수 있도록 하는 데 필요한 접근자 규칙을 정의 하는 방법을 설명 합니다.
title: 사용자 지정 연결된 속성
ms.assetid: E9C0C57E-6098-4875-AA3E-9D7B36E160E0
ms.date: 07/18/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 0512c4c599180144cc16148044e8722597411edd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155127"
---
# <a name="custom-attached-properties"></a>사용자 지정 연결된 속성

*연결 된 속성* 은 XAML 개념입니다. 연결 된 속성은 일반적으로 종속성 속성의 특수 한 형식으로 정의 됩니다. 이 항목에서는 연결 된 속성을 종속성 속성으로 구현 하는 방법과 연결 된 속성을 XAML에서 사용할 수 있도록 하는 데 필요한 접근자 규칙을 정의 하는 방법에 대해 설명 합니다.

## <a name="prerequisites"></a>필수 구성 요소

기존 종속성 속성의 소비자 관점에서 종속성 속성을 이해 하 고 [종속성 속성 개요](dependency-properties-overview.md)를 읽어야 한다고 가정 합니다. 또한 [연결 된 속성 개요](attached-properties-overview.md)를 읽어야 합니다. 이 항목의 예제를 따르려면 XAML을 이해 하 고 c + +, c # 또는 Visual Basic를 사용 하 여 기본 Windows 런타임 앱을 작성 하는 방법을 알고 있어야 합니다.

## <a name="scenarios-for-attached-properties"></a>연결 된 속성에 대 한 시나리오

정의 하는 클래스 이외의 클래스에 사용할 수 있는 속성 설정 메커니즘이 있는 이유가 있는 경우 연결 된 속성을 만들 수 있습니다. 이에 대 한 가장 일반적인 시나리오는 레이아웃 및 서비스 지원입니다. 기존 레이아웃 속성의 예로는 [**canvas. ZIndex**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) 및 [**canvas**](/dotnet/api/system.windows.controls.canvas.top)가 있습니다. 레이아웃 시나리오에서 레이아웃 제어 요소에 자식 요소로 존재 하는 요소는 부모 요소에 대 한 레이아웃 요구 사항을 개별적으로 표현할 수 있습니다. 각 요소는 부모가 연결 된 속성으로 정의 하는 속성 값을 설정 합니다. Windows 런타임 API의 서비스 지원 시나리오에 대 한 예는 [**ScrollViewer IsZoomChainingEnabled**](/uwp/api/windows.ui.xaml.controls.scrollviewer.iszoomchainingenabled)와 같은 연결 된 [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)속성의 집합입니다.

> [!WARNING]
> Windows 런타임 XAML 구현의 기존 제한 사항은 사용자 지정 연결 된 속성에 애니메이션 효과를 적용할 수 없다는 것입니다.

## <a name="registering-a-custom-attached-property"></a>사용자 지정 연결 된 속성 등록

엄격하게 기타 형식에서만 사용하도록 연결된 속성을 정의하는 경우 속성이 등록된 클래스가 [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject)에서 파생할 필요는 없습니다. 하지만 지원 속성 저장소를 사용할 수 있도록 연결 된 속성을 가진 일반적인 모델을 따라야 하는 경우 접근자의 target 매개 변수를 사용 하 여 **DependencyObject** 를 사용 해야 합니다.

[**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty)형식의 **public** **static** **readonly** 속성을 선언 하 여 연결 된 속성을 종속성 속성으로 정의 합니다. [**Registerattached**](/uwp/api/windows.ui.xaml.dependencyproperty.registerattached) 메서드의 반환 값을 사용 하 여이 속성을 정의 합니다. 속성 이름은 ' Property ' 문자열을 끝에 추가 하 여 **registerattached** *name* 매개 변수로 지정 하는 연결 된 속성 이름과 일치 해야 합니다. 이것은 표시 되는 속성과 관련 하 여 종속성 속성의 식별자 이름을 지정 하기 위한 설정 된 규칙입니다.

사용자 지정 연결 된 속성을 정의 하는 기본 영역은 사용자 지정 종속성 속성과 다릅니다. [사용자 지정 종속성 속성](custom-dependency-properties.md)에 설명 된 래퍼 기법을 사용 하는 대신 정적 **Get**_propertyname_ 을 제공 하 고 연결 된 속성에 대 한 접근자로_propertyname_ 메서드를 **설정**해야 합니다. 접근자는 xaml 파서에서 주로 사용 되지만, 다른 호출자도이를 사용 하 여 비 XAML 시나리오에서 값을 설정할 수 있습니다.

> [!IMPORTANT]
> 접근자를 올바르게 정의 하지 않으면 XAML 프로세서에서 연결 된 속성에 액세스할 수 없으며,이를 사용 하려고 시도 하는 모든 사용자가 XAML 파서 오류가 발생할 수 있습니다. 또한 디자인 및 코딩 도구는 \* 참조 된 어셈블리에서 사용자 지정 종속성 속성이 나타날 때 명명 식별자에 대 한 "속성" 규칙을 사용 하는 경우가 많습니다.

## <a name="accessors"></a>접근자

**Get**_PropertyName_ 접근자에 대 한 시그니처는 this 여야 합니다.

`public static`_valueType_ **가져오기**_PropertyName_`(DependencyObject target)`

Microsoft Visual Basic의 경우이는입니다.

`Public Shared Function Get`_PropertyName_ `(ByVal target As DependencyObject) As ` _valueType_`)`

*대상* 개체는 구현에서 보다 구체적인 형식일 수 있지만 [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject)에서 파생 되어야 합니다. *ValueType* 반환 값은 구현에서 보다 구체적인 형식일 수도 있습니다. 기본 **개체** 형식은 허용 되지만 연결 된 속성이 형식 안전성을 적용 하려는 경우가 많습니다. Getter 및 setter 서명에 입력 하는 것은 권장 되는 형식 안전 기술입니다.

**Set**_PropertyName_ 접근자의 시그니처는 this 여야 합니다.

`public static void Set`_PropertyName_ ` (DependencyObject target , ` _valueType_` value)`

Visual Basic의 경우이는입니다.

`Public Shared Sub Set`_PropertyName_ ` (ByVal target As DependencyObject, ByVal value As ` _valueType_`)`

*대상* 개체는 구현에서 보다 구체적인 형식일 수 있지만 [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject)에서 파생 되어야 합니다. *값* 개체와 해당 *valueType* 은 구현에서 보다 구체적인 형식일 수 있습니다. 이 메서드의 값은 태그에서 연결 된 속성을 발견할 때 XAML 프로세서에서 제공 되는 입력입니다. 특성 값 (궁극적으로는 문자열)에서 적절 한 형식을 만들 수 있도록 사용 하는 형식에 대 한 형식 변환 또는 기존 태그 확장 지원이 있어야 합니다. 기본 **개체** 형식은 적합 하지만 형식 안전성이 더 필요한 경우가 많습니다. 이렇게 하려면 접근자에 형식을 적용 합니다.

> [!NOTE]
> 또한 속성 요소 구문을 통해 의도 된 용도를 사용 하는 연결 된 속성을 정의할 수 있습니다. 이 경우 값에 대해 형식 변환이 필요 하지 않지만 원하는 값을 XAML로 생성할 수 있는지를 보장 해야 합니다. [**R**](/dotnet/api/system.windows.visualstatemanager) 는 속성 요소 사용만 지 원하는 기존 연결 된 속성의 예입니다.

## <a name="code-example"></a>코드 예제

이 예제에서는 사용자 지정 연결 된 속성에 대 한 **Get** 및 **Set** 접근자 뿐만 아니라 종속성 속성 등록 ( [**registerattached**](/uwp/api/windows.ui.xaml.dependencyproperty.registerattached) 메서드 사용)을 보여 줍니다. 예제에서 연결된 속성 이름은 `IsMovable`입니다. 따라서 접근자의 이름은 `GetIsMovable` 및 `SetIsMovable`입니다. 연결 된 속성의 소유자는 자체 UI가 없는 라는 서비스 클래스입니다. `GameService` **GameService** 연결 된 속성이 사용 되는 경우에만 연결 된 속성 서비스를 제공 합니다.

C + +/CX에서 연결 된 속성을 정의 하는 것은 약간 더 복잡 합니다. 헤더와 코드 파일의 요소를 구분 하는 방법을 결정 해야 합니다. 또한 [사용자 지정 종속성 속성](custom-dependency-properties.md)에 설명 된 이유 때문에 **get** 접근자만 있는 속성으로 식별자를 노출 해야 합니다. C + +/CX에서는 .NET **읽기 전용** 키에 의존 하 고 단순 속성의 암시적 백업에 의존 하지 않고이 속성 필드 관계를 명시적으로 정의 해야 합니다. 또한 앱이 처음 시작 될 때, 연결 된 속성이 필요한 XAML 페이지가 로드 되기 전에 한 번만 실행 되는 도우미 함수 내에서 연결 된 속성의 등록을 수행 해야 합니다. 모든 종속성 또는 연결 된 속성에 대 한 속성 등록 도우미 함수를 호출 하는 일반적인 장소는 app.xaml **App**  /  파일에 대 한 코드의 앱[**응용 프로그램**](/uwp/api/windows.ui.xaml.application.-ctor) 생성자 내에서 발생 합니다.

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
    [default_interface]
    runtimeclass GameService : Windows.UI.Xaml.DependencyObject
    {
        GameService();
        static Windows.UI.Xaml.DependencyProperty IsMovableProperty{ get; };
        static Boolean GetIsMovable(Windows.UI.Xaml.DependencyObject target);
        static void SetIsMovable(Windows.UI.Xaml.DependencyObject target, Boolean value);
    }
}

// GameService.h
...
    static Windows::UI::Xaml::DependencyProperty IsMovableProperty() { return m_IsMovableProperty; }
    static bool GetIsMovable(Windows::UI::Xaml::DependencyObject const& target) { return winrt::unbox_value<bool>(target.GetValue(m_IsMovableProperty)); }
    static void SetIsMovable(Windows::UI::Xaml::DependencyObject const& target, bool value) { target.SetValue(m_IsMovableProperty, winrt::box_value(value)); }

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

## <a name="setting-your-custom-attached-property-from-xaml-markup"></a>XAML 태그에서 사용자 지정 연결 된 속성 설정

> [!NOTE]
> C + +/WinRT를 사용 하는 경우 다음 섹션으로 건너뜁니다 ([c + +/WinRT를 사용 하 여 사용자 지정 연결 된 속성을 명령적으로 설정](#setting-your-custom-attached-property-imperatively-with-cwinrt)).

연결 된 속성을 정의 하 고 해당 지원 멤버를 사용자 지정 형식의 일부로 포함 한 후에는 XAML 사용에 대 한 정의를 사용할 수 있도록 해야 합니다. 이렇게 하려면 관련 클래스를 포함 하는 코드 네임 스페이스를 참조 하는 XAML 네임 스페이스를 매핑해야 합니다. 라이브러리의 일부로 연결 된 속성을 정의한 경우 해당 라이브러리를 앱에 대 한 앱 패키지의 일부로 포함 해야 합니다.

XAML에 대 한 XML 네임 스페이스 매핑은 일반적으로 XAML 페이지의 루트 요소에 배치 됩니다. 예를 `GameService` `UserAndCustomControls` 들어, 이전 코드 조각에 표시 된 연결 된 속성 정의를 포함 하는 네임 스페이스에 있는 클래스의 경우 매핑은 다음과 같을 수 있습니다.

```xaml
<UserControl
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
  xmlns:uc="using:UserAndCustomControls"
  ... >
```

매핑을 사용 하면 `GameService.IsMovable` Windows 런타임 정의 하는 기존 형식을 포함 하 여 대상 정의와 일치 하는 모든 요소에서 연결 된 속성을 설정할 수 있습니다.

```xaml
<Image uc:GameService.IsMovable="True" .../>
```

동일한 매핑된 XML 네임 스페이스 내에 있는 요소에 대 한 속성을 설정 하는 경우에도 연결 된 속성 이름에 접두사를 포함 해야 합니다. 접두사가 소유자 유형을 한정 하기 때문입니다. 연결 된 속성의 특성은 특성이 포함 된 요소와 동일한 XML 네임 스페이스 내에 있는 것으로 간주할 수 없습니다. 그러나 일반적인 XML 규칙에 따라 특성은 요소에서 네임 스페이스를 상속할 수 있습니다. 예를 들어 `GameService.IsMovable` 사용자 지정 형식 `ImageWithLabelControl` (정의가 표시 되지 않음)에서를 설정 하 고 둘 다 동일한 접두사에 매핑된 동일한 코드 네임 스페이스에 정의 된 경우에도 XAML은 여전히이로 설정 됩니다.

```xaml
<uc:ImageWithLabelControl uc:GameService.IsMovable="True" .../>
```

> [!NOTE]
> C + +/CX를 사용 하 여 XAML UI를 작성 하는 경우 XAML 페이지에서 해당 형식을 사용 하는 경우 연결 된 속성을 정의 하는 사용자 지정 형식에 대 한 헤더를 포함 해야 합니다. 각 XAML 페이지에는 연결 된 코드 숨김이 헤더 (.xaml)가 있습니다. 여기서는 연결 된 속성의 소유자 형식에 대 한 정의에 헤더를 포함 ( ** \# include**사용) 해야 합니다.

## <a name="setting-your-custom-attached-property-imperatively-with-cwinrt"></a>C + +/WinRT를 사용 하 여 명령적으로 사용자 지정 연결 된 속성 설정

C + +/WinRT를 사용 하는 경우 명령적 코드에서 사용자 지정 연결 된 속성에 액세스할 수 있지만 XAML 태그에서는 액세스할 수 없습니다. 아래 코드에서는 방법을 보여 줍니다.

```xaml
<Image x:Name="gameServiceImage"/>
```

```cppwinrt
// MainPage.h
...
#include "GameService.h"
...

// MainPage.cpp
...
MainPage::MainPage()
{
    InitializeComponent();

    GameService::SetIsMovable(gameServiceImage(), true);
}
...
```

## <a name="value-type-of-a-custom-attached-property"></a>사용자 지정 연결 된 속성의 값 형식

사용자 지정 연결 된 속성의 값 형식으로 사용 되는 형식은 사용, 정의 또는 사용 및 정의 모두에 영향을 줍니다. 연결 된 속성의 값 형식은 여러 위치에서 선언 됩니다. **Get** 및 **Set** 접근자 메서드의 시그니처와 [**registerattached**](/uwp/api/windows.ui.xaml.dependencyproperty.registerattached) 호출의 *propertyType* 매개 변수를 함께 선언 합니다.

연결 된 속성에 대 한 가장 일반적인 값 유형 (사용자 지정 또는 기타)은 간단한 문자열입니다. 이는 연결 된 속성이 일반적으로 XAML 특성 사용을 위한 것 이며 문자열을 값 형식으로 사용 하면 속성을 간단 하 게 유지할 수 있기 때문입니다. 정수, double 또는 열거형 값과 같은 문자열 메서드로 네이티브 변환이 가능한 다른 기본 형식은 일반적으로 연결 된 속성에 대 한 값 형식입니다. 네이티브 문자열 변환을 지원 하지 않는 다른 값 형식을 연결 된 속성 값으로 사용할 수 있습니다. 그러나이 경우에는 사용 또는 구현에 대해 선택할 수도 있습니다.

- 연결 된 속성을 그대로 둘 수 있지만 연결 된 속성은 연결 된 속성이 속성 요소인 경우에만 사용을 지원할 수 있으며, 값은 개체 요소로 선언 됩니다. 이 경우 속성 형식은 XAML 사용을 개체 요소로 지원 해야 합니다. 기존 Windows 런타임 참조 클래스의 경우 형식이 XAML 개체 요소 사용을 지원 하는지 확인 하려면 XAML 구문을 확인 합니다.
- 연결 된 속성을 그대로 둘 수 있지만이 속성은 문자열로 표현할 수 있는 **바인딩** 또는 **StaticResource** 같은 XAML 참조 기술을 통해 특성 사용에만 사용할 수 있습니다.

## <a name="more-about-the-canvasleft-example"></a>Canvas에 대 한 자세한 정보 **. 왼쪽** 예제

이전에 연결 된 속성 사용 예에서는 캔버스를 설정 하는 여러 가지 방법을 살펴보았습니다 [**. 왼쪽**](/dotnet/api/system.windows.controls.canvas.left) 에 연결 된 속성을 설정 했습니다. 그러나 [**캔버스**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) 에서 개체와 상호 작용 하는 방법 및 시기에 대 한 변경 사항은 무엇 인가요? 연결 된 속성을 구현 하는 경우 다른 개체에서 연결 된 속성 값을 찾으면 일반적인 연결 된 속성 소유자 클래스에서 수행 해야 하는 작업을 확인 하는 것이 특정 예제를 추가로 살펴볼 것입니다.

[**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) 의 main 함수는 UI에서 절대 위치에 배치 된 레이아웃 컨테이너입니다. **캔버스** 의 자식은 기본 클래스의 정의 된 속성 [**자식**](/uwp/api/windows.ui.xaml.controls.panel.children)에 저장 됩니다. 모든 패널 캔버스는 절대 위치 지정을 사용 하는 유일한 **캔버스** 입니다. **Canvas** 에만 중요할 수 있는 속성을 추가 하 고, **uielement**의 자식 **요소인 경우에** 는 일반적인 [**uielement**](/uwp/api/Windows.UI.Xaml.UIElement) 형식의 개체 모델을 크게 만듭니다. 모든 **UIElement** 에서 사용할 수 있는 연결 된 속성이 될 **캔버스** 의 레이아웃 컨트롤 속성을 정의 하 여 개체 모델을 더 명확 하 게 유지 합니다.

실제 패널을 위해 [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) 는 프레임 워크 수준 [**측정값과**](/uwp/api/windows.ui.xaml.uielement.measure) [**정렬**](/uwp/api/windows.ui.xaml.uielement.arrange) 메서드를 재정의 하는 동작을 포함 합니다. 여기서 **Canvas** 는 자식에 대 한 연결 된 속성 값을 실제로 확인 합니다. **측정값** 및 **정렬** 패턴의 일부는 모든 콘텐츠를 반복 하는 루프 이며 패널에는 패널의 자식 항목으로 간주 되는 항목을 명시적으로 만드는 [**자식**](/uwp/api/windows.ui.xaml.controls.panel.children) 속성이 있습니다. 따라서 **Canvas** 레이아웃 동작은 이러한 자식을 반복 하 고 각 자식에 대해 정적 [**GetLeft**](/uwp/api/windows.ui.xaml.controls.canvas.getleft) 및 [**GetTop**](/uwp/api/windows.ui.xaml.controls.canvas.gettop) 호출을 수행 하 여 연결 된 속성에 기본값이 아닌 값이 포함 되어 있는지 확인 합니다 (기본값은 0). 그런 다음 이러한 값을 사용 하 여 각 자식에서 제공 하는 특정 값에 따라 **캔버스** 사용 가능 레이아웃 공간에 각 자식을 절대 배치 하 고 **정렬을**사용 하 여 커밋됨.

코드는 이러한 의사 코드와 같습니다.

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
> 패널 작동 방식에 대 한 자세한 내용은 [XAML 사용자 지정 패널 개요](../design/layout/custom-panels-overview.md)를 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [**System.windows.dependencyproperty.registerattached**](/uwp/api/windows.ui.xaml.dependencyproperty.registerattached)
* [연결된 속성 개요](attached-properties-overview.md)
* [사용자 지정 종속성 속성](custom-dependency-properties.md)
* [XAML 개요](xaml-overview.md)