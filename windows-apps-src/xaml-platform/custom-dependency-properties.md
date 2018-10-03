---
author: jwmsft
description: C++, C# 또는 Visual Basic으로 작성한 Windows 런타임 앱의 사용자 지정 종속성 속성을 정의하고 구현하는 방법에 대해 설명합니다.
title: 사용자 지정 종속성 속성
ms.assetid: 5ADF7935-F2CF-4BB6-B1A5-F535C2ED8EF8
ms.author: jimwalk
ms.date: 07/12/2018
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
ms.openlocfilehash: ddeccfe4c5e198afd77eaa4a81fc017543291ba1
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4315913"
---
# <a name="custom-dependency-properties"></a>사용자 지정 종속성 속성

여기에서는 C++, C# 또는 Visual Basic으로 작성된 Windows 런타임 앱의 고유 종속성 속성을 정의하고 구현하는 방법에 대해 설명합니다. 앱 개발자 및 구성 요소 작성자가 사용자 지정 종속성 속성을 만들려고 하는 이유를 나열합니다. 사용자 지정 종속성 속성 구현 단계와 종속성 속성의 성능, 유용성 또는 다양성을 향상시킬 수 있는 몇 가지 모범 사례를 설명합니다.

## <a name="prerequisites"></a>필수 조건

개발자가 [종속성 속성 개요](dependency-properties-overview.md)를 읽었고 기존 종속성 속성의 소비자 관점에서 종속성 속성을 이해한다고 가정합니다. 이 항목에 있는 예를 이해하려면 XAML과 C++, C# 또는 Visual Basic을 사용하여 기본 Windows 런타임 앱을 작성하는 방법도 알고 있어야 합니다.

## <a name="what-is-a-dependency-property"></a>종속성 속성이란?

속성에 대해 스타일 지정, 데이터 바인딩, 애니메이션 및 기본값을 지원하려면 종속성 속성으로 구현해야 합니다. 종속성 속성 값은 클래스의 필드로 저장되지 않고 xaml 프레임워크에서 저장되며, [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 메서드를 호출하여 속성을 Windows 런타임 속성 시스템에 등록할 때 검색되는 키를 사용하여 참조됩니다.   종속성 속성은 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)에서 파생된 형식에서만 사용할 수 있습니다. 그러나 **DependencyObject**는 클래스 계층에서 매우 상위이므로 UI 및 표시 지원을 위한 클래스는 대부분 종속성 속성을 지원할 수 있습니다. 종속성 속성과 이 설명서의 설명 내용에 사용된 일부 용어 및 규칙에 대한 자세한 내용은 [종속성 속성 개요](dependency-properties-overview.md)를 참조하세요.

Windows 런타임의 종속성 속성 예는 [**Control.Background**](https://msdn.microsoft.com/library/windows/apps/br209395), [**FrameworkElement.Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 및 [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/br209702) 등 여러 가지가 있습니다.

규칙에 따라 클래스별로 노출된 각 종속성 속성에는 동일한 클래스에 대해 노출되고 종속성 속성의 식별자를 제공하는 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 형식의 해당 **public static readonly** 속성이 있습니다. 식별자 이름 지정 규칙은 종속성 속성 이름이 오고 이름 뒤에 "Property" 문자열을 추가하는 것입니다. 예를 들어 **Control.Background** 속성의 해당 **DependencyProperty** 식별자는 [**Control.BackgroundProperty**](https://msdn.microsoft.com/library/windows/apps/br209396)입니다. 식별자는 종속성 속성에 대한 정보를 등록된 대로 저장하며, [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 호출 등 종속성 속성과 관련된 다른 작업에 사용될 수 있습니다.

## <a name="property-wrappers"></a>속성 래퍼

종속성 속성에는 일반적으로 래퍼 구현이 포함되어 있습니다. 래퍼가 없으면 속성을 가져오거나 설정하는 유일한 방법은 종속성 속성 유틸리티 메서드 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 및 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)를 사용하여 식별자를 매개 변수로 전달하는 것입니다. 이 방법은 원칙적으로 속성인 항목에 부자연스러운 사용법입니다. 그러나 래퍼가 있으면 종속성 속성을 참조하는 고유 코드 및 모든 기타 코드에서 사용하는 언어에 자연스러운 간단한 개체-속성 구문을 사용할 수 있습니다.

사용자 지정 종속성 속성을 구현하고 호출하기 쉽게 공개하려면 속성 래퍼도 정의하세요. 속성 래퍼는 리플렉션 또는 정적 분석 프로세스에 종속성 속성에 대한 기본 정보를 보고하는 경우에도 유용합니다. 특히 래퍼는 [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011) 같은 특성을 지정하는 위치입니다.

## <a name="when-to-implement-a-property-as-a-dependency-property"></a>속성을 종속성 속성으로 구현하는 시기

클래스가 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)에서 파생된 경우 클래스에서 공개 읽기/쓰기 속성을 구현할 때마다 속성을 종속성 속성으로 작동하게 할 수 있는 옵션이 있습니다. 개인 필드로 속성을 지원하는 일반적인 기술이 충분한 경우가 있습니다. 사용자 지정 속성을 종속성 속성으로 정의하는 것이 불필요하거나 부적절한 경우도 있습니다. 속성을 지원하려는 시나리오에 따라 선택이 달라질 수 있습니다.

속성이 Windows 런타임 또는 Windows 런타임 앱의 다음 기능 중 하나 이상을 지원하도록 하려는 경우 해당 속성을 종속성 속성으로 구현하는 것을 고려할 수 있습니다.

- [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849)을 통한 속성 설정
- [**{Binding}**](binding-markup-extension.md)을 사용하여 데이터 바인딩에 유효한 대상 속성 역할 수행
- [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490)를 통해 애니메이션 값 지원
- 다음에 의해 속성 값이 변경된 시점 보고
  - 속성 시스템 자체에서 수행된 작업
  - 환경
  - 사용자 작업
  - 읽기 및 쓰기 스타일

## <a name="checklist-for-defining-a-dependency-property"></a>종속성 속성 정의 검사 목록

종속성 속성 정의는 개념 집합으로 간주될 수 있습니다. 구현에서는 코드의 한 줄에 여러 개념이 언급될 수 있으므로 이러한 개념이 반드시 절차적 단계일 필요는 없습니다. 이 목록은 간단한 개요만 제공합니다. 이 항목의 뒷 부분에서 각 개념을 더 자세히 설명하고 여러 언어로 코드 예를 제공합니다.

- 속성 시스템에 속성 이름을 등록하여([**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 호출) 소유자 형식 및 속성 값 형식을 지정합니다.
  - 속성 메타데이터를 예상하는 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)에는 필수 매개 변수가 있습니다. 해당 값으로 **null**을 지정하거나, 속성 변경 동작이나 [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357)를 호출하여 복원할 수 있는 메타데이터 기반 기본값을 원하는 경우 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.propertymetadata) 인스턴스를 지정합니다.
- [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 식별자를 소유자 형식의 **public static readonly** 속성 멤버로 정의합니다.
- 구현하는 언어에 사용되는 속성 접근자 모델 다음에 래퍼 속성을 정의합니다. 래퍼 속성 이름은 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)에서 사용한 *name* 문자열과 일치해야 합니다. [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 및 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)를 호출하고 고유 속성의 식별자를 매개 변수로 전달하여 **get** 및 **set** 접근자를 구현하고 래핑하는 종속성 속성과 래퍼를 연결합니다.
- (옵션) [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011) 같은 특성을 래퍼에 지정합니다.

> [!NOTE]
> 사용자 지정 연결 된 속성을 정의 하는 경우 일반적으로 래퍼를 생략 합니다. 대신 XAML 프로세서가 사용할 수 있는 다른 스타일의 접근자를 작성합니다. [사용자 지정 연결된 속성](custom-attached-properties.md)을 참조하세요. 

## <a name="registering-the-property"></a>속성 등록

속성이 종속성 속성이 되도록 하려면 Windows 런타임 속성 시스템에서 관리하는 속성 저장소에 해당 속성을 등록해야 합니다.  속성을 등록하려면 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 메서드를 호출합니다.

Microsoft .NET 언어(C# 및 Microsoft Visual Basic)의 경우 클래스 본문 내에서(클래스 내부이나 멤버 정의 외부임) [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)를 호출합니다. 식별자는 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 메서드 호출에서 반환 값으로 제공됩니다. [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 호출은 일반적으로 정적 생성자로 수행되거나, 클래스의 일부인 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 형식의 **public static readonly** 속성 초기화의 일부로 수행됩니다. 이 속성은 종속성 속성의 식별자를 노출합니다. 다음은 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 호출의 예입니다.

> [!NOTE]
> 종속성 속성 등록 식별자의 일부로 속성 정의 일반적인 구현 이지만 클래스 정적 생성자에서 종속성 속성을 등록할 수도 있습니다. 종속성 속성을 초기화하는 데 두 줄 이상의 코드가 필요한 경우 이 방법이 적절할 수 있습니다.

C + + /CX 헤더 및 코드 파일 사이 구현을 분할 하는 방법에 대 한 옵션 수 있습니다. 일반적인 분할은 **get** 구현은 포함되고 **set**는 포함되지 않도록 식별자 자체를 헤더의 **publicstatic** 속성으로 선언하는 것입니다. **get** 구현은 초기화되지 않은 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 인스턴스인 개인 필드를 참조합니다. 래퍼 및 해당 래퍼의 **get** 및 **set** 구현을 선언할 수도 있습니다. 이 경우 헤더에 일부 최소 구현이 포함됩니다. 래퍼에 Windows 런타임 특성이 필요한 경우 헤더에도 특성이 필요합니다. 코드 파일에서 앱이 처음으로 시작될 때만 실행되는 도우미 함수 내에 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 호출을 배치합니다. **Register**의 반환 값을 사용하여 헤더에서 선언한 정적이나 초기화되지 않은 식별자를 채웁니다. 이는 처음에 구현 파일의 루트 범위에서 **nullptr**로 설정한 식별자입니다.

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
> C + + 코드 CX, 개인 필드 이유와 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 표면을 공개 읽기 전용 속성은 종속성 속성을 사용 하는 다른 호출자가 속성 시스템 유틸리티를 필요로 하는 Api를 사용할 수도 수 있도록 하는 이유는 가 공개 식별자입니다. 식별자를 개인 상태로 유지하면 다른 사용자가 이러한 유틸리티 API를 사용할 수 없습니다. 이러한 API 및 시나리오의 예로는 선택에 따라 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 또는 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361), [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357), [**GetAnimationBaseValue**](https://msdn.microsoft.com/library/windows/apps/br242358), [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257)및 [**Setter.Property**](https://msdn.microsoft.com/library/windows/apps/br208836)가 있습니다. Windows 런타임 메타데이터 규칙에서는 공용 필드가 허용되지 않으므로 여기에서 공용 필드를 사용할 수 없습니다.

## <a name="dependency-property-name-conventions"></a>종속성 속성 이름 규칙

종속성 속성에 대한 명명 규칙이 있습니다. 예외 상황을 제외하고는 항상 이 규칙을 따릅니다. 종속성 속성에는 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)의 첫 번째 매개 변수로 제공되는 고유 기본 이름(앞의 예에서는 "Label")이 있습니다. 이름은 각 등록 형식 내에서 고유해야 하며 이러한 고유성 요구 사항은 모든 상속되는 멤버에도 적용됩니다. 기본 형식을 통해 상속되는 종속성 속성은 이미 등록 형식의 일부로 간주됩니다. 상속되는 속성의 이름은 다시 등록될 수 없습니다.

> [!WARNING]
> 하지만 여기 문자열 식별자가 될 수를 제공 하는 이름은 선택한 언어의 프로그래밍에서 유효한, 일반적으로 xaml에서 에서도 종속성 속성을 설정할 수 하고자 합니다. XAML에서 설정하도록 하려면 선택하는 속성 이름이 유효한 XAML 이름이어야 합니다. 자세한 내용은 [XAML 개요](xaml-overview.md)를 참조하세요.

식별자 속성을 만드는 경우 등록한 속성 이름을 "Property" 접미사와 연결합니다(예: "LabelProperty"). 이 속성은 종속성 속성 식별자이며 고유 속성 래퍼에서 수행하는 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 및 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 호출의 입력으로 사용됩니다. 속성 시스템 및 [**{x:Bind}**](x-bind-markup-extension.md) 등의 다른 XAML 프로세서에서도 사용됩니다.

## <a name="implementing-the-wrapper"></a>래퍼 구현

속성 래퍼는 **get** 구현에서 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359)를, **set** 구현에서 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)를 호출합니다.

> [!WARNING]
> 예외적인, 래퍼 구현 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 및 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 작업만 수행 해야 합니다. 그렇지 않으면 속성이 XAML을 통해 설정되는 경우와 코드를 통해 설정되는 경우에 동작이 달라집니다. 효율성을 위해 XAML 파서는 종속성 속성을 설정할 때 래퍼를 무시하고 **SetValue**를 통해 백업 저장소에 통신합니다.

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

## <a name="property-metadata-for-a-custom-dependency-property"></a>사용자 지정 종속성 속성의 속성 메타데이터

속성 메타데이터가 종속성 속성에 할당되면 속성 소유자 형식이나 서브클래스의 모든 인스턴스에 대한 해당 속성에 동일한 메타데이터가 적용됩니다. 속성 메타데이터에서는 다음 두 동작을 지정할 수 있습니다.

- 속성 시스템이 속성의 모든 케이스에 할당하는 기본값
- 속성 값 변경이 발견될 때마다 속성 시스템 내에서 자동으로 호출되는 정적 콜백 메서드

### <a name="calling-register-with-property-metadata"></a>속성 메타데이터로 레지스터 호출

이전 [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 호출 예에서는 *propertyMetadata* 매개 변수에 대해 null 값을 전달했습니다. 종속성 속성에서 기본값을 제공하거나 속성이 변경된 콜백을 사용할 수 있도록 하려면 이 접근 권한 값 중 하나 또는 둘 다를 제공하는 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 인스턴스를 정의해야 합니다.

일반적으로 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771)를 인라인으로 만든 인스턴스로서, [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)용 매개 변수 내에서 제공합니다.

> [!NOTE]
> [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) 구현을 정의 하는 경우 **PropertyMetadata** 인스턴스를 정의 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 생성자를 호출 하는 것이 아니라 [**PropertyMetadata.Create**](https://msdn.microsoft.com/library/windows/apps/hh702099) 유틸리티 메서드를 사용 해야 합니다.

이 다음 예에서는 [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770) 값으로 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 인스턴스를 참조하여 이전에 보여진 [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 예를 수정합니다. "OnLabelChanged" 콜백의 구현은 이 섹션의 뒷부분에 설명되어 있습니다.

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

속성을 설정 해제하면 속성이 항상 특정 기본값을 반환하도록 종속성 속성에 대한 기본값을 지정할 수 있습니다. 이 값은 해당 속성의 유형에 대한 내부 기본값과 다를 수 있습니다.

기본값을 지정하지 않으면 참조 형식의 경우 종속성 속성 기본값이 Null이고, 값 형식이나 언어 primitive의 경우 해당 형식의 기본값입니다(예를 들어, 정수의 경우 0이나 문자열의 경우 빈 문자열). 기본값을 설정하는 주된 이유는 속성에서 [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357)를 호출하면 이 값이 복원된다는 것입니다. 개별 속성 기준 기본값 설정이 생성자 특히 값 형식의 기본값 설정보다 더 편리할 수 있습니다. 그러나 참조 형식의 경우 기본값 설정으로 의도하지 않은 단일 패턴이 만들어지지 않도록 해야 합니다. 자세한 내용은 이 항목의 뒷부분에 있는 [모범 사례](#best-practices)를 참조하세요.

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
> [**UnsetValue**](https://msdn.microsoft.com/library/windows/apps/br242371)기본값을 사용 하 여 등록 하지 마세요. 등록하는 경우 속성 소비자를 혼란스럽게 하고 속성 시스템 내에 의도하지 않은 결과를 발생시킵니다.

### <a name="createdefaultvaluecallback"></a>CreateDefaultValueCallback

일부 시나리오에서는 두 개 이상의 UI 스레드에서 사용되는 개체에 대한 종속성 속성을 정의하게 됩니다. 여러 앱에 의해 사용되는 데이터 개체나 두 개 이상의 앱에서 사용하는 컨트롤을 정의하는 경우 이럴 수 있습니다. 기본값 인스턴스가 아닌, 속성을 등록한 스레드에 제한된 [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) 구현을 제공하여 서로 다른 UI 스레드 간 개체 교환을 가능하게 할 수 있습니다. 기본적으로 [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812)은 기본값에 대한 팩터리를 정의합니다. **CreateDefaultValueCallback**에 의해 반환되는 값은 항상 개체를 사용하는 현재 UI **CreateDefaultValueCallback** 스레드와 연결되어 있습니다.

[**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812)을 지정하는 메타데이터를 정의하기 위해서는 [**PropertyMetadata.Create**](https://msdn.microsoft.com/library/windows/apps/hh702115)을 호출하여 메타데이터 인스턴스를 반환해야 합니다. [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 생성자에는 **CreateDefaultValueCallback** 매개 변수를 포함하는 서명이 없습니다.

[**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812)에 대한 전형적인 구현 패턴은 새 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 클래스를 생성하고 **DependencyObject**의 각 속성의 특정 속성 값을 설정한 다음 **CreateDefaultValueCallback** 메서드의 반환 값을 통해 **Object** 참조로서 새 클래스를 반환하는 것입니다.

### <a name="property-changed-callback-method"></a>속성 변경 콜백 메서드

해당 속성과 다른 종속성 속성의 조작을 정의하거나 속성이 변경될 때마다 개체의 내부 속성 또는 상태를 업데이트하기 위해 속성 변경 콜백 메서드를 정의할 수 있습니다. 콜백을 호출하면 유효한 속성 값 변경이 있다고 속성 시스템이 결정합니다. 콜백 메서드는 정적이므로 콜백의 *d* 매개 변수가 중요합니다. 변경이 보고된 클래스 인스턴스를 알려주기 때문입니다. 일반 구현에서는 이벤트 데이터의 [**NewValue**](https://msdn.microsoft.com/library/windows/apps/br242364) 속성을 사용하며 대개 *d*로 전달되는 개체에서 다른 변경을 수행하는 방법으로 해당 값을 처리합니다. 속성 변경에 대한 또 다른 응답은 **NewValue**에서 보고한 값을 거부하거나 [**OldValue**](https://msdn.microsoft.com/library/windows/apps/br242365)를 복원하거나 값을 **NewValue**에 적용되는 프로그래밍 제약 조건으로 설정하는 것입니다.

다음 예는 [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770) 구현을 보여 줍니다. 이전 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 예에서 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) 구성 인수의 일부로 참조된다고 표시된 메서드를 구현합니다. 이 콜백으로 설명되는 시나리오에서는 클래스에 이름이 "HasLabelValue"인, 계산된 읽기 전용 속성도 있습니다(구현 표시 안 됨). "Label" 속성이 재평가될 때마다 이 콜백 메서드가 호출되며 콜백을 통해 종속 계산 값이 종속성 속성 변경 내용과 동기화된 상태를 유지할 수 있습니다.

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

### <a name="property-changed-behavior-for-structures-and-enumerations"></a>구조 및 열거에 대해 속성이 변경된 동작

[**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362)의 유형이 열거 또는 구조인 경우 구조의 내부 값 또는 열거형 값이 변경되지 않더라도 콜백이 호출될 수 있습니다. 이것은 값이 변할 경우에만 호출되는 문자열과 같은 기본 시스템과는 다릅니다. 이것은 이 값들에 대한 boxiung 및 unboxing 작업의 부작용으로서 내부적으로 수행됩니다. 값이 열거 또는 구조인 속성에 대한 [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770) 메서드가 있을 경우 직접 값을 캐스팅하고 지금 캐스팅 값에 사용할 수 있는 오버로드된 비교 연산자를 사용하여 [**OldValue**](https://msdn.microsoft.com/library/windows/apps/br242365) 및 [**NewValue**](https://msdn.microsoft.com/library/windows/apps/br242364)를 비교해야 합니다. 또는, 그러한 연산자를 사용할 수 없을 경우(사용자 지정 구조 사례일 수 있음), 개별 값을 비교해야 할 수 있습니다. 그 결과 값이 변하지 않았다면 일반적으로 어떤 작업이든 선택하지 않게 됩니다.

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

사용자 지정 종속성 속성을 정의하는 경우 모범 사례로 다음 사항을 고려합니다.

### <a name="dependencyobject-and-threading"></a>DependencyObject 및 스레딩

모든 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 인스턴스는 Windows 런타임 앱에 표시되는 현재 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)와 연결된 UI 스레드에 만들어야 합니다. 각 **DependencyObject**는 주 UI 스레드에 만들어야 하지만 개체는 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br230616)를 호출하여 다른 스레드의 디스패처 참조를 사용하여 액세스할 수 있습니다.

[**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)의 스레딩 측면은 일반적으로 UI 스레드에서 실행되는 코드만 종속성 속성의 값을 변경하거나 읽을 수 있다는 의미이므로 관련이 있습니다. 스레딩 문제는 보통 **async** 패턴 및 백그라운드 작업자 스레드를 올바르게 사용하는 일반적인 UI 코드로 방지할 수 있습니다. 일반적으로 직접 **DependencyObject** 유형을 정의하여 **DependencyObject**가 적합하지 않을 수 있는 데이터 원본이나 다른 시나리오에 사용하려고 하는 경우에만 **DependencyObject** 관련 스레딩 문제가 발생합니다.

### <a name="avoiding-unintentional-singletons"></a>의도하지 않은 단일 패턴 방지

참조 형식이 사용되는 종속성 속성을 선언하고 [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771)를 설정하는 코드의 일부로 해당 참조 형식의 생성자를 호출하는 경우 의도하지 않은 단일 패턴이 발생할 수 있습니다. 모든 종속성 속성 사용에서 **PropertyMetadata** 인스턴스를 하나만 공유하므로 생성된 단일 참조 형식을 공유하려는 것입니다. 그러면 종속성 속성을 통해 설정한 값 형식의 모든 하위 속성이 의도하지 않은 방식으로 다른 개체에 전파됩니다.

Null이 아닌 값이 필요한 경우 클래스 생성자를 사용하여 참조 형식 종속성 속성의 초기 값을 설정할 수 있으나 이렇게 하면 [종속성 속성 개요](dependency-properties-overview.md)를 위해 로컬 값으로 간주됩니다. 클래스에서 템플릿을 지원하는 경우 이 목적을 위해서는 템플릿을 사용하는 것이 더 적절할 수 있습니다. 단일 패턴을 방지하지만 유용한 기본값을 제공하는 또 다른 방법은 해당 클래스의 값에 적절한 기본값을 제공하는 참조 형식의 정적 속성을 노출하는 것입니다.

### <a name="collection-type-dependency-properties"></a>컬렉션 형식 종속성 속성

컬렉션 형식 종속성 속성에는 고려해야 할 몇 가지 추가 구현 문제가 있습니다.

Windows 런타임 API에서 컬렉션 형식 종속성 속성은 상대적으로 자주 사용되지 않습니다. 대부분의 경우 항목이 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 서브클래스인 컬렉션을 사용할 수 있으나 컬렉션 속성 자체는 기본 CLR 또는 C++ 속성으로 구현됩니다. 종속성 속성이 관련된 몇 가지 일반적인 시나리오에 컬렉션이 반드시 적합한 것은 아니기 때문입니다. 예를 들면 다음과 같습니다.

- 일반적으로 컬렉션은 애니메이션하지 않습니다.
- 일반적으로 스타일 또는 템플릿을 사용하여 컬렉션의 항목을 미리 채우지 않습니다.
- 컬렉션에 바인딩하는 것이 주요 시나리오이긴 하지만 컬렉션이 바인딩 소스이기 위해 종속성 속성일 필요는 없습니다. 바인딩 대상의 경우 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/br242803) 또는 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348)의 서브클래스를 사용하여 컬렉션 항목을 지원하거나 보기 모델 패턴을 사용하는 것이 더 일반적입니다. 컬렉션 바인딩에 대한 자세한 내용은 [데이터 바인딩 심층 분석](https://msdn.microsoft.com/library/windows/apps/mt210946)을 참조하세요.
- 컬렉션 변경 알림은 **INotifyPropertyChanged** 또는 **INotifyCollectionChanged** 같은 인터페이스를 통해 또는 [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/ms668604.aspx)에서 컬렉션 형식을 파생시키는 방법을 통해 더 효과적으로 설명됩니다.

하지만 컬렉션 형식 종속성 속성에 대한 시나리오도 존재합니다. 다음의 3개 섹션에서는 컬렉션 형식 종속성 속성을 구현하는 방법에 대한 몇 가지 지침을 제공합니다.

### <a name="initializing-the-collection"></a>컬렉션 초기화

종속성 속성을 만드는 경우 종속성 속성 메타데이터로 기본값을 설정할 수 있습니다. 하지만 단일 정적 컬렉션을 기본값으로 사용하지 않도록 주의해야 합니다. 대신 의도적으로 컬렉션 속성 소유자 클래스에 대해 클래스 생성자 논리의 일부로 컬렉션 값을 고유(인스턴스) 컬렉션으로 설정해야 합니다.

### <a name="change-notifications"></a>변경 알림

컬렉션을 종속성 속성으로 정의해도 "PropertyChanged" 콜백 메서드를 호출하는 속성 시스템이라는 이유로 컬렉션의 항목에 대한 변경 알림을 자동으로 제공하지 않습니다. 컬렉션 또는 컬렉션 항목 알림이 필요하면(예를 들어 데이터 바인딩 시나리오의 경우) **INotifyPropertyChanged** 또는 **INotifyCollectionChanged** 인터페이스를 구현합니다. 자세한 내용은 [데이터 바인딩 심층 분석](https://msdn.microsoft.com/library/windows/apps/mt210946)을 참조하세요.

### <a name="dependency-property-security-considerations"></a>종속성 속성 보안 고려 사항

종속성 속성은 public 속성으로 선언합니다. 종속성 속성 식별자는 **public static readonly** 멤버로 선언합니다. 언어(예: **protected**)에서 허용하는 다른 액세스 수준을 선언하려고 시도해도 종속성 속성은 항상 속성-시스템 API와 함께 식별자를 통해 액세스할 수 있습니다. 종속성 속성 식별자는 내부 또는 개인으로 선언할 수 없습니다. 이렇게 하면 속성 시스템이 올바르게 작동할 수 없기 때문입니다.

래퍼 속성은 순전히 편의를 위한 것입니다. 래퍼에 적용되는 보안 메커니즘은 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 또는 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)를 대신 호출하여 무시할 수 있습니다. 그러므로 래퍼 속성을 공개 상태로 유지하세요. 그렇지 않으면 실질적인 보안상 장점을 전혀 제공하지도 않으면서 정상 호출자가 속성을 사용하기 더 어려워집니다.

Windows 런타임은 사용자 지정 종속성 속성을 읽기 전용으로 등록할 방법을 제공하지 않습니다.

### <a name="dependency-properties-and-class-constructors"></a>종속성 속성 및 클래스 생성자

클래스 생성자는 가상 메서드를 호출하지 않아야 한다는 일반적인 원칙이 있습니다. 파생된 클래스 생성자의 기본 초기화를 수행하기 위해 생성자가 호출될 수 있기 때문입니다. 또한 생성된 개체 인스턴스가 아직 완전히 초기화되지 않은 경우 생성자를 통해 가상 메서드를 입력할 수도 있습니다. [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)에서 이미 파생된 클래스에서 파생을 시도하는 경우 속성 시스템이 내부에서 해당 서비스의 일부로 가상 메서드를 호출하고 노출합니다. 가능한 런타임 초기화 문제를 방지하려면 클래스 생성자 내에 종속성 속성 값을 설정하지 마세요.

### <a name="registering-the-dependency-properties-for-ccx-apps"></a>C++/CX 앱의 종속성 속성 등록

C++/CX로 속성 등록을 위해 구현하는 일은 C#의 경우보다 어렵습니다. 이는 헤더와 구현 파일과 구분해야 하며 구현 파일의 루트 범위에서 초기화하는 것은 잘못된 용례이기 때문입니다. Visual C++ 구성 요소 확장(C++/CX)은 루트 범위의 정적 이니셜라이저 코드를 **DllMain**에 직접 배치하는 반면, C# 컴파일러는 클래스에 정적 이니셜라이저를 할당하여 **DllMain** 로드 잠금 문제를 방지합니다. 여기서는 클래스당 함수 하나씩, 클래스에 대한 종속성 속성 등록을 모두 수행하는 도우미 함수를 선언하는 방식이 가장 좋습니다. 그런 다음, 앱이 사용하는 각 사용자 지정 클래스에 대해 사용할 각 사용자 지정 클래스에 의해 노출되는 도우미 등록 함수를 참조해야 합니다. `InitializeComponent` 이전에 [**Application constructor**](https://msdn.microsoft.com/library/windows/apps/br242325)(`App::App()`)의 일환으로 각 도우미 등록 함수를 한 번 호출합니다. 이 생성자는 앱이 실제로 처음 참조될 때만 실행되며 예를 들어 일시 중단된 앱이 다시 시작되는 경우 다시 실행되지 않습니다. 또한 이전 C++ 등록 예제에서 본 것처럼, 각 [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) 호출 시 **nullptr** 확인은 함수 호출자가 해당 속성을 두 번 등록할 수 없도록 하므로 매우 중요합니다. 두 번째로 등록 호출이 발생하고 이러한 확인이 이루어지지 않는 경우 속성 이름이 중복 항목이므로 앱이 충돌합니다. C++/CX 버전 샘플의 코드를 원하는 경우 [XAML 사용자 및 사용자 지정 컨트롤 샘플](http://go.microsoft.com/fwlink/p/?linkid=238581)에서 이 구현 패턴을 참조할 수 있습니다.

## <a name="related-topics"></a>관련 항목

- [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)
- [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)
- [종속성 속성 개요](dependency-properties-overview.md)
- [XAML 사용자 및 사용자 지정 컨트롤 샘플](http://go.microsoft.com/fwlink/p/?linkid=238581)
 