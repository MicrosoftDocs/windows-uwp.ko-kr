---
author: stevewhims
description: 이번 항목에서는 이벤트가 발생하는 런타임 클래스를 포함해 Windows 런타임 구성 요소를 작성하는 방법에 대해서 설명합니다. 또한 구성 요소를 사용하여 이벤트를 처리하는 앱에 대해서도 설명합니다.
title: C++/WinRT의 이벤트 작성
ms.author: stwhi
ms.date: 07/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 작성, 이벤트
ms.localizationpriority: medium
ms.openlocfilehash: 1b0aa1a53ce69369c442cef7f3b5912631593242
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2018
ms.locfileid: "4176194"
---
# <a name="author-events-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)의 이벤트 작성

이번 항목에서는 차변 발생 시 이벤트가 발생하는 은행 계좌 런타임 클래스를 포함해 Windows 런타임 구성 요소를 작성하는 방법에 대해서 설명합니다. 또한 은행 계좌 런타임 클래스를 사용하면서 함수를 호출하여 잔액을 조정하거나, 발생하는 이벤트를 처리하는 주요 앱에 대해서도 설명합니다.

> [!NOTE]
> C++/WinRT Visual Studio Extension(VSIX)(프로젝트 템플릿 지원과 C++/WinRT MSBuild 속성 및 대상 제공)의 설치 및 사용에 대한 자세한 내용은 [C++/WinRT에 대한 Visual Studio 지원 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)를 참조하세요.

> [!IMPORTANT]
> C++/WinRT를 사용해 런타임 클래스를 사용하거나 작성하는 방법을 더욱 쉽게 이해할 수 있는 필수 개념과 용어에 대해서는 [C++/WinRT를 통한 API 사용](consume-apis.md)과 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Windows 런타임 구성 요소(BankAccountWRC) 만들기

먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. **Visual c + +** 만들기 > **Windows 유니버설** > **Windows 런타임 구성 요소 (C + + WinRT)** 프로젝트를 만들어서 이름을 *BankAccountWRC* ("은행 계좌 Windows 런타임 구성 요소")에 대 한 합니다.

새로 만든 프로젝트에는 `Class.idl`이라는 이름의 파일이 포함되어 있습니다. 해당 파일의 이름을 `BankAccount.idl` (이름 바꾸기는 `.idl` 파일에는 자동으로 종속 바꿉니다 `.h` 및 `.cpp` 파일, 너무). 내용을 `BankAccount.idl` 아래 목록과 함께 합니다.

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    runtimeclass BankAccount
    {
        BankAccount();
        event Windows.Foundation.EventHandler<Single> AccountIsInDebit;
        void AdjustBalance(Single value);
    };
}
```

파일을 저장합니다. 프로젝트는 순간 완료 될 때까지 빌드 없습니다 수 있지만 **BankAccount** 런타임 클래스를 구현 하는 소스 코드 파일을 생성 하기 때문에 작업을 수행 하는 것은 이제 구축 합니다. 해 보시기 이제 빌드 (이 단계에서 표시 되는 수 빌드 오류가 발생을 사용 하 여 작업을 수행 해야 합니다. `Class.h` 및 `Class.g.h` 찾을 수 없음). 빌드 과정은 `midl.exe` 구성 요소의 Windows 런타임 메타 데이터 파일을 만들려면 도구가 실행 되어 (은 `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`). 그런 다음 `cppwinrt.exe` 도구가 실행되어(`-component` 옵션과 함께) 구성 요소를 작성하도록 지원하는 소스 코드 파일을 생성합니다. 이러한 파일 **BankAccount** 런타임 클래스 IDL에서 선언 된 구현을 시작할 수 있는 스텁이 포함 됩니다. 이 스텁이 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h`와 `BankAccount.cpp`입니다.

파일 탐색기에서 스텁 파일을 복사 `BankAccount.h` 및 `BankAccount.cpp` 폴더에서 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` 는 프로젝트 파일이 포함 된 폴더로 `\BankAccountWRC\BankAccountWRC\`, 및 대상에 파일을 대체 합니다. 이제 `BankAccount.h`와 `BankAccount.cpp`를 열고 런타임 클래스를 구현합니다. `BankAccount.h`에서 전용 멤버 2개를 BankAccount 구현체(팩터리 구현체 *아님*)에 추가합니다.

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        winrt::event<Windows::Foundation::EventHandler<float>> m_accountIsInDebitEvent;
        float m_balance{ 0.f };
    };
}
...
```

위에서 알 수 있듯이 이벤트는 특정 대리자 형식으로 매개 변수화 [**winrt::event**](/uwp/cpp-ref-for-winrt/event) 구조체 템플릿, 측면에서 구현 됩니다.

`BankAccount.cpp`에서 아래 예제에 표시된 대로 함수를 구현합니다. C++/WinRT에서 IDL이 선언한 이벤트는 오버로드된 함수의 집합으로 구현됩니다(속성이 오버로드된 get 및 set 함수 집합으로 구현되는 것과 유사한 방식). 한 오버로드는 등록할 대리자를 가지며 토큰을 반환합니다. 다른 오버로드는 토큰을 가지며 관련 대리자의 등록을 취소합니다.

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(winrt::event_token const& token)
    {
        m_accountIsInDebitEvent.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
        if (m_balance < 0.f) m_accountIsInDebitEvent(*this, m_balance);
    }
}
```

이벤트 취소자에 대한 오버로드를 구현할 필요는 없습니다(자세한 내용은 [등록된 대리자 취소](handle-events.md#revoke-a-registered-delegate) 참조). C++/WinRT 프로젝션에 의해 처리됩니다. 다른 오버로드는 사용자 시나리오에 최적으로 구현하는 유연성을 제공하기 위해 프로젝션에 반영되지 않습니다. 이처럼 [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) 및 [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) 호출은 효율적이며 동시성/스레드로부터 안전한 기본값입니다. 하지만 매우 많은 이벤트를 처리하는 경우 각각에 대한 이벤트 필드 대신 일부 밀도가 낮은 구현을 원할 수 있습니다.

잔액이 마이너스가 될 경우 **AdjustBalance** 함수의 구현체에서 **AccountIsInDebit** 이벤트가 발생하는 시나리오는 위에서 확인할 수 있습니다.

경고 메시지로 인해에서 경우 빌드를 해결 하거나 **C/c + +** 프로젝트 속성을 설정 > **일반** > **경고를 오류로 처리** **아니요 (/ WX-)**, 프로젝트를 다시 빌드합니다.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>주요 앱(BankAccountCoreApp)을 만들어 Windows 런타임 구성 요소 테스트

이제 새 프로젝트를 만듭니다(`BankAccountWRC` 솔루션에서, 혹은 새로운 솔루션에서). **Visual c + +** 만들기 > **Windows 유니버설** > **Core App (C + + WinRT)** 프로젝트를 만들어서 이름을 *BankAccountCoreApp*합니다.

참조를 추가 하 고 이동 `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (또는 두 프로젝트가 동일한 솔루션의 경우 프로젝트-프로젝트 참조를 추가). **추가**와 **확인**을 차례대로 클릭합니다. 이제 BankAccountCoreApp을 빌드합니다. 오류가 표시 한 이벤트에는 페이로드 파일인 `readme.txt` 하지 존재, Windows 런타임 구성 요소 프로젝트에서 해당 파일을 제외, 후 BankAccountCoreApp 리 빌드합니다.

빌드 과정에서 `cppwinrt.exe` 도구가 실행되면서 참조된 `.winmd` 파일을 소스 코드 파일로 처리합니다. 이 소스 코드 파일에는 구성 요소를 사용할 때 지원할 목적으로 프로젝션된 형식이 포함되어 있습니다. 구성 요소의 런타임 클래스에 맞게 프로젝션된 형식의 헤더(`BankAccountWRC.h`)가 `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\` 폴더로 생성됩니다.

이 헤더를 `App.cpp`에 추가합니다.

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

또한 `App.cpp`에서 다음 코드를 추가하여 BankAccount(프로젝션된 형식의 기본 생성자 사용)를 인스턴스화하고, 이벤트 처리기를 등록하고, 계좌에서 차변이 발생하도록 합니다.

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_bankAccount.AccountIsInDebit([](const auto &, float balance)
        {
            WINRT_ASSERT(balance < 0.f);
        });
    }
    ...

    void Uninitialize()
    {
        m_bankAccount.AccountIsInDebit(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

이제 창을 클릭할 때마다 은행 계좌의 잔고에서 1이 감액됩니다. 이벤트가 예상한 대로 발생 하는지을 보여 주기 위해 **AccountIsInDebit** 이벤트를 처리 하는 람다 식 중단점을 삽입 하 고 앱을 실행 창 내부를 클릭 합니다.

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>매개 변수가 있는 대리자와는 ABI에서 간단한 신호

응용 프로그램 이진 인터페이스 (ABI)에서 이벤트에 액세스할 수 있어야 합니다&mdash;예: 구성 요소와 그 소비 응용 프로그램을 사용 하 여&mdash;이벤트는 Windows 런타임 대리자 형식을 사용 해야 합니다. 위의 예제에는 [**Windows::Foundation::EventHandler\ < T\ >**](/uwp/api/windows.foundation.eventhandler) Windows 런타임 대리자 형식을 사용합니다. [**< TSender, TResult\ > TypedEventHandler\**](/uwp/api/windows.foundation.eventhandler) Windows 런타임 대리자 형식의 또 다른 예입니다.

이러한 두 대리자 형식에 대 한 형식 매개 변수 형식 매개 변수가 Windows 런타임 형식 이어야 너무 하므로, ABI 통과 해야 합니다. 등의 숫자 및 문자열 기본 형식 뿐만 아니라 첫 번째 및 제 3 자 런타임 클래스를 포함 합니다. 컴파일러에 도움이 되는 "*WinRT 형식 이어야 합니다.*" 오류 제약 조건의 잊을 경우.

모든 매개 변수 또는 이벤트 인수를 전달 해야 하지 고유의 간단한 Windows 런타임 대리자 형식을 정의할 수 있습니다. 아래 예제에서는 간단한 **BankAccount** 런타임 클래스의 버전을 보여 줍니다. **SignalDelegate** 이라는 대리자 형식을 선언 하 고를 사용 하는 이벤트 매개 변수를 사용 하는 대신 신호 유형을 이벤트를 발생 시킵니다.

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    delegate void SignalDelegate();

    runtimeclass BankAccount
    {
        BankAccount();
        event BankAccountWRC.SignalDelegate SignalAccountIsInDebit;
        void AdjustBalance(Single value);
    };
}
```

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

        winrt::event_token SignalAccountIsInDebit(BankAccountWRC::SignalDelegate const& handler);
        void SignalAccountIsInDebit(winrt::event_token const& token);
        void AdjustBalance(float value);

    private:
        winrt::event<BankAccountWRC::SignalDelegate> m_signal;
        float m_balance{ 0.f };
    };
}
```

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::SignalAccountIsInDebit(BankAccountWRC::SignalDelegate const& handler)
    {
        return m_signal.add(handler);
    }

    void BankAccount::SignalAccountIsInDebit(winrt::event_token const& token)
    {
        m_signal.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
        if (m_balance < 0.f)
        {
            m_signal();
        }
    }
}
```

```cppwinrt
// App.cpp
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_bankAccount.SignalAccountIsInDebit([] { /* ... */ });
    }
    ...

    void Uninitialize()
    {
        m_bankAccount.SignalAccountIsInDebit(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>매개 변수가 있는 대리자, 간단한 신호 및 프로젝트 내에서 콜백을

이벤트 내부적 으로만 사용 되는 경우 내에서 C + + [**winrt::event**](/uwp/cpp-ref-for-winrt/event) 구조체 템플릿을 계속 사용할 수 있지만 C + 매개 변수화 합니다 (전체에서 아니라 바이너리) 프로젝트 WinRT + /winrt의 비 Windows 런타임 [**winrt:: delegate&lt;... T&gt; **](/uwp/cpp-ref-for-winrt/delegate) 구조체 템플릿은 효율적이 고 참조 계산 대리자입니다. 임의 개수의 매개 변수를 지원 하 고 Windows 런타임 형식에 국한 되지는 않습니다.

아래 예제에서는 먼저 (기본적으로 간단한 신호는) 매개 변수를 사용 하지 않는 서명과 문자열을 사용 하는 다음 하나의 대리자를 표시 합니다.

```cppwinrt
winrt::event<winrt::delegate<>> signal;
signal.add([] { std::wcout << L"Hello, "; });
signal.add([] { std::wcout << L"World!" << std::endl; });
signal();

winrt::event<winrt::delegate<std::wstring>> log;
log.add([](std::wstring const& message) { std::wcout << message.c_str() << std::endl; });
log.add([](std::wstring const& message) { Persist(message); });
log(L"Hello, World!");
```

추가 하는 방법을 이벤트에 원하는 만큼의 구독 대리자를 확인 합니다. 그러나 이벤트와 관련 된 몇 가지 오버 헤드가 있습니다. 필요한 모든만 단일 구독 대리자를 사용 하 여 간단한 콜백 이면 [**winrt:: delegate를 사용할 수 있습니다&lt;. T&gt; **](/uwp/cpp-ref-for-winrt/delegate) 자체적으로 합니다.

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

C +에서 포트 하는 경우 + /CX 코드 베이스 이벤트 및 대리인이 내부적으로 사용 프로젝트에서 다음 **winrt:: delegate** C + 해당 패턴을 복제 하는 데 도움이 되는 + WinRT 합니다.

## <a name="design-guidelines"></a>디자인 지침

이벤트 및 대리자 하지 함수 매개 변수로 전달 하는 것이 좋습니다. **추가** 기능의 [**winrt::event**](/uwp/cpp-ref-for-winrt/event) 한 예외 되므로 경우 대리자를 통과 해야 합니다. 이 지침에 대 한 이유 대리자 (측면에서 지원 여부 하나의 클라이언트 등록 또는 다중) 여러 Windows 런타임 언어에서 다른 폼 걸릴 수 있으므로 때문입니다. 이벤트에는 여러 구독자 모델을 사용 하 여 훨씬 더 예측할 수이 고 일관 된 옵션을 구성합니다.

두 개의 매개 변수가 이벤트 처리기 대리자에 대 한 서명으로 구성 되어야: *보낸 사람* (**IInspectable**) 및 *인수* (일부 이벤트 인수 형식을 [**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)예를 들어).

참고 하는 내부 API를 디자인 하는 경우 다음이 지침 적용할 필요 하지 않습니다. 하지만 내부 Api 되는 경우가 종종 공개 시간이 지남에 따라 합니다.

## <a name="related-topics"></a>관련 항목
* [C++/WinRT를 통한 API 작성](author-apis.md)
* [C++/WinRT를 통한 API 사용](consume-apis.md)
* [C++/WinRT의 대리자를 사용한 이벤트 처리](handle-events.md)
