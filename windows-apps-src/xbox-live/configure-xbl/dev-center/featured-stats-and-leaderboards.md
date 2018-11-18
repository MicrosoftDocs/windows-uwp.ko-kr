---
title: 주요 통계 및 순위표 2017
author: shrutimundra
description: 파트너 센터에서 Xbox Live 기능을 갖춘 통계 및 순위표 2017을 구성 하는 방법에 알아봅니다.
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.author: kevinasg
ms.date: 10/30/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, 게임, uwp, windows 10, Xbox one, 추천 통계 및 순위표, 순위표, 통계 2017 년 파트너 센터
ms.openlocfilehash: eebf1e96601f7bb576088f71239196f0bb5a124c
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7153660"
---
# <a name="configuring-featured-stats-and-leaderboards-2017-in-partner-center"></a>파트너 센터의 주요 통계 및 순위표 2017 구성

게임 통계 서비스와 상호 작용을, 상태는 [파트너 센터](https://partner.microsoft.com/dashboard)에서 정의 해야 합니다. 모든 주요 통계를 순위표로 자동으로 작동 하도록 낮추는 GameHub에 표시 됩니다. 원시 값 저장 됩니다. 게임을 새 값을 제공할지 여부를 확인 하는 것에 대 한 논리를 소유 하는 반면,

![게임 허브의 도전 과제 페이지의 스크린샷](../../images/dev-center/featured-stats-and-leaderboards/featured-stats-and-leaderboards-2.png) 위 그림 주요 통계 타이틀의 GameHub에서 어떻게 보이는지 보여 줍니다. 주요 통계는 내에서 빨간색 상자를 표시 합니다.

데이터 플랫폼 2017을 사용 하 여 플레이어의 GameHub 페이지에서 추천 하는 소셜 순위표에 사용 되는 상태를 구성 하기만 하면 됩니다.

추천된 상태 및 순위표 연결 된 게임을 구성 하려면 파트너 센터를 사용할 수 있습니다. 다음을 수행 하 여 구성을 추가 합니다.

1. **서비스**아래의 타이틀에 대 한 **주요 통계 및 순위표** 섹션으로 이동 > **Xbox Live** > **주요 통계 및 순위표**합니다.
2. 모달 폼 열리는 **New** 단추를 클릭 합니다. 입력 되 면 **저장**을 클릭 합니다.

![이미지의 새로운 기능 갖춘된 상태/순위표 대화 상자](../../images/dev-center/featured-stats-and-leaderboards/featured-stats.png)

**표시 이름** 필드는 사용자의 GameHub에 표시 됩니다. Xbox Live 서비스 구성의 **Localize 문자열** 에서이 문자열을 지역화할 수 있습니다.

**ID** 필드 통계 이름이 며가 참조 하 여 상태 코드에서 업데이트할 때 됩니다. 자세한 내용은 [업데이트 통계](../../leaderboards-and-stats-2017/player-stats-updating.md) 를 참조 하세요.

**형식** 는 정수, 소수점, 백분율, 짧은 timespan, 긴 timespan 및 문자열 stat. 옵션의 데이터 형식을 포함 합니다.

각 **형식** 옵션이 허용 되는 값 또는 선택 하면 아래로 드롭다운 목록에서 서식 지정에 대 한 정보를 제공 합니다.

* 정수 통계 1, 2 또는 100 같은 정수를 수락합니다.
* 10 진수 통계 1.05 또는 12.00 같은 두 개의 소수 자릿수를 사용 하 여 소수 수락
* 백분율 통계는 0과 100 사이의 정수를 수락합니다. '%' 전체 숫자 끝에 추가 됩니다. (예: 0%, 100%)
* 짧은 timespan 통계 02시 10분: 30 같은 h:mm: ss 형식 따르고 묻습니다의 상태에 대 한 시간 단위를 제공할 수 있습니다.   사용 가능한 시간 단위가 밀리초, 초, 분, 시간 및 일입니다.
* 긴 timespan 상태 1 d 2 h 10 m과 같은 Xd Xh Xm 형식을 따르면이 상태도 묻습니다의 상태에 대 한 시간 단위를 제공할 수 있습니다.

**정렬** 필드의 순위표 오름차순 또는 내림차순으로 정렬 순서를 변경할 수 있습니다.

추천된 상태 및 순위표 구성 하는 경우 다음 요구를 하십시오 note:

| 개발자 유형 | 요구 사항 | 제한 |
|----------------|-------------|-------|
| Xbox Live 크리에이터스 프로그램 | 주요 통계도 모든 통계를 지정 하는 없으며 | 20 |
| ID@XboxMicrosoft 파트너 | 최소한 3 주요 통계를 지정 해야 합니다. | 20 |

## <a name="next-steps"></a>다음 단계

다음 코드에서 통계를 업데이트 해야 합니다.  자세한 내용은 [업데이트 통계](../../leaderboards-and-stats-2017/player-stats-updating.md) 를 참조 하세요.
