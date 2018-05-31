---
author: stevewhims
description: 이번 항목에서는 이벤트가 발생하는 런타임 클래스를 포함해 Windows 런타임 구성 요소를 작성하는 방법에 대해서 설명합니다. 또한 구성 요소를 사용하여 이벤트를 처리하는 앱에 대해서도 설명합니다.
title: C++/WinRT의 이벤트 작성
ms.author: stwhi
ms.date: 04/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 작성, 이벤트
ms.localizationpriority: medium
ms.openlocfilehash: b7574f1a3406dae665ced80294f7bc1cf91aeb8c
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832027"
---
# <a name="author-events-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)의 이벤트 작성
> [!NOTE]
> **일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.**

이번 항목에서는 차변 발생 시 이벤트가 발생하는 은행 계좌 런타임 클래스를 포함해 Windows 런타임 구성 요소를 작성하는 방법에 대해서 설명합니다. 또한 은행 계좌 런타임 클래스를 사용하면서 함수를 호출하여 잔액을 조정하거나, 발생하는 이벤트를 처리하는 주요 앱에 대해서도 설명합니다.

> [!NOTE]
> C++/WinRT Visual Studio Extension(VSIX)(프로젝트 템플릿 지원과 C++/WinRT MSBuild 속성 및 대상 제공)의 현재 가용성에 대한 자세한 내용은 [C++/WinRT에 대한 Visual Studio 지원 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)를 참조하세요.

> [!IMPORTANT]
> C++/WinRT를 사용해 런타임 클래스를 사용하거나 작성하는 방법을 더욱 쉽게 이해할 수 있는 필수 개념과 용어에 대해서는 [C++/WinRT를 통한 API 사용](consume-apis.md)과 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="windowsfoundationeventhandlerlttgt-and-typedeventhandlerlttgt"></a>Windows::Foundation::EventHandler&lt;T&gt; 및 TypedEventHandler&lt;T&gt;
Windows 런타임 구성 요소에서 구현된 런타임 클래스의 이벤트가 발생하도록 하려면 이벤트의 대리자 형식으로 [**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler) 또는 [**TypedEventHandler**](/uwp/api/windows.foundation.eventhandler)를 사용해야 합니다. 형식 매개 변수는 Windows 런타임 형식이어야 하므로 타사 런타임 클래스 외에 기본 형식도 허용됩니다.

이러한 제약 조건을 잊을 경우 컴파일러가 "*must be WinRT type*" 오류 메시지를 처리하는 데 도움이 될 수 있습니다.

## <a name="winrtdelegatelttgt"></a>winrt::delegate&lt;...T&gt;
C++ 형식(동일한 프로젝트에서 작성 및 사용됨)의 이벤트가 발생하도록 하려면 C++/WinRT의 **winrt::delegate&lt;...T&gt;** 를 이벤트의 대리자 형식으로 사용할 수 있습니다. 이때는 입력 매개 변수가 Windows 런타임 형식일 필요 없습니다.

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Windows 런타임 구성 요소(BankAccountWRC) 만들기
먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. **Visual C++ Windows Runtime Component (C++/WinRT)** 프로젝트를 만든 다음 이름을 *BankAccountWRC*("은행 계좌 Windows 런타임 구성 요소"인 경우)로 지정합니다.

새로 만든 프로젝트에는 `Class.idl`이라는 이름의 파일이 포함되어 있습니다. 파일 이름을 `BankAccountWRC.idl`로 변경합니다. 그러면 빌드 과정에서 구성 요소의 Windows 런타임 메타데이터 파일 이름이 구성 요소를 따라 자동으로 지정됩니다. `BankAccountWRC.idl`에서 아래 나열한 것과 같이 인터페이스를 정의합니다.

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

파일을 저장하고 프로젝트를 빌드합니다. 이때 빌드가 완료되지는 않습니다. 하지만 빌드 과정에서 `midl.exe` 도구가 실행되어 구성 요소의 Windows 런타임 메타데이터 파일(`\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`)을 생성합니다. 그런 다음 `cppwinrt.exe` 도구가 실행되어(`-component` 옵션과 함께) 구성 요소를 작성하도록 지원하는 소스 코드 파일을 생성합니다. 소스 코드 파일에는 IDL로 선언한 `BankAccount` 런타임 클래스의 구현을 시작할 수 있는 스텁이 포함됩니다. 이 스텁이 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h`와 `BankAccount.cpp`입니다.

스텁 파일인 `BankAccount.h`와 `BankAccount.cpp`를 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\`에서 프로젝트 폴더인 `\BankAccountWRC\BankAccountWRC\`로 복사합니다. **솔루션 탐색기**에서 **모든 파일 표시**가 설정되어 있는지 확인합니다. 복사한 스텁 파일을 마우스 오른쪽 버튼으로 클릭하고 **프로젝트에 포함**을 클릭합니다. `Class.h`와 `Class.cpp`를 마우스 오른쪽 버튼으로 클릭한 후 **프로젝트에서 제외**를 클릭합니다.

이제 `BankAccount.h`와 `BankAccount.cpp`를 열고 런타임 클래스를 구현합니다. `BankAccount.h`에서 전용 멤버 2개를 BankAccount 구현체(팩터리 구현체 *아님*)에 추가합니다.

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        event<Windows::Foundation::EventHandler<float>> accountIsInDebitEvent;
        float balance{ 0.f };
    };
}
...
```

`BankAccount.cpp`에서 아래와 같이 함수를 구현합니다.

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(event_token const& token)
    {
        accountIsInDebitEvent.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        balance += value;
        if (balance < 0.f) accountIsInDebitEvent(*this, balance);
    }
}
```

잔액이 마이너스가 될 경우 **AdjustBalance** 함수의 구현체에서 **AccountIsInDebit** 이벤트가 발생합니다.

경고 메시지로 인해 빌드가 중단되는 경우에는 프로젝트 속성 **C/C++** > **일반** > **경고를 오류로 처리**를 **아니오(/WX-)** 로 설정한 후 프로젝트를 다시 빌드하세요.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>주요 앱(BankAccountCoreApp)을 만들어 Windows 런타임 구성 요소 테스트
이제 새 프로젝트를 만듭니다(`BankAccountWRC` 솔루션에서, 혹은 새로운 솔루션에서). **Visual C++ Core App (C++/WinRT)** 프로젝트를 만들어서 이름을 *BankAccountCoreApp*이라고 지정합니다.

참조를 추가하고 `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`로 이동합니다. **추가**와 **확인**을 차례대로 클릭합니다. 이제 BankAccountCoreApp을 빌드합니다. 페이로드 파일인 `readme.txt`가 존재하지 않는다는 오류 메시지가 표시되면 해당 파일을 Windows 런타임 구성 요소 프로젝트에서 제외하여 리빌드한 후 BankAccountCoreApp을 리빌드합니다.

빌드 과정에서 `cppwinrt.exe` 도구가 실행되면서 참조된 `.winmd` 파일을 소스 코드 파일로 처리합니다. 이 소스 코드 파일에는 구성 요소를 사용할 때 지원할 목적으로 프로젝션된 형식이 포함되어 있습니다. 구성 요소의 런타임 클래스에 맞게 프로젝션된 형식의 헤더(`BankAccountWRC.h`)가 `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\` 폴더로 생성됩니다.

이 헤더를 `App.cpp`에 추가합니다.

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

또한 `App.cpp`에서 다음 코드를 추가하여 BankAccount(프로젝션된 형식의 기본 생성자 사용)를 인스턴스화하고, 이벤트 처리기를 등록하고, 계좌에서 차변이 발생하도록 합니다.

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    event_token eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        eventToken = bankAccount.AccountIsInDebit([](const auto &, float balance)
        {
            assert(balance < 0.f);
        });
    }
    ...

    void Uninitialize()
    {
        bankAccount.AccountIsInDebit(eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

이제 창을 클릭할 때마다 은행 계좌의 잔고에서 1이 감액됩니다. 이벤트가 예상한 대로 발생하는지 확인하려면 람다 식에 중단점을 입력한 후 앱을 실행하여 창 내부를 클릭합니다.

## <a name="related-topics"></a>관련 항목
* [C++/WinRT를 통한 API 작성](author-apis.md)
* [C++/WinRT를 통한 API 사용](consume-apis.md)
* [C++/WinRT의 대리자를 사용한 이벤트 처리](handle-events.md)
