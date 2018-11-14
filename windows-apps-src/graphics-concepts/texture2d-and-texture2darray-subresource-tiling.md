---
title: Texture2D 및 Texture2DArray 하위 리소스 타일링
description: 다음은 Texture2D 및 Texture2DArray 하위 리소스 타일 방식을 나타낸 표입니다.
ms.assetid: 2DC14DFC-5299-44D9-895F-5A223D3FD530
keywords:
- Texture2D 및 Texture2DArray 하위 리소스 타일링
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 245581e4eb2a8526b242feadb5877590283e24f9
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6256949"
---
# <a name="texture2d-and-texture2darray-subresource-tiling"></a>Texture2D 및 Texture2DArray 하위 리소스 타일링


다음은 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 및 [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) 하위 리소스 타일 방식을 나타낸 표입니다. 표에 표기된 값에 꼬리 MIP 압축은 포함되지 않았습니다.

## <a name="span-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spansubresources-with-multisample-counts-of-1"></a><span id="Subresources-with-multisample-counts-of-1"></span><span id="subresources-with-multisample-counts-of-1"></span><span id="SUBRESOURCES-WITH-MULTISAMPLE-COUNTS-OF-1"></span>다중 샘플 개수가 1개인 하위 리소스


다음은 다중 샘플 수가 1개인 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 및 [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) 하위 리소스 타일 방식을 나타낸 표입니다.

| 비트/픽셀(1샘플/픽셀) | 타일 크기(픽셀, WxH) |
|-----------------------------|-------------------------------|
| 8                           | 256x256                       |
| 16                          | 256x128                       |
| 32                          | 128x128                       |
| 64                          | 128x64                        |
| 128                         | 64x64                         |
| BC1,4                       | 512x256                       |
| BC2,3,5,6,7                 | 256x256                       |

 

스트리밍 리소스로 지원되지 않는 포맷 비트 개수는 96 bpp 포맷, 비디오 포맷, DXGI\_FORMAT\_R1\_UNORM, DXGI\_FORMAT\_R8G8\_B8G8\_UNORM, 그리고 DXGI\_FORMAT\_R8R8\_G8B8\_UNORM입니다.

## <a name="span-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspansubresources-with-various-multisample-counts"></a><span id="Subresources-with-various-multisample-counts"></span><span id="subresources-with-various-multisample-counts"></span><span id="SUBRESOURCES-WITH-VARIOUS-MULTISAMPLE-COUNTS"></span>다중 샘플 개수가 여러 개인 하위 리소스


다음은 다중 샘플 수가 여러 개인 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 및 [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) 하위 리소스 타일 방식을 나타낸 표입니다.

| 비트/픽셀(1샘플/픽셀) | 타일 크기(픽셀, WxH) |
|-----------------------------|-------------------------------|
| 1                           | 1x1                           |
| 2                           | 2x1                           |
| 4                           | 2x2                           |
| 8                           | 4x2                           |
| 16                          | 4x4                           |

 

스트리밍 리소스로 지원하기 위해서는 샘플 개수 1개와 4개만 필요(허용)합니다. 2, 8 및 16도 표에 있기는 하지만 스트리밍 리소스는 현재 2, 8 및 16을 지원하지 않습니다.

단, 스트리밍 리소스가 지원하지는 않지만 스트리밍 리소스가 아닌 경우에는 구현할 때 2, 8 및 /또는 16 샘플 MSAA(다중 샘플 앤티앨리어싱) 모드를 지원하도록 선택할 수 있습니다.

샘플 개수가 1보다 큰 스트리밍 리소스는 128 bpp 포맷을 사용할 수 없습니다.

지원되는 샘플 개수와 포맷이 제한적인 이유는 사양에 따라 하드웨어가 다르기 때문입니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 영역의 타일링 방법](how-a-streaming-resource-s-area-is-tiled.md)

 

 




