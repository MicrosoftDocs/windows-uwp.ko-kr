---
author: stevewhims
description: 이 항목에서는 C +를 사용 하 여 간단한 사용자 지정 컨트롤 만들기 (영문)의 단계는 조정과 + / WinRT 합니다. 직접 기능 리치 클라이언트 및 사용자 지정할 수 있는 UI 컨트롤을 만들려면 정보를 여기에 구축할 수 있습니다.
title: XAML 사용자 지정 (템플릿) 컨트롤을 C + + / WinRT
ms.author: stwhi
ms.date: 08/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c + +, cpp, winrt, 프로젝션, XAML, 사용자 지정, 템플릿, 컨트롤
ms.localizationpriority: medium
ms.openlocfilehash: c108175c66d27b2cdbd910a0f7653ca1befb68e9
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2843695"
---
# <a name="xaml-custom-templated-controls-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>XAML 사용 하 여 사용자 지정 (템플릿) 컨트롤 [C + + / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

XAML [컨트롤](/uwp/api/windows.ui.xaml.controls.control) 종류에 따라 사용자 지정 컨트롤을 만들려면-UI (사용자 인터페이스) 스택 제공 하는 유연성은 유니버설 Windows 플랫폼 (UWP)의 가장 강력한 기능 중 하나입니다. XAML UI 프레임 워크는 [사용자 지정 종속성 속성](/windows/uwp/xaml-platform/custom-dependency-properties) 및 연결 된 속성 및 [컨트롤 서식 파일](/windows/uwp/design/controls-and-patterns/control-templates)을 쉽게 기능 향상 및 사용자 지정할 수 있는 컨트롤을 만들 수 있도록 하는 등의 기능을 제공 합니다. 이 항목에서는 C +를 사용 하 여 사용자 지정 (템플릿) 컨트롤을 만드는 단계 조정과 + / WinRT 합니다.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>빈 응용 프로그램 (BgLabelControlApp) 만들기
먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. 만들기를 **Visual c + + 빈 응용 프로그램 (C + + / WinRT)** 프로젝트, 및 *BgLabelControlApp*라는 이름을 지정 합니다.

사용자 지정 (템플릿) 컨트롤을 나타내는 새 클래스를 작성 하려고 합니다. 또한 동일한 컴파일 단위 내에서 클래스를 작성하고 사용할 계획입니다. 하지만이 클래스 및 XAML 태그에서 런타임 클래스 수 하려는 이유를 인스턴스화할 수 있도록 원합니다. 그 밖에도 런타임 클래스를 작성하고 사용하는 데 모두 C++/WinRT를 사용합니다.

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

위의 목록 (DP) 종속성 속성을 선언할 때 수행 하는 패턴을 보여줍니다. 각 DP에 두 부분인 있습니다. 첫째, 형식 [속성](/uwp/api/windows.ui.xaml.dependencyproperty)의 읽기 전용 정적 속성을 선언 합니다. DP plus *속성*의 이름이 있습니다. 이 정적 속성을 사용 하 여 구현에서 됩니다. 둘째, 형식과 DP 사용자의 이름을 사용 하 여 읽기 / 쓰기 인스턴스 속성을 선언합니다.

> [!NOTE]
> 부동 소수점 형식과 DP 하려는 경우 다음 쉽게 `double` (`Double` [MIDL](/uwp/midl-3/)3.0에서). 선언 및 형식의 DP 구현 `float` (`Single` MIDL에서), 오류 메시지에 결과 XAML 태그에서 해당 DP에 대 한 값을 설정 하 고 *를 텍스트에서 'Windows.Foundation.Single'를 만들지 못했습니다. '<NUMBER>'* 합니다.

파일을 저장하고 프로젝트를 빌드합니다. 빌드 과정에서 `midl.exe` 도구가 실행되어 런타임 클래스를 설명하는 Windows 런타임 메타데이터 파일(`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`)을 생성합니다. 그런 다음 `cppwinrt.exe` 도구가 실행되어 런타임 클래스를 작성하거나 사용하도록 지원하는 소스 코드 파일을 생성합니다. 이러한 파일 시작 하면 IDL에 선언 된 **BgLabelControl** 런타임 클래스를 구현 하는데 스텁을 포함 합니다. 이 스텁이 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h`와 `BgLabelControl.cpp`입니다.

스텁 파일인 `BgLabelControl.h`와 `BgLabelControl.cpp`를 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\`에서 프로젝트 폴더인 `\BgLabelControlApp\BgLabelControlApp\`로 복사합니다. **솔루션 탐색기**에서 **모든 파일 표시**가 설정되어 있는지 확인합니다. 복사한 스텁 파일을 마우스 오른쪽 버튼으로 클릭하고 **프로젝트에 포함**을 클릭합니다.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>**BgLabelControl** 사용자 지정 컨트롤 클래스를 구현 합니다.
이제 `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h`와 `BgLabelControl.cpp`를 열고 런타임 클래스를 구현합니다. `BgLabelControl.h`, 기본 스타일 키 설정, **레이블** 및 **LabelProperty**을 구현 하려면 생성자를 변경, 종속성 속성의 값에 대 한 변경 내용을 처리할 수 있는 **OnLabelChanged** 라는 정적 이벤트 처리기를 추가 하 고 개인 구성원 추가 **LabelProperty**에 대 한 지원 필드를 저장 합니다.

이 연습에서는 **OnLabelChanged**사용 하지 않을 것입니다. 하지만 속성 변경 콜백을 함께 종속성 속성을 등록 하는 방법을 볼 수 있도록 인스턴스가 있습니다.

에 추가한 후에 `BgLabelControl.h` 다음과 같습니다.

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

해당 생성자 **BgLabelControl** 자체에 대 한 기본 스타일 키를 설정합니다. 하지만 어떤 *는* 기본 스타일? 기본 스타일을 사용자 지정 (템플릿) 제어 해야&mdash;기본 컨트롤 서식 파일을 포함 하&mdash;는 컨트롤의 소비자 스타일 및/또는 서식 파일 설정 하지 하는 경우와 자체적으로 렌더링 사용할 수 있습니다. 이 섹션에서는 기본 스타일을 포함 하는 프로젝트를 태그 파일을 추가 합니다.

프로젝트 노드에서 새 폴더를 만들고 "테마" 라는 이름을 지정 합니다. 아래에서 `Themes`, **Visual c + +** 형식의 새 항목 추가 > **XAML** > **XAML 보기**하 고 "Generic.xaml" 라는 이름을 지정 합니다. 폴더 및 파일 이름을 포함 해야하는 다음과 같은 사용자 지정 컨트롤에 대 한 기본 스타일을 찾으려고 XAML 프레임 워크에 대 한 순서 대로 합니다. 기본 내용을 삭제 `Generic.xaml`, 아래 태그를 붙여넣습니다.

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

이 경우에 기본 스타일을 설정 하는 유일한 속성은 컨트롤 서식 파일입니다. 서식 파일 (해당 배경 XAML [컨트롤](/uwp/api/windows.ui.xaml.controls.control) 종류의 모든 인스턴스에 있는 **Background** 속성에 바인딩된) 사각형, 그리고 (텍스트가 있는 **BgLabelControl::Label** 종속성 속성에 바인딩된) 텍스트 요소를 구성 됩니다.

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>주 UI 페이지 **BgLabelControl** 의 인스턴스를 추가 합니다.

`MainPage.xaml`을 엽니다. 여기에는 메인 UI 페이지에 사용할 XAML 태그가 포함되어 있습니다. (내부) ( **StackPanel**) **단추** 요소 바로 뒤 다음 태그를 추가 합니다.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

또한 다음 지시문을 포함을 추가 `MainPage.h` **기본 페이지** 유형 (XAML 태그와 필수적 코드 컴파일의 조합)가 **BgLabelControl** 사용자 지정 컨트롤 종류를 인식 되도록 합니다.

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

이제 프로젝트를 빌드하고 실행합니다. 기본 컨트롤 서식 파일은 바인딩 (영문) 배경 브러시 하 고 태그에서 **BgLabelControl** 인스턴스의 레이블을 볼 수 있습니다.

이 연습에서는 보여준 사용자 지정 (템플릿) 컨트롤의 간단한 예제 C + + / WinRT 합니다. 임의로 풍부 하 고 완전 한 기능의 사용자 지정 컨트롤을 만들 수 있습니다. 예, 사용자 지정 컨트롤 무엇 인가 편집 가능한 데이터 눈금, 비디오 플레이어, 또는 3 차원 기 시각화 도우미로 복잡 한 형태의 걸릴 수 있습니다.

## <a name="important-apis"></a>중요 API
* [컨트롤](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)

## <a name="related-topics"></a>관련 항목
* [컨트롤 템플릿](/windows/uwp/design/controls-and-patterns/control-templates)
* [사용자 지정 종속성 속성](/windows/uwp/xaml-platform/custom-dependency-properties)
