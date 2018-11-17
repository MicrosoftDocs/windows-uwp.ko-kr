---
author: mijacobs
Description: Edge-based animations show or hide UI that originates from the edge of the screen.
title: UWP 앱의 가장자리 기반 UI 애니메이션
ms.assetid: 5A8F73B1-F4F6-424b-9EDF-A9766C5DEAE8
label: Motion--edge-based UI
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0a1cb6cee78d7c7d1e59e64dd151539ce6b3e5d7
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7170058"
---
# <a name="edge-based-ui-animations"></a>가장자리 기반 UI 애니메이션





가장자리 기반 애니메이션은 화면의 가장자리에서 시작되는 UI를 표시하거나 숨깁니다. 표시 및 숨기기 동작은 사용자 또는 앱에서 시작할 수 있습니다. UI는 앱에 오버레이하거나 주 앱 표면의 일부가 될 수 있습니다. UI가 앱 화면의 일부일 경우 나머지 앱 화면은 UI를 포함하도록 크기를 조정해야 합니다.

> **중요 API**: [**EdgeUIThemeTransition 클래스**](https://msdn.microsoft.com/library/windows/apps/hh702324)


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


-   화면으로 확장되지 않는 사용자 지정 메시지나 오류 바를 표시하거나 숨기려면 가장자리 UI 애니메이션을 사용합니다.
-   작업 창이나 사용자 지정 화상 키보드처럼 화면의 안쪽 깊숙이 미끄러지듯 들어오는 UI를 표시하려면 패널 애니메이션을 사용하세요.
-   연결되는 가장자리로부터 UI를 밀어넣습니다.
-   출발 지점이 된 가장자리를 향해 UI를 밀어냅니다.
-   UI를 밀어넣거나 밀어내는 것에 대한 응답으로 앱 콘텐츠의 크기를 조정해야 하는 경우 페이드 애니메이션을 크기 조정에 사용합니다.
    -   UI를 밀어넣는 경우 가장자리 UI 또는 패널 애니메이션 다음에 페이드 애니메이션을 사용하세요.
    -   UI를 밀어내는 경우 가장자리 UI 또는 패널 애니메이션과 동시에 페이드 애니메이션을 사용하세요.
-   알림에 이러한 애니메이션을 적용하지 마세요. 가장자리 기반 UI 내에 알림을 포함하면 안 됩니다.
-   화면의 가장자리에 없는 UI 컨테이너나 컨트롤에 가장자리 UI 또는 패널 애니메이션을 적용하지 마세요. 이러한 애니메이션은 화면의 가장자리에 있는 UI를 표시하고 닫고 그 크기를 변경하는 용도로만 사용합니다. 다른 유형의 UI를 이동하려면 위치 변경 애니메이션을 사용합니다.

    ![Edge UI 또는 패널 UI를 사용하는 경우 및 위치 변경을 사용하는 경우를 보여 줍니다.](images/edgevsreposition.png)

## <a name="related-articles"></a>관련 문서


**개발자용**
* [애니메이션 개요](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [가장자리 기반 UI 애니메이션](https://msdn.microsoft.com/library/windows/apps/xaml/jj649428)
* [빠른 시작: 라이브러리 애니메이션을 사용하여 UI에 애니메이션 효과 주기](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**EdgeUIThemeTransition 클래스**](https://msdn.microsoft.com/library/windows/apps/hh702324)
* [**PaneThemeTransition 클래스**](https://msdn.microsoft.com/library/windows/apps/hh969160)
* [페이드 애니메이션](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [위치 변경 애니메이션](https://msdn.microsoft.com/library/windows/apps/xaml/jj649434)

 

 




