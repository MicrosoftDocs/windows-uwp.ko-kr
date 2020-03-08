---
description: 이벤트를 발생시키는 런타임 클래스가 포함된 Windows 런타임 구성 요소를 작성하는 방법을 보여 줍니다. 또한 구성 요소를 사용하고 이벤트를 처리하는 앱도 보여 줍니다.
title: C++/WinRT의 이벤트 작성
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 작성, 이벤트
ms.localizationpriority: medium
ms.openlocfilehash: 6fb9b98ec362b59ad2593bbce24654f1dcfc7638
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853288"
---
# <a name="author-events-in-cwinrt"></a>C++/WinRT의 이벤트 작성

이 항목에서는 해당 잔액이 차변으로 전환될 때 이벤트가 발생하는 은행 계좌 &mdash;를 나타내는 런타임 클래스를 포함하는 Windows 런타임 구성 요소를 작성하는 방법에 대해서 설명합니다. 이 항목에서는 은행 계좌 런타임 클래스를 사용하면서 함수를 호출하여 잔액을 조정하거나, 발생하는 모든 이벤트를 처리하는 주요 앱에 대해서도 설명합니다.

> [!NOTE]
> 프로젝트 템플릿 및 빌드 지원을 함께 제공하는 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Visual Studio 확장(VSIX) 및 NuGet 패키지를 설치하고 사용하는 방법에 대한 자세한 내용은 [Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

> [!IMPORTANT]
> C++/WinRT를 사용해 런타임 클래스를 사용하거나 작성하는 방법을 더욱 쉽게 이해할 수 있는 필수 개념과 용어에 대해서는 [C++/WinRT를 통한 API 사용](consume-apis.md)과 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Windows 런타임 구성 요소(BankAccountWRC) 만들기

먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. **Windows 런타임 구성 요소(C++/WinRT)** 프로젝트를 만든 다음, 이름을 *BankAccountWRC*("은행 계좌 Windows 런타임 구성 요소"인 경우)로 지정합니다. 프로젝트 이름을 *BankAccountWRC*로 지정하면 이 항목의 나머지 단계를 가장 쉽게 수행할 수 있습니다. 아직 프로젝트를 빌드하지 마세요.

새로 만든 프로젝트에는 `Class.idl`이라는 이름의 파일이 포함되어 있습니다. 해당 파일의 이름을 바꿉니다`BankAccount.idl`(`.idl` 파일의 이름을 바꾸면 자동으로 종속 `.h` 및 `.cpp` 파일의 이름도 바뀜). `BankAccount.idl`의 콘텐츠를 아래 목록으로 바꿉니다.

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

파일을 저장합니다. 현재는 프로젝트 빌드가 완료되지 않지만, 지금 빌드하는 것은 **BankAccount** 런타임 클래스를 구현할 원본 코드 파일을 생성하므로 유용한 작업입니다. 따라서 계속해서 지금 빌드합니다(이 단계에서 표시될 것으로 예상되는 빌드 오류는 발견되는 `Class.h` 및 `Class.g.h`로 처리해야 함).

빌드 프로세스에서 `midl.exe` 도구가 실행되어 구성 요소의 Windows 런타임 메타데이터 파일(`\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`)을 생성합니다. 그런 다음, `cppwinrt.exe` 도구가 실행되어(`-component` 옵션과 함께) 구성 요소를 작성하도록 지원하는 원본 코드 파일을 생성합니다. 원본 코드 파일에는 IDL로 선언한 **BankAccount** 런타임 클래스의 구현을 시작할 수 있는 스텁이 포함됩니다. 이 스텁이 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h`와 `BankAccount.cpp`입니다.

프로젝트 노드를 마우스 오른쪽 단추로 클릭하고 **파일 탐색기에서 폴더 열기**를 클릭합니다. 파일 탐색기에서 프로젝트 폴더가 열립니다. 여기에서 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` 폴더에서 스텁 파일 `BankAccount.h` 및 `BankAccount.cpp`를 복사하여 프로젝트 파일을 포함하는 폴더인 `\BankAccountWRC\BankAccountWRC\`에 붙여넣고 대상에서 파일을 바꿉니다. 이제 `BankAccount.h`와 `BankAccount.cpp`를 열고 런타임 클래스를 구현합니다. `BankAccount.h`에서 프라이빗 멤버 두 개를 **BankAccount** 구현(팩터리 구현 *아님*)에 추가합니다.

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

위에서 살펴본 대로, 이벤트는 특정 대리자 형식으로 매개 변수가 있는 [**winrt::event**](/uwp/cpp-ref-for-winrt/event) 구조체 템플릿을 기반으로 구현됩니다.

`BankAccount.cpp`에서는 아래 코드 예제와 같이 함수를 구현합니다. C++/WinRT에서 IDL로 선언된 이벤트는 오버로드된 함수 세트로 구현됩니다(속성이 오버로드된 get 및 set 함수 세트로 구현되는 것과 유사한 방식). 한 오버로드는 등록할 대리자를 가지며 토큰을 반환합니다. 다른 오버로드는 토큰을 가지며 관련 대리자의 등록을 취소합니다.

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(winrt::event_token const& token) noexcept
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

> [!NOTE]
> 자동 이벤트 취소자에 대한 자세한 정보는 [등록된 대리자 취소](handle-events.md#revoke-a-registered-delegate)를 참조하세요. 이벤트에 대한 자동 이벤트 취소자 구현을 무료로 가져올 수 있습니다. 즉, 이벤트 취소자(C++/WinRT 프로젝션을 통해 제공)에 대한 오버로드를 구현할 필요가 없습니다.

다른 오버로드(등록 및 수동 해지 오버로드)는 프로젝션에 반영되지 *않습니다*. 이는 시나리오에 최적으로 구현할 수 있는 유연성을 제공하기 위한 것입니다. 이러한 구현에 나온 것처럼 [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) 및 [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) 호출은 효율적이며 동시성/스레드로부터 안전한 기본값입니다. 하지만 매우 많은 수의 이벤트를 처리하는 경우 각각에 대한 이벤트 필드 대신 일부 밀도가 낮은 구현을 원할 수 있습니다.

잔액이 마이너스가 될 경우 **AdjustBalance** 함수의 구현에서 **AccountIsInDebit** 이벤트가 발생하는 시나리오는 위에서 확인할 수 있습니다.

경고 메시지로 인해 빌드가 중단되는 경우에는 경고를 해결하거나 프로젝트 속성 **C/C++**  > **일반** > **경고를 오류로 처리**를 **아니요(/WX-)** 로 설정한 후 프로젝트를 다시 빌드합니다.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>주요 앱(BankAccountCoreApp)을 만들어 Windows 런타임 구성 요소 테스트

이제 *BankAccountWRC* 솔루션 또는 새로운 솔루션에서 새 프로젝트를 만듭니다. **주요 앱(C++/WinRT)** 프로젝트를 만들어서 이름을 *BankAccountCoreApp*이라고 지정합니다.

> [!NOTE]
> 앞에서 설명한 것처럼 Windows 런타임 구성 요소에 대한 Windows 런타임 메타데이터 파일(*BankAccountWRC*라는 프로젝트)이 `\BankAccountWRC\Debug\BankAccountWRC\` 폴더에 생성됩니다. 해당 경로의 첫 번째 세그먼트는 솔루션 파일이 포함된 폴더의 이름입니다. 다음 세그먼트는 `Debug`라는 하위 디렉터리입니다. 마지막 세그먼트는 Windows 런타임 구성 요소에 대해 이름이 지정된 하위 디렉터리입니다. 프로젝트 *BankAccountWRC*의 이름을 지정하지 않은 경우 메타데이터 파일은 `\<YourProjectName>\Debug\<YourProjectName>\` 폴더에 있습니다.

이제 핵심 앱 프로젝트(*BankAccountCoreApp*)에서 참조를 추가하고 `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`로 이동하거나, 두 프로젝트가 동일한 솔루션에 있는 경우 프로젝트 간 참조를 추가합니다. **추가**, **확인**을 차례로 클릭합니다. 이제 *BankAccountCoreApp*을 빌드합니다. 드물지만 페이로드 파일인 `readme.txt`가 존재하지 않는다는 오류가 표시되는 경우에는 해당 파일을 Windows 런타임 구성 요소 프로젝트에서 제외하여 다시 빌드한 다음, *BankAccountCoreApp*을 다시 빌드합니다.

빌드 프로세스에서 `cppwinrt.exe` 도구가 실행되면서 참조된 `.winmd` 파일을 원본 코드 파일로 처리합니다. 이 원본 코드 파일에는 구성 요소를 사용할 때 지원할 목적으로 프로젝션된 형식이 포함되어 있습니다. 구성 요소의 런타임 클래스에 맞게 프로젝션된 형식의 헤더(`BankAccountWRC.h`)가 `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\` 폴더로 생성됩니다.

이 헤더를 `App.cpp`에 추가합니다.

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

또한 `App.cpp`에서 다음 코드를 추가하여 **BankAccount**(프로젝션된 형식의 기본 생성자 사용)를 인스턴스화하고 이벤트 처리기를 등록한 다음, 계좌에서 차변이 발생하도록 합니다.

`WINRT_ASSERT`는 매크로 정의이며 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)로 확장됩니다.

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
            WINRT_ASSERT(balance < 0.f); // Put a breakpoint here.
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

이제 창을 클릭할 때마다 은행 계좌의 잔고에서 1이 감액됩니다. 이벤트가 예상한 대로 발생하는지 확인하려면 **AccountIsInDebit** 이벤트를 처리하는 람다 식에 중단점을 입력한 후 앱을 실행하여 창 내부를 클릭합니다.

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>ABI에서 매개 변수가 있는 대리자 및 단순 신호

구성 요소와 해당 소비 애플리케이션 사이 같은 ABI(Application Binary Interface)를 통해 이벤트에 액세스할 수 있어야 하는 경우 이벤트에는 Windows 런타임 대리자 형식이 사용되어야 합니다. 위 예제에서는 [**Windows::Foundation::EventHandler\<T\>** ](/uwp/api/windows.foundation.eventhandler) Windows 런타임 대리자 형식을 사용합니다. [**TypedEventHandler\<TSender, TResult\>** ](/uwp/api/windows.foundation.eventhandler)는 Windows 런타임 대리자 형식의 또 다른 예입니다.

두 대리자 형식의 형식 매개 변수는 ABI를 가로질러야 하므로 형식 매개 변수도 Windows 런타임 형식이어야 합니다. 여기에는 Microsoft 및 타사 런타임 클래스와 숫자, 문자열 등의 기본 형식이 포함됩니다. 해당 제약 조건을 잊을 경우 컴파일러가 “WinRT 형식이어야 합니다.” 오류를 처리하는 데 도움이 될 수 있습니다. 

이벤트를 통해 매개 변수나 인수를 전달할 필요가 없는 경우에는 고유한 단순 Windows 런타임 대리자 형식을 정의할 수 있습니다. 아래 예제에서는 **BankAccount** 런타임 클래스의 더 단순한 버전을 보여 줍니다. **SignalDelegate**라는 대리자 형식을 선언한 다음, 이 대리자 형식을 사용하여 매개 변수가 있는 이벤트 대신 신호 형식 이벤트를 발생시킵니다.

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

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>프로젝트 내의 매개 변수가 있는 대리자, 단순 신호 및 콜백
Visual Studio 프로젝트 내부에 있는 이벤트가 필요한 경우(이진 파일이 아닌) 해당 이벤트가 Windows 런타임 형식으로 제한되지 않는 경우에도 [**winrt::event**](/uwp/cpp-ref-for-winrt/event)\<Delegate\> 클래스 템플릿을 사용할 수 있습니다. **winrt::delegate**도 비 Windows 런타임 매개 변수를 지원하므로 간단히 실제 Windows 런타임 대리자 형식 대신 [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate)를 사용하면 됩니다.

아래 예제에서는 매개 변수(기본적으로 단순 신호)를 사용하지 않는 대리자 서명을 보여 준 후 문자열을 사용하는 대리자 서명을 보여 줍니다.

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

구독 대리자를 원하는 만큼 이벤트에 추가하는 방법을 알 수 있습니다. 그러나 이벤트와 연결된 몇 가지 오버헤드가 있습니다. 단일 구독 대리자만 사용하는 간단한 콜백만 필요한 경우에는 자체적으로 [**winrt::delegate&lt;... T&gt;** ](/uwp/cpp-ref-for-winrt/delegate)를 사용할 수 있습니다.

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

이벤트 및 대리자가 프로젝트 내에서 내부적으로 사용되는 C++/CX 코드베이스에서 이식하는 경우에는 **winrt::delegate**를 통해 C++/WinRT에서 해당 패턴을 복제할 수 있습니다.

## <a name="design-guidelines"></a>디자인 지침

대리자가 아닌 이벤트를 함수 매개 변수로 전달하는 것이 좋습니다. [**winrt::event**](/uwp/cpp-ref-for-winrt/event)의 **add** 함수는 하나의 예외입니다. 이 경우에는 대리자를 전달해야 하기 때문입니다. 대리자는 하나 또는 여러 개의 클라이언트 등록을 지원하는지 여부에 따라 다양한 Windows 런타임 언어에 걸쳐 다른 형식을 사용할 수 있기 때문에 이 지침이 제공됩니다. 여러 구독자 모델이 있는 이벤트는 훨씬 더 예측 가능하고 일관성 있는 옵션을 구성합니다.

이벤트 처리기 대리자의 서명은 두 개의 매개 변수인 ‘발신자’(**IInspectable**) 및 *args*([**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)와 같은 일부 이벤트 인수 형식)로 구성됩니다. 

내부 API를 디자인하는 경우에는 이 지침이 반드시 적용되는 것은 아닙니다. 그래도 내부 API는 일반적으로 시간이 지남에 따라 공개됩니다.

## <a name="related-topics"></a>관련 항목
* [C++/WinRT를 통한 API 작성](author-apis.md)
* [C++/WinRT를 통한 API 사용](consume-apis.md)
* [C++/WinRT의 대리자를 사용한 이벤트 처리](handle-events.md)