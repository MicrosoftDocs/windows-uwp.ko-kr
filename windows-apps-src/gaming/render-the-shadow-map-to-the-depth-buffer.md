---
title: 그림자 맵을 깊이 버퍼로 렌더링 합니다.
description: 광원의 관점에서 렌더링 하 여 그림자 볼륨을 나타내는 2 차원 깊이 지도를 만듭니다.
ms.assetid: 7f3d0208-c379-8871-cc48-027047c6c2d0
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 렌더링, 그림자 맵, 깊이 버퍼, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 5f492b1007a96b893abf6cdd1e7c6686cd5a41ee
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159237"
---
# <a name="render-the-shadow-map-to-the-depth-buffer"></a>그림자 맵을 깊이 버퍼로 렌더링 합니다.




광원의 관점에서 렌더링 하 여 그림자 볼륨을 나타내는 2 차원 깊이 지도를 만듭니다. 깊이 맵은 그림자로 렌더링 될 공간을 마스크 합니다. 연습 2 부 [: Direct3D 11에서 깊이 버퍼를 사용 하 여 섀도 볼륨 구현](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="clear-the-depth-buffer"></a>깊이 버퍼 지우기


렌더링 하기 전에 항상 깊이 버퍼를 지워야 합니다.

```cpp
context->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), DirectX::Colors::CornflowerBlue);
context->ClearDepthStencilView(m_shadowDepthView.Get(), D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 1.0f, 0);
```

## <a name="render-the-shadow-map-to-the-depth-buffer"></a>그림자 맵을 깊이 버퍼로 렌더링 합니다.


섀도 렌더링 패스에 대해 깊이 버퍼를 지정 하지만 렌더링 대상을 지정 하지는 않습니다.

조명 뷰포트, 꼭 짓 점 셰이더를 지정 하 고 밝은 공간 상수 버퍼를 설정 합니다. 이 패스에 front face 고르기를 사용 하 여 그림자 버퍼에 배치 된 깊이 값을 최적화 합니다.

대부분의 장치에서 픽셀 셰이더에 nullptr를 지정 하거나 픽셀 셰이더를 완전히 지정 하는 것을 건너뛸 수 있습니다. 그러나 null 픽셀 셰이더 집합을 사용 하 여 Direct3D 장치에서 그리기를 호출 하면 일부 드라이버에서 예외가 throw 될 수 있습니다. 이 예외를 방지 하려면 섀도 렌더링 패스에 대 한 최소 픽셀 셰이더를 설정할 수 있습니다. 이 셰이더의 출력이 throw 됩니다. 모든 픽셀에서 [**무시**](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-discard) 를 호출할 수 있습니다.

그림자를 캐스트할 수 있는 개체를 렌더링 하지만 그림자를 캐스트할 수 없는 개체 (예: 방의 바닥 또는 최적화를 위해 섀도 패스에서 제거 되는 개체)를 렌더링 하지 않습니다.

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

**뷰를 최적화 합니다.**  깊이 버퍼에서 최대한의 전체 자릿수를 얻을 수 있도록 구현이 밀접 한 뷰를 계산 하는지 확인 합니다. 그림자 기술에 대 한 자세한 팁은 [그림자 깊이 맵을 개선 하는 일반적인 기법을](/windows/desktop/DxTechArts/common-techniques-to-improve-shadow-depth-maps) 참조 하세요.

## <a name="vertex-shader-for-shadow-pass"></a>섀도 패스에 대 한 꼭 짓 점 셰이더


꼭 짓 점 셰이더의 단순화 된 버전을 사용 하 여 밝은 공간에서 꼭 짓 점 위치만 렌더링 합니다. 조명이 수직이 나 보조 변환 등을 포함 하지 마세요.

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

이 연습의 다음 부분에서는 [깊이 테스트를 사용](render-the-scene-with-depth-testing.md)하 여 렌더링 하 여 그림자를 추가 하는 방법에 대해 알아봅니다.

 

 