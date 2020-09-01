---
description: 'C + +, c # 또는 Visual Basic를 사용 하 여 Windows 런타임 앱의 사용자 지정 종속성 속성을 정의 하 고 구현 하는 방법을 설명 합니다.'
title: 사용자 지정 종속성 속성
ms.assetid: 5ADF7935-F2CF-4BB6-B1A5-F535C2ED8EF8
ms.date: 07/12/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: e7a14383f127f12b5ba13e67e1690c0d7a936c82
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174217"
---
# <a name="custom-dependency-properties"></a>사용자 지정 종속성 속성

여기서는 c + +, c # 또는 Visual Basic를 사용 하 여 Windows 런타임 앱에 대 한 고유한 종속성 속성을 정의 하 고 구현 하는 방법을 설명 합니다. 앱 개발자와 구성 요소 작성자가 사용자 지정 종속성 속성을 만들려는 이유가 나와 있습니다. 사용자 지정 종속성 속성에 대 한 구현 단계 뿐만 아니라 종속성 속성의 성능을 향상 시킬 수 있는 몇 가지 모범 사례를 설명 합니다.

## <a name="prerequisites"></a>필수 구성 요소

[종속성 속성 개요](dependency-properties-overview.md) 를 읽고 기존 종속성 속성의 소비자 관점에서 종속성 속성을 이해 하 고 있다고 가정 합니다. 이 항목의 예제를 따르려면 XAML을 이해 하 고 c + +, c # 또는 Visual Basic를 사용 하 여 기본 Windows 런타임 앱을 작성 하는 방법을 알고 있어야 합니다.

## <a name="what-is-a-dependency-property"></a>종속성 속성 이란?

속성에 대 한 스타일 지정, 데이터 바인딩, 애니메이션 및 기본값을 지원 하려면 종속성 속성으로 구현 해야 합니다. 종속성 속성 값은 클래스에 필드로 저장 되지 않고, xaml 프레임 워크에 의해 저장 되며, 속성을 속성 시스템에 Windows 런타임 등록 하는 경우에는 [**DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty.register) 메서드를 호출 하 여 검색 되는 키를 사용 하 여 참조 됩니다.   종속성 속성은 [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject)에서 파생 된 형식 에서만 사용할 수 있습니다. 그러나 **DependencyObject** 는 클래스 계층 구조에서 매우 높습니다. 따라서 UI 및 프레젠테이션 지원을 위한 대부분의 클래스는 종속성 속성을 지원할 수 있습니다. 종속성 속성에 대 한 자세한 내용과이 설명서에서 설명 하는 데 사용 되는 용어 및 규칙에 대 한 자세한 내용은 [종속성 속성 개요](dependency-properties-overview.md)를 참조 하세요.

Windows 런타임에서 종속성 속성의 예로는 [**컨트롤. Background**](/uwp/api/windows.ui.xaml.controls.control.background), [**FrameworkElement. Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)및 [**TextBox**](/uwp/api/windows.ui.xaml.controls.textbox.text)등이 있습니다.

규칙은 클래스에 의해 노출 되는 각 종속성 속성에 동일한 클래스에 노출 되는 [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) 형식의 해당 **public static readonly** 속성이 있으므로에서 종속성 속성에 대 한 식별자를 제공 한다는 것입니다. 식별자 이름은 다음 규칙을 따릅니다. 종속성 속성의 이름은 이름 끝에 "Property" 문자열이 추가 됩니다. 예를 들어, BackgroundProperty 속성에 해당 하 **는** **DependencyProperty** 식별자는 [**control.**](/uwp/api/windows.ui.xaml.controls.control.backgroundproperty). 식별자는 등록 된 종속성 속성에 대 한 정보를 저장 하 고, [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue)호출과 같이 종속성 속성을 포함 하는 다른 작업에 사용할 수 있습니다.

## <a name="property-wrappers"></a>속성 래퍼

종속성 속성에는 일반적으로 래퍼 구현이 있습니다. 래퍼가 없으면 속성을 가져오거나 설정 하는 유일한 방법은 종속성 속성 유틸리티 메서드인 [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) 와 [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) 를 사용 하 여 식별자를 매개 변수로 전달 하는 것입니다. 이는 속성에 서비스가 표면적으로 된 항목에 대 한 보다 자연스럽 된 사용입니다. 그러나 래퍼를 사용 하 여 코드와 종속성 속성을 참조 하는 다른 모든 코드는 사용 중인 언어에 대 한 자연 스러운 간단한 개체 속성 구문을 사용할 수 있습니다.

사용자 지정 종속성 속성을 직접 구현 하 고 해당 속성을 public이 고 호출 하기 쉽도록 하려면 속성 래퍼를 정의 합니다. 속성 래퍼는 리플렉션 또는 정적 분석 프로세스에 대 한 종속성 속성에 대 한 기본 정보를 보고 하는 데에도 유용 합니다. 특히, 래퍼는 [**ContentPropertyAttribute**](/uwp/api/Windows.UI.Xaml.Markup.ContentPropertyAttribute)와 같은 특성을 저장 하는 위치입니다.

## <a name="when-to-implement-a-property-as-a-dependency-property"></a>속성을 종속성 속성으로 구현하는 시기

클래스가 [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject)에서 파생 되는 한 클래스에서 공용 읽기/쓰기 속성을 구현할 때마다 속성을 종속성 속성으로 사용할 수 있는 옵션이 있습니다. 경우에 따라 전용 필드를 사용 하 여 속성을 백업 하는 일반적인 방법을 사용 하는 것이 적절 합니다. 사용자 지정 속성을 종속성 속성으로 정의 하는 것은 항상 필요 하거나 적절 하지 않습니다. 선택은 속성에서 지원 하려는 시나리오에 따라 달라 집니다.

속성을 종속성 속성으로 구현 하는 것이 좋습니다 .이 속성을 종속성 속성으로 구현 하 여 Windows 런타임 또는 Windows 런타임 앱의 이러한 기능 중 하나 이상을 지원할 수 있습니다.

- 스타일을 통해 속성 설정 [ **Style**](/uwp/api/Windows.UI.Xaml.Style)
- [ **{Binding}** 을 (를) 사용 하 여 데이터 바인딩의 유효한 대상 속성으로 작동](binding-markup-extension.md)
- Storyboard를 통해 애니메이션 된 값 지원 [ **Storyboard**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)
- 속성의 값이 다음에 의해 변경 된 경우 보고:
  - 속성 시스템 자체에서 수행 하는 작업
  - 환경
  - 사용자 작업
  - 스타일 읽기 및 쓰기

## <a name="checklist-for-defining-a-dependency-property"></a>종속성 속성을 정의 하기 위한 검사 목록

종속성 속성을 정의 하는 것은 개념 집합으로 간주할 수 있습니다. 이러한 개념은 몇 가지 개념을 구현에서 코드 한 줄로 해결할 수 있기 때문에 반드시 절차적 단계는 아닙니다. 이 목록에서는 간략 한 개요만 제공 합니다. 각 개념에 대 한 자세한 내용은이 항목의 뒷부분에서 설명 하며, 예제 코드는 여러 언어로 표시 됩니다.

- 속성 이름에 속성 시스템 (호출 [**레지스터**](/uwp/api/windows.ui.xaml.dependencyproperty.register))을 등록 하 여 소유자 유형 및 속성 값의 유형을 지정 합니다.
  - 속성 메타 데이터를 필요로 하는 [**레지스터**](/uwp/api/windows.ui.xaml.dependencyproperty.register) 에 대 한 필수 매개 변수가 있습니다. 이에 대해 **null** 을 지정 하거나, 속성 변경 동작 또는 [**clearvalue**](/uwp/api/windows.ui.xaml.dependencyobject.clearvalue)를 호출 하 여 복원할 수 있는 메타 데이터 기반 기본값을 지정 하려면 [**PropertyMetadata**](/uwp/api/windows.ui.xaml.propertymetadata)의 인스턴스를 지정 합니다.
- Owner 형식의 **public static readonly** 속성 멤버로 [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) 식별자를 정의 합니다.
- 구현 하는 언어에서 사용 되는 속성 접근자 모델을 따라 래퍼 속성을 정의 합니다. 래퍼 속성 이름은 [**Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register)에서 사용한 *이름* 문자열과 일치 해야 합니다. [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) 와 [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) 를 호출 하 고 고유한 속성의 식별자를 매개 변수로 전달 하 여 래퍼를 래핑하는 종속성 속성에 연결 하려면 **get** 및 **set** 접근자를 구현 합니다.
- 필드 래퍼에 [**ContentPropertyAttribute**](/uwp/api/Windows.UI.Xaml.Markup.ContentPropertyAttribute) 와 같은 특성을 추가 합니다.

> [!NOTE]
> 사용자 지정 연결 된 속성을 정의 하는 경우 일반적으로 래퍼를 생략 합니다. 대신 XAML 프로세서에서 사용할 수 있는 다른 스타일의 접근자를 작성 합니다. [사용자 지정 연결 된 속성](custom-attached-properties.md)을 참조 하세요. 

## <a name="registering-the-property"></a>속성 등록

속성이 종속성 속성인 경우 속성을 Windows 런타임 속성 시스템에 의해 유지 관리 되는 속성 저장소에 등록 해야 합니다.  속성을 등록 하려면 [**register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) 메서드를 호출 합니다.

Microsoft .NET 언어 (c # 및 Microsoft Visual Basic)의 경우 클래스 본문 내에서 [**Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) 를 호출 합니다 (클래스 내에서는 멤버 정의 외부). 이 식별자는 [**Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) 메서드 호출에서 반환 값으로 제공 됩니다. [**레지스터**](/uwp/api/windows.ui.xaml.dependencyproperty.register) 호출은 일반적으로 정적 생성자 또는 클래스의 일부로 [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) 형식의 **public static readonly** 속성을 초기화 하는 과정의 일부로 생성 됩니다. 이 속성은 종속성 속성의 식별자를 노출 합니다. 다음은 [**레지스터**](/uwp/api/windows.ui.xaml.dependencyproperty.register) 호출의 예입니다.

> [!NOTE]
> 종속성 속성을 식별자 속성 정의의 일부로 등록 하는 것은 일반적인 구현 이지만 클래스 정적 생성자에서 종속성 속성을 등록할 수도 있습니다. 이 접근 방식은 종속성 속성을 초기화 하는 코드 줄이 두 개 이상 필요한 경우에 적합할 수 있습니다.

C + +/CX의 경우 헤더와 코드 파일 간에 구현을 분할 하는 방법에 대 한 옵션이 있습니다. 일반적인 분할은 **get** 구현이 있지만 **set**은 없는 헤더에서 식별자 자체를 **공용 정적** 속성으로 선언 하는 것입니다. **Get** 구현은 초기화 되지 않은 [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) 인스턴스인 전용 필드를 참조 합니다. 래퍼를 선언 하 고 래퍼를 **get** 및 **set로** 구현할 수도 있습니다. 이 경우 헤더에는 최소한의 구현이 포함 됩니다. 래퍼에 특성 Windows 런타임 필요한 경우에는 헤더의 특성도 필요 합니다. 앱이 처음 초기화 될 때만 실행 되는 도우미 함수 내에서 [**등록**](/uwp/api/windows.ui.xaml.dependencyproperty.register) 호출을 코드 파일에 배치 합니다. **레지스터** 의 반환 값을 사용 하 여 처음에 구현 파일의 루트 범위에서 **nullptr** 로 설정 하는 헤더에 선언 된 초기화 되지 않은 정적 식별자를 채웁니다.

```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null)
);
```

```vb
Public Shared ReadOnly LabelProperty As DependencyProperty = 
    DependencyProperty.Register("Label", 
      GetType(String), 
      GetType(ImageWithLabelControl), 
      New PropertyMetadata(Nothing))
```

```cppwinrt
// ImageWithLabelControl.idl
namespace ImageWithLabelControlApp
{
    runtimeclass ImageWithLabelControl : Windows.UI.Xaml.Controls.Control
    {
        ImageWithLabelControl();
        static Windows.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}

// ImageWithLabelControl.h
...
private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
...

// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ nullptr }
);
...
```

```cpp
//.h file
//using namespace Windows::UI::Xaml::Controls;
//using namespace Windows::UI::Xaml::Interop;
//using namespace Windows::UI::Xaml;
//using namespace Platform;

public ref class ImageWithLabelControl sealed : public Control
{
private:
    static DependencyProperty^ _LabelProperty;
...
public:
    static void RegisterDependencyProperties();
    static property DependencyProperty^ LabelProperty
    {
        DependencyProperty^ get() {return _LabelProperty;}
    }
...
};

//.cpp file
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml.Interop;

DependencyProperty^ ImageWithLabelControl::_LabelProperty = nullptr;

// This function is called from the App constructor in App.xaml.cpp
// to register the properties
void ImageWithLabelControl::RegisterDependencyProperties()
{ 
    if (_LabelProperty == nullptr)
    { 
        _LabelProperty = DependencyProperty::Register(
          "Label", Platform::String::typeid, ImageWithLabelControl::typeid, nullptr);
    } 
}
```

> [!NOTE]
> C + +/CX 코드의 경우 전용 필드와 [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) 를 표시 하는 공용 읽기 전용 속성이 있는 이유는 종속성 속성을 사용 하는 다른 호출자가 식별자를 public으로 요구 하는 속성 시스템 유틸리티 api를 사용할 수 있도록 하는 것입니다. 식별자를 개인으로 유지 하는 경우 사용자는 이러한 유틸리티 Api를 사용할 수 없습니다. 이러한 API 및 시나리오의 예로는 [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) 또는 [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) By choice, [**clearvalue**](/uwp/api/windows.ui.xaml.dependencyobject.clearvalue), [**Getanimationbasevalue**](/uwp/api/windows.ui.xaml.dependencyobject.getanimationbasevalue), [**setbinding**](/uwp/api/windows.ui.xaml.frameworkelement.setbinding)및 [**Setter 속성이**](/uwp/api/windows.ui.xaml.setter.property)있습니다. Windows 런타임 메타 데이터 규칙이 public 필드를 허용 하지 않기 때문에이에 대 한 public 필드를 사용할 수 없습니다.

## <a name="dependency-property-name-conventions"></a>종속성 속성 이름 규칙

종속성 속성에 대 한 명명 규칙이 있습니다. 예외적인 상황을 제외 하 고 모두 팔 로우 합니다. 종속성 속성 자체에는 [**Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register)의 첫 번째 매개 변수로 제공 되는 기본 이름 (앞의 예제에서는 "레이블")이 있습니다. 이름은 각 등록 형식 내에서 고유 해야 하며, 상속 된 멤버에도 고유성 요구 사항이 적용 됩니다. 기본 형식을 통해 상속 된 종속성 속성은 이미 등록 된 형식의 일부로 간주 됩니다. 상속 된 속성의 이름을 다시 등록할 수 없습니다.

> [!WARNING]
> 여기서 제공 하는 이름은 원하는 언어의 프로그래밍에서 유효한 문자열 식별자 일 수 있지만 일반적으로 종속성 속성을 XAML로도 설정할 수 있습니다. XAML로 설정 하려면 선택한 속성 이름이 올바른 XAML 이름 이어야 합니다. 자세한 내용은 [XAML 개요](xaml-overview.md)를 참조 하세요.

식별자 속성을 만들 때 속성의 이름을 접미사 "Property" (예: "LabelProperty")와 함께 등록 한 것과 결합 합니다. 이 속성은 종속성 속성에 대 한 식별자 이며 고유한 속성 래퍼에서 수행 하는 [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) 및 [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) 호출에 대 한 입력으로 사용 됩니다. 속성 시스템 및 [ **{X:bind}** 와 같은 다른 XAML 프로세서에도 사용 됩니다.](x-bind-markup-extension.md)

## <a name="implementing-the-wrapper"></a>래퍼 구현

속성 래퍼는 **get** 구현에서 [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) 를 호출 하 고 **집합** 구현에서 [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) 를 호출 해야 합니다.

> [!WARNING]
> 예외적인 경우를 제외 하 고 래퍼 구현은 [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) 및 [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) 작업을 수행 해야 합니다. 그렇지 않으면 코드가 코드를 통해 설정 된 경우와 XAML을 통해 속성을 설정할 때 다른 동작이 발생 합니다. 효율성을 위해 XAML 파서는 종속성 속성을 설정할 때 래퍼를 무시 합니다. 및는 **SetValue**를 통해 백업 저장소와 통신 합니다.

```csharp
public String Label
{
    get { return (String)GetValue(LabelProperty); }
    set { SetValue(LabelProperty, value); }
}
```

```vb
Public Property Label() As String
    Get
        Return DirectCast(GetValue(LabelProperty), String) 
    End Get 
    Set(ByVal value As String)
        SetValue(LabelProperty, value)
    End Set
End Property
```

```cppwinrt
// ImageWithLabelControl.h
...
winrt::hstring Label()
{
    return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
}

void Label(winrt::hstring const& value)
{
    SetValue(m_labelProperty, winrt::box_value(value));
}
...
```

```cpp
//using namespace Platform;
public:
...
  property String^ Label
  {
    String^ get() {
      return (String^)GetValue(LabelProperty);
    }
    void set(String^ value) {
      SetValue(LabelProperty, value);
    }
  }
```

## <a name="property-metadata-for-a-custom-dependency-property"></a>사용자 지정 종속성 속성의 속성 메타 데이터

속성 메타 데이터가 종속성 속성에 할당 되 면 속성 소유자 형식 또는 해당 하위 클래스의 모든 인스턴스에 대해 동일한 메타 데이터가 해당 속성에 적용 됩니다. 속성 메타 데이터에서 다음 두 가지 동작을 지정할 수 있습니다.

- 속성 시스템에서 속성의 모든 사례에 할당 하는 기본값입니다.
- 속성 값 변경이 검색 될 때마다 속성 시스템에서 자동으로 호출 되는 정적 콜백 메서드입니다.

### <a name="calling-register-with-property-metadata"></a>속성 메타 데이터를 사용 하 여 등록 호출

[**DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty.register)를 호출 하는 이전 예제에서 *propertyMetadata* 매개 변수에 대해 null 값을 전달 했습니다. 종속성 속성이 기본값을 제공 하거나 속성 변경 콜백을 사용 하도록 하려면 이러한 기능 중 하나 또는 둘 모두를 제공 하는 [**PropertyMetadata**](/uwp/api/Windows.UI.Xaml.PropertyMetadata) 인스턴스를 정의 해야 합니다.

일반적으로 [**PropertyMetadata**](/uwp/api/Windows.UI.Xaml.PropertyMetadata) [**에 대 한**](/uwp/api/windows.ui.xaml.dependencyproperty.register)매개 변수 내에서 인라인 생성 인스턴스로 제공 됩니다.

> [!NOTE]
> [**CreateDefaultValueCallback**](/uwp/api/windows.ui.xaml.createdefaultvaluecallback) 구현을 정의 하는 경우 **PropertyMetadata** 인스턴스를 정의 하기 위해 [**PropertyMetadata**](/uwp/api/Windows.UI.Xaml.PropertyMetadata) 생성자를 호출 하는 대신 유틸리티 메서드 [**PropertyMetadata**](/uwp/api/windows.ui.xaml.propertymetadata.create) 를 사용 해야 합니다.

다음 예에서는 이전에 표시 된 DependencyProperty를 수정 합니다. [**Propertychangedcallback**](/uwp/api/windows.ui.xaml.propertychangedcallback) 값을 사용 하 여 [**PropertyMetadata**](/uwp/api/Windows.UI.Xaml.PropertyMetadata) 인스턴스를 참조 하 여 예제를 [**등록 합니다.**](/uwp/api/windows.ui.xaml.dependencyproperty.register) "OnLabelChanged" 콜백의 구현은이 단원의 뒷부분에 나와 있습니다.

```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null,new PropertyChangedCallback(OnLabelChanged))
);
```

```vb
Public Shared ReadOnly LabelProperty As DependencyProperty =
    DependencyProperty.Register("Label",
      GetType(String),
      GetType(ImageWithLabelControl),
      New PropertyMetadata(
        Nothing, new PropertyChangedCallback(AddressOf OnLabelChanged)))
```

```cppwinrt
// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ nullptr, Windows::UI::Xaml::PropertyChangedCallback{ &ImageWithLabelControl::OnLabelChanged } }
);
...
```

```cpp
DependencyProperty^ ImageWithLabelControl::_LabelProperty =
    DependencyProperty::Register("Label",
    Platform::String::typeid,
    ImageWithLabelControl::typeid,
    ref new PropertyMetadata(nullptr,
      ref new PropertyChangedCallback(&ImageWithLabelControl::OnLabelChanged))
    );
```

### <a name="default-value"></a>기본값

속성이 설정 되어 있지 않은 경우 속성에서 항상 특정 기본값을 반환 하도록 종속성 속성의 기본값을 지정할 수 있습니다. 이 값은 해당 속성의 형식에 대 한 기본 기본값과 다를 수 있습니다.

기본값이 지정 되지 않은 경우 종속성 속성의 기본값은 참조 형식의 경우 null이 고, 값 형식 또는 언어 기본 형식의 기본값 (예: 정수의 경우 0, 문자열의 경우 빈 문자열)의 기본값입니다. 기본값을 설정 하는 주된 이유는 속성에서 [**Clearvalue**](/uwp/api/windows.ui.xaml.dependencyobject.clearvalue) 를 호출할 때이 값이 복원 된다는 것입니다. 속성 단위로 기본값을 설정 하는 것은 생성자에서 기본값을 설정 하는 것 보다 더 편리할 수 있습니다. 특히 값 형식에 대 한 값입니다. 그러나 참조 형식의 경우 기본값을 설정 해도 의도 하지 않은 singleton 패턴이 생성 되지 않도록 해야 합니다. 자세한 내용은이 항목의 뒷부분에 나오는 [모범 사례](#best-practices) 를 참조 하십시오.

```cppwinrt
// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Windows::UI::Xaml::PropertyChangedCallback{ &ImageWithLabelControl::OnLabelChanged } }
);
...
```

> [!NOTE]
> [**이 dependencyproperty.unsetvalue로**](/uwp/api/windows.ui.xaml.dependencyproperty.unsetvalue)의 기본값을 사용 하 여 등록 하지 마십시오. 이렇게 하면 속성 소비자가 혼동 되며 속성 시스템 내에서 의도 하지 않은 결과가 발생 합니다.

### <a name="createdefaultvaluecallback"></a>CreateDefaultValueCallback

일부 시나리오에서는 둘 이상의 UI 스레드에서 사용 되는 개체에 대 한 종속성 속성을 정의 합니다. 여러 앱에서 사용 하는 데이터 개체 또는 둘 이상의 앱에서 사용 하는 컨트롤을 정의 하는 경우이 문제가 발생할 수 있습니다. 속성을 등록 한 스레드와 연결 된 기본 값 인스턴스가 아닌 [**CreateDefaultValueCallback**](/uwp/api/windows.ui.xaml.createdefaultvaluecallback) 구현을 제공 하 여 서로 다른 UI 스레드 간에 개체를 교환할 수 있습니다. 기본적으로 [**CreateDefaultValueCallback**](/uwp/api/windows.ui.xaml.createdefaultvaluecallback) 는 기본값에 대 한 팩터리를 정의 합니다. **CreateDefaultValueCallback** 에서 반환 되는 값은 항상 개체를 사용 하는 현재 UI **CreateDefaultValueCallback** 스레드와 연결 됩니다.

[**CreateDefaultValueCallback**](/uwp/api/windows.ui.xaml.createdefaultvaluecallback)를 지정 하는 메타 데이터를 정의 하려면 [**PropertyMetadata**](/uwp/api/windows.ui.xaml.propertymetadata.create) 를 호출 하 여 메타 데이터 인스턴스를 반환 해야 합니다. [**PropertyMetadata**](/uwp/api/Windows.UI.Xaml.PropertyMetadata) 생성자에는 **CreateDefaultValueCallback** 매개 변수를 포함 하는 서명이 없습니다.

[**CreateDefaultValueCallback**](/uwp/api/windows.ui.xaml.createdefaultvaluecallback) 에 대 한 일반적인 구현 패턴은 새 [**dependencyobject**](/uwp/api/Windows.UI.Xaml.DependencyObject) 클래스를 만들고, **dependencyobject** 의 각 속성에 대 한 특정 속성 값을 의도 된 기본값으로 설정한 다음, **CreateDefaultValueCallback** 메서드의 반환 값을 통해 새 클래스를 **개체** 참조로 반환 하는 것입니다.

### <a name="property-changed-callback-method"></a>속성 변경 콜백 메서드

속성 변경 콜백 메서드를 정의 하 여 다른 종속성 속성과의 속성 상호 작용을 정의 하거나 속성이 변경 될 때마다 개체의 내부 속성 또는 상태를 업데이트할 수 있습니다. 콜백을 호출하면 유효한 속성 값 변경이 있다고 속성 시스템이 결정합니다. 콜백 메서드는 정적 이기 때문에 콜백의 *d* 매개 변수는 변경 내용을 보고 한 클래스의 인스턴스를 알려 주므로 중요 합니다. 일반적인 구현에서는 이벤트 데이터의 [**NewValue**](/uwp/api/windows.ui.xaml.dependencypropertychangedeventargs.newvalue) 속성을 사용 하 고 일반적으로 *d*로 전달 된 개체에 대 한 다른 변경을 수행 하 여 해당 값을 처리 합니다. 속성 변경에 대 한 추가 응답은 **NewValue**에서 보고 하는 값을 거부 하거나, [**OldValue**](/uwp/api/windows.ui.xaml.dependencypropertychangedeventargs.oldvalue)를 복원 하거나, **NewValue**에 적용 되는 프로그래밍 제약 조건으로 값을 설정 하는 것입니다.

다음 예제에서는 [**Propertychangedcallback**](/uwp/api/windows.ui.xaml.propertychangedcallback) 구현을 보여 줍니다. [**PropertyMetadata**](/uwp/api/Windows.UI.Xaml.PropertyMetadata)에 대 한 생성 인수의 일부로 이전 [**레지스터**](/uwp/api/windows.ui.xaml.dependencyproperty.register) 예제에서 참조 한 메서드를 구현 합니다. 이 콜백에 의해 해결 된 시나리오는 클래스에 "HasLabelValue" (구현이 표시 되지 않음) 라는 계산 된 읽기 전용 속성을 포함 한다는 것입니다. "Label" 속성이 재평가 될 때마다이 콜백 메서드가 호출 되 고 콜백이 종속 된 계산 값을 종속성 속성의 변경 내용과 동기화 상태로 유지할 수 있습니다.

```csharp
private static void OnLabelChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    ImageWithLabelControl iwlc = d as ImageWithLabelControl; //null checks omitted
    String s = e.NewValue as String; //null checks omitted
    if (s == String.Empty)
    {
        iwlc.HasLabelValue = false;
    } else {
        iwlc.HasLabelValue = true;
    }
}
```

```vb
    Private Shared Sub OnLabelChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
        Dim iwlc As ImageWithLabelControl = CType(d, ImageWithLabelControl) ' null checks omitted
        Dim s As String = CType(e.NewValue,String) ' null checks omitted
        If s Is String.Empty Then
            iwlc.HasLabelValue = False
        Else
            iwlc.HasLabelValue = True
        End If
    End Sub
```

```cppwinrt
void ImageWithLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    auto iwlc{ d.as<ImageWithLabelControlApp::ImageWithLabelControl>() };
    auto s{ winrt::unbox_value<winrt::hstring>(e.NewValue()) };
    iwlc.HasLabelValue(s.size() != 0);
}
```

```cpp
static void OnLabelChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    ImageWithLabelControl^ iwlc = (ImageWithLabelControl^)d;
    Platform::String^ s = (Platform::String^)(e->NewValue);
    if (s->IsEmpty()) {
        iwlc->HasLabelValue=false;
    }
}
```

### <a name="property-changed-behavior-for-structures-and-enumerations"></a>구조체 및 열거형의 속성 변경 동작

[**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) 형식이 열거형 또는 구조체 인 경우 구조체의 내부 값 또는 열거형 값이 변경 되지 않은 경우에도 콜백이 호출 될 수 있습니다. 이는 값이 변경 된 경우에만 호출 되는 문자열과 같은 시스템 기본 형식과는 다릅니다. 이는 내부적으로 수행 되는 이러한 값에 대 한 box 및 unbox 작업의 부작용입니다. 값이 열거형 또는 구조체 인 속성에 대 한 [**Propertychangedcallback**](/uwp/api/windows.ui.xaml.propertychangedcallback) 메서드가 있는 경우 값을 직접 캐스팅 하 고 현재 캐스팅 값에 사용할 수 있는 오버 로드 된 비교 연산자를 사용 하 여 [**OldValue**](/uwp/api/windows.ui.xaml.dependencypropertychangedeventargs.oldvalue) 및 [**NewValue**](/uwp/api/windows.ui.xaml.dependencypropertychangedeventargs.newvalue) 를 비교 해야 합니다. 또는 이러한 연산자를 사용할 수 없는 경우 (사용자 지정 구조에 대 한 경우) 개별 값을 비교 해야 할 수 있습니다. 일반적으로 값이 변경 되지 않은 경우에는 아무 작업도 수행 하지 않도록 선택 합니다.

```csharp
private static void OnVisibilityValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    if ((Visibility)e.NewValue != (Visibility)e.OldValue)
    {
        //value really changed, invoke your changed logic here
    } // else this was invoked because of boxing, do nothing
}
```

```vb
Private Shared Sub OnVisibilityValueChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
    If CType(e.NewValue,Visibility) != CType(e.OldValue,Visibility) Then
        '  value really changed, invoke your changed logic here
    End If
    '  else this was invoked because of boxing, do nothing
End Sub
```

```cppwinrt
static void OnVisibilityValueChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    auto oldVisibility{ winrt::unbox_value<Windows::UI::Xaml::Visibility>(e.OldValue()) };
    auto newVisibility{ winrt::unbox_value<Windows::UI::Xaml::Visibility>(e.NewValue()) };

    if (newVisibility != oldVisibility)
    {
        // The value really changed; invoke your property-changed logic here.
    }
    // Otherwise, OnVisibilityValueChanged was invoked because of boxing; do nothing.
}
```

```cpp
static void OnVisibilityValueChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    if ((Visibility)e->NewValue != (Visibility)e->OldValue)
    {
        //value really changed, invoke your changed logic here
    } 
    // else this was invoked because of boxing, do nothing
    }
}
```

## <a name="best-practices"></a>모범 사례

사용자 지정 종속성 속성을 정의할 때 모범 사례를 염두에 두고 다음 사항을 고려 하십시오.

### <a name="dependencyobject-and-threading"></a>DependencyObject 및 스레딩

모든 [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) 인스턴스는 Windows 런타임 앱에 표시 되는 현재 [**창과**](/uwp/api/Windows.UI.Xaml.Window) 연결 된 UI 스레드에서 만들어야 합니다. 각 **DependencyObject** 를 주 UI 스레드에 만들어야 하지만 [**디스패처**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher)를 호출 하 여 다른 스레드의 디스패처 참조를 사용 하 여 개체에 액세스할 수 있습니다.

[**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) 의 스레딩 측면은 일반적으로 UI 스레드에서 실행 되는 코드만 변경 하거나 종속성 속성의 값을 읽을 수 있다는 것을 의미 하기 때문에 관련이 있습니다. 스레드 문제는 일반적으로 **비동기** 패턴 및 백그라운드 작업자 스레드를 올바르게 사용 하는 일반적인 UI 코드에서 피할 수 있습니다. 일반적으로 사용자 고유의 **dependencyobject** 형식을 정의 하 고이를 데이터 원본 또는 **dependencyobject** 가 반드시 적절 하지 않아도 되는 다른 시나리오에 사용 하려는 경우에만 **dependencyobject**관련 스레딩 문제를 실행 합니다.

### <a name="avoiding-unintentional-singletons"></a>의도 하지 않은 단일 항목 방지

참조 형식을 사용 하는 종속성 속성을 선언 하 고 [**PropertyMetadata**](/uwp/api/Windows.UI.Xaml.PropertyMetadata)를 설정 하는 코드의 일부로 해당 참조 형식에 대 한 생성자를 호출 하는 경우 의도 하지 않은 singleton이 발생할 수 있습니다. 종속성 속성을 모두 사용 하면 **PropertyMetadata** 의 인스턴스를 하나만 공유 하므로 생성 된 단일 참조 형식을 공유 하려고 합니다. 종속성 속성을 통해 설정한 값 형식의 모든 하위 속성은 의도 하지 않은 방식으로 다른 개체에 전파 됩니다.

Null이 아닌 값을 원하는 경우 클래스 생성자를 사용 하 여 참조 형식 종속성 속성의 초기 값을 설정할 수 있지만이 값은 [종속성 속성 개요](dependency-properties-overview.md)의 용도에 대 한 로컬 값으로 간주 됩니다. 클래스가 템플릿을 지 원하는 경우이 용도로 템플릿을 사용 하는 것이 더 적합할 수 있습니다. Singleton 패턴을 방지 하지만 여전히 유용한 기본값을 제공 하는 또 다른 방법은 해당 클래스의 값에 대 한 적절 한 기본값을 제공 하는 정적 속성을 참조 형식에 노출 하는 것입니다.

### <a name="collection-type-dependency-properties"></a>컬렉션 형식 종속성 속성

컬렉션 형식 종속성 속성에는 고려할 몇 가지 추가 구현의 문제점이 있습니다.

컬렉션 형식 종속성 속성은 Windows 런타임 API에서 비교적 드물게 발생 합니다. 대부분의 경우 항목이 [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) 하위 클래스 이지만 컬렉션 속성 자체가 기존 CLR 또는 c + + 속성으로 구현 되는 컬렉션을 사용할 수 있습니다. 이는 컬렉션은 종속성 속성이 관련 된 일반적인 시나리오에 반드시 적합할 필요는 없기 때문입니다. 예:

- 일반적으로 컬렉션에 애니메이션 효과를 주지 않습니다.
- 일반적으로 스타일 또는 템플릿을 사용 하 여 컬렉션의 항목을 미리 채웁니다.
- 컬렉션에 바인딩하는 것은 주요 시나리오 이지만 컬렉션은 종속성 속성이 될 필요는 없습니다. 바인딩 대상의 경우 [**ItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) 또는 [**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate) 의 서브 클래스를 사용 하 여 컬렉션 항목을 지원 하거나 뷰 모델 패턴을 사용 하는 것이 더 일반적입니다. 컬렉션과의 바인딩에 대 한 자세한 내용은 [데이터 바인딩 심층](../data-binding/data-binding-in-depth.md)분석을 참조 하세요.
- **INotifyPropertyChanged** 또는 **INotifyCollectionChanged**와 같은 인터페이스를 통해 또는 [**system.collections.objectmodel.observablecollection &lt; T &gt; **](/dotnet/api/system.collections.objectmodel.observablecollection-1)에서 컬렉션 형식을 파생 하 여 컬렉션 변경에 대 한 알림을 더 쉽게 해결할 수 있습니다.

그러나 컬렉션 형식 종속성 속성에 대 한 시나리오가 존재 합니다. 다음 세 섹션에서는 컬렉션 형식 종속성 속성을 구현 하는 방법에 대 한 몇 가지 지침을 제공 합니다.

### <a name="initializing-the-collection"></a>컬렉션 초기화

종속성 속성을 만들 때 종속성 속성 메타 데이터를 통해 기본값을 설정할 수 있습니다. 그러나 singleton 정적 컬렉션을 기본값으로 사용 하지 않도록 주의 해야 합니다. 대신 컬렉션 속성의 owner 클래스에 대 한 클래스 생성자 논리의 일부로 컬렉션 값을 고유 (인스턴스) 컬렉션으로 의도적으로 설정 해야 합니다.

### <a name="change-notifications"></a>변경 알림

컬렉션을 종속성 속성으로 정의 하면 "PropertyChanged" 콜백 메서드를 호출 하는 속성 시스템을 통해 컬렉션의 항목에 대 한 변경 알림이 자동으로 제공 되지 않습니다. 컬렉션 또는 컬렉션 항목에 대 한 알림을 원할 경우 (예: 데이터 바인딩 시나리오의 경우) **INotifyPropertyChanged** 또는 **INotifyCollectionChanged** 인터페이스를 구현 합니다. 자세한 내용은 [심층 데이터 바인딩](../data-binding/data-binding-in-depth.md)을 참조 하세요.

### <a name="dependency-property-security-considerations"></a>종속성 속성 보안 고려 사항

종속성 속성을 공용 속성으로 선언 합니다. 종속성 속성 식별자를 **공용 정적 읽기 전용** 멤버로 선언 합니다. 언어에서 허용 하는 다른 액세스 수준 (예: **protected**)을 선언 하려고 해도 종속성 속성은 항상 속성 시스템 api와 함께 식별자를 통해 액세스할 수 있습니다. 속성 시스템이 제대로 작동할 수 없기 때문에 종속성 속성 식별자를 internal 또는 private으로 선언 하는 작업은 작동 하지 않습니다.

래퍼 속성은 단지 편의를 위해, 대신 [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) 또는 [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) 를 호출 하 여 래퍼에 적용 되는 보안 메커니즘을 건너뛸 수 있습니다. 따라서 래퍼 속성을 공개로 유지 합니다. 그렇지 않으면 실제 보안 혜택을 제공 하지 않고 합법적인 호출자가 사용할 수 있는 속성을 어렵게 만듭니다.

Windows 런타임는 사용자 지정 종속성 속성을 읽기 전용으로 등록 하는 방법을 제공 하지 않습니다.

### <a name="dependency-properties-and-class-constructors"></a>종속성 속성 및 클래스 생성자

클래스 생성자가 가상 메서드를 호출 하지 않아야 하는 일반적인 원칙이 있습니다. 이는 생성자를 호출 하 여 파생 클래스 생성자의 기본 초기화를 수행 하 고, 생성 되는 개체 인스턴스가 아직 완전히 초기화 되지 않았을 때 생성자를 통해 가상 메서드를 입력 하는 경우에 발생할 수 있기 때문입니다. [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject)에서 이미 파생 된 클래스에서 파생 하는 경우 속성 시스템 자체에서 해당 서비스의 일부로 내부적으로 가상 메서드를 호출 하 고 노출 한다는 점에 주의 해야 합니다. 런타임 초기화에 대 한 잠재적인 문제를 방지 하려면 클래스의 생성자 내에서 종속성 속성 값을 설정 하지 마세요.

### <a name="registering-the-dependency-properties-for-ccx-apps"></a>C + +/CX 앱에 대 한 종속성 속성 등록

C++/CX로 속성 등록을 위해 구현하는 일은 C#의 경우보다 어렵습니다. 이는 헤더와 구현 파일과 구분해야 하며 구현 파일의 루트 범위에서 초기화하는 것은 잘못된 용례이기 때문입니다. (Visual C++ 구성 요소 확장 (c + +/CX)은 루트 범위의 정적 이니셜라이저 코드를 **DllMain**에 직접 배치 하는 반면, c # 컴파일러는 정적 이니셜라이저를 클래스에 할당 하므로 **DllMain** 로드 잠금 문제를 방지 합니다. 여기에서 가장 좋은 방법은 클래스에 대 한 모든 종속성 속성 등록을 수행 하는 도우미 함수를 선언 하는 것입니다. 클래스 마다 함수 하나를 선언 합니다. 그런 다음 앱이 사용 하는 각 사용자 지정 클래스에 대해 사용 하려는 각 사용자 지정 클래스에 의해 노출 되는 도우미 등록 함수를 참조 해야 합니다. 이전에 [**응용 프로그램 생성자**](/uwp/api/windows.ui.xaml.application.-ctor) ()의 일부로 각 도우미 등록 함수를 한 번 호출 `App::App()` `InitializeComponent` 합니다. 이 생성자는 앱이 처음으로 참조 되는 경우에만 실행 됩니다. 예를 들어 일시 중단 된 앱이 다시 시작 되 면 다시 실행 되지 않습니다. 또한 이전 C++ 등록 예제에서 본 것처럼, 각 [**Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) 호출 시 **nullptr** 확인은 함수 호출자가 해당 속성을 두 번 등록할 수 없도록 하므로 매우 중요합니다. 두 번째 등록 호출은 속성 이름이 중복 되기 때문에 이러한 검사 없이 앱을 중단 시킬 수 있습니다. 샘플의 c + +/CX 버전에 대 한 코드를 살펴보면 [XAML 사용자 및 사용자 지정 컨트롤 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/XAML%20user%20and%20custom%20controls%20sample) 에서이 구현 패턴을 볼 수 있습니다.

## <a name="related-topics"></a>관련 항목

- [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject)
- [**DependencyProperty. Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register)
- [종속성 속성 개요](dependency-properties-overview.md)
- [XAML 사용자 및 사용자 지정 컨트롤 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/XAML%20user%20and%20custom%20controls%20sample)
 