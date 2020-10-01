---
Description: ResourceDictionary 요소와 키 입력 리소스를 정의하는 방법 그리고 앱 또는 앱 패키지의 일부로 정의하는 다른 리소스와 XAML 리소스가 어떻게 관련되는지에 대해 설명합니다.
MS-HAID: dev\_ctrl\_layout\_txt.resourcedictionary\_and\_xaml\_resource\_references
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: ResourceDictionary 및 XAML 리소스 참조
ms.assetid: E3CBFA3D-6AF5-44E1-B9F9-C3D3EA8A25CE
label: ResourceDictionary and XAML resource references
template: detail.hbs
ms.date: 09/24/2020
ms.topic: conceptual
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 6c8a9a7fd335d2b250215145b88513c6254b0116
ms.sourcegitcommit: 465306045aeefb7d34703a9ae26235c638a445e8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/28/2020
ms.locfileid: "91402066"
---
# <a name="resourcedictionary-and-xaml-resource-references"></a>ResourceDictionary 및 XAML 리소스 참조

 

XAML을 사용하여 앱의 UI 또는 리소스를 정의할 수 있습니다. 리소스는 일반적으로 두 번 이상 사용할 것으로 예상하는 일부 개체의 정의입니다. 향후 XAML 리소스 참조용으로 해당 이름처럼 작동하는 리소스 키를 지정합니다. 앱 전체에서나 앱 내의 임의의 XAML 페이지에서 리소스를 참조할 수 있습니다. Windows 런타임 XAML에서 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 요소를 사용하여 리소스를 정의할 수 있습니다. [StaticResource 태그 확장](../../xaml-platform/staticresource-markup-extension.md) 또는 [ThemeResource 태그 확장](../../xaml-platform/themeresource-markup-extension.md)을 사용하여 리소스를 참조할 수 있습니다.

가장 일반적으로 XAML 리소스로 선언하려고 하는 XAML 요소는 [Style](/uwp/api/Windows.UI.Xaml.Style), [ControlTemplate](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate), 애니메이션 구성 요소 및 [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush) 서브클래스입니다. 여기에서는 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 및 키 입력 리소스를 정의하는 방법 그리고 앱 또는 앱 패키지의 일부로 정의하는 다른 리소스와 XAML 리소스가 어떻게 관련되는지에 대해 설명합니다. 또한 [MergedDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries) 및 [ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries)와 같은 리소스 사전 고급 기능에 대해 설명합니다.

**필수 구성 요소**

사용자가 XAML 태그를 이해하며 [XAML 개요](../../xaml-platform/xaml-overview.md)를 읽었다고 가정합니다.

## <a name="define-and-use-xaml-resources"></a>XAML 리소스 정의 및 사용

XAML 리소스는 태그에서 두 번 이상 참조되는 개체입니다. 이와 같이 리소스는 일반적으로 태그 파일의 맨 위 또는 별도의 파일에 있는 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary)에 정의됩니다.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <x:String x:Key="greeting">Hello world</x:String>
        <x:String x:Key="goodbye">Goodbye world</x:String>
    </Page.Resources>

    <TextBlock Text="{StaticResource greeting}" Foreground="Gray" VerticalAlignment="Center"/>
</Page>
```

이 예에서는

-   `<Page.Resources>…</Page.Resources>` - 리소스 사전을 정의합니다.
-   `<x:String>` - “greeting” 키를 사용하여 리소스를 정의합니다.
-   `{StaticResource greeting}` - [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)의 [Text](/uwp/api/windows.ui.xaml.controls.textblock.text) 속성에 할당된 “greeting” 키를 사용하여 리소스를 찾습니다.

> **참고**&nbsp;&nbsp;[ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 관련 개념을 **리소스** 빌드 작업, 리소스(.resw) 파일 또는 앱 패키지를 생성하는 코드 프로젝트 구성 컨텍스트에서 설명하는 다른 “리소스”와 혼동하지 마세요.

리소스는 문자열이 아니어도 되며, 스타일, 템플릿, 브러시 및 색과 같은 공유 가능 개체가 될 수 있습니다. 그러나 컨트롤, 도형 및 기타 [FrameworkElement](/uwp/api/Windows.UI.Xaml.FrameworkElement)는 공유할 수 없으므로 재사용 가능 리소스로 선언할 수 없습니다. 공유에 대한 자세한 내용은 이 항목의 뒷부분에 있는 [XAML 리소스는 공유 가능이어야 함](#xaml-resources-must-be-shareable) 섹션을 참조하세요.

여기에서 브러시와 문자열은 모두 리소스로 선언되며 페이지의 컨트롤에서 사용합니다.

```XAML
<Page
    x:Class="SpiderMSDN.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <SolidColorBrush x:Key="myFavoriteColor" Color="green"/>
        <x:String x:Key="greeting">Hello world</x:String>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource myFavoriteColor}" Text="{StaticResource greeting}" VerticalAlignment="Top"/>
    <Button Foreground="{StaticResource myFavoriteColor}" Content="{StaticResource greeting}" VerticalAlignment="Center"/>
</Page>
```

모든 리소스에는 키가 있어야 합니다. 일반적으로 이 키는 `x:Key="myString"`으로 정의된 문자열입니다. 그러나 다름과 같은 몇 가지 다른 방법으로 키를 지정할 수 있습니다.

-   [Style](/uwp/api/Windows.UI.Xaml.Style) 및 [ControlTemplate](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)에는 **TargetType**이 필요하며, [x:Key](../../xaml-platform/x-key-attribute.md)가 지정되지 않은 경우 **TargetType**을 키로 사용합니다. 이 경우 키는 문자열이 아니라 실제 형식 개체입니다. (아래 예제 참조)
-   [x:Key](../../xaml-platform/x-key-attribute.md)가 지정되지 않은 경우 **TargetType**이 있는 [DataTemplate](/uwp/api/Windows.UI.Xaml.DataTemplate) 리소스는 **TargetType**을 키로 사용합니다. 이 경우 키는 문자열이 아니라 실제 형식 개체입니다.
-   [x:Key](../../xaml-platform/x-key-attribute.md) 대신 [x:Name](../../xaml-platform/x-name-attribute.md)을 사용할 수 있습니다. 그러나 x:Name에서는 리소스의 코드 숨김 필드도 생성합니다. x:Name 필드는 페이지를 로드할 때 초기화해야 하므로 x:Key보다 효율성이 떨어집니다.

[StaticResource 태그 확장 기능](../../xaml-platform/staticresource-markup-extension.md)에서는 문자열 이름으로만 자원을 검색할 수 있습니다([x:Key](../../xaml-platform/x-key-attribute.md) 또는 [x:Name](../../xaml-platform/x-name-attribute.md)). 그러나 [Style](/uwp/api/windows.ui.xaml.frameworkelement.style)과 [ContentTemplate](/uwp/api/windows.ui.xaml.controls.contentcontrol.contenttemplate) 또는 [ItemTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 속성이 설정되지 않은 컨트롤에 사용할 스타일과 템플릿을 결정하는 경우 XAML 프레임워크는 암시적 스타일 리소스(x:Key 또는 x:Name이 아니라 **TargetType** 사용)도 찾습니다.

여기서 [Style](/uwp/api/Windows.UI.Xaml.Style)에는 **typeof(Button)** 의 암시적 키가 있으며 페이지 맨 아래에 있는 [Button](/uwp/api/Windows.UI.Xaml.Controls.Button)에서 [Style](/uwp/api/windows.ui.xaml.frameworkelement.style) 속성을 지정하지 않으므로 키가 **typeof(Button)** 인 스타일을 찾습니다.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <Style TargetType="Button">
            <Setter Property="Background" Value="Red"/>
        </Style>
    </Page.Resources>
    <Grid>
       <!-- This button will have a red background. -->
       <Button Content="Button" Height="100" VerticalAlignment="Center" Width="100"/>
    </Grid>
</Page>
```

암시적 스타일 및 이 스타일의 작동 방식에 대한 자세한 내용은 [스타일링 컨트롤](xaml-styles.md) 및 [컨트롤 템플릿](control-templates.md)을 참조하세요.

## <a name="look-up-resources-in-code"></a>코드에서 리소스 조회

다른 모든 사전과 마찬가지로 리소스 사전의 멤버에 액세스합니다.

> [!WARNING]
> 코드에서 리소스를 찾을 경우 `Page.Resources` 사전의 리소스만 확인됩니다. [StaticResource 태그 확장](../../xaml-platform/staticresource-markup-extension.md)과 달리 코드에서는 첫 번째 사전에서 리소스를 찾을 수 없는 경우 `Application.Resources` 사전으로 대체되지 않습니다.

 

이 예제에서는 페이지 리소스 사전에서 `redButtonStyle` 리소스를 검색하는 방법을 보여 줍니다.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <Style TargetType="Button" x:Key="redButtonStyle">
            <Setter Property="Background" Value="red"/>
        </Style>
    </Page.Resources>
</Page>
```

```csharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            Style redButtonStyle = (Style)this.Resources["redButtonStyle"];
        }
    }
```
```cppwinrt
    MainPage::MainPage()
    {
        InitializeComponent();
        Windows::UI::Xaml::Style style = Resources().TryLookup(winrt::box_value(L"redButtonStyle")).as<Windows::UI::Xaml::Style>();
    }
```

코드에서 앱 전체 리소스를 검색하려면 여기에 표시된 대로 **Application.Current.Resources**를 사용하여 앱 리소스 사전을 가져옵니다.

```XAML
<Application
    x:Class="MSDNSample.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SpiderMSDN">
    <Application.Resources>
        <Style TargetType="Button" x:Key="appButtonStyle">
            <Setter Property="Background" Value="red"/>
        </Style>
    </Application.Resources>

</Application>
```

```csharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            Style appButtonStyle = (Style)Application.Current.Resources["appButtonStyle"];
        }
    }
```
```cppwinrt
    MainPage::MainPage()
    {
        InitializeComponent();
        Windows::UI::Xaml::Style style = Application::Current().Resources()
                                                               .TryLookup(winrt::box_value(L"appButtonStyle"))
                                                               .as<Windows::UI::Xaml::Style>();
    }
```

코드에서 애플리케이션 리소스도 추가할 수 있습니다.

그러나 작업 전에 염두에 두어야 할 두 가지 사항이 있습니다.

-   첫째, 페이지에서 리소스를 사용하려고 하기 전에 리소스를 추가해야 합니다.
-   둘째, 앱의 생성자에 리소스를 추가할 수 없습니다.

이와 같이 [Application.OnLaunched](/uwp/api/windows.ui.xaml.application.onlaunched) 메서드로 리소스를 추가하면 두 문제 모두 방지할 수 있습니다.

```csharp
// App.xaml.cs
    
sealed partial class App : Application
{
    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;
        if (rootFrame == null)
        {
            SolidColorBrush brush = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 0, 255, 0)); // green
            this.Resources["brush"] = brush;
            // … Other code that VS generates for you …
        }
    }
}
```
```cppwinrt
// App.cpp

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
        Windows::UI::Xaml::Media::SolidColorBrush brush{ Windows::UI::ColorHelper::FromArgb(255, 0, 255, 0) };
        Resources().Insert(winrt::box_value(L"brush"), winrt::box_value(brush));
        // … Other code that VS generates for you …
```

## <a name="every-frameworkelement-can-have-a-resourcedictionary"></a>모든 FrameworkElement에 ResourceDictionary가 있을 수 있습니다.

[FrameworkElement](/uwp/api/Windows.UI.Xaml.FrameworkElement)는 컨트롤이 상속하는 기본 클래스이며 [Resources](/uwp/api/windows.ui.xaml.frameworkelement.resources) 속성이 있습니다. 따라서 모든 **FrameworkElement**에 로컬 리소스 사전을 추가할 수 있습니다.

여기서 [Page](/uwp/api/Windows.UI.Xaml.Controls.Page) 및 [Border](/uwp/api/Windows.UI.Xaml.Controls.Border)에는 모두 리소스 사전이 있으며 "greeting"이라는 리소스가 있습니다. ‘textBlock2’라는 [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)은 **Border** 내부에 있으므로 해당 리소스를 찾을 때 **Border** 리소스, **Page** 리소스, [Application](/uwp/api/Windows.UI.Xaml.Application) 리소스 순으로 검색됩니다. **TextBlock** 은 "Hola mundo"입니다.

코드에서 요소의 리소스에 액세스하려면 해당 요소의 [Resources](/uwp/api/windows.ui.xaml.frameworkelement.resources) 속성을 사용합니다. XAML이 아니라 코드에서 [FrameworkElement](/uwp/api/Windows.UI.Xaml.FrameworkElement)의 리소스에 액세스하면 부모 요소의 사전이 아니라 해당 사전만 검색합니다.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <x:String x:Key="greeting">Hello world</x:String>
    </Page.Resources>
    
    <StackPanel>
        <!-- Displays "Hello world" -->
        <TextBlock x:Name="textBlock1" Text="{StaticResource greeting}"/>

        <Border x:Name="border">
            <Border.Resources>
                <x:String x:Key="greeting">Hola mundo</x:String>
            </Border.Resources>
            <!-- Displays "Hola mundo" -->
            <TextBlock x:Name="textBlock2" Text="{StaticResource greeting}"/>
        </Border>

        <!-- Displays "Hola mundo", set in code. -->
        <TextBlock x:Name="textBlock3"/>
    </StackPanel>
</Page>

```

```csharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            textBlock3.Text = (string)border.Resources["greeting"];
        }
    }
```
```cppwinrt
    MainPage::MainPage()
    {
        InitializeComponent();
        textBlock3().Text(unbox_value<hstring>(border().Resources().TryLookup(winrt::box_value(L"greeting"))));
    }
```

## <a name="merged-resource-dictionaries"></a>병합된 리소스 사전

*병합된 리소스 사전*은 일반적으로 다른 파일에 있는 다른 리소스 사전에 리소스 사전을 결합합니다.

> **팁**&nbsp;&nbsp;Microsoft Visual Studio에서 **프로젝트** 메뉴의 **추가 &gt; 새 항목... &gt; 리소스 사전** 옵션을 사용하여 리소스 사전 파일을 만들 수 있습니다.

여기서는 Dictionary1.xaml이라는 개별 XAML 파일에 리소스 사전을 정의합니다.

```XAML
<!-- Dictionary1.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="Red"/>

</ResourceDictionary>

```

이 사전을 사용하기 위해 다음과 같이 페이지의 사전과 병합합니다.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml"/>
            </ResourceDictionary.MergedDictionaries>

            <x:String x:Key="greeting">Hello world</x:String>

        </ResourceDictionary>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource brush}" Text="{StaticResource greeting}" VerticalAlignment="Center"/>
</Page>
```

이 예제에서는 다음을 수행합니다. `<Page.Resources>`에서 `<ResourceDictionary>`를 선언합니다. `<Page.Resources>`에 리소스를 추가하면 XAML 프레임워크에서 리소스 사전을 암시적으로 만들지만, 이 예제에서는 아무 리소스 사전이 아니라 병합된 사전이 포함된 리소스 사전을 원합니다.

따라서 `<ResourceDictionary>`를 선언한 다음 `<ResourceDictionary.MergedDictionaries>` 컬렉션에 항목을 추가합니다. 각 항목의 형식은 `<ResourceDictionary Source="Dictionary1.xaml"/>`입니다. 둘 이상의 사전을 추가하려면 첫 번째 항목 뒤에 `<ResourceDictionary Source="Dictionary2.xaml"/>` 항목을 추가합니다.

`<ResourceDictionary.MergedDictionaries>…</ResourceDictionary.MergedDictionaries>` 후에 선택적으로 기본 사전에 추가 리소스를 넣을 수 있습니다. 일반 사전과 마찬가지로 병합 대상 사전의 리소스를 사용합니다. 위의 예제에서 `{StaticResource brush}`는 자식/병합된 사전(Dictionary1.xaml)에서 리소스를 찾는 반면 `{StaticResource greeting}`은 기본 페이지 사전에서 리소스를 찾습니다.

리소스 조회 시퀀스에서는 해당 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary)의 모든 기타 키 입력 리소스를 확인한 후에야 [MergedDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries) 사전을 확인합니다. 해당 수준을 검색한 후 조회가 병합된 사전에 도달하고 **MergedDictionaries**의 각 항목이 확인됩니다. 병합된 사전이 여러 개 있는 경우 **MergedDictionaries** 속성에서 선언된 순서의 반대 순서로 사전을 확인합니다. 다음 예제에서 Dictionary2.xaml와 Dictionary1.xaml에서 동일한 키를 선언한 경우 **MergedDictionaries** 집합에서 마지막이기 때문에 Dictionary2.xaml의 키가 먼저 사용됩니다.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml"/>
                <ResourceDictionary Source="Dictionary2.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource brush}" Text="greetings!" VerticalAlignment="Center"/>
</Page>
```

하나의 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 범위 내에서 사전의 키 고유성을 확인합니다. 그러나 해당 범위가 다른 [MergedDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries) 파일의 다른 항목으로 확장되지는 않습니다.

조회 시퀀스와 병합된 사전 범위의 고유 키 적용 누락을 함께 사용하여 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 리소스의 대체 값 시퀀스를 만들 수 있습니다. 예를 들어 앱의 상태 및 사용자 기본 설정 데이터에 동기화되는 리소스 사전을 사용하여 시퀀스의 마지막 병합된 리소스 사전에서 특정 브러시 색상에 대한 사용자 기본 설정을 저장할 수 있습니다. 그러나 사용자 기본 설정이 없는 경우에는 이전 초기 [MergedDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries) 파일의 **ResourceDictionary** 리소스에 대해 동일한 키 문자열을 정의할 수 있으며, 대체 값으로 사용될 수 있습니다. 기본 리소스 사전에 입력하는 모든 값은 항상 병합된 사전을 확인하기 전에 확인되므로 대체 기술을 사용하려는 경우 기본 리소스 사전에 해당 리소스를 정의하지 마세요.

## <a name="theme-resources-and-theme-dictionaries"></a>테마 리소스 및 테마 사전

[ThemeResource](../../xaml-platform/themeresource-markup-extension.md)는 [StaticResource](../../xaml-platform/staticresource-markup-extension.md)와 비슷하지만, 테마가 변경될 때 리소스 검색을 다시 평가합니다.

이 예제에서는 [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)의 포그라운드를 현재 테마의 값으로 설정합니다.

```XAML
<TextBlock Text="hello world" Foreground="{ThemeResource FocusVisualWhiteStrokeThemeBrush}" VerticalAlignment="Center"/>
```

테마 사전은 사용자가 현재 소유 중인 디바이스에서 사용하고 있는 테마에 따라 달라지는 자원을 보유하는 특별한 유형의 병합된 사전입니다. 예를 들어 ‘밝은’ 테마는 흰색 브러시를 사용할 수 있으나 ‘어두운’ 테마는 어두운 색 브러시를 사용할 수 있습니다. 브러시는 확인되는 대상 리소스를 변경하지만 해당 브러시를 리소스로 사용하는 컨트롤의 컴퍼지션이 동일할 수 있습니다. 고유한 템플릿 및 스타일에서 테마 전환 동작을 재현하려면 [MergedDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries)를 속성으로 사용하여 이러한 항목을 기본 사전으로 병합하지 말고 [ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries) 속성을 사용하세요.

[ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries)의 각 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 요소에는 [x:Key](../../xaml-platform/x-key-attribute.md) 값이 있어야 합니다. 해당 값은 관련 테마를 명명하는 문자열입니다. 예: "기본값", "어둡게", "밝게" 또는 "고대비". 일반적으로 `Dictionary1`과 `Dictionary2`에서는 이름은 같지만 값이 다른 리소스를 정의합니다.

여기에서는 밝은 테마에는 빨간색 텍스트를 사용하고 어두운 테마에는 파란색 텍스트를 사용합니다.

```XAML
<!-- Dictionary1.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="Red"/>

</ResourceDictionary>

<!-- Dictionary2.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="blue"/>

</ResourceDictionary>
```

이 예제에서는 [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)의 포그라운드를 현재 테마의 값으로 설정합니다.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.ThemeDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml" x:Key="Light"/>
                <ResourceDictionary Source="Dictionary2.xaml" x:Key="Dark"/>
            </ResourceDictionary.ThemeDictionaries>
        </ResourceDictionary>
    </Page.Resources>
    <TextBlock Foreground="{StaticResource brush}" Text="hello world" VerticalAlignment="Center"/>
</Page>
```

테마 사전의 경우, 참조를 만드는 데 [ThemeResource 태그 확장](../../xaml-platform/themeresource-markup-extension.md)이 사용되고 시스템에서 테마 변경을 감지할 때마다 리소스 조회에 사용되는 활성 사전은 동적으로 변경됩니다. 시스템에서 수행된 조회 동작은 특정 테마 사전의 [x:Key](../../xaml-platform/x-key-attribute.md)에 대한 활성 테마 매핑을 기반으로 합니다.

Windows 런타임이 기본적으로 컨트롤에 사용하는 템플릿과 유사한 기본 XAML 디자인 리소스에서 테마 사전이 구성되는 방식을 검토하는 것이 유용할 수 있습니다. 텍스트 편집기 또는 IDE를 사용하여 \\(Program Files)\\Windows Kits\\10\\DesignTime\\CommonConfiguration\\Neutral\\UAP\\&lt;SDK version&gt;\\Generic의 XAML 파일을 엽니다. 우선 generic.xaml에서 테마 사전이 정의된 방식을 확인하고 각 테마 사전이 동일한 키를 정의하는 방식을 확인합니다. 이러한 키는 각각 테마 사전의 외부에 있고 XAML에서 나중에 정의되는 다양한 키 입력 요소의 컴퍼지션 요소에서 참조됩니다. 기본 컨트롤 템플릿 없이 테마 리소스와 추가 템플릿만 포함하는 디자인을 위한 별도의 themeresources.xaml 파일도 있습니다. 테마 영역은 generic.xaml에 있는 것과 중복됩니다.

XAML 디자인 도구를 사용하여 스타일 및 템플릿 복사본을 편집할 경우 디자인 도구는 XAML 디자인 리소스 사전에서 세션을 추출하여 앱과 프로젝트의 일부인 XAML 사전 요소의 로컬 복사본으로 배치합니다.

자세한 내용과 앱에서 사용할 수 있는 테마별 시스템 리소스의 목록은 [XAML 테마 리소스](xaml-theme-resources.md)를 참조하세요.

## <a name="lookup-behavior-for-xaml-resource-references"></a>XAML 리소스의 참조 조회 동작

*조회 동작*은 XAML 리소스 시스템이 XAML 리소스를 찾는 방법을 설명하는 용어입니다. 조회는 앱의 XAML에 있는 일부 위치에서 키가 XAML 리소스 참조로 참조될 때 발생합니다. 먼저 리소스 시스템에는 범위를 기반으로 리소스의 존재를 확인할 예측 가능한 동작이 있습니다. 초기 범위에서 리소스를 찾지 못하면 범위가 확장됩니다. 조회 동작은 앱 또는 시스템에 의해 XAML 리소스가 정의되었을 수 있는 위치와 범위 전반에 걸쳐 계속됩니다. 모든 가능한 리소스 조회 시도가 실패하는 경우 종종 오류가 발생합니다. 일반적으로 개발 과정 중에 이러한 오류를 없앨 수 있습니다.

XAML 리소스 참조에 대한 조회 동작은 실제 사용이 적용되는 개체 및 고유 [Resources](/uwp/api/windows.ui.xaml.frameworkelement.resources) 속성을 사용하여 시작됩니다. 해당 위치에 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary)가 있는 경우 이 **ResourceDictionary**에서 요청된 키가 있는 항목을 확인합니다. 일반적으로 동일한 개체의 리소스를 정의한 후 참조하지 않으므로 이러한 첫 번째 수준의 조회는 관련되는 경우가 거의 없습니다. 실제로 **Resources** 속성은 이 위치에 존재하지 않는 경우가 많습니다. XAML 내의 거의 모든 위치에서 XAML 리소스 참조를 만들 수 있으며 [FrameworkElement](/uwp/api/Windows.UI.Xaml.FrameworkElement) 하위 클래스의 속성으로 제한되지 않습니다.

그런 다음 조회 시퀀스는 앱의 런타임 개체 트리에서 다음 부모 개체를 확인합니다. [FrameworkElement.Resources](/uwp/api/windows.ui.xaml.frameworkelement.resources)가 있고 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary)를 보유한 경우 지정된 키 문자열이 포함된 사전 항목을 요청합니다. 리소스를 찾으면 조회 시퀀스가 중지되고 참조를 작성한 위치에 해당 개체가 제공됩니다. 그렇지 않으면 개체 트리 루트 방향의 다음 부모 수준으로 조회 동작이 진행됩니다. 검색은 XAML의 루트 요소에 도달하여 모든 가능한 즉시 실행 리소스 위치 검색이 완료될 때까지 재귀적으로 위쪽으로 계속됩니다.

> **참고**&nbsp;&nbsp;이러한 리소스 조회 동작을 활용하기 위해, 그리고 XAML 태그 스타일 규칙으로 인해 일반적으로 페이지의 루트 수준에서 모든 즉시 실행 리소스를 정의합니다.

 

요청된 리소스를 즉시 실행 리소스에서 찾지 못한 경우 다음 조회 단계는 [Application.Resources](/uwp/api/windows.ui.xaml.application.resources) 속성을 확인하는 것입니다. **Application.Resources**는 앱 탐색 구조의 여러 페이지에서 참조하는 앱 특정 리소스를 지정할 가장 적합한 위치입니다.

컨트롤 템플릿에는 참조 조회에서 가능한 또 다른 위치인 테마 사전이 있습니다. 테마 사전은 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 요소가 해당 루트인 단일 XAML 파일입니다. 테마 사전은 [Application.Resources](/uwp/api/windows.ui.xaml.application.resources)에서 병합된 사전일 수 있습니다. 테마 사전은 템플릿 지정된 사용자 지정 컨트롤의 컨트롤 관련 테마 사전일 수도 있습니다.

마지막으로 플랫폼 리소스에 대한 리소스 조회가 있습니다. 플랫폼 리소스에는 시스템 UI 테마 각각에 대해 정의되고 Windows 런타임 앱의 UI에 사용하는 모든 컨트롤의 기본 모양을 정의하는 컨트롤 템플릿이 포함되어 있습니다. 또한 플랫폼 리소스에는 시스템 전반의 모양과 테마와 관련된 명명된 리소스 집합이 포함되어 있습니다. 이러한 리소스는 기술적으로 [MergedDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries) 항목이며, 따라서 앱이 로드된 후 XAML 또는 코드에서 조회하는 데 사용할 수 있습니다. 예를 들어 시스템 테마 리소스에는 앱 텍스트 색과 운영 체제 및 사용자 기본 설정에서 제공되는 시스템 창의 텍스트 색을 일치시키기 위한 [Color](/uwp/api/Windows.UI.Color) 정의를 제공하는 "SystemColorWindowTextColor"라는 리소스가 포함되어 있습니다. 앱에 대한 다른 XAML 스타일이 이 스타일을 참조하거나, 코드가 리소스 조회 값을 가져올 수 있습니다(예제에서는 **Color**에 캐스트 가능).

자세한 내용과 XAML을 사용하는 Windows 앱에서 사용할 수 있는 테마별 시스템 리소스의 목록을 보려면 [XAML 테마 리소스](xaml-theme-resources.md)를 참조하세요.

이러한 위치 어디에서도 요청된 키를 계속 찾지 못하는 경우 XAML 구문 분석 오류/예외가 발생합니다. 특정 상황에서는 XAML 구문 분석 예외가 XAML 태그 컴파일 작업 또는 XAML 디자인 환경에서 발견되지 않은 런타임 예외일 수 있습니다.

각 리소스가 다른 수준에서 정의되는 경우 리소스 사전의 계층화된 조회 동작 때문에 각각 동일한 문자열 값을 키로 가지는 여러 리소스 항목을 의도적으로 정의할 수 있습니다. 다시 말해 키는 모든 지정된 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 내에서 고유해야 하지만 고유성 요구 사항이 조회 동작 시퀀스 전체로 확장되지는 않습니다. 조회하는 동안 성공적으로 검색되는 첫 번째 개체만 XAML 리소스 참조에 사용된 다음 조회가 중지됩니다. 이러한 동작을 통해 앱 XAML 내의 다양한 위치에서 동일한 XAML 리소스를 요청할 수 있으나 XAML 리소스 참조가 작성된 범위 및 특정 조회가 동작하는 방식에 따라 다른 리소스를 얻게 됩니다.

##  <a name="forward-references-within-a-resourcedictionary"></a>리소스 사전 내의 전방 참조


고정 리소스 사전 내에 있는 XAML 리소스 참조는 키를 사용하여 이미 정의된 리소스를 참조해야 하며 해당 리소스는 리소스 참조 전에 사전적으로 표시되어야 합니다. XAML 리소스 참조는 전방 참조를 해석할 수 없습니다. 이러한 이유로 다른 리소스 내에서 XAML 리소스 참조를 사용하는 경우 다른 리소스에서 사용되는 리소스가 먼저 리소스 사전에서 정의되도록 리소스 사전 구조를 디자인해야 합니다.

앱 수준에서 정의된 리소스는 즉시 실행 리소스를 참조할 수 없습니다. 앱 리소스는 실제로 처음에(앱이 처음 시작될 때 그리고 모든 탐색 페이지 콘텐츠가 로드되기 전에) 처리되기 때문에 이는 전방 참조를 시도하는 것과 마찬가지입니다. 그러나 모든 즉시 실행 리소스는 앱 리소스를 참조할 수 있고 이는 전방 참조 상황을 방지하기 위한 유용한 기술이 될 수 있습니다.

## <a name="xaml-resources-must-be-shareable"></a>공유 가능해야 하는 XAML 리소스


개체가 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary)에 있으려면 ‘공유 가능’해야 합니다. 

앱의 개체 트리가 생성되어 런타임에 사용되는 경우 개체는 트리의 여러 위치에 존재할 수 없으므로 반드시 공유 가능 상태가 되어야 합니다. 내부적으로 리소스 시스템은 각 XAML 리소스가 요청될 때 앱의 개체 그래프에서 사용할 리소스 값 복사본을 만듭니다.

일반적으로 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 및 Windows 런타임 XAML은 공유 가능한 사용을 위해 다음 개체를 지원합니다.

-   스타일 및 템플릿([FrameworkTemplate](/uwp/api/Windows.UI.Xaml.FrameworkTemplate)에서 파생된 [Style](/uwp/api/Windows.UI.Xaml.Style) 및 클래스)
-   브러시 및 색([Brush](/uwp/api/Windows.UI.Xaml.Media.Brush)에서 파생된 클래스 및 [Color](/uwp/api/Windows.UI.Color) 값)
-   [Storyboard](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)를 포함한 애니메이션 형식
-   변형([GeneralTransform](/uwp/api/Windows.UI.Xaml.Media.GeneralTransform)에서 파생된 클래스)
-   [Matrix](/uwp/api/Windows.UI.Xaml.Media.Matrix) 및 [Matrix3D](/uwp/api/Windows.UI.Xaml.Media.Media3D.Matrix3D)
-   [Point](/uwp/api/Windows.Foundation.Point) 값
-   [Thickness](/uwp/api/Windows.UI.Xaml.Thickness) 및 [CornerRadius](/uwp/api/Windows.UI.Xaml.CornerRadius) 같은 특정한 다른 UI 관련 구조
-   [XAML 기본 데이터 형식](../../xaml-platform/xaml-intrinsic-data-types.md)

필요한 구현 패턴을 따를 경우 공유 가능한 리소스로 사용자 지정 형식을 사용할 수도 있습니다. 지원 코드(또는 포함하는 런타임 구성 요소)에서 해당 클래스를 정의한 다음 XAML에서 리소스로 인스턴스화합니다. 개체 데이터 원본과 데이터 바인딩을 위한 [IValueConverter](/uwp/api/Windows.UI.Xaml.Data.IValueConverter) 구현을 예로 들 수 있습니다.

XAML 파서가 클래스를 인스턴스화하려면 생성자가 필요하므로 사용자 지정 형식에는 기본 생성자가 있어야 합니다. **UIElement**는 공유할 수 없으므로 리소스로 사용되는 사용자 지정 형식의 상속에 [UIElement](/uwp/api/Windows.UI.Xaml.UIElement) 클래스를 포함할 수 없습니다. 이 클래스는 항상 런타임 앱의 개체 그래프에서 한 위치에 있는 단일 UI 요소만 나타냅니다.

## <a name="usercontrol-usage-scope"></a>UserControl 사용 범위


[UserControl](/uwp/api/Windows.UI.Xaml.Controls.UserControl) 요소에는 정의 범위 및 사용 범위에 대한 고유한 개념이 있기 때문에 리소스 조회 동작에 대한 특별한 상황이 있습니다. 해당 정의 범위에서 XAML 리소스를 참조하는 **UserControl**은 고유 정의 범위 조회 시퀀스 내에서 해당 리소스 조회를 지원할 수 있어야 합니다. 다시 말해 앱 리소스에 액세스할 수 없습니다. **UserControl** 사용 범위에서는 리소스 참조가 해당 사용 페이지 루트에 대한 조회 시퀀스 내에 있는 것으로 처리되고(로드된 개체 트리에 있는 개체의 모든 다른 리소스 참조와 마찬가지임) 앱 리소스에 액세스할 수 있습니다.

## <a name="resourcedictionary-and-xamlreaderload"></a>ResourceDictionary 및 XamlReader.Load

[ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary)를 루트 또는 [XamlReader.Load](/uwp/api/windows.ui.xaml.markup.xamlreader.load) 메서드 XAML 입력의 일부로 사용할 수 있습니다. 이 모든 참조가 로드용으로 제출된 XAML에 완전히 자체 포함되어 있는 경우 해당 XAML에 XAML 리소스 참조를 포함시킬 수도 있습니다. **XamlReader.Load**는 [Application.Resources](/uwp/api/windows.ui.xaml.application.resources)는 물론 다른 **ResourceDictionary** 개체도 인식하지 못하는 컨텍스트에서 XAML을 구문 분석합니다. 또한 **XamlReader.Load**에 제출된 XAML 내에서 `{ThemeResource}`를 사용하지 마세요.

## <a name="using-a-resourcedictionary-from-code"></a>코드에서 ResourceDictionary 사용

대부분의 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 시나리오는 XAML에서 배타적으로 처리됩니다. **ResourceDictionary** 컨테이너 및 리소스를 XAML 파일 또는 UI 정의 파일의 XAML 노드 집합으로 선언합니다. 그런 다음, XAML 리소스 참조를 사용하여 XAML의 다른 부분에서 해당 리소스를 요청합니다. 하지만 앱이 실행 중인 동안 실행되는 코드를 사용하여 **ResourceDictionary**의 콘텐츠를 조정하거나 **ResourceDictionary** 콘텐츠를 쿼리하여 리소스가 이미 정의되어 있는지 확인해야 하는 특정 시나리오가 있습니다. 이러한 코드 호출은 **ResourceDictionary** 인스턴스에서 수행되므로 먼저 하나의 항목, 즉 [FrameworkElement.Resources](/uwp/api/windows.ui.xaml.frameworkelement.resources)를 가져와 개체 트리에서 즉시 실행 **ResourceDictionary**를 검색하거나 `Application.Current.Resources`를 검색해야 합니다.

C\# 또는 Microsoft Visual Basic 코드에서 인덱서([Item](/dotnet/api/system.windows.resourcedictionary.item))를 사용하여 특정 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary)의 리소스를 참조할 수 있습니다. **ResourceDictionary**는 문자열 키가 입력된 사전이므로 인덱서는 정수 인덱스가 아닌 문자열 키를 사용합니다. Visual C++ 구성 요소 확장(C++/CX) 코드에서 [Lookup](/uwp/api/windows.ui.xaml.resourcedictionary.lookup)을 사용합니다.

코드를 사용하여 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary)를 검사 또는 변경하려는 경우, [Lookup](/uwp/api/windows.ui.xaml.resourcedictionary.lookup) 또는 [Item](/dotnet/api/system.windows.resourcedictionary.item)과 같은 API에 대한 동작은 즉시 실행 리소스에서 앱 리소스로 트래버스되지 않습니다. 이는 XAML 페이지가 로드될 때에만 발생하는 XAML 파서 동작입니다. 런타임 시 키에 대한 범위가 당시 사용 중인 **ResourceDictionary** 인스턴스에 자체 포함됩니다. 그러나 그 범위는 [MergedDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries)로 확장되지 않습니다.

또한 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary)에 없는 키를 요청하는 경우 오류가 발생하지 않고 단순히 반환 값이 **null**로 제공될 수 있습니다. 반환되는 **null**을 값으로 사용하려는 경우 여전히 오류가 발생할 수 있습니다. 이 오류는 **ResourceDictionary** 호출이 아니라 속성의 setter에서 올 수 있습니다. 오류를 피할 수 있는 유일한 방법은 속성이 **null**을 유효한 값으로 수용하는 경우입니다. 이 동작이 XAML 구문 분석 시 XAML 조회 동작과 대조됩니다. 구문 분석 시 XAML에서 제공된 키를 해석하지 못하면 해당 속성에서 **null**이 허용되는 경우에도 XAML 구문 분석 오류가 발생합니다.

병합된 리소스 사전은 런타임에 병합된 사전을 참조하는 기본 리소스 사전의 인덱스 범위에 포함됩니다. 다시 말해 기본 사전의 **Item** 또는 [Lookup](/uwp/api/windows.ui.xaml.resourcedictionary.lookup)을 사용하여 병합된 사전에 실제로 정의된 모든 개체를 찾을 수 있습니다. 이 경우 조회 동작은 구문 분석 시 XAML 조회 동작을 모방합니다. 각각 동일한 키가 있는 개체가 여러 개 병합된 사전에 있는 경우 마지막으로 추가된 사전의 개체가 반환됩니다.

**Add**(C\# 또는 Visual Basic) 또는 [Insert](/uwp/api/windows.ui.xaml.resourcedictionary.insert)(C++/CX)를 호출하여 기존 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary)에 항목을 추가할 수 있습니다. 즉시 실행 리소스 또는 앱 리소스에 항목을 추가할 수 있습니다. 이러한 API 호출에는 키가 필요하며 이를 통해 **ResourceDictionary**의 각 항목에 키가 있어야 한다는 요구 사항이 충족됩니다. 그러나 런타임에 **ResourceDictionary**에 추가하는 항목은 XAML 리소스 참조와 관련이 없습니다. XAML 리소스 참조에 대한 필수 조회는 앱 로드 시 XAML이 처음으로 구문 분석되거나 테마 변경이 검색될 때 발생합니다. 런타임에 컬렉션에 추가된 리소스는 당시에 사용할 수 없으며, **ResourceDictionary**를 변경해도 여기서 이미 검색된 리소스는 그 값을 변경하더라도 무효화되지 않습니다.

런타임에 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary)에서 항목을 제거하거나 항목의 일부 또는 전부를 복사할 수 있으며 다른 작업을 수행할 수도 있습니다. **ResourceDictionary** 멤버 목록은 사용 가능한 API를 나타냅니다. **ResourceDictionary**에 기본 컬렉션 인터페이스를 지원하는 예상 API가 있으므로 C\# 또는 Visual Basic 및 C++/CX 사용 여부에 따라 해당 API 옵션이 달라집니다.

## <a name="resourcedictionary-and-localization"></a>ResourceDictionary 및 지역화


처음에는 지역화될 문자열이 XAML [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary)에 포함되어 있을 수 있습니다. 이 경우 **ResourceDictionary**가 아닌 프로젝트 리소스로 이러한 문자열을 저장합니다. XAML에서 문자열을 제거하지 않고 소유 요소에 [x:Uid 지시문](../../xaml-platform/x-uid-directive.md) 값을 제공합니다. 그런 다음 리소스 파일에서 리소스를 정의합니다. 리소스 이름을 *XUIDValue*.*PropertyName* 형식으로 입력하고 지역화되어야 하는 문자열의 리소스 값을 입력합니다.

## <a name="custom-resource-lookup"></a>사용자 지정 리소스 조회

고급 시나리오에서는 이 항목에서 설명하는 XAML 리소스 조회와 다른 동작을 갖는 클래스를 구현할 수 있습니다. 이렇게 하려면 [CustomXamlResourceLoader](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 클래스를 구현한 다음, [StaticResource](../../xaml-platform/staticresource-markup-extension.md) 또는 [ThemeResource](../../xaml-platform/themeresource-markup-extension.md)를 사용하는 대신 리소스 참조에 [CustomResource 태그 확장](../../xaml-platform/customresource-markup-extension.md)을 사용하여 해당 동작에 액세스할 수 있습니다. 대부분의 앱에는 이를 요구하는 시나리오가 없습니다. 자세한 내용은 [CustomXamlResourceLoader](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader)를 참조하세요.

 
## <a name="related-topics"></a>관련 항목

* [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary)
* [XAML 개요](../../xaml-platform/xaml-overview.md)
* [StaticResource 태그 확장](../../xaml-platform/staticresource-markup-extension.md)
* [ThemeResource 태그 확장](../../xaml-platform/themeresource-markup-extension.md)
* [XAML 테마 리소스](xaml-theme-resources.md)
* [컨트롤 스타일 지정](xaml-styles.md)
* [x:Key 특성](../../xaml-platform/x-key-attribute.md)

 

 
