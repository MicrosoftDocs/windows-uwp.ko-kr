---
author: mtoepke
title: 깊이 버퍼 장치 리소스 만들기
description: 섀도 볼륨에 대한 깊이 테스트를 지원하는 데 필요한 Direct3D 디바이스 리소스를 만드는 방법을 알아봅니다.
ms.assetid: 86d5791b-1faa-17e4-44a8-bbba07062756
---

# 깊이 버퍼 장치 리소스 만들기


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


섀도 볼륨에 대한 깊이 테스트를 지원하는 데 필요한 Direct3D 디바이스 리소스를 만드는 방법을 알아봅니다. [연습: Direct3D 11에서 깊이 버퍼를 사용하여 섀도 볼륨 구현](implementing-depth-buffers-for-shadow-mapping.md)의 1부입니다.

## 필요한 리소스


섀도 볼륨에 대한 깊이 맵을 렌더링하려면 다음과 같은 Direct3D 장치 종속 리소스가 필요합니다.

-   깊이 맵에 대한 리소스(버퍼)
-   리소스에 대한 깊이 스텐실 보기 및 셰이더 리소스 보기
-   비교 샘플러 상태 개체
-   라이트 POV 행렬용 상수 버퍼
-   그림자 맵을 렌더링하기 위한 뷰포트(일반적으로 정사각형 뷰포트)
-   앞면 컬링을 가능하게 해주는 렌더링 상태 개체
-   아직 사용하지 않는 경우 뒷면 컬링으로 전환하기 위한 렌더링 상태 개체도 필요합니다.

이러한 리소스 만들기는 장치 종속 리소스 만들기 루틴에 포함되어야 하는데, 예를 들어 새 장치 드라이버를 설치하거나 사용자가 다른 그래픽 어댑터에 연결된 모니터로 앱을 이동하는 경우 그와 같이 렌더러가 해당 리소스를 다시 만들 수 있습니다.

## 기능 지원 확인


깊이 맵을 만들기 전에 Direct3D 디바이스에서 [**CheckFeatureSupport**](https://msdn.microsoft.com/library/windows/desktop/ff476497) 메서드를 호출하고 **D3D11\_FEATURE\_D3D9\_SHADOW\_SUPPORT**를 요청한 다음 [**D3D11\_FEATURE\_DATA\_D3D9\_SHADOW\_SUPPORT**](https://msdn.microsoft.com/library/windows/desktop/jj247569) 구조를 제공합니다.

```cpp
D3D11_FEATURE_DATA_D3D9_SHADOW_SUPPORT isD3D9ShadowSupported;
ZeroMemory(&isD3D9ShadowSupported, sizeof(isD3D9ShadowSupported));
pD3DDevice->CheckFeatureSupport(
    D3D11_FEATURE_D3D9_SHADOW_SUPPORT,
    &isD3D9ShadowSupported,
    sizeof(D3D11_FEATURE_D3D9_SHADOW_SUPPORT)
    );

if (isD3D9ShadowSupported.SupportsDepthAsTextureWithLessEqualComparisonFilter)
{
    // Init shadow map resources

```

이 기능이 지원되지 않는 경우 단순 비교 함수를 호출하는 셰이더 모델 4 수준 9\_x에 대해 컴파일된 셰이더를 로드하지 마세요. 대부분의 경우 이 기능이 지원되지 않는다는 것은 GPU가 최소한 WDDM 1.2를 지원하도록 업데이트되지 않은 드라이버가 있는 레거시 장치임을 의미합니다. 디바이스가 기능 레벨 10\_0 이상을 지원하는 경우 셰이더 모델 4\_0에 대해 컴파일된 샘플 비교 셰이더를 로드할 수 있습니다.

## 깊이 버퍼 만들기


먼저, 높은 정밀도 깊이 형식으로 깊이 맵을 만들어 봅니다. 먼저, 일치하는 셰이더 리소스 보기 속성을 설정합니다. 장치 메모리가 부족하거나 하드웨어에서 지원하지 않는 형식 등으로 인해 리소스 만들기에 실패하면 낮은 정밀도 형식을 시도해 보고 일치시킬 속성을 변경합니다.

이 단계는 선택적 단계로서, 중간 해상도 Direct3D 기능 수준 9\_1 디바이스에서 렌더링하는 경우 등 낮은 정밀도 깊이 형식만 필요한 경우에 수행합니다.

```cpp
D3D11_TEXTURE2D_DESC shadowMapDesc;
ZeroMemory(&shadowMapDesc, sizeof(D3D11_TEXTURE2D_DESC));
shadowMapDesc.Format = DXGI_FORMAT_R24G8_TYPELESS;
shadowMapDesc.MipLevels = 1;
shadowMapDesc.ArraySize = 1;
shadowMapDesc.SampleDesc.Count = 1;
shadowMapDesc.BindFlags = D3D11_BIND_SHADER_RESOURCE | D3D11_BIND_DEPTH_STENCIL;
shadowMapDesc.Height = static_cast<UINT>(m_shadowMapDimension);
shadowMapDesc.Width = static_cast<UINT>(m_shadowMapDimension);

HRESULT hr = pD3DDevice->CreateTexture2D(
    &shadowMapDesc,
    nullptr,
    &m_shadowMap
    );
```

그런 다음 리소스 보기를 만듭니다. 깊이 스텐실 보기에서 MIP 조각을 0으로 설정하고, 셰이더 리소스 보기에서 MIP 수준을 1로 설정합니다. 이러한 두 보기 모두에 TEXTURE2D의 텍스처 차원이 있고, 이러한 두 보기 모두가 일치하는 [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059) 형식을 사용해야 합니다.

```cpp
D3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc;
ZeroMemory(&depthStencilViewDesc, sizeof(D3D11_DEPTH_STENCIL_VIEW_DESC));
depthStencilViewDesc.Format = DXGI_FORMAT_D24_UNORM_S8_UINT;
depthStencilViewDesc.ViewDimension = D3D11_DSV_DIMENSION_TEXTURE2D;
depthStencilViewDesc.Texture2D.MipSlice = 0;

D3D11_SHADER_RESOURCE_VIEW_DESC shaderResourceViewDesc;
ZeroMemory(&shaderResourceViewDesc, sizeof(D3D11_SHADER_RESOURCE_VIEW_DESC));
shaderResourceViewDesc.ViewDimension = D3D11_SRV_DIMENSION_TEXTURE2D;
shaderResourceViewDesc.Format = DXGI_FORMAT_R24_UNORM_X8_TYPELESS;
shaderResourceViewDesc.Texture2D.MipLevels = 1;

hr = pD3DDevice->CreateDepthStencilView(
    m_shadowMap.Get(),
    &depthStencilViewDesc,
    &m_shadowDepthView
    );

hr = pD3DDevice->CreateShaderResourceView(
    m_shadowMap.Get(),
    &shaderResourceViewDesc,
    &m_shadowResourceView
    );
```

## 비교 상태 만들기


이제 비교 샘플러 상태 개체를 만듭니다. 기능 수준 9\_1은 D3D11\_COMPARISON\_LESS\_EQUAL만 지원합니다. 필터링 선택 사항은 [다양한 하드웨어에서 그림자 맵 지원](target-a-range-of-hardware.md)에 자세히 설명되어 있으며, 점 필터링을 선택하여 빠르게 그림자 맵을 사용할 수 있습니다.

D3D11\_TEXTURE\_ADDRESS\_BORDER 주소 모드를 지정할 수 있으며, 이 주소 모드를 지정하면 기능 수준 9\_1 디바이스에서 작동합니다. 이 내용은 깊이 테스트를 하기 전에 픽셀이 조명의 시각 절두체에 있는지 여부를 테스트하지 않는 픽셀 셰이더에 적용됩니다. 각 테두리에 대해 0 또는 1을 지정하여 조명의 시각 절두체 밖에 있는 픽셀이 깊이 테스트를 통과하는지 아니면 실패하는지를 제어할 수 있으므로 픽셀이 켜지는지 여부를 제어할 수 있습니다.

기능 수준 9\_1에서 다음 필수 값을 설정해야 합니다. **MinLOD**를 0으로 설정하고, **MaxLOD**를 **D3D11\_FLOAT32\_MAX**로 설정하고 **MaxAnisotropy**를 0으로 설정합니다.

```cpp
D3D11_SAMPLER_DESC comparisonSamplerDesc;
ZeroMemory(&comparisonSamplerDesc, sizeof(D3D11_SAMPLER_DESC));
comparisonSamplerDesc.AddressU = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressV = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressW = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.BorderColor[0] = 1.0f;
comparisonSamplerDesc.BorderColor[1] = 1.0f;
comparisonSamplerDesc.BorderColor[2] = 1.0f;
comparisonSamplerDesc.BorderColor[3] = 1.0f;
comparisonSamplerDesc.MinLOD = 0.f;
comparisonSamplerDesc.MaxLOD = D3D11_FLOAT32_MAX;
comparisonSamplerDesc.MipLODBias = 0.f;
comparisonSamplerDesc.MaxAnisotropy = 0;
comparisonSamplerDesc.ComparisonFunc = D3D11_COMPARISON_LESS_EQUAL;
comparisonSamplerDesc.Filter = D3D11_FILTER_COMPARISON_MIN_MAG_MIP_POINT;

// Point filtered shadows can be faster, and may be a good choice when
// rendering on hardware with lower feature levels. This sample has a
// UI option to enable/disable filtering so you can see the difference
// in quality and speed.

DX::ThrowIfFailed(
    pD3DDevice->CreateSamplerState(
        &comparisonSamplerDesc,
        &m_comparisonSampler_point
        )
    );
```

## 렌더 상태 만들기


이제 앞면 컬링을 사용하도록 설정하는 데 사용할 수 있는 렌더 상태를 만듭니다. 기능 수준 9\_1 디바이스를 사용하려면 **DepthClipEnable**을 **true**로 설정해야 합니다.

```cpp
D3D11_RASTERIZER_DESC drawingRenderStateDesc;
ZeroMemory(&drawingRenderStateDesc, sizeof(D3D11_RASTERIZER_DESC));
drawingRenderStateDesc.CullMode = D3D11_CULL_BACK;
drawingRenderStateDesc.FillMode = D3D11_FILL_SOLID;
drawingRenderStateDesc.DepthClipEnable = true; // Feature level 9_1 requires DepthClipEnable == true
DX::ThrowIfFailed(
    pD3DDevice->CreateRasterizerState(
        &drawingRenderStateDesc,
        &m_drawingRenderState
        )
    );
```

뒷면 컬링을 사용하도록 설정하는 데 사용할 수 있는 렌더 상태를 만듭니다. 렌더링 코드에서 이미 뒷면 컬링을 켠 경우 이 단계를 건너뛸 수 있습니다.

```cpp
D3D11_RASTERIZER_DESC shadowRenderStateDesc;
ZeroMemory(&shadowRenderStateDesc, sizeof(D3D11_RASTERIZER_DESC));
shadowRenderStateDesc.CullMode = D3D11_CULL_FRONT;
shadowRenderStateDesc.FillMode = D3D11_FILL_SOLID;
shadowRenderStateDesc.DepthClipEnable = true;

DX::ThrowIfFailed(
    pD3DDevice->CreateRasterizerState(
        &shadowRenderStateDesc,
        &m_shadowRenderState
        )
    );
```

## 상수 버퍼 만들기


조명의 관점에서 렌더링을 위한 상수 버퍼를 만드는 것을 잊지 마세요. 또한 이 상수 버퍼를 사용하여 셰이더에 조명 위치를 지정할 수 있습니다. 점 광원에 대해 원근 행렬을 사용하고, 방향성 광원(예제: 햇빛)에 대해 직교 행렬을 사용합니다.

```cpp
DX::ThrowIfFailed(
    m_deviceResources->GetD3DDevice()->CreateBuffer(
        &viewProjectionConstantBufferDesc,
        nullptr,
        &m_lightViewProjectionBuffer
        )
    );
```

상수 버퍼 데이터를 채웁니다. 초기화하는 동안 상수 버퍼를 한 번 업데이트하고 이전 프레임 이후 조명 값이 변경된 경우 다시 업데이트합니다.

```cpp
{
    XMMATRIX lightPerspectiveMatrix = XMMatrixPerspectiveFovRH(
        XM_PIDIV2,
        1.0f,
        12.f,
        50.f
        );

    XMStoreFloat4x4(
        &m_lightViewProjectionBufferData.projection,
        XMMatrixTranspose(lightPerspectiveMatrix)
        );

    // Point light at (20, 15, 20), pointed at the origin. POV up-vector is along the y-axis.
    static const XMVECTORF32 eye = { 20.0f, 15.0f, 20.0f, 0.0f };
    static const XMVECTORF32 at = { 0.0f, 0.0f, 0.0f, 0.0f };
    static const XMVECTORF32 up = { 0.0f, 1.0f, 0.0f, 0.0f };

    XMStoreFloat4x4(
        &m_lightViewProjectionBufferData.view,
        XMMatrixTranspose(XMMatrixLookAtRH(eye, at, up))
        );

    // Store the light position to help calculate the shadow offset.
    XMStoreFloat4(&m_lightViewProjectionBufferData.pos, eye);
}
```

```cpp
context->UpdateSubresource(
    m_lightViewProjectionBuffer.Get(),
    0,
    NULL,
    &m_lightViewProjectionBufferData,
    0,
    0
    );
```

## 뷰포트 만들기


그림자 맵으로 렌더링하기 위한 별도의 뷰포트가 필요합니다. 뷰포트는 장치 기반 리소스가 아니므로 코드의 어느 곳에나 만들 수 있습니다. 그림자 맵과 함께 뷰포트를 만들면 그림자 맵 차원에 적절한 뷰포트의 차원을 보다 편리하게 유지할 수 있습니다.

```cpp
// Init viewport for shadow rendering
ZeroMemory(&m_shadowViewport, sizeof(D3D11_VIEWPORT));
m_shadowViewport.Height = m_shadowMapDimension;
m_shadowViewport.Width = m_shadowMapDimension;
m_shadowViewport.MinDepth = 0.f;
m_shadowViewport.MaxDepth = 1.f;
```

이 연습의 다음 부분에서 [깊이 버퍼로 렌더링](render-the-shadow-map-to-the-depth-buffer.md)하여 그림자 맵을 만드는 방법을 알아보세요.

 

 






<!--HONumber=May16_HO2-->


