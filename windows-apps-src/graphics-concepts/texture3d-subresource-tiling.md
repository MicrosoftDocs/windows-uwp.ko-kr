---
title: "Texture3D 하위 리소스 타일링"
description: "다음은 Texture3D 하위 리소스 타일 방식을 나타낸 표입니다."
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords: "Texture3D 하위 리소스 타일링"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: d51d20ddaeca5aa0689104b3dd71e36b1a5d4132
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/22/2017
---
# <a name="texture3d-subresource-tiling"></a>Texture3D 하위 리소스 타일링


다음은 [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) 하위 리소스 타일 방식을 나타낸 표입니다. 표에 표기된 값에 꼬리 MIP 압축은 포함되지 않았습니다.

이 표는 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 타일을 가져와서 x/y 크기를 각각 4로 나눈 다음 16개의 깊이 레이어를 더한 것입니다. 첫 번째 평면(첫 번째 16개의 깊이 레이어를 정의하는 타일이 2D 평면)의 모든 타일이 후속 평면 이전에 표시됩니다.

**참고**  스트리밍 리소스의 초기 구현 시 [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562)가 스트리밍 리소스에서 지원되지는 않지만 향후 리소스에서 지원할 수 있도록 원하는 타일 모양이 여기에 나열됩니다.

 

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

 

 




