---
title: 삼각형 스트립
description: 삼각형 스트립은 일련의 연결된 삼각형입니다. 삼각형이 연결되었기 때문에 응용 프로그램에서 각 삼각형의 세 꼭짓점을 반복해서 지정할 필요가 없습니다.
ms.assetid: BACC74C5-14E5-4ECC-9139-C2FD1808DB82
keywords:
- 삼각형 스트립
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f60f0f65868d4dec67bf77a329d4b952c20ec44a
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6032691"
---
# <a name="triangle-strips"></a>삼각형 스트립


삼각형 스트립은 일련의 연결된 삼각형입니다. 삼각형이 연결되었기 때문에 응용 프로그램에서 각 삼각형의 세 꼭짓점을 반복해서 지정할 필요가 없습니다. 예를 들어 다음 삼각형 스트립을 정의하려면 꼭짓점 7개만 있으면 됩니다.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>예제


![꼭짓점 7개가 포함된 삼각형 스트립 그림](images/tristrip.png)

시스템에서는 첫 번째 삼각형을 그리는 데 꼭짓점 v1, v2 및 v3를 사용하고, 두 번째 삼각형을 그리는 데 v2, v4 및 v3를 사용하고, 세 번째 삼각형을 그리는 데 v3, v4 및 v5를 사용하고, 네 번째 삼각형을 그리는 데 v4, v6 및 v5를 사용하는 방식을 취합니다. 두 번째와 네 번째 삼각형의 꼭짓점은 작동하지 않습니다. 모든 삼각형을 시계 방향으로 그려야 하기 때문입니다.

3D 장면의 개체는 대부분 삼각형 스트립으로 구성됩니다. 즉, 삼각형 스트립을 사용하여 메모리 및 처리 시간을 효율적으로 사용하는 방식으로 복잡한 개체를 지정할 수 있기 때문입니다.

다음 그림은 렌더링된 삼각형 스트립을 보여 줍니다.

![렌더링된 삼각형 스트립 그림](images/tstrip2.png)

다음 코드는 이 삼각형 스트립의 꼭짓점을 만드는 방법을 보여 줍니다.

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

아래의 코드 예제는 이 삼각형 스트립을 Direct3D로 렌더링하는 방법을 보여 줍니다.

```
//
// It is assumed that d3dDevice is a valid
// pointer to a device interface.
//
d3dDevice->DrawPrimitive( D3DPT_TRIANGLESTRIP, 0, 4);
```

## <a name="span-idpolygonsspanspan-idpolygonsspanspan-idpolygonsspanpolygons"></a><span id="Polygons"></span><span id="polygons"></span><span id="POLYGONS"></span>다각형


삼각형 스트립은 다각형을 만드는 데 종종 사용됩니다. 다각형은 세 개 이상의 꼭짓점을 사용하여 그리는 닫힌 3D 그림입니다. 가장 간단한 다각형은 삼각형입니다. Microsoft Direct3D는 삼각형을 사용하여 대부분의 다각형을 만듭니다. 삼각형의 꼭짓점 세 개가 항상 동일 평면에 있기 때문입니다. 비평면 꼭짓점을 렌더링하는 것은 효율적이지 않습니다. 삼각형을 결합하여 크고 복잡한 다각형 및 메시를 만들 수 있습니다.

다음 그림에서는 정육면체를 보여 줍니다. 두 개의 삼각형은 정육면체의 각 표면을 구성합니다. 삼각형 전체 집합이 모여서 하나의 정육면체 기본 형식을 구성합니다. 텍스처를 기본 형식의 표면에 적용하여 단일 입체 양식으로 표현할 수 있습니다. 자세한 내용은 [텍스처](textures.md)를 참조하세요.

![각 표면에 삼각형 두 개가 있는 정육면체 그림](images/cube3d.png)

삼각형을 사용하여 기본 형식의 표면이 부드러운 곡선으로 표시되도록 만들 수도 있습니다. 다음 그림에서는 삼각형을 사용하여 구형을 시뮬레이션할 수 있는 방법을 보여 줍니다. 재질을 적용한 후 구를 렌더링할 때 곡선으로 보이도록 만들 수 있습니다.

![삼각형을 사용하여 시뮬레이션된 구의 그림](images/sphere3d.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[기본 요소](primitives.md)

 

 




