---
title: "그래픽 파이프라인"
description: "Direct3D 그래픽 파이프라인은 실시간 게임 응용 프로그램의 그래픽을 생성하도록 설계되었습니다. 데이터는 각각의 구성 가능하거나 프로그래밍 가능한 단계를 통해 입력에서 출력으로 흐릅니다."
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- "그래픽 파이프라인"
- "파이프라인 단계"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: af583deb51b93bffc05c4371466fa8412bb0476d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="graphics-pipeline"></a>그래픽 파이프라인


Direct3D 그래픽 파이프라인은 실시간 게임 응용 프로그램의 그래픽을 생성하도록 설계되었습니다. 데이터는 각각의 구성 가능하거나 프로그래밍 가능한 단계를 통해 입력에서 출력으로 흐릅니다.

모든 단계는 Direct3D API를 사용하여 구성할 수 있습니다. 공통 셰이더 코어(모서리가 둥근 사각형 블록)를 사용하는 단계는 [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561) 프로그래밍 언어를 사용하여 프로그래밍할 수 있습니다. 이를 통해 파이프라인이 매우 유연하고 적응력이 높아집니다.

가장 일반적으로 사용되는 것은 VS(꼭짓점 셰이더)와 PS(픽셀 셰이더) 단계입니다. 이러한 셰이더 단계를 제공하지 않으면 기본 무동작, 통과 꼭짓점 및 픽셀 셰이더가 사용됩니다.

![direct3d 11 프로그래밍 가능 파이프라인의 데이터 흐름 다이어그램](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>입력 어셈블러 단계

 

| | |
|-|-|
|목적|[IA(입력 어셈블러) 단계](input-assembler-stage--ia-.md)는 의미 체계 ID를 포함해 삼각형, 선, 점과 같은 기본 요소 및 인접 데이터를 파이프라인에 제공하여 아직 처리되지 않은 기본 요소에 대한 처리 단계를 줄임으로써 셰이더의 효율성을 높입니다.|
|입력|메모리의 사용자가 채운 버퍼에서 가져온 기본 요소 데이터(삼각형, 선 및/또는 점). 인접 데이터도 될 수 있습니다. 삼각형은 각 삼각형당 3개의 꼭짓점, 그리고 인접 데이터에 대한 3개의 꼭짓점이 있을 수 있습니다.|
|출력|시스템 생성 값(예: 기본 요소 ID, 인스턴스 ID 또는 꼭지점 ID)이 연결된 기본 요소.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Input Assembler (IA) stage](input-assembler-stage--ia-.md) supplies primitive and adjacency data to the pipeline, such as triangles, lines and points, including semantics IDs to help make shaders more efficient by reducing processing to primitives that haven't already been processed.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"> Primitive data (triangles, lines, and/or points), from user-filled buffers in memory. And possibly adjacency data. A triangle would be 3 vertices for each triangle and possibly 3 vertices for adjacency data per triangle.  </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Primitives with attached system-generated values (such as a primitive ID, an instance ID, or a vertex ID). </td>
</tr>
</tbody>
</table>
 -->



## <a name="vertex-shader-stage"></a>꼭짓점 셰이더 단계
 

| | |
|-|-|
|목표|[VS(꼭짓점 셰이더) 단계](vertex-shader-stage--vs-.md)는 꼭짓점을 처리하며, 일반적으로 변환, 스킨 지정, 조명 등의 작업을 수행합니다. 꼭짓점 셰이더는 단일 입력 꼭짓점을 사용하여 단일 출력 꼭짓점을 생성합니다. 변환, 스킨 지정, 모핑, 꼭짓점별 조명과 같은 개별 꼭짓점별 작업입니다.|
|입력|VertexID 및 InstanceID 시스템 생성 값이 있는 단일 꼭짓점. 각 꼭짓점 셰이더 입력 꼭짓점은 최대 16개의 32비트 벡터(각각 최대 4개의 구성 요소)로 구성될 수 있습니다.|
|출력|단일 꼭짓점. 각 출력 꼭짓점은 16개의 32비트 4-구성 요소 벡터로 구성될 수 있습니다.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Vertex Shader (VS) stage](vertex-shader-stage--vs-.md) processes vertices, typically performing operations such as transformations, skinning, and lighting. A vertex shader takes a single input vertex and produces a single output vertex. Individual per-vertex operations, such as:
<ul>
<li>Transformations</li>
<li>Skinning</li>
<li>Morphing</li>
<li>Per-vertex lighting</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">A single vertex, with VertexID and InstanceID system-generated values. Each vertex shader input vertex can be comprised of up to 16 32-bit vectors (up to 4 components each).</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">A single vertex. Each output vertex can be comprised of as many as 16 32-bit 4-component vectors.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="hull-shader-stage"></a>헐 셰이더 단계
 

| | |
|-|-|
|목적|[HS(헐 셰이더) 단계](hull-shader-stage--hs-.md)는 공간 분할 단계 중 하나로, 모델의 한 표면을 효율적으로 많은 삼각형으로 나눕니다. 헐 셰이더는 패치당 한 번 호출되고, 하위 표면을 정의하는 입력 제어점을 패치를 만드는 제어점으로 변환합니다. 또한 TS(분할기) 단계 및 DS(도메인 셰이더) 단계에 데이터를 제공하기 위한 일부 패치별 계산도 수행합니다.|
|입력|하위 표면을 함께 정의하는 1에서 32까지의 입력 제어점.|
|출력|패치를 함께 구성하는 1에서 32까지의 출력 제어점. 헐 셰이더는 제어점의 수, 패치 면의 유형, 공간 분할 시 분할 유형을 포함하여 TS(분할기) 단계의 상태를 선언합니다.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Hull Shader (HS) stage](hull-shader-stage--hs-.md) is one of the tessellation stages, which efficiently break up a single surface of a model into many triangles. A hull shader is invoked once per patch, and it transforms input control points that define a low-order surface into control points that make up a patch. It also does some per-patch calculations to provide data for the Tessellator (TS) stage and the Domain Shader (DS) stage.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Between 1 and 32 input control points, which together define a low-order surface. </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Between 1 and 32 output control points, which together make up a patch. The hull shader declares the state for the Tessellator (TS) stage, including the number of control points, the type of patch face, and the type of partitioning to use when tessellating. </td>
</tr>
</tbody>
</table>
-->

## <a name="tessellator-stage"></a>분할기 단계
 

| | |
|-|-|
|목표|[TS(분할기) 단계](tessellator-stage--ts-.md)에서는 기하 도형 패치를 나타내는 도메인의 샘플링 패턴을 만들고, 이러한 샘플을 연결하는 더 작은 개체(삼각형, 점 또는 선)의 집합을 생성합니다.|
|입력|분할기는 헐 셰이더 단계로부터 전달되는 공간 분할 요소(도메인을 얼마나 세부적으로 공간 분할할지 지정) 및 분할 유형(패치를 분할하는 데 사용할 알고리즘을 지정)을 사용하여 패치당 한 번 작동합니다. |
|출력|분할기는 uv(및 선택적으로 w) 좌표 및 표면 토폴로지를 도메인 셰이더 단계로 출력합니다.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Tessellator (TS) stage](tessellator-stage--ts-.md) creates a sampling pattern of the domain that represents the geometry patch and generates a set of smaller objects (triangles, points, or lines) that connect these samples.   </td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"> The tessellator operates once per patch using the tessellation factors (which specify how finely the domain will be tessellated) and the type of partitioning (which specifies the algorithm used to slice up a patch) that are passed in from the hull-shader stage. </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The tessellator outputs uv (and optionally w) coordinates and the surface topology to the domain-shader stage.     </td>
</tr>
</tbody>
</table>
 -->

## <a name="domain-shader-stage"></a>도메인 셰이더 단계
 

| | |
|-|-|
|목적|[DS(도메인 셰이더) 단계](domain-shader-stage--ds-.md)는 출력 패치에서 세분화된 지점의 꼭지점 위치를 계산합니다. 이 단계에서 각 도메인 샘플에 해당하는 꼭지점 위치도 계산합니다. 도메인 셰이더는 분할기 단계 출력 지점당 한 번씩 실행되며, 헐 셰이더 출력 패치 및 출력 패치 상수, 분할기 단계 출력 UV 좌표에 대한 읽기 전용 액세스 권한을 가집니다.|
|입력|도메인 셰이더는 [HS(헐 셰이더) 단계](hull-shader-stage--hs-.md)의 출력 제어점을 사용합니다. 헐 셰이더 출력에는 제어점, 패치 상수 데이터, 공간 분할 요소(공간 분할 요소에는 고정 함수 분할기에서 사용하는 값뿐 아니라 원시 값, 예를 들어 기하 모핑을 용이하게 하는 정수 공간 분할로 반올림하기 전의 값을 포함할 수 있음)를 포함합니다. 도메인 셰이더는 [TS(분할기) 단계](tessellator-stage--ts-.md)의 출력 좌표당 한 번씩 호출됩니다.|
|출력|DS(도메인 셰이더) 단계는 출력 패치의 세분화된 지점의 꼭지점 위치를 출력합니다.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Domain Shader (DS) stage](domain-shader-stage--ds-.md) calculates the vertex position of a subdivided point in the output patch; it calculates the vertex position that corresponds to each domain sample. A domain shader is run once per tessellator stage output point and has read-only access to the hull shader output patch and output patch constants, and the tessellator stage output UV coordinates.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><ul>
<li>A domain shader consumes output control points from the [Hull Shader (HS) stage](hull-shader-stage--hs-.md). The hull shader outputs include:
<ul>
<li>Control points.</li>
<li>Patch constant data.</li>
<li>Tessellation factors. The tessellation factors can include the values used by the fixed-function tessellator as well as the raw values (before rounding by integer tessellation, for example), which facilitates geomorphing, for example.</li>
</ul></li>
<li>A domain shader is invoked once per output coordinate from the [Tessellator (TS) stage](tessellator-stage--ts-.md).</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Domain Shader (DS) stage outputs the vertex position of a subdivided point in the output patch.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="geometry-shader-stage"></a>기하 도형 셰이더 단계
 

| | |
|-|-|
|목적|[GS(기하 도형 셰이더) 단계](geometry-shader-stage--gs-.md)는 기본 요소인 삼각형, 선, 점 전체를 인접한 꼭짓점과 함께 처리합니다. 이 단계는 기하 도형 확장 및 축소를 지원합니다. 이는 점 스프라이트 확장, 동적 입자 시스템, 털/핀 생성, 섀도 볼륨 생성, 단일 패스 큐브맵으로 렌더링, 기본 요소별 재질 바꾸기, 기본 요소별 재질 설정을 포함한 알고리즘에 유용합니다. 여기에는 무게 중심 좌표를 기본 요소 데이터로 생성하는 기능이 포함되어 있어 픽셀 셰이더가 사용자 지정 특성 보간을 수행할 수 있습니다. |
|입력|한 꼭짓점에서만 작동하는 꼭짓점 셰이더와 달리, 기하 도형 셰이더의 입력은 전체 기본 요소에 대한 꼭짓점(삼각형의 세 꼭짓점, 선의 두 꼭짓점, 점의 한 꼭짓점)입니다.|
|출력|GS(기하 도형 셰이더) 단계는 선택한 단일 토폴로지를 형성하는 여러 꼭짓점을 출력할 수 있습니다. 사용 가능한 기하 도형 셰이더 출력 토폴로지는 <strong>tristrip</strong>, <strong>linestrip</strong> 및 <strong>pointlist</strong>입니다. 내보내는 기본 요소의 숫자는 기하 도형 셰이더 호출마다 자유롭게 변할 수 있으나, 내보낼 수 있는 최대 꼭짓점의 수는 고정적으로 선언될 수 있습니다. 기하 도형 셰이더 호출에서 내보내는 스트립 길이는 임의의 길이가 될 수 있으며, 새 스트립은 [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL 함수를 통해 만들 수 있습니다.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Geometry Shader (GS) stage](geometry-shader-stage--gs-.md) processes entire primitives: triangles, lines, and points, along with their adjacent vertices. It is useful for algorithms including Point Sprite Expansion, Dynamic Particle Systems, and Shadow Volume Generation. It supports geometry amplification and de-amplification.
<ul>
<li>Point Sprite Expansion</li>
<li>Dynamic Particle Systems</li>
<li>Fur/Fin Generation</li>
<li>Shadow Volume Generation</li>
<li>Single Pass Render-to-Cubemap</li>
<li>Per-Primitive Material Swapping</li>
<li>Per-Primitive Material Setup, including generation of barycentric coordinates as primitive data so that a pixel shader can perform custom attribute interpolation.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Unlike vertex shaders, which operate on a single vertex, the geometry shader's inputs are the vertices for a full primitive (three vertices for triangles, two vertices for lines, or single vertex for point).</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Geometry Shader (GS) stage is capable of outputting multiple vertices forming a single selected topology. Available geometry shader output topologies are <strong>tristrip</strong>, <strong>linestrip</strong>, and <strong>pointlist</strong>. The number of primitives emitted can vary freely within any invocation of the geometry shader, though the maximum number of vertices that could be emitted must be declared statically. Strip lengths emitted from a geometry shader invocation can be arbitrary, and new strips can be created via the [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL function.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="stream-output-stage"></a>스트림 출력 단계
 

| | |
|-|-|
|목적|[SO(스트림 출력) 단계](stream-output-stage--so-.md)에서는 이전의 활성 단계에서 하나 이상의 메모리 내 버퍼로 꼭짓점 데이터를 연속적으로 출력(또는 스트리밍)합니다. 메모리로 스트림 출력된 데이터는 입력 데이터로 다시 파이프라인으로 재순환하거나 CPU에서 다시 읽을 수 있습니다.|
|출력|이전 파이프라인 단계의 꼭짓점 데이터.|
|출력|SO(스트림 출력) 단계에서는 이전 활성 단계(예: GS(기하 도형 셰이더) 단계)의 꼭짓점 데이터를 하나 이상의 메모리 내 버퍼로 연속적으로 출력(또는 스트리밍)합니다. GS(기하 도형 셰이더) 단계가 비활성 상태이고 SO(스트림 출력) 단계가 활성 상태인 경우, DS(도메인 셰이더) 단계(또는 DS 단계도 비활성 상태일 경우 VS(꼭짓점 셰이더) 단계)의 꼭짓점 데이터를 메모리 내 버퍼로 연속적으로 출력합니다.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Stream Output (SO) stage](stream-output-stage--so-.md) continuously outputs (or streams) vertex data from the previous active stage to one or more buffers in memory. Data streamed out to memory can be recirculated back into the pipeline as input data, or read-back from the CPU.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Vertex data from a previous pipeline stage.   </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Stream Output (SO) stage continuously outputs (or streams) vertex data from the previous active stage, such as the Geometry Shader (GS) stage, to one or more buffers in memory. If the Geometry Shader (GS) stage is inactive, and the Stream Output (SO) stage is active, it continuously outputs vertex data from the Domain Shader (DS) stage to buffers in memory (or if the DS is also inactive, from the Vertex Shader (VS) stage).</td>
</tr>
</tbody>
</table>
-->

## <a name="rasterizer-stage"></a>래스터라이저 단계
 

| | |
|-|-|
|목적|[RS(래스터라이저) 단계](rasterizer-stage--rs-.md)는 뷰에 없는 기본 요소를 잘라내고, PS(픽셀 셰이더) 단계를 위한 기본 요소를 준비하고, 픽셀 셰이더를 호출하는 방법을 결정합니다. 실시간 3D 그래픽을 표시하기 위해 벡터 정보(형태 또는 기본 요소로 구성)를 래스터 이미지(픽셀로 구성)로 변환합니다.|
|입력|래스터화 단계에 들어오는 꼭짓점(x, y, z, w)은 동종 클립 공간에 있는 것으로 가정됩니다. 이 좌표 공간에서 X축은 오른쪽, Y축은 위쪽, Z축은 카메라에서 먼 쪽을 가리킵니다.|
|출력|렌더링해야 하는 실제 픽셀입니다. 픽셀 셰이더에서 보간에 사용할 일부 꼭짓점 특성도 포함합니다.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Rasterizer (RS) stage](rasterizer-stage--rs-.md) clips primitives that aren't in view, prepares primitives for the Pixel Shader (PS) stage, and determines how to invoke pixel shaders. Converts vector information (composed of shapes or primitives) into a raster image (composed of pixels) for the purpose of displaying real-time 3D graphics.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Vertices (x,y,z,w) coming into the Rasterizer stage are assumed to be in homogeneous clip-space. In this coordinate space the X axis points right, Y points up and Z points away from camera.</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The actual pixels that need to be rendered. Includes some vertex attributes for use in interpolation by the pixel Shader.</td>
</tr>
</tbody>
</table>
 -->

## <a name="pixel-shader-stage"></a>픽셀 셰이더 단계
 

| | |
|-|-|
|목적|[PS(픽셀 셰이더) 단계](pixel-shader-stage--ps-.md)는 기본 요소에 대한 보간된 데이터를 받아 색상과 같은 픽셀별 데이터를 생성합니다.|
|입력|파이프라인이 기하 도형 셰이더 없이 구성된 경우, 픽셀 셰이더는 16, 32비트, 4-구성 요소 입력으로 제한됩니다. 그렇지 않은 경우, 최대 32, 32비트, 4-구성 요소 입력을 취할 수 있습니다. 픽셀 셰이더 입력 데이터는 꼭짓점 특성(원근 수정을 포함하거나 원근 수정 없이 보간 가능)을 포함하거나 기본 요소별 상수로 취급할 수 있습니다. 픽셀 셰이더 입력은 선언된 보간 모드에 따라 래스터화되는 기본 요소의 꼭짓점 특성으로부터 보간됩니다. 래스터화 전에 기본 요소가 잘리는 경우, 클리핑 프로세스 도중에도 보간 모드가 적용됩니다. |
|출력|픽셀 셰이더는 최대 8, 32비트, 4-구성 요소 색을 출력할 수 있습니다. 픽셀이 삭제된 경우에는 아무 색도 출력하지 않습니다. 픽셀 셰이더 출력 등록 구성 요소는 사용하기 전에 선언되어야 합니다. 각 등록에는 고유의 출력 쓰기 마스크가 허용됩니다.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Pixel Shader (PS) stage](pixel-shader-stage--ps-.md) receives interpolated data for a primitive and generates per-pixel data such as color.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><p>When the pipeline is configured without a geometry shader, a pixel shader is limited to 16, 32-bit, 4-component inputs. Otherwise, a pixel shader can take up to 32, 32-bit, 4-component inputs.</p>
<p>Pixel shader input data includes vertex attributes (that can be interpolated with or without perspective correction) or can be treated as per-primitive constants. Pixel shader inputs are interpolated from the vertex attributes of the primitive being rasterized, based on the interpolation mode declared. If a primitive gets clipped before rasterization, the interpolation mode is honored during the clipping process as well.</p></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left"><p>A pixel shader can output up to 8, 32-bit, 4-component colors, or no color if the pixel is discarded. Pixel shader output register components must be declared before they can be used; each register is allowed a distinct output-write mask.</p></td>
</tr>
</tbody>
</table>
-->
 

## <a name="output-merger-stage"></a>출력 병합기 단계
 

| | |
|-|-|
|목적|[OM(출력 병합기) 단계](output-merger-stage--om-.md)는 다양한 유형의 출력 데이터(픽셀 셰이더 값, 깊이 및 스텐실 정보)를 렌더링 대상 및 깊이/스텐실 버퍼의 내용과 결합하여 최종 파이프라인 결과를 생성합니다.|
|입력|출력 병합기 입력은 파이프라인 상태, 픽셀 셰이더에서 생성한 픽셀 데이터, 렌더링 대상의 내용 및 깊이/스텐실 버퍼의 내용입니다.|
|출력|최종 렌더링된 픽셀 색입니다.|
| | |


<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Output Merger (OM) stage](output-merger-stage--om-.md) combines various types of output data (pixel shader values, depth and stencil information) with the contents of the render target and depth/stencil buffers to generate the final pipeline result.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><ul>
<li>Pipeline state</li>
<li>The pixel data generated by the pixel shaders</li>
<li>The contents of the render targets</li>
<li>The contents of the depth/stencil buffers.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The final rendered pixel color.</td>
</tr>
</tbody>
</table>


| | |
|-|-|
|Purpose|xxxx|
|Input|yyyy|
|Output|zzzz|
| | |
-->

## <a name="related-topics"></a>관련 항목


[Direct3D 그래픽 학습 가이드](index.md)

[계산 파이프라인](compute-pipeline.md)

 

 
