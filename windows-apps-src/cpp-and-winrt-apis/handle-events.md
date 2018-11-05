---
author: stevewhims
description: 이번 항목에서는 C++/WinRT를 사용해 이벤트 처리 대리자를 등록하거나 취소하는 방법에 대해서 설명합니다.
title: C++/WinRT의 대리자를 사용한 이벤트 처리
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션된, 프로젝션, 처리, 이벤트, 대리자
ms.localizationpriority: medium
ms.openlocfilehash: 9ca257de29be8e732e05c343a4b782b1676627bf
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6052298"
---
# <a name="handle-events-by-using-delegates-in-cwinrt"></a>C++/WinRT의 대리자를 사용한 이벤트 처리

이 항목에서는 등록 하 고 사용 하 여 이벤트 처리 대리자를 해지 하는 방법을 보여 줍니다 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). 표준 C++ 함수와 같은 개체를 사용해 이벤트를 처리할 수 있습니다.

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
void MainPage::ClickHandler(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
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

> [!IMPORTANT]
> 대리자를 등록, 위의 코드 예제는 원시 *이* 포인터 (현재 개체를 가리킴)로 전달 합니다. 강한 또는 약한 참조 현재 개체를 설정 하는 방법을 알아보려면 [ *이* 포인터 이벤트 처리 대리자를 사용 하 여 안전 하 게 액세스](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)섹션에서 **멤버 함수를 대리인으로 사용 하는 경우** 하위 섹션을 참조 하세요.

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

    Button().Click([this](IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
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

    auto click_handler = [](IInspectable const& sender, RoutedEventArgs const& /* args */)
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
            // ...
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

위의 예와 같이 강력한 참조 대신 버튼에 대 한 약한 참조를 저장할 수 있습니다 (참조 [강력 하 고 약한 참조를 C + + WinRT](weak-references.md)).

또는 대리자를 등록할 때 유형 [**winrt:: event_revoker**](/uwp/cpp-ref-for-winrt/event-revoker)) (of 이벤트 취소 자를 요청 **auto_revoke** (즉, 형식 [**winrt:: auto_revoke_t**](/uwp/cpp-ref-for-winrt/auto-revoke-t)값)를 지정할 수 있습니다. 이벤트 취소 자를 수에 대 한 이벤트 소스 (이벤트를 발생 시키는 개체)에 대 한 약한 참조를 보유 합니다. **event_revoker::revoke** 멤버 함수를 호출하여 수동으로 취소할 수 있지만 함수가 범위를 벗어나면 이벤트 취소자는 자동으로 그 함수를 호출합니다. **취소** 함수는 이벤트 소스가 여전히 존재하는지 확인합니다. 존재하는 경우 대리인을 취소합니다. 이번 예제에서는 이벤트 소스를 저장할 필요도 없고, 소멸자도 필요 없습니다.

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button button)
    {
        m_event_revoker = button.Click(winrt::auto_revoke, [this](IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
        {
            // ...
        });
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button::Click_revoker m_event_revoker;
};
```

아래는 [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트의 문서 항목에서 가져온 구분 블록입니다. 블록을 보면 서로 다른 등록 및 취소 함수가 3개 있습니다. 세 번째 오버로드에서 어떤 유형의 이벤트 취소자를 선언해야 할지 정확히 알 수 있습니다.

```cppwinrt
// Register
winrt::event_token Click(winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;

// Revoke with event_token
void Click(winrt::event_token const& token) const;

// Revoke with event_revoker
Button::Click_revoker Click(winrt::auto_revoke_t,
    winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;
```

> [!NOTE]
> 위의 코드 예제에서 `Button::Click_revoker` 에 대 한 형식 별칭은 `winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase>`. 비슷한 패턴이 모든 C++/WinRT 이벤트에 적용됩니다. 각 Windows 런타임 이벤트에 이벤트 취소 자의 반환 하 고 취소의 형식이 이벤트 소스의 구성원 revoke 함수가 오버 로드 합니다. 따라서 또 다른 예로, 할 [**corewindow:: Sizechanged**](/uwp/api/windows.ui.core.corewindow.sizechanged) 이벤트에 **CoreWindow::SizeChanged_revoker**형식의 값을 반환 하는 등록 함수 오버 로드 합니다.


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
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& /* sender */, RetrievalProgress const& args)
    {
        uint32_t bytes_retrieved = args.BytesRetrieved;
        // use bytes_retrieved;
    });

    async_op_with_progress.Completed(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& sender, AsyncStatus const /* asyncStatus */)
    {
        SyndicationFeed syndicationFeed = sender.GetResults();
        // use syndicationFeed;
    });
    
    // or (but this function must then be a coroutine, and return IAsyncAction)
    // SyndicationFeed syndicationFeed{ co_await async_op_with_progress };
}
```

위의 "코루틴" 주석에서도 알 수 있듯이 비동기 작업에서 완료된 이벤트에 대리자를 사용하지 않아도 코루틴을 더욱 자연스럽게 사용할 수 있다는 것을 알 수 있습니다. 자세한 내용과 코드 예제는 [C++/WinRT로 동시성 및 비동기 작업](concurrency.md)을 참조하세요.

> [!NOTE]
> 비동기 작업 또는 작업에 대 한 둘 이상의 *완료 처리기* 를 구현 하는 것이 올바르지 합니다. 완료 된 이벤트에 대 한 단일 대리자 하거나 할 수 있습니다 `co_await` 것입니다. 둘 다가 경우 두 번째 실패 합니다.

코 루틴 대신 대리자를 사용 하 여 스틱, 하는 경우 간단한 구문을 선택할 수 있습니다.

```cppwinrt
async_op_with_progress.Completed(
    [](auto&& /*sender*/, AsyncStatus const /* args */)
{
    // ...
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

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>이벤트 처리 대리자를 사용 하 여 *이* 포인터를 안전 하 게 액세스

개체의 멤버 함수를 사용 하 여 이벤트를 처리 하거나에서 개체의 멤버 함수 내에 있는 람다 함수 내에서 다음 필요한 경우 이벤트 수신자 (이벤트를 처리 하는 개체)와 이벤트 소스 (개체의 상대적 수명에 대 한 생각 하는 경우 이벤트를 발생). 자세한 내용은 및 코드 예제를 참조 하세요 [강력 하 고 약한 참조를 C + + WinRT](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate).

## <a name="important-apis"></a>중요 API
* [winrt:: auto_revoke_t 마커 구조체](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [winrt::implements::get_weak 함수](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [winrt::implements::get_strong 함수](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT의 이벤트 작성](author-events.md)
* [C++/WinRT로 동시성 및 비동기 작업](concurrency.md)
* [강력 하 고 약한 참조를 C + + WinRT](weak-references.md)
