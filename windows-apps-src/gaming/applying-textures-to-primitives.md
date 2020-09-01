---
title: 기본 형식에 질감 적용
description: 여기서는 원시 질감 데이터를 로드 하 고 기본 형식에 대해 깊이와 효과를 사용 하 여 만든 큐브를 사용 하 여 해당 데이터를 3D 기본 형식에 적용 합니다.
ms.assetid: aeed09e3-c47a-4dd9-d0e8-d1b8bdd7e9b4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 질감, directx
ms.localizationpriority: medium
ms.openlocfilehash: 349dcc4e8cc65a6265579e84c6810dcac79938a1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156487"
---
# <a name="apply-textures-to-primitives"></a>기본 형식에 질감 적용

여기서는 원시 질감 데이터를 로드 하 고 [기본 형식에 대해 깊이와 효과를 사용](using-depth-and-effects-on-primitives.md)하 여 만든 큐브를 사용 하 여 해당 데이터를 3d 기본 형식에 적용 합니다. 또한 간단한 점-제품 조명 모델을 소개 합니다. 여기서는 큐브 서피스가 광원을 기준으로 하는 거리 및 각도에 따라 더 어둡거나 더 어두워집니다.

**목표:** 기본 형식에 질감을 적용 하려면입니다.

## <a name="prerequisites"></a>필수 구성 요소

이 항목을 최대한 활용 하려면 c + +에 대해 잘 알고 있어야 합니다. 또한 그래픽 프로그래밍 개념에 대 한 기본 경험이 필요 합니다. 그리고 가급적 [빠른 시작: DirectX 리소스 설정, 이미지 표시](setting-up-directx-resources.md), [셰이더 및 그리기 기본 형식 만들기](creating-shaders-and-drawing-primitives.md), [기본 형식에 대 한 깊이 및 효과 사용](using-depth-and-effects-on-primitives.md)과 같은 작업을 이미 수행 해야 합니다.

**완료 시간:** 20 분

<a name="instructions"></a>Instructions
------------
### <a name="1-defining-variables-for-a-textured-cube"></a>1. 텍스처 큐브에 대 한 변수 정의

먼저, 텍스처 큐브에 대해 **Basicvertex** 및 **ConstantBuffer** 구조를 정의 해야 합니다. 이러한 구조는 큐브에 대 한 꼭 짓 점, 방향 및 텍스처와 큐브를 표시 하는 방법을 지정 합니다. 그렇지 않은 경우에는 [기본 형식에 대 한 깊이와 효과를 사용 하 여](using-depth-and-effects-on-primitives.md)이전 자습서와 유사한 변수를 선언 합니다.

```cppcx
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

### <a name="2-creating-vertex-and-pixel-shaders-with-surface-and-texture-elements"></a>2. 표면 및 질감 요소를 사용 하 여 꼭 짓 점 및 픽셀 셰이더 만들기

여기서는 [기본 형식에 대 한 깊이와 효과를 사용 하 여](using-depth-and-effects-on-primitives.md)이전 자습서에서 보다 복잡 한 꼭 짓 점 및 픽셀 셰이더를 만듭니다. 이 앱의 꼭 짓 점 셰이더는 각 꼭 짓 점 위치를 프로젝션 공간으로 변환 하 고 꼭 짓 점 질감 좌표를 픽셀 셰이더에 전달 합니다.

꼭 짓 점 셰이더 코드의 레이아웃을 설명 하는 [**D3D11 \_ INPUT \_ 요소 \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_input_element_desc) 구조의 앱 배열에는 세 가지 레이아웃 요소가 있습니다. 한 요소는 꼭 짓 점 위치를 정의 하 고, 다른 요소는 표면 법선 벡터 (표면이 일반적으로 직면 하는 방향)를 정의 하 고, 세 번째 요소는 질감 좌표를 정의 합니다.

Orbiting 텍스처 큐브를 정의 하는 꼭 짓 점, 인덱스 및 상수 버퍼를 만듭니다.

**Orbiting 텍스처 큐브를 정의 하려면**

1.  먼저 큐브를 정의 합니다. 각 꼭 짓 점에는 위치, 표면 법선 벡터 및 질감 좌표가 할당 됩니다. 각 모퉁이에 대해 여러 개의 꼭 짓 점을 사용 하 여 각 면에 대해 서로 다른 일반 벡터 및 질감 좌표를 정의할 수 있습니다.
2.  다음으로 큐브 정의를 사용 하 여 꼭 짓 점 및 인덱스 버퍼 ([**D3D11 \_ BUFFER \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc) 및 [**D3D11 \_ subresource \_ 데이터**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data))에 대해 설명 합니다. 각 버퍼에 대해 [**ID3D11Device:: CreateBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) 를 한 번씩 호출 합니다.
3.  다음으로, 모델, 뷰 및 프로젝션 매트릭스를 꼭 짓 점 셰이더에 전달 하기 위한 상수 버퍼 ([**D3D11 \_ buffer \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc))를 만듭니다. 나중에 상수 버퍼를 사용 하 여 큐브를 회전 하 고 큐브 뷰 프로젝션을 적용할 수 있습니다. [**ID3D11Device:: CreateBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) 를 호출 하 여 상수 버퍼를 만듭니다.
4.  마지막으로 X = 0, Y = 1, Z = 2의 카메라 위치에 해당 하는 뷰 변환을 지정 합니다.

```cppcx
auto loadVSTask = DX::ReadDataAsync(L"SimpleVertexShader.cso");
auto loadPSTask = DX::ReadDataAsync(L"SimplePixelShader.cso");

auto createVSTask = loadVSTask.then([this](const std::vector<byte>& vertexShaderBytecode)
{
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
auto createPSTask = loadPSTask.then([this](const std::vector<byte>& pixelShaderBytecode)
{
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
auto createCubeTask = (createPSTask && createVSTask).then([this]()
{
    // In the array below, which will be used to initialize the cube vertex buffers,
    // multiple vertices are used for each corner to allow different normal vectors and
    // texture coordinates to be defined for each face.
    BasicVertex cubeVertices[] =
    {
        { DirectX::XMFLOAT3(-0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +Y (top face)
        { DirectX::XMFLOAT3(0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
        { DirectX::XMFLOAT3(0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
        { DirectX::XMFLOAT3(-0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

        { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -Y (bottom face)
        { DirectX::XMFLOAT3(0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
        { DirectX::XMFLOAT3(0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
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
        { DirectX::XMFLOAT3(0.5f,  0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
        { DirectX::XMFLOAT3(0.5f, -0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
        { DirectX::XMFLOAT3(-0.5f, -0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

        { DirectX::XMFLOAT3(0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -Z (back face)
        { DirectX::XMFLOAT3(-0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
        { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
        { DirectX::XMFLOAT3(0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },
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

    D3D11_BUFFER_DESC vertexBufferDesc = { 0 };
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

    D3D11_BUFFER_DESC constantBufferDesc = { 0 };
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
        -1.00000000f, 0.00000000f, 0.00000000f, 0.00000000f,
        0.00000000f, 0.89442718f, 0.44721359f, 0.00000000f,
        0.00000000f, 0.44721359f, -0.89442718f, -2.23606800f,
        0.00000000f, 0.00000000f, 0.00000000f, 1.00000000f
    );
});
```

### <a name="3-creating-textures-and-samplers"></a>3. 질감 및 샘플러 만들기

여기서는 [기본 형식에 대 한 깊이와 효과를 사용 하 여](using-depth-and-effects-on-primitives.md)이전 자습서에서와 같이 색을 적용 하는 대신 질감 데이터를 큐브에 적용 합니다.

원시 질감 데이터를 사용 하 여 질감을 만듭니다.

**질감 및 샘플러를 만들려면**

1.  먼저 디스크의 texturedata 파일에서 원시 질감 데이터를 읽습니다.
2.  그런 다음 원시 질감 데이터를 참조 하는 [**D3D11 \_ subresource \_ 데이터**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) 구조를 생성 합니다.
3.  그런 다음 [**D3D11 \_ TEXTURE2D \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_texture2d_desc) 구조를 채워서 질감을 설명 합니다. 그런 다음 [**ID3D11Device:: CreateTexture2D**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createtexture2d) 에 대 한 호출에서 [**D3D11 \_ Subresource \_ DATA**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) 및 **D3D11 \_ TEXTURE2D \_ DESC** 구조를 전달 하 여 질감을 만듭니다.
4.  다음에는 셰이더가 텍스처를 사용할 수 있도록 질감의 셰이더 리소스 뷰를 만듭니다. 셰이더 리소스 뷰를 만들려면 [**D3D11 \_ 셰이더 \_ 리소스 뷰 \_ \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_shader_resource_view_desc) 를 채워서 셰이더 리소스 뷰를 설명 하 고 셰이더 리소스 뷰 설명 및 텍스처를 [**ID3D11Device:: CreateShaderResourceView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createshaderresourceview)에 전달 합니다. 일반적으로 보기 설명과 질감 설명을 일치 시킵니다.
5.  다음으로, 질감에 대 한 샘플러 상태를 만듭니다. 이 샘플러 상태는 관련 질감 데이터를 사용 하 여 특정 질감 좌표의 색이 결정 되는 방법을 정의 합니다. 샘플러 상태를 설명 하는 [**D3D11 \_ 샘플러 \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_sampler_desc) 구조를 채웁니다. 그런 다음 [**ID3D11Device:: CreateSamplerState**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createsamplerstate) 에 대 한 호출에서 **D3D11 \_ 샘플러 \_ DESC** 구조를 전달 하 여 샘플러 상태를 만듭니다.
6.  마지막으로, 각 프레임을 회전 하 여 큐브에 애니메이션 효과를 주는 데 사용할 *각도* 변수를 선언 합니다.

```cppcx
// Load the raw texture data from disk and construct a subresource description that references it.
auto loadTDTask = DX::ReadDataAsync(L"texturedata.bin");

auto constructSubresourceTask = loadTDTask.then([this](const std::vector<byte>& textureData)
{
    D3D11_SUBRESOURCE_DATA textureSubresourceData = { 0 };
    textureSubresourceData.pSysMem = textureData.data();

    // Specify the size of a row in bytes, known as a priori about the texture data.
    textureSubresourceData.SysMemPitch = 1024;

    // As this is not a texture array or 3D texture, this parameter is ignored.
    textureSubresourceData.SysMemSlicePitch = 0;

    // Create a texture description from information known as a priori about the data.
    // Generalized texture loading code can be found in the Resource Loading sample.
    D3D11_TEXTURE2D_DESC textureDesc = { 0 };
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

### <a name="4-rotating-and-drawing-the-textured-cube-and-presenting-the-rendered-image"></a>4. 질감이 있는 큐브를 회전 하 고 그리고 렌더링 된 이미지를 표시 합니다.

이전 자습서에서와 마찬가지로 장면을 계속해서 렌더링 및 표시하기 위해 무한 루프를 입력합니다. 회전 양만큼 **rotationY** 인라인 함수 (basicmath. h)를 호출 하 여 Y 축을 중심으로 큐브의 모델 매트릭스를 회전 하는 값을 설정 합니다. 그런 다음 [**ID3D11DeviceContext:: UpdateSubresource**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-updatesubresource) 를 호출 하 여 상수 버퍼를 업데이트 하 고 큐브 모델을 회전 합니다. 그런 다음 [**ID3D11DeviceContext:: OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) 를 호출 하 여 렌더링 대상과 깊이 스텐실 뷰를 지정 합니다. [**ID3D11DeviceContext:: ClearRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-clearrendertargetview) 를 호출 하 여 렌더링 대상을 진한 파란색으로 지우고 [**ID3D11DeviceContext:: ClearDepthStencilView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-cleardepthstencilview) 를 호출 하 여 깊이 버퍼를 지웁니다.

무한 루프에서는 녹색 표면에 텍스처 정육면체도 그립니다.

**텍스처 큐브를 그리려면**

1.  먼저 [**ID3D11DeviceContext:: IASetInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) 를 호출 하 여 꼭 짓 점 버퍼 데이터가 입력-어셈블러 단계로 스트리밍되는 방식을 설명 합니다.
2.  그런 다음 [**ID3D11DeviceContext:: IASetVertexBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) 및 [**ID3D11DeviceContext:: IASetIndexBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer) 을 호출 하 여 꼭 짓 점 및 인덱스 버퍼를 입력-어셈블러 단계에 바인딩합니다.
3.  그런 다음 [**D3D11 \_ 기본 \_ 토폴로지 \_ TRIANGLESTRIP**](/previous-versions/windows/desktop/legacy/ff476189(v=vs.85)) 값을 사용 하 여 [**ID3D11DeviceContext:: IASetPrimitiveTopology**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology) 를 호출 하 여 꼭 짓 점 데이터를 삼각형 스트립으로 해석 하는 입력 어셈블러 단계를 지정 합니다.
4.  그런 다음 [**ID3D11DeviceContext:: VSSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) 를 호출 하 여 꼭 짓 점 셰이더 코드를 사용 하 여 꼭 짓 점 셰이더 단계를 초기화 하 [**:P**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) 고 픽셀 셰이더 코드를 사용 하 여 픽셀 셰이더 단계를 초기화 합니다.
5.  그런 다음 [**ID3D11DeviceContext:: VSSetConstantBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers) 를 호출 하 여 꼭 짓 점 셰이더 파이프라인 단계에서 사용 되는 상수 버퍼를 설정 합니다.
6.  그런 다음 [**PSSetShaderResources**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshaderresources) 를 호출 하 여 질감의 셰이더 리소스 뷰를 픽셀 셰이더 파이프라인 단계에 바인딩합니다.
7.  다음으로 [**Pssetsamplers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetsamplers) 를 호출 하 여 샘플러 상태를 픽셀 셰이더 파이프라인 단계로 설정 합니다.
8.  마지막으로 [**ID3D11DeviceContext::D rawindexed**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) 를 호출 하 여 큐브를 그린 다음 렌더링 파이프라인에 제출 합니다.

이전 자습서에서와 같이 [**Idxgiswapchain::P 다시 보낸**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) 것을 호출 하 여 렌더링 된 이미지를 창에 표시 합니다.

```cppcx
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

이 항목에서는 원시 질감 데이터를 로드 하 고이 데이터를 3D 기본 형식에 적용 했습니다.