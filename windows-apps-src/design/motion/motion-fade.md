---
Description: 항목을 보기로 가져오거나 보기 외부로 이동하려면 페이드 애니메이션을 사용합니다. 두 가지의 일반적인 페이드 애니메이션은 페이드 인 및 페이드 아웃입니다.
title: UWP 앱의 페이드 애니메이션
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3d8642e911a3ad4275e0a7a0f147ca9d70f415b0
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366794"
---
# <a name="fade-animations"></a>페이드 애니메이션



항목을 보기로 가져오거나 보기 외부로 이동하려면 페이드 애니메이션을 사용합니다. 두 가지의 일반적인 페이드 애니메이션은 페이드 인 및 페이드 아웃입니다.

> **중요 API**: [**FadeInThemeAnimation 클래스**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation)하십시오 [ **FadeOutThemeAnimation 클래스**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


-   앱이 관련이 없거나 텍스트가 많은 요소 간을 전환할 때는 페이드 아웃을 사용한 다음 페이드 인을 사용합니다. 이렇게 하면 들어오는 개체가 보이기 전에 나가는 개체가 완전히 사라집니다.
-   요소의 크기가 일정하게 유지되고 같은 항목을 보는 것처럼 느껴지게 하려면 나가는 요소 위에 들어오는 요소를 페이드 인합니다. 페이드 인이 완료되면 나가는 항목이 제거될 수 있습니다. 이렇게 해야만 들어오는 항목이 나가는 항목을 완전히 덮을 수 있습니다.
-   목록의 항목을 추가하거나 삭제하는 데 페이드 애니메이션을 사용하지 마세요. 대신, 해당 용도에 맞게 만들어진 목록 애니메이션을 사용합니다.
-   페이지의 전체 콘텐츠를 변경하는 데 페이드 애니메이션을 사용하지 마세요. 대신, 해당 용도에 맞게 만들어진 페이지 전환 애니메이션을 사용합니다.
-   페이드 아웃은 요소를 제거하는 적절한 방법입니다.
## <a name="related-articles"></a>관련 문서

* [애니메이션 개요](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [페이드 애니메이션 적용](https://docs.microsoft.com/previous-versions/windows/apps/jj649429(v=win.10))
* [빠른 시작: 애니메이션 라이브러리 애니메이션을 사용 하 여 UI](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**FadeInThemeAnimation 클래스**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation)
* [**FadeOutThemeAnimation 클래스**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)

 

 




