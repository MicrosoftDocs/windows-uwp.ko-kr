---
title: 기본 토폴로지
description: Direct3D는 점 목록, 선 목록 및 삼각형 스트립 등 파이프라인이 파이프라인에서 해석 되 고 렌더링 되는 방식을 정의 하는 몇 가지 기본 토폴로지를 지원 합니다.
ms.assetid: 7AA5A4A2-0B7C-431D-B597-684D58C02BA5
keywords:
- 기본 토폴로지
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 45abb0c356b4ee6923bf6edd0b462f568749de5e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156337"
---
# <a name="primitive-topologies"></a>기본 토폴로지


Direct3D는 점 목록, 선 목록 및 삼각형 스트립 등 파이프라인이 파이프라인에서 해석 되 고 렌더링 되는 방식을 정의 하는 몇 가지 기본 토폴로지를 지원 합니다.

## <a name="span-idprimitive_typesspanspan-idprimitive_typesspanspan-idprimitive_typesspanbasic-primitive-topologies"></a><span id="Primitive_Types"></span><span id="primitive_types"></span><span id="PRIMITIVE_TYPES"></span>기본 기본 토폴로지


다음 기본 기본 토폴로지 (또는 기본 형식)가 지원 됩니다.

-   [점 목록](point-lists.md)
-   [선 목록](line-lists.md)
-   [선 스트립](line-strips.md)
-   [삼각형 목록](triangle-lists.md)
-   [삼각형 스트립](triangle-strips.md)

각 기본 형식에 대 한 시각화는이 항목의 뒷부분에 있는 [굴곡 방향 및 선행 꼭 짓 점 위치](#winding-direction-and-leading-vertex-positions)에서 다이어그램을 참조 하세요.

[입력 어셈블러 (IA) 단계](input-assembler-stage--ia-.md) 는 꼭 짓 점 및 인덱스 버퍼에서 데이터를 읽고, 데이터를 이러한 기본 형식으로 어셈블한 후 나머지 파이프라인 단계로 데이터를 보냅니다.

## <a name="span-idprimitive_adjacencyspanspan-idprimitive_adjacencyspanspan-idprimitive_adjacencyspanprimitive-adjacency"></a><span id="Primitive_Adjacency"></span><span id="primitive_adjacency"></span><span id="PRIMITIVE_ADJACENCY"></span>기본 인접


모든 Direct3D 기본 형식 (지점 목록 제외)은 두 가지 버전으로 사용할 수 있습니다. 즉, 인접 한 기본 형식으로, 인접 하지 않은 기본 형식은 하나입니다. 인접 한 기본 형식에는 주변 꼭 짓 점 중 일부가 포함 되 고, 인접 하지 않은 기본 형식에는 대상 기본 형식의 꼭 짓 점이 포함 됩니다. 예를 들어 줄 목록 기본 형식에는 인접을 포함 하는 해당 줄 목록 기본 형식이 있습니다.

인접 한 기본 형식은 기 하 도형에 대 한 자세한 정보를 제공 하기 위한 것 이며 기 하 도형 셰이더를 통해서만 볼 수 있습니다. 인접은 실루엣 검색, 섀도 볼륨 입체 면을 사용 하는 기 하 도형 셰이더에 유용 합니다.

예를 들어 인접으로 삼각형 목록을 그려야 한다고 가정 합니다. 36 교점 (인접)이 포함 된 삼각형 목록은 완성 된 기본 형식 6 개를 생성 합니다. 인접 (줄 스트립 제외)을 포함 하는 기본 형식에는 인접 한 꼭 짓 점이 없는 동일한 기본 형식의 꼭 짓 점 수가 두 배 이상 포함 됩니다. 여기서 각 추가 꼭 짓 점은 인접 한

## <a name="span-idwinding_direction_and_leading_vertex_positionsspanspan-idwinding_direction_and_leading_vertex_positionsspanspan-idwinding_direction_and_leading_vertex_positionsspanspan-idwinding-direction-and-leading-vertex-positionsspanwinding-direction-and-leading-vertex-positions"></a><span id="Winding_Direction_and_Leading_Vertex_Positions"></span><span id="winding_direction_and_leading_vertex_positions"></span><span id="WINDING_DIRECTION_AND_LEADING_VERTEX_POSITIONS"></span><span id="winding-direction-and-leading-vertex-positions"></span>권선 방향 및 선행 꼭 짓 점 위치


다음 그림에 표시 된 것 처럼 선행 꼭 짓 점은 기본에서 인접해 있지 않은 첫 번째 꼭 짓 점입니다. 각각 다른 기본 형식에 사용 되는 경우 기본 형식에는 여러 개의 선행 꼭지점이 정의 될 수 있습니다.

-   인접 한 삼각형 스트립의 경우 선행 꼭 짓 점은 0, 2, 4, 6 등입니다.
-   인접이 있는 줄 스트립의 경우 선행 꼭 짓 점은 1, 2, 3 등입니다.
-   반면에 인접 한 기본 형식에는 선행 꼭지점이 없습니다.

다음 그림에서는 입력 어셈블러에서 생성할 수 있는 모든 기본 형식에 대 한 꼭 짓 점 순서를 보여 줍니다.

![기본 형식에 대 한 꼭 짓 점 순서 다이어그램](images/d3d10-primitive-topologies.png)

위의 그림에 나와 있는 기호는 다음 표에 설명 되어 있습니다.

| 기호                                                                                   | 이름              | 설명                                                                         |
|------------------------------------------------------------------------------------------|-------------------|-------------------------------------------------------------------------------------|
| ![꼭 짓 점의 기호](images/d3d10-primitive-topologies-vertex.png)                     | 꼭짓점            | 3D 공간의 점입니다.                                                                |
| ![권선 방향 기호](images/d3d10-primitive-topologies-winding-direction.png) | 권선 방향 | 기본 형식을 어셈블하는 데 사용할 꼭 짓 점입니다. 시계 방향 또는 시계 반대 방향으로 지정할 수 있습니다. |
| ![선행 꼭 짓 점의 기호](images/d3d10-primitive-topologies-leading-vertex.png)       | 선행 꼭 짓 점    | 비상수 데이터를 포함 하는 기본 형식에서 인접 하지 않은 첫 번째 꼭 짓 점입니다.       |

 

## <a name="span-idgenerating_multiple_stripsspanspan-idgenerating_multiple_stripsspanspan-idgenerating_multiple_stripsspangenerating-multiple-strips"></a><span id="Generating_Multiple_Strips"></span><span id="generating_multiple_strips"></span><span id="GENERATING_MULTIPLE_STRIPS"></span>여러 구획 생성


구획 자르기를 통해 여러 개의 스트립을 생성할 수 있습니다. [RestartStrip](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-so-restartstrip) HLSL 함수를 명시적으로 호출 하거나 인덱스 버퍼에 특수 인덱스 값을 삽입 하 여 스트립 잘라내기를 수행할 수 있습니다. 이 값은-1 이며,이 값은 32 비트 인덱스의 경우 0xffffffff이 고 16 비트 인덱스의 경우 0xffff입니다.

인덱스-1은 현재 스트립의 명시적 ' 잘라내기 ' 또는 ' 다시 시작 '을 나타냅니다. 이전 인덱스는 이전 기본 또는 스트립을 완료 하 고 다음 인덱스는 새 기본 또는 스트립을 시작 합니다.

여러 스트립을 생성 하는 방법에 대 한 자세한 내용은 [GS (Geometry Shader) 단계](geometry-shader-stage--gs-.md)를 참조 하세요.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[IA(입력 어셈블러) 단계](input-assembler-stage--ia-.md)

[그래픽 파이프라인](graphics-pipeline.md)

 

 