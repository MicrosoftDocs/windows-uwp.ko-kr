---
title: 셰이더 개체 이식
description: OpenGL ES 2.0에서 단순 렌더러를 이식할 때 첫 번째 단계는 Direct3D 11에서 해당 하는 꼭 짓 점 및 조각 셰이더 개체를 설정 하 고, 컴파일된 후 주 프로그램이 셰이더 개체와 통신할 수 있는지 확인 하는 것입니다.
ms.assetid: 0383b774-bc1b-910e-8eb6-cc969b3dcc08
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 포트, 셰이더, direct3d, opengl
ms.localizationpriority: medium
ms.openlocfilehash: a5405b49bddb31752c42ace82d89b128a71d78a2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171997"
---
# <a name="port-the-shader-objects"></a>셰이더 개체 이식




**중요 API**

-   [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device)
-   [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)

OpenGL ES 2.0에서 단순 렌더러를 이식할 때 첫 번째 단계는 Direct3D 11에서 해당 하는 꼭 짓 점 및 조각 셰이더 개체를 설정 하 고, 컴파일된 후 주 프로그램이 셰이더 개체와 통신할 수 있는지 확인 하는 것입니다.

> **참고**    새 Direct3D 프로젝트를 만들었는지 확인 합니다. 그렇지 않은 경우에 [는 유니버설 Windows 플랫폼에 대 한 새 DirectX 11 프로젝트 만들기 (UWP)](user-interface.md)의 지침을 따르세요. 이 연습에서는 사용자가 화면에 그리기 위한 DXGI 및 Direct3D 리소스를 만들었으며 템플릿에서 제공 하는 것으로 가정 합니다.

 

OpenGL ES 2.0와 마찬가지로 Direct3D의 컴파일된 셰이더는 그리기 컨텍스트와 연결 되어야 합니다. 그러나 Direct3D에는 se 당 셰이더 프로그램 개체의 개념이 없습니다. 대신 [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)에 셰이더를 직접 할당 해야 합니다. 이 단계는 셰이더 개체 만들기 및 바인딩에 대한 OpenGL ES 2.0 프로세스를 따르고 Direct3D에서 해당 API 동작을 제공합니다.

<a name="instructions"></a>Instructions
------------

### <a name="step-1-compile-the-shaders"></a>1 단계: 셰이더 컴파일

이 간단한 OpenGL ES 2.0 샘플에서 셰이더는 텍스트 파일로 저장 되 고 런타임 컴파일의 문자열 데이터로 로드 됩니다.

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

Direct3D에서 셰이더는 런타임 중에 컴파일되지 않습니다. 프로그램의 나머지 부분이 컴파일될 때 항상 CCEFILES로 컴파일됩니다. Microsoft Visual Studio를 사용 하 여 앱을 컴파일하면 HLSL 파일이 응용 프로그램에서 로드 해야 하는 CSO (.cs) 파일로 컴파일됩니다. 패키지를 만들 때 앱에 이러한 CSO 파일을 포함 해야 합니다!

> **참고**    다음 예제에서는 **auto** 키워드 및 람다 구문을 사용 하 여 셰이더 로드 및 컴파일을 비동기적으로 수행 합니다. ReadDataAsync ()는 CSO 파일에서 바이트 데이터의 배열 (fileData)로 읽는 템플릿에 대해 구현 되는 메서드입니다.

 

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

### <a name="step-2-create-and-load-the-vertex-and-fragment-pixel-shaders"></a>2 단계: 꼭 짓 점 및 조각 (픽셀) 셰이더 만들기 및 로드

OpenGL ES 2.0에는 CPU에서 실행 되는 기본 프로그램과 GPU에서 실행 되는 셰이더 사이에서 인터페이스 역할을 하는 셰이더 "program"의 개념이 있습니다. 셰이더는 컴파일되어 컴파일된 소스에서 로드 되며, GPU에서 실행할 수 있도록 하는 프로그램과 연결 됩니다.

OpenGL ES 2.0: 꼭 짓 점 및 조각 셰이더를 음영 프로그램으로 로드

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

Direct3D에는 셰이더 프로그램 개체의 개념이 없습니다. 대신 [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) 인터페이스의 셰이더 생성 메서드 (예: [**ID3D11Device:: CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 또는 [**ID3D11Device:: CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)) 중 하나가 호출 될 때 셰이더가 생성 됩니다. 현재 그리기 컨텍스트에 대 한 셰이더를 설정 하기 위해 버텍스 셰이더에 대 한 [**ID3D11DeviceContext:: VSSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) 또는 조각 셰이더에 대 한 [**ID3D11DeviceContext::P ssetshader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) 와 같은 set 셰이더 메서드를 사용 하 여 해당 [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) 을 제공 합니다.

Direct3D 11: 그래픽 장치 그리기 컨텍스트에 대 한 셰이더를 설정 합니다.

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

### <a name="step-3-define-the-data-to-supply-to-the-shaders"></a>3 단계: 셰이더를 제공 하는 데이터 정의

Microsoft의 OpenGL ES 2.0 예제에서는 셰이더 파이프라인을 위해 선언 하는 것이 **균일** 합니다.

-   **u \_ mvpMatrix**: 큐브의 모델 좌표를 사용 하 고이를 스캔 변환에 대 한 2d 프로젝션 좌표로 변환 하는 최종 모델-뷰-프로젝션 변환 매트릭스를 나타내는 부동 소수점 배열입니다.

및 꼭 짓 점 데이터에 대 한 두 가지 **특성** 값:

-   ** \_ 위치**: 꼭 짓 점의 모델 좌표에 대 한 4 부동 벡터입니다.
-   ** \_ color**: 꼭 짓 점에 연결 된 RGBA 색 값에 대 한 4 부동 벡터입니다.

Open GL ES 2.0: 유니폼 및 특성에 대한 GLSL 정의

``` syntax
uniform mat4 u_mvpMatrix;
attribute vec4 a_position;
attribute vec4 a_color;
```

해당 하는 주 프로그램 변수는 렌더러 개체의 필드로 정의 됩니다 (이 경우). [방법: 간단한 OPENGL ES 2.0 렌더러를 Direct3D 11로 이식](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)의 헤더를 참조 하세요. 이 작업을 완료 한 후에는 주 프로그램이 셰이더 파이프라인에 대해 이러한 값을 제공 하는 메모리 내의 위치를 지정 해야 합니다. 이러한 값은 일반적으로 그리기 호출 바로 앞에서 수행 합니다.

OpenGL ES 2.0: uniform 및 attribute 데이터의 위치 표시

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

Direct3D에는 동일한 의미의 "특성" 또는 "균일" 개념이 없습니다. 또는 최소한이 구문을 공유 하지 않습니다. 대신, 주 프로그램과 셰이더 프로그램 간에 공유 되는 리소스 (Direct3D 하위)로 표현 되는 상수 버퍼가 있습니다. 꼭 짓 점 위치 및 색과 같은 이러한 하위 리소스 중 일부는 HLSL 의미 체계로 설명 됩니다. 상수 버퍼 및 HLSL 의미 체계에 대 한 자세한 내용은 OpenGL ES 2.0 개념, [포트 프레임 버퍼 개체, 다양 한 형식 및 특성](porting-uniforms-and-attributes.md)을 참조 하세요.

이 프로세스를 Direct3D로 이동 하는 경우 uniform을 Direct3D 상수 버퍼 (cbuffer)로 변환 하 고 **register** HLSL 의미 체계를 사용 하 여 lookup에 대 한 레지스터를 할당 합니다. 두 꼭 짓 점 특성은 셰이더 파이프라인 단계에 대 한 입력 요소로 처리 되며 셰이더를 알리는 [HLSL 의미 체계](/windows/desktop/direct3dhlsl/dcl-usage---ps) (위치 및 COLOR0)도 할당 됩니다. 픽셀 셰이더는 sv \_ 위치를 사용 하며,이는 \_ GPU에 의해 생성 된 시스템 값 임을 나타내는 sv 접두사가 있습니다. 이 경우 검색 변환 중에 생성 되는 픽셀 위치입니다. VertexShaderInput 및 PixelShaderInput는 선행 버텍스 버퍼를 정의 하는 데 사용 되 고 ( [꼭 짓 점 버퍼와 데이터를](port-the-vertex-buffers-and-data-config.md)참조), 후자의 데이터는 파이프라인의 이전 단계 (이 경우에는 꼭 짓 점 셰이더)의 결과로 생성 되기 때문에 상수 버퍼로 선언 되지 않습니다.

Direct3D: 상수 버퍼 및 꼭 짓 점 데이터에 대 한 HLSL 정의

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

상수 버퍼 및 HLSL 의미 체계의 응용 프로그램에 이식 하는 방법에 대 한 자세한 내용은 [포트 프레임 버퍼 개체, 다양 한 형식 및 특성](porting-uniforms-and-attributes.md)을 참조 하세요.

상수 또는 꼭 짓 점 버퍼를 사용 하 여 셰이더 파이프라인으로 전달 되는 데이터의 레이아웃 구조는 다음과 같습니다.

Direct3D 11: 상수 및 꼭 짓 점 버퍼 레이아웃 선언

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

\*상수 버퍼 요소에 대해 DirectXMath XM 형식을 사용 합니다 .이 형식은 셰이더 파이프라인으로 전송 될 때 콘텐츠에 대 한 적절 한 압축 및 맞춤 기능을 제공 하기 때문입니다. 표준 Windows 플랫폼 float 형식 및 배열을 사용 하는 경우 직접 압축 및 맞춤을 수행 해야 합니다.

상수 버퍼를 바인딩하려면 레이아웃 설명을 [**CD3D11 \_ buffer \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-cd3d11_buffer_desc) 구조체로 만들고 [**ID3DDevice:: createbuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer)에 전달 합니다. 그런 다음 render 메서드에서 상수 버퍼를 그리기 전에 [**ID3D11DeviceContext:: UpdateSubresource**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-updatesubresource) 에 전달 합니다.

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

꼭 짓 점 버퍼는 이와 유사 하 게 만들어지고 업데이트 되며, 다음 단계에서 [꼭 짓 점 버퍼와 데이터를 포트](port-the-vertex-buffers-and-data-config.md)에 설명 합니다.

<a name="next-step"></a>다음 단계
---------

[꼭 짓 점 버퍼 및 데이터를 이식 합니다.](port-the-vertex-buffers-and-data-config.md)
## <a name="related-topics"></a>관련 항목


[방법: 간단한 OpenGL ES 2.0 렌더러를 Direct3D 11로 이식](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[꼭 짓 점 버퍼 및 데이터를 이식 합니다.](port-the-vertex-buffers-and-data-config.md)

[인 포트](port-the-glsl.md)

[화면에 그리기](draw-to-the-screen.md)

 

 