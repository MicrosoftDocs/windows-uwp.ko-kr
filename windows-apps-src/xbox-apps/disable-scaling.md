---
title: 크기 조정을 끄는 방법
description: 기본 배율 인수를 해제 하 고 응용 프로그램에서 실제 1910 x 1080 픽셀 장치 차원을 사용 하도록 하는 방법에 대해 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6e68c1fc-a407-4c0b-b0f4-e445ccb72ff3
ms.localizationpriority: medium
ms.openlocfilehash: 404bdd9a4b25254c1941928dbfb0b548492f03a5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174707"
---
# <a name="how-to-turn-off-scaling"></a>크기 조정을 끄는 방법   
기본적으로 응용 프로그램은 HTML 앱의 경우 200% (XAML의 경우) 및 150%로 확장 됩니다. 기본 배율 인수를 해제할 수 있습니다. 이렇게 하면 응용 프로그램에서 장치의 실제 픽셀 크기 (1910 x 1080 픽셀)를 사용 합니다.   
   
## <a name="html"></a>HTML   
다음 코드 조각을 사용 하 여 배율 인수를 옵트아웃 (opt out) 할 수 있습니다. 
   
```
var result = Windows.UI.ViewManagement.ApplicationViewScaling.trySetDisableLayoutScaling(true);
```

또는 다음과 같은 웹 친화적인 메서드를 사용할 수 있습니다.   

```   
@media (max-height: 1080px) {   
    @-ms-viewport {   
        height: 1080px;   
    }   
}   
```

## <a name="xaml"></a>XAML
다음 코드 조각을 사용 하 여 배율 인수를 옵트아웃 (opt out) 할 수 있습니다.   
   
```
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```
   
## <a name="directxc"></a>DirectX/c + +   
DirectX/c + + 응용 프로그램은 크기가 조정 되지 않습니다. 자동 크기 조정은 HTML 및 XAML 응용 프로그램에만 적용 됩니다.  

## <a name="see-also"></a>참고 항목
- [Xbox에 대 한 모범 사례](tailoring-for-xbox.md)
- [Xbox One의 UWP](index.md)
