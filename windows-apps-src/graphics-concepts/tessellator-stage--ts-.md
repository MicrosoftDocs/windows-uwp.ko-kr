---
title: TS(분할기) 단계
description: Tessellator (TS) 단계는 기 하 도형 패치를 나타내는 도메인의 샘플링 패턴을 만들고 이러한 예제를 연결 하는 작은 개체 (삼각형, 요소 또는 선) 집합을 생성 합니다.
ms.assetid: 2F006F3D-5A04-4B3F-8BC7-55567EFCFA6C
keywords:
- TS(분할기) 단계
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 90dfb8d28be786cb542e72fde5a24bed4de68f78
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167977"
---
# <a name="tessellator-ts-stage"></a>TS(분할기) 단계


Tessellator (TS) 단계는 기 하 도형 패치를 나타내는 도메인의 샘플링 패턴을 만들고 이러한 예제를 연결 하는 작은 개체 (삼각형, 요소 또는 선) 집합을 생성 합니다.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>목적 및 사용


다음 다이어그램은 Direct3D 그래픽 파이프라인의 단계를 강조 표시 합니다.

![선체, tessellator 및 도메인 셰이더 단계를 강조 하는 direct3d 11 파이프라인 다이어그램](images/d3d11-pipeline-stages-tessellation.png)

다음 다이어그램에서는 공간 분할 (tessellation) 단계를 진행 하는 과정을 보여 줍니다.

![공간 분할 진행 다이어그램](images/tess-prog.png)

진행은 낮은 세부 정보 화면으로 시작 합니다. 다음 진행은 이러한 샘플을 연결 하는 해당 geometry 패치, 도메인 샘플 및 삼각형을 사용 하 여 입력 패치를 강조 표시 합니다. 마지막으로 이러한 샘플에 해당 하는 꼭 짓 점이 강조 표시 됩니다.

Direct3D 런타임은 공간 분할을 구현 하는 세 단계를 지원 합니다 .이 경우 낮은 수준의 세부 정보를 GPU의 상위 수준 기본 형식으로 변환 합니다. 최대 공간 분할 타일 (또는 분할)을 렌더링 하기에 적합 한 구조로 정렬 합니다.

공간 분할 단계는 함께 작동 하 여 Direct3D 그래픽 파이프라인 내에서 자세한 렌더링을 위해 더 높은 순서 표면 (모델을 간단 하 고 효율적으로 유지)을 여러 삼각형으로 변환 합니다.

공간 분할은 GPU를 사용 하 여 쿼드 패치, 삼각형 패치 또는 isolines에서 생성 된 표면에서 보다 자세한 표면을 계산 합니다. 높은 순서가 지정 된 표면의 근사치를 위해 각 패치는 공간 분할 (tessellation) 요소를 사용 하는 삼각형, 점이 나 선으로 나뉩니다. Direct3D 그래픽 파이프라인은 세 가지 파이프라인 단계를 사용 하 여 공간 분할을 구현 합니다.

-   [선체 셰이더 (HS) 단계](hull-shader-stage--hs-.md) -각 입력 패치 (쿼드, 삼각형 또는 선)에 해당 하는 기 하 도형 패치 (및 패치 상수)를 생성 하는 프로그래밍 가능한 셰이더 단계입니다.
-   Tessellator (TS) 단계-geometry 패치를 나타내는 도메인의 샘플링 패턴을 만들고 이러한 예제를 연결 하는 작은 개체 (삼각형, 요소 또는 선) 집합을 생성 하는 고정 함수 파이프라인 단계입니다.
-   [도메인 셰이더 (DS) 단계](domain-shader-stage--ds-.md) -각 도메인 샘플에 해당 하는 꼭 짓 점 위치를 계산 하는 프로그래밍 가능한 셰이더 단계입니다.

하드웨어에서 공간 분할을 구현 하 여 그래픽 파이프라인은 하위 수준 (하위 다각형 수) 모델을 평가 하 고 더 높은 세부 정보를 렌더링할 수 있습니다. 소프트웨어 공간 분할을 수행할 수 있지만, 하드웨어에 의해 구현 된 공간 분할은 모델 크기 및 paralyzing 새로 고침 빈도에 시각적 세부 정보를 추가 하지 않고 변위 매핑 지원을 포함 하 여 놀라운 양의 시각적 세부 정보를 생성할 수 있습니다.

공간 분할의 이점:

-   공간 분할은 많은 메모리와 대역폭을 저장 하므로 응용 프로그램은 저해상도 모델에서 더 높은 자세한 표면을 렌더링할 수 있습니다. Direct3D graphics 파이프라인에서 구현 된 공간 분할 기술은 화려한 표면 정보를 생성할 수 있는 변위 매핑도 지원 합니다.
-   공간 분할은 연속으로 계산 될 수 있는 연속 또는 뷰 종속 된 세부 수준에 대 한 확장 가능한 렌더링 기법을 지원 합니다.
-   공간 분할은 낮은 빈도로 더 낮은 빈도로 계산을 수행 하 여 성능을 향상 시킵니다. 여기에는 실제 애니메이션 또는 충돌 검색 또는 소프트 본문 dynamics에 대 한 물리 계산에 대 한 blend 셰이프 또는 모핑 대상을 사용한 혼합 계산이 포함 될 수 있습니다.

Direct3D 그래픽 파이프라인은 CPU에서 GPU로 작업을 오프 로드 하는 하드웨어의 공간 분할을 구현 합니다. 이로 인해 응용 프로그램에서 많은 수의 모핑 대상과/또는 보다 정교한 스키닝/변형 모델을 구현 하는 경우 성능이 크게 향상 될 수 있습니다.

Tessellator는 [선체 셰이더](hull-shader-stage--hs-.md) 를 파이프라인에 바인딩하여 초기화 되는 고정 된 함수 스테이지입니다. ( [방법: Tessellator 단계 초기화](/windows/desktop/direct3d11/direct3d-11-advanced-stages-tessellator-initialize))를 참조 하세요. Tessellator 단계는 도메인 (4, 3 또는 줄)을 여러 개의 작은 개체 (삼각형, 점이 나 선)로 세분화 하는 것입니다. Tessellator는 정규화 된 (일 대 일) 좌표계에서 정식 도메인을 바둑판식으로 배열 합니다. 예를 들어 쿼드 도메인은 unit 제곱으로 공간 분할 됩니다.

### <a name="span-idphases_in_the_tessellator__ts__stagespanspan-idphases_in_the_tessellator__ts__stagespanspan-idphases_in_the_tessellator__ts__stagespanphases-in-the-tessellator-ts-stage"></a><span id="Phases_in_the_Tessellator__TS__stage"></span><span id="phases_in_the_tessellator__ts__stage"></span><span id="PHASES_IN_THE_TESSELLATOR__TS__STAGE"></span>TS (Tessellator) 단계의 단계

TS (Tessellator) 단계는 두 단계로 작동 합니다.

-   첫 번째 단계에서는 32 비트 부동 소수점 산술 연산을 사용 하 여 공간 분할 요소를 처리 하 고, 반올림 문제를 해결 하 고, 매우 작은 요소를 줄이고, 요소를 줄이고 결합 합니다.
-   두 번째 단계에서는 선택한 분할 유형에 따라 지점 또는 토폴로지 목록을 생성 합니다. 이는 tessellator 단계의 핵심 작업 이며 고정 소수점 산술 연산에 16 비트 분수를 사용 합니다. 고정 소수점 연산을 사용 하면 허용 되는 전체 자릿수를 유지 하면서 하드웨어 가속을 사용할 수 있습니다. 예를 들어 64 측정기의 전체 패치가 지정 된 경우이 정밀도는 2 mm 해상도로 점수를 놓을 수 있습니다.

    | 분할 유형 | 범위                       |
    |----------------------|-----------------------------|
    | \_홀수 홀수      | \[1 ... 63\]                  |
    | \_짝수     | TessFactor 범위: \[ 2.64\] |
    | 정수              | TessFactor 범위: \[ 1.64\] |
    | Pow2                 | TessFactor 범위: \[ 1.64\] |

     

공간 분할은 두 개의 프로그래밍 가능한 셰이더 단계인 [선체 셰이더](hull-shader-stage--hs-.md) 및 [도메인 셰이더](domain-shader-stage--ds-.md)를 사용 하 여 구현 됩니다. 이러한 셰이더 단계는 셰이더 모델 5에 정의 된 HLSL 코드를 사용 하 여 프로그래밍 됩니다. 셰이더 대상은 hs: hs \_ 5 \_ 0 및 ds \_ 5 \_ 0입니다. 제목은 셰이더를 만든 다음, 셰이더가 파이프라인에 바인딩될 때 런타임에 전달 되는 컴파일된 셰이더에서 하드웨어에 대 한 코드를 추출 합니다.

### <a name="span-idenabling_disabling_tessellationspanspan-idenabling_disabling_tessellationspanspan-idenabling_disabling_tessellationspanenablingdisabling-tessellation"></a><span id="Enabling_disabling_tessellation"></span><span id="enabling_disabling_tessellation"></span><span id="ENABLING_DISABLING_TESSELLATION"></span>공간 분할 사용/사용 안 함

선체 셰이더를 만들어 tessellator 단계에 바인딩하여 공간 분할 (tessellation)을 사용 하도록 설정 합니다. 공간 분할 패치의 최종 꼭 짓 점 위치를 생성 하려면 [도메인 셰이더](domain-shader-stage--ds-.md) 를 만들어 도메인 셰이더 단계에 바인딩해야 합니다. 공간 분할을 사용 하도록 설정한 후에는 입력 어셈블러 (IA) 단계에 대 한 데이터 입력이 패치 데이터 여야 합니다. 입력 어셈블러 토폴로지는 패치 상수 토폴로지 여야 합니다.

공간 분할을 사용 하지 않도록 설정 하려면 선체 셰이더 및 도메인 셰이더를 **NULL**로 설정 합니다. 기 하 [도형 셰이더 (GS) 단계](geometry-shader-stage--gs-.md) 와 [스트림 출력 (즉,) 단계](stream-output-stage--so-.md) 모두를 사용할 수 없습니다.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>입력


Tessellator는 공간 분할 요소 (도메인을 공간 분할 하는 정도를 지정 하는 공간 분할)와 단계에서 전달 된 분할 유형 (패치를 조각화 하는 데 사용 되는 알고리즘을 지정 하는)을 사용 하 여 패치 당 한 번만 작동 합니다.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>출력


Tessellator는 uv (및 선택적으로) 좌표와 표면 토폴로지를 도메인 셰이더 단계로 출력 합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[그래픽 파이프라인](graphics-pipeline.md)

 

 