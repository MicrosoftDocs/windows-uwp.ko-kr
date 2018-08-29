---
author: stevewhims
description: 이 항목에서는 C +를 사용 하 여 간단한 사용자 지정 컨트롤을 만드는 과정을 단계별로 안내 + WinRT 합니다. 고유한 기능이 풍부 하 고, 사용자 지정 UI 컨트롤을 만드는 정보를 여기에서 만들 수 있습니다.
title: C++/WinRT을 사용한 XAML 사용자 지정(템플릿) 컨트롤
ms.author: stwhi
ms.date: 08/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c + +, cpp, winrt, 프로젝션, XAML, 사용자 지정, 템플릿, 컨트롤
ms.localizationpriority: medium
ms.openlocfilehash: c108175c66d27b2cdbd910a0f7653ca1befb68e9
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/29/2018
ms.locfileid: "2917312"
---
# <a name="xaml-custom-templated-controls-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>XAML 사용자 지정 (템플릿 기반) 컨트롤을 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

유니버설 Windows 플랫폼 (UWP)의 가장 강력한 기능 중 하나를 XAML [컨트롤](/uwp/api/windows.ui.xaml.controls.control) 형식에 따라 사용자 지정 컨트롤을 만드는 사용자 인터페이스 (UI) 스택을 제공 하는 유연성입니다. XAML UI 프레임 워크는 [사용자 지정 종속성 속성](/windows/uwp/xaml-platform/custom-dependency-properties) 및 연결 된 속성 및 [컨트롤 템플릿](/windows/uwp/design/controls-and-patterns/control-templates)을 쉽게 기능이 풍부 하 고 사용자 지정 가능한 컨트롤을 만들 수 있는 등의 기능을 제공 합니다. 이 항목에서는 C +를 사용 하 여 사용자 지정 (템플릿의) 컨트롤을 만드는 단계를 안내 + WinRT 합니다.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>빈 앱 (BgLabelControlApp) 만들기
먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. 만들기는 **Visual c + + Blank App (C + + WinRT)** 프로젝트를 만들어서 *BgLabelControlApp*이름을 지정 합니다.

사용자 지정 (템플릿 기반) 컨트롤을 나타내는 새 클래스를 작성 하겠습니다. 또한 동일한 컴파일 단위 내에서 클래스를 작성하고 사용할 계획입니다. 하지만 런타임 클래스가 될 것은 결국이 클래스 XAML 태그에서 인스턴스화할 수 있습니다. 그 밖에도 런타임 클래스를 작성하고 사용하는 데 모두 C++/WinRT를 사용합니다.

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

위의 목록을 종속성 속성 DP ()를 선언 하는 경우에 나오는 패턴을 보여 줍니다. 각 DP 하는 방법은 두 가지입니다. 먼저 [DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)형식의 읽기 전용 정적 속성을 선언합니다. DP와 *속성*이름이 있습니다. 구현에서이 정적 속성을 사용 합니다. 둘째, 형식과 DP의 이름을 사용 하 여 읽기-쓰기 인스턴스 속성을 선언합니다.

> [!NOTE]
> 부동 소수점 형식으로는 DP를 원하는 경우 사항을 `double` (`Double` [MIDL](/uwp/midl-3/)3.0에서). 선언 및 구현 형식 DP `float` (`Single` 있지만 midl에서), 다음 XAML 태그에서 해당 DP 값을 설정 하면 오류가 발생 하 고 *텍스트에서 'Windows.Foundation.Single'를 만들지 못했습니다 '<NUMBER>'*.

파일을 저장하고 프로젝트를 빌드합니다. 빌드 과정에서 `midl.exe` 도구가 실행되어 런타임 클래스를 설명하는 Windows 런타임 메타데이터 파일(`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`)을 생성합니다. 그런 다음 `cppwinrt.exe` 도구가 실행되어 런타임 클래스를 작성하거나 사용하도록 지원하는 소스 코드 파일을 생성합니다. 이러한 파일에는 idl로 선언한 **BgLabelControl** 런타임 클래스의 구현을 시작할 수 있는 스텁이 포함 됩니다. 이 스텁이 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h`와 `BgLabelControl.cpp`입니다.

스텁 파일인 `BgLabelControl.h`와 `BgLabelControl.cpp`를 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\`에서 프로젝트 폴더인 `\BgLabelControlApp\BgLabelControlApp\`로 복사합니다. **솔루션 탐색기**에서 **모든 파일 표시**가 설정되어 있는지 확인합니다. 복사한 스텁 파일을 마우스 오른쪽 버튼으로 클릭하고 **프로젝트에 포함**을 클릭합니다.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>**BgLabelControl** 사용자 지정 컨트롤 클래스를 구현 합니다.
이제 `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h`와 `BgLabelControl.cpp`를 열고 런타임 클래스를 구현합니다. `BgLabelControl.h`기본 스타일 키를 설정, **레이블** 및 **연결: LabelProperty**구현 하기 위한 생성자 변경, **OnLabelChanged** 처리 종속성 속성의 값을 변경 하 라는 정적 이벤트 처리기를 추가 및 전용 멤버를 추가 합니다. **연결: LabelProperty**에 대 한 지원 필드를 저장 합니다.

이 연습에서는 **OnLabelChanged**사용 하지 않을 것입니다. 하지만 속성 변경 콜백이 있는 종속성 속성을 등록 하는 방법을 볼 수 있도록이 있습니다.

추가한 후에 `BgLabelControl.h` 이 같습니다.

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

    static void OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e);

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

void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e) {}
...
```

## <a name="design-the-default-style-for-bglabelcontrol"></a>**BgLabelControl** 에 대 한 기본 스타일을 디자인 합니다.

해당 생성자 **BgLabelControl** 자체에 대 한 기본 스타일 키를 설정합니다. 하지만 ** 는 기본 스타일? 사용자 지정 (템플릿 기반) 컨트롤은 기본 스타일 해야 하는 경우&mdash;기본 컨트롤 템플릿을 포함 된&mdash;컨트롤의 소비자 스타일 및 템플릿을 설정 하지 않는 경우 사용 하 여 자체적으로 렌더링 하는 데 사용할 수 있는 합니다. 이 섹션에서는 태그 파일은 기본 스타일을 포함 하는 프로젝트에 추가 됩니다.

프로젝트 노드에서 새 폴더를 만들고 "Themes" 라는 이름을 지정 합니다. 아래에서 `Themes`, **Visual c + +** 형식의 새 항목 추가 > **XAML** > **XAML 보기**하 고 "Generic.xaml" 라는 이름을 지정 합니다. 폴더 및 파일 이름을 사용자 지정 컨트롤에 대 한 기본 스타일을 찾을 수 XAML 프레임 워크에서 어떤 양상이 해야 합니다. 기본 내용을 삭제 `Generic.xaml`, 아래 태그에 붙여 넣습니다.

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

이 경우 기본 스타일을 설정 하는 유일한 속성에는 컨트롤 템플릿을입니다. 템플릿을은 정사각형 (XAML [컨트롤](/uwp/api/windows.ui.xaml.controls.control) 형식의 모든 인스턴스에 있는 **Background** 속성에 바인딩된 인 배경) 및 텍스트 요소 (텍스트가 있는 **BgLabelControl::Label** 종속성 속성에 바인딩된)로 구성 됩니다.

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>**BgLabelControl** 의 인스턴스를 메인 UI 페이지 추가

`MainPage.xaml`을 엽니다. 여기에는 메인 UI 페이지에 사용할 XAML 태그가 포함되어 있습니다. **단추** 요소 (내 **StackPanel**) 직후 다음 태그를 추가 합니다.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

또한 다음 include 지시문을 추가 `MainPage.h` **MainPage** 유형 (XAML 태그와 명령적 코드를 컴파일하는 조합)는 **BgLabelControl** 사용자 지정 컨트롤 형식을 알아야 합니다.

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

이제 프로젝트를 빌드하고 실행합니다. 태그에서 **BgLabelControl** 인스턴스의 레이블에 배경 브러시를 기본 컨트롤 템플릿이 바인딩된 표시 됩니다.

이 연습에서는 사용자 지정 (템플릿의) 컨트롤의 간단한 예제에 설명 했습니다 C + + WinRT 합니다. 임의로 풍부 하 고 완전 한 기능의 고유한 사용자 지정 컨트롤을 만들 수 있습니다. 예를 들어, 사용자 지정 컨트롤에 있는 편집 가능한 데이터 그리드, 비디오 플레이어 또는 3D 기 하 도형의 시각화 도우미 복잡 한 형태의 걸릴 수 있습니다.

## <a name="important-apis"></a>중요 API
* [컨트롤](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)

## <a name="related-topics"></a>관련 항목
* [컨트롤 템플릿](/windows/uwp/design/controls-and-patterns/control-templates)
* [사용자 지정 종속성 속성](/windows/uwp/xaml-platform/custom-dependency-properties)
