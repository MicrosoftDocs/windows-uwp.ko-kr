---
title: 새로운 기능 Xbox Live Api-2017 년 7 월
author: KevinAsgari
description: 새로운 기능 Xbox Live Api-2017 년 7 월
ms.assetid: ''
ms.author: kevinasg
ms.date: 07/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 2017 년 7 월 하나 새로운 기능, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 9583b1f51c9dbd11b803ac0d1c8871d81bc16b84
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5394962"
---
# <a name="whats-new-for-the-xbox-live-apis---july-2017"></a>새로운 기능에 대 한 Xbox Live Api-2017 년 7 월

추가 된 항목에 대 한 [새로운-2017 년 6 월자](1706-whats-new.md) 문서를 참조 하십시오 2017 년 6 월 릴리스에서 합니다.

모든 Xbox Live Api에 최근 변경 된 코드를 보려면 [Xbox Live API GitHub 커밋 기록](https://github.com/Microsoft/xbox-live-api/commits/master) 을 확인할 수도 있습니다.

## <a name="xbox-live-features"></a>Xbox Live 기능

### <a name="multiplayer-updates"></a>멀티 플레이 업데이트

이제 활동 핸들 및 검색 쿼리 응답에는 사용자 지정 세션 속성이 포함 됩니다.

### <a name="tournaments"></a>토너먼트

새 Api가 토너먼트 지원 하기 위해 추가 되었습니다. 이제 xbox::services::tournaments::tournament_service 클래스를 사용 하 여 타이틀에서 토너먼트 서비스에 액세스할 수 있습니다.
이러한 새 토너먼트 Api는 다음과 같은 시나리오를 사용 하도록 설정 합니다.
* 현재 타이틀에 대 한 모든 기존 토너먼트 찾으려면 서비스를 쿼리 합니다.
* 서비스에서는 토너먼트에 대 한 세부 정보를 검색 합니다.
* 팀은 토너먼트에 대 한 목록을 검색 하려면 서비스를 쿼리 합니다.
* 서비스에서는 토너먼트에 대 한 팀에 대 한 세부 정보를 검색 합니다.
* 실시간으로 활동 (RTA) 구독을 사용 하 여 토너먼트 및 팀에 변경 내용을 추적 합니다.
