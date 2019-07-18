---
description: 전체 Direct2D 코드 예제를 사용하여 C++/WinRT를 통해 COM 클래스 및 인터페이스를 사용하는 방법을 보여 줍니다.
title: C++/WinRT를 통한 COM 구성 요소 사용
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, COM, 구성 요소, 클래스, 인터페이스
ms.localizationpriority: medium
ms.openlocfilehash: bb28ec7afa22f81033bfce2aff530119e53a4b91
ms.sourcegitcommit: 7585bf66405b307d7ed7788d49003dc4ddba65e6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67660155"
---
# <a name="consume-com-components-with-cwinrt"></a>C++/WinRT를 통한 COM 구성 요소 사용

[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 라이브러리의 기능을 사용하여 DirectX API의 고성능 2차원 및 3차원 그래픽과 같은 COM 구성 요소를 사용할 수 있습니다. C++/WinRT는 성능 손상 없이 DirectX를 사용하는 가장 간단한 방법입니다. 이 항목에서는 Direct2D 코드 예제를 사용하여 C++/WinRT를 통해 COM 클래스 및 인터페이스를 사용하는 방법을 보여 줍니다. 동일한 C++/WinRT 프로젝트에서 COM 및 Windows 런타임 프로그래밍을 함께 사용할 수도 있습니다.

이 항목의 끝에는 최소 Direct2D 애플리케이션의 전체 소스 코드 목록이 나와 있습니다. 해당 코드에서 발췌한 내용을 사용하여 C++/WinRT 라이브러리의 다양한 기능을 통해 C++/WinRT에서 COM 구성 요소를 사용하는 방법을 설명하겠습니다.

## <a name="com-smart-pointers-winrtcomptruwpcpp-ref-for-winrtcom-ptr"></a>COM 스마트 포인터([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

COM을 사용하여 프로그래밍하는 경우 개체가 아닌 인터페이스로 직접 작업합니다(COM의 발전된 형태인 Windows 런타임 API의 경우에도 백그라운드에서 동일한 방식으로 작동함). 예를 들어 COM 클래스에서 함수를 호출하려면 클래스를 활성화하고 인터페이스를 다시 가져온 다음, 해당 인터페이스에서 함수를 호출합니다. 개체의 상태에 액세스하려면 데이터 멤버에 직접 액세스하지 않고, 대신 인터페이스에서 접근자 및 변경자 함수를 호출합니다.

보다 구체적으로, 인터페이스 ‘포인터’ 조작을 말하는 것입니다.  이러한 용도로, C++/WinRT에 있는 COM 스마트 포인터 형식, 즉 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) 형식을 활용합니다.

```cppwinrt
#include <d2d1_1.h>
...
winrt::com_ptr<ID2D1Factory1> factory;
```

위의 코드는 [**ID2D1Factory1**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1factory1) COM 인터페이스에 대한 초기화되지 않은 스마트 포인터를 선언하는 방법을 보여 줍니다. 스마트 포인터가 초기화되지 않았으므로, 아직 실제 개체에 속하는 **ID2D1Factory1** 인터페이스를 가리키지는 않습니다(인터페이스를 가리키지 않음). 그러나 잠재적으로 인터페이스를 가리킬 수 있으며, 스마트 포인터로서 COM 참조 계산을 통해 가리키는 인터페이스의 소유 개체 및 해당 인터페이스에서 함수를 호출하는 데 사용되는 매체의 수명을 관리할 수 있습니다.

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>인터페이스 포인터를 **void**로 반환하는 COM 함수

[**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) 함수를 호출하여 초기화되지 않은 스마트 포인터의 기본 원시 포인터에 쓸 수 있습니다.

```cppwinrt
D2D1_FACTORY_OPTIONS options{ D2D1_DEBUG_LEVEL_NONE };
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

위의 코드는 **void\*\*** 형식인 마지막 매개 변수를 통해 **ID2D1Factory1** 인터페이스 포인터를 반환하는 [**D2D1CreateFactory**](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory) 함수를 호출합니다. 대부분의 COM 함수는 **void\*\*** 를 반환합니다. 이러한 함수의 경우 표시된 대로 [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)를 사용합니다.

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>특정 인터페이스 포인터를 반환하는 COM 함수

[**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 함수는 **ID3D11Device\*\*** 형식인 끝에서 세 번째 매개 변수를 통해 [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) 인터페이스 포인터를 반환합니다. 이와 같이 특정 인터페이스 포인터를 반환하는 함수의 경우 [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function)를 사용합니다.

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

섹션에서 이 코드 예제 앞에 있는 코드 예제는 원시 **D2D1CreateFactory** 함수를 호출하는 방법을 보여 줍니다. 그러나 이 항목의 코드 예제는 **D2D1CreateFactory**를 호출할 때 원시 API를 래핑하는 도우미 함수 템플릿을 사용하므로, 실제로 [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function)를 사용합니다.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>인터페이스 포인터를 **IUnknown**으로 반환하는 COM 함수

[**DWriteCreateFactory**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) 함수는 [**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown) 형식인 마지막 매개 변수를 통해 DirectWrite 팩터리 인터페이스 포인터를 반환합니다. 이러한 함수의 경우 [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function)를 사용하지만, **IUnknown**으로 재해석 캐스팅합니다.

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcomptr"></a>**winrt::com_ptr** 재배치

> [!IMPORTANT]
> 이미 배치된 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr)이 있고(내부 원시 포인터에 이미 대상이 있음) 다른 개체를 가리키도록 다시 배치하려는 경우, 아래 코드 예제와 같이 `nullptr`을 먼저 할당해야 합니다. 할당하지 않으면, [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function) 또는 [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)를 호출할 때 이미 배치된 **com_ptr**에서 내부 포인터가 Null이 아님을 어설션하여 문제가 부각됩니다.

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

## <a name="handle-hresult-error-codes"></a>HRESULT 오류 코드 처리

COM 함수에서 반환된 HRESULT의 값을 확인하고 오류 코드를 나타내는 이벤트에서 예외를 throw하려면 [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)를 호출합니다.

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>특정 인터페이스 포인터를 사용하는 COM 함수

[**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function) 함수를 호출하여 동일한 형식의 특정 인터페이스 포인터를 사용하는 함수에 **com_ptr**을 전달할 수 있습니다.

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

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>**IUnknown** 인터페이스 포인터를 사용하는 COM 함수

[**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#get_unknown-function) 프리 함수를 호출하여 **IUnknown** 인터페이스 포인터를 사용하는 함수에 **com_ptr**을 전달할 수 있습니다.

```cppwinrt
winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
    ...
    winrt::get_unknown(CoreWindow::GetForCurrentThread()),
    ...));
```

## <a name="passing-and-returning-com-smart-pointers"></a>COM 스마트 포인터 전달 및 반환

**winrt::com_ptr** 형식의 COM 스마트 포인터를 사용하는 함수는 상수 참조 또는 참조로 사용해야 합니다.

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

**winrt::com_ptr**을 반환하는 함수는 값으로 반환해야 합니다.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>다른 인터페이스에 대한 COM 스마트 포인터 쿼리

[**com_ptr::** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function) 함수로 사용하여 다른 인터페이스에 대한 COM 스마트 포인터를 쿼리할 수 있습니다. 쿼리가 실패하면 함수에서 예외를 throw합니다.

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

또는 쿼리의 성공 여부를 확인하기 위해 `nullptr`과 비교할 수 있는 값을 반환하는 [**com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrtry_as-function)를 사용합니다.

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>최소 Direct2D 애플리케이션의 전체 소스 코드 목록

이 소스 코드 예제를 빌드 및 실행하려는 경우, 먼저 Visual Studio에서 새 **코어 앱(C++/WinRT)** 을 만듭니다. `Direct2D`는 프로젝트에 적합한 이름이지만, 원하는 이름을 임의로 지정할 수 있습니다. `App.cpp`를 열고 전체 내용을 삭제한 다음, 아래 목록을 붙여넣습니다.

아래 코드는 가능한 경우 [winrt::com_ptr::capture 함수](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcapture-function)를 사용합니다. `WINRT_ASSERT`는 매크로 정의이며 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)로 확장됩니다.

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
        factory.capture(adapter, &IDXGIAdapter::GetParent);
        return factory;
    }

    void CreateDeviceSwapChainBitmap(
        winrt::com_ptr<IDXGISwapChain1> const& swapchain,
        winrt::com_ptr<ID2D1DeviceContext> const& target)
    {
        WINRT_ASSERT(swapchain);
        WINRT_ASSERT(target);

        winrt::com_ptr<IDXGISurface> surface;
        surface.capture(swapchain, &IDXGISwapChain1::GetBuffer, 0);

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
    CoreApplication::Run(winrt::make<App>());
}
```

## <a name="working-with-com-types-such-as-bstr-and-variant"></a>BSTR, VARIANT 등의 COM 형식 작업

C++/WinRT는 COM 인터페이스 구현 및 호출을 모두 지원합니다. BSTR, VARIANT 등의 COM 형식을 사용하려면 [WIL(Windows 구현 라이브러리)](https://github.com/Microsoft/wil)에서 제공하는 **wil::unique_bstr**, **wil::unique_variant**(리소스 수명 관리) 등의 래퍼를 사용하는 것이 좋습니다.

[WIL](https://github.com/Microsoft/wil)은 ATL(액티브 템플릿 라이브러리) 및 Visual C++ 컴파일러의 COM 지원과 같은 프레임워크를 대체합니다. 또한 고유한 래퍼를 작성하거나 적절한 API와 함께 BSTR, VARIANT 등의 COM 형식을 원시 형식으로 사용하는 것보다 WIL이 권장됩니다.

## <a name="avoiding-namespace-collisions"></a>네임스페이스 충돌 방지

일반적으로 C++/WinRT에서는 using 지시문을 자유롭게 사용할 수 있습니다(이 항목의 코드 목록에 있는 설명 참조). 그러나 이로 인해 충돌하는 이름을 전역 네임스페이스로 가져오는 문제가 발생할 수 있습니다. 예를 들면 다음과 같습니다.

C++/WinRT에 이름이 [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)인 형식이 포함되어 있지만 COM에서는 [ **::IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown)라는 이름의 형식을 정의합니다. 따라서 COM 헤더를 사용하는 C++/WinRT 프로젝트에서 다음 코드를 고려해 보세요.

```cppwinrt
using namespace winrt::Windows::Foundation;
...
void MyFunction(IUnknown*); // error C2872:  'IUnknown': ambiguous symbol
```

정규화되지 않은 이름인 *IUnknown*은 전역 네임스페이스에서 충돌하므로 *모호한 기호* 컴파일러 오류가 발생합니다. 대신 다음과 같이 C++/WinRT 버전의 이름을 **winrt** 네임스페이스로 격리할 수 있습니다.

```cppwinrt
namespace winrt
{
    using namespace Windows::Foundation;
}
...
void MyFunctionA(IUnknown*); // Ok.
void MyFunctionB(winrt::IUnknown const&); // Ok.
```

아니면, 간편하게 `using namespace winrt`를 사용할 수도 있습니다. 다음과 같이 전역 버전의 *IUnknown*을 한정하면 됩니다.

```cppwinrt
using namespace winrt;
namespace winrt
{
    using namespace Windows::Foundation;
}
...
void MyFunctionA(::IUnknown*); // Ok.
void MyFunctionB(winrt::IUnknown const&); // Ok.
```

물론, 이 방법은 모든 C++/WinRT 네임스페이스에서 작동합니다.

```cppwinrt
namespace winrt
{
    using namespace Windows::Storage;
    using namespace Windows::System;
}
```

그 다음, **winrt::Windows::Storage::StorageFile**(예를 들어, 간단히 **winrt::StorageFile**)을 참조할 수 있습니다.

## <a name="important-apis"></a>중요 API
* [winrt::check_hresult 함수](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::com_ptr 구조체 템플릿](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown 구조체](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
