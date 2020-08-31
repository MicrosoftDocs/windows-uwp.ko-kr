---
title: Texture3D 하위 리소스 타일링
description: Texture2D 바둑판식 배열의 픽셀당 비트 수를 기준으로 Texture3D 하위가 바둑판식으로 배열 되는 방법을 보여 주는 표를 참조 하세요.
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- Texture3D 하위 리소스 타일링
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9ee4ff5c87022f9fd303b1331665a2551704cb93
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167947"
---
# <a name="texture3d-subresource-tiling"></a>Texture3D 하위 리소스 타일링


다음 표에서는 [**Texture3D**](/windows/desktop/direct3dhlsl/sm5-object-texture3d) 하위를 바둑판식으로 배열 하는 방법을 보여 줍니다. 이 테이블의 값은 tail 밉 압축을 계산 하지 않습니다.

이 표에서는 [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) 바둑판식 배열을 사용 하 여 x/y 차원을 각각 4로 나누고 깊이를 16 개 계층으로 추가 합니다. 첫 번째 평면 (깊이의 첫 16 개 계층을 정의 하는 타일의 2D 평면)의 모든 타일이 후속 평면 앞에 표시 됩니다.

**참고**스트리밍 리소스의  [**Texture3D**](/windows/desktop/direct3dhlsl/sm5-object-texture3d) 지원은 스트리밍 리소스의 초기 구현에서 노출 되지 않지만 향후 릴리스에서 지원 될 수 있도록 원하는 타일 셰이프가 여기에 나열 되어 있습니다.

 

| 비트/픽셀 (1 개 샘플/픽셀) | 타일 차원 (픽셀, WxHxD) |
|-----------------------------|---------------------------------|
| 8                           | 64x32x32                        |
| 16                          | 32x32x32                        |
| 32                          | 32x32x16                        |
| 64                          | 32x16x16                        |
| 128                         | 16x16x16                        |
| BC1, 4                       | 128x64x16                       |
| BC2, 3, 5, 6, 7                 | 64x64x16                        |

 

스트리밍 리소스에서 지원 되지 않는 형식 비트 수는 96 bpp 형식, 비디오 형식, DXGI \_ 형식 \_ R1- \_ orm, dxgi \_ format \_ R8G8 \_ B8G8 \_ unorm 및 dxgi \_ 형식 \_ R8R8 \_ G8B8 \_ unorm입니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[스트리밍 리소스 영역의 타일링 방법](how-a-streaming-resource-s-area-is-tiled.md)

 

 