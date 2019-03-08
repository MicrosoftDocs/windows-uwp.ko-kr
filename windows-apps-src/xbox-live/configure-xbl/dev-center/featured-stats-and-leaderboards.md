---
title: 주요 통계 및 순위표 2017
description: 파트너 센터에서 Xbox Live 주요 통계 및 순위표 2017 구성 하는 방법 알아보기
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 10/30/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, 게임, uwp, windows 10, 하나는 Xbox, 주요 통계 및 순위표, 순위표, 통계 2017 파트너 센터
ms.openlocfilehash: e152ea8aa745536f0b7843f4f7d372a79dc3223f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655108"
---
# <a name="configuring-featured-stats-and-leaderboards-2017-in-partner-center"></a>파트너 센터에서 주요 통계 및 순위표 2017 구성

통계 서비스와 상호 작용할 게임에 상태에서 정의 해야 [파트너 센터](https://partner.microsoft.com/dashboard)합니다. 모든 주요 통계를 GameHub leaderboard로 자동으로 작동 하므로에 표시 됩니다. 원시 값을 저장 하겠습니다, 게임이 새 값을 제공 해야 하는 경우를 결정 하기 위한 논리를 소유 하는 반면 합니다.

![게임 허브 성과 페이지의 스크린 샷](../../images/dev-center/featured-stats-and-leaderboards/featured-stats-and-leaderboards-2.png) 위 그림을 제목 GameHub의 주요 통계 모양을 보여 줍니다. 주요 통계는 내 빨간색 상자를 표시 합니다.

데이터 플랫폼 2017을 사용 하 여 플레이어의 GameHub 페이지에서 추천 목록에 소셜 leaderboard에 사용 되는 상태를 구성 해야 합니다.

주요 상태 및 연결 된 게임 순위표를 구성 하려면 파트너 센터를 사용할 수 있습니다. 다음을 수행 하 여 구성을 추가 합니다.

1. 로 이동 합니다 **주요 통계 및 순위표** 아래에 있는 제목으로 섹션 **Services** > **Xbox Live**  >  **통계 및 순위표를 추천**합니다.
2. 클릭 합니다 **새로 만들기** 단추를 모달 폼을 엽니다. 입력, 클릭 **저장할**합니다.

![새 항목 추천된 stat/순위표 대화 상자의 이미지](../../images/dev-center/featured-stats-and-leaderboards/featured-stats.png)

합니다 **표시 이름** 필드는 사용자에 게는 GameHub에 표시 됩니다. 이 문자열을 지역화할 수 있는 합니다 **문자열을 지역화** Xbox Live 서비스 구성의 합니다.

합니다 **ID** 필드 상태 이름을 이며 참조 하는 방법 됩니다에 상태 코드를 업데이트 하는 경우. 참조 된 [통계 업데이트](../../leaderboards-and-stats-2017/player-stats-updating.md) 대 한 자세한 내용은 합니다.

합니다 **형식** 는 stat의 데이터 형식입니다. Integer, Decimal, 백분율, 짧은 시간 범위, 긴 시간 범위 및 문자열 옵션에 포함 됩니다.

각 **형식** 옵션은 허용 되는 값 또는 선택 될 때 다운 드롭다운 아래에서 서식 지정에 대 한 정보를 제공 합니다.

* 정수 통계 1, 2, 또는 100과 같은 정수를 허용합니다.
* 10 진수 통계 1.05 또는 12.00와 같은 두 개의 소수 자릿수가 소수 허용
* 백분율 통계는 0과 100 사이의 정수를 허용합니다. '%'는 정수의 끝에 추가 됩니다. (예: 0%, 100%)
* 짧은 시간 범위 통계 02시 10분: 30 같은 h:mm: ss 형식을 따릅니다 및에서 상태에 대 한 시간 단위를 제공 하도록 요청 합니다.   사용 가능한 시간 단위에는 시간 (밀리초), 초, 분, 시간 및 일은입니다.
* 긴 timespan stat 1 d 2 시간 10 분 등 Xd Xh Xm 형식에 따라이 상태는 알려 달라고 할 것의 상태에 대 한 시간 단위를 제공 합니다.

합니다 **정렬** 필드 순위표 오름차순 또는 내림차순의 정렬 순서를 변경할 수 있습니다.

주요 통계 및 순위표를 구성 하는 경우 다음 요구 사항을 note 하십시오.

| 개발자 유형 | 요구 사항 | 제한 |
|----------------|-------------|-------|
| Xbox Live 크리에이터스 프로그램 | 주요 통계로 모든 통계를 지정할 필요가 없습니다. | 20 |
| ID@Xbox Microsoft 파트너 | 최소 3 개의 주요 통계를 지정 해야 합니다. | 20 |

## <a name="next-steps"></a>다음 단계

다음 코드에서 통계를 업데이트 해야 합니다.  참조 [통계 업데이트](../../leaderboards-and-stats-2017/player-stats-updating.md) 대 한 자세한 내용은 합니다.
