---
author: mijacobs
Description: Content transition animations let you change the content of an area of the screen while keeping the container or background constant. New content fades in. If there is existing content to be replaced, that content fades out.
title: 콘텐츠 전환 애니메이션에 대한 지침
ms.assetid: 0188FDB4-E183-466f-8A03-EE3FF5C474B1
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 14d5120632833e91e82ed7dd717ba04a9abb0efb
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5754083"
---
# <a name="content-transition-animations"></a>콘텐츠 전환 애니메이션



콘텐츠 전환 애니메이션을 사용하면 컨테이너 또는 배경을 일정하게 유지하면서 화면 영역의 콘텐츠를 변경할 수 있습니다. 새 콘텐츠가 페이드 인됩니다. 바꿀 기존 콘텐츠가 있는 경우 해당 콘텐츠는 페이드 아웃됩니다.

> **중요 API**: [**ContentThemeTransition 클래스(XAML)**](https://msdn.microsoft.com/library/windows/apps/br243104)

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


-   빈 컨테이너에 넣을 새 항목 집합이 있을 때 시작 애니메이션을 사용하세요. 예를 들어, 앱을 최초로 로드한 다음 앱의 콘텐츠 중 일부를 즉시 표시할 수 없는 경우도 있습니다. 콘텐츠가 표시할 수 있게 되면 콘텐츠 전환 애니메이션을 사용하여 그 최신 콘텐츠를 보기로 가져옵니다.
-   어떤 콘텐츠의 집합을 이미 보기 내의 동일한 컨테이너에 상주하는 다른 콘텐츠의 집합으로 바꾸려면 콘텐츠 전환을 사용합니다.
-   새 콘텐츠를 가져오려면 일반 페이지 흐름이나 읽는 순서와 반대로 해당 콘텐츠를 보기로 밀어 올립니다(아래에서 위쪽으로).
-   새 콘텐츠를 논리적인 방식으로 소개합니다. 예를 들어 콘텐츠의 가장 중요한 부분을 마지막에 소개합니다.
-   콘텐츠를 업데이트할 콘테이너가 두 개 이상일 경우 모든 전환 애니메이션을 시차나 지연 없이 동시에 트리거합니다.
-   전체 페이지가 변경되는 경우에는 콘텐츠 전환 애니메이션을 사용하지 마세요. 이 경우에는 페이지 전환 애니메이션을 대신 사용합니다.
-   콘텐츠를 새로 고치기만 하는 경우에는 콘텐츠 전환 애니메이션을 사용하지 마세요. 콘텐츠 전환 애니메이션은 이동을 나타내는 데 사용됩니다. 새로 고침의 경우 페이드 애니메이션을 사용합니다.



## <a name="related-articles"></a>관련 문서

**개발자용(XAML)**
* [애니메이션 개요](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [콘텐츠 전환 애니메이션](https://msdn.microsoft.com/library/windows/apps/xaml/jj649426)
* [빠른 시작: 라이브러리 애니메이션을 사용하여 UI에 애니메이션 효과 주기](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**ContentThemeTransition 클래스**](https://msdn.microsoft.com/library/windows/apps/br243104)

 

 




