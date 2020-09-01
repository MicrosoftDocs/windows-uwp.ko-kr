---
title: 사각형
description: Direct3D 및 Windows 프로그래밍에서 화면 개체는 경계 사각형 측면에서 참조 됩니다.
ms.assetid: 3B78AE66-2C1A-4191-BDCA-D737E33460BA
keywords:
- 사각형
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a30aa1a2901f109a4f13316024785981023975b8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156257"
---
# <a name="rectangles"></a>사각형

Direct3D 및 Windows 프로그래밍에서 화면 개체는 경계 사각형 측면에서 참조 됩니다. 경계 사각형의 면은 항상 화면 측면에 평행이 되므로 사각형은 왼쪽 위 모퉁이와 오른쪽 아래 모퉁이의 두 점으로 설명할 수 있습니다.

## <a name="span-idbounding_rectanglesspanspan-idbounding_rectanglesspanspan-idbounding_rectanglesspanbounding-rectangles"></a><span id="Bounding_rectangles"></span><span id="bounding_rectangles"></span><span id="BOUNDING_RECTANGLES"></span>경계 사각형


대부분의 응용 프로그램에서는 [**사각형**](/previous-versions/dd162897(v=vs.85)) 에 blitting 때 사용할 경계 사각형에 대 한 정보를 전달 하거나 적중 검색을 수행할 때 사용할 경계 사각형에 대 한 정보를 전달 합니다. C + +에서 **RECT** 구조체의 정의는 다음과 같습니다.

```cpp
typedef struct tagRECT { 
    LONG    left;    // This is the upper-left corner x-coordinate.
    LONG    top;     // The upper-left corner y-coordinate.
    LONG    right;   // The lower-right corner x-coordinate.
    LONG    bottom;  // The lower-right corner y-coordinate.
} RECT, *PRECT, NEAR *NPRECT, FAR *LPRECT; 
```

앞의 예제에서 왼쪽 및 최상위 멤버는 경계 사각형의 왼쪽 위 모퉁이에 대 한 x 및 y 좌표입니다. 마찬가지로 오른쪽 및 아래 멤버는 오른쪽 아래 모퉁이의 좌표를 구성 합니다. 다음 그림에서는 이러한 값을 시각화할 수 있는 방법을 보여 줍니다.

![왼쪽, 위쪽, 오른쪽 및 아래쪽 값으로 바인딩된 사각형의 그림](images/rect.png)

오른쪽 가장자리에 있는 픽셀의 열과 아래쪽 가장자리에 있는 픽셀의 행이 RECT에 포함 되지 않습니다. 예를 들어, {10, 10, 138, 138} 멤버가 포함 된 RECT를 잠그면 개체 128 픽셀의 너비와 높이가 반환 됩니다.

효율성, 일관성 및 사용 편의성을 위해 모든 Direct3D 프레젠테이션 함수는 사각형에서 작동 합니다.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>관련 항목


[좌표계 및 기하 도형](coordinate-systems-and-geometry.md)

 

 