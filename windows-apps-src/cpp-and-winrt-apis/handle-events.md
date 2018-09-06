---
author: stevewhims
description: 이번 항목에서는 C++/WinRT를 사용해 이벤트 처리 대리자를 등록하거나 취소하는 방법에 대해서 설명합니다.
title: C++/WinRT의 대리자를 사용한 이벤트 처리
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션된, 프로젝션, 처리, 이벤트, 대리자
ms.localizationpriority: medium
ms.openlocfilehash: a29c095e49b49baa63bd547c0bb928ad7f78aa86
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2018
ms.locfileid: "3396694"
---
# <a name="handle-events-by-using-delegates-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)의 대리자를 사용한 이벤트 처리
이번 항목에서는 C++/WinRT를 사용해 이벤트 처리 대리자를 등록하거나 취소하는 방법에 대해서 설명합니다. 표준 C++ 함수와 같은 개체를 사용해 이벤트를 처리할 수 있습니다.

> [!NOTE]
> C++/WinRT Visual Studio Extension(VSIX)(프로젝트 템플릿 지원과 C++/WinRT MSBuild 속성 및 대상 제공)의 설치 및 사용에 대한 자세한 내용은 [C++/WinRT에 대한 Visual Studio 지원 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)를 참조하세요.

## <a name="register-a-delegate-to-handle-an-event"></a>이벤트 처리를 위한 대리자 등록
다음은 버튼의 클릭 이벤트를 처리하는 간단한 예제입니다. 멤버 함수를 등록하여 이러한 이벤트를 처리할 때는 XAML 태그를 사용하는 것이 일반적입니다.

```xaml
// MainPage.xaml
<Button x:Name="Button" Click="ClickHandler">Click Me</Button>
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    Button().Content(box_value(L"Clicked"));
}
```

태그에서 선언적으로 등록하지 않고 멤버 함수를 명령적으로 등록하여 이벤트를 처리할 수도 있습니다. 아래 코드 예제에서는 쉽게 알기 어렵지만 [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 호출에 대한 인수는 [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) 대리자 인스터스입니다. 이러한 경우에는 개체와 포인터-멤버 함수를 갖는 **RoutedEventHandler** 생성자 오버로드를 사용합니다.

```cppwinrt
// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click({ this, &MainPage::ClickHandler });
}
```

**RoutedEventHandler**를 생성하는 다른 방법도 있습니다. 아래는 [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) 문서 항목에서 가져온 구문 블록입니다(페이지의 **언어** 드롭다운에서 *C++/WinRT* 선택). 아래 예제를 보면 다양한 생성자가 있습니다. 하나는 람다 함수를, 다른 하나는 프리 함수를, 그리고 나머지 하나(위에서 사용한 것)는 개체와 포인터-멤버 함수를 갖습니다.

```cppwinrt
struct RoutedEventHandler : winrt::Windows::Foundation::IUnknown
{
    RoutedEventHandler(std::nullptr_t = nullptr) noexcept;
    template <typename L> RoutedEventHandler(L lambda);
    template <typename F> RoutedEventHandler(F* function);
    template <typename O, typename M> RoutedEventHandler(O* object, M method);
    void operator()(winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e) const;
};
```

함수 호출 연산자의 구문 역시 알아두는 것이 좋습니다. 대리자의 매개 변수로 무엇을 사용해야 할지 알 수 있기 때문입니다. 보다시피 이 경우에는 함수 호출 연산자 구문이 **MainPage::ClickHandler**의 매개 변수와 일치합니다.

이벤트 처리기의 작업이 많지 않다면 멤버 함수가 아닌 람다 함수를 사용하는 것도 좋습니다. 한 번 더 아래 코드 예제에서는 쉽게 알 수 없지만 **RoutedEventHandler** 대리자가 람다 함수에서 생성되고 있으며, 이 람다 함수는 마찬가지로 함수 호출 연산자의 구문과 일치해야 합니다.

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click([this](IInspectable const&, RoutedEventArgs const&)
    {
        Button().Content(box_value(L"Clicked"));
    });
}
```

대리자를 생성할 때는 좀 더 명시적으로 선택할 수 있습니다. 예를 들어, 대리자를 전달하거나, 1회 이상 사용하는 경우가 그렇습니다.

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();

    auto click_handler = [](IInspectable const& sender, RoutedEventArgs const&)
    {
        sender.as<winrt::Windows::UI::Xaml::Controls::Button>().Content(box_value(L"Clicked"));
    };
    Button().Click(click_handler);
    AnotherButton().Click(click_handler);
}
```

## <a name="revoke-a-registered-delegate"></a>등록된 대리자 취소
대리자를 등록하면 일반적으로 토큰이 반환됩니다. 이후 반환된 토큰을 사용하여 대리자를 취소할 수 있습니다. 이 말은 대리자가 이벤트에서 등록 해제된 후 이벤트가 다시 발생하는 경우에는 대리자를 취소할 수 없다는 것을 의미합니다. 쉽게 말해서 위의 코드 예제에는 어디에도 취소하는 방법이 나와있지 않습니다. 하지만 다음 코드 예제에서는 구조체의 private 데이터 멤버에 토큰을 저장한 후 소멸자에서 처리기를 취소합니다.

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button const& button) : m_button(button)
    {
        m_token = m_button.Click([this](IInspectable const&, RoutedEventArgs const&)
        {
            ...
        });
    }
    ~Example()
    {
        m_button.Click(m_token);
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button m_button;
    winrt::event_token m_token;
};
```

위의 예와 같이 강력한 참조 대신 버튼에 약한 참조를 저장할 수 있습니다([C++/WinRT의 약한 참조](weak-references.md) 참조).

또는 대리자를 등록할 때 (즉, 값이 형식을 [**winrt::auto_revoke_t**](/uwp/cpp-ref-for-winrt/auto-revoke-t)) **그** 는 이벤트 취소 자 (요청 형식 [**winrt:: event_revoker**](/uwp/cpp-ref-for-winrt/event-revoker))를 지정할 수 있습니다. 이벤트 취소 자를 수에 대 한 이벤트 소스 (이벤트를 발생 시키는 개체)에 대 한 약한 참조를 보유 합니다. **event_revoker::revoke** 멤버 함수를 호출하여 수동으로 취소할 수 있지만 함수가 범위를 벗어나면 이벤트 취소자는 자동으로 그 함수를 호출합니다. **취소** 함수는 이벤트 소스가 여전히 존재하는지 확인합니다. 존재하는 경우 대리인을 취소합니다. 이번 예제에서는 이벤트 소스를 저장할 필요도 없고, 소멸자도 필요 없습니다.

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button button)
    {
        m_event_revoker = button.Click(winrt::auto_revoke, [this](IInspectable const&, RoutedEventArgs const&)
        {
            ...
        });
    }

private:
    winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase> m_event_revoker;
};
```

아래는 [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트의 문서 항목에서 가져온 구분 블록입니다. 블록을 보면 서로 다른 등록 및 취소 함수가 3개 있습니다. 세 번째 오버로드에서 어떤 유형의 이벤트 취소자를 선언해야 할지 정확히 알 수 있습니다.

```cppwinrt
// Register
winrt::event_token Click(winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;

// Revoke with event_token
void Click(winrt::event_token const& token) const;

// Revoke with event_revoker
winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase> Click(winrt::auto_revoke_t,
    winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;
```

비슷한 패턴이 모든 C++/WinRT 이벤트에 적용됩니다.

페이지 탐색 시나리오에서는 처리기 취소를 고려할 수 있습니다. 페이지 탐색 후 다른 페이지 탐색이 반복될 경우에는 페이지에서 다른 페이지를 탐색할 때 처리기를 취소할 수 있습니다. 또는 동일한 페이지 인스턴스를 다시 사용하는 경우에는 토큰 값을 확인하여 아직 설정되지 않은 경우에만 등록합니다(`if (!m_token){ ... }`). 세 번째 옵션은 이벤트 취소자를 데이터 멤버로 페이지에 저장하는 것입니다. 마지막으로 네 번째 옵션은 이번 항목 후반에 설명하겠지만 람다 함수에서 강한 또는 약한 참조를 *this* 개체로 캡처하는 것입니다.

## <a name="delegate-types-for-asynchronous-actions-and-operations"></a>비동기 작업을 위한 대리자 유형
위의 예제에서는 **RoutedEventHandler** 대리자 유형을 사용하였지만 그 밖에도 다른 대리자 유형이 많습니다. 예를 들어, 진행률 유무에 상관없이 비동기 작업은 완료되었거나 진행 중이면서 해당 유형의 대리자가 필요한 이벤트가 있습니다. 진행률이 포함된 비동기 작업에서 진행 중인 이벤트([**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)를 구현하는 모든 것)는 [**AsyncOperationProgressHandler**](/uwp/api/windows.foundation.asyncoperationprogresshandler) 유형의 대리자가 필요합니다. 다음은 람다 함수를 사용해 해당 유형의 대리자를 작성하는 코드 예제입니다. 이 예제에는 [**AsyncOperationWithProgressCompletedHandler**](/uwp/api/windows.foundation.asyncoperationwithprogresscompletedhandler) 대리자를 작성하는 방법도 나와있습니다.

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    auto async_op_with_progress = syndicationClient.RetrieveFeedAsync(rssFeedUri);

    async_op_with_progress.Progress(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const&, RetrievalProgress const& args)
    {
        uint32_t bytes_retrieved = args.BytesRetrieved;
        // use bytes_retrieved;
    });

    async_op_with_progress.Completed(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& sender, AsyncStatus const)
    {
        SyndicationFeed syndicationFeed = sender.GetResults();
        // use syndicationFeed;
    });
    
    // or (but this function must then be a coroutine and return IAsyncAction)
    // SyndicationFeed syndicationFeed{ co_await async_op_with_progress };
}
```

위의 "코루틴" 주석에서도 알 수 있듯이 비동기 작업에서 완료된 이벤트에 대리자를 사용하지 않아도 코루틴을 더욱 자연스럽게 사용할 수 있다는 것을 알 수 있습니다. 자세한 내용과 코드 예제는 [C++/WinRT로 동시성 및 비동기 작업](concurrency.md)을 참조하세요.

하지만 대리자를 계속 사용하는 경우 간단한 구문을 선택할 수 있습니다.

```cppwinrt
async_op_with_progress.Completed(
    [](auto&& /*sender*/, AsyncStatus const)
{
    ....
});
```

## <a name="delegate-types-that-return-a-value"></a>값을 반환하는 대리자 유형
일부 대리자 유형은 스스로 값을 반환해야 합니다. 한 예로 문자열을 반환하는 [**ListViewItemToKeyHandler**](/uwp/api/windows.ui.xaml.controls.listviewitemtokeyhandler)가 있습니다. 다음은 해당 유형의 대리자를 작성하는 예제입니다(단, 람다 함수가 값을 반환합니다).

```cppwinrt
using namespace winrt::Windows::UI::Xaml::Controls;

winrt::hstring f(ListView listview)
{
    return ListViewPersistenceHelper::GetRelativeScrollPosition(listview, [](IInspectable const& item)
    {
        return L"key for item goes here";
    });
}
```

## <a name="using-the-this-object-in-an-event-handler"></a>이벤트 처리기에서 *this* 개체 사용
개체의 멤버 함수 내에 있는 람다 함수에서 이벤트를 처리하는 경우에는 이벤트 수신자(이벤트를 처리하는 개체)와 이벤트 소스(이벤트를 발생시키는 개체)의 상대적 수명에 대해서 생각해야 합니다

대부분 경우에 수신자의 수명이 지정된 람다 함수의 *this* 포인터에 대한 모든 종속성보다 깁니다. 이러한 경우 중 일부는, 예를 들어 UI 페이지가 페이지의 컨트롤에서 발생한 이벤트를 처리할 때 같은 경우에 분명해집니다. 버튼 수명은 페이지보다 길지 못하며, 처리기 역시 마찬가지입니다. 이는 수신자가 소스(데이터 멤버 등)를 소유하고 있거나, 혹은 수신자와 소스가 서로 형제여서 다른 개체에 직접 종속되는 경우에도 동일하게 적용됩니다. 처리기의 수명이 종속되는 *this*보다 짧은 경우가 있다고 확신한다면 강하거나 약한 수명을 고려할 필요 없이 *this*를 정상적으로 캡처할 수 있습니다.

하지만 *this* 수명이 처리기에서 사용하는 것보다 짧은 경우도 존재합니다(비동기 작업에서 발생하는 완료/진행 이벤트의 처리기 포함).

- 코루틴을 작성하여 비동기 메서드를 구현하는 것도 가능합니다.
- 일부 XAML UI 프레임워크 개체(예: [**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel))를 사용하는 것은 드물지만 수신자가 이벤트 소스에서 등록 해제하지 않고 작업을 마쳤다면 가능합니다.

이러한 경우에는 처리기의 코드에서, 혹은 잘못된 *this* 개체를 계속해서 사용하려는 코루틴의 시도에서 액세스 위반이 발생합니다.

> [!IMPORTANT]
> 이러한 상황 중 한 가지에 부딪힌다면 *this* 개체의 수명을 비롯해 캡처된 *this* 개체의 수명이 캡처보다 긴지도 생각해봐야 합니다. 만약 그렇지 않다면 상황에 따라 강한 또는 약한 참조를 사용해 캡처하세요. [**implements::get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) 및 [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)를 참조하세요.
> 그 밖에도 시나리오에 적합하거나, 혹은 스레딩 고려 사항에 따라 가능하다면 수신자가 이벤트를 끝낸 후 또는 수신자의 소멸자에서 처리기를 취소하는 방법도 있습니다.

다음 코드 예제에서는 설명을 위해 [**SwapChainPanel.CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) 이벤트를 사용합니다. 이 이벤트는 수신자에 대한 약한 참조를 캡처하는 람다 함수를 사용해 이벤트 처리기를 등록합니다. 약한 참조에 대한 자세한 내용은 [C++/WinRT의 약한 참조](weak-references.md)를 참조하세요. 

```cppwinrt
winrt::Windows::UI::Xaml::Controls::SwapChainPanel m_swapChainPanel;
winrt::event_token m_compositionScaleChangedEventToken;

void RegisterEventHandler()
{
    m_compositionScaleChangedEventToken = m_swapChainPanel.CompositionScaleChanged([weakReferenceToThis{ get_weak() }]
        (Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
        Windows::Foundation::IInspectable const& object)
    {
        if (auto strongReferenceToThis = weakReferenceToThis.get())
        {
            strongReferenceToThis->OnCompositionScaleChanged(sender, object);
        }
    });
}

void OnCompositionScaleChanged(Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
    Windows::Foundation::IInspectable const& object)
{
    // Here, we know that the "this" object is valid.
}
```

람다 캡처 절에서는 *this*에 대한 약한 참조를 나타내는 임시 변수가 생성됩니다. 람다 함수의 본문에서는 *this*에 대한 강한 참조를 얻을 수 있다면 **OnCompositionScaleChanged** 함수가 호출됩니다. 이러한 방식으로 **OnCompositionScaleChanged** 내부에서는 *this*를 안전하게 사용할 수 있습니다.

## <a name="important-apis"></a>중요 API
* [winrt::auto_revoke_t](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [winrt::implements::get_weak 함수](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [winrt::implements::get_strong 함수](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT의 이벤트 작성](author-events.md)
* [C++/WinRT로 동시성 및 비동기 작업](concurrency.md)
* [C++/WinRT의 약한 참조](weak-references.md)
