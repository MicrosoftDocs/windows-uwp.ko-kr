---
title: 계층 1
description: 이번 섹션에서는 계층 1 지원에 대해서 설명합니다.
ms.assetid: 153DA429-0C99-4691-AEB4-124FD9B17BE2
keywords:
- 계층 1
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9aab54e23d59b21901e3aa4700581d2d7eeeee8e
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5743979"
---
# <a name="tier-1"></a>계층 1


이번 섹션에서는 계층 1 지원에 대해서 설명합니다.

## <a name="span-idtier1generallimitationsspanspan-idtier1generallimitationsspanspan-idtier1generallimitationsspantier-1-general-limitations"></a><span id="Tier_1_general_limitations"></span><span id="tier_1_general_limitations"></span><span id="TIER_1_GENERAL_LIMITATIONS"></span>계층 1 일반 제한 사항


-   최소 기능 수준이 11.0인 하드웨어
-   퀼팅(quilting) 지원 없음
-   Texture1D 또는 Texture3D 지원 없음
-   2, 8 또는 16 샘플 MSAA(Multisample Antialiasing) 지원 없음 128 bpp 형식이 없는 것을 제외하고 4x만 필요함
-   표준 재구성 패턴 없음(64KB 타일 및 꼬리 Mip 압축 내 레이아웃은 하드웨어 제조사에 따라 다름)
-   중복 매핑이 있을 경우 타일 접근 방법 제한 [중복 매핑에 대한 타일 액세스 제한](tile-access-limitations-with-duplicate-mappings.md)을 참조하세요.

## <a name="span-idspecificlimitationsaffectingtier1onlyspanspan-idspecificlimitationsaffectingtier1onlyspanspan-idspecificlimitationsaffectingtier1onlyspanspecific-limitations-affecting-tier-1-only"></a><span id="Specific_limitations_affecting_tier_1_only"></span><span id="specific_limitations_affecting_tier_1_only"></span><span id="SPECIFIC_LIMITATIONS_AFFECTING_TIER_1_ONLY"></span>계층 1에만 영향을 미치는 특정 제한 사항


### <a name="span-idreadingwritingtostreamingresourcesthathavenullmappingsspanspan-idreadingwritingtostreamingresourcesthathavenullmappingsspanspan-idreadingwritingtostreamingresourcesthathavenullmappingsspanreadingwriting-to-streaming-resources-that-have-null-mappings"></a><span id="Reading_writing_to_streaming_resources_that_have_NULL_mappings"></span><span id="reading_writing_to_streaming_resources_that_have_null_mappings"></span><span id="READING_WRITING_TO_STREAMING_RESOURCES_THAT_HAVE_NULL_MAPPINGS"></span>NULL 매핑을 갖는 스트리밍 리소스에 대한 읽기/쓰기

스트리밍 리소스는 **NULL** 매핑을 가질 수 있지만 이 리소스에 대한 읽기/쓰기는 장치 제거를 포함해 어떤 결과가 나올지 알 수 없습니다. 응용 프로그램은 단일 더미 페이지를 비어있는 모든 영역에 매핑하여 이 문제를 해결할 수 있습니다. 쓰기 순서가 정해져 있지 않기 때문에 다수의 렌더링 대상 위치로 매핑된 페이지에 작성 및 렌더링하는 경우에는 주의가 필요합니다.

### <a name="span-idnoshaderinstructionsforclampinglodandmappedstatusfeedbackspanspan-idnoshaderinstructionsforclampinglodandmappedstatusfeedbackspanspan-idnoshaderinstructionsforclampinglodandmappedstatusfeedbackspanno-shader-instructions-for-clamping-lod-and-mapped-status-feedback"></a><span id="No_shader_instructions_for_clamping_LOD_and_mapped_status_feedback"></span><span id="no_shader_instructions_for_clamping_lod_and_mapped_status_feedback"></span><span id="NO_SHADER_INSTRUCTIONS_FOR_CLAMPING_LOD_AND_MAPPED_STATUS_FEEDBACK"></span>LOD 및 매핑된 상태 피드백을 고정할 수 있는 셰이더 명령어의 부재

LOD 및 매핑된 상태 피드백을 고정할 수 있는 셰이더 명령어를 사용하지 못합니다. [HLSL 스트리밍 리소스 노출](hlsl-streaming-resources-exposure.md)을 참조하세요.

### <a name="span-idalignmentconstraintsforstandardtileshapesspanspan-idalignmentconstraintsforstandardtileshapesspanspan-idalignmentconstraintsforstandardtileshapesspanalignment-constraints-for-standard-tile-shapes"></a><span id="Alignment_constraints_for_standard_tile_shapes"></span><span id="alignment_constraints_for_standard_tile_shapes"></span><span id="ALIGNMENT_CONSTRAINTS_FOR_STANDARD_TILE_SHAPES"></span>표준 타일 모양에 대한 정렬 제한

크기가 모두 표준 타일 크기의 배수인 Mip(가장 미세한 것부터 시작)이 표준 타일 모양을 지원하며, 개별 타일을 임의로 매핑/매핑 해제할 수 있다는 것만 보장됩니다. 크기가 표준 타일 크기의 배수가 아닌 스트리밍 리소스에서는 첫 번째 Mipmap이 더욱 거친 모든 Mipmap과 함께 비표준 타일 모양을 가질 수 있기 때문에 이러한 Mip 집합에게는 N 64KB 타일이 적합합니다(응용 프로그램에 N이 보고됨). 이러한 N 타일은 하나의 유닛으로 압축되는 것으로 알려져 있기 때문에 N 타일 각각의 매핑이 타일 풀에서 임의로 연결되지 않은 곳에 위치할 수 있다고 하더라도 언제든지 응용 프로그램에서 완전히 매핑되거나 완전히 매핑 해제해야 합니다.

### <a name="span-idarrayofmipmapsthatarentamultipleofstandardtilesizespanspan-idarrayofmipmapsthatarentamultipleofstandardtilesizespanspan-idarrayofmipmapsthatarentamultipleofstandardtilesizespanarray-of-mipmaps-that-arent-a-multiple-of-standard-tile-size"></a><span id="Array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="ARRAY_OF_MIPMAPS_THAT_AREN_T_A_MULTIPLE_OF_STANDARD_TILE_SIZE"></span>표준 타일 크기의 배수가 아닌 Mipmap 배열

모든 크기에서 표준 타일 크기의 배수가 아닌 Mipmap으로 배열된 스트리밍 리소스는 배열 크기가 1보다 클 수 없습니다.

### <a name="span-idswitchingbetweenreferencingtilesinatilepoolviaabufferandtextureresourcespanspan-idswitchingbetweenreferencingtilesinatilepoolviaabufferandtextureresourcespanspan-idswitchingbetweenreferencingtilesinatilepoolviaabufferandtextureresourcespanswitching-between-referencing-tiles-in-a-tile-pool-via-a-buffer-and-texture-resource"></a><span id="Switching_between_referencing_tiles_in_a_tile_pool_via_a_Buffer_and_Texture_resource"></span><span id="switching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resource"></span><span id="SWITCHING_BETWEEN_REFERENCING_TILES_IN_A_TILE_POOL_VIA_A_BUFFER_AND_TEXTURE_RESOURCE"></span>버퍼 및 텍스처 리소스를 통한 타일 풀의 참조 타일 전환

[버퍼](introduction-to-buffers.md) 리소스를 통한 타일 풀의 타일 참조에서 [텍스처](introduction-to-textures.md) 리소스를 통한 동일한 타일 참조로, 혹은 그 반대 방향으로 전환하려면 해당하는 타일 풀 타일에 대한 매핑을 정의하면서 가장 최근에 업데이트하거나 복사한 타일 매핑의 리소스 크기가 타일에 액세스하는 데 사용할 리소스 크기와 동일해야 합니다(버퍼 vs 텍스처\*). 그렇지 않으면 장치 재설정을 포함하여 어떻게 동작할지 알 수 없습니다.

예를 들어 타일 매핑을 업데이트하여 버퍼에 대한 타일 매핑을 정의하고, [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 리소스를 통해 타일 풀의 동일한 타일에 대한 타일 매핑을 업데이트한 다음 버퍼를 통해 타일에 액세스하는 것은 잘못입니다. 이를 해결하려면 버퍼 및 텍스처 공유 타일을 서로 전환할 때 리소스에 대한 타일 매핑을 재정의하거나, 혹은 버퍼 리소스와 텍스처 리소스 사이에 타일 풀의 타일을 절대로 공유하지 않는 것이 좋습니다.

### <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>최소/최대 축소 필터링

최소/최대 축소 필터링은 지원되지 않습니다. [스트리밍 리소스 텍스처 샘플링 기능](streaming-resources-texture-sampling-features.md)을 참조하세요.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 기능 계층](streaming-resources-features-tiers.md)

 

 




