---
title: 시스템 생성 값 사용
description: 시스템 생성 값은 셰이더 작업에서 어느 정도의 효율성을 허용하기 위해 IA(입력 어셈블러) 단계(사용자가 제공한 입력 의미 체계 기반)에서 생성됩니다.
ms.assetid: C7CBA81D-68CA-4E9A-95E3-8185C280C843
keywords:
- 시스템 생성 값 사용
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6efe7aa27721f519ba93052abf2d0e8189f58941
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8741921"
---
# <a name="span-iddirect3dconceptsusingsystem-generatedvaluesspanusing-system-generated-values"></a><span id="direct3dconcepts.using_system-generated_values"></span>시스템 생성 값 사용


시스템 생성 값은 셰이더 작업에서 어느 정도의 효율성을 허용하기 위해 [IA(입력 어셈블러) 단계](input-assembler-stage--ia-.md)(사용자가 제공한 입력 [의미 체계](https://msdn.microsoft.com/library/windows/desktop/bb509647) 기반)에서 생성됩니다. 후속 셰이더 단계에서는 인스턴스 ID([VS(꼭짓점 셰이더) 단계](vertex-shader-stage--vs-.md)에 표시), 꼭짓점 ID(VS에 표시) 또는 기본 ID([GS(기하 도형 셰이더) 단계](geometry-shader-stage--gs-.md)/[PS(픽셀 셰이더) 단계](pixel-shader-stage--ps-.md)에 표시)와 같은 데이터를 연결하여 해당 단계에서 처리를 최적화하는 시스템 값을 찾을 수 있습니다.

예를 들어 VS 단계에서는 인스턴스 ID를 찾아 셰이더에 대한 추가 꼭짓점별 데이터를 확인하거나 다른 작업을 수행할 수 있고, GS 및 PS 단계에서는 같은 방법으로 기본 ID를 사용하여 기본 형식별 데이터를 확인할 수 있습니다.

## <a name="span-idvertexidspanspan-idvertexidspanspan-idvertexidspanvertexid"></a><span id="VertexID"></span><span id="vertexid"></span><span id="VERTEXID"></span>VertexID


꼭짓점 ID는 각 셰이더 단계에서 각 꼭짓점을 식별하는 데 사용됩니다. 서명되지 않은 32비트 정수이며 기본값은 0입니다. [IA(입력 어셈블러) 단계](input-assembler-stage--ia-.md)에서 기본 형식을 처리할 때 꼭짓점에 할당됩니다. 꼭짓점 ID 의미 체계를 셰이더 입력 선언에 연결하여 꼭짓점별 ID를 생성하라고 IA 단계에 알릴 수 있습니다.

IA는 셰이더 단계에서 사용할 수 있도록 각 꼭짓점에 꼭짓점 ID를 추가합니다. 각 그리기 호출의 경우 꼭짓점 ID가 1씩 증가합니다. 인덱싱된 그리기 호출에서 개수가 시작 값으로 초기화됩니다. 꼭짓점 ID가 오버플로되면(2³²–1을 초과) 0으로 래핑됩니다.

모든 기본 형식의 경우 꼭짓점에는 인접성과 관계 없이 연결된 꼭짓점 ID가 있습니다.

## <a name="span-idprimitiveidspanspan-idprimitiveidspanspan-idprimitiveidspanprimitiveid"></a><span id="PrimitiveID"></span><span id="primitiveid"></span><span id="PRIMITIVEID"></span>PrimitiveID


기본 ID는 각 셰이더 단계에서 각 기본 형식을 식별하는 데 사용됩니다. 서명되지 않은 32비트 정수이며 기본값은 0입니다. [IA(입력 어셈블러) 단계](input-assembler-stage--ia-.md)에서 기본 형식을 처리할 때 기본 형식에 할당됩니다. 기본 ID를 생성하라고 IA 단계에 알리려면 기본 ID 의미 체계를 셰이더 입력 선언에 연결합니다.

IA 단계에서는 [GS(기하 도형 셰이더) 단계](geometry-shader-stage--gs-.md) 또는 [VS(꼭짓점 셰이더) 단계](vertex-shader-stage--vs-.md)(IA 단계 이후의 첫 번째 활성 단계)에서 사용할 기본 ID를 각 기본 형식에 추가합니다. 그러나 각각의 인덱싱된 그리기 호출에서 기본 ID는 1씩 증가하지만 새 인스턴스가 시작될 때마다 기본 ID가 0으로 초기화됩니다. 그 외의 그리기 호출은 인스턴스 ID 값을 변경하지 않습니다. 인스턴스 ID가 오버플로되면(2³²–1을 초과) 0으로 래핑됩니다.

[PS(픽셀 셰이더) 단계](pixel-shader-stage--ps-.md)에는 기본 ID에 대한 별도의 입력이 없습니다. 하지만 기본 ID를 지정하는 모든 픽셀 셰이더 입력에서는 상수 보간 모드를 사용합니다.

인접한 기본 형식의 기본 ID를 자동으로 생성하는 기능은 지원되지 않습니다. 인접한 삼각형 스트립과 같은 인접한 기본 형식의 경우 인접하지 않은 삼각형 스트립의 기본 형식 집합처럼 내부 기본 형식(인접하지 않은 기본 형식)에 기본 ID만이 유지됩니다.

## <a name="span-idinstanceidspanspan-idinstanceidspanspan-idinstanceidspaninstanceid"></a><span id="InstanceID"></span><span id="instanceid"></span><span id="INSTANCEID"></span>InstanceID


인스턴스 ID는 각 셰이더 단계에서 현재 처리되고 있는 기하 도형 인스턴스를 식별하는 데 사용됩니다. 서명되지 않은 32비트 정수이며 기본값은 0입니다.

꼭짓점 셰이더 입력 선언이 인스턴스 ID 의미 체계를 포함하는 경우 [IA(입력 어셈블러) 단계](input-assembler-stage--ia-.md)에서는 각 꼭짓점에 인스턴스 ID를 추가합니다. 각각의 인덱싱된 그리기 호출에서 인스턴스 ID는 1씩 증가합니다. 그 외의 그리기 호출은 인스턴스 ID 값을 변경하지 않습니다. 인스턴스 ID가 오버플로되면(2³²–1을 초과) 0으로 래핑됩니다.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>예제


다음 그림에서는 [IA(입력 어셈블러) 단계](input-assembler-stage--ia-.md)에서 인스턴스화된 삼각형 스트립에 시스템 값이 연결되는 방식을 보여 줍니다.

![인스턴스화된 삼각형 스트립에 대한 시스템 값 그림](images/d3d10-ia-example.png)

다음 표는 동일한 삼각형 스트립의 두 인스턴스에 대해 생성되는 시스템 값을 보여줍니다. 첫 번째 인스턴스(인스턴스 U)는 파란색으로 표시되고 두 번째 인스턴스(인스턴스 V)는 녹색으로 표시됩니다. 실선은 기본 형식의 꼭짓점을 연결하고 점선은 인접한 꼭짓점을 연결합니다.

다음 표는 인스턴스 U에 대해 시스템에서 생성하는 값을 보여줍니다.

| 꼭짓점 데이터    | C,U | D,U | E,U | F,U | G,U | H,U | I,U | J,U | K,U | L,U |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |

 

삼각형 스트립 인스턴스 U에는 다음과 같이 시스템에서 생성한 값과 3개의 삼각형 기본 형식이 있습니다.

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 0   | 0   | 0   |

 

다음 표는 인스턴스 V에 대해 시스템에서 생성하는 값을 보여줍니다.

| 꼭짓점 데이터    | C,V | D,V | E,V | F,V | G,V | H,V | I,V | J,V | K,V | L,V |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   |

 

삼각형 스트립 인스턴스 V에는 다음과 같이 시스템에서 생성한 값과 3개의 삼각형 기본 형식이 있습니다.

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 1   | 1   | 1   |

 

[IA(입력 어셈블러) 단계](input-assembler-stage--ia-.md)에서는 ID(꼭짓점, 기본 형식, 인스턴스)를 생성합니다. 그리고 각 인스턴스에는 고유한 인스턴스 ID가 지정됩니다. 데이터는 스트립 잘라내기로 마무리되며, 이를 통해 삼각형 스트립의 각 인스턴스가 분리됩니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[IA(입력 어셈블러) 단계](input-assembler-stage--ia-.md)

 

 




