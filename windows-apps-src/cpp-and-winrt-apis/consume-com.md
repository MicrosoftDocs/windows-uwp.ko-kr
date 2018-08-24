---
author: stevewhims
description: 이 항목 전체 Direct2D 코드 예제를 사용 하 여 C +를 사용 하는 방법을 보여주는 + / WinRT COM 클래스와 인터페이스를 사용 하도록 합니다.
title: 사용 (영문) DirectX 및 기타 COM Api와 C + + / WinRT
ms.author: stwhi
ms.date: 07/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c + +, cpp, winrt, COM, 구성 요소, 클래스, 인터페이스
ms.localizationpriority: medium
ms.openlocfilehash: b87eb90ed5ecf731cc851e81e81ad016956e5fea
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2837095"
---
# <a name="consume-directx-and-other-com-apis-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>사용 (영문) DirectX 및 기타 COM Api와 [C + + / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

C + 시설을 사용 하 여 수 + / DirectX Api의 고성능 2 차원 및 3 차원 그래픽 등의 COM 구성 요소를 사용 하기 WinRT 라이브러리입니다. C + + / WinRT 성능에 영향을 미치지 않고 DirectX를 사용 하 여 가장 간단한 방법은 합니다. 이 항목 Direct2D 코드 예제를 사용 하 여 C +를 사용 하는 방법을 보여주는 + / WinRT COM 클래스와 인터페이스를 사용 하도록 합니다. 동일한 C + 내에서 COM 및 Windows 런타임 프로그래밍을 혼합 물론, 수, + / WinRT 프로젝트입니다.

이 항목의 끝에 최소한의 Direct2D 응용 프로그램의 전체 소스 코드 샘플을 찾을 수 있습니다. 해당 코드에서 발췌 한 내용 들고 하 고 C +를 사용 하 여 COM 구성 요소를 사용 하는 방법을 설명 하기 위해이 사용 하는 + / C +의 다양 한 기능을 사용 하 여 WinRT + / WinRT 라이브러리입니다.

## <a name="com-smart-pointers-winrtcomptruwpcpp-ref-for-winrtcom-ptr"></a>COM 스마트 포인터 ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

COM와 프로그래밍 하는 경우 인터페이스를 사용 하 여 직접 아니라 (즉의 백그라운드에서 Windows 런타임 Api에 대 한 COM의 발전은 true도) 개체와 함께 작동 합니다. COM 클래스에서 함수를 호출할 수 등 활성화 하면 클래스를 다시, 인터페이스를 가져오고 해당 인터페이스에서 함수를 호출 합니다. 개체의 상태에 액세스 하려면 있습니다 하지 해당 데이터 멤버에 직접 액세스 합니다. 대신, 인터페이스에서 접근자 및을 (를) 함수를 호출합니다.

더 구체적으로 인터페이스 *포인터*와 상호작용 하는 방법에 대 한 통화 하는 것입니다. C +에서 COM 스마트 포인터 형식의 있는지 여부를 활용 우리는를 위해 + / WinRT&mdash; [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) 유형입니다.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
```

위의 코드는 [**ID2D1Factory1**](https://msdn.microsoft.com/library/Hh404596) COM 인터페이스에 대 한 초기화 되지 않은 스마트 포인터를 선언 하는 방법을 보여줍니다. 스마트 포인터 초기화 된, 되지 않으므로 (것 가리키고 있지 않은 인터페이스를 전혀) 모든 실제 개체에 속한 **ID2D1Factory1** 인터페이스를 가리키는 아직 됩니다. 그렇게; 액 되었지만 하 고 (되는 스마트 포인터)를 통해 COM 참조 (영문)를 가리키는 인터페이스의 소유 하는 개체의 수명 관리 하 고 해당 인터페이스 함수를 호출할 수 있는 중간 규모 수를 계산 하는 기능입니다.

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>으로 인터페이스 포인터를 반환 하는 COM 함수 **void\ * \ ***

원시 포인터의 기본이 되는 초기화 되지 않은 스마트 포인터에 쓸 수 [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function) 함수를 호출할 수 있습니다.

```cppwinrt
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

해당 마지막 매개 변수를 통해,는 **ID2D1Factory1** 인터페이스 포인터를 반환 하는 [**D2D1CreateFactory**](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory) 함수를 호출 하는 위의 코드 **void\ * \ *** 유형입니다. 많은 COM 함수를 반환 하는 **void\ * \ *** 합니다. 이러한 기능에 대 한 [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function) 와 같이 사용 합니다.

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>특정 인터페이스 포인터를 반환 하는 COM 함수

해당 antepenultimate 매개 변수를 통해,는 [**ID3D11Device**](https://msdn.microsoft.com/library/Hh404596) 인터페이스 포인터를 반환 하는 [**D3D11CreateDevice**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) 함수 **ID3D11Device\ * \ *** 유형입니다. 와 같은 특정 인터페이스 포인터를 반환 하는 함수, [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)를 사용 합니다.

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

전에이 섹션의 코드 예제에서는 원시 **D2D1CreateFactory** 함수를 호출 하는 방법을 보여줍니다. 하지만이 항목에 대 한 코드 예제에서는 **D2D1CreateFactory**를 호출 하면 원시 API를 배치 하는 도우미 함수 서식 파일을 사용 하 여 하 고 코드 예제에서는 [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)실제로 사용 되는 하므로 실제로 합니다.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>으로 인터페이스 포인터를 반환 하는 COM 함수 **IUnknown\ * \ ***

해당 마지막 매개 변수를 통해,는 DirectWrite 공장 인터페이스 포인터를 반환 하는 [**DWriteCreateFactory**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) 함수 **IUnknown\ * \ *** 유형입니다. 이러한 함수에 대 한 [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)사용 하지만 하는 캐스팅 다시 해석 **IUnknown\ * \ *** 합니다.

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcomptr"></a>다시 **winrt::com_ptr** 장착

> [!IMPORTANT]
> 이미 꽂혀 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) 있는 경우 (해당 내부 원시 포인터에 이미 대상) 하 고 다시 다른 개체를 가리키도록 장착 하려면 다음 처음에 할당 해야 `nullptr` 을&mdash;아래 코드 예제와 같이 합니다. 이렇게 하지 않으면 다음 이미 장착 **com_ptr** 그려집니다 문제 주의 ( [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function) 또는 [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)호출) 하는 경우 해당 내부 포인터 null이 아닌 가정 하 여.

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

COM 함수에서 반환 된 HRESULT 값을 확인 하 고 오류 코드를 나타내는 예외를 throw 하려면 [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)를 호출 합니다.

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>특정 인터페이스 포인터를 사용 하는 COM 함수

함수에 전달 하 여 **com_ptr** 는 같은 종류의 특정 인터페이스 포인터를 사용 하는 [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#comptrget-function) 함수를 호출할 수 있습니다.

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

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>**IUnknown** 인터페이스 포인터를 사용 하는 COM 함수

함수에 전달 하 여 **com_ptr** 는 **IUnknown** 인터페이스 포인터를 사용 하는 [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#getunknown-function) 무료 함수를 호출할 수 있습니다.

```cppwinrt
winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
    ...
    winrt::get_unknown(CoreWindow::GetForCurrentThread()),
    ...));
```

## <a name="passing-and-returning-com-smart-pointers"></a>스마트 포인터를 전달 하 고, COM를 반환 합니다.

**Winrt::com_ptr** 의 형태로 COM 스마트 포인터 라인으로 전환 하는 함수 상수 참조 또는 참조 하 여 수행 해야 합니다.

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

**Winrt::com_ptr** 를 반환 하는 함수 값으로이 수행 해야 합니다.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>다른 인터페이스에 대 한 COM 스마트 포인터를 쿼리 합니다.

다른 인터페이스에 대 한 COM 스마트 포인터를 쿼리 하는 [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#comptras-function) 함수를 사용할 수 있습니다. 쿼리 제대로 되지 않으면 예외를 throw 하는 함수입니다.

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

또는 [**com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#comptrtryas-function)를 검사할 수 있는 값을 반환 하는 사용 하 여 `nullptr` 쿼리 성공 했는지 여부를 볼 수 있습니다.

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>최소한의 Direct2D 응용 프로그램의 전체 소스 코드 샘플

새 만들기를 구성 하 고 Visual Studio에서 먼저 다음이 소스 코드 예제를 실행 하려는 경우 **코어 응용 프로그램 (C + + / WinRT)** 합니다. `Direct2D` 프로젝트에 대 한 적절 한 이름을 이지만 하면가 원하는 이름을 지정할 수 있습니다. Open `App.cpp`의 전체 내용을 삭제 하 고 아래 목록에 붙여넣습니다.

```cppwinrt
#include "pch.h"

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

        WINRT_TRACE("Draw %.2f x %.2f @ %.2f\n", size.width, size.height, m_dpi);
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

## <a name="important-apis"></a>중요 API
* [winrt::check_hresult](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown 구조체](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
