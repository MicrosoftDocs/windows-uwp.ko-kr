---
description: 광고 성능 보고서를 사용 하 여 ad 단위의 viewability 최적화에 대 한 지침과 viewability 메트릭을 측정 하는 방법에 대해 알아봅니다.
title: 광고 단위의 가시성 최적화
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, 지침, viewability
ms.localizationpriority: medium
ms.openlocfilehash: 1653651c41128d359fcacddf0c0d7d408e798d07
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88942793"
---
# <a name="optimize-the-viewability-of-your-ad-units"></a>광고 단위의 가시성 최적화

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 정보](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

[광고 성능 보고서](../publish/advertising-performance-report.md) 에는 ad 단위에 대 한 viewability 메트릭이 포함 되어 있습니다. 광고 업계는 광고를 전달 하는 것이 아니라 valuing 볼 수 있는 Viewability으로 전환 되기 때문에 중요 한 메트릭입니다. 광고주는 사용자가 광고를 볼 수 있기 때문에 볼 수 있는 광고를 입찰 하는 경향이 있습니다.  

IAB viewability 지침에 따라 배너 광고는 다음 조건을 충족 하는 경우 볼 수 있는 것으로 간주 됩니다.

* 픽셀 요구 사항: 알림의 픽셀 중 50% 이상이 응용 프로그램의 볼 수 있는 공간에 있습니다.
* 시간 요구 사항: 픽셀 요구 사항이 충족 된 시간은 1 초 보다 크거나 같으며 ad 렌더링 후에 발생 합니다.

비디오 광고는 다음 조건을 충족 하는 경우 볼 수 있는 것으로 간주 됩니다.

* 픽셀 요구 사항: 알림의 픽셀 중 50% 이상이 응용 프로그램의 볼 수 있는 부분에 있습니다.
* 시간 요구 사항: 비디오가 픽셀 요구 사항을 충족 하 고 두 개의 연속 시간 (초) 동안 실행 된 후 ad 렌더링을 재생 합니다.

Viewability는 다음 수식을 사용 하 여 계산 됩니다.

**Viewability = [본 노출] * 100/[총 ad 노출]**

## <a name="guidelines-to-improve-ad-unit-viewability"></a>Ad unit viewability 개선 지침

광고 단위에 대해 볼 수 있는 상대를 최적화 하려면 다음 지침을 따르세요.

1. **성능 측정** &nbsp; &nbsp; [광고 성능 보고서](../publish/advertising-performance-report.md) 를 사용 하 여 각 광고 단위의 viewability 메트릭을 검토할 수 있습니다.
2.  **Ad 컨트롤을 적절 하 게 배치** &nbsp; &nbsp; 일반적으로 광고에 대해 가장 쉽게 볼 수 있는 위치는 접기 바로 위에 있습니다. 즉, 사용자가 스크롤하지 않고도 볼 수 있는 페이지의 표시 되는 부분 아래에 있습니다. 페이지 맨 위에 표시 되는 광고는 볼 수 없습니다. 페이지의 각 요소를 검사 하 여 앱에서 볼 수 있는 공간을 최적화 하는 방법을 확인 하는 것이 좋습니다. Ad 컨트롤이 앱 배경에 배치 되지 않았는지 확인 합니다.
3.  **페이지 길이 관리** &nbsp; &nbsp; 페이지 콘텐츠가 짧으면 viewability 높아집니다. 앱 페이지를 더 길게 만들어야 하는 경우 무한 스크롤을 구현 합니다.
4.  **광고 위치 수정** &nbsp; &nbsp; 사용자가 스크롤하면 광고가 고정 된 위치에 유지 되는지 확인 합니다. 이를 통해 더 높은 보기 시간을 얻고 viewability 요금을 높일 수 있습니다.
5.  **불투명도 설정** &nbsp; &nbsp; Ad 컨트롤을 포함 하는 컨테이너의 불투명도가 100% 인지 확인 합니다.
6.  **지연 로드 구현** &nbsp; &nbsp; 사용자가 광고 컨트롤을 포함 하는 뷰로 스크롤할 때까지 광고를 로드 하지 마십시오. 이렇게 하면 사용자가 볼 수 있도록 ad가 더 이상 표시 됩니다.
7.  **다양 한 ad 크기** &nbsp; &nbsp; 를 사용 하 여 실험 [광고 성능 보고서](../publish/advertising-performance-report.md)를 사용 하 여 몇 가지 ad 크기를 선택 하 고 각 항목에 대해 viewability을 측정 합니다. 가장 잘 작동 하는 항목을 선택 합니다.
8.  **앱 디자인** &nbsp; &nbsp; 다시 방문 페이지 로드 시간을 줄이기 위해 앱 페이지 디자인을 개선 합니다. 사용자는 더 빠르게 로드 되는 페이지에 더 오래 유지 되는 경향이 있습니다.
