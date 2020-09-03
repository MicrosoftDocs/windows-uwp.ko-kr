---
title: C++/WinRT를 사용하여 "Hello, World!" 앱 만들기
description: 이 토픽에서는 C++/WinRT를 사용하여 Windows 10 UWP "Hello, World!" 앱을 만드는 과정을 안내합니다. 이 앱의 UI는 XAML(Extensible Application Markup Language)을 사용하여 정의됩니다.
ms.date: 07/11/2020
ms.topic: article
keywords: windows 10, uwp, cppwinrt, C++/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: bb6a76f2e8096d63907daf5ededdb6a22eb72a6c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175207"
---
# <a name="create-a-hello-world-app-using-cwinrt"></a>C++/WinRT를 사용하여 "Hello, World!" 앱 만들기

이 토픽에서는 C++/WinRT를 사용하여 Windows 10 UWP(유니버설 Windows 플랫폼) "Hello, World!" 앱을 만드는 과정을 안내합니다. 이 앱의 UI(사용자 인터페이스)는 XAML(Extensible Application Markup Language)을 사용하여 정의됩니다.

C++/WinRT는 WinRT(Windows Runtime) API를 위한 완전한 현대식 표준 C++17 언어 프로젝션입니다. 자세한 내용과 추가 연습 및 코드 예제는 [C++ /WinRT](../cpp-and-winrt-apis/index.md) 설명서를 참조하세요. 시작하는 데 도움이 되는 토픽은 [C++/WinRT 시작](../cpp-and-winrt-apis/get-started.md)입니다.

## <a name="set-up-visual-studio-for-cwinrt"></a>C++/WinRT용 Visual Studio 설정

&mdash;프로젝트 템플릿 및 빌드 지원을 함께 제공하는 C++/WinRT Visual Studio 확장(VSIX) 및 NuGet 패키지를 설치하고 사용하는 방법을 포함&mdash;하는 C++/WinRT용 Visual Studio 개발 설정에 대한 자세한 내용은 [Visual Studio의 C++/WinRT 지원](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

Visual Studio를 다운로드하려면 [다운로드](https://visualstudio.microsoft.com/downloads/)를 참조하세요.

XAML에 대한 소개는 [XAML 개요](../xaml-platform/xaml-overview.md)를 참조하세요.

## <a name="create-a-blank-app-helloworldcppwinrt"></a>비어 있는 앱(HelloWorldCppWinRT) 만들기

첫 번째 앱은 "Hello, World!" 앱으로 대화형 작업, 레이아웃 및 스타일의 일부 기본 기능을 보여줍니다.

먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. **비어 있는 앱(C++/WinRT)** 프로젝트를 만들고 이름을 *HelloWorldCppWinRT*라고 지정합니다. **솔루션 및 프로젝트를 같은 디렉터리에 배치**를 선택하지 않아야 합니다. 일반적으로 사용 가능한 최신(미리 보기 아님) 버전의 Windows SDK를 대상으로 합니다.

이 항목의 뒤쪽 섹션에서는 프로젝트(단, 다음까지 빌드하지 않음)를 빌드하게 됩니다.

### <a name="about-the-project-files"></a>프로젝트 파일 정보

일반적으로 프로젝트 폴더의 각 `.xaml`(XAML 태그) 파일에는 해당 `.idl`, `.h` 및 `.cpp` 파일이 들어 있습니다. 이러한 파일은 XAML 페이지 형식으로 컴파일됩니다.

XAML 태그 파일을 수정하여 UI 요소를 만들 수 있으며, 이러한 요소를 데이터 소스에 바인딩할 수 있습니다(이 작업을 [데이터 바인딩](../data-binding/index.md)이라고 함). XAML 페이지의 사용자 지정 논리(예: 이벤트 처리기)를 추가하도록 `.h` 및 `.cpp` 파일(경우에 따라 `.idl` 파일)을 수정합니다.

프로젝트 파일을 살펴보겠습니다.

- `App.idl`, `App.xaml`, `App.h` 및 `App.cpp`. 이러한 파일은 앱의 진입점을 포함하는 [**Windows::UI::Xaml::Application**](/uwp/api/windows.ui.xaml.application) 클래스의 앱 특수화를 나타냅니다. `App.xaml`은 페이지 관련 태그를 포함하지 않지만, 여기서 사용자 인터페이스 요소 스타일과 모든 페이지에서 액세스할 수 있도록 만들려는 다른 요소를 추가할 수 있습니다. `.h` 및 `.cpp` 파일에는 다양한 애플리케이션 수명 주기 이벤트에 대한 처리기가 포함되어 있습니다. 일반적으로 앱이 시작될 때 앱을 초기화하고 앱이 일시 중단되거나 종료될 때 정리를 수행하는 사용자 지정 코드를 여기에 추가할 수 있습니다.
- `MainPage.idl`, `MainPage.xaml`, `MainPage.h` 및 `MainPage.cpp`. 앱의 기본 주(시작) 페이지 형식에 대한 XAML 태그 및 구현인 **MainPage** 런타임 클래스를 포함합니다. **MainPage**에서는 탐색을 지원하지 않지만, 시작하는 데 필요한 몇 가지 기본 UI와 이벤트 처리기를 제공합니다.
- `pch.h` 및 `pch.cpp` 이러한 파일은 프로젝트의 미리 컴파일된 헤더 파일을 나타냅니다. `pch.h`에서, 자주 변경되지 않는 헤더 파일을 포함한 다음, 프로젝트의 다른 파일에 `pch.h`를 포함합니다.

## <a name="a-first-look-at-the-code"></a>코드 개요

### <a name="runtime-classes"></a>런타임 클래스

아시다시피, C#으로 작성된 UWP(유니버설 Windows 플랫폼) 앱의 모든 클래스는 Windows 런타임 형식입니다. 그러나 C++/WinRT 애플리케이션에서 형식을 작성하는 경우 해당 형식이 Windows 런타임 형식인지, 아니면 일반 C++ 클래스/구조체/열거형인지 선택할 수 있습니다.

프로젝트의 모든 XAML 페이지 형식은 Windows 런타임 형식이어야 합니다. 따라서 **MainPage**는 Windows 런타임 형식입니다. 특히 *런타임 클래스*입니다. XAML 페이지에서 사용하는 형식도 Windows 런타임 형식이어야 합니다. [Windows 런타임 구성 요소](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)를 작성할 때 다른 앱에서 사용할 수 있는 형식을 작성하려면 Windows 런타임 형식을 작성해야 합니다. 그 외의 경우에는 일반 C++ 형식이어도 됩니다. 일반적으로 Windows 런타임 형식은 모든 Windows 런타임 언어를 통해 사용할 수 있습니다.

형식이 Windows 런타임 형식이라는 것을 보여주는 좋은 증거 중 하나는 형식이 인터페이스 정의 언어(`.idl`) 파일 내부의 [MIDL(Microsoft Interface Definition Language)](/uwp/midl-3/)에 정의된다는 것입니다. **MainPage**를 예로 들어 살펴보겠습니다.

```idl
// MainPage.idl
namespace HelloWorldCppWinRT
{
    [default_interface]
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        Int32 MyProperty;
    }
}
```

다음은 `MainPage.h`에 표시된 것처럼 **MainPage** 런타임 클래스 및 해당 활성화 팩터리 구현의 기본 구조입니다.

```cppwinrt
// MainPage.h
...
namespace winrt::HelloWorldCppWinRT::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        int32_t MyProperty();
        void MyProperty(int32_t value);
        ...
    };
}

namespace winrt::HelloWorldCppWinRT::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}
```    

특정 형식의 런타임 클래스를 작성해야 하는지 여부에 대한 자세한 내용은 [C++/WinRT를 통한 API 작성](../cpp-and-winrt-apis/author-apis.md) 항목을 참조하세요. [XAML 컨트롤, C++/WinRT 속성에 바인딩](../cpp-and-winrt-apis/binding-property.md) 토픽에서 런타임 클래스와 IDL(`.idl` 파일) 간 연결에 대한 자세한 내용을 읽고 따라 하세요. 해당 항목에서는 새로운 런타임 클래스 작성 프로세스를 진행하며, 첫 번째 단계는 새 **Midl 파일(.idl)** 항목을 프로젝트에 추가하는 것입니다.

이제 **HelloWorldCppWinRT** 프로젝트에 몇 가지 기능을 추가해 보겠습니다.

## <a name="step-1-modify-your-startup-page"></a>1단계. 시작 페이지 수정

UI(사용자 인터페이스)를 형성하는 컨트롤을 작성할 수 있도록 **솔루션 탐색기**에서 `MainPage.xaml`을 엽니다.

이미 그 안에 있는 **StackPanel**및 해당 콘텐츠를 삭제합니다. 그 자리에 다음 XAML을 붙여넣습니다.

```xaml
<StackPanel x:Name="contentPanel" Margin="120,30,0,0">
    <TextBlock HorizontalAlignment="Left" Text="Hello, World!" FontSize="36"/>
    <TextBlock Text="What's your name?"/>
    <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
        <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
        <Button x:Name="inputButton" Content="Say &quot;Hello&quot;"/>
    </StackPanel>
    <TextBlock x:Name="greetingOutput"/>
</StackPanel>
```

이 새 [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel)에는 사용자 이름을 묻는 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock), 사용자 이름을 수락하는 [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox), [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) 및 또 다른 **TextBlock** 요소가 있습니다.

*myButton*이라는 **단추**를 삭제했으므로, 코드에서 이 단추에 대한 참조를 제거해야 합니다. 따라서 `MainPage.cpp`에서 **MainPage::ClickHandler** 함수 내부의 코드 줄을 삭제합니다.

이제 매우 간단한 유니버설 Windows 앱을 만들었습니다. UWP 앱의 모양을 확인하려면 앱을 빌드하고 실행합니다.

![UWP 앱 화면(컨트롤 포함)](images/xaml-hw-app2.png)

앱에서 텍스트 상자에 내용을 입력할 수 있습니다. 그러나 아직은 단추를 클릭해도 아무것도 수행되지 않습니다.

## <a name="step-2-add-an-event-handler"></a>2단계. 이벤트 처리기 추가

`MainPage.xaml`에서 *inputButton*이라는 **단추**를 찾고, 해당 [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트에 대한 이벤트 처리기를 선언합니다. 이제 **Button**의 태그가 다음과 같이 표시됩니다.

```xaml
<Button x:Name="inputButton" Content="Say &quot;Hello&quot;" Click="inputButton_Click"/>
```

다음과 같이 이벤트 처리기를 구현합니다.

```cppwinrt
// MainPage.h
struct MainPage : MainPageT<MainPage>
{
    ...
    void inputButton_Click(
        winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e);
};

// MainPage.cpp
namespace winrt::HelloWorldCppWinRT::implementation
{
    ...
    void MainPage::inputButton_Click(
        winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e)
    {
        greetingOutput().Text(L"Hello, " + nameInput().Text() + L"!");
    }
}
```

자세한 내용은 [대리자를 사용한 이벤트 처리](../cpp-and-winrt-apis/handle-events.md)를 참조하세요.

구현은 텍스트 상자에서 사용자 이름을 검색하여 인사말을 만드는 데 사용한 다음, *greetingOutput* 텍스트 블록에 인사말을 표시합니다.

앱을 빌드하여 실행합니다. 텍스트 상자에 사용자 이름을 입력하고 단추를 클릭합니다. 앱은 맞춤형 인사말을 표시합니다.

![메시지가 표시된 앱 화면](images/xaml-hw-app3.png)

## <a name="step-3-style-the-startup-page"></a>3단계. 시작 페이지 스타일 지정

### <a name="choose-a-theme"></a>테마 선택

앱의 모양과 느낌은 손쉽게 사용자 지정할 수 있습니다. 기본적으로 앱은 밝은 색 스타일의 리소스를 사용합니다. 또한 시스템 리소스에는 어두운 테마가 포함되어 있습니다.

어두운 테마를 사용하려면 `App.xaml`을 편집하고, [**Application::RequestedTheme**](/uwp/api/windows.ui.xaml.application.requestedtheme)에 대한 값을 추가합니다.

```xaml
<Application
    ...
    RequestedTheme="Dark">

</Application>
```

앱에서 주로 이미지 또는 비디오를 표시하는 경우에는 어두운 테마를 사용하는 것이 좋고, 텍스트가 많이 들어 있는 앱에는 밝은 테마를 사용하는 것이 좋습니다. 사용자 지정 색 구성표를 사용하려는 경우 앱의 모양과 느낌에 가장 잘 맞는 테마를 사용하세요.

> [!NOTE]
> 앱이 시작되면 테마가 적용됩니다. 앱이 실행되는 동안에는 테마를 변경할 수 없습니다.

### <a name="use-system-styles"></a>시스템 스타일 사용

이 섹션에서는 텍스트의 모양을 변경합니다(예를 들어 글꼴 크기를 더 크게 변경).

`MainPage.xaml`에서 "이름을 입력하세요." **TextBlock**을 찾습니다. [**Style**](/uwp/api/windows.ui.xaml.style) 속성을 *BaseTextBlockStyle* 시스템 리소스 키의 참조로 설정합니다.

```xaml
<TextBlock Text="What's your name?" Style="{ThemeResource BaseTextBlockStyle}"/>
```

*BaseTextBlockStyle*은 `\Program Files (x86)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\<version>\Generic\generic.xaml`의 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)에 정의되는 리소스 키입니다. 해당 스타일을 사용하여 설정되는 속성 값은 다음과 같습니다.

```xaml
<Style x:Key="BaseTextBlockStyle" TargetType="TextBlock">
    <Setter Property="FontFamily" Value="XamlAutoFontFamily" />
    <Setter Property="FontWeight" Value="SemiBold" />
    <Setter Property="FontSize" Value="14" />
    <Setter Property="TextTrimming" Value="None" />
    <Setter Property="TextWrapping" Value="Wrap" />
    <Setter Property="LineStackingStrategy" Value="MaxHeight" />
    <Setter Property="TextLineBounds" Value="Full" />
</Style>
```

또한 `MainPage.xaml`에서 `greetingOutput`이라는 **TextBlock**을 찾습니다. 마찬가지로 **Style** 속성을 *BaseTextBlockStyle*로 설정합니다. 지금 앱을 빌드하고 실행하면 두 텍스트 블록의 모양이 변경된 것을 볼 수 있습니다(예를 들어 글꼴 크기가 더 크게 변경됨).

## <a name="step-4-have-the-ui-adapt-to-different-window-sizes"></a>4단계. UI를 다양한 창 크기에 맞게 조정

작은 디스플레이를 사용하는 디바이스에서도 UI가 제대로 보이도록, 바뀌는 창 크기에 맞게 UI를 동적으로 조정하겠습니다. 이렇게 하려면 [**VisualStateManager**](/uwp/api/Windows.UI.Xaml.VisualStateManager) 섹션을 `MainPage.xaml`에 추가합니다. 여러 창 크기에 대한 다양한 시각적 상태를 정의한 다음, 각 시각적 상태에 적용되도록 속성을 설정합니다.

### <a name="adjust-the-ui-layout"></a>UI 레이아웃 조정

이 XAML 블록을 루트 **StackPanel** 요소의 첫 번째 자식 요소로 추가합니다.

```xaml
<StackPanel ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="wideState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="641" />
                </VisualState.StateTriggers>
            </VisualState>
            <VisualState x:Name="narrowState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="0" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="contentPanel.Margin" Value="20,30,0,0"/>
                    <Setter Target="inputPanel.Orientation" Value="Vertical"/>
                    <Setter Target="inputButton.Margin" Value="0,4,0,0"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    ...
</StackPanel>
```

앱을 빌드하여 실행합니다. 창이 641 DIP(디바이스 독립적 픽셀)보다 좁은 크기로 조정되기 전에는 UI가 이전과 동일하게 표시됩니다. 이제 *narrowState* 시각적 상태 및 해당 상태에 대해 정의된 모든 속성 setter가 적용됩니다.

`wideState`라는 [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState)에는 해당 [**MinWindowWidth**](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) 속성이 641로 설정된 [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger)가 있습니다. 즉, 창 너비가 최소값인 641 DIP(장치 독립적인 픽셀)보다 작지 않은 경우에만 상태가 적용됩니다. 이 상태에 대해 어떤 [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) 개체도 정의하지 않으므로, 페이지 내용에 대한 XAML에 정의된 레이아웃 속성이 사용됩니다.

두 번째 [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState), `narrowState`에는 해당 [**MinWindowWidth**](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) 속성이 0으로 설정된 [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger)가 있습니다. 창 너비가 0보다 크지만 641 DIP보다는 작은 경우에 이 상태가 적용됩니다. 정확히 641 DIP에서 `wideState`가 적용됩니다. `narrowState` 상태에서는 UI에 있는 컨트롤의 레이아웃 속성을 변경하도록 [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) 개체를 정의합니다.

- *contentPanel* 요소의 왼쪽 여백을 120에서 20으로 줄입니다.
- *inputPanel* 요소의 [**Orientation**](/uwp/api/windows.ui.xaml.controls.stackpanel.orientation)을 **Horizontal**에서 **Vertical**로 변경합니다.
- 4 DIP의 상단 여백을 *inputButton* 요소에 추가합니다.

## <a name="summary"></a>요약

이 연습에서는 Windows 유니버설 앱에 콘텐츠를 추가하는 방법, 대화형 작업을 추가하는 방법 및 UI 모양을 변경하는 방법에 대해 알아보았습니다.