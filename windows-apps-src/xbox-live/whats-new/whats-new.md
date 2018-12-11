---
title: Xbox Live에 대 한 새로운 기능
description: Xbox Live SDK에 대 한 새로운 기능
ms.date: 10/23/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 33e5b6afbf0d60679bfce1789be2d965fd881f1c
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8918272"
---
# <a name="whats-new-for-xbox-live"></a>Xbox Live의 새로운 기능
또한 모든 Xbox Live Api 최근 코드 변경 내용을 보려면 [Xbox Live API GitHub 커밋 기록](https://github.com/Microsoft/xbox-live-api/commits/master) 을 확인할 수 있습니다.

## <a name="in-this-article"></a>문서 내용

* [2018 년 6 월](#june-2018)
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

C Api 일부 Xbox Live 기능을 사용할 수 있습니다. 새로운 API 계층을 사용자 지정 메모리 관리, 비동기 작업의 경우 수동 스레드 관리 및 새 HTTP 라이브러리를 비롯 한 지원 되는 기능에 대 한 다양 한 혜택을 제공 합니다.

자세한 내용은 [Xbox Live C Api](../xsapi-flat-c.md)를 참조 하세요.

## <a name="august-2017"></a>2017년 8월

### <a name="xbox-live-features"></a>Xbox Live 기능

#### <a name="in-game-clubs"></a>게임의 클럽

이제 "게임의 클럽" 만들 수 있습니다. 게임의 클럽 표준 Xbox 클럽 점에서 다릅니다 개발자가 완전히 사용자 지정할 수 있으며 내부와 외부 게임에서 사용할 수 있습니다. 게임 개발자가 신속 하 게 모든 종류의 고유한 요구 사항에 맞는 팀, 부족, 명, 길드 등과 같은 게임 내에서 영구 그룹 시나리오를 빌드하는 데 사용할 수 있습니다.

피드 LFG, 및 Mixer 자유롭게 Xbox 콘솔, PC 또는 iOS/Android 장치에서를 채팅 같은 클럽 기능을 사용 하 여 게임을 서로 연결 하는 Xbox live 멤버를 유지 하는 모든 Xbox 환경에서 게임 외부에서 게임의 클럽 액세스할 수 있습니다.

Api는 만들기 및 관리 게임 내에서 직접 게임의 클럽 수 있습니다. 이러한 Api는 xbox::services::clubs 네임 스페이스에 존재합니다.

## <a name="july-2017"></a>2017년 7월

### <a name="xbox-live-features"></a>Xbox Live 기능

#### <a name="tournaments"></a>토너먼트

새로운 Api가 토너먼트 지원 하기 위해 추가 되었습니다. 이제 xbox::services::tournaments::tournament_service 클래스를 사용 하 여 타이틀에서 토너먼트 서비스에 액세스 합니다.
이러한 새 토너먼트 Api는 다음과 같은 시나리오를 사용 합니다.

* 현재 제목에 대 한 모든 기존 토너먼트 찾으려면 서비스를 쿼리 합니다.
* 서비스에서는 토너먼트에 대 한 세부 정보를 검색 합니다.
* 팀은 토너먼트에 대 한 목록을 검색 서비스를 쿼리 합니다.
* 서비스에서는 토너먼트에 대 한 팀에 대 한 세부 정보를 검색 합니다.
* 실시간으로 활동 (RTA) 구독을 사용 하 여 토너먼트 및 팀에 변경 내용을 추적 합니다.

## <a name="june-2017"></a>2017년 6월

### <a name="xbox-live-features"></a>Xbox Live 기능

#### <a name="game-chat-2"></a>게임 채팅 2

게임 채팅 업데이트 및 향상 된 버전을 출시 되었습니다. 자세한 내용은 [Game Chat 2 개요](../multiplayer/chat/game-chat-2-overview.md)를 참조 하세요.

### <a name="xbox-live-tools"></a>Xbox Live 도구

#### <a name="xbox-live-powershell-module"></a>Xbox Live PowerShell 모듈

* PowerShell 모듈 개발 컴퓨터의 샌드박스를 전환 하기 쉽도록에 추가 되었습니다. 자세한 내용은 [도구](../tools/tools.md) 를 참조 하세요.

#### <a name="bug-fixes"></a>버그 수정

* 다양 한 버그가 수정 되었습니다. 전체 목록은 [GitHub 커밋 내역](https://github.com/Microsoft/xbox-live-api/commits/master) 을 확인 합니다.

## <a name="may-2017"></a>2017년 5월

### <a name="xbox-services-apis"></a>Xbox 서비스 Api

#### <a name="multiplayer"></a>멀티 플레이

* 이제 검색 핸들을 쿼리 응답에는 사용자 지정 세션 속성이 포함 됩니다.

#### <a name="bug-fixes"></a>버그 수정

* 고정 "잘못 된 json" 대신 유효한 HTTP 오류 코드를 반환 합니다.

## <a name="april-2017"></a>2017년 4월

### <a name="xbox-services-apis"></a>Xbox 서비스 Api

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Xbox Live Api 유니버설 Windows 플랫폼 (UWP) 및 Xbox One 모두 제목에 대 한 Visual Studio 2017을 지원 하도록 업데이트 되었습니다.

#### <a name="tournaments"></a>토너먼트

새로운 Api가 토너먼트 지원 하기 위해 추가 되었습니다. 이제 사용할 수는 `xbox::services::tournaments::tournament_service` 타이틀에서 토너먼트 서비스에 액세스 하는 클래스입니다.

이러한 새 토너먼트 Api는 다음과 같은 시나리오를 사용 합니다.

* 현재 제목에 대 한 모든 기존 토너먼트 찾으려면 서비스를 쿼리 합니다.
* 서비스에서는 토너먼트에 대 한 세부 정보를 검색 합니다.
* 팀은 토너먼트에 대 한 목록을 검색 서비스를 쿼리 합니다.
* 서비스에서는 토너먼트에 대 한 팀에 대 한 세부 정보를 검색 합니다.
* 실시간으로 활동 (RTA) 구독을 사용 하 여 토너먼트 및 팀에 변경 내용을 추적 합니다.

## <a name="march-2017"></a>2017년 3월

### <a name="xbox-services-api"></a>Xbox 서비스 API

#### <a name="data-platform-2017"></a>데이터 플랫폼 2017

간소화 된 통계 API가 도입 되었습니다.  일반적으로 해당 통계 규칙 XDP 또는 파트너 센터에서 정의 하는 이벤트를 전송 했습니다 및 클라우드에서 시작 값을 업데이트는 이러한 합니다.  우리 통계 2013으로이 모델을 참조 하세요.

통계 2017을 사용 하 여 타이틀에에서 포함 되었습니다 시작 값을 제어 합니다.  간단 하 게 가장 최근의 시작 값을 사용 하 여 API를 호출 하 고 이벤트에 대 한 필요 없이 직접 서비스에 전송 하는 합니다.  이 새 `StatsManager` API를 더 많은에 [플레이어 통계](../leaderboards-and-stats-2017/player-stats.md) 를 읽을 수 있습니다

#### <a name="github"></a>GitHub

Xbox Live API (XSAPI)는 GitHub에서 사용할 수 있는 이제 [https://github.com/Microsoft/xbox-live-api](https://github.com/Microsoft/xbox-live-api).  하지만 XDK 또는 NuGet으로 제공 되는 이진 파일을 사용 하 여 패키지는 것이 좋습니다 소스를 사용 하는 및 소스 코드 기여 환영 합니다.  

### <a name="xbox-live-creators-program"></a>Xbox Live 크리에이터스 프로그램

Xbox Live 크리에이터 스 프로그램은 개발자에 게 더 광범위 하 게 Xbox Live 기능의 하위 집합을 제공 하는 개발자 프로그램입니다.  에 있는 경우는 ID@Xbox 프로그램에 영향을 사용할 수 없게 됩니다.

자세한 내용은 [개발자 프로그램 개요](../developer-program-overview.md)에서 프로그램에 대 한 합니다.

### <a name="documentation"></a>설명서

다음 새 문서를 가지가 있습니다.

| 문서 | 설명 |
|---------|-------------|
|[Xbox Live 서비스 구성](../xbox-live-service-configuration.md) | Xbox Live 타이틀에 대 한 서비스 구성을 업데이트 정보
| [구성 Xbox Live Unity의](../get-started-with-creators/configure-xbox-live-in-unity.md) | Xbox Live 크리에이터 스 프로그램 개발자 용 Unity 설치에 대 한 새로운 정보 |
| [Xbox Live 샌드박스](../xbox-live-sandboxes.md) | Xbox Live 샌드박스 및 콘텐츠 격리 하는 간단한 가이드 |
| [Xbox Live 테스트 계정](../xbox-live-test-accounts.md) | 직장 계정 및 파트너 센터에서 만드는 방법에는 방법에 대 한 정보 테스트 |

## <a name="archived"></a>보관

* [2016년 12월](1612-whats-new.md)
* [2016년 11월](1611-whats-new.md)
* [2016년 8월](1608-whats-new.md)
* [2016년 6월](1606-whats-new.md)
* [2016년 4월](1603-whats-new.md)
* [2016년 3월](1603-whats-new.md)
* [2016년 2월](1602-whats-new.md)
* [2015년 10월](1510-whats-new.md)
* [2015 년 9 월](1509-whats-new.md)
* [2015 년 8 월](1508-whats-new.md)
* [2015년 6월](1506-whats-new.md)
