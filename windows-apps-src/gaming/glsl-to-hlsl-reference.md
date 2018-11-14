---
author: mtoepke
title: GLSL-HLSL 참조
description: OpenGL ES 2.0에서 Direct3D 11로 그래픽 아키텍처를 포팅하여 UWP(유니버설 Windows 플랫폼)용 게임을 만드는 경우 GLSL(OpenGL Shader Language) 코드를 Microsoft HLSL(High Level Shader Language) 코드로 포팅합니다.
ms.assetid: 979d19f6-ef0c-64e4-89c2-a31e1c7b7692
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, glsl, hlsl, opengl, directx, 셰이더
ms.localizationpriority: medium
ms.openlocfilehash: 30c925f9ebb07d578147dfba373fdeb3baa364fe
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6183564"
---
# <a name="glsl-to-hlsl-reference"></a>GLSL-HLSL 참조



[OpenGL ES 2.0에서 Direct3D 11로 그래픽 아키텍처를 포팅](port-from-opengl-es-2-0-to-directx-11-1.md)하여 UWP(유니버설 Windows 플랫폼)용 게임을 만드는 경우 GLSL(OpenGL Shader Language) 코드를 Microsoft HLSL(High Level Shader Language) 코드로 포팅합니다. 여기서 참조되는 GLSL은 OpenGL ES 2.0과 호환됩니다. HLSL은 Direct3D 11과 호환됩니다. Direct3D 11과 Direct3D의 이전 버전과의 차이점에 대한 자세한 내용은 [기능 매핑](feature-mapping.md)을 참조하세요.

-   [OpenGL ES 2.0과 Direct3D 11 비교](#comparing-opengl-es-20-with-direct3d-11)
-   [HLSL로 GLSL 변수 포팅](#porting-glsl-variables-to-hlsl)
-   [GLSL 형식을 HLSL로 포팅](#porting-glsl-types-to-hlsl)
-   [GLSL 미리 정의된 전역 변수를 HLSL로 포팅](#porting-glsl-pre-defined-global-variables-to-hlsl)
-   [HLSL로의 GLSL 변수 포팅 예](#examples-of-porting-glsl-variables-to-hlsl)
    -   [GLSL의 uniform, attribute 및 varying](#uniform-attribute-and-varying-in-glsl)
    -   [HLSL의 상수 버퍼 및 데이터 전송](#constant-buffers-and-data-transfers-in-hlsl)
-   [Direct3D로의 OpenGL 렌더링 코드 포팅 예](#examples-of-porting-opengl-rendering-code-to-direct3d)
-   [관련 항목](#related-topics)

## <a name="comparing-opengl-es-20-with-direct3d-11"></a>OpenGL ES 2.0과 Direct3D 11 비교


OpenGL ES 2.0과 Direct3D 11은 유사점이 많습니다. 이 둘의 렌더링 파이프라인 및 그래픽 기능은 유사합니다. 그러나 Direct3D 11은 렌더링 구현 및 API이며, 사양이 아닙니다. OpenGL ES 2.0은 렌더링 사양 및 API이며, 구현이 아닙니다. Direct3D 11과 OpenGL ES 2.0은 일반적으로 다음과 같은 방식에서 다릅니다.

| OpenGL ES 2.0                                                                                         | Direct3D 11                                                                                                            |
|-------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| 하드웨어 및 운영 체제 독립적 사양과 공급업체에서 제공한 구현             | Windows 플랫폼에 대한 하드웨어 추상화 및 인증의 Microsoft 구현                                |
| 하드웨어 다양성에 대해 추상화되었으며, 대부분의 리소스가 런타임에서 관리됨                                     | 하드웨어 레이아웃에 대한 직접적인 액세스(앱이 리소스 및 처리를 관리할 수 있음)                                              |
| 타사 라이브러리를 통해 높은 수준의 모듈 제공(예: SDL(Simple DirectMedia Layer)) | Windows 앱 개발을 단순화하기 위해 높은 수준의 모듈(예: Direct2D)이 낮은 수준의 모듈을 기반으로 빌드됨             |
| 확장을 통한 하드웨어 공급업체 차별화                                                         | Microsoft는 일반적인 방식으로 API에 선택적 기능을 추가하므로 특정 하드웨어 공급업체에 구속되지 않음 |

 

GLSL과 HLSL은 일반적으로 다음과 같은 방식에서 다릅니다.

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
<td align="left">절차적, 단계 중심(C 등)</td>
<td align="left">개체 지향, 데이터 중심(C++ 등)</td>
</tr>
<tr class="even">
<td align="left">셰이더 컴파일이 그래픽 API에 통합되어 있음</td>
<td align="left">HLSL 컴파일러는 Direct3D가 셰이더를 드라이버에 전달하기 전에 중간 이진 표현으로 <a href="https://msdn.microsoft.com/library/windows/desktop/bb509633">셰이더를 컴파일</a>합니다.
<div class="alert">
<strong>참고</strong>이 이진 표현은 하드웨어 독립적입니다. 일반적으로 앱 런타임이 아니라 앱 빌드 시간에 컴파일됩니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><a href="#porting-glsl-variables-to-hlsl">변수</a> 저장소 한정자</td>
<td align="left">입력 레이아웃 선언을 통한 데이터 전송 및 상수 버퍼</td>
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
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/bb509695">texture.Sample</a> [datatype.Function]</td>
</tr>
<tr class="even">
<td align="left">sampler2D [datatype]</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/ff471525">Texture2D</a> [datatype]</td>
</tr>
<tr class="odd">
<td align="left">행 중심 행렬(기본값)</td>
<td align="left">열 중심 행렬(기본값)
<div class="alert">
<strong>참고</strong>  <strong>row_major</strong> 형식 한정자를 사용 하 여 한 변수의 레이아웃을 변경 합니다. 자세한 내용은 <a href="https://msdn.microsoft.com/library/windows/desktop/bb509706">변수 구문</a>을 참조하세요. 또한 전역 기본값을 변경하기 위한 pragma나 컴파일러 플래그를 지정할 수 있습니다.
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

 

> **참고**HLSL에서는 텍스처와 샘플러가 별도 객체 두 개로 존재 합니다. Direct3D 9와 마찬가지로, GLSL에서 텍스처 바인딩은 샘플러 상태의 일부입니다.

 

GLSL에서 상당수의 OpenGL 상태는 미리 정의된 전역 변수로 표시됩니다. 예를 들어, GLSL에서는 **gl\_Position** 변수를 사용하여 꼭짓점 위치를 지정하고 **gl\_FragColor** 변수를 사용하여 조각 색을 지정합니다. HLSL에서는 Direct3D 상태를 명시적으로 앱 코드에서 셰이더로 전달합니다. 예를 들어, Direct3D와 HLSL을 사용하는 경우 꼭짓점 셰이더에 대한 입력은 꼭짓점 버퍼의 데이터 형식과 일치해야 하고, 앱 코드의 상수 버퍼 구조는 셰이더 코드의 상수 버퍼([cbuffer](https://msdn.microsoft.com/library/windows/desktop/bb509581)) 구조와 일치해야 합니다.

## <a name="porting-glsl-variables-to-hlsl"></a>HLSL로 GLSL 변수 포팅


GLSL에서는 전역 셰이더 변수 선언에 한정자를 적용하여 셰이더에서 해당 변수에 특정 동작을 제공합니다. HLSL에서는 셰이더에 전달하고 셰이더로부터 반환되는 인수로 셰이더의 흐름을 정의하기 때문에 이러한 한정자가 필요하지 않습니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">GLSL 변수 동작</th>
<th align="left">HLSL 해당 변수 동작</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>uniform</strong></p>
<p>uniform 변수를 앱 코드에서 꼭짓점 셰이더나 조각 셰이더로 전달하거나 두 셰이더 모두에게 전달합니다. 해당 셰이더로 삼각형을 그리기 전에 모든 uniform의 값을 설정해야 하는데, 이렇게 해야만 해당 값이 삼각형 메시를 그리는 내내 동일하게 유지됩니다. 이러한 값은 uniform입니다. 일부 uniform은 전체 프레임에 대해 설정되며, 나머지 다른 uniform은 하나의 특정 꼭짓점-픽셀 셰이더 쌍에 대해 고유하게 설정됩니다.</p>
<p>uniform 변수는 다각형별 변수입니다.</p></td>
<td align="left"><p>상수 버퍼를 사용합니다.</p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476896">방법: 상수 버퍼 만들기</a> 및 <a href="https://msdn.microsoft.com/library/windows/desktop/bb509581">셰이더 상수</a>를 참조하세요.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>varying</strong></p>
<p>꼭짓점 셰이더 내부에서 varying 변수를 초기화하여 조각 셰이더 내의 이름이 같은 varying 변수로 전달합니다. 꼭짓점 셰이더는 각 꼭짓점에서 varying 변수의 값을 설정만 하기 때문에, 래스터라이저는 해당 값을 보간하여(관점 수정 방식으로) 조각별 값을 생성하여 조각 셰이더로 전달합니다. 이러한 변수는 각 삼각형에서 달라집니다.</p></td>
<td align="left">꼭짓점 셰이더에서 픽셀 셰이더에 대한 입력으로 반환하는 구조를 사용합니다. 의미 체계 값이 일치하는지 확인합니다.</td>
</tr>
<tr class="odd">
<td align="left"><p><strong>특성</strong></p>
<p>attribute는 앱 코드에서 꼭짓점 셰이더로만 전달하는 꼭짓점에 대한 설명의 일부입니다. uniform과 달리, 각 꼭짓점에 대해 attribute의 값을 설정합니다. 그러면 각 꼭짓점이 서로 다른 값을 가질 수 있습니다. attribute 변수는 꼭짓점별 변수입니다.</p></td>
<td align="left"><p>Direct3D 앱 코드에서 꼭짓점 버퍼를 정의하여 꼭짓점 셰이더에 정의되어 있는 꼭짓점 입력에 일치시킵니다. 선택적으로 인덱스 버퍼를 정의합니다. <a href="https://msdn.microsoft.com/library/windows/desktop/ff476899">방법: 꼭짓점 버퍼 만들기</a> 및 <a href="https://msdn.microsoft.com/library/windows/desktop/ff476897">방법: 인덱스 버퍼 만들기</a>를 참조하세요.</p>
<p>Direct3D 앱 코드에서 입력 레이아웃을 만들고 꼭짓점 입력에서 의미 체계 값을 해당 레이아웃에 일치시킵니다. <a href="https://msdn.microsoft.com/library/windows/desktop/bb205117#Create_the_Input_Layout">입력 레이아웃 만들기</a>를 참조하세요.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>const</strong></p>
<p>셰이더에 컴파일되는 상수로서, 절대 변경되지 않습니다.</p></td>
<td align="left"><strong>static const</strong>를 사용합니다. <strong>static</strong>은 값이 상수 버퍼에 표시되지 않음을 의미하고, <strong>const</strong>는 셰이더가 값을 변경할 수 없음을 의미합니다. 따라서 값은 해당 이니셜라이저를 기반으로 컴파일 시간에 알 수 있습니다.</td>
</tr>
</tbody>
</table>

 

GLSL에서 한정자가 없는 변수는 각 셰이더에 private인 일반 전역 변수입니다.

데이터를 텍스처(HLSL의 경우 [Texture2D](https://msdn.microsoft.com/library/windows/desktop/ff471525)) 및 연결된 해당 샘플러(HLSL의 경우 [SamplerState](https://msdn.microsoft.com/library/windows/desktop/bb509644))로 전달하는 경우 일반적으로 픽셀 셰이더에서 해당 데이터를 전역 변수로 선언합니다.

## <a name="porting-glsl-types-to-hlsl"></a>GLSL 형식을 HLSL로 포팅


다음 표를 사용하여 GLSL 형식을 HLSL로 포팅합니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">GLSL 형식</th>
<th align="left">HLSL 형식</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">스칼라 형식: float, int, bool</td>
<td align="left"><p>스칼라 형식: float, int, bool</p>
<p>또한 uint, double</p>
<p>자세한 내용은 <a href="https://msdn.microsoft.com/library/windows/desktop/bb509646">스칼라 형식</a>을 참조하세요.</p></td>
</tr>
<tr class="even">
<td align="left"><p>벡터 형식</p>
<ul>
<li>부동 소수점 벡터: vec2, vec3, vec4</li>
<li>부울 벡터: bvec2, bvec3, bvec4</li>
<li>부호 있는 정수 벡터: ivec2, ivec3, ivec4</li>
</ul></td>
<td align="left"><p>벡터 형식</p>
<ul>
<li>float2, float3 float4 및 float1</li>
<li>bool2, bool3, bool4 및 bool1</li>
<li>int2, int3, int4 및 int1</li>
<li><p>또한 이러한 형식에는 float, bool 및 int와 유사한 벡터 확장이 포함됩니다.</p>
<ul>
<li>uint</li>
<li>min10float, min16float</li>
<li>min12int, min16int</li>
<li>min16uint</li>
</ul></li>
</ul>
<p>자세한 내용은 <a href="https://msdn.microsoft.com/library/windows/desktop/bb509707">벡터 형식</a> 및 <a href="https://msdn.microsoft.com/library/windows/desktop/bb509568">키워드</a>를 참조하세요.</p>
<p>벡터는 float4로 정의되는 형식이기도 합니다(typedef vector &lt;float, 4&gt; vector;). 자세한 내용은 <a href="https://msdn.microsoft.com/library/windows/desktop/bb509702">사용자 정의 형식</a>을 참조하세요.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>행렬 형식</p>
<ul>
<li>mat2: 2x2 float 행렬</li>
<li>mat3: 3x3 float 행렬</li>
<li>mat4: 4x4 float 행렬</li>
</ul></td>
<td align="left"><p>행렬 형식</p>
<ul>
<li>float2x2</li>
<li>float3x3</li>
<li>float4x4</li>
<li>또한 float1x1, float1x2, float1x3, float1x4, float2x1, float2x3, float2x4, float3x1, float3x2, float3x4, float4x1, float4x2, float4x3</li>
<li><p>또한 이러한 형식에는 float와 유사한 행렬 확장이 포함됩니다.</p>
<ul>
<li>int, uint, bool</li>
<li>min10float, min16float</li>
<li>min12int, min16int</li>
<li>min16uint</li>
</ul></li>
</ul>
<p>또한 <a href="https://msdn.microsoft.com/library/windows/desktop/bb509623">행렬 형식</a>을 사용하여 행렬을 정의할 수 있습니다.</p>
<p>예: matrix &lt;float, 2, 2&gt; fMatrix = {0.0f, 0.1, 2.1f, 2.2f};</p>
<p>행렬은 float4x4로 정의되는 형식이기도 합니다(typedef matrix &lt;float, 4, 4&gt; matrix;). 자세한 내용은 <a href="https://msdn.microsoft.com/library/windows/desktop/bb509702">사용자 정의 형식</a>을 참조하세요.</p></td>
</tr>
<tr class="even">
<td align="left"><p>float, int, 샘플러에 대한 정밀도 한정자</p>
<ul>
<li><p>highp</p>
<p>이 한정자는 min16float에서 제공하는 것보다 크고 전체 32비트 float보다 작은 최소 정밀도 요구 사항을 제공합니다. HLSL에서 이에 해당하는 내용은 다음과 같습니다.</p>
<p>highp float -&gt; float</p>
<p>highp int -&gt; int</p></li>
<li><p>mediump</p>
<p>이 한정자는 float에 적용되고 int는 HLSL의 min16float 및 min12int에 해당합니다. 가수의 최소 10비트이고, min10float와는 다릅니다.</p></li>
<li><p>lowp</p>
<p>float에 적용되는 이 한정자는 -2~2 범위의 부동 소수점을 제공합니다. HLSL의 min10float에 해당합니다.</p></li>
</ul></td>
<td align="left"><p>정밀도 형식</p>
<ul>
<li>min16float: 최소 16 비트 부동 소수점 값</li>
<li><p>min10float</p>
<p>최소 고정 소수점 부호 있는 2.8비트 값(정수의 2비트 및 소수부의 8비트)입니다. 8비트 소수부는 1을 제외하는 대신 1을 포함하여 전체 포함 범위 -2~2를 제공할 수 있습니다.</p></li>
<li>min16int: 최소 16비트 부호 있는 정수</li>
<li><p>min12int: 최소 12비트 부호 있는 정수</p>
<p>이 형식은 10Level9(<a href="https://msdn.microsoft.com/library/windows/desktop/ff476876">9_x 기능 수준</a>)용이며, 여기서 정수는 부동 소수점 숫자로 표현됩니다. 이 정밀도는 정수를 16비트 부동 소수점 숫자로 에뮬레이트할 때 얻을 수 있는 정밀도입니다.</p></li>
<li>min16uint: 최소 16비트 부호 없는 정수</li>
</ul>
<p>자세한 내용은 <a href="https://msdn.microsoft.com/library/windows/desktop/bb509646">스칼라 형식</a> 및 <a href="https://msdn.microsoft.com/library/windows/desktop/hh968108">HLSL 최소 정밀도 사용</a>을 참조하세요.</p></td>
</tr>
<tr class="odd">
<td align="left">sampler2D</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/ff471525">Texture2D</a></td>
</tr>
<tr class="even">
<td align="left">samplerCube</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/bb509700">TextureCube</a></td>
</tr>
</tbody>
</table>

 

## <a name="porting-glsl-pre-defined-global-variables-to-hlsl"></a>GLSL 미리 정의된 전역 변수를 HLSL로 포팅


이 표를 사용하여 GLSL 미리 정의된 전역 변수를 HLSL로 포팅합니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">GLSL 미리 정의된 전역 변수</th>
<th align="left">HLSL 의미 체계</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>gl_Position</strong></p>
<p>이 변수는 <strong>vec4</strong> 형식입니다.</p>
<p>꼭짓점 위치</p>
<p>예 - gl_Position = position;</p></td>
<td align="left"><p>SV_Position</p>
<p>Direct3D 9의 POSITION</p>
<p>이 의미 체계는 <strong>float4</strong> 형식입니다.</p>
<p>꼭짓점 셰이더 출력</p>
<p>꼭짓점 위치</p>
<p>예 - float4 vPosition : SV_Position;</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_PointSize</strong></p>
<p>이 변수는 <strong>float</strong> 형식입니다.</p>
<p>포인트 크기</p></td>
<td align="left"><p>PSIZE</p>
<p>Direct3D 9를 대상으로 하지 않는 경우 의미 없음</p>
<p>이 의미 체계는 <strong>float</strong> 형식입니다.</p>
<p>꼭짓점 셰이더 출력</p>
<p>포인트 크기</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_FragColor</strong></p>
<p>이 변수는 <strong>vec4</strong> 형식입니다.</p>
<p>조각 색</p>
<p>예 - gl_FragColor = vec4(colorVarying, 1.0);</p></td>
<td align="left"><p>SV_Target</p>
<p>Direct3D 9의 COLOR</p>
<p>이 의미 체계는 <strong>float4</strong> 형식입니다.</p>
<p>픽셀 셰이더 출력</p>
<p>픽셀 색</p>
<p>예 - float4 Color[4] : SV_Target;</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FragData [n]</strong></p>
<p>이 변수는 <strong>vec4</strong> 형식입니다.</p>
<p>색 첨부 n의 조각 색</p></td>
<td align="left"><p>SV_Target[n]</p>
<p>이 의미 체계는 <strong>float4</strong> 형식입니다.</p>
<p>n 렌더링 대상에 저장되는 픽셀 셰이더 출력 값, 여기서 0 &lt;= n &lt;= 7입니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_FragCoord</strong></p>
<p>이 변수는 <strong>vec4</strong> 형식입니다.</p>
<p>프레임 버퍼 내의 조각 위치</p></td>
<td align="left"><p>SV_Position</p>
<p>Direct3D 9에서는 사용할 수 없음</p>
<p>이 의미 체계는 <strong>float4</strong> 형식입니다.</p>
<p>픽셀 셰이더 입력</p>
<p>화면 공간 좌표</p>
<p>예 - float4 screenSpace : SV_Position</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FrontFacing</strong></p>
<p>이 변수는 <strong>bool</strong> 형식입니다.</p>
<p>조각이 앞면 기본 요소에 속하는지 여부를 결정합니다.</p></td>
<td align="left"><p>SV_IsFrontFace</p>
<p>Direct3D 9의 VFACE</p>
<p>SV_IsFrontFace는 <strong>bool</strong> 형식입니다.</p>
<p>VFACE는 <strong>float</strong> 형식입니다.</p>
<p>픽셀 셰이더 입력</p>
<p>기본 요소 방향</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_PointCoord</strong></p>
<p>이 변수는 <strong>vec2</strong> 형식입니다.</p>
<p>포인트 내의 조각 위치(포인트 래스터화에만 해당)</p></td>
<td align="left"><p>SV_Position</p>
<p>Direct3D 9의 VPOS</p>
<p>SV_Position은 <strong>float4</strong> 형식입니다.</p>
<p>VPOS는 <strong>float2</strong> 형식입니다.</p>
<p>픽셀 셰이더 입력</p>
<p>화면 공간에서의 픽셀 또는 샘플 위치</p>
<p>예 - float4 pos : SV_Position</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FragDepth</strong></p>
<p>이 변수는 <strong>float</strong> 형식입니다.</p>
<p>깊이 버퍼 데이터</p></td>
<td align="left"><p>SV_Depth</p>
<p>Direct3D 9의 DEPTH</p>
<p>SV_Depth는 <strong>float</strong> 형식입니다.</p>
<p>픽셀 셰이더 출력</p>
<p>깊이 버퍼 데이터</p></td>
</tr>
</tbody>
</table>

 

의미 체계를 사용하여 꼭짓점 셰이더 입력 및 픽셀 셰이더 입력에 대한 위치, 색 등을 지정합니다. 입력 레이아웃의 의미 체계 값을 꼭짓점 셰이더 입력과 일치시켜야 합니다. 예는 [HLSL로의 GLSL 변수 포팅 예](#examples-of-porting-glsl-variables-to-hlsl)를 참조하세요. HLSL 의미 체계에 대한 자세한 내용은 [의미 체계](https://msdn.microsoft.com/library/windows/desktop/bb509647)를 참조하세요.

## <a name="examples-of-porting-glsl-variables-to-hlsl"></a>HLSL로의 GLSL 변수 포팅 예


여기서는 OpenGL/GLSL 코드에서 GLSL 변수를 사용하는 예와 Direct3D/HLSL 코드에서의 동일한 예를 보여 줍니다.

### <a name="uniform-attribute-and-varying-in-glsl"></a>GLSL의 uniform, attribute 및 varying

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

GLSL 꼭짓점 셰이더 코드

``` syntax
//The shader entry point is the main method.
void main()
{
colorVarying = color; //Use the varying variable to pass the color to the fragment shader
gl_Position = position; //Copy the position to the gl_Position pre-defined global variable
}
```

GLSL 조각 셰이더 코드

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

다음은 데이터를 HLSL 꼭짓점 셰이더로 전달한 다음 해당 데이터가 픽셀 셰이더로 흐르는 방식을 보여 주는 예입니다. 앱 코드에서 꼭짓점 및 상수 버퍼를 정의합니다. 그런 다음 꼭짓점 셰이더 코드에서 [cbuffer](https://msdn.microsoft.com/library/windows/desktop/bb509581)로 상수 버퍼를 정의하고 꼭짓점별 데이터와 픽셀 셰이더 입력 데이터를 저장합니다. 여기서는 **VertexShaderInput** 및 **PixelShaderInput**이라는 구조를 사용합니다.

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

HLSL 꼭짓점 셰이더 코드

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

## <a name="examples-of-porting-opengl-rendering-code-to-direct3d"></a>Direct3D로의 OpenGL 렌더링 코드 포팅 예


여기서는 OpenGL ES 2.0 코드의 렌더링 예와 Direct3D 11 코드의 동일한 예를 보여 줍니다.

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


* [OpenGL ES 2.0에서 Direct3D 11로 포팅](port-from-opengl-es-2-0-to-directx-11-1.md)

 

 




