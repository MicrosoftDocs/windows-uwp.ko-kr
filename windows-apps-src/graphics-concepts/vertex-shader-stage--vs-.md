---
title: VS(꼭짓점 셰이더) 단계
description: VS(꼭짓점 셰이더) 단계에서는 꼭짓점을 처리하며, 일반적으로 변환, 스킨 지정, 조명 등의 작업을 수행합니다. 꼭짓점 셰이더는 항상 단일 입력 꼭짓점을 사용하여 단일 출력 꼭짓점을 생성합니다.
ms.assetid: 5133C4BB-B4E6-4697-9276-F718AD44869C
keywords:
- VS(꼭짓점 셰이더) 단계
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d6b9c67220c282ef1677559d586013c14366967a
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5703084"
---
# <a name="vertex-shader-vs-stage"></a>VS(꼭짓점 셰이더) 단계


VS(꼭짓점 셰이더) 단계에서는 꼭짓점을 처리하며, 일반적으로 변환, 스킨 지정, 조명 등의 작업을 수행합니다. 꼭짓점 셰이더는 항상 단일 입력 꼭짓점을 사용하여 단일 출력 꼭짓점을 생성합니다.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>목적 및 사용


VS(꼭짓점 셰이더) 단계는 다음과 같은 개별 꼭짓점을 처리하는 데 사용됩니다.

-   변환
-   스킨 지정
-   모핑
-   꼭짓점별 조명

꼭짓점 셰이더 단계는 프로그래밍 셰이더 단계로써 [그래픽 파이프라인](graphics-pipeline.md) 다이어그램에서 둥근 블록으로 표시됩니다. 이 셰이더 단계는 셰이더 모델 4.0[공통 셰이더 핵심](https://msdn.microsoft.com/library/windows/desktop/bb509580)을 사용합니다.

VS(꼭짓점 셰이더) 단계에서는 입력 어셈블러에서 꼭짓점을 처리합니다. 꼭짓점 셰이더는 항상 단일 입력 꼭짓점에서 작동하여 단일 출력 꼭짓점을 생성합니다. 파이프라인이 실행되도록 꼭짓점 셰이더 단계는 항상 활성 상태여야 합니다. 꼭짓점 수정이나 변환이 필요하지 않은 경우 통과 꼭짓점 셰이더를 작성하고 파이프라인으로 설정해야 합니다.

각 꼭짓점 셰이더 입력 꼭짓점은 최대 16개의 32비트 벡터(각각 최대 4개의 구성 요소)로 구성될 수 있습니다. 각 출력 꼭짓점은 16개의 32비트 4-구성 요소 벡터로 구성될 수 있습니다. 모든 꼭짓점 셰이더에는 최소 한 개의 입력과 한 개의 출력이 있어야 하며, 그 크기는 스칼라 값 하나 이상이어야 합니다.

꼭짓점 셰이더 단계에서는 입력 어셈블러의 두 시스템 생성 값인 VertexID 및 InstanceID를 사용할 수 있습니다(시스템 값 및 의미 체계 참조). VertexID와 InstanceID는 모두 꼭짓점 수준에서 의미를 갖고, 하드웨어에서 생성한 ID는 이 값을 이해하는 첫 번째 단계에만 제공할 수 있습니다. 따라서 이러한 ID 값은 꼭짓점 셰이더 단계에만 제공할 수 있습니다.

꼭짓점 셰이더는 항상 인접한 입력 기본 토폴로지의 인접 꼭짓점을 비롯한 모든 꼭짓점에서 실행됩니다. 꼭짓점 셰이더가 실행된 횟수는 VSInvocations 파이프라인 통계를 사용하여 CPU에서 쿼리할 수 있습니다.

꼭짓점 셰이더는 장면 공간을 파생할 필요가 없는 로드 및 텍스처 샘플링 작업을 수행할 수 있습니다(HLSL 내장 함수 사용: ([Sample(DirectX HLSL 텍스처 개체)](https://msdn.microsoft.com/library/windows/desktop/bb509695), [SampleCmpLevelZero(DirectX HLSL 텍스처 개체)](https://msdn.microsoft.com/library/windows/desktop/bb509697) 및 [SampleGrad(DirectX HLSL 텍스처 개체)](https://msdn.microsoft.com/library/windows/desktop/bb509698)).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>입력


시스템 생성 값 VertexID 및 InstanceID가 있는 단일 꼭짓점입니다. 각 꼭짓점 셰이더 입력 꼭짓점은 최대 16개의 32비트 벡터(각각 최대 4개의 구성 요소)로 구성될 수 있습니다.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>출력


단일 꼭짓점. 각 출력 꼭짓점은 16개의 32비트 4-구성 요소 벡터로 구성될 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[그래픽 파이프라인](graphics-pipeline.md)

 

 




