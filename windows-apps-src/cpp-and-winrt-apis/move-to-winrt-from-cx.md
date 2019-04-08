---
description: 이 항목은 C++/CX 코드를 C++/WinRT의 해당 코드에 포트하는 방법을 보여 줍니다.
title: C++/CX에서 C++/WinRT로 이동
ms.date: 01/17/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 이식, 마이그레이션, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: fe988bffbf024308fb5d43da7ed538e5330b58de
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635078"
---
# <a name="move-to-cwinrt-from-ccx"></a>C++/CX에서 C++/WinRT로 이동

이 항목에서는의 코드를 이식 하는 방법을 보여 줍니다.는 [C + + /cli CX](/cpp/cppcx/visual-c-language-reference-c-cx) 에 해당 하는 프로젝트 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)합니다.

## <a name="porting-strategies"></a>포팅 전략

하려면 포트 점진적으로 C + + CX 코드를 C + + /cli WinRT을 수 있습니다. C + + /CX 및 C + + /cli WinRT 코드 XAML 컴파일러 지원 및 Windows 런타임 구성 요소를 제외 하 고 동일한 프로젝트에서 공존할 수 있습니다. 이러한 두 가지 예외에 대 한 해야 대상으로 하거나 C + + /CX 또는 C + + WinRT 동일한 프로젝트 내에서.

> [!IMPORTANT]
> XAML 응용 프로그램을 작성 하는 프로젝트를 권장 하는 워크플로가 하나 경우 C + 중 하나를 사용 하 여 Visual Studio에서 새 프로젝트를 만들어 + WinRT 프로젝트 템플릿 (참조 [Visual Studio 지원 C + + /cli WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). 그런 다음 C +에서 소스 코드와 태그를 통해 복사를 시작 + CX 프로젝트입니다. 새 XAML 페이지를 추가할 수 있습니다 **프로젝트** \> **새 항목 추가...** \> **Visual c + +** > **빈 페이지 (C + + /cli WinRT)** 합니다.
>
> 요소 코드에서 XAML C + Windows 런타임 구성 요소를 사용할 수는 또는 + CX 것 처럼 프로젝트입니다. 이동 하거나 많은 C + + 구성 요소를 수 있습니다 하 고 변경한 다음 C + XAML 프로젝트 CX 코드 + WinRT 합니다. 남기 세요 다른 XAML 프로젝트 C + + /CX에서는 만들 새 C + + WinRT 구성 요소 보십시오 이식 C + + CX 코드 구성 요소 및 XAML 프로젝트에서. 있을 수는 C + + /cli CX 구성 요소 프로젝트와 함께 C + + /cli WinRT 구성 요소 프로젝트를 동일한 솔루션 내에서 둘 다 응용 프로그램 프로젝트에서 참조 하 고 다른 간에 점진적으로 포트입니다. 참조 [Interop 간에 C + + /cli WinRT 및 C + + CX](interop-winrt-cx.md) 두 언어는 예측을 사용 하 여 동일한 프로젝트에 대 한 자세한 내용은 합니다.

> [!NOTE]
> [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)와 Windows SDK 모두 루트 네임스페이스인 **Windows**에 유형을 선언합니다. C++/WinRT에 프로젝션된 Windows 유형은 Windows 유형과 동일한 정규화된 이름을 가지지만 C++ **winrt** 네임스페이스에 배치됩니다. 이처럼 서로 다른 네임스페이스를 사용하면 사용자가 원하는 대로 C++/CX에서 C++/WinRT로 포트할 수 있습니다.

위에서 언급 한 예외를 염두에 첫 번째 단계에서 이식 C + + CX 프로젝트 C + + /cli WinRT는 수동으로 추가 C + +를 WinRT 지원 (참조 [Visual Studio 지원 C + + WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). 이 작업을 수행 하려면 설치 합니다 [Microsoft.Windows.CppWinRT NuGet 패키지](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) 프로젝트로 합니다. Visual Studio에서 프로젝트를 열고 클릭 **프로젝트** \> **NuGet 패키지 관리...** \> **찾아보기**를 입력 하거나 붙여 넣습니다 **Microsoft.Windows.CppWinRT** 검색 상자에서 검색 결과에서 항목을 선택 하 고 클릭 **설치** 해당 프로젝트에 대 한 패키지를 설치 합니다. 해당 변경의 효과 중 하나는 프로젝트에서 C++/CX에 대한 지원이 꺼진다는 것입니다. C +에 종속성의 모든 빌드 메시지 도움이 찾기 (및 포트)을 해제 하는 지원을 유지 하는 것이 좋습니다 + /CX 또는 있습니다 다시 켤 수 지원 (프로젝트 속성에서 **C/c + +** \> **일반** \> **Windows 런타임 확장 사용** \> **예 (/ZW)**)를 점진적으로 포트입니다.

프로젝트 속성을 확인 하십시오 **일반적인** \> **대상 플랫폼 버전** 10.0.17134.0 (Windows 10, 버전 1803)로 설정 된 이상.

컴파일된 헤더 파일에(일반적으로 `pch.h`) `winrt/base.h`를 포함합니다.

```cppwinrt
#include <winrt/base.h>
```

C++/WinRT 프로젝션된 Windows API 헤더를 포함하는 경우(예를 들어, `winrt/Windows.Foundation.h`) 자동으로 포함되기 때문에 명시적으로 이와 같은 `winrt/base.h`를 포함할 필요가 없습니다.

프로젝트가 [Windows 런타임 C++ 템플릿 라이브러리(WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 유형도 사용하는 경우 [WRL에서 C++/WinRT로 이동](move-to-winrt-from-wrl.md)을 참조합니다.

## <a name="parameter-passing"></a>매개 변수-전달
작성 하는 경우 C + + 전달 CX 소스 코드를 C + + /CX 형식으로 hat 함수 매개 변수로 (\^) 참조 합니다.

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

C++/WinRT에서 동기화 함수를 위해 기본적으로 `const&` 매개 변수를 사용해야 합니다. 이를 통해 복사본과 연동된 오버헤드를 방지할 수 있습니다. 하지만 사용자 코루틴이 값으로 캡처하고 수명 문제를 방지할 수 있도록 값으로 전달을 사용해야 합니다(자세한 내용은 [C++/WinRT 동시성 및 비동기 작업](concurrency.md) 참조).

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

C++/WinRT 개체는 기본적으로 지원 Windows 런타임 개체에 대한 인터페이스 포인터를 보유하는 값입니다. C++/WinRT 개체를 복사할 때 컴파일러는 캡슐화된 인터페이스 포인터를 복사하여 참조 횟수가 증가합니다. 복사의 최종 소멸은 감소 참조 수를 포함합니다. 따라서 필요한 경우에만 오버헤드가 복사됩니다.

## <a name="variable-and-field-references"></a>변수 및 필드 참조
작성 하는 경우 C + + /cli CX 소스 코드를 hat를 사용 (\^) Windows 런타임 개체 및 화살표를 참조 하는 변수 (-&gt;) hat 변수 역참조 연산자.

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

해당 하는 C +를 이식 하는 경우 + WinRT 코드 따라서 hats를 제거 하 고 화살표 연산자를 변경 하 여 가져올 수 있습니다 (-&gt;) 점 연산자 (.). C + + /cli 예상 WinRT 형식은 값 및 포인터가 아닌 합니다.

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

기본 생성자는 C + + /cli CX hat 포인터를 null로 초기화 합니다. 다음은 C + + /cli CX 코드 예제에서는 정확한 형식에 초기화 되지 않은 것의 변수/필드를 만들겠습니다. 즉,를 처음 참조 하지 않습니다는 **TextBlock**; 나중에 대 한 참조를 할당 하려고 합니다.

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

해당 C + + /cli WinRT, 참조 [지연 된 초기화](consume-apis.md#delayed-initialization)합니다.

## <a name="properties"></a>속성
C++/CX 언어 확장은 속성의 개념을 포함합니다. C++/CX 소스 코드를 쓸 때 필드처럼 속성에 액세스할 수 있습니다. 표준 C++는 속성의 개념을 가지지 않으므로 C++/WinRT에서 get 및 set 함수를 호출합니다.

다음 예에서 **XboxUserId**, **UserState**, **PresenceDeviceRecords** 및 **Size**는 모두 속성입니다.

### <a name="retrieving-a-value-from-a-property"></a>속성에서 값 검색
여기서 C++/CX에서 속성 값을 가져오는 방법을 설명합니다.

```cppcx
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

```cppcx
record->UserState = newValue;
```

C++/WinRT에서 해당하는 작업을 수행하려면, 속성과 동일한 이름으로 함수를 호출하고 인수를 전달합니다.

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>클래스 인스턴스 만들기
작업할 C + + /cli는 hat 라고 것에 대 한 핸들을 통해 CX 개체 (\^) 참조 합니다. `ref new` 키워드를 통해 새로운 개체를 만들면 [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646)를 호출하여 런타임 클래스의 새로운 인스턴스를 활성화합니다.

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

C++/WinRT 개체는 값입니다. 따라서 스택에 또는 개체의 필드로 이를 할당할 수 있습니다. C++/WinRT 개체를 할당하기 위해 *절대*`ref new`(또는 `new`)를 사용해서는 안 됩니다. 백그라운드에서 **RoActivateInstance**는 여전히 호출되고 있습니다.

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

리소스가 초기화하기에 비싼 경우 초기화가 실제로 필요할 때까지 초기화를 지연하는 것이 일반적입니다.

```cppcx
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

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>기본 런타임 클래스에서 파생으로 변환
것이 일반적으로 참조 기본 파생 형식의 개체를 참조 하는 것이 알고 있는 있습니다. C + + /CX에서는 사용할 `dynamic_cast` 에 *캐스트* 는 참조 기본에는 참조에서 파생 합니다. 합니다 `dynamic_cast` 실제로 숨겨진 호출 [ **QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521)합니다. 다음은 일반적인 예제&mdash;종속성 속성 변경 이벤트를 처리 하 고에서 캐스팅 하려는 **DependencyObject** 종속성 속성을 소유 하는 실제 형식입니다.

```cppcx
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

해당 C + + WinRT 코드는 대체 합니다 `dynamic_cast` 에 대 한 호출을 사용 하 여는 [ **IUnknown::try_as** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function) 함수를 캡슐화 하는 **QueryInterface**합니다. 호출 하는 옵션도 있습니다 [ **IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)를 대신 하는 경우 예외를 throw 필요한 인터페이스 (요청 하는 형식의 기본 인터페이스)에 대 한 쿼리는 반환 되지 않습니다. 다음은 C + + /cli WinRT 코드 예제입니다.

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

```cppcx
auto token = myButton->Click += ref new RoutedEventHandler([=](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

C++/WinRT에서와 동일합니다.

```cppwinrt
auto token = myButton().Click([=](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

lambda 함수 대신 대리인을 무료 함수 또는 포인터-회원-함수로 구현할 수 있습니다. 자세한 정보는 [C++/WinRT의 대리자를 사용한 이벤트 처리](handle-events.md)를 참조하세요.

이벤트 및 대리인이 내부적으로 사용되는(이진 전체에서가 아니라) C++/CX 코드 베이스에서 포트하는 경우 [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate)은 C++/WinRT에서 해당 패턴을 복제하는 데 도움이 됩니다. 도 참조 하세요 [대리자, 간단한 신호 및 프로젝트 내에서 콜백 매개 변수가 있는](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project)합니다.

## <a name="revoking-a-delegate"></a>대리인 취소
C++/CX에서`-=` 연산자를 사용하여 이전 이벤트 등록을 취소합니다.

```cppcx
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
| **Platform:: agile\^** | [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform:: array\^** | 참조 [포트 **platform:: array\^**](#port-platformarray) |
| **Platform:: exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform:: invalidargumentexception\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform:: object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform:: string\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagileref"></a>포트 **platform:: agile\^**  하려면 **winrt::agile_ref**
합니다 **platform:: agile\^**  형식 C + + /cli CX 모든 스레드에서 액세스할 수 있는 Windows 런타임 클래스를 나타냅니다. C + + /cli WinRT 표현은 [ **winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref)합니다.

C++/CX에서.

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

C++/WinRT에서.

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>포트 **platform:: array\^**
옵션에는 이니셜라이저 목록을 사용 하 여 포함 된 **std:: array**, 또는 **std:: vector**합니다. 자세한 내용은 및 코드 예제를 참조 하세요 [표준 이니셜라이저 목록](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists) 하 고 [표준 배열 및 벡터](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-arrays-and-vectors)입니다.

### <a name="port-platformexception-to-winrthresulterror"></a>포트 **platform:: exception\^**  하려면 **winrt::hresult_error**
**platform:: exception\^**  형식이 생성 되는 C + + Windows 런타임 API가 아닌 S 돌아오면 CX\_확인 HRESULT입니다. C++/WinRT에 대한 등가는 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)입니다.

포트 C + + WinRT를 사용 하는 모든 코드를 변경 **platform:: exception\^**  사용 하도록 **winrt::hresult_error**합니다.

C++/CX에서.

```cppcx
catch (Platform::Exception^ ex)
```

C++/WinRT에서.

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT는 이러한 예외 클래스를 제공합니다.

| 예외 유형 | 기본 클래스 | HRESULT |
| ---- | ---- | ---- |
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | [  **hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function) 호출 |
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

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

그리고 C++/WinRT의 등가.

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>포트 **platform:: object\^**  하려면 **winrt::Windows::Foundation::IInspectable**
모든 C++/WinRT 유형과 마찬가지로, **winrt::Windows::Foundation::IInspectable**은 값 유형입니다. 해당 유형의 변수를 null로 초기화하는 방법입니다.

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>포트 **platform:: string\^**  하려면 **winrt::hstring**
**Platform:: string\^**  Windows 런타임 HSTRING ABI 형식과 동일 합니다. C++/WinRT에 대한 등가는 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)입니다. 그러나 C++/WinRT에서는 **std::wstring** 같은 C++ 표준 라이브러리 전각 문자열 형식 및/또는 전각 문자열 리터럴을 사용해 Windows 런타임 API를 호출할 수 있습니다. 자세한 내용과 코드 예제는 [C++/WinRT의 문자열 처리](strings.md)를 참조하세요.

C + + /CX에 액세스할 수 있습니다 합니다 [ **Platform::String::Data** ](https://docs.microsoft.com/en-us/cpp/cppcx/platform-string-class#data) C 스타일 문자열을 검색할 속성 **const wchar_t\***  (예를 들어 전달 하는 배열 되도록 **std::wcout**).

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

C++/WinRT에서 같은 작업을 수행하려면 [**hstring::c_str**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) 함수를 이용하여 **std::wstring**에서와 마찬가지로 null 종료 C-스타일 문자열 버전을 가져옵니다.

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

사용 하거나 문자열을 반환 하는 Api를 구현 하는 데 있어 일반적으로 변경한 모든 C + + /CX 코드를 사용 하는 **platform:: string\^**  사용 하려면 **winrt::hstring** 대신 합니다.

문자열을 가지는 C++/CX의 예는 다음과 같습니다.

```cppcx
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

#### <a name="tostring"></a>Tostring)

C + + /cli CX 제공 합니다 [object:: tostring](/cpp/cppcx/platform-object-class?view=vs-2017#tostring) 메서드.

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

C + + /cli WinRT이이 기능을 직접 제공 하지 않지만 대안을 받을 수 있습니다.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

## <a name="important-apis"></a>중요 API
* [winrt::delegate 구조체 템플릿](/uwp/cpp-ref-for-winrt/delegate)
* [winrt::hresult_error 구조체](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::hstring 구조체](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 네임스페이스](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>관련 항목
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [작성할 이벤트 C + + /cli WinRT](author-events.md)
* [동시성 및 비동기 작업을 사용 하 여 C + + /cli WinRT](concurrency.md)
* [C++/WinRT를 통한 API 사용](consume-apis.md)
* [C + 대리자를 사용 하 여 이벤트를 처리 + WinRT](handle-events.md)
* [C++/WinRT와 C++/CX 간의 상호 운용성](interop-winrt-cx.md)
* [Microsoft 인터페이스 정의 언어 3.0 참조](/uwp/midl-3)
* [WRL에서 C++/WinRT로 이동](move-to-winrt-from-wrl.md)
* [문자열 처리 C + + /cli WinRT](strings.md)
