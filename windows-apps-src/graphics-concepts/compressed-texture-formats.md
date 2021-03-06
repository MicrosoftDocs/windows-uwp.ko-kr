---
title: 압축된 텍스처 형식
description: 이 섹션에는 압축된 텍스처 형식의 내부 구성에 대한 정보가 있습니다.
ms.assetid: 24D17B9F-8CA7-4006-9E0F-178C6B3CAEC9
keywords:
- 압축된 텍스처 형식
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3171eba376911157a6ad2687fe3879df751615ac
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662198"
---
# <a name="compressed-texture-formats"></a>압축된 텍스처 형식


이 섹션에는 압축된 텍스처 형식의 내부 구성에 대한 정보가 있습니다. 압축된 형식으로 또는 압축된 형식에서 변환에 Direct3D 기능을 사용할 수 있으므로 압축된 텍스처 사용을 위해 이러한 세부 사항이 필요하지는 않습니다. 그러나 이 정보는 압축된 표면 데이터에서 직접 작업하려는 경우 유용합니다.

Direct3D는 텍스처 맵을 4x4 텍셀 블록으로 나누는 압축 형식을 사용합니다. 텍스처에 투명성이 없고 텍스처가 불투명하거나 1비트 알파로 투명성이 지정되는 경우 8바이트 블록이 텍스처 맵 블록을 나타냅니다. 텍스처 맵에 투명 텍셀이 없는 경우 알파 채널을 사용하여 16바이트 블록이 이를 나타냅니다.

단일 텍스처는 해당 데이터가 16개 텍셀 그룹당 64비트나 128비트로 저장되도록 지정해야 합니다. 64비트 블록(형식 BC1)이 텍스처에 사용되는 경우 같은 텍스처 내에 블록별로 불투명 및 1비트 알파 형식을 혼합하는 것이 가능합니다. 즉, 색의 부호 없는 정수 크기의 비교\_0과 색\_1 16 텍셀의 각 블록에 대해 고유 하 게 수행 됩니다.

128비트 블록이 사용되면 전체 텍스처에 대해 명시적(형식 BC2) 또는 보간 모드(형식 BC3)에 알파 채널을 지정해야 합니다. 색과 마찬가지로 보간된 모드를 선택하면 8개의 보간된 알파나 6개의 보간된 알파 모드를 블록 단위로 사용할 수 있습니다. 알파 크기 비교 다시\_0 및 알파\_1 블록 단위로 별로 고유 하 게 수행 됩니다.

BCn 형식에 대한 피치는 블록이 아닌 바이트 단위로 측정됩니다. 예를 들어, 너비가 16 경우 하면 4 개 블록의 피치 (4\*BC1, 4 8\*BC2 또는 BC3 16).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련된 항목


[압축 된 질감 리소스](compressed-texture-resources.md)

 

 




