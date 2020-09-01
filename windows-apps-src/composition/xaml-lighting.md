---
title: XAML 조명
description: 광원 개체는 SceneLightingEffect와 함께 사용 되어 동적 조명 및 반사를 시뮬레이션 합니다.
ms.date: 06/28/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppcx
- cppwinrt
ms.openlocfilehash: 315427d2f9212cb9ecfec01e71ed9373deb5cfba
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163547"
---
# <a name="xaml-lighting"></a>XAML 조명

[**CompositionLight**](/uwp/api/Windows.UI.Composition.CompositionLight) 개체는 [**SceneLightingEffect**](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) 와 함께 동적 조명 및 반사를 시뮬레이션 하는 데 사용 됩니다.

[**시각적 개체**](/uwp/api/Windows.UI.Composition.Visual) 및 XAML [**UIElements**](/uwp/api/Windows.UI.Xaml.UIElement)에 조명을 적용할 수 있습니다.

## <a name="applying-lights-to-xaml-uielements"></a>XAML에 광원 적용 UIElements

[**Xamllight**](/uwp/api/windows.ui.xaml.media.xamllight) 개체는 동적으로 가벼운 XAML UIElements에 [**CompositionLights**](/uwp/api/Windows.UI.Composition.CompositionLight) 를 적용 하는 데 사용 됩니다. XamlLight는 UIElements 또는 XAML 브러시를 대상으로 하 고, UIElements 트리에 조명을 적용 하 고, 현재 사용 중인지 여부에 따라 CompositionLight 리소스의 수명을 관리 하는 데 사용할 수 있는 메서드를 제공 합니다.

- XamlLight를 사용 하는 **브러시** 를 대상으로 하는 경우 해당 브러시를 사용 하는 모든 UIElements 부분이 조명에서 켜 집니다.
- XamlLight를 사용 하 여 **uielement** 를 대상으로 하는 경우 전체 UIElement 및 자식 UIElements가 모두 빛에 의해 켜 집니다.

## <a name="creating-and-using-a-xamllight"></a>XamlLight 만들기 및 사용

[**Xamllight**](/uwp/api/windows.ui.xaml.media.xamllight) 는 사용자 지정 조명을 만드는 데 사용할 수 있는 기본 클래스입니다.

이 예제에서는 대상 UIElements 및 브러시에 다중 색 스포트라이트를 적용 하는 사용자 지정 XamlLight에 대 한 정의를 보여 줍니다.

```csharp
public sealed class OrangeSpotLight : XamlLight
{
    // Register an attached property that lets you set a UIElement
    // or Brush as a target for this light type in markup.
    public static readonly DependencyProperty IsTargetProperty =
        DependencyProperty.RegisterAttached(
        "IsTarget",
        typeof(bool),
        typeof(OrangeSpotLight),
        new PropertyMetadata(null, OnIsTargetChanged)
    );

    public static void SetIsTarget(DependencyObject target, bool value)
    {
        target.SetValue(IsTargetProperty, value);
    }

    public static Boolean GetIsTarget(DependencyObject target)
    {
        return (bool)target.GetValue(IsTargetProperty);
    }

    // Handle attached property changed to automatically target and untarget UIElements and Brushes.
    private static void OnIsTargetChanged(DependencyObject obj, DependencyPropertyChangedEventArgs e)
    {
        var isAdding = (bool)e.NewValue;

        if (isAdding)
        {
            if (obj is UIElement)
            {
                XamlLight.AddTargetElement(GetIdStatic(), obj as UIElement);
            }
            else if (obj is Brush)
            {
                XamlLight.AddTargetBrush(GetIdStatic(), obj as Brush);
            }
        }
        else
        {
            if (obj is UIElement)
            {
                XamlLight.RemoveTargetElement(GetIdStatic(), obj as UIElement);
            }
            else if (obj is Brush)
            {
                XamlLight.RemoveTargetBrush(GetIdStatic(), obj as Brush);
            }
        }
    }

    protected override void OnConnected(UIElement newElement)
    {
        if (CompositionLight == null)
        {
            // OnConnected is called when the first target UIElement is shown on the screen.
            // This lets you delay creation of the composition object until it's actually needed.
            var spotLight = Window.Current.Compositor.CreateSpotLight();
            spotLight.InnerConeColor = Colors.Orange;
            spotLight.OuterConeColor = Colors.Yellow;
            spotLight.InnerConeAngleInDegrees = 30;
            spotLight.OuterConeAngleInDegrees = 45;
            CompositionLight = spotLight;
        }
    }

    protected override void OnDisconnected(UIElement oldElement)
    {
        // OnDisconnected is called when there are no more target UIElements on the screen.
        // The CompositionLight should be disposed when no longer required.
        // For SDK 15063, see Remarks in the XamlLight class documentation.
        if (CompositionLight != null)
        {
            CompositionLight.Dispose();
            CompositionLight = null;
        }
    }

    protected override string GetId()
    {
        return GetIdStatic();
    }

    private static string GetIdStatic()
    {
        // This specifies the unique name of the light.
        // In most cases you should use the type's FullName.
        return typeof(OrangeSpotLight).FullName;
    }
}
```

```vb
Public NotInheritable Class OrangeSpotLight
    Inherits XamlLight

    ' Register an attached property that lets you set a UIElement
    ' or Brush as a target for this light type in markup.
    Public Shared ReadOnly IsTargetProperty As DependencyProperty = DependencyProperty.RegisterAttached(
            "IsTarget",
            GetType(Boolean),
            GetType(OrangeSpotLight),
            New PropertyMetadata(Nothing, New PropertyChangedCallback(AddressOf OnIsTargetChanged)
            )
        )

    Public Shared Sub SetIsTarget(target As DependencyObject, value As Boolean)
        target.SetValue(IsTargetProperty, value)
    End Sub

    Public Shared Function GetIsTarget(target As DependencyObject) As Boolean
        Return DirectCast(target.GetValue(IsTargetProperty), Boolean)
    End Function

    ' Handle attached property changed to automatically target And untarget UIElements And Brushes.
    Public Shared Sub OnIsTargetChanged(obj As DependencyObject, e As DependencyPropertyChangedEventArgs)
        Dim isAdding = DirectCast(e.NewValue, Boolean)

        If isAdding Then
            If TypeOf obj Is UIElement Then
                XamlLight.AddTargetElement(GetIdStatic(), TryCast(obj, UIElement))
            ElseIf TypeOf obj Is Brush Then
                XamlLight.AddTargetBrush(GetIdStatic(), TryCast(obj, Brush))
            End If
        Else
            If TypeOf obj Is UIElement Then
                XamlLight.RemoveTargetElement(GetIdStatic(), TryCast(obj, UIElement))
            ElseIf TypeOf obj Is Brush Then
                XamlLight.RemoveTargetBrush(GetIdStatic(), TryCast(obj, Brush))
            End If
        End If
    End Sub

    Protected Overrides Sub OnConnected(newElement As UIElement)
        If CompositionLight Is Nothing Then
            ' OnConnected Is called when the first target UIElement Is shown on the screen.
            ' This lets you delay creation of the composition object until it's actually needed.
            Dim spotLight = Window.Current.Compositor.CreateSpotLight()
            spotLight.InnerConeColor = Colors.Orange
            spotLight.OuterConeColor = Colors.Yellow
            spotLight.InnerConeAngleInDegrees = 30
            spotLight.OuterConeAngleInDegrees = 45
            CompositionLight = spotLight
        End If
    End Sub

    Protected Overrides Sub OnDisconnected(oldElement As UIElement)
        ' OnDisconnected Is called when there are no more target UIElements on the screen.
        ' The CompositionLight should be disposed when no longer required.
        If CompositionLight IsNot Nothing Then
            CompositionLight.Dispose()
            CompositionLight = Nothing
        End If
    End Sub

    Protected Overrides Function GetId() As String
        Return GetIdStatic()
    End Function

    Private Shared Function GetIdStatic() As String
        ' This specifies the unique name of the light.
        ' In most cases you should use the type's FullName.
        Return GetType(OrangeSpotLight).FullName
    End Function
End Class
```

```cppwinrt
// For the C++/WinRT code example below, you'll need to add a Midl File (.idl) file to your project.

// OrangeSpotLight.idl
namespace MyApp
{
    [default_interface]
    runtimeclass OrangeSpotLight : Windows.UI.Xaml.Media.XamlLight
    {
        OrangeSpotLight();
        static Windows.UI.Xaml.DependencyProperty IsTargetProperty{ get; };
        static Boolean GetIsTarget(Windows.UI.Xaml.DependencyObject target);
        static void SetIsTarget(Windows.UI.Xaml.DependencyObject target, Boolean value);
    }
}

// OrangeSpotLight.h
struct OrangeSpotLight : OrangeSpotLightT<OrangeSpotLight>
{
    OrangeSpotLight() = default;

    winrt::hstring GetId();

    static Windows::UI::Xaml::DependencyProperty IsTargetProperty() { return m_isTargetProperty; }

    static bool GetIsTarget(Windows::UI::Xaml::DependencyObject const& target)
    {
        return winrt::unbox_value<bool>(target.GetValue(m_isTargetProperty));
    }

    static void SetIsTarget(Windows::UI::Xaml::DependencyObject const& target, bool value)
    {
        target.SetValue(m_isTargetProperty, winrt::box_value(value));
    }

    void OnConnected(Windows::UI::Xaml::UIElement const& newElement);
    void OnDisconnected(Windows::UI::Xaml::UIElement const& oldElement);

    static void OnIsTargetChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e);

    inline static winrt::hstring GetIdStatic()
    {
        // This specifies the unique name of the light. In most cases you should use the type's full name.
        return winrt::xaml_typename<MyApp::OrangeSpotLight>().Name;
    }

private:
    static Windows::UI::Xaml::DependencyProperty m_isTargetProperty;
};

// OrangeSpotLight.cpp
Windows::UI::Xaml::DependencyProperty OrangeSpotLight::m_isTargetProperty =
    Windows::UI::Xaml::DependencyProperty::RegisterAttached(
        L"IsTarget",
        winrt::xaml_typename<bool>(),
        winrt::xaml_typename<MyApp::OrangeSpotLight>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(false), Windows::UI::Xaml::PropertyChangedCallback{ &OrangeSpotLight::OnIsTargetChanged } }
);

void OrangeSpotLight::OnConnected(Windows::UI::Xaml::UIElement const& /* newElement */)
{
    if (!CompositionLight())
    {
        // OnConnected is called when the first target UIElement is shown on the screen. This enables delaying composition object creation until it's actually necessary.
        auto spotLight{ Windows::UI::Xaml::Window::Current().Compositor().CreateSpotLight() };
        spotLight.InnerConeColor(Windows::UI::Colors::Orange());
        spotLight.OuterConeColor(Windows::UI::Colors::Yellow());
        spotLight.InnerConeAngleInDegrees(30);
        spotLight.OuterConeAngleInDegrees(45);
        CompositionLight(spotLight);
    }
}

void OrangeSpotLight::OnDisconnected(Windows::UI::Xaml::UIElement const& /* oldElement */)
{
    // OnDisconnected is called when there are no more target UIElements on the screen.
    // Dispose of composition resources when no longer in use.
    if (CompositionLight())
    {
        CompositionLight(nullptr);
    }
}

winrt::hstring OrangeSpotLight::GetId()
{
    return OrangeSpotLight::GetIdStatic();
}

void OrangeSpotLight::OnIsTargetChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    auto uie{ d.try_as<Windows::UI::Xaml::UIElement>() };
    auto brush{ d.try_as<Windows::UI::Xaml::Media::Brush>() };

    auto isAdding = winrt::unbox_value<bool>(e.NewValue());
    if (isAdding)
    {

        if (uie)
        {
            Windows::UI::Xaml::Media::XamlLight::AddTargetElement(OrangeSpotLight::GetIdStatic(), uie);
        }
        else if (brush)
        {
            Windows::UI::Xaml::Media::XamlLight::AddTargetBrush(OrangeSpotLight::GetIdStatic(), brush);
        }
    }
    else
    {
        if (uie)
        {
            Windows::UI::Xaml::Media::XamlLight::RemoveTargetElement(OrangeSpotLight::GetIdStatic(), uie);
        }
        else if (brush)
        {
            Windows::UI::Xaml::Media::XamlLight::RemoveTargetBrush(OrangeSpotLight::GetIdStatic(), brush);
        }
    }
}

// MainPage.h
...
#include "OrangeSpotLight.h"
...
struct MainPage : MainPageT<MainPage>
{
    MainPage()
    {
        InitializeComponent();

        OrangeSpotLight::SetIsTarget(spotlitBrush(), true);
        OrangeSpotLight::SetIsTarget(spotlitUIElement(), true);
    }
...
};
```

```cppcx
// OrangeSpotLight.h:
public ref class OrangeSpotLight sealed :
    public Windows::UI::Xaml::Media::XamlLight
{
public:
    OrangeSpotLight();

    static property Windows::UI::Xaml::DependencyProperty^ IsTargetProperty
    {
        Windows::UI::Xaml::DependencyProperty^ get() { return m_isTargetProperty; }
    };
    static void SetIsTarget(Windows::UI::Xaml::DependencyObject^ target, bool value);
    static bool GetIsTarget(Windows::UI::Xaml::DependencyObject^ target);

protected:
    virtual void OnConnected(Windows::UI::Xaml::UIElement^ newElement) override;
    virtual void OnDisconnected(Windows::UI::Xaml::UIElement^ oldElement) override;
    virtual Platform::String^ GetId() override;

private:
    static Windows::UI::Xaml::DependencyProperty^ m_isTargetProperty;
    static void OnIsTargetChanged(Windows::UI::Xaml::DependencyObject^ obj, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e);

    inline static Platform::String^ GetIdStatic()
    {
        // This specifies the unique name of the light. In most cases you should use the type's FullName.
        return OrangeSpotLight::typeid->FullName;
    }
};

//OrangeSpotLight.cpp:

// Register an attached property that lets you set a UIElement
// or Brush as a target for this light type in markup.
DependencyProperty^ OrangeSpotLight::m_isTargetProperty = DependencyProperty::RegisterAttached(
    "IsTarget",
    bool::typeid,
    OrangeSpotLight::typeid,
    ref new PropertyMetadata(0.0, ref new PropertyChangedCallback(OnIsTargetChanged))
);

OrangeSpotLight::OrangeSpotLight()
{
}

void OrangeSpotLight::SetIsTarget(DependencyObject^ target, bool value)
{
    target->SetValue(IsTargetProperty, value);
}

bool OrangeSpotLight::GetIsTarget(DependencyObject^ target)
{
    return static_cast<bool>(target->GetValue(IsTargetProperty));
}

// Handle attached property changed to automatically target and untarget UIElements and Brushes.
void OrangeSpotLight::OnIsTargetChanged(DependencyObject^ obj, DependencyPropertyChangedEventArgs^ e)
{
    auto isAdding = static_cast<bool>(e->NewValue);

    if (isAdding)
    {
        if (dynamic_cast<UIElement^>(obj))
        {
            XamlLight::AddTargetElement(GetIdStatic(), static_cast<UIElement^>(obj));
        }
        else if (dynamic_cast<Brush^>(obj))
        {
            XamlLight::AddTargetBrush(GetIdStatic(), static_cast<Brush^>(obj));
        }
    }
    else
    {
        if (dynamic_cast<UIElement^>(obj))
        {
            XamlLight::RemoveTargetElement(GetIdStatic(), static_cast<UIElement^>(obj));
        }
        else if (dynamic_cast<Brush^>(obj))
        {
            XamlLight::RemoveTargetBrush(GetIdStatic(), static_cast<Brush^>(obj));
        }
    }
}
void OrangeSpotLight::OnConnected(UIElement^ newElement)
{
    if (CompositionLight == nullptr)
    {
        // OnConnected is called when the first target UIElement is shown on the screen.
        // This lets you delay creation of the composition object until it's actually needed.
        auto spotLight = Window::Current->Compositor->CreateSpotLight();
        spotLight->InnerConeColor = Colors::Orange;
        spotLight->OuterConeColor = Colors::Yellow;
        spotLight->InnerConeAngleInDegrees = 30;
        spotLight->OuterConeAngleInDegrees = 45;
        CompositionLight = spotLight;
    }
}

void OrangeSpotLight::OnDisconnected(UIElement^ oldElement)
{
    // OnDisconnected is called when there are no more target UIElements on the screen.
    // The CompositionLight should be disposed when no longer required.
    // For SDK 15063, see Remarks in the XamlLight class documentation.
    if (CompositionLight != nullptr)
    {
        delete CompositionLight;
        CompositionLight = nullptr;
    }
}

Platform::String^ OrangeSpotLight::GetId()
{
    return GetIdStatic();
}
```

그런 다음이 조명을 XAML UIElement 나 브러시에 적용 하 여 조명을 확인할 수 있습니다. 이 예제에서는 다른 잠재적 사용을 보여 줍니다.

> [!Important]
> [C + +/Winrt](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)의 경우 아래 태그에서 두 개의 항목을 제거 `local:OrangeSpotLight.IsTarget="True"` 합니다. 연결 된 속성은 이미 코드 숨김으로 설정 되어 있습니다.

```xaml
<StackPanel Width="100">
    <StackPanel.Lights>
        <local:OrangeSpotLight/>
    </StackPanel.Lights>

    <!-- This border is lit by the OrangeSpotLight, but its content is not. -->
    <Border BorderThickness="4" Margin="2">
        <Border.BorderBrush>
            <SolidColorBrush x:Name="spotlitBrush" Color="White" local:OrangeSpotLight.IsTarget="True"/>
        </Border.BorderBrush>
        <Rectangle Fill="LightGray" Height="20"/>
    </Border>

    <!-- This border and its content are lit by the OrangeSpotLight. -->
    <Border x:Name="spotlitUIElement" BorderThickness="4" BorderBrush="PaleGreen" Margin="2"
            local:OrangeSpotLight.IsTarget="True">
        <Rectangle Fill="LightGray" Height="20"/>
    </Border>

    <!-- This border and its content are not lit by the OrangeSpotLight. -->
    <Border BorderThickness="4" BorderBrush="PaleGreen" Margin="2">
        <Rectangle Fill="LightGray" Height="20"/>
    </Border>
</StackPanel>
```

이 XAML의 결과는 다음과 같습니다.

![Xaml 빛에 의해 lit 요소 예](images/orange-spot-light.png)

> [!Important]
> 위 예제에 표시 된 것 처럼 태그에서 UIElement를 설정 하는 것은 최소 버전이 Windows 10 크리에이터 스 업데이트 이상과 같은 앱에 대해서만 지원 됩니다. 이전 버전을 대상으로 하는 앱의 경우 코드 숨김으로 조명을 만들어야 합니다.

## <a name="additional-resources"></a>추가 리소스

* [Windowsuidevlabs GitHub](https://github.com/microsoft/WindowsCompositionSamples)의 고급 UI 및 컴퍼지션 샘플.