---
title: VS(꼭짓점 셰이더) 단계
description: 꼭 짓 점 셰이더 (VS) 단계에서는 일반적으로 변환, 스킨, 조명 등의 작업을 수행 하는 꼭 짓 점을 처리 합니다. 꼭 짓 점 셰이더는 단일 입력 버텍스를 사용 하 고 단일 출력 꼭 짓 점을 생성 합니다.
ms.assetid: 5133C4BB-B4E6-4697-9276-F718AD44869C
keywords:
- VS(꼭짓점 셰이더) 단계
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 88a990470d0ff8aea5c6415584bd63b2b4b65981
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156057"
---
# <a name="vertex-shader-vs-stage"></a>VS(꼭짓점 셰이더) 단계


꼭 짓 점 셰이더 (VS) 단계에서는 일반적으로 변환, 스킨, 조명 등의 작업을 수행 하는 꼭 짓 점을 처리 합니다. 꼭 짓 점 셰이더는 단일 입력 버텍스를 사용 하 고 단일 출력 꼭 짓 점을 생성 합니다.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>목적 및 사용


꼭 짓 점 셰이더 (VS) 단계는 다음과 같은 개별 꼭 짓 점 처리에 사용 됩니다.

-   변환
-   어니언
-   모핑
-   꼭 짓 점 별 조명

꼭 짓 점 셰이더 단계는 프로그래밍 가능한 셰이더 단계입니다. [그래픽 파이프라인](graphics-pipeline.md) 다이어그램에서 모퉁이가 둥근 블록으로 표시 됩니다. 이 셰이더 단계에서는 셰이더 모델 4.0 [공통 셰이더 코어](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core)를 사용 합니다.

꼭 짓 점 셰이더 (VS) 단계는 입력 어셈블러에서 꼭 짓 점을 처리 합니다. 꼭 짓 점 셰이더는 항상 단일 입력 꼭 짓 점에 대해 작업을 수행 하 고 단일 출력 꼭 짓 점을 생성 합니다. 꼭 짓 점 셰이더 단계는 파이프라인을 실행 하기 위해 항상 활성 상태 여야 합니다. 꼭 짓 점 수정과 변환이 필요 하지 않은 경우 통과 꼭 짓 점 셰이더를 만들어 파이프라인으로 설정 해야 합니다.

각 꼭 짓 점 셰이더 입력 꼭 짓 점은 최대 16 32 비트 벡터 (각각 최대 4 개의 구성 요소)로 구성 될 수 있습니다. 각 출력 꼭 짓 점은 16 32 비트 4 구성 요소 벡터로 구성 될 수 있습니다. 모든 꼭 짓 점 셰이더는 하나 이상의 입력 및 하나의 출력이 있어야 하 고 하나의 스칼라 값이 될 수 있습니다.

꼭 짓 점 셰이더 단계는 입력 어셈블러에서 시스템 생성 값 두 개를 사용할 수 있습니다. VertexID 및 InstanceID (시스템 값 및 의미 체계 참조) VertexID 및 InstanceID는 꼭 짓 점 수준에서 모두 의미가 있으므로 하드웨어에 의해 생성 된 Id는 해당 id를 이해 하는 첫 번째 단계에만 공급할 수 있습니다. 이러한 ID 값은 꼭 짓 점 셰이더 단계에만 공급할 수 있습니다.

꼭 짓 점 셰이더는 인접 한 입력 기본 토폴로지의 인접 한 꼭 짓 점을 포함 하 여 항상 모든 꼭 짓 점에 대해 실행 됩니다. VSInvocations 파이프라인 통계를 사용 하 여 CPU에서 꼭 짓 점 셰이더를 실행 한 횟수를 쿼리할 수 있습니다.

꼭 짓 점 셰이더는 화면 공간 파생물 (HLSL 내장 함수: [Sample (DIRECTX HLSL 텍스처](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-sample)개체), [SAMPLECMPLEVELZERO (directx HLSL 텍스처](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplecmplevelzero)개체) 및 [SampleGrad (directx HLSL 텍스처 개체)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplegrad))을 사용 하는 부하 및 질감 샘플링 작업을 수행할 수 있습니다.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>입력


VertexID 및 InstanceID 시스템 생성 값을 포함 하는 단일 꼭 짓 점입니다. 각 꼭 짓 점 셰이더 입력 꼭 짓 점은 최대 16 32 비트 벡터 (각각 최대 4 개의 구성 요소)로 구성 될 수 있습니다.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>출력


단일 꼭 짓 점입니다. 각 출력 꼭 짓 점은 16 32 비트 4 구성 요소 벡터로 구성 될 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[그래픽 파이프라인](graphics-pipeline.md)

 

 