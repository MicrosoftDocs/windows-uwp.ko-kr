---
title: 점 목록
description: 점 목록은 격리된 점으로 렌더링되는 꼭짓점의 모음입니다. 응용 프로그램은 스타 필드용 3D 장면에서 점 목록을 사용하거나 다각형 표면에서 점선을 사용할 수 있습니다.
ms.assetid: 332954AE-019F-4A91-B773-E3A7C92F3297
keywords:
- 점 목록
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b4c2e0f6e99099df4bdda9452521acf3b0cd2b8f
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5927597"
---
# <a name="point-lists"></a>점 목록


점 목록은 격리된 점으로 렌더링되는 꼭짓점의 모음입니다. 응용 프로그램은 스타 필드용 3D 장면에서 점 목록을 사용하거나 다각형 표면에서 점선을 사용할 수 있습니다.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>예제


다음 그림은 렌더링된 점 목록을 보여 줍니다.

![점 목록 그림](images/pointlst.png)

응용 프로그램은 점 목록에 재료와 텍스처를 적용할 수 있습니다. 재료 또는 텍스처의 색상은 그려진 점에서만 나타나며, 점 사이에서는 나타나지 않습니다.

다음 코드는 이 점 목록에 대해 꼭짓점을 생성하는 방법을 보여 줍니다.

```
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

아래의 코드 예제는 이 점 목록을 Direct3D로 렌더링하는 방법을 보여 줍니다.

```
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_POINTLIST, 0, 6 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[기본 요소](primitives.md)

 

 




