---
description: C++/WinRT를 사용하여 간단한 사용자 지정 컨트롤을 만드는 단계를 안내합니다. 여기에 나와 있는 정보에 따라 기능이 풍부하고 사용자 지정 가능한 UI 컨트롤을 만들 수 있습니다.
title: C++/WinRT을 사용한 XAML 사용자 지정(템플릿) 컨트롤
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10, uwp, standard, c + +, cpp, winrt, 프로젝션, XAML, 사용자 지정, 템플릿 컨트롤
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ce4f7eea074233c625a2cc92ef773f0b06c2be9f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635148"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>C++/WinRT을 사용한 XAML 사용자 지정(템플릿) 컨트롤

> [!IMPORTANT]
> 핵심 개념 및 용어를 사용 하 고 사용 하 여 런타임 클래스를 작성 하는 방법에 대 한 이해를 지 원하는 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)를 참조 하세요 [C +를 사용 하 여 Api 사용 + WinRT](consume-apis.md) 및 [작성 Api C + + WinRT](author-apis.md)합니다.

유니버설 Windows 플랫폼 (UWP)의 가장 강력한 기능 중 하나는 사용자 인터페이스 (UI) 스택에 XAML 기반 사용자 지정 컨트롤을 만드는 데 제공 하는 유연성 [ **제어** ](/uwp/api/windows.ui.xaml.controls.control) 형식입니다. XAML UI 프레임 워크와 같은 기능을 제공 [사용자 지정 종속성 속성](/windows/uwp/xaml-platform/custom-dependency-properties) 하 고 [연결 된 속성](/windows/uwp/xaml-platform/custom-attached-properties), 및 [컨트롤 템플릿](/windows/uwp/design/controls-and-patterns/control-templates)를 쉽게 만들 수 있도록 기능이 풍부 하 고 사용자 지정할 수 있는 컨트롤입니다. 이 항목에서는 C +를 사용 하 여 사용자 지정 (템플릿 기반) 컨트롤을 만드는 단계를 통해 + WinRT 합니다.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>빈 앱 (BgLabelControlApp) 만들기
먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. 만들기는 **Visual c + +** > **Windows 범용** > **비어 있는 앱 (C + + /cli WinRT)** 프로젝트를 만들고 이름을 *BgLabelControlApp* . 이 항목의 뒷부분에 나오는 섹션에서 이동 하 여 프로젝트를 빌드할 (그때 까지는 빌드 안 함).

사용자 지정 (템플릿 기반) 컨트롤을 나타내는 새 클래스를 작성 하겠습니다. 또한 동일한 컴파일 단위 내에서 클래스를 작성하고 사용할 계획입니다. 하지만 런타임 클래스일 하려는 이유는이 클래스에 대 한 XAML 태그에서 인스턴스화할 수 하고자 합니다. 그 밖에도 런타임 클래스를 작성하고 사용하는 데 모두 C++/WinRT를 사용합니다.

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

위의 목록 (DP) 종속성 속성을 선언할 때 따르는 패턴을 보여 줍니다. 각 DP 하는 방법은 두 가지입니다. 첫째, 형식의 읽기 전용 정적 속성을 선언 [ **DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty)합니다. 더하기에 DP의 이름이 *속성*합니다. 구현에서이 정적 속성을 사용 합니다. 둘째, 형식 및 프로그램 DP의 이름을 사용 하 여 읽기 / 쓰기 인스턴스 속성을 선언합니다. 작성 하려는 경우는 *연결 된 속성* DP), (대신의 코드 예제를 참조 하십시오 [사용자 지정 연결 된 속성](/windows/uwp/xaml-platform/custom-attached-properties)합니다.

> [!NOTE]
> 부동 소수점 형식과 DP를 하려는 경우 다음 있도록 `double` (`Double` 에 [MIDL 3.0](/uwp/midl-3/)). 선언 및 구현 형식의 DP `float` (`Single` MIDL에서), 오류 발생 후 XAML 태그에서 해당 DP에 대 한 값을 설정 하 고 *'Windows.Foundation.Single' 텍스트에서 만들지 못했습니다. '<NUMBER>'*.

파일을 저장하고 프로젝트를 빌드합니다. 빌드 과정에서 `midl.exe` 도구가 실행되어 런타임 클래스를 설명하는 Windows 런타임 메타데이터 파일(`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`)을 생성합니다. 그런 다음 `cppwinrt.exe` 도구가 실행되어 런타임 클래스를 작성하거나 사용하도록 지원하는 소스 코드 파일을 생성합니다. 스텁 구현 하는 데 이러한 파일에 포함 된 **BgLabelControl** 프로그램 IDL에 선언 된 런타임 클래스. 이 스텁이 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h`와 `BgLabelControl.cpp`입니다.

스텁 파일인 `BgLabelControl.h`와 `BgLabelControl.cpp`를 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\`에서 프로젝트 폴더인 `\BgLabelControlApp\BgLabelControlApp\`로 복사합니다. **솔루션 탐색기**에서 **모든 파일 표시**가 설정되어 있는지 확인합니다. 복사한 스텁 파일을 마우스 오른쪽 버튼으로 클릭하고 **프로젝트에 포함**을 클릭합니다.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>구현 된 **BgLabelControl** 사용자 지정 컨트롤 클래스
이제 `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h`와 `BgLabelControl.cpp`를 열고 런타임 클래스를 구현합니다. `BgLabelControl.h`, 생성자는 기본 스타일 키를 구현 설정 변경 **레이블을** 및 **LabelProperty**, 라는 정적 이벤트 처리기를 추가 **OnLabelChanged** 를 종속성 속성의 값으로 변경 내용을 처리 및 저장에 대 한 지원 필드를 private 멤버를 추가 **LabelProperty**합니다.

추가한 후에 `BgLabelControl.h` 다음과 유사 합니다.

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

이 연습에서는 사용 하지 않을 **OnLabelChanged**합니다. 하지만 없는 속성 변경 콜백을 사용 하 여 종속성 속성을 등록 하는 방법을 볼 수 있도록 합니다. 구현의 **OnLabelChanged** 도 기본 프로젝션 된 형식에서 파생된 된 예상된 형식을 가져오는 방법을 보여 줍니다 (기본 예상 형식이 **DependencyObject**,이 경우). 및 다음 프로젝션 된 형식을 구현 하는 형식에 대 한 포인터를 가져오는 방법을 보여 줍니다. 두 번째 작업 자연스럽 게만 되도록 프로젝션 된 형식 (즉, 런타임 클래스를 구현 하는 프로젝트)를 구현 하는 프로젝트에서 가능 합니다.

> [!NOTE]
> Windows SDK (Windows 10, 버전 1809) 10.0.17763.0 버전을 설치 하지 않은 경우 나중에 다음 호출 해야 [ **winrt::from_abi** ](/uwp/cpp-ref-for-winrt/from-abi) 위의 종속성 속성 변경된 이벤트 처리기에서 대신 [ **winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self)합니다.

## <a name="design-the-default-style-for-bglabelcontrol"></a>에 대 한 기본 스타일 디자인 **BgLabelControl**

해당 생성자에서 **BgLabelControl** 자체에 대 한 기본 스타일 키를 설정 합니다. 그렇다면 *는* 기본 스타일? 사용자 지정 (템플릿 기반) 컨트롤에서 기본 스타일 해야&mdash;기본 컨트롤 템플릿이 포함 된&mdash;스타일 및/또는 템플릿 소비자 컨트롤에 설정 하지 않는 경우 사용 하 여 자체적으로 렌더링 하는 데 사용할 수 있는 합니다. 이 섹션에는 기본 스타일을 포함 하는 프로젝트에 마크업 파일을 추가 합니다.

프로젝트 노드에서 새 폴더를 만들고 "테마" 라는 이름을 지정 합니다. 아래 `Themes`, 유형의 새 항목 추가 **Visual c + +** > **XAML** > **XAML 뷰**, "Generic.xaml" 이라는 이름을 지정 합니다. 폴더 및 파일 이름을 사용자 지정 컨트롤에 대 한 기본 스타일을 찾으려면 XAML 프레임 워크에 대 한 순서 대로 다음과 같은 수 해야 합니다. 기본 내용을 삭제 `Generic.xaml`, 아래의 태그에 붙여넣습니다.

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

이 경우 기본 스타일을 설정 하는 유일한 속성은 컨트롤 템플릿입니다. 템플릿을 사각형으로 이루어져 (해당 배경 바인딩되는 **백그라운드** 속성은는 XAML의 모든 인스턴스 [ **컨트롤** ](/uwp/api/windows.ui.xaml.controls.control) 형식), 및 (해당 텍스트 요소 텍스트에 바인딩되는 **BgLabelControl::Label** 종속성 속성).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>인스턴스를 추가 **BgLabelControl** 주 UI 페이지

`MainPage.xaml`을 엽니다. 여기에는 메인 UI 페이지에 사용할 XAML 태그가 포함되어 있습니다. 바로 뒤를 **단추** 요소 (내 합니다 **StackPanel**), 다음 태그를 추가 합니다.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

또한 다음 include 지시문을 추가 `MainPage.h` 있도록 합니다 **MainPage** 형식 (명령적 코드 및 XAML 태그 컴파일의 조합) 인지를 **BgLabelControl** 사용자 지정 컨트롤 형식입니다. 사용 하려는 경우 **BgLabelControl** 다른 XAML 페이지에서 추가한이 너무 동일한 include 지시문이 페이지에 대 한 헤더 파일에 있습니다. 또는 또는 입력 단일 include 지시문 미리 컴파일된 헤더 파일에 있습니다.

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

이제 프로젝트를 빌드하고 실행합니다. 기본 컨트롤 템플릿이 배경 브러시를 레이블 하의 바인딩된를 표시 합니다 **BgLabelControl** 태그에는 인스턴스.

이 연습에서는 사용자 지정 (템플릿 기반) 컨트롤의 간단한 예제에 설명 했습니다 C + + /cli WinRT 합니다. 임의로 풍부 하 고 모든 기능을 갖춘 사용자 지정 컨트롤을 만들 수 있습니다. 예를 들어, 사용자 지정 컨트롤에는 편집 가능한 데이터 표, 비디오 플레이어, 또는 3 차원 기 하 도형의 시각화 도우미 것 만큼 복잡 한 항목 형식의 걸릴 수 있습니다.

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>구현 *overridable* 와 같은 함수 **MeasureOverride** 고 **OnApplyTemplate**

사용자 지정 컨트롤을 파생 합니다 [ **제어** ](/uwp/api/windows.ui.xaml.controls.control) 기본 런타임 클래스에서 파생 되는 런타임 클래스에서 직접 추가 합니다. 재정의 가능한 메서드가 되며 **제어**를 [ **FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement), 및 [ **UIElement** ](/uwp/api/windows.ui.xaml.uielement) 파생된 클래스에서 재정의할 수 있습니다. 작업을 수행 하는 방법을 보여 주는 코드 예제는 다음과 같습니다.

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

*재정의 가능한* 함수 다른 언어에서 자신을 다르게 표시 합니다. C#, 예를 들어, 재정의 가능 함수는 일반적으로 나타납니다 보호 된 가상 함수입니다. C + + /cli WinRT, 가상 또는 보호 하지만 여전히 재정의할 하 고 위에 표시 된 대로 사용자 지정 구현을 제공할 수 있습니다.

## <a name="important-apis"></a>중요 API
* [컨트롤 클래스](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty 클래스](/uwp/api/windows.ui.xaml.dependencyproperty)
* [FrameworkElement 클래스](/uwp/api/windows.ui.xaml.frameworkelement)
* [UIElement 클래스](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>관련 항목
* [컨트롤 템플릿](/windows/uwp/design/controls-and-patterns/control-templates)
* [사용자 지정 종속성 속성](/windows/uwp/xaml-platform/custom-dependency-properties)
