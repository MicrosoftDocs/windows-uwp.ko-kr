---
author: jebishop
ms.assetid: fb8ae71d-5c88-4c85-9257-a9607d5179b1
title: 조명
description: 조명 개체는 SceneLightingEffect와 함께 동적 조명 및 반사를 시뮬레이트하기 위해 사용됩니다.
ms.author: jimwalk
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 316b79fa9fc9a700d606f0881a89a299523330b1
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
ms.locfileid: "1673710"
---
# <a name="lighting"></a>조명

[**CompositionLight**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionLight) 개체는 [**SceneLightingEffect**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect)와 함께 동적 조명 및 반사를 시뮬레이트하기 위해 사용됩니다.

[**시각 효과**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 및 XAML [**UIElements**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)에 조명을 적용할 수 있습니다.

## <a name="applying-lights-to-xaml-uielements"></a>XAML UIElements에 조명 적용

[**XamlLight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamllight) 개체는 [**CompositionLights**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionLight)에 적용하여 동적으로 XAML UIElements를 조명하는 데 사용됩니다. XamlLight는 UIElements 표적화 또는 다른 XAML 브러시에 대한 메서드를 제공하며 조명을 UIElements 트리에 적용하고 현재 사용 여부에 기반하여 CompositionLight 리소스의 수명을 관리합니다.

* **브러시**를 XamlLight로 대상으로 지정하면 해당 브러시를 사용하는 모든 UIElements의 일부가 조명에 의해 켜집니다.
* XamlLight로 **UIElement**를 대상으로 지정하는 경우 전체 UIElement 및 하위 UIElements가 모두 조명에 의해 켜집니다.

## <a name="creating-and-using-a-xamllight"></a>XamlLight 만들기 및 사용

[**XamlLight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamllight)는 사용자 지정 조명을 생성하는 데 사용할 수 있는 기본 클래스입니다.

특정 요소를 대상으로 지정하기 위해 연결된 속성을 사용하는 사용자 지정 XamlLight의 예는 다음과 같습니다.

```csharp
public sealed class FancyOrangeSpotLight : XamlLight
{
    // Register an attached proeprty that enables apps to set a UIElement or Brush as a target for this light type in markup.
    public static readonly DependencyProperty IsTargetProperty =
        DependencyProperty.RegisterAttached(
        "IsTarget",
        typeof(Boolean),
        typeof(FancyOrangeSpotLight),
        new PropertyMetadata(null, OnIsTargetChanged)
    );
    public static void SetIsTarget(DependencyObject target, Boolean value)
    {
        target.SetValue(IsTargetProperty, value);
    }
    public static Boolean GetIsTarget(DependencyObject target)
    {
        return (Boolean) target.GetValue(IsTargetProperty);
    }

    // Handle attached property changed to automatically target and untarget UIElements and Brushes.
    private static void OnIsTargetChanged(DependencyObject obj,
                                            DependencyPropertyChangedEventArgs e)
    {
        var isAdding = (Boolean)e.NewValue;

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
        // OnConnected is called when the first target UIElement is shown on the screen. This enables delaying composition object creation until it's actually necessary.
        var spotLight = Window.Current.Compositor.CreateSpotLight();
        spotLight.InnerConeColor = Colors.Orange;
        spotLight.OuterConeColor = Colors.Yellow;
        spotLight.InnerConeAngleInDegrees = 30;
        spotLight.OuterConeAngleInDegrees = 45;
        CompositionLight = spotLight;
    }

    protected override void OnDisconnected(UIElement oldElement)
    {
        // OnDisconnected is called when there are no more target UIElements on the screen. The CompositionLight should be disposed when no longer required.
        CompositionLight.Dispose();
        CompositionLight = null;
    }

    protected override string GetId()
    {
        return GetIdStatic();
    }

    private static string GetIdStatic()
    {
        // This specifies the unique name of the light. In most cases you should use the type's FullName.
        return typeof(FancyPointerTrackerLight).FullName;
    }
}
```

또한 위에 정의된 사용자 지정 조명의 다른 가능한 사용을 보여주는 예는 다음과 같습니다.

```xml
<Page>
    <Page.Lights>
        <local:SimpleOrangeSpotLight />
    </Page.Lights>

    <StackPanel>
        <!-- this border will be lit by a FancyOrangeSpotLight, but not its children -->
        <Border BorderThickness="1">
            <Border.BorderBrush>
                <SolidColorBrush Color="Orange" local:FancyOrangeSpotLight.IsTarget="true" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>

        <!-- this border and its children will be lit by FancyOrangeSpotLight -->
        <Border BorderThickness="2" local:FancyOrangeSpotLight.IsTarget="true">
            <Border.BorderBrush>
                <SolidColorBrush Color="Purple" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>

        <!-- this border will not be lit -->
        <Border BorderThickness="2">
            <Border.BorderBrush>
                <SolidColorBrush Color="Green" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>
    </StackPanel>
<Page>
```

> [!Important]
> 위 예에서 마크업에서 UIElement.Lights 설정은 최소 Windows 10 크리에이터 업데이트 이상에 해당하는 버전의 앱에서만 지원됩니다. 이전 버전을 대상으로 하는 앱의 경우 코드 숨김에서 조명을 만들어야 합니다.

## <a name="additional-resources"></a>추가 리소스

* [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs)의 고급 UI 및 Composition 샘플.
