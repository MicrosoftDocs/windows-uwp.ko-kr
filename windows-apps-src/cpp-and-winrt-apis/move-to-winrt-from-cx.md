---
description: 이 항목은 C++/CX 코드를 C++/WinRT의 해당 코드에 포트하는 방법을 보여 줍니다.
title: C++/CX에서 C++/WinRT로 이동
ms.date: 10/18/2018
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 이식, 마이그레이션, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 5a6a778f1efe16d56c24e437a0c25a8b8c5e3bc7
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8927238"
---
# <a name="move-to-cwinrt-from-ccx"></a>C++/CX에서 C++/WinRT로 이동

이 항목에서는 코드에 포트 하는 방법을 보여 줍니다.는 [C + + CX](/cpp/cppcx/visual-c-language-reference-c-cx) 에 해당 하는 프로젝트 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="porting-strategies"></a>포팅 전략

하려면 점진적으로 포트 C + + /CX 코드를 C + + /winrt를 할 수 있습니다. C + + CX 및 C + + /winrt 코드는 XAML 컴파일러를 지원 하 고 Windows 런타임 구성 요소를 제외 하 고 동일한 프로젝트에 공존할 수 있습니다. 이러한 두 가지 예외에 대 한 C + 중 하나를 대상으로 해야 + /CX 또는 C + + 동일한 프로젝트에서 WinRT 합니다.

> [!IMPORTANT]
> 프로젝트 빌드 XAML 응용 프로그램을 다음 하나의 워크플로 권장 되는 먼저 C + 중 하나를 사용 하 여 Visual Studio에서 새 프로젝트를 만들려면 + /winrt 프로젝트 템플릿과 (참조 [Visual Studio 지원 C + + /winrt 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)). 그런 다음에서 C + 소스 코드 및 태그를 통해 복사를 시작 + CX 프로젝트입니다. **프로젝트**를 사용 하 여 새 XAML 페이지를 추가할 수 있습니다 \> **새 항목 추가**  \>  **Visual c + +** > **페이지를 비어 있는 템플릿 (C + + WinRT)**.
>
> 또는 인수 코드에서 XAML C + Windows 런타임 구성 요소를 사용할 수 + CX 프로젝트 포팅 것 처럼 합니다. 이동 많은 C + + CX 코드는 구성 요소에 하 고 다음 변경 XAML 프로젝트 C + + WinRT 합니다. 또는 다른 XAML 프로젝트 C + + /CX 만드는 새로운 C + + WinRT 구성 요소 시작 포팅 C + + /CX 코드는 XAML 프로젝트를 구성 합니다. 있을 수는 C + + CX 구성 요소 프로젝트와 함께 C + + 같은 솔루션에서 WinRT 구성 요소 프로젝트 응용 프로그램 프로젝트에서 둘 다를 참조 하 고 다른 하나에서 점진적으로 포트입니다. 참조 [Interop 간에 C + + /winrt와 C + + CX](interop-winrt-cx.md) 두 언어 프로젝션을 사용 하 여 동일한 프로젝트에 대 한 자세한 내용은 합니다.

> [!NOTE]
> [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)와 Windows SDK 모두 루트 네임스페이스인 **Windows**에 유형을 선언합니다. C++/WinRT에 프로젝션된 Windows 유형은 Windows 유형과 동일한 정규화된 이름을 가지지만 C++ **winrt** 네임스페이스에 배치됩니다. 이처럼 서로 다른 네임스페이스를 사용하면 사용자가 원하는 대로 C++/CX에서 C++/WinRT로 포트할 수 있습니다.

위에서 언급 한 예외 염두에 첫 번째 단계에서 포팅하는 C + + CX 프로젝트를 C + + /winrt는 수동으로 추가 C + + /winrt 지원을 (참조 [Visual Studio 지원 C + + /winrt 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)). 이를 수행하려면 `.vcxproj` 파일을 편집하고 `<PropertyGroup Label="Globals">`를 찾고, 해당 속성 그룹 내에서 `<CppWinRTEnabled>true</CppWinRTEnabled>` 속성을 설정합니다. 해당 변경의 효과 중 하나는 프로젝트에서 C++/CX에 대한 지원이 꺼진다는 것입니다. 지원을 끈 채로 유지 찾기 (및 포트) 빌드 메시지 도움말 수 있도록 모든 종속성에 C + 하는 것이 좋습니다 + /CX 하거나 수 지원을 다시 켜고 (프로젝트 속성에서 **C/c + +** \> **일반** \> **사용 Windows 런타임 확장** \> **예 (/ZW)**)를 점진적으로 포트입니다.

**일반**프로젝트 속성을 확인 \> **대상 플랫폼 버전** 10.0.17134.0 (Windows 10, 버전 1803)로 설정 되어 이상.

컴파일된 헤더 파일에(일반적으로 `pch.h`) `winrt/base.h`를 포함합니다.

```cppwinrt
#include <winrt/base.h>
```

C++/WinRT 프로젝션된 Windows API 헤더를 포함하는 경우(예를 들어, `winrt/Windows.Foundation.h`) 자동으로 포함되기 때문에 명시적으로 이와 같은 `winrt/base.h`를 포함할 필요가 없습니다.

프로젝트가 [Windows 런타임 C++ 템플릿 라이브러리(WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 유형도 사용하는 경우 [WRL에서 C++/WinRT로 이동](move-to-winrt-from-wrl.md)을 참조합니다.

## <a name="parameter-passing"></a>매개 변수-전달
C++/CX 소스 코드를 쓸 때 C++/CX 유형을 hat(\^) 참조로 함수 매개 변수로 전달합니다.

```cpp
void LogPresenceRecord(PresenceRecord^ record);
```

C++/WinRT에서 동기화 함수를 위해 기본적으로 `const&` 매개 변수를 사용해야 합니다. 이를 통해 복사본과 연동된 오버헤드를 방지할 수 있습니다. 하지만 사용자 코루틴이 값으로 캡처하고 수명 문제를 방지할 수 있도록 값으로 전달을 사용해야 합니다(자세한 내용은 [C++/WinRT 동시성 및 비동기 작업](concurrency.md) 참조).

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

C++/WinRT 개체는 기본적으로 지원 Windows 런타임 개체에 대한 인터페이스 포인터를 보유하는 값입니다. C++/WinRT 개체를 복사할 때 컴파일러는 캡슐화된 인터페이스 포인터를 복사하여 참조 횟수가 증가합니다. 복사의 최종 소멸은 감소 참조 수를 포함합니다. 따라서 필요한 경우에만 오버헤드가 복사됩니다.

## <a name="variable-and-field-references"></a>변수 및 필드 참조
C++/CX 소스 코드를 쓸 때 hat(\^) 변수를 사용하여 Windows 런타임 개체를 참조하고, 화살표(-&gt;) 연산자를 사용하여 hat 변수를 역참조합니다.

```cpp
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

이식 해당 C + + /winrt 코드 기본적으로 hat을 제거 하 고 화살표 연산자를 변경 (-&gt;)를 점 연산자 (.) 때문에 C + + /winrt 프로젝션 된 형식은 값 및 포인터가 아닌 합니다.

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

## <a name="properties"></a>속성
C++/CX 언어 확장은 속성의 개념을 포함합니다. C++/CX 소스 코드를 쓸 때 필드처럼 속성에 액세스할 수 있습니다. 표준 C++는 속성의 개념을 가지지 않으므로 C++/WinRT에서 get 및 set 함수를 호출합니다.

다음 예에서 **XboxUserId**, **UserState**, **PresenceDeviceRecords** 및 **Size**는 모두 속성입니다.

### <a name="retrieving-a-value-from-a-property"></a>속성에서 값 검색
여기서 C++/CX에서 속성 값을 가져오는 방법을 설명합니다.

```cpp
void Sample::LogPresenceRecord(PresenceRecord^ record)
{
    auto id = record->XboxUserId;
    auto state = record->UserState;
    auto size = record->PresenceDeviceRecords->Size;
}
```

해당 C++/WinRT 소스는 매개 변수 없이 속성과 동일한 이름의 함수를 호출합니다.

```cppwinrt
void Sample::LogPresenceRecord(PresenceRecord const& record)
{
    auto id = record.XboxUserId();
    auto state = record.UserState();
    auto size = record.PresenceDeviceRecords().Size();
}
```

**PresenceDeviceRecords** 함수는 자체에 **크기** 함수를 가지는 Windows 런타임 개체를 반환합니다. 반환된 개체가 C++/WinRT 프로젝션된 형식이기도 하므로 점 연산자를 사용하여 역참조하여 **Size**를 호출합니다.

### <a name="setting-a-property-to-a-new-value"></a>속성을 새 값으로 설정
속성을 새 값으로 설정하는 것은 비슷한 패턴을 따릅니다. 먼저 C++/CX에서.

```cpp
record->UserState = newValue;
```

C++/WinRT에서 해당하는 작업을 수행하려면, 속성과 동일한 이름으로 함수를 호출하고 인수를 전달합니다.

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>클래스 인스턴스 만들기
일반적으로 hat(\^) 참조로 알려진 이에 대한 핸들을 통해 C++/CX 개체를 작업합니다. `ref new` 키워드를 통해 새로운 개체를 만들면 [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646)를 호출하여 런타임 클래스의 새로운 인스턴스를 활성화합니다.

```cpp
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

C++/WinRT 개체는 값입니다. 따라서 스택에 또는 개체의 필드로 이를 할당할 수 있습니다. C++/WinRT 개체를 할당하기 위해 *절대* `ref new`(또는 `new`)를 사용해서는 안 됩니다. 백그라운드에서 **RoActivateInstance**는 여전히 호출되고 있습니다.

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

리소스가 초기화하기에 비싼 경우 초기화가 실제로 필요할 때까지 초기화를 지연하는 것이 일반적입니다.

```cpp
using namespace Windows::Storage::Streams;

class Sample
{
public:
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer^ m_gamerPicBuffer;
};
```

C++/WinRT로 포트된 동일한 코드. `nullptr` 생성자의 사용에 주의합니다. 해당 생성자에 대한 자세한 내용은 [C++/WinRT를 통한 API 사용](consume-apis.md)을 참조합니다.

```cppwinrt
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

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>기본 런타임 클래스에서 파생 된 버전을 변환
하는 참조를-기반 파생 형식의 개체 참조를 알고 있는 괜찮습니다. C + + /CX를 사용 하면 `dynamic_cast` 에 *캐스트* 자료를 참조는 참조-파생 클래스에 있습니다. `dynamic_cast` [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521)실제로 숨겨진된 호출 됩니다. 다음은 일반적인 예제&mdash;종속성 속성 변경 이벤트를 처리 하 고 종속성 속성을 소유 하는 실제 형식으로 다시 **DependencyObject** 에서 캐스팅 하 고 싶은 합니다.

```cpp
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

해당 C + + /winrt 코드를 대체 합니다 `dynamic_cast` [**Try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function) 함수를 호출 하 여 사용 중인 **QueryInterface**캡슐화 합니다. 필요한 인터페이스 (요청 하 고 형식의 기본 인터페이스)에 대 한 쿼리를 반환 하지는 예외를 throw 하는 대신 [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)호출 하는 옵션이 있습니다. 다음은 C + + WinRT 코드 예제입니다.

```cppwinrt
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // succeeded ...
    }

    try
    {
        BgLabelControlApp::BgLabelControl theControl{ d.as<BgLabelControlApp::BgLabelControl>() };
        // succeeded ...
    }
    catch (winrt::hresult_no_interface const&)
    {
        // failed ...
    }
}
```

## <a name="event-handling-with-a-delegate"></a>대리인으로 이벤트 처리
여기에 lambda 함수를 대리인으로 사용하여 C++/CX에서 이벤트를 처리하는 일반적인 예가 나와 있습니다.

```cpp
auto token = myButton->Click += ref new RoutedEventHandler([&](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
});
```

C++/WinRT에서와 동일합니다.

```cppwinrt
auto token = myButton().Click([&](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
});
```

lambda 함수 대신 대리인을 무료 함수 또는 포인터-회원-함수로 구현할 수 있습니다. 자세한 정보는 [C++/WinRT의 대리자를 사용한 이벤트 처리](handle-events.md)를 참조하세요.

이벤트 및 대리인이 내부적으로 사용되는(이진 전체에서가 아니라) C++/CX 코드 베이스에서 포트하는 경우 [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate)은 C++/WinRT에서 해당 패턴을 복제하는 데 도움이 됩니다. 또한 [매개 변수화 된 대리자, 간단한 신호, 및 프로젝트 내에서 콜백을](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project)참조 하십시오.

## <a name="revoking-a-delegate"></a>대리인 취소
C++/CX에서`-=` 연산자를 사용하여 이전 이벤트 등록을 취소합니다.

```cpp
myButton->Click -= token;
```

C++/WinRT에서와 동일합니다.

```cppwinrt
myButton().Click(token);
```

자세한 정보 및 옵션은 [등록된 대리인 취소](handle-events.md#revoke-a-registered-delegate)를 참조합니다.

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>C++/WinRT 유형으로 C++/CX **플랫폼** 유형 매핑
C++/CX는 **플랫폼** 네임스페이스에서 몇 가지 데이터 형식을 제공합니다. 이러한 유형은 표준 C++가 아니므로 Windows 런타임 언어 확장을 활성화한 경우에만 이를 사용할 수 있습니다(Visual Studio 프로젝트 속성 **C/C++** > **일반** > **Windows 런타임 확장 사용** > **예(/ZW)**). 아래 표는 **플랫폼** 유형에서 C++/WinRT의 해당 유형으로 포트하는 데 도움이 됩니다. 이를 수행하면 C++/WinRT가 표준 C++이이기 때문에 `/ZW` 옵션을 끌 수 있습니다.

| C++/CX | C++/WinRT |
| ---- | ---- |
| **플랫폼:: Agile\ ^** | [**winrt:: agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagileref"></a>포트 **플랫폼:: Agile\ ^** **winrt:: agile_ref** 를
**플랫폼:: Agile\ ^** 형식 C + + CX 모든 스레드에서 액세스할 수 있는 Windows 런타임 클래스를 나타냅니다. C + + WinRT 등가 [**winrt:: agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref)합니다.

C++/CX에서.

```cpp
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

C++/WinRT에서.

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformexception-to-winrthresulterror"></a>**Platform::Exception\^** 을 **winrt::hresult_error**로 포트
Windows 런타임 API가 비- S\_OK HRESULT를 반환하면 **Platform::Exception\^** 유형이 C++/CX에서 생성됩니다. C++/WinRT에 대한 등가는 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)입니다.

C++/WinRT로 포트하려면 **Platform::Exception\^** 를 사용하는 모든 코드를 **winrt::hresult_error**를 사용하도록 변경합니다.

C++/CX에서.

```cpp
catch (Platform::Exception^ ex)
```

C++/WinRT에서.

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT는 이러한 예외 클래스를 제공합니다.

| 예외 유형 | 기본 클래스 | HRESULT |
| ---- | ---- | ---- |
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function) 호출 |
| [**winrt::hresult_access_denied**](/uwp/cpp-ref-for-winrt/error-handling/hresult-access-denied) | **winrt::hresult_error** | E_ACCESSDENIED |
| [**winrt::hresult_canceled**](/uwp/cpp-ref-for-winrt/error-handling/hresult-canceled) | **winrt::hresult_error** | ERROR_CANCELLED |
| [**winrt::hresult_changed_state**](/uwp/cpp-ref-for-winrt/error-handling/hresult-changed-state) | **winrt::hresult_error** | E_CHANGED_STATE |
| [**winrt::hresult_class_not_available**](/uwp/cpp-ref-for-winrt/error-handling/hresult-class-not-available) | **winrt::hresult_error** | CLASS_E_CLASSNOTAVAILABLE |
| [**winrt::hresult_illegal_delegate_assignment**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-delegate-assignment) | **winrt::hresult_error** | E_ILLEGAL_DELEGATE_ASSIGNMENT |
| [**winrt::hresult_illegal_method_call**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-method-call) | **winrt::hresult_error** | E_ILLEGAL_METHOD_CALL |
| [**winrt::hresult_illegal_state_change**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-state-change) | **winrt::hresult_error** | E_ILLEGAL_STATE_CHANGE |
| [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) | **winrt::hresult_error** | E_INVALIDARG |
| [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) | **winrt::hresult_error** | E_NOINTERFACE |
| [**winrt::hresult_not_implemented**](/uwp/cpp-ref-for-winrt/error-handling/hresult-not-implemented) | **winrt::hresult_error** | E_NOTIMPL |
| [**winrt::hresult_out_of_bounds**](/uwp/cpp-ref-for-winrt/error-handling/hresult-out-of-bounds) | **winrt::hresult_error** | E_BOUNDS |
| [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) | **winrt::hresult_error** | RPC_E_WRONG_THREAD |

각 클래스(**hresult_error** 기본 클래스를 통해)는 오류의 HRESULT를 반환하는 [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function) 함수와 해당 HRESULT의 문자열 표현을 반환하는 [**메시지**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrormessage-function) 함수를 제공합니다.

C++/CX에서 예외를 throw하는 예는 다음과 같습니다.

```cpp
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

그리고 C++/WinRT의 등가.

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>**Platform::Object\^** 를 **winrt::Windows::Foundation::IInspectable**로 포트
모든 C++/WinRT 유형과 마찬가지로, **winrt::Windows::Foundation::IInspectable**은 값 유형입니다. 해당 유형의 변수를 null로 초기화하는 방법입니다.

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>**Platform::String\^** 을 **winrt::hstring**으로 포트
**Platform::String\^** 은 Windows 런타임 HSTRING ABI 형식에 해당합니다. C++/WinRT에 대한 등가는 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)입니다. 그러나 C++/WinRT에서는 **std::wstring** 같은 C++ 표준 라이브러리 전각 문자열 형식 및/또는 전각 문자열 리터럴을 사용해 Windows 런타임 API를 호출할 수 있습니다. 자세한 내용과 코드 예제는 [C++/WinRT의 문자열 처리](strings.md)를 참조하세요.

C++/CX에서 [**Platform::String::Data**](https://docs.microsoft.com/en-us/cpp/cppcx/platform-string-class#data) 속성에 액세스하여 문자열을 C-스타일 **const wchar_t\*** 배열로 검색할 수 있습니다(예를 들면 이를 **std::wcout**에 전달하여).

```C++
auto var = titleRecord->TitleName->Data();
```

C++/WinRT에서 같은 작업을 수행하려면 [**hstring::c_str**](/uwp/api/windows.foundation.uri#hstringcstr-function) 함수를 이용하여 **std::wstring**에서와 마찬가지로 null 종료 C-스타일 문자열 버전을 가져옵니다.

```C++
auto var = titleRecord.TitleName().c_str();
```

문자열을 가지거나 반환하는 API 구현에서는 일반적으로 **Platform::String\^** 을 사용하는 모든 C++/CX 코드를 변경하여 **winrt::hstring**을 대신 사용합니다.

문자열을 가지는 C++/CX의 예는 다음과 같습니다.

```cpp
void LogWrapLine(Platform::String^ str);
```

C++/WinRT의 경우 이처럼 [MIDL 3.0](/uwp/midl-3)에서 API를 선언할 수 있습니다.

```idl
// LogType.idl
void LogWrapLine(String str);
```

C++/WinRT 도구 체인이 다음과 같이 사용자를 위해 소스 코드를 생성합니다.

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

## <a name="important-apis"></a>중요 API
* [winrt::delegate 구조 템플릿](/uwp/cpp-ref-for-winrt/delegate)
* [winrt::hresult_error 구조](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::hstring 구조](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 네임스페이스](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>관련 항목
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT의 이벤트 작성](author-events.md)
* [C++/WinRT로 동시성 및 비동기 작업](concurrency.md)
* [C++/WinRT를 통한 API 사용](consume-apis.md)
* [C++/WinRT의 대리자를 사용한 이벤트 처리](handle-events.md)
* [C++/WinRT와 C++/CX 사이의 상호 운용성](interop-winrt-cx.md)
* [Microsoft 인터페이스 정의 언어 3.0 참조](/uwp/midl-3)
* [WRL에서 C++/WinRT로 이동](move-to-winrt-from-wrl.md)
* [C++/WinRT의 문자열 처리](strings.md)
