---
title: 시스템에서 생성된 값 사용
description: 시스템에서 생성 된 값은 사용자 제공 입력 의미 체계를 기반으로 하는 입력 어셈블러 (IA) 단계에서 생성 되어 셰이더 작업에서 특정 효율성을 허용 합니다.
ms.assetid: C7CBA81D-68CA-4E9A-95E3-8185C280C843
keywords:
- 시스템에서 생성된 값 사용
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7217b52c6e9f9882997649c5f843eb119d741e0b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156157"
---
# <a name="span-iddirect3dconceptsusing_system-generated_valuesspanusing-system-generated-values"></a><span id="direct3dconcepts.using_system-generated_values"></span>시스템에서 생성된 값 사용


시스템에서 생성 된 값은 사용자 제공 입력 [의미 체계](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics)를 기반으로 하는 [입력 어셈블러 (IA) 단계](input-assembler-stage--ia-.md) 에서 생성 되어 셰이더 작업에서 특정 효율성을 허용 합니다. 인스턴스 id ( [꼭 짓 점 셰이더 (vs) 단계](vertex-shader-stage--vs-.md)에 표시 됨), 꼭 짓 점 id (vs에 표시 됨) 또는 기본 ID ( [기 하 도형 셰이더 (GS) 스테이지](geometry-shader-stage--gs-.md)픽셀 셰이더 (v) 단계에 표시 됨)와 같은 데이터를 연결 하 여 / [Pixel Shader (PS) stage](pixel-shader-stage--ps-.md)후속 셰이더 단계에서 이러한 시스템 값을 검색 하 여 해당 단계의 처리를 최적화할 수 있습니다.

예를 들어 VS 단계는 인스턴스 ID를 검색 하 여 셰이더에 대 한 추가 꼭 짓 점 데이터를 얻거나 다른 작업을 수행할 수 있습니다. GS 및 PS 단계에서는 기본 ID를 사용 하 여 기본 데이터를 동일한 방식으로 가져올 수 있습니다.

## <a name="span-idvertexidspanspan-idvertexidspanspan-idvertexidspanvertexid"></a><span id="VertexID"></span><span id="vertexid"></span><span id="VERTEXID"></span>VertexID


꼭 짓 점 ID는 각 셰이더 단계에서 각 꼭 짓 점을 식별 하는 데 사용 됩니다. 기본값은 0 인 32 비트 부호 없는 정수입니다. [입력 어셈블러 (IA) 단계](input-assembler-stage--ia-.md)에서 기본 형식을 처리 하는 경우 꼭 짓 점에 할당 됩니다. 버텍스 ID 의미 체계를 셰이더 입력 선언에 연결 하 여 IA 단계에 꼭지점 별 ID를 생성 하도록 알립니다.

IA는 셰이더 단계에서 사용 하기 위해 각 꼭 짓 점에 꼭 짓 점 ID를 추가 합니다. 각 그리기 호출에 대해 꼭 짓 점 ID는 1 씩 증가 합니다. 인덱싱된 그리기 호출에서 개수는 다시 시작 값으로 다시 설정 됩니다. 꼭 짓 점 ID가 오버플로 (2 ³ ² – 1 초과) 이면 0으로 래핑합니다.

모든 기본 형식의 경우 꼭 짓 점에는 인접 한 항목에 관계 없이 연결 된 꼭 짓 점 ID가 있습니다.

## <a name="span-idprimitiveidspanspan-idprimitiveidspanspan-idprimitiveidspanprimitiveid"></a><span id="PrimitiveID"></span><span id="primitiveid"></span><span id="PRIMITIVEID"></span>PrimitiveID


기본 ID는 각 셰이더 단계에서 각 기본 형식을 식별 하는 데 사용 됩니다. 기본값은 0 인 32 비트 부호 없는 정수입니다. [입력 어셈블러 (IA) 단계](input-assembler-stage--ia-.md)에서 기본 형식을 처리할 때 기본에 할당 됩니다. 기본 ID를 생성 하도록 IA 단계에 알리려면 기본 ID 의미 체계를 셰이더 입력 선언에 연결 합니다.

IA 단계에서는 각 기본에 대 한 기본 ID를 [Geometry shader (GS) 단계](geometry-shader-stage--gs-.md) 또는 [버텍스 셰이더 (VS) 단계](vertex-shader-stage--vs-.md) 에서 사용할 수 있도록 각 기본에 추가 합니다. 인덱싱된 각 그리기 호출에 대해 기본 ID는 1 씩 증가 하지만 새 인스턴스가 시작 될 때마다 기본 ID가 0으로 다시 설정 됩니다. 다른 모든 그리기 호출에서는 인스턴스 ID의 값을 변경 하지 않습니다. 인스턴스 ID가 오버플로 (2 ³ ² – 1 초과) 이면 0으로 래핑합니다.

[픽셀 셰이더 (PS) 단계](pixel-shader-stage--ps-.md) 에 기본 ID에 대 한 별도의 입력이 없습니다. 그러나 기본 ID를 지정 하는 모든 픽셀 셰이더 입력은 상수 보간 모드를 사용 합니다.

인접 한 기본 형식에 대 한 기본 ID를 자동으로 생성 하는 것은 지원 되지 않습니다. 인접 한 삼각형 스트립 등 인접이 있는 기본 형식의 경우 인접 하지 않고 삼각형 스트립의 기본 형식 집합과 마찬가지로 기본 형식 (인접 하지 않은 기본 형식)에 대해서만 기본 ID가 유지 됩니다.

## <a name="span-idinstanceidspanspan-idinstanceidspanspan-idinstanceidspaninstanceid"></a><span id="InstanceID"></span><span id="instanceid"></span><span id="INSTANCEID"></span>InstanceID


인스턴스 ID는 각 셰이더 단계에서 현재 처리 중인 기 하 도형 인스턴스를 식별 하는 데 사용 됩니다. 기본값은 0 인 32 비트 부호 없는 정수입니다.

[입력 어셈블러 (IA) 단계](input-assembler-stage--ia-.md) 는 꼭 짓 점 셰이더 입력 선언에 인스턴스 id 의미 체계가 포함 된 경우 각 꼭 짓 점에 인스턴스 id를 추가 합니다. 인덱싱된 각 그리기 호출에 대해 인스턴스 ID는 1 씩 증가 합니다. 다른 모든 그리기 호출에서는 인스턴스 ID의 값이 변경 되지 않습니다. 인스턴스 ID가 오버플로 (2 ³ ² – 1 초과) 이면 0으로 래핑합니다.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>예 들어


다음 그림에서는 [입력 어셈블러 (IA) 단계](input-assembler-stage--ia-.md)에서 시스템 값을 인스턴스화된 삼각형 스트립에 연결 하는 방법을 보여 줍니다.

![인스턴스화된 삼각형 스트립의 시스템 값 그림](images/d3d10-ia-example.png)

다음 표에서는 동일한 삼각형 스트립의 두 인스턴스에 대해 생성 된 시스템 값을 보여 줍니다. 첫 번째 인스턴스 (인스턴스 U)는 파란색으로 표시 되 고 두 번째 인스턴스 (인스턴스 V)는 녹색으로 표시 됩니다. 실선은 기본 형식에서 꼭 짓 점을 연결 하 고, 파선은 인접 한 꼭 짓 점을 연결 합니다.

다음 표에서는 인스턴스 U에 대 한 시스템 생성 값을 보여 줍니다.

| 꼭 짓 점 데이터    | C, U | D, U | E, U | F, U | G, U | H, U | I, U | J, U | K, U | L, U |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |

 

삼각형 스트립 인스턴스 U에는 다음과 같은 시스템 생성 값을 포함 하는 3 개의 삼각형 기본 형식이 있습니다.

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 0   | 0   | 0   |

 

다음 표에서는 인스턴스 V에 대 한 시스템 생성 값을 보여 줍니다.

| 꼭 짓 점 데이터    | C, V | D, V | E, V | F, V | G, V | H, V | I, V | J, V | K, V | L, V |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   |

 

삼각형 스트립 인스턴스 V에는 다음과 같은 시스템 생성 값을 포함 하는 3 개의 삼각형 기본 형식이 있습니다.

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 1   | 1   | 1   |

 

[입력 어셈블러 (IA) 단계](input-assembler-stage--ia-.md) 에서 id (버텍스, primitive 및 instance)가 생성 됩니다. 각 인스턴스에는 고유한 인스턴스 ID가 지정 되어 있습니다. 데이터는 줄무늬의 각 인스턴스를 구분 하는 줄무늬 자르기로 끝납니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[IA(입력 어셈블러) 단계](input-assembler-stage--ia-.md)

 

 