---
Description: 사용자가 항목을 탭 할 때 시각적 피드백을 사용자에 게 제공 하려면 포인터 애니메이션을 사용 합니다.
title: 포인터 클릭 애니메이션
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d429f55b6c8004ea0b5e16f842f5c7ee492d754d
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218850"
---
# <a name="pointer-click-animations"></a>포인터 클릭 애니메이션



사용자가 항목을 탭 할 때 시각적 피드백을 사용자에 게 제공 하려면 포인터 애니메이션을 사용 합니다. 포인터 아래로 애니메이션은 눌러진 항목을 약간 축소 하 고 tilts, 항목을 처음 누를 때 재생 됩니다. 항목을 원래 위치로 복원 하는 포인터 위로 애니메이션은 사용자가 포인터를 놓을 때 재생 됩니다.


> **중요 한 api**: [**PointerUpThemeAnimation 클래스**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation), [**PointerDownThemeAnimation 클래스**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

-   포인터 위로 애니메이션을 사용 하는 경우 사용자가 포인터를 놓을 때 애니메이션을 즉시 트리거합니다. 그러면 탭으로 트리거된 작업 (예: 새 페이지로 이동)이 응답 하는 속도가 느린 경우에도 사용자에 게 해당 작업이 인식 되었다는 즉각적인 피드백이 제공 됩니다.

## <a name="related-articles"></a>관련된 문서

* [애니메이션 개요](./xaml-animation.md)
* [포인터 클릭에 애니메이션 적용](/previous-versions/windows/apps/jj649432(v=win.10))
* [빠른 시작: 라이브러리 애니메이션을 사용 하 여 UI에 애니메이션 효과 주기](/previous-versions/windows/apps/hh452703(v=win.10))
* [**PointerUpThemeAnimation 클래스**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation)
* [**PointerDownThemeAnimation 클래스**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)

 

 
