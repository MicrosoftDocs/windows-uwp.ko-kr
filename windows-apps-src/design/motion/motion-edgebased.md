---
Description: 가장자리 기반 애니메이션은 화면 가장자리에서 시작 되는 UI를 표시 하거나 숨깁니다.
title: 가장자리 기반 UI 애니메이션
ms.assetid: 5A8F73B1-F4F6-424b-9EDF-A9766C5DEAE8
label: Motion--edge-based UI
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 02108ad2926fc1514ca94f08d11f565bc342a62d
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970318"
---
# <a name="edge-based-ui-animations"></a>가장자리 기반 UI 애니메이션





가장자리 기반 애니메이션은 화면 가장자리에서 시작 되는 UI를 표시 하거나 숨깁니다. 표시 및 숨기기 동작은 사용자 또는 앱에서 시작할 수 있습니다. UI는 앱을 오버레이 하거나 주 앱 표면의 일부가 될 수 있습니다. UI가 앱 표면의 일부인 경우 응용 프로그램의 나머지 부분을 수용할 수 있도록 크기를 조정 해야 할 수 있습니다.

> **중요 한 api**: [ **EdgeUIThemeTransition 클래스**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


-   Edge UI 애니메이션을 사용 하 여 화면으로 확장 되지 않는 사용자 지정 메시지 또는 오차 막대를 표시 하거나 숨깁니다.
-   패널 애니메이션을 사용 하 여 작업 창 또는 사용자 지정 소프트 키보드와 같은 상당한 거리를 화면에 표시 하는 UI를 표시 합니다.
-   에서 UI가 연결 될 동일한 가장자리에서 UI를 밉니다.
-   UI를 가져온 가장자리와 동일 하 게 이동 합니다.
-   응용 프로그램의 콘텐츠를 슬라이딩/아웃 UI에 대 한 응답으로 크기를 조정 해야 하는 경우 크기 조정에 대해 페이드 애니메이션을 사용 합니다.
    -   UI가 슬라이딩 인 경우 가장자리 UI 또는 패널 애니메이션 뒤에 페이드 애니메이션을 사용 합니다.
    -   UI를 슬라이딩 하는 경우 가장자리 UI 또는 패널 애니메이션과 동시에 페이드 애니메이션을 사용 합니다.
-   이러한 애니메이션을 알림에 적용 하지 마십시오. 알림은에 지 기반 UI 내에 포함 되어서는 안 됩니다.
-   화면 가장자리에 없는 UI 컨테이너 또는 컨트롤에 가장자리 UI 또는 패널 애니메이션을 적용 하지 마세요. 이러한 애니메이션은 화면 가장자리에서 UI를 표시, 크기 조정 및 해제 하는 데만 사용 됩니다. 다른 형식의 UI를 이동 하려면 애니메이션 위치 변경을 사용 합니다.

    ![가장자리 ui 또는 패널 애니메이션을 사용 하는 경우 및 위치 변경을 사용 하는 경우를 보여 줍니다.](images/edgevsreposition.png)

## <a name="related-articles"></a>관련된 문서


**개발자용**
* [애니메이션 개요](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [가장자리 기반 UI에 애니메이션 효과 적용](https://docs.microsoft.com/previous-versions/windows/apps/jj649428(v=win.10))
* [빠른 시작: 라이브러리 애니메이션을 사용 하 여 UI에 애니메이션 효과 주기](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**EdgeUIThemeTransition 클래스**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)
* [**PaneThemeTransition 클래스**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PaneThemeTransition)
* [페이드 애니메이션](https://docs.microsoft.com/previous-versions/windows/apps/jj649429(v=win.10))
* [위치에 애니메이션 적용](https://docs.microsoft.com/previous-versions/windows/apps/jj649434(v=win.10))

 

 




