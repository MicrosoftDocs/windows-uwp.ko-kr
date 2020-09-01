---
Description: 팝업 애니메이션을 사용 하 여 flyouts 또는 사용자 지정 팝업 UI 요소에 대 한 팝업 UI를 표시 하 고 숨깁니다. 팝업 요소는 앱 내용 위에 표시 되 고 사용자가 팝업 요소 외부에서 탭 하거나 클릭 하는 경우 해제 되는 컨테이너입니다.
title: 팝업 UI 애니메이션
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f21e529d519913e5b7fd2b19bd63f40c5a3d2009
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172387"
---
# <a name="pop-up-ui-animations"></a>팝업 UI 애니메이션



팝업 애니메이션을 사용 하 여 flyouts 또는 사용자 지정 팝업 UI 요소에 대 한 팝업 UI를 표시 하 고 숨깁니다. 팝업 요소는 앱 내용 위에 표시 되 고 사용자가 팝업 요소 외부에서 탭 하거나 클릭 하는 경우 해제 되는 컨테이너입니다.

> **중요 한 api**: [**PopInThemeAnimation 클래스**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation), [**PopupThemeTransition 클래스**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


-   팝업 애니메이션을 사용 하 여 앱 페이지 자체에 속하지 않는 사용자 지정 팝업 UI 요소를 표시 하거나 숨깁니다. Windows에서 제공 하는 공용 컨트롤에는 이러한 애니메이션이 이미 내장 되어 있습니다.
-   도구 설명 또는 대화 상자에는 팝업 애니메이션을 사용 하지 마세요.
-   앱의 기본 콘텐츠 내에서 UI를 표시 하거나 숨기려면 팝업 애니메이션을 사용 하지 마세요. 기본 앱 콘텐츠 위에 표시 되는 팝업 컨테이너를 표시 하거나 숨기려면 팝업 애니메이션을 사용 합니다.

## <a name="related-articles"></a>관련된 문서

* [애니메이션 개요](./xaml-animation.md)
* [팝업 UI에 애니메이션 적용](/previous-versions/windows/apps/jj649433(v=win.10))
* [빠른 시작: 라이브러리 애니메이션을 사용 하 여 UI에 애니메이션 효과 주기](/previous-versions/windows/apps/hh452703(v=win.10))
* [**PopInThemeAnimation 클래스**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation)
* [**PopOutThemeAnimation 클래스**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation)
* [**PopupThemeTransition 클래스**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)

 

 