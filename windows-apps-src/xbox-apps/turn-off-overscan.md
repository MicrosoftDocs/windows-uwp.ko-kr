---
author: payzer
title: "오버스캔을 끄는 방법"
description: 
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 32a875348debac9aec9f5a26bc4e7e0af2a0a5b4
ms.openlocfilehash: abd06e78364ff32cc10d733e33b153b854dbc467

---

# 화면 가장자리까지 UI를 그리는 방법   
기본적으로 응용 프로그램에는 뷰포트 가장자리에 위치한 테두리가 있습니다. 이는 TV 안전 영역을 고려하기 위한 것입니다. 자세한 내용은 [Xbox 및 TV용 디자인](http://go.microsoft.com/fwlink/?LinkID=760736#tv-safe-area)을 참조하세요.  이 기능을 끄고 화면 가장자리까지 그리는 것이 좋습니다. 응용 프로그램이 시작될 때 다음 코드를 추가하여 화면 가장자리까지 그릴 수 있습니다.
   
`Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);`
   
참고: C++/DirectX 응용 프로그램의 경우 이에 대해 걱정할 필요가 없습니다. 시스템에서 항상 화면 가장자리까지 응용 프로그램을 렌더링합니다.



<!--HONumber=Jun16_HO5-->


