---
author: stevewhims
description: 이번 항목에서는 Windows, 외부 구성 요소 공급업체 또는 사용자 자신 중 어디에서 구현되었든 상관없이 C++/WinRT API를 사용하는 방법에 대해서 설명합니다.
title: C++/WinRT를 통한 API 사용
ms.author: stwhi
ms.date: 04/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션된, 프로젝션, 구현체, 런타임 클래스, 활성화
ms.localizationpriority: medium
ms.openlocfilehash: e777aca8f1d9f3892f67b10c785c056b14c8070c
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832247"
---
# <a name="consume-apis-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)를 통한 API 사용
> [!NOTE]
> **일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.**

이번 항목에서는 Windows에 포함되든, 혹은 외부 구성 요소 공급업체나 사용자 자신 중 어디에서 구현되든 상관없이 C++/WinRT API를 사용하는 방법에 대해서 설명합니다.

## <a name="if-the-api-is-in-a-windows-namespace"></a>API가 Windows 네임스페이스에 있는 경우
이는 Windows 런타임 API를 가장 흔하게 사용하는 경우입니다. 다음은 간단한 코드 예제입니다.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };
}
```

위에 추가된 헤더인 `winrt/Windows.Foundation.h`는 SDK의 일부로 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\` 폴더 안에 있습니다. 이 폴더의 헤더에는 C++/WinRT에 프로젝션된 Windows API가 포함됩니다. Windows 네임스페이스의 형식을 사용할 때마다 해당 네임스페이스와 일치하는 C++/WinRT 프로젝션 헤더를 추가하세요. `using namespace` 지시문은 선택 사항이지만 편리합니다.

이번 예제에서 `winrt/Windows.Foundation.h`에는 [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) 런타임 클래스에 프로젝션되는 형식이 포함됩니다.

> [!TIP]
> *프로젝션된 형식*이란 API를 사용할 목적으로 런타임 클래스를 포장하는 래퍼를 말합니다. *프로젝션된 인터페이스*는 Windows 런타임 인터페이스를 포장하는 래퍼를 말합니다.

위의 코드 예제에서는 C++/WinRT를 초기화한 후 공개적으로 기록되는 생성자(이번 예제에서는 [**Uri(문자열)**](/uwp/api/windows.foundation.uri#Windows_Foundation_Uri__ctor_System_String_)) 중 하나를 통해 프로젝션된 형식인 **Uri**를 생성합니다. 이러한 이유로 가장 공통적인 사용 사례이며, 더 이상은 할 것이 없습니다.

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>API가 Windows 런타임 구성 요소에서 구현되는 경우
이번 섹션은 직접 구성 요소를 작성했든, 혹은 공급업체에서 제공했든 상관없이 적용됩니다.

> [!NOTE]
> C++/WinRT Visual Studio Extension(VSIX)(프로젝트 템플릿 지원과 C++/WinRT MSBuild 속성 및 대상 제공)의 현재 가용성에 대한 자세한 내용은 [C++/WinRT에 대한 Visual Studio 지원 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)를 참조하세요.

응용 프로그램 프로젝트에서 Windows 런타임 구성 요소의 Windows 런타임 메타데이터(`.winmd`) 파일을 참조하여 빌드합니다. 빌드 도중 `cppwinrt.exe` 도구가 구성 요소의 API 표면을 완전하게 설명하거나 *프로젝션*하는 표준 C++ 라이브러리를 생성합니다. 다시 말해서 생성된 라이브러리에는 구성 요소에 프로젝션된 형식이 포함됩니다.

그런 다음 Windows 네임스페이스 형식과 마찬가지로 헤더를 추가한 다음 생성자 중 하나를 통해 프로젝션된 형식을 생성합니다. 응용 프로그램 프로젝트의 시작 코드가 런타임 클래스를 등록하고, 프로젝션된 형식의 생성자는 [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646)를 호출하여 참조된 구성 요소의 런타임 클래스를 활성화합니다.

```cppwinrt
#include <winrt/BankAccountWRC.h>

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    ...
};
```

Windows 런타임 구성 요소에서 구현된 API의 사용에 대한 자세한 내용과 코드, 그리고 연습은 [C++/WinRT의 이벤트 작성](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)을 참조하세요.

## <a name="if-the-api-is-implemented-in-the-consuming-project"></a>API가 사용하는 프로젝트에 구현된 경우
XAML UI에서 사용되는 형식은 XAML과 동일한 프로젝트에 있다고 해도 런타임 클래스이어야 합니다.

이러한 시나리오에서는 프로젝션된 형식을 런타임 클래스의 Windows 런타임 메타데이터(`.winmd`)에서 생성합니다. 한 번 더 헤더를 추가하지만 이번에는 프로젝션된 형식을 `nullptr` 생성자를 통해 생성합니다. 생성자는 초기화를 수행하지 않기 대문에 다음에는 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 도우미 함수를 통해 값을 인스턴스에 할당하여 필요한 생성자 인수를 전달해야 합니다. 사용하는 코드와 동일한 프로젝트에서 구현되는 런타임 클래스는 등록하거나, 혹은 Windows 런타임/COM 활성화를 통해 인스턴스화할 필요도 없습니다.

```cppwinrt
// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
    ...
    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
        ...
    };
}
...
// MainPage.cpp
...
#include "BookstoreViewModel.h"

MainPage::MainPage()
{
    m_mainViewModel = make<Bookstore::implementation::BookstoreViewModel>();
    ...
}
```

사용하는 프로젝트에서 구현된 런타임 클래스의 사용에 대한 자세한 내용과 코드, 그리고 연습은 [XAML 컨트롤, C++/WinRT 속성 바인딩](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)을 참조하세요.

## <a name="instantiating-and-returning-projected-types-and-interfaces"></a>프로젝션된 형식 및 인터페이스의 인스턴스화와 반환
다음은 프로젝션된 형식과 인터페이스가 사용하는 프로젝트에서 어떻게 보이는지 나타낸 예제입니다.

```cppwinrt
struct MyRuntimeClass : MyProject::IMyRuntimeClass, impl::require<MyRuntimeClass,
    Windows::Foundation::IStringable, Windows::Foundation::IClosable>
```

**MyRuntimeClass**는 프로젝션된 형식입니다. 프로젝션된 인터페이스에는 **IMyRuntimeClass**, **IStringable** 및 **IClosable**이 포함됩니다. 이번 항목에서는 프로젝션된 형식을 인스턴스화할 수 있는 여러 가지 방법에 대해서 설명하였습니다. 다음은 **MyRuntimeClass**를 예로 사용한 복습이자 요약입니다.

```cppwinrt
// The runtime class is implemented in another compilation unit (it's either a Windows API,
// or it's implemented in a second- or third-party component).
MyProject::MyRuntimeClass myrc1;

// The runtime class is implemented in the same compilation unit.
MyProject::MyRuntimeClass myrc2{ nullptr };
myrc2 = winrt::make<MyProject::implementation::MyRuntimeClass>();
```

- 프로젝션된 형식의 모든 인터페이스에 속한 멤버에 액세스할 수 있습니다.
- 프로젝션된 형식을 호출자에게 반환할 수 있습니다.
- 프로젝션된 형식과 인터페이스는 [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)에서 파생됩니다. 따라서 프로젝션된 형식 또는 인터페이스에 대해 [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)를 호출하여 다른 프로젝션된 인터페이스에 대한 쿼리를 실행할 수 있으며, 다른 프로젝션된 인터페이스 역시 사용하거나 호출자에게 반환할 수 있습니다. **as** 멤버 함수는 [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521)와 같이 작용합니다.

```cppwinrt
void f(MyProject::MyRuntimeClass const& myrc)
{
    myrc.ToString();
    myrc.Close();
    IClosable iclosable = myrc.as<IClosable>();
    iclosable.Close();
}
```

## <a name="important-apis"></a>중요 API
* [winrt::make 함수 템플릿](/uwp/cpp-ref-for-winrt/make)
* [winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT의 이벤트 작성](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)
* [XAML 컨트롤, C++/WinRT 속성 바인딩](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
