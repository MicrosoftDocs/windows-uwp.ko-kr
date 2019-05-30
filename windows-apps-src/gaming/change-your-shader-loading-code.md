---
title: OpenGL ES 2.0 셰이더 파이프라인과 Direct3D 비교
description: 개념적으로 Direct3D 11 셰이더 파이프라인은 OpenGL ES 2.0의 셰이더 파이프라인과 매우 유사합니다.
ms.assetid: 3678a264-e3f9-72d2-be91-f79cd6f7c4ca
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, opengl, direct3d, 셰이더 파이프라인
ms.localizationpriority: medium
ms.openlocfilehash: 8793ef8b44df1ca1d93133383666434f525f2d07
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368971"
---
# <a name="compare-the-opengl-es-20-shader-pipeline-to-direct3d"></a>OpenGL ES 2.0 셰이더 파이프라인과 Direct3D 비교




**중요 한 Api**

-   [입력 어셈블러 단계](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage)
-   [꼭 짓 점 셰이더 단계](https://docs.microsoft.com/previous-versions//bb205146(v=vs.85))
-   [픽셀 셰이더 단계](https://docs.microsoft.com/previous-versions//bb205146(v=vs.85))

개념적으로 Direct3D 11 셰이더 파이프라인은 OpenGL ES 2.0의 셰이더 파이프라인과 매우 유사합니다. 그러나 API 디자인의 관점에서 셰이더 단계를 만들고 관리하기 위한 주요 구성 요소는 두 가지 주 인터페이스 [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 및 [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)의 일부입니다. 이 항목에서는 일반적인 OpenGL ES 2.0 셰이더 파이프라인 API 패턴을 이러한 인터페이스의 Direct3D 11 셰이더 파이프라인 API 패턴에 매핑하려고 합니다.

## <a name="reviewing-the-direct3d-11-shader-pipeline"></a>Direct3D 11 셰이더 파이프라인 검토


셰이더 개체는 [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 인터페이스에 대한 메서드(예: [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 및 [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader))로 만듭니다.

Direct3D 11 그래픽 파이프라인은 [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 인터페이스의 인스턴스에 의해 관리되며 다음 단계를 포함합니다.

-   [입력 어셈블러 단계](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage) - 입력 어셈블러 단계는 파이프라인에 데이터(삼각형, 선 및 점)를 제공합니다. [**ID3D11DeviceContext1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 이 단계를 지 원하는 메서드에 "IA" 접두사로 추가 됩니다.
-   [꼭짓점 셰이더 단계](https://docs.microsoft.com/previous-versions//bb205146(v=vs.85)) - 꼭짓점 셰이더 단계에서는 꼭짓점을 처리하며, 일반적으로 변환, 스킨 지정 및 조명과 같은 작업을 수행합니다. 꼭짓점 셰이더는 항상 단일 입력 꼭짓점을 사용하여 단일 출력 꼭짓점을 생성합니다. [**ID3D11DeviceContext1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 이 단계를 지 원하는 메서드에 "및" 접두사로 추가 됩니다.
-   [스트림 출력 단계](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-stream-stage) - 스트림 출력 단계에서는 파이프라인에서 메모리를 거쳐 래스터라이저로 기본 요소 데이터를 스트림합니다. 데이터를 스트림 출력하거나 래스터라이저로 전달할 수 있습니다. 메모리로 스트림 출력된 데이터는 입력 데이터 또는 CPU에서 다시 읽기로 파이프라인으로 다시 재순환될 수 있습니다. [**ID3D11DeviceContext1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 이 단계를 지 원하는 메서드에 "등" 접두사로 추가 됩니다.
-   [래스터라이저 단계](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage) - 래스터라이저는 기본 요소를 잘라내고, 픽셀 셰이더에 대해 기본 요소를 준비하고, 픽셀 셰이더를 호출하는 방법을 결정합니다. 래스터화를 비활성화할 수 있습니다 다음 픽셀 셰이더가 없음을 파이프라인에 게 알려 서 (픽셀 셰이더 단계를 사용 하 여 NULL로 [ **ID3D11DeviceContext::PSSetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader)), 깊이 및 스텐실 테스트 (및 해제 DepthEnable 및 StencilEnable을 FALSE로 설정 [ **D3D11\_깊이\_스텐실\_DESC**](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_depth_stencil_desc)). 사용하지 않도록 설정되어 있는 동안 래스터화 관련 파이프라인 카운터는 업데이트되지 않습니다.
-   [픽셀 셰이더 단계](https://docs.microsoft.com/previous-versions//bb205146(v=vs.85)) - 픽셀 셰이더 단계는 기본 요소에 대한 보간된 데이터를 받아 픽셀별 데이터(예: 색)를 생성합니다. [**ID3D11DeviceContext1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 이 단계를 지 원하는 메서드는 "PS" 접두사가 붙어 있습니다.
-   [출력 병합기 단계](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage) - 출력 병합기 단계는 다양한 유형의 출력 데이터(픽셀 셰이더 값, 깊이 및 스텐실 정보)를 렌더링 대상 및 깊이/스텐실 버퍼의 내용과 결합하여 최종 파이프라인 결과를 생성합니다. [**ID3D11DeviceContext1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 이 단계를 지 원하는 메서드에 "OM" 접두사로 추가 됩니다.

(이 밖에도 기 하 도형 셰이더, 헐 셰이더, tesselators, 및 도메인 셰이더 단계에 불과하지만 OpenGL ES 2.0에서 없습니다 유사한 기능을 있기 때문 않습니다 여기.) 이러한 단계에 대 한 메서드의 전체 목록은 참조 합니다 [ **ID3D11DeviceContext** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) 하 고 [ **ID3D11DeviceContext1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 참조 페이지입니다. **ID3D11DeviceContext1**은 Direct3D 11용으로 **ID3D11DeviceContext**를 확장합니다.

## <a name="creating-a-shader"></a>셰이더 만들기


Direct3D에서는 셰이더 리소스를 컴파일 및 로드하기 전에 셰이더 리소스가 만들어지지 않습니다. 대신 이 리소스는 HLSL이 로드될 때 만들어집니다. 따라서이 특정 형식의 초기화 된 셰이더 리소스를 만드는 glCreateShader 직접 유사한 함수가 (GL 같은\_꼭 짓 점\_셰이더 또는 GL\_조각\_셰이더). 대신 셰이더는 [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 및 [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)와 같은 특정 함수를 통해 HLSL이 로드된 후 만들어지며, 이러한 함수는 형식과 컴파일된 HLSL을 매개 변수로 사용합니다.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                                             |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCreateShader | 컴파일된 셰이더 개체를 로드하여 CSO로 버퍼로 전달한 후 [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 및 [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)를 호출합니다. |

 

## <a name="compiling-a-shader"></a>셰이더 컴파일


Direct3D 셰이더는 UWP(유니버설 Windows 플랫폼) 앱에서 컴파일된 셰이더 개체(.cso) 파일로 미리 컴파일되고 Windows 런타임 파일 API 중 하나를 사용하여 로드되어야 합니다. (데스크톱 앱에 런타임 시 문자열 또는 텍스트 파일에서 셰이더를 컴파일할 수 있습니다.) CSO 파일은 Microsoft Visual Studio 프로젝트의 일부 이며.cso 파일 확장명이 동일한 이름을 유지 하는 모든.hlsl 파일에서 빌드됩니다. 배포 시 CSO 파일이 패키지에 포함되어 있는지 확인하세요!

| OpenGL ES 2.0                          | Direct3D 11                                                                                                                                                                   |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCompileShader                        | 해당 없음. Visual Studio에서 셰이더를 .cso 파일로 컴파일하여 패키지에 포함시킵니다.                                                                                     |
| 컴파일 상태에 대해 glGetShaderiv 사용 | 해당 없음. 컴파일에 오류가 있는 경우 Visual Studio FXC(FX 컴파일러)에서 컴파일 출력을 참조하세요. 컴파일이 성공적이면 해당 CSO 파일이 만들어집니다. |

 

## <a name="loading-a-shader"></a>셰이더 로드


셰이더 만들기 섹션에 나와 있는 것처럼, Direct3D 11은 해당 CSO 파일이 버퍼에 로드되어 다음 표의 메서드 중 하나로 전달되면 셰이더를 만듭니다.

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                                                                                                           |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ShaderSource  | 컴파일된 셰이더 개체를 로드한 후 [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 및 [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)를 호출합니다. |

 

## <a name="setting-up-the-pipeline"></a>파이프라인 설정


OpenGL ES 2.0에는 실행을 위한 여러 셰이더가 포함되어 있는 "셰이더 프로그램" 개체가 있습니다. 개별 셰이더는 셰이더 프로그램 개체에 연결됩니다. 그러나 Direct3D 11에서는 렌더링 컨텍스트([**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1))와 직접 작업하여 해당 컨텍스트에 대한 셰이더를 만듭니다.

| OpenGL ES 2.0   | Direct3D 11                                                                                   |
|-----------------|-----------------------------------------------------------------------------------------------|
| glCreateProgram | 해당 없음. Direct3D 11은 셰이더 프로그램 개체 추상화를 사용하지 않습니다.                          |
| glLinkProgram   | 해당 없음. Direct3D 11은 셰이더 프로그램 개체 추상화를 사용하지 않습니다.                          |
| glUseProgram    | 해당 없음. Direct3D 11은 셰이더 프로그램 개체 추상화를 사용하지 않습니다.                          |
| glGetProgramiv  | [  **ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)에 대해 만든 참조를 사용합니다. |

 

정적 [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 메서드를 사용하여 [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 및 [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11device2)의 인스턴스를 만듭니다.

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
  D3D11_SDK_VERSION, // Always set this to D3D11_SDK_VERSION for UWP apps.
  &device, // Returns the Direct3D device created.
  &m_featureLevel, // Returns feature level of device created.
  &m_d3dContext // Returns the device's immediate context.
);
```

## <a name="setting-the-viewports"></a>뷰포트 설정


Direct3D 11에서 뷰포트를 설정하는 작업은 OpenGL ES 2.0에서 뷰포트를 설정하는 방법과 매우 유사합니다. Direct3D 11에서 호출할 [ **ID3D11DeviceContext::RSSetViewports** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) 으로 구성 된 [ **CD3D11\_뷰포트**](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/jj151722(v=vs.85))합니다.

Direct3D 11: 뷰포트를 설정 합니다.

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
| glViewport    | [**CD3D11\_VIEWPORT**](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/jj151722(v=vs.85)), [**ID3D11DeviceContext::RSSetViewports**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) |

 

## <a name="configuring-the-vertex-shaders"></a>꼭짓점 셰이더 구성


셰이더가 로드되면 Direct3D 11에서 꼭짓점 셰이더 구성이 완료됩니다. uniform은 [**ID3D11DeviceContext1::VSSetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vssetconstantbuffers1)을 통해 상수 버퍼로 전달됩니다.

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader)                       |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1::VSGetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vsgetshader)                       |
| glGetUniformfv, glGetUniformiv   | [**ID3D11DeviceContext1::VSGetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vsgetconstantbuffers1). |

 

## <a name="configuring-the-pixel-shaders"></a>픽셀 셰이더 구성


셰이더가 로드되면 Direct3D 11에서 픽셀 셰이더 구성이 완료됩니다. uniform은 [**ID3D11DeviceContext1::PSSetConstantBuffers1.** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-pssetconstantbuffers1)을 통해 상수 버퍼로 전달됩니다.

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)                         |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1::PSGetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-psgetshader)                       |
| glGetUniformfv, glGetUniformiv   | [**ID3D11DeviceContext1::PSGetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-psgetconstantbuffers1). |

 

## <a name="generating-the-final-results"></a>최종 결과 생성


파이프라인이 완료되면 셰이더 단계의 결과를 백 버퍼에 그립니다. Direct3D 11에서는 Open GL ES 2.0과 마찬가지로 그리기 명령을 호출하여 결과를 백 버퍼에 색상 맵으로 출력한 다음 해당 백 버퍼를 다시 디스플레이로 보내는 작업이 여기에 포함됩니다.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                         |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glDrawElements | [**ID3D11DeviceContext1::Draw**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw)하십시오 [ **ID3D11DeviceContext1::DrawIndexed** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) (또는 다른 그리기\* 메서드 [  **ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)). |
| eglSwapBuffers | [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)                                                                                                                                                                              |

 

## <a name="porting-glsl-to-hlsl"></a>GLSL을 HLSL로 포팅


GLSL과 HLSL은 복합 형식 지원 및 구문(일부 전체 구문)을 제외하고는 그렇게 다르지 않습니다. 많은 개발자는 일반적인 OpenGL ES 2.0 명령 및 정의를 HLSL의 명령 및 정의로 별칭을 지정하여 둘 사이를 포팅하는 것이 가장 쉽다는 사실을 알고 있습니다. Direct3D에서는 셰이더 모델 버전을 사용하여 그래픽 인터페이스에서 지원하는 HLSL의 기능 집합을 표현합니다. OpenGL에는 HLSL에 대한 다양한 버전 사양이 포함되어 있습니다. 다음 표는 다른 버전의 관점에서 Direct3D 11 및 OpenGL ES 2.0에 대해 정의된 셰이더 언어 기능 집합의 대략적인 아이디어를 제공합니다.

| 셰이더 언어           | GLSL 기능 버전                                                                                                                                                                                                      | Direct3D 셰이더 모델 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Direct3D 11 HLSL          | ~4.30.                                                                                                                                                                                                                    | SM 5.0                |
| OpenGL ES 2.0용 GLSL ES | 1.40. OpenGL ES 2.0용 GLSL ES의 이전 구현에서는 1.10~1.30을 사용할 수도 있습니다. GlGetString 사용 하 여 원래 코드 검사 (GL\_음영\_언어\_버전) 또는 glGetString (음영\_언어\_버전) 결정 하 합니다. | ~SM 2.0               |

 

두 셰이더 언어 간 차이점 및 일반적인 구문 매핑에 대한 자세한 내용은 [GLSL-HLSL 참조](glsl-to-hlsl-reference.md)를 읽어 보세요.

## <a name="porting-the-opengl-intrinsics-to-hlsl-semantics"></a>OpenGL 내부 기능을 HLSL 의미 체계로 포팅


Direct3D 11 HLSL 의미 체계는 uniform 또는 특성 이름과 같은 문자열로서, 앱과 셰이더 프로그램 간에 전달되는 값을 식별하는 데 사용됩니다. 이러한 의미 체계는 가능한 다양한 문자열 중 어느 것도 될 수 있지만, 모범 사례는 용도를 나타내는 POSITION 또는 COLOR와 같은 문자열을 사용하는 것입니다. 상수 버퍼 또는 버퍼 입력 레이아웃을 만들 때 이러한 의미 체계를 할당합니다. 또한 유사한 값에 대해 별도의 레지스터를 사용하도록 0과 7 사이의 숫자를 의미 체계에 추가할 수 있습니다. 예를 들어 다음과 같은 가치를 제공해야 합니다. COLOR0, COLOR1, COLOR2...

접두사로 사용 하는 의미 체계 "SV\_" 시스템 값 의미 체계는 셰이더 프로그램에서 하도록 작성 된; (CPU에서 실행) 자체 앱에서 수정할 수 없습니다. 일반적으로 여기에는 그래픽 파이프라인의 다른 셰이더 단계의 입력 또는 출력인 값이나, 전적으로 GPU에 의해 생성된 값이 포함됩니다.

또한 SV\_ 의미 체계에 대 한 입력을 지정 하거나 셰이더 단계에서 출력을 사용할 때 다른 동작 있어야 합니다. 예를 들어, SV\_SV 고 꼭 짓 점 셰이더 단계 동안 변환 된 꼭 짓 점 데이터를 포함 하는 위치 (출력)\_래스터화 중 보간된 픽셀 위치 값을 포함 하는 위치 (입력).

다음은 일반적인 OpenGL ES 2.0 셰이더 내부 기능에 대한 몇 가지 매핑입니다.

| OpenGL 시스템 값 | 이 HLSL 의미 체계 사용                                                                                                                                                   |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| gl\_위치        | 꼭짓점 버퍼 데이터의 POSITION(n). SV\_위치는 픽셀 셰이더를 픽셀 위치를 제공 하 고 앱에서 쓸 수 없습니다.                                        |
| gl\_보통          | 꼭짓점 버퍼에서 제공하는 일반 데이터의 NORMAL(n)                                                                                                                 |
| gl\_TexCoord\[n\]   | 셰이더에 제공되는 텍스처 UV(일부 OpenGL 설명서에서는 ST) 좌표 데이터의 TEXCOORD(n)                                                                       |
| gl\_FragColor       | 셰이더에 제공되는 RGBA 색 데이터의 COLOR(n). 좌표 데이터에 대해 동일하게 처리됩니다. 의미 체계는 단순히 해당 데이터가 색 데이터임을 식별할 수 있도록 도와줄 뿐입니다. |
| gl\_FragData\[n\]   | SV\_대상\[n\] 대상 질감 또는 기타 픽셀 버퍼에서 픽셀 셰이더 쓰기입니다.                                                                               |

 

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

이 예에서 SV\_대상이 셰이더 실행을 완료 하는 경우 (4 개 부동 소수점 값을 사용 하 여 벡터 정의 됨) 픽셀 색에 기록 되는 렌더링 대상의 위치입니다.

Direct3D에서 의미 체계를 사용하는 방법에 대한 자세한 내용은 [HLSL 의미 체계](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics)를 읽어 보세요.

 

 




