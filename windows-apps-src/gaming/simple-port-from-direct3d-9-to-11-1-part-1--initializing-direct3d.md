---
title: Direct3D 11 초기화
description: Direct3d 장치 및 장치 컨텍스트에 대 한 핸들을 가져오는 방법 및 DXGI를 사용 하 여 스왑 체인을 설정 하는 방법을 비롯 하 여 direct3d 9 초기화 코드를 Direct3D 11로 변환 하는 방법을 보여 줍니다.
ms.assetid: 1bd5e8b7-fd9d-065c-9ff3-1a9b1c90da29
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, direct3d 11, 초기화, 포팅, direct3d 9
ms.localizationpriority: medium
ms.openlocfilehash: 576b065293f792732bb36f91c9c4117cf3f4a5c6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159177"
---
# <a name="initialize-direct3d-11"></a>Direct3D 11 초기화



**요약**

-   1 부: Direct3D 11 초기화
-   [2 부: 렌더링 프레임 워크 변환](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   [3 부: 게임 루프를 이식 합니다.](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)


Direct3d 장치 및 장치 컨텍스트에 대 한 핸들을 가져오는 방법 및 DXGI를 사용 하 여 스왑 체인을 설정 하는 방법을 비롯 하 여 direct3d 9 초기화 코드를 Direct3D 11로 변환 하는 방법을 보여 줍니다. 포트의 1 부에서는 [간단한 Direct3D 9 앱에서 DirectX 11 및 유니버설 Windows 플랫폼 (UWP) 연습을](walkthrough--simple-port-from-direct3d-9-to-11-1.md) 안내 합니다.

## <a name="initialize-the-direct3d-device"></a>Direct3D 장치 초기화


Direct3D 9에서는 [**IDirect3D9:: CreateDevice**](/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-createdevice)를 호출 하 여 direct3d 장치에 대 한 핸들을 만들었습니다. [**IDirect3D9 인터페이스**](/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3d9) 에 대 한 포인터를 가져오고 Direct3D 장치 및 스왑 체인의 구성을 제어 하기 위해 여러 매개 변수를 지정 했습니다. 이렇게 하여 [**GetDeviceCaps**](/windows/desktop/api/wingdi/nf-wingdi-getdevicecaps)를 호출하여 디바이스가 수행할 수 없는 작업을 디바이스에 요청하지 않았는지 확인했습니다.

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

Direct3D 11에서 장치 컨텍스트와 그래픽 인프라는 장치 자체와는 별개의 것으로 간주 됩니다. 초기화는 여러 단계로 구분 됩니다.

먼저 장치를 만듭니다. 장치에서 지 원하는 기능 수준 목록이 제공 됩니다 .이는 GPU에 대해 알아야 하는 사항 대부분을 알려 줍니다. 또한 Direct3D에 액세스 하기 위해 인터페이스를 만들 필요가 없습니다. 대신 [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) core API를 사용 합니다. 그러면 장치에 대 한 핸들 및 장치의 즉각적인 컨텍스트가 제공 됩니다. 장치 컨텍스트는 파이프라인 상태를 설정 하 고 렌더링 명령을 생성 하는 데 사용 됩니다.

Direct3D 11 장치 및 컨텍스트를 만든 후에는 COM 포인터 기능을 활용 하 여 추가 기능을 포함 하는 인터페이스의 최신 버전을 얻을 수 있으며 항상 권장 됩니다.

> **참고**    D3D \_ 기능 \_ 수준 \_ 9 \_ 1 (셰이더 모델 2.0에 해당)은 Microsoft Store 게임에서 지원 해야 하는 최소 수준입니다. (9 \_ 를 지원 하지 않는 경우 게임의 ARM 패키지가 인증에 실패 합니다. 1.)에 셰이더 모델 3 기능에 대 한 렌더링 경로를 포함 하는 경우 \_ \_ 배열에 D3D 기능 수준 \_ 9 \_ 3을 포함 해야 합니다.

 

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


Direct3D 11에는 DXGI (DirectX graphics infrastructure) 라는 장치 API가 포함 되어 있습니다. DXGI 인터페이스를 사용 하 여 교환 체인을 구성 하 고 공유 장치를 설정 하는 방법을 제어할 수 있습니다 (예:). Direct3D 초기화의이 단계에서는 DXGI를 사용 하 여 스왑 체인을 만듭니다. 장치를 만들었으므로 DXGI 어댑터에 대 한 인터페이스 체인을 따라갈 수 있습니다.

Direct3D 장치는 DXGI에 대해 COM 인터페이스를 구현 합니다. 먼저 해당 인터페이스를 가져와서 장치를 호스트 하는 DXGI 어댑터를 요청 하는 데 사용 해야 합니다. 그런 다음 DXGI 어댑터를 사용 하 여 DXGI 팩터리를 만듭니다.

> **참고**    이러한 인터페이스는 COM 인터페이스 이므로 첫 번째 응답은 [**QueryInterface**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))를 사용 하는 것일 수 있습니다. [**Microsoft:: WRL:: ComPtr**](/cpp/windows/comptr-class) 스마트 포인터를 대신 사용 해야 합니다. 그런 다음 [**As ()**](/previous-versions/br230426(v=vs.140)) 메서드를 호출 하 여 올바른 인터페이스 형식의 빈 COM 포인터를 제공 합니다.

 

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

이제 DXGI 팩터리가 있으므로이를 사용 하 여 스왑 체인을 만들 수 있습니다. 스왑 체인 매개 변수를 정의 해 보겠습니다. Surface 형식을 지정 해야 합니다. Direct2D와 호환 되므로 [**DXGI \_ 형식 \_ B8G8R8A8 \_ unorm**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format) 을 선택 합니다. 이 예제에서는 사용 되지 않으므로 표시 크기 조정, 다중 샘플링 및 스테레오 렌더링을 해제 합니다. CoreWindow에서 직접 실행 되므로 너비와 높이를 0으로 설정 하 고 전체 화면 값을 자동으로 가져올 수 있습니다.

> **참고**    항상 *SDKVersion* 매개 변수를 \_ \_ UWP 앱에 대 한 D3D11 SDK 버전으로 설정 합니다.

 

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

화면이 실제로 표시 될 수 있는 것 보다 더 자주 렌더링 되지 않도록 하려면 프레임 대기 시간을 1로 설정 하 고 [**DXGI 스왑 효과를 사용 하 여 \_ \_ \_ \_ 순차적으로 전환**](/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_effect)합니다. 이렇게 하면 전원이 절약 되며 매장 인증 요구 사항이 적용 됩니다. 이 연습의 2 부에서 화면에 표시 하는 방법에 대해 자세히 알아보세요.

> **참고**    렌더링 스레드가 차단 되는 동안 다른 작업을 계속 하기 위해 다중 스레딩 (예: [**ThreadPool**](/uwp/api/Windows.System.Threading) 작업 항목)을 사용할 수 있습니다.

 

**Direct3D 11**

```cpp
dxgiDevice->SetMaximumFrameLatency(1);
```

이제 렌더링을 위해 백 버퍼를 설정할 수 있습니다.

## <a name="configure-the-back-buffer-as-a-render-target"></a>백 버퍼를 렌더링 대상으로 구성


먼저 백 버퍼에 대 한 핸들을 가져와야 합니다. 백 버퍼는 DXGI 스왑 체인이 소유 하는 반면, DirectX 9에서는 Direct3D 장치에서 소유 했습니다. 그런 다음 백 버퍼를 사용 하 여 렌더링 대상 *뷰* 를 만들어 렌더링 대상으로 사용 하도록 Direct3D 장치에 지시 합니다.

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

이제 장치 컨텍스트가 재생 됩니다. 장치 컨텍스트 인터페이스를 사용 하 여 새로 만든 렌더링 대상 뷰를 사용 하도록 Direct3D에 지시 합니다. 뷰포트로 전체 창을 대상으로 지정할 수 있도록 백 버퍼의 너비와 높이를 검색 합니다. 백 버퍼는 스왑 체인에 연결 되어 있기 때문에 창 크기가 변경 되 면 (예: 사용자가 게임 창을 다른 모니터로 끌 때) 백 버퍼의 크기를 조정 해야 하 고 일부 설치를 다시 실행 해야 합니다.

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

이제 장치 핸들 및 전체 화면 렌더링 대상이 있으므로 geometry를 로드 하 고 그릴 준비가 되었습니다. [2 부: 렌더링](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)으로 계속 진행 합니다.

 

 