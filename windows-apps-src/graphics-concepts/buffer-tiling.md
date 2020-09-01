---
title: 버퍼 타일링
description: 버퍼 리소스는 64KB 타일로 나뉘어 크기가 64KB의 배수가 아닌 경우 마지막 타일에 몇 개의 빈 공간을 포함 합니다.
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- 버퍼 타일링
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fdb78dc2cff6ccedec58acbc946068e14511fdc2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156357"
---
# <a name="buffer-tiling"></a>버퍼 타일링


[버퍼](introduction-to-buffers.md) 리소스는 64kb 타일로 나뉘어 크기가 64kb의 배수가 아닌 경우 마지막 타일에 몇 개의 빈 공간을 포함 합니다.

구조적 버퍼에는 바둑판식으로 표시할 stride에 대 한 제약 조건이 없어야 합니다. 하지만 [**StructuredBuffers**](/windows/desktop/direct3dhlsl/sm5-object-structuredbuffer) 를 사용 하기 위한 하드웨어에서 가능한 성능 최적화는 첫 번째 장소에서 바둑판식으로 만들어 영향을 받을 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 영역의 타일링 방법](how-a-streaming-resource-s-area-is-tiled.md)

 

 