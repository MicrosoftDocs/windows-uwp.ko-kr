---
title: 비매핑된 타일을 사용하는 래스터라이저 동작
description: 이 섹션에서는 비매핑된 타일을 사용하는 래스터라이저 동작을 설명합니다.
ms.assetid: AC7B818D-E52B-4727-AEA2-39CFDC279CE7
keywords:
- 비매핑된 타일을 사용하는 래스터라이저 동작
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e3089444820f990644526eaafb7f2ef9007fa70a
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8708331"
---
# <a name="span-iddirect3dconceptsrasterizerbehaviorwithnon-mappedtilesspanrasterizer-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.rasterizer_behavior_with_non-mapped_tiles"></span>비매핑된 타일을 사용하는 래스터라이저 동작


이 섹션에서는 비매핑된 타일을 사용하는 래스터라이저 동작을 설명합니다.

## <a name="span-iddepthstencilviewspanspan-iddepthstencilviewspanspan-iddepthstencilviewspandepthstencilview"></a><span id="DepthStencilView"></span><span id="depthstencilview"></span><span id="DEPTHSTENCILVIEW"></span>DepthStencilView


DSV(깊이 스텐실 뷰) 읽기 및 쓰기 동작은 하드웨어 지원 수준에 따라 다릅니다. 자세한 요구 사항은 [스트리밍 리소스 기능 계층](streaming-resources-features-tiers.md)의 전반적인 읽기 및 쓰기 동작을 참조하세요.

이상적 동작은 다음과 같습니다.

타일이 DepthStencilView에 매핑되지 않은 경우, 깊이 읽기의 반환 값은 0이며 깊이 읽기 값에 대해 구성된 일체의 연산에 제공됩니다. 누락된 깊이 타일에 대한 쓰기는 삭제됩니다. 쓰기 처리에 대한 이러한 이상적 정의는 [계층 2](tier-2.md)에 필요하지 않습니다. 매핑되지 않은 타일에 대한 쓰기는 후속 읽기에서 선택할 수 있는 캐시에 남게 됩니다.

## <a name="span-idrendertargetviewspanspan-idrendertargetviewspanspan-idrendertargetviewspanrendertargetview"></a><span id="RenderTargetView"></span><span id="rendertargetview"></span><span id="RENDERTARGETVIEW"></span>RenderTargetView


RTV(렌더링 대상 뷰) 읽기 및 쓰기 동작은 하드웨어 지원 수준에 따라 다릅니다. 자세한 요구 사항은 [스트리밍 리소스 기능 계층](streaming-resources-features-tiers.md)의 전반적인 읽기 및 쓰기 동작을 참조하세요.

모든 구현에서 동시에 바인딩되는 RTV(및 DSV)마다 매핑된 영역 대 비매핑된 영역의 비율은 서로 다를 수 있으며, 서로 다른 크기의 표면 형식(타일 모양이 다름)을 가질 수 있습니다.

이상적 동작은 다음과 같습니다.

RTV의 읽기가 누락 타일에서 0을 반환하고 쓰기가 누락됩니다. 쓰기 처리에 대한 이러한 이상적 정의는 [계층 2](tier-2.md)에 필요하지 않습니다. 매핑되지 않은 타일에 대한 쓰기는 후속 읽기에서 선택할 수 있는 캐시에 남게 됩니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스에 대한 파이프라인 액세스](pipeline-access-to-streaming-resources.md)

 

 




