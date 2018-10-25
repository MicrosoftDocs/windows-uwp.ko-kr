---
title: 비매핑된 타일을 사용하는 SRV 동작
description: 비매핑된 타일과 관련된 셰이더 리소스 뷰(SRV) 읽기 동작은 하드웨어 지원 수준에 따라 다릅니다.
ms.assetid: 0E1D64BE-EB09-4F9D-9800-BD23A3B374EE
keywords:
- 비매핑된 타일을 사용하는 SRV 동작
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cd5a5308504bda662ae3aff1968122fcfe020ca3
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5572562"
---
# <a name="span-iddirect3dconceptssrvbehaviorwithnon-mappedtilesspansrv-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.srv_behavior_with_non-mapped_tiles"></span>비매핑된 타일을 사용하는 SRV 동작


비매핑된 타일과 관련된 셰이더 리소스 뷰(SRV) 읽기 동작은 하드웨어 지원 수준에 따라 다릅니다. 자세한 요구 사항은 [스트리밍 리소스 기능 계층](streaming-resources-features-tiers.md)의 읽기 동작을 참조하세요. 이 섹션에는 [계층 2](tier-2.md)가 필요한 이상적 동작이 요약되어 있습니다.

SRV의 텍셀 집합에서 읽는 텍스처 필터 작업을 생각해 봅시다. 비매핑된 타일에 속하는 텍셀은 매핑된 텍셀의 기여와 함께 전체 필터 작업에 형식의 모든 누락되지 않은 구성 요소에 대해 0(및 누락된 구성 요소에 대해 기본값)을 기여합니다. 텍셀은 데이터가 매핑 또는 비매핑된 타일에서 오는지 여부와 상관없이 모두 가중되고 결합됩니다.

일부 초기 [계층 2](tier-2.md) 수준 하드웨어는 이 사양 요구 사항을 충족하지 않아 비매핑된 타일에 속하는 텍셀(가중치가 0이 아님)이 있는 경우 앞서 설명한 기본값을 포함하여 0을 전체 필터 결과로 반환합니다. 다른 어떤 하드웨어도 필터에 모든 (가중치가 0이 아닌) 텍셀을 포함해야 하는 요구 사항을 누락시킬 수 없습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스에 대한 파이프라인 액세스](pipeline-access-to-streaming-resources.md)

 

 




