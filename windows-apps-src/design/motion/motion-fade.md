---
author: mijacobs
Description: Use fade animations to bring items into a view or to take items out of a view. The two common fade animations are fade-in and fade-out.
title: UWP 앱의 페이드 애니메이션
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d2a9745e35f19066b094b2be187620858166dbd5
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7168339"
---
# <a name="fade-animations"></a>페이드 애니메이션



항목을 보기로 가져오거나 보기 외부로 이동하려면 페이드 애니메이션을 사용합니다. 두 가지의 일반적인 페이드 애니메이션은 페이드 인 및 페이드 아웃입니다.

> **중요 API**: [**FadeInThemeAnimation 클래스**](https://msdn.microsoft.com/library/windows/apps/br210298), [**FadeOutThemeAnimation 클래스**](https://msdn.microsoft.com/library/windows/apps/br210302)


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


-   앱이 관련이 없거나 텍스트가 많은 요소 간을 전환할 때는 페이드 아웃을 사용한 다음 페이드 인을 사용합니다. 이렇게 하면 들어오는 개체가 보이기 전에 나가는 개체가 완전히 사라집니다.
-   요소의 크기가 일정하게 유지되고 같은 항목을 보는 것처럼 느껴지게 하려면 나가는 요소 위에 들어오는 요소를 페이드 인합니다. 페이드 인이 완료되면 나가는 항목이 제거될 수 있습니다. 이렇게 해야만 들어오는 항목이 나가는 항목을 완전히 덮을 수 있습니다.
-   목록의 항목을 추가하거나 삭제하는 데 페이드 애니메이션을 사용하지 마세요. 대신, 해당 용도에 맞게 만들어진 목록 애니메이션을 사용합니다.
-   페이지의 전체 콘텐츠를 변경하는 데 페이드 애니메이션을 사용하지 마세요. 대신, 해당 용도에 맞게 만들어진 페이지 전환 애니메이션을 사용합니다.
-   페이드 아웃은 요소를 제거하는 적절한 방법입니다.
## <a name="related-articles"></a>관련 문서

* [애니메이션 개요](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [페이드 애니메이션](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [빠른 시작: 라이브러리 애니메이션을 사용하여 UI에 애니메이션 효과 주기](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**FadeInThemeAnimation 클래스**](https://msdn.microsoft.com/library/windows/apps/br210298)
* [**FadeOutThemeAnimation 클래스**](https://msdn.microsoft.com/library/windows/apps/br210302)

 

 




