---
description: 이 문서에서는 C++/WinRT를 사용하여 WinUI 3용 XAML 템플릿 기반 컨트롤을 만드는 과정을 안내합니다.
title: C++/WinRT를 사용하는 WinUI 3 앱용 템플릿 기반 XAML 컨트롤
ms.date: 07/09/2020
ms.topic: article
keywords: Windows 10, UWP, 사용자 지정 컨트롤, 템플릿 기반 컨트롤, WinUI, C++/WinRT
ms.author: drewbat
author: drewbatgit
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: d319374791eeb0a02b0291c66f25f55e31bcbc4b
ms.sourcegitcommit: 6759309a3fbb6ede498c95c04c05f57a074ab070
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99069170"
---
# <a name="templated-xaml-controls-for-winui-3-apps-with-cwinrt"></a>C++/WinRT를 사용하는 WinUI 3 앱용 템플릿 기반 XAML 컨트롤

이 문서에서는 C++/WinRT를 사용하여 WinUI 3용 템플릿 기반 XAML 컨트롤을 만드는 과정을 안내합니다. 템플릿 기반 컨트롤은 **Microsoft.UI.Xaml.Controls.Control** 에서 상속되며, XAML 컨트롤 템플릿을 사용하여 사용자 지정할 수 있는 시각적 구조와 시각적 동작을 포함합니다. 이 문서는 [C++/WinRT를 사용한 XAML 사용자 지정(템플릿 기반) 컨트롤](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl) 문서와 동일한 시나리오를 설명하지만 WinUI 3을 사용하도록 조정되었습니다.

이 문서의 단계를 수행하기 전에 먼저 개발 환경이 WinUI 3 앱을 만들도록 구성되어 있는지 확인해야 합니다. 설정에 대한 자세한 내용은 [데스크톱 앱용 WinUI 3 시작](./get-started-winui3-for-desktop.md)을 참조하세요. 또한 [Visual Studio Marketplace](https://marketplace.visualstudio.com)에서 최신 버전의 [C++/WinRT VSIX(Visual Studio 확장)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)를 다운로드하여 설치해야 합니다.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>비어 있는 앱(BgLabelControlApp) 만들기

먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. `Create a new project` 대화 상자에서 **비어 있는 앱(UWP의 WinUI)** 프로젝트 템플릿을 선택하고, C++ 언어 버전을 선택해야 합니다. 파일 이름이 아래 예제의 코드와 일치하도록 프로젝트 이름을 "BgLabelControlApp"으로 설정합니다. **대상 버전** 을 Windows 10, 버전 1903(빌드 18362)으로, **최소 버전** 을 Windows 10, 버전 1803(빌드 17134)으로 설정합니다. 이 연습은 **빈 앱, 패키지됨(데스크톱의 WinUI)** 프로젝트 템플릿을 사용하여 만든 데스크톱 앱에서도 작동하며, **BgLabelControlApp(데스크톱)** 프로젝트의 모든 단계를 수행하면 됩니다.

![비어 있는 앱 프로젝트 템플릿](images/WinUI-cpp-newproject-UWP.png)

## <a name="add-a-templated-control-to-your-app"></a>앱에 템플릿 기반 컨트롤 추가

템플릿 기반 컨트롤을 추가하려면 도구 모음에서 **프로젝트** 메뉴를 클릭하거나 **솔루션 탐색기** 에서 마우스 오른쪽 단추로 프로젝트를 클릭하고, **새 항목 추가** 를 선택합니다. **Visual C++->WinUI** 아래에서 **사용자 지정 컨트롤(WinUI)** 템플릿을 선택합니다. 새 컨트롤의 이름을 "BgLabelControl"로 지정하고, *추가* 를 클릭합니다. 그러면 세 개의 새 파일이 프로젝트에 추가됩니다. `BgLabelControl.h`는 컨트롤 선언을 포함하는 헤더이고, `BgLabelControl.cpp`는 컨트롤의 C++/WinRT 구현을 포함합니다. `BgLabelControl.idl`은 컨트롤을 런타임 클래스로 인스턴스화할 수 있는 인터페이스 정의 파일입니다.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>BgLabelControl 사용자 지정 컨트롤 클래스 구현

다음 단계에서는 런타임 클래스를 구현하도록 프로젝트 디렉터리에 있는 `BgLabelControl.idl`, `BgLabelControl.h` 및 `BgLabelControl.cpp` 파일의 코드를 업데이트합니다. 


템플릿 기반 컨트롤 클래스는 XAML 태그에서 인스턴스화되며, 이에 따라 런타임 클래스가 됩니다. 완성된 프로젝트를 빌드하는 경우 MIDL 컴파일러(midl.exe)에서 `BgLabelControl.idl` 파일을 사용하여 구성 요소의 소비자에서 참조할 컨트롤에 대한 Windows 런타임 메타데이터 파일(.winmd)을 생성합니다. 런타임 클래스를 만드는 방법에 대한 자세한 내용은 [C++/WinRT를 사용하여 API 작성](/windows/uwp/cpp-and-winrt-apis/author-apis)을 참조하세요.

만들고 있는 템플릿 기반 컨트롤은 컨트롤의 레이블로 사용할 문자열인 단일 속성을 공개합니다. `BgLabelControl.idl`의 내용을 다음 코드로 바꿉니다.

```cppwinrt
// BgLabelControl.idl
namespace BgLabelControlApp
{
    runtimeclass BgLabelControl : Microsoft.UI.Xaml.Controls.Control
    {
        BgLabelControl();
        static Microsoft.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}
```

위 목록에서는 DP(종속성 속성)를 선언할 때 따라야 하는 패턴을 보여 줍니다. 각 DP에는 두 개의 조각이 있습니다. 먼저 DependencyProperty 형식의 읽기 전용 정적 속성을 선언합니다. 여기에는 DP 및 Property라는 이름이 포함됩니다. 구현에서 이 정적 속성을 사용합니다. 두 번째로, DP의 형식 및 이름으로 읽기-쓰기 인스턴스 속성을 선언합니다. DP 대신 연결된 속성을 작성하려면 [사용자 지정 연결된 속성](/windows/uwp/xaml-platform/custom-attached-properties)의 코드 예제를 참조하세요.

위의 코드에서 참조되는 XAML 클래스는 Microsoft.UI.Xaml 네임스페이스에 있습니다. 이는 Windows.UI.XAML 네임스페이스에 정의된 UWP XAML 컨트롤과는 달리 이러한 항목을 WinUI 컨트롤로 구별하는 것입니다.

BgLabelControl.h의 내용을 다음 코드로 바꿉니다.

```cppwinrt
// BgLabelControl.h
#pragma once
#include "BgLabelControl.g.h"

namespace winrt::BgLabelControlApp::implementation
{
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

        static Microsoft::UI::Xaml::DependencyProperty LabelProperty() { return m_labelProperty; }

        static void OnLabelChanged(Microsoft::UI::Xaml::DependencyObject const&, Microsoft::UI::Xaml::DependencyPropertyChangedEventArgs const&);

    private:
        static Microsoft::UI::Xaml::DependencyProperty m_labelProperty;
    };
}
namespace winrt::BgLabelControlApp::factory_implementation
{
    struct BgLabelControl : BgLabelControlT<BgLabelControl, implementation::BgLabelControl>
    {
    };
}
```

위에 표시된 코드는 **Label** 및 **LabelProperty** 속성을 구현하고, 종속성 속성 값의 변경을 처리하는 **OnLabelChanged** 라는 정적 이벤트 처리기를 추가하고, **LabelProperty** 에 대한 지원 필드를 저장하는 프라이빗 멤버를 추가합니다. 헤더 파일에서 참조되는 XAML 클래스는 UWP UI 프레임워크에서 사용하는 Windows.UI.Xaml 네임스페이스 대신 WinUI 3 프레임워크에 속하는 Microsoft.UI.Xaml 네임스페이스에 있습니다.


다음으로, BgLabelControl.cpp의 내용을 다음 코드로 바꿉니다.

```cppwinrt
// BgLabelControl.cpp
#include "pch.h"
#include "BgLabelControl.h"
#if __has_include("BgLabelControl.g.cpp")
#include "BgLabelControl.g.cpp"
#endif

namespace winrt::BgLabelControlApp::implementation
{
    Microsoft::UI::Xaml::DependencyProperty BgLabelControl::m_labelProperty =
        Microsoft::UI::Xaml::DependencyProperty::Register(
            L"Label",
            winrt::xaml_typename<winrt::hstring>(),
            winrt::xaml_typename<BgLabelControlApp::BgLabelControl>(),
            Microsoft::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Microsoft::UI::Xaml::PropertyChangedCallback{ &BgLabelControl::OnLabelChanged } }
    );

    void BgLabelControl::OnLabelChanged(Microsoft::UI::Xaml::DependencyObject const& d, Microsoft::UI::Xaml::DependencyPropertyChangedEventArgs const& /* e */)
    {
        if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
        {
            // Call members of the projected type via theControl.

            BgLabelControlApp::implementation::BgLabelControl* ptr{ winrt::get_self<BgLabelControlApp::implementation::BgLabelControl>(theControl) };
            // Call members of the implementation type via ptr.
        }
    }
}
```
이 연습에서는 **OnLabelChanged** 콜백을 사용하지 않지만, 이 콜백은 속성 변경 콜백을 사용하여 종속성 속성을 등록하는 방법을 확인하기 위해 제공됩니다. 또한 **OnLabelChanged** 구현은 기본 프로젝션된 형식에서 파생된 프로젝션된 형식(이 경우 DependencyObject임)을 가져오는 방법도 보여 줍니다. 또한 프로젝션된 형식을 구현하는 형식에 대한 포인터를 가져오는 방법을 보여 줍니다. 이 두 번째 작업은 기본적으로 프로젝션된 형식을 구현하는 프로젝트(런타임 클래스를 구현하는 프로젝트)에서만 가능합니다.

[xaml_typename](/uwp/cpp-ref-for-winrt/xaml-typename) 함수는 WinUI 3 프로젝트 템플릿에 기본적으로 포함되지 않는 Windows.UI.Xaml.Interop 네임스페이스를 통해 제공됩니다. 이 네임스페이스와 연결된 헤더 파일(`pch.h`)을 포함하도록 프로젝트의 미리 컴파일된 헤더 파일에 줄을 추가합니다.

```cppwinrt
// pch.h
...
#include <winrt/Windows.UI.Xaml.Interop.h>
...
```



## <a name="define-the-default-style-for-bglabelcontrol"></a>BgLabelControl의 기본 스타일 정의

해당 생성자에서 **BgLabelControl** 은 스스로 기본 스타일 키를 설정합니다. 템플릿 기반 컨트롤에는 기본 컨트롤 템플릿이 포함된 기본 스타일이 있어야 합니다. 그러면 컨트롤의 소비자에서 스타일 및/또는 템플릿을 설정하지 않은 경우 자체적으로 렌더링하는 데 사용할 수 있습니다. 이 섹션에서는 기본 스타일을 포함하는 프로젝트에 태그 파일을 추가합니다.

**모든 파일 표시** 가 아직 설정되어 있는지 확인합니다(**솔루션 탐색기** 에서). 프로젝트 노드 아래에서 새 폴더(필터가 아니라 폴더)를 만들고 이름을 "Themes"로 지정합니다. `Themes` 아래에서 **Visual C++ > WinUI > 리소스 사전(WinUI)** 형식의 새 항목을 추가하고, 이름을 "Generic.xaml"로 지정합니다. XAML 프레임워크에서 템플릿 기반 컨트롤의 기본 스타일을 찾을 수 있도록 폴더 및 파일 이름이 이와 비슷해야 합니다. Generic.xaml의 기본 내용을 삭제하고, 아래 태그를 붙여넣습니다.

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

이 경우 기본 스타일이 설정하는 속성은 컨트롤 템플릿뿐입니다. 템플릿은 사각형(해당 배경이 XAML **Control** 형식의 모든 인스턴스에 있는 **Background** 속성에 바인딩됨) 및 텍스트 요소(해당 텍스트가 **BgLabelControl::Label** 종속성 속성에 바인딩됨)로 구성됩니다.

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>주 UI 페이지에 BgLabelControl 인스턴스 추가

`MainPage.xaml`을 엽니다. 여기에는 기본 UI 페이지에 사용할 XAML 태그가 포함되어 있습니다. **StackPanel** 내부에서 **Button** 요소 바로 뒤에 다음 태그를 추가합니다.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

또한 **MainPage** 형식(컴파일 XAML 태그 및 명령형 코드의 조합)에서 **BgLabelControl** 템플릿 기반 컨트롤 형식을 인식할 수 있도록 다음 include 지시문을 `MainPage.h`에 추가합니다. 다른 XAML 페이지에서 **BgLabelControl** 을 사용하려면 해당 페이지의 헤더 파일에 동일한 include 지시문을 추가합니다. 또는 미리 컴파일된 헤더 파일에 단일 include 지시문을 넣으면 됩니다.

```cppwinrt
//MainPage.h
...
#include "BgLabelControl.h"
...
```

이제 프로젝트를 빌드하고 실행합니다. 기본 컨트롤 템플릿이 배경 브러시에 바인딩되고 태그의 **BgLabelControl** 인스턴스 레이블에 바인딩되는 것을 알 수 있습니다.

![템플릿 기반 컨트롤 결과](images/winui-templated-control-result.png)

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>재정의 가능 함수(예: **MeasureOverride** 및 **OnApplyTemplate)** 구현

**Control** 런타임 클래스에서 템플릿 기반 컨트롤을 파생시킵니다. 이 클래스 자체는 기본 런타임 클래스에서 추가로 파생됩니다. 그리고 파생 클래스에서 재정의할 수 있는 **Control**, **FrameworkElement** 및 **UIElement** 의 재정의 가능 메서드가 있습니다. 작업을 수행하는 방법을 보여 주는 코드 예제는 다음과 같습니다.

```cppwinrt
// Control overrides.
void OnPointerPressed(Microsoft::UI::Xaml::Input::PointerRoutedEventArgs const& /* e */) const { ... };

// FrameworkElement overrides.
Windows::Foundation::Size MeasureOverride(Windows::Foundation::Size const& /* availableSize */) const { ... };
void OnApplyTemplate() const { ... };

// UIElement overrides.
Microsoft::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer() const { ... };
```

‘재정의 가능’ 함수는 다른 언어 프로젝션에서는 다르게 표현됩니다. 예를 들어 C#에서 재정의 가능 함수는 일반적으로 보호된 가상 함수로 표시됩니다. C++/WinRT에서는 가상도 아니고 보호되지도 않지만 위에 표시된 것처럼 고유한 구현을 함수를 재정의하고 고유한 구현을 제공할 수 있습니다.



## <a name="generating-the-control-source-files-without-using-a-template"></a>템플릿을 사용하지 않고 컨트롤 소스 파일 생성

이 섹션에서는 **사용자 지정 컨트롤** 항목 템플릿을 사용하지 않고 사용자 지정 컨트롤을 만드는 데 필요한 소스 파일을 생성하는 방법을 보여 줍니다. 

먼저 새 Midl 파일(.idl) 항목을 프로젝트에 추가합니다. **프로젝트** 메뉴에서 **새 항목 추가...** 를 선택하고, 검색 상자에서 "MIDL"을 입력하여 .idl 파일 항목을 찾습니다. 이름이 이 문서의 단계와 일치하도록 새 파일 이름을 `BgLabelControl.idl`로 지정합니다. `BgLabelControl.idl`의 기본 콘텐츠를 삭제하고, 위의 단계에 표시된 런타임 클래스 선언에 붙여넣습니다.


새 .idl 파일이 저장되면 다음 단계는 템플릿 기반 컨트롤을 구현하는 데 사용할 .cpp 및 .h 구현 파일에 대한 Windows 런타임 메타데이터 파일(.winmd) 및 스텁을 생성하는 것입니다. 이러한 파일은 솔루션을 빌드하여 생성합니다. 그러면 MIDL 컴파일러(midl.exe)에서 사용자가 만든 .idl 파일을 컴파일합니다. 솔루션이 성공적으로 빌드되지 않으면 Visual Studio에서 출력 창에 빌드 오류를 표시하지만 필요한 파일은 생성됩니다.

BgLabelControl.h 및 BgLabelControl.cpp 스텁 파일을 \BgLabelControlApp\BgLabelControlApp\Generated Files\sources\에서 프로젝트 폴더로 복사합니다. **솔루션 탐색기** 에서 [모든 파일 표시]가 설정되어 있는지 확인합니다. 복사한 스텁 파일을 마우스 오른쪽 단추로 클릭하고 **프로젝트에 포함** 을 클릭합니다.

컴파일러는 생성된 파일이 컴파일되지 않도록 static_assert 줄을 BgLabelControl.h 및 BgLabelControl.cpp의 위쪽에 배치합니다. 컨트롤을 구현하는 경우 프로젝트 디렉터리에 배치한 파일에서 이러한 줄을 제거해야 합니다. 이 연습에서는 파일의 전체 내용을 위에 제공된 코드로 덮어쓰면 됩니다.
