---
title: . C a l 코드와 DXGI 및 Direct3D 비교
description: DXGI (DirectX Graphics Interface)와 여러 Direct3D Api는 동일한 역할을 합니다. 이 항목은 다양 한 측면에서 DXGI 및 Direct3D 11을 이해 하는 데 도움이 됩니다.
ms.assetid: 90f5ecf1-dd5d-fea3-bed8-57a228898d2a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 안 면, dxgi, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: a9e521810c5220d412afb98d25a00f5ac499af1b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165217"
---
# <a name="compare-egl-code-to-dxgi-and-direct3d"></a>. C a l 코드와 DXGI 및 Direct3D 비교




**중요 API**

-   [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)
-   [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)
-   [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)

DXGI (DirectX Graphics Interface)와 여러 Direct3D Api는 동일한 역할을 합니다. 이 항목은 다양 한 측면에서 DXGI 및 Direct3D 11을 이해 하는 데 도움이 됩니다.

DXGI 및 Direct3D는 리소스를 구성 하 고, 셰이더에 그릴 렌더링 컨텍스트를 가져오고, 결과를 창에 표시 하는 메서드를 제공 합니다. 그러나 DXGI와 Direct3D에는 몇 가지 추가 옵션이 포함 되어 있으며,이 경우에는 더 많은 작업이 필요 합니다.

> **참고**    이 지침은 [Khronos Native Platform Graphics Interface 1.4 (1.4-4 월 6 일, 2011) \[ PDF \] ](https://www.khronos.org/registry/egl/specs/eglspec.1.4.20110406.pdf)에 있는 Khronos 그룹의 공개 사양을 기반으로 합니다. 다른 플랫폼 및 개발 언어와 관련 된 구문의 차이점은이 지침에서 다루지 않습니다.

 

## <a name="how-does-dxgi-and-direct3d-compare"></a>DXGI와 Direct3D는 어떻게 비교 되나요?


DXGI와 Direct3D에서의 큰 장점은 창 화면으로 그리기를 시작 하는 것은 비교적 간단 하다는 점입니다. 이는 OpenGL ES 2.0 및 그와 같은 것이 여러 플랫폼 공급자에 의해 구현 되는 사양 이지만, DXGI 및 Direct3D는 하드웨어 공급 업체 드라이버가 준수 해야 하는 단일 참조 이기 때문입니다. 즉, Microsoft는 특정 공급 업체에서 제공 하는 기능 하위 집합에 초점을 맞춘 것이 아니라 공급 업체의 다양 한 기능을 구현 하는 Api 집합을 구현 해야 합니다. 반면, Direct3D는 매우 광범위 한 그래픽 하드웨어 플랫폼과 기능 수준을 포함 하는 단일 Api 집합을 제공 하 고, 플랫폼에서 경험 하는 개발자에 게 더 많은 유연성을 제공 합니다.

예를 들어, DXGI와 Direct3D는 다음과 같은 동작을 위한 Api를 제공 합니다.

-   프레임 버퍼 (DXGI에서 "스왑 체인" 이라고 함)를 가져오고 읽고 씁니다.
-   UI 창과 프레임 버퍼 연결
-   그릴 렌더링 컨텍스트 가져오기 및 구성
-   특정 렌더링 컨텍스트에 대 한 그래픽 파이프라인에 명령을 실행 합니다.
-   셰이더 리소스를 만들고 관리 하며 렌더링 콘텐츠와 연결
-   특정 렌더링 대상 (예: 질감)으로 렌더링
-   그래픽 리소스로 렌더링 된 결과와 함께 창의 표시 화면을 업데이트 합니다.

그래픽 파이프라인을 구성 하는 기본 Direct3D 프로세스를 보려면 Microsoft Visual Studio 2015에서 DirectX 11 앱 (유니버설 Windows) 템플릿을 확인 하세요. 이 클래스의 기본 렌더링 클래스는 Direct3D 11 그래픽 인프라를 설정 하 고,이에 대 한 기본 리소스를 구성 하 고, 화면 회전과 같은 UWP (유니버설 Windows 플랫폼) 앱 기능을 지원 하기 위한 좋은 기준을 제공 합니다.

뛰어난 l은 Direct3D 11을 기준으로 하는 Api를 거의 포함 하지 않으며 플랫폼에 특정 한 명명 및 전문 용어에 익숙하지 않은 경우 후자를 탐색 하는 것이 어려울 수 있습니다. 다음은 중심을 가져오는 데 도움이 되는 간단한 개요입니다.

먼저, Direct3D 인터페이스 매핑에 대 한 기본가 나 개체를 검토 합니다.

| 고 대 추상화 | 유사한 Direct3D 표시                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **E글 표시**  | (UWP 앱 용) Direct3D에서 표시 핸들은 [**Windows:: UI:: CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) API (또는 HWND를 노출 하는 **ICoreWindowInterop** 인터페이스)를 통해 가져옵니다. 어댑터와 하드웨어 구성은 각각 [**Idxgid 어댑터**](/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) 및 [**IDXGIDevice1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2) COM 인터페이스를 사용 하 여 설정 됩니다.                                                                                                                                                                                                                                                           |
| **E글 표면**  | Direct3D에서 버퍼 및 기타 창 리소스 (표시 또는 오프 스크린)는 [**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) ([**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) (표시 버퍼)와 같은 dxgi 리소스를 얻는 데 사용 되는 팩터리 패턴 구현인를 비롯 한 특정 DXGI 인터페이스에 의해 생성 및 구성 됩니다. 그래픽 장치 및 해당 리소스를 나타내는 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 는 [**D3D11Device:: createdevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice)를 사용 하 여 획득 됩니다. 렌더링 대상의 경우 [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) 인터페이스를 사용 합니다. |
| **E글 컨텍스트**  | Direct3D에서는 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 인터페이스를 사용 하 여 그래픽 파이프라인에 대 한 명령을 구성 하 고 실행 합니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **EGLConfig**   | Direct3D 11에서 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 인터페이스의 메서드를 사용 하 여 버퍼, 질감, 스텐실 및 셰이더와 같은 그래픽 리소스를 만들고 구성 합니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

 

이제 다음은 UWP 앱 용으로 단순 그래픽 표시, 리소스 및 컨텍스트를 설정 하는 가장 기본적인 프로세스입니다.

1.  [**CoreWindow:: GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread)를 호출 하 여 앱의 핵심 UI 스레드에 대 한 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 개체에 대 한 핸들을 가져옵니다.
2.  UWP 앱의 경우 [**IDXGIAdapter2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiadapter2) [**:: CreateSwapChainForCoreWindow**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow)를 사용 하 여 교환 체인을 획득 하 고 1 단계에서 얻은 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 참조를 전달 합니다. 반환 되는 [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) 인스턴스를 가져옵니다. 렌더러 개체 및 해당 렌더링 스레드에 대해 범위를 합니다.
3.  [**D3D11Device:: CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 메서드를 호출 하 여 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 및 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 인스턴스를 가져옵니다. 렌더러 개체로도 범위를 합니다.
4.  렌더러의 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 개체에서 메서드를 사용 하 여 셰이더, 질감 및 기타 리소스를 만듭니다.
5.  렌더러의 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 개체에서 메서드를 사용 하 여 버퍼를 정의 하 고 셰이더를 실행 하 고 파이프라인 단계를 관리 합니다.
6.  파이프라인이 실행 되 고 백 버퍼에 프레임을 그리면 [**IDXGISwapChain1::P resent1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)를 사용 하 여 화면에 표시 합니다.

이 프로세스를 자세히 검토 하려면 [DirectX 그래픽 시작](/windows/desktop/getting-started-with-directx-graphics)을 검토 하세요. 이 문서의 나머지 부분에서는 기본 그래픽 파이프라인 설정 및 관리에 대 한 일반적인 여러 단계를 다룹니다.
> **참고**    Windows 데스크톱 앱에는 [**D3D11Device:: CreateDeviceAndSwapChain**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdeviceandswapchain)와 같은 Direct3D 스왑 체인을 얻기 위한 다른 api가 있으며 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 개체를 사용 하지 않습니다.

 

## <a name="obtaining-a-window-for-display"></a>표시할 창 가져오기


이 예제에서 eglGetDisplay는 Microsoft Windows 플랫폼과 관련 된 창 리소스에 대해 HWND를 전달 합니다. Apple의 iOS (Cocoa) 및 Google Android 같은 다른 플랫폼에는 창 리소스에 대 한 서로 다른 핸들이 나 참조가 있으며 호출 구문이 완전히 다를 수 있습니다. 디스플레이를 가져온 후 초기화 하 고, 기본 구성을 설정 하 고, 그릴 수 있는 백 버퍼를 사용 하 여 화면을 만듭니다.

표시 하 고, 고도를 사용 하 여 구성 합니다.

``` syntax
// Obtain an EGL display object.
EGLDisplay display = eglGetDisplay(GetDC(hWnd));
if (display == EGL_NO_DISPLAY)
{
  return EGL_FALSE;
}

// Initialize the display
if (!eglInitialize(display, &majorVersion, &minorVersion))
{
  return EGL_FALSE;
}

// Obtain the display configs
if (!eglGetConfigs(display, NULL, 0, &numConfigs))
{
  return EGL_FALSE;
}

// Choose the display config
if (!eglChooseConfig(display, attribList, &config, 1, &numConfigs))
{
  return EGL_FALSE;
}

// Create a surface
surface = eglCreateWindowSurface(display, config, (EGLNativeWindowType)hWnd, NULL);
if (surface == EGL_NO_SURFACE)
{
  return EGL_FALSE;
}
```

Direct3D에서 UWP 앱의 주 창은 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 개체로 표현 됩니다 .이 개체는 Direct3D에 대해 생성 하는 "뷰 공급자"의 초기화 프로세스의 일부로 [**CoreWindow:: GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) 를 호출 하 여 앱 개체에서 가져올 수 있습니다. (Direct3D XAML interop를 사용 하는 경우 XAML 프레임 워크의 뷰 공급자를 사용 합니다.) Direct3D 보기 공급자를 만드는 프로세스는 [보기를 표시 하도록 앱을 설정](/previous-versions/windows/apps/hh465077(v=win.10))하는 방법에 설명 되어 있습니다.

Direct3D 용 CoreWindow 가져오기

``` syntax
CoreWindow::GetForCurrentThread();
```

[**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 참조를 가져온 후에는 주 개체의 **Run** 메서드를 실행 하 고 창 이벤트 처리를 시작 하는 창을 활성화 해야 합니다. 그런 다음 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 및 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)를 만든 후이를 사용 하 여 기본 [**IDXGIDevice1**](/windows/desktop/api/dxgi/nn-dxgi-idxgidevice1) 및 [**Idxgid 어댑터**](/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) 를 가져옵니다. 그러면 [**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) 개체를 가져와 [**DXGI \_ 스왑 \_ 체인 \_ DESC1**](/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1) 구성에 따라 스왑 체인 리소스를 만들 수 있습니다.

Direct3D 용 CoreWindow에서 DXGI 스왑 체인 구성 및 설정

``` syntax
// Called when the CoreWindow object is created (or re-created).
void SimpleDirect3DApp::SetWindow(CoreWindow^ window)
{
  // Register event handlers with the CoreWindow object.
  // ...

  // Obtain your ID3D11Device1 and ID3D11DeviceContext1 objects
  // In this example, m_d3dDevice contains the scoped ID3D11Device1 object
  // ...

  ComPtr<IDXGIDevice1>  dxgiDevice;
  // Get the underlying DXGI device of the Direct3D device.
  m_d3dDevice.As(&dxgiDevice);

  ComPtr<IDXGIAdapter> dxgiAdapter;
  dxgiDevice->GetAdapter(&dxgiAdapter);

  ComPtr<IDXGIFactory2> dxgiFactory;
  dxgiAdapter->GetParent(
    __uuidof(IDXGIFactory2), 
    &dxgiFactory);

  DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};
  swapChainDesc.Width = static_cast<UINT>(m_d3dRenderTargetSize.Width); // Match the size of the window.
  swapChainDesc.Height = static_cast<UINT>(m_d3dRenderTargetSize.Height);
  swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
  swapChainDesc.Stereo = false;
  swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
  swapChainDesc.SampleDesc.Quality = 0;
  swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
  swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
  swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All UWP apps must use this SwapEffect.
  swapChainDesc.Flags = 0;

  // ...

  Windows::UI::Core::CoreWindow^ window = m_window.Get();
  dxgiFactory->CreateSwapChainForCoreWindow(
    m_d3dDevice.Get(),
    reinterpret_cast<IUnknown*>(window),
    &swapChainDesc,
    nullptr, // Allow on all displays.
    &m_swapChainCoreWindow);
}
```

프레임을 표시 하기 위해 프레임을 준비한 후 [**IDXGISwapChain1::P resent1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1) 메서드를 호출 합니다.

Direct3D 11에서는 EGLSurface와 동일한 추상화가 없습니다. [**IDXGISurface1**](/windows/desktop/api/dxgi/nn-dxgi-idxgisurface1)있지만 다른 방식으로 사용 됩니다. 가장 가까운 개념 근사값은 셰이더 파이프라인이 그릴 백 버퍼로 텍스처 ([**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d))를 할당 하는 데 사용 하는 [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) 개체입니다.

Direct3D 11에서 스왑 체인에 대 한 백 버퍼 설정

``` syntax
ComPtr<ID3D11RenderTargetView>    m_d3dRenderTargetViewWin; // scoped to renderer object

// ...

ComPtr<ID3D11Texture2D> backBuffer2;
    
m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(&backBuffer2));

m_d3dDevice->CreateRenderTargetView(
  backBuffer2.Get(),
  nullptr,
    &m_d3dRenderTargetViewWin);
```

창이 만들어지거나 크기가 변경 될 때마다이 코드를 호출 하는 것이 좋습니다. 렌더링 하는 동안 버텍스 버퍼 또는 셰이더와 같은 다른 하위 리소스를 설정 하기 전에 [**ID3D11DeviceContext1:: OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) 를 사용 하 여 렌더링 대상 뷰를 설정 합니다.

``` syntax
// Set the render target for the draw operation.
m_d3dContext->OMSetRenderTargets(
        1,
        d3dRenderTargetView.GetAddressOf(),
        nullptr);
```

## <a name="creating-a-rendering-context"></a>렌더링 컨텍스트 만들기


1.4에서 "표시"는 창 리소스 집합을 나타냅니다. 일반적으로 표시 개체에 대 한 특성 집합을 제공 하 고 반환 화면을 가져오기 때문에 디스플레이에 대 한 "surface"를 구성 합니다. 해당 컨텍스트를 만들고 화면 및 디스플레이에 바인딩하여 표면의 콘텐츠를 표시 하는 컨텍스트를 만듭니다.

호출 흐름은 일반적으로 다음과 유사 합니다.

-   표시 또는 창 리소스 핸들을 사용 하 여 E글 Getdisplay를 호출 하 고 표시 개체를 가져옵니다.
-   EglInitialize를 사용 하 여 디스플레이를 초기화 합니다.
-   사용 가능한 표시 구성을 가져오고 eglGetConfigs 및 eglChooseConfig를 사용 하 여 하나를 선택 합니다.
-   EglCreateWindowSurface를 사용 하 여 창 화면을 만듭니다.
-   E글 Createcontext로 그리기 위한 표시 컨텍스트를 만듭니다.
-   표시 컨텍스트를 표시 하 고 eglMakeCurrent를 사용 하 여 화면에 바인딩합니다.

이전 섹션에서는 E글 표시 및 Egldisplay를 만들었으며, 이제 E글 표시를 사용 하 여 컨텍스트를 만들고, 출력을 매개 변수화 하는 구성 된 Egldisplay를 사용 하 여 해당 컨텍스트를 표시와 연결 합니다.

고가와 함께 렌더링 컨텍스트 가져오기 1.4

```cpp
// Configure your EGLDisplay and obtain an EGLSurface here ...
// ...

// Create a drawing context from the EGLDisplay
context = eglCreateContext(display, config, EGL_NO_CONTEXT, contextAttribs);
if (context == EGL_NO_CONTEXT)
{
  return EGL_FALSE;
}   
   
// Make the context current
if (!eglMakeCurrent(display, surface, surface, context))
{
  return EGL_FALSE;
}
```

Direct3D 11의 렌더링 컨텍스트는 어댑터를 나타내며 버퍼 및 셰이더와 같은 Direct3D 리소스를 만들 수 있도록 하는 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 개체로 표현 됩니다. [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 개체를 사용 하 여 그래픽 파이프라인을 관리 하 고 셰이더를 실행할 수 있습니다.

Direct3D 기능 수준에 주의 하세요. DirectX 9.1에서 DirectX 11까지 이전 Direct3D 하드웨어 플랫폼을 지 원하는 데 사용 됩니다. 태블릿 등의 낮은 파워 그래픽 하드웨어를 사용 하는 많은 플랫폼은 DirectX 9.1 기능에만 액세스할 수 있으며, 이전에 지원 되 던 그래픽 하드웨어는 9.1에서 11 까지입니다.

DXGI 및 Direct3D를 사용 하 여 렌더링 컨텍스트 만들기

```cpp

// ... 

UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;
ComPtr<IDXGIDevice> dxgiDevice;

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
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> d3dContext;

D3D11CreateDevice(
  nullptr, // Specify nullptr to use the default adapter.
  D3D_DRIVER_TYPE_HARDWARE,
  nullptr,
  creationFlags, // Set set debug and Direct2D compatibility flags.
  featureLevels, // List of feature levels this app can support.
  ARRAYSIZE(featureLevels),
  D3D11_SDK_VERSION, // Always set this to D3D11_SDK_VERSION for UWP apps.
  &device, // Returns the Direct3D device created.
  &m_featureLevel, // Returns feature level of device created.
  &d3dContext // Returns the device immediate context.
);
```

## <a name="drawing-into-a-texture-or-pixmap-resource"></a>질감 또는 pixmap 리소스로 그리기


OpenGL ES 2.0를 사용 하 여 질감에 그리려면 픽셀 버퍼 또는 PBuffer를 구성 합니다. 성공적으로 구성 하 고이에 대 한 EGLSurface를 만든 후에는 렌더링 컨텍스트를 제공 하 고 셰이더 파이프라인을 실행 하 여 질감에 그릴 수 있습니다.

OpenGL ES 2.0를 사용 하 여 픽셀 버퍼에 그리기

``` syntax
// Create a pixel buffer surface to draw into
EGLConfig pBufConfig;
EGLint totalpBufAttrs;

const EGLint pBufConfigAttrs[] =
{
    // Configure the pBuffer here...
};
 
eglChooseConfig(eglDsplay, pBufConfigAttrs, &pBufConfig, 1, &totalpBufAttrs);
EGLSurface pBuffer = eglCreatePbufferSurface(eglDisplay, pBufConfig, EGL_TEXTURE_RGBA); 
```

Direct3D 11에서 [**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d) 리소스를 만들고 렌더링 대상으로 만듭니다. [**D3D11 \_ 렌더링 \_ 대상 \_ 뷰 \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_view_desc)를 사용 하 여 렌더링 대상을 구성 합니다. 이 렌더링 대상을 사용 하 여 [**ID3D11DeviceContext::D raw**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) 메서드 (또는 장치 컨텍스트에서 비슷한 그리기 작업)를 호출 하면 \* 결과가 질감으로 그려집니다.

Direct3D 11을 사용 하 여 질감으로 그리기

``` syntax
ComPtr<ID3D11Texture2D> renderTarget1;

D3D11_RENDER_TARGET_VIEW_DESC renderTargetDesc = {0};
// Configure renderTargetDesc here ...

m_d3dDevice->CreateRenderTargetView(
  renderTarget1.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);

// Later, in your render loop...

// Set the render target for the draw operation.
m_d3dContext->OMSetRenderTargets(
        1,
        d3dRenderTargetView.GetAddressOf(),
        nullptr);
```

이 질감은 [**ID3D11ShaderResourceView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview)연결 된 경우 셰이더에 전달할 수 있습니다.

## <a name="drawing-to-the-screen"></a>화면으로 그리기


E글 컨텍스트를 사용 하 여 버퍼를 구성 하 고 데이터를 업데이트 한 후에는이에 바인딩된 셰이더를 실행 하 고 결과를 배경 버퍼에 표시 합니다. E글 Swapbuffers를 호출 하 여 백 버퍼를 표시 합니다.

GL ES 2.0: 그리기를 화면으로 엽니다.

``` syntax
glDrawElements(GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);

eglSwapBuffers(drawContext->eglDisplay, drawContext->eglSurface);
```

Direct3D 11에서 [**Idxgiswapchain::P resent1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)를 사용 하 여 버퍼를 구성 하 고 셰이더를 바인딩합니다. 그런 다음 [**ID3D11DeviceContext1::D raw**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) \* 메서드 중 하나를 호출 하 여 셰이더를 실행 하 고 스왑 체인의 백 버퍼로 구성 된 렌더링 대상에 결과를 그립니다. 그런 다음 **Idxgiswapchain::P resent1**를 호출 하 여 디스플레이에 백 버퍼를 표시 하기만 하면 됩니다.

Direct3D 11: 화면에 그리기

``` syntax

m_d3dContext->DrawIndexed(
        m_indexCount,
        0,
        0);

// ...

m_swapChainCoreWindow->Present1(1, 0, &parameters);
```

## <a name="releasing-graphics-resources"></a>그래픽 리소스 해제


이상에서 E글 표시를 Egldisplay에 전달 하 여 창 리소스를 해제 합니다.

가 중에서 디스플레이 종료 (1.4)

```cpp
EGLBoolean eglTerminate(eglDisplay);
```

UWP 앱에서는 CoreWindow를 사용 하 여 [**CoreWindow:: close**](/uwp/api/windows.ui.core.corewindow.close)를 닫을 수 있지만이는 보조 UI 창에만 사용할 수 있습니다. 기본 UI 스레드 및 연결된 해당 CoreWindow는 닫을 수 없습니다. 대신, 운영 체제에 의해 만료됩니다. 그러나 보조 CoreWindow를 닫으면 [**CoreWindow:: closed**](/uwp/api/windows.ui.core.corewindow.closed) 이벤트가 발생 합니다.

## <a name="api-reference-mapping-for-egl-to-direct3d-11"></a>에 대 한 API 참조 매핑-Direct3D 11에 대 한


| 안 면 API                          | 유사한 Direct3D 11 API 또는 동작                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| eglBindAPI                       | 해당 없음.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| E글 Bindteximage                  | [**ID3D11Device:: CreateTexture2D**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createtexture2d) 를 호출 하 여 2d 텍스처를 설정 합니다.                                                                                                                                                                                                                                                                                                                                                                                          |
| eglChooseConfig                  | Direct3D에서는 기본 프레임 버퍼 구성 집합을 제공 하지 않습니다. 스왑 체인의 구성                                                                                                                                                                                                                                                                                                                                                                                           |
| E글 Copybuffers                   | 버퍼 데이터를 복사 하려면 [**ID3D11DeviceContext:: CopyStructureCount**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copystructurecount)를 호출 합니다. 리소스를 복사 하려면 [**ID3DDeviceCOntext:: CopyResource**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copyresource)를 호출 합니다.                                                                                                                                                                                                                                                      |
| E글 Createcontext                 | Direct3D 장치 및 기본 Direct3D 직접 컨텍스트 ([**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 개체)에 대 한 핸들을 모두 반환 하는 [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice)를 호출 하 여 direct3d 장치 컨텍스트를 만듭니다. 반환 된 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 개체에서 [**ID3D11Device2:: CreateDeferredContext**](/windows/desktop/api/d3d11_2/nf-d3d11_2-id3d11device2-createdeferredcontext2) 를 호출 하 여 Direct3D 지연 컨텍스트를 만들 수도 있습니다. |
| E글 Createpbufferfromclientbuffer | 모든 버퍼는 [**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)와 같은 Direct3D 하위 리소스로 읽고 기록 됩니다. [**ID3D11DeviceContext1: CopyResource**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copyresource)와 같은 메서드를 사용 하 여 하나에서 다른 호환 하위 리소스 형식으로 복사 합니다.                                                                                                                                                                                                     |
| E글 Createpbuffersurface          | 스왑 체인 없이 Direct3D 장치를 만들려면 정적 [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 메서드를 호출 합니다. Direct3D 렌더링 대상 보기의 경우 [**ID3D11Device:: CreateRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview)를 호출 합니다.                                                                                                                                                                                                                               |
| eglCreatePixmapSurface           | 스왑 체인 없이 Direct3D 장치를 만들려면 정적 [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 메서드를 호출 합니다. Direct3D 렌더링 대상 보기의 경우 [**ID3D11Device:: CreateRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview)를 호출 합니다.                                                                                                                                                                                                                               |
| E글 Createwindowsurface           | Ontain [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) (표시 버퍼의 경우) 및 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) (그래픽 장치 및 해당 리소스에 대 한 가상 인터페이스). **ID3D11Device1** 를 사용 하 여 **IDXGISwapChain1**에 제공 하는 프레임 버퍼를 만드는 데 사용할 수 있는 [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) 를 정의 합니다.                                                                                         |
| eglDestroyContext                | 해당 없음. [**ID3D11DeviceContext::D iscardview1**](/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-discardview1) 를 사용 하 여 렌더링 대상 뷰를 제거 합니다. 부모 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)를 닫으려면 인스턴스를 null로 설정 하 고 플랫폼에서 리소스를 회수할 때까지 기다립니다. 장치 컨텍스트는 직접 제거할 수 없습니다.                                                                                                                                                |
| eglDestroySurface                | 해당 없음. 플랫폼에서 UWP 앱의 CoreWindow를 닫으면 그래픽 리소스가 정리 됩니다.                                                                                                                                                                                                                                                                                                                                                                                                 |
| E글 Getcurrentdisplay             | [**CoreWindow:: GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) 를 호출 하 여 현재 주 응용 프로그램 창에 대 한 참조를 가져옵니다.                                                                                                                                                                                                                                                                                                                                                         |
| E글 Getcurrentsurface             | 현재 [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview)입니다. 일반적으로이는 렌더러 개체로 범위가 지정 됩니다.                                                                                                                                                                                                                                                                                                                                                         |
| E글 Geterror                      | 오류는 DirectX 인터페이스에서 대부분의 메서드에 의해 반환 되는 Hresult로 얻어집니다. 메서드가 HRESULT를 반환 하지 않는 경우 [**GetLastError**](/windows/desktop/api/errhandlingapi/nf-errhandlingapi-getlasterror)를 호출 합니다. 시스템 오류를 HRESULT 값으로 변환 하려면 [** \_ \_ WIN32 매크로의 hresult**](/windows/desktop/api/winerror/nf-winerror-hresult_from_win32)를 사용   합니다.                                                                                                                                                                                                  |
| eglInitialize                    | [**CoreWindow:: GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) 를 호출 하 여 현재 주 응용 프로그램 창에 대 한 참조를 가져옵니다.                                                                                                                                                                                                                                                                                                                                                         |
| eglMakeCurrent                   | [**ID3D11DeviceContext1:: OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets)를 사용 하 여 현재 컨텍스트에서 그릴 렌더링 대상을 설정 합니다.                                                                                                                                                                                                                                                                                                                                  |
| E글 Querycontext                  | 해당 없음. 그러나 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 인스턴스의 렌더링 대상과 일부 구성 데이터를 가져올 수 있습니다. 사용 가능한 메서드 목록에 대 한 링크를 참조 하세요.                                                                                                                                                                                                                                                                                           |
| eglQuerySurface                  | 해당 없음. 그러나 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 인스턴스의 메서드에서 뷰포트 및 현재 그래픽 하드웨어에 대 한 데이터를 가져올 수 있습니다. 사용 가능한 메서드 목록에 대 한 링크를 참조 하세요.                                                                                                                                                                                                                                                                               |
| eglReleaseTexImage               | 해당 없음.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| E글 Releasethread                 | 일반 GPU 다중 스레딩을 보려면 [다중 스레딩](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro)을 참조 하세요.                                                                                                                                                                                                                                                                                                                                                                              |
| eglSurfaceAttrib                 | [**D3D11 \_ 렌더링 \_ 대상 \_ 뷰 \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_view_desc) 를 사용 하 여 Direct3D 렌더링 대상 보기를 구성 합니다.                                                                                                                                                                                                                                                                                                                                                               |
| E글 Swapbuffers                   | [**IDXGISwapChain1::P resent1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)를 사용 합니다.                                                                                                                                                                                                                                                                                                                                                                                                                     |
| E글 Swapinterval                  | [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1)를 참조 하세요.                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| E글 종결                     | 그래픽 파이프라인의 출력을 표시 하는 데 사용 되는 CoreWindow는 운영 체제에서 관리 합니다.                                                                                                                                                                                                                                                                                                                                                                                          |
| E글 Waitclient                    | 공유 표면의 경우 IDXGIKeyedMutex를 사용 합니다. 일반 GPU 다중 스레딩을 보려면 [다중 스레딩](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro)을 참조 하세요.                                                                                                                                                                                                                                                                                                                                    |
| eglWaitGL                        | 공유 표면의 경우 IDXGIKeyedMutex를 사용 합니다. 일반 GPU 다중 스레딩을 보려면 [다중 스레딩](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro)을 참조 하세요.                                                                                                                                                                                                                                                                                                                                    |
| E글 Waitnative                    | 공유 표면의 경우 IDXGIKeyedMutex를 사용 합니다. 일반 GPU 다중 스레딩을 보려면 [다중 스레딩](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro)을 참조 하세요.                                                                                                                                                                                                                                                                                                                                    |

 

 

 