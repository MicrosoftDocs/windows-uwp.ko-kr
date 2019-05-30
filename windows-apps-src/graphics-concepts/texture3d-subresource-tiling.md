---
title: Texture3D 하위 리소스 타일링
description: 다음은 Texture3D 하위 리소스 타일 방식을 나타낸 표입니다.
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- Texture3D 하위 리소스 타일링
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5b63fdeeffd4b95afab6556b6f0318732ff988b0
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370900"
---
# <a name="texture3d-subresource-tiling"></a>Texture3D 하위 리소스 타일링


다음은 [**Texture3D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture3d) 하위 리소스 타일 방식을 나타낸 표입니다. 표에 표기된 값에 꼬리 MIP 압축은 포함되지 않았습니다.

이 표는 [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d) 타일을 가져와서 x/y 크기를 각각 4로 나눈 다음 16개의 깊이 레이어를 더한 것입니다. 첫 번째 평면(첫 번째 16개의 깊이 레이어를 정의하는 타일이 2D 평면)의 모든 타일이 후속 평면 이전에 표시됩니다.

**참고**  스트리밍 리소스의 초기 구현 시   [**Texture3D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture3d)가 스트리밍 리소스에서 지원되지는 않지만 향후 리소스에서 지원할 수 있도록 원하는 타일 모양이 여기에 나열됩니다.

 

| 비트/픽셀(1샘플/픽셀) | 타일 크기(픽셀, WxHxD) |
|-----------------------------|---------------------------------|
| 8                           | 64x32x32                        |
| 16                          | 32x32x32                        |
| 32                          | 32x32x16                        |
| 64                          | 32x16x16                        |
| 128                         | 16x16x16                        |
| BC1,4                       | 128x64x16                       |
| BC2,3,5,6,7                 | 64x64x16                        |

 

리소스를 스트리밍 지원 되지 않습니다 하는 형식이 비트 수는 96 bpp 형식, 비디오 형식 DXGI\_형식\_R1\_UNORM, DXGI\_형식\_R8G8\_B8G8\_UNORM, 및 DXGI\_형식\_R8R8\_G8B8\_UNORM 합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련된 항목


[리소스의 영역을 바둑판식으로 스트리밍 하는 방법](how-a-streaming-resource-s-area-is-tiled.md)

 

 




