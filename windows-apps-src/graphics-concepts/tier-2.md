---
title: 계층 2
description: 스트리밍 리소스에 대 한 계층 2 지원에는 계층 1 이상의 기능이 추가 됩니다. 예를 들어 크기가 적어도 하나 이상의 표준 타일 셰이프 일 때 압축 되지 않은 질감 밉를 보장 합니다. 고정 (LOD) 및 셰이더 작업에 대 한 상태 가져오기에 대 한 셰이더 지침 또한 NULL로 매핑된 타일에서 읽으면 샘플링 된 값이 0으로 처리 됩니다.
ms.assetid: 111A28EA-661A-4D29-921A-F2E376A46DC5
keywords:
- 계층 2
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 62fbbf3f21d614739918478b58394e51dbe577f9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175167"
---
# <a name="tier-2"></a>계층 2


스트리밍 리소스에 대 한 계층 2 지원에는 계층 1 이상의 기능이 추가 됩니다. 예를 들어 크기가 적어도 하나 이상의 표준 타일 셰이프 일 때 압축 되지 않은 질감 밉를 보장 합니다. 고정 (LOD) 및 셰이더 작업에 대 한 상태 가져오기에 대 한 셰이더 지침 또한 NULL로 매핑된 타일에서 읽으면 샘플링 된 값이 0으로 처리 됩니다.

## <a name="span-idtier_2_general_supportspanspan-idtier_2_general_supportspanspan-idtier_2_general_supportspantier-2-general-support"></a><span id="Tier_2_general_support"></span><span id="tier_2_general_support"></span><span id="TIER_2_GENERAL_SUPPORT"></span>계층 2 일반 지원


계층 2 지원에는 다음이 포함 됩니다.

-   기능 수준 11.1의 최소 하드웨어입니다.
-   이전 계층의 모든 기능 ( [계층 1](tier-1.md) 특정 제한 없음) 및 다음 항목의 추가 사항
-   고정 LOD에 대 한 셰이더 지침 및 매핑된 상태 피드백을 사용할 수 있습니다. [HLSL streaming resources 노출](hlsl-streaming-resources-exposure.md)을 참조 하세요.

그 외에도 다음과 같은 몇 가지 특정 지원 문제가 있습니다.

## <a name="span-idnon-mapped_tilesspanspan-idnon-mapped_tilesspanspan-idnon-mapped_tilesspannon-mapped-tiles"></a><span id="Non-mapped_tiles"></span><span id="non-mapped_tiles"></span><span id="NON-MAPPED_TILES"></span>매핑되지 않은 타일


매핑되지 않은 타일에서 읽은 모든 형식의 누락 된 구성 요소에서 0을 반환 하 고 누락 된 구성 요소에 대 한 기본값을 반환 합니다.

매핑되지 않은 타일에 대 한 쓰기는 메모리 이동에서 중지 되지만 이후에 같은 주소에 대 한 읽기는 동일한 주소에 대해 읽기를 시작할 수도 있고 선택 하지 못할 수도 있습니다.

## <a name="span-idtexture_filteringspanspan-idtexture_filteringspanspan-idtexture_filteringspantexture-filtering"></a><span id="Texture_filtering"></span><span id="texture_filtering"></span><span id="TEXTURE_FILTERING"></span>질감 필터링


Null 및**null** 이 아닌 타일을 **cmio 하** 는 공간을 포함 하는 질감 필터링은 전체 필터 작업에 **null** 타일에 대 한 텍셀에 0 (누락 된 형식 구성 요소에 대 한 기본값 포함)을 적용 합니다. 일부 초기 하드웨어는이 요구 사항을 충족 하지 않으며 텍셀 (0이 아닌 가중치 포함)가 **NULL** 타일에 속하는 경우 전체 필터 결과에 대해 0 (누락 된 형식 구성 요소에 대 한 기본값 포함)을 반환 합니다. 다른 하드웨어는 필터 작업에 모든 (0이 아닌 가중치) 텍셀를 포함 하는 요구 사항을 충족할 수 없습니다.

**NULL** 텍셀 액세스를 설정 하면 질감 읽기에 대 한 상태 피드백의 [**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) 작업이 false를 반환 합니다. 이는 텍스처 액세스 결과를 셰이더에 쓸 수 있는 방법과 질감 형식에 있는 구성 요소 수에 관계 없이 적용 됩니다 .이 조합은 질감에 액세스할 필요가 없는 것 처럼 보일 수 있습니다.

## <a name="span-idalignment_constraintsspanspan-idalignment_constraintsspanspan-idalignment_constraintsspanalignment-constraints"></a><span id="Alignment_constraints"></span><span id="alignment_constraints"></span><span id="ALIGNMENT_CONSTRAINTS"></span>정렬 제약 조건


표준 타일 셰이프에 대 한 맞춤 제약 조건: 모든 차원에서 하나 이상의 표준 타일을 채우는 Mip 맵을는 표준 바둑판식 배열을 사용 하 고 나머지는 N 개 타일 (응용 프로그램에 보고 됨)에 하나의 **단위로** 압축 된 것으로 간주 됩니다. 응용 프로그램은 타일 풀의 임의로 분리 된 위치에 N 타일을 매핑할 수 있지만 압축 된 타일을 모두 매핑하거나 모두 매핑하지 않아야 합니다. 밉 압축은 배열 조각 마다 압축 된 타일의 고유한 집합입니다.

## <a name="span-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>최소/최대 감소 필터링


최소/최대 감소 필터링이 지원 됩니다. [스트리밍 리소스 질감 샘플링 기능](streaming-resources-texture-sampling-features.md)을 참조 하세요.

## <a name="span-idlimitationsspanspan-idlimitationsspanspan-idlimitationsspanlimitations"></a><span id="Limitations"></span><span id="limitations"></span><span id="LIMITATIONS"></span>제한을


모든 차원의 표준 타일 크기 보다 mip 맵을 작은 스트리밍 리소스는 배열 크기가 1 보다 클 수 없습니다.

중복 매핑이 계속 적용 되는 경우 타일에 액세스할 수 있는 방법에 대 한 제한 사항입니다. [중복 매핑으로 타일 액세스 제한을](tile-access-limitations-with-duplicate-mappings.md)참조 하세요.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 기능 계층](streaming-resources-features-tiers.md)

 

 