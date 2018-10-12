---
author: stevewhims
description: 이번 항목에서는 Windows, 외부 구성 요소 공급업체 또는 사용자 자신 중 어디에서 구현되었든 상관없이 C++/WinRT API를 사용하는 방법에 대해서 설명합니다.
title: C++/WinRT를 통한 API 사용
ms.author: stwhi
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션된, 프로젝션, 구현체, 런타임 클래스, 활성화
ms.localizationpriority: medium
ms.openlocfilehash: 9b1cd05f974bf9193e84919a5e679ef996746d7e
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4536866"
---
# <a name="consume-apis-with-cwinrt"></a>C++/WinRT를 통한 API 사용

이 항목에서는 사용 하는 방법을 보여 줍니다 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Api의 Windows에 포함 되 든 제 3 자 구성 요소 공급 업체에서 구현한 또는 직접 구현 합니다.

## <a name="if-the-api-is-in-a-windows-namespace"></a>API가 Windows 네임스페이스에 있는 경우
이는 Windows 런타임 API를 가장 흔하게 사용하는 경우입니다. 메타데이터에서 정의된 Windows 네임스페이스의 모든 형식에 대해, C++/WinRT는 C++ 친화적인 등가(*프로젝션된 형식*이라고 함)를 정의합니다. 프로젝션된 형식은 Windows 형식과 동일한 정규화된 이름을 가지지만 C++ 구문을 사용하여 C++ **winrt** 네임스페이스에 배치됩니다. 예를 들어, [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri)는 C++/WinRT에 **winrt::Windows::Foundation::Uri**로 프로젝션됩니다.

다음은 간단한 코드 예제입니다.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };
    Uri combinedUri = contosoUri.CombineUri(L"products");
}
```

위에 추가된 헤더인 `winrt/Windows.Foundation.h`는 SDK의 일부로 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\` 폴더 안에 있습니다. 이 폴더의 헤더에는 C++/WinRT에 프로젝션된 Windows 네임스페이스 형식이 포함됩니다. 이번 예제에서 `winrt/Windows.Foundation.h`에는 [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) 런타임 클래스에 프로젝션되는 형식인 **winrt::Windows::Foundation::Uri**가 포함됩니다.

> [!TIP]
> Windows 네임스페이스의 형식을 사용할 때마다 해당 네임스페이스와 일치하는 C++/WinRT 헤더를 추가하세요. `using namespace` 지시문은 선택 사항이지만 편리합니다.

위의 코드 예제에서는 C++/WinRT를 초기화한 후 공개적으로 기록되는 생성자(이번 예제에서는 [**Uri(문자열)**](/uwp/api/windows.foundation.uri#Windows_Foundation_Uri__ctor_System_String_)) 중 하나를 통해 프로젝션된 형식인 **winrt::Windows::Foundation::Uri**의 값을 스택 할당합니다. 이러한 이유로 가장 공통적인 사용 사례이며, 일반적으로 더 이상은 할 것이 없습니다. C++/WinRT 프로젝션된 형식 값이 있으면 모든 동일한 구성원을 가지므로 이를 실제 Windows 런타임 형식의 인스턴스처럼 처리할 수 있습니다.

사실, 그 프로젝션된 값은 프록시입니다. 기본적으로 지원 개체에 대한 스마트 포인터인 것입니다. 프로젝션된 값은 생성자는 [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646)를 호출하여 Windows 런타임 클래스(이 경우 **Windows.Foundation.Uri**)를 지원하는 인스턴스를 만들고 해당 개체의 기본 인터페이스를 새 프로젝션된 값 내에 저장합니다. 아래에 나타난 것처럼 프로젝션된 값의 구성원에 대한 호출은 실제로 스마트 포인터를 통해 상태 변경이 발생할 위치인 지원하는 개체에 위임합니다.

![프로젝션된 Windows::Foundation::Uri 형식](images/uri.png)

`contosoUri` 값이 범위 밖으로 벗어나면 소멸되고 기본 인터페이스에 대한 참조를 릴리스합니다. 해당 참조가 지원하는 Windows 런타임 **Windows.Foundation.Uri** 개체에 대한 마지막 참조인 경우 지원 개체도 소멸할 수 있습니다.

> [!TIP]
> *프로젝션된 형식*이란 API를 사용할 목적으로 런타임 클래스를 포장하는 래퍼를 말합니다. *프로젝션된 인터페이스*는 Windows 런타임 인터페이스를 포장하는 래퍼를 말합니다.

## <a name="cwinrt-projection-headers"></a>C++/WinRT 프로젝션 헤더
C++/WinRT에서 Windows 네임스페이스 API를 사용하려면 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt` 폴더에서 헤더를 포함해야 합니다. 하위 네임스페이스의 형식이 바로 상위 네임스페이스의 형식을 참조하는 것은 일반적입니다. 따라서 각 C++/WinRT 프로젝션 헤더를 자동으로 상위 네임스페이스 헤더 파일에 포함합니다. 따라서 이를 명시적으로 포함하지 *않아도* 됩니다. 그러나 포함한 경우에도 오류가 발생하지 않습니다.

예를 들어 [**Windows::Security::Cryptography::Certificates**](/uwp/api/windows.security.cryptography.certificates) 네임스페이스의 경우 해당하는 C++/WinRT 형식 정의는 `winrt/Windows.Security.Cryptography.Certificates.h`에 위치합니다. **Windows::Security::Cryptography::Certificates**은 상위 **Windows::Security::Cryptography** 네임스페이스의 형식을 필요로 합니다. 해당 네임스페이스의 형식에는 자체 상위 형식인 **Windows::Security**가 필요할 수 있습니다.

따라서 `winrt/Windows.Security.Cryptography.Certificates.h`를 포함하면 해당 파일이 `winrt/Windows.Security.Cryptography.h`를 포함하고 `winrt/Windows.Security.Cryptography.h`가 `winrt/Windows.Security.h`를 포함합니다. `winrt/Windows.h`가 없기 때문에 여기에서 길이 멈춥니다. 이 전이적 포함 과정은 두 번째 수준 네임스페이스에서 중지됩니다.

이 프로세스는 상위 네임스페이스에 정의된 클래스에 필요한 *선언* 및 *구현*을 제공하는 헤더 파일을 전이적으로 포함합니다.

한 네임스페이스에 있는 형식의 구성원은 다른 관련되지 않은 네임스페이스에서 하나 이상의 형식을 참조할 수 있습니다. 컴파일러가 이러한 구성원 정의를 성공적으로 컴파일하려면 컴파일러는 이러한 모든 형식의 닫힘에 대한 형식 선언을 볼 필요가 있습니다. 따라서 각 C++/WinRT 프로젝션 헤더는 모든 종속 형식을 *선언*하는 데 필요한 네임스페이스 헤더를 포함합니다. 상위 네임스페이스와 달리 이 프로세스는 참조된 형식에 대한 *구현*에서 가져오지 *않습니다*.

> [!IMPORTANT]
> 실제로 관련 없는 네임스페이스에서 선언한 형식을 *사용*하려는 경우(인스턴스화, 메서드 호출 등) 형식에 대한 적절한 네임스페이스 헤더 파일을 포함해야 합니다. *구현*이 아닌 *선언*만 자동으로 포함됩니다.

예를 들어 `winrt/Windows.Security.Cryptography.Certificates.h`만 포함하는 경우, 이러한 네임스페이스(등을 전이적으로)에서 선언을 가져올 수 있습니다.

- Windows.Foundation
- Windows.Foundation.Collections
- Windows.Networking
- Windows.Storage.Streams
- Windows.Security.Cryptography

즉, 일부 API는 포함한 헤더에서 전방 선언됩니다. 하지만 그 정의는 아직 포함하지 않은 헤더에 있습니다. 따라서 [**Windows::Foundation::Uri::RawUri**](/uwp/api/windows.foundation.uri.rawuri)를 호출하는 경우 구성원이 정의되지 않았음을 나타내는 링커 오류를 받게 됩니다. 해결 방법은 명시적으로 `#include <winrt/Windows.Foundation.h>`. 일반적으로 이와 같은 링커 오류가 표시되는 경우 API 네임스페이스에 대해 명명된 헤더를 포함하고 다시 빌드합니다.

## <a name="accessing-members-via-the-object-via-an-interface-or-via-the-abi"></a>개체, 인터페이스, ABI를 통해 구성원 액세스
C++/WinRT 프로젝션과 관련하여 Windows 런타임 클래스의 런타임 표현은 기본 ABI 인터페이스보다 많지 않습니다. 하지만 편의를 위해 작성자가 의도한 방식으로 클래스에 대해 코딩할 수 있습니다. 예를 들어 클래스의 메서드인 것처럼 [**Uri**](/uwp/api/windows.foundation.uri)의 **ToString** 메서드를 호출할 수 있습니다(사실 내부적으로 개별 **IStringable** 인터페이스의 메서드입니다).

```cppwinrt
Uri contosoUri{ L"http://www.contoso.com" };
WINRT_ASSERT(contosoUri.ToString() == L"http://www.contoso.com/"); // QueryInterface is called at this point.
```

이 편리는 적절한 인터페이스에 대한 쿼리를 통해 수행됩니다. 하지만 여러분은 항상 이를 제어할 수 있습니다. IStringable을 직접 검색하고 이를 직접 사용하여 약간의 성능을 위해 편의성을 양보할 수 있습니다. 아래 코드 예제에서 일회성 쿼리를 통해 런타임 시 실제 IStringable 인터페이스 포인터를 가져옵니다. 이후 **ToString**에 대한 호출은 직접적이며 더 이상 **QueryInterface**에 대한 호출을 방지합니다.

```cppwinrt
IStringable stringable = contosoUri; // One-off QueryInterface.
WINRT_ASSERT(stringable.ToString() == L"http://www.contoso.com/");
```

동일한 인터페이스에서 여러 메서드를 호출할 것을 알고 있는 경우 이 방법을 선택할 수 있습니다.

참고로, ABI 수준에서 구성원에 액세스하려는 경우 할 수 있습니다. 아래의 예제는 방법과 추가 세부 정보, [C++/WinRT와 ABI 사이의 상호 운용성](interop-winrt-abi.md)의 코드 예제를 보여 줍니다.

```cppwinrt
int port = contosoUri.Port(); // Access the Port "property" accessor via C++/WinRT.

winrt::com_ptr<ABI::Windows::Foundation::IUriRuntimeClass> abiUri = contosoUri.as<ABI::Windows::Foundation::IUriRuntimeClass>();
HRESULT hr = abiUri->get_Port(&port); // Access the get_Port ABI function.
```

## <a name="delayed-initialization"></a>지연된 초기화
프로젝션된 형식의 기본 생성자도 지원하는 Windows 런타임 개체를 만들 수 있습니다. 런타임 개체 없이 Windows 런타임 개체를 생성하여(나중에 해당 작업을 지연시킬 수 있도록) 프로젝션된 형식의 변수를 생성하려면 그렇게 할 수 있습니다. 프로젝션된 형식의 특수 C++/WinRT `nullptr_t` 생성자를 사용하여 변수 또는 필드를 선언합니다.

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer m_gamerPicBuffer{ nullptr };
};
```

`nullptr_t` 생성자를 *제외한* 프로젝션된 형식의 모든 생성자는 지원하는 Windows 런타임 개체를 만들 수 있습니다. `nullptr_t` 생성자는 기본적으로 작동하지 않습니다. 다음 번에 초기화할 프로젝션된 개체를 필요로 합니다. 따라서 런타임 클래스에 기본 생성자가 있는지 여부에 상관없이 효율적인 지연된 초기화에 대한 이 방법을 사용할 수 있습니다.

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>API가 Windows 런타임 구성 요소에서 구현되는 경우
이번 섹션은 직접 구성 요소를 작성했든, 혹은 공급업체에서 제공했든 상관없이 적용됩니다.

> [!NOTE]
> C++/WinRT Visual Studio Extension(VSIX)(프로젝트 템플릿 지원과 C++/WinRT MSBuild 속성 및 대상 제공)의 설치 및 사용에 대한 자세한 내용은 [C++/WinRT에 대한 Visual Studio 지원 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)를 참조하세요.

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
- 프로젝션된 형식과 인터페이스는 [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)에서 파생됩니다. 따라서 프로젝션된 형식 또는 인터페이스에 대해 [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)를 호출하여 다른 프로젝션된 인터페이스에 대한 쿼리를 실행할 수 있으며, 다른 프로젝션된 인터페이스 역시 사용하거나 호출자에게 반환할 수 있습니다. **as** 구성원 함수는 [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521)와 같이 작용합니다.

```cppwinrt
void f(MyProject::MyRuntimeClass const& myrc)
{
    myrc.ToString();
    myrc.Close();
    IClosable iclosable = myrc.as<IClosable>();
    iclosable.Close();
}
```

## <a name="activation-factories"></a>정품 인증 공장
C++/WinRT 개체를 만드는 편리한 직접적인 방법은 다음과 같습니다.

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
CurrencyFormatter currency{ L"USD" };
```

But there may be times that you'll want to create the activation factory yourself, and then create objects from it at your convenience. 여기에 [**winrt::get_activation_factory**](/uwp/cpp-ref-for-winrt/get-activation-factory) 함수 템플릿을 사용하는 방법을 보여 주는 몇 가지 예가 있습니다.

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
auto factory = winrt::get_activation_factory<CurrencyFormatter, ICurrencyFormatterFactory>();
CurrencyFormatter currency = factory.CreateCurrencyFormatterCode(L"USD");
```

```cppwinrt
using namespace winrt::Windows::Foundation;
...
auto factory = winrt::get_activation_factory<Uri, IUriRuntimeClassFactory>();
Uri account = factory.CreateUri(L"http://www.contoso.com");
```

위 두 예제에서 클래스는 Windows 네임스페이스의 형식입니다. 이 예에서 **BankAccountWRC::BankAccount**는 Windows 런타임 구성 요소에 구현된 사용자 지정 형식입니다.

```cppwinrt
auto factory = winrt::get_activation_factory<BankAccountWRC::BankAccount>();
BankAccountWRC::BankAccount account = factory.ActivateInstance<BankAccountWRC::BankAccount>();
```

## <a name="important-apis"></a>중요 API
* [QueryInterface 인터페이스](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [RoActivateInstance 함수](https://msdn.microsoft.com/library/br224646)
* [Windows::Foundation::Uri 클래스](/uwp/api/windows.foundation.uri)
* [winrt::get_activation_factory 함수 템플릿](/uwp/cpp-ref-for-winrt/get-activation-factory)
* [winrt::make 함수 템플릿](/uwp/cpp-ref-for-winrt/make)
* [winrt::Windows::Foundation::IUnknown 구조체](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT의 이벤트 작성](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)
* [C++/WinRT와 ABI 사이의 상호 운용성](interop-winrt-abi.md)
* [C++/WinRT 소개](intro-to-using-cpp-with-winrt.md)
* [XAML 컨트롤, C++/WinRT 속성 바인딩](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
