---
description: 전체 Direct2D 코드 예제를 사용하여 C++/WinRT를 통해 COM 클래스 및 인터페이스를 사용하는 방법을 보여 줍니다.
title: C++/WinRT로 작성된 COM 구성 요소 사용
ms.date: 07/23/2018
ms.topic: article
keywords: windows 10, uwp, standard, c + +, cpp, winrt, COM 구성 요소, 클래스, 인터페이스
ms.localizationpriority: medium
ms.openlocfilehash: 16425fd6d296a4abd4ed62c0c64cd23ef1f88891
ms.sourcegitcommit: 9031a51f9731f0b675769e097aa4d914b4854e9e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58618410"
---
# <a name="consume-com-components-with-cwinrt"></a>C++/WinRT로 작성된 COM 구성 요소 사용

기능을 사용할 수는 [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) DirectX Api의 고성능 2d 및 3d 그래픽 등의 COM 구성 요소를 사용 하는 라이브러리입니다. C++/ WinRT는 DirectX를 사용 하 여 성능 저하 없이 대 한 가장 간단한 방법입니다. 이 항목에서는 Direct2D 코드 예제를 사용 하 여 C +를 사용 하는 방법을 + WinRT COM 클래스 및 인터페이스를 사용 하도록 합니다. 동일한 COM 및 Windows 런타임 프로그래밍 물론 혼합할 수 있습니다 C++/WinRT 프로젝트입니다.

이 항목의 끝 최소 Direct2D 응용 프로그램의 전체 소스 코드 샘플을 찾을 수 있습니다. 해당 코드에서 발췌 한 내용이 리프트 하 고 사용 하 여 C +를 사용 하 여 COM 구성 요소를 사용 하는 방법을 보여 드리겠습니다 + C +의 다양 한 기능을 사용 하 여 WinRT + WinRT 라이브러리입니다.

## <a name="com-smart-pointers-winrtcomptruwpcpp-ref-for-winrtcom-ptr"></a>COM 스마트 포인터 ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

COM을 사용 하 여 프로그래밍 인터페이스를 사용 하 여 직접 아닌 (하는 개체의 COM의 진화 된 Windows 런타임 Api에 대 한 백그라운드 true도)를 사용 하 여 작업 합니다. COM 클래스에는 함수를 호출 하려면 예를 들어, 활성화 하면 클래스는 인터페이스를 다시 가져오고 해당 인터페이스에서 함수를 호출 합니다. 개체의 상태에 액세스 하려면 없는 멤버에 액세스 하는 데이터 직접; 대신, 인터페이스의 접근자와 변경자 (mutator) 함수를 호출합니다.

더 구체적으로, 우리가 이야기할 인터페이스와 상호 작용 *포인터*합니다. 이 위해에서는 혜택도에서 COM 스마트 포인터 형식과의 존재 여부 C++/WinRT&mdash;는 [ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr) 형식입니다.

```cppwinrt
#include <d2d1_1.h>
...
winrt::com_ptr<ID2D1Factory1> factory;
```

위의 코드는 초기화 되지 않은 스마트 포인터를 선언 하는 방법을 보여 줍니다는 [ **ID2D1Factory1** ](https://msdn.microsoft.com/library/Hh404596) COM 인터페이스입니다. 아직 가리키는 스마트 포인터가 초기화 되었으므로 **ID2D1Factory1** (해당 가리키고 있지 않으면 인터페이스 전혀) 실제 개체에 속하는 인터페이스입니다. 이렇게; 가능성이 되었지만 및 COM 참조를 가리키는 인터페이스의 소유 하는 개체의 수명을 관리 하는 데 해당 인터페이스 함수를 호출 하는 미디어 수 계산을 통해 수 있는지 (스마트 포인터 중).

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>인터페이스 포인터를 반환 하는 COM 함수 **void**

호출할 수 있습니다 합니다 [ **com_ptr::put_void** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) 함수는 초기화 되지 않은 스마트 포인터에 대 한 쓰기를 원시 포인터 원본으로 사용 합니다.

```cppwinrt
D2D1_FACTORY_OPTIONS options{ D2D1_DEBUG_LEVEL_NONE };
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

호출 위의 코드는 [ **D2D1CreateFactory** ](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory) 반환 하는 함수를 **ID2D1Factory1** 인터페이스 포인터에는 마지막 매개 변수인 통해 **void \* \***  형식입니다. 많은 COM 함수는 반환 된 **void\*\*** 합니다. 이러한 함수를 사용 하 여 [ **com_ptr::put_void** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) 같이 합니다.

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>특정 인터페이스 포인터를 반환 하는 COM 함수

합니다 [ **D3D11CreateDevice** ](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 함수에서 반환 된 [ **ID3D11Device** ](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) 있는 해당 마지막에서 세 번째 매개 변수를 통해 인터페이스 포인터 **ID3D11Device\* \***  형식입니다. 특정 인터페이스 포인터를 반환 하는 함수를 사용 하 여 [ **com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function)합니다.

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

전에 원시 호출 하는 방법을 보여 줍니다이 섹션의 코드 예제 **D2D1CreateFactory** 함수입니다. 이 항목에 대 한 코드 예제에서는 호출 하면 사실 이지만 **D2D1CreateFactory**, 원시 API를 래핑하는 도우미 함수 템플릿을 사용 하 여 및 코드 예제에서는 실제로 사용 하므로 [ **com_ptr::put** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function).

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>인터페이스 포인터를 반환 하는 COM 함수 **IUnknown**

합니다 [ **DWriteCreateFactory** ](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) 함수에는 마지막 매개 변수인 통해 DirectWrite 팩터리 인터페이스 포인터를 반환 [ **IUnknown** ](https://msdn.microsoft.com/library/windows/desktop/ms680509)형식입니다. 이러한 함수를 사용 하 여 [ **com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function), 하지만 캐스팅 재해석 **IUnknown**합니다.

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcomptr"></a>다시 사용자는 **winrt::com_ptr**

> [!IMPORTANT]
> 있는 경우는 [ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr) 이미 장착 되는 (해당 내부 원시 포인터가 이미 대상) 다른 개체를 가리키도록 재연결을 하 고 할당 하려면 먼저 `nullptr` 되도록&mdash;아래 코드 예제에 표시 된 대로 합니다. 그렇지 않으면 다음을 이미-장착 **com_ptr** 주의 문제 그려집니다 (호출 하는 경우 [ **com_ptr::put** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function) 하거나 [ **com_ptr:: put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)) 해당 내부 포인터가 null이 아닌 어설션하 여 합니다.

```cppwinrt
winrt::com_ptr<ID2D1SolidColorBrush> brush;
...
    brush.put()
...
brush = nullptr; // Important because we're about to re-seat
target->CreateSolidColorBrush(
    color_orange,
    D2D1::BrushProperties(0.8f),
    brush.put()));
```

## <a name="handle-hresult-error-codes"></a>HRESULT 오류 코드를 처리 합니다.

COM 함수에서 반환 되는 HRESULT의 값을 확인 하 고 이벤트의 오류 코드를 나타내는 예외를 throw 하려면 호출 [ **winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)합니다.

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>특정 인터페이스 포인터를 사용 하는 COM 함수

호출할 수 있습니다는 [ **com_ptr::get** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function) 전달 하는 함수에 **com_ptr** 동일한 형식의 특정 인터페이스 포인터를 사용 하는 함수를 합니다.

```cppwinrt
... ExampleFunction(
    winrt::com_ptr<ID2D1Factory1> const& factory,
    winrt::com_ptr<IDXGIDevice> const& dxdevice)
{
    ...
    winrt::check_hresult(factory->CreateDevice(dxdevice.get(), ...));
    ...
}
```

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>COM 사용 하는 함수는 **IUnknown** 인터페이스 포인터

호출할 수 있습니다는 [ **winrt::get_unknown** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#get_unknown-function) 전달할 함수를 사용 가능한 사용자 **com_ptr** 사용 하는 함수에는 **IUnknown** 인터페이스 포인터입니다.

```cppwinrt
winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
    ...
    winrt::get_unknown(CoreWindow::GetForCurrentThread()),
    ...));
```

## <a name="passing-and-returning-com-smart-pointers"></a>전달 및 반환 COM 스마트 포인터

형식의 COM 스마트 포인터를 수행 하는 함수를 **winrt::com_ptr** 또는 참조에 대 한 상수 참조로 수행 해야 합니다.

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

반환 하는 함수를 **winrt::com_ptr** 값으로 이렇게 해야 합니다.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>다른 인터페이스에 대 한 쿼리는 COM 스마트 포인터

사용할 수는 [ **com_ptr::as** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function) 다른 인터페이스에 대 한 COM 스마트 포인터를 쿼리 하는 함수입니다. 쿼리 성공 하지 못하면 예외를 throw 하는 함수입니다.

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

또는 사용 하 여 [ **com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrtry_as-function)를 검사할 수 있는 값을 반환 하는 `nullptr` 쿼리 성공 했는지 여부를 확인 합니다.

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>최소 Direct2D 응용 프로그램의 전체 소스 코드 목록

빌드 및 Visual Studio에서이 원본 코드 예제에서는 먼저 실행 하려는 경우 새로 만듭니다 **Core 앱 (C++/WinRT)** 합니다. `Direct2D` 프로젝트에 대 한 적절 한 이름 이지만 사용자가 원하는 대로 이름을 지정할 수 있습니다. 열기 `App.cpp`, 전체 콘텐츠를 삭제 하 고 아래 목록에 붙여 넣습니다.

```cppwinrt
#include "pch.h"
#include <d2d1_1.h>
#include <d3d11.h>
#include <dxgi1_2.h>
#include <winrt/Windows.Graphics.Display.h>

using namespace winrt;

using namespace Windows;
using namespace Windows::ApplicationModel::Core;
using namespace Windows::UI;
using namespace Windows::UI::Core;
using namespace Windows::Graphics::Display;

namespace
{
    winrt::com_ptr<ID2D1Factory1> CreateFactory()
    {
        D2D1_FACTORY_OPTIONS options{};

#ifdef _DEBUG
        options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

        winrt::com_ptr<ID2D1Factory1> factory;

        winrt::check_hresult(D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            options,
            factory.put()));

        return factory;
    }

    HRESULT CreateDevice(D3D_DRIVER_TYPE const type, winrt::com_ptr<ID3D11Device>& device)
    {
        WINRT_ASSERT(!device);

        return D3D11CreateDevice(
            nullptr,
            type,
            nullptr,
            D3D11_CREATE_DEVICE_BGRA_SUPPORT,
            nullptr, 0,
            D3D11_SDK_VERSION,
            device.put(),
            nullptr,
            nullptr);
    }

    winrt::com_ptr<ID3D11Device> CreateDevice()
    {
        winrt::com_ptr<ID3D11Device> device;
        HRESULT hr{ CreateDevice(D3D_DRIVER_TYPE_HARDWARE, device) };

        if (DXGI_ERROR_UNSUPPORTED == hr)
        {
            hr = CreateDevice(D3D_DRIVER_TYPE_WARP, device);
        }

        winrt::check_hresult(hr);
        return device;
    }

    winrt::com_ptr<ID2D1DeviceContext> CreateRenderTarget(
        winrt::com_ptr<ID2D1Factory1> const& factory,
        winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(factory);
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };

        winrt::com_ptr<ID2D1Device> d2device;
        winrt::check_hresult(factory->CreateDevice(dxdevice.get(), d2device.put()));

        winrt::com_ptr<ID2D1DeviceContext> target;
        winrt::check_hresult(d2device->CreateDeviceContext(D2D1_DEVICE_CONTEXT_OPTIONS_NONE, target.put()));
        return target;
    }

    winrt::com_ptr<IDXGIFactory2> GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };

        winrt::com_ptr<IDXGIAdapter> adapter;
        winrt::check_hresult(dxdevice->GetAdapter(adapter.put()));

        winrt::com_ptr<IDXGIFactory2> factory;
        winrt::check_hresult(adapter->GetParent(__uuidof(factory), factory.put_void()));
        return factory;
    }

    void CreateDeviceSwapChainBitmap(
        winrt::com_ptr<IDXGISwapChain1> const& swapchain,
        winrt::com_ptr<ID2D1DeviceContext> const& target)
    {
        WINRT_ASSERT(swapchain);
        WINRT_ASSERT(target);

        winrt::com_ptr<IDXGISurface> surface;
        winrt::check_hresult(swapchain->GetBuffer(0, __uuidof(surface), surface.put_void()));

        D2D1_BITMAP_PROPERTIES1 const props{ D2D1::BitmapProperties1(
            D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW,
            D2D1::PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_IGNORE)) };

        winrt::com_ptr<ID2D1Bitmap1> bitmap;

        winrt::check_hresult(target->CreateBitmapFromDxgiSurface(surface.get(),
            props,
            bitmap.put()));

        target->SetTarget(bitmap.get());
    }

    winrt::com_ptr<IDXGISwapChain1> CreateSwapChainForCoreWindow(winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIFactory2> const factory{ GetDxgiFactory(device) };

        DXGI_SWAP_CHAIN_DESC1 props{};
        props.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
        props.SampleDesc.Count = 1;
        props.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
        props.BufferCount = 2;
        props.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;

        winrt::com_ptr<IDXGISwapChain1> swapChain;

        winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
            device.get(),
            winrt::get_unknown(CoreWindow::GetForCurrentThread()),
            &props,
            nullptr, // all or nothing
            swapChain.put()));

        return swapChain;
    }

    constexpr D2D1_COLOR_F color_white{ 1.0f,  1.0f,  1.0f,  1.0f };
    constexpr D2D1_COLOR_F color_orange{ 0.92f,  0.38f,  0.208f,  1.0f };
}

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    winrt::com_ptr<ID2D1Factory1> m_factory;
    winrt::com_ptr<ID2D1DeviceContext> m_target;
    winrt::com_ptr<IDXGISwapChain1> m_swapChain;
    winrt::com_ptr<ID2D1SolidColorBrush> m_brush;
    float m_dpi{};

    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const&)
    {
    }

    void Load(hstring const&)
    {
        CoreWindow const window{ CoreWindow::GetForCurrentThread() };

        window.SizeChanged([&](auto&&...)
        {
            if (m_target)
            {
                ResizeSwapChainBitmap();
                Render();
            }
        });

        DisplayInformation const display{ DisplayInformation::GetForCurrentView() };
        m_dpi = display.LogicalDpi();

        display.DpiChanged([&](DisplayInformation const& display, IInspectable const&)
        {
            if (m_target)
            {
                m_dpi = display.LogicalDpi();
                m_target->SetDpi(m_dpi, m_dpi);
                CreateDeviceSizeResources();
                Render();
            }
        });

        m_factory = CreateFactory();
        CreateDeviceIndependentResources();
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        CoreWindow const window{ CoreWindow::GetForCurrentThread() };
        window.Activate();

        Render();
        CoreDispatcher const dispatcher{ window.Dispatcher() };
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const&) {}

    void Draw()
    {
        m_target->Clear(color_white);

        D2D1_SIZE_F const size{ m_target->GetSize() };
        D2D1_RECT_F const rect{ 100.0f, 100.0f, size.width - 100.0f, size.height - 100.0f };
        m_target->DrawRectangle(rect, m_brush.get(), 100.0f);

        char buffer[1024];
        (void)snprintf(buffer, sizeof(buffer), "Draw %.2f x %.2f @ %.2f\n", size.width, size.height, m_dpi);
        ::OutputDebugStringA(buffer);
    }

    void Render()
    {
        if (!m_target)
        {
            winrt::com_ptr<ID3D11Device> const device{ CreateDevice() };
            m_target = CreateRenderTarget(m_factory, device);
            m_swapChain = CreateSwapChainForCoreWindow(device);

            CreateDeviceSwapChainBitmap(m_swapChain, m_target);

            m_target->SetDpi(m_dpi, m_dpi);

            CreateDeviceResources();
            CreateDeviceSizeResources();
        }

        m_target->BeginDraw();
        Draw();
        m_target->EndDraw();

        HRESULT const hr{ m_swapChain->Present(1, 0) };

        if (S_OK != hr && DXGI_STATUS_OCCLUDED != hr)
        {
            ReleaseDevice();
        }
    }

    void ReleaseDevice()
    {
        m_target = nullptr;
        m_swapChain = nullptr;

        ReleaseDeviceResources();
    }

    void ResizeSwapChainBitmap()
    {
        WINRT_ASSERT(m_target);
        WINRT_ASSERT(m_swapChain);

        m_target->SetTarget(nullptr);

        if (S_OK == m_swapChain->ResizeBuffers(0, // all buffers
            0, 0, // client area
            DXGI_FORMAT_UNKNOWN, // preserve format
            0)) // flags
        {
            CreateDeviceSwapChainBitmap(m_swapChain, m_target);
            CreateDeviceSizeResources();
        }
        else
        {
            ReleaseDevice();
        }
    }

    void CreateDeviceIndependentResources()
    {
    }

    void CreateDeviceResources()
    {
        winrt::check_hresult(m_target->CreateSolidColorBrush(
            color_orange,
            D2D1::BrushProperties(0.8f),
            m_brush.put()));
    }

    void CreateDeviceSizeResources()
    {
    }

    void ReleaseDeviceResources()
    {
        m_brush = nullptr;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(App());
}
```

## <a name="working-with-com-types-such-as-bstr-and-variant"></a>BSTR 및 VARIANT 같이 COM 형식 사용

알 수 있듯이 C++/WinRT 구현 및 호출 하는 COM 인터페이스에 대 한 지원을 제공 합니다. BSTR 및 VARIANT를 같이 COM 형식 사용에 대 한 옵션 (함께 적절 한 Api)는 원시 형태로 사용 하려면 항상입니다. 또는 같은 프레임 워크에서 제공 하는 래퍼를 사용할 수 있습니다 합니다 [액티브 템플릿 라이브러리 (ATL)](/cpp/atl/active-template-library-atl-concepts), 또는 시각적 개체에서 C++ 컴파일러의 [COM 지원](/cpp/cpp/compiler-com-support), 또는 사용자 고유의 래퍼에 의해서도 합니다.

## <a name="important-apis"></a>중요 API
* [winrt::check_hresult 함수](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::com_ptr 구조체 템플릿](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown struct](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
