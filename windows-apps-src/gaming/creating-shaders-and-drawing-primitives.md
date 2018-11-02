---
author: mtoepke
title: 셰이더 및 그리기 기본 요소 만들기
description: 여기에서는 HLSL 소스 파일을 사용해 셰이더를 컴파일하고 만드는 방법을 설명합니다. 이 셰이더를 사용하면 디스플레이에 원형을 그릴 수 있습니다.
ms.assetid: 91113bbe-96c9-4ef9-6482-39f1ff1a70f4
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 셰이더, 기본 형식, directx
ms.localizationpriority: medium
ms.openlocfilehash: 475a69837796b0b64be27c96f10b42d5b61390c1
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5971446"
---
# <a name="create-shaders-and-drawing-primitives"></a>셰이더 및 그리기 기본 요소 만들기



여기에서는 HLSL 소스 파일을 사용해 셰이더를 컴파일하고 만드는 방법을 설명합니다. 이 셰이더를 사용하면 디스플레이에 원형을 그릴 수 있습니다.

꼭짓점 및 픽셀 셰이더를 사용하여 노란색 삼각형을 만들어 그립니다. Direct3D 장치, 스왑 체인 및 렌더링 대상 보기를 만든 후, 디스크의 이진 셰이더 개체 파일의 데이터를 읽습니다.

**목표:** 셰이더를 만들고 원형을 그리기 위함입니다.

## <a name="prerequisites"></a>필수 조건


사용자가 C++에 익숙하다고 가정합니다. 그래픽 프로그래밍 개념에 대한 기본 경험도 필요합니다.

또한 [빠른 시작: DirectX 리소스 설정 및 이미지 표시](setting-up-directx-resources.md)를 완료했다고 가정합니다.

**완료 시간:** 20분입니다.

## <a name="instructions"></a>지침

### <a name="1-compiling-hlsl-source-files"></a>1. HLSL 소스 파일 컴파일

Microsoft Visual Studio에서는 [fxc.exe](https://msdn.microsoft.com/library/windows/desktop/bb232919) HLSL 코드 컴파일러를 사용하여 .hlsl 소스 파일(SimpleVertexShader.hlsl 및 SimplePixelShader.hlsl)을 .cso 이진 셰이더 개체 파일(SimpleVertexShader.cso 및 SimplePixelShader.cso)로 컴파일합니다. HLSL 코드 컴파일러에 대한 자세한 내용은 효과 컴파일러 도구를 참조하세요. 셰이더 코드 컴파일에 대한 자세한 내용은 [셰이더 컴파일](https://msdn.microsoft.com/library/windows/desktop/bb509633)을 참조하세요.

SimpleVertexShader.hlsl의 코드는 다음과 같습니다.

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

SimplePixelShader.hlsl의 코드는 다음과 같습니다.

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

DirectX 11 앱(유니버설 Windows) 템플릿에서 DirectXHelper.h의 DX::ReadDataAsync 함수를 사용하여 디스크의 파일에서 데이터를 비동기적으로 읽습니다.

### <a name="3-creating-vertex-and-pixel-shaders"></a>3. 꼭짓점 및 픽셀 셰이더 만들기

SimpleVertexShader.cso 파일의 데이터를 읽고 데이터를 *vertexShaderBytecode* 바이트 배열에 할당합니다. 바이트 배열과 함께 [**ID3D11Device::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524)를 호출하여 꼭짓점 셰이더([**ID3D11VertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476641))를 만듭니다. 삼각형을 그릴 수 있도록 SimpleVertexShader.hlsl 소스에서 꼭짓점 깊이 값을 0.5로 설정합니다. [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180) 구조의 배열을 채워 꼭짓점 셰이더 코드의 레이아웃을 설명한 다음 [**ID3D11Device::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512)을 호출하여 레이아웃을 만듭니다. 배열에는 꼭짓점 위치를 정의하는 하나의 레이아웃 요소가 있습니다. SimplePixelShader.cso 파일의 데이터를 읽고 데이터를 *pixelShaderBytecode* 바이트 배열에 할당합니다. 해당 바이트 배열과 함께 [**ID3D11Device::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)를 호출하여 픽셀 셰이더([**ID3D11PixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476576))를 만듭니다. 삼각형을 노란색으로 만들기 위해 SimplePixelShader.hlsl 소스에서 픽셀 값을 (1,1,1,1)로 설정합니다. 이 값을 변경하여 색을 변경할 수 있습니다.

간단한 삼각형을 정의하는 꼭짓점 및 인덱스 버퍼를 만듭니다. 먼저 삼각형을 정의하고, 다음으로 삼각형 정의를 사용하여 꼭짓점 및 인덱스 버퍼([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) 및 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220))를 설명하고, 최종적으로 각 버퍼에 대해 [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)를 한 번씩 호출합니다.

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

꼭짓점 및 픽셀 셰이더, 꼭짓점 셰이더 레이아웃, 꼭짓점 및 인덱스 버퍼를 사용하여 노란색 삼각형을 그립니다.

### <a name="4-drawing-the-triangle-and-presenting-the-rendered-image"></a>4. 삼각형 그리기 및 렌더링된 이미지 표시

장면을 계속해서 렌더링 및 표시하기 위해 무한 루프를 입력합니다. [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464)를 호출하여 렌더링 대상을 출력 대상으로 지정합니다. { 0.071f, 0.04f, 0.561f, 1.0f }와 함께 [**ID3D11DeviceContext::ClearRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476388)를 호출하여 렌더링 대상을 파란색 단색으로 지웁니다.

무한 루프에서, 파란색 표면에 노란색 삼각형을 그립니다.

**노란색 삼각형을 그리려면**

1.  먼저 [**ID3D11DeviceContext::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454)을 호출하여 꼭짓점 버퍼 데이터를 입력-어셈블러 단계로 스트리밍하는 방법을 설명합니다.
2.  [**ID3D11DeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) 및 [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453)를 호출하여 꼭짓점 및 인덱스 버퍼를 입력-어셈블러 단계로 바인딩합니다.
3.  [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455)를 [**D3D11\_PRIMITIVE\_TOPOLOGY\_TRIANGLESTRIP**](https://msdn.microsoft.com/library/windows/desktop/ff476189#D3D11_PRIMITIVE_TOPOLOGY_TRIANGLESTRIP) 값과 함께 호출하여, 꼭짓점 데이터를 삼각형 스트립으로 해석할 입력-어셈블러 단계를 지정합니다.
4.  [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493)를 호출하여 꼭짓점 셰이더 단계를 꼭짓점 셰이더 코드로 초기화하고, [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472)를 호출하여 픽셀 셰이더 단계를 픽셀 셰이더 코드로 초기화합니다.
5.  마지막으로 [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409)를 호출하여 큐브를 그린 후 렌더링 파이프라인에 제출합니다.

[**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576)를 호출하여 렌더링된 이미지를 창에 표시합니다.

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


꼭짓점 및 픽셀 셰이더를 사용하여 노란색 삼각형을 만들어 그립니다.

이제 선회하는 3D 큐브를 만들고 조명 효과를 적용합니다.

[원형에 깊이 및 효과 사용](using-depth-and-effects-on-primitives.md)

 

 




