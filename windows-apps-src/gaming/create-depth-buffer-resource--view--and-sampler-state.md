---
title: 깊이 버퍼 장치 리소스 만들기
description: 섀도 볼륨의 깊이 테스트를 지 원하는 데 필요한 Direct3D 장치 리소스를 만드는 방법에 대해 알아봅니다.
ms.assetid: 86d5791b-1faa-17e4-44a8-bbba07062756
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, direct3d, 깊이 버퍼
ms.localizationpriority: medium
ms.openlocfilehash: 0032d77bb8d572229ea77df736c807a0a85e9ecb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175377"
---
# <a name="create-depth-buffer-device-resources"></a>깊이 버퍼 장치 리소스 만들기




섀도 볼륨의 깊이 테스트를 지 원하는 데 필요한 Direct3D 장치 리소스를 만드는 방법에 대해 알아봅니다. 연습 1 부 [: Direct3D 11에서 깊이 버퍼를 사용 하 여 섀도 볼륨 구현](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="resources-youll-need"></a>필요한 리소스


섀도 볼륨에 대 한 깊이 맵을 렌더링 하려면 다음 Direct3D 장치 종속 리소스가 필요 합니다.

-   깊이 맵의 리소스 (버퍼)
-   리소스에 대 한 깊이 스텐실 뷰 및 셰이더 리소스 뷰
-   비교 샘플러 상태 개체입니다.
-   옅은 POV 행렬의 상수 버퍼
-   그림자 맵을 렌더링 하기 위한 뷰포트 (일반적으로 정사각형 뷰포트)
-   Front face 고르기을 사용 하도록 설정 하는 렌더링 상태 개체
-   또한 아직 사용 하지 않는 경우 뒤로 얼굴 고르기 다시 전환 하려면 렌더링 상태 개체가 필요 합니다.

이러한 리소스 생성은 장치 종속 리소스 생성 루틴에 포함 되어야 합니다. 즉, 새 장치 드라이버가 설치 되어 있거나 사용자가 다른 그래픽 어댑터에 연결 된 모니터로 앱을 이동 하는 경우 렌더러에서 다시 만들 수 있습니다.

## <a name="check-feature-support"></a>기능 지원 확인


깊이 맵을 만들기 전에 Direct3D 장치에서 [**CheckFeatureSupport**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport) 메서드를 호출 하 고, **D3D11 \_ 기능 \_ D3D9 \_ 섀도 \_ 지원을**요청 하 고, [**D3D11 \_ 기능 \_ DATA \_ D3D9 \_ shadow \_ 지원**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_feature_data_d3d9_shadow_support) 구조를 제공 합니다.

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

이 기능이 지원 되지 않는 경우 \_ 샘플 비교 함수를 호출 하는 셰이더 모델 4 수준 9 x에 대해 컴파일된 셰이더를 로드 하지 마십시오. 대부분의 경우이 기능을 지원 하지 않는 것은 GPU가 WDDM 1.2 이상을 지원 하도록 업데이트 되지 않은 드라이버를 사용 하는 레거시 장치 임을 의미 합니다. 장치에서 최소 기능 수준 10 0을 지 원하는 경우 \_ 셰이더 모델 4 0에 대해 컴파일된 샘플 비교 셰이더를 대신 로드할 수 있습니다 \_ .

## <a name="create-depth-buffer"></a>깊이 버퍼 만들기


먼저 더 높은 정밀도 깊이 형식의 깊이 맵을 만들어 보세요. 먼저 일치 하는 셰이더 리소스 뷰 속성을 설정 합니다. 장치 메모리가 부족 하거나 하드웨어가 지원 하지 않는 형식이 기 때문에 리소스 만들기가 실패 하는 경우에는 더 낮은 정밀도 형식으로 시도 하 고 일치 하는 속성을 변경 합니다.

중간 해상도 Direct3D 기능 수준 9 1 장치에서 렌더링 하는 경우와 같이 낮은 정밀도 깊이 형식만 필요한 경우에는이 단계를 선택 하는 것이 좋습니다 \_ .

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

그런 다음 리소스 뷰를 만듭니다. 깊이 스텐실 뷰에서 밉 조각을 0으로 설정 하 고 셰이더 리소스 뷰에서 밉 레벨을 1로 설정 합니다. 둘 다 TEXTURE2D의 질감 차원을 가지 며 둘 다 일치 하는 [**DXGI \_ 형식을**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)사용 해야 합니다.

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

## <a name="create-comparison-state"></a>비교 상태 만들기


이제 비교 샘플러 상태 개체를 만듭니다. 기능 수준 9 \_ 1은 D3D11 \_ 비교가 \_ 더 작음을 지원 \_ 합니다. 필터링 선택은 [하드웨어 범위에서 섀도 맵 지원](target-a-range-of-hardware.md) 에 자세히 설명 되어 있습니다. 또는 더 빠른 섀도 맵에 대 한 포인트 필터링을 선택 합니다.

D3D11 \_ 텍스처 \_ 주소 테두리 주소 모드를 지정할 수 \_ 있으며이 주소는 기능 수준 9 1 장치에서 작동 \_ 합니다. 이는 깊이가 테스트를 수행 하기 전에 해당 픽셀이 밝은 뷰 부분에 있는지 여부를 테스트 하지 않는 픽셀 셰이더에 적용 됩니다. 각 테두리에 대해 0 또는 1을 지정 하 여 광원의 뷰 외부에 있는 픽셀이 깊이 테스트를 통과 또는 실패 하는지 여부와이에 따라 lit 또는 그림자를 표시할지 여부를 제어할 수 있습니다.

기능 수준 9 \_ 1에서 다음과 같은 필수 값을 설정 해야 합니다. **MinLOD** 가 0으로 설정 되 고 **MaxLOD** 가 **D3D11 \_ FLOAT32 \_ MAX**로 설정 되 고 **MaxAnisotropy** 가 0으로 설정 됩니다.

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

## <a name="create-render-states"></a>렌더링 상태 만들기


이제 front face 고르기을 사용 하도록 설정 하는 데 사용할 수 있는 렌더링 상태를 만듭니다. 기능 수준 9 \_ 1 장치에는 **DepthClipEnable** 을 **true**로 설정 해야 합니다.

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

뒤로 얼굴 고르기을 사용 하도록 설정 하는 데 사용할 수 있는 렌더링 상태를 만듭니다. 렌더링 코드가 이미 뒷면을 고르기 면이 단계를 건너뛸 수 있습니다.

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

## <a name="create-constant-buffers"></a>상수 버퍼 만들기


조명의 관점에서 렌더링 하는 데 사용할 상수 버퍼를 반드시 만들어야 합니다. 또한이 상수 버퍼를 사용 하 여 셰이더에 빛의 위치를 지정할 수 있습니다. 점 광원에 대해 원근감 매트릭스를 사용 하 고 방향 광원 (예: 햇빛)에 직교 행렬을 사용 합니다.

```cpp
DX::ThrowIfFailed(
    m_deviceResources->GetD3DDevice()->CreateBuffer(
        &viewProjectionConstantBufferDesc,
        nullptr,
        &m_lightViewProjectionBuffer
        )
    );
```

상수 버퍼 데이터를 채웁니다. 초기화 중에 상수 버퍼를 한 번 업데이트 하 고, 이전 프레임 이후 광원 값이 변경 된 경우 다시 업데이트 합니다.

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

## <a name="create-a-viewport"></a>뷰포트 만들기


그림자 맵으로 렌더링 하려면 별도의 뷰포트가 필요 합니다. 뷰포트는 장치 기반 리소스가 아닙니다. 코드의 다른 곳에서 자유롭게 만들 수 있습니다. 그림자 맵과 함께 뷰포트를 만들면 뷰포트 적절 한의 차원을 섀도 지도 차원과 유지 하는 데 도움이 될 수 있습니다.

```cpp
// Init viewport for shadow rendering
ZeroMemory(&m_shadowViewport, sizeof(D3D11_VIEWPORT));
m_shadowViewport.Height = m_shadowMapDimension;
m_shadowViewport.Width = m_shadowMapDimension;
m_shadowViewport.MinDepth = 0.f;
m_shadowViewport.MaxDepth = 1.f;
```

이 연습의 다음 부분에서는 [깊이 버퍼로 렌더링](render-the-shadow-map-to-the-depth-buffer.md)하 여 그림자 맵을 만드는 방법에 대해 알아봅니다.

 

 