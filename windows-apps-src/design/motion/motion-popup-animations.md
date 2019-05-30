---
Description: 플라이아웃의 팝업 UI 또는 사용자 지정 팝업 UI 요소를 표시하고 숨기려면 팝업 애니메이션을 사용합니다. 팝업 요소는 앱 콘텐츠 위에 나타났다가 사용자가 팝업 요소 외부를 탭하거나 클릭할 때 해제되는 컨테이너입니다.
title: UWP 앱의 팝업 UI 애니메이션
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ee3d6a7fc29ec2adfeb149a3bc84f27c482c3be7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366707"
---
# <a name="pop-up-ui-animations"></a>팝업 UI 애니메이션



플라이아웃의 팝업 UI 또는 사용자 지정 팝업 UI 요소를 표시하고 숨기려면 팝업 애니메이션을 사용합니다. 팝업 요소는 앱 콘텐츠 위에 나타났다가 사용자가 팝업 요소 외부를 탭하거나 클릭할 때 해제되는 컨테이너입니다.

> **중요 API**: [**PopInThemeAnimation class**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation), [**PopupThemeTransition class**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


-   앱 페이지 자체의 일부가 아닌 사용자 지정 팝업 UI 요소를 표시하거나 숨기려면 팝업 애니메이션을 사용합니다. Windows가 제공하는 공용 컨트롤에는 이러한 애니메이션이 기본으로 제공되어 있습니다.
-   도구 설명이나 대화 상자에는 팝업 애니메이션을 사용하지 마세요.
-   앱의 기본 콘텐츠 내에서 UI를 표시하거나 숨기는 데 팝업 애니메이션을 사용하지 마세요. 기본 앱 콘텐츠 맨 위에 표시되는 팝업 컨테이너를 표시하거나 숨기는 데에만 팝업 애니메이션을 사용합니다.

## <a name="related-articles"></a>관련 문서

* [애니메이션 개요](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [UI를 팝업에 애니메이션 적용](https://docs.microsoft.com/previous-versions/windows/apps/jj649433(v=win.10))
* [빠른 시작: 애니메이션 라이브러리 애니메이션을 사용 하 여 UI](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**PopInThemeAnimation 클래스**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation)
* [**PopOutThemeAnimation class**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation)
* [**PopupThemeTransition 클래스**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)

 

 




