---
description: C++/CX 코드를 C++/WinRT의 해당 코드로 이동하는 방법을 보여 줍니다.
title: C++/CX에서 C++/WinRT로 이동
ms.date: 01/17/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 이식, 마이그레이션, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 7fbe10e41da1b330d6f5042bea109a8a0e04f8ad
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66360156"
---
# <a name="move-to-cwinrt-from-ccx"></a>C++/CX에서 C++/WinRT로 이동

이 항목에서는 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 프로젝트의 코드를 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)의 코드로 이식하는 방법을 보여 줍니다.

## <a name="porting-strategies"></a>이식 전략

C++/CX 코드를 C++/WinRT로 점진적으로 이식할 수 있습니다. C++/CX 및 C++/WinRT 코드는 같은 프로젝트에 공존할 수 있으며, XAML 컴파일러 지원 및 Windows 런타임 구성 요소의 예외가 있습니다. 이 두 가지 예외를 위해 C++/CX 또는 C++/WinRT를 동일한 프로젝트 내에서 대상으로 지정해야 합니다.

> [!IMPORTANT]
> 프로젝트가 XAML 애플리케이션을 빌드하는 경우 권장하는 한 가지 워크플로는 먼저 C++/WinRT 프로젝트 템플릿 중 하나를 사용하여 Visual Studio에서 새 프로젝트를 만드는 것입니다([Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) 참조). 그런 다음, C++/CX 프로젝트에서 원본 코드 및 태그 복사를 시작합니다. **프로젝트** \> **새 항목 추가...** \> **Visual C++**  >  **비어 있는 페이지(C++/WinRT)** 를 사용하여 새 XAML 페이지를 추가할 수 있습니다.
>
> 또는 Windows 런타임 구성 요소를 사용하여 이식할 때 XAML C++/CX 프로젝트에서 코드를 팩터링할 수 있습니다. 최대한 많은 C++/CX 코드를 구성 요소로 이동한 다음, XAML 프로젝트를 C++/WinRT로 변경합니다. 또는 XAML 프로젝트를 C++/CX로 남겨 두고, 새 C++/WinRT 구성 요소를 만들고, XAML 프로젝트에서 구성 요소로 C++/CX 코드 이식을 시작합니다. 또한 동일한 솔루션 내에 C++/WinRT 구성 요소 프로젝트와 함께 C++/CX 구성 요소 프로젝트를 포함하고, 애플리케이션 프로젝트에서 두 프로젝트를 모두 참조하고, 점진적으로 서로 이식할 수 있습니다. 동일한 프로젝트에서 두 언어 프로젝션을 사용하는 방법에 대한 자세한 내용은 [C++/WinRT 및 C++/CX 사이의 Interop](interop-winrt-cx.md)를 참조하세요.

> [!NOTE]
> [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 및 Windows SDK는 둘 다 루트 네임스페이스인 **Windows**에서 형식을 선언합니다. C++/WinRT에 프로젝션된 Windows 형식은 Windows 형식과 동일한 정규화된 이름을 사용하지만 C++ **winrt** 네임스페이스에 배치됩니다. 이렇게 네임스페이스를 구별하여 원하는 대로 C++/CX에서 C++/WinRT로 이식할 수 있습니다.

위에 언급된 예외 사항을 고려하면서 C++/CX 프로젝트를 C++/WinRT로 이식하는 첫 번째 단계는 C++/WinRT 지원을 수동으로 추가하는 것입니다([Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) 참조). 이 작업을 수행하려면 [Microsoft.Windows.CppWinRT NuGet 패키지](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)를 프로젝트에 설치합니다. Visual Studio에서 프로젝트를 열고 **프로젝트** \> **NuGet 패키지 관리...** \> **찾아보기**를 클릭합니다. 검색 상자에 **Microsoft.Windows.CppWinRT**를 입력하거나 붙여넣고 검색 결과에서 항목을 선택한 다음, **설치**를 클릭하여 해당 프로젝트용 패키지를 설치합니다. 해당 변경의 효과 중 하나는 프로젝트에서 C++/CX에 대한 지원이 꺼진다는 것입니다. 빌드 메시지가 C++/CX에서 모든 종속성을 찾고 이식하는 데 도움이 되도록 지원을 끈 채로 두는 것이 좋습니다. 또는 지원을 다시 켜고(프로젝트 속성에서 **C/C++** \> **일반** \> **Windows 런타임 확장 사용** \> **예(/ZW)** ) 점진적으로 이식할 수 있습니다.

프로젝트 속성 **일반** \> **대상 플랫폼 버전**이 10.0.17134.0(Windows 10 버전 1803) 이상으로 설정되었는지 확인합니다.

미리 컴파일된 헤더 파일에(일반적으로 `pch.h`) `winrt/base.h`를 포함합니다.

```cppwinrt
#include <winrt/base.h>
```

C++/WinRT 프로젝션된 Windows API 헤더를 포함하는 경우(예: `winrt/Windows.Foundation.h`) 자동으로 포함되기 때문에 명시적으로 이와 같은 `winrt/base.h`를 포함할 필요가 없습니다.

프로젝트가 [Windows 런타임 C++ 템플릿 라이브러리(WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 형식도 사용하는 경우 [WRL에서 C++/WinRT로 이동](move-to-winrt-from-wrl.md)을 참조하세요.

## <a name="parameter-passing"></a>매개 변수 전달
C++/CX 원본 코드를 작성할 때 C++/CX 형식을 hat(\^) 참조로 함수 매개 변수로 전달합니다.

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

C++/WinRT에서 동기 함수를 위해 기본적으로 `const&` 매개 변수를 사용해야 합니다. 이를 통해 복사본과 연동된 오버헤드를 방지할 수 있습니다. 하지만 사용자 코루틴이 값으로 캡처하고 수명 문제를 방지할 수 있도록 값으로 전달을 사용해야 합니다(자세한 내용은 [C++/WinRT 동시성 및 비동기 작업](concurrency.md) 참조).

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

C++/WinRT 개체는 기본적으로 지원 Windows 런타임 개체에 대한 인터페이스 포인터를 보유하는 값입니다. C++/WinRT 개체를 복사할 때 컴파일러는 캡슐화된 인터페이스 포인터를 복사하여 참조 횟수가 증가합니다. 복사의 최종 소멸은 감소 참조 수를 포함합니다. 따라서 필요한 경우에만 오버헤드가 복사됩니다.

## <a name="variable-and-field-references"></a>변수 및 필드 참조
C++/CX 원본 코드를 작성할 때 hat(\^) 변수를 사용하여 Windows 런타임 개체를 참조하고, 화살표(-&gt;) 연산자를 사용하여 hat 변수를 역참조합니다.

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

해당 C++/WinRT 코드에 이식할 때 먼저 hat을 제거하고 화살표 연산자(-&gt;)를 점 연산자(.)로 변경합니다. C++/WinRT 프로젝션된 형식은 포인터가 아니라 값입니다.

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

C++/CX hat 포인터의 기본 생성자가 이 형식을 null로 초기화합니다. 다음은 올바른 형식이지만 초기화되지 않는 변수/필드를 만드는 C++/CX 코드 예제입니다. 즉, 초기에 **TextBlock**을 참조하지 않으며, 나중에 참조를 할당하려고 합니다.

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

C++/WinRT의 해당 형식에 대해서는 [지연된 초기화](consume-apis.md#delayed-initialization)를 참조하세요.

## <a name="properties"></a>속성
C++/CX 언어 확장은 속성의 개념을 포함합니다. C++/CX 원본 코드를 작성할 때 필드처럼 속성에 액세스할 수 있습니다. 표준 C++는 속성의 개념을 가지지 않으므로 C++/WinRT에서 get 및 set 함수를 호출합니다.

다음 예제에서 **XboxUserId**, **UserState**, **PresenceDeviceRecords** 및 **Size**는 모두 속성입니다.

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

해당 C++/WinRT 원본 코드는 매개 변수 없이 속성과 동일한 이름의 함수를 호출합니다.

```cppwinrt
void Sample::LogPresenceRecord(PresenceRecord const& record)
{
    auto id = record.XboxUserId();
    auto state = record.UserState();
    auto size = record.PresenceDeviceRecords().Size();
}
```

**PresenceDeviceRecords** 함수는 자체에 **Size** 함수를 가지는 Windows 런타임 개체를 반환합니다. 반환된 개체가 C++/WinRT 프로젝션된 형식이기도 하므로 점 연산자를 사용하여 역참조하여 **Size**를 호출합니다.

### <a name="setting-a-property-to-a-new-value"></a>속성을 새 값으로 설정
속성을 새 값으로 설정하는 것은 비슷한 패턴을 따릅니다. 먼저 C++/CX의 경우 다음과 같습니다.

```cppcx
record->UserState = newValue;
```

C++/WinRT에서 해당하는 작업을 수행하려면, 속성과 동일한 이름으로 함수를 호출하고 인수를 전달합니다.

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>클래스 인스턴스 만들기
일반적으로 hat(\^) 참조로 알려진 이에 대한 핸들을 통해 C++/CX 개체를 작업합니다. `ref new` 키워드를 통해 새로운 개체를 만들면 [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance)를 호출하여 런타임 클래스의 새로운 인스턴스를 활성화합니다.

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

C++/WinRT 개체는 값입니다. 따라서 스택에 또는 개체의 필드로 이를 할당할 수 있습니다. C++/WinRT 개체를 할당하기 위해 ‘절대’ `ref new`(또는 `new`)를 사용해서는 안 됩니다.  백그라운드에서 **RoActivateInstance**는 여전히 호출되고 있습니다.

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

C++/WinRT로 이식된 동일한 코드. `nullptr` 생성자의 사용에 주의합니다. 해당 생성자에 대한 자세한 내용은 [C++/WinRT를 통한 API 사용](consume-apis.md)을 참조하세요.

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

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>기본 런타임 클래스에서 파생된 항목 클래스로 변환
파생된 형식의 개체를 참조하는 기본에 대한 참조를 포함하는 것이 일반적입니다. C++/CX에서는 `dynamic_cast`를 사용하여 기본에 대한 참조를 파생에 대한 참조로 ‘캐스팅’합니다.  `dynamic_cast`는 실제로 [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))에 대한 숨겨진 호출입니다. 다음은 종속성 속성 변경 이벤트를 처리하며 **DependencyObject**에서 다시 종속성 속성을 소유하는 실제 형식으로 캐스팅하려는 일반적인 예제입니다.

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

해당하는 C++/WinRT 코드는 `dynamic_cast`를 [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function) 함수 호출(**QueryInterface**를 캡슐화함)로 바꿉니다. 필수 인터페이스(요청하는 형식의 기본 인터페이스)에 대한 쿼리가 반환되지 않는 경우 대신에 예외를 throw하는 [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)를 호출하는 옵션도 있습니다. 다음은 C++/WinRT 코드 예제입니다.

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

## <a name="event-handling-with-a-delegate"></a>대리자로 이벤트 처리
여기에 람다 함수를 대리자로 사용하여 C++/CX에서 이벤트를 처리하는 일반적인 예제가 나와 있습니다.

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

람다 함수 대신 대리자를 자유 함수 또는 멤버 포인터 함수로 구현할 수 있습니다. 자세한 내용은 [C++/WinRT의 대리자를 사용한 이벤트 처리](handle-events.md)를 참조하세요.

이벤트 및 대리자가 내부적으로 사용되는(이진 전체에서가 아니라) C++/CX 코드베이스에서 이식하는 경우 [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate)는 C++/WinRT에서 해당 패턴을 복제하는 데 도움이 됩니다. [프로젝트 내의 매개 변수가 있는 대리자, 단순 신호 및 콜백](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project)을 참조하세요.

## <a name="revoking-a-delegate"></a>대리자 취소
C++/CX에서`-=` 연산자를 사용하여 이전 이벤트 등록을 취소합니다.

```cppcx
myButton->Click -= token;
```

C++/WinRT에서와 동일합니다.

```cppwinrt
myButton().Click(token);
```

자세한 내용과 옵션은 [등록된 대리자 취소](handle-events.md#revoke-a-registered-delegate)를 참조하세요.

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>C++/WinRT 형식에 C++/CX **Platform** 형식 매핑
C++/CX는 **Platform** 네임스페이스에서 몇 가지 데이터 형식을 제공합니다. 이 형식은 표준 C++가 아니므로 Windows 런타임 언어 확장을 활성화한 경우에만 이를 사용할 수 있습니다(Visual Studio 프로젝트 속성 **C/C++**  > **일반** > **Windows 런타임 확장 사용** > **예(/ZW)** ). 아래 표는 **Platform** 형식에서 C++/WinRT의 해당 형식으로 이식하는 데 도움이 됩니다. 이를 수행하면 C++/WinRT가 표준 C++이이기 때문에 `/ZW` 옵션을 끌 수 있습니다.

| C++/CX | C++/WinRT |
| ---- | ---- |
| **Platform::Agile\^** | [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform::Array\^** | [**Platform::Array\^ 이식**](#port-platformarray) 참조 |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagileref"></a>**Platform::Agile\^** 을 **winrt::agile_ref**로 이식
C++/CX의 **Platform::Agile\^** 형식은 스레드에서 액세스할 수 있는 Windows 런타임 클래스를 나타냅니다. C++/WinRT의 해당 항목은 [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref)입니다.

C++/CX에서.

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

C++/WinRT에서.

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>**Platform::Array\^** 이식
옵션에는 이니셜라이저 목록, **std::array** 또는 **std::vector** 사용이 포함됩니다. 자세한 내용과 코드 예제는 [표준 이니셜라이저 목록](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists) 및 [표준 배열 및 벡터](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-arrays-and-vectors)를 참조하세요.

### <a name="port-platformexception-to-winrthresulterror"></a>**Platform::Exception\^** 을 **winrt::hresult_error**로 이식
Windows 런타임 API가 S\_OK HRESULT가 아닌 값을 반환하면 **Platform::Exception\^** 형식이 C++/CX에서 생성됩니다. C++/WinRT의 해당 항목은 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)입니다.

C++/WinRT로 이식하려면 **Platform::Exception\^** 을 사용하는 모든 코드를 **winrt::hresult_error**를 사용하도록 변경합니다.

C++/CX에서.

```cppcx
catch (Platform::Exception^ ex)
```

C++/WinRT에서.

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT는 이 예외 클래스를 제공합니다.

| 예외 유형 | 기본 클래스 | HRESULT |
| ---- | ---- | ---- |
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) 호출 |
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

각 클래스(**hresult_error** 기본 클래스를 통해)는 오류의 HRESULT를 반환하는 [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) 함수와 해당 HRESULT의 문자열 표현을 반환하는 [**message**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errormessage-function) 함수를 제공합니다.

C++/CX에서 예외를 throw하는 예제는 다음과 같습니다.

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

그리고 C++/WinRT의 해당 항목.

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>**Platform::Object\^** 를 **winrt::Windows::Foundation::IInspectable**로 이식
모든 C++/WinRT 형식과 마찬가지로, **winrt::Windows::Foundation::IInspectable**은 값 형식입니다. 해당 형식의 변수를 null로 초기화하는 방법은 다음과 같습니다.

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>**Platform::String\^** 을 **winrt::hstring**으로 이식
**Platform::String\^** 은 Windows 런타임 HSTRING ABI 형식에 해당합니다. C++/WinRT의 해당 항목은 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)입니다. 그러나 C++/WinRT에서는 **std::wstring** 같은 C++ 표준 라이브러리 와이드 문자열 형식 및/또는 와이드 문자열 리터럴을 사용해 Windows 런타임 API를 호출할 수 있습니다. 자세한 내용과 코드 예제는 [C++/WinRT의 문자열 처리](strings.md)를 참조하세요.

C++/CX에서 [**Platform::String::Data**](https://docs.microsoft.com/en-us/cpp/cppcx/platform-string-class#data) 속성에 액세스하여 문자열을 C 스타일 **const wchar_t\*** 배열로 검색할 수 있습니다(예를 들면 이를 **std::wcout**에 전달).

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

C++/WinRT에서 같은 작업을 수행하려면 [**hstring::c_str**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) 함수를 이용하여 **std::wstring**에서와 마찬가지로 null 종료 C 스타일 문자열 버전을 가져옵니다.

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

문자열을 가져오거나 반환하는 API 구현에서는 일반적으로 **Platform::String\^** 을 사용하는 모든 C++/CX 코드를 변경하여 **winrt::hstring**을 대신 사용합니다.

문자열을 가져오는 C++/CX API의 예제는 다음과 같습니다.

```cppcx
void LogWrapLine(Platform::String^ str);
```

C++/WinRT의 경우 이처럼 [MIDL 3.0](/uwp/midl-3)에서 API를 선언할 수 있습니다.

```idl
// LogType.idl
void LogWrapLine(String str);
```

C++/WinRT 도구 체인이 다음과 같이 사용자를 위해 원본 코드를 생성합니다.

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

#### <a name="tostring"></a>ToString()

C++/CX는 [Object::ToString](/cpp/cppcx/platform-object-class?view=vs-2017#tostring) 메서드를 제공합니다.

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

C++/WinRT는 이 기능을 직접 제공하지 않지만 대체 기능으로 전환할 수 있습니다.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

## <a name="important-apis"></a>중요 API
* [winrt::delegate 구조 템플릿](/uwp/cpp-ref-for-winrt/delegate)
* [winrt::hresult_error 구조체](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::hstring 구조체](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 네임스페이스](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>관련 항목
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT의 이벤트 작성](author-events.md)
* [C++/WinRT를 통한 동시성 및 비동기 작업](concurrency.md)
* [C++/WinRT를 통한 API 사용](consume-apis.md)
* [C++/WinRT의 대리자를 사용한 이벤트 처리](handle-events.md)
* [C++/WinRT와 C++/CX 간의 상호 운용성](interop-winrt-cx.md)
* [Microsoft 인터페이스 정의 언어 3.0 참조](/uwp/midl-3)
* [WRL에서 C++/WinRT로 이동](move-to-winrt-from-wrl.md)
* [C++/WinRT의 문자열 처리](strings.md)
