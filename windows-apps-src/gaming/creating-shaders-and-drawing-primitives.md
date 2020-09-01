---
title: 셰이더 및 그리기 기본 형식 만들기
description: 여기서는 HLSL 소스 파일을 사용 하 여 표시에서 기본 형식을 그리는 데 사용할 수 있는 셰이더를 컴파일하고 만드는 방법을 보여 줍니다.
ms.assetid: 91113bbe-96c9-4ef9-6482-39f1ff1a70f4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 셰이더, 기본 형식, directx
ms.localizationpriority: medium
ms.openlocfilehash: e900767b594b195ac5e1dd1d5777cfab16c2ece4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175367"
---
# <a name="create-shaders-and-drawing-primitives"></a>셰이더 및 그리기 기본 형식 만들기



여기서는 HLSL 소스 파일을 사용 하 여 표시에서 기본 형식을 그리는 데 사용할 수 있는 셰이더를 컴파일하고 만드는 방법을 보여 줍니다.

꼭 짓 점 및 픽셀 셰이더를 사용 하 여 노란색 삼각형을 만들고 그립니다. Direct3D 장치, 스왑 체인 및 렌더링 대상 뷰를 만든 후에는 디스크의 이진 셰이더 개체 파일에서 데이터를 읽습니다.

**목표:** 셰이더를 만들고 기본 형식을 그리려면입니다.

## <a name="prerequisites"></a>필수 구성 요소


C + +에 대해 잘 알고 있다고 가정 합니다. 그래픽 프로그래밍 개념에 대한 기본 경험도 필요합니다.

또한 [빠른 시작: DirectX 리소스를 설정 하 고 이미지를 표시](setting-up-directx-resources.md)하는 것으로 가정 합니다.

**완료 시간:** 20 분

## <a name="instructions"></a>Instructions

### <a name="1-compiling-hlsl-source-files"></a>1. HLSL 원본 파일 컴파일

Microsoft Visual Studio는 [fxc.exe](/windows/desktop/direct3dtools/fxc) HLSL 코드 컴파일러를 사용 하 여 HLSL 원본 파일 (HLSL 및 SimplePixelShader)을 .cccccacacacacacaacacacacacacacccccccca HLSL 코드 컴파일러에 대 한 자세한 내용은 효과-컴파일러 도구를 참조 하세요. 셰이더 코드를 컴파일하는 방법에 대 한 자세한 내용은 [셰이더 컴파일](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-part1)을 참조 하세요.

Hlsl의 코드는 다음과 같습니다. SimpleVertexShader.

```hlsl
struct VertexShaderInput
{
    DirectX::XMFLOAT2 pos : POSITION;
};

struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

PixelShaderInput SimpleVertexShader(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;

    // For this lesson, set the vertex depth value to 0.5, so it is guaranteed to be drawn.
    vertexShaderOutput.pos = float4(input.pos, 0.5f, 1.0f);

    return vertexShaderOutput;
}
```

Hlsl의 코드는 다음과 같습니다. SimplePixelShader.

```hlsl
struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

float4 SimplePixelShader(PixelShaderInput input) : SV_TARGET
{
    // Draw the entire triangle yellow.
    return float4(1.0f, 1.0f, 0.0f, 1.0f);
}
```

### <a name="2-reading-data-from-disk"></a>2. 디스크에서 데이터 읽기

DirectX 11 앱 (유니버설 Windows) 템플릿에서 ReadDataAsync의 DX:: 함수를 사용 하 여 디스크의 파일에서 데이터를 비동기적으로 읽습니다.

### <a name="3-creating-vertex-and-pixel-shaders"></a>3. 꼭 짓 점 및 픽셀 셰이더 만들기

SimpleVertexShader 파일에서 데이터를 읽고 *vertexShaderBytecode* 바이트 배열에 데이터를 할당 합니다. 바이트 배열을 사용 하 여 [**ID3D11Device:: CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 를 호출 하 여 꼭 짓 점 셰이더 ([**ID3D11VertexShader**](/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader))를 만듭니다. SimpleVertexShader hlsl 원본에서 꼭 짓 점 깊이 값을 0.5로 설정 하 여 삼각형이 그려지도록 합니다. [**D3D11 \_ INPUT \_ 요소 \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_input_element_desc) 구조체의 배열을 채워서 꼭 짓 점 셰이더 코드의 레이아웃을 설명한 다음 [**ID3D11Device:: createinputlayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout) 을 호출 하 여 레이아웃을 만듭니다. 배열에는 꼭 짓 점 위치를 정의 하는 레이아웃 요소가 하나 있습니다. SimplePixelShader 파일에서 데이터를 읽고 *pixelShaderBytecode* 바이트 배열에 데이터를 할당 합니다. 바이트 배열을 사용 하 여 [**ID3D11Device:: CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) 를 호출 하 여 픽셀 셰이더 ([**ID3D11PixelShader**](/windows/desktop/api/d3d11/nn-d3d11-id3d11pixelshader))를 만듭니다. SimplePixelShader hlsl 원본에서 픽셀 값을 (1, 1, 1, 1)로 설정 하 여 삼각형을 노란색으로 만듭니다. 이 값을 변경 하 여 색을 변경할 수 있습니다.

간단한 삼각형을 정의 하는 꼭 짓 점 및 인덱스 버퍼를 만듭니다. 이렇게 하려면 먼저 삼각형을 정의 하 고, 다음 삼각형 정의를 사용 하 여 꼭 짓 점 및 인덱스 버퍼 ([**D3D11 \_ BUFFER \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc) 및 [**D3D11 \_ subresource \_ 데이터**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data))를 설명 하 고, 마지막으로 각 버퍼에 대해 [**ID3D11Device:: createbuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) 를 한 번씩 호출 합니다.

```cpp
        auto loadVSTask = DX::ReadDataAsync(L"SimpleVertexShader.cso");
        auto loadPSTask = DX::ReadDataAsync(L"SimplePixelShader.cso");
        
        // Load the raw vertex shader bytecode from disk and create a vertex shader with it.
        auto createVSTask = loadVSTask.then([this](const std::vector<byte>& vertexShaderBytecode) {


          ComPtr<ID3D11VertexShader> vertexShader;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateVertexShader(
                  vertexShaderBytecode->Data,
                  vertexShaderBytecode->Length,
                  nullptr,
                  &vertexShader
                  )
              );

          // Create an input layout that matches the layout defined in the vertex shader code.
          // For this lesson, this is simply a DirectX::XMFLOAT2 vector defining the vertex position.
          const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
          {
              { "POSITION", 0, DXGI_FORMAT_R32G32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
          };

          ComPtr<ID3D11InputLayout> inputLayout;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateInputLayout(
                  basicVertexLayoutDesc,
                  ARRAYSIZE(basicVertexLayoutDesc),
                  vertexShaderBytecode->Data,
                  vertexShaderBytecode->Length,
                  &inputLayout
                  )
              );
        });
        
        // Load the raw pixel shader bytecode from disk and create a pixel shader with it.
        auto createPSTask = loadPSTask.then([this](const std::vector<byte>& pixelShaderBytecode) {
          ComPtr<ID3D11PixelShader> pixelShader;
          DX::ThrowIfFailed(
              m_d3dDevice->CreatePixelShader(
                  pixelShaderBytecode->Data,
                  pixelShaderBytecode->Length,
                  nullptr,
                  &pixelShader
                  )
              );
        });

        // Create vertex and index buffers that define a simple triangle.
        auto createTriangleTask = (createPSTask && createVSTask).then([this] () {

          DirectX::XMFLOAT2 triangleVertices[] =
          {
              float2(-0.5f, -0.5f),
              float2( 0.0f,  0.5f),
              float2( 0.5f, -0.5f),
          };

          unsigned short triangleIndices[] =
          {
              0, 1, 2,
          };

          D3D11_BUFFER_DESC vertexBufferDesc = {0};
          vertexBufferDesc.ByteWidth = sizeof(float2) * ARRAYSIZE(triangleVertices);
          vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
          vertexBufferDesc.CPUAccessFlags = 0;
          vertexBufferDesc.MiscFlags = 0;
          vertexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA vertexBufferData;
          vertexBufferData.pSysMem = triangleVertices;
          vertexBufferData.SysMemPitch = 0;
          vertexBufferData.SysMemSlicePitch = 0;

          ComPtr<ID3D11Buffer> vertexBuffer;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &vertexBufferDesc,
                  &vertexBufferData,
                  &vertexBuffer
                  )
              );

          D3D11_BUFFER_DESC indexBufferDesc;
          indexBufferDesc.ByteWidth = sizeof(unsigned short) * ARRAYSIZE(triangleIndices);
          indexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          indexBufferDesc.BindFlags = D3D11_BIND_INDEX_BUFFER;
          indexBufferDesc.CPUAccessFlags = 0;
          indexBufferDesc.MiscFlags = 0;
          indexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA indexBufferData;
          indexBufferData.pSysMem = triangleIndices;
          indexBufferData.SysMemPitch = 0;
          indexBufferData.SysMemSlicePitch = 0;

          ComPtr<ID3D11Buffer> indexBuffer;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &indexBufferDesc,
                  &indexBufferData,
                  &indexBuffer
                  )
              );
        });
```

꼭 짓 점 및 픽셀 셰이더, 꼭 짓 점 셰이더 레이아웃 및 꼭 짓 점 및 인덱스 버퍼를 사용 하 여 노란색 삼각형을 그립니다.

### <a name="4-drawing-the-triangle-and-presenting-the-rendered-image"></a>4. 삼각형을 그리고 렌더링 된 이미지를 표시 합니다.

무한 루프를 입력 하 여 장면을 지속적으로 렌더링 하 고 표시 합니다. [**ID3D11DeviceContext:: OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) 를 호출 하 여 렌더링 대상을 출력 대상으로 지정 합니다. {0.071 f, 0.04 f, 0.561 f, 1.0 f}를 사용 하 여 [**ID3D11DeviceContext:: ClearRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-clearrendertargetview) 를 호출 하 여 렌더링 대상을 진한 파란색으로 지웁니다.

무한 루프에서는 파란색 화면에 노란색 삼각형을 그립니다.

**노란색 삼각형을 그리려면**

1.  먼저 [**ID3D11DeviceContext:: IASetInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) 를 호출 하 여 꼭 짓 점 버퍼 데이터가 입력-어셈블러 단계로 스트리밍되는 방식을 설명 합니다.
2.  그런 다음 [**ID3D11DeviceContext:: IASetVertexBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) 및 [**ID3D11DeviceContext:: IASetIndexBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer) 을 호출 하 여 꼭 짓 점 및 인덱스 버퍼를 입력-어셈블러 단계에 바인딩합니다.
3.  그런 다음 [**D3D11 \_ 기본 \_ 토폴로지 \_ TRIANGLESTRIP**](/previous-versions/windows/desktop/legacy/ff476189(v=vs.85)) 값을 사용 하 여 [**ID3D11DeviceContext:: IASetPrimitiveTopology**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology) 를 호출 하 여 꼭 짓 점 데이터를 삼각형 스트립으로 해석 하는 입력 어셈블러 단계를 지정 합니다.
4.  그런 다음 [**ID3D11DeviceContext:: VSSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) 를 호출 하 여 꼭 짓 점 셰이더 코드를 사용 하 여 꼭 짓 점 셰이더 단계를 초기화 하 [**:P**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) 고 픽셀 셰이더 코드를 사용 하 여 픽셀 셰이더 단계를 초기화 합니다.
5.  마지막으로 [**ID3D11DeviceContext::D rawindexed**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) 를 호출 하 여 삼각형을 그리고 렌더링 파이프라인에 제출 합니다.

[**Idxgiswapchain::P 다시 보낸**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) 후 렌더링 된 이미지를 창에 표시 합니다.

```cpp
            // Specify the render target we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                nullptr // Use no depth stencil.
                );

            // Clear the render target to a solid color.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());

            // Set the vertex and index buffers, and specify the way they define geometry.
            UINT stride = sizeof(float2);
            UINT offset = 0;
            m_d3dDeviceContext->IASetVertexBuffers(
                0,
                1,
                vertexBuffer.GetAddressOf(),
                &stride,
                &offset
                );

            m_d3dDeviceContext->IASetIndexBuffer(
                indexBuffer.Get(),
                DXGI_FORMAT_R16_UINT,
                0
                );

            m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

            // Set the vertex and pixel shader stage state.
            m_d3dDeviceContext->VSSetShader(
                vertexShader.Get(),
                nullptr,
                0
                );

            m_d3dDeviceContext->PSSetShader(
                pixelShader.Get(),
                nullptr,
                0
                );

            // Draw the cube.
            m_d3dDeviceContext->DrawIndexed(
                ARRAYSIZE(triangleIndices),
                0,
                0
                );

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
```

## <a name="summary-and-next-steps"></a>요약 및 다음 단계


꼭 짓 점 및 픽셀 셰이더를 사용 하 여 노란색 삼각형을 만들고 그렸습니다.

다음으로 orbiting 3D 큐브를 만들고 조명 효과를 적용 합니다.

[기본 형식에 깊이 및 효과 사용](using-depth-and-effects-on-primitives.md)

 

 