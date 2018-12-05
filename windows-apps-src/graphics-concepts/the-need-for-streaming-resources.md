---
title: 스트리밍 리소스 필요
description: 스트리밍 리소스는 접근하지 못하는 표면 지역을 저장하는 데 GPU 메모리를 낭비하지 않고, 하드웨어에게 인접 타일에 대한 필터링 방식을 지정하는 데 필요합니다.
ms.assetid: A88BE65B-104F-4176-9809-C12580A3684C
keywords:
- 스트리밍 리소스 필요
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0e0354b0e727e84d562bf63779e74be72f87198f
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8709537"
---
# <a name="the-need-for-streaming-resources"></a>스트리밍 리소스 필요


스트리밍 리소스는 접근하지 못하는 표면 지역을 저장하는 데 GPU 메모리를 낭비하지 않고, 하드웨어에게 인접 타일에 대한 필터링 방식을 지정하는 데 필요합니다.

## <a name="span-idstreamingresourcesorsparsetexturesspanspan-idstreamingresourcesorsparsetexturesspanspan-idstreamingresourcesorsparsetexturesspanstreaming-resources-or-sparse-textures"></a><span id="Streaming_resources_or_sparse_textures"></span><span id="streaming_resources_or_sparse_textures"></span><span id="STREAMING_RESOURCES_OR_SPARSE_TEXTURES"></span>스트리밍 리소스 또는 저밀도 텍스처


*스트리밍 리소스*(Direct3D 11에서는 *타일식 리소스*로 불림)는 적은 양의 물리 메모리를 사용하는 대용량 논리 리소스입니다.

스트리밍 리소스는 *저밀도 텍스처*라고도 불립니다. 여기에서 "저밀도"란 타일 방식의 리소스를 비롯해 타일로 처리되는 주요 원인이겠지만 모든 리소스가 바로 매핑되지 않는다는 것을 의미합니다. 실제로 응용 프로그램은 리소스의 모든 지역과 Mip에 대해 아무런 데이터도 작성되지 않는 스트리밍 리소스를 의도적으로 작성할 수 있습니다. 따라서 콘텐츠 자체의 밀도가 낮아질 수 있으며, GPU 메모리의 콘텐츠 매핑이 일정 시간 더욱 희박해집니다.

## <a name="span-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanspan-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanspan-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanwithout-tiling-memory-allocations-are-managed-at-subresource-granularity"></a><span id="Without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="WITHOUT_TILING__MEMORY_ALLOCATIONS_ARE_MANAGED_AT_SUBRESOURCE_GRANULARITY"></span>타일링이 없을 경우 하위 리소스로 세분화하여 관리되는 메모리 할당


그래픽 시스템(운영 체제, 디스플레이 드라이버 및 그래픽 하드웨어)이 스트리밍 리소스를 지원하지 않을 경우에는 모든 Direct3D 메모리 할당을 하위 리소스로 세분화하여 관리합니다.

[버퍼](introduction-to-buffers.md)에서는 전체 버퍼가 하위 리소스가 됩니다.

[텍스처](textures.md)(예: [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525))에서는 각 Mip 수준이 하위 리소스이고, 텍스처 배열(예: [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526))의 경우에는 일정한 배열 부분의 각 Mip 수준이 하위 리소스입니다. 그래픽 시스템은 이러한 하위 리소스의 세분화 수준에서 메모리 할당 매핑을 관리할 수밖에 없습니다. 스트리밍 리소스의 관점에서 보면 여기에서 "매핑"이란 데이터가 GPU에 보이도록 만드는 것을 의미합니다.

## <a name="span-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanspan-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanspan-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanwithout-tiling-cant-access-only-a-small-portion-of-mipmap-chain"></a><span id="Without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="WITHOUT_TILING__CAN_T_ACCESS_ONLY_A_SMALL_PORTION_OF_MIPMAP_CHAIN"></span>타일링이 없을 경우 작은 부분의 Mipmap 사슬에만 접근하지 못함


응용 프로그램이 특정 렌더링 연산만 작은 부분의 이미지 Mipmap 사슬(일정한 Mipmap의 전체 영역이 아닐 수도 있음)에 접근해야 한다는 사실을 알고 있다고 가정하겠습니다. 이때 응용 프로그램이 그래픽 시스템에게 이 요건에 대해 알릴 수 있다면 가장 이상적입니다. 그렇다면 그래픽 시스템이 지나치게 많은 메모리를 호출하지 않고 필요한 메모리만 GPU에 매핑할 수 있도록 제어할 것입니다.

하지만 실제로 스트리밍 리소스를 지원하지 않는 그래픽 시스템은 하위 리소스의 세분화 수준(접근할 수 있는 전체 Mipmap 수준의 범위 등)으로 GPU에 매핑해야 하는 메모리에 대해서만 알 수 있습니다. 둘 중 무엇이든 그래픽 시스템에서 메모리 수요에 대한 오류는 없기 때문에 메모리를 참조하는 렌더링 명령을 실행하려면 전체 하위 리소스를 매핑하는 데 지나치게 많은 GPU 메모리를 잠재적으로 사용할 수 밖에 없습니다. 이는 Direct3D에서 스트리밍 리소스 지원 없이 대용량의 메모리 할당을 사용하기 어려운 다양한 이유 중 한 가지에 불과합니다.

## <a name="span-idsoftwarepagingtobreakthesurfaceintosmallertilesspanspan-idsoftwarepagingtobreakthesurfaceintosmallertilesspanspan-idsoftwarepagingtobreakthesurfaceintosmallertilesspansoftware-paging-to-break-the-surface-into-smaller-tiles"></a><span id="Software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="SOFTWARE_PAGING_TO_BREAK_THE_SURFACE_INTO_SMALLER_TILES"></span>표면을 더욱 적은 수의 타일로 세분화하기 위한 소프트웨어 페이징


소프트웨어 페이징은 하드웨어가 처리할 수 있을 만큼 적은 수의 타일로 표면을 세분화하는 데 사용됩니다.

Direct3D는 한쪽 면에 최대 16,384 픽셀을 생성할 수 있는 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 표면을 지원합니다. 픽셀 수가 16384x16384이고 픽셀당 4바이트인 이미지는 1GB의 비디오 메모리를 소모합니다(Mipmap이 추가되면 소모량은 2배로 늘어납니다). 실제로 단일 렌더링 연산에서 1GB를 모두 참조할 필요는 거의 없습니다.

일부 게임 개발자들은 최대 128k x 128k의 크기로 지형 표면을 모델링합니다. 기존 GPU에서 이렇게 모델링할 수 있는 방법은 하드웨어에서 처리할 수 있을 만큼 적은 수의 타일로 표면을 세분화하는 것입니다. 응용 프로그램은 먼저 어떤 타일이 필요한지 판단한 후 타일을 소프트웨어 페이징 시스템인 GPU의 텍스처 캐시에 로드합니다.

이러한 접근법의 중요한 단점은 하드웨어가 페이징에 대해서 아무것도 알지 못한다는 데 있습니다. 즉, 타일을 배치하는 장면에서 이미지의 일부를 표시해야 할 경우 하드웨어는 고정 함수(효율적인) 타일 필터링 방법을 알지 못합니다. 이 말은 자체 소프트웨어 타일링을 관리하는 응용 프로그램이 셰이더 코드의 수동 텍스처 필터링에 의존할 수 밖에 없을 뿐만 아니라(양호한 품질의 이방성 필터를 원하는 경우 비용이 매우 고가임) 고정 함수 하드웨어 필터링이 계속해서 이용하려면 데이터가 포함된 타일 주위의 메모리 작성 여백(gutter)을 피할 수 없다는 것을 의미합니다.

## <a name="span-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanspan-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanspan-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanmaking-tiled-representation-of-surface-allocations-a-first-class-feature"></a><span id="Making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="MAKING_TILED_REPRESENTATION_OF_SURFACE_ALLOCATIONS_A_FIRST-CLASS_FEATURE"></span>표면 할당에 대한 타일식 표현 기능이 우수해야 하는 이유


그래픽 시스템에서 표면 할당에 대한 타일식 표현 기능이 우수하다면 응용 프로그램이 하드웨어에게 사용할 타일을 지정할 수 있습니다. 그렇게 되면 응용 프로그램이 접근할 필요 없는 표면 지역을 저장하는 데 GPU 메모리를 낭비하지 않을 뿐만 아니라 하드웨어가 인접 타일의 필터링 방법을 판단하기 때문에 직접 소프트웨어 타일링을 실행하는 개발자의 부담을 일부 줄여줄 수 있습니다.

하지만 종합적인 솔루션을 위해서는 표면 내 타일링 지원 여부에 상관없이 현재 최대 표면 크기가 16384이지만 응용 프로그램에 필요한 128K+에는 미치지도 못한다는 사실에 대처할 수 있는 무언가가 필요합니다. 대용량의 텍스처 크기를 지원하는 하드웨어가 한 가지 방법이 될 수는 있지만 비용을 비롯해 이를 위해 상쇄되는 부분 또한 만만치 않습니다.

Direct3D의 텍스처 필터 경로와 렌더링 경로는 렌더링 시 표면 품질을 떨어뜨리는 뷰포트 범위를 지원하거나, 혹은 필터링 시 표면 가장 자리의 텍스처 래핑을 지원하는 등 다른 요건과 함께 16K 텍스처를 지원하면서 정밀도 측면에서 이미 포화 상태입니다. 한 가지 방법으로 텍스처 크기가 16K를 넘어 커질수록 기능이나 정밀도를 일정 부분 포기하는 방식의 상쇄 관계를 정의하는 것이 가능합니다. 이러한 상쇄 관계에도 불구하고 하드웨어 시스템에서 대용량의 텍스처 크기를 처리할 수 있는 기능과 관련하여 하드웨어 비용이 추가로 발생할 수도 있습니다.

## <a name="span-idissuewithlargetexturesprecisionforlocationsonsurfacespanspan-idissuewithlargetexturesprecisionforlocationsonsurfacespanspan-idissuewithlargetexturesprecisionforlocationsonsurfacespanissue-with-large-textures-precision-for-locations-on-surface"></a><span id="Issue_with_large_textures__precision_for_locations_on_surface"></span><span id="issue_with_large_textures__precision_for_locations_on_surface"></span><span id="ISSUE_WITH_LARGE_TEXTURES__PRECISION_FOR_LOCATIONS_ON_SURFACE"></span>대용량의 텍스처 문제: 표면의 위치 정밀도


텍스처의 용량이 커지면서 발생하는 한 가지 문제는 단정밀도 부동 소수점 텍스처 좌표(및 래스터화 지원을 위한 관련 보간)가 표면의 위치를 정확히 지정할 수 있는 정밀도가 부족하다는 데 있습니다. 그러면 텍스처 필터링도 어려워질 수 있습니다. 합리적 대안을 감안했을 때 지나칠 수도 있지만 한 가지 비용이 많이 드는 방법으로 배정밀도 보간 지원을 요구하는 것도 가능합니다.

## <a name="span-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanspan-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanspan-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanenabling-multiple-resources-of-different-dimensions-to-share-memory"></a><span id="Enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="ENABLING_MULTIPLE_RESOURCES_OF_DIFFERENT_DIMENSIONS_TO_SHARE_MEMORY"></span>다른 크기의 다중 리소스를 사용한 메모리 공유


스트리밍 리소스를 통한 또 하나의 시나리오로서 다른 크기/형식의 다중 리소스를 사용해 동일한 메모리를 공유할 수 있습니다. 간혹 응용 프로그램은 동시에 사용하지 않는 것으로 알려진 리소스 집합을 독점하거나, 혹은 잠시 사용한 후 제거한 다음 다른 리소스를 생성할 목적으로 생성되는 리소스를 가지고 있습니다. "스트리밍 리소스"에서 생각해볼 수 있는 일반성은 사용자가 동일한(중복된) 메모리에 각기 다른 다수의 리소스를 할당할 수 있다는 데 있습니다. 다시 말해서 크기.형식 등을 정의하는 "리소스"의 생성 및 제거가 응용 프로그램의 관점에서 리소스를 뒷받침하는 메모리 관리와 분리될 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스](streaming-resources.md)

 

 




