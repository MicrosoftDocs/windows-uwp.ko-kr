---
description: 스칼라 값은 **IInspectable**이 필요한 함수에 전달되기 전에 참조 클래스 개체 내에 래핑되어야 합니다. 이 래핑 프로세스에 대해 값을 *boxing*한다고 합니다.
title: C++/WinRT를 사용해 스칼라 값을 IInspectable로 boxing 및 unboxing
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, XAML, 컨트롤, boxing, 스칼라, 값
ms.localizationpriority: medium
ms.openlocfilehash: 3c1a64b97b40608e877f18b764ae92835d2bc4a7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154377"
---
# <a name="boxing-and-unboxing-scalar-values-to-iinspectable-with-cwinrt"></a>C++/WinRT를 사용해 스칼라 값을 IInspectable로 boxing 및 unboxing
 
[**IInspectable 인터페이스**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)는 Windows 런타임(WinRT)에서 모든 런타임 클래스의 루트 인터페이스입니다. 이는 [**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown)이 모든 COM 인터페이스 및 클래스의 루트에 위치하고, **System.Object**가 모든 [공용 형식 시스템](/dotnet/standard/base-types/common-type-system) 클래스의 루트에 위치하는 것과 유사합니다.

다시 말해서 **IInspectable**이 필요한 함수에는 모든 런타임 클래스 인스턴스를 전달할 수 있습니다. 하지만 숫자나 텍스트 같은 스칼라 값은 이 함수로 직접 전달할 수 없습니다. 대신 스칼라 값을 참조 클래스 개체 안에 래핑해야 합니다. 이 래핑 프로세스에 대해 값을 *boxing*한다고 합니다.

> [!IMPORTANT]
> Windows 런타임 API에 전달할 수 있는 모든 형식을 boxing 및 unboxing할 수 있습니다. 즉, Windows 런타임 형식입니다. 숫자 및 텍스트 값(문자열)은 위에서 지정된 예제입니다. 또 다른 예제는 IDL에 정의된 `struct`입니다. 일반 C++ `struct`(IDL에 정의되어 있지 않음)를 boxing하려고 하면 컴파일러에서 Windows 런타임 형식만 boxing할 수 있다고 알려줍니다. 런타임 클래스는 Windows 런타임 형식이지만, boxing하지 않고 런타임 API에 전달할 수도 있습니다.

[C++/WinRT](./intro-to-using-cpp-with-winrt.md)는 [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) 함수를 제공합니다. 이 함수는 스칼라 값을 가져와 **IInspectable**에 boxing되는 값을 반환합니다. **IInspectable**을 다시 스칼라 값으로 unboxing하기 위해서 [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) 및 [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 함수가 존재합니다.

## <a name="examples-of-boxing-a-value"></a>값을 boxing하는 예제
[**LaunchActivatedEventArgs::Arguments**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.Arguments) 접근자 함수는 스칼라 값으로 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)을 반환합니다. 이 **hstring** 값을 아래와 같이 boxing하여 **IInspectable**이 필요한 함수에 전달할 수 있습니다.

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    ...
    rootFrame.Navigate(winrt::xaml_typename<BlankApp1::MainPage>(), winrt::box_value(e.Arguments()));
    ...
}
```

XAML [**단추**](/uwp/api/windows.ui.xaml.controls.button)의 콘텐츠 속성을 설정하려면 [**Button::Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content?) 변경자 함수를 호출합니다. 콘텐츠 속성을 문자열 값으로 설정할 때는 이 코드를 사용할 수 있습니다.

```cppwinrt
Button().Content(winrt::box_value(L"Clicked"));
```

먼저 [**hstring**](/uwp/cpp-ref-for-winrt/hstring) 변환 생성자가 문자열 리터럴을 **hstring**으로 변환합니다. 그러면 **hstring**을 가져오는 **winrt::box_value** 오버로드가 호출됩니다.

## <a name="examples-of-unboxing-an-iinspectable"></a>IInspectable을 unboxing하는 예제
**IInspectable**이 필요한 사용자 고유의 함수에서는 [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value)를 사용해 unboxing하거나 [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or)을 사용해 기본값으로 unboxing할 수 있습니다.

```cppwinrt
void Unbox(winrt::Windows::Foundation::IInspectable const& object)
{
    hstring hstringValue = unbox_value<hstring>(object); // Throws if object is not a boxed string.
    hstringValue = unbox_value_or<hstring>(object, L"Default"); // Returns L"Default" if object is not a boxed string.
    float floatValue = unbox_value_or<float>(object, 0.f); // Returns 0.0 if object is not a boxed float.
}
```

## <a name="determine-the-type-of-a-boxed-value"></a>boxing된 값 형식 확인
boxing된 값을 받았으나 이 값에 포함된 형식이 무엇인지 확실하지 않은 경우(unboxing하려면 형식을 알아야 함) 해당 [**IPropertyValue**](/uwp/api/windows.foundation.ipropertyvalue) 인터페이스에 대해 boxing된 값을 쿼리하고 **Type**을 호출할 수 있습니다. 코드 예제는 다음과 같습니다.

`WINRT_ASSERT`는 매크로 정의이며 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)로 확장됩니다.

```cppwinrt
float pi = 3.14f;
auto piInspectable = winrt::box_value(pi);
auto piPropertyValue = piInspectable.as<winrt::Windows::Foundation::IPropertyValue>();
WINRT_ASSERT(piPropertyValue.Type() == winrt::Windows::Foundation::PropertyType::Single);
```

## <a name="important-apis"></a>중요 API
* [IInspectable 인터페이스](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [winrt::box_value 함수 템플릿](/uwp/cpp-ref-for-winrt/box-value)
* [winrt::hstring 구조체](/uwp/cpp-ref-for-winrt/hstring)
* [winrt::unbox_value 함수 템플릿](/uwp/cpp-ref-for-winrt/unbox-value)
* [winrt::unbox_value_or 함수 템플릿](/uwp/cpp-ref-for-winrt/unbox-value-or)