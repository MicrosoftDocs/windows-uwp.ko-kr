---
title: 기본 요소에 깊이 및 효과 사용
description: 여기서는 기본 형식에 깊이, 원근, 색 및 기타 효과를 사용하는 방법에 대해 설명합니다.
ms.assetid: 71ef34c5-b4a3-adae-5266-f86ba257482a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 깊이, 효과, 기본 형식, directx
ms.localizationpriority: medium
ms.openlocfilehash: 02911338da858e3718235736cee7969a7bdebae2
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8697841"
---
# <a name="use-depth-and-effects-on-primitives"></a>기본 형식에 깊이 및 효과 사용



여기에서는 원형에 깊이, 원근, 색 및 기타 효과를 사용하는 방법에 대해 설명합니다.

**목표:** 3D 개체를 만들고 기본 꼭짓점 조명 및 색을 적용합니다.

## <a name="prerequisites"></a>필수 조건


사용자가 C++에 익숙하다고 가정합니다. 그래픽 프로그래밍 개념에 대한 기본 경험도 필요합니다.

또한 사용자가 [빠른 시작: DirectX 리소스 설정 및 이미지 표시](setting-up-directx-resources.md) 및 [셰이더 및 그리기 원형 만들기](creating-shaders-and-drawing-primitives.md)를 완료했다고 가정합니다.

**완료 시간:** 20분입니다.

<a name="instructions"></a>지침
------------
### <a name="1-defining-cube-variables"></a>1. 큐브 변수 정의

먼저 큐브에 대해 **SimpleCubeVertex** 및 **ConstantBuffer** 구조를 정의해야 합니다. 이러한 구조는 큐브에 대한 꼭짓점 위치와 색을 지정하고 큐브를 보는 방법도 지정합니다. [**ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx)을 사용하여 [**ID3D11DepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476377) 및 [**ID3D11Buffer**](https://msdn.microsoft.com/library/windows/desktop/ff476351)를 선언하고 **ConstantBuffer**의 인스턴스를 선언합니다.

```cpp
struct SimpleCubeVertex
{
    DirectX::XMFLOAT3 pos;   // Position
    DirectX::XMFLOAT3 color; // Color
};

struct ConstantBuffer
{
    DirectX::XMFLOAT4X4 model;
    DirectX::XMFLOAT4X4 view;
    DirectX::XMFLOAT4X4 projection;
};

// This class defines the application as a whole.
ref class Direct3DTutorialFrameworkView : public IFrameworkView
{
private:
    Platform::Agile<CoreWindow> m_window;
    ComPtr<IDXGISwapChain1> m_swapChain;
    ComPtr<ID3D11Device1> m_d3dDevice;
    ComPtr<ID3D11DeviceContext1> m_d3dDeviceContext;
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    ComPtr<ID3D11DepthStencilView> m_depthStencilView;
    ComPtr<ID3D11Buffer> m_constantBuffer;
    ConstantBuffer m_constantBufferData;
```

### <a name="2-creating-a-depth-stencil-view"></a>2. 깊이 스텐실 보기 만들기

렌더링-대상 보기를 만드는 것 외에도 깊이-스텐실 보기를 만듭니다. 깊이-스텐실 보기를 만들면 Direct3D는 카메라에 더 가까운 개체를 카메라에서 더 먼 개체 앞에 효과적으로 렌더링할 수 있습니다. 깊이-스텐실 버퍼에 대한 보기를 만들기 전에 깊이-스텐실 버퍼를 만들어야 합니다. [**D3D11\_TEXTURE2D\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476253)를 채워 깊이-스텐실 버퍼를 설명한 다음 [**ID3D11Device::CreateTexture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476521)를 호출하여 깊이-스텐실 버퍼를 만듭니다. 깊이-스텐실 보기를 만들기 위해 [**D3D11\_DEPTH\_STENCIL\_VIEW\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476112)를 채워 깊이-스텐실 보기를 설명하고, 깊이-스텐실 보기 설명 및 깊이-스텐실 버퍼를 [**ID3D11Device::CreateDepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476507)에 전달합니다.

```cpp
        // Once the render target view is created, create a depth stencil view.  This
        // allows Direct3D to efficiently render objects closer to the camera in front
        // of objects further from the camera.

        D3D11_TEXTURE2D_DESC backBufferDesc = {0};
        backBuffer->GetDesc(&backBufferDesc);

        D3D11_TEXTURE2D_DESC depthStencilDesc;
        depthStencilDesc.Width = backBufferDesc.Width;
        depthStencilDesc.Height = backBufferDesc.Height;
        depthStencilDesc.MipLevels = 1;
        depthStencilDesc.ArraySize = 1;
        depthStencilDesc.Format = DXGI_FORMAT_D24_UNORM_S8_UINT;
        depthStencilDesc.SampleDesc.Count = 1;
        depthStencilDesc.SampleDesc.Quality = 0;
        depthStencilDesc.Usage = D3D11_USAGE_DEFAULT;
        depthStencilDesc.BindFlags = D3D11_BIND_DEPTH_STENCIL;
        depthStencilDesc.CPUAccessFlags = 0;
        depthStencilDesc.MiscFlags = 0;
        ComPtr<ID3D11Texture2D> depthStencil;
        DX::ThrowIfFailed(
            m_d3dDevice->CreateTexture2D(
                &depthStencilDesc,
                nullptr,
                &depthStencil
                )
            );

        D3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc;
        depthStencilViewDesc.Format = depthStencilDesc.Format;
        depthStencilViewDesc.ViewDimension = D3D11_DSV_DIMENSION_TEXTURE2D;
        depthStencilViewDesc.Flags = 0;
        depthStencilViewDesc.Texture2D.MipSlice = 0;
        DX::ThrowIfFailed(
            m_d3dDevice->CreateDepthStencilView(
                depthStencil.Get(),
                &depthStencilViewDesc,
                &m_depthStencilView
                )
            );
```

### <a name="3-updating-perspective-with-the-window"></a>3. 창으로 원근 업데이트

창 크기에 따라 상수 버퍼에 대한 원근 투영 매개 변수를 업데이트합니다. 매개 변수를 70도 보기 필드(깊이 범위 0.01 - 100)로 고정합니다.

```cpp
        // Finally, update the constant buffer perspective projection parameters
        // to account for the size of the application window.  In this sample,
        // the parameters are fixed to a 70-degree field of view, with a depth
        // range of 0.01 to 100.  For a generalized camera class, see Lesson 5.

        float xScale = 1.42814801f;
        float yScale = 1.42814801f;
        if (backBufferDesc.Width > backBufferDesc.Height)
        {
            xScale = yScale *
                static_cast<float>(backBufferDesc.Height) /
                static_cast<float>(backBufferDesc.Width);
        }
        else
        {
            yScale = xScale *
                static_cast<float>(backBufferDesc.Width) /
                static_cast<float>(backBufferDesc.Height);
        }

        m_constantBufferData.projection = DirectX::XMFLOAT4X4(
            xScale, 0.0f,    0.0f,  0.0f,
            0.0f,   yScale,  0.0f,  0.0f,
            0.0f,   0.0f,   -1.0f, -0.01f,
            0.0f,   0.0f,   -1.0f,  0.0f
            );
```

### <a name="4-creating-vertex-and-pixel-shaders-with-color-elements"></a>4. 색 요소와 함께 꼭짓점 및 픽셀 셰이더 만들기

이 앱에서는 이전 자습서인 [셰이더 및 그리기 원형 만들기](creating-shaders-and-drawing-primitives.md)에서 설명한 것보다 좀 더 복잡한 꼭짓점 및 픽셀 셰이더를 만듭니다. 앱의 꼭짓점 셰이더는 각 꼭짓점 위치를 투영 공간으로 변환하고 꼭짓점 색을 픽셀 셰이더에 전달합니다.

꼭짓점 셰이더 색의 레이아웃을 설명하는 [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180) 구조의 앱 배열에는 두 가지 레이아웃 요소가 있습니다. 한 요소는 꼭짓점 위치를 정의하고 다른 요소는 색을 정의합니다.

선회하는 큐브를 정의하기 위해 꼭짓점, 인덱스 및 상수 버퍼를 만듭니다.

**선회하는 큐브를 정의하려면**

1.  먼저 큐브를 정의합니다. 각 꼭짓점에 위치 외에 색도 할당합니다. 이렇게 하면 각 면을 구분할 수 있도록 픽셀 셰이더에서 각 면에 서로 다른 색을 적용할 수 있습니다.
2.  그런 다음 큐브 정의를 사용해 꼭짓점 및 인덱스 버퍼([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) 및 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220))를 설명합니다. 각 버퍼에 대해 [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)를 한 번 호출합니다.
3.  이제 꼭짓점 셰이더에 모델, 보기 및 투영 행렬을 전달하기 위해 상수 버퍼([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092))를 만듭니다. 나중에 상수 버퍼를 사용하여 큐브를 회전하고 큐브에 원근 투영을 적용할 수 있습니다. [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)를 호출하여 상수 버퍼를 만듭니다.
4.  X = 0, Y = 1, Z = 2의 카메라 위치에 해당하는 보기 변환을 지정합니다.
5.  마지막으로 모든 프레임을 회전하여 큐브 애니메이션을 만드는 데 사용할 *degree* 변수를 선언합니다.

```cpp
        
        auto loadVSTask = DX::ReadDataAsync(L"SimpleVertexShader.cso");
        auto loadPSTask = DX::ReadDataAsync(L"SimplePixelShader.cso");
        
        
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
          // For this lesson, this is simply a DirectX::XMFLOAT3 vector defining the vertex position, and
          // a DirectX::XMFLOAT3 vector defining the vertex color.
          const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
          {
              { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
              { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
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
        
        
        // Create vertex and index buffers that define a simple unit cube.
        auto createCubeTask = (createPSTask && createVSTask).then([this] () {

          // In the array below, which will be used to initialize the cube vertex buffers,
          // each vertex is assigned a color in addition to a position.  This will allow
          // the pixel shader to color each face differently, enabling them to be distinguished.
          SimpleCubeVertex cubeVertices[] =
          {
              { float3(-0.5f, 0.5f, -0.5f), float3(0.0f, 1.0f, 0.0f) }, // +Y (top face)
              { float3( 0.5f, 0.5f, -0.5f), float3(1.0f, 1.0f, 0.0f) },
              { float3( 0.5f, 0.5f,  0.5f), float3(1.0f, 1.0f, 1.0f) },
              { float3(-0.5f, 0.5f,  0.5f), float3(0.0f, 1.0f, 1.0f) },

              { float3(-0.5f, -0.5f,  0.5f), float3(0.0f, 0.0f, 1.0f) }, // -Y (bottom face)
              { float3( 0.5f, -0.5f,  0.5f), float3(1.0f, 0.0f, 1.0f) },
              { float3( 0.5f, -0.5f, -0.5f), float3(1.0f, 0.0f, 0.0f) },
              { float3(-0.5f, -0.5f, -0.5f), float3(0.0f, 0.0f, 0.0f) },
          };

          unsigned short cubeIndices[] =
          {
              0, 1, 2,
              0, 2, 3,

              4, 5, 6,
              4, 6, 7,

              3, 2, 5,
              3, 5, 4,

              2, 1, 6,
              2, 6, 5,

              1, 7, 6,
              1, 0, 7,

              0, 3, 4,
              0, 4, 7
          };

          D3D11_BUFFER_DESC vertexBufferDesc = {0};
          vertexBufferDesc.ByteWidth = sizeof(SimpleCubeVertex) * ARRAYSIZE(cubeVertices);
          vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
          vertexBufferDesc.CPUAccessFlags = 0;
          vertexBufferDesc.MiscFlags = 0;
          vertexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA vertexBufferData;
          vertexBufferData.pSysMem = cubeVertices;
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
          indexBufferDesc.ByteWidth = sizeof(unsigned short) * ARRAYSIZE(cubeIndices);
          indexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          indexBufferDesc.BindFlags = D3D11_BIND_INDEX_BUFFER;
          indexBufferDesc.CPUAccessFlags = 0;
          indexBufferDesc.MiscFlags = 0;
          indexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA indexBufferData;
          indexBufferData.pSysMem = cubeIndices;
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


          // Create a constant buffer for passing model, view, and projection matrices
          // to the vertex shader.  This will allow us to rotate the cube and apply
          // a perspective projection to it.

          D3D11_BUFFER_DESC constantBufferDesc = {0};
          constantBufferDesc.ByteWidth = sizeof(m_constantBufferData);
          constantBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          constantBufferDesc.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
          constantBufferDesc.CPUAccessFlags = 0;
          constantBufferDesc.MiscFlags = 0;
          constantBufferDesc.StructureByteStride = 0;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &constantBufferDesc,
                  nullptr,
                  &m_constantBuffer
                  )
              );

          // Specify the view transform corresponding to a camera position of
          // X = 0, Y = 1, Z = 2.  For a generalized camera class, see Lesson 5.

          m_constantBufferData.view = DirectX::XMFLOAT4X4(
              -1.00000000f, 0.00000000f,  0.00000000f,  0.00000000f,
               0.00000000f, 0.89442718f,  0.44721359f,  0.00000000f,
               0.00000000f, 0.44721359f, -0.89442718f, -2.23606800f,
               0.00000000f, 0.00000000f,  0.00000000f,  1.00000000f
              );

        });
        
        // This value will be used to animate the cube by rotating it every frame.
        float degree = 0.0f;
        
```

### <a name="5-rotating-and-drawing-the-cube-and-presenting-the-rendered-image"></a>5. 큐브 회전하고 그리기 및 렌더링된 이미지 표시

장면을 계속해서 렌더링 및 표시하기 위해 무한 루프를 입력합니다. 회전 양과 함께 **rotationY** 인라인 함수(BasicMath.h)를 호출하여 Y축을 중심으로 큐브의 모델 행렬을 회전할 값을 설정합니다. [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486)를 호출하여 상수 버퍼를 업데이트하고 큐브 모델을 회전합니다. [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464)를 호출하여 렌더링 대상을 출력 대상으로 지정합니다. 이 **OMSetRenderTargets** 호출의 경우 깊이-스텐실 보기를 전달합니다. [**ID3D11DeviceContext::ClearRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476388)를 호출하여 렌더링 대상을 파란색 단색으로 지우고 [**ID3D11DeviceContext::ClearDepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476387)를 호출하여 깊이 버퍼를 지웁니다.

무한 루프에서, 파란색 표면에 큐브도 그립니다.

**큐브를 그리려면**

1.  먼저 [**ID3D11DeviceContext::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454)을 호출하여 꼭짓점 버퍼 데이터를 입력-어셈블러 단계로 스트리밍하는 방법을 설명합니다.
2.  [**ID3D11DeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) 및 [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453)를 호출하여 꼭짓점 및 인덱스 버퍼를 입력-어셈블러 단계로 바인딩합니다.
3.  [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455)를 [**D3D11\_PRIMITIVE\_TOPOLOGY\_TRIANGLESTRIP**](https://msdn.microsoft.com/library/windows/desktop/ff476189#D3D11_PRIMITIVE_TOPOLOGY_TRIANGLESTRIP) 값과 함께 호출하여, 꼭짓점 데이터를 삼각형 스트립으로 해석할 입력-어셈블러 단계를 지정합니다.
4.  [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493)를 호출하여 꼭짓점 셰이더 단계를 꼭짓점 셰이더 코드로 초기화하고, [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472)를 호출하여 픽셀 셰이더 단계를 픽셀 셰이더 코드로 초기화합니다.
5.  [**ID3D11DeviceContext::VSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476491)를 호출하여 꼭짓점 셰이더 파이프라인 단계에서 사용되는 상수 버퍼를 설정합니다.
6.  마지막으로 [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409)를 호출하여 큐브를 그린 후 렌더링 파이프라인에 제출합니다.

[**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576)를 호출하여 렌더링된 이미지를 창에 표시합니다.

```cpp
            // Update the constant buffer to rotate the cube model.
            m_constantBufferData.model = XMMatrixRotationY(-degree);
            degree += 1.0f;

            m_d3dDeviceContext->UpdateSubresource(
                m_constantBuffer.Get(),
                0,
                nullptr,
                &m_constantBufferData,
                0,
                0
                );

            // Specify the render target and depth stencil we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                m_depthStencilView.Get()
                );

            // Clear the render target to a solid color, and reset the depth stencil.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            m_d3dDeviceContext->ClearDepthStencilView(
                m_depthStencilView.Get(),
                D3D11_CLEAR_DEPTH,
                1.0f,
                0
                );

            m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());

            // Set the vertex and index buffers, and specify the way they define geometry.
            UINT stride = sizeof(SimpleCubeVertex);
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

            m_d3dDeviceContext->VSSetConstantBuffers(
                0,
                1,
                m_constantBuffer.GetAddressOf()
                );

            m_d3dDeviceContext->PSSetShader(
                pixelShader.Get(),
                nullptr,
                0
                );

            // Draw the cube.
            m_d3dDeviceContext->DrawIndexed(
                ARRAYSIZE(cubeIndices),
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


원형에 깊이, 원근, 색 및 기타 효과를 사용했습니다.

이제 원형에 텍스처를 적용합니다.

[원형에 텍스처 적용](applying-textures-to-primitives.md)

 

 




