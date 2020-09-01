---
title: Texture2D 및 Texture2DArray 하위 리소스 타일링
description: 다음 표에서는 Texture2D 및 Texture2DArray 하위를 바둑판식으로 배열 하는 방법을 보여 줍니다.
ms.assetid: 2DC14DFC-5299-44D9-895F-5A223D3FD530
keywords:
- Texture2D 및 Texture2DArray 하위 리소스 타일링
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 28152f39983f4831a9efa981efcb85fb65fa0204
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156177"
---
# <a name="texture2d-and-texture2darray-subresource-tiling"></a>Texture2D 및 Texture2DArray 하위 리소스 타일링


다음 표에서는 [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) 및 [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) 하위를 바둑판식으로 배열 하는 방법을 보여 줍니다. 이러한 테이블의 값은 tail 밉 압축을 계산 하지 않습니다.

## <a name="span-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spansubresources-with-multisample-counts-of-1"></a><span id="Subresources-with-multisample-counts-of-1"></span><span id="subresources-with-multisample-counts-of-1"></span><span id="SUBRESOURCES-WITH-MULTISAMPLE-COUNTS-OF-1"></span>다중 샘플 개수가 1 인 하위 리소스


이 표에서는 다중 샘플 개수가 1 인 [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) 및 [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) 하위가 바둑판식으로 배열 되는 방법을 보여 줍니다.

| 비트/픽셀 (1 개 샘플/픽셀) | 타일 크기 (픽셀, WxH) |
|-----------------------------|-------------------------------|
| 8                           | 256x256                       |
| 16                          | 256x128                       |
| 32                          | 128x128                       |
| 64                          | 128x64                        |
| 128                         | 64x64                         |
| BC1, 4                       | 512x256                       |
| BC2, 3, 5, 6, 7                 | 256x256                       |

 

스트리밍 리소스에서 지원 되지 않는 형식 비트 수는 96 bpp 형식, 비디오 형식, DXGI \_ 형식 \_ R1- \_ orm, dxgi \_ format \_ R8G8 \_ B8G8 \_ unorm 및 dxgi \_ 형식 \_ R8R8 \_ G8B8 \_ unorm입니다.

## <a name="span-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspansubresources-with-various-multisample-counts"></a><span id="Subresources-with-various-multisample-counts"></span><span id="subresources-with-various-multisample-counts"></span><span id="SUBRESOURCES-WITH-VARIOUS-MULTISAMPLE-COUNTS"></span>다양 한 다중 샘플 수가 포함 된 하위 리소스


다음 표에서는 다양 한 다중 샘플 수가 있는 [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) 및 [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) 하위의 바둑판식 배열 방식을 보여 줍니다.

| 비트/픽셀 (1 개 샘플/픽셀) | 타일 크기 (픽셀, WxH) |
|-----------------------------|-------------------------------|
| 1                           | 1x1                           |
| 2                           | 2x1                           |
| 4                           | 2x2                           |
| 8                           | 4x2                           |
| 16                          | 4x4                           |

 

스트리밍 리소스로 지원 되는 샘플 개수 1과 4만 필요 합니다 (및 허용). 스트리밍 리소스는 현재 표시 되는 경우에도 2, 8, 16을 지원 하지 않습니다.

구현에서는 스트리밍 리소스에서 지원 하지 않는 경우에도 비 스트리밍 리소스에 대해 2, 8,/또는 16 샘플 다중 샘플 앤티엘리어싱 (MSAA) 모드를 지원 하도록 선택할 수 있습니다.

샘플 수가 1 보다 큰 스트리밍 리소스는 128 bpp 형식을 사용할 수 없습니다.

지원 되는 샘플 개수와 형식에 대 한 제약 조건은 사양의 하드웨어 불일치로 인해 발생 합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 영역의 타일링 방법](how-a-streaming-resource-s-area-is-tiled.md)

 

 