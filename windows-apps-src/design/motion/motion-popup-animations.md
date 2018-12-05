---
Description: Use pop-up animations to show and hide pop-up UI for flyouts or custom pop-up UI elements. Pop-up elements are containers that appear over the app's content and are dismissed if the user taps or clicks outside of the pop-up element.
title: UWP 앱의 팝업 UI 애니메이션
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d79c369e14236b827bdc18aba6c74349528728b3
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8698187"
---
# <a name="pop-up-ui-animations"></a>팝업 UI 애니메이션



플라이아웃의 팝업 UI 또는 사용자 지정 팝업 UI 요소를 표시하고 숨기려면 팝업 애니메이션을 사용합니다. 팝업 요소는 앱 콘텐츠 위에 나타났다가 사용자가 팝업 요소 외부를 탭하거나 클릭할 때 해제되는 컨테이너입니다.

> **중요 API**: [**PopInThemeAnimation 클래스**](https://msdn.microsoft.com/library/windows/apps/br210383), [**PopupThemeTransition 클래스**](https://msdn.microsoft.com/library/windows/apps/hh969172)


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


-   앱 페이지 자체의 일부가 아닌 사용자 지정 팝업 UI 요소를 표시하거나 숨기려면 팝업 애니메이션을 사용합니다. Windows가 제공하는 공용 컨트롤에는 이러한 애니메이션이 기본으로 제공되어 있습니다.
-   도구 설명이나 대화 상자에는 팝업 애니메이션을 사용하지 마세요.
-   앱의 기본 콘텐츠 내에서 UI를 표시하거나 숨기는 데 팝업 애니메이션을 사용하지 마세요. 기본 앱 콘텐츠 맨 위에 표시되는 팝업 컨테이너를 표시하거나 숨기는 데에만 팝업 애니메이션을 사용합니다.

## <a name="related-articles"></a>관련 문서

* [애니메이션 개요](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [팝업 UI 애니메이션](https://msdn.microsoft.com/library/windows/apps/xaml/jj649433)
* [빠른 시작: 라이브러리 애니메이션을 사용하여 UI에 애니메이션 효과 주기](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**PopInThemeAnimation 클래스**](https://msdn.microsoft.com/library/windows/apps/br210383)
* [**PopOutThemeAnimation 클래스**](https://msdn.microsoft.com/library/windows/apps/br210391)
* [**PopupThemeTransition 클래스**](https://msdn.microsoft.com/library/windows/apps/hh969172)

 

 




