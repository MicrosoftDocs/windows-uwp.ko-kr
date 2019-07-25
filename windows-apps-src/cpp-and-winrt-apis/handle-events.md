---
description: C++/WinRT를 사용하여 이벤트 처리 대리자를 등록하거나 취소하는 방법을 보여 줍니다.
title: C++/WinRT의 대리자를 사용한 이벤트 처리
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션된, 프로젝션, 처리, 이벤트, 대리자
ms.localizationpriority: medium
ms.openlocfilehash: b64fbe93198af95402161873c1d68d0da41f33f7
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270110"
---
# <a name="handle-events-by-using-delegates-in-cwinrt"></a>C++/WinRT의 대리자를 사용한 이벤트 처리

본 항목에서는 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)를 사용하여 이벤트 처리 대리자를 등록하거나 취소하는 방법을 보여 줍니다. 표준 C++ 함수와 같은 개체를 사용해 이벤트를 처리할 수 있습니다.

> [!NOTE]
> 프로젝트 템플릿 및 빌드 지원을 함께 제공하는 C++/WinRT Visual Studio 확장(VSIX) 및 NuGet 패키지를 설치하고 사용하는 방법에 대한 자세한 내용은 [Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

## <a name="using-visual-studio-2019-to-add-an-event-handler"></a>Visual Studio 2019를 사용하여 이벤트 처리기 추가

프로젝트에 이벤트 처리기를 추가하는 편리한 방법은 Visual Studio 2019에서 XAML 디자이너 사용자 인터페이스(UI)를 사용하는 것입니다. XAML 디자이너에서 XAML 페이지를 열고 이벤트를 처리하려는 컨트롤을 선택합니다. 해당 컨트롤에 대한 속성 페이지에서 번개 아이콘을 클릭하여 해당 컨트롤에서 소싱되는 모든 이벤트를 나열합니다. 그런 다음, 처리하려는 이벤트(예: *OnClicked*)를 두 번 클릭합니다.

고유한 구현으로 바꿀 준비가 된 사용자 소스 파일에 XAML 디자이너가 적절한 이벤트 처리기 함수 프로토타입(및 스텁 구현)을 추가합니다.

> [!NOTE]
> 일반적으로 사용자 이벤트 처리기는 Midl 파일(`.idl`)에서 설명할 필요가 없습니다. 따라서 XAML 디자이너는 사용자 Midl 파일에 이벤트 처리기 함수 프로토타입을 추가하지 않습니다. `.h` 및 `.cpp` 파일에만 추가합니다.

## <a name="register-a-delegate-to-handle-an-event"></a>이벤트 처리를 위한 대리자 등록

다음은 단추의 클릭 이벤트를 처리하는 간단한 예제입니다. 멤버 함수를 등록하여 이와 같은 이벤트를 처리할 때는 XAML 태그를 사용하는 것이 일반적입니다.

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

태그에서 선언적으로 등록하지 않고 멤버 함수를 명령적으로 등록하여 이벤트를 처리할 수도 있습니다. 아래 코드 예제에서는 쉽게 알기 어렵지만 [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 호출에 대한 인수는 [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) 대리자 인스턴스입니다. 이 경우에는 개체와 멤버 포인터 함수를 사용하는 **RoutedEventHandler** 생성자 오버로드를 사용합니다.

```cppwinrt
// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click({ this, &MainPage::ClickHandler });
}
```

> [!IMPORTANT]
> 대리자를 등록할 때 위의 코드 예제에서는 현재 개체를 가리키는 원시 *this* 포인터를 전달합니다. 현재 개체에 대한 강력한 참조 또는 약한 참조를 설정하는 방법을 알아보려면 [멤버 함수를 대리자로 사용하는 경우](weak-references.md#if-you-use-a-member-function-as-a-delegate)를 참조하세요.

**RoutedEventHandler**를 생성하는 다른 방법도 있습니다. 아래는 [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) 문서 항목에서 가져온 구문 블록입니다(웹 페이지의 오른쪽 위 모서리에 있는 **언어** 드롭다운에서 *C++/WinRT* 선택). 아래 예제를 보면 다양한 생성자가 있습니다. 하나는 람다 함수를, 다른 하나는 프리 함수를, 그리고 나머지 하나(위에서 사용한 것)는 개체와 멤버 포인터 함수를 사용합니다.

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

> [!NOTE]
> 지정된 이벤트의 경우 해당 대리자 매개 변수의 세부 정보를 확인하려면 먼저 이벤트 자체에 대한 문서 항목으로 이동합니다. [UIElement.KeyDown 이벤트](/uwp/api/windows.ui.xaml.uielement.keydown)를 예제로 사용해 보겠습니다. 해당 항목을 방문하여 **언어** 드롭다운에서 *C++/WinRT*를 선택합니다. 항목의 시작 부분에 있는 구문 블록에는 다음이 표시됩니다.
> 
> ```cppwinrt
> // Register
> event_token KeyDown(KeyEventHandler const& handler) const;
> ```
>
> 위 정보는 **UIElement.KeyDown** 이벤트(현재 항목)에 대리자 형식의 **KeyEventHandler**가 있다는 것을 알려 줍니다. KeyEventHandler가 이 이벤트 유형에 대리자를 등록할 때 전달하는 형식이기 때문입니다. 따라서 이제 항목의 링크에 따라 해당 [KeyEventHandler delegate](/uwp/api/windows.ui.xaml.input.keyeventhandler) 형식으로 이동합니다. 여기서 구문 블록은 함수 호출 연산자를 포함합니다. 위에서 언급한 것처럼 대리자의 매개 변수로 무엇을 사용해야 할지 알려 줍니다.
> 
> ```cppwinrt
> void operator()(winrt::Windows::Foundation::IInspectable const& sender, winrt::Windows::UI::Xaml::Input::KeyRoutedEventArgs const& e) const;
> ```
>
>  살펴본 대로 대리자는 **IInspectable**을 sender로 사용하고 [KeyRoutedEventArgs 클래스](/uwp/api/windows.ui.xaml.input.keyroutedeventargs) 인스턴스를 args로 사용하도록 선언해야 합니다.
>
> 또 다른 예제로 [Popup.Closed 이벤트](/uwp/api/windows.ui.xaml.controls.primitives.popup.closed)를 살펴보겠습니다. 해당 대리자 형식은 [EventHandler\<IInspectable\>](/uwp/api/windows.foundation.eventhandler)입니다. 따라서 대리자는 **IInspectable**을 sender로 사용하고 또 다른 **IInspectable**(**EventHandler**의 형식 매개 변수이므로)을 args로 사용합니다.

이벤트 처리기의 작업이 많지 않다면 멤버 함수가 아닌 람다 함수를 사용하는 것도 좋습니다. 그래도 아래 코드 예제에서는 쉽게 알 수 없지만 **RoutedEventHandler** 대리자가 람다 함수에서 생성되고 있으며, 이 람다 함수도 위에서 설명한 함수 호출 연산자의 구문과 일치해야 합니다.

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

대리자를 생성할 때는 좀 더 명시적으로 선택할 수 있습니다. 예를 들어 대리자를 전달하거나, 2회 이상 사용하는 경우가 그렇습니다.

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

대리자를 등록하면 일반적으로 토큰이 반환됩니다. 이후 반환된 토큰을 사용하여 대리자를 취소할 수 있습니다. 이 말은 대리자가 이벤트에서 등록 해제된 후 이벤트가 다시 발생하는 경우에는 대리자를 취소할 수 없다는 것을 의미합니다.

쉽게 설명하기 위해 위의 코드 예제에는 어디에도 취소하는 방법이 나와있지 않습니다. 하지만 다음 코드 예제에서는 구조체의 프라이빗 데이터 멤버에 토큰을 저장한 후 소멸자에서 처리기를 취소합니다.

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

위의 예제와 같이 강력한 참조 대신 단추에 약한 참조를 저장할 수 있습니다([C++/WinRT의 강력한 참조 및 약한 참조](weak-references.md) 참조).

> [!NOTE]
> 이벤트 원본에서 해당 이벤트를 동기적으로 발생시키면 처리기를 취소하고 이벤트를 더 이상 받지 않을 것임을 확신할 수 있습니다. 그러나 비동기 이벤트의 경우 해지 후에도(특히 소멸자 내에서 해지하는 경우) 진행 중인 이벤트에서 소멸을 시작한 후 개체에 도달할 수 있습니다. 소멸하기 전에 구독을 취소할 장소를 찾으면 문제가 완화될 수 있습니다. 또는 강력한 해결 방법으로 [이벤트 처리 대리자를 사용하여 안전하게 *this* 포인터 액세스](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)를 참조하세요.

그 밖에 대리자를 등록할 때 **winrt::auto_revoke**([**winrt::auto_revoke_t**](/uwp/cpp-ref-for-winrt/auto-revoke-t) 형식의 값)를 지정하여 이벤트 취소자([**winrt::event_revoker**](/uwp/cpp-ref-for-winrt/event-revoker) 형식)를 요청하는 방법도 있습니다. 이벤트 취소자는 이벤트 원본(이벤트를 발생하게 만든 개체)에 대한 약한 참조를 보유합니다. **event_revoker::revoke** 멤버 함수를 호출하여 수동으로 취소할 수 있지만 함수가 범위를 벗어나면 이벤트 취소자는 자동으로 그 함수를 호출합니다. **취소** 함수는 이벤트 원본이 여전히 존재하는지 확인합니다. 존재하는 경우 대리자를 취소합니다. 이번 예제에서는 이벤트 원본을 저장할 필요도 없고, 소멸자도 필요 없습니다.

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

아래는 [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트의 문서 항목에서 가져온 구문 블록입니다. 블록을 보면 서로 다른 등록 및 취소 함수가 3개 있습니다. 세 번째 오버로드에서 어떤 형식의 이벤트 취소자를 선언해야 할지 정확히 알 수 있습니다.

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
> 위의 코드 예제에서 `Button::Click_revoker`는 `winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase>`의 형식 별칭입니다. 비슷한 패턴이 모든 C++/WinRT 이벤트에 적용됩니다. 각 Windows 런타임 이벤트에는 이벤트 취소자를 반환하는 취소 함수 오버로드가 있으며 해당 취소자의 형식은 이벤트 원본의 멤버입니다. 따라서 또 다른 예제로 [**CoreWindow::SizeChanged**](/uwp/api/windows.ui.core.corewindow.sizechanged) 이벤트에는 **CoreWindow::SizeChanged_revoker** 형식 값을 반환하는 등록 함수 오버로드가 있습니다.

페이지 탐색 시나리오에서는 처리기 취소를 고려할 수 있습니다. 페이지 탐색 후 다른 페이지 탐색이 반복될 경우에는 페이지에서 다른 페이지를 탐색할 때 처리기를 취소할 수 있습니다. 또는 동일한 페이지 인스턴스를 다시 사용하는 경우에는 토큰 값을 확인하여 아직 설정되지 않은 경우에만 등록합니다(`if (!m_token){ ... }`). 세 번째 옵션은 이벤트 취소자를 데이터 멤버로 페이지에 저장하는 것입니다. 마지막으로 네 번째 옵션은 이번 항목 후반에 설명하겠지만 람다 함수에서 *this* 개체에 대한 강력한 참조나 약한 참조를 캡처하는 것입니다.

### <a name="if-your-auto-revoke-delegate-fails-to-register"></a>자동 취소 대리자를 등록하지 못하는 경우

대리자를 등록할 때 [**winrt::auto_revoke**](/uwp/cpp-ref-for-winrt/auto-revoke-t)를 지정하려고 하면 [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) 예외가 발생하는 경우 일반적으로 해당 이벤트 소스가 약한 참조를 지원하지 않는 것입니다. [**Windows.UI.Composition**](/uwp/api/windows.ui.composition) 네임스페이스 등에서 일반적으로 발생하는 상황입니다. 이런 경우 자동 취소 기능을 사용할 수 없습니다. 이벤트 처리기를 수동으로 취소하도록 폴백해야 합니다.

## <a name="delegate-types-for-asynchronous-actions-and-operations"></a>비동기 작업을 위한 대리자 형식

위의 예제에서는 **RoutedEventHandler** 대리자 형식을 사용하지만 그 밖에도 다른 대리자 형식이 많습니다. 예를 들어 진행률 유무에 상관없이 비동기 작업은 완료되었거나 진행 중이면서 해당 형식의 대리자가 필요한 이벤트가 있습니다. 진행률이 포함된 비동기 작업에서 진행 중인 이벤트([**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)를 구현하는 모든 것)는 [**AsyncOperationProgressHandler**](/uwp/api/windows.foundation.asyncoperationprogresshandler) 형식의 대리자가 필요합니다. 다음은 람다 함수를 사용해 해당 형식의 대리자를 작성하는 코드 예제입니다. 이 예제에는 [**AsyncOperationWithProgressCompletedHandler**](/uwp/api/windows.foundation.asyncoperationwithprogresscompletedhandler) 대리자를 작성하는 방법도 나와있습니다.

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

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

위의 “코루틴” 주석에서도 알 수 있듯이 비동기 작업에서 완료된 이벤트에 대리자를 사용하지 않아도 코루틴을 더욱 자연스럽게 사용할 수 있다는 것을 알 수 있습니다. 자세한 내용과 코드 예제는 [C++/WinRT로 동시성 및 비동기 작업](concurrency.md)을 참조하세요.

> [!NOTE]
> 비동기 작업에 대해 ‘완료 처리기’를 둘 이상 구현하는 것은 올바르지 않습니다.  완료 이벤트에 단일 대리자를 사용하거나 완료 이벤트를 `co_await`할 수 있습니다. 둘 다 사용하면 두 번째는 실패합니다.

코루틴 대신에 대리자를 계속 사용하는 경우 더 간단한 구문을 선택할 수 있습니다.

```cppwinrt
async_op_with_progress.Completed(
    [](auto&& /*sender*/, AsyncStatus const /* args */)
{
    // ...
});
```

## <a name="delegate-types-that-return-a-value"></a>값을 반환하는 대리자 형식

일부 대리자 형식은 스스로 값을 반환해야 합니다. 한 예로 문자열을 반환하는 [**ListViewItemToKeyHandler**](/uwp/api/windows.ui.xaml.controls.listviewitemtokeyhandler)가 있습니다. 다음은 해당 형식의 대리자를 작성하는 예제입니다(단, 람다 함수가 값을 반환함).

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

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>이벤트 처리 대리자를 사용하여 안전하게 *this* 포인터 액세스

개체의 멤버 함수를 사용하거나 개체의 멤버 함수에 있는 람다 함수 내에서 이벤트를 처리하는 경우, 이벤트 수신자(이벤트를 처리하는 개체)와 이벤트 원본(이벤트가 발생하는 개체)의 상대 수명을 고려해야 합니다. 자세한 내용 및 코드 예제는 [C++/WinRT의 강력한 참조 및 약한 참조](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)를 참조하세요.

## <a name="important-apis"></a>중요 API
* [winrt::auto_revoke_t 마커 구조체](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [winrt::implements::get_weak 함수](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [winrt::implements::get_strong 함수](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT의 이벤트 작성](author-events.md)
* [C++/WinRT를 통한 동시성 및 비동기 작업](concurrency.md)
* [C++/WinRT의 강력하고 약한 참조](weak-references.md)
