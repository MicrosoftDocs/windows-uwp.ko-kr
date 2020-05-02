---
description: C++/WinRT를 사용하면 표준 C++ 와이드 문자열 형식을 사용하여 Windows 런타임 API를 호출하거나, winrt::hstring 형식을 사용할 수 있습니다.
title: C++/WinRT의 문자열 처리
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 문자열
ms.localizationpriority: medium
ms.openlocfilehash: 1771c3754e8e9580514f646ae8589b1982911fc7
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79448568"
---
# <a name="string-handling-in-cwinrt"></a>C++/WinRT의 문자열 처리

[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)에서는 **std::wstring** 같은 C++ 표준 라이브러리 전각 문자열 형식을 사용해 Windows 런타임 API를 호출할 수 있습니다(참고: **std::string** 같은 반각 문자열 형식은 사용할 수 없음). C++/WinRT에는 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)이라고 불리는 사용자 지정 문자열 형식이 없습니다(C++/WinRT 기본 라이브러리인 `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`에 정의됨). 또한 Windows 런타임 생성자, 함수 및 속성이 실제로 가져와 반환하는 문자열 형식이기도 합니다. 하지만 대부분 경우 **hstring**의 변환 생성자와 변환 연산자 덕분에 클라이언트 코드의 **hstring** 인식 여부를 선택할 수 있습니다. API를 직접 ‘작성’하는 경우에는 **hstring**에 대해 알아둘 필요가 더욱 많습니다. 

C++에는 문자열 형식이 많습니다. 또한 C++ 표준 라이브러리의 **std::basic_string** 외에도 변형된 형식이 여러 라이브러리에 존재합니다. C++17에는 다양한 문자열 변환 유틸리티를 비롯해 **std::basic_string_view**도 있기 때문에 모든 문자열 형식의 차이를 좁힐 수 있습니다.  [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)은 **std::basic_string_view**가 디자인된 목적인 상호 운용성을 제공하기 위해 **std::wstring_view**와의 상호 운용성을 제공합니다.

## <a name="using-stdwstring-and-optionally-winrthstring-with-uri"></a>**std::wstring**(선택적으로 **winrt::hstring**)에 **Uri** 사용
[**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri)는 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)에서 생성됩니다.

```cppwinrt
public:
    Uri(winrt::hstring uri) const;
```

하지만 **hstring**에는 [변환 생성자](/uwp/cpp-ref-for-winrt/hstring#hstringhstring-constructor)가 있기 때문에 잘 모르더라도 작업하는 데 문제가 없습니다. 다음은 와이드 문자열 리터럴에서, 와이드 문자열 보기에서, 그리고 **std::wstring**에서 **Uri**를 생성하는 방법을 나타낸 코드 예제입니다.

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <string_view>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    using namespace std::literals;

    winrt::init_apartment();

    // You can make a Uri from a wide string literal.
    Uri contosoUri{ L"http://www.contoso.com" };

    // Or from a wide string view.
    Uri contosoSVUri{ L"http://www.contoso.com"sv };

    // Or from a std::wstring.
    std::wstring wideString{ L"http://www.adventure-works.com" };
    Uri awUri{ wideString };
}
```

속성 접근자인 [**Uri::Domain**](https://docs.microsoft.com/uwp/api/windows.foundation.uri.Domain)은 **hstring** 형식입니다.

```cppwinrt
public:
    winrt::hstring Domain();
```

다시 얘기하지만 **hstring**에는 [**std::wstring_view**로 변환할 수 있는 연산자](/uwp/cpp-ref-for-winrt/hstring#hstringoperator-stdwstring_view)가 있기 때문에 세부 정보를 반드시 알아야 하는 것은 아닙니다.

```cppwinrt
// Access a property of type hstring, via a conversion operator to a standard type.
std::wstring domainWstring{ contosoUri.Domain() }; // L"contoso.com"
domainWstring = awUri.Domain(); // L"adventure-works.com"

// Or, you can choose to keep the hstring unconverted.
hstring domainHstring{ contosoUri.Domain() }; // L"contoso.com"
domainHstring = awUri.Domain(); // L"adventure-works.com"
```

마찬가지로 [**IStringable::ToString**](https://docs.microsoft.com/windows/desktop/api/windows.foundation/nf-windows-foundation-istringable-tostring) 역시 hstring을 반환합니다.

```cppwinrt
public:
    hstring ToString() const;
```

**Uri**는 [**IStringable**](https://docs.microsoft.com/windows/desktop/api/windows.foundation/nn-windows-foundation-istringable) 인터페이스를 구현합니다.

```cppwinrt
// Access hstring's IStringable::ToString, via a conversion operator to a standard type.
std::wstring tostringWstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringWstring = awUri.ToString(); // L"http://www.adventure-works.com/"

// Or you can choose to keep the hstring unconverted.
hstring tostringHstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringHstring = awUri.ToString(); // L"http://www.adventure-works.com/"
```

[**hstring::c_str 함수**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function)를 사용하여 표준 전각 함수를 **hstring**에서 가져올 수 있습니다(**std::wstring**에서 가져오는 방식과 동일).

```cppwinrt
#include <iostream>
std::wcout << tostringHstring.c_str() << std::endl;
```
**hstring**이 있는 경우에는 여기에서 **Uri**를 생성할 수 있습니다.

```cppwinrt
Uri awUriFromHstring{ tostringHstring };
```

**hstring**을 가져오는 메서드를 고려하세요.

```cppwinrt
public:
    Uri CombineUri(winrt::hstring relativeUri) const;
```

앞에서 방금 보았던 옵션들 역시 모두 이러한 경우에 적용됩니다.

```cppwinrt
std::wstring contact{ L"contact" };
contosoUri = contosoUri.CombineUri(contact);
    
std::wcout << contosoUri.ToString().c_str() << std::endl;
```

**hstring**은 멤버로서 **std::wstring_view** 변환 연산자가 있기 때문에 아무런 대가 없이 변환이 이루어집니다.

```cppwinrt
void legacy_print(std::wstring_view view);

void Print(winrt::hstring const& hstring)
{
    legacy_print(hstring);
}
```

## <a name="winrthstring-functions-and-operators"></a>**winrt::hstring** 함수와 연산자
[**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)일 때는 다양한 생성자, 연산자, 함수 및 반복기가 구현됩니다.

**hstring**은 범위이기 때문에 범위 기반 `for`와 함께, 혹은 `std::for_each`와 함께 사용할 수 있습니다. 또한 C++ 표준 라이브러리에서 상응하는 항목을 자연스럽게, 그리고 효율적으로 비교할 수 있는 비교 연산자도 제공합니다. 여기에는 **hstring**을 연결 컨테이너의 키로 사용하는 데 필요한 모든 것이 포함됩니다.

다수의 C++ 라이브러리가 **std::string**을 사용하며, UTF-8 텍스트에서만 유효하다는 사실은 이미 잘 알고 있습니다. 편의상 앞뒤로 변환을 위해 [**winrt::to_string**](/uwp/cpp-ref-for-winrt/to-string) 및 [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring)과 같은 도우미를 제공합니다.

`WINRT_ASSERT`는 매크로 정의이며 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)로 확장됩니다.

```cppwinrt
winrt::hstring w{ L"Hello, World!" };

std::string c = winrt::to_string(w);
WINRT_ASSERT(c == "Hello, World!");

w = winrt::to_hstring(c);
WINRT_ASSERT(w == L"Hello, World!");
```

**hstring** 함수 및 연산자에 대한 자세한 예제와 내용은 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) API 참조 항목을 참조하세요.

## <a name="the-rationale-for-winrthstring-and-winrtparamhstring"></a>**winrt::hstring** 및 **winrt::param::hstring**의 이론적 근거
Windows 런타임은 **wchar_t** 문자와 관련하여 구현되지만 Windows 런타임의 ABI(Application Binary Interface)는 **std::wstring** 또는 **std::wstring_view**가 제공하는 것의 하위 세트가 아닙니다. 따라서 이 둘을 사용하면 상당한 비효율성으로 이어질 수 있습니다. 대신 C++/WinRT는 **winrt::hstring**을 제공합니다. 이는 기본 [HSTRING](https://docs.microsoft.com/windows/desktop/WinRT/hstring)을 따를 뿐만 아니라 **std::wstring** 인터페이스와 비슷한 인터페이스 뒤에서 구현되기 때문에 문자열을 변경할 수 없다는 것을 의미합니다. 

**winrt::hstring**을 논리적으로 허용해야 하는 C++/WinRT 입력 매개 변수에 실제로 **winrt::param::hstring**이 필요하다는 사실을 확인할 수 있습니다. **param** 네임스페이스에는 C++ 표준 라이브러리 형식에 자연스럽게 바인딩하여 복사본과 기타 비효율성을 회피할 수 있도록 입력 매개 변수를 최적화하는 데만 사용되는 형식 세트만 포함됩니다. 이 형식을 직접 사용해서는 안 됩니다. 사용자 고유의 함수에 최적화를 사용하려면 **std::wstring_view**를 사용하세요. [매개 변수를 ABI 경계로 전달](/windows/uwp/cpp-and-winrt-apis/pass-parms-to-abi)도 참조하세요.

결론적으로 Windows 런타임 문자열 관리를 위한 고유 정보는 대부분 무시하고 알고 있는 정보만으로도 효율적으로 작업할 수 있습니다. 문자열이 Windows 런타임에서 얼마나 많이 사용되는지 생각해보면 이는 매우 중요합니다.

## <a name="formatting-strings"></a>문자열 형식 지정
문자열 형식을 지정하는 한 가지 옵션은 **std::wostringstream**입니다. 다음은 간단한 디버그 추적 메시지의 형식을 지정하여 표시하는 예제입니다.

```cppwinrt
#include <sstream>
#include <winrt/Windows.UI.Input.h>
#include <winrt/Windows.UI.Xaml.Input.h>
...
void MainPage::OnPointerPressed(winrt::Windows::UI::Xaml::Input::PointerRoutedEventArgs const& e)
{
    winrt::Windows::Foundation::Point const point{ e.GetCurrentPoint(nullptr).Position() };
    std::wostringstream wostringstream;
    wostringstream << L"Pointer pressed at (" << point.X << L"," << point.Y << L")" << std::endl;
    ::OutputDebugString(wostringstream.str().c_str());
}
```

## <a name="the-correct-way-to-set-a-property"></a>속성을 설정하는 올바른 방법

setter 함수로 값을 전달하여 속성을 설정합니다. 예를 들면 다음과 같습니다.

```cppwinrt
// The right way to set the Text property.
myTextBlock.Text(L"Hello!");
```

아래는 잘못된 코드입니다. 컴파일할 수는 있지만 **Text()** 접근자 함수에서 반환된 임시 **winrt::hstring**을 수정한 다음, 결과를 버리는 것이 전부입니다.

```cppwinrt
// *Not* the right way to set the Text property.
myTextBlock.Text() = L"Hello!";
```

## <a name="important-apis"></a>중요 API
* [winrt::hstring 구조체](/uwp/cpp-ref-for-winrt/hstring)
* [:::: to_hstring 함수](/uwp/cpp-ref-for-winrt/to-hstring)
* [:::: to_string 함수](/uwp/cpp-ref-for-winrt/to-string)
