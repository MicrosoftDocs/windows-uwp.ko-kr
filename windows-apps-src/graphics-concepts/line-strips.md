---
title: 선 스트립
description: 선 스트립은 연결된 선 세그먼트로 구성되는 기본 요소입니다. 응용 프로그램은 닫히지 않은 다각형을 만드는 데 선 스트립을 사용할 수 있습니다. 닫힌 다각형은 마지막 꼭짓점이 선 세그먼트에 의해 첫 번째 꼭짓점에 연결되는 다각형입니다.
ms.assetid: 6E8C58E1-B463-44FD-A69F-81CCBF25D856
keywords:
- 선 스트립
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d17a79c14e981ab0c2c0414074aff17c90a0b478
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291631"
---
# <a name="line-strips"></a>선 스트립

선 스트립은 연결된 선 세그먼트로 구성되는 기본 요소입니다. 응용 프로그램은 닫히지 않은 다각형을 만드는 데 선 스트립을 사용할 수 있습니다. 닫힌 다각형은 마지막 꼭짓점이 선 세그먼트에 의해 첫 번째 꼭짓점에 연결되는 다각형입니다. 응용 프로그램이 선 스트립을 기반으로 다각형을 만들면 꼭짓점이 동일 평면상에 있지 않을 수 있습니다.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>예제


다음 그림은 렌더링된 선 스트립을 보여줍니다.

![선 스트립 그림](images/linstrip.gif)

다음 코드는 이 선 스트립의 꼭짓점을 만드는 방법을 보여줍니다.

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

아래의 코드 예제는 Direct3D에서 선 스트립을 렌더링하는 방법을 보여줍니다.

```cpp
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_LINESTRIP, 0, 5 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련된 항목


[기본 형식](primitives.md)

 

 




