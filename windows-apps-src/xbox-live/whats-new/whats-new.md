---
title: Xbox Live의 새로운 기능
description: Xbox Live SDK에 대 한 새로운 기능
ms.date: 10/23/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 33e5b6afbf0d60679bfce1789be2d965fd881f1c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614118"
---
# <a name="whats-new-for-xbox-live"></a>Xbox Live의 새로운 기능
확인할 수도 있습니다는 [Xbox Live API GitHub 커밋 기록을](https://github.com/Microsoft/xbox-live-api/commits/master) 모든 Xbox Live Api의 최근 코드 변경 내용을 확인 합니다.

## <a name="in-this-article"></a>문서 내용

* [2018년 6월](#june-2018)
* [2017년 8월](#august-2017)
* [2017년 7월](#july-2017)
* [2017년 6월](#june-2017)
* [2017년 5월](#may-2017)
* [2017년 4월](#april-2017)
* [2017년 3월](#march-2017)
* [보관](#archived)

## <a name="june-2018"></a>2018 년 6 월

### <a name="xbox-live-features"></a>Xbox Live 기능

#### <a name="c-api-layer-for-xsapi"></a>XSAPI에 대 한 C API 계층

C Api 일부 Xbox Live 기능에 사용할 수 있습니다. 새 API 계층을 사용자 지정 메모리 관리, 비동기 작업에 대 한 수동 스레드 관리 및 새 HTTP 라이브러리를 포함 하 여 지원 되는 기능에 대 한 다양 한 이점 제공 합니다.

자세한 내용은 [Xbox Live C Api](../xsapi-flat-c.md)합니다.

## <a name="august-2017"></a>2017년 8월

### <a name="xbox-live-features"></a>Xbox Live 기능

#### <a name="in-game-clubs"></a>게임 내 클럽

개발자는 "게임 내 클럽"를 만들 수 있습니다. 개발자가 완전히 사용자 지정이 가능 하며 내부 및 외부 게임에서 사용할 수 있다는 점에서 표준 Xbox 클럽에서 게임 내 클럽 다릅니다. 게임 개발자는 모든 종류의 고유한 요구 사항과 일치 하는 팀, 부족, 명, 길드 등 게임 내 영구 그룹 시나리오를 신속 하 게 빌드를 사용할 수 있습니다.

피드를 LFG, 및 Mixer 자유롭게 Xbox 콘솔, PC 또는 iOS/Android 장치에서를 채팅 같은 club 기능을 사용 하 여 게임 하는 서로 연결 하는 Xbox 라이브 구성원 상태를 유지할 수 있는 Xbox 경험에서 외부 게임에서 게임 내 클럽을 액세스할 수 있습니다.

Api 만들기 및 게임 내에서 직접 게임 내 클럽 관리를 사용할 수 있습니다. 이러한 Api xbox::services::clubs 네임 스페이스에 존재합니다.

## <a name="july-2017"></a>2017년 7월

### <a name="xbox-live-features"></a>Xbox Live 기능

#### <a name="tournaments"></a>토너먼트

토너먼트를 지원 하기 위해 새 Api 추가 되었습니다. 이제 제목에서 토너먼트 서비스에 액세스 하려면 xbox::services::tournaments::tournament_service 클래스를 사용할 수 있습니다.
이러한 새 토너먼트 Api는 다음과 같은 시나리오를 지원 합니다.

* 현재 제목에 대 한 모든 기존 토너먼트 찾을 서비스를 쿼리 합니다.
* 서비스에서 한 토너먼트에 대 한 세부 정보를 검색 합니다.
* 토너먼트에 대 한 팀의 목록을 검색 하려면 서비스를 쿼리 합니다.
* 서비스에서의 팀을 토너먼트에 대 한 세부 정보를 검색 합니다.
* 변경 내용을 추적 토너먼트 및 팀 실시간 활동 (RTA) 구독을 사용 하 여 합니다.

## <a name="june-2017"></a>2017년 6월

### <a name="xbox-live-features"></a>Xbox Live 기능

#### <a name="game-chat-2"></a>게임 채팅 2

게임 채팅의 업데이트 및 향상 된 버전을 출시 되었습니다. 자세한 내용은 참조는 [게임 채팅 2 개요](../multiplayer/chat/game-chat-2-overview.md)합니다.

### <a name="xbox-live-tools"></a>Xbox Live 도구

#### <a name="xbox-live-powershell-module"></a>Xbox Live PowerShell 모듈

* PowerShell 모듈 개발 컴퓨터에 샌드박스를 전환할 수 있도록 추가 되었습니다. 자세한 내용은 참조 하세요. [도구](../tools/tools.md)

#### <a name="bug-fixes"></a>버그 수정

* 다양 한 버그 수정. 확인 합니다 [GitHub 커밋 기록을](https://github.com/Microsoft/xbox-live-api/commits/master) 전체 목록입니다.

## <a name="may-2017"></a>2017년 5월

### <a name="xbox-services-apis"></a>Xbox 서비스 Api

#### <a name="multiplayer"></a>멀티 플레이

* 이제 검색 핸들을 쿼리 응답에 사용자 지정 세션 속성을 포함 합니다.

#### <a name="bug-fixes"></a>버그 수정

* 올바른 HTTP 오류 코드를 대신 반환 되 고 "잘못 된 json"을 고정 합니다.

## <a name="april-2017"></a>2017년 4월

### <a name="xbox-services-apis"></a>Xbox 서비스 Api

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Xbox Live Api 유니버설 Windows 플랫폼 (UWP) 및 Xbox One 타이틀에 대 한 Visual Studio 2017을 지원 하도록 업데이트 되었습니다.

#### <a name="tournaments"></a>토너먼트

토너먼트를 지원 하기 위해 새 Api 추가 되었습니다. 이제 사용할 수 있습니다는 `xbox::services::tournaments::tournament_service` 을 제목에서 토너먼트 서비스에 액세스 하는 클래스입니다.

이러한 새 토너먼트 Api는 다음과 같은 시나리오를 지원 합니다.

* 현재 제목에 대 한 모든 기존 토너먼트 찾을 서비스를 쿼리 합니다.
* 서비스에서 한 토너먼트에 대 한 세부 정보를 검색 합니다.
* 토너먼트에 대 한 팀의 목록을 검색 하려면 서비스를 쿼리 합니다.
* 서비스에서의 팀을 토너먼트에 대 한 세부 정보를 검색 합니다.
* 변경 내용을 추적 토너먼트 및 팀 실시간 활동 (RTA) 구독을 사용 하 여 합니다.

## <a name="march-2017"></a>2017년 3월

### <a name="xbox-services-api"></a>Xbox Services API

#### <a name="data-platform-2017"></a>데이터 플랫폼 2017

간소화 된 통계 API를 도입 했습니다.  일반적으로 XDP 또는 파트너 센터에서 정의 하는 상태 규칙에 해당 하는 이벤트를 전송 해야 하 고 클라우드에서 stat 값이 업데이트 합니다.  통계 2013이 모델을 참조합니다.

통계 2017을 사용 하 여에 제목은 이제 상태 값을 제어 합니다.  가장 최근 상태 값을 사용 하 여 API를 호출 하면 됩니다 하 고 이벤트에 대 한 필요 없이 직접 서비스에 전송 하는 합니다.  이 작업은 새로운 `StatsManager` API 하에서 자세히 알아볼 수 [플레이어 통계](../leaderboards-and-stats-2017/player-stats.md)

#### <a name="github"></a>GitHub

Xbox Live API (XSAPI)에서 GitHub에서 제공 됩니다 [ https://github.com/Microsoft/xbox-live-api ](https://github.com/Microsoft/xbox-live-api)합니다.  하지만 NuGet 또는 XDK를 사용 하 여 제공 되는 이진 파일을 사용 하 여 패키지는 것이 좋습니다 원본을 사용 하 여 시작 하는 및 소스 코드 기여를 환영 합니다.  

### <a name="xbox-live-creators-program"></a>Xbox Live 크리에이터스 프로그램

Xbox Live 크리에이터 스 프로그램은 광범위 한 개발자에 게 Xbox Live 기능의 하위 집합을 제공 하는 개발자 프로그램입니다.  이미 있는 경우는 ID@Xbox 프로그램에 영향을 주지 사용할 수 없게 됩니다.

자세한 내용은에서 프로그램에 대 한 [개발자 프로그램 개요](../developer-program-overview.md)합니다.

### <a name="documentation"></a>설명서

다음 새 문서를 가지가 있습니다.

| 문서 | 설명 |
|---------|-------------|
|[Xbox Live 서비스 구성](../xbox-live-service-configuration.md) | Xbox Live 제목에 대 한 서비스 구성을 수행 하는 업데이트 된 정보
| [구성 Xbox Live Unity에서](../get-started-with-creators/configure-xbox-live-in-unity.md) | Xbox Live 크리에이터 스 프로그램 개발자를 위한 Unity 설치에서 새 정보 |
| [Xbox Live 샌드박스](../xbox-live-sandboxes.md) | 샌드박스 Xbox Live 및 콘텐츠 격리에 대 한 간단한 지침 |
| [Xbox Live 테스트 계정](../xbox-live-test-accounts.md) | 파트너 센터에서 만들고 하는 방법과 계정 작업을 하는 방법에 대 한 정보 테스트 |

## <a name="archived"></a>보관

* [2016년 12월](1612-whats-new.md)
* [2016년 11월](1611-whats-new.md)
* [2016년 8월](1608-whats-new.md)
* [2016 년 6 월](1606-whats-new.md)
* [2016 년 4 월](1603-whats-new.md)
* [2016 년 3 월](1603-whats-new.md)
* [2016 년 2 월](1602-whats-new.md)
* [2015 년 10 월](1510-whats-new.md)
* [2015 년 9 월](1509-whats-new.md)
* [2015 년 8 월](1508-whats-new.md)
* [2015 년 6 월](1506-whats-new.md)
