---
title: VBV(꼭짓점 버퍼 보기) 및 IBV(인덱스 버퍼 보기)
description: Direct3D 렌더링에서 꼭 짓 점에 대 한 데이터 및 정수 인덱스를 포함 하는 VBV (버텍스 buffer view) 및 IBV (인덱스 버퍼 뷰)에 대해 알아봅니다.
ms.assetid: 695115D2-9DA0-41F2-9416-33BFAB698129
keywords:
- VBV (버텍스 buffer view)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a616f2bad8f478b2d20e96b183ba944950fef8a8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156097"
---
# <a name="vertex-buffer-view-vbv-and-index-buffer-view-ibv"></a>VBV(꼭짓점 버퍼 보기) 및 IBV(인덱스 버퍼 보기)


꼭 짓 점 버퍼는 꼭 짓 점 목록에 대 한 데이터를 보유 합니다. 각 꼭 짓 점에 대 한 데이터에는 위치, 색, 일반 벡터, 질감 좌표 등이 포함 될 수 있습니다. 인덱스 버퍼는 꼭 짓 점 버퍼에 정수 인덱스 (오프셋)를 저장 하며, 전체 꼭 짓 점 목록의 하위 집합으로 구성 된 개체를 정의 하 고 렌더링 하는 데 사용 됩니다.

단일 꼭 짓 점의 정의는 다음과 같이 정의 하는 응용 프로그램에 따라 일반적입니다.

``` syntax
struct CUSTOMVERTEX { 
        FLOAT x, y, z;      // The position
        FLOAT nx, ny, nz;   // The normal
        DWORD color;        // RGBA color
        FLOAT tu, tv;       // The texture coordinates. 
}; 
```

그런 다음, 꼭 짓 점 버퍼를 만들 때 CUSTOMVERTEX의 정의가 그래픽 드라이버로 전달 됩니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[Views](views.md)

 

 




