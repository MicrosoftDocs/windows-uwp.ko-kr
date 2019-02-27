---
Description: Use pointer animations to provide users with visual feedback when the user taps on an item.
title: UWP 앱의 포인터 클릭 애니메이션
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f1abd71cda8358e3f7e2fe36091f9c42f05bcb00
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116019"
---
# <a name="pointer-click-animations"></a>포인터 클릭 애니메이션



사용자가 항목을 탭할 때 사용자에게 시각적 피드백을 제공하려면 포인터 애니메이션을 사용합니다. 포인터 아래로 애니메이션은 눌린 항목을 약간 축소하고 기울이며 항목을 처음 탭할 때 재생됩니다. 포인터 위로 애니메이션은 항목을 원래 위치로 복원하며 사용자가 포인터를 놓을 때 재생됩니다.


> **중요 API**: [**PointerUpThemeAnimation 클래스**](https://msdn.microsoft.com/library/windows/apps/hh969168), [**PointerDownThemeAnimation 클래스**](https://msdn.microsoft.com/library/windows/apps/hh969164)


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

-   포인터 위로 애니메이션을 사용할 때는 사용자가 포인터를 놓을 때 애니메이션을 즉시 트리거합니다. 이 애니메이션은 탭에 의해 트리거된 동작(예: 새 페이지로 이동)에 응답하는 것이 더 느린 경우에도 동작이 인식되었음을 사용자에게 즉시 알립니다.

## <a name="related-articles"></a>관련 문서

* [애니메이션 개요](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [포인터 클릭 애니메이션](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432)
* [빠른 시작: 라이브러리 애니메이션을 사용하여 UI에 애니메이션 효과 주기](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**PointerUpThemeAnimation 클래스**](https://msdn.microsoft.com/library/windows/apps/hh969168)
* [**PointerDownThemeAnimation 클래스**](https://msdn.microsoft.com/library/windows/apps/hh969164)

 

 




