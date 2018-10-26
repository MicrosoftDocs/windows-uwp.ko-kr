---
author: mtoepke
title: 기본 요소에 텍스처 적용
description: 여기에서는 원형에 깊이 및 효과 사용에서 만든 큐브를 사용해 원시 텍스처 데이터를 로드하고 3D 원형에 적용합니다.
ms.assetid: aeed09e3-c47a-4dd9-d0e8-d1b8bdd7e9b4
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 텍스처, directx
ms.localizationpriority: medium
ms.openlocfilehash: 252613bbea7f4cdb720758d3435cf0920dd93efa
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5567334"
---
# <a name="apply-textures-to-primitives"></a>기본 요소에 텍스처 적용



여기에서는 [원형에 깊이 및 효과 사용](using-depth-and-effects-on-primitives.md)에서 만든 큐브를 사용해 원시 텍스처 데이터를 로드하고 3D 원형에 적용합니다. 또한 광원과의 거리 및 각도에 따라 큐브 표면이 더 밝거나 더 어두워지는 간단한 닷(dot) 제품 조명 모델을 소개합니다.

**목표:** 원형에 텍스처를 적용합니다.

## <a name="prerequisites"></a>필수 조건


사용자가 C++에 익숙하다고 가정합니다. 그래픽 프로그래밍 개념에 대한 기본 경험도 필요합니다.

또한 사용자가 [빠른 시작: DirectX 리소스 설정 및 이미지 표시](setting-up-directx-resources.md), [셰이더 및 그리기 원형 만들기](creating-shaders-and-drawing-primitives.md) 및 [원형에 깊이 및 효과 사용](using-depth-and-effects-on-primitives.md)을 완료했다고 가정합니다.

**완료 시간:** 20분입니다.

<a name="instructions"></a>지침
------------

### <a name="1-defining-variables-for-a-textured-cube"></a>1. 텍스처 처리된 큐브용 변수 정의

먼저 텍스처 처리된 큐브에 대해 **BasicVertex** 및 **ConstantBuffer** 구조를 정의해야 합니다. 이러한 구조는 큐브에 대한 꼭짓점 위치, 방향 및 텍스처를 지정하고 큐브를 보는 방법도 지정합니다. 그렇지 않으면 이전 자습서인 [원형에 깊이 및 효과 사용](using-depth-and-effects-on-primitives.md)에서와 유사한 변수를 선언합니다.

```cpp
struct BasicVertex
{
    DirectX::XMFLOAT3 pos;  // Position
    DirectX::XMFLOAT3 norm; // Surface normal vector
    DirectX::XMFLOAT2 tex;  // Texture coordinate
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

### <a name="2-creating-vertex-and-pixel-shaders-with-surface-and-texture-elements"></a>2. 표면 및 텍스처 요소와 함께 꼭짓점 및 픽셀 셰이더 만들기

여기에서는 이전 자습서인 [원형에 깊이 및 효과 사용](using-depth-and-effects-on-primitives.md)에서보다 좀 더 복잡한 꼭짓점 및 픽셀 셰이더를 만듭니다. 이 앱의 꼭짓점 셰이더는 각 꼭짓점 위치를 투영 공간으로 변환하고 꼭짓점 텍스처 좌표를 픽셀 셰이더에 전달합니다.

꼭짓점 셰이더 코드의 레이아웃을 설명하는 [**D3D11_INPUT_ELEMENT_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180) 구조의 앱 배열에는 세 개의 레이아웃 요소가 있습니다. 첫 번째 요소는 꼭짓점 위치를 정의하고, 두 번째 요소는 표면 일반 꼭짓점(표면이 일반적으로 향하는 방향)을 정의하고, 세 번째 요소는 텍스처 좌표를 정의합니다.

선회하는 텍스처 처리된 큐브를 정의하는 꼭짓점, 인덱스 및 상수 버퍼를 만듭니다.

**선회하는 텍스처 처리된 큐브를 정의하려면**

1.  먼저 큐브를 정의합니다. 각 꼭짓점에 위치, 표면 일반 벡터 및 텍스처 좌표를 할당합니다. 각 면에 대해 서로 다른 일반 벡터 및 텍스처 좌표가 정의될 수 있도록 각 모서리에 여러 꼭짓점을 사용합니다.
2.  그런 다음 큐브 정의를 사용해 꼭짓점 및 인덱스 버퍼([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) 및 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220))를 설명합니다. 각 버퍼에 대해 [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)를 한 번 호출합니다.
3.  이제 꼭짓점 셰이더에 모델, 보기 및 투영 행렬을 전달하기 위해 상수 버퍼([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092))를 만듭니다. 나중에 상수 버퍼를 사용하여 큐브를 회전하고 큐브에 원근 투영을 적용할 수 있습니다. [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)를 호출하여 상수 버퍼를 만듭니다.
4.  마지막으로 X = 0, Y = 1, Z = 2의 카메라 위치에 해당하는 보기 변환을 지정합니다.

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
          // These correspond to the elements of the BasicVertex struct defined above.
          const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
          {
              { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
              { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
              { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT,    0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
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
          // multiple vertices are used for each corner to allow different normal vectors and
          // texture coordinates to be defined for each face.
          BasicVertex cubeVertices[] =
          {
              { DirectX::XMFLOAT3(-0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +Y (top face)
              { DirectX::XMFLOAT3( 0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3( 0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(-0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -Y (bottom face)
              { DirectX::XMFLOAT3( 0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3( 0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3(0.5f,  0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +X (right face)
              { DirectX::XMFLOAT3(0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3(0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3(-0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -X (left face)
              { DirectX::XMFLOAT3(-0.5f,  0.5f,  0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3(-0.5f,  0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +Z (front face)
              { DirectX::XMFLOAT3( 0.5f,  0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3( 0.5f, -0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3( 0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -Z (back face)
              { DirectX::XMFLOAT3(-0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3( 0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },
          };

          unsigned short cubeIndices[] =
          {
              0, 1, 2,
              0, 2, 3,

              4, 5, 6,
              4, 6, 7,

              8, 9, 10,
              8, 10, 11,

              12, 13, 14,
              12, 14, 15,

              16, 17, 18,
              16, 18, 19,

              20, 21, 22,
              20, 22, 23
          };

          D3D11_BUFFER_DESC vertexBufferDesc = {0};
          vertexBufferDesc.ByteWidth = sizeof(BasicVertex) * ARRAYSIZE(cubeVertices);
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
```

### <a name="3-creating-textures-and-samplers"></a>3. 텍스처 및 샘플러 만들기

여기에서는 이전 자습서인 [원형에 깊이 및 효과 사용](using-depth-and-effects-on-primitives.md)에서와 마찬가지로 색을 적용하기보다는 큐브에 텍스처 데이터를 적용합니다.

원시 텍스처 데이터를 사용해 텍스처를 만듭니다.

**텍스처 및 샘플러를 만들려면**

1.  먼저 디스크의 texturedata.bin 파일에서 원시 텍스처 데이터를 읽습니다.
2.  해당 원시 텍스처 데이터를 참조하는 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 구조를 구성합니다.
3.  텍스처를 설명하기 위해 [**D3D11\_TEXTURE2D\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476253) 구조를 채웁니다. 호출의 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 및 **D3D11\_TEXTURE2D\_DESC** 구조를 [**ID3D11Device::CreateTexture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476521)에 전달하여 텍스처를 만듭니다.
4.  셰이더가 텍스처를 사용할 수 있도록 텍스처의 셰이더-리소스 보기를 만듭니다. 셰이더-리소스 보기를 만들기 위해 [**D3D11\_SHADER\_RESOURCE\_VIEW\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476211)를 채워 셰이더-리소스 보기를 설명하고, 셰이더-리소스 보기 설명 및 텍스처를 [**ID3D11Device::CreateShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476519)에 전달합니다. 일반적으로 보기 설명을 텍스처 설명과 같게 합니다.
5.  텍스처의 샘플러 상태를 만듭니다. 이 샘플러 상태는 관련 텍스처 데이터를 사용해 특정 텍스처 좌표에 대해 색을 결정하는 방법을 정의합니다. 샘플러 상태를 설명하기 위해 [**D3D11\_SAMPLER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476207) 구조를 채웁니다. 호출의 **D3D11\_SAMPLER\_DESC** 구조를 [**ID3D11Device::CreateSamplerState**](https://msdn.microsoft.com/library/windows/desktop/ff476518)에 전달하여 샘플러 상태를 만듭니다.
6.  마지막으로 모든 프레임을 회전하여 큐브 애니메이션을 만드는 데 사용할 *degree* 변수를 선언합니다.

```cpp
        
        // Load the raw texture data from disk and construct a subresource description that references it.
        auto loadTDTask = DX::ReadDataAsync(L"texturedata.bin");
          
        auto constructSubresourceTask = loadTDTask.then([this](const std::vector<byte>& vertexShaderBytecode) {  
        
          D3D11_SUBRESOURCE_DATA textureSubresourceData = {0};
          textureSubresourceData.pSysMem = textureData->Data;

          // Specify the size of a row in bytes, known as a priori about the texture data.
          textureSubresourceData.SysMemPitch = 1024;

          // As this is not a texture array or 3D texture, this parameter is ignored.
          textureSubresourceData.SysMemSlicePitch = 0;

          // Create a texture description from information known as a priori about the data.
          // Generalized texture loading code can be found in the Resource Loading sample.
          D3D11_TEXTURE2D_DESC textureDesc = {0};
          textureDesc.Width = 256;
          textureDesc.Height = 256;
          textureDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM;
          textureDesc.Usage = D3D11_USAGE_DEFAULT;
          textureDesc.CPUAccessFlags = 0;
          textureDesc.MiscFlags = 0;

          // Most textures contain more than one MIP level.  For simplicity, this sample uses only one.
          textureDesc.MipLevels = 1;

          // As this will not be a texture array, this parameter is ignored.
          textureDesc.ArraySize = 1;

          // Don't use multi-sampling.
          textureDesc.SampleDesc.Count = 1;
          textureDesc.SampleDesc.Quality = 0;

          // Allow the texture to be bound as a shader resource.
          textureDesc.BindFlags = D3D11_BIND_SHADER_RESOURCE;

          ComPtr<ID3D11Texture2D> texture;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateTexture2D(
                  &textureDesc,
                  &textureSubresourceData,
                  &texture
                  )
              );

          // Once the texture is created, we must create a shader resource view of it
          // so that shaders may use it.  In general, the view description will match
          // the texture description.
          D3D11_SHADER_RESOURCE_VIEW_DESC textureViewDesc;
          ZeroMemory(&textureViewDesc, sizeof(textureViewDesc));
          textureViewDesc.Format = textureDesc.Format;
          textureViewDesc.ViewDimension = D3D11_SRV_DIMENSION_TEXTURE2D;
          textureViewDesc.Texture2D.MipLevels = textureDesc.MipLevels;
          textureViewDesc.Texture2D.MostDetailedMip = 0;

          ComPtr<ID3D11ShaderResourceView> textureView;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateShaderResourceView(
                  texture.Get(),
                  &textureViewDesc,
                  &textureView
                  )
              );

          // Once the texture view is created, create a sampler.  This defines how the color
          // for a particular texture coordinate is determined using the relevant texture data.
          D3D11_SAMPLER_DESC samplerDesc;
          ZeroMemory(&samplerDesc, sizeof(samplerDesc));

          samplerDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;

          // The sampler does not use anisotropic filtering, so this parameter is ignored.
          samplerDesc.MaxAnisotropy = 0;

          // Specify how texture coordinates outside of the range 0..1 are resolved.
          samplerDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
          samplerDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
          samplerDesc.AddressW = D3D11_TEXTURE_ADDRESS_WRAP;

          // Use no special MIP clamping or bias.
          samplerDesc.MipLODBias = 0.0f;
          samplerDesc.MinLOD = 0;
          samplerDesc.MaxLOD = D3D11_FLOAT32_MAX;

          // Don't use a comparison function.
          samplerDesc.ComparisonFunc = D3D11_COMPARISON_NEVER;

          // Border address mode is not used, so this parameter is ignored.
          samplerDesc.BorderColor[0] = 0.0f;
          samplerDesc.BorderColor[1] = 0.0f;
          samplerDesc.BorderColor[2] = 0.0f;
          samplerDesc.BorderColor[3] = 0.0f;

          ComPtr<ID3D11SamplerState> sampler;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateSamplerState(
                  &samplerDesc,
                  &sampler
                  )
              );
        });

        // This value will be used to animate the cube by rotating it every frame;
        float degree = 0.0f;
```

### <a name="4-rotating-and-drawing-the-textured-cube-and-presenting-the-rendered-image"></a>4. 텍스처 처리된 큐브를 회전하고 그리기 및 렌더링된 이미지 표시

이전 자습서에서와 마찬가지로 장면을 계속해서 렌더링 및 표시하기 위해 무한 루프를 입력합니다. 회전 양과 함께 **rotationY** 인라인 함수(BasicMath.h)를 호출하여 Y축을 중심으로 큐브의 모델 행렬을 회전할 값을 설정합니다. [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486)를 호출하여 상수 버퍼를 업데이트하고 큐브 모델을 회전합니다. [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464)를 호출하여 렌더링 대상 및 깊이-스텐실 보기를 지정합니다. [**ID3D11DeviceContext::ClearRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476388)를 호출하여 렌더링 대상을 파란색 단색으로 지우고 [**ID3D11DeviceContext::ClearDepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476387)를 호출하여 깊이 버퍼를 지웁니다.

무한 루프에서, 파란색 표면에 텍스처 처리된 큐브도 그립니다.

**텍스처 처리된 큐브를 그리려면**

1.  먼저 [**ID3D11DeviceContext::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454)을 호출하여 꼭짓점 버퍼 데이터를 입력-어셈블러 단계로 스트리밍하는 방법을 설명합니다.
2.  [**ID3D11DeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) 및 [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453)를 호출하여 꼭짓점 및 인덱스 버퍼를 입력-어셈블러 단계로 바인딩합니다.
3.  [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455)를 [**D3D11\_PRIMITIVE\_TOPOLOGY\_TRIANGLESTRIP**](https://msdn.microsoft.com/library/windows/desktop/ff476189#D3D11_PRIMITIVE_TOPOLOGY_TRIANGLESTRIP) 값과 함께 호출하여, 꼭짓점 데이터를 삼각형 스트립으로 해석할 입력-어셈블러 단계를 지정합니다.
4.  [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493)를 호출하여 꼭짓점 셰이더 단계를 꼭짓점 셰이더 코드로 초기화하고, [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472)를 호출하여 픽셀 셰이더 단계를 픽셀 셰이더 코드로 초기화합니다.
5.  [**ID3D11DeviceContext::VSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476491)를 호출하여 꼭짓점 셰이더 파이프라인 단계에서 사용되는 상수 버퍼를 설정합니다.
6.  [**PSSetShaderResources**](https://msdn.microsoft.com/library/windows/desktop/ff476473)를 호출하여 텍스처의 셰이더-리소스 보기를 픽셀 셰이더 파이프라인 단계로 바인딩합니다.
7.  [**PSSetSamplers**](https://msdn.microsoft.com/library/windows/desktop/ff476471)를 호출하여 샘플러 상태를 픽셀 셰이더 파이프라인 단계로 바인딩합니다.
8.  마지막으로 [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409)를 호출하여 큐브를 그린 후 렌더링 파이프라인에 제출합니다.

이전 자습서에서와 마찬가지로 [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576)를 호출하여, 렌더링된 이미지를 창에 표시합니다.

```cpp
            // Update the constant buffer to rotate the cube model.
            m_constantBufferData.model = DirectX::XMMatrixRotationY(-degree);
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
            UINT stride = sizeof(BasicVertex);
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

            m_d3dDeviceContext->PSSetShaderResources(
                0,
                1,
                textureView.GetAddressOf()
                );

            m_d3dDeviceContext->PSSetSamplers(
                0,
                1,
                sampler.GetAddressOf()
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

## <a name="summary"></a>요약


원시 텍스처 데이터를 로드하여 3D 원형에 적용했습니다.

 

 




