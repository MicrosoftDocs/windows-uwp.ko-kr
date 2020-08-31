---
title: 화면 가장자리에 UI를 그리는 방법
description: 뷰포트의 가장자리에 배치 된 기본 테두리를 끄고 화면 가장자리에 UI를 그리는 방법에 대해 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
ms.localizationpriority: medium
ms.openlocfilehash: d34da1bbf129358f4549b3a4a04a7c3f84f872ab
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154847"
---
# <a name="how-to-draw-ui-to-the-edge-of-the-screen"></a>화면 가장자리에 UI를 그리는 방법   
기본적으로 응용 프로그램에는 TV 안전 영역을 고려 하 여 뷰포트의 가장자리에 배치 되는 테두리가 있습니다. 자세한 내용은 [Xbox 및 TV 디자인](../design/devices/designing-for-tv.md#tv-safe-area)을 참조 하세요. 

이 기능을 해제 하 고 화면 가장자리까지 그리는 것이 좋습니다. 응용 프로그램이 시작 될 때 다음 코드를 추가 하 여 화면 가장자리까지 그릴 수 있습니다.
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> C + +/DirectX 응용 프로그램은이에 대해 걱정 하지 않아도 됩니다. 시스템은 항상 화면 가장자리에 응용 프로그램을 렌더링 합니다.

## <a name="see-also"></a>참고 항목
- [Xbox에 대 한 모범 사례](tailoring-for-xbox.md)
- [Xbox One의 UWP](index.md)
