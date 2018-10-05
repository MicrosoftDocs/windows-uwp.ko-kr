---
author: stevewhims
description: 이 항목에서는 C +를 사용 하 여 간단한 사용자 지정 컨트롤을 만드는 과정을 단계별로 안내 + WinRT 합니다. 고유한 기능이 풍부 하 고, 사용자 지정 UI 컨트롤을 만드는 정보를 여기에서 빌드할 수 있습니다.
title: C++/WinRT을 사용한 XAML 사용자 지정(템플릿) 컨트롤
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c + +, cpp, winrt, 프로젝션, XAML, 사용자 지정, 템플릿, 컨트롤
ms.localizationpriority: medium
ms.openlocfilehash: 539876113ce2aba563cfa65b13571cbf3998cc2d
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4390661"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>C++/WinRT을 사용한 XAML 사용자 지정(템플릿) 컨트롤

> [!IMPORTANT]
> 필수 개념과 용어를 사용 하 고 사용 하 여 런타임 클래스를 작성 하는 방법에 대 한 이해를 지 원하는 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)를 참조 하세요 [통한 Api 사용 하 여 C + + WinRT](consume-apis.md) 및 [작성자 Api C + + WinRT](author-apis.md)합니다.

유니버설 Windows 플랫폼 (UWP)의 가장 강력한 기능 중 하나를 XAML [**컨트롤**](/uwp/api/windows.ui.xaml.controls.control) 형식에 따라 사용자 지정 컨트롤을 만드는 사용자 인터페이스 (UI) 스택을 제공 하는 유연성입니다. XAML UI 프레임 워크는 [사용자 지정 종속성 속성](/windows/uwp/xaml-platform/custom-dependency-properties) 및 연결 된 속성 및 [컨트롤 템플릿](/windows/uwp/design/controls-and-patterns/control-templates)을 쉽게 기능이 풍부 하 고 사용자 지정 가능한 컨트롤을 만들 수 있는 등의 기능을 제공 합니다. 이 항목에서는 C +를 사용 하 여 사용자 지정 (템플릿) 컨트롤을 만드는 단계를 안내 + WinRT 합니다.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>빈 앱 (BgLabelControlApp) 만들기
먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. **Visual c + +** 만들기 > **Windows 유니버설** > **빈 앱 (C + + WinRT)** 프로젝트를 만들어서 *BgLabelControlApp*이름을 지정 합니다.

사용자 지정 (템플릿) 컨트롤을 나타내는 새 클래스를 작성 하겠습니다. 또한 동일한 컴파일 단위 내에서 클래스를 작성하고 사용할 계획입니다. 하지만이 클래스에 대 한 XAML 태그와 런타임 클래스가 될 것은 결국 인스턴스화할 수 있습니다. 그 밖에도 런타임 클래스를 작성하고 사용하는 데 모두 C++/WinRT를 사용합니다.

새로운 런타임 클래스를 작성하려면 먼저 새 **Midl 파일(.idl)** 항목을 프로젝트에 추가합니다. 이름을 `BgLabelControl.idl`이라고 지정합니다. `BgLabelControl.idl`의 기본 내용을 삭제한 후 아래 런타임 클래스 선언을 붙여 넣습니다.

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

위의 목록을 종속성 속성 DP ()를 선언 하는 경우 수행 하는 패턴을 보여 줍니다. 각 DP 하는 방법은 두 가지입니다. 먼저, [**DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty)형식의 읽기 전용 정적 속성을 선언합니다. DP plus *속성*의 이름이 있습니다. 구현에서이 정적 속성을 사용 합니다. 둘째, 형식 및 이름에 DP의 읽기-쓰기 인스턴스 속성을 선언합니다.

> [!NOTE]
> DP는 부동 소수점 형식을 사용 하 여 원하는 경우 사항을 `double` (`Double` [MIDL](/uwp/midl-3/)3.0에서). 선언 및 구현 형식 DP `float` (`Single` MIDL에서), XAML 태그에서 해당 DP에 대 한 값을 설정 하면 오류가 발생 하 고 *텍스트에서 'Windows.Foundation.Single'를 만들지 못했습니다 '<NUMBER>'* 합니다.

파일을 저장하고 프로젝트를 빌드합니다. 빌드 과정에서 `midl.exe` 도구가 실행되어 런타임 클래스를 설명하는 Windows 런타임 메타데이터 파일(`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`)을 생성합니다. 그런 다음 `cppwinrt.exe` 도구가 실행되어 런타임 클래스를 작성하거나 사용하도록 지원하는 소스 코드 파일을 생성합니다. 이러한 파일에 구현에 idl로 선언 된 **BgLabelControl** 런타임 클래스를 시작할 수 있는 스텁이 포함 됩니다. 이 스텁이 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h`와 `BgLabelControl.cpp`입니다.

스텁 파일인 `BgLabelControl.h`와 `BgLabelControl.cpp`를 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\`에서 프로젝트 폴더인 `\BgLabelControlApp\BgLabelControlApp\`로 복사합니다. **솔루션 탐색기**에서 **모든 파일 표시**가 설정되어 있는지 확인합니다. 복사한 스텁 파일을 마우스 오른쪽 버튼으로 클릭하고 **프로젝트에 포함**을 클릭합니다.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>**BgLabelControl** 사용자 지정 컨트롤 클래스를 구현 합니다.
이제 `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h`와 `BgLabelControl.cpp`를 열고 런타임 클래스를 구현합니다. `BgLabelControl.h`기본 스타일 키를 설정, **레이블** 및 **연결: LabelProperty**구현 하기 위한 생성자 변경, **OnLabelChanged** 처리 종속성 속성의 값을 변경 하 라는 정적 이벤트 처리기를 추가 및 전용 멤버를 추가 합니다. **연결: LabelProperty**에 대 한 지원 필드를 저장 합니다.

추가한 후에 `BgLabelControl.h` 다음과 같이 표시 됩니다.

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

`BgLabelControl.cpp`, 다음과 같은 정적 멤버를 정의 합니다.

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

이 연습에서는 **OnLabelChanged**사용 하지 않을 것입니다. 하지만 속성 변경 콜백이 있는 종속성 속성을 등록 하는 방법을 볼 수는 있습니다. **OnLabelChanged** 의 구현에는 기본 프로젝션 된 형식 (기본 프로젝션 된 형식이 **DependencyObject**이 경우)에서 파생 된 프로젝션 된 형식을 가져오는 방법을 보여 줍니다. 및 다음 프로젝션 된 형식을 구현 하는 형식에 대 한 포인터를 가져오는 방법을 보여 줍니다. 두 번째 작업이 프로젝션 된 형식 (즉, 런타임 클래스를 구현 하는 프로젝트)를 구현 하는 프로젝트에서 가능한 당연히 됩니다.

> [!NOTE]
> Windows SDK 버전 10.0.17763.0 (Windows 10, 버전 1809)를 설치 하지 않은 경우를 [**winrt:: from_abi**](/uwp/cpp-ref-for-winrt/from-abi) [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self)대신 위에서 종속성 속성 변경된 이벤트 처리기에서 호출 해야 다음 나중에.

## <a name="design-the-default-style-for-bglabelcontrol"></a>**BgLabelControl** 에 대 한 기본 스타일을 디자인 합니다.

해당 생성자 **BgLabelControl** 자체에 대 한 기본 스타일 키를 설정합니다. 하지만 어떤 *은* 기본 스타일? 기본 스타일 해야 하는 사용자 지정 (템플릿) 컨트롤&mdash;기본 컨트롤 템플릿을 포함 된&mdash;컨트롤의 소비자 스타일 및/또는 템플릿을 설정 하지 않는 경우에 사용 하 여 자체적으로 렌더링 하는 데 사용할 수 있는 합니다. 이 섹션에서는 태그 파일 우리의 기본 스타일을 포함 하는 프로젝트에 추가 됩니다.

프로젝트 노드에서 새 폴더를 만들고 "Themes" 라는 이름을 지정 합니다. 아래에서 `Themes`, **Visual c + +** 형식의 새 항목 추가 > **XAML** > **XAML 보기**하 고 "Generic.xaml" 라는 이름을 지정 합니다. 폴더 및 파일 이름을 맺어야 사용자 지정 컨트롤에 대 한 기본 스타일을 찾으려면 XAML 프레임 워크에 대 한 순서로 같이 합니다. 기본 내용을 삭제 `Generic.xaml`, 아래 태그에 붙여 넣습니다.

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

이 경우 기본 스타일을 설정 하는 유일한 속성은 컨트롤 템플릿. 템플릿을은 정사각형 (해당 백그라운드 XAML [**컨트롤**](/uwp/api/windows.ui.xaml.controls.control) 형식의 모든 인스턴스가 **백그라운드** 속성에 바인딩된) 및 텍스트 요소 (텍스트가 있는 **BgLabelControl::Label** 종속성 속성에 바인딩된)로 이루어져 있습니다.

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>메인 UI 페이지를 **BgLabelControl** 의 인스턴스를 추가 합니다.

`MainPage.xaml`을 엽니다. 여기에는 메인 UI 페이지에 사용할 XAML 태그가 포함되어 있습니다. **StackPanel**) (내 **단추** 요소, 직후 다음 태그를 추가 합니다.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

또한 다음 include 지시문을 추가 `MainPage.h` **MainPage** 유형 (XAML 태그와 명령적 코드를 컴파일하는 조합) **BgLabelControl** 사용자 지정 컨트롤 유형 인식 하는 수 있도록 합니다.

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

이제 프로젝트를 빌드하고 실행합니다. 기본 컨트롤 템플릿을 바인딩는 배경 브러시를 및 태그에서 **BgLabelControl** 인스턴스의 레이블을 표시 됩니다.

이 연습에서는 사용자 지정 (템플릿) 컨트롤의 간단한 예제에 설명 했습니다 C + + WinRT 합니다. 임의로 풍부 하 고 완전 한 기능의 고유한 사용자 지정 컨트롤을 만들 수 있습니다. 예를 들어, 사용자 지정 컨트롤에 다른 편집 가능한 데이터 그리드, 동영상 플레이어, 또는 시각화 도우미의 3D 기 하 도형으로 복잡 한 형태로 걸릴 수 있습니다.

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>구현 *재정의 가능한* **MeasureOverride** **OnApplyTemplate** 등의 기능

사용자 지정 컨트롤을 파생 [**컨트롤**](/uwp/api/windows.ui.xaml.controls.control) 런타임 클래스에서 기본 런타임 클래스에서 파생 그 자체가 추가 합니다. 및 **제어**, [**FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement)및 파생된 클래스에서 재정의할 수 있는 [**UIElement**](/uwp/api/windows.ui.xaml.uielement) 의 재정의 가능한 가지가 있습니다. 이렇게 하는 방법을 보여 주는 코드 예제는 다음과 같습니다.

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

*재정의 가능* 함수 자체 다르게에 있는 다른 언어 프로젝션. C#에서 예를 들어 재정의 가능한 함수 일반적으로 표시 보호 된 가상 함수. C + + 가상도 보호는 WinRT, 하지만 여전히 재정의 하 여과 위에 표시 된 대로 사용자 지정 구현을 제공할 수 있습니다.

## <a name="important-apis"></a>중요 API
* [컨트롤 클래스](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty 클래스](/uwp/api/windows.ui.xaml.dependencyproperty)
* [FrameworkElement 클래스](/uwp/api/windows.ui.xaml.frameworkelement)
* [UIElement 클래스](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>관련 항목
* [컨트롤 템플릿](/windows/uwp/design/controls-and-patterns/control-templates)
* [사용자 지정 종속성 속성](/windows/uwp/xaml-platform/custom-dependency-properties)
