---
title: 사각형
description: Direct3D 및 Windows 프로그래밍에서 화면의 개체는 경계 사각형의 관점에서 참조됩니다.
ms.assetid: 3B78AE66-2C1A-4191-BDCA-D737E33460BA
keywords:
- 사각형
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: aa94eb00058ba3297e7ca7cc4f93581d9281fd1c
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7702544"
---
# <a name="rectangles"></a>사각형


Direct3D 및 Windows 프로그래밍 전체에서 경계 사각형을 사용하여 화면의 개체를 참조합니다. 경계 사각형의 측면은 화면의 측면과 항상 평행하므로 사각형은 위 오른쪽 모서리와 아래 오른쪽 모서리라는 두 개의 점으로 설명할 수 있습니다.

## <a name="span-idboundingrectanglesspanspan-idboundingrectanglesspanspan-idboundingrectanglesspanbounding-rectangles"></a><span id="Bounding_rectangles"></span><span id="bounding_rectangles"></span><span id="BOUNDING_RECTANGLES"></span>경계 사각형


대부분의 응용 프로그램은 [**RECT**](https://msdn.microsoft.com/library/windows/desktop/dd162897) 구조(또는 그 typedef된 별칭)를 사용하여 화면으로 블리팅할 때 또는 히트 감지를 수행할 때 사용할 경계 사각형에 대한 정보를 운반합니다. C++에서 **RECT** 구조의 정의는 다음과 같습니다.

```
typedef struct tagRECT { 
    LONG    left;    // This is the upper-left corner x-coordinate.
    LONG    top;     // The upper-left corner y-coordinate.
    LONG    right;   // The lower-right corner x-coordinate.
    LONG    bottom;  // The lower-right corner y-coordinate.
} RECT, *PRECT, NEAR *NPRECT, FAR *LPRECT; 
```

앞의 예제에서 왼쪽 및 상단 멤버는 경계 사각형 위 왼쪽 모서리의 x 및 y 좌표입니다. 마찬가지로 오른쪽 및 하단 멤버는 아래 오른쪽 모서리의 좌표를 구성합니다. 다음 그림은 이런 값을 시각화하는 방법을 보여 줍니다.

![왼쪽, 상단, 오른쪽, 하단 값에 의해 경계가 설정된 사각형 그림](images/rect.png)

오른쪽 가장자리의 픽셀 열과 하단 가장자리의 픽셀 행은 RECT에 포함되지 않습니다. 예를 들어 멤버 {10, 10, 138, 138}로 RECT를 고정하면 너비와 높이가 128픽셀인 개체가 만들어집니다.

효율성, 일관성, 사용 편의성을 위해 모든 Direct3D 프레젠테이션 함수는 사각형에서 작동합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[좌표계 및 기하 도형](coordinate-systems-and-geometry.md)

 

 




