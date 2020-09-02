---
title: 계층 1
description: Direct3D의 스트리밍 리소스 기능에 대 한 계층 1 지원에 영향을 주는 일반적인 제한 사항에 대해 알아봅니다.
ms.assetid: 153DA429-0C99-4691-AEB4-124FD9B17BE2
keywords:
- 계층 1
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2ff90c75e3f4aefa28ddad5528d39d2f2d541bb0
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304645"
---
# <a name="tier-1"></a>계층 1


이 섹션에서는 계층 1 지원에 대해 설명 합니다.

## <a name="span-idtier_1_general_limitationsspanspan-idtier_1_general_limitationsspanspan-idtier_1_general_limitationsspantier-1-general-limitations"></a><span id="Tier_1_general_limitations"></span><span id="tier_1_general_limitations"></span><span id="TIER_1_GENERAL_LIMITATIONS"></span>계층 1 일반 제한 사항


-   기능 수준 11.0의 최소 하드웨어입니다.
-   Quilting 지원 되지 않습니다.
-   Texture1D 또는 Texture3D 지원 하지 않습니다.
-   2, 8 또는 16 샘플 다중 샘플 앤티엘리어싱 (MSAA)이 지원 되지 않습니다. 128 bpp 형식만 제외 하 고 4x만 필요 합니다.
-   표준 swizzle 패턴 (64KB 타일 내의 레이아웃 및 비상 밉 압축은 하드웨어 공급 업체까지)이 없습니다.
-   중복 매핑이 있는 경우 타일에 액세스할 수 있는 방법에 대 한 제한 사항입니다. [중복 매핑으로 타일 액세스 제한을](tile-access-limitations-with-duplicate-mappings.md)참조 하세요.

## <a name="span-idspecific_limitations_affecting_tier_1_onlyspanspan-idspecific_limitations_affecting_tier_1_onlyspanspan-idspecific_limitations_affecting_tier_1_onlyspanspecific-limitations-affecting-tier-1-only"></a><span id="Specific_limitations_affecting_tier_1_only"></span><span id="specific_limitations_affecting_tier_1_only"></span><span id="SPECIFIC_LIMITATIONS_AFFECTING_TIER_1_ONLY"></span>계층 1에만 영향을 주는 특정 제한 사항


### <a name="span-idreading_writing_to_streaming_resources_that_have_null_mappingsspanspan-idreading_writing_to_streaming_resources_that_have_null_mappingsspanspan-idreading_writing_to_streaming_resources_that_have_null_mappingsspanreadingwriting-to-streaming-resources-that-have-null-mappings"></a><span id="Reading_writing_to_streaming_resources_that_have_NULL_mappings"></span><span id="reading_writing_to_streaming_resources_that_have_null_mappings"></span><span id="READING_WRITING_TO_STREAMING_RESOURCES_THAT_HAVE_NULL_MAPPINGS"></span>NULL 매핑이 있는 스트리밍 리소스에 대 한 읽기/쓰기

스트리밍 리소스는 **NULL** 매핑을 포함할 수 있지만 해당 리소스에서 읽거나 쓸 때 장치를 제거 하는 등의 정의 되지 않은 결과가 생성 됩니다. 응용 프로그램은 단일 더미 페이지를 모든 빈 영역에 매핑하여이 문제를 해결할 수 있습니다. 쓰기 순서가 정의 되지 않기 때문에 여러 렌더링 대상 위치에 매핑되는 페이지를 작성 하 고 렌더링 하는 경우 주의 해야 합니다.

### <a name="span-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanspan-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanspan-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanno-shader-instructions-for-clamping-lod-and-mapped-status-feedback"></a><span id="No_shader_instructions_for_clamping_LOD_and_mapped_status_feedback"></span><span id="no_shader_instructions_for_clamping_lod_and_mapped_status_feedback"></span><span id="NO_SHADER_INSTRUCTIONS_FOR_CLAMPING_LOD_AND_MAPPED_STATUS_FEEDBACK"></span>고정 LOD 및 매핑된 상태 피드백에 대 한 셰이더 명령이 없습니다.

고정 LOD에 대 한 셰이더 명령과 매핑된 상태 피드백을 사용할 수 없습니다. [HLSL streaming resources 노출](hlsl-streaming-resources-exposure.md)을 참조 하세요.

### <a name="span-idalignment_constraints_for_standard_tile_shapesspanspan-idalignment_constraints_for_standard_tile_shapesspanspan-idalignment_constraints_for_standard_tile_shapesspanalignment-constraints-for-standard-tile-shapes"></a><span id="Alignment_constraints_for_standard_tile_shapes"></span><span id="alignment_constraints_for_standard_tile_shapes"></span><span id="ALIGNMENT_CONSTRAINTS_FOR_STANDARD_TILE_SHAPES"></span>표준 타일 셰이프에 대 한 맞춤 제약 조건

표준 타일 크기의 모든 배수로, 표준 타일 크기의 배수로, 개별 타일이 임의로 매핑/매핑되지 않을 수 있는 mips (부터부터 시작)만 보장 됩니다. 모든 성긴 mip 맵을 함께 표준 타일 크기의 배수가 아닌 차원이 포함 된 스트리밍 리소스의 첫 번째 밉은 표준 바둑판식 배열 셰이프를 포함할 수 있습니다 .이는이 mips 집합 (응용 프로그램에 보고 됨)에 대 한 N 64KB 타일로 맞춤입니다. 이러한 N 개 타일은 하나의 단위로 압축 된 것으로 간주 됩니다. 즉, 지정 된 시간에 응용 프로그램에서 완전히 매핑되거나 완전히 매핑되지 않아야 합니다. 단, 각 N 타일의 매핑은 타일 풀의 임의로 분리 된 위치에 있을 수 있습니다.

### <a name="span-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanspan-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanspan-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanarray-of-mipmaps-that-arent-a-multiple-of-standard-tile-size"></a><span id="Array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="ARRAY_OF_MIPMAPS_THAT_AREN_T_A_MULTIPLE_OF_STANDARD_TILE_SIZE"></span>표준 타일 크기의 배수가 아닌 mip 맵을의 배열입니다.

모든 차원에서 표준 타일 크기의 배수가 아닌 mip 맵을를 사용 하는 스트리밍 리소스는 배열 크기가 1 보다 클 수 없습니다.

### <a name="span-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanspan-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanspan-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanswitching-between-referencing-tiles-in-a-tile-pool-via-a-buffer-and-texture-resource"></a><span id="Switching_between_referencing_tiles_in_a_tile_pool_via_a_Buffer_and_Texture_resource"></span><span id="switching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resource"></span><span id="SWITCHING_BETWEEN_REFERENCING_TILES_IN_A_TILE_POOL_VIA_A_BUFFER_AND_TEXTURE_RESOURCE"></span>버퍼 및 텍스처 리소스를 통해 타일 풀에서 참조 하는 타일 간 전환

[텍스처](introduction-to-textures.md) 리소스를 통해 동일한 타일을 참조 하기 위해 [버퍼](introduction-to-buffers.md) 리소스를 통해 타일 풀에서 참조 하는 타일을 전환 하려면 타일에 \* 액세스 하는 데 사용할 리소스 차원과 동일한 리소스 차원 (버퍼 및 질감)에 대 한 타일 매핑 또는 타일 매핑 복사의 가장 최근 업데이트를 수행 해야 합니다. 그렇지 않으면 장치를 다시 설정 하는 기회를 포함 하 여 동작이 정의 되지 않습니다.

따라서 예를 들어, 타일 매핑을 업데이트 하 여 버퍼에 대 한 타일 매핑을 정의한 다음 [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) 리소스를 통해 타일 풀의 동일한 타일로 타일 매핑을 업데이트 하 고 버퍼를 통해 타일에 액세스할 수 있습니다. 작업을 수행 하는 작업은 버퍼와 질감 사이를 전환할 때 (또는 그 반대의 경우) 리소스에 대 한 타일 매핑을 다시 정의 하는 것입니다.

### <a name="span-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>최소/최대 감소 필터링

최소/최대 감소 필터링은 지원 되지 않습니다. [스트리밍 리소스 질감 샘플링 기능](streaming-resources-texture-sampling-features.md)을 참조 하세요.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 기능 계층](streaming-resources-features-tiers.md)

 

 