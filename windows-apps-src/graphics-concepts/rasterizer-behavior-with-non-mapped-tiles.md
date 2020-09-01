---
title: 비매핑된 타일을 사용하는 래스터라이저 동작
description: 매핑되지 않은 타일이 있는 DepthStencilView (DSV) 및 RenderTargetView (RTV) 래스터 라이저 동작에 대해 알아봅니다.
ms.assetid: AC7B818D-E52B-4727-AEA2-39CFDC279CE7
keywords:
- 비매핑된 타일을 사용하는 래스터라이저 동작
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f8085d8d29a86c0c5da82f6cb98c57c037b81beb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171887"
---
# <a name="span-iddirect3dconceptsrasterizer_behavior_with_non-mapped_tilesspanrasterizer-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.rasterizer_behavior_with_non-mapped_tiles"></span>비매핑된 타일을 사용하는 래스터라이저 동작


이 섹션에서는 매핑되지 않은 타일의 래스터 동작에 대해 설명 합니다.

## <a name="span-iddepthstencilviewspanspan-iddepthstencilviewspanspan-iddepthstencilviewspandepthstencilview"></a><span id="DepthStencilView"></span><span id="depthstencilview"></span><span id="DEPTHSTENCILVIEW"></span>DepthStencilView


DSV (깊이 스텐실 뷰) 읽기 및 쓰기 동작은 하드웨어 지원 수준에 따라 달라 집니다. 요구 사항에 대 한 분석은 [스트리밍 리소스 기능 계층](streaming-resources-features-tiers.md)에 대 한 전반적인 읽기 및 쓰기 동작을 참조 하세요.

다음은 이상적인 동작입니다.

타일이 DepthStencilView에서 매핑되지 않은 경우 읽기 깊이의 반환 값은 0 이며,이 값은 깊이 읽기 값에 대해 구성 된 모든 작업에 공급 됩니다. 누락 된 깊이 타일에 대 한 쓰기는 삭제 됩니다. 이에 대 한 적절 한 쓰기 처리 정의는 [계층 2](tier-2.md)에 필요 하지 않습니다. 매핑되지 않은 타일에 대 한 쓰기는 이후 읽기가 선택 될 수 있는 캐시에서 끝날 수 있습니다.

## <a name="span-idrendertargetviewspanspan-idrendertargetviewspanspan-idrendertargetviewspanrendertargetview"></a><span id="RenderTargetView"></span><span id="rendertargetview"></span><span id="RENDERTARGETVIEW"></span>RenderTargetView


RTV (렌더링 대상 보기) 읽기 및 쓰기 동작은 하드웨어 지원 수준에 따라 달라 집니다. 요구 사항에 대 한 분석은 [스트리밍 리소스 기능 계층](streaming-resources-features-tiers.md)에 대 한 전반적인 읽기 및 쓰기 동작을 참조 하세요.

모든 구현에서 동시에 서로 다른 RTVs (및 DSV)는 서로 다른 영역을 매핑할 수 있으며 서로 다른 크기의 표면 형식 (다른 타일 셰이프)을 가질 수 있습니다.

다음은 이상적인 동작입니다.

누락 된 타일에서 RTVs의 읽기는 0을 반환 하 고 쓰기는 삭제 됩니다. 이에 대 한 적절 한 쓰기 처리 정의는 [계층 2](tier-2.md)에 필요 하지 않습니다. 매핑되지 않은 타일에 대 한 쓰기는 이후 읽기가 선택 될 수 있는 캐시에서 끝날 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스에 대한 파이프라인 액세스](pipeline-access-to-streaming-resources.md)

 

 




