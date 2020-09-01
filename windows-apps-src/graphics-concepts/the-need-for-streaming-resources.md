---
title: 스트리밍 리소스 필요
description: GPU 메모리는 액세스할 수 없는 표면의 영역을 저장 하 고, 인접 한 타일을 통해 필터링 하는 방법을 하드웨어에 알리기 위해 필요 합니다.
ms.assetid: A88BE65B-104F-4176-9809-C12580A3684C
keywords:
- 스트리밍 리소스 필요
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b2e95427be7cdae450e60317d13765bb9372e84b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173027"
---
# <a name="the-need-for-streaming-resources"></a>스트리밍 리소스 필요


GPU 메모리는 액세스할 수 없는 표면의 영역을 저장 하 고, 인접 한 타일을 통해 필터링 하는 방법을 하드웨어에 알리기 위해 필요 합니다.

## <a name="span-idstreaming_resources_or_sparse_texturesspanspan-idstreaming_resources_or_sparse_texturesspanspan-idstreaming_resources_or_sparse_texturesspanstreaming-resources-or-sparse-textures"></a><span id="Streaming_resources_or_sparse_textures"></span><span id="streaming_resources_or_sparse_textures"></span><span id="STREAMING_RESOURCES_OR_SPARSE_TEXTURES"></span>스트리밍 리소스 또는 스파스 질감


Direct3D 11에서 *바둑판식 리소스* 라고 하는 *스트리밍 리소스* 는 작은 양의 실제 메모리를 사용 하는 대규모 논리적 리소스입니다.

스트리밍 리소스의 또 다른 이름은 *스파스 질감*입니다. "스파스"는 리소스의 바둑판식 특성 및이를 바둑판식으로 배열 하는 주요 이유를 모두 전달 합니다. 즉, 일부는 한 번에 매핑될 수 없습니다. 실제로 응용 프로그램은 리소스의 모든 지역 + mips에 대해 데이터를 작성 하지 않은 스트리밍 리소스를 의도적으로 작성할 수 있습니다. 따라서 콘텐츠 자체는 스파스 일 수 있으며, 지정 된 시간에 GPU (그래픽 처리 장치) 메모리의 콘텐츠 매핑은 더 많은 스파스의 하위 집합입니다.

## <a name="span-idwithout_tiling__memory_allocations_are_managed_at_subresource_granularityspanspan-idwithout_tiling__memory_allocations_are_managed_at_subresource_granularityspanspan-idwithout_tiling__memory_allocations_are_managed_at_subresource_granularityspanwithout-tiling-memory-allocations-are-managed-at-subresource-granularity"></a><span id="Without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="WITHOUT_TILING__MEMORY_ALLOCATIONS_ARE_MANAGED_AT_SUBRESOURCE_GRANULARITY"></span>바둑판식으로 배열 하지 않으면 메모리 할당이 하위 리소스 세분성에서 관리 됩니다.


스트리밍 리소스를 지원 하지 않는 그래픽 시스템 (즉, 운영 체제, 디스플레이 드라이버 및 그래픽 하드웨어)에서 그래픽 시스템은 하위 리소스 세분성의 모든 Direct3D 메모리 할당을 관리 합니다.

[버퍼](introduction-to-buffers.md)의 경우 전체 버퍼는 하위 리소스입니다.

[질감](textures.md)의 경우 (예: [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d)) 각 밉 수준은 하위 리소스입니다. 질감 배열의 경우 (예: [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray)) 지정 된 배열 조각에서 각 밉 수준은 하위 리소스입니다. 그래픽 시스템은이 하위 리소스 세분성에서 할당 매핑을 관리 하는 기능만 노출 합니다. 스트리밍 리소스의 컨텍스트에서 "매핑"은 GPU에 데이터를 표시 하는 것을 의미 합니다.

## <a name="span-idwithout_tiling__can_t_access_only_a_small_portion_of_mipmap_chainspanspan-idwithout_tiling__can_t_access_only_a_small_portion_of_mipmap_chainspanspan-idwithout_tiling__can_t_access_only_a_small_portion_of_mipmap_chainspanwithout-tiling-cant-access-only-a-small-portion-of-mipmap-chain"></a><span id="Without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="WITHOUT_TILING__CAN_T_ACCESS_ONLY_A_SMALL_PORTION_OF_MIPMAP_CHAIN"></span>바둑판식으로 배열 하지 않으면 밉 맵 체인의 작은 부분만 액세스할 수 없습니다.


응용 프로그램에서 특정 렌더링 작업에서 이미지 밉 맵 체인의 작은 부분에만 액세스 하면 되는 것으로 알고 있다고 가정 합니다 (지정 된 밉 맵 전체 영역에도 해당 하지 않을 수 있음). 이상적으로는 앱이 이러한 요구 사항을 그래픽 시스템에 알릴 수 있습니다. 그러면 그래픽 시스템은 필요한 메모리가 너무 많은 메모리를 페이징 하지 않고 GPU에서 매핑되는지 확인 하려고 합니다.

실제로, 스트리밍 리소스를 지원 하지 않고, 그래픽 시스템은 하위 리소스 세분성에서 GPU에 매핑해야 하는 메모리에 대 한 정보를 받을 수 있습니다 (예: 액세스할 수 있는 전체 밉 맵 수준 범위). 그래픽 시스템에는 오류가 발생 하지 않기 때문에 메모리의 일부를 참조 하는 렌더링 명령이 실행 되기 전에 전체 하위 리소스를 매핑해야 하는 많은 GPU 메모리를 사용 해야 할 수도 있습니다. 이는 스트리밍 리소스를 지원 하지 않고 Direct3D에서 사용 하기 어려운 많은 메모리 할당을 사용 하는 문제 중 하나일 뿐입니다.

## <a name="span-idsoftware_paging_to_break_the_surface_into_smaller_tilesspanspan-idsoftware_paging_to_break_the_surface_into_smaller_tilesspanspan-idsoftware_paging_to_break_the_surface_into_smaller_tilesspansoftware-paging-to-break-the-surface-into-smaller-tiles"></a><span id="Software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="SOFTWARE_PAGING_TO_BREAK_THE_SURFACE_INTO_SMALLER_TILES"></span>화면을 더 작은 타일로 분리 하는 소프트웨어 페이징


소프트웨어 페이징은 하드웨어를 처리 하기에 충분 한 크기의 타일로 화면을 분리 하는 데 사용할 수 있습니다.

Direct3D는 지정 된 쪽에서 최대 16384 픽셀의 [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) 표면을 지원 합니다. 16384 와이드 x 16384 및 픽셀당 4 바이트 이미지는 1GB의 비디오 메모리를 사용 하 고 mip 맵을를 추가 하면 해당 금액이 두 배가 됩니다. 실제로 모든 1GB는 단일 렌더링 작업에서 참조할 필요가 거의 없습니다.

일부 게임 개발자는 128K에 의해 128K 만큼의 지형 표면을 모델링 합니다. 이를 통해 기존 Gpu에서 작업 하는 방식은 하드웨어를 처리할 수 있을 정도로 작은 타일로 화면을 분리 하는 것입니다. 응용 프로그램은 필요한 타일을 확인 하 고 GPU에서 소프트웨어 페이징 시스템의 질감 캐시로 로드 해야 합니다.

이 접근 방식에 대 한 중요 한 단점은 하드웨어에서 발생 하는 페이징에 대 한 정보를 알 수 없는 것입니다. 이미지의 일부가 타일을 cmio 화면에 표시 되어야 하는 경우 하드웨어는 타일에서 고정 함수 (즉, 효율적)를 수행 하는 방법을 알지 못합니다. 즉, 자체 소프트웨어 타일을 관리 하는 응용 프로그램은 셰이더 코드에서 수동 질감 필터링을 사용 해야 합니다 (좋은 품질 이방성 필터를 사용 하는 경우 비용이 매우 비쌉니다). 또한 고정 된 기능 하드웨어 필터링이 계속 해 서 일부 지원을 제공할 수 있도록 인접 타일의 데이터를 포함 하는 타일 주위에 메모리를 제작 하는 데 필요한 메모리를 확보 해야 합니다.

## <a name="span-idmaking_tiled_representation_of_surface_allocations_a_first-class_featurespanspan-idmaking_tiled_representation_of_surface_allocations_a_first-class_featurespanspan-idmaking_tiled_representation_of_surface_allocations_a_first-class_featurespanmaking-tiled-representation-of-surface-allocations-a-first-class-feature"></a><span id="Making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="MAKING_TILED_REPRESENTATION_OF_SURFACE_ALLOCATIONS_A_FIRST-CLASS_FEATURE"></span>표면 할당을 바둑판식으로 표현 하는 최고 수준의 기능


화면 할당의 바둑판식 표현이 그래픽 시스템의 최고 수준의 기능이 면 응용 프로그램은 사용할 수 있도록 타일을 하드웨어에 지시할 수 있습니다. 이러한 방식으로 응용 프로그램에서 알고 있는 표면의 영역을 저장 하는 GPU 메모리가 부족 하 여 액세스할 수 없으며, 하드웨어는 인접 한 타일을 필터링 하는 방법을 이해 하 여 소프트웨어 타일을 직접 수행 하는 개발자에 게 발생 하는 문제를 줄일 수 있습니다.

그러나 전체 솔루션을 제공 하려면 화면 내에서 바둑판식 배열이 지원 되는지 여부에 관계 없이, 최대 표면 차원이 응용 프로그램이 이미 원하는 128K + 근처에는 현재 16384입니다. 하드웨어에 큰 텍스처 크기를 지 원하는 것은 한 가지 방법 이지만이 경로를 전환 하는 데 상당한 비용 및/또는 절충이 있습니다.

Direct3D's 텍스처 필터 경로 및 렌더링 경로는 렌더링 중에 화면에 발생 하는 뷰포트 범위를 지원 하거나 필터링 중에 화면 가장자리에서 질감 줄 바꿈을 지원 하는 등의 다른 요구 사항으로 16K 질감을 지 원하는 전체 자릿수 측면에서 이미 포화 상태를 유지 합니다. 질감 크기가 16K를 초과 하 여 기능/전체 자릿수가 특정 방식으로 제공 되는 경우 균형을 정의 하는 것이 좋습니다. 그러나이 concession를 사용 하는 경우에도 하드웨어 시스템 전체에서 더 큰 질감 크기로 전환 하는 기능을 사용 하 여 추가 하드웨어 비용이 필요할 수 있습니다.

## <a name="span-idissue_with_large_textures__precision_for_locations_on_surfacespanspan-idissue_with_large_textures__precision_for_locations_on_surfacespanspan-idissue_with_large_textures__precision_for_locations_on_surfacespanissue-with-large-textures-precision-for-locations-on-surface"></a><span id="Issue_with_large_textures__precision_for_locations_on_surface"></span><span id="issue_with_large_textures__precision_for_locations_on_surface"></span><span id="ISSUE_WITH_LARGE_TEXTURES__PRECISION_FOR_LOCATIONS_ON_SURFACE"></span>많은 텍스처에 대 한 문제: 표면의 위치에 대 한 전체 자릿수


질감으로 재생 되는 한 가지 문제는 단일 정밀도 부동 소수점 질감 좌표 (및 래스터화를 지 원하는 연결 된 interpolators)가 정밀도가 부족 하 여 표면의 위치를 정확 하 게 지정 하는 것입니다. 텍스처 필터링이 뒤따르게 됩니다. 한 가지 부담이 큰 옵션은 배정밀도를 지 원하는 것입니다. 하지만 적절 한 대안을 제공 하는 것이 좋습니다.

## <a name="span-idenabling_multiple_resources_of_different_dimensions_to_share_memoryspanspan-idenabling_multiple_resources_of_different_dimensions_to_share_memoryspanspan-idenabling_multiple_resources_of_different_dimensions_to_share_memoryspanenabling-multiple-resources-of-different-dimensions-to-share-memory"></a><span id="Enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="ENABLING_MULTIPLE_RESOURCES_OF_DIFFERENT_DIMENSIONS_TO_SHARE_MEMORY"></span>다른 차원의 여러 리소스를 사용 하 여 메모리 공유


스트리밍 리소스에서 제공 될 수 있는 또 다른 시나리오는 다른 차원/형식의 여러 리소스가 동일한 메모리를 공유할 수 있도록 하는 것입니다. 응용 프로그램에는 동시에 사용 하지 않는 것으로 알려진 단독 리소스 집합이 있거나, 매우 간단한 사용을 위해서만 생성 된 다음 다른 리소스를 만든 경우에만 생성 되는 리소스가 포함 되어 있습니다. "스트리밍 리소스"를 벗어날 수 있는 일반 성을 형식은 사용자가 동일한 (겹치는) 메모리에서 여러 다른 리소스를 가리키도록 할 수 있다는 것입니다. 즉, 차원/형식을 정의 하는 "리소스"의 생성 및 소멸은 응용 프로그램의 관점에서 리소스의 기반이 되는 메모리 관리에서 분리 될 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스](streaming-resources.md)

 

 