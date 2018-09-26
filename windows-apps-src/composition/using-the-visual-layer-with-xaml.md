---
author: jaster
ms.assetid: b7a4ac8a-d91e-461b-a060-cc6fcea8e778
title: XAML로 시각적 계층 사용
description: 시각적 계층 API를 기존 XAML 콘텐츠와 함께 사용하여 고급 애니메이션 및 효과를 만드는 기술을 알아봅니다.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d45881ace6be3b0af88f14692837e96ab9b58d18
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/26/2018
ms.locfileid: "4211443"
---
# <a name="using-the-visual-layer-with-xaml"></a>시각적 계층을 XAML과 함께 사용

시각적 계층 기능을 사용하는 대부분의 앱은 XAML을 사용하여 기본 UI 콘텐츠를 정의합니다. Windows 10 1주년 업데이트에는 XAML 프레임워크 및 시각적 계층에 대한 새로운 기능이 있으며 이 두 가지 기술을 쉽게 결합하여 매력적인 사용자 환경을 만들 수 있습니다.
XAML 및 시각적 계층 interop 기능은 XAML API를 단독으로 사용하여 구현할 수 없는 고급 애니메이션 및 효과를 만드는 데 사용할 수 있습니다. 여기에는 다음이 포함됩니다.

- 흐리고 불투명한 유리 효과 같은 브러시 효과
- 동적 조명 효과
- 스크롤 기반 애니메이션 및 시차
- 자동 레이아웃 애니메이션
- 정확한 픽셀 그림자

이러한 효과와 애니메이션은 기존 XAML 콘텐츠에 적용할 수 있으므로 새로운 기능을 활용하기 위해 XAML 앱 구조를 크게 변경하지 않아도 됩니다.
레이아웃 애니메이션, 그림자 및 흐림 효과는 아래 레시피 섹션에서 설명합니다. 시차를 구현하는 코드 샘플에 대한 자세한 내용은 [ParallaxingListItems 샘플](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems)을 참조하세요. [WindowsUIDevLabs 리포지토리](https://github.com/Microsoft/WindowsUIDevLabs)에 애니메이션, 그림자 및 효과를 구현하기 위한 다른 몇 가지 샘플도 있습니다.

## <a name="the-xamlcompositionbrushbase-class"></a>XamlCompositionBrushBase 클래스

**XamlCompositionBrush**는 **CompositionBrush**로 영역을 칠하는 XAML 브러시에 대한 기본 클래스를 제공합니다. 이는 쉽게 흐리고 불투명한 유리 같은 구성 효과를 XAML UI 요소에 적용하는 데 사용할 수 있습니다.

XAML UI와 브러시 사용에 대한 자세한 내용은 [**브러시**](/windows/uwp/design/style/brushes#xamlcompositionbrushbase) 섹션을 참조하세요.

코드 샘플은 [**XamlCompositionBrushBase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)에 대한 참조 페이지를 참조하세요.

## <a name="the-xamllight-class"></a>XamlLight 클래스

**XamlLight**는 **CompositionLight**로 동적으로 영역을 밝히는 XAML 조명 효과에 대한 기본 클래스를 제공합니다.

XAML UI 요소 조명을 포함한 조명 사용에 대한 자세한 내용은 [**조명을**](xaml-lighting.md) 섹션을 참조하세요.

코드 샘플은 [**XamlLight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamllight)에 대한 참조 페이지를 참조하세요.

## <a name="the-elementcompositionpreview-class"></a>ElementCompositionPreview 클래스

[**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.aspx)는 XAML 및 시각적 계층 interop 기능을 제공하는 정적 클래스입니다. 시각적 계층 및 해당 기능의 개요는 [시각적 계층](https://msdn.microsoft.com/windows/uwp/graphics/visual-layer)을 참조하세요. **ElementCompositionPreview** 클래스는 다음 메서드를 제공합니다.

-   [**GetElementVisual**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): 이 요소를 렌더링하는 데 사용할 "핸드아웃" 시각적 개체를 가져옵니다.
-   [**SetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual.aspx): 이 요소의 시각적 트리의 마지막 자식으로 "핸드인" 시각적 개체를 설정합니다. 이 시각적 개체는 나머지 요소의 맨 위에 표시됩니다. 
-   [**GetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): **SetElementChildVisual**을 사용하여 시각적 개체 설정을 검색합니다.
-   [**GetScrollViewerManipulationPropertySet**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): **ScrollViewer**에서 스크롤 오프셋 기반 60fps 애니메이션을 만드는 데 사용할 수 있는 개체를 가져옵니다.

## <a name="remarks-on-elementcompositionpreviewgetelementvisual"></a>ElementCompositionPreview.GetElementVisual에 대한 설명

**ElementCompositionPreview.GetElementVisual**은 지정된 **UIElement**를 렌더링하는 데 사용할 "핸드아웃" 시각적 개체를 반환합니다. **Visual.Opacity**, **Visual.Offset** 및 **Visual.Size**와 같은 속성은 UIElement의 상태에 따라 XAML 프레임워크에 의해 설정됩니다. 이렇게 하면 암시적 위치 변경 애니메이션 등의 기술을 사용할 수 있습니다(*레시피* 참조).

**Offset** 및 **Size**는 XAML 프레임워크 레이아웃의 결과로 설정되므로 개발자는 이러한 속성에 애니메이션을 적용하거나 수정할 때 주의해야 합니다. 개발자는 요소의 왼쪽 위 모서리에 레이아웃의 부모와 동일한 위치가 있는 경우에만 Offset에 애니메이션 효과를 주거나 수정해야 합니다. Size는 일반적으로 수정할 수 없지만 속성에 액세스하는 것은 유용할 수 있습니다. 예를 들어 아래의 그림자 및 불투명한 유리 샘플은 애니메이션에 대한 입력으로 핸드아웃 시각적 개체 크기를 사용합니다.

그 외 주의할 점은 업데이트된 핸드아웃 시각적 개체 속성은 해당 UIElement에 반영되지 않습니다. 예를 들어 **UIElement.Opacity**를 0.5로 설정하면 해당 핸드아웃 시각적 개체의 Opacity가 0.5로 설정됩니다. 그러나 핸드아웃 시각적 개체의 **Opacity**를 0.5로 설정하면 콘텐츠의 불투명도가 50%로 표시되지만 해당 UIElement의 Opacity 속성 값이 변경되지는 않습니다.

### <a name="example-of-offset-animation"></a>**Offset** 애니메이션 예

#### <a name="incorrect"></a>틀림

```xaml
<Border>
      <Image x:Name="MyImage" Margin="5" />
</Border>
```

```csharp
// Doesn’t work because Image has a margin!
ElementCompositionPreview.GetElementVisual(MyImage).StartAnimation("Offset", parallaxAnimation);
```

#### <a name="correct"></a>맞음

```xaml
<Border>
    <Canvas Margin="5">
        <Image x:Name="MyImage" />
    </Canvas>
</Border>
```

```csharp
// This works because the Canvas parent doesn’t generate a layout offset.
ElementCompositionPreview.GetElementVisual(MyImage).StartAnimation("Offset", parallaxAnimation);
```

## <a name="the-elementcompositionpreviewsetelementchildvisual-method"></a>**ElementCompositionPreview.SetElementChildVisual** 메서드

**ElementCompositionPreview.SetElementChildVisual**을 통해 개발자가 요소의 시각적 트리의 일부로 나타나는 "핸드인" 시각적 개체를 제공할 수 있습니다. 이렇게 하면 개발자가 XAML UI 내에 시각적 기반 콘텐츠를 표시할 수 있는 "컴퍼지션 섬"을 만들 수 있습니다. 시각적 기반 콘텐츠는 XAML 콘텐츠의 동일한 접근성과 사용자 환경을 보장하지 않으므로 개발자는 이 기술을 사용하는 것에 대해 보수적이어야 합니다. 따라서 일반적으로 이 기술은 아래의 레시피 섹션에 설명된 것처럼 사용자 지정 효과를 구현하는 데 있어 필요한 경우에만 사용하는 것이 좋습니다.

## <a name="getalphamask-methods"></a>**GetAlphaMask** 메서드

[**Image**](https://msdn.microsoft.com/library/windows/apps/br242752), [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 및 [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)은 회색조 이미지로 요소의 모양을 표시하는 **CompositionBrush**를 반환하는 **GetAlphaMask**라는 메서드를 구현합니다. 이 **CompositionBrush**는 컴퍼지션 **DropShadow**에 대한 입력으로 사용할 수 있으므로 그림자는 사각형 대신 요소의 모양을 반영할 수 있습니다. 이렇게 하면 텍스트, 알파 이미지 및 모양에 대한 정확한 픽셀, 등고선 기반 그림자를 사용할 수 있습니다. 이 API의 예를 보려면 아래의 *그림자*를 참조하세요.

## <a name="recipes"></a>레시피

### <a name="reposition-animation"></a>위치 변경 애니메이션

개발자는 컴퍼지션 암시적 애니메이션을 사용하여 부모 관련 요소의 레이아웃 변경 사항에 자동으로 애니메이션 효과를 줄 수 있습니다. 예를 들어 아래 단추의 **Margin**이 변경된 경우 새 레이아웃 위치에 자동으로 애니메이션 효과를 줍니다.

#### <a name="implementation-overview"></a>구현 개요

1. 대상 요소에 대한 핸드아웃 **시각적 개체**를 가져옵니다.
1. **Offset** 속성 변경 사항에 자동으로 애니메이션 효과를 주는 **ImplicitAnimationCollection**을 만듭니다.
1. 백업 시각적 개체와 **ImplicitAnimationCollection**을 연결합니다.

```xaml
<Button x:Name="RepositionTarget" Content="Click Me" />
```

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeRepositionAnimation(RepositionTarget);
}

private void InitializeRepositionAnimation(UIElement repositionTarget)
{
    var targetVisual = ElementCompositionPreview.GetElementVisual(repositionTarget);
    Compositor compositor = targetVisual.Compositor;

    // Create an animation to animate targetVisual's Offset property to its final value
    var repositionAnimation = compositor.CreateVector3KeyFrameAnimation();
    repositionAnimation.Duration = TimeSpan.FromSeconds(0.66);
    repositionAnimation.Target = "Offset";
    repositionAnimation.InsertExpressionKeyFrame(1.0f, "this.FinalValue");

    // Run this animation when the Offset Property is changed
    var repositionAnimations = compositor.CreateImplicitAnimationCollection();
    repositionAnimations["Offset"] = repositionAnimation;

    targetVisual.ImplicitAnimations = repositionAnimations;
}
```

### <a name="drop-shadow"></a>그림자

**UIElement**에 정확한 픽셀 그림자를 적용합니다(예: 사진을 포함하는 **타원**). 그림자에는 앱에서 만든 **SpriteVisual**이 필요하므로 **ElementCompositionPreview.SetElementChildVisual**을 사용하여 **SpriteVisual**을 포함하는 "호스트" 요소를 만들어야 합니다.

#### <a name="implementation-overview"></a>구현 개요

1. 호스트 요소에 대한 핸드아웃 **시각적 개체**를 가져옵니다.
2. Windows.UI.Composition **DropShadow**를 만듭니다.
3. 마스크를 통해 대상 요소에서 모양을 가져오도록 **DropShadow**를 구성합니다.
    - **DropShadow**는 기본적으로 사각형이므로 대상이 사각형인 경우에는 구성할 필요가 없습니다.
4. 새 **SpriteVisual**에 그림자를 첨부하고 **SpriteVisual**을 호스트 요소의 자식으로 설정합니다.
5. **ExpressionAnimation**을 사용하여 호스트 크기에 **SpriteVisual** 크기를 바인딩합니다.

```xaml
<Grid Width="200" Height="200">
    <Canvas x:Name="ShadowHost" />
    <Ellipse x:Name="CircleImage">
        <Ellipse.Fill>
            <ImageBrush ImageSource="Assets/Images/2.jpg" Stretch="UniformToFill" />
        </Ellipse.Fill>
    </Ellipse>
</Grid>
```

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeDropShadow(ShadowHost, CircleImage);
}

private void InitializeDropShadow(UIElement shadowHost, Shape shadowTarget)
{
    Visual hostVisual = ElementCompositionPreview.GetElementVisual(shadowHost);
    Compositor compositor = hostVisual.Compositor;

    // Create a drop shadow
    var dropShadow = compositor.CreateDropShadow();
    dropShadow.Color = Color.FromArgb(255, 75, 75, 80);
    dropShadow.BlurRadius = 15.0f;
    dropShadow.Offset = new Vector3(2.5f, 2.5f, 0.0f);
    // Associate the shape of the shadow with the shape of the target element
    dropShadow.Mask = shadowTarget.GetAlphaMask();

    // Create a Visual to hold the shadow
    var shadowVisual = compositor.CreateSpriteVisual();
    shadowVisual.Shadow = dropShadow;

    // Add the shadow as a child of the host in the visual tree
   ElementCompositionPreview.SetElementChildVisual(shadowHost, shadowVisual);

    // Make sure size of shadow host and shadow visual always stay in sync
    var bindSizeAnimation = compositor.CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation.SetReferenceParameter("hostVisual", hostVisual);

    shadowVisual.StartAnimation("Size", bindSizeAnimation);
}
```

다음 두 가지 목록은 동일한 XAML 구조를 사용하는 이전 C# 코드의 [C++/WinRT](https://aka.ms/cppwinrt) 및 [C++/CX](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx) 등가를 표시합니다.

```cppwinrt
#include <winrt/Windows.UI.Composition.h>
#include <winrt/Windows.UI.Xaml.h>
#include <winrt/Windows.UI.Xaml.Hosting.h>
#include <winrt/Windows.UI.Xaml.Shapes.h>
...
MainPage()
{
    InitializeComponent();
    InitializeDropShadow(ShadowHost(), CircleImage());
}

int32_t MyProperty();
void MyProperty(int32_t value);

void InitializeDropShadow(Windows::UI::Xaml::UIElement const& shadowHost, Windows::UI::Xaml::Shapes::Shape const& shadowTarget)
{
    auto hostVisual{ Windows::UI::Xaml::Hosting::ElementCompositionPreview::GetElementVisual(shadowHost) };
    auto compositor{ hostVisual.Compositor() };

    // Create a drop shadow
    auto dropShadow{ compositor.CreateDropShadow() };
    dropShadow.Color(Windows::UI::ColorHelper::FromArgb(255, 75, 75, 80));
    dropShadow.BlurRadius(15.0f);
    dropShadow.Offset(Windows::Foundation::Numerics::float3{ 2.5f, 2.5f, 0.0f });
    // Associate the shape of the shadow with the shape of the target element
    dropShadow.Mask(shadowTarget.GetAlphaMask());

    // Create a Visual to hold the shadow
    auto shadowVisual = compositor.CreateSpriteVisual();
    shadowVisual.Shadow(dropShadow);

    // Add the shadow as a child of the host in the visual tree
    Windows::UI::Xaml::Hosting::ElementCompositionPreview::SetElementChildVisual(shadowHost, shadowVisual);

    // Make sure size of shadow host and shadow visual always stay in sync
    auto bindSizeAnimation{ compositor.CreateExpressionAnimation(L"hostVisual.Size") };
    bindSizeAnimation.SetReferenceParameter(L"hostVisual", hostVisual);

    shadowVisual.StartAnimation(L"Size", bindSizeAnimation);
}
```

```cpp
#include "WindowsNumerics.h"

MainPage::MainPage()
{
    InitializeComponent();
    InitializeDropShadow(ShadowHost, CircleImage);
}

void MainPage::InitializeDropShadow(Windows::UI::Xaml::UIElement^ shadowHost, Windows::UI::Xaml::Shapes::Shape^ shadowTarget)
{
    auto hostVisual = Windows::UI::Xaml::Hosting::ElementCompositionPreview::GetElementVisual(shadowHost);
    auto compositor = hostVisual->Compositor;

    // Create a drop shadow
    auto dropShadow = compositor->CreateDropShadow();
    dropShadow->Color = Windows::UI::ColorHelper::FromArgb(255, 75, 75, 80);
    dropShadow->BlurRadius = 15.0f;
    dropShadow->Offset = Windows::Foundation::Numerics::float3(2.5f, 2.5f, 0.0f);
    // Associate the shape of the shadow with the shape of the target element
    dropShadow->Mask = shadowTarget->GetAlphaMask();

    // Create a Visual to hold the shadow
    auto shadowVisual = compositor->CreateSpriteVisual();
    shadowVisual->Shadow = dropShadow;

    // Add the shadow as a child of the host in the visual tree
    Windows::UI::Xaml::Hosting::ElementCompositionPreview::SetElementChildVisual(shadowHost, shadowVisual);

    // Make sure size of shadow host and shadow visual always stay in sync
    auto bindSizeAnimation = compositor->CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation->SetReferenceParameter("hostVisual", hostVisual);

    shadowVisual->StartAnimation("Size", bindSizeAnimation);
}
```

### <a name="frosted-glass"></a>불투명한 유리

백그라운드 콘텐츠에 색조를 부여하고 흐리게 하는 효과를 만듭니다. 효과를 사용하려면 개발자는 Win2D NuGet 패키지를 설치해야 합니다. 설치 지침은 [Win2D 홈페이지](http://microsoft.github.io/Win2D/html/Introduction.htm)를 참조하세요.

#### <a name="implementation-overview"></a>구현 개요

1.  호스트 요소에 대한 핸드아웃 **시각적 개체**를 가져옵니다.
2.  Win2D 및 **CompositionEffectSourceParameter**를 사용하여 흐림 효과 트리를 만듭니다.
3.  효과 트리를 기반으로 **CompositionEffectBrush**를 만듭니다.
4.  **SpriteVisual** 뒤의 콘텐츠에 효과를 적용할 수 있도록 **CompositionEffectBrush**의 입력을 **CompositionBackdropBrush**로 설정합니다.
5.  **CompositionEffectBrush**를 새 **SpriteVisual**의 콘텐츠로 설정하고 **SpriteVisual**을 호스트 요소의 자식으로 설정합니다. 또는 XamlCompositionBrushBase를 사용할 수 있습니다.
6.  **ExpressionAnimation**을 사용하여 호스트 크기에 **SpriteVisual** 크기를 바인딩합니다.

```xaml
<Grid Width="300" Height="300" Grid.Column="1">
    <Image
        Source="Assets/Images/2.jpg"
        Width="200"
        Height="200" />
    <Canvas
        x:Name="GlassHost"
        Width="150"
        Height="300"
        HorizontalAlignment="Right" />
</Grid>
```

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeFrostedGlass(GlassHost);
}

private void InitializeFrostedGlass(UIElement glassHost)
{
    Visual hostVisual = ElementCompositionPreview.GetElementVisual(glassHost);
    Compositor compositor = hostVisual.Compositor;

    // Create a glass effect, requires Win2D NuGet package
    var glassEffect = new GaussianBlurEffect
    { 
        BlurAmount = 15.0f,
        BorderMode = EffectBorderMode.Hard,
        Source = new ArithmeticCompositeEffect
        {
            MultiplyAmount = 0,
            Source1Amount = 0.5f,
            Source2Amount = 0.5f,
            Source1 = new CompositionEffectSourceParameter("backdropBrush"),
            Source2 = new ColorSourceEffect
            {
                Color = Color.FromArgb(255, 245, 245, 245)
            }
        }
    };

    //  Create an instance of the effect and set its source to a CompositionBackdropBrush
    var effectFactory = compositor.CreateEffectFactory(glassEffect);
    var backdropBrush = compositor.CreateBackdropBrush();
    var effectBrush = effectFactory.CreateBrush();

    effectBrush.SetSourceParameter("backdropBrush", backdropBrush);

    // Create a Visual to contain the frosted glass effect
    var glassVisual = compositor.CreateSpriteVisual();
    glassVisual.Brush = effectBrush;

    // Add the blur as a child of the host in the visual tree
    ElementCompositionPreview.SetElementChildVisual(glassHost, glassVisual);

    // Make sure size of glass host and glass visual always stay in sync
    var bindSizeAnimation = compositor.CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation.SetReferenceParameter("hostVisual", hostVisual);

    glassVisual.StartAnimation("Size", bindSizeAnimation);
}
```

## <a name="additional-resources"></a>추가 리소스

- [시각적 계층 개요](https://msdn.microsoft.com/windows/uwp/composition/visual-layer)
- [**ElementCompositionPreview** 클래스](https://msdn.microsoft.com/library/windows/apps/mt608976)
- [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs)의 고급 UI 및 Composition 샘플
- [BasicXamlInterop 샘플](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/BasicXamlInterop)
- [ParallaxingListItems 샘플](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems)
