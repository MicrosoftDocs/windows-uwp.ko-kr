---
title: Direct3D 11 초기화
description: Direct3D 디바이스와 디바이스 컨텍스트로 핸들을 가져오는 방법 및 DXGI를 사용하여 스왑 체인을 설정하는 방법을 포함하여 Direct3D 11로 Direct3D 9 초기화 코드를 변환하는 방법을 보여 줍니다.
ms.assetid: 1bd5e8b7-fd9d-065c-9ff3-1a9b1c90da29
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, direct3d 11, 초기화, 포팅, direct3d 9
ms.localizationpriority: medium
ms.openlocfilehash: c5a7f33ddbc6d70af5293b92165892c2098e452d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368029"
---
# <a name="initialize-direct3d-11"></a>Direct3D 11 초기화



**요약**

-   1단계: Direct3D 11 초기화
-   [2 부: 렌더링 프레임 워크를 변환 합니다.](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   [3 부: 게임 루프 포트](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)


Direct3D 디바이스와 디바이스 컨텍스트로 핸들을 가져오는 방법 및 DXGI를 사용하여 스왑 체인을 설정하는 방법을 포함하여 Direct3D 11로 Direct3D 9 초기화 코드를 변환하는 방법을 보여 줍니다. [간단한 Direct3D 9 앱을 DirectX 11 및 UWP(유니버설 Windows 플랫폼)으로 포팅](walkthrough--simple-port-from-direct3d-9-to-11-1.md) 연습의 1부.

## <a name="initialize-the-direct3d-device"></a>Direct3D 디바이스 초기화


Direct3D 9에서 [**IDirect3D9::CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-createdevice)를 호출하여 Direct3D 디바이스에 대한 핸들을 만들었습니다. [  **IDirect3D9 interface**](https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3d9)로 포인터를 가져오는 것으로 시작했으며 Direct3D 디바이스 및 스왑 체인의 구성을 제어하기 위한 여러 가지 매개 변수를 지정했습니다. 이렇게 하여 [**GetDeviceCaps**](https://docs.microsoft.com/windows/desktop/api/wingdi/nf-wingdi-getdevicecaps)를 호출하여 디바이스가 수행할 수 없는 작업을 디바이스에 요청하지 않았는지 확인했습니다.

Direct3D 9

```cpp
UINT32 AdapterOrdinal = 0;
D3DDEVTYPE DeviceType = D3DDEVTYPE_HAL;
D3DCAPS9 caps;
m_pD3D->GetDeviceCaps(AdapterOrdinal, DeviceType, &caps); // caps bits

D3DPRESENT_PARAMETERS params;
ZeroMemory(&params, sizeof(D3DPRESENT_PARAMETERS));

// Swap chain parameters:
params.hDeviceWindow = m_hWnd;
params.AutoDepthStencilFormat = D3DFMT_D24X8;
params.BackBufferFormat = D3DFMT_X8R8G8B8;
params.MultiSampleQuality = D3DMULTISAMPLE_NONE;
params.MultiSampleType = D3DMULTISAMPLE_NONE;
params.SwapEffect = D3DSWAPEFFECT_DISCARD;
params.Windowed = true;
params.PresentationInterval = 0;
params.BackBufferCount = 2;
params.BackBufferWidth = 0;
params.BackBufferHeight = 0;
params.EnableAutoDepthStencil = true;
params.Flags = 2;

m_pD3D->CreateDevice(
    0,
    D3DDEVTYPE_HAL,
    m_hWnd,
    64,
    &params,
    &m_pd3dDevice
    );
```

Direct3D 11에서 디바이스 컨텍스트 및 그래픽 인프라는 장치 자체에서 별도로 간주됩니다. 초기화는 여러 단계로 나뉩니다.

먼저 장치를 만듭니다. 기능 수준 및 장치 지원 목록을 가져옵니다. 이 목록은 GPU에 대해 알아야 하는 대부분을 알려줍니다. 또한 Direct3D에 액세스하기 위해 인터페이스를 만들 필요가 없습니다. 대신 [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 핵심 API를 사용합니다. 이렇게 하면 장치와 장치의 즉각적인 컨텍스트에 대한 핸들이 제공됩니다. 디바이스 컨텍스트는 파이프라인 상태를 설정하고 렌더링 명령을 생성하는 데 사용됩니다.

Direct3D 11 디바이스 및 컨텍스트를 만든 후 COM 포인터 기능을 활용하여 최신 버전의 인터페이스를 가져옵니다. 이 인터페이스는 추가 기능을 포함하며 항상 권장됩니다.

> **참고**    D3D\_기능\_수준\_9\_1 (-2.0 셰이더 모델에 해당)은 Microsoft Store 게임은 지 원하는 데 필요한 최소 수준입니다. (9를 지원 하지 않는 경우 게임의 ARM 패키지 인증 하지 못합니다\_1.) 게임에는 또한 셰이더 모델 3 기능에 대 한 렌더링 패스 경우 D3D 포함할지\_기능\_수준\_9\_배열에는 3입니다.

 

Direct3D 11

```cpp
// This flag adds support for surfaces with a different color channel 
// ordering than the API default. It is required for compatibility with
// Direct2D.
UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
// If the project is in a debug build, enable debugging via SDK Layers.
creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

// This example only uses feature level 9.1.
D3D_FEATURE_LEVEL featureLevels[] = 
{
    D3D_FEATURE_LEVEL_9_1
};

// Create the Direct3D 11 API device object and a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> context;
D3D11CreateDevice(
    nullptr, // Specify nullptr to use the default adapter.
    D3D_DRIVER_TYPE_HARDWARE,
    nullptr,
    creationFlags,
    featureLevels,
    ARRAYSIZE(featureLevels),
    D3D11_SDK_VERSION, // UWP apps must set this to D3D11_SDK_VERSION.
    &device, // Returns the Direct3D device created.
    nullptr,
    &context // Returns the device immediate context.
    );

// Store pointers to the Direct3D 11.2 API device and immediate context.
device.As(&m_d3dDevice);

context.As(&m_d3dContext);
```

## <a name="create-a-swap-chain"></a>스왑 체인 만들기


Direct3D 11에는 DXGI(DirectX 그래픽 인프라)를 호출하는 장치 API가 포함되어 있습니다. DXGI 인터페이스에서는 예를 들어 스왑 체인을 구성하고 공유 장치를 설정하는 방법을 제어할 수 있습니다. Direct3D 초기화의 이 단계에서 DXGI를 사용하여 스왑 체인을 만들도록 하겠습니다. 장치를 만들었기 때문에, DXGI 어댑터에 대한 인터페이스 체인을 다시 수행할 수 있습니다.

Direct3D 장치는 DXGI에 대한 COM 인터페이스를 구현합니다. 먼저 해당 인터페이스를 가져오고 이 인터테이스를 사용하여 장치를 호스트하는 DXGI 어댑터를 요청합니다. 그런 다음 DXGI 어댑터를 사용하여 DXGI 팩터리를 만듭니다.

> **참고**    COM 인터페이스를 사용 하 여 첫 번째 응답 될 수 이들은 [ **QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))합니다. 대신 [**Microsoft::WRL::ComPtr**](https://docs.microsoft.com/cpp/windows/comptr-class) 포인터 스마트를 사용해야 합니다. 그런 다음 [**As()** ](https://docs.microsoft.com/previous-versions/br230426(v=vs.140)) 메서드를 호출하고 올바른 인터페이스 유형의 빈 COM 포인터를 제공합니다.

 

**Direct3D 11**

```cpp
ComPtr<IDXGIDevice2> dxgiDevice;
m_d3dDevice.As(&dxgiDevice);

// Then, the adapter hosting the device;
ComPtr<IDXGIAdapter> dxgiAdapter;
dxgiDevice->GetAdapter(&dxgiAdapter);

// Then, the factory that created the adapter interface:
ComPtr<IDXGIFactory2> dxgiFactory;
dxgiAdapter->GetParent(
    __uuidof(IDXGIFactory2),
    &dxgiFactory
    );
```

이제 DXGI 팩터리가 있으므로 이 팩터리를 사용하여 스왑 체인을 만들 수 있습니다. 스왑 체인 매개 변수를 정의하도록 하겠습니다. 노출 된 형식으로 지정 해야 선택 [ **DXGI\_형식\_B8G8R8A8\_UNORM** ](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format) Direct2D를 사용 하 여 호환 되므로 합니다. 이 예제에서 사용되지 않기 때문에 디스플레이 배율, 다중 샘플링 및 스테레오 렌더링을 끄겠습니다. CoreWindow에서 직접 실행 중이기 때문에 너비와 높이가 0으로 설정된 상태로 두고 전체 화면 값을 자동으로 가져올 수 있습니다.

> **참고**    항상 설정 된 *SDKVersion* D3D11 매개 변수\_SDK\_UWP 앱에 대 한 버전입니다.

 

**Direct3D 11**

```cpp
ComPtr<IDXGISwapChain1> swapChain;
dxgiFactory->CreateSwapChainForCoreWindow(
    m_d3dDevice.Get(),
    reinterpret_cast<IUnknown*>(window),
    &swapChainDesc,
    nullptr,
    &swapChain
    );
swapChain.As(&m_swapChain);
```

화면 실제로 표시할 수 있는 보다 더 자주 렌더링 되지 것을 보장 하려면 1을 사용 하 여 프레임 대기 시간 설정 [ **DXGI\_교환\_효과\_대칭 이동\_순차** ](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_effect). 이는 전원을 절약하며 스토어 인증 요구 사항입니다. 이 연습의 2부에서는 화면에 표시하는 것에 대한 자세한 내용은 알아봅니다.

> **참고**    따르면 다중 스레딩 (예를 들어 [ **ThreadPool** ](https://docs.microsoft.com/uwp/api/Windows.System.Threading) 작업 항목) 렌더링 스레드가 차단 된 동안 다른 작업을 계속 하려면.

 

**Direct3D 11**

```cpp
dxgiDevice->SetMaximumFrameLatency(1);
```

이제 렌더링을 위한 백 버퍼를 설정할 수 있습니다.

## <a name="configure-the-back-buffer-as-a-render-target"></a>백 버퍼를 렌더링 대상으로 구성


먼저 백 버퍼에 대한 핸들을 가져와야 합니다. (Note DXGI 스왑 체인 백 버퍼가 소유는 Direct3D 장치에서 소유 된 DirectX 9에서 반면.) 렌더링 대상을 만들어 렌더링 대상으로 사용 하도록 Direct3D 장치를 지정할 것 *보기* 백 버퍼를 사용 하 여 합니다.

**Direct3D 11**

```cpp
ComPtr<ID3D11Texture2D> backBuffer;
m_swapChain->GetBuffer(
    0,
    __uuidof(ID3D11Texture2D),
    &backBuffer
    );

// Create a render target view on the back buffer.
m_d3dDevice->CreateRenderTargetView(
    backBuffer.Get(),
    nullptr,
    &m_renderTargetView
    );
```

이제 디바이스 컨텍스트가 기능을 합니다. 디바이스 컨텍스트 인터페이스를 사용하여 Direct3D에서 새로 만든 렌더링 대상 보기를 사용하도록 지시합니다. 뷰포트로 전체 창을 대상으로 지정할 수 있도록 백 버퍼의 너비와 높이를 검색할 것입니다. 백 버퍼는 스왑 체인에 연결되어 있으므로 창 크기가 변경되는 경우(예: 사용자가 게임 창을 다른 모니터로 끄는 경우) 백 버퍼의 크기를 조정해야 하고 일부 설정을 다시 실행해야 합니다.

**Direct3D 11**

```cpp
D3D11_TEXTURE2D_DESC backBufferDesc = {0};
backBuffer->GetDesc(&backBufferDesc);

CD3D11_VIEWPORT viewport(
    0.0f,
    0.0f,
    static_cast<float>(backBufferDesc.Width),
    static_cast<float>(backBufferDesc.Height)
    );

m_d3dContext->RSSetViewports(1, &viewport);
```

이제 장치 핸들과 전체 화면 렌더링 대상이 있으므로 기하 도형을 로드하고 그릴 수 있습니다. 계속 해 서 [2 부: 렌더링](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)합니다.

 

 




