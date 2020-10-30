---
description: 콘텐츠 전환 애니메이션을 사용 하면 컨테이너 또는 백그라운드 상수를 유지 하면서 화면 영역의 콘텐츠를 변경할 수 있습니다. 새 콘텐츠가 페이드 인 됩니다. 기존 콘텐츠가 바뀔 경우 해당 콘텐츠가 페이드 아웃 됩니다.
title: 콘텐츠 전환 애니메이션에 대한 지침
ms.assetid: 0188FDB4-E183-466f-8A03-EE3FF5C474B1
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ed70a100b4aec7a2c1490cfc48e4d2f6ceac950e
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029784"
---
# <a name="content-transition-animations"></a>콘텐츠 전환 애니메이션



콘텐츠 전환 애니메이션을 사용 하면 컨테이너 또는 백그라운드 상수를 유지 하면서 화면 영역의 콘텐츠를 변경할 수 있습니다. 새 콘텐츠가 페이드 인 됩니다. 기존 콘텐츠가 바뀔 경우 해당 콘텐츠가 페이드 아웃 됩니다.

> **중요 한 api** : [ **ContentThemeTransition 클래스 (XAML)**](/uwp/api/windows.ui.xaml.media.animation.contentthemetransition)

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


-   빈 컨테이너로 가져올 새 항목 집합이 있는 경우에는 나타내기 애니메이션을 사용 합니다. 예를 들어 앱의 초기 로드 후 앱 콘텐츠의 일부를 즉시 표시 하지 못할 수 있습니다. 콘텐츠를 표시할 준비가 되 면 콘텐츠 전환 애니메이션을 사용 하 여 해당 하는 콘텐츠를 뷰로 가져옵니다.
-   콘텐츠 전환을 사용 하 여 한 콘텐츠 집합을 보기 내의 동일한 컨테이너에 이미 있는 다른 콘텐츠 집합으로 바꿉니다.
-   새 콘텐츠를 가져올 때 일반 페이지 흐름이 나 읽기 순서를 기준으로 해당 콘텐츠를 아래쪽에서 위쪽으로 이동 합니다.
-   논리적 방식으로 새 콘텐츠를 소개 합니다. 예를 들어 마지막으로 가장 중요 한 콘텐츠를 소개 합니다.
-   콘텐츠를 업데이트할 컨테이너가 둘 이상 있는 경우에는 시차 또는 지연 없이 모든 전환 애니메이션을 동시에 트리거합니다.
-   전체 페이지를 변경 하는 경우 콘텐츠 전환 애니메이션을 사용 하지 마세요. 이 경우 페이지 전환 애니메이션을 대신 사용 합니다.
-   콘텐츠를 새로 고치는 경우 콘텐츠 전환 애니메이션을 사용 하지 마세요. 콘텐츠 전환 애니메이션은 움직임을 보여 주기 위한 것입니다. 새로 고침의 경우 페이드 애니메이션을 사용 합니다.



## <a name="related-articles"></a>관련된 문서

**개발자용(XAML)**
* [애니메이션 개요](./xaml-animation.md)
* [콘텐츠 전환에 애니메이션 적용](/previous-versions/windows/apps/jj649426(v=win.10))
* [빠른 시작: 라이브러리 애니메이션을 사용 하 여 UI에 애니메이션 효과 주기](/previous-versions/windows/apps/hh452703(v=win.10))
* [**ContentThemeTransition 클래스**](/uwp/api/windows.ui.xaml.media.animation.contentthemetransition)

 

 
