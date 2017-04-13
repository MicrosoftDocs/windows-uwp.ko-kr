---
author: Jwmsft
Description: "기본 두 페이지 피어 투 피어 UWP(유니버설 Windows 플랫폼) 앱에서 탐색하는 방법을 알아봅니다."
title: "두 페이지 간의 피어 투 피어 탐색"
ms.assetid: 0A364C8B-715F-4407-9426-92267E8FB525
label: Peer-to-peer navigation between two pages
template: detail.hbs
op-migration-status: ready
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.openlocfilehash: 7e1529d641920c93ce7914c39d38001c2cbdfd78
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="peer-to-peer-navigation-between-two-pages"></a>두 페이지 간의 피어 투 피어 탐색

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

기본 두 페이지 피어 투 피어 UWP(유니버설 Windows 플랫폼) 앱에서 탐색하는 방법을 알아봅니다.

![두 페이지 피어 투 피어 탐색 예제](images/nav-peertopeer-2page.png)

<div class="important-apis" >
<b>중요 API</b><br/>
<ul>
<li>[**Windows.UI.Xaml.Controls.Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)</li>
<li>[**Windows.UI.Xaml.Controls.Page**](https://msdn.microsoft.com/library/windows/apps/br227503)</li>
<li>[**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300)</li>
</ul>
</div>



## <a name="create-the-blank-app"></a>빈 앱 만들기


1.  Microsoft Visual Studio 메뉴에서 **파일 &gt; 새 프로젝트**를 선택합니다.
2.  **새 프로젝트** 대화 상자의 왼쪽 창에서 **Visual C# -&gt; Windows -&gt; 유니버설** 또는 **Visual C++ -&gt; Windows -&gt; 유니버설** 노드를 선택합니다.
3.  가운데 창에서 **빈 앱**을 선택합니다.
4.  **이름** 상자에 **NavApp1**을 입력하고 **확인** 단추를 선택합니다.

    솔루션이 만들어지고 **솔루션 탐색기**에 프로젝트 파일이 나타납니다.

5.  프로그램을 실행하려면 메뉴에서 **디버그** &gt; **디버깅 시작**을 선택하거나 F5 키를 누릅니다.

    빈 페이지가 표시됩니다.

6.  디버깅을 중지하고 Visual Studio로 돌아가려면 Shift+F5를 누릅니다.

## <a name="add-basic-pages"></a>기본 페이지 추가

이제 프로젝트에 두 개의 콘텐츠 페이지를 추가하는데,

다음 단계를 두 번 수행하여 탐색할 두 페이지를 추가합니다.

1.  **솔루션 탐색기**에서, **BlankApp** 프로젝트 노드를 마우스 오른쪽 단추로 클릭하여 바로 가기 메뉴를 엽니다.
2.  바로 가기 메뉴에서 **추가** &gt; **새 항목**을 선택합니다.
3.  **새 항목 추가** 대화 상자의 가운데 창에서 **빈 페이지**를 선택합니다.
4.  **이름** 입력란에 **Page1**(또는 **Page2**)을 입력하고 **추가** 단추를 누릅니다.

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

 

Page1.xaml의 UI에 다음 콘텐츠를 추가합니다.

-   `pageTitle`이라는 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 요소를 루트 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)의 자식 요소로 추가합니다. [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) 속성을 `Page 1`로 변경합니다.

```xaml
<TextBlock x:Name="pageTitle" Text="Page 1" />
```

-   다음 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 요소를 루트 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)의 자식 요소로, `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 요소 뒤에 추가합니다.

    
```xaml
<HyperlinkButton Content="Click to go to page 2"
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

다음 코드를 Page1.xaml 코드 숨김 파일의 `Page1` 클래스에 추가하여 이전에 추가한 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)의 `Click` 이벤트를 처리합니다. 이제 Page2.xaml로 이동합니다.

> [!div class="tabbedCodeSnippets"]
```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2));
}
```
```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid));
}
```

Page2.xaml의 UI를 다음과 같이 변경합니다.

-   `pageTitle`이라는 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 요소를 루트 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)의 자식 요소로 추가합니다. [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) 속성의 값을 `Page 2`로 변경합니다.

```xaml
<TextBlock x:Name="pageTitle" Text="Page 2" />
```

-   다음 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 요소를 루트 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)의 자식 요소로, `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 요소 뒤에 추가합니다.

```xaml
<HyperlinkButton Content="Click to go to page 1" 
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

다음 코드를 Page2.xaml 코드 숨김 파일의 `Page2` 클래스에 추가하여 이전에 추가한 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)의 `Click` 이벤트를 처리합니다. 이제 Page1.xaml로 이동합니다.

> [!NOTE]
> C++ 프로젝트의 경우 다른 페이지를 참조하는 각 페이지의 헤더 파일에 `#include` 지시문을 추가해야 합니다. 여기에 제공된 페이지 간 탐색 예제의 경우 page1.xaml.h 파일에 `#include "Page2.xaml.h"`가 포함됩니다. 그러면 page2.xaml.h에는 `#include "Page1.xaml.h"`가 포함됩니다.

> [!div class="tabbedCodeSnippets"]
```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page1));
}
```
```cpp
void Page2::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid));
}
```

이제 콘텐츠 페이지를 준비했으므로 앱이 시작될 때 Page1.xaml이 표시되도록 해야 합니다.

app.xaml 코드 숨김 파일을 열고 `OnLaunched` 처리기를 변경합니다.

여기서는 `MainPage` 대신 [**Frame.Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694)에 대한 호출에서 `Page1`을 지정합니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> protected override void OnLaunched(LaunchActivatedEventArgs e)
> {
>     Frame rootFrame = Window.Current.Content as Frame;
> 
>     // Do not repeat app initialization when the Window already has content,
>     // just ensure that the window is active
>     if (rootFrame == null)
>     {
>         // Create a Frame to act as the navigation context and navigate to the first page
>         rootFrame = new Frame();
> 
>         rootFrame.NavigationFailed += OnNavigationFailed;
> 
>         if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
>         {
>             //TODO: Load state from previously suspended application
>         }
> 
>         // Place the frame in the current Window
>         Window.Current.Content = rootFrame;
>     }
> 
>     if (rootFrame.Content == null)
>     {
>         // When the navigation stack isn&#39;t restored navigate to the first page,
>         // configuring the new page by passing required information as a navigation
>         // parameter
>         rootFrame.Navigate(typeof(Page1), e.Arguments);
>     }
>     // Ensure the current window is active
>     Window.Current.Activate();
> }
> ```
> ```cpp
> void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ e)
> {
>     auto rootFrame = dynamic_cast<Frame^>(Window::Current->Content);
> 
>     // Do not repeat app initialization when the Window already has content,
>     // just ensure that the window is active
>     if (rootFrame == nullptr)
>     {
>         // Create a Frame to act as the navigation context and associate it with
>         // a SuspensionManager key
>         rootFrame = ref new Frame();
> 
>         rootFrame->NavigationFailed += 
>             ref new Windows::UI::Xaml::Navigation::NavigationFailedEventHandler(
>                 this, &amp;App::OnNavigationFailed);
> 
>         if (e->PreviousExecutionState == ApplicationExecutionState::Terminated)
>         {
>             // TODO: Load state from previously suspended application
>         }
>         
>         // Place the frame in the current Window
>         Window::Current->Content = rootFrame;
>     }
> 
>     if (rootFrame->Content == nullptr)
>     {
>         // When the navigation stack isn&#39;t restored navigate to the first page,
>         // configuring the new page by passing required information as a navigation
>         // parameter
>         rootFrame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid), e->Arguments);
>     }
> 
>     // Ensure the current window is active
>     Window::Current->Activate();
> }
> ```

**참고**  여기서 코드는 앱의 초기 창 프레임에 대한 탐색이 실패할 경우 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694)의 반환 값을 사용하여 앱 예외를 발생시킵니다. **Navigate**가 **true**를 반환하면 탐색이 수행됩니다.

이제 앱을 빌드하고 실행합니다. "Click to go to page 2"라는 링크를 클릭합니다. 맨 위의 "Page 2"라는 두 번째 페이지가 로드되어 프레임에 표시됩니다.

## <a name="frame-and-page-classes"></a>Frame 및 Page 클래스

앱이 기능을 추가하기 전에, 지금 추가한 페이지에서 앱의 탐색을 지원하는 방법을 살펴보겠습니다.

먼저 App.xaml 코드 숨김 파일의 `App.OnLaunched` 메서드에서 앱에 대한 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)(`rootFrame`)이 생성됩니다. 이 **Frame**에 콘텐츠를 표시하려면 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 메서드를 사용합니다.

**참고**  
[**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 클래스에서는 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694), [**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568), [**GoForward**](https://msdn.microsoft.com/library/windows/apps/br242693) 등의 다양한 탐색 메서드와 [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543), [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547), [**BackStackDepth**](https://msdn.microsoft.com/library/windows/apps/hh967995) 등의 속성을 지원합니다.

 
이 예제에서는 `Page1`이 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 메서드에 전달됩니다. 이 메서드는 앱의 현재 창의 콘텐츠를 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)으로 설정하고 지정하는 페이지의 콘텐츠를 **Frame**(이 예제에서는 Page1.xaml 또는 기본적으로 MainPage.xaml)에 로드합니다.

`Page1` 은 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) 클래스의 하위 클래스입니다. **Page** 클래스에는 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br227504) 속성이 있는데, 이는 **Page**를 포함하는 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)을 가져오는 읽기 전용 속성입니다. [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br227737)의 [**Click**](https://msdn.microsoft.com/library/windows/apps/br242739) 이벤트 처리기가 ` Frame.Navigate(typeof(Page2))`를 호출하면 앱 창의 **Frame**에 Page2.xaml의 내용이 표시됩니다.

프레임에 페이지가 로드되면 해당 페이지가 [**Frame**](https://msdn.microsoft.com/library/windows/apps/dn298572)의 [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543) 또는 [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547)에 [**PageStackEntry**](https://msdn.microsoft.com/library/windows/apps/br227504)로 추가됩니다.

## <a name="pass-information-between-pages"></a>페이지 간 정보 전달

이 앱은 두 페이지 간에 탐색하지만, 아직 흥미로운 것은 나오지 않았습니다. 앱에 여러 페이지가 있으면 정보를 공유해야 하는 경우가 많습니다. 첫 번째 페이지에서 두 번째 페이지로 정보를 전달해보겠습니다.

Page1.xaml에서, 앞서 추가한 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)을 다음 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)로 바꿉니다.

여기서는 텍스트 문자열 입력을 위한 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 레이블 및 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)(`name`)를 추가합니다.

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Text="Enter your name"/>
    <TextBox HorizontalAlignment="Center" Width="200" Name="name"/>
    <HyperlinkButton Content="Click to go to page 2" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

Page1.xaml 코드 숨김 파일의 `HyperlinkButton_Click` 이벤트 처리기에서 `name` [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)의 `Text` 속성을 참조하는 매개 변수를 `Navigate` 메서드에 추가합니다.

> [!div class="tabbedCodeSnippets"]
```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2), name.Text);
}
```
```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid), name->Text);
}
```

Page2.xaml 코드 숨김 파일에서 다음을 사용하여 `OnNavigatedTo` 메서드를 재정의합니다.

> [!div class="tabbedCodeSnippets"]
```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    if (e.Parameter is string)
    {
        greeting.Text = "Hi, " + e.Parameter.ToString();
    }
    else
    {
        greeting.Text = "Hi!";
    }
    base.OnNavigatedTo(e);
}
```
```cpp
void Page2::OnNavigatedTo(NavigationEventArgs^ e)
{
    if (dynamic_cast<Platform::String^>(e->Parameter) != nullptr)
    {
        greeting->Text = "Hi," + e->Parameter->ToString();
    }
    else
    {
        greeting->Text = "Hi!";
    }
    ::Windows::UI::Xaml::Controls::Page::OnNavigatedTo(e);
}
```

앱을 실행하고, 입력란에 이름을 입력하고, **Click to go to page 2**라는 링크를 클릭합니다. [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br227737)의 [**Click**](https://msdn.microsoft.com/library/windows/apps/br242739) 이벤트에서 `this.Frame.Navigate(typeof(Page2), tb1.Text)`를 호출할 때 `name.Text` 속성이 `Page2`로 전달되었으며 이벤트 데이터의 값은 페이지에 표시되는 메시지에 사용됩니다.

## <a name="cache-a-page"></a>페이지 캐시

페이지 콘텐츠 및 상태는 기본적으로 캐시되지 않으므로, 앱의 각 페이지에서 사용하도록 설정해야 합니다.

기본 피어 투 피어 예제에서는, 뒤로 단추([뒤로 단추 탐색](navigation-history-and-backwards-navigation.md)에서 설명)이 없지만 `Page2`에서 뒤로 단추를 클릭하면 `Page1`의 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)(및 기타 필드)가 기본 상태로 설정됩니다. 이를 해결하는 한 가지 방법은 [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) 속성을 사용하여 페이지가 프레임의 페이지 캐시에 추가되도록 지정하는 것입니다.

`Page1`의 생성자에서 [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506)를 [**Enabled**](https://msdn.microsoft.com/library/windows/apps/br243284)로 설정합니다. 그렇게 하면 프레임에 대한 페이지 캐시가 초과될 때까지 페이지의 모든 콘텐츠 및 상태 값이 유지됩니다.

프레임에 대한 캐시 크기 제한을 무시하려는 경우 [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506)를 [**Required**](https://msdn.microsoft.com/library/windows/apps/br243284)로 설정합니다. 그러나 캐시 크기 제한은 장치의 메모리 제한에 따라 중요할 수 있습니다.

> [!NOTE]
> [**CacheSize**](https://msdn.microsoft.com/library/windows/apps/br242683) 속성은 프레임에 대해 캐시될 수 있는 탐색 기록의 페이지의 수를 지정합니다.

> [!div class="tabbedCodeSnippets"]
```csharp
public Page1()
{
    this.InitializeComponent();
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
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
 

 




