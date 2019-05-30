---
Description: 사용자가 항목을 탭할 때 사용자에게 시각적 피드백을 제공하려면 포인터 애니메이션을 사용합니다.
title: UWP 앱의 포인터 클릭 애니메이션
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ee87a62796ed51d09d44cabecd0b49873145bd90
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366080"
---
# <a name="pointer-click-animations"></a>포인터 클릭 애니메이션



사용자가 항목을 탭할 때 사용자에게 시각적 피드백을 제공하려면 포인터 애니메이션을 사용합니다. 포인터 아래로 애니메이션은 눌린 항목을 약간 축소하고 기울이며 항목을 처음 탭할 때 재생됩니다. 포인터 위로 애니메이션은 항목을 원래 위치로 복원하며 사용자가 포인터를 놓을 때 재생됩니다.


> **중요 API**: [**PointerUpThemeAnimation 클래스**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation)하십시오 [ **PointerDownThemeAnimation 클래스**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

-   포인터 위로 애니메이션을 사용할 때는 사용자가 포인터를 놓을 때 애니메이션을 즉시 트리거합니다. 이 애니메이션은 탭에 의해 트리거된 동작(예: 새 페이지로 이동)에 응답하는 것이 더 느린 경우에도 동작이 인식되었음을 사용자에게 즉시 알립니다.

## <a name="related-articles"></a>관련 문서

* [애니메이션 개요](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [애니메이션 적용에 대 한 포인터 클릭](https://docs.microsoft.com/previous-versions/windows/apps/jj649432(v=win.10))
* [빠른 시작: 애니메이션 라이브러리 애니메이션을 사용 하 여 UI](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**PointerUpThemeAnimation 클래스**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation)
* [**PointerDownThemeAnimation 클래스**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)

 

 




