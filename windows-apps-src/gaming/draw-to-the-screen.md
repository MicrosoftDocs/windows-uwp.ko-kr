---
author: mtoepke
title: "화면에 그리기"
description: "마지막으로, 화면에 회전하는 큐브를 그리는 코드를 포팅합니다."
ms.assetid: cc681548-f694-f613-a19d-1525a184d4ab
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 1b7431c20e25173a0aa3f8d6ee0d407be869d60a

---

# 화면에 그리기


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)
-   [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)
-   [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)

마지막으로, 화면에 회전하는 큐브를 그리는 코드를 포팅합니다.

OpenGL ES 2.0에서 그리기 컨텍스트는 EGLContext 형식으로 정의되며, 이 형식에는 창에 표시되는 최종 이미지를 구성하는 데 사용되는 렌더링 대상에 그리는 데 필요한 리소스 창과 창 및 화면 매개 변수가 포함됩니다. 이 컨텍스트를 사용하여 디스플레이에 셰이더 파이프라인의 결과를 올바르게 표시하도록 그래픽 리소스를 구성합니다. 기본 리소스 중 하나는 "백 버퍼"(또는 "프레임 버퍼 개체")이며, 여기에는 디스플레이에 표시할 준비가 된 최종, 작성된 렌더링 대상이 포함됩니다.

Direct3D를 사용하면 디스플레이에 그리기 위한 그래픽 리소스를 구성하는 프로세스가 더욱 교육적으로 되어 좀 더 많은 API를 필요로 하게 됩니다. (그렇지만 Microsoft Visual Studio Direct3D 템플릿이 이 프로세스를 현저히 단순화할 수 있습니다.) 컨텍스트(Direct 3D 디바이스 컨텍스트라고도 함)를 가져오려면 먼저 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) 개체를 가져와서 사용하여 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 개체를 만들고 구성해야 합니다. 이러한 두 개체는 디스플레이에 그리는 데 필요한 특정 리소스를 구성하는 데 함께 사용됩니다.

요약하자면, DXGI API는 그래픽 어댑터에 직접 관련된 리소스를 관리하기 위한 API를 주로 포함하고 있고, Direct3D는 GPU와 CPU에서 실행되는 주 프로그램 간을 연결할 수 있게 해주는 API를 포함하고 있습니다.

이 샘플에서의 비교를 위해 각 API의 관련 형식이 다음에 나와 있습니다.

-   [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575): 그래픽 장치 및 해당 리소스의 가상 표현을 제공합니다.
-   [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598): 버퍼를 구성하고 렌더링 명령을 실행할 수 있는 인터페이스를 제공합니다.
-   [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631): 스왑 체인은 OpenGL ES 2.0의 백 버퍼와 유사합니다. 스왑 체인은 디스플레이를 위한 렌더링된 최종 이미지가 포함된 그래픽 어댑터상의 메모리 영역이며, 화면에 최신 렌더를 표시하기 위해 작성 및 "스왑"될 수 있는 여러 버퍼를 포함하고 있기 때문에 "스왑 체인"이라고 합니다.
-   [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582): Direct3D 디바이스 컨텍스트가 그리는 2D 비트맵 버퍼가 포함되어 있고, 이는 스왑 체인에 의해 표시됩니다. OpenGL ES 2.0과 마찬가지로, 여러 렌더링 대상이 있을 수 있으며 이 중 일부는 스왑 체인에 바인딩되어 있지 않지만 멀티패스 음영 기술에 사용됩니다.

템플릿에서 렌더러 개체에는 다음과 같은 필드가 포함되어 있습니다.

Direct3D 11: 장치 및 디바이스 컨텍스트 선언

``` syntax
Platform::Agile<Windows::UI::Core::CoreWindow>       m_window;

Microsoft::WRL::ComPtr<ID3D11Device1>                m_d3dDevice;
Microsoft::WRL::ComPtr<ID3D11DeviceContext1>          m_d3dContext;
Microsoft::WRL::ComPtr<IDXGISwapChain1>                      m_swapChainCoreWindow;
Microsoft::WRL::ComPtr<ID3D11RenderTargetView>          m_d3dRenderTargetViewWin;
```

백 버퍼가 렌더링 대상으로 구성되어 스왑 체인에 제공되는 방법은 다음과 같습니다.

``` syntax
ComPtr<ID3D11Texture2D> backBuffer;
m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(backBuffer));
m_d3dDevice->CreateRenderTargetView(
  backBuffer.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);
```

스왑 체인이 디스플레이에 사용할 수 있는 "백 버퍼"로 텍스처를 나타내는 [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)에 대한 [**IDXGISurface1**](https://msdn.microsoft.com/library/windows/desktop/ff471343)을 Direct3D 런타임에서 암시적으로 만듭니다.

Direct3D 장치 및 디바이스 컨텍스트와 렌더링 대상의 초기화 및 구성은 Direct3D 템플릿의 사용자 지정 **CreateDeviceResources** 및 **CreateWindowSizeDependentResources** 메서드에 있습니다.

EGL 및 EGLContext 형식에 관련된 Direct3D 디바이스 컨텍스트에 대한 자세한 내용은 [DXGI 및 Direct3D로 EGL 코드 포팅](moving-from-egl-to-dxgi.md)을 읽어 보세요.

## 지침

### 1단계: 장면 렌더링 및 표시

큐브 데이터를 업데이트한 후(이 경우 약간 y축을 기준으로 회전) Render 메서드는 뷰포트를 그리기 컨텍스트(EGLContext)의 차원으로 설정합니다. 이 컨텍스트에는 구성된 디스플레이(EGLDisplay)를 사용하여 창 화면(EGLSurface)에 표시할 색 버퍼가 포함됩니다. 이때 예제에서는 꼭짓점 데이터 특성을 업데이트하고, 인덱스 버퍼를 다시 바인딩하고, 큐브를 그리고, 디스플레이 화면에 음영 파이프라인으로 그려진 색 버퍼에서 스왑합니다.

OpenGL ES 2.0: 디스플레이를 위해 프레임 렌더링

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

Direct3D 11에서 이 프로세스는 매우 유사합니다. (Direct3D 템플릿의 뷰포트 및 렌더링 대상 구성을 사용하고 있다고 가정합니다.)

-   [**ID3D11DeviceContext1::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/hh446790)에 대한 호출로 상수 버퍼(이 경우, 모델-보기-투영 행렬)를 업데이트합니다.
-   [**ID3D11DeviceContext1::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456)로 꼭짓점 버퍼를 설정합니다.
-   [**ID3D11DeviceContext1::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453)로 인덱스 버퍼를 설정합니다.
-   [**ID3D11DeviceContext1::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455)로 특정 삼각형 토폴로지(삼각형 목록)를 설정합니다.
-   [**ID3D11DeviceContext1::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454)으로 꼭짓점 버퍼의 입력 레이아웃을 설정합니다.
-   [**ID3D11DeviceContext1::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493)로 꼭짓점 셰이더를 바인딩합니다.
-   [**ID3D11DeviceContext1::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472)로 조각 셰이더를 바인딩합니다.
-   셰이더를 통해 인덱싱된 꼭짓점을 보내고 색 결과를 [**ID3D11DeviceContext1::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409)로 렌더링 대상 버퍼에 출력합니다.
-   [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797)로 렌더링 대상 버퍼를 표시합니다.

Direct3D 11: 디스플레이를 위해 프레임 렌더링

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

[**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797)을 호출하면 구성된 디스플레이에 프레임이 출력됩니다.

## 이전 단계


[GLSL 포팅](port-the-glsl.md)

## 설명

이 예제에서는 특히 UWP(유니버설 Windows 플랫폼) DirectX 앱에 대한 장치 리소스 구성의 복잡성이 심하기 때문에 해당 내용에 주석을 달았습니다. 전체 템플릿 코드를 검토하되, 특히 창과 장치 리소스 설정 및 관리를 수행하는 부분을 검토하는 것이 좋습니다. UWP 앱은 회전 이벤트뿐만 아니라 일시 중단/다시 시작 이벤트를 지원해야 하며, 템플릿은 인터페이스 손실이나 디스플레이 매개 변수 변경을 처리하기 위한 모범 사례를 보여 줍니다.

## 관련 항목


* [방법: 간단한 OpenGL ES 2.0 렌더러를 Direct3D 11로 포팅](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [셰이더 개체 포팅](port-the-shader-config.md)
* [GLSL 포팅](port-the-glsl.md)
* [화면에 그리기](draw-to-the-screen.md)

 

 







<!--HONumber=Jun16_HO4-->


