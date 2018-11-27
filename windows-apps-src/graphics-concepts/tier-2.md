---
title: 계층 2
description: 스트리밍 리소스에 대한 계층 2 지원은 크기가 표준 타일 모양 1개 이상일 때 비압축 텍스처 Mipmap 보장, LOD를 고정하거나 셰이더 연산에 대한 상태를 가져올 수 있는 셰이더 명령어, 그리고 샘플링 값을 0으로 처리하는 NULL-매핑된 타일의 읽기 등 계층 1보다 지원하는 기능이 추가됩니다.
ms.assetid: 111A28EA-661A-4D29-921A-F2E376A46DC5
keywords:
- 계층 2
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6f9f9a69c0e30459929d1e31084ea88b3f7ebbd0
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7704231"
---
# <a name="tier-2"></a>계층 2


스트리밍 리소스에 대한 계층 2 지원은 크기가 표준 타일 모양 1개 이상일 때 비압축 텍스처 Mipmap 보장, LOD를 고정하거나 셰이더 연산에 대한 상태를 가져올 수 있는 셰이더 명령어, 그리고 샘플링 값을 0으로 처리하는 NULL-매핑된 타일의 판독값 등 계층 1보다 지원하는 기능이 추가됩니다.

## <a name="span-idtier2generalsupportspanspan-idtier2generalsupportspanspan-idtier2generalsupportspantier-2-general-support"></a><span id="Tier_2_general_support"></span><span id="tier_2_general_support"></span><span id="TIER_2_GENERAL_SUPPORT"></span>계층 2 일반 지원


계층 2에서 지원하는 기능은 다음과 같습니다.

-   최소 기능 수준이 11.1인 하드웨어
-   이전 계층의 모든 기능([계층 1](tier-1.md) 특정 제한 제외)에 더하여 다음과 같은 기능이 추가로 지원됩니다.
-   LOD 및 매핑된 상태 피드백을 고정할 수 있는 셰이더 명령어를 사용할 수 있습니다. [HLSL 스트리밍 리소스 노출](hlsl-streaming-resources-exposure.md)을 참조하세요.

이 밖에도 다음과 같이 몇 가지 특정 지원 문제가 있습니다.

## <a name="span-idnon-mappedtilesspanspan-idnon-mappedtilesspanspan-idnon-mappedtilesspannon-mapped-tiles"></a><span id="Non-mapped_tiles"></span><span id="non-mapped_tiles"></span><span id="NON-MAPPED_TILES"></span>비매핑된 타일


비매핑된 타일의 읽기는 형식에서 누락되지 않은 모든 요소의 경우에는 0을, 그리고 누락된 요소의 경우에는 기본값을 반환합니다.

비매핑된 타일에 대한 쓰기는 메모리까지 전달되지 않고 캐시에서 종료되어 동일한 주소에 대한 읽기가 처리하거나 처리하지 못할 수도 있습니다.

## <a name="span-idtexturefilteringspanspan-idtexturefilteringspanspan-idtexturefilteringspantexture-filtering"></a><span id="Texture_filtering"></span><span id="texture_filtering"></span><span id="TEXTURE_FILTERING"></span>텍스처 필터링


**NULL** 및 비**NULL** 타일을 공간에 배치하는 텍스처 필터링은 **NULL** 타일의 텍셀일 경우 0(누락된 형식 요소의 기본값)을 전체 필터 연산에 지정합니다. 일부 초기의 하드웨어는 이러한 요건을 충족하지 못하고 어떤 텍셀(0이 아닌 가중치)이든 **NULL** 타일에 해당하면 전체 필터 결과로 0(누락된 형식 요소의 기본값)을 반환합니다. 그 밖에 다른 하드웨어는 필터 연산에 0이 아닌 가중치의 모든 텍셀을 포함해야 하는 요건을 반드시 충족해야 합니다.

**NULL** 텍셀 액세스는 상태 피드백에 대한 [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) 연산에서 텍스처 읽기가 false를 반환하는 원인이 됩니다. 이는 셰이더에서 텍스처 액세스 결과의 쓰기 마스킹 방식과 텍스처 형식의 요소 수(텍스처 액세스가 필요 없어 보이는 조합)와 상관없습니다.

## <a name="span-idalignmentconstraintsspanspan-idalignmentconstraintsspanspan-idalignmentconstraintsspanalignment-constraints"></a><span id="Alignment_constraints"></span><span id="alignment_constraints"></span><span id="ALIGNMENT_CONSTRAINTS"></span>정렬 제한


표준 타일 모양의 정렬 제한: 모든 크기에서 표준 타일을 1개 이상 채우는 Mipmap은 표준 타일링 사용이 보장되며, 나머지는 하나의 **유닛**으로서 N 타일로 압축되는 것으로 알려져 있습니다(응용 프로그램에 N이 보고됨). 응용 프로그램은 N 타일을 타일 풀에서 임의로 연결되지 않은 위치에 매핑할 수 있지만 압축된 타일을 모두 매핑하든지, 혹은 하나라도 매핑해서는 안 됩니다. Mip 압축은 배열 부분당 압축된 타일의 고유 집합입니다.

## <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>최소/최대 감소 필터링


최소/최대 축소 필터링이 지원됩니다. [스트리밍 리소스 텍스처 샘플링 기능](streaming-resources-texture-sampling-features.md)을 참조하세요.

## <a name="span-idlimitationsspanspan-idlimitationsspanspan-idlimitationsspanlimitations"></a><span id="Limitations"></span><span id="limitations"></span><span id="LIMITATIONS"></span>제한 사항


모든 크기에서 표준 타일 크기보다 작은 Mipmap으로 배열된 스트리밍 리소스는 배열 크기가 1보다 커서는 안 됩니다.

중복 매핑이 있을 때 타일 액세스 방식에 대한 제한은 계속해서 적용됩니다. [중복 매핑에 대한 타일 액세스 제한](tile-access-limitations-with-duplicate-mappings.md)을 참조하세요.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 기능 계층](streaming-resources-features-tiers.md)

 

 




