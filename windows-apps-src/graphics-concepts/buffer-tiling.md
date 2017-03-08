---
title: "버퍼 타일링"
description: "버퍼 리소스는 64KB의 크기로 분할되며, 크기가 64KB의 곱이 아닌 경우 마지막 타일에 약간의 빈 공간이 포함됩니다."
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- "버퍼 타일링"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 2382974de7146f4ba64c2422a01c4d3d297a8977
ms.lasthandoff: 02/07/2017

---

# <a name="buffer-tiling"></a>버퍼 타일링


[버퍼](introduction-to-buffers.md) 리소스는 64KB의 크기로 분할되며, 크기가 64KB의 곱이 아닌 경우 마지막 타일에 약간의 빈 공간이 포함됩니다.

구조적 버퍼는 바둑판식으로 표시할 stride에 대한 제약 조건이 없어야 합니다. 그러나 먼저 stride를 바둑판식으로 표시되게 만들어 [**StructuredBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff471514) 사용을 위해 하드웨어에서 가능한 성능 최적화를 희생해야 할 수 있습니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 영역의 타일링 방법](how-a-streaming-resource-s-area-is-tiled.md)

 

 





