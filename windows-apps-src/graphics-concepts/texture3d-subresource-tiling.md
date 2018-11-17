---
title: Texture3D 하위 리소스 타일링
description: 다음은 Texture3D 하위 리소스 타일 방식을 나타낸 표입니다.
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- Texture3D 하위 리소스 타일링
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 17970d509fa2bf6b80431e1c07b5d135c7dcb112
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7153096"
---
# <a name="texture3d-subresource-tiling"></a>Texture3D 하위 리소스 타일링


다음은 [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) 하위 리소스 타일 방식을 나타낸 표입니다. 표에 표기된 값에 꼬리 MIP 압축은 포함되지 않았습니다.

이 표는 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 타일을 가져와서 x/y 크기를 각각 4로 나눈 다음 16개의 깊이 레이어를 더한 것입니다. 첫 번째 평면(첫 번째 16개의 깊이 레이어를 정의하는 타일이 2D 평면)의 모든 타일이 후속 평면 이전에 표시됩니다.

**참고** 스트리밍 리소스의 초기 구현에서 스트리밍 리소스의 [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) 지원 않지만 원하는 타일 모양이 여기에 향후 릴리스에서 지원할 나열 됩니다.

 

| 비트/픽셀(1샘플/픽셀) | 타일 크기(픽셀, WxHxD) |
|-----------------------------|---------------------------------|
| 8                           | 64x32x32                        |
| 16                          | 32x32x32                        |
| 32                          | 32x32x16                        |
| 64                          | 32x16x16                        |
| 128                         | 16x16x16                        |
| BC1,4                       | 128x64x16                       |
| BC2,3,5,6,7                 | 64x64x16                        |

 

스트리밍 리소스로 지원되지 않는 포맷 비트 개수는 96 bpp 포맷, 비디오 포맷, DXGI\_FORMAT\_R1\_UNORM, DXGI\_FORMAT\_R8G8\_B8G8\_UNORM, 그리고 DXGI\_FORMAT\_R8R8\_G8B8\_UNORM입니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 영역의 타일링 방법](how-a-streaming-resource-s-area-is-tiled.md)

 

 




