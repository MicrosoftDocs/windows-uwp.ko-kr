---
title: 컴퍼지션 네이티브 상호 운용
description: Compositor API는 콘텐츠를 직접 이동 하는 데 사용할 수 있는 기본 상호 운용 인터페이스를 제공 합니다.
ms.date: 12/07/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.assetid: 16ad97eb-23f1-0264-23a9-a1791b4a5b95
ms.openlocfilehash: 4ae214f99cad9d8d2db644b184c12172a56a443b
ms.sourcegitcommit: 7d01748e3ef084fcf04cc0e2830c8ec66e8d1252
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/08/2020
ms.locfileid: "96849477"
---
# <a name="composition-native-interoperation-with-directx-and-direct2d"></a>DirectX 및 Direct2D와의 컴퍼지션 기본 상호 운용성

ICompositorInterop API는 콘텐츠를 compositor으로 직접 이동할 수 있도록 하는 [**ICompositorInterop**](/windows/desktop/api/windows.ui.composition.interop/nn-windows-ui-composition-interop-icompositorinterop), [**ICompositionDrawingSurfaceInterop**](/windows/desktop/api/windows.ui.composition.interop/nn-windows-ui-composition-interop-icompositiondrawingsurfaceinterop)및 [**ICompositionGraphicsDeviceInterop**](/windows/desktop/api/windows.ui.composition.interop/nn-windows-ui-composition-interop-icompositiongraphicsdeviceinterop) native 상호 운용 인터페이스를 제공 합니다.

네이티브 상호 운용성은 DirectX 질감으로 지원 되는 표면 개체를 중심으로 구성 됩니다. 표면은 [**CompositionGraphicsDevice**](/uwp/api/Windows.UI.Composition.CompositionGraphicsDevice)라는 팩터리 개체에서 만들어집니다. 이 개체는 서피스의 비디오 메모리를 할당 하는 데 사용 하는 기본 Direct2D 또는 Direct3D 장치 개체에 의해 지원 됩니다. 컴퍼지션 API는 기본 DirectX 장치를 만들지 않습니다. 응용 프로그램을 만들고 **CompositionGraphicsDevice** 개체에 전달 하는 것은 응용 프로그램의 책임입니다. 응용 프로그램은 한 번에 둘 이상의 **CompositionGraphicsDevice** 개체를 만들 수 있으며 여러 **CompositionGraphicsDevice** 개체에 대해 렌더링 장치와 동일한 DirectX 장치를 사용할 수 있습니다.

## <a name="creating-a-surface"></a>화면 만들기

각 [**CompositionGraphicsDevice**](/uwp/api/Windows.UI.Composition.CompositionGraphicsDevice) 는 표면 팩터리 역할을 합니다. 각 표면은 초기 크기 (0, 0)로 만들지만 유효한 픽셀은 만들 수 없습니다. 초기 상태의 표면은 시각적 트리에서 즉시 사용 될 수 있습니다. 예를 들어 [**CompositionSurfaceBrush**](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 및 [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual)를 사용 하는 경우에는 초기 상태에서 표면이 화면 출력에 영향을 주지 않습니다. 지정 된 알파 모드가 "불투명" 이더라도 완전히 투명 합니다.

경우에 따라 DirectX 장치를 렌더링 하지 못할 수 있습니다. 응용 프로그램이 잘못 된 인수를 특정 DirectX Api에 전달 하거나 시스템에서 그래픽 어댑터를 다시 설정 하거나 드라이버가 업데이트 되는 경우에 이러한 상황이 발생할 수 있습니다. Direct3D에는 어떤 이유로 든 장치를 분실 한 경우 응용 프로그램이 비동기적으로 검색 하는 데 사용할 수 있는 API가 있습니다. DirectX 장치를 분실 한 경우 응용 프로그램은 해당 장치를 삭제 하 고, 새 장치를 만들고, 잘못 된 DirectX 장치와 이전에 연결 된 [**CompositionGraphicsDevice**](/uwp/api/Windows.UI.Composition.CompositionGraphicsDevice) 개체에 전달 해야 합니다.

## <a name="loading-pixels-into-a-surface"></a>표면에 픽셀 로드

화면에 픽셀을 로드 하려면 응용 프로그램에서 요청 하는 내용에 따라 질감 또는 Direct2D 컨텍스트를 나타내는 DirectX 인터페이스를 반환 하는 [**Begindraw**](/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-begindraw) 메서드를 호출 해야 합니다. 그런 다음 응용 프로그램은 해당 질감에 픽셀을 렌더링 하거나 업로드 해야 합니다. 응용 프로그램이 완료 되 면 [**Enddraw**](/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-enddraw) 메서드를 호출 해야 합니다. 이 시점에는 컴포지션에 사용할 수 있는 새 픽셀만 사용할 수 있지만 다음에 시각적 트리에 대 한 모든 변경 내용이 커밋될 때까지 화면에 표시 되지 않습니다. **Enddraw** 를 호출 하기 전에 시각적 트리가 커밋되면 진행 중인 업데이트가 화면에 표시 되지 않으며 표면에는 **begindraw** 이전에 있던 내용이 계속 표시 됩니다. **Enddraw** 를 호출 하면 begindraw에서 반환 된 텍스처 또는 Direct2D context 포인터가 무효화 됩니다. 응용 프로그램은 **Enddraw** 호출을 초과 하는 포인터를 캐시 하면 안 됩니다.

응용 프로그램은 지정 된 [**CompositionGraphicsDevice**](/uwp/api/Windows.UI.Composition.CompositionGraphicsDevice)에 대해 한 번에 하나의 화면에서 begindraw만 호출할 수 있습니다. [**Begindraw**](/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-begindraw)를 호출한 후에는 다른 응용 프로그램에서 **begindraw** 를 호출 하기 전에 응용 프로그램에서 해당 화면에 [**enddraw**](/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-enddraw) 를 호출 해야 합니다. API가 민첩 하므로 응용 프로그램은 여러 작업자 스레드에서 렌더링을 수행 하려는 경우 이러한 호출을 동기화 해야 합니다. 응용 프로그램에서 한 화면 렌더링을 중단 하 고 일시적으로 다른 화면으로 전환 하려는 경우 응용 프로그램에서 [**SuspendDraw**](/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-suspenddraw) 메서드를 사용할 수 있습니다. 이렇게 하면 다른 **Begindraw** 가 성공 하지만 첫 번째 표면 업데이트를 화면 컴포지션에 사용할 수 없습니다. 이를 통해 응용 프로그램은 트랜잭션 방식으로 여러 업데이트를 수행할 수 있습니다. 표면이 일시 중단 되 면 응용 프로그램은 [**ResumeDraw**](/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-resumedraw) 메서드를 호출 하 여 업데이트를 계속 하거나 **enddraw** 를 호출 하 여 업데이트를 수행 하도록 선언할 수 있습니다. 즉, 지정 된 **CompositionGraphicsDevice** 한 번에 하나의 서피스만 적극적으로 업데이트 될 수 있습니다. 각 그래픽 장치는이 상태를 서로 독립적으로 유지 하므로 응용 프로그램이 서로 다른 그래픽 장치에 속한 경우 두 개의 서피스로 동시에 렌더링할 수 있습니다. 그러나 이렇게 하면 두 서피스의 비디오 메모리가 함께 풀링되지 않기 때문에 메모리 효율성이 떨어집니다.

[**Begindraw**](/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-begindraw), [**SuspendDraw**](/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-suspenddraw), [**ResumeDraw**](/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-resumedraw) 및 [**enddraw**](/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-enddraw) 메서드는 응용 프로그램이 잘못 된 작업을 수행 하는 경우 (예: 잘못 된 인수를 전달 하거나 다른 표면에서 **Enddraw** 를 호출 하기 전에 표면에서 **begindraw** 를 호출 하는 경우) 오류를 반환 합니다. 이러한 종류의 오류는 응용 프로그램 버그를 나타내며, 이러한 오류는 빠른 오류로 처리 되는 것으로 예상 됩니다. 기본 DirectX 장치를 분실 한 경우에도 **Begindraw** 에서 오류를 반환할 수 있습니다. 응용 프로그램에서 DirectX 장치를 다시 만들고 다시 시도할 수 있으므로이 오류는 치명적이 지 않습니다. 따라서 응용 프로그램은 단순히 렌더링을 생략 하 여 장치 손실을 처리할 것으로 예상 됩니다. 어떤 이유로 든 **Begindraw** 가 실패 하는 경우 첫 번째 위치가 시작 되지 않으므로 응용 프로그램에서 **enddraw** 를 호출 하지 않아야 합니다.

## <a name="scrolling"></a>스크롤

성능상의 이유로 응용 프로그램에서 [**Begindraw**](/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-begindraw) 를 호출 하면 반환 되는 질감의 내용이 표면의 이전 콘텐츠로 보장 되지 않습니다. 응용 프로그램은 콘텐츠가 무작위로 인 것으로 가정 해야 하며, 응용 프로그램은 렌더링 전에 화면을 지우거 나 업데이트 된 전체 사각형을 포함 하는 데 충분 한 불투명 내용을 그려 모든 픽셀에 대 한 작업을 수행 하도록 해야 합니다. 이는 텍스처 포인터가 **begindraw** 와 [**enddraw**](/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-enddraw) 호출 사이 에서만 유효 하다는 것과 결합 되어 응용 프로그램에서 이전 콘텐츠를 화면 밖으로 복사할 수 없게 합니다. 이러한 이유로 응용 프로그램이 동일한 표면 픽셀 복사를 수행할 수 있도록 하는 [**Scroll**](/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-scroll) 메서드를 제공 합니다.

## <a name="cwinrt-usage-example"></a>C + +/WinRT 사용 예

다음 코드 예제는 상호 운용 시나리오를 보여 줍니다. 이 예제에서는 Windows 컴퍼지션의 Windows 런타임 기반 노출 영역 및 interop 헤더의 형식과 함께 형식을 결합 하 고 COM 기반 DirectWrite 및 Direct2D Api를 사용 하 여 텍스트를 렌더링 하는 코드를 사용 합니다. 이 예제에서는 [**begindraw**](/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-begindraw) 와 [**enddraw**](/windows/desktop/api/windows.ui.composition.interop/nf-windows-ui-composition-interop-icompositiondrawingsurfaceinterop-enddraw) 를 사용 하 여 이러한 기술 간의 원활한 상호 운용성을 만듭니다. 이 예제에서는 DirectWrite를 사용 하 여 텍스트를 레이아웃 한 다음 Direct2D를 사용 하 여 렌더링 합니다. 컴퍼지션 그래픽 장치는 초기화 시에 직접 Direct2D 장치를 수락 합니다. 이를 통해 **begindraw** 은 **ID2D1DeviceContext** 인터페이스 포인터를 반환할 수 있습니다 .이는 응용 프로그램이 각 그리기 작업에서 반환 된 ID3D11Texture2D 인터페이스를 래핑하는 Direct2D 컨텍스트를 만드는 것 보다 훨씬 효율적입니다.

아래의 c + +/WinRT 코드 예제를 시험 하려면 먼저 Visual Studio에서 새 **핵심 앱 (c + +/winrt)** 프로젝트를 만듭니다. 요구 사항은 [c + +/vb에 대 한 Visual Studio 지원](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조 하세요. `pch.h`및 `App.cpp` 소스 코드 파일의 내용을 아래 코드 목록으로 바꾼 다음, 빌드 및 실행 합니다. 응용 프로그램은 "Hello, 세계!" 문자열을 렌더링 합니다. 투명 한 배경의 검정 텍스트입니다.

```cppwinrt
// pch.h
#pragma once
#include <windows.h>
#include <D2d1_1.h>
#include <D3d11_4.h>
#include <Dwrite.h>
#include <Windows.Graphics.DirectX.Direct3D11.interop.h>
#include <Windows.ui.composition.interop.h>
#include <unknwn.h>

#include <winrt/Windows.ApplicationModel.Core.h>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Graphics.DirectX.h>
#include <winrt/Windows.Graphics.DirectX.Direct3D11.h>
#include <winrt/Windows.UI.Composition.h>
#include <winrt/Windows.UI.Core.h>
#include <winrt/Windows.UI.Input.h>
```

```cppwinrt
// App.cpp
//*********************************************************
//
// Copyright (c) Microsoft. All rights reserved.
// This code is licensed under the MIT License (MIT).
// THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, 
// INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, 
// FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. 
// IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, 
// DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, 
// TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH 
// THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
//
//*********************************************************

#include "pch.h"

using namespace winrt;
using namespace winrt::Windows::ApplicationModel::Core;
using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::Foundation::Numerics;
using namespace winrt::Windows::Graphics::DirectX;
using namespace winrt::Windows::Graphics::DirectX::Direct3D11;
using namespace winrt::Windows::UI;
using namespace winrt::Windows::UI::Composition;
using namespace winrt::Windows::UI::Core;

namespace abi
{
    using namespace ABI::Windows::Foundation;
    using namespace ABI::Windows::Graphics::DirectX;
    using namespace ABI::Windows::UI::Composition;
}

// An app-provided helper to render lines of text.
struct SampleText
{
    SampleText(winrt::com_ptr<::IDWriteTextLayout> const& text, CompositionGraphicsDevice const& compositionGraphicsDevice) :
        m_text(text),
        m_compositionGraphicsDevice(compositionGraphicsDevice)
    {
        // Create the surface just big enough to hold the formatted text block.
        DWRITE_TEXT_METRICS metrics;
        winrt::check_hresult(m_text->GetMetrics(&metrics));
        winrt::Windows::Foundation::Size surfaceSize{ metrics.width, metrics.height };

        CompositionDrawingSurface drawingSurface{ m_compositionGraphicsDevice.CreateDrawingSurface(
            surfaceSize,
            DirectXPixelFormat::B8G8R8A8UIntNormalized,
            DirectXAlphaMode::Premultiplied) };

        // Cache the interop pointer, since that's what we always use.
        m_drawingSurfaceInterop = drawingSurface.as<abi::ICompositionDrawingSurfaceInterop>();

        // Draw the text
        DrawText();

        // If the rendering device is lost, the application will recreate and replace it. We then
        // own redrawing our pixels.
        m_deviceReplacedEventToken = m_compositionGraphicsDevice.RenderingDeviceReplaced(
            [this](CompositionGraphicsDevice const&, RenderingDeviceReplacedEventArgs const&)
            {
                // Draw the text again.
                DrawText();
                return S_OK;
            });
    }

    ~SampleText()
    {
        m_compositionGraphicsDevice.RenderingDeviceReplaced(m_deviceReplacedEventToken);
    }

    // Return the underlying surface to the caller.
    auto Surface()
    {
        // To the caller, the fact that we have a drawing surface is an implementation detail.
        // Return the base interface instead.
        return m_drawingSurfaceInterop.as<ICompositionSurface>();
    }

private:
    // The text to draw.
    winrt::com_ptr<::IDWriteTextLayout> m_text;

    // The composition surface that we use in the visual tree.
    winrt::com_ptr<abi::ICompositionDrawingSurfaceInterop> m_drawingSurfaceInterop;

    // The device that owns the surface.
    CompositionGraphicsDevice m_compositionGraphicsDevice{ nullptr };
    //winrt::com_ptr<abi::ICompositionGraphicsDevice> m_compositionGraphicsDevice2;

    // For managing our event notifier.
    winrt::event_token m_deviceReplacedEventToken;

    // We may detect device loss on BeginDraw calls. This helper handles this condition or other
    // errors.
    bool CheckForDeviceRemoved(HRESULT hr)
    {
        if (hr == S_OK)
        {
            // Everything is fine: go ahead and draw.
            return true;
        }

        if (hr == DXGI_ERROR_DEVICE_REMOVED)
        {
            // We can't draw at this time, but this failure is recoverable. Just skip drawing for
            // now. We will be asked to draw again once the Direct3D device is recreated.
            return false;
        }

        // Any other error is unexpected and, therefore, fatal.
        winrt::check_hresult(hr);
        return true;
    }

    // Renders the text into our composition surface
    void DrawText()
    {
        // Begin our update of the surface pixels. If this is our first update, we are required
        // to specify the entire surface, which nullptr is shorthand for (but, as it works out,
        // any time we make an update we touch the entire surface, so we always pass nullptr).
        winrt::com_ptr<::ID2D1DeviceContext> d2dDeviceContext;
        POINT offset;
        if (CheckForDeviceRemoved(m_drawingSurfaceInterop->BeginDraw(nullptr,
            __uuidof(ID2D1DeviceContext), d2dDeviceContext.put_void(), &offset)))
        {
            d2dDeviceContext->Clear(D2D1::ColorF(D2D1::ColorF::Black, 0.f));

            // Create a solid color brush for the text. A more sophisticated application might want
            // to cache and reuse a brush across all text elements instead, taking care to recreate
            // it in the event of device removed.
            winrt::com_ptr<::ID2D1SolidColorBrush> brush;
            winrt::check_hresult(d2dDeviceContext->CreateSolidColorBrush(
                D2D1::ColorF(D2D1::ColorF::Black, 1.0f), brush.put()));

            // Draw the line of text at the specified offset, which corresponds to the top-left
            // corner of our drawing surface. Notice we don't call BeginDraw on the D2D device
            // context; this has already been done for us by the composition API.
            d2dDeviceContext->DrawTextLayout(D2D1::Point2F((float)offset.x, (float)offset.y), m_text.get(),
                brush.get());

            // Our update is done. EndDraw never indicates rendering device removed, so any
            // failure here is unexpected and, therefore, fatal.
            winrt::check_hresult(m_drawingSurfaceInterop->EndDraw());
        }
    }
};

struct DeviceLostEventArgs
{
    DeviceLostEventArgs(IDirect3DDevice const& device) : m_device(device) {}
    IDirect3DDevice Device() { return m_device; }
    static DeviceLostEventArgs Create(IDirect3DDevice const& device) { return DeviceLostEventArgs{ device }; }

private:
    IDirect3DDevice m_device;
};

struct DeviceLostHelper
{
    DeviceLostHelper() = default;

    ~DeviceLostHelper()
    {
        StopWatchingCurrentDevice();
        m_onDeviceLostHandler = nullptr;
    }

    IDirect3DDevice CurrentlyWatchedDevice() { return m_device; }

    void WatchDevice(winrt::com_ptr<::IDXGIDevice> const& dxgiDevice)
    {
        // If we're currently listening to a device, then stop.
        StopWatchingCurrentDevice();

        // Set the current device to the new device.
        m_device = nullptr;
        winrt::check_hresult(::CreateDirect3D11DeviceFromDXGIDevice(dxgiDevice.get(), reinterpret_cast<::IInspectable**>(winrt::put_abi(m_device))));

        // Get the DXGI Device.
        m_dxgiDevice = dxgiDevice;

        // QI For the ID3D11Device4 interface.
        winrt::com_ptr<::ID3D11Device4> d3dDevice{ m_dxgiDevice.as<::ID3D11Device4>() };

        // Create a wait struct.
        m_onDeviceLostHandler = nullptr;
        m_onDeviceLostHandler = ::CreateThreadpoolWait(DeviceLostHelper::OnDeviceLost, (PVOID)this, nullptr);

        // Create a handle and a cookie.
        m_eventHandle.attach(::CreateEvent(nullptr, false, false, nullptr));
        winrt::check_bool(bool{ m_eventHandle });
        m_cookie = 0;

        // Register for device lost.
        ::SetThreadpoolWait(m_onDeviceLostHandler, m_eventHandle.get(), nullptr);
        winrt::check_hresult(d3dDevice->RegisterDeviceRemovedEvent(m_eventHandle.get(), &m_cookie));
    }

    void StopWatchingCurrentDevice()
    {
        if (m_dxgiDevice)
        {
            // QI For the ID3D11Device4 interface.
            auto d3dDevice{ m_dxgiDevice.as<::ID3D11Device4>() };

            // Unregister from the device lost event.
            ::CloseThreadpoolWait(m_onDeviceLostHandler);
            d3dDevice->UnregisterDeviceRemoved(m_cookie);

            // Clear member variables.
            m_onDeviceLostHandler = nullptr;
            m_eventHandle.close();
            m_cookie = 0;
            m_device = nullptr;
        }
    }

    void DeviceLost(winrt::delegate<DeviceLostHelper const*, DeviceLostEventArgs const&> const& handler)
    {
        m_deviceLost = handler;
    }

    winrt::delegate<DeviceLostHelper const*, DeviceLostEventArgs const&> m_deviceLost;

private:
    void RaiseDeviceLostEvent(IDirect3DDevice const& oldDevice)
    {
        m_deviceLost(this, DeviceLostEventArgs::Create(oldDevice));
    }

    static void CALLBACK OnDeviceLost(PTP_CALLBACK_INSTANCE /* instance */, PVOID context, PTP_WAIT /* wait */, TP_WAIT_RESULT /* waitResult */)
    {
        auto deviceLostHelper = reinterpret_cast<DeviceLostHelper*>(context);
        auto oldDevice = deviceLostHelper->m_device;
        deviceLostHelper->StopWatchingCurrentDevice();
        deviceLostHelper->RaiseDeviceLostEvent(oldDevice);
    }

private:
    IDirect3DDevice m_device;
    winrt::com_ptr<::IDXGIDevice> m_dxgiDevice;
    PTP_WAIT m_onDeviceLostHandler{ nullptr };
    winrt::handle m_eventHandle;
    DWORD m_cookie{ 0 };
};

struct SampleApp : implements<SampleApp, IFrameworkViewSource, IFrameworkView>
{
    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const&)
    {
    }

    // Run once when the application starts up
    void Initialize()
    {
        // Create a Direct2D device.
        CreateDirect2DDevice();

        // To create a composition graphics device, we need to QI for another interface
        winrt::com_ptr<abi::ICompositorInterop> compositorInterop{ m_compositor.as<abi::ICompositorInterop>() };

        // Create a graphics device backed by our D3D device
        winrt::com_ptr<abi::ICompositionGraphicsDevice> compositionGraphicsDeviceIface;
        winrt::check_hresult(compositorInterop->CreateGraphicsDevice(
            m_d2dDevice.get(),
            compositionGraphicsDeviceIface.put()));
        m_compositionGraphicsDevice = compositionGraphicsDeviceIface.as<CompositionGraphicsDevice>();
    }

    void Load(hstring const&)
    {
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        CoreWindow window = CoreWindow::GetForCurrentThread();
        window.Activate();

        CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const& window)
    {
        m_compositor = Compositor{};
        m_target = m_compositor.CreateTargetForCurrentView();
        ContainerVisual root = m_compositor.CreateContainerVisual();
        m_target.Root(root);

        Initialize();

        winrt::check_hresult(
            ::DWriteCreateFactory(
                DWRITE_FACTORY_TYPE_SHARED,
                __uuidof(m_dWriteFactory),
                reinterpret_cast<::IUnknown**>(m_dWriteFactory.put())
            )
        );

        winrt::check_hresult(
            m_dWriteFactory->CreateTextFormat(
                L"Segoe UI",
                nullptr,
                DWRITE_FONT_WEIGHT_REGULAR,
                DWRITE_FONT_STYLE_NORMAL,
                DWRITE_FONT_STRETCH_NORMAL,
                36.f,
                L"en-US",
                m_textFormat.put()
            )
        );

        Rect windowBounds{ window.Bounds() };
        std::wstring text{ L"Hello, World!" };

        winrt::check_hresult(
            m_dWriteFactory->CreateTextLayout(
                text.c_str(),
                (uint32_t)text.size(),
                m_textFormat.get(),
                windowBounds.Width,
                windowBounds.Height,
                m_textLayout.put()
            )
        );

        Visual textVisual{ CreateVisualFromTextLayout(m_textLayout) };
        textVisual.Size({ windowBounds.Width, windowBounds.Height });
        root.Children().InsertAtTop(textVisual);
    }

    // Called when Direct3D signals the device lost event.
    void OnDirect3DDeviceLost(DeviceLostHelper const* /* sender */, DeviceLostEventArgs const& /* args */)
    {
        // Create a new Direct2D device.
        CreateDirect2DDevice();

        // Restore our composition graphics device to good health.
        winrt::com_ptr<abi::ICompositionGraphicsDeviceInterop> compositionGraphicsDeviceInterop{ m_compositionGraphicsDevice.as<abi::ICompositionGraphicsDeviceInterop>() };
        winrt::check_hresult(compositionGraphicsDeviceInterop->SetRenderingDevice(m_d2dDevice.get()));
    }

    // Create a surface that is asynchronously filled with an image
    ICompositionSurface CreateSurfaceFromTextLayout(winrt::com_ptr<::IDWriteTextLayout> const& text)
    {
        // Create our wrapper object that will handle downloading and decoding the image (assume
        // throwing new here).
        SampleText textSurface{ text, m_compositionGraphicsDevice };

        // The caller is only interested in the underlying surface.
        return textSurface.Surface();
    }

    // Create a visual that holds an image.
    Visual CreateVisualFromTextLayout(winrt::com_ptr<::IDWriteTextLayout> const& text)
    {
        // Create a sprite visual
        SpriteVisual spriteVisual{ m_compositor.CreateSpriteVisual() };

        // The sprite visual needs a brush to hold the image.
        CompositionSurfaceBrush surfaceBrush{
            m_compositor.CreateSurfaceBrush(CreateSurfaceFromTextLayout(text))
        };

        // Associate the brush with the visual.
        CompositionBrush brush{ surfaceBrush.as<CompositionBrush>() };
        spriteVisual.Brush(brush);

        // Return the visual to the caller as an IVisual.
        return spriteVisual;
    }

private:
    CompositionTarget m_target{ nullptr };
    Compositor m_compositor{ nullptr };
    winrt::com_ptr<::ID2D1Device> m_d2dDevice;
    winrt::com_ptr<::IDXGIDevice> m_dxgiDevice;
    //winrt::com_ptr<abi::ICompositionGraphicsDevice> m_compositionGraphicsDevice;
    CompositionGraphicsDevice m_compositionGraphicsDevice{ nullptr };
    std::vector<SampleText> m_textSurfaces;
    DeviceLostHelper m_deviceLostHelper;
    winrt::com_ptr<::IDWriteFactory> m_dWriteFactory;
    winrt::com_ptr<::IDWriteTextFormat> m_textFormat;
    winrt::com_ptr<::IDWriteTextLayout> m_textLayout;


    // This helper creates a Direct2D device, and registers for a device loss
    // notification on the underlying Direct3D device. When that notification is
    // raised, the OnDirect3DDeviceLost method is called.
    void CreateDirect2DDevice()
    {
        uint32_t createDeviceFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

        // Array with DirectX hardware feature levels in order of preference.
        D3D_FEATURE_LEVEL featureLevels[] =
        {
            D3D_FEATURE_LEVEL_11_1,
            D3D_FEATURE_LEVEL_11_0,
            D3D_FEATURE_LEVEL_10_1,
            D3D_FEATURE_LEVEL_10_0,
            D3D_FEATURE_LEVEL_9_3,
            D3D_FEATURE_LEVEL_9_2,
            D3D_FEATURE_LEVEL_9_1
        };

        // Create the Direct3D 11 API device object and a corresponding context.
        winrt::com_ptr<::ID3D11Device> d3DDevice;
        winrt::com_ptr<::ID3D11DeviceContext> d3DImmediateContext;
        D3D_FEATURE_LEVEL d3dFeatureLevel{ D3D_FEATURE_LEVEL_9_1 };

        winrt::check_hresult(
            ::D3D11CreateDevice(
                nullptr, // Default adapter.
                D3D_DRIVER_TYPE_HARDWARE,
                0, // Not asking for a software driver, so not passing a module to one.
                createDeviceFlags, // Set debug and Direct2D compatibility flags.
                featureLevels,
                ARRAYSIZE(featureLevels),
                D3D11_SDK_VERSION,
                d3DDevice.put(),
                &d3dFeatureLevel,
                d3DImmediateContext.put()
            )
        );

        // Initialize Direct2D resources.
        D2D1_FACTORY_OPTIONS d2d1FactoryOptions{ D2D1_DEBUG_LEVEL_NONE };

        // Initialize the Direct2D Factory.
        winrt::com_ptr<::ID2D1Factory1> d2D1Factory;
        winrt::check_hresult(
            ::D2D1CreateFactory(
                D2D1_FACTORY_TYPE_SINGLE_THREADED,
                __uuidof(d2D1Factory),
                &d2d1FactoryOptions,
                d2D1Factory.put_void()
            )
        );

        // Create the Direct2D device object.
        // Obtain the underlying DXGI device of the Direct3D device.
        m_dxgiDevice = d3DDevice.as<::IDXGIDevice>();

        m_d2dDevice = nullptr;
        winrt::check_hresult(
            d2D1Factory->CreateDevice(m_dxgiDevice.get(), m_d2dDevice.put())
        );

        m_deviceLostHelper.WatchDevice(m_dxgiDevice);
        m_deviceLostHelper.DeviceLost({ this, &SampleApp::OnDirect3DDeviceLost });
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<SampleApp>());
}
```

## <a name="ccx-usage-example"></a>C + +/CX 사용 예제

> [!NOTE]
> 이 코드 예제는 c + +/CX 응용 프로그램을 유지 관리 하는 데 도움이 됩니다. 하지만 새로운 응용 프로그램에 대해 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)를 사용하는 것이 좋습니다. C++/WinRT는 헤더 파일 기반 라이브러리로 구현된 WinRT(Windows 런타임) API용 최신의 완전한 표준 C++17 언어 프로젝션이며, 최신 Windows API에 최고 수준의 액세스를 제공하도록 설계되었습니다.

아래의 c + +/CX 코드 예제에서는 예제의 DirectWrite 및 Direct2D 부분을 생략 합니다.

```cppcx
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
