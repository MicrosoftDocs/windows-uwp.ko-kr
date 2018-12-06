---
title: 셰이더 개체 포팅
description: OpenGL ES 2.0에서 간단한 렌더러를 포팅하는 경우 첫 번째 단계는 Direct3D 11에서 해당하는 꼭짓점 및 조각 셰이더 개체를 설정하고 주 프로그램이 셰이더 개체가 컴파일된 후 이 셰이더 개체와 통신할 수 있는지 확인하는 것입니다.
ms.assetid: 0383b774-bc1b-910e-8eb6-cc969b3dcc08
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, 포트, 셰이더, direct3d, opengl
ms.localizationpriority: medium
ms.openlocfilehash: f061d31ca779cb4c6cbe76f163e190996a6985cb
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8755339"
---
# <a name="port-the-shader-objects"></a>셰이더 개체 포팅




**중요 API**

-   [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379)
-   [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)

OpenGL ES 2.0에서 간단한 렌더러를 포팅하는 경우 첫 번째 단계는 Direct3D 11에서 해당하는 꼭짓점 및 조각 셰이더 개체를 설정하고 주 프로그램이 셰이더 개체가 컴파일된 후 이 셰이더 개체와 통신할 수 있는지 확인하는 것입니다.

> **참고**  Direct3D 프로젝트를 새로 했나요? 그렇지 않은 경우 지침에 따라 [UWP(유니버설 Windows 플랫폼)용 새 DirectX 11 프로젝트를 만듭니다](user-interface.md). 이 연습에서는 화면에 그리기 위한 DXGI 및 Direct3D 리소스를 만들었다고 가정합니다. 이 리소스는 템플릿으로 제공됩니다.

 

OpenGL ES 2.0과 매우 유사한 Direct3D의 컴파일된 셰이더는 그리기 컨텍스트에 연결되어야 합니다. 그러나 Direct3D에는 셰이더 프로그램 개체의 개념 자체가 없습니다. 대신 셰이더를 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)에 직접 할당해야 합니다. 이 단계는 셰이더 개체 만들기 및 바인딩에 대한 OpenGL ES 2.0 프로세스를 따르고 Direct3D에서 해당 API 동작을 제공합니다.

<a name="instructions"></a>지침
------------

### <a name="step-1-compile-the-shaders"></a>1단계: 셰이더 컴파일

이 간단한 OpenGL ES 2.0 예제에서 셰이더를 텍스트 파일로 저장하고 런타임 컴파일을 위해 문자열 데이터로 로드합니다.

OpenGL ES 2.0: 셰이더 컴파일

``` syntax
GLuint __cdecl CompileShader (GLenum shaderType, const char *shaderSrcStr)
// shaderType can be GL_VERTEX_SHADER or GL_FRAGMENT_SHADER. Returns 0 if compilation fails.
{
  GLuint shaderHandle;
  GLint compiledShaderHandle;
   
  // Create an empty shader object.
  shaderHandle = glCreateShader(shaderType);

  if (shaderHandle == 0)
  return 0;

  // Load the GLSL shader source as a string value. You could obtain it from
  // from reading a text file or hardcoded.
  glShaderSource(shaderHandle, 1, &shaderSrcStr, NULL);
   
  // Compile the shader.
  glCompileShader(shaderHandle);

  // Check the compile status
  glGetShaderiv(shaderHandle, GL_COMPILE_STATUS, &compiledShaderHandle);

  if (!compiledShaderHandle) // error in compilation occurred
  {
    // Handle any errors here.
              
    glDeleteShader(shaderHandle);
    return 0;
  }

  return shaderHandle;

}
```

Direct3D에서 셰이더는 런타임 중에 컴파일되지 않습니다. 셰이더는 나머지 프로그램을 컴파일할 때 항상 CSO 파일로 컴파일됩니다. Microsoft Visual Studio에서 앱을 컴파일하면 HLSL 파일은 앱이 로드해야 하는 CSO(.cso) 파일로 컴파일됩니다. 이러한 앱을 패키지로 만들 때 앱과 함께 CSO 파일을 포함해야 합니다!

> **참고**  다음 예제에서는 셰이더 로드 및 컴파일을 비동기적으로 **자동** 키워드와 람다 구문을 사용 하 여 수행 합니다. ReadDataAsync()는 바이트 데이터 배열(fileData)로 CSO 파일에서 읽는 템플릿에 구현된 메서드입니다.

 

Direct3D 11: 셰이더 컴파일

``` syntax
auto loadVSTask = DX::ReadDataAsync(m_projectDir + "SimpleVertexShader.cso");
auto loadPSTask = DX::ReadDataAsync(m_projectDir + "SimplePixelShader.cso");

auto createVSTask = loadVSTask.then([this](Platform::Array<byte>^ fileData) {

m_d3dDevice->CreateVertexShader(
  fileData->Data,
  fileData->Length,
  nullptr,
  &m_vertexShader);

auto createPSTask = loadPSTask.then([this](Platform::Array<byte>^ fileData) {
  m_d3dDevice->CreatePixelShader(
    fileData->Data,
    fileData->Length,
    nullptr,
    &m_pixelShader;
};
```

### <a name="step-2-create-and-load-the-vertex-and-fragment-pixel-shaders"></a>2단계: 꼭짓점 및 조각 (픽셀)셰이더 만들기 및 로드

OpenGL ES 2.0에는 GPU에서 실행되는 셰이더와 CPU에서 실행되는 주 프로그램 간 인터페이스 역할을 하는 셰이더 "프로그램"이라는 개념이 있습니다. 셰이더는 컴파일(또는 컴파일된 소스에서 로드)되고 GPU에서 실행할 수 있는 프로그램과 연결됩니다.

OpenGL ES 2.0: 셰이더 프로그램에 꼭짓점 및 조각 셰이더 로드

``` syntax
GLuint __cdecl LoadShaderProgram (const char *vertShaderSrcStr, const char *fragShaderSrcStr)
{
  GLuint programObject, vertexShaderHandle, fragmentShaderHandle;
  GLint linkStatusCode;

  // Load the vertex shader and compile it to an internal executable format.
  vertexShaderHandle = CompileShader(GL_VERTEX_SHADER, vertShaderSrcStr);
  if (vertexShaderHandle == 0)
  {
    glDeleteShader(vertexShaderHandle);
    return 0;
  }

   // Load the fragment/pixel shader and compile it to an internal executable format.
  fragmentShaderHandle = CompileShader(GL_FRAGMENT_SHADER, fragShaderSrcStr);
  if (fragmentShaderHandle == 0)
  {
    glDeleteShader(fragmentShaderHandle);
    return 0;
  }

  // Create the program object proper.
  programObject = glCreateProgram();
   
  if (programObject == 0)    return 0;

  // Attach the compiled shaders
  glAttachShader(programObject, vertexShaderHandle);
  glAttachShader(programObject, fragmentShaderHandle);

  // Compile the shaders into binary executables in memory and link them to the program object..
  glLinkProgram(programObject);

  // Check the project object link status and determine if the program is available.
  glGetProgramiv(programObject, GL_LINK_STATUS, &linkStatusCode);

  if (!linkStatusCode) // if link status <> 0
  {
    // Linking failed; delete the program object and return a failure code (0).

    glDeleteProgram (programObject);
    return 0;
  }

  // Deallocate the unused shader resources. The actual executables are part of the program object.
  glDeleteShader(vertexShaderHandle);
  glDeleteShader(fragmentShaderHandle);

  return programObject;
}

// ...

glUseProgram(renderer->programObject);
```

Direct3D에는 셰이더 프로그램 개체의 개념이 없습니다. 대신 [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) 인터페이스(예:[**ID3D11Device::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) 또는 [**ID3D11Device::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513))에서 셰이더 만들기 메서드 중 하나를 호출하면 셰이더가 만들어집니다. 현재 그리기 텍스트에 대한 셰이더를 설정하기 위해 꼭짓점 셰이더의 [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) 또는 조각 셰이더의 [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) 등 집합 셰이더 메서드와 함께 해당 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)에 이러한 셰이더를 제공합니다.

Direct3D 11: 그래픽 디바이스 그리기 컨텍스트에 대한 셰이더를 설정합니다.

``` syntax
m_d3dContext->VSSetShader(
  m_vertexShader.Get(),
  nullptr,
  0);

m_d3dContext->PSSetShader(
  m_pixelShader.Get(),
  nullptr,
  0);
```

### <a name="step-3-define-the-data-to-supply-to-the-shaders"></a>3단계: 셰이더에 제공할 데이터 정의

OpenGL ES 2.0 예제에는 셰이더 파이프라인에 대해 선언하기 위한 하나의 **uniform**이 있습니다.

-   **u\_mvpMatrix**: 큐브에 대한 모델 좌표를 가져오고 스캔 변환을 위한 2D 투영 좌표로 이 좌표를 변환하는 최종 model-view-projection 변환 행렬을 나타내는 float의 4x4 배열

그리고 **attribute** 꼭짓점 데이터에 대한 두 가지 값:

-   **a\_position**: 꼭짓점의 모델 좌표에 대한 소수점 4-float 벡터
-   **a\_color**: 꼭짓점과 연결된 RGBA 색상 값에 대한 4-float 벡터

Open GL ES 2.0: 유니폼 및 특성에 대한 GLSL 정의

``` syntax
uniform mat4 u_mvpMatrix;
attribute vec4 a_position;
attribute vec4 a_color;
```

이 경우에 해당 주 프로그램 변수는 렌더러 개체에서 필드로 정의됩니다. [방법: 간단한 OpenGL ES 2.0 렌더러를 Direct3D 11로 포팅](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md) 헤더를 참조하세요. 완료되면 주 프로그램이 셰이더 파이프라인에 대한 해당 값을 제공할 메모리의 위치를 지정해야 합니다. 이 작업은 일반적으로 그리기 호출 직전에 수행합니다.

OpenGL ES 2.0: 유니폼 및 특성 데이터의 위치 표시

``` syntax

// Inform the shader of the attribute locations
loc = glGetAttribLocation(renderer->programObject, "a_position");
glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
    sizeof(Vertex), 0);
glEnableVertexAttribArray(loc);

loc = glGetAttribLocation(renderer->programObject, "a_color");
glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
    sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
glEnableVertexAttribArray(loc);


// Inform the shader program of the uniform location
renderer->mvpLoc = glGetUniformLocation(renderer->programObject, "u_mvpMatrix");
```

Direct3D에는 같은 의미의 "특성" 또는 "유니폼"의 개념이 없습니다(또는 적어도 이 구문을 공유하지 않음). 대신, Direct3D 하위 리소스(주 프로그램 및 셰이더 프로그램 간에 공유되는 리소스)로 나타내는 상수 버퍼가 있습니다. 꼭짓점 위치 및 색상 등 이러한 하위 리소스 중 일부는 HLSL 의미 체계로 설명합니다. OpenGL ES 2.0 개념과 관련이 있으므로 상수 버퍼 및 HLSL 의미 체계에 자세한 내용은 [프레임 버퍼 개체, 유니폼 및 특성 포팅](porting-uniforms-and-attributes.md)을 참조하세요.

Direct3D로 이 프로세스를 이동할 때 해당 유니폼을 Direct3D 상수 버퍼(cbuffer)로 변환하고 **register** HLSL 의미 체계에서 조회를 위해 이 버퍼에 레지스터를 할당합니다. 두 꼭짓점 특성은 셰이더 파이프라인 단계에 대한 입력 요소로 처리되고, 또한 셰이더에 알려주는 할당된 [HLSL 의미 체계](https://msdn.microsoft.com/library/windows/desktop/bb205574)(POSITION 및 COLOR0)입니다. 픽셀 셰이더는 GPU에서 생성하는 시스템 값임을 나타내는 SV\_ 접두사가 있는 SV_POSITION을 가져옵니다. 이 경우 스캔 변환 중에 생성된 픽셀 위치입니다.VertexShaderInput은 꼭짓점 버퍼를 정의하는 데 사용되고 PixelShaderInput에 대한 데이터는 파이프라인에서 이전 단계의 결과로 생성되기 때문에(이 경우 꼭짓점 셰이더) VertexShaderInput 및 PixelShaderInput은 상수 버퍼로 선언되지 않습니다([꼭짓점 버퍼 및 데이터 포팅](port-the-vertex-buffers-and-data-config.md) 참조).

Direct3D: 상수 버퍼 및 꼭짓점 데이터에 대한 HLSL 정의

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};

// Per-vertex data used as input to the vertex shader.
struct VertexShaderInput
{
  float4 pos : POSITION;
  float4 color : COLOR0;
};

// Per-vertex color data passed through the pixel shader.
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR0;
};
```

HLSL 의미 체계의 상수 버퍼 및 응용 프로그램으로 포팅에 대한 자세한 내용은 [프레임 버퍼 개체, 유니폼 및 특성 포팅](porting-uniforms-and-attributes.md)을 참조하세요.

다음은 상수 또는 꼭짓점 버퍼가 있는 셰이더 파이프라인으로 전달되는 데이터의 레이아웃에 대한 구조입니다.

Direct3D 11: 상수 및 꼭짓점 버퍼 레이아웃 선언

``` syntax
// Constant buffer used to send MVP matrices to the vertex shader.
struct ModelViewProjectionConstantBuffer
{
  DirectX::XMFLOAT4X4 modelViewProjection;
};

// Used to send per-vertex data to the vertex shader.
struct VertexPositionColor
{
  DirectX::XMFLOAT4 pos;
  DirectX::XMFLOAT4 color;
};
```

상수 버퍼 요소에 대한 DirectXMath XM\* 유형을 사용합니다. 이 유형은 셰이더 파이프라인에 전송될 때 콘텐츠에 대한 적절한 압축 및 맞춤을 제공하기 때문입니다. 표준 Windows 플랫폼 float 형식 및 배열을 사용하는 경우 압축 및 맞춤은 직접 수행해야 합니다.

상수 버퍼를 바인딩하기 위해 레이아웃 설명을 [**CD3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/jj151620) 구조로 만들고 이 구조를 [**ID3DDevice::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)에 전달합니다. 그런 다음 해당 렌더링 메서드에서 그리기 전에 상수 버퍼를 [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486)에 전달합니다.

Direct3D 11: 상수 버퍼 바인딩

``` syntax
CD3D11_BUFFER_DESC constantBufferDesc(sizeof(ModelViewProjectionConstantBuffer), D3D11_BIND_CONSTANT_BUFFER);

m_d3dDevice->CreateBuffer(
  &constantBufferDesc,
  nullptr,
  &m_constantBuffer);

// ...

// Only update shader resources that have changed since the last frame.
m_d3dContext->UpdateSubresource(
  m_constantBuffer.Get(),
  0,
  NULL,
  &m_constantBufferData,
  0,
  0);
```

꼭짓점 버퍼가 유사하게 생성되고 업데이트되며, 다음 단계 [꼭짓점 버퍼 및 데이터 포팅](port-the-vertex-buffers-and-data-config.md)에 설명되어 있습니다.

<a name="next-step"></a>다음 단계
---------

[꼭짓점 버퍼 및 데이터 포팅](port-the-vertex-buffers-and-data-config.md)
## <a name="related-topics"></a>관련 항목


[방법: 간단한 OpenGL ES 2.0 렌더러를 Direct3D 11로 포팅](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[꼭짓점 버퍼 및 데이터 포팅](port-the-vertex-buffers-and-data-config.md)

[GLSL 포팅](port-the-glsl.md)

[화면에 그리기](draw-to-the-screen.md)

 

 




