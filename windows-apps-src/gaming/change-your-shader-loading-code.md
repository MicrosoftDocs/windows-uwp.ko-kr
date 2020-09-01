---
title: OpenGL ES 2.0 셰이더 파이프라인을 Direct3D와 비교
description: 개념적으로 Direct3D 11 셰이더 파이프라인은 OpenGL ES 2.0에 있는 것과 매우 비슷합니다.
ms.assetid: 3678a264-e3f9-72d2-be91-f79cd6f7c4ca
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, opengl, direct3d, 셰이더 파이프라인
ms.localizationpriority: medium
ms.openlocfilehash: 3f7f512ce0eeb793f999ce133f8a2c32c35cd1cd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165377"
---
# <a name="compare-the-opengl-es-20-shader-pipeline-to-direct3d"></a>OpenGL ES 2.0 셰이더 파이프라인을 Direct3D와 비교




**중요 API**

-   [입력-어셈블러 단계](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage)
-   [꼭 짓 점-셰이더 단계](/previous-versions/bb205146(v=vs.85))
-   [픽셀 셰이더 단계](/previous-versions/bb205146(v=vs.85))

개념적으로 Direct3D 11 셰이더 파이프라인은 OpenGL ES 2.0에 있는 것과 매우 비슷합니다. 그러나 API 디자인 측면에서 셰이더 단계를 만들고 관리 하는 주요 구성 요소는 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 및 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)의 두 가지 기본 인터페이스의 일부입니다. 이 항목에서는 일반적인 OpenGL ES 2.0 셰이더 파이프라인 API 패턴을 이러한 인터페이스에서 해당 하는 Direct3D 11에 매핑하려고 시도 합니다.

## <a name="reviewing-the-direct3d-11-shader-pipeline"></a>Direct3D 11 셰이더 파이프라인 검토


셰이더 개체는 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 인터페이스의 메서드 (예: [**ID3D11Device1:: CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 및 [**ID3D11Device1:: CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader))를 사용 하 여 생성 됩니다.

Direct3D 11 graphics 파이프라인은 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 인터페이스의 인스턴스를 통해 관리 되며 다음 단계를 수행 합니다.

-   [입력-어셈블러 단계](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage). 입력 어셈블러 단계는 파이프라인에 데이터 (삼각형, 선 및 요소)를 제공 합니다. 이 단계를 지 원하는 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 메서드에는 "IA" 접두사가 붙습니다.
-   [꼭 짓 점-셰이더 단계](/previous-versions/bb205146(v=vs.85)) -꼭 짓 점 셰이더 단계에서는 일반적으로 변환, 스킨, 조명 등의 작업을 수행 하는 꼭 짓 점을 처리 합니다. 꼭 짓 점 셰이더는 항상 단일 입력 버텍스를 사용 하 고 단일 출력 꼭 짓 점을 생성 합니다. 이 단계를 지 원하는 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 메서드에는 "VS" 접두사가 붙습니다.
-   [스트림 출력 단계](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-stream-stage) -스트림 출력 단계는 파이프라인의 기본 데이터를 래스터 라이저 방식으로 메모리에 스트리밍합니다. 데이터를 스트리밍 하거나 래스터 라이저로 전달할 수 있습니다. 메모리로 스트리밍되는 데이터는 입력 데이터로 다시 파이프라인으로 재순환 되거나 CPU에서 다시 읽을 수 있습니다. 이 단계를 지 원하는 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 메서드에는 "그렇다면" 접두사가 붙습니다.
-   [래스터 라이저 단계](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage) -래스터 라이저는 기본 형식을, 픽셀 셰이더의 기본 형식을 준비 하 고, 픽셀 셰이더를 호출 하는 방법을 결정 합니다. 파이프라인에 픽셀 셰이더 ( [**ID3D11DeviceContext::P SSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader)를 사용 하 여 픽셀 셰이더 단계를 NULL로 설정)를 표시 하 고 깊이 및 스텐실 테스트를 사용 하지 않도록 설정 하 여 래스터화를 사용 하지 않도록 설정할 수 있습니다. DepthEnable 및 StencilEnable는 [**D3D11 \_ DEPTH \_ 스텐실 \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_depth_stencil_desc)에서 FALSE로 설정 합니다. 사용 하지 않도록 설정 되어 있는 동안에는 래스터화 관련 파이프라인 카운터가 업데이트 되지 않습니다.
-   [픽셀 셰이더 단계](/previous-versions/bb205146(v=vs.85)) -픽셀 셰이더 단계는 기본 형식에 대 한 보간된 데이터를 받고 색과 같은 픽셀 별 데이터를 생성 합니다. 이 단계를 지 원하는 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 메서드에는 "PS" 접두사가 붙습니다.
-   [출력-병합기 단계](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage) -출력 병합기 단계는 다양 한 형식의 출력 데이터 (픽셀 셰이더 값, 깊이 및 스텐실 정보)를 렌더링 대상 및 깊이/스텐실 버퍼의 내용과 결합 하 여 최종 파이프라인 결과를 생성 합니다. 이 단계를 지 원하는 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 메서드에는 "OM" 접두사가 붙습니다.

기 하 도형 셰이더, 선체 셰이더, tesselators 및 도메인 셰이더에 대 한 단계도 있지만 OpenGL ES 2.0에는 analogues 없기 때문에 여기서는 설명 하지 않습니다. 이러한 단계에 대 한 전체 메서드 목록은 [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) 및 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 참조 페이지를 참조 하세요. **ID3D11DeviceContext1** 는 Direct3D 11의 **ID3D11DeviceContext** 를 확장 합니다.

## <a name="creating-a-shader"></a>셰이더 만들기


Direct3D에서 셰이더 리소스는 컴파일하고 로드 하기 전에 생성 되지 않습니다. 대신 HLSLis 로드 될 때 리소스가 생성 됩니다. 따라서 특정 형식의 초기화 된 셰이더 리소스 (예: GL \_ 꼭 짓 점 \_ 셰이더 또는 gl \_ 조각 \_ 셰이더)를 만드는 glCreateShader에는 직접적으로 유사한 함수가 없습니다. 대신, [**ID3D11Device1:: CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 및 [**ID3D11Device1:: CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)와 같은 특정 함수를 사용 하 여 HLSL을 로드 한 후에 셰이더를 만들고이를 매개 변수로 사용 합니다.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                                             |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCreateShader | 컴파일된 셰이더 개체를 성공적으로 로드 한 후 [**ID3D11Device1:: CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 및 [**ID3D11Device1:: CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) 를 호출 하 여 cso를 버퍼로 전달 합니다. |

 

## <a name="compiling-a-shader"></a>셰이더 컴파일


Direct3D 셰이더는 유니버설 Windows 플랫폼 (UWP) 앱에서 컴파일된 셰이더 개체 (.cas) 파일로 미리 컴파일되어 Windows 런타임 파일 Api 중 하나를 사용 하 여 로드 해야 합니다. 데스크톱 앱은 런타임에 텍스트 파일 또는 문자열에서 셰이더를 컴파일할 수 있습니다. Cso 파일은 Microsoft Visual Studio 프로젝트에 포함 되는 모든. m a c 파일을 기반으로 하며, 확장명이 .cafile 인 경우에만 동일한 이름을 유지 합니다. 제공할 때 패키지에 포함 되어 있는지 확인 합니다.

| OpenGL ES 2.0                          | Direct3D 11                                                                                                                                                                   |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCompileShader                        | 해당 없음. Visual Studio에서 파일을 .ccefile로 컴파일하여 패키지에 포함 합니다.                                                                                     |
| 컴파일 상태에 glGetShaderiv 사용 | 해당 없음. 컴파일에 오류가 발생 한 경우 Visual Studio의 FX 컴파일러 (FXC.EXE)에서 컴파일 출력을 참조 하세요. 컴파일이 성공적으로 완료 되 면 해당 하는 CSFILE이 만들어집니다. |

 

## <a name="loading-a-shader"></a>셰이더 로드


셰이더를 만드는 방법에 대 한 섹션에서 설명한 것 처럼 Direct3D 11은 해당 하는 CSO 파일이 버퍼로 로드 되 고 다음 테이블의 메서드 중 하나로 전달 될 때 셰이더를 만듭니다.

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                                                                                                           |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ShaderSource  | 컴파일된 셰이더 개체를 성공적으로 로드 한 후 [**ID3D11Device1:: CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 및 [**ID3D11Device1:: CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) 를 호출 합니다. |

 

## <a name="setting-up-the-pipeline"></a>파이프라인 설정


OpenGL ES 2.0에는 실행을 위한 여러 셰이더를 포함 하는 "셰이더 프로그램" 개체가 있습니다. 개별 셰이더는 셰이더 프로그램 개체에 연결 됩니다. 그러나 Direct3D 11에서는 렌더링 컨텍스트 ([**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1))를 직접 사용 하 고 셰이더를 만듭니다.

| OpenGL ES 2.0   | Direct3D 11                                                                                   |
|-----------------|-----------------------------------------------------------------------------------------------|
| 인 글 Createprogram | 해당 없음. Direct3D 11은 셰이더 프로그램 개체 추상화를 사용 하지 않습니다.                          |
| 인 글 Linkprogram   | 해당 없음. Direct3D 11은 셰이더 프로그램 개체 추상화를 사용 하지 않습니다.                          |
| 인 글 Useprogram    | 해당 없음. Direct3D 11은 셰이더 프로그램 개체 추상화를 사용 하지 않습니다.                          |
| 글 Get프로그래밍 Iv  | 만든 참조를 사용 하 여 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)합니다. |

 

정적 [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 메서드를 사용 하 여 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 및 [**ID3D11Device1**](/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11device2) 의 인스턴스를 만듭니다.

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

## <a name="setting-the-viewports"></a>뷰포트를 설정 하는 중


Direct3D 11에서 뷰포트를 설정 하는 것은 OpenGL ES 2.0에서 뷰포트를 설정 하는 방법과 매우 비슷합니다. Direct3D 11에서 구성 된 [**CD3D11 \_ 뷰포트**](/previous-versions/windows/desktop/legacy/jj151722(v=vs.85))를 사용 하 여 [**ID3D11DeviceContext:: RSSetViewports**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) 를 호출 합니다.

Direct3D 11: 뷰포트 설정

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
| 인 글 Viewport    | [**CD3D11 \_ 뷰포트**](/previous-versions/windows/desktop/legacy/jj151722(v=vs.85)), [ **ID3D11DeviceContext:: RSSetViewports**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) |

 

## <a name="configuring-the-vertex-shaders"></a>꼭 짓 점 셰이더 구성


Direct3D 11에서 꼭 짓 점 셰이더 구성은 셰이더가 로드 될 때 수행 됩니다. [**ID3D11DeviceContext1:: VSSetConstantBuffers1**](/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vssetconstantbuffers1)를 사용 하 여 상수 버퍼로 전달 되는 형식입니다.

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader)                       |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1:: VSGetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vsgetshader)                       |
| glGetUniformfv, glGetUniformiv   | [**ID3D11DeviceContext1:: VSGetConstantBuffers1**](/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vsgetconstantbuffers1). |

 

## <a name="configuring-the-pixel-shaders"></a>픽셀 셰이더 구성


Direct3D 11에서 픽셀 셰이더 구성은 셰이더가 로드 될 때 수행 됩니다. [ **ID3D11DeviceContext1::P ssetconstantbuffers1** 를 사용 하 여 상수 버퍼로 전달 되는 형식입니다.](/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-pssetconstantbuffers1)

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)                         |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1::P SGetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-psgetshader)                       |
| glGetUniformfv, glGetUniformiv   | [**ID3D11DeviceContext1::P sgetconstantbuffers1**](/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-psgetconstantbuffers1). |

 

## <a name="generating-the-final-results"></a>최종 결과 생성


파이프라인이 완료 되 면 셰이더 단계의 결과를 백 버퍼에 그립니다. Direct3D 11에서 Open GL ES 2.0와 마찬가지로,이 작업을 수행 하려면 그리기 명령을 호출 하 여 결과를 백 버퍼에서 색 맵으로 출력 하 고 버퍼를 다시 표시로 thensending 합니다.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                         |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 요소 | [**ID3D11DeviceContext1::D raw**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw), [**ID3D11DeviceContext1::D Rawindexed**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) (또는 \* [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)의 다른 그리기 메서드). |
| E글 Swapbuffers | [**IDXGISwapChain1::Present1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)                                                                                                                                                                              |

 

## <a name="porting-glsl-to-hlsl"></a>HLSL로가는


HLSL SL 및는 복잡 한 형식 지원 및 구문 외에도 전체 구문을 크게 다르지 않습니다. 대부분의 개발자는 일반적인 OpenGL ES 2.0 명령 및 정의를 해당 HLSL 동등한 것으로 별칭을 하 여 둘 사이의 포트를 가장 쉽게 찾을 수 있습니다. Direct3D에서는 셰이더 모델 버전을 사용 하 여 그래픽 인터페이스에서 지 원하는 HLSL의 기능 집합을 표현 합니다. OpenGL에 HLSL에 대 한 다른 버전 사양이 있습니다. 다음 표에서는 Direct3D 11 및 OpenGL ES 2.0에 대해 정의 된 셰이더 언어 기능 집합을 다른 버전의 측면에서 대략적으로 파악할 수 있도록 시도 합니다.

| 셰이더 언어           | 고 SL 기능 버전                                                                                                                                                                                                      | Direct3D 셰이더 모델 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Direct3D 11 HLSL          | ~ 4.30.                                                                                                                                                                                                                    | SM 5.0                |
| OpenGL ES 2.0에 대 한 글 합니다. | 1.40. OpenGL ES 2.0에 대 한 이전 버전의 구현은 1.10 ~ 1.30를 사용할 수 있습니다. GlGetString (GL \_ 음영 \_ 언어 \_ 버전) 또는 glGetString (음영 언어 버전)를 사용 하 여 원본 코드를 확인 \_ \_ 하 여 확인 합니다. | ~ SM 2.0               |

 

두 셰이더 언어와 일반적인 구문 매핑 간의 차이점에 대 한 자세한 내용은 [HLSL 참조를 참조](glsl-to-hlsl-reference.md)하세요.

## <a name="porting-the-opengl-intrinsics-to-hlsl-semantics"></a>HLSL 의미 체계에 OpenGL 내장 함수 포팅


Direct3D 11 HLSL 의미 체계는 응용 프로그램과 셰이더 프로그램 간에 전달 되는 값을 식별 하는 데 사용 되는 균일 한 또는 특성 이름과 같은 문자열입니다. 사용할 수 있는 문자열은 다양 하지만 사용을 나타내는 위치나 색과 같은 문자열을 사용 하는 것이 가장 좋습니다. 상수 버퍼 또는 버퍼 입력 레이아웃을 생성할 때 이러한 의미 체계를 할당 합니다. 또한 유사한 값에 대해 별도의 레지스터를 사용 하도록 0에서 7 사이의 숫자를 의미 체계에 추가할 수 있습니다. 예: COLOR0, COLOR1, COLOR2 ...

"SV"로 시작 하는 의미 체계는 \_ 셰이더 프로그램에서 작성 한 시스템 값 의미 체계입니다. 앱 자체 (CPU에서 실행)는 수정할 수 없습니다. 일반적으로 이러한 값은 그래픽 파이프라인의 다른 셰이더 단계에서 입력 또는 출력 이거나 GPU에 의해 완전히 생성 되는 값을 포함 합니다.

또한, SV \_ 의미 체계는 셰이더 단계에서의 입력 또는 출력을 지정 하는 데 사용 되는 경우 서로 다른 동작을 포함 합니다. 예를 들어 SV \_ position (출력)에는 꼭 짓 점 셰이더 단계 동안 변환 된 꼭 짓 점 데이터가 포함 되 고, sv \_ position (입력)에는 래스터화 중 보간된 픽셀 위치 값이 포함 됩니다.

다음은 일반적인 OpenGL ES 2.0 셰이더 instrinsics에 대 한 몇 가지 매핑입니다.

| OpenGL 시스템 값 | 이 HLSL 의미 체계 사용                                                                                                                                                   |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| gl \_ 위치        | 꼭 짓 점 버퍼 데이터의 위치 (n)입니다. SV \_ 위치는 픽셀 셰이더에 픽셀 위치를 제공 하며 앱에서 쓸 수 없습니다.                                        |
| gl \_ 보통          | 버텍스 버퍼에서 제공 하는 일반 데이터의 경우 NORMAL (n)입니다.                                                                                                                 |
| gl \_ TexCoord \[ n\]   | 질감 UV (일부 OpenGL 설명서의 ST)에 대해 TEXCOORD (n)-셰이더에 제공 된 데이터를 조정 합니다.                                                                       |
| gl \_ FragColor       | 셰이더에 제공 되는 RGBA 색 데이터의 색 (n)입니다. 데이터를 조정 하는 것과 동일 하 게 처리 됩니다. 의미 체계는 색 데이터 인지를 식별 하는 데 도움이 됩니다. |
| gl \_ FragData \[ n\]   | \_ \[ \] 픽셀 셰이더에서 대상 질감이 나 기타 픽셀 버퍼로 쓰기 위한 SV 대상 n입니다.                                                                               |

 

의미 체계를 코딩 하는 방법은 OpenGL ES 2.0에서 내장 함수를 사용 하는 것과 다릅니다. OpenGL에서 구성 또는 선언 없이 많은 내장 함수에 직접 액세스할 수 있습니다. Direct3D에서는 특정 상수 버퍼의 필드를 특정 의미 체계를 사용 하도록 선언 하거나,이를 셰이더의 **main ()** 메서드의 반환 값으로 선언 해야 합니다.

상수 버퍼 정의에 사용 되는 의미 체계의 예는 다음과 같습니다.

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

이 코드는 단순 상수 버퍼 쌍을 정의 합니다.

다음은 조각 셰이더에 의해 반환 되는 값을 정의 하는 데 사용 되는 의미 체계의 예입니다.

```cpp
// A pass-through for the (interpolated) color data.
float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color,1.0f);
}
```

이 경우, SV \_ target은 셰이더가 실행을 완료할 때 픽셀 색 (4 개의 float 값이 있는 벡터로 정의 됨)이 기록 되는 렌더링 대상의 위치입니다.

Direct3D와의 의미 체계 사용에 대 한 자세한 내용은 [HLSL 의미 체계](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics)를 참조 하세요.

 

 