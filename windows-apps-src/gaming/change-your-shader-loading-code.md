---
author: mtoepke
title: "OpenGL ES 2.0 셰이더 파이프라인과 Direct3D 비교"
description: "개념적으로 Direct3D 11 셰이더 파이프라인은 OpenGL ES 2.0의 셰이더 파이프라인과 매우 유사합니다."
ms.assetid: 3678a264-e3f9-72d2-be91-f79cd6f7c4ca
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: bc13df5e7f2648897be31b5cda634d23ffae8b6b

---

# OpenGL ES 2.0 셰이더 파이프라인과 Direct3D 비교


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [입력 어셈블러 단계](https://msdn.microsoft.com/library/windows/desktop/bb205116)
-   [꼭짓점 셰이더 단계](https://msdn.microsoft.com/library/windows/desktop/bb205146#Vertex_Shader_Stage)
-   [픽셀 셰이더 단계](https://msdn.microsoft.com/library/windows/desktop/bb205146#Pixel_Shader_Stage)

개념적으로 Direct3D 11 셰이더 파이프라인은 OpenGL ES 2.0의 셰이더 파이프라인과 매우 유사합니다. 그러나 API 디자인의 관점에서 셰이더 단계를 만들고 관리하기 위한 주요 구성 요소는 두 가지 주 인터페이스 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) 및 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)의 일부입니다. 이 항목에서는 일반적인 OpenGL ES 2.0 셰이더 파이프라인 API 패턴을 이러한 인터페이스의 Direct3D 11 셰이더 파이프라인 API 패턴에 매핑하려고 합니다.

## Direct3D 11 셰이더 파이프라인 검토


셰이더 개체는 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) 인터페이스에 대한 메서드(예: [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) 및 [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513))로 만듭니다.

Direct3D 11 그래픽 파이프라인은 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 인터페이스의 인스턴스에 의해 관리되며 다음 단계를 포함합니다.

-   [입력 어셈블러 단계](https://msdn.microsoft.com/library/windows/desktop/bb205116) - 입력 어셈블러 단계는 파이프라인에 데이터(삼각형, 선 및 점)를 제공합니다. 이 단계를 지원하는 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 메서드에는 접두사 "IA"가 붙습니다.
-   [꼭짓점 셰이더 단계](https://msdn.microsoft.com/library/windows/desktop/bb205146#Vertex_Shader_Stage) - 꼭짓점 셰이더 단계에서는 꼭짓점을 처리하며, 일반적으로 변환, 스킨 지정 및 조명과 같은 작업을 수행합니다. 꼭짓점 셰이더는 항상 단일 입력 꼭짓점을 사용하여 단일 출력 꼭짓점을 생성합니다. 이 단계를 지원하는 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 메서드에는 접두사 "VS"가 붙습니다.
-   [스트림 출력 단계](https://msdn.microsoft.com/library/windows/desktop/bb205121) - 스트림 출력 단계에서는 파이프라인에서 메모리를 거쳐 래스터라이저로 기본 요소 데이터를 스트림합니다. 데이터를 스트림 출력하거나 래스터라이저로 전달할 수 있습니다. 메모리로 스트림 출력된 데이터는 입력 데이터 또는 CPU에서 다시 읽기로 파이프라인으로 다시 재순환될 수 있습니다. 이 단계를 지원하는 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 메서드에는 접두사 "SO"가 붙습니다.
-   [래스터라이저 단계](https://msdn.microsoft.com/library/windows/desktop/bb205125) - 래스터라이저는 기본 요소를 잘라내고, 픽셀 셰이더에 대해 기본 요소를 준비하고, 픽셀 셰이더를 호출하는 방법을 결정합니다. 파이프라인에 픽셀 셰이더가 없음을 알리고([**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472)를 사용하여 픽셀 셰이더 단계를 NULL로 설정), 깊이 및 스텐실 테스트를 사용하지 않도록 설정하여([**D3D11\_DEPTH\_STENCIL\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476110)에서 DepthEnable 및 StencilEnable을 FALSE로 설정) 래스터화를 사용하지 않도록 설정할 수 있습니다. 사용하지 않도록 설정되어 있는 동안 래스터화 관련 파이프라인 카운터는 업데이트되지 않습니다.
-   [픽셀 셰이더 단계](https://msdn.microsoft.com/library/windows/desktop/bb205146#Pixel_Shader_Stage) - 픽셀 셰이더 단계는 기본 요소에 대한 보간된 데이터를 받아 픽셀별 데이터(예: 색)를 생성합니다. 이 단계를 지원하는 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 메서드에는 접두사 "PS"가 붙습니다.
-   [출력 병합기 단계](https://msdn.microsoft.com/library/windows/desktop/bb205120) - 출력 병합기 단계는 다양한 유형의 출력 데이터(픽셀 셰이더 값, 깊이 및 스텐실 정보)를 렌더링 대상 및 깊이/스텐실 버퍼의 내용과 결합하여 최종 파이프라인 결과를 생성합니다. 이 단계를 지원하는 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 메서드에는 접두사 "OM"이 붙습니다.

(기하 도형 셰이더, 헐 셰이더, 테설레이터 및 도메인 셰이더에 대한 단계도 있지만 이러한 단계는 OpenGL ES 2.0에 아날로그가 없으므로 여기서 다루지 않습니다.) 이러한 단계에 대한 전체 메서드 목록은 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) 및 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 참조 페이지를 참조하세요. **ID3D11DeviceContext1**은 Direct3D 11용으로 **ID3D11DeviceContext**를 확장합니다.

## 셰이더 만들기


Direct3D에서는 셰이더 리소스를 컴파일 및 로드하기 전에 셰이더 리소스가 만들어지지 않습니다. 대신 이 리소스는 HLSL이 로드될 때 만들어집니다. 따라서 glCreateShader와 직접적으로 유사한 함수는 없습니다. glCreateShader는 특정 형식의 초기화된 셰이더 리소스를 만듭니다(예: GL\_VERTEX\_SHADER or GL\_FRAGMENT\_SHADER). 대신 셰이더는 [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) 및 [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)와 같은 특정 함수를 통해 HLSL이 로드된 후 만들어지며, 이러한 함수는 형식과 컴파일된 HLSL을 매개 변수로 사용합니다.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                                             |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCreateShader | 컴파일된 셰이더 개체를 로드하여 CSO로 버퍼로 전달한 후 [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) 및 [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)를 호출합니다. |

 

## 셰이더 컴파일


Direct3D 셰이더는 UWP(유니버설 Windows 플랫폼) 앱에서 컴파일된 셰이더 개체(.cso) 파일로 미리 컴파일되고 Windows 런타임 파일 API 중 하나를 사용하여 로드되어야 합니다. (데스크톱 앱은 런타임에 텍스트 파일이나 문자열에서 셰이더를 컴파일할 수 있습니다). CSO 파일은 Microsoft Visual Studio 프로젝트의 일부인 .hlsl 파일에서 작성되며 .cso 파일 확장명만 포함하면서 동일한 이름을 유지합니다. 배포 시 CSO 파일이 패키지에 포함되어 있는지 확인하세요!

| OpenGL ES 2.0                          | Direct3D 11                                                                                                                                                                   |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCompileShader                        | 해당 없음. Visual Studio에서 셰이더를 .cso 파일로 컴파일하여 패키지에 포함시킵니다.                                                                                     |
| 컴파일 상태에 대해 glGetShaderiv 사용 | 해당 없음. 컴파일에 오류가 있는 경우 Visual Studio FXC(FX 컴파일러)에서 컴파일 출력을 참조하세요. 컴파일이 성공적이면 해당 CSO 파일이 만들어집니다. |

 

## 셰이더 로드


셰이더 만들기 섹션에 나와 있는 것처럼, Direct3D 11은 해당 CSO 파일이 버퍼에 로드되어 다음 표의 메서드 중 하나로 전달되면 셰이더를 만듭니다.

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                                                                                                           |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ShaderSource  | 컴파일된 셰이더 개체를 로드한 후 [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) 및 [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)를 호출합니다. |

 

## 파이프라인 설정


OpenGL ES 2.0에는 실행을 위한 여러 셰이더가 포함되어 있는 "셰이더 프로그램" 개체가 있습니다. 개별 셰이더는 셰이더 프로그램 개체에 연결됩니다. 그러나 Direct3D 11에서는 렌더링 컨텍스트([**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598))와 직접 작업하여 해당 컨텍스트에 대한 셰이더를 만듭니다.

| OpenGL ES 2.0   | Direct3D 11                                                                                   |
|-----------------|-----------------------------------------------------------------------------------------------|
| glCreateProgram | 해당 없음. Direct3D 11은 셰이더 프로그램 개체 추상화를 사용하지 않습니다.                          |
| glLinkProgram   | 해당 없음. Direct3D 11은 셰이더 프로그램 개체 추상화를 사용하지 않습니다.                          |
| glUseProgram    | 해당 없음. Direct3D 11은 셰이더 프로그램 개체 추상화를 사용하지 않습니다.                          |
| glGetProgramiv  | [
            **ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)에 대해 만든 참조를 사용합니다. |

 

정적 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 메서드를 사용하여 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 및 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/dn280493)의 인스턴스를 만듭니다.

``` syntax
Microsoft::WRL::ComPtr<ID3D11Device1>          m_d3dDevice;
Microsoft::WRL::ComPtr<ID3D11DeviceContext1>  m_d3dContext;

// ...

D3D11CreateDevice(
  nullptr, // Specify nullptr to use the default adapter.
  D3D_DRIVER_TYPE_HARDWARE,
  nullptr,
  creationFlags, // Set set debug and Direct2D compatibility flags.
  featureLevels, // List of feature levels this app can support.
  ARRAYSIZE(featureLevels),
  D3D11_SDK_VERSION, // Always set this to D3D11_SDK_VERSION for Windows Store apps.
  &device, // Returns the Direct3D device created.
  &m_featureLevel, // Returns feature level of device created.
  &m_d3dContext // Returns the device's immediate context.
);
```

## 뷰포트 설정


Direct3D 11에서 뷰포트를 설정하는 작업은 OpenGL ES 2.0에서 뷰포트를 설정하는 방법과 매우 유사합니다. Direct3D 11에서 구성된 [**CD3D11\_VIEWPORT**](https://msdn.microsoft.com/library/windows/desktop/jj151722)를 사용하여 [**ID3D11DeviceContext::RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480)를 호출합니다.

Direct3D 11: 뷰포트를 설정합니다.

``` syntax
CD3D11_VIEWPORT viewport(
        0.0f,
        0.0f,
        m_d3dRenderTargetSize.Width,
        m_d3dRenderTargetSize.Height
        );
m_d3dContext->RSSetViewports(1, &viewport);
```

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| glViewport    | [
            **CD3D11\_VIEWPORT**](https://msdn.microsoft.com/library/windows/desktop/jj151722), [**ID3D11DeviceContext::RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480) |

 

## 꼭짓점 셰이더 구성


셰이더가 로드되면 Direct3D 11에서 꼭짓점 셰이더 구성이 완료됩니다. uniform은 [**ID3D11DeviceContext1::VSSetConstantBuffers1**](https://msdn.microsoft.com/library/windows/desktop/hh446795)을 통해 상수 버퍼로 전달됩니다.

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524)                       |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1::VSGetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476489)                       |
| glGetUniformfv, glGetUniformiv   | [
            **ID3D11DeviceContext1::VSGetConstantBuffers1**](https://msdn.microsoft.com/library/windows/desktop/hh446793). |

 

## 픽셀 셰이더 구성


셰이더가 로드되면 Direct3D 11에서 픽셀 셰이더 구성이 완료됩니다. uniform은 [**ID3D11DeviceContext1::PSSetConstantBuffers1.**](https://msdn.microsoft.com/library/windows/desktop/hh404649)을 통해 상수 버퍼로 전달됩니다.

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)                         |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1::PSGetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476468)                       |
| glGetUniformfv, glGetUniformiv   | [
            **ID3D11DeviceContext1::PSGetConstantBuffers1**](https://msdn.microsoft.com/library/windows/desktop/hh404645). |

 

## 최종 결과 생성


파이프라인이 완료되면 셰이더 단계의 결과를 백 버퍼에 그립니다. Direct3D 11에서는 Open GL ES 2.0과 마찬가지로 그리기 명령을 호출하여 결과를 백 버퍼에 색상 맵으로 출력한 다음 해당 백 버퍼를 다시 디스플레이로 보내는 작업이 여기에 포함됩니다.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                         |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glDrawElements | [
            **ID3D11DeviceContext1::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407), [**ID3D11DeviceContext1::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409)(또는 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/ff476385)에 대한 다른 Draw\* 메서드) |
| eglSwapBuffers | [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797)                                                                                                                                                                              |

 

## GLSL을 HLSL로 포팅


GLSL과 HLSL은 복합 형식 지원 및 구문(일부 전체 구문)을 제외하고는 그렇게 다르지 않습니다. 많은 개발자는 일반적인 OpenGL ES 2.0 명령 및 정의를 HLSL의 명령 및 정의로 별칭을 지정하여 둘 사이를 포팅하는 것이 가장 쉽다는 사실을 알고 있습니다. Direct3D에서는 셰이더 모델 버전을 사용하여 그래픽 인터페이스에서 지원하는 HLSL의 기능 집합을 표현합니다. OpenGL에는 HLSL에 대한 다양한 버전 사양이 포함되어 있습니다. 다음 표는 다른 버전의 관점에서 Direct3D 11 및 OpenGL ES 2.0에 대해 정의된 셰이더 언어 기능 집합의 대략적인 아이디어를 제공합니다.

| 셰이더 언어           | GLSL 기능 버전                                                                                                                                                                                                      | Direct3D 셰이더 모델 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Direct3D 11 HLSL          | ~4.30.                                                                                                                                                                                                                    | SM 5.0                |
| OpenGL ES 2.0용 GLSL ES | 1.40. OpenGL ES 2.0용 GLSL ES의 이전 구현에서는 1.10~1.30을 사용할 수도 있습니다. 이러한 사실을 확인하려면 glGetString(GL\_SHADING\_LANGUAGE\_VERSION) 또는 glGetString(SHADING\_LANGUAGE\_VERSION)으로 원래 코드를 검사합니다. | ~SM 2.0               |

 

두 셰이더 언어 간 차이점 및 일반적인 구문 매핑에 대한 자세한 내용은 [GLSL-HLSL 참조](glsl-to-hlsl-reference.md)를 읽어 보세요.

## OpenGL 내부 기능을 HLSL 의미 체계로 포팅


Direct3D 11 HLSL 의미 체계는 uniform 또는 특성 이름과 같은 문자열로서, 앱과 셰이더 프로그램 간에 전달되는 값을 식별하는 데 사용됩니다. 이러한 의미 체계는 가능한 다양한 문자열 중 어느 것도 될 수 있지만, 모범 사례는 용도를 나타내는 POSITION 또는 COLOR와 같은 문자열을 사용하는 것입니다. 상수 버퍼 또는 버퍼 입력 레이아웃을 만들 때 이러한 의미 체계를 할당합니다. 또한 유사한 값에 대해 별도의 레지스터를 사용하도록 0과 7 사이의 숫자를 의미 체계에 추가할 수 있습니다. 예를 들어 COLOR0, COLOR1, COLOR2...와 같이 합니다.

접두사 "SV\_"를 붙인 의미 체계는 셰이더 프로그램에 의해 작성된 시스템 값 의미 체계입니다. 앱 자체(CPU에서 실행)에서는 이러한 의미 체계를 수정할 수 없습니다. 일반적으로 여기에는 그래픽 파이프라인의 다른 셰이더 단계의 입력 또는 출력인 값이나, 전적으로 GPU에 의해 생성된 값이 포함됩니다.

또한 SV\_ 의미 체계가 셰이더 단계의 입력 또는 출력을 지정하는 데 사용되는 경우 다른 동작을 포함합니다. 예를 들어 SV\_POSITION(출력)에는 꼭짓점 셰이더 단계 동안 변환된 꼭짓점 데이터가 포함되고, SV\_POSITION(input)에는 래스터화 동안 보간된 픽셀 위치 값이 포함됩니다.

다음은 일반적인 OpenGL ES 2.0 셰이더 내부 기능에 대한 몇 가지 매핑입니다.

| OpenGL 시스템 값 | 이 HLSL 의미 체계 사용                                                                                                                                                   |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| gl\_Position        | 꼭짓점 버퍼 데이터의 POSITION(n). SV\_POSITION은 픽셀 셰이더에 픽셀 위치를 제공하고 앱에 의해 작성될 수 없습니다.                                        |
| gl\_Normal          | 꼭짓점 버퍼에서 제공하는 일반 데이터의 NORMAL(n)                                                                                                                 |
| gl\_TexCoord\[n\]   | 셰이더에 제공되는 텍스처 UV(일부 OpenGL 설명서에서는 ST) 좌표 데이터의 TEXCOORD(n)                                                                       |
| gl\_FragColor       | 셰이더에 제공되는 RGBA 색 데이터의 COLOR(n). 좌표 데이터에 대해 동일하게 처리됩니다. 의미 체계는 단순히 해당 데이터가 색 데이터임을 식별할 수 있도록 도와줄 뿐입니다. |
| gl\_FragData\[n\]   | 픽셀 셰이더에서 대상 텍스처 또는 다른 픽셀 버퍼로 쓰기 위한 SV\_Target\[n\]입니다.                                                                               |

 

의미 체계에 대해 코딩하는 메서드는 OpenGL ES 2.0에서 내부 기능을 사용하는 것과 다릅니다. OpenGL에서는 구성이나 선언 없이 직접 많은 내부 기능에 액세스할 수 있습니다. Direct3D에서는 특정 의미 체계를 사용하기 위해 특정 상수 버퍼에서 필드를 선언해야 하거나, 셰이더의 **main()** 메서드에 대한 반환 값으로 필드를 선언합니다.

다음은 상수 버퍼 정의에서 사용되는 의미 체계의 예입니다.

```cpp
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR0;
};

// The position is interpolated to the pixel value by the system. The per-vertex color data is also interpolated and passed through the pixel shader. 
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR0;
};
```

이 코드에서는 한 쌍의 단순한 상수 버퍼를 정의합니다.

또한 다음은 조각 셰이더에서 반환하는 값을 정의하는 데 사용되는 의미 체계의 예입니다.

```cpp
// A pass-through for the (interpolated) color data.
float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color,1.0f);
}
```

이 경우 SV\_TARGET은 셰이더가 실행을 완료할 때 픽셀 색(4개의 float 값이 포함된 하나의 벡터로 정의됨)이 작성되는 렌더링 대상의 위치입니다.

Direct3D에서 의미 체계를 사용하는 방법에 대한 자세한 내용은 [HLSL 의미 체계](https://msdn.microsoft.com/library/windows/desktop/bb509647)를 읽어 보세요.

 

 







<!--HONumber=Jun16_HO4-->


