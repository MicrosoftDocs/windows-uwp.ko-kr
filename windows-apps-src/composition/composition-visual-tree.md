---
ms.assetid: f1297b7d-1a10-52ae-dd84-6d1ad2ae2fe6
title: 컴퍼지션 시각
description: 컴퍼지션 시각적 개체는 컴퍼지션 API의 다른 모든 기능이 사용하고 빌드되는 시각적 트리 구조를 구성합니다. API를 사용하면 개발자가 시각적 트리의 단일 노드를 나타내는 시각적 개체를 하나 또는 여러 개 정의하고 만들 수 있습니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6b1c0b78ca45d98428f38518b337b5889f595c49
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8943381"
---
# <a name="composition-visual"></a>컴퍼지션 시각

컴퍼지션 시각적 개체는 컴퍼지션 API의 다른 모든 기능이 사용하고 빌드되는 시각적 트리 구조를 구성합니다. API를 사용하면 개발자가 시각적 트리의 단일 노드를 나타내는 시각적 개체를 하나 또는 여러 개 정의하고 만들 수 있습니다.

## <a name="visuals"></a>화면 효과

시각적 트리 구조는 세 가지 시각적 개체 형식과, 시각적 개체 콘텐츠에 영향을 주는 여러 하위 클래스가 있는 기본 브러시 클래스로 구성됩니다.

- [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) – 기준 개체, 대부분의 속성은 여기에 있으며 다른 시각적 개체에 의해 상속됩니다.
- [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) – [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)에서 파생되며 자식 시각적 개체를 만들 수 있는 기능을 추가합니다.
- [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) – [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810)에서 파생되며 시각적 개체가 이미지, 효과 또는 단색 등의 픽셀을 렌더링할 수 있도록 브러시를 연결하는 기능을 추가합니다.

[**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589398) 및 [**CompositionColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush), [**CompositionSurfaceBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush), [**CompositionEffectBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush) 등의 하위 클래스를 사용하여 SpriteVisuals에 콘텐츠 및 효과를 적용할 수 있습니다. 브러시에 대한 자세한 내용을 보려면 [**CompositionBrush 개요**](https://docs.microsoft.com/windows/uwp/composition/composition-brushes)를 참조하세요.

## <a name="the-compositionvisual-sample"></a>CompositionVisual 샘플

여기서는 앞에서 나열한 세 가지 시각적 형식을 보여 주는 몇 가지 샘플 코드를 살펴보겠습니다. 이 샘플에서는 애니메이션 또는 더 복잡한 효과 등의 개념은 제공하지 않으며 이러한 모든 시스템에서 사용하는 구성 요소만 다룹니다. (전체 샘플 코드는 이 문서의 끝에 나열되어 있습니다.)

샘플에는 클릭하여 화면으로 끌 수 있는 단색 사각형이 여러 개 포함되어 있습니다. 사각형을 클릭하면 앞으로 가져와서 45도 회전되고 끌면 불투명해집니다.

다음을 포함하여 API 작업에 대한 많은 기본 개념을 보여 줍니다.

- 작성자 만들기
- CompositionColorBrush를 사용하여 SpriteVisual 만들기
- 시각적 개체 클리핑
- 시각적 개체 회전
- 불투명도 설정
- 컬렉션에서 시각적 개체의 위치 변경

## <a name="creating-a-compositor"></a>작성자 만들기

팩터리로 사용하기 위해 [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789)를 만들고 변수에 저장하는 작업은 매우 간단합니다. 다음 코드 조각은 새 **Compositor**를 만드는 방법을 보여 줍니다.

```cs
_compositor = new Compositor();
```

## <a name="creating-a-spritevisual-and-colorbrush"></a>SpriteVisual 및 ColorBrush 만들기

[**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789)를 사용하면 필요할 때마다 [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) 및 [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) 등의 개체를 쉽게 만들 수 있습니다.

```cs
var visual = _compositor.CreateSpriteVisual();
visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
```

이 코드는 매우 간단하지만 강력한 개념을 보여 주며 [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) 개체는 효과 시스템의 핵심입니다. **SpriteVisual**은 뛰어난 유연성을 제공하며 색, 이미지 및 효과 생성의 상호 작용을 허용합니다. **SpriteVisual**은 브러시(이 경우 단색)로 2D 사각형을 채울 수 있는 단일 시각적 개체 형식입니다.

## <a name="clipping-a-visual"></a>시각적 개체 클리핑

[**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789)는 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)에 대한 클립을 만드는 데도 사용할 수 있습니다. 다음은 샘플에서 [**InsetClip**](https://msdn.microsoft.com/library/windows/apps/Dn706825)을 사용하여 시각적 개체의 각 면을 트리밍하는 예제입니다.

```cs
var clip = _compositor.CreateInsetClip();
clip.LeftInset = 1.0f;
clip.RightInset = 1.0f;
clip.TopInset = 1.0f;
clip.BottomInset = 1.0f;
_currentVisual.Clip = clip;
```

API의 다른 개체와 마찬가지로 [**InsetClip**](https://msdn.microsoft.com/library/windows/apps/Dn706825)은 속성에 애니메이션을 적용할 수 있습니다.

## <a name="span-idrotatingaclipspanspan-idrotatingaclipspanspan-idrotatingaclipspanrotating-a-clip"></a><span id="Rotating_a_Clip"></span><span id="rotating_a_clip"></span><span id="ROTATING_A_CLIP"></span>클립 회전

회전을 통해 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)를 변환할 수 있습니다. 이때 [**RotationAngle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.rotationangle)은 라디안과 각도를 모두 지원합니다. 기본값은 라디안이지만 다음 코드 조각에서처럼 쉽게 각도를 지정할 수 있습니다.

```cs
child.RotationAngleInDegrees = 45.0f;
```

회전은 이러한 작업을 쉽게 수행하기 위해 API에서 제공하는 여러 가지 변환 구성 요소 중 하나입니다. 그 밖에 Offset, Scale, Orientation, RotationAxis, 4x4 TransformMatrix 등이 있습니다.

## <a name="setting-opacity"></a>불투명도 설정

부동 소수점 값을 사용하여 시각적 개체의 불투명도를 간단히 설정할 수 있습니다. 예를 들어 샘플에서 모든 사각형은 .8 불투명도로 시작합니다.

```cs
visual.Opacity = 0.8f;
```

회전과 마찬가지로 [**Opacity**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.opacity) 속성에 애니메이션을 적용할 수 있습니다.

## <a name="changing-the-visuals-position-in-the-collection"></a>컬렉션에서 시각적 개체의 위치 변경

컴퍼지션 API는 [**VisualCollection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection)의 시각적 개체 위치를 여러 가지 방법으로 변경할 수 있습니다. [**InsertAbove**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertabove)를 사용하여 다른 시각적 개체 위에 배치할 수도 있고, [**InsertBelow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertbelow)를 사용하여 아래에 배치할 수도 있고, [**InsertAtTop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertattop)을 사용하여 맨 위로 이동할 수도 있고, [**InsertAtBottom**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertatbottom)을 사용하여 맨 아래에 배치할 수도 있습니다.

이 샘플에서 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)을 클릭하면 맨 위로 정렬됩니다.

```cs
parent.Children.InsertAtTop(_currentVisual);
```

## <a name="full-example"></a>전체 예제

전체 샘플에서는 위의 모든 개념이 함께 사용되어 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 개체의 간단한 트리를 구성하여 XAML, WWA 또는 DirectX를 사용하지 않고 불투명도를 변경합니다. 이 샘플에서는 자식 **Visual** 개체를 만들고 추가하고 속성을 변경하는 방법을 보여 줍니다.

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
