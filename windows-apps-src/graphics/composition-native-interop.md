---
author: scottmill
ms.assetid: 16ad97eb-23f1-0264-23a9-a1791b4a5b95
title: "BeginDraw 및 EndDraw를 사용하여 컴퍼지션 네이티브 DirectX 및 Direct2D 상호 운용"
description: "Windows.UI.Composition API는 콘텐츠를 작성자로 직접 이동할 수 있는 네이티브 상호 운용 인터페이스를 제공합니다."
translationtype: Human Translation
ms.sourcegitcommit: b3d198af0c46ec7a2041a7417bccd56c05af760e
ms.openlocfilehash: b5308c8023990996a93277dd1bcfb8298c0bbf4f

---
# BeginDraw 및 EndDraw를 사용하여 컴퍼지션 네이티브 DirectX 및 Direct2D 상호 운용

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

Windows.UI.Composition API는 콘텐츠를 작성자로 직접 이동할 수 있는 [**ICompositorInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620068), [**ICompositionDrawingSurfaceInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620058), 및 [**ICompositionGraphicsDeviceInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620065) 네이티브 상호 운용 인터페이스를 제공합니다.

네이티브 상호 운용은 DirectX 텍스처가 지원되는 표면 개체 주위로 구조화됩니다. 표면은 [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749)라는 팩터리 개체에서 만들어집니다. 이 개체는 기본 Direct3D 또는 Direct2D 장치 개체에 의해 지원되며 표면의 비디오 메모리를 할당하는 데 사용됩니다. 컴퍼지션 API는 기본 DirectX 장치를 만들지 않습니다. 기본 DirectX 장치를 만들어 **CompositionGraphicsDevice** 개체로 전달하는 것은 응용 프로그램에서 담당합니다. 응용 프로그램은 한 번에 둘 이상의 **CompositionGraphicsDevice** 개체를 만들 수 있으며, 여러 **CompositionGraphicsDevice** 개체에 대한 렌더링 장치로 동일한 DirectX 장치를 사용할 수 있습니다.

## 표면 만들기

각 [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749)는 표면 팩터리 역할을 합니다. 각 표면은 만들어질 때 초기 크기(0,0이 될 수 있음)가 설정되며 유효 픽셀은 없습니다. 초기 상태의 표면은 시각적 트리에서 즉시 사용될 수 있지만(예를 들어 [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) 및 [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433)을 통해) 초기 상태의 표면은 화면 출력에 영향을 주지 않습니다. 이 표면은 완전히 투명하며 지정된 알파 모드가 "불투명"인 경우에도 마찬가지입니다.

경우에 따라 DirectX 장치를 사용할 수 없도록 렌더링될 수 있습니다. 이러한 문제가 발생하는 원인은 대부분 응용 프로그램에서 특정 DirectX API에 잘못된 인수를 전달하거나, 시스템에 의해 그래픽 어댑터가 초기화되거나, 드라이버가 업데이트되었기 때문입니다. Direct3D에는 어떤 이유로든 장치가 분실된 경우 응용 프로그램에서 비동기적으로 검색하는 데 사용할 수 있는 API가 있습니다. DirectX 장치가 분실된 경우 응용 프로그램은 이를 삭제하고 새로 만든 다음 잘못된 DirectX 장치에 이전에 연결되었던 모든 [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) 개체에 전달해야 합니다.

## 표면에 픽셀 로드

표면에 픽셀을 로드하려면 응용 프로그램의 요청에 따라 Direct2D 컨텍스트 또는 텍스처를 나타내는 DirectX 인터페이스를 반환하는 [**BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620059.aspx) 메서드를 응용 프로그램에서 호출해야 합니다. 그런 다음 응용 프로그램은 픽셀을 해당 텍스처로 렌더링하거나 업로드해야 합니다. 응용 프로그램이 완료되면 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) 메서드를 호출해야 합니다. 이 시점에서만 컴퍼지션에서 새 픽셀을 사용할 수 있지만 다음에 시각적 트리에 대한 모든 변경 사항이 커밋될 때까지 화면에 표시되지 않습니다. **EndDraw**가 호출되기 전에 시각적 트리가 커밋되면 진행 중인 업데이트가 화면에 표시되지 않고 표면에 **BeginDraw** 전의 콘텐츠가 계속 표시됩니다. **EndDraw**가 호출되면 BeginDraw에서 반환한 텍스처 또는 Direct2D 컨텍스트 포인터가 무효화됩니다. **EndDraw** 호출 후에는 응용 프로그램에서 해당 포인터를 캐시할 수 없습니다.

응용 프로그램에서는 지정된 [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749)에 대해 한 번에 한 표면에서만 BeginDraw를 호출할 수 있습니다. [**BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620059.aspx) 호출 후 응용 프로그램은 해당 표면에서 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060)를 호출한 다음 다른 표면에서 **BeginDraw**를 호출해야 합니다. API는 Agile이므로 여러 작업자 스레드에서 렌더링을 수행하려면 응용 프로그램에서 이러한 호출을 동기화합니다. 응용 프로그램에서 한 표면의 렌더링을 중단하고 임시로 다른 표면으로 전환하려는 경우 [**SuspendDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620064.aspx) 메서드를 사용할 수 있습니다. 이렇게 하면 다른 **BeginDraw**가 성공하지만 화면 컴퍼지션에서 첫 번째 표면 업데이트를 사용할 수 없습니다. 이 경우 응용 프로그램에서 트랜잭션 방식으로 여러 업데이트를 수행할 수 있습니다. 표면이 일시 중단되면 응용 프로그램에서 [**ResumeDraw**](https://msdn.microsoft.com/library/windows/apps/mt620062) 메서드를 호출하여 업데이트를 계속 진행하거나 **EndDraw**를 호출하여 해당 업데이트가 완료된 것으로 선언할 수 있습니다. 즉, 지정된 **CompositionGraphicsDevice**에 대해 한 번에 한 표면만 업데이트할 수 있습니다. 각 그래픽 장치는 상호 독립적으로 이 상태를 유지하므로 표면이 서로 다른 그래픽 장치에 속한 경우 응용 프로그램에서 두 표면을 동시에 렌더링할 수 있습니다. 그러나 이러한 두 표면의 비디오 메모리를 함께 풀링할 수 없으므로 메모리 효율성이 떨어집니다.

응용 프로그램에서 잘못된 인수를 전달하거나 한 표면에서 **EndDraw**를 호출하기 전에 다른 표면에서 **BeginDraw**를 호출하는 등 잘못된 작업을 수행한 경우 [**BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620059.aspx), [**SuspendDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620064.aspx), [**ResumeDraw**](https://msdn.microsoft.com/library/windows/apps/mt620062) 및 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) 메서드에서 오류를 반환합니다. 이러한 오류는 응용 프로그램 버그를 의미하므로 빠른 오류로 처리되어야 합니다. 기본 DirectX 장치가 분실된 경우에도 **BeginDraw**에서 오류를 반환할 수 있습니다. 응용 프로그램에서 해당 DirectX 장치를 다시 만든 다음 다시 시도할 수 있으므로 이 오류는 치명적이지 않습니다. 따라서 응용 프로그램은 렌더링을 생략하여 장치 분실을 처리해야 합니다. 어떤 이유로든 **BeginDraw**가 실패한 경우 시작되지 않았으므로 응용 프로그램에서 **EndDraw**를 호출해서는 안 됩니다.

## 스크롤

성능상의 이유로 응용 프로그램에서 [**BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620059.aspx)를 호출하는 경우 반환된 텍스처의 콘텐츠가 반드시 표면의 이전 콘텐츠라는 보장이 없습니다. 응용 프로그램에서는 콘텐츠가 임의적이라는 것을 감안하여 렌더링 전에 표면을 지우거나 업데이트된 사각형이 모두 채워지도록 충분히 불투명한 콘텐츠를 그려 모든 픽셀이 터치되도록 해야 합니다. 그 외에 **BeginDraw** 및 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) 호출 사이에서만 텍스처 포인터가 유효하므로 응용 프로그램이 표면 외부에서 이전 콘텐츠를 복사할 수 없습니다. 이러한 이유로 Microsoft에서는 응용 프로그램에서 동일 표면 픽셀 복사를 수행할 수 있는 [**Scroll**](https://msdn.microsoft.com/library/windows/apps/mt620063) 메서드를 제공합니다.

## 사용 예제

다음 샘플에서는 응용 프로그램에서 그리기 표면을 만들고 [**BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/apps/mt620059.aspx) 및 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060)를 사용하여 표면을 텍스트로 채우는 매우 간단한 시나리오를 설명합니다. 응용 프로그램에서는 DirectWrite를 사용하여 텍스트의 레이아웃을 지정하고(세부 항목은 표시되지 않음) Direct2D를 사용하여 렌더링합니다. 컴퍼지션 그래픽 장치는 초기화 시 직접 Direct2D 장치를 사용합니다. 그러므로 **BeginDraw**는 ID2D1DeviceContext 인터페이스 포인터를 반환할 수 있습니다. 이 방법은 그리기 작업을 수행할 때마다 응용 프로그램에서 Direct2D 컨텍스트를 만들어 반환된 ID3D11Texture2D 인터페이스를 래핑하는 것보다 훨씬 효율적입니다.

```cpp
//------------------------------------------------------------------------------
//
// Copyright (C) Microsoft. All rights reserved.
//
//------------------------------------------------------------------------------

#include "stdafx.h"

using namespace Microsoft::WRL;
using namespace Windows::Foundation;
using namespace Windows::Graphics::DirectX;
using namespace Windows::UI::Composition;

// This is an app-provided helper to render lines of text
class SampleText
{
private:
    // The text to draw
    ComPtr<IDWriteTextLayout> _text;

    // The composition surface that we use in the visual tree
    ComPtr<ICompositionDrawingSurfaceInterop> _drawingSurfaceInterop;

    // The device that owns the surface
    ComPtr<ICompositionGraphicsDevice> _compositionGraphicsDevice;

    // For managing our event notifier
    EventRegistrationToken _deviceReplacedEventToken;

public:
    SampleText(IDWriteTextLayout* text, ICompositionGraphicsDevice* compositionGraphicsDevice) :
        _text(text),
        _compositionGraphicsDevice(compositionGraphicsDevice)
    {
        // Create the surface just big enough to hold the formatted text block.
        DWRITE_TEXT_METRICS metrics;
        FailFastOnFailure(text->GetMetrics(&metrics));
        Windows::Foundation::Size surfaceSize = { metrics.width, metrics.height };
        ComPtr<ICompositionDrawingSurface> drawingSurface;
        FailFastOnFailure(_compositionGraphicsDevice->CreateDrawingSurface(
            surfaceSize,
            DirectXPixelFormat::DirectXPixelFormat_B8G8R8A8UIntNormalized,
            DirectXAlphaMode::DirectXAlphaMode_Ignore,
            &drawingSurface));

        // Cache the interop pointer, since that's what we always use.
        FailFastOnFailure(drawingSurface.As(&_drawingSurfaceInterop));

        // Draw the text
        DrawText();

        // If the rendering device is lost, the application will recreate and replace it. We then
        // own redrawing our pixels.
        FailFastOnFailure(_compositionGraphicsDevice->add_RenderingDeviceReplaced(
            Callback<RenderingDeviceReplacedEventHandler>([this](
                ICompositionGraphicsDevice* source, IRenderingDeviceReplacedEventArgs* args)
                -> HRESULT
            {
                // Draw the text again
                DrawText();
                return S_OK;
            }).Get(),
            &_deviceReplacedEventToken));
    }

    ~SampleText()
    {
        FailFastOnFailure(_compositionGraphicsDevice->remove_RenderingDeviceReplaced(
            _deviceReplacedEventToken));
    }

    // Return the underlying surface to the caller
    ComPtr<ICompositionSurface> get_Surface()
    {
        // To the caller, the fact that we have a drawing surface is an implementation detail.
        // Return the base interface instead
        ComPtr<ICompositionSurface> surface;
        FailFastOnFailure(_drawingSurfaceInterop.As(&surface));
        return surface;
    }

private:
    // We may detect device loss on BeginDraw calls. This helper handles this condition or other
    // errors.
    bool CheckForDeviceRemoved(HRESULT hr)
    {
        if (SUCCEEDED(hr))
        {
            // Everything is fine -- go ahead and draw
            return true;
        }
        else if (hr == DXGI_ERROR_DEVICE_REMOVED)
        {
            // We can't draw at this time, but this failure is recoverable. Just skip drawing for
            // now. We will be asked to draw again once the Direct3D device is recreated
            return false;
        }
        else
        {
            // Any other error is unexpected and, therefore, fatal
            FailFast();
        }
    }

    // Renders the text into our composition surface
    void DrawText()
    {
        // Begin our update of the surface pixels. If this is our first update, we are required
        // to specify the entire surface, which nullptr is shorthand for (but, as it works out,
        // any time we make an update we touch the entire surface, so we always pass nullptr).
        ComPtr<ID2D1DeviceContext> d2dDeviceContext;
        POINT offset;
        if (CheckForDeviceRemoved(_drawingSurfaceInterop->BeginDraw(nullptr,
            __uuidof(ID2D1DeviceContext), &d2dDeviceContext, &offset)))
        {
            // Create a solid color brush for the text. A more sophisticated application might want
            // to cache and reuse a brush across all text elements instead, taking care to recreate
            // it in the event of device removed.
            ComPtr<ID2D1SolidColorBrush> brush;
            FailFastOnFailure(d2dDeviceContext->CreateSolidColorBrush(
                D2D1::ColorF(D2D1::ColorF::Black, 1.0f), &brush));

            // Draw the line of text at the specified offset, which corresponds to the top-left
            // corner of our drawing surface. Notice we don't call BeginDraw on the D2D device
            // context; this has already been done for us by the composition API.
            d2dDeviceContext->DrawTextLayout(D2D1::Point2F(offset.x, offset.y), _text.Get(),
                brush.Get());

            // Our update is done. EndDraw never indicates rendering device removed, so any
            // failure here is unexpected and, therefore, fatal.
            FailFastOnFailure(_drawingSurfaceInterop->EndDraw());
        }
    }
};

class SampleApp
{
    ComPtr<ICompositor> _compositor;
    ComPtr<ID2D1Device> _d2dDevice;
    ComPtr<ICompositionGraphicsDevice> _compositionGraphicsDevice;
    std::vector<ComPtr<SampleText>> _textSurfaces;

public:
    // Run once when the application starts up
    void Initialize(ICompositor* compositor)
    {
        // Cache the compositor (created outside of this method)
        _compositor = compositor;

        // Create a Direct2D device (helper implementation not shown here)
        FailFastOnFailure(CreateDirect2DDevice(&_d2dDevice));

        // To create a composition graphics device, we need to QI for another interface
        ComPtr<ICompositorInterop> compositorInterop;
        FailFastOnFailure(_compositor.As(&compositorInterop));

        // Create a graphics device backed by our D3D device
        FailFastOnFailure(compositorInterop->CreateGraphicsDevice(
            _d2dDevice.Get(),
            &_compositionGraphicsDevice));
    }

    // Called when Direct3D signals the device lost event
    void OnDirect3DDeviceLost()
    {
        // Create a new device
        FailFastOnFailure(CreateDirect2DDevice(_d2dDevice.ReleaseAndGetAddressOf()));

        // Restore our composition graphics device to good health
        ComPtr<ICompositionGraphicsDeviceInterop> compositionGraphicsDeviceInterop;
        FailFastOnFailure(_compositionGraphicsDevice.As(&compositionGraphicsDeviceInterop));
        FailFastOnFailure(compositionGraphicsDeviceInterop->SetRenderingDevice(_d2dDevice.Get()));
    }

    // Create a surface that is asynchronously filled with an image
    ComPtr<ICompositionSurface> CreateSurfaceFromTextLayout(IDWriteTextLayout* text)
    {
        // Create our wrapper object that will handle downloading and decoding the image (assume
        // throwing new here)
        SampleText* textSurface = new SampleText(text, _compositionGraphicsDevice.Get());

        // Keep our image alive
        _textSurfaces.push_back(textSurface);

        // The caller is only interested in the underlying surface
        return textSurface->get_Surface();
    }

    // Create a visual that holds an image
    ComPtr<IVisual> CreateVisualFromTextLayout(IDWriteTextLayout* text)
    {
        // Create a sprite visual
        ComPtr<ISpriteVisual> spriteVisual;
        FailFastOnFailure(_compositor->CreateSpriteVisual(&spriteVisual));

        // The sprite visual needs a brush to hold the image
        ComPtr<ICompositionSurfaceBrush> surfaceBrush;
        FailFastOnFailure(_compositor->CreateSurfaceBrushWithSurface(
            CreateSurfaceFromTextLayout(text).Get(),
            &surfaceBrush));

        // Associate the brush with the visual
        ComPtr<ICompositionBrush> brush;
        FailFastOnFailure(surfaceBrush.As(&brush));
        FailFastOnFailure(spriteVisual->put_Brush(brush.Get()));

        // Return the visual to the caller as the base class
        ComPtr<IVisual> visual;
        FailFastOnFailure(spriteVisual.As(&visual));

        return visual;
    }

private:
    // This helper (implementation not shown here) creates a Direct2D device and registers
    // for a device loss notification on the underlying Direct3D device. When that notification is
    // raised, assume the OnDirect3DDeviceLost method is called.
    HRESULT CreateDirect2DDevice(ID2D1Device** ppDevice);
};
```

 

 







<!--HONumber=Jun16_HO4-->


