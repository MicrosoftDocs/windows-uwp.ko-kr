---
title: TS(분할기) 단계
description: TS(분할기) 단계에서는 기하 도형 패치를 나타내는 도메인의 샘플링 패턴을 만들고 이러한 샘플을 연결하는 일단의 더 작은 개체(삼각형, 점 또는 선)를 생성합니다.
ms.assetid: 2F006F3D-5A04-4B3F-8BC7-55567EFCFA6C
keywords:
- TS(분할기) 단계
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7768d63405281d3155affc6c9f09c62568761718
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7702033"
---
# <a name="tessellator-ts-stage"></a>TS(분할기) 단계


TS(분할기) 단계에서는 기하 도형 패치를 나타내는 도메인의 샘플링 패턴을 만들고 이러한 샘플을 연결하는 일단의 더 작은 개체(삼각형, 점 또는 선)를 생성합니다.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>목적 및 사용


다음 다이어그램에는 Direct3D 그래픽 파이프라인의 단계가 나와 있습니다.

![헐 셰이더, 분할기 및 도메인 셰이더 단계가 강조 표시되어 있는 direct3d 11 파이프라인의 다이어그램](images/d3d11-pipeline-stages-tessellation.png)

다음 다이어그램은 공간 분할 단계의 진행을 보여 줍니다.

![공간 분할 진행 다이어그램](images/tess-prog.png)

진행은 저 세부도 분할 표면에서 시작됩니다. 다음 진행은 해당하는 기하 도형 패치, 도메인 샘플 및 이러한 샘플을 연결하는 삼각형을 포함하는 입력 패치를 강조 표시합니다. 마지막 진행은 이러한 샘플에 해당하는 꼭짓점을 강조 표시합니다.

Direct3D 런타임은 GPU에서 저 세부도 분할 표면을 상위 세부도 원형으로 변환하는 공간 분할을 구현하는 세 단계를 지원합니다. 공간 분할은 상위 표면을 렌더링에 적합한 구조로 타일링(또는 분할)합니다.

공간 분할 단계는 함께 작동하여 보다 Direct3D 그래픽 파이프라인에서 세부적으로 렌더링하기 위해 상위 표면(모델을 간단하고 효율적으로 유지)을 많은 삼각형으로 변환합니다.

공간 분할은 GPU를 사용하여 사각형 패치, 삼각형 패치 또는 등치선에서 구성된 표면으로부터 보다 세부적인 표면을 계산합니다. 상위 표면을 개산하기 위해 공간 분할 요소를 사용하여 각 패치가 삼각형, 점 또는 선으로 세분화됩니다. Direct3D 그래픽 파이프라인은 세 파이프라인 단계를 사용하여 공간 분할을 구현합니다.

-   [HS(헐 셰이더) 단계](hull-shader-stage--hs-.md) - 각 입력 패치(사각형, 삼각형 또는 선)에 해당하는 기하 도형 패치(및 패치 상수)를 생성하는 프로그래밍 가능 셰이더 단계입니다.
-   TS(분할기) 단계 - 기하 도형 패치를 나타내고 이러한 샘플을 연결하는 더 작은 개체(삼각형, 점 또는 선)의 집합을 생성하는 도메인의 샘플링 패턴을 만드는 고정 함수 파이프라인 단계입니다.
-   [DS(도메인 셰이더) 단계](domain-shader-stage--ds-.md) - 각 도메인 샘플에 해당하는 꼭짓점 위치를 계산하는 프로그래밍 가능 셰이더 단계입니다.

그래픽 파이프라인은 하드웨어에서 공간 분할을 구현하여 하위 세부도(다각형 수가 적음) 모델을 평가하고 상위 세부도로 렌더링할 수 있습니다. 소프트웨어 공간 분할이 가능하지만 하드웨어에서 구현된 공간 분할은 모델 크기에 시각적 세부 정보를 추가하고 새로 고침 빈도를 마비시키지 않고 놀라울 정도의 시각적 세부 정보를 생성할 수 있습니다(변위 매핑에 대한 지원 포함).

공간 분할의 이점:

-   공간 분할은 메모리 및 대역폭을 크게 절약해주므로 응용 프로그램이 저 해상도 모델에서 보다 세부적인 표면을 렌더링할 수 있습니다. 또한 Direct3D 그래픽 파이프라인에서 구현된 공간 분할 기법은 놀라운 수준의 표면 세부 정보를 생성할 수 있는 변위 매핑도 지원합니다.
-   공간 분할은 즉석에서 계산 수 있는 연속적 또는 뷰 종속적 세부도와 같은 확장 가능 렌더링 기법을 지원합니다.
-   공간 분할은 비용이 높은 계산을 더 낮은 빈도로 수행하여 성능을 개선합니다(하위 세부도 모델에 대해 계산을 수행). 여기에는 사실적 애니메이션을 위한 혼합 형태 또는 모프 대상을 사용한 혼합 계산이나 충돌 탐지 또는 연체 역학을 위한 물리 계산이 포함될 수 있습니다.

Direct3D 그래픽 파이프라인은 하드웨어에서 공간 분할을 구현하여 작업을 CPU에서 GPU로 분산합니다. 그러므로 응용 프로그램이 많은 수의 모프 대상 및/또는 보다 정교한 스킨 지정/변형 모델을 구현하는 경우 성능이 현저히 개선될 수 있습니다.

분할기는 [헐 셰이더](hull-shader-stage--hs-.md)를 파이프라인에 바인딩하여 초기화되는 고정 함수 단계입니다. ([방법: 분할기 단계 초기화](https://msdn.microsoft.com/library/windows/desktop/ff476341) 참조). 분할기 단계의 목적은 도메인(사각형, 삼각형 또는 선)을 여러 개의 더 작은 개체(삼각형, 점 또는 선)로 세분화하는 것입니다. 분할기는 정규(0~1) 좌표계에서 정식 도메인을 타일링합니다. 예를 들어 사각형 도메인은 단위 정사각형으로 공간 분할됩니다.

### <a name="span-idphasesinthetessellatortsstagespanspan-idphasesinthetessellatortsstagespanspan-idphasesinthetessellatortsstagespanphases-in-the-tessellator-ts-stage"></a><span id="Phases_in_the_Tessellator__TS__stage"></span><span id="phases_in_the_tessellator__ts__stage"></span><span id="PHASES_IN_THE_TESSELLATOR__TS__STAGE"></span>TS(분할기) 단계의 작업 단계

TS(분할기) 단계는 2개 작업 단계로 작동합니다.

-   첫 번째 작업 단계에서는 32비트 부동 소수점 연산으로 공간 분할 요소를 처리하여 반올림 문제를 해결하고, 매우 작은 요소를 처리하고, 요소를 환원 및 결합합니다.
-   두 번째 작업 단계에서는 선택한 분할 유형에 따라 점 또는 토폴로지 목록을 생성합니다. 이 작업 단계가 분할기 단계의 핵심 작업으로, 16비트 분수 및 고정 소수점 연산을 사용합니다. 고정 소수점 연산은 허용 가능한 정밀도를 유지하면서도 하드웨어 가속을 사용할 수 있게 해줍니다. 예를 들어 64미터의 너비의 패치가 있을 경우 이 정밀도는 2mm 해상도로 점을 배치할 수 있습니다.

    | 분할 유형 | 범위                       |
    |----------------------|-----------------------------|
    | Fractional\_odd      | \[1...63\]                  |
    | Fractional\_even     | TessFactor range: \[2..64\] |
    | Integer              | TessFactor range: \[1..64\] |
    | Pow2                 | TessFactor range: \[1..64\] |

     

공간 분할은 두 개의 프로그래밍 가능 셰이더 단계 [헐 셰이더](hull-shader-stage--hs-.md) 및 [도메인 셰이더](domain-shader-stage--ds-.md)로 구현됩니다. 이러한 셰이더 단계는 셰이더 모델 5에서 정의되는 HLSL 코드로 프로그래밍되어 있습니다. 셰이더 대상은 hs\_5\_0 및 ds\_5\_0입니다. 제목이 셰이더를 만든 후 하드웨어용 코드가 컴파일된 셰이더에서 추출되어 셰이더가 파이프라인에 바인딩될 때 런타임으로 전달됩니다.

### <a name="span-idenablingdisablingtessellationspanspan-idenablingdisablingtessellationspanspan-idenablingdisablingtessellationspanenablingdisabling-tessellation"></a><span id="Enabling_disabling_tessellation"></span><span id="enabling_disabling_tessellation"></span><span id="ENABLING_DISABLING_TESSELLATION"></span>공간 분할 활성화/비활성화

헐 셰이더를 만들어 헐 셰이더 단계에 바인딩하여(그러면 분할기 단계가 자동으로 설정됨) 공간 분할을 활성화합니다. 또한 공간 분할된 패치에서 최종 꼭짓점 위치를 생성하기 위해 [도메인 셰이더](domain-shader-stage--ds-.md)를 만들어 도메인 셰이더 단계에 바인딩해야 합니다. 공간 분할이 활성화되면 IA(입력 어셈블러) 단계로 입력되는 데이터는 패치 데이터여야 합니다. 입력 어셈블러 토폴로지는 패치 상수 토폴로지여야 합니다.

공간 분할을 사용하지 않으려면 헐 셰이더 및 도메인 셰이더를 **NULL**로 설정합니다. [GS(기하 도형 셰이더) 단계](geometry-shader-stage--gs-.md) 및 [SO(스트림 출력) 단계](stream-output-stage--so-.md)는 헐 셰이더 출력 제어 점 또는 패치 데이터를 읽을 수 없습니다.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>입력


분할기는 헐 셰이더 단계로부터 전달되는 공간 분할 요소(도메인을 얼마나 세부적으로 공간 분할할지 지정) 및 분할 유형(패치를 분할하는 데 사용할 알고리즘을 지정)을 사용하여 패치당 헌 번 작동합니다.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>출력


분할기는 uv(및 선택적으로 w) 좌표 및 표면 토폴로지를 도메인 셰이더 단계로 출력합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[그래픽 파이프라인](graphics-pipeline.md)

 

 




