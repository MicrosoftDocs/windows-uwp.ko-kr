---
title: 버퍼 타일링
description: 버퍼 리소스는 64KB의 크기로 분할되며, 크기가 64KB의 곱이 아닌 경우 마지막 타일에 약간의 빈 공간이 포함됩니다.
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- 버퍼 타일링
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 03769964bfe3eff13314e62b8594edd5509b26fb
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044252"
---
# <a name="buffer-tiling"></a>버퍼 타일링


[버퍼](introduction-to-buffers.md) 리소스는 64KB의 크기로 분할되며, 크기가 64KB의 곱이 아닌 경우 마지막 타일에 약간의 빈 공간이 포함됩니다.

구조적 버퍼는 바둑판식으로 표시할 stride에 대한 제약 조건이 없어야 합니다. 그러나 먼저 stride를 바둑판식으로 표시되게 만들어 [**StructuredBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff471514) 사용을 위해 하드웨어에서 가능한 성능 최적화를 희생해야 할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 영역의 타일링 방법](how-a-streaming-resource-s-area-is-tiled.md)

 

 



