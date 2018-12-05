---
title: 그림자 맵을 깊이 버퍼로 렌더링
description: 광원의 관점에서 렌더링하여 그림자 볼륨을 나타내는 2차원 깊이 맵을 만듭니다.
ms.assetid: 7f3d0208-c379-8871-cc48-027047c6c2d0
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, 렌더링, 그림자 지도, 깊이 버퍼, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 27cd535dc51a330937c345acf352677a42c652eb
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8729409"
---
# <a name="render-the-shadow-map-to-the-depth-buffer"></a>그림자 맵을 깊이 버퍼로 렌더링




광원의 관점에서 렌더링하여 그림자 볼륨을 나타내는 2차원 깊이 맵을 만듭니다. 깊이 맵은 그림자에서 렌더링되는 공간을 마스킹합니다. [연습의 2부: Direct3D 11의 깊이 버퍼를 사용하여 그림자 볼륨 구현](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="clear-the-depth-buffer"></a>깊이 버퍼 지우기


항상 여기에 렌더링하기 전에 깊이 버퍼를 지웁니다.

```cpp
context->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), DirectX::Colors::CornflowerBlue);
context->ClearDepthStencilView(m_shadowDepthView.Get(), D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 1.0f, 0);
```

## <a name="render-the-shadow-map-to-the-depth-buffer"></a>그림자 맵을 깊이 버퍼로 렌더링


그림자 렌더링 단계에서 깊이 버퍼를 지정하지만 렌더링 대상은 지정하지 않습니다.

광원 뷰포트, 꼭짓점 셰이더를 지정하고 광원 공간 상수 버퍼를 설정합니다. 이 단계에 대해 전면 컬링을 사용하여 그림자 버퍼에 배치된 깊이 값을 최적화합니다.

대부분의 장치에서 픽셀 셰이더에 대한 nullptr을 지정합니다. 또는 완전히 픽셀 셰이더 지정을 건너뜁니다. 하지만 null 픽셀 셰이더 집합을 사용하여 Direct3D 장치를 그리는 경우 일부 드라이버는 예외를 throw할 수 있습니다. 이 예외를 방지하려면 그림자 렌더링 단계에 대한 최소한의 픽셀 셰이더를 설정할 수 있습니다. 이 셰이더의 출력이 throw됩니다. 모든 픽셀에서 [**discard**](https://msdn.microsoft.com/library/windows/desktop/bb943995)를 호출할 수 있습니다.

그림자를 캐스팅할 수 있는 개체를 렌더링하지만 그림자를 캐스팅할 수 없는 기하 도형(예: 방의 바닥 또는 최적화상의 이유로 그림자 단계에서 제거된 개체)을 렌더링할 필요는 없습니다.

```cpp
void ShadowSceneRenderer::RenderShadowMap()
{
    auto context = m_deviceResources->GetD3DDeviceContext();

    // Render all the objects in the scene that can cast shadows onto themselves or onto other objects.

    // Only bind the ID3D11DepthStencilView for output.
    context->OMSetRenderTargets(
        0,
        nullptr,
        m_shadowDepthView.Get()
        );

    // Note that starting with the second frame, the previous call will display
    // warnings in VS debug output about forcing an unbind of the pixel shader
    // resource. This warning can be safely ignored when using shadow buffers
    // as demonstrated in this sample.

    // Set rendering state.
    context->RSSetState(m_shadowRenderState.Get());
    context->RSSetViewports(1, &m_shadowViewport);

    // Each vertex is one instance of the VertexPositionTexNormColor struct.
    UINT stride = sizeof(VertexPositionTexNormColor);
    UINT offset = 0;
    context->IASetVertexBuffers(
        0,
        1,
        m_vertexBuffer.GetAddressOf(),
        &stride,
        &offset
        );

    context->IASetIndexBuffer(
        m_indexBuffer.Get(),
        DXGI_FORMAT_R16_UINT, // Each index is one 16-bit unsigned integer (short).
        0
        );

    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
    context->IASetInputLayout(m_inputLayout.Get());

    // Attach our vertex shader.
    context->VSSetShader(
        m_simpleVertexShader.Get(),
        nullptr,
        0
        );

    // Send the constant buffers to the Graphics device.
    context->VSSetConstantBuffers(
        0,
        1,
        m_lightViewProjectionBuffer.GetAddressOf()
        );

    context->VSSetConstantBuffers(
        1,
        1,
        m_rotatedModelBuffer.GetAddressOf()
        );

    // In some configurations, it's possible to avoid setting a pixel shader
    // (or set PS to nullptr). Not all drivers are tolerant of this, so to be
    // safe set a minimal shader here.
    //
    // Direct3D will discard output from this shader because the render target
    // view is unbound.
    context->PSSetShader(
        m_textureShader.Get(),
        nullptr,
        0
        );

    // Draw the objects.
    context->DrawIndexed(
        m_indexCountCube,
        0,
        0
        );
}
```

**보기 절두체 최적화:**  깊이 버퍼에서 최고의 정밀도를 얻을 수 있도록 구현에서 세밀한 보기 절두체를 계산해야 합니다. 그림자 기술에 대한 자세한 팁은 [그림자 깊이 맵 향상을 위한 일반 기술](https://msdn.microsoft.com/library/windows/desktop/ee416324)을 참조하세요.

## <a name="vertex-shader-for-shadow-pass"></a>그림자 단계의 꼭짓점 셰이더


단순화된 버전의 꼭짓점 셰이더를 사용하여 조명 공간에서 꼭짓점 위치를 렌더링합니다. 조명 법선, 보조 변환 등을 포함하지 마세요.

```cpp
PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput output;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projected space.
    pos = mul(pos, model);
    pos = mul(pos, view);
    pos = mul(pos, projection);
    output.pos = pos;

    return output;
}
```

이 연습의 다음 부분에서 [깊이 테스트를 통해 렌더링](render-the-scene-with-depth-testing.md)하여 그림자를 추가하는 방법을 알아보세요.

 

 




