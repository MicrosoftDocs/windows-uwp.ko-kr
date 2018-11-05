---
author: payzer
title: 화면 가장자리까지 UI를 그리는 방법
description: 제목 보호 영역에 대한 자동 크기 조정을 끄는 방법.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
ms.localizationpriority: medium
ms.openlocfilehash: 32932845db47ed47b7e80f68cf4e424ba97e85c0
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6030019"
---
# <a name="how-to-draw-ui-to-the-edge-of-the-screen"></a>화면 가장자리까지 UI를 그리는 방법   
기본적으로 응용 프로그램에는 TV 안전 영역에 맞게 뷰포트 가장자리에 배치된 테두리가 있습니다. 자세한 내용은 [Xbox 및 TV용 디자인](../design/devices/designing-for-tv.md#tv-safe-area)을 참조하세요. 

이 기능을 끄고 화면 가장자리까지 그리는 것이 좋습니다. 응용 프로그램이 시작될 때 다음 코드를 추가하여 화면 가장자리까지 그릴 수 있습니다.
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> C++/DirectX 응용 프로그램의 경우 이에 대해 걱정할 필요가 없습니다. 시스템에서 항상 화면 가장자리까지 응용 프로그램을 렌더링합니다.

## <a name="see-also"></a>참고 항목
- [Xbox에 적용할 수 있는 최선의 방법](tailoring-for-xbox.md)
- [Xbox One의 UWP](index.md)
