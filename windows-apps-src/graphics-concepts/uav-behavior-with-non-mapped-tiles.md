---
title: 비매핑된 타일을 사용하는 UAV 동작
description: UAV(정렬되지 않은 액세스 보기) 읽기 및 쓰기 동작은 하드웨어 지원 수준에 따라 다릅니다.
ms.assetid: CDB224E2-CC07-4568-9AAC-C8DC74536561
keywords:
- 비매핑된 타일을 사용하는 UAV 동작
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c72931d2f6bf1e892e174bc409f20a042d5e4c92
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631628"
---
# <a name="span-iddirect3dconceptsuavbehaviorwithnon-mappedtilesspanuav-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.uav_behavior_with_non-mapped_tiles"></span>비 매핑된 타일을 사용 하 여 UAV 동작


UAV(정렬되지 않은 액세스 보기) 읽기 및 쓰기 동작은 하드웨어 지원 수준에 따라 다릅니다. 자세한 요구 사항은 [스트리밍 리소스 기능 계층](streaming-resources-features-tiers.md)의 전반적인 읽기 및 쓰기 동작을 참조하세요. 이 섹션에서는 가장 이상적인 동작을 요약합니다.

UAV에 있는 매핑되지 않은 타일에서 읽는 셰이더 작업은 형식의 누락되지 않은 모든 구성 요소에서 0을 반환하고 누락된 구성 요소에 대해서는 기본값을 반환합니다.

매핑되지 않은 타일에 쓰기를 시도하는 셰이더 작업은 매핑되지 않은 영역에 아무 것도 쓰지 못하는 반면 매핑된 영역에 대한 쓰기 작업은 계속 진행됩니다. 쓰기 처리에 대한 이러한 이상적 정의는 [계층 2](tier-2.md)에 필요하지 않습니다. 매핑되지 않은 타일에 대한 쓰기는 후속 읽기에서 선택할 수 있는 캐시에 남게 됩니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련된 항목


[리소스를 스트리밍에 대 한 파이프라인 액세스](pipeline-access-to-streaming-resources.md)

 

 




