---
description: Windows, 타사 구성 요소 공급업체 또는 사용자 자신이 구현하는지 여부에 관계없이 C++/WinRT API를 사용하는 방법을 보여 줍니다.
title: C++/WinRT를 통한 API 사용
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션된, 프로젝션, 구현, 런타임 클래스, 활성화
ms.localizationpriority: medium
ms.openlocfilehash: 81c8edc65f78de14c1c42611ea1e8d97046128ae
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170367"
---
# <a name="consume-apis-with-cwinrt"></a>C++/WinRT를 통한 API 사용

이번 항목에서는 Windows에 포함되든, 혹은 외부 구성 요소 공급업체나 사용자 자신 중 어디에서 구현되든 상관없이 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) API를 사용하는 방법에 대해서 설명합니다.

## <a name="if-the-api-is-in-a-windows-namespace"></a>API가 Windows 네임스페이스에 있는 경우
이는 Windows 런타임 API를 가장 흔하게 사용하는 경우입니다. 메타데이터에서 정의된 Windows 네임스페이스의 모든 형식에 대해 C++/WinRT는 C++에 편리한 형식(‘프로젝션된 형식’이라고 함)을 정의합니다. 프로젝션된 형식은 Windows 형식과 동일한 정규화된 이름을 가지지만 C++ 구문을 사용하여 C++ **winrt** 네임스페이스에 배치됩니다. 예를 들어 [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri)는 C++/WinRT에 **winrt::Windows::Foundation::Uri**로 프로젝션됩니다.

다음은 간단한 코드 예제입니다. 다음 코드 예제를 복사하여 **Windows 콘솔 애플리케이션(C++/WinRT)** 프로젝트의 주 원본 코드 파일에 직접 붙여넣으려는 경우 먼저 프로젝트 속성에서 **미리 컴파일된 헤더 사용 안 함**을 설정합니다.

```cppwinrt
// main.cpp
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

포함된 헤더인 `winrt/Windows.Foundation.h`는 SDK의 일부로 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\` 폴더 안에 있습니다. 이 폴더의 헤더에는 C++/WinRT에 프로젝션된 Windows 네임스페이스 형식이 포함됩니다. 이번 예제에서 `winrt/Windows.Foundation.h`에는 [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) 런타임 클래스의 프로젝션된 형식인 **winrt::Windows::Foundation::Uri**가 포함됩니다.

> [!TIP]
> Windows 네임스페이스의 형식을 사용할 때마다 해당 네임스페이스와 일치하는 C++/WinRT 헤더를 추가하세요. `using namespace` 지시문은 선택 사항이지만 편리합니다.

위의 코드 예제에서는 C++/WinRT를 초기화한 후 공개적으로 기록되는 생성자(이번 예제에서는 [**Uri(문자열)** ](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_)) 중 하나를 통해 프로젝션된 형식인 **winrt::Windows::Foundation::Uri**의 값을 스택 할당합니다. 이러한 이유로 가장 공통적인 사용 사례이며, 일반적으로 더 이상은 할 것이 없습니다. C++/WinRT 프로젝션된 형식 값이 있으면 모든 동일한 멤버를 가지므로 이를 실제 Windows 런타임 형식의 인스턴스처럼 처리할 수 있습니다.

사실, 그 프로젝션된 값은 프록시입니다. 기본적으로 지원 개체에 대한 스마트 포인터인 것입니다. 프로젝션된 값의 생성자는 [**RoActivateInstance**](/windows/desktop/api/roapi/nf-roapi-roactivateinstance)를 호출하여 Windows 런타임 클래스(이 경우 **Windows.Foundation.Uri**)를 지원하는 인스턴스를 만들고 해당 개체의 기본 인터페이스를 새 프로젝션된 값 내에 저장합니다. 아래에 나타난 것처럼 프로젝션된 값의 멤버에 대한 호출은 실제로 스마트 포인터를 통해 상태 변경이 발생할 위치인 지원하는 개체에 위임합니다.

![프로젝션된 Windows::Foundation::Uri 형식](images/uri.png)

`contosoUri` 값이 범위를 벗어나면 소멸되고 기본 인터페이스에 대한 참조를 릴리스합니다. 해당 참조가 지원하는 Windows 런타임 **Windows.Foundation.Uri** 개체에 대한 마지막 참조인 경우 지원 개체도 소멸할 수 있습니다.

> [!TIP]
> ‘프로젝션된 형식’이란 API를 사용할 목적으로 런타임 클래스에 대한 래퍼를 말합니다. ‘프로젝션된 인터페이스’는 Windows 런타임 인터페이스에 대한 래퍼를 말합니다.

## <a name="cwinrt-projection-headers"></a>C++/WinRT 프로젝션 헤더
C++/WinRT에서 Windows 네임스페이스 API를 사용하려면 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt` 폴더에서 헤더를 포함해야 합니다. 하위 네임스페이스의 형식이 바로 부모 네임스페이스의 형식을 참조하는 것은 일반적입니다. 결과적으로 각 C++/WinRT 프로젝션 헤더에 부모 네임스페이스 헤더 파일이 자동으로 포함되므로, 명시적으로 포함할 ‘필요’가 없습니다. 그러나 포함해도 오류가 발생하지는 않습니다.

예를 들어 [**Windows::Security::Cryptography::Certificates**](/uwp/api/windows.security.cryptography.certificates) 네임스페이스의 경우 해당하는 C++/WinRT 형식 정의는 `winrt/Windows.Security.Cryptography.Certificates.h`에 위치합니다. **Windows::Security::Cryptography::Certificates**는 부모 **Windows::Security::Cryptography** 네임스페이스의 형식을 필요로 하며, 해당 네임스페이스의 형식에는 자체 부모 형식인 **Windows::Security**가 필요할 수 있습니다.

따라서 `winrt/Windows.Security.Cryptography.Certificates.h`를 포함하면 해당 파일이 `winrt/Windows.Security.Cryptography.h`를 포함하고 `winrt/Windows.Security.Cryptography.h`가 `winrt/Windows.Security.h`를 포함합니다. `winrt/Windows.h`가 없기 때문에 여기에서 추적이 중지됩니다. 이 전이적 포함 프로세스는 두 번째 수준 네임스페이스에서 중지됩니다.

이 프로세스는 부모 네임스페이스에 정의된 클래스에 필요한 ‘선언’ 및 ‘구현’을 제공하는 헤더 파일을 전이적으로 포함합니다.

한 네임스페이스에 있는 형식의 멤버는 다른 관련되지 않은 네임스페이스에서 하나 이상의 형식을 참조할 수 있습니다. 컴파일러가 이러한 멤버 정의를 성공적으로 컴파일하려면 컴파일러는 이러한 모든 형식의 클로저에 대한 형식 선언을 볼 필요가 있습니다. 따라서 각 C++/WinRT 프로젝션 헤더는 모든 종속 형식을 ‘선언’하는 데 필요한 네임스페이스 헤더를 포함합니다. 부모 네임스페이스와 달리 이 프로세스는 참조된 형식에 대한 ‘구현’에서 끌어오지 ‘않습니다’.

> [!IMPORTANT]
> 실제로 관련 없는 네임스페이스에서 선언한 형식을 ‘사용’하려는 경우(인스턴스화, 메서드 호출 등) 형식에 대한 적절한 네임스페이스 헤더 파일을 포함해야 합니다. ‘구현’이 아닌 ‘선언’만 자동으로 포함됩니다.

예를 들어 `winrt/Windows.Security.Cryptography.Certificates.h`만 포함하는 경우, 이 네임스페이스(등을 전이적으로)에서 선언을 끌어올 수 있습니다.

- Windows.Foundation
- Windows.Foundation.Collections
- Windows.Networking
- Windows.Storage.Streams
- Windows.Security.Cryptography

즉, 일부 API는 포함한 헤더에서 정방향 선언됩니다. 하지만 그 정의는 아직 포함하지 않은 헤더에 있습니다. 따라서 [**Windows::Foundation::Uri::RawUri**](/uwp/api/windows.foundation.uri.rawuri)를 호출하는 경우 멤버가 정의되지 않았음을 나타내는 링커 오류를 받게 됩니다. 해결 방법은 명시적으로 `#include <winrt/Windows.Foundation.h>`를 추가하는 것입니다. 일반적으로 이와 같은 링커 오류가 표시되는 경우 API 네임스페이스에 대해 명명된 헤더를 포함하고 다시 빌드합니다.

## <a name="accessing-members-via-the-object-via-an-interface-or-via-the-abi"></a>개체, 인터페이스 또는 ABI를 통해 멤버 액세스
C++/WinRT 프로젝션과 관련하여 Windows 런타임 클래스의 런타임 표현은 기본 ABI 인터페이스보다 많지 않습니다. 하지만 편의를 위해 작성자가 의도한 방식으로 클래스에 대해 코딩할 수 있습니다. 예를 들어 클래스의 메서드인 것처럼 [**Uri**](/uwp/api/windows.foundation.uri)의 **ToString** 메서드를 호출할 수 있습니다(사실 내부적으로 개별 **IStringable** 인터페이스의 메서드임).

`WINRT_ASSERT`는 매크로 정의이며 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)로 확장됩니다.

```cppwinrt
Uri contosoUri{ L"http://www.contoso.com" };
WINRT_ASSERT(contosoUri.ToString() == L"http://www.contoso.com/"); // QueryInterface is called at this point.
```

이 편의성은 적절한 인터페이스에 대한 쿼리를 통해 제공됩니다. 하지만 여러분은 항상 이를 제어할 수 있습니다. IStringable을 직접 검색하고 이를 직접 사용하여 약간의 성능을 위해 편의성을 양보할 수 있습니다. 아래 코드 예제에서 일회성 쿼리를 통해 런타임 시 실제 IStringable 인터페이스 포인터를 가져옵니다. 이후 **ToString**에 대한 호출은 직접적이며 **QueryInterface**에 대한 추가 호출을 방지합니다.

```cppwinrt
...
IStringable stringable = contosoUri; // One-off QueryInterface.
WINRT_ASSERT(stringable.ToString() == L"http://www.contoso.com/");
```

동일한 인터페이스에서 여러 메서드를 호출할 것을 알고 있는 경우 이 방법을 선택할 수 있습니다.

참고로, ABI 수준에서 멤버에 액세스하려는 경우 이 작업을 수행할 수 있습니다. 아래의 예제는 방법과 추가 세부 정보, [C++/WinRT와 ABI 사이의 interop](interop-winrt-abi.md)의 코드 예제를 보여 줍니다.

```cppwinrt
#include <Windows.Foundation.h>
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
using namespace winrt::Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };

    int port{ contosoUri.Port() }; // Access the Port "property" accessor via C++/WinRT.

    winrt::com_ptr<ABI::Windows::Foundation::IUriRuntimeClass> abiUri{
        contosoUri.as<ABI::Windows::Foundation::IUriRuntimeClass>() };
    HRESULT hr = abiUri->get_Port(&port); // Access the get_Port ABI function.
}
```

## <a name="delayed-initialization"></a>지연된 초기화

C++/WinRT에서 프로젝션된 각 형식에는 특수 C++/WinRT **std::nullptr_t** 생성자가 있습니다. 이를 제외하고는 모든 프로젝션된 형식 생성자(기본 생성자 포함)에서 지원 Windows 런타임 개체를 만들고 이에 대한 스마트 포인터를 제공합니다. 따라서 이 규칙은 초기화되지 않은 로컬 변수, 초기화되지 않은 글로벌 변수 및 초기화되지 않은 멤버 변수와 같은 기본 생성자가 사용되는 모든 위치에 적용됩니다.

반면, 나중에 작업을 지연시킬 수 있도록 프로젝션된 형식의 변수를 생성하고 이에 따라 지원 Windows 런타임 개체를 구성하지 않으려는 경우 그렇게 할 수 있습니다. 해당 특수 C++/WinRT **std::nullptr_t** 생성자(C++/WinRT 프로젝션에서 모든 런타임 클래스에 주입)를 사용하여 변수 또는 필드를 선언합니다. 아래 코드 예제에서는 *m_gamerPicBuffer*가 있는 특수 생성자를 사용합니다.

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>
using namespace winrt::Windows::Storage::Streams;

#define MAX_IMAGE_SIZE 1024

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

int main()
{
    winrt::init_apartment();
    Sample s;
    // ...
    s.DelayedInit();
}
```

**std::nullptr_t** 생성자를 *제외한* 프로젝션된 형식의 모든 생성자는 지원하는 Windows 런타임 개체를 만들 수 있습니다. **std::nullptr_t** 생성자는 기본적으로 작동하지 않습니다. 다음번에 초기화할 프로젝션된 개체가 필요합니다. 따라서 런타임 클래스에 기본 생성자가 있는지 여부에 상관없이 효율적인 지연된 초기화에 대한 이 방법을 사용할 수 있습니다.

이 고려 사항은 벡터 및 맵과 같은 기본 생성자를 호출하는 다른 위치에 영향을 줍니다. **비어 있는 앱(C++/WinRT)** 프로젝트가 필요한 다음 코드 예제를 살펴봅니다.

```cppwinrt
std::map<int, TextBlock> lookup;
lookup[2] = value;
```

할당은 새 **TextBlock**을 만든 후 즉시 `value`로 덮어씁니다. 해결 방법은 다음과 같습니다.

```cppwinrt
std::map<int, TextBlock> lookup;
lookup.insert_or_assign(2, value);
```

또한 [기본 생성자가 컬렉션에 미치는 영향](./move-to-winrt-from-cx.md#how-the-default-constructor-affects-collections)을 참조하세요.

### <a name="dont-delay-initialize-by-mistake"></a>실수로 지연 초기화하지 않습니다.

실수로 **std::nullptr_t** 생성자를 호출하지 않도록 주의합니다. 컴파일러의 충돌 해결에서는 이 생성자가 팩터리 생성자보다 선호됩니다. 예를 들어, 다음과 같은 두 런타임 클래스 정의를 생각해 보세요.

```idl
// GiftBox.idl
runtimeclass GiftBox
{
    GiftBox();
}

// Gift.idl
runtimeclass Gift
{
    Gift(GiftBox giftBox); // You can create a gift inside a box.
}
```

박스 안에 들어 있지 않은 **Gift**를 생성한다고 가정해 보겠습니다(초기화되지 않은 **GiftBox**를 사용하여 **Gift** 생성). 먼저 *잘못된* 방법을 살펴보겠습니다. **GiftBox**를 사용하는 **Gift** 생성자가 있는 것을 알 수 있습니다. 하지만 null **GiftBox**(아래에서처럼 균일 초기화를 통해 **Gift** 생성자 호출)를 전달하는 경우 원하는 결과를 얻지 *못하게* 됩니다.

```cppwinrt
// These are *not* what you intended. Doing it in one of these two ways
// actually *doesn't* create the intended backing Windows Runtime Gift object;
// only an empty smart pointer.

Gift gift{ nullptr };
auto gift{ Gift(nullptr) };
```

여기서 얻는 것은 초기화되지 않은 **Gift**입니다. 초기화되지 않은 **GiftBox**를 가진 **Gift**는 얻을 수 없습니다. 다음은 *올바른* 방법입니다.

```cppwinrt
// Doing it in one of these two ways creates an initialized
// Gift with an uninitialized GiftBox.

Gift gift{ GiftBox{ nullptr } };
auto gift{ Gift(GiftBox{ nullptr }) };
```

잘못된 예제에서 `nullptr` 리터럴을 전달하여 지연 초기화 생성자 대신 해결합니다. 팩터리 생성자 대신 해결하기 위해서는 매개 변수 형식이 **GiftBox**여야 합니다. 올바른 예제에 나와 있는 것처럼 명시적으로 지연 초기화 **GiftBox**를 전달하는 옵션이 아직 있습니다.

다음 예제 *역시* 올바른 방법으로, 매개 변수에 **std::nullptr_t**가 아닌 GiftBox 형식이 있기 때문입니다.

```cppwinrt
GiftBox giftBox{ nullptr };
Gift gift{ giftBox }; // Calls factory constructor.
```

`nullptr` 리터럴을 전달할 때만 모호성이 발생합니다.

## <a name="dont-copy-construct-by-mistake"></a>실수로 복사 생성하지 않습니다.

이 주의 사항은 위의 [실수로 지연 초기화하지 않습니다.](#dont-delay-initialize-by-mistake) 섹션에 설명된 것과 비슷합니다.

지연 초기화 생성자 외에, C++/WinRT 프로젝션에서는 모든 런타임 클래스에 복사 생성자도 삽입합니다. 생성 중인 개체와 동일한 형식을 허용하는 단일 매개 변수 생성자입니다. 결과 스마트 포인터는 해당 생성자 매개 변수가 가리키는 것과 동일한 지원 Windows 런타임 개체를 가리킵니다. 결과로 동일한 지원 개체를 가리키는 두 개의 스마트 포인터 개체를 얻게 됩니다.

코드 예제에서 사용할 런타임 클래스 정의는 다음과 같습니다.

```idl
// GiftBox.idl
runtimeclass GiftBox
{
    GiftBox(GiftBox biggerBox); // You can place a box inside a bigger box.
}
```

**GiftBox**를 더 큰 **GiftBox** 안에 생성한다고 가정해 보겠습니다.

```cppwinrt
GiftBox bigBox{ ... };

// These are *not* what you intended. Doing it in one of these two ways
// copies bigBox's backing-object-pointer into smallBox.
// The result is that smallBox == bigBox.

GiftBox smallBox{ bigBox };
auto smallBox{ GiftBox(bigBox) };
```

작업을 수행하는 *올바른* 방법은 활성화 팩터리를 명시적으로 호출하는 것입니다.

```cppwinrt
GiftBox bigBox{ ... };

// These two ways call the activation factory explicitly.

GiftBox smallBox{
    winrt::get_activation_factory<GiftBox, IGiftBoxFactory>().CreateInstance(bigBox) };
auto smallBox{
    winrt::get_activation_factory<GiftBox, IGiftBoxFactory>().CreateInstance(bigBox) };
```

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>API가 Windows 런타임 구성 요소에서 구현되는 경우
이번 섹션은 직접 구성 요소를 작성했든, 혹은 공급업체에서 제공했든 상관없이 적용됩니다.

> [!NOTE]
> 프로젝트 템플릿 및 빌드 지원을 함께 제공하는 C++/WinRT Visual Studio 확장(VSIX) 및 NuGet 패키지를 설치하고 사용하는 방법에 대한 자세한 내용은 [Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

애플리케이션 프로젝트에서 Windows 런타임 구성 요소의 Windows 런타임 메타데이터(`.winmd`) 파일을 참조하여 빌드합니다. 빌드 도중 `cppwinrt.exe` 도구가 구성 요소의 API 표면을 완전하게 설명하거나 ‘프로젝션’하는 표준 C++ 라이브러리를 생성합니다. 다시 말해서 생성된 라이브러리에는 구성 요소에 프로젝션된 형식이 포함됩니다.

그런 다음, Windows 네임스페이스 형식과 마찬가지로 헤더를 추가한 다음, 생성자 중 하나를 통해 프로젝션된 형식을 생성합니다. 애플리케이션 프로젝트의 시작 코드가 런타임 클래스를 등록하고, 프로젝션된 형식의 생성자는 [**RoActivateInstance**](/windows/desktop/api/roapi/nf-roapi-roactivateinstance)를 호출하여 참조된 구성 요소의 런타임 클래스를 활성화합니다.

```cppwinrt
#include <winrt/BankAccountWRC.h>

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    ...
};
```

Windows 런타임 구성 요소에서 구현된 API 사용에 대한 자세한 내용과 코드 및 연습은 [C++/WinRT를 사용한 C++/WinRT 런타임 구성 요소](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md) 및 [C++/WinRT의 이벤트 작성](./author-events.md)을 참조하세요.

## <a name="if-the-api-is-implemented-in-the-consuming-project"></a>API가 사용하는 프로젝트에 구현된 경우
XAML UI에서 사용되는 형식은 XAML과 동일한 프로젝트에 있다고 해도 런타임 클래스이어야 합니다.

이러한 시나리오에서는 프로젝션된 형식을 런타임 클래스의 Windows 런타임 메타데이터(`.winmd`)에서 생성합니다. 다시 말해, 헤더를 포함하지만 이번에는 **std::nullptr_t** 생성자를 통해 프로젝션된 형식을 생성합니다. 생성자는 초기화를 수행하지 않기 때문에 다음에는 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 도우미 함수를 통해 값을 인스턴스에 할당하여 필요한 생성자 인수를 전달해야 합니다. 사용하는 코드와 동일한 프로젝트에서 구현되는 런타임 클래스는 등록하거나, 혹은 Windows 런타임/COM 활성화를 통해 인스턴스화할 필요도 없습니다.

이 코드 예제에는 **비어 있는 앱(C++/WinRT)** 프로젝트가 필요합니다.

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
    m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();
    ...
}
```

사용하는 프로젝트에서 구현된 런타임 클래스의 사용에 대한 자세한 내용과 코드, 그리고 연습은 [XAML 컨트롤, C++/WinRT 속성에 바인딩](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)을 참조하세요.

## <a name="instantiating-and-returning-projected-types-and-interfaces"></a>프로젝션된 형식 및 인터페이스의 인스턴스화와 반환
다음은 프로젝션된 형식과 인터페이스가 사용하는 프로젝트에서 어떻게 보이는지 나타낸 예제입니다. 이 예제의 프로젝션된 형식과 같은 프로젝션된 형식은 도구에서 생성되며 직접 작성하지 않는다는 점을 기억하세요.

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
- 프로젝션된 형식을 호출자에 반환할 수 있습니다.
- 프로젝션된 형식과 인터페이스는 [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)에서 파생됩니다. 따라서 프로젝션된 형식 또는 인터페이스에 대해 [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)를 호출하여 다른 프로젝션된 인터페이스에 대한 쿼리를 실행할 수 있으며, 다른 프로젝션된 인터페이스 역시 사용하거나 호출자에 반환할 수 있습니다. **as** 멤버 함수는 [**QueryInterface**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))와 같이 작동합니다.

```cppwinrt
void f(MyProject::MyRuntimeClass const& myrc)
{
    myrc.ToString();
    myrc.Close();
    IClosable iclosable = myrc.as<IClosable>();
    iclosable.Close();
}
```

## <a name="activation-factories"></a>활성화 팩터리
C++/WinRT 개체를 만드는 편리한 직접적인 방법은 다음과 같습니다.

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
CurrencyFormatter currency{ L"USD" };
```

하지만 활성화 팩터리를 직접 만들려는 경우 귀하의 편의에 따라 여기에서 개체를 만듭니다. 여기에 [**winrt::get_activation_factory**](/uwp/cpp-ref-for-winrt/get-activation-factory) 함수 템플릿을 사용하는 방법을 보여 주는 몇 가지 예제가 있습니다.

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

위 두 예제에서 클래스는 Windows 네임스페이스의 형식입니다. 이 예제에서 **BankAccountWRC::BankAccount**는 Windows 런타임 구성 요소에 구현된 사용자 지정 형식입니다.

```cppwinrt
auto factory = winrt::get_activation_factory<BankAccountWRC::BankAccount>();
BankAccountWRC::BankAccount account = factory.ActivateInstance<BankAccountWRC::BankAccount>();
```

## <a name="membertype-ambiguities"></a>멤버/형식 모호성

멤버 함수와 형식의 이름이 같을 경우 모호성이 있습니다. 멤버 함수의 C++ 정규화되지 않은 이름 조회 규칙으로 인해 네임스페이스에서 검색하기 전에 클래스를 검색합니다. *SFINAE*(substitution failure is not an error, 대체 오류는 오류가 아님) 규칙이 적용되지 않습니다(함수 템플릿의 오버로드 확인 중에 적용됨). 따라서 클래스 내의 이름이 적절하지 않으면 컴파일러에서 더 일치하는 항목을 계속 검색하지 않고 단지 오류만 보고할 뿐입니다.

```cppwinrt
struct MyPage : Page
{
    void DoWork()
    {
        // This doesn't compile. You get the error
        // "'winrt::Windows::Foundation::IUnknown::as':
        // no matching overloaded function found".
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Style>() };
    }
}
```

위의 컴파일러는 [**FrameworkElement.Style()** ](/uwp/api/windows.ui.xaml.frameworkelement.style)(C++/WinRT에서는 멤버 함수임)을 템플릿 매개 변수로 [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)에 전달한다고 간주합니다. 해결 방법은 `Style` 이름을 [**Windows::UI::Xaml::Style**](/uwp/api/windows.ui.xaml.style) 형식으로 해석하도록 강제하는 것입니다.

```cppwinrt
struct MyPage : Page
{
    void DoWork()
    {
        // One option is to fully-qualify it.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Windows::UI::Xaml::Style>() };

        // Another is to force it to be interpreted as a struct name.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<struct Style>() };

        // If you have "using namespace Windows::UI;", then this is sufficient.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Xaml::Style>() };

        // Or you can force it to be resolved in the global namespace (into which
        // you imported the Windows::UI::Xaml namespace when you did
        // "using namespace Windows::UI::Xaml;".
        auto style = Application::Current().Resources().
            Lookup(L"MyStyle").as<::Style>();
    }
}
```

이름 뒤에 `::`이 나오는 경우 정규화되지 않은 이름 조회에는 특별한 예외가 있습니다. 이 경우 함수, 변수 및 열거형 값이 무시됩니다. 이렇게 하면 다음과 같은 작업을 수행할 수 있습니다.

```cppwinrt
struct MyPage : Page
{
    void DoSomething()
    {
        Visibility(Visibility::Collapsed); // No ambiguity here (special exception).
    }
}
```

`Visibility()`에 대한 호출이 [**UIElement.Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) 멤버 함수 이름으로 확인됩니다. 그러나 `Visibility::Collapsed` 매개 변수에는 `Visibility` 단어 뒤에 `::`이 있으므로 해당 메서드 이름이 무시되고 컴파일러에서 열거형 클래스를 찾습니다.

## <a name="important-apis"></a>중요 API
* [QueryInterface 인터페이스](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [RoActivateInstance 함수](/windows/desktop/api/roapi/nf-roapi-roactivateinstance)
* [Windows::Foundation::Uri 클래스입니다.](/uwp/api/windows.foundation.uri)
* [winrt::get_activation_factory 함수 템플릿](/uwp/cpp-ref-for-winrt/get-activation-factory)
* [winrt::make 함수 템플릿](/uwp/cpp-ref-for-winrt/make)
* [winrt::Windows::Foundation::IUnknown 구조체](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT의 이벤트 작성](./author-events.md)
* [C++/WinRT와 ABI 사이의 상호 운용성](./interop-winrt-abi.md)
* [C++/WinRT 소개](./intro-to-using-cpp-with-winrt.md)
* [C++/WinRT를 사용한 Windows 런타임 구성 요소](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)
* [XAML 컨트롤(C++/WinRT 속성에 바인딩)](./binding-property.md)