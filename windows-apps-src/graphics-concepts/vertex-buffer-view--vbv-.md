---
title: "VBV(꼭짓점 버퍼 보기) 및 IBV(인덱스 버퍼 보기)"
description: "꼭짓점 버퍼는 꼭짓점 목록에 대한 데이터를 저장합니다."
ms.assetid: 695115D2-9DA0-41F2-9416-33BFAB698129
keywords: "VBV(꼭짓점 버퍼 보기)"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 502b0e4816e31ebace93d3250f7da335d2540272
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="vertex-buffer-view-vbv-and-index-buffer-view-ibv"></a>VBV(꼭짓점 버퍼 보기) 및 IBV(인덱스 버퍼 보기)


꼭짓점 버퍼는 꼭짓점 목록에 대한 데이터를 저장합니다. 각 꼭짓점에 대한 데이터에는 위치, 색, 일반 벡터, 텍스처 좌표 등이 포함될 수 있습니다. 인덱스 버퍼는 꼭짓점 버퍼에 대한 정수 인덱스(오프셋)를 보관하며, 꼭짓점 전체 목록의 하위 집합으로 이루어진 개체를 정의하고 렌더링하는 데 사용됩니다.

단일 꼭짓점의 정의는 종종 다음과 같이 정의할 응용 프로그램에 따라 결정됩니다.

``` syntax
struct CUSTOMVERTEX { 
        FLOAT x, y, z;      // The position
        FLOAT nx, ny, nz;   // The normal
        DWORD color;        // RGBA color
        FLOAT tu, tv;       // The texture coordinates. 
}; 
```

CUSTOMVERTEX의 정의는 꼭짓점 버퍼를 만들 때 그래픽 드라이버에 전달됩니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[보기](views.md)

 

 



