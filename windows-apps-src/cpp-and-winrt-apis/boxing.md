---
description: 스칼라 값은 **IInspectable**이 필요한 함수로 전달되기 전에 참조 클래스 개체 내에서 래핑되어야 합니다. 이러한 래핑 프로세스를 두고 값을 *박싱(boxing)* 한다고 합니다.
title: C++/WinRT를 사용해 스칼라 값을 IInspectable로 박싱(boxing) 및 언박싱(unboxing)
ms.date: 04/10/2018
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, XAML, 컨트롤, 박싱, 스칼라, 값
ms.localizationpriority: medium
ms.openlocfilehash: 5c86d1ac8ce83ea092ce0e2730ea0e9d4a201b94
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7993279"
---
# <a name="boxing-and-unboxing-scalar-values-to-iinspectable-with-cwinrt"></a>C++/WinRT를 사용해 스칼라 값을 IInspectable로 박싱(boxing) 및 언박싱(unboxing)
 
[**IInspectable 인터페이스**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)는 Windows 런타임(WinRT)에서 모든 런타임 클래스의 루트 인터페이스입니다. 이는 [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)이 모든 COM 인터페이스 및 클래스의 루트에 위치하고, **System.Object**가 모든 [공용 형식 시스템](https://docs.microsoft.com/dotnet/standard/base-types/common-type-system) 클래스의 루트에 위치하는 것과 유사합니다.

다시 말해서 **IInspectable**이 필요한 함수에게는 모든 런타임 클래스 인스터스를 전달할 수 있습니다. 하지만 숫자나 텍스트 같은 스칼라 값은 이러한 함수로 직접 전달할 수 없습니다. 대신 스칼라 값을 참조 클래스 개체 안에 래핑해야 합니다. 이러한 래핑 프로세스를 두고 값을 *박싱(boxing)* 한다고 합니다.

[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 는 스칼라 값을 **IInspectable**로 박싱 되는 값을 반환 하는 [**winrt:: box_value**](/uwp/cpp-ref-for-winrt/box-value) 함수를 제공 합니다. **IInspectable**을 다시 스칼라 값으로 언박싱하기 위해서 [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) 및 [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 함수가 존재합니다.

## <a name="examples-of-boxing-a-value"></a>값을 박싱하는 예제
[**LaunchActivatedEventArgs::Arguments**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.Arguments) 접근자 함수는 스칼라 값으로 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)을 반환합니다. 이 **hstring** 값은 아래와 같이 박싱을 통해 **IInspectable**이 필요한 함수에게 전달할 수 있습니다.

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    ...
    rootFrame.Navigate(winrt::xaml_typename<BlankApp1::MainPage>(), winrt::box_value(e.Arguments()));
    ...
}
```

XAML [**버튼**](/uwp/api/windows.ui.xaml.controls.button)의 내용 속성을 설정하려면 [**Button::Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content?) 변경자 함수를 호출합니다. 내용 속성을 문자열 값으로 설정할 때는 이 코드를 사용할 수 있습니다.

```cppwinrt
Button().Content(winrt::box_value(L"Clicked"));
```

먼저 [**hstring**](/uwp/cpp-ref-for-winrt/hstring) 변환 생성자가 문자열 리터럴을 **hstring**으로 변환합니다. 그러면 **hstring**을 가져오는 **winrt::box_value** 오버로드가 호출됩니다.

## <a name="examples-of-unboxing-an-iinspectable"></a>IInspectable을 언박싱하는 예제
**IInspectable**이 필요한 사용자 고유의 함수에서는 [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value)를 사용해 언박싱하거나 [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or)를 사용해 기본 값으로 언박싱할 수 있습니다.

```cppwinrt
void Unbox(winrt::Windows::Foundation::IInspectable const& object)
{
    hstring hstringValue = unbox_value<hstring>(object); // Throws if object is not a boxed string.
    hstringValue = unbox_value_or<hstring>(object, L"Default"); // Returns L"Default" if object is not a boxed string.
    float floatValue = unbox_value_or<float>(object, 0.f); // Returns 0.0 if object is not a boxed float.
}
```

## <a name="determine-the-type-of-a-boxed-value"></a>박싱된 값 유형 확인
박싱된 값을 받고 여기에 포함된 유형이 무엇인지 확실하지 않은 경우(언박싱하려면 유형을 알아야 함) 해당 [**IPropertyValue**](/uwp/api/windows.foundation.ipropertyvalue) 인터페이스에 대해 박싱된 값을 쿼리한 다음 거기에서 **Type**을 호출할 수 있습니다. 코드 예제는 다음과 같습니다.

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
