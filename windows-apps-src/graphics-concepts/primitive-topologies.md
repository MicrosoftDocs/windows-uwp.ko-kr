---
title: 기본 토폴로지
description: Direct3D는 여러 개의 기본 토폴로지를 지원하는데, 이 토폴로지는 점 목록, 선 목록, 삼각형 스트립 등 파이프라인이 꼭짓점을 해석 및 렌더링하는 방식을 정의합니다.
ms.assetid: 7AA5A4A2-0B7C-431D-B597-684D58C02BA5
keywords:
- 기본 토폴로지
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 85d1c41fc10f509f3872fb1e4a0af5fa1e1e7c30
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8885065"
---
# <a name="primitive-topologies"></a>기본 토폴로지


Direct3D는 여러 개의 기본 토폴로지를 지원하는데, 이 토폴로지는 점 목록, 선 목록, 삼각형 스트립 등 파이프라인이 꼭짓점을 해석 및 렌더링하는 방식을 정의합니다.

## <a name="span-idprimitivetypesspanspan-idprimitivetypesspanspan-idprimitivetypesspanbasic-primitive-topologies"></a><span id="Primitive_Types"></span><span id="primitive_types"></span><span id="PRIMITIVE_TYPES"></span>기초적 기본 토폴로지


지원되는 기초적 기본 토폴로지(또는 기본 유형)는 다음과 같습니다.

-   [점 목록](point-lists.md)
-   [선 목록](line-lists.md)
-   [선 스트립](line-strips.md)
-   [삼각형 목록](triangle-lists.md)
-   [삼각형 스트립](triangle-strips.md)

각 기본 유형의 시각화는 이 항목 뒷부분의 [권선 방향과 선행 꼭짓점 위치](#winding-direction-and-leading-vertex-positions)의 다이어그램을 참조하세요.

[IA(입력 어셈블러) 단계](input-assembler-stage--ia-.md)는 꼭짓점 및 인덱스 버퍼의 데이터를 읽고 이 데이터를 이러한 기본 요소로 조합한 다음 데이터를 나머지 파이프라인 단계로 전송합니다.

## <a name="span-idprimitiveadjacencyspanspan-idprimitiveadjacencyspanspan-idprimitiveadjacencyspanprimitive-adjacency"></a><span id="Primitive_Adjacency"></span><span id="primitive_adjacency"></span><span id="PRIMITIVE_ADJACENCY"></span>기본 인접


모든 Direct3D 기본 형식(점 목록 제외)은 인접이 있는 기본 형식과 인접이 없는 기본 형식의 두 가지 버전으로 사용할 수 있습니다. 인접이 있는 기본 요소에는 주변 꼭짓점 일부가 포함되지만 인접이 없는 기본 요소에는 대상 기본 요소의 꼭짓점만 포함됩니다. 예를 들어 선 목록 기본 요소에는 인접이 포함된 해당 선 목록 기본 요소가 있습니다.

인접 기본 요소는 기하 도형에 대한 더 많은 정보를 제공하기 위한 것으로 기하 도형 셰이더를 통해서만 볼 수 있습니다. 인접은 실루엣 감지, 섀도 볼륨 돌출 등을 사용하는 기하 도형 셰이더에 유용합니다.

예를 들어 인접을 사용하여 삼각형 목록을 그리려 한다고 가정해 보세요. 36개의 꼭짓점이 (인접과 함께) 포함된 삼각형 목록은 6개의 완성된 기본 요소를 산출합니다. 인접이 있는 기본 요소(선 스트립 제외)에는 인접이 없는 동등한 기본 요소의 정확히 두 배의 꼭짓점이 포함되는데, 여기서 추가적인 각각의 꼭짓점은 인접 꼭짓점입니다.

## <a name="span-idwindingdirectionandleadingvertexpositionsspanspan-idwindingdirectionandleadingvertexpositionsspanspan-idwindingdirectionandleadingvertexpositionsspanspan-idwinding-direction-and-leading-vertex-positionsspanwinding-direction-and-leading-vertex-positions"></a><span id="Winding_Direction_and_Leading_Vertex_Positions"></span><span id="winding_direction_and_leading_vertex_positions"></span><span id="WINDING_DIRECTION_AND_LEADING_VERTEX_POSITIONS"></span><span id="winding-direction-and-leading-vertex-positions"></span>권선 방향 및 선행 꼭짓점 위치


다음 그림에서 보듯 선행 꼭짓점은 기본 요소에서 첫 번째 비인접 꼭짓점입니다. 각각의 꼭짓점이 서로 다른 기본 요소에 사용되는 한 하나의 기본 형식에서 여러 개의 꼭짓점을 정의할 수 있습니다.

-   인접이 있는 삼각형 스트립의 경우, 선행 꼭짓점은 0, 2, 4, 6, ... 입니다.
-   인접이 있는 선 스트립의 경우, 선행 꼭짓점은 1, 2, 3, ...입니다.
-   반면 인접 기본 요소에는 선행 꼭짓점이 없습니다.

다음 그림은 IA(입력 어셈블러)가 만들 수 있는 모든 기본 형식의 꼭짓점 순서를 보여 줍니다.

![기본 형식의 꼭짓점 순서 다이어그램](images/d3d10-primitive-topologies.png)

앞 그림의 기호는 다음 표에 설명되어 있습니다.

| 기호                                                                                   | 이름              | 설명                                                                         |
|------------------------------------------------------------------------------------------|-------------------|-------------------------------------------------------------------------------------|
| ![꼭짓점 기호](images/d3d10-primitive-topologies-vertex.png)                     | 꼭짓점            | 3D 공간의 점.                                                                |
| ![권선 방향 기호](images/d3d10-primitive-topologies-winding-direction.png) | 권선 방향 | 기본 요소 조합 시 꼭짓점 순서. 시계 방향 또는 반시계 방향일 수 있습니다. |
| ![선행 꼭짓점 기호](images/d3d10-primitive-topologies-leading-vertex.png)       | 선행 꼭짓점    | 상수별 데이터를 포함하는 기본 요소의 첫 번째 비인접 꼭짓점.       |

 

## <a name="span-idgeneratingmultiplestripsspanspan-idgeneratingmultiplestripsspanspan-idgeneratingmultiplestripsspangenerating-multiple-strips"></a><span id="Generating_Multiple_Strips"></span><span id="generating_multiple_strips"></span><span id="GENERATING_MULTIPLE_STRIPS"></span>여러 스트립 생성


스트립 자르기를 통해 여러 개의 스트립을 생성할 수 있습니다. 명시적으로 [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL 함수를 호출하거나 특수 인덱스 값을 인덱스 버퍼에 삽입하여 스트립 자르기를 수행할 수 있습니다. 이 값은 -1로서 32비트 인덱스의 경우에는 0xffffffff, 16비트 인덱스의 경우에는 0xffff입니다.

인덱스 –1은 현재 스트립의 명시적 '잘라내기' 또는 '다시 시작'을 나타냅니다. 이전 인덱스는 이전 기본 요소 또는 스트립을 완성하며, 다음 인덱스는 새 기본 요소 또는 스트립을 시작합니다.

여러 스트립 생성에 대한 자세한 내용은 [GS(기하 도형 셰이더) 단계](geometry-shader-stage--gs-.md)를 참조하세요.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[IA(입력 어셈블러) 단계](input-assembler-stage--ia-.md)

[그래픽 파이프라인](graphics-pipeline.md)

 

 




