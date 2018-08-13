---
author: mtoepke
title: EGL 코드와 DXGI 및 Direct3D 비교
description: DXGI(DirectX Graphics Interface) 및 여러 Direct3D API는 EGL과 동일한 역할을 합니다. 이 항목은 EGL의 관점에서 DXGI 및 Direct3D 11을 이해하는 데 도움이 됩니다.
ms.assetid: 90f5ecf1-dd5d-fea3-bed8-57a228898d2a
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, egl, dxgi, direct3d
ms.openlocfilehash: 7d7e4058eccd39911bd84d3967ef07b93b6ee89d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.locfileid: "220782"
---
# <a name="compare-egl-code-to-dxgi-and-direct3d"></a>EGL 코드와 DXGI 및 Direct3D 비교


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)
-   [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)
-   [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)

DXGI(DirectX Graphics Interface) 및 여러 Direct3D API는 EGL과 동일한 역할을 합니다. 이 항목은 EGL의 관점에서 DXGI 및 Direct3D 11을 이해하는 데 도움을 줍니다.

EGL과 마찬가지로, DXGI 및 Direct3D는 그래픽 리소스를 구성하고, 그리는 셰이더에 대한 렌더링 컨텍스트를 가져오고, 결과를 창에 표시할 수 있는 메서드를 제공합니다. 그러나 DXGI 및 Direct3D에는 좀 더 많은 옵션이 있으며 EGL에서 포팅할 경우 올바르게 설정하기 위해 수행해야 할 작업이 더 많습니다.

> **참고** 이 지침은 [Khronos Group의 EGL 1.4용 개방형 사양에 기반을 두고 있으며, 이 사양은 Khronos 기본 플랫폼 그래픽 인터페이스(EGL 버전 1.4 - 2011년 4월 6일)\[PDF\]](http://www.khronos.org/registry/egl/specs/eglspec.1.4.20110406.pdf)에서 살펴볼 수 있습니다. 다른 플랫폼 및 개발 언어 관련 구문 차이는 이 지침에서 다루지 않습니다.

 

## <a name="how-does-dxgi-and-direct3d-compare"></a>DXGI와 Direct3D는 어떻게 비교할까요?


EGL이 DXGI 및 Direct3D에 대해 갖는 커다란 이점은 창 화면에 그리기를 시작하기가 비교적 간단하다는 사실입니다. 이는 OpenGL ES 2.0(및 따라서 EGL)은 여러 플랫폼 공급자에 의해 구현된 사양인 반면, DXGI 및 Direct3D는 하드웨어 공급업체 드라이버가 준수해야 하는 단일 참조이기 때문입니다. 즉, Microsoft는 특정 공급업체에서 제공하는 일부 기능에 집중하거나 공급업체별 설정 명령을 보다 단순한 API에 결합하는 대신 가능한 최고로 폭넓은 공급업체 기능 집합을 가능하게 하는 API 집합을 구현해야 하는 것입니다. 반면, Direct3D는 매우 폭넓은 범위의 그래픽 하드웨어 플랫폼 및 기능 수준을 다루고 플랫폼에 대한 경험이 풍부한 개발자에게 보다 큰 유연성을 제공하는 단일 API 집합을 제공합니다.

EGL과 마찬가지로, DXGI 및 Direct3D는 다음과 같은 동작에 대한 API를 제공합니다.

-   프레임 버퍼(DXGI에서는 "스왑 체인"이라고 함) 가져오기와 읽기 및 프레임 버퍼에 쓰기
-   프레임 버퍼와 UI 창 연결
-   그리는 렌더링 컨텍스트 가져오기 및 구성
-   특정 렌더링 컨텍스트의 그래픽 파이프라인에 대한 명령 실행
-   셰이더 리소스 만들기 및 관리, 셰이더 리소스와 렌더링 컨텍스트 연결
-   특정 렌더링 대상(예: 텍스처)으로 렌더링
-   그래픽 리소스로 렌더링한 결과로 창의 디스플레이 화면 업데이트

그래픽 파이프라인을 구성하는 기본 Direct3D 프로세스를 보려면 Microsoft Visual Studio 2015의 DirectX 11 앱(유니버설 Windows) 템플릿을 확인하세요. 이 템플릿의 기본 렌더링 클래스는 Direct3D 11 그래픽 인프라를 설정하고, 해당 기본 리소스를 구성하고, 화면 회전과 같은 UWP(유니버설 Windows 플랫폼) 앱 기능을 지원하기 위한 좋은 기준을 제공합니다.

EGL에는 Direct3D 11에 비해 매우 적은 수의 API가 포함되어 있으므로, 해당 플랫폼에 한정된 명명 및 전문 용어에 익숙하지 않은 경우 Direct3D 11을 탐색하기가 어려울 수 있습니다. 다음은 이해를 돕기 위한 간단한 개요입니다.

먼저 기본적인 EGL 개체-Direct3D 인터페이스 매핑을 검토하세요.

| EGL 추상화 | 유사한 Direct3D 표현                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EGLDisplay**  | Direct3D(UWP 앱용)에서는 [**Windows::UI::CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) API(또는 HWND를 표시하는 **ICoreWindowInterop** 인터페이스)를 통해 디스플레이 핸들을 가져옵니다. 어댑터 및 하드웨어 구성은 각각 [**IDXGIAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174523) 및 [**IDXGIDevice1**](https://msdn.microsoft.com/library/windows/desktop/hh404543) COM 인터페이스로 설정합니다.                                                                                                                                                                                                                                                           |
| **EGLSurface**  | Direct3D에서 버퍼 및 기타 창 리소스(표시 또는 오프스크린)는 [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556)([**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)(디스플레이 버퍼)과 같은 DXGI 리소스를 획득하는 데 사용되는 팩터리 패턴 구현)를 비롯한 특정 DXGI 인터페이스에 의해 만들어지고 구성됩니다. 그래픽 디바이스 및 해당 리소스를 나타내는 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)은 [**D3D11Device::CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)로 획득합니다. 렌더링 대상에 대해서는 [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) 인터페이스를 사용합니다. |
| **EGLContext**  | Direct3D에서는 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 인터페이스를 사용하여 그래픽 파이프라인을 구성하고 그래픽 파이프라인에 대한 명령을 실행합니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **EGLConfig**   | Direct3D 11에서는 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) 인터페이스의 메서드를 사용하여 버퍼, 텍스처, 스텐실 및 셰이더와 같은 그래픽 리소스를 만들고 구성합니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

 

다음은 단순 그래픽 디스플레이와 UWP 앱용 DXGI 및 Direct3D에서 리소스 및 컨텍스트를 설정하기 위한 가장 기본적인 프로세스입니다.

1.  [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589)를 호출하여 앱의 핵심 UI 스레드에 대한 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 개체의 핸들을 가져옵니다.
2.  UWP 앱의 경우 [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559)를 사용하여 [**IDXGIAdapter2**](https://msdn.microsoft.com/library/windows/desktop/hh404537)에서 스왑 체인을 가져온 다음 1단계에서 얻은 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 참조를 이 체인에 전달합니다. 그에 대한 대가로 [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631) 인스턴스를 얻게 됩니다. 렌더러 개체와 렌더링 스레드로 범위를 지정합니다.
3.  [**D3D11Device::CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 메서드를 호출하여 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) 및 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 인스턴스를 가져옵니다. 이러한 인스턴스의 범위도 렌더러 개체로 지정합니다.
4.  렌더러의 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) 개체에 대해 메서드를 사용하여 셰이더, 텍스처 및 기타 리소스를 만듭니다.
5.  렌더러의 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 개체에 대해 메서드를 사용하여 버퍼를 정의하고 셰이더를 실행한 다음 파이프라인 단계를 관리합니다.
6.  파이프라인을 실행했으며 백 버퍼에 프레임을 그렸으면 [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797)을 사용하여 이 프레임을 화면에 표시합니다.

이 프로세스를 좀더 자세히 살펴보려면 [DirectX 그래픽 시작](https://msdn.microsoft.com/library/windows/desktop/hh309467)을 참조하세요. 이 항목의 나머지 내용에서는 기본 그래픽 파이프라인 설정 및 관리에 대한 여러 일반적인 단계를 설명합니다.
> **참고** Windows 데스크톱 앱의 경우 Direct3D 스왑 체인(예: [**D3D11Device::CreateDeviceAndSwapChain**](https://msdn.microsoft.com/library/windows/desktop/ff476083))을 가져오기 위한 API가 다르며, [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 개체를 사용하지 않습니다.

 

## <a name="obtaining-a-window-for-display"></a>디스플레이를 위한 창 가져오기


이 예제에서 eglGetDisplay는 Microsoft Windows 플랫폼 관련 창 리소스를 위해 HWND로 전달됩니다. Apple의 iOS(Cocoa) 및 Google의 Android와 같은 다른 플랫폼의 경우 창 리소스에 대한 핸들이나 리소스가 다르므로 호출 구문이 서로 다를 수 있습니다. 디스플레이를 가져온 후 초기화하고, 기본 설정 구성을 설정하고, 그릴 수 있는 백 버퍼로 화면을 만듭니다.

디스플레이를 가져와서 EGL로 구성합니다.

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

Direct3D에서 UWP 앱의 주 창은 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 개체로 표현되며, 이 개체는 Direct3D용으로 구성한 "뷰 공급자" 초기화 프로세스의 일부로 [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589)를 호출하여 앱 개체에서 가져올 수 있습니다. Direct3D-XAML interop을 사용하는 경우 XAML 프레임워크의 뷰 공급자를 사용합니다. Direct3D 뷰 공급자를 만드는 프로세스는 [뷰를 표시하도록 앱을 설정하는 방법](https://msdn.microsoft.com/library/windows/apps/hh465077)에 설명되어 있습니다.

Direct3D에 대한 CoreWindow를 가져옵니다.

``` syntax
CoreWindow::GetForCurrentThread();
```

[**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 참조를 가져온 후 창을 활성화해야 합니다. 그러면 주 개체의 **Run** 메서드가 실행되고 창 이벤트 처리가 시작됩니다. 그런 다음 [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528) 구성을 기반으로 스왑 체인 리소스를 만들기 위해 [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556) 개체를 가져올 수 있도록, [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) 및 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)을 만들고 사용하여 기본 [**IDXGIDevice1**](https://msdn.microsoft.com/library/windows/desktop/ff471331) 및 [**IDXGIAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174523)를 가져옵니다.

Direct3D에 대한 CoreWindow에서 DXGI 스왑 체인을 구성 및 설정합니다.

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
  swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All Windows Store apps must use this SwapEffect.
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

프레임을 표시하기 위해 준비한 후 [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) 메서드를 호출합니다.

Direct3D 11에는 EGLSurface와 동일한 추상화가 없습니다. [**IDXGISurface1**](https://msdn.microsoft.com/library/windows/desktop/ff471343)이 있지만 다르게 사용됩니다. 개념적으로 가장 유사한 것은 [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) 개체로서, 텍스처([**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635))를 셰이더 파이프라인이 그리는 백 버퍼로 할당하는 데 사용됩니다.

Direct3D 11에서 스왑 체인의 백 버퍼를 설정합니다.

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

창을 만들거나 창 크기가 변경될 때마다 이 코드를 호출하는 것이 좋습니다. 렌더링하는 동안, 꼭짓점 버퍼 또는 셰이더와 같은 다른 하위 리소스를 설정하기 전에 [**ID3D11DeviceContext1::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464)로 렌더링 대상 뷰를 설정합니다.

``` syntax
// Set the render target for the draw operation.
m_d3dContext->OMSetRenderTargets(
        1,
        d3dRenderTargetView.GetAddressOf(),
        nullptr);
```

## <a name="creating-a-rendering-context"></a>렌더링 컨텍스트 만들기


EGL 1.4에서 "디스플레이"는 창 리소스 집합을 나타냅니다. 일반적으로 디스플레이 개체에 특성 집합을 제공하고 그에 따라 화면이 표시되는 방식으로 디스플레이를 위한 "화면"을 구성합니다. 컨텍스트를 만들고 화면 및 디스플레이에 바인딩하여 화면의 콘텐츠를 표시하기 위한 컨텍스트를 만듭니다.

호출 흐름은 일반적으로는 다음과 같습니다.

-   디스플레이 또는 창 리소스에 대한 핸들로 eglGetDisplay를 호출하여 디스플레이 개체를 가져옵니다.
-   eglInitialize로 디스플레이를 초기화합니다.
-   사용할 수 있는 디스플레이 구성을 가져오고 eglGetConfigs 및 eglChooseConfig로 하나를 선택합니다.
-   eglCreateWindowSurface로 창 화면을 만듭니다.
-   eglCreateContext로 그리기에 대한 디스플레이 컨텍스트를 만듭니다.
-   eglMakeCurrent로 디스플레이 컨텍스트를 디스플레이 및 화면에 바인딩합니다.

이전 섹션에서 EGLDisplay 및 EGLSurface를 만들었고, 이제 EGLDisplay를 사용하여 컨텍스트를 만들고 해당 컨텍스트를 디스플레이와 연결하고, 구성된 EGLSurface를 사용하여 출력을 매개 변수화합니다.

EGL 1.4에서 렌더링 컨텍스트를 가져옵니다.

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

Direct3D 11에서 렌더링 컨텍스트는 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) 개체(어댑터를 나타내며, 버퍼 및 셰이더와 같은 Direct3D 리소스를 만들 수 있음) 및 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 개체(그래픽 파이프라인을 관리하고 셰이더를 실행할 수 있음)로 표현됩니다.

Direct3D 기능 수준에 유의하세요! 이러한 기능 수준은 이전 Direct3D 하드웨어 플랫폼(DirectX 9.1~DirectX 11)을 지원하는 데 사용됩니다. 태블릿과 같은 저전력 그래픽 하드웨어를 사용하는 많은 플랫폼은 DirectX 9.1 기능에만 액세스할 수 있으며, 지원되는 이전 그래픽 하드웨어는 9.1~11일 수 있습니다.

DXGI 및 Direct3D로 렌더링 컨텍스트를 만듭니다.

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
  D3D11_SDK_VERSION, // Always set this to D3D11_SDK_VERSION for Windows Store apps.
  &device, // Returns the Direct3D device created.
  &m_featureLevel, // Returns feature level of device created.
  &d3dContext // Returns the device immediate context.
);
```

## <a name="drawing-into-a-texture-or-pixmap-resource"></a>텍스처 또는 pixmap 리소스로 그리기


OpenGL ES 2.0을 사용하여 텍스처로 그리려면 픽셀 버퍼 또는 PBuffer를 구성합니다. EGLSurface를 구성하고 만든 후 렌더링 컨텍스트에 제공하고 셰이더 파이프라인을 실행하여 텍스처로 그릴 수 있습니다.

OpenGL ES 2.0을 사용하여 픽셀 버퍼에 그립니다.

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

Direct3D 11에서 [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635) 리소스를 만들어 렌더링 대상으로 만듭니다. [**D3D11_RENDER_TARGET_VIEW_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476201)를 사용하여 렌더링 대상을 구성합니다. 이 렌더링 대상을 사용하여 [**ID3D11DeviceContext::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407) 메서드(또는 디바이스 컨텍스트에 대한 유사한 그리기\* 작업)를 호출하면 결과가 텍스처로 그려집니다.

Direct3D 11을 사용하여 텍스처로 그립니다.

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

이 텍스처가 [**ID3D11ShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476628)와 연결되어 있는 경우 이 텍스처를 셰이더로 전달할 수 있습니다.

## <a name="drawing-to-the-screen"></a>화면에 그리기


EGLContext를 사용하여 버퍼를 구성하고 데이터를 업데이트한 후 이 컨텍스트에 바인딩되어 있는 셰이더를 실행하고 glDrawElements를 사용하여 결과를 백 버퍼에 그립니다. eglSwapBuffers를 호출하여 백 버퍼를 표시합니다.

Open GL ES 2.0: 화면에 그립니다.

``` syntax
glDrawElements(GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);

eglSwapBuffers(drawContext->eglDisplay, drawContext->eglSurface);
```

Direct3D 11에서는 [**IDXGISwapChain::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797)을 사용하여 버퍼를 구성하고 셰이더를 바인딩합니다. 그런 다음 [**ID3D11DeviceContext1::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407)\* 메서드 중 하나를 호출하여 셰이더를 실행하고 스왑 체인의 백 버퍼로 구성되어 있는 렌더링 대상에 결과를 그립니다. 그런 다음 **IDXGISwapChain::Present1**을 호출하여 백 버퍼를 디스플레이에 표시하기만 하면 됩니다.

Direct3D 11: 화면에 그립니다.

``` syntax

m_d3dContext->DrawIndexed(
        m_indexCount,
        0,
        0);

// ...

m_swapChainCoreWindow->Present1(1, 0, &parameters);
```

## <a name="releasing-graphics-resources"></a>그래픽 리소스 해제


EGL에서는 EGLDisplay를 eglTerminate로 전달하여 창 리소스를 해제합니다.

EGL 1.4에서 디스플레이를 종료합니다.

```cpp
EGLBoolean eglTerminate(eglDisplay);
```

UWP 앱에서 [**CoreWindow::Close**](https://msdn.microsoft.com/library/windows/apps/br208260)를 사용하여 CoreWindow를 닫을 수 있기는 하지만 보조 UI 창에만 사용할 수 있습니다. 기본 UI 스레드 및 연결된 해당 CoreWindow는 닫을 수 없습니다. 대신, 운영 체제에 의해 만료됩니다. 그러나 보조 CoreWindow가 닫히면 [**CoreWindow::Closed**](https://msdn.microsoft.com/library/windows/apps/br208261) 이벤트가 발생합니다.

## <a name="api-reference-mapping-for-egl-to-direct3d-11"></a>EGL-Direct3D 11 API 참조 매핑


| EGL API                          | 유사한 Direct3D 11 API 또는 동작                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| eglBindAPI                       | 해당 없음.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglBindTexImage                  | [**ID3D11Device::CreateTexture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476521)를 호출하여 2D 텍스처를 설정합니다.                                                                                                                                                                                                                                                                                                                                                                                          |
| eglChooseConfig                  | Direct3D는 기본 프레임 버퍼 구성 집합을 제공하지 않습니다. 스왑 체인의 구성입니다.                                                                                                                                                                                                                                                                                                                                                                                           |
| eglCopyBuffers                   | 버퍼 데이터를 복사하려면 [**ID3D11DeviceContext::CopyStructureCount**](https://msdn.microsoft.com/library/windows/desktop/ff476393)를 호출합니다. 리소스를 복사하려면 [**ID3DDeviceCOntext::CopyResource**](https://msdn.microsoft.com/library/windows/desktop/ff476392)를 호출합니다.                                                                                                                                                                                                                                                      |
| eglCreateContext                 | Direct3D 디바이스에 대한 핸들 및 기본 Direct3D 즉각적인 컨텍스트([**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 개체)를 둘 다 반환하는 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)를 호출하여 Direct3D 디바이스 컨텍스트를 만듭니다. 또한 반환된 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) 개체에 대해 [**ID3D11Device2::CreateDeferredContext**](https://msdn.microsoft.com/library/windows/desktop/dn280495)를 호출하여 Direct3D 지연된 컨텍스트를 만들 수 있습니다. |
| eglCreatePbufferFromClientBuffer | 모든 버퍼는 Direct3D 하위 리소스(예: [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635))로 읽히고 쓰여집니다. [**ID3D11DeviceContext1:CopyResource**](https://msdn.microsoft.com/library/windows/desktop/ff476392)와 같은 메서드를 사용하여 호환되는 하위 리소스 형식 간에 복사합니다.                                                                                                                                                                                                     |
| eglCreatePbufferSurface          | 스왑 체인 없는 Direct3D 디바이스를 만들려면 정적 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 메서드를 호출합니다. Direct3D 렌더링 대상 뷰의 경우 [**ID3D11Device::CreateRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476517)를 호출합니다.                                                                                                                                                                                                                               |
| eglCreatePixmapSurface           | 스왑 체인 없는 Direct3D 디바이스를 만들려면 정적 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 메서드를 호출합니다. Direct3D 렌더링 대상 뷰의 경우 [**ID3D11Device::CreateRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476517)를 호출합니다.                                                                                                                                                                                                                               |
| eglCreateWindowSurface           | [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)(디스플레이 버퍼용) 및 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)(그래픽 디바이스 및 해당 리소스용 가상 인터페이스)을 가져옵니다. **ID3D11Device1**을 사용하여, **IDXGISwapChain1**에 제공하는 프레임 버퍼를 만드는 데 사용할 수 있는 [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)를 정의합니다.                                                                                         |
| eglDestroyContext                | 해당 없음. [**ID3D11DeviceContext::DiscardView1**](https://msdn.microsoft.com/library/windows/desktop/jj247573)을 사용하여 렌더링 대상 뷰를 제거합니다. 부모 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)을 닫으려면 인스턴스를 null로 설정하고 플랫폼이 리소스를 회수할 때까지 기다립니다. 디바이스 컨텍스트를 직접 삭제할 수는 없습니다.                                                                                                                                                |
| eglDestroySurface                | 해당 없음. UWP 앱의 CoreWindow가 플랫폼에 의해 닫히면 그래픽 리소스가 정리됩니다.                                                                                                                                                                                                                                                                                                                                                                                                 |
| eglGetCurrentDisplay             | [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589)를 호출하여 현재 메인 앱 창에 대한 참조를 가져옵니다.                                                                                                                                                                                                                                                                                                                                                         |
| eglGetCurrentSurface             | 현재 [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)입니다. 일반적으로 렌더러 개체로 범위가 지정됩니다.                                                                                                                                                                                                                                                                                                                                                         |
| eglGetError                      | 오류는 DirectX 인터페이스에 있는 대부분의 메서드에 의해 반환되는 HRESULT로 표시됩니다. 메서드가 HRESULT를 반환하지 않으면 [**GetLastError**](https://msdn.microsoft.com/library/windows/desktop/ms679360)를 호출합니다. 시스템 오류를 HRESULT 값으로 변환하려면 [**HRESULT\_FROM\_WIN32**](https://msdn.microsoft.com/library/windows/desktop/ms680746) 매크로를 사용합니다.                                                                                                                                                                                                  |
| eglInitialize                    | [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589)를 호출하여 현재 메인 앱 창에 대한 참조를 가져옵니다.                                                                                                                                                                                                                                                                                                                                                         |
| eglMakeCurrent                   | [**ID3D11DeviceContext1::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464)를 사용하여 현재 컨텍스트에서 그리기 위한 렌더링 대상을 설정합니다.                                                                                                                                                                                                                                                                                                                                  |
| eglQueryContext                  | 해당 없음. 그러나 일부 구성 데이터 및 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) 인스턴스에서 렌더링 대상을 가져올 수 있습니다. (사용 가능한 메서드 목록은 링크를 참조하세요.)                                                                                                                                                                                                                                                                                           |
| eglQuerySurface                  | 해당 없음. 그러나 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) 인스턴스의 메서드에서 뷰포트 및 현재 그래픽 하드웨어에 대한 데이터를 가져올 수 있습니다. (사용 가능한 메서드 목록은 링크를 참조하세요.)                                                                                                                                                                                                                                                                               |
| eglReleaseTexImage               | 해당 없음.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglReleaseThread                 | 일반 GPU 다중 스레딩에 대해서는 [다중 스레딩](https://msdn.microsoft.com/library/windows/desktop/ff476891)을 읽어 보세요.                                                                                                                                                                                                                                                                                                                                                                              |
| eglSurfaceAttrib                 | [**D3D11\_RENDER\_TARGET\_VIEW\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476201)를 사용하여 Direct3D 렌더링 대상 뷰를 구성합니다.                                                                                                                                                                                                                                                                                                                                                               |
| eglSwapBuffers                   | [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797)을 사용합니다.                                                                                                                                                                                                                                                                                                                                                                                                                     |
| eglSwapInterval                  | [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)을 참조하세요.                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| eglTerminate                     | 그래픽 파이프라인의 출력을 표시하는 데 사용되는 CoreWindow는 운영 체제에 의해 관리됩니다.                                                                                                                                                                                                                                                                                                                                                                                          |
| eglWaitClient                    | 공유 화면의 경우 IDXGIKeyedMutex를 사용합니다. 일반 GPU 다중 스레딩에 대해서는 [다중 스레딩](https://msdn.microsoft.com/library/windows/desktop/ff476891)을 읽어 보세요.                                                                                                                                                                                                                                                                                                                                    |
| eglWaitGL                        | 공유 화면의 경우 IDXGIKeyedMutex를 사용합니다. 일반 GPU 다중 스레딩에 대해서는 [다중 스레딩](https://msdn.microsoft.com/library/windows/desktop/ff476891)을 읽어 보세요.                                                                                                                                                                                                                                                                                                                                    |
| eglWaitNative                    | 공유 화면의 경우 IDXGIKeyedMutex를 사용합니다. 일반 GPU 다중 스레딩에 대해서는 [다중 스레딩](https://msdn.microsoft.com/library/windows/desktop/ff476891)을 읽어 보세요.                                                                                                                                                                                                                                                                                                                                    |

 

 

 




