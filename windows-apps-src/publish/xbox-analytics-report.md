---
author: jnHs
Description: The Xbox analytics report in the Windows Dev Center dashboard shows you statistics about how your customers are engaging with the Xbox features in your product.
title: Xbox 분석 보고서
ms.author: wdg-dev-content
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, xbox 분석, xbox live 분석, xbox 통계
ms.localizationpriority: medium
ms.openlocfilehash: 9e69c41ec2ae6dface93b9f3148e699e448faa18
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/30/2018
ms.locfileid: "3114710"
---
# <a name="xbox-analytics-report"></a>Xbox 분석 보고서

Windows 개발자 센터 대시보드의 **Xbox 분석** 보고서에는 고객이 게임에서 Xbox 기능을 사용하는 방식에 대한 통계가 표시됩니다. 또한 클라이언트 오류를 해결하는 데 도움이 되는 서비스 상태 정보를 제공합니다.

> [!IMPORTANT]
> Xbox용 게임이나 Xbox Live 서비스를 사용하는 게임을 게시하는 경우에만 이 보고서가 표시됩니다. 이렇게 하려면 [개념 승인 프로세스](../gaming/concept-approval.md)를 통해 [Microsoft](../xbox-live/developer-program-overview.md#microsoft-partners) 파트너가 게시 된 게임 및 통해 제출한 게임을 포함 하는 이동 해야는 [ ID@Xbox 프로그램](../xbox-live/developer-program-overview.md#id). [Xbox Live 크리에이터 스 프로그램](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) 을 통해 게시 된 게임 현재이 보고서에 표시 되지 않습니다.

**분석**을 확장하고 **Xbox 분석**을 선택하여 게임의 왼쪽 탐색 메뉴에서 **Xbox 분석** 보고서를 확인할 수 있습니다.  대시보드에서 이 데이터를 보거나 [보고서를 다운로드](download-analytic-reports.md)하여 오프라인으로 볼 수 있습니다.


## <a name="overview-tab"></a>개요 탭

**개요** 탭의 섹션에는 플레이어 및 사용하는 Xbox Live 기능에 대한 정보가 표시됩니다.

이러한 많은 통계에 대해 **Xbox 평균**이 표시되므로 고객이 평균 Xbox 고객과 비교하여 Xbox를 사용하는 방식을 쉽게 볼 수 있습니다.

> [!NOTE]
> 이러한 통계는 모든 Xbox 고객이 아니라 Xbox Live에 연결된 고객의 통계입니다.


### <a name="concurrent-usage"></a>동시 사용

이 섹션에서는 분당 또는 시간별로 게임을 하는 평균 고객 수에 대해 거의 실시간으로 사용 데이터(5~15분 대기 시간)가 표시됩니다. 이 섹션의 오른쪽 상단 모서리에 있는 필터 아이콘을 선택하여 시간 범위를 선택할 수 있습니다(**마지막 시간**부터 최대 **지난 7일**까지).


### <a name="gamerscore-distribution"></a>게이머 점수 분포

이 섹션에서는 고객의 게이머 점수에 대한 정보가 표시됩니다. **모든 게임**을 선택하여 고객 전체의 게이머 점수 분포를 확인하거나 **이 게임**을 선택하여 해당 게임을 통해서만 얻은 게이머 점수의 분포를 볼 수 있습니다.


### <a name="achievement-unlocks"></a>도전 과제 완료

이 섹션에는 지정된 시간 범위에서 각 도전 과제를 완료한 총 고객 수가 표시됩니다. 이 섹션의 오른쪽 상단 모서리에 있는 필터 아이콘을 선택하여 시간 범위를 선택할 수 있습니다(**지난 1일**, **지난 30일** 또는 **수명**).


### <a name="game-statistics"></a>게임 통계

이 섹션에는 게임 고객을 위해 다른 데이터를 표시하도록 선택할 수 있는 탭이 포함되어 있습니다. 이 섹션의 통계는 특정 제품 내에서가 아니라 일반적인 기능 사용을 나타냅니다.

- **소셜 사용** 탭에는 고객이 사회적으로 상호 작용하는 방식과 관련된 데이터가 표시됩니다.
   - **게임 초대**는 초대장을 보낸 고객의 비율을 표시합니다(모든 게임).
   - **파티 채팅**은 파티 채팅을 사용하는 고객의 비율을 표시합니다(모든 게임).
   - **문자 메시지**는 Xbox 셸을 통해 메시지를 보내는 고객의 비율을 표시합니다(모든 게임).
- **스트리밍 사용** 탭은 Twitch 및 YouTube에서 게임 플레이를 보거나 스트리밍하는 게임 고객의 비율을 표시합니다(모든 게임).
- **게임 DVR 사용** 탭은 고객이 기록하고 게임 플레이를 보는 방법과 관련된 데이터를 표시합니다. 게임 클립 및 게임 플레이의 스크린샷을 보고 업로드한 고객의 비율을 볼 수 있습니다(모든 게임).


### <a name="friends-and-followers"></a>친구 및 팔로워

이 섹션은 게임을 플레이하는 고객에 대한 **친구의 평균 수** 및 **팔로워의 평균 수**를 표시합니다.


### <a name="accessory-usage"></a>액세서리 사용

이 차트는 외부 하드 드라이브를 사용하고 Xbox Elite 무선 컨트롤러(Xbox에서)를 사용하는 게임 고객의 비율을 표시합니다.

이 데이터는 제품을 외부 하드 드라이브에 설치했거나 제품을 플레이하는 동안 Elite 컨트롤러를 사용했던 고객을 의미하지는 않습니다. 제품의 고객 중 몇 명이 일반적으로 이 기능을 사용하는지 나타냅니다.


### <a name="connection-type"></a>연결 형식

이 차트는 **유선** 및 **무선**(Xbox에서) 인터넷 연결을 사용하는 제품 고객의 비율을 표시합니다.


## <a name="xbox-live-service-health-tab"></a>Xbox Live 서비스 상태 탭

**Xbox Live 서비스 상태 탭**의 섹션은 속도 제한을 포함하여 Xbox Live 클라이언트 오류의 영향을 이해하는 데 도움을 줍니다. 또한 끝점 및 상태 코드별로 드릴다운하여 이러한 문제를 해결하는 데 도움이 되는 정보를 얻을 수 있으며 제품의 호출별 Xbox Live 서비스 가용성에 대한 정보를 계속해서 제공합니다.

> [!NOTE]
> 이 정보를 검토하고 문제를 해결할 때 일반적으로 고객에게 가장 큰 영향을 미치는 오류인 속도 제한을 우선 순위로 지정하는 것이 좋습니다.


### <a name="apply-filters"></a>필터 적용

탭 위쪽에서 데이터가 표시되는 기간을 선택할 수 있습니다. 기본 설정은 **30D**(30일)이지만, **7D**(7일) 동안 데이터를 표시하거나 사용자가 지정한 데이터 범위에서 데이터를 표시하도록 선택할 수 있습니다(30일 이하). 사용자 지정 날짜 범위의 경우 모든 차트는 입력한 날짜 범위 내에 제공된 데이터의 첫 번째 날과 마지막 날까지 차트 범위를 잘라냅니다.

또한 **필터**를 확장하여 패키지 버전, 장치 유형 및/또는 샌드박스를 기준으로 이 페이지의 모든 데이터를 필터링할 수 있습니다.
- **패키지 버전**: 기본 필터는 **모든 버전**이지만 서비스 상태 데이터를 특정 패키지 버전으로 제한할 수 있습니다.
- **장치 유형**: 기본 설정은 **모든 장치**이지만 서비스 상태 데이터를 특정 장치 유형으로 제한할 수 있습니다.
- **샌드박스**: 기본 설정은 **RETAIL**이지만 서비스 상태 데이터를 특정 샌드박스로 제한할 수 있습니다.

아래에 나열된 모든 차트의 정보는 선택한 날짜 범위와 모든 필터를 반영합니다. 또한 일부 섹션에서는 추가 필터를 적용할 수 있습니다.


### <a name="client-errors-by-service"></a>서비스별 클라이언트 오류

**서비스별 클라이언트 오류** 차트는 선택한 기간 동안 각 Xbox Live 서비스의 일일 클라이언트(4xx) 오류 수를 표시합니다.

또한 **속도 제한**을 선택하여 속도 제한 오류만 볼 수도 있습니다. 선택한 기간 동안 각 Xbox Live 서비스에서 일일 속도 제한(429) 및 속도 제한 예외(429E) 오류 수를 표시합니다.

> [!NOTE]
> 429E 상태 코드는 실제로 200개의 상태 코드로 성공적으로 반환되었지만 당시 서비스가 많은 양을 경험한 경우 속도 제한이 있었을 수 있으므로 실행된 것과 동일하게 취급하는 것이 좋습니다(429).

기본적으로 이 차트는 오류 수별 상위 6개 서비스를 표시합니다. 이 섹션의 오른쪽 상단 모서리에 있는 필터 아이콘을 선택하여 다른 서비스를 선택할 수 있습니다. 한 번에 최대 6개의 서비스에 대한 오류를 볼 수 있습니다.

> [!NOTE]
> 범례에는 각 서비스에 고유한 접두사만 표시됩니다(예: **presence.xboxlive.com** 대신 **presence**). 전체 서비스 주소는 **Xbox Live 서비스 상태** 탭의 **끝점별 클라이언트 오류** 표에서 찾을 수 있습니다.


### <a name="service-availability"></a>서비스 가용성

**서비스 가용성** 차트는 선택한 기간 동안 각 Xbox Live 서비스의 일일 가용성을 표시합니다. 이는 *1-(총 서버(5xx) 오류/총 응답)* 으로 계산되며 제품에 따라 다르고 Xbox Live 전체에는 해당되지 않습니다.

기본적으로 이 차트는 가용성이 가장 낮은 6개의 서비스를 표시합니다. 이 섹션의 오른쪽 상단 모서리에 있는 필터 아이콘을 선택하여 다른 서비스를 선택할 수 있습니다. 한 번에 최대 6개의 서비스에 대한 가용성을 볼 수 있습니다.

> [!NOTE]
> 범례에는 각 서비스에 고유한 접두사만 표시됩니다(예: **presence.xboxlive.com** 대신 **presence**). 전체 서비스 주소는 **Xbox Live 서비스 상태** 탭의 **끝점별 클라이언트 오류** 표에서 찾을 수 있습니다.


### <a name="client-errors-by-endpoint"></a>끝점별 클라이언트 오류

**끝점별 클라이언트 오류** 표는 선택한 기간 동안 각 Xbox Live 서비스, 끝점 및 상태 코드별로 분류된 일일 클라이언트(4xx) 오류 수를 표시합니다. 기본적으로 표는 서비스 응답의 총 수를 내림차순으로 정렬하지만, 자체적으로 열 머리글 중 하나를 클릭하여 정렬 순서를 변경할 수 있습니다.

또한 **속도 제한**을 선택하여 속도 제한 오류만 볼 수도 있습니다. 선택한 기간 동안 각 Xbox Live 서비스, 끝점 및 상태 코드에서 일일 속도 제한(429) 및 속도 제한 예외(429E) 오류 수를 표시합니다.

> [!NOTE]
> 429E 상태 코드는 실제로 200개의 상태 코드로 성공적으로 반환되었지만 당시 서비스가 많은 양을 경험한 경우 속도 제한이 있었을 수 있으므로 실행된 것과 동일하게 취급하는 것이 좋습니다(429).










 

 