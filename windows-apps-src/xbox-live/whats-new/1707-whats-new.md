---
title: 새로운 Xbox Live Api-2017 년 7 월
description: 새로운 Xbox Live Api-2017 년 7 월
ms.assetid: ''
ms.date: 07/16/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 2017 년 7 월 새로운 하나 xbox
ms.localizationpriority: medium
ms.openlocfilehash: 47db721a98b71fd6f4b5175a88ddccd00048d8d9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618828"
---
# <a name="whats-new-for-the-xbox-live-apis---july-2017"></a>새로운 기능-2017 년 7 월 Xbox Live Api 용

참조 하세요 합니다 [새로운 기능-2017 년 6 월](1706-whats-new.md) 추가 된 기능에 대 한 문서는 2017 년 6 월 릴리스에서 합니다.

확인할 수도 있습니다는 [Xbox Live API GitHub 커밋 기록을](https://github.com/Microsoft/xbox-live-api/commits/master) 모든 Xbox Live Api의 최근 코드 변경 내용을 확인 합니다.

## <a name="xbox-live-features"></a>Xbox Live 기능

### <a name="multiplayer-updates"></a>멀티 플레이 업데이트

이제 작업 핸들 및 검색 쿼리 응답에 사용자 지정 세션 속성을 포함 합니다.

### <a name="tournaments"></a>토너먼트

토너먼트를 지원 하기 위해 새 Api 추가 되었습니다. 이제 제목에서 토너먼트 서비스에 액세스 하려면 xbox::services::tournaments::tournament_service 클래스를 사용할 수 있습니다.
이러한 새 토너먼트 Api는 다음과 같은 시나리오를 지원 합니다.
* 현재 제목에 대 한 모든 기존 토너먼트 찾을 서비스를 쿼리 합니다.
* 서비스에서 한 토너먼트에 대 한 세부 정보를 검색 합니다.
* 토너먼트에 대 한 팀의 목록을 검색 하려면 서비스를 쿼리 합니다.
* 서비스에서의 팀을 토너먼트에 대 한 세부 정보를 검색 합니다.
* 변경 내용을 추적 토너먼트 및 팀 실시간 활동 (RTA) 구독을 사용 하 여 합니다.
