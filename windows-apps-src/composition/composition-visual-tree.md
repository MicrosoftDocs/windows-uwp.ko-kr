---
ms.assetid: f1297b7d-1a10-52ae-dd84-6d1ad2ae2fe6
title: 컴퍼지션 시각
description: 컴퍼지션 시각적 개체는 컴퍼지션 API의 다른 모든 기능을 사용 하 고 작성 하는 시각적 트리 구조를 구성 합니다. 개발자는이 API를 사용 하 여 시각적 트리의 단일 노드를 나타내는 하나 이상의 시각적 개체를 정의 하 고 만들 수 있습니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d85df48b4f43759013f80623595d919ac6c77337
ms.sourcegitcommit: ef3cdca5e9b8f032f46174da4574cb5593d32d56
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90593437"
---
# <a name="composition-visual"></a>컴퍼지션 시각

컴퍼지션 시각적 개체는 컴퍼지션 API의 다른 모든 기능을 사용 하 고 작성 하는 시각적 트리 구조를 구성 합니다. 개발자는이 API를 사용 하 여 시각적 트리의 단일 노드를 나타내는 하나 이상의 시각적 개체를 정의 하 고 만들 수 있습니다.

## <a name="visuals"></a>시각적 개체

시각적 트리 구조와 시각적 트리 구조를 구성 하는 몇 가지 시각적 유형과 시각적 개체의 내용에 영향을 주는 여러 서브 클래스가 포함 된 기본 브러시 클래스가 있습니다.

- [**시각적**](/uwp/api/Windows.UI.Composition.Visual) 개체-기본 개체, 대부분의 속성은 여기에 있으며 다른 시각적 개체에 의해 상속 됩니다.
- [**System.windows.media.containervisual>**](/uwp/api/Windows.UI.Composition.ContainerVisual) – [**시각적 개체**](/uwp/api/Windows.UI.Composition.Visual)에서 파생 되 고 자식을 만드는 기능을 추가 합니다.
  - [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) – [**system.windows.media.containervisual>**](/uwp/api/Windows.UI.Composition.ContainerVisual)에서 파생 됩니다. 에는 브러시를 연결 하 여 이미지가 이미지, 효과 또는 단색을 포함 하는 픽셀을 렌더링할 수 있는 기능이 있습니다.
  - [**LayerVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) – [**system.windows.media.containervisual>**](/uwp/api/Windows.UI.Composition.ContainerVisual)에서 파생 됩니다. 시각적 개체의 자식은 단일 계층으로 결합 됩니다.<br/>(_Windows 10 버전 1607, SDK 14393에서 도입 되었습니다._)
  - [**ShapeVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) – [**system.windows.media.containervisual>**](/uwp/api/Windows.UI.Composition.ContainerVisual)에서 파생 됩니다. CompositionShape의 루트인 시각적 트리 노드입니다.<br/>(_Windows 10 버전 1803, SDK 17134에서 도입 되었습니다._)
  - [**Redirectvisual 개체**](/uwp/api/Windows.UI.Composition.SpriteVisual) – [**system.windows.media.containervisual>**](/uwp/api/Windows.UI.Composition.ContainerVisual)에서 파생 됩니다. 시각적 개체는 다른 시각적 개체에서 해당 콘텐츠를 가져옵니다.<br/>(_Windows 10 버전 1809, SDK 17763에서 도입 되었습니다._)
  - [**SceneVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) – [**system.windows.media.containervisual>**](/uwp/api/Windows.UI.Composition.ContainerVisual)에서 파생 됩니다. 3D 장면의 노드에 대 한 컨테이너 시각적 개체입니다.<br/>(_Windows 10 버전 1903, SDK 18362에서 도입 되었습니다._)

[**CompositionColorBrush**](/uwp/api/Windows.UI.Composition.CompositionColorBrush), [**CompositionSurfaceBrush**](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 및 [**CompositionEffectBrush**](/uwp/api/Windows.UI.Composition.CompositionEffectBrush)를 포함 하 여 [**CompositionBrush**](/uwp/api/Windows.UI.Composition.CompositionBrush) 및 해당 하위 클래스를 사용 하 여 SpriteVisuals에 콘텐츠와 효과를 적용할 수 있습니다. 브러시에 대해 자세히 알아보려면 [**CompositionBrush 개요**](./composition-brushes.md)를 참조 하세요.

## <a name="the-compositionvisual-sample"></a>CompositionVisual 샘플

여기서는 앞에 나열 된 세 가지 다른 시각적 개체 유형을 보여 주는 샘플 코드를 살펴보겠습니다. 이 샘플에서는 애니메이션이 나 더 복잡 한 효과와 같은 개념을 다루지 않지만 이러한 모든 시스템에서 사용 하는 빌딩 블록을 포함 합니다. (전체 샘플 코드는이 문서의 끝에 나열 되어 있습니다.)

이 샘플에서는 화면을 마우스로 클릭 하 여 끌 수 있는 단색 사각형의 수를 표시 합니다. 사각형을 클릭 하면 전면으로 이동 하 여 45도 회전 하 고, 끌 때 불투명 하 게 됩니다.

다음을 포함 하 여 API를 사용 하는 다양 한 기본 개념을 보여 줍니다.

- Compositor 만들기
- CompositionColorBrush를 사용 하 여 SpriteVisual 만들기
- 시각적 개체 클리핑
- 시각적 개체 회전
- 불투명도 설정
- 컬렉션에서 시각적 개체의 위치를 변경 합니다.

## <a name="creating-a-compositor"></a>Compositor 만들기

[**Compositor**](/uwp/api/Windows.UI.Composition.Compositor) 를 만들어 팩터리에서 사용할 변수에 저장 하는 것은 간단한 작업입니다. 다음 코드 조각에서는 새 **Compositor**를 만드는 방법을 보여 줍니다.

```cs
_compositor = new Compositor();
```

## <a name="creating-a-spritevisual-and-colorbrush"></a>SpriteVisual 및 ColorBrush 만들기

[**Compositor**](/uwp/api/Windows.UI.Composition.Compositor) 를 사용 하면 [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) 및 [**CompositionColorBrush**](/uwp/api/Windows.UI.Composition.CompositionColorBrush)와 같이 필요할 때마다 개체를 쉽게 만들 수 있습니다.

```cs
var visual = _compositor.CreateSpriteVisual();
visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
```

이는 몇 줄의 코드에 불과합니다. [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) 개체가 효과 시스템의 핵심 이라는 강력한 개념을 보여 줍니다. **SpriteVisual** 에서는 색, 이미지 및 효과를 만들 때 뛰어난 유연성과 상호 작용을 사용할 수 있습니다. **SpriteVisual** 는 2d 사각형을 브러시 (이 경우 단색)로 채울 수 있는 단일 시각적 유형입니다.

## <a name="clipping-a-visual"></a>시각적 개체 클리핑

[**Compositor**](/uwp/api/Windows.UI.Composition.Compositor) 를 사용 하 여 [**시각적 개체**](/uwp/api/Windows.UI.Composition.Visual)에 대 한 클립을 만들 수도 있습니다. 다음은 [**InsetClip**](/uwp/api/Windows.UI.Composition.InsetClip) 를 사용 하 여 시각적 개체의 각 측면을 트리밍하는 샘플의 예제입니다.

```cs
var clip = _compositor.CreateInsetClip();
clip.LeftInset = 1.0f;
clip.RightInset = 1.0f;
clip.TopInset = 1.0f;
clip.BottomInset = 1.0f;
_currentVisual.Clip = clip;
```

API의 다른 개체와 마찬가지로 [**InsetClip**](/uwp/api/Windows.UI.Composition.InsetClip) 는 속성에 애니메이션을 적용할 수 있습니다.

## <a name="span-idrotating_a_clipspanspan-idrotating_a_clipspanspan-idrotating_a_clipspanrotating-a-clip"></a><span id="Rotating_a_Clip"></span><span id="rotating_a_clip"></span><span id="ROTATING_A_CLIP"></span>클립 회전

[**시각적 개체**](/uwp/api/Windows.UI.Composition.Visual) 는 회전을 사용 하 여 변형할 수 있습니다. [**RotationAngle**](/uwp/api/windows.ui.composition.visual.rotationangle) 는 라디안 및도를 모두 지원 합니다. 기본값은 라디안 이지만 다음 코드 조각과 같이 각도를 지정 하는 것이 쉽습니다.

```cs
child.RotationAngleInDegrees = 45.0f;
```

이러한 작업을 더 쉽게 수행 하기 위해 API에서 제공 하는 변환 구성 요소 집합의 한 가지 예로 회전이 있습니다. 다른 항목에는 Offset, Scale, Orientation, RotationAxis 및 4x4 TransformMatrix가 포함 됩니다.

## <a name="setting-opacity"></a>불투명도 설정

시각적 개체의 불투명도를 설정 하는 작업은 float 값을 사용 하는 간단한 작업입니다. 예를 들어 샘플에서 모든 사각형은. 8 불투명도에서 시작 합니다.

```cs
visual.Opacity = 0.8f;
```

회전과 마찬가지로 [**Opacity**](/uwp/api/windows.ui.composition.visual.opacity) 속성에 애니메이션을 적용할 수 있습니다.

## <a name="changing-the-visuals-position-in-the-collection"></a>컬렉션에서 시각적 개체의 위치 변경

컴퍼지션 API를 사용 하면 다양 한 방법으로 [**VisualCollection**](/uwp/api/windows.ui.composition.visualcollection) 의 시각적 위치를 변경할 수 있습니다. [**InsertAbove**](/uwp/api/windows.ui.composition.visualcollection.insertabove)를 사용 하 여 다른 시각적 개체 위에 배치 하거나 아래 [**insertbelow**](/uwp/api/windows.ui.composition.visualcollection.insertbelow) [**InsertAtTop**](/uwp/api/windows.ui.composition.visualcollection.insertattop)를 사용 하 여 위쪽으로 이동 하거나 [**InsertAtBottom**](/uwp/api/windows.ui.composition.visualcollection.insertatbottom)를 사용 하 여 아래쪽으로 이동할 수 있습니다.

이 샘플에서는 클릭 한 [**시각적 개체**](/uwp/api/Windows.UI.Composition.Visual) 를 위쪽으로 정렬 합니다.

```cs
parent.Children.InsertAtTop(_currentVisual);
```

## <a name="full-example"></a>전체 예

전체 샘플에서 위의 모든 개념은 XAML, WWA 또는 DirectX를 사용 하지 않고 불투명도를 변경 하기 위해 간단한 [**시각적**](/uwp/api/Windows.UI.Composition.Visual) 개체 트리를 생성 하 고 탐색 하는 데 함께 사용 됩니다. 이 샘플에서는 자식 **시각적** 개체를 만들고 추가 하는 방법 및 속성을 변경 하는 방법을 보여 줍니다.

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Numerics;
using System.Text;
using System.Threading.Tasks;
using Windows.ApplicationModel.Core;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.Composition;
using Windows.UI.Core;

namespace compositionvisual
{
    class VisualProperties : IFrameworkView
    {
        //------------------------------------------------------------------------------
        //
        // VisualProperties.Initialize
        //
        // This method is called during startup to associate the IFrameworkView with the
        // CoreApplicationView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Initialize(CoreApplicationView view)
        {
            _view = view;
            _random = new Random();
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.SetWindow
        //
        // This method is called when the CoreApplication has created a new CoreWindow,
        // allowing the application to configure the window and start producing content
        // to display.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.SetWindow(CoreWindow window)
        {
            _window = window;
            InitNewComposition();
            _window.PointerPressed += OnPointerPressed;
            _window.PointerMoved += OnPointerMoved;
            _window.PointerReleased += OnPointerReleased;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerPressed
        //
        // This method is called when the user touches the screen, taps it with a stylus
        // or clicks the mouse.
        //
        //------------------------------------------------------------------------------

        void OnPointerPressed(CoreWindow window, PointerEventArgs args)
        {
            Point position = args.CurrentPoint.Position;

            //
            // Walk our list of visuals to determine who, if anybody, was selected
            //
            foreach (var child in _root.Children)
            {
                //
                // Did we hit this child?
                //
                Vector3 offset = child.Offset;
                Vector2 size = child.Size;

                if ((position.X >= offset.X) &&
                    (position.X < offset.X + size.X) &&
                    (position.Y >= offset.Y) &&
                    (position.Y < offset.Y + size.Y))
                {
                    //
                    // This child was hit. Since the children are stored back to front,
                    // the last one hit is the front-most one so it wins
                    //
                    _currentVisual = child as ContainerVisual;
                    _offsetBias = new Vector2((float)(offset.X - position.X),
                                              (float)(offset.Y - position.Y));
                }
            }

            //
            // If a visual was hit, bring it to the front of the Z order
            //
            if (_currentVisual != null)
            {
                ContainerVisual parent = _currentVisual.Parent as ContainerVisual;
                parent.Children.Remove(_currentVisual);
                parent.Children.InsertAtTop(_currentVisual);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerMoved
        //
        // This method is called when the user moves their finger, stylus or mouse with
        // a button pressed over the screen.
        //
        //------------------------------------------------------------------------------

        void OnPointerMoved(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual is selected, drag it with the pointer position and
            // make it opaque while we drag it
            //
            if (_currentVisual != null)
            {
                //
                // Set up the properties of the visual the first time it is
                // dragged. This will last for the duration of the drag
                //
                if (!_dragging)
                {
                    _currentVisual.Opacity = 1.0f;

                    //
                    // Transform the first child of the current visual so that
                    // the image is rotated
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngleInDegrees = 45.0f;
                        child.CenterPoint = new Vector3(_currentVisual.Size.X / 2, _currentVisual.Size.Y / 2, 0);
                        break;
                    }

                    //
                    // Clip the visual to its original layout rect by using an inset
                    // clip with a one-pixel margin all around
                    //
                    var clip = _compositor.CreateInsetClip();
                    clip.LeftInset = 1.0f;
                    clip.RightInset = 1.0f;
                    clip.TopInset = 1.0f;
                    clip.BottomInset = 1.0f;
                    _currentVisual.Clip = clip;

                    _dragging = true;
                }

                Point position = args.CurrentPoint.Position;
                _currentVisual.Offset = new Vector3((float)(position.X + _offsetBias.X),
                                                    (float)(position.Y + _offsetBias.Y),
                                                    0.0f);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerReleased
        //
        // This method is called when the user lifts their finger or stylus from the
        // screen, or lifts the mouse button.
        //
        //------------------------------------------------------------------------------

        void OnPointerReleased(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual was selected, make it transparent again when it is
            // released and restore the transform and clip
            //
            if (_currentVisual != null)
            {
                if (_dragging)
                {
                    //
                    // Remove the transform from the first child
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngle = 0.0f;
                        child.CenterPoint = new Vector3(0.0f, 0.0f, 0.0f);
                        break;
                    }

                    _currentVisual.Opacity = 0.8f;
                    _currentVisual.Clip = null;
                    _dragging = false;
                }

                _currentVisual = null;
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Load
        //
        // This method is called when a specific page is being loaded in the
        // application.  It is not used for this application.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Load(string unused)
        {

        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Run
        //
        // This method is called by CoreApplication.Run() to actually run the
        // dispatcher's message pump.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Run()
        {
            _window.Activate();
            _window.Dispatcher.ProcessEvents(CoreProcessEventsOption.ProcessUntilQuit);
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Uninitialize
        //
        // This method is called during shutdown to disconnect the CoreApplicationView,
        // and CoreWindow from the IFrameworkView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Uninitialize()
        {
            _window = null;
            _view = null;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.InitNewComposition
        //
        // This method is called by SetWindow(), where we initialize Composition after
        // the CoreWindow has been created.
        //
        //------------------------------------------------------------------------------

        void InitNewComposition()
        {
            //
            // Set up Windows.UI.Composition Compositor, root ContainerVisual, and associate with
            // the CoreWindow.
            //

            _compositor = new Compositor();

            _root = _compositor.CreateContainerVisual();



            _compositionTarget = _compositor.CreateTargetForCurrentView();
            _compositionTarget.Root = _root;

            //
            // Create a few visuals for our window
            //
            for (int index = 0; index < 20; index++)
            {
                _root.Children.InsertAtTop(CreateChildElement());
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.CreateChildElement
        //
        // Creates a small sub-tree to represent a visible element in our application.
        //
        //------------------------------------------------------------------------------

        Visual CreateChildElement()
        {
            //
            // Each element consists of three visuals, which produce the appearance
            // of a framed rectangle
            //
            var element = _compositor.CreateContainerVisual();
            element.Size = new Vector2(100.0f, 100.0f);

            //
            // Position this visual randomly within our window
            //
            element.Offset = new Vector3((float)(_random.NextDouble() * 400), (float)(_random.NextDouble() * 400), 0.0f);

            //
            // The outer rectangle is always white
            //
            //Note to preview API users - SpriteVisual and Color Brush replace SolidColorVisual
            //for example instead of doing
            //var visual = _compositor.CreateSolidColorVisual() and
            //visual.Color = Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF);
            //we now use the below

            var visual = _compositor.CreateSpriteVisual();
            element.Children.InsertAtTop(visual);
            visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
            visual.Size = new Vector2(100.0f, 100.0f);

            //
            // The inner rectangle is inset from the outer by three pixels all around
            //
            var child = _compositor.CreateSpriteVisual();
            visual.Children.InsertAtTop(child);
            child.Offset = new Vector3(3.0f, 3.0f, 0.0f);
            child.Size = new Vector2(94.0f, 94.0f);

            //
            // Pick a random color for every rectangle
            //
            byte red = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte green = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte blue = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            child.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, red, green, blue));

            //
            // Make the subtree root visual partially transparent. This will cause each visual in the subtree
            // to render partially transparent, since a visual's opacity is multiplied with its parent's
            // opacity
            //
            element.Opacity = 0.8f;

            return element;
        }

        // CoreWindow / CoreApplicationView
        private CoreWindow _window;
        private CoreApplicationView _view;

        // Windows.UI.Composition
        private Compositor _compositor;
        private CompositionTarget _compositionTarget;
        private ContainerVisual _root;
        private ContainerVisual _currentVisual;
        private Vector2 _offsetBias;
        private bool _dragging;

        // Helpers
        private Random _random;
    }


    public sealed class VisualPropertiesFactory : IFrameworkViewSource
    {
        //------------------------------------------------------------------------------
        //
        // VisualPropertiesFactory.CreateView
        //
        // This method is called by CoreApplication to provide a new IFrameworkView for
        // a CoreWindow that is being created.
        //
        //------------------------------------------------------------------------------

        IFrameworkView IFrameworkViewSource.CreateView()
        {
            return new VisualProperties();
        }


        //------------------------------------------------------------------------------
        //
        // main
        //
        //------------------------------------------------------------------------------

        static int Main(string[] args)
        {
            CoreApplication.Run(new VisualPropertiesFactory());

            return 0;
        }
    }
}
```