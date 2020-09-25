---
title: HLSL 참조
description: OpenGL ES 2.0에서 Direct3D 11로 그래픽 아키텍처를 이식할 유니버설 Windows 플랫폼 (UWP) 게임을 만들려면 그래픽 아키텍처를 Microsoft HLSL (High Level Shader Language) 코드로 이식 합니다.
ms.assetid: 979d19f6-ef0c-64e4-89c2-a31e1c7b7692
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 고 sl, hlsl, opengl, directx, 셰이더
ms.localizationpriority: medium
ms.openlocfilehash: b6be3cf92162b0e871a04712754a882506767cd6
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218176"
---
# <a name="glsl-to-hlsl-reference"></a>HLSL 참조



[OPENGL ES 2.0에서 Direct3D 11로 그래픽 아키텍처를 이식할](port-from-opengl-es-2-0-to-directx-11-1.md) 유니버설 WINDOWS 플랫폼 (UWP) 게임을 만들려면 그래픽 아키텍처를 Microsoft HLSL (High Level Shader language) 코드로 이식 합니다. 이에 대 한 참조는 OpenGL ES 2.0와 호환 됩니다. HLSL는 Direct3D 11과 호환 됩니다. Direct3d 11과 이전 버전의 Direct3D 간의 차이점에 대 한 정보는 [기능 매핑](feature-mapping.md)을 참조 하세요.

-   [OpenGL ES 2.0과 Direct3D 11 비교](#comparing-opengl-es-20-with-direct3d-11)
-   [HLSL로 고 자신 변수 포팅](#porting-glsl-variables-to-hlsl)
-   [HLSL에 게 고 대 형식 포팅](#porting-glsl-types-to-hlsl)
-   [HLSL에 대 한 글 미리 정의 된 전역 변수 포팅](#porting-glsl-pre-defined-global-variables-to-hlsl)
-   [HLSL에 대 한 고이 변수를 이식 하는 예](#examples-of-porting-glsl-variables-to-hlsl)
    -   [일관, 특성 및 다양 한 인 글](#uniform-attribute-and-varying-in-glsl)
    -   [HLSL의 상수 버퍼 및 데이터 전송](#constant-buffers-and-data-transfers-in-hlsl)
-   [Direct3D에 OpenGL 렌더링 코드를 이식 하는 예제](#examples-of-porting-opengl-rendering-code-to-direct3d)
-   [관련 항목](#related-topics)

## <a name="comparing-opengl-es-20-with-direct3d-11"></a>OpenGL ES 2.0과 Direct3D 11 비교


OpenGL ES 2.0 및 Direct3D 11은 많은 유사성을 갖습니다. 둘 다 유사한 렌더링 파이프라인과 그래픽 기능이 있습니다. 하지만 Direct3D 11은 사양이 아닌 렌더링 구현 및 API입니다. OpenGL ES 2.0은 구현이 아닌 렌더링 사양 및 API입니다. Direct3D 11 및 OpenGL ES 2.0은 일반적으로 다음과 같은 점에서 다릅니다.

| OpenGL ES 2.0                                                                                         | Direct3D 11                                                                                                            |
|-------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| 공급 업체에서 제공 하는 구현과 하드웨어 및 운영 체제에 대 한 규정 준수             | Windows 플랫폼의 Microsoft 하드웨어 추상화 및 인증 구현                                |
| 하드웨어 다양성을 위해 추상화 되었으며, 런타임이 대부분의 리소스를 관리 합니다.                                     | 하드웨어 레이아웃에 대 한 직접 액세스 앱이 리소스 및 처리를 관리할 수 있음                                              |
| 타사 라이브러리를 통해 상위 수준 모듈을 제공 합니다 (예: 간단한 SDL (DirectMedia Layer)). | Direct2D와 같은 더 높은 수준의 모듈은 Windows 앱 개발을 간소화 하기 위해 더 낮은 모듈을 기반으로 합니다.             |
| 하드웨어 공급 업체에서 확장을 통해 구분                                                         | Microsoft는 특정 하드웨어 공급 업체와 관련이 없도록 일반적인 방법으로 API에 선택적 기능을 추가 합니다. |

 

일반적으로는 다음과 같은 방식으로 HLSL와의 차이가 있습니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">GLSL</th>
<th align="left">HLSL</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">절차적, 단계 중심 (C와 유사)</td>
<td align="left">개체 지향, 데이터 중심 (c + +와 같은)</td>
</tr>
<tr class="even">
<td align="left">그래픽 API에 통합 된 셰이더 컴파일</td>
<td align="left">HLSL 컴파일러는 Direct3D를 드라이버에 전달 하기 전에 셰이더를 중간 이진 표현으로 <a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl-part1">컴파일합니다</a> .
<div class="alert">
<strong>참고</strong>    이 이진 표현은 하드웨어에 독립적입니다. 일반적으로 앱이 아닌 앱 빌드 시간에 컴파일됩니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><a href="#porting-glsl-variables-to-hlsl">변수</a> 저장소 한정자</td>
<td align="left">입력 레이아웃 선언을 통해 상수 버퍼 및 데이터 전송</td>
</tr>
<tr class="even">
<td align="left"><p><a href="#porting-glsl-types-to-hlsl">형식</a></p>
<p>일반적인 벡터 형식: vec2/3/4</p>
<p>lowp, mediump, highp</p></td>
<td align="left"><p>일반적인 벡터 형식: float2/3/4</p>
<p>min10float, min16float</p></td>
</tr>
<tr class="odd">
<td align="left">texture2D [Function]</td>
<td align="left"><a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-sample">질감. Sample</a> [datatype. 칩셋용으로</td>
</tr>
<tr class="even">
<td align="left">sampler2D [datatype]</td>
<td align="left"><a href="/windows/desktop/direct3dhlsl/sm5-object-texture2d">Texture2D</a> [datatype]</td>
</tr>
<tr class="odd">
<td align="left">행-주요 행렬 (기본값)</td>
<td align="left">열-주요 행렬 (기본값)
<div class="alert">
<strong>참고</strong>    <strong>Row_major</strong> 형식-한정자를 사용 하 여 하나의 변수에 대 한 레이아웃을 변경할 수 있습니다. 자세한 내용은 <a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl-variable-syntax">변수 구문</a>을 참조 하세요. 컴파일러 플래그 또는 pragma를 지정 하 여 전역 기본값을 변경할 수도 있습니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">조각 셰이더</td>
<td align="left">픽셀 셰이더</td>
</tr>
</tbody>
</table>

 

> **참고**    HLSL에는 두 개의 개별 개체로 질감 및 샘플러가 있습니다. Direct3D 9와 같은 고도에서 질감 바인딩은 샘플러 상태의 일부입니다.

 

에는 미리 정의 된 전역 변수로 많은 OpenGL 상태를 표시 합니다. 예를 들어, 고 대 수를 사용 하면 **gl \_ ** 위치 변수를 사용 하 여 꼭 짓 점 위치를 지정 하 고 **gl \_ FragColor** 변수를 사용 하 여 조각 색을 지정 합니다. HLSL에서는 앱 코드에서 셰이더에 명시적으로 Direct3D 상태를 전달 합니다. 예를 들어 Direct3D 및 HLSL를 사용 하는 경우 꼭 짓 점 셰이더에 대 한 입력은 꼭 짓 점 버퍼의 데이터 형식과 일치 해야 하 고, 앱 코드에서 상수 버퍼의 구조는 셰이더 코드의[cbuffer](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants)(상수 버퍼) 구조와 일치 해야 합니다.

## <a name="porting-glsl-variables-to-hlsl"></a>HLSL로 고 자신 변수 포팅


이를 위해 한정자 (한정자)를 전역 셰이더 변수 선언에 적용 하 여 해당 변수를 셰이더에 특정 동작으로 지정 합니다. HLSL에서 셰이더에 전달 하는 인수를 사용 하 여 셰이더의 흐름을 정의 하 고 셰이더에서 반환 하기 때문에 이러한 한정자가 필요 하지 않습니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">고 SL 변수 동작</th>
<th align="left">HLSL 해당</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>균일</strong></p>
<p>응용 프로그램 코드에서 꼭 짓 점 및 조각 셰이더 중 하나 또는 둘 다로 일관 된 변수를 전달 합니다. 삼각형 메시의 그리기 전체에서 값이 동일 하 게 유지 되도록 해당 셰이더에 삼각형을 그리기 전에 모든 전체 값 형식의 값을 설정 해야 합니다. 이러한 값은 균일 합니다. 일부 전체 형식은 전체 프레임에 대해 설정 되 고 다른 하나는 특정 꼭 짓 점 픽셀 셰이더 쌍으로 고유 하 게 설정 됩니다.</p>
<p>균일 한 변수는 polygon 단위 변수입니다.</p></td>
<td align="left"><p>상수 버퍼를 사용 합니다.</p>
<p><a href="/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-constant-how-to">방법: 상수 버퍼</a> 및 <a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants">셰이더 상수</a>만들기를 참조 하세요.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>변동</strong></p>
<p>꼭 짓 점 셰이더 내에서 다양 한 변수를 초기화 하 고이를 조각 셰이더의 동일 하 게 명명 된 다양 한 변수에 전달 합니다. 꼭 짓 점 셰이더는 각 꼭 짓 점에서 다양 한 변수의 값만 설정 하기 때문에 래스터 라이저는 조각 셰이더에 전달할 조각 값을 생성 하는 해당 값 (관점에 따라 올바른 방식)을 보간합니다. 이러한 변수는 각 삼각형 마다 다릅니다.</p></td>
<td align="left">꼭 짓 점 셰이더에서 반환 하는 구조체를 픽셀 셰이더에 대 한 입력으로 사용 합니다. 의미 값이 일치 하는지 확인 합니다.</td>
</tr>
<tr class="odd">
<td align="left"><p><strong>특성도</strong></p>
<p>특성은 앱 코드에서 꼭 짓 점 셰이더 만으로 전달 하는 꼭 짓 점에 대 한 설명의 일부입니다. 균일 한 것과 달리 각 꼭 짓 점에 대 한 각 특성 값을 설정 하 여 각 꼭 짓 점에 서로 다른 값을 지정할 수 있습니다. 특성 변수는 꼭 짓 점 별 변수입니다.</p></td>
<td align="left"><p>Direct3D 앱 코드에서 꼭 짓 점 버퍼를 정의 하 고 꼭 짓 점 셰이더에 정의 된 꼭 짓 점 입력과 일치 시킵니다. 필요에 따라 인덱스 버퍼를 정의 합니다. <a href="/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-vertex-how-to">방법: 꼭 짓 점 버퍼 만들기</a> 및 <a href="/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-index-how-to">방법: 인덱스 버퍼 만들기</a>를 참조 하세요.</p>
<p>Direct3D 앱 코드에서 입력 레이아웃을 만들고 의미 체계 값을 꼭 짓 점 입력의 값과 일치 시킵니다. <a href="/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage-getting-started">입력 레이아웃 만들기를</a>참조 하세요.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>const</strong></p>
<p>셰이더에 컴파일되고 변경 되지 않는 상수입니다.</p></td>
<td align="left"><strong>정적 const</strong>를 사용 합니다. <strong>static</strong> 은 값이 상수 버퍼에 노출 되지 않음을 의미 하며, <strong>const</strong> 는 셰이더가 값을 변경할 수 없음을 의미 합니다. 따라서 값은 이니셜라이저에 따라 컴파일 시간에 알 수 있습니다.</td>
</tr>
</tbody>
</table>

 

인 경우 한정자가 없는 변수는 각 셰이더에 전용으로 사용 되는 일반적인 전역 변수입니다.

데이터를 질감 (HLSL에서[Texture2D](/windows/desktop/direct3dhlsl/sm5-object-texture2d) ) 및 연결 된 샘플러 (HLSL의[samplerstate](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-sampler) )에 전달 하는 경우 일반적으로 픽셀 셰이더에 전역 변수로 선언 합니다.

## <a name="porting-glsl-types-to-hlsl"></a>HLSL에 게 고 대 형식 포팅


이 표를 사용 하 여 고가 HLSL에 게이를 이식할 수 있습니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">가이 유형</th>
<th align="left">HLSL 형식</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">스칼라 형식: float, int, bool</td>
<td align="left"><p>스칼라 형식: float, int, bool</p>
<p>또한 uint, double</p>
<p>자세한 내용은 <a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl-scalar">스칼라 형식</a>을 참조 하세요.</p></td>
</tr>
<tr class="even">
<td align="left"><p>벡터 형식</p>
<ul>
<li>부동 소수점 벡터: vec2, vec3, vec4</li>
<li>부울 vector: bvec2, bvec3, bvec4</li>
<li>부호 있는 정수 벡터: ivec2, ivec3, ivec4</li>
</ul></td>
<td align="left"><p>벡터 형식</p>
<ul>
<li>float2, float3, float4 및 float1</li>
<li>bool2, bool3, bool4 및 bool1</li>
<li>int2, int3, int4 및 int1</li>
<li><p>이러한 형식에는 float, bool 및 int와 유사한 벡터 확장도 있습니다.</p>
<ul>
<li>uint</li>
<li>min10float, min16float</li>
<li>min12int, min16int</li>
<li>min16uint</li>
</ul></li>
</ul>
<p>자세한 내용은 <a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl-vector">벡터 형식</a> 및 <a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl-appendix-keywords">키워드</a>를 참조 하세요.</p>
<p>또한 vector는 float4 (typedef vector &lt; float, 4 &gt; vector;)로 정의 됩니다. 자세한 내용은 <a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl-user-defined">사용자 정의 형식</a>을 참조 하세요.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>행렬 형식</p>
<ul>
<li>mat2:2x2 float 행렬</li>
<li>mat3:3x3 float 행렬</li>
<li>mat4:4x4 float 행렬</li>
</ul></td>
<td align="left"><p>행렬 형식</p>
<ul>
<li>float2x2</li>
<li>float3x3</li>
<li>float4x4</li>
<li>또한 float1x1, float1x2, float1x3, float1x4, float2x1, float2x3, float2x4, float3x1, float3x2, float3x4, float4x1, float4x2, float4x3</li>
<li><p>이러한 형식에는 float와 유사한 행렬 확장도 있습니다.</p>
<ul>
<li>int, uint, bool</li>
<li>min10float, min16float</li>
<li>min12int, min16int</li>
<li>min16uint</li>
</ul></li>
</ul>
<p>행렬 <a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl-matrix">형식을</a> 사용 하 여 행렬을 정의할 수도 있습니다.</p>
<p>예: matrix &lt; float, 2, 2 &gt; fmatrix = {0.0 f, 0.1, 2.1 f, 2.2 f};</p>
<p>또한 행렬은 float4x4 (typedef matrix &lt; float, 4, 4 &gt; matrix;)로 정의 됩니다. 자세한 내용은 <a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl-user-defined">사용자 정의 형식</a>을 참조 하세요.</p></td>
</tr>
<tr class="even">
<td align="left"><p>float, int, 샘플러에 대 한 전체 자릿수 한정자</p>
<ul>
<li><p>highp</p>
<p>이 한정자는 min16float에서 제공 하는 것 보다 크고 전체 32 비트 float 보다 작은 최소 정밀도 요구 사항을 제공 합니다. HLSL에 해당 하는 항목은 다음과 같습니다.</p>
<p>highp float- &gt; float</p>
<p>highp int- &gt; int</p></li>
<li><p>mediump</p>
<p>Float 및 int에 적용 되는이 한정자는 HLSL의 min16float 및 min12int와 동일 합니다. Min10float와 같이가 수의 최소 10 비트입니다.</p></li>
<li><p>lowp</p>
<p>Float에 적용 되는이 한정자는 부동 소수점 범위-2-2를 제공 합니다. HLSL의 min10float에 해당 합니다.</p></li>
</ul></td>
<td align="left"><p>전체 자릿수 형식</p>
<ul>
<li>min16float: 최소 16 비트 부동 소수점 값</li>
<li><p>min10float</p>
<p>최소 고정 소수점 부호 있는 2.8 비트 값 (정수 2 비트 및 8 비트 소수 부분 구성 요소)입니다. 8 비트 소수 부분 구성 요소는-2에서 2 까지의 전체 포괄 범위를 제공 하기 위해 exclusive 대신 1을 사용할 수 있습니다.</p></li>
<li>min16int: 최소 16 비트 부호 있는 정수</li>
<li><p>min12int: 최소 12 비트 부호 있는 정수</p>
<p>이 형식은 부동 소수점 숫자로 정수를 나타내는 10Level9 (<a href="/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro">9_x 기능 수준</a>) 용입니다. 16 비트 부동 소수점 숫자로 정수를 에뮬레이트할 때 얻을 수 있는 전체 자릿수입니다.</p></li>
<li>min16uint: 최소 16 비트 부호 없는 정수</li>
</ul>
<p>자세한 내용은 <a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl-scalar">스칼라 형식</a> 및 <a href="/windows/desktop/direct3dhlsl/using-hlsl-minimum-precision">HLSL 최소 전체 자릿수 사용</a>을 참조 하세요.</p></td>
</tr>
<tr class="odd">
<td align="left">sampler2D</td>
<td align="left"><a href="/windows/desktop/direct3dhlsl/sm5-object-texture2d">Texture2D</a></td>
</tr>
<tr class="even">
<td align="left">samplerCube</td>
<td align="left"><a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-type">TextureCube</a></td>
</tr>
</tbody>
</table>

 

## <a name="porting-glsl-pre-defined-global-variables-to-hlsl"></a>HLSL에 대 한 글 미리 정의 된 전역 변수 포팅


이 표를 사용 하 여 HLSL에 게 미리 정의 된 전역 변수를 이식 합니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">글이 미리 정의 된 전역 변수</th>
<th align="left">HLSL 의미 체계</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>gl_Position</strong></p>
<p>이 변수는 <strong>vec4</strong>형식입니다.</p>
<p>꼭 짓 점 위치</p>
<p>예:-gl_Position = Position;</p></td>
<td align="left"><p>SV_Position</p>
<p>Direct3D 9의 위치</p>
<p>이 의미 체계는 <strong>float4</strong>형식입니다.</p>
<p>꼭 짓 점 셰이더 출력</p>
<p>꼭 짓 점 위치</p>
<p>예: float4 vPosition: SV_Position;</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_PointSize</strong></p>
<p>이 변수는 <strong>float</strong>형식입니다.</p>
<p>포인트 크기</p></td>
<td align="left"><p>PSIZE</p>
<p>Direct3D 9를 대상으로 하지 않는 한 의미가 없습니다.</p>
<p>이 의미 체계는 <strong>float</strong>형식입니다.</p>
<p>꼭 짓 점 셰이더 출력</p>
<p>포인트 크기</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_FragColor</strong></p>
<p>이 변수는 <strong>vec4</strong>형식입니다.</p>
<p>조각 색</p>
<p>예: gl_FragColor = vec4 (colorVarying, 1.0);</p></td>
<td align="left"><p>SV_Target</p>
<p>Direct3D 9의 색</p>
<p>이 의미 체계는 <strong>float4</strong>형식입니다.</p>
<p>픽셀 셰이더 출력</p>
<p>픽셀 색</p>
<p>예: float4 Color [4]: SV_Target;</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FragData [n]</strong></p>
<p>이 변수는 <strong>vec4</strong>형식입니다.</p>
<p>색 첨부 파일의 조각 색 n</p></td>
<td align="left"><p>SV_Target [n]</p>
<p>이 의미 체계는 <strong>float4</strong>형식입니다.</p>
<p>N 렌더링 대상에 저장 되는 픽셀 셰이더 출력 값입니다. 여기서 0은 &lt; n &lt; = 7입니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_FragCoord</strong></p>
<p>이 변수는 <strong>vec4</strong>형식입니다.</p>
<p>프레임 버퍼 내의 조각 위치</p></td>
<td align="left"><p>SV_Position</p>
<p>Direct3D 9에서 사용할 수 없음</p>
<p>이 의미 체계는 <strong>float4</strong>형식입니다.</p>
<p>픽셀 셰이더 입력</p>
<p>화면 공간 좌표</p>
<p>예: float4 screenSpace: SV_Position</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FrontFacing</strong></p>
<p>이 변수는 <strong>bool</strong>형식입니다.</p>
<p>조각이 front 연결 기본 형식에 속하는지 여부를 확인 합니다.</p></td>
<td align="left"><p>SV_IsFrontFace</p>
<p>Direct3D 9의 VFACE</p>
<p>SV_IsFrontFace은 <strong>bool</strong>형식입니다.</p>
<p>VFACE는 <strong>float</strong>형식입니다.</p>
<p>픽셀 셰이더 입력</p>
<p>기본 연결</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_PointCoord</strong></p>
<p>이 변수는 <strong>vec2</strong>형식입니다.</p>
<p>지점 내의 조각 위치 (점 래스터화만 해당)</p></td>
<td align="left"><p>SV_Position</p>
<p>Direct3D 9의 VPOS</p>
<p>SV_Position <strong>float4</strong>유형입니다.</p>
<p>VPOS는 <strong>float2</strong>유형입니다.</p>
<p>픽셀 셰이더 입력</p>
<p>화면 공간의 픽셀 또는 샘플 위치입니다.</p>
<p>예: float4 pos: SV_Position</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FragDepth</strong></p>
<p>이 변수는 <strong>float</strong>형식입니다.</p>
<p>깊이 버퍼 데이터</p></td>
<td align="left"><p>SV_Depth</p>
<p>Direct3D 9의 깊이</p>
<p>SV_Depth은 <strong>float</strong>형식입니다.</p>
<p>픽셀 셰이더 출력</p>
<p>깊이 버퍼 데이터</p></td>
</tr>
</tbody>
</table>

 

의미 체계를 사용 하 여 꼭 짓 점 셰이더 입력 및 픽셀 셰이더 입력에 대해 위치, 색 등을 지정 합니다. 입력 레이아웃의 의미 체계 값을 꼭 짓 점 셰이더 입력과 일치 해야 합니다. 예제를 보려면 HLSL에 고 대 한 [변수를 이식 하는 예제](#examples-of-porting-glsl-variables-to-hlsl)를 참조 하세요. HLSL 의미 체계에 대 한 자세한 내용은 [의미 체계](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics)를 참조 하세요.

## <a name="examples-of-porting-glsl-variables-to-hlsl"></a>HLSL에 대 한 고이 변수를 이식 하는 예


여기에서는 OpenGL/글 SL 코드에서가 중에 고 하는 예제를 사용 하 고 Direct3D/HLSL code의 동일한 예제를 보여 줍니다.

### <a name="uniform-attribute-and-varying-in-glsl"></a>일관, 특성 및 다양 한 인 글

OpenGL 앱 코드

``` syntax
// Uniform values can be set in app code and then processed in the shader code.
uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

// Incoming position of vertex
attribute vec4 position;
 
// Incoming color for the vertex
attribute vec3 color;
 
// The varying variable tells the shader pipeline to pass it  
// on to the fragment shader.
varying vec3 colorVarying;
```

고 SL 꼭 짓 점 셰이더 코드

``` syntax
//The shader entry point is the main method.
void main()
{
colorVarying = color; //Use the varying variable to pass the color to the fragment shader
gl_Position = position; //Copy the position to the gl_Position pre-defined global variable
}
```

고 SL 조각 셰이더 코드

``` syntax
void main()
{
//Pad the colorVarying vec3 with a 1.0 for alpha to create a vec4 color
//and assign that color to the gl_FragColor pre-defined global variable
//This color then becomes the fragment's color.
gl_FragColor = vec4(colorVarying, 1.0);
}
```

### <a name="constant-buffers-and-data-transfers-in-hlsl"></a>HLSL의 상수 버퍼 및 데이터 전송

다음은 픽셀 셰이더에 이동 하는 HLSL 꼭 짓 점 셰이더에 데이터를 전달 하는 방법의 예입니다. 앱 코드에서 꼭 짓 점 및 상수 버퍼를 정의 합니다. 그런 다음 꼭 짓 점 셰이더 코드에서 상수 버퍼를 [cbuffer](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants) 로 정의 하 고 꼭 짓 점 데이터와 픽셀 셰이더 입력 데이터를 저장 합니다. 여기에서는 **VertexShaderInput** 및 **PixelShaderInput**라는 구조를 사용 합니다.

Direct3D 앱 코드

```cpp
struct ConstantBuffer
{
    XMFLOAT4X4 model;
    XMFLOAT4X4 view;
    XMFLOAT4X4 projection;
};
struct SimpleCubeVertex
{
    XMFLOAT3 pos;   // position
    XMFLOAT3 color; // color
};

 // Create an input layout that matches the layout defined in the vertex shader code.
 const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
 {
     { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
     { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
 };

// Create vertex and index buffers that define a geometry.
```

HLSL 꼭 짓 점 셰이더 코드

``` syntax
cbuffer ModelViewProjectionCB : register( b0 )
{
    matrix model; 
    matrix view;
    matrix projection;
};
// The POSITION and COLOR semantics must match the semantics in the input layout Direct3D app code.
struct VertexShaderInput
{
    float3 pos : POSITION; // Incoming position of vertex 
    float3 color : COLOR; // Incoming color for the vertex
};

struct PixelShaderInput
{
    float4 pos : SV_Position; // Copy the vertex position.
    float4 color : COLOR; // Pass the color to the pixel shader.
};

PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;

    // shader source code

    return vertexShaderOutput;
}
```

HLSL 픽셀 셰이더 코드

``` syntax
// Collect input from the vertex shader. 
// The COLOR semantic must match the semantic in the vertex shader code.
struct PixelShaderInput
{
    float4 pos : SV_Position;
    float4 color : COLOR; // Color for the pixel
};

// Set the pixel color value for the renter target. 
float4 main(PixelShaderInput input) : SV_Target
{
    return input.color;
}
```

## <a name="examples-of-porting-opengl-rendering-code-to-direct3d"></a>Direct3D에 OpenGL 렌더링 코드를 이식 하는 예제


여기에서는 OpenGL ES 2.0 코드에서 렌더링 하는 예제를 보여 주고, Direct3D 11 code의 동일한 예제를 보여 줍니다.

OpenGL 렌더링 코드

``` syntax
// Bind shaders to the pipeline. 
// Both vertex shader and fragment shader are in a program.
glUseProgram(m_shader->getProgram());
 
// Input asssembly 
// Get the position and color attributes of the vertex.

m_positionLocation = glGetAttribLocation(m_shader->getProgram(), "position");
glEnableVertexAttribArray(m_positionLocation);

m_colorLocation = glGetAttribColor(m_shader->getProgram(), "color");
glEnableVertexAttribArray(m_colorLocation);
 
// Bind the vertex buffer object to the input assembler.
glBindBuffer(GL_ARRAY_BUFFER, m_geometryBuffer);
glVertexAttribPointer(m_positionLocation, 4, GL_FLOAT, GL_FALSE, 0, NULL);
glBindBuffer(GL_ARRAY_BUFFER, m_colorBuffer);
glVertexAttribPointer(m_colorLocation, 3, GL_FLOAT, GL_FALSE, 0, NULL);
 
// Draw a triangle with 3 vertices.
glDrawArray(GL_TRIANGLES, 0, 3);
```

Direct3D 렌더링 코드

```cpp
// Bind the vertex shader and pixel shader to the pipeline.
m_d3dDeviceContext->VSSetShader(vertexShader.Get(),nullptr,0);
m_d3dDeviceContext->PSSetShader(pixelShader.Get(),nullptr,0);
 
// Declare the inputs that the shaders expect.
m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());
m_d3dDeviceContext->IASetVertexBuffers(0, 1, vertexBuffer.GetAddressOf(), &stride, &offset);

// Set the primitive's topology.
m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

// Draw a triangle with 3 vertices. triangleVertices is an array of 3 vertices.
m_d3dDeviceContext->Draw(ARRAYSIZE(triangleVertices),0);
```

## <a name="related-topics"></a>관련 항목


* [OpenGL ES 2.0에서 Direct3D 11로의 포트](port-from-opengl-es-2-0-to-directx-11-1.md)

 

 