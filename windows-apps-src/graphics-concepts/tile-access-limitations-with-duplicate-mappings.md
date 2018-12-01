---
title: 중복 매핑을 사용하는 타일 액세스 제한
description: 중복되는 원본과 대상을 이용해 스트리밍 리소스를 복사하거나, 혹은 렌더링 영역의 공유 타일로 렌더링할 때처럼 중복 매핑을 사용하여 타일에 액세스하는 데는 제한이 따릅니다.
ms.assetid: 6E40B1DC-BCF1-4B09-82A8-7B2D9B209A61
keywords:
- 중복 매핑을 사용하는 타일 액세스 제한
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d5563a9909ba3d6cb3deaae43bcf9e55b4b2c880
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8338157"
---
# <a name="tile-access-limitations-with-duplicate-mappings"></a>중복 매핑을 사용하는 타일 액세스 제한


중복되는 원본과 대상을 이용해 스트리밍 리소스를 복사하거나, 혹은 렌더링 영역의 공유 타일로 렌더링할 때처럼 중복 매핑을 사용하여 타일에 액세스하는 데는 제한이 따릅니다.

## <a name="span-idcopyingstreamingresourceswithoverlappingsourceanddestinationspanspan-idcopyingstreamingresourceswithoverlappingsourceanddestinationspanspan-idcopyingstreamingresourceswithoverlappingsourceanddestinationspancopying-streaming-resources-with-overlapping-source-and-destination"></a><span id="Copying_streaming_resources_with_overlapping_source_and_destination"></span><span id="copying_streaming_resources_with_overlapping_source_and_destination"></span><span id="COPYING_STREAMING_RESOURCES_WITH_OVERLAPPING_SOURCE_AND_DESTINATION"></span>중복되는 원본과 대상을 이용한 스트리밍 리소스 복사


두 리소스가 스트리밍 리소스가 아니고, Copy\* 연산이 중복 복사본을 지원하더라도 Copy\* 연산에서 원본 및 대상 영역의 타일이 복사 영역에 중복 매핑이 있어서 서로 중복될 수 있는 경우에는 Copy\* 연산이 마치 원본이 대상에 복사되기 전에 임시 위치에 복사되는 것처럼 대처합니다. 하지만 중복이 분명하지 않은 경우(예: 원본 및 대상 리소스가 다르지만 매핑을 공유하는 경우, 매핑이 일정 표면에 걸쳐 중복되는 경우)에는 공유하는 타일에 대한 복사 연산 결과를 알 수 없습니다.

## <a name="span-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspanspan-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspanspan-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspancopying-to-streaming-resource-with-duplicated-tiles-in-destination-area"></a><span id="Copying_to_streaming_resource_with_duplicated_tiles_in_destination_area"></span><span id="copying_to_streaming_resource_with_duplicated_tiles_in_destination_area"></span><span id="COPYING_TO_STREAMING_RESOURCE_WITH_DUPLICATED_TILES_IN_DESTINATION_AREA"></span>대상 영역의 중복 타일을 사용하여 스트리밍 리소스에 복사


대상 영역의 중복 타일을 사용하여 스트리밍 리소스에 복사할 경우에는 데이터가 동일하지 않으면 이 타일의 결과를 알 수 없습니다. 예를 들어 타일마다 서로 다른 순서로 타일을 작성할 수도 있기 때문입니다.

## <a name="span-iduavaccessestoduplicatetilesmappingsspanspan-iduavaccessestoduplicatetilesmappingsspanspan-iduavaccessestoduplicatetilesmappingsspanuav-accesses-to-duplicate-tiles-mappings"></a><span id="UAV_accesses_to_duplicate_tiles_mappings"></span><span id="uav_accesses_to_duplicate_tiles_mappings"></span><span id="UAV_ACCESSES_TO_DUPLICATE_TILES_MAPPINGS"></span>중복 타일 매핑에 대한 UAV 액세스


예를 들어 스트리밍 리소스의 UAV(Unordered Access View)가 자신의 영역에, 혹은 다른 리소스가 파이프라인에 바인딩되어 중복 타일 매핑이 있다고 가정하겠습니다. 이때 다른 스레드로 실행할 경우에는 마치 UAV에 대한 메모리 액세스 순서가 지정되지 않는 것처럼 중복 타일에 대한 액세스 순서가 정의되지 않습니다.

## <a name="span-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanspan-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanspan-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanrendering-after-tile-mapping-changes-or-content-updates-from-outside-mappings"></a><span id="Rendering_after_tile_mapping_changes_or_content_updates_from_outside_mappings"></span><span id="rendering_after_tile_mapping_changes_or_content_updates_from_outside_mappings"></span><span id="RENDERING_AFTER_TILE_MAPPING_CHANGES_OR_CONTENT_UPDATES_FROM_OUTSIDE_MAPPINGS"></span>타일 매핑 변경 또는 외부 매핑의 콘텐츠 업데이트 이후 렌더링


스트리밍 리소스의 타일 매핑이 변경되었거나, 혹은 타일 풀에서 매핑된 타일의 콘텐츠가 다른 스트리밍 리소스의 매핑을 통해 변경된 후 스트리밍 리소스가 렌더링 대상 뷰 또는 깊이 스텐실 뷰를 통해 렌더링되는 경우에는 응용 프로그램이 고정 함수인 Clear API를 사용하여 매핑 여부에 상관없이 렌더링 대상 영역 내에서 변경된 타일을 지우거나 Copy\*/Update\* API를 사용하여 완전히 복사해야 합니다.

응용 프로그램이 지우거나 복사하지 못하면 렌더링 대상 뷰 또는 깊이 스텐실 뷰의 하드웨어 최적화 구조가 진부해져서 일부 하드웨어에서는 렌더링 품질이 떨어질 뿐만 아니라 다른 하드웨어로 인한 불일치까지 발생합니다. 이렇게 하드웨어에서 사용하는 숨겨진 최적화 데이터 구조는 개별 매핑에 대해 로컬이어서 동일한 메모리에 대한 다른 매핑에 보이지 않을 수 있습니다.

리소스 뷰를 지우면(리소스 뷰의 모든 요소를 하나의 값으로 설정) 직사각형으로 렌더링 대상 뷰를 지울 수 있습니다. 스트리밍 리소스를 지원하는 하드웨어에서는 리소스 뷰의 지우기 기능 역시 깊이 전용 표면에 대해(스텐실 제외) 직사각형을 이용한 깊이 스텐실 뷰의 지우기를 지원해야 합니다. 이 연산으로 응용 프로그램은 표면에서 필요한 영역만 지울 수 있습니다.

응용 프로그램이 스트리밍 리소스에서 매핑이 변경된 영역의 기존 메모리 내용을 보존해야 하는 경우에는 해당 응용 프로그램이 지우기 요건을 해결해야 합니다. 응용 프로그램은 먼저 (변경된 내용을 임시 표면으로 복사하여) 타일 매핑이 변경된 내용을 저장하고 필요한 지우기 명령을 실행한 다음 내용을 다시 복사하여 이 요건을 해결할 수 있습니다. 이렇게 증분 렌더링을 위해 표면 내용을 보존할 수 있지만 렌더링 최적화가 손실되어 이후 표면에 대한 렌더링 성능이 저하될 수도 있습니다.

타일이 다수의 스트리밍 리소스에 동시에 매핑되고 스트리밍 리소스 중 하나를 통해 어떤 수단(렌더링, 복사 등)으로든 타일 내용이 조작되는 경우, 동일한 타일을 다른 스트리밍 리소스를 통해 렌더링하려면 앞에서 설명한 대로 타일을 먼저 지워야 합니다.

## <a name="span-idrenderingtotilessharedoutsiderenderareaspanspan-idrenderingtotilessharedoutsiderenderareaspanspan-idrenderingtotilessharedoutsiderenderareaspanrendering-to-tiles-shared-outside-render-area"></a><span id="Rendering_to_tiles_shared_outside_render_area"></span><span id="rendering_to_tiles_shared_outside_render_area"></span><span id="RENDERING_TO_TILES_SHARED_OUTSIDE_RENDER_AREA"></span>렌더링 영역 외부의 공유 타일로 렌더링


예를 들어 스트리밍 리소스의 영역이 렌더링되고, 렌더링 영역에서 참조하는 타일 풀의 타일 역시 렌더링 영역 외부에서 매핑된다고 가정하겠습니다(동시든 아니든 다른 스트리밍 리소스를 통하는 경우도 포함). 이러한 타일로 렌더링되는 데이터는 기본 메모리 레이아웃이 호환 가능하더라도 나머지 매핑을 통해 확인했을 때 올바로 표시된다는 보장이 없습니다. 이는 일부 하드웨어에서 사용하는 최적화 데이터 구조가 렌더링 가능한 표면의 개별 매핑에 로컬이지만 동일한 메모리 위치에 대한 다른 매핑에게는 보이지 않는다는 사실에 기인합니다.

이러한 제약은 렌더링되는 매핑에서 액세스 가능한 동일한 메모리에 대한 다른 모든 매핑으로 복사하여 해결할 수 있습니다(또는 이전 콘텐츠가 더 이상 필요 없는 경우에는 해당 메모리를 지우거나 다른 데이터를 복사하는 방법도 있음). 이와 같은 해결책이 소용 없을 것 같아 보이지만 동일한 메모리에 대한 다른 모든 매핑이 콘텐츠 액세스 방법을 정확히 이해할 뿐만 아니라 적어도 물리적 메모리를 하나만 사용하여 메모리를 절감하는 효과 역시 그대로입니다.

또한 매핑을 공유하는 다른 스트리밍 리소스를 서로 바꿔가며 사용할 때도(읽기만 하지 않는 경우) 다수의 타일식 리소스 사이에 데이터 액세스 순서를 정하는 제약 조건(장벽)을 지정해야 합니다.

## <a name="span-idrenderingtotilessharedwithinrenderareaspanspan-idrenderingtotilessharedwithinrenderareaspanspan-idrenderingtotilessharedwithinrenderareaspanrendering-to-tiles-shared-within-render-area"></a><span id="Rendering_to_tiles_shared_within_render_area"></span><span id="rendering_to_tiles_shared_within_render_area"></span><span id="RENDERING_TO_TILES_SHARED_WITHIN_RENDER_AREA"></span>렌더링 영역 내부의 공유 타일로 렌더링


다수의 타일이 동일한 타일 풀 위치로 매핑되는 렌더링 영역 내부로 스트리밍 리소스의 영역이 렌더링되는 경우에는 이 타일에 대한 렌더링 결과를 알 수 없습니다.

## <a name="span-iddatacompatibilityacrossstreamingresourcessharingtilesspanspan-iddatacompatibilityacrossstreamingresourcessharingtilesspanspan-iddatacompatibilityacrossstreamingresourcessharingtilesspandata-compatibility-across-streaming-resources-sharing-tiles"></a><span id="Data_compatibility_across_streaming_resources_sharing_tiles"></span><span id="data_compatibility_across_streaming_resources_sharing_tiles"></span><span id="DATA_COMPATIBILITY_ACROSS_STREAMING_RESOURCES_SHARING_TILES"></span>스트리밍 리소스 공유 타일 간 데이터 호환성


다수의 스트리밍 리소스가 동일한 타일 풀 위치에 대한 매핑을 가지고 있고, 각 리소스가 동일한 데이터에 액세스하는 데 사용된다고 가정하겠습니다. 이 시나리오는 하드웨어 최적화 구조에 대한 문제를 방지할 수 있는 나머지 규칙을 회피하고, 적절한 장벽이 지정되고(다수의 타일식 리소스 간 데이터 액세스 순서를 정하는 제약 조건 지정), 스트리밍 리소스가 서로 호환된다는 전제 하에서만 유효합니다.

여기에서는 스트리밍 리소스 공유 타일이 호환되지 않을 경우 어떤 문제가 생기는지 설명하겠습니다. 중복 타일 매핑 사이에 동일한 데이터에 액세스하는 비호환성 조건은 사용하는 표면 크기 또는 형식이 다르거나, 혹은 리소스에서 렌더링 대상 및 깊이 스텐실 바인드 플래그가 다를 때입니다. 이어서 호환되지 않는 리소스에서 매핑을 통해 읽거나 렌더링하면 매핑 유형이 한 가지인 메모리에 대한 쓰기 연산의 결과는 알 수 없습니다.

나머지 리소스 공유 매핑이 새로운 데이터로 처음 초기화되는 경우에는(다른 용도의 메모리 재활용) 데이터가 호환되지 않는 해석으로 유출되지 않기 때문에 후속 읽기 또는 렌더링 연산이 괜찮습니다. 하지만 이처럼 비보환 매핑에 대한 액세스 사이를 서로 전환하려면 장벽을 지정해야 합니다(다수의 타일식 리소스 간 데이터 액세스 순서를 정하는 제약 조건 지정).

렌더링 대상 또는 깊이 스텐실 바인드 플래그가 어떤 리소스 공유 매핑에도 서로 설정되지 않으면 제약 조건은 훨씬 더 줄어듭니다. 형식 및 표면 유형(예: Texture2D)이 동일하기만 하다면 타일을 공유할 수 있습니다. 호환 가능한 다른 형식은 BC\* 표면을 비롯해 BC6H 및 R32G32B32A32 같이 요소 형식당 동일한 크기의 비압축 32비트 또는 16비트 같은 경우입니다. 요소 형식당 32비트는 다수가 R32\_\*라는 별칭으로 불릴 수 있습니다(R10G10B10A2\_\*, R8G8B8A8\_\*, B8G8R8A8\_\*,B8G8R8X8\_\*,R16G16\_\*). 이러한 연산은 항상 비스트리밍 리소스에서 허용되어 왔습니다.

형식이 호환되고, 타일이 솔리드 색상으로 채워져 있다면 압축 타일과 비압축 타일 간 공유도 문제 없습니다.

마지막으로 렌더링 대상 또는 깊이 스텐실 바인드 플래그가 없다는 것을 제외하고 리소스 공유 타일 매핑에 대한 공통점이 없는 경우에는 0으로 채워진 메모리만 안전하게 공유할 수 있습니다. 그러면 임의의 리소스 형식(일반적으로 0)에 대한 정의로 0이 무엇을 디코딩하든지 매핑이 표시됩니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스에 대한 파이프라인 액세스](pipeline-access-to-streaming-resources.md)

 

 




