---
ms.assetid: 16ad97eb-23f1-0264-23a9-a1791b4a5b95
title: 컴퍼지션 네이티브 상호 운용
description: Windows.UI.Composition API는 콘텐츠를 작성자로 직접 이동할 수 있는 네이티브 상호 운용 인터페이스를 제공합니다.
ms.date: 06/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2f5eabfaf8e526899c17b2d0da688a5135133f65
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8745224"
---
# <a name="composition-native-interoperation-with-directx-and-direct2d"></a>DirectX 및 Direct2D를 사용하여 컴퍼지션 네이티브 상호 운용

Windows.UI.Composition API는 콘텐츠를 작성자로 직접 이동할 수 있는 [**ICompositorInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620068), [**ICompositionDrawingSurfaceInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620058), 및 [**ICompositionGraphicsDeviceInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620065) 네이티브 상호 운용 인터페이스를 제공합니다.

네이티브 상호 운용은 DirectX 텍스처가 지원되는 표면 개체 주위로 구조화됩니다. 표면은 [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749)라는 팩터리 개체에서 만들어집니다. 이 개체는 기본 Direct3D 또는 Direct2D 장치 개체에 의해 지원되며 표면의 비디오 메모리를 할당하는 데 사용됩니다. 컴퍼지션 API는 기본 DirectX 장치를 만들지 않습니다. 기본 DirectX 장치를 만들어 **CompositionGraphicsDevice** 개체로 전달하는 것은 응용 프로그램에서 담당합니다. 응용 프로그램은 한 번에 둘 이상의 **CompositionGraphicsDevice** 개체를 만들 수 있으며, 여러 **CompositionGraphicsDevice** 개체에 대한 렌더링 장치로 동일한 DirectX 장치를 사용할 수 있습니다.

## <a name="creating-a-surface"></a>표면 만들기

각 [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749)는 표면 팩터리 역할을 합니다. 각 표면은 만들어질 때 초기 크기(0,0이 될 수 있음)가 설정되며 유효 픽셀은 없습니다. 초기 상태의 표면은 시각적 트리에서 즉시 사용될 수 있지만(예를 들어 [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) 및 [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433)을 통해) 초기 상태의 표면은 화면 출력에 영향을 주지 않습니다. 이 표면은 완전히 투명하며 지정된 알파 모드가 "불투명"인 경우에도 마찬가지입니다.

경우에 따라 DirectX 장치를 사용할 수 없도록 렌더링될 수 있습니다. 이러한 문제가 발생하는 원인은 대부분 응용 프로그램에서 특정 DirectX API에 잘못된 인수를 전달하거나, 시스템에 의해 그래픽 어댑터가 초기화되거나, 드라이버가 업데이트되었기 때문입니다. Direct3D에는 어떤 이유로든 장치가 분실된 경우 응용 프로그램에서 비동기적으로 검색하는 데 사용할 수 있는 API가 있습니다. DirectX 장치가 분실된 경우 응용 프로그램은 이를 삭제하고 새로 만든 다음 잘못된 DirectX 장치에 이전에 연결되었던 모든 [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) 개체에 전달해야 합니다.

## <a name="loading-pixels-into-a-surface"></a>표면에 픽셀 로드

표면에 픽셀을 로드하려면 응용 프로그램의 요청에 따라 Direct2D 컨텍스트 또는 텍스처를 나타내는 DirectX 인터페이스를 반환하는 [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx) 메서드를 응용 프로그램에서 호출해야 합니다. 그런 다음 응용 프로그램은 픽셀을 해당 텍스처로 렌더링하거나 업로드해야 합니다. 응용 프로그램이 완료되면 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) 메서드를 호출해야 합니다. 이 시점에서만 컴퍼지션에서 새 픽셀을 사용할 수 있지만 다음에 시각적 트리에 대한 모든 변경 사항이 커밋될 때까지 화면에 표시되지 않습니다. **EndDraw**가 호출되기 전에 시각적 트리가 커밋되면 진행 중인 업데이트가 화면에 표시되지 않고 표면에 **BeginDraw** 전의 콘텐츠가 계속 표시됩니다. **EndDraw**가 호출되면 BeginDraw에서 반환한 텍스처 또는 Direct2D 컨텍스트 포인터가 무효화됩니다. **EndDraw** 호출 후에는 응용 프로그램에서 해당 포인터를 캐시할 수 없습니다.

응용 프로그램에서는 지정된 [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749)에 대해 한 번에 한 표면에서만 BeginDraw를 호출할 수 있습니다. [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx) 호출 후 응용 프로그램은 해당 표면에서 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060)를 호출한 다음 다른 표면에서 **BeginDraw**를 호출해야 합니다. API는 Agile이므로 여러 작업자 스레드에서 렌더링을 수행하려면 응용 프로그램에서 이러한 호출을 동기화합니다. 응용 프로그램에서 한 표면의 렌더링을 중단하고 임시로 다른 표면으로 전환하려는 경우 [**SuspendDraw**](https://msdn.microsoft.com/library/windows/apps/mt620064.aspx) 메서드를 사용할 수 있습니다. 이렇게 하면 다른 **BeginDraw**가 성공하지만 화면 컴퍼지션에서 첫 번째 표면 업데이트를 사용할 수 없습니다. 이 경우 응용 프로그램에서 트랜잭션 방식으로 여러 업데이트를 수행할 수 있습니다. 표면이 일시 중단되면 응용 프로그램에서 [**ResumeDraw**](https://msdn.microsoft.com/library/windows/apps/mt620062) 메서드를 호출하여 업데이트를 계속 진행하거나 **EndDraw**를 호출하여 해당 업데이트가 완료된 것으로 선언할 수 있습니다. 즉, 지정된 **CompositionGraphicsDevice**에 대해 한 번에 한 표면만 업데이트할 수 있습니다. 각 그래픽 장치는 상호 독립적으로 이 상태를 유지하므로 표면이 서로 다른 그래픽 장치에 속한 경우 응용 프로그램에서 두 표면을 동시에 렌더링할 수 있습니다. 그러나 이러한 두 표면의 비디오 메모리를 함께 풀링할 수 없으므로 메모리 효율성이 떨어집니다.

응용 프로그램에서 잘못된 인수를 전달하거나 한 표면에서 **EndDraw**를 호출하기 전에 다른 표면에서 **BeginDraw**를 호출하는 등 잘못된 작업을 수행한 경우 [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx), [**SuspendDraw**](https://msdn.microsoft.com/library/windows/apps/mt620064.aspx), [**ResumeDraw**](https://msdn.microsoft.com/library/windows/apps/mt620062) 및 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) 메서드에서 오류를 반환합니다. 이러한 오류는 응용 프로그램 버그를 의미하므로 빠른 오류로 처리되어야 합니다. 기본 DirectX 장치가 분실된 경우에도 **BeginDraw**에서 오류를 반환할 수 있습니다. 응용 프로그램에서 해당 DirectX 장치를 다시 만든 다음 다시 시도할 수 있으므로 이 오류는 치명적이지 않습니다. 따라서 응용 프로그램은 렌더링을 생략하여 장치 분실을 처리해야 합니다. 어떤 이유로든 **BeginDraw**가 실패한 경우 시작되지 않았으므로 응용 프로그램에서 **EndDraw**를 호출해서는 안 됩니다.

## <a name="scrolling"></a>스크롤

성능상의 이유로 응용 프로그램에서 [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx)를 호출하는 경우 반환된 텍스처의 콘텐츠가 반드시 표면의 이전 콘텐츠라는 보장이 없습니다. 응용 프로그램에서는 콘텐츠가 임의적이라는 것을 감안하여 렌더링 전에 표면을 지우거나 업데이트된 사각형이 모두 채워지도록 충분히 불투명한 콘텐츠를 그려 모든 픽셀이 터치되도록 해야 합니다. 그 외에 **BeginDraw** 및 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) 호출 사이에서만 텍스처 포인터가 유효하므로 응용 프로그램이 표면 외부에서 이전 콘텐츠를 복사할 수 없습니다. 이러한 이유로 Microsoft에서는 응용 프로그램에서 동일 표면 픽셀 복사를 수행할 수 있는 [**Scroll**](https://msdn.microsoft.com/library/windows/apps/mt620063) 메서드를 제공합니다.

## <a name="usage-example"></a>사용 예제

다음 코드 예제는 상호 운용 시나리오를 보여 줍니다. 이 예제에서는 interop 머리글과 COM 기반 DirectWrite 및 Direct2D Api를 사용 하 여 텍스트를 렌더링 하는 코드의 종류와 함께 Windows 컴퍼지션의 Windows 런타임 기반 노출 영역에서 형식을 결합 합니다. 이 예제에서는 [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx) 및 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) 를 사용 하 여 원활한 이러한 기술 간의 상호 작용할 수 있도록 합니다. 이 예제에서는 DirectWrite를 사용 하 여 텍스트를 배치 하 고 Direct2D 렌더링을 사용 합니다. 컴퍼지션 그래픽 장치는 초기화 시 직접 Direct2D 장치를 사용합니다. 이렇게 하면 **BeginDraw** 이 응용 프로그램 각 그리기 작업에서 반환 된 ID3D11Texture2D 인터페이스를 래핑하는 Direct2D 컨텍스트를 만들어 보다 훨씬 더 효율적는 **ID2D1DeviceContext** 인터페이스 포인터를 반환할 수 있습니다.

아래의 코드 예제 2 가지 있습니다. 첫 번째는 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 예제입니다 (완료) 하 여 다음 C + + (이 예제에서는 DirectWrite 및 Direct2D 부분을 생략)는 CX 코드 예제입니다.

사용 하 여 C + + WinRT 아래 코드 예제에서는 먼저 새 만듭니다 **Core App (C + + WinRT)** Visual Studio에서 프로젝트 (요구 사항에 대 한 참조 [Visual Studio 지원 C + + /winrt 및 VSIX](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-and-the-vsix)). 프로젝트를 만드는 동안 대상 버전 **Windows 10, 버전 1803 (10.0;으로 선택 빌드 17134)**. 이 코드는에 대 한 빌드 및 테스트의 버전입니다. 내용을 `App.cpp` 소스 코드 파일, 아래 코드 목록을 사용 하 여 다음 빌드 및 실행 합니다. 응용 프로그램이 렌더링 문자열 "Hello, World!" 투명 배경에서 검은색 텍스트입니다.

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

#include <D2d1_1.h>
#include <D3d11_4.h>
#include <Dwrite.h>
#include <Windows.Graphics.DirectX.Direct3D11.interop.h>
#include <Windows.ui.composition.interop.h>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Graphics.DirectX.h>
#include <winrt/Windows.Graphics.DirectX.Direct3D11.h>
#include <winrt/Windows.UI.Composition.h>

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
        m_eventHandle = ::CreateEvent(nullptr, false, false, nullptr);
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

    void Initialize(CoreApplicationView const &)
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
    CoreApplication::Run(SampleApp());
}
```

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
