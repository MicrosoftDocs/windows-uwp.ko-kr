---
description: C++/WinRT를 사용하여 간단한 사용자 지정 컨트롤을 만드는 단계를 안내합니다. 여기에 나와 있는 정보에 따라 기능이 풍부하고 사용자 지정 가능한 UI 컨트롤을 만들 수 있습니다.
title: C++/WinRT를 사용한 XAML 사용자 지정(템플릿 기반) 컨트롤
ms.date: 04/24/2019
ms.topic: article
keywords: Windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, XAML, 사용자 지정, 템플릿 기반, 컨트롤
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6acbd62a8fa75eefb39598dd5bbb6ec1270388c4
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79209098"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>C++/WinRT를 사용한 XAML 사용자 지정(템플릿 기반) 컨트롤

> [!IMPORTANT]
> [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)를 사용해 런타임 클래스를 사용하거나 작성하는 방법을 더욱 쉽게 이해할 수 있는 필수 개념과 용어에 대해서는 [C++/WinRT를 통한 API 사용](consume-apis.md)과 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

UWP(유니버설 Windows 플랫폼)의 가장 강력한 기능 중 하나는 XAML [**컨트롤**](/uwp/api/windows.ui.xaml.controls.control) 형식을 기반으로 사용자 지정 컨트롤을 만들기 위해 UI(사용자 인터페이스) 스택이 제공하는 유연성입니다. XAML UI 프레임워크는 [사용자 지정 종속성 속성](/windows/uwp/xaml-platform/custom-dependency-properties) 및 [연결 속성](/windows/uwp/xaml-platform/custom-attached-properties)과 같은 기능뿐 아니라, 기능이 풍부하고 사용자 지정 가능한 컨트롤을 쉽게 작성할 수 있는 [컨트롤 템플릿](/windows/uwp/design/controls-and-patterns/control-templates)을 제공합니다. 이 항목에서는 C++/WinRT를 사용하여 사용자 지정(템플릿 기반) 컨트롤을 만드는 단계를 안내합니다.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>비어 있는 앱(BgLabelControlApp) 만들기
먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. **비어 있는 앱(C++/WinRT)** 프로젝트를 만들어서 이름을 *BgLabelControlApp*으로 지정합니다. 이 항목의 뒤쪽 섹션에서는 프로젝트(다음까지 빌드하지 않음)를 빌드하게 됩니다.

> [!NOTE]
> &mdash;프로젝트 템플릿 및 빌드 지원을 함께 제공하는 C++/WinRT Visual Studio 확장(VSIX) 및 NuGet 패키지를 설치하고 사용하는 방법을 포함&mdash;하는 C++/WinRT용 Visual Studio 개발 설정에 대한 자세한 내용은 [Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

사용자 지정(템플릿 기반) 컨트롤을 나타내는 새 클래스를 작성합니다. 동일한 컴파일 단위 내에서 클래스를 작성하고 사용합니다. 하지만 XAML 태그에서 이 클래스를 인스턴스화할 수 있어야 하므로 결국 런타임 클래스가 될 것입니다. 그 밖에도 런타임 클래스를 작성하고 사용하는 데 모두 C++/WinRT를 사용합니다.

새로운 런타임 클래스를 작성하려면 먼저 새 **Midl 파일(.idl)** 항목을 프로젝트에 추가합니다. 이름을 `BgLabelControl.idl`이라고 지정합니다. `BgLabelControl.idl`의 기본 콘텐츠를 삭제한 후 아래 런타임 클래스 선언에 붙여넣습니다.

```idl
// BgLabelControl.idl
namespace BgLabelControlApp
{
    runtimeclass BgLabelControl : Windows.UI.Xaml.Controls.Control
    {
        BgLabelControl();
        static Windows.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}
```

위 목록에서는 DP(종속성 속성)를 선언할 때 따라야 하는 패턴을 보여 줍니다. 각 DP에는 두 개의 조각이 있습니다. 먼저 [**DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty) 형식의 읽기 전용 정적 속성을 선언합니다. 여기에는 DP 및 ‘속성’의 이름이 포함됩니다.  구현에서 이 정적 속성을 사용합니다. 두 번째로, DP의 형식 및 이름으로 읽기-쓰기 인스턴스 속성을 선언합니다. DP 대신 ‘연결된 속성’을 작성하려면 [사용자 지정 연결 속성](/windows/uwp/xaml-platform/custom-attached-properties)에서 코드 예제를 참조하세요. 

> [!NOTE]
> DP를 부동 소수점 형식과 함께 사용하려면 DP를 `double`([MIDL 3.0](/uwp/midl-3/)의 `Double`)로 설정합니다. `float`(MIDL의 `Single`) 형식의 DP를 선언하고 구현한 후 XAML 태그에서 해당 DP의 값을 설정하면 ‘텍스트 ‘<NUMBER>’에서 ‘Windows.Foundation.Single’을 만들지 못했습니다.’ 오류가 발생합니다.  .

파일을 저장하고 프로젝트를 빌드합니다. 빌드 프로세스에서 `midl.exe` 도구가 실행되어 런타임 클래스를 설명하는 Windows 런타임 메타데이터 파일(`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`)을 생성합니다. 그런 다음, `cppwinrt.exe` 도구가 실행되어 런타임 클래스를 작성하거나 사용하도록 지원하는 원본 코드 파일을 생성합니다. 원본 코드 파일에는 IDL로 선언한 **BgLabelControl** 런타임 클래스의 구현을 시작할 수 있는 스텁이 포함됩니다. 이 스텁이 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h`와 `BgLabelControl.cpp`입니다.

스텁 파일인 `BgLabelControl.h`와 `BgLabelControl.cpp`를 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\`에서 프로젝트 폴더인 `\BgLabelControlApp\BgLabelControlApp\`로 복사합니다. **솔루션 탐색기**에서 **모든 파일 표시**가 설정되어 있는지 확인합니다. 복사한 스텁 파일을 마우스 오른쪽 단추로 클릭하고 **프로젝트에 포함**을 클릭합니다.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>**BgLabelControl** 사용자 지정 컨트롤 클래스를 구현합니다.
이제 `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h`와 `BgLabelControl.cpp`를 열고 런타임 클래스를 구현합니다. `BgLabelControl.h`에서는 생성자를 변경하여 기본 스타일 키를 설정하고, **Label** 및 **LabelProperty**를 구현하고, **OnLabelChanged**라는 정적 이벤트 처리기를 추가하여 변경된 종속성 속성 값을 처리하고, **LabelProperty**의 지원 필드를 저장할 프라이빗 멤버를 추가합니다.

해당 항목을 추가한 후 `BgLabelControl.h`는 다음과 같이 표시됩니다.

```cppwinrt
// BgLabelControl.h
...
struct BgLabelControl : BgLabelControlT<BgLabelControl>
{
    BgLabelControl() { DefaultStyleKey(winrt::box_value(L"BgLabelControlApp.BgLabelControl")); }

    winrt::hstring Label()
    {
        return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
    }

    void Label(winrt::hstring const& value)
    {
        SetValue(m_labelProperty, winrt::box_value(value));
    }

    static Windows::UI::Xaml::DependencyProperty LabelProperty() { return m_labelProperty; }

    static void OnLabelChanged(Windows::UI::Xaml::DependencyObject const&, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const&);

private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
};
...
```

`BgLabelControl.cpp`에서 이와 같은 정적 멤버를 정의합니다.

```cppwinrt
// BgLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty BgLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<BgLabelControlApp::BgLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Windows::UI::Xaml::PropertyChangedCallback{ &BgLabelControl::OnLabelChanged } }
);

void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& /* e */)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // Call members of the projected type via theControl.

        BgLabelControlApp::implementation::BgLabelControl* ptr{ winrt::get_self<BgLabelControlApp::implementation::BgLabelControl>(theControl) };
        // Call members of the implementation type via ptr.
    }
}
...
```

이 연습에서는 **OnLabelChanged**를 사용하지 않습니다. 하지만 속성 변경 콜백에 종속성 속성을 등록하는 방법을 확인할 수 있도록 제공됩니다. **OnLabelChanged** 구현은 기본 프로젝션된 형식에서 파생된 프로젝션된 형식을 가져오는 방법도 보여 줍니다(기본 프로젝션된 형식은 이 클래스의 **DependencyObject**임). 또한 프로젝션된 형식을 구현하는 형식에 대한 포인터를 가져오는 방법을 보여 줍니다. 이 두 번째 작업은 기본적으로 프로젝션된 형식을 구현하는 프로젝트(런타임 클래스를 구현하는 프로젝트)에서만 가능합니다.

> [!NOTE]
> Windows SDK 버전 10.0.17763.0(Windows 10 버전 1809) 이상을 설치하지 않은 경우 [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self) 대신, 위의 종속성 속성 변경 이벤트 처리기에서 [**winrt::from_abi**](/uwp/cpp-ref-for-winrt/from-abi)를 호출해야 합니다.

## <a name="design-the-default-style-for-bglabelcontrol"></a>**BgLabelControl**의 기본 스타일을 디자인합니다.

해당 생성자에서 **BgLabelControl**은 스스로 기본 스타일 키를 설정합니다. 그러나 기본 스타일이란 ‘무엇인가요’?  사용자 지정(템플릿 기반) 컨트롤의 경우 컨트롤의 소비자가 스타일 및/또는 템플릿을 설정하지 않는 경우 자체적으로 렌더링하는 데 사용할 수 있는 기본 컨트롤 템플릿을 포함하는 기본 스타일이 있어야 합니다. 이 섹션에서는 기본 스타일을 포함하는 프로젝트에 태그 파일을 추가합니다.

프로젝트 노드 아래에서 새 폴더(필터가 아니라 폴더)를 만들고 이름을 "Themes"로 지정합니다. `Themes` 아래에서 형식 **Visual C++**  > **XAML** > **XAML 보기**의 새 항목을 추가하고 이름을 “Generic.xaml”로 지정합니다. XAML 프레임워크가 사용자 지정 컨트롤의 기본 스타일을 찾으려면 폴더 및 파일 이름이 이와 같아야 합니다. `Generic.xaml`의 기본 콘텐츠를 삭제하고 아래 태그에 붙여넣습니다.

```xaml
<!-- \Themes\Generic.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:BgLabelControlApp">

    <Style TargetType="local:BgLabelControl" >
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="local:BgLabelControl">
                    <Grid Width="100" Height="100" Background="{TemplateBinding Background}">
                        <TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" Text="{TemplateBinding Label}"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

이 경우 기본 스타일이 설정하는 속성은 컨트롤 템플릿뿐입니다. 템플릿은 사각형(해당 배경이 XAML [**Control**](/uwp/api/windows.ui.xaml.controls.control) 형식의 모든 인스턴스에 있는 **Background** 속성에 바인딩됨) 및 텍스트 요소(해당 텍스트가 **BgLabelControl::Label** 종속성 속성에 바인딩됨)로 구성됩니다.

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>기본 UI 페이지에 **BgLabelControl** 인스턴스 추가

`MainPage.xaml`을 엽니다. 여기에는 기본 UI 페이지에 사용할 XAML 태그가 포함되어 있습니다. **StackPanel** 내부에서 **Button** 요소 바로 뒤에 다음 태그를 추가합니다.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

또한 **MainPage** 형식(컴파일 XAML 태그 및 필수 코드의 조합)이 **BgLabelControl** 사용자 지정 컨트롤 형식을 인식하도록 `MainPage.h`에 다음 include 지시문을 추가합니다. 다른 XAML 페이지에서 **BgLabelControl**을 사용하려면 해당 페이지의 헤더 파일에 동일한 include 지시문을 추가합니다. 또는 미리 컴파일된 헤더 파일에 단일 include 지시문을 넣으면 됩니다.

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

이제 프로젝트를 빌드하고 실행합니다. 기본 컨트롤 템플릿이 배경 브러시에 바인딩되고 태그의 **BgLabelControl** 인스턴스 레이블에 바인딩되는 것을 알 수 있습니다.

이 연습에서는 C++/WinRT의 사용자 지정(템플릿 기반) 컨트롤에 대한 간단한 예제를 보여 줍니다. 임의로 풍부하고 모든 기능을 갖춘 고유한 사용자 지정 컨트롤을 만들 수 있습니다. 예를 들어 사용자 지정 컨트롤은 편집 가능한 데이터 표, 비디오 플레이어 또는 3D 기하 도형 시각화 도우미만큼 복잡한 형식을 사용할 수 있습니다.

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>**MeasureOverride** 및 **OnApplyTemplate** 같은 ‘재정의 가능’ 함수 구현 

기본 런타임 클래스에서 추가로 파생된 [**Control**](/uwp/api/windows.ui.xaml.controls.control) 런타임 클래스에서 사용자 지정 컨트롤을 파생시킵니다. 파생 클래스에서 재정의할 수 있는 **Control**, [**FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement) 및 [**UIElement**](/uwp/api/windows.ui.xaml.uielement)의 재정의 가능 메서드도 있습니다. 작업을 수행하는 방법을 보여 주는 코드 예제는 다음과 같습니다.

```cppwinrt
struct BgLabelControl : BgLabelControlT<BgLabelControl>
{
...
    // Control overrides.
    void OnPointerPressed(Windows::UI::Xaml::Input::PointerRoutedEventArgs const& /* e */) const { ... };

    // FrameworkElement overrides.
    Windows::Foundation::Size MeasureOverride(Windows::Foundation::Size const& /* availableSize */) const { ... };
    void OnApplyTemplate() const { ... };

    // UIElement overrides.
    Windows::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer() const { ... };
...
};
```

‘재정의 가능’ 함수는 다른 언어 프로젝션에서는 다르게 표현됩니다.  예를 들어 C#에서 재정의 가능 함수는 일반적으로 보호된 가상 함수로 표시됩니다. C++/WinRT에서는 가상도 아니고 보호되지도 않지만 위에 표시된 것처럼 고유한 구현을 함수를 재정의하고 고유한 구현을 제공할 수 있습니다.

## <a name="important-apis"></a>중요 API
* [Control 클래스](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty 클래스](/uwp/api/windows.ui.xaml.dependencyproperty)
* [FrameworkElement 클래스](/uwp/api/windows.ui.xaml.frameworkelement)
* [UIElement 클래스](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>관련 항목
* [컨트롤 템플릿](/windows/uwp/design/controls-and-patterns/control-templates)
* [사용자 지정 종속성 속성](/windows/uwp/xaml-platform/custom-dependency-properties)
