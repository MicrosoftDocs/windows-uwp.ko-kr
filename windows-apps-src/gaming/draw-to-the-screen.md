---
title: 화면에 그리기
description: GXDI 및 Direct3D Api를 사용 하 여 OpenGL 코드를 DirectX로 이식 하 고 회전 하는 큐브를 화면에 그리는 방법에 대해 알아봅니다.
ms.assetid: cc681548-f694-f613-a19d-1525a184d4ab
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, 그래픽
ms.localizationpriority: medium
ms.openlocfilehash: 6499f860dce0c3bb4f596b372a2de02b04b0d4ac
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175347"
---
# <a name="draw-to-the-screen"></a>화면에 그리기




**중요 API**

-   [**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)
-   [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview)
-   [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1)

마지막으로 회전 하는 큐브를 그리는 코드를 화면에 이식 합니다.

OpenGL ES 2.0에서 그리기 컨텍스트는 창에 표시 되는 최종 이미지를 구성 하는 데 사용 되는 렌더링 대상으로 그리기에 필요한 리소스 뿐만 아니라 창 및 surface 매개 변수를 포함 하는 E글 컨텍스트 형식으로 정의 됩니다. 이 컨텍스트를 사용 하 여 디스플레이에 셰이더 파이프라인의 결과를 올바르게 표시 하도록 그래픽 리소스를 구성할 수 있습니다. 기본 리소스 중 하나는 표시에 사용할 수 있는 최종 합성 렌더링 대상을 포함 하는 "백 버퍼" (또는 "프레임 버퍼 개체")입니다.

Direct3D를 사용 하는 경우 디스플레이에 그리기 위한 그래픽 리소스를 구성 하는 프로세스는 더 didactic 더 많은 Api가 필요 합니다. Microsoft Visual Studio Direct3D 템플릿에서는이 프로세스를 크게 간소화할 수 있습니다. 컨텍스트 (Direct3D 장치 컨텍스트)를 가져오려면 먼저 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 개체를 가져온 다음이 개체를 사용 하 여 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 개체를 만들고 구성 해야 합니다. 이러한 두 개체를 함께 사용 하 여 표시로 그리기에 필요한 특정 리소스를 구성 합니다.

간단히 말해서, DXGI Api에는 그래픽 어댑터와 직접 관련 된 리소스를 관리 하기 위한 Api가 포함 되어 있으며, Direct3D에는 CPU에서 실행 되는 GPU와 기본 프로그램 간 인터페이스를 사용할 수 있도록 하는 Api가 포함 되어 있습니다.

이 샘플의 비교를 위해 각 API의 관련 형식은 다음과 같습니다.

-   [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1): 그래픽 장치 및 해당 리소스에 대 한 가상 표현을 제공 합니다.
-   [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1): 버퍼를 구성 하 고 렌더링 명령을 실행 하기 위한 인터페이스를 제공 합니다.
-   [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1): 스왑 체인은 OpenGL ES 2.0의 백 버퍼와 유사 합니다. 표시를 위해 렌더링 된 최종 이미지를 포함 하는 그래픽 어댑터의 메모리 영역입니다. 이를 "스왑 체인" 이라고 하며,이를 "교환" 하 여 화면에 최신 렌더링을 제공할 수 있습니다.
-   [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview):이는 Direct3D 장치 컨텍스트가 그려지는 2d 비트맵 버퍼를 포함 하 고 교환 체인에서 제공 됩니다. OpenGL ES 2.0와 마찬가지로 여러 렌더링 대상이 있을 수 있으며, 일부는 스왑 체인에 바인딩되지 않지만 다중 패스 음영 기술에 사용 됩니다.

템플릿에서 렌더러 개체에는 다음 필드가 포함 됩니다.

Direct3D 11: 장치 및 장치 컨텍스트 선언

``` syntax
Platform::Agile<Windows::UI::Core::CoreWindow>       m_window;

Microsoft::WRL::ComPtr<ID3D11Device1>                m_d3dDevice;
Microsoft::WRL::ComPtr<ID3D11DeviceContext1>          m_d3dContext;
Microsoft::WRL::ComPtr<IDXGISwapChain1>                      m_swapChainCoreWindow;
Microsoft::WRL::ComPtr<ID3D11RenderTargetView>          m_d3dRenderTargetViewWin;
```

백 버퍼를 렌더링 대상으로 구성 하 고 스왑 체인에 제공 하는 방법은 다음과 같습니다.

``` syntax
ComPtr<ID3D11Texture2D> backBuffer;
m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(backBuffer));
m_d3dDevice->CreateRenderTargetView(
  backBuffer.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);
```

Direct3D 런타임은 스왑 체인이 표시 하는 데 사용할 수 있는 "백 버퍼"로 텍스처를 나타내는 [**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)에 대 한 [**IDXGISurface1**](/windows/desktop/api/dxgi/nn-dxgi-idxgisurface1) 를 암시적으로 만듭니다.

Direct3d 장치 및 장치 컨텍스트의 초기화 및 구성과 렌더링 대상은 Direct3D 템플릿의 custom **CreateDeviceResources** and **CreateWindowSizeDependentResources** 메서드를 통해 찾을 수 있습니다.

관련 된 Direct3D 장치 컨텍스트에 대 한 자세한 내용은이에 대 한 정보를 [참조 하세요.](moving-from-egl-to-dxgi.md)

## <a name="instructions"></a>Instructions

### <a name="step-1-rendering-the-scene-and-displaying-it"></a>1 단계: 장면 렌더링 및 표시

큐브 데이터를 업데이트 한 후 (이 경우 y 축 주위에서 조금씩 회전) Render 메서드는 뷰포트를 그리기 컨텍스트의 차원 (E글 컨텍스트)으로 설정 합니다. 이 컨텍스트에는 구성 된 디스플레이 (E글 표시)를 사용 하 여 창 화면에 표시 되는 색 버퍼 (EGLSurface)가 포함 됩니다. 이번에는이 예제에서 꼭 짓 점 데이터 특성을 업데이트 하 고, 인덱스 버퍼를 다시 바인딩하고, 큐브를 그리고, 음영 파이프라인으로 그려진 색 버퍼를 표시 화면으로 바꿉니다.

OpenGL ES 2.0: 표시할 프레임 렌더링

``` syntax
void Render(GraphicsContext *drawContext)
{
  Renderer *renderer = drawContext->renderer;

  int loc;
   
  // Set the viewport
  glViewport ( 0, 0, drawContext->width, drawContext->height );
   
   
  // Clear the color buffer
  glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
  glEnable(GL_DEPTH_TEST);


  // Use the program object
  glUseProgram (renderer->programObject);

  // Load the a_position attribute with the vertex position portion of a vertex buffer element
  loc = glGetAttribLocation(renderer->programObject, "a_position");
  glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
      sizeof(Vertex), 0);
  glEnableVertexAttribArray(loc);

  // Load the a_color attribute with the color position portion of a vertex buffer element
  loc = glGetAttribLocation(renderer->programObject, "a_color");
  glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
      sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
  glEnableVertexAttribArray(loc);

  // Bind the index buffer
  glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);

  // Load the MVP matrix
  glUniformMatrix4fv(renderer->mvpLoc, 1, GL_FALSE, (GLfloat*) &renderer->mvpMatrix.m[0][0]);

  // Draw the cube
  glDrawElements(GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);

  eglSwapBuffers(drawContext->eglDisplay, drawContext->eglSurface);
}
```

Direct3D 11에서 프로세스는 매우 유사 합니다. (Direct3D 템플릿에서 뷰포트 및 렌더링 대상 구성을 사용 하 고 있다고 가정 합니다.

-   상수 버퍼 (이 경우 모델-뷰-프로젝션 행렬)를 [**ID3D11DeviceContext1:: UpdateSubresource**](/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-updatesubresource1)에 대 한 호출로 업데이트 합니다.
-   [**ID3D11DeviceContext1:: IASetVertexBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers)를 사용 하 여 꼭 짓 점 버퍼를 설정 합니다.
-   [**ID3D11DeviceContext1:: IASetIndexBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer)를 사용 하 여 인덱스 버퍼를 설정 합니다.
-   [**ID3D11DeviceContext1:: IASetPrimitiveTopology**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology)를 사용 하 여 특정 삼각형 토폴로지 (삼각형 목록)를 설정 합니다.
-   [**ID3D11DeviceContext1:: IASetInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout)를 사용 하 여 꼭 짓 점 버퍼의 입력 레이아웃을 설정 합니다.
-   [**ID3D11DeviceContext1:: VSSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader)를 사용 하 여 꼭 짓 점 셰이더를 바인딩합니다.
-   [**ID3D11DeviceContext1::P SSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader)를 사용 하 여 조각 셰이더를 바인딩합니다.
-   셰이더를 통해 인덱싱된 버텍스를 보내고 [**ID3D11DeviceContext1::D rawindexed**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed)로 색 결과를 렌더링 대상 버퍼에 출력 합니다.
-   [**IDXGISwapChain1::P resent1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)를 사용 하 여 렌더링 대상 버퍼를 표시 합니다.

Direct3D 11: 표시할 프레임 렌더링

``` syntax
void RenderObject::Render()
{
  // ...

  // Only update shader resources that have changed since the last frame.
  m_d3dContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    NULL,
    &m_constantBufferData,
    0,
    0);

  // Set up the IA stage corresponding to the current draw operation.
  UINT stride = sizeof(VertexPositionColor);
  UINT offset = 0;
  m_d3dContext->IASetVertexBuffers(
    0,
    1,
    m_vertexBuffer.GetAddressOf(),
    &stride,
    &offset);

  m_d3dContext->IASetIndexBuffer(
    m_indexBuffer.Get(),
    DXGI_FORMAT_R16_UINT,
    0);

  m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
  m_d3dContext->IASetInputLayout(m_inputLayout.Get());

  // Set up the vertex shader corresponding to the current draw operation.
  m_d3dContext->VSSetShader(
    m_vertexShader.Get(),
    nullptr,
    0);

  m_d3dContext->VSSetConstantBuffers(
    0,
    1,
    m_constantBuffer.GetAddressOf());

  // Set up the pixel shader corresponding to the current draw operation.
  m_d3dContext->PSSetShader(
    m_pixelShader.Get(),
    nullptr,
    0);

  m_d3dContext->DrawIndexed(
    m_indexCount,
    0,
    0);

    // ...

  m_swapChainCoreWindow->Present1(1, 0, &parameters);
}

```

[**IDXGISwapChain1::P resent1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1) 가 호출 되 면 프레임이 구성 된 표시로 출력 됩니다.

## <a name="previous-step"></a>이전 단계


[인 포트](port-the-glsl.md)

## <a name="remarks"></a>설명

이 예제는 주석을 다 (UWP) DirectX 앱에 대 유니버설 Windows 플랫폼 한 장치 리소스를 구성 하는 복잡 한 정도를 합니다. 전체 템플릿 코드, 특히 창 및 장치 리소스 설정 및 관리를 수행 하는 파트를 검토 하는 것이 좋습니다. UWP 앱은 일시 중단/다시 시작 이벤트 뿐만 아니라 회전 이벤트를 지원 해야 하며, 템플릿에서 인터페이스 손실 또는 표시 매개 변수의 변경에 대 한 모범 사례를 보여 줍니다.

## <a name="related-topics"></a>관련 항목


* [방법: 간단한 OpenGL ES 2.0 렌더러를 Direct3D 11로 이식](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [셰이더 개체 이식](port-the-shader-config.md)
* [인 포트](port-the-glsl.md)
* [화면에 그리기](draw-to-the-screen.md)

 

 