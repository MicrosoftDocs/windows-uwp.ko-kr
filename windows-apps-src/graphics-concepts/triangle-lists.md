---
title: 삼각형 목록
description: 삼각형 목록은 격리된 삼각형의 목록입니다. 격리된 삼각형은 서로 가까울 수도 있고 가깝지 않을 수도 있습니다. 삼각형 목록에는 3개 이상의 꼭짓점이 있어야 하고 꼭짓점의 총수는 3의 배수여야 합니다.
ms.assetid: BC50D532-9E9C-4AAE-B466-9E8C4AD1862A
keywords:
- 삼각형 목록
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 53fd2b132fda018030b7555a9cdac718ec1f1cc4
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291561"
---
# <a name="triangle-lists"></a>삼각형 목록

삼각형 목록은 격리된 삼각형의 목록입니다. 격리된 삼각형은 서로 가까울 수도 있고 가깝지 않을 수도 있습니다. 삼각형 목록에는 3개 이상의 꼭짓점이 있어야 하고 꼭짓점의 총수는 3의 배수여야 합니다.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>예제


삼각형 목록을 사용하여 분리된 조각으로 구성된 개체를 만듭니다. 예를 들어 3D 게임에서 역장 벽을 만드는 한 가지 방법은 연결되지 않은 소형 삼각형을 대량으로 지정하는 것입니다. 그런 다음 삼각형 목록에 빛을 방출하는 재질 및 텍스처를 적용합니다. 그러면 벽에 있는 각 삼각형이 빛나게 됩니다. 사용자가 역장을 볼 때 삼각형 사이의 간격을 통해 벽 뒤의 장면을 부분적으로 볼 수 있습니다.

삼각형 목록은 가장자리가 날카롭고 고우러드 음영으로 음영 처리된 기본 형식을 만드는 데도 유용합니다. [표면 및 꼭짓점 일반 벡터](face-and-vertex-normal-vectors.md)를 참조하세요.

다음 그림은 렌더링된 삼각형 목록을 보여 줍니다.

![렌더링된 삼각형 목록 그림](images/trilist.png)

다음 코드는 이 삼각형 목록의 꼭짓점을 만드는 방법을 보여 줍니다.

```cpp
struct CUSTOMVERTEX
{
    float x,y,z;
};

CUSTOMVERTEX Vertices[] = 
{
    {-5.0, -5.0, 0.0},
    { 0.0,  5.0, 0.0},
    { 5.0, -5.0, 0.0},
    {10.0,  5.0, 0.0},
    {15.0, -5.0, 0.0},
    {20.0,  5.0, 0.0}

};
```

아래의 코드 예제는 이 삼각형 목록을 Direct3D로 렌더링하는 방법을 보여 줍니다.

```cpp
//
// It is assumed that d3dDevice is a valid
// pointer to a device interface.
//
d3dDevice->DrawPrimitive( D3DPT_TRIANGLELIST, 0, 2 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련된 항목


[기본 형식](primitives.md)

 

 




