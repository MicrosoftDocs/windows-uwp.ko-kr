---
title: Xbox Live 개발자 가이드
description: Xbox Live 서비스를 사용 하 여 게임을 Xbox Live 게임 네트워크에 연결 하는 방법을 알아봅니다.
ms.date: 08/22/2017
ms.topic: article
keywords: windows 10, uwp, 게임, xbox, xbox live
ms.localizationpriority: medium
ms.openlocfilehash: a3bd61c1c068689d364d7bafe89a90ed307fa126
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8922111"
---
# <a name="what-is-xbox-live"></a>Xbox Live란 무엇인가요?

Xbox Live는 전 세계의 수 백만 게이머들을 연결하는 최상의 게임 네트워크입니다. Xbox Live 기능 및 서비스를 활용 하기 위해 Windows 10 또는 Xbox One 게임에 Xbox Live를 추가할 수 있습니다.

Xbox Live 크리에이터 스 프로그램을 사용 하 여 [파트너 센터](https://partner.microsoft.com/dashboard) 계정을 가진 모든 사용자가 Windows 10 Pc와 Xbox One 콘솔에서 실행할 수 있는 Xbox Live가 지원 UWP (유니버설 Windows 플랫폼) 게임을 빌드할 수 있습니다.

멀티 플레이, 도전 과제, 네이티브 Xbox 콘솔 개발을 포함 하 여 전체 Xbox Live 경험을 활용 하고자 하는 게임 개발자를 위한 추가 개발자 프로그램을 [개발자 프로그램 개요](developer-program-overview.md)에 자세히 설명 되어 있습니다.

게임에 Xbox Live를 추가 하는 일부의 이유는 다음과 같습니다.

- Xbox Live 상태로 제공 게이머 Xbox One 및 Windows 10에서 게이머는 친구와 재생 하 고 플레이어의 거 대 한 커뮤니티와 연결할 수 있도록 합니다.
- Xbox Live 게임 클립 서사적 공유, 게이머 점수, amassing 및 아바타를 늘리는 플레이어가 게임 레거시 잠금 해제 도전 과제를 정의 하 여 빌드할 수 있습니다.
- Xbox Live 게이머를 재생 하 고 마지막으로 다른 Xbox One 또는 다른 장치에서 해당 하는 모든 저장을 가져오는 PC에서 선택할 수 있습니다.
- 1 십억 넘는 멀티 플레이 일치 항목 매달 재생을 Xbox Live 속도 성능과 안정성에 대 한 기본 제공 합니다.
- 장치 간 멀티 플레이어를 사용 하 여 게이머는 Xbox One 또는 Windows 10 PC에서 재생 되는 여부에 관계 없이 친구와 재생할 수 있습니다.

> [!note]
> 이 항목은 게임에 Xbox Live에 대 한 지원을 추가 하고자 하는 게임 개발자를 위한 합니다. 소비자 Xbox Live 정보를 찾고 [Xbox Live](http://www.xbox.com/live/)를 확인 합니다.

## <a name="how-xbox-live-works"></a>Xbox Live의 작동 방식

기술적인 측면에서 Xbox Live는 컬렉션 프로필, 친구 및 현재 상태, 통계, 순위표, 도전 과제, 멀티 플레이어, 매치 메이 킹 등의 Xbox Live 기능을 노출 하는 마이크로 서비스입니다. Xbox Live 데이터 클라우드에 저장 되 고 REST 끝점을 사용 하 여 액세스할 수 있고 클라이언트 쪽 Api 집합에서 액세스할 수 있는 보안 websocket 게임 개발자가 설계 합니다.

REST Api 외에도 클라이언트 쪽 나머지 기능을 래핑하는 Api 있습니다. 자세한 내용은 [Xbox Live Api 소개](introduction-to-xbox-live-apis.md)를 참조 하세요.

### <a name="get-started-with-xbox-live"></a>Xbox Live 시작

다음 가이드 Xbox Live 개발 시작, UWP 또는 Xbox 인지에 관계 없이 개발자 콘솔 수 있습니다.  게임 엔진을 사용 하 여 설정 하는 방법에 대 한 가이드도 있습니다.

| 항목                                                                                                                                             | 설명                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [개발자 프로그램 개요](developer-program-overview.md) | Xbox Live 개발을 사용할 수 있는 다양 한 개발자 프로그램에 설명 합니다. |
| [Xbox Live 크리에이터스 프로그램 시작](get-started-with-creators/get-started-with-xbox-live-creators.md) | Xbox Live Xbox Live 크리에이터 스 프로그램에서 시작 하는 방법. |
| [Xbox Live 시작 된 ID@Xbox 개발자 관리](get-started-with-partner/get-started-with-xbox-live-partner.md) | Xbox live 개발자에서 시작 하는 방법의 ID@Xbox 프로그램. |

### <a name="using-xbox-live"></a>Xbox Live를 사용 하 여

만든 타이틀이 있는지 한 번 및 기본 작업,이 섹션에서는 필요한 백그라운드에서 이동 하 고 코딩을 시작 합니다.

| 항목                                                                                                                                             | 설명                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Xbox Live를 사용 하 여](using-xbox-live/using-xbox-live.md) | 후 설치 타이틀 Xbox Live SDK 통합을 구현에 로그인 하 고 Xbox Live 프로그래밍에 대 한 자세한 정보를 준비가 됩니다.
| [Xbox Live를 호출 하기 위한 모범 사례](using-xbox-live/best-practices/best-practices-for-calling-xbox-live.md) | Xbox Live는 호출 패턴의 기본 사항을 잘 이해 및 유용한 타이틀을 보장 하기 제대로 수행 속도 제한 오지 않도록 합니다.
| [Xbox Live 서비스 API 문제 해결](using-xbox-live/troubleshooting/troubleshooting-the-xbox-live-services-api.md) | 발생할 수 있는 일반적인 문제 및를 해결 하는 방법에 대 한 제안 합니다.

### <a name="xbox-live-social-platform"></a>Xbox Live 소셜 플랫폼

Xbox Live 소셜 기능은 55 백만 넘는 활성 사용자에 게 인지도 확산 청중을 성장 조직적으로 유치 할 수 있습니다.  이 섹션에서는 Xbox Live 소셜 기능을 사용 하 여 시작 하는 방법을 설명 합니다.

| 항목                                                                                                                                             | 설명                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Xbox Live 소셜 플랫폼](social-platform/social-platform.md) | 경우 하면 로그인 할 수는 사용자, Xbox Live의 소셜 같은 기능을 활용 하 여 사용자의 소셜 그래프, 다양 한 상태 등을 사용 하 여 시작할 수 합니다. |

### <a name="xbox-live-data-platform"></a>Xbox Live 데이터 플랫폼

Xbox Live 데이터 플랫폼 드라이브 플레이어 통계, 도전 과제 및 순위표로 사용 합니다.  이 일련의 항목을 제목에서이 방법을 사용 하는 방법에 대 한 자세한 정보를 읽습니다.

| 항목                                                                                                                                             | 설명                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Xbox Live 데이터 플랫폼](data-platform/data-platform.md) | 가장 잘 타이틀과 통계, 순위표 및 도전 과제를 통합 하는 방법에 대 한 지침 뿐만 아니라 데이터 플랫폼에 간략하게 설명 합니다.
| [플레이어 통계](leaderboards-and-stats-2017/player-stats.md) | 통계는 순위표의 기초가 됩니다.  정의 하 고 여기서 사용 하는 방법을 알아봅니다.
| [순위표](leaderboards-and-stats-2017/leaderboards.md) | 지능적으로 순위표를 통합 하 여 사용자의 경쟁 면 해 합니다.
| [도전 과제](achievements-2017/achievements.md) | 도전 과제는 Xbox Live 및 플레이어 참여의 좋은 드라이버에서 가장 잘 알려진 기능 중 하나입니다. 타이틀에서 사용 하는 방법을 알아봅니다.

### <a name="xbox-live-multiplayer-platform"></a>Xbox Live 멀티 플레이 플랫폼

멀티 플레이어 게임 플레이 환경을 최신 상태로 유지 하 고 타이틀의 수명을 연장할 수 있는 좋은 방법입니다.  Xbox Live는 광범위 한 멀티 플레이어 및 연결 기능을 제공 합니다.  API의 다양 한 수준의 단순성 vs 유연성을 제공 하는 몇 가지 옵션이 있습니다.

| 항목                                                                                                                                             | 설명                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Xbox Live 멀티 플레이 플랫폼](multiplayer/multiplayer-intro.md) | Xbox Live 멀티 플레이 개발 하는 멀티 플레이어 관리자 및 Xbox 통합 멀티 플레이어 (XIM)와 같은 새로운 Api에 익숙하지 않은 경우 여기서 시작 합니다. |
| [멀티 플레이어 시나리오](multiplayer/multiplayer-scenarios.md) | 제안과 어떻게 수 멀티 플레이어에 통합할 타이틀에 대 한 지침입니다. |
| [Xbox 통합 멀티 플레이어](multiplayer/xbox-integrated-multiplayer.md) | Xbox 통합 멀티 플레이어 (XIM)는 추가 멀티 플레이어, 실시간 네트워킹 및 채팅 타이틀을 쉽게 독립적인된 인터페이스입니다. |
| [멀티 플레이어 관리자](multiplayer/multiplayer-manager.md) | 멀티 플레이어 관리자는 일반적인 멀티 플레이어 시나리오에 초점을 맞춘 API를 제공 합니다. |

### <a name="xbox-live-storage-platform"></a>Xbox Live 저장소 플랫폼

Xbox Live 저장소 플랫폼 타이틀 저장소와 연결 된 저장소를 제공합니다.  이 두 개의 서로 다르지만 상호 보완적 서비스입니다.  연결 된 저장은 로그인 사용자가 위치에 관계 없이 디바이스 간에 로밍 하는 클라우드 게임 저장을 구현할 수 있습니다.  타이틀 저장소 blob 제목 및 공유에서 다른 사용자 또는 사용자 마다 될 수 있는 데이터를 저장할 수 있습니다.

| 항목                                                                                                                                             | 설명                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Xbox Live 저장소 플랫폼](storage-platform/storage-platform.md) | 클라우드에서 게임 저장, 인스턴트 재생, 사용자 기본 설정 및 기타 데이터를 저장 하기 위한 Xbox Live 저장소 서비스를 사용 합니다. |
| [연결된 저장소](storage-platform/connected-storage/connected-storage-technical-overview.md) | 개요 및 연결 된 저장소의 프로그래밍 가이드 |
| [타이틀 저장소](storage-platform/xbox-live-title-storage/xbox-live-title-storage.md) | 개요 및 타이틀 저장소의 프로그래밍 가이드 |
