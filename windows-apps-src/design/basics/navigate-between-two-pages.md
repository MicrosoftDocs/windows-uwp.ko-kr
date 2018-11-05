---
author: Jwmsft
Description: Learn how to enable peer-to-peer navigation between two basic pages in an Universal Windows Platform (UWP) app.
title: 두 페이지 간의 피어 투 피어 탐색
ms.assetid: 0A364C8B-715F-4407-9426-92267E8FB525
label: Peer-to-peer navigation between two pages
template: detail.hbs
op-migration-status: ready
ms.author: jimwalk
ms.date: 07/13/2018
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 91a1ca0ee99833280aaa41ca4d9c94d043a78e0a
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6034712"
---
# <a name="implement-navigation-between-two-pages"></a>두 페이지 간의 탐색 구현

앱에서 기본 피어 투 피어 탐색을 할 수 있도록 프레임과 페이지를 사용하는 방법을 알아봅니다. 

> **중요 APIs**: [**Windows.UI.Xaml.Controls.Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 클래스, [**Windows.UI.Xaml.Controls.Page**](https://msdn.microsoft.com/library/windows/apps/br227503) 클래스, [**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300) 네임스페이스

![피어 투 피어 탐색](images/peertopeer.png)

## <a name="1-create-a-blank-app"></a>1. 빈 앱 만들기

1.  Microsoft Visual Studio 메뉴에서 **파일 ** > ** 새 프로젝트**를 선택합니다.
2.  **새 프로젝트** 대화 상자의 왼쪽 창에서 **Visual C#** > ** Windows -** > ** 유니버설** 또는 **Visual C++** > ** Windows -** > ** Universal** 노드를 선택합니다.
3.  가운데 창에서 **빈 앱**을 선택합니다.
4.  **이름** 상자에 **NavApp1**을 입력하고 **확인** 단추를 선택합니다.
    솔루션이 만들어지고 **솔루션 탐색기**에 프로젝트 파일이 나타납니다.
5.  프로그램을 실행하려면 메뉴에서 **디버그** > **디버깅 시작**을 선택하거나 F5 키를 누릅니다.
    빈 페이지가 표시됩니다.
6.  디버깅을 중지하고 Visual Studio로 돌아가려면 앱을 종료하거나, 메뉴에서 **디버깅 중지**를 클릭합니다.

## <a name="2-add-basic-pages"></a>2. 기본 페이지 추가

이제 프로젝트에 두 개의 페이지를 추가합니다.

1.  **솔루션 탐색기**에서, **BlankApp** 프로젝트 노드를 마우스 오른쪽 단추로 클릭하여 바로 가기 메뉴를 엽니다.
2.  바로 가기 메뉴에서 **추가** > **새 항목**을 선택합니다.
3.  **새 항목 추가** 대화 상자의 가운데 창에서 **빈 페이지**를 선택합니다.
4.  **이름** 입력란에 **Page1**(또는 **Page2**)을 입력하고 **추가** 단추를 누릅니다.
5. 두 번째 페이지를 추가하려면 1-4 단계를 반복하세요.

이제 이러한 파일이 NavApp1 프로젝트의 일부로 나열됩니다.

<table>
<thead>
<tr class="header">
<th align="left">C#</th>
<th align="left">C++</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cs</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cs</li>
</ul></td>
<td style="vertical-align:top;"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cpp</li>
<li>Page1.xaml.h</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cpp</li>
<li>Page2.xaml.h

</li>
</ul></td>
</tr>
</tbody>
</table>

Page1.xaml에 다음 콘텐츠를 추가합니다.

-   `pageTitle`이라는 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 요소를 루트 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)의 자식 요소로 추가합니다. [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) 속성을 `Page 1`로 변경합니다.
```xaml
<TextBlock x:Name="pageTitle" Text="Page 1" />
```

-   루트 [**그리드**](https://msdn.microsoft.com/library/windows/apps/br242704) 전후 자식 요소로 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 요소는 `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 요소입니다.
```xaml
<HyperlinkButton Content="Click to go to page 2"
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

Page1.xaml 코드 숨김 파일에서 Page2.xaml 탐색을 위해 추가한 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 의 `Click` 이벤트를 처리할 다음 코드를 추가합니다.

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2));
}
```

```cppwinrt
void Page1::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page2>());
}
```

```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid));
}
```

Page2.xaml에서 다음 콘텐츠를 추가합니다.

-   `pageTitle`이라는 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 요소를 루트 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)의 자식 요소로 추가합니다. [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) 속성의 값을 `Page 2`로 변경합니다.
```xaml
<TextBlock x:Name="pageTitle" Text="Page 2" />
```

-   루트 [**그리드**](https://msdn.microsoft.com/library/windows/apps/br242704) 전후 자식 요소로 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 요소는 `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 요소입니다.
```xaml
<HyperlinkButton Content="Click to go to page 1" 
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

Page2.xaml 코드 숨김 파일에서 Page1.xaml 탐색을 위해 추가한 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 의 `Click` 이벤트를 처리할 다음 코드를 추가합니다.

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page1));
}
```

```cppwinrt
void Page2::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page1>());
}
```

```cpp
void Page2::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid));
}
```

> [!NOTE]
> C++ 프로젝트의 경우 다른 페이지를 참조하는 각 페이지의 헤더 파일에 `#include` 지시문을 추가해야 합니다. 여기에 제공된 페이지 간 탐색 예제의 경우 page1.xaml.h 파일에 `#include "Page2.xaml.h"`가 포함됩니다. 그러면 page2.xaml.h에는 `#include "Page1.xaml.h"`가 포함됩니다.

이제 페이지를 준비했으므로 앱이 시작될 때 Page1.xaml이 표시되도록 해야 합니다.

App.xaml 코드 숨김 파일을 열고 `OnLaunched` 처리기를 변경합니다.

여기서는 `MainPage` 대신 [**Frame.Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694)에 대한 호출에서 `Page1`을 지정합니다.

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
 
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();
        rootFrame.NavigationFailed += OnNavigationFailed;
 
        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }
 
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }
 
    if (rootFrame.Content == null)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame.Navigate(typeof(Page1), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    Frame rootFrame{ nullptr };
    auto content = Window::Current().Content();
    if (content)
    {
        rootFrame = content.try_as<Frame>();
    }

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // Create a Frame to act as the navigation context and associate it with
        // a SuspensionManager key
        rootFrame = Frame();

        rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

        if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated)
        {
            // Restore the saved session state only when appropriate, scheduling the
            // final launch steps after the restore is complete
        }

        if (e.PrelaunchActivated() == false)
        {
            if (rootFrame.Content() == nullptr)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(xaml_typename<NavApp1::Page1>(), box_value(e.Arguments()));
            }
            // Place the frame in the current Window
            Window::Current().Content(rootFrame);
            // Ensure the current window is active
            Window::Current().Activate();
        }
    }
    else
    {
        if (e.PrelaunchActivated() == false)
        {
            if (rootFrame.Content() == nullptr)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(xaml_typename<NavApp1::Page1>(), box_value(e.Arguments()));
            }
            // Ensure the current window is active
            Window::Current().Activate();
        }
    }
}
```

```cpp
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ e)
{
    auto rootFrame = dynamic_cast<Frame^>(Window::Current->Content);

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // Create a Frame to act as the navigation context and associate it with
        // a SuspensionManager key
        rootFrame = ref new Frame();

        rootFrame->NavigationFailed += 
            ref new Windows::UI::Xaml::Navigation::NavigationFailedEventHandler(
                this, &App::OnNavigationFailed);

        if (e->PreviousExecutionState == ApplicationExecutionState::Terminated)
        {
            // TODO: Load state from previously suspended application
        }
        
        // Place the frame in the current Window
        Window::Current->Content = rootFrame;
    }

    if (rootFrame->Content == nullptr)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid), e->Arguments);
    }

    // Ensure the current window is active
    Window::Current->Activate();
}
```

> [!NOTE]
> 여기에 코드는 앱의 초기 창 프레임에 대 한 탐색이 실패할 경우 앱 예외를 [**탐색**](https://msdn.microsoft.com/library/windows/apps/br242694) 의 반환 값을 사용 합니다. **Navigate**가 **true**를 반환하면 탐색이 수행됩니다.

이제 앱을 빌드하고 실행합니다. "Click to go to page 2"라는 링크를 클릭합니다. 맨 위의 "Page 2"라는 두 번째 페이지가 로드되어 프레임에 표시됩니다.

### <a name="about-the-frame-and-page-classes"></a>Frame 및 Page 클래스 정보

앱이 기능을 추가하기 전에, 지금 추가한 페이지에서 앱의 탐색을 지원하는 방법을 살펴보겠습니다.

먼저 App.xaml 코드 숨김 파일의 `App.OnLaunched` 메서드에서 앱에 대해 `rootFrame`라는 [**프레임**](https://msdn.microsoft.com/library/windows/apps/br242682)이 생성됩니다. **Frame** 클래스에서는 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694), [**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568), [**GoForward**](https://msdn.microsoft.com/library/windows/apps/br242693) 등의 다양한 탐색 메서드와 [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543), [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547), [**BackStackDepth**](https://msdn.microsoft.com/library/windows/apps/hh967995) 등의 속성을 지원합니다.
 
이 **Frame**에 콘텐츠를 표시하려면 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 메서드를 사용합니다. 기본값으로 이 메서드는 MainPage.xaml을 로드합니다. 여기 예에서 `Page1`은 **Navigate** 메서드로 전달되며, 메서드는 **프레임**에 `Page1`를 로드합니다. 

`Page1` [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) 클래스의 하위 클래스입니다. **Page** 클래스에는 **Frame** 속성이 있는데, 이는 **Page**를 포함하는 **Frame**을 가져오는 읽기 전용 속성입니다. `Page1` **HyperlinkButton**의 **Click** 이벤트 처리기가 `this.Frame.Navigate(typeof(Page2))`를 호출할 때, **Frame**은 Page2.xaml의 콘텐츠를 표시합니다.

마지막으로 페이지가 프레임에 로드될 때마다, 이 페이지는 [**PageStackEntry**](https://msdn.microsoft.com/library/windows/apps/dn298572)로 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br227504)의 [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543)나 [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547)에 추가되어  [기록 및 뒤로 탐색](navigation-history-and-backwards-navigation.md)을 사용할 수 있습니다.

## <a name="3-pass-information-between-pages"></a>3. 페이지 간 정보 전달

이 앱은 두 페이지 간에 탐색하지만, 아직 흥미로운 것은 나오지 않았습니다. 앱에 여러 페이지가 있으면 정보를 공유해야 하는 경우가 많습니다. 첫 번째 페이지에서 두 번째 페이지로 정보를 전달해보겠습니다.

Page1.xaml에서, **HyperlinkButton** 추가한 다음 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)을 사용 하 여 이전 바꿉니다.

여기서는 텍스트 문자열 입력을 위한 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 레이블 및 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) `name`를 추가합니다.

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Text="Enter your name"/>
    <TextBox HorizontalAlignment="Center" Width="200" Name="name"/>
    <HyperlinkButton Content="Click to go to page 2" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

에 `HyperlinkButton_Click` Page1.xaml 코드 숨김 파일의 이벤트 처리기는 매개 변수 참조를 추가 합니다 `Text` 속성의는 `name` **TextBox** 에 `Navigate` 메서드.

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2), name.Text);
}
```

```cppwinrt
void Page1::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page2>(), winrt::box_value(name().Text()));
}
```

```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid), name->Text);
}
```

Page2.xaml에서, 앞서 추가한 **HyperlinkButton**을 다음 **StackPanel**로 바꿉니다.

여기에 Page1에서 전달된 텍스트 문자열을 표시하기 위한 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)을 추가합니다.

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Name="greeting"/>
    <HyperlinkButton Content="Click to go to page 1" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

Page2.xaml 코드 숨김 파일에서 다음을 추가하여 `OnNavigatedTo` 메서드를 재정의합니다.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    if (e.Parameter is string && !string.IsNullOrWhiteSpace((string)e.Parameter))
    {
        greeting.Text = $"Hi, {e.Parameter.ToString()}";
    }
    else
    {
        greeting.Text = "Hi!";
    }
    base.OnNavigatedTo(e);
}
```

```cppwinrt
void Page2::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto propertyValue{ e.Parameter().as<Windows::Foundation::IPropertyValue>() };
    if (propertyValue.Type() == Windows::Foundation::PropertyType::String)
    {
        greeting().Text(L"Hi, " + winrt::unbox_value<winrt::hstring>(e.Parameter()));
    }
    else
    {
        greeting().Text(L"Hi!");
    }
    __super::OnNavigatedTo(e);
}
```

```cpp
void Page2::OnNavigatedTo(NavigationEventArgs^ e)
{
    if (dynamic_cast<Platform::String^>(e->Parameter) != nullptr)
    {
        greeting->Text = "Hi, " + e->Parameter->ToString();
    }
    else
    {
        greeting->Text = "Hi!";
    }
    ::Windows::UI::Xaml::Controls::Page::OnNavigatedTo(e);
}
```

앱을 실행하고, 입력란에 이름을 입력하고, **Click to go to page 2**라는 링크를 클릭합니다. 

`Page1` **HyperlinkButton**의 **Click** 이벤트가 `this.Frame.Navigate(typeof(Page2), name.Text)`를 호출할 때, `name.Text` 속성이 `Page2`로 전달되며, 이벤트 데이터의 값을 페이지에 표시되는 메시지에 사용합니다.

## <a name="4-cache-a-page"></a>4. 페이지 캐시

페이지 콘텐츠 및 상태는 기본적으로 캐시되지 않으므로, 캐시 정보를 사용하고 싶다면 앱의 각 페이지에서 이를 사용하도록 설정해야 합니다.

기본 피어 투 피어 예제에는 뒤로 단추가 없습니다([뒤로 탐색](navigation-history-and-backwards-navigation.md)에서 설명). 그러나, `Page2`의 뒤로 단추를 클릭했다면, `Page1`의 **TextBox**(다른 모든 필드)가 기본값 상태로 설정될 것입니다. 이를 해결하는 한 가지 방법은 [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) 속성을 사용하여 페이지가 프레임의 페이지 캐시에 추가되도록 지정하는 것입니다. 

`Page1` 생성자에서, **NavigationCacheMode**를 **Enabled**로 설정해 프레임 페이지 케시가 초과하지 않는 한 페이지의 모든 콘텐츠와 상태 값을 보관할 수 있습니다. [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506)를 [**Required**](https://msdn.microsoft.com/library/windows/apps/br243284)로 설정하면, 프레임에서 캐시 저장되는 탐색 기록의 페이지 수를 지정하는  [**CacheSize**](https://msdn.microsoft.com/library/windows/apps/br242683) 제한을 무시할 수 있습니다. 그러나 캐시 크기 제한은 장치의 메모리 제한에 따라 중요할 수 있습니다.

```csharp
public Page1()
{
    this.InitializeComponent();
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

```cppwinrt
Page1::Page1()
{
    InitializeComponent();
    NavigationCacheMode(Windows::UI::Xaml::Navigation::NavigationCacheMode::Enabled);
}
```

```cpp
Page1::Page1()
{
    this->InitializeComponent();
    this->NavigationCacheMode = Windows::UI::Xaml::Navigation::NavigationCacheMode::Enabled;
}
```

## <a name="related-articles"></a>관련 문서
* [UWP 앱의 탐색 디자인 기본 사항](https://msdn.microsoft.com/library/windows/apps/dn958438)
* [탭 및 피벗에 대한 지침](https://msdn.microsoft.com/library/windows/apps/dn997788)
* [탐색 창에 대한 지침](https://msdn.microsoft.com/library/windows/apps/dn997766)
