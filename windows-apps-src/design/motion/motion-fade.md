---
description: 페이드 애니메이션을 사용 하 여 항목을 뷰로 가져오거나 보기에서 항목을 가져옵니다. 두 가지 일반적인 페이드 애니메이션은 페이드 인 및 페이드 아웃입니다.
title: 페이드 애니메이션
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 81e632a25398003daa1e73d82c23343f57fd3cd0
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034226"
---
# <a name="fade-animations"></a>페이드 애니메이션



페이드 애니메이션을 사용 하 여 항목을 뷰로 가져오거나 보기에서 항목을 가져옵니다. 두 가지 일반적인 페이드 애니메이션은 페이드 인 및 페이드 아웃입니다.

> **중요 한 api** : [**FadeInThemeAnimation 클래스**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation), [**FadeOutThemeAnimation 클래스**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


-   앱이 관련이 없는 요소나 텍스트를 많이 사용 하는 요소 간을 전환 하는 경우 페이드 아웃 후 페이드 인을 사용 합니다. 이렇게 하면 들어오는 개체가 표시 되기 전에 나가는 개체가 완전히 사라질 수 있습니다.
-   요소의 크기가 일정 하 게 유지 되 고 사용자가 같은 항목을 확인 하려는 경우에는 나가는 요소 위에 들어오는 요소 또는 요소를 페이드 인 합니다. 페이드 인이 완료 되 면 나가는 항목을 제거할 수 있습니다. 들어오는 항목이 들어오는 항목을 완전히 처리 하는 경우에만이 옵션을 사용할 수 있습니다.
-   목록에서 항목을 추가 하거나 삭제 하려면 애니메이션 페이드를 사용 하지 마십시오. 대신 해당 목적으로 만든 목록 애니메이션을 사용 합니다.
-   애니메이션 페이드를 방지 하 여 페이지의 전체 내용을 변경 합니다. 대신 해당 목적으로 만든 페이지 전환 애니메이션을 사용 합니다.
-   페이드 아웃은 요소를 제거 하는 미묘한 방법입니다.
## <a name="related-articles"></a>관련된 문서

* [애니메이션 개요](./xaml-animation.md)
* [페이드 애니메이션](/previous-versions/windows/apps/jj649429(v=win.10))
* [빠른 시작: 라이브러리 애니메이션을 사용 하 여 UI에 애니메이션 효과 주기](/previous-versions/windows/apps/hh452703(v=win.10))
* [**FadeInThemeAnimation 클래스**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation)
* [**FadeOutThemeAnimation 클래스**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)

 

 
