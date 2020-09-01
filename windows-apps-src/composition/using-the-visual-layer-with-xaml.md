---
ms.assetid: b7a4ac8a-d91e-461b-a060-cc6fcea8e778
title: XAML과 함께 시각적 계층 사용
description: 시각적 계층 API를 기존 XAML 콘텐츠와 함께 사용 하 여 고급 애니메이션과 효과를 만드는 방법에 대해 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5b0c8f9909f59cb3a0dd5e16a6bb1c46fc069bfb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160807"
---
# <a name="using-the-visual-layer-with-xaml"></a>XAML과 함께 시각적 계층 사용

시각적 계층 기능을 사용 하는 대부분의 앱은 XAML을 사용 하 여 기본 UI 콘텐츠를 정의 합니다. Windows 10 기념일 업데이트에는 XAML 프레임 워크 및 시각적 계층에 새로운 기능이 있습니다 .이를 통해 이러한 두 기술을 결합 하 여 뛰어난 사용자 환경을 만들 수 있습니다.
Xaml 및 시각적 계층 interop 기능을 사용 하 여 XAML Api를 단독으로 사용할 수 없는 고급 애니메이션 및 효과를 만들 수 있습니다. 다음 내용이 포함됩니다.

- 흐림 효과 및 frosted 유리와 같은 브러시 효과
- 동적 조명 효과
- 스크롤 구동 애니메이션 및 시차
- 자동 레이아웃 애니메이션
- 픽셀-완벽 한 그림자

이러한 효과와 애니메이션은 기존 XAML 콘텐츠에 적용 될 수 있으므로 새 기능을 활용 하기 위해 XAML 앱을 크게 재구성할 필요가 없습니다.
레이아웃 애니메이션, 그림자 및 흐림 효과는 아래 조리법 섹션에서 설명 합니다. 시차을 구현 하는 코드 샘플은 [ParallaxingListItems 샘플](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems)을 참조 하세요. [Windowsuidevlabs 리포지토리에](https://github.com/microsoft/WindowsCompositionSamples) 는 애니메이션, 그림자 및 효과를 구현 하기 위한 몇 가지 다른 샘플도 있습니다.

## <a name="the-xamlcompositionbrushbase-class"></a>XamlCompositionBrushBase 클래스

**XamlCompositionBrush** 는 **CompositionBrush**를 사용 하 여 영역을 그리는 XAML 브러시에 대 한 기본 클래스를 제공 합니다. 이를 사용 하 여 흐림 효과 또는 frosted 유리와 같은 컴퍼지션 효과를 XAML UI 요소에 쉽게 적용할 수 있습니다.

XAML UI에서 브러시를 사용 하는 방법에 대 한 자세한 내용은 [**브러시**](../design/style/brushes.md#xamlcompositionbrushbase) 섹션을 참조 하십시오.

코드 샘플은 [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)의 참조 페이지를 참조하세요.

## <a name="the-xamllight-class"></a>XamlLight 클래스

**Xamllight** 는 **CompositionLight**영역을 동적으로 밝게 하는 XAML 조명 효과에 대 한 기본 클래스를 제공 합니다.

조명 XAML UI 요소를 비롯 한 광원 사용에 대 한 자세한 내용은 [**조명**](xaml-lighting.md) 섹션을 참조 하세요.

코드 예제를 보려면 [**Xamllight**](/uwp/api/windows.ui.xaml.media.xamllight)에 대 한 참조 페이지를 참조 하세요.

## <a name="the-elementcompositionpreview-class"></a>ElementCompositionPreview 클래스

[**ElementCompositionPreview**](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview) 는 XAML 및 비주얼 계층 interop 기능을 제공 하는 정적 클래스입니다. 시각적 계층 및 해당 기능에 대 한 개요는 [시각적 계층](./visual-layer.md)을 참조 하세요. **ElementCompositionPreview** 클래스는 다음 메서드를 제공 합니다.

-   [**Getelementvisual**](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)개체:이 요소를 렌더링 하는 데 사용 되는 "유인물" 시각적 개체 가져오기
-   [**Setelementchildvisual**](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual)개체: "수동" 시각적 개체를이 요소의 시각적 트리의 마지막 자식으로 설정 합니다. 이 시각적 개체는 요소의 나머지 부분 위에 그려집니다. 
-   [**GetElementChildVisual**](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual): **SetElementChildVisual**을 사용하여 시각적 개체 설정을 검색합니다.
-   [**GetScrollViewerManipulationPropertySet**](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual): **ScrollViewer** 에서 스크롤 오프셋을 기반으로 60fps 애니메이션을 만드는 데 사용할 수 있는 개체를 가져옵니다.

## <a name="remarks-on-elementcompositionpreviewgetelementvisual"></a>ElementCompositionPreview에 대 한 설명

**ElementCompositionPreview** 지정 된 **UIElement**를 렌더링 하는 데 사용 되는 "유인물" 시각적 개체를 반환 합니다. **표시**되는 속성은 UIElement의 상태를 기반으로 하 **는 XAML** 프레임 워크에 의해 설정 **됩니다.** 이렇게 하면 암시적인 애니메이션 위치 변경 ( *조리법*참조)과 같은 기술을 사용할 수 있습니다.

**오프셋과** **크기** 는 XAML 프레임 워크 레이아웃의 결과로 설정 되므로 개발자는 이러한 속성을 수정 하거나 애니메이션 효과를 적용할 때 주의 해야 합니다. 개발자는 요소의 왼쪽 위 모퉁이가 레이아웃의 부모와 동일한 위치인 경우에만 오프셋을 수정 하거나 애니메이션 효과를 적용 해야 합니다. 크기는 일반적으로 수정 되지 않아야 하지만 속성에 액세스 하는 것이 유용할 수 있습니다. 예를 들어, 아래의 그림자 및 Frosted 유리 샘플은 유인물 시각적 개체의 크기를 애니메이션에 대 한 입력으로 사용 합니다.

추가 주의 사항으로, 유인물 시각적 개체의 업데이트 된 속성은 해당 UIElement에 반영 되지 않습니다. 예를 들어 **UIElement** 를 0.5로 설정 하면 해당 하는 유인물 시각적 개체의 불투명도가 0.5로 설정 됩니다. 그러나 유인물 시각적 개체의 **불투명도** 를 0.5로 설정 하면 콘텐츠가 50% 불투명도로 표시 되지만 해당 UIElement의 opacity 속성 값은 변경 되지 않습니다.

### <a name="example-of-offset-animation"></a>**오프셋** 애니메이션의 예

#### <a name="incorrect"></a>틀렸습니다.

```xaml
<Border>
      <Image x:Name="MyImage" Margin="5" />
</Border>
```

```csharp
// Doesn’t work because Image has a margin!
ElementCompositionPreview.GetElementVisual(MyImage).StartAnimation("Offset", parallaxAnimation);
```

#### <a name="correct"></a>정답입니다.

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

## <a name="the-elementcompositionpreviewsetelementchildvisual-method"></a>**ElementCompositionPreview. SetElementChildVisual**

**ElementCompositionPreview** 개발자는 요소 시각적 트리의 일부로 표시 되는 "수동" 시각적 개체를 제공할 수 있습니다. 이를 통해 개발자는 시각적 기반 콘텐츠가 XAML UI 내에 표시 될 수 있는 "컴퍼지션 섬"을 만들 수 있습니다. 시각적 개체 기반 콘텐츠는 XAML 콘텐츠에 대 한 액세스 가능성 및 사용자 환경이 동일 하지 않기 때문에 개발자는이 기법을 사용 하는 것이 좋습니다. 따라서 일반적으로이 기법은 아래의 조리법 섹션에서 볼 수 있는 것과 같은 사용자 지정 효과를 구현 하는 데 필요한 경우에만 사용 하는 것이 좋습니다.

## <a name="getalphamask-methods"></a>**GetAlphaMask** 메서드

[**이미지**](/uwp/api/Windows.UI.Xaml.Controls.Image), [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)및 [**shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 는 각각 요소의 모양의 회색조 이미지를 나타내는 **CompositionBrush** 을 반환 하는 **GetAlphaMask** 라는 메서드를 구현 합니다. 이 **CompositionBrush** 는 컴퍼지션 **dropshadow**의 입력으로 사용할 수 있으므로 그림자는 사각형 대신 요소의 모양을 반영할 수 있습니다. 이를 통해 텍스트, 알파 및 모양이 포함 된 이미지에 대해 픽셀을 완벽 하 게 배분 된 그림자를 사용할 수 있습니다. 이 API에 대 한 예제는 아래 *그림자 삭제* 를 참조 하세요.

## <a name="recipes"></a>레시피

### <a name="reposition-animation"></a>애니메이션 위치 변경

개발자는 컴퍼지션 암시적 애니메이션을 사용 하 여 부모를 기준으로 요소 레이아웃의 변경 내용에 자동으로 애니메이션 효과를 적용할 수 있습니다. 예를 들어 아래 단추의 **여백을** 변경 하면 새 레이아웃 위치에 자동으로 애니메이션 효과가 적용 됩니다.

#### <a name="implementation-overview"></a>구현 개요

1. 대상 요소에 대 한 유인물 **시각적 개체** 가져오기
1. **Offset** 속성의 변경 내용에 자동으로 애니메이션 효과를 주는 **ImplicitAnimationCollection** 만들기
1. **ImplicitAnimationCollection** 을 지원 되는 시각적 개체와 연결

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

예를 들어 그림을 포함 하는 **타원과** 같은 픽셀 수준의 완벽 한 그림자를 **UIElement**에 적용 합니다. 섀도를 사용 하려면 앱에서 만든 **SpriteVisual** 가 필요 하므로 **ElementCompositionPreview**를 사용 하 여 **SpriteVisual** 를 포함 하는 "host" 요소를 만들어야 합니다.

#### <a name="implementation-overview"></a>구현 개요

1. 호스트 요소에 대 한 유인물 **시각적 개체** 가져오기
2. Windows. a u. 컴퍼지션 **Dropshadow** 를 만듭니다.
3. 마스크를 통해 대상 요소에서 셰이프를 가져오도록 **Dropshadow** 를 구성 합니다.
    - **Dropshadow** 는 기본적으로 사각형 이므로 대상이 사각형 인 경우에는 필요 하지 않습니다.
4. 새 **SpriteVisual**에 그림자를 연결 하 고 **SpriteVisual** 를 host 요소의 자식으로 설정 합니다.
5. **SpriteVisual** 크기를 **expressionanimation** 을 사용 하 여 호스트 크기에 바인딩

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

다음 두 목록은 동일한 XAML 구조를 사용 하 여 이전 C&#35; 코드와 동일한 [c + +/Winrt](../cpp-and-winrt-apis/index.md) 및 [c + +/cx](/cpp/cppcx/visual-c-language-reference-c-cx) 를 보여 줍니다.

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

배경 내용의 흐린 및 색조로 효과를 만듭니다. 개발자는 효과를 사용 하려면 Win2D NuGet 패키지를 설치 해야 합니다. 설치 지침은 [Win2D 홈 페이지](https://microsoft.github.io/Win2D/html/Introduction.htm) 를 참조 하세요.

#### <a name="implementation-overview"></a>구현 개요

1.  호스트 요소에 대 한 유인물 **시각적 개체** 가져오기
2.  Win2D 및 **CompositionEffectSourceParameter** 를 사용 하 여 흐림 효과 트리 만들기
3.  효과 트리를 기반으로 **CompositionEffectBrush** 만들기
4.  **CompositionEffectBrush** 의 입력을 **CompositionBackdropBrush**로 설정 하 여 효과를 **SpriteVisual** 뒤의 내용에 적용할 수 있도록 합니다.
5.  **CompositionEffectBrush** 을 새 **SpriteVisual**의 콘텐츠로 설정 하 고 **SpriteVisual** 를 host 요소의 자식으로 설정 합니다. XamlCompositionBrushBase를 사용할 수도 있습니다.
6.  **SpriteVisual** 크기를 **expressionanimation** 을 사용 하 여 호스트 크기에 바인딩

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

- [시각적 계층 개요](./visual-layer.md)
- [**ElementCompositionPreview** 클래스](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview)
- [Windowsuidevlabs GitHub](https://github.com/microsoft/WindowsCompositionSamples) 의 고급 UI 및 컴퍼지션 샘플
- [BasicXamlInterop 샘플](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK%2010586/BasicXamlInterop)
- [ParallaxingListItems 샘플](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems)