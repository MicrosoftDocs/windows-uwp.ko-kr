---
title: 기본 형식에 깊이 및 효과 사용
description: 여기서는 기본 형식에 깊이, 원근, 색 및 기타 효과를 사용 하는 방법을 보여 줍니다.
ms.assetid: 71ef34c5-b4a3-adae-5266-f86ba257482a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 깊이, 효과, 기본 형식, directx
ms.localizationpriority: medium
ms.openlocfilehash: 99931ef0abef10cb5c517c4c5be04e2afe3056a2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159017"
---
# <a name="use-depth-and-effects-on-primitives"></a>기본 형식에 깊이 및 효과 사용



여기서는 기본 형식에 깊이, 원근, 색 및 기타 효과를 사용 하는 방법을 보여 줍니다.

**목표:** 3D 개체를 만들고 기본 꼭 짓 점 조명을 적용 하 고 색을 지정 합니다.

## <a name="prerequisites"></a>필수 조건


C + +에 대해 잘 알고 있다고 가정 합니다. 그래픽 프로그래밍 개념에 대한 기본 경험도 필요합니다.

또한 [빠른 시작: DirectX 리소스를 설정](setting-up-directx-resources.md) 하 고 이미지를 표시 하 고 [셰이더 및 그리기 기본 형식을 만드는](creating-shaders-and-drawing-primitives.md)것으로 가정 합니다.

**완료 시간:** 20 분

<a name="instructions"></a>Instructions
------------
### <a name="1-defining-cube-variables"></a>1. 큐브 변수 정의

먼저 큐브에 대해 **SimpleCubeVertex** 및 **ConstantBuffer** 구조체를 정의 해야 합니다. 이러한 구조는 큐브에 대 한 꼭 짓 점 및 색과 큐브를 표시 하는 방법을 지정 합니다. [**ComPtr**](/cpp/windows/comptr-class) 를 사용 하 여 [**ID3D11DepthStencilView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview) 및 [**ID3D11Buffer**](/windows/desktop/api/d3d11/nn-d3d11-id3d11buffer) 를 선언 하 고 **ConstantBuffer**의 인스턴스를 선언 합니다.

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

### <a name="2-creating-a-depth-stencil-view"></a>2. 깊이 스텐실 뷰 만들기

렌더링 대상 뷰를 만드는 것 외에도 깊이 스텐실 뷰를 만듭니다. 심도 스텐실 뷰를 사용 하면 Direct3D에서 카메라의 개체 앞에 있는 카메라와 더 가깝게 개체를 효율적으로 렌더링할 수 있습니다. 깊이 스텐실 버퍼에 대 한 보기를 만들려면 깊이 스텐실 버퍼를 만들어야 합니다. 깊이 스텐실 버퍼를 설명 하는 [**D3D11 \_ TEXTURE2D \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_texture2d_desc) 를 채운 다음 [**ID3D11Device:: CreateTexture2D**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createtexture2d) 를 호출 하 여 깊이 스텐실 버퍼를 만듭니다. 깊이 스텐실 뷰를 만들려면 깊이 스텐실 뷰를 설명 하는 [**D3D11 \_ depth \_ 스텐실 \_ 뷰 \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_depth_stencil_view_desc) 를 채우고 깊이 스텐실 뷰 설명 및 깊이 스텐실 버퍼를 [**ID3D11Device:: CreateDepthStencilView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createdepthstencilview)에 전달 합니다.

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

### <a name="3-updating-perspective-with-the-window"></a>3. 창에서 큐브 뷰를 업데이트 합니다.

창 크기에 따라 상수 버퍼에 대 한 큐브 뷰 프로젝션 매개 변수를 업데이트 합니다. 70 ~ 0.01 ~ 100의 깊이 범위를 갖는 수준 필드에 대 한 매개 변수를 수정 합니다.

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

### <a name="4-creating-vertex-and-pixel-shaders-with-color-elements"></a>4. 색 요소를 사용 하 여 꼭 짓 점 및 픽셀 셰이더 만들기

이 앱에서는 이전 자습서에 설명 된 것 보다 더 복잡 한 꼭 짓 점 및 픽셀 셰이더를 만듭니다. [셰이더 및 그리기 기본 형식 만들기](creating-shaders-and-drawing-primitives.md). 앱의 꼭 짓 점 셰이더는 각 꼭 짓 점 위치를 프로젝션 공간으로 변환 하 고 꼭 짓 점 색을 픽셀 셰이더에 전달 합니다.

꼭 짓 점 셰이더 코드의 레이아웃을 설명 하는 [**D3D11 \_ INPUT \_ 요소 \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_input_element_desc) 구조의 앱 배열에는 두 개의 레이아웃 요소가 있습니다. 한 요소는 꼭 짓 점 위치를 정의 하 고 다른 요소는 색을 정의 합니다.

Orbiting 큐브를 정의 하기 위해 꼭 짓 점, 인덱스 및 상수 버퍼를 만듭니다.

**Orbiting 큐브를 정의 하려면**

1.  먼저 큐브를 정의 합니다. 각 꼭 짓 점에는 위치와 함께 색을 할당 합니다. 이렇게 하면 픽셀 셰이더가 각 얼굴을 다르게 색으로 지정할 수 있으므로 얼굴을 구분할 수 있습니다.
2.  다음으로 큐브 정의를 사용 하 여 꼭 짓 점 및 인덱스 버퍼 ([**D3D11 \_ BUFFER \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc) 및 [**D3D11 \_ subresource \_ 데이터**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data))에 대해 설명 합니다. 각 버퍼에 대해 [**ID3D11Device:: CreateBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) 를 한 번씩 호출 합니다.
3.  다음으로, 모델, 뷰 및 프로젝션 매트릭스를 꼭 짓 점 셰이더에 전달 하기 위한 상수 버퍼 ([**D3D11 \_ buffer \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc))를 만듭니다. 나중에 상수 버퍼를 사용 하 여 큐브를 회전 하 고 큐브 뷰 프로젝션을 적용할 수 있습니다. [**ID3D11Device:: CreateBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) 를 호출 하 여 상수 버퍼를 만듭니다.
4.  다음으로 X = 0, Y = 1, Z = 2의 카메라 위치에 해당 하는 보기 변환을 지정 합니다.
5.  마지막으로, 각 프레임을 회전 하 여 큐브에 애니메이션 효과를 주는 데 사용할 *각도* 변수를 선언 합니다.

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

### <a name="5-rotating-and-drawing-the-cube-and-presenting-the-rendered-image"></a>5. 큐브 회전 및 그리기 및 렌더링 된 이미지 표시

무한 루프를 입력 하 여 장면을 지속적으로 렌더링 하 고 표시 합니다. 회전 양만큼 **rotationY** 인라인 함수 (basicmath. h)를 호출 하 여 Y 축을 중심으로 큐브의 모델 매트릭스를 회전 하는 값을 설정 합니다. 그런 다음 [**ID3D11DeviceContext:: UpdateSubresource**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-updatesubresource) 를 호출 하 여 상수 버퍼를 업데이트 하 고 큐브 모델을 회전 합니다. [**ID3D11DeviceContext:: OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) 를 호출 하 여 렌더링 대상을 출력 대상으로 지정 합니다. 이 **OMSetRenderTargets** 호출에서 깊이 스텐실 뷰를 전달 합니다. [**ID3D11DeviceContext:: ClearRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-clearrendertargetview) 를 호출 하 여 렌더링 대상을 진한 파란색으로 지우고 [**ID3D11DeviceContext:: ClearDepthStencilView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-cleardepthstencilview) 를 호출 하 여 깊이 버퍼를 지웁니다.

무한 루프에서는 파란색 표면에도 입방체를 그립니다.

**큐브를 그리려면**

1.  먼저 [**ID3D11DeviceContext:: IASetInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) 를 호출 하 여 꼭 짓 점 버퍼 데이터가 입력-어셈블러 단계로 스트리밍되는 방식을 설명 합니다.
2.  그런 다음 [**ID3D11DeviceContext:: IASetVertexBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) 및 [**ID3D11DeviceContext:: IASetIndexBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer) 을 호출 하 여 꼭 짓 점 및 인덱스 버퍼를 입력-어셈블러 단계에 바인딩합니다.
3.  그런 다음 [**D3D11 \_ 기본 \_ 토폴로지 \_ TRIANGLESTRIP**](/previous-versions/windows/desktop/legacy/ff476189(v=vs.85)) 값을 사용 하 여 [**ID3D11DeviceContext:: IASetPrimitiveTopology**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology) 를 호출 하 여 꼭 짓 점 데이터를 삼각형 스트립으로 해석 하는 입력 어셈블러 단계를 지정 합니다.
4.  그런 다음 [**ID3D11DeviceContext:: VSSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) 를 호출 하 여 꼭 짓 점 셰이더 코드를 사용 하 여 꼭 짓 점 셰이더 단계를 초기화 하 [**:P**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) 고 픽셀 셰이더 코드를 사용 하 여 픽셀 셰이더 단계를 초기화 합니다.
5.  그런 다음 [**ID3D11DeviceContext:: VSSetConstantBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers) 를 호출 하 여 꼭 짓 점 셰이더 파이프라인 단계에서 사용 되는 상수 버퍼를 설정 합니다.
6.  마지막으로 [**ID3D11DeviceContext::D rawindexed**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) 를 호출 하 여 큐브를 그린 다음 렌더링 파이프라인에 제출 합니다.

[**Idxgiswapchain::P 다시 보낸**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) 후 렌더링 된 이미지를 창에 표시 합니다.

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


기본 형식에 깊이, 원근, 색 및 기타 효과를 사용 했습니다.

다음으로 기본 형식에 질감을 적용 합니다.

[기본 형식에 질감 적용](applying-textures-to-primitives.md)

 

 