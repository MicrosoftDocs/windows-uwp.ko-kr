---
author: mijacobs
Description: "플라이아웃의 팝업 UI 또는 사용자 지정 팝업 UI 요소를 표시하고 숨기려면 팝업 애니메이션을 사용합니다. 팝업 요소는 앱 콘텐츠 위에 나타났다가 사용자가 팝업 요소 외부를 탭하거나 클릭할 때 해제되는 컨테이너입니다."
title: "UWP 앱의 팝업 UI 애니메이션"
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: c91e5cd3d4bad1b29d070f4750beb3dd95b3c5dc
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/22/2017
---
# <a name="pop-up-ui-animations"></a>팝업 UI 애니메이션

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

플라이아웃의 팝업 UI 또는 사용자 지정 팝업 UI 요소를 표시하고 숨기려면 팝업 애니메이션을 사용합니다. 팝업 요소는 앱 콘텐츠 위에 나타났다가 사용자가 팝업 요소 외부를 탭하거나 클릭할 때 해제되는 컨테이너입니다.

<div class="important-apis" >
<b>중요 API</b><br/>
<ul>
<li>[**PopInThemeAnimation 클래스**](https://msdn.microsoft.com/library/windows/apps/br210383)</li>
<li>[**PopupThemeTransition 클래스**](https://msdn.microsoft.com/library/windows/apps/hh969172)</li>
</ul>
</div>


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

 

 




