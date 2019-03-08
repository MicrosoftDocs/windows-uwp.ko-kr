---
title: 새로운 기능-2017 년 4 월 Xbox Live Api 용
description: 새로운 기능-2017 년 4 월 Xbox Live Api 용
ms.assetid: ''
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, arena, 토너먼트
ms.localizationpriority: medium
ms.openlocfilehash: a9256bfce05c14a0bfa63c7ec656dd899648d844
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651308"
---
# <a name="whats-new-for-the-xbox-live-apis---april-2017"></a>새로운 기능-2017 년 4 월 Xbox Live Api 용

참조 하세요 합니다 [새로운 기능-2017 년 3 월](1703-whats-new.md) 추가 된 기능에 대 한 문서는 2017 년 3 월 릴리스에서 합니다.

## <a name="xbox-services-apis"></a>Xbox 서비스 Api

### <a name="visual-studio-2017"></a>Visual Studio 2017

Xbox Live Api 유니버설 Windows 플랫폼 (UWP) 및 Xbox One 타이틀에 대 한 Visual Studio 2017을 지원 하도록 업데이트 되었습니다.

### <a name="tournaments"></a>토너먼트

토너먼트를 지원 하기 위해 새 Api 추가 되었습니다. 이제 사용할 수 있습니다는 `xbox::services::tournaments::tournament_service` 을 제목에서 토너먼트 서비스에 액세스 하는 클래스입니다.

이러한 새 토너먼트 Api는 다음과 같은 시나리오를 지원 합니다.

* 현재 제목에 대 한 모든 기존 토너먼트 찾을 서비스를 쿼리 합니다.
* 서비스에서 한 토너먼트에 대 한 세부 정보를 검색 합니다.
* 토너먼트에 대 한 팀의 목록을 검색 하려면 서비스를 쿼리 합니다.
* 서비스에서의 팀을 토너먼트에 대 한 세부 정보를 검색 합니다.
* 변경 내용을 추적 토너먼트 및 팀 실시간 활동 (RTA) 구독을 사용 하 여 합니다.
