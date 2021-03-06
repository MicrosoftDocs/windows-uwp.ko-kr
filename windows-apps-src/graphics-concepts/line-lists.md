---
title: 선 목록
description: 선 목록은 격리된 선 세그먼트의 목록입니다. 선 목록은 진눈깨비나 폭우를 3D 장면에 추가하는 등의 작업에 유용합니다. 응용 프로그램은 꼭짓점 배열을 채워 선 목록을 만듭니다.
ms.assetid: 42BF32A1-3535-42A3-82C5-3945CB309F2C
keywords:
- 선 목록
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ac66066a4140ace5905ff6bc52a7b1290341beea
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291591"
---
# <a name="line-lists"></a>선 목록

선 목록은 격리된 선 세그먼트의 목록입니다. 선 목록은 진눈깨비나 폭우를 3D 장면에 추가하는 등의 작업에 유용합니다. 응용 프로그램은 꼭짓점 배열을 채워 선 목록을 만듭니다. 선 목록에서 꼭짓점의 개수는 2 이상의 짝수여야 합니다.

-   [예제](#example)
-   [관련된 항목](#related-topics)

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>예제


다음 그림은 렌더링된 선 목록을 보여줍니다.

![선 목록의 그림](images/linelst.png)

선 목록에 재료와 텍스처를 적용할 수 있습니다. 재료나 텍스처의 색상은 선 사이의 점이 아닌 그려진 선을 따라서만 표시됩니다.

다음 코드는 이 선 목록의 꼭짓점을 만드는 방법을 보여줍니다.

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

아래의 코드 예제는 Direct3D에서 선 목록을 렌더링하는 방법을 보여줍니다.

```cpp
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_LINELIST, 0, 3 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련된 항목


[기본 형식](primitives.md)

 

 




