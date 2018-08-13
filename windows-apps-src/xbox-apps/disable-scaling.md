---
author: payzer
title: 크기 조정을 끄는 방법
description: 기본 배율 인수를 끄는 지침입니다.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 6e68c1fc-a407-4c0b-b0f4-e445ccb72ff3
ms.openlocfilehash: 4361437c8b6d67431bf879f9eb1cfbf3c03e592e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.locfileid: "220214"
---
# <a name="how-to-turn-off-scaling"></a>크기 조정을 끄는 방법   
기본적으로 응용 프로그램은 XAML 앱의 경우 200%로, HTML 앱의 경우 150%로 크기가 조정됩니다. 기본 배율 인수를 끌 수 있습니다. 이렇게 하면 응용 프로그램에서 디바이스의 실제 픽셀 크기(1910 x 1080 픽셀)를 사용합니다.   
   
## <a name="html"></a>HTML   
다음 코드 조각을 사용하여 배율 인수를 옵트아웃(opt out)할 수 있습니다. 
   
```
var result = Windows.UI.ViewManagement.ApplicationViewScaling.trySetDisableLayoutScaling(true);
```

또는 웹 기반 방법을 사용할 수 있습니다.   

```   
@media (max-height: 1080px) {   
    @-ms-viewport {   
        height: 1080px;   
    }   
}   
```

## <a name="xaml"></a>XAML
다음 코드 조각을 사용하여 배율 인수를 옵트아웃(opt out)할 수 있습니다.   
   
```
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```
   
## <a name="directxc"></a>DirectX/C++   
DirectX/C++ 응용 프로그램은 크기가 조정되지 않습니다. 자동 크기 조정은 HTML 및 XAML 응용 프로그램에만 적용됩니다.  

## <a name="see-also"></a>참고 항목
- [Xbox에 적용할 수 있는 최선의 방법](tailoring-for-xbox.md)
- [Xbox One의 UWP](index.md)
