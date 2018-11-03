---
title: Xbox Live 서비스 구성
author: KevinAsgari
description: 게임에 대 한 Xbox Live 서비스를 구성 하는 방법을 알아봅니다.
ms.assetid: 631c415b-5366-4a84-ba4f-4f513b229c32
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox 서비스 구성
ms.localizationpriority: medium
ms.openlocfilehash: f23f35ca105a3ba2bbf9d5ec01cf94c4e20b2a4f
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5990673"
---
# <a name="xbox-live-service-configuration"></a>Xbox Live 서비스 구성

## <a name="what-is-service-configuration"></a>서비스 구성 란 무엇 인가요?

[매치 메이 킹](multiplayer/multiplayer-concepts.md#smartmatch-matchmaking) [도전 과제](achievements-2017/achievements.md), [순위표](leaderboards-and-stats-2017/leaderboards.md) 등의 Xbox Live 기능 중 일부에 익숙한 수도 있습니다.

하지 않을 경우를 예로 순위표 간략하게 설명 하겠습니다. 순위표 허용 다른 플레이어와 비교는 성과 나타내는 값을 참조 하세요.  아케이드 게임에서 레이싱 게임에서 랩 시간 또는 1 인칭 슈팅에서 헤드샷 예를 들어 최고 점수 합니다. 하지만 해당 물리적 컴퓨터에서 플레이 한 플레이어에서 최고 점수를 표시 하는 아케이드 컴퓨터, Xbox Live와 달리 전 세계에서 최고 점수를 표시할 수 있습니다.

하지만 이런 상황은 대 한 Xbox Live에 순위표에 대 한 알 수 있도록 몇 가지 일회성 구성을 수행 해야 합니다. 예를 들어를 오름차순 또는 내림차순 값, 값 해야 정렬 하는 여부 및 데이터의 어떤 부분이 것 해야 수 정렬 합니다.

이 구성은 대부분의 [개발자 센터](http://dev.windows.com) 에서 발생합니다. 하지만 특정 개발자가 [Xbox 개발자 포털 (XDP)을](http://xdp.xboxlive.com)사용 합니다.

Xbox Live 크리에이터 스 프로그램의 일부로 개발 타이틀 인 경우 [개발자 센터](http://dev.windows.com)를 사용 하 고 설정 하는 방법을 알아보려면 [하기 시작 된 Xbox Live](get-started-with-creators/get-started-with-xbox-live-creators.md) 를 읽을 수 있습니다.

경우는 ID@Xbox 개발자나 게시자는 Microsoft 파트너는 작업에 읽어보세요.

## <a name="choose-your-development-portal"></a>개발 포털 선택

위에서 설명한 대로 Xbox Live 서비스를 구성 하는 사용할 수 있는 두 개의 서로 다른 포털 있습니다. Windows 개발자 센터에서 [http://dev.windows.com](http://dev.windows.com) Xbox 개발 포털 (XDP)에서 [http://xdp.xboxlive.com](http://xdp.xboxlive.com).

Windows 개발자 센터, 앞의 제목을 모두에 대 한 좋지만 특정 기능을 여전히 하려는 경우 XDP 사용 합니다. 이 섹션에서는 타이틀을 구성 하는 위치 알려줄 하는 데 도움이 됩니다.

선택한 포털에 따라 특정 서비스 구성 페이지에 대 한 정보를 찾을 수 있습니다.

* [Windows 개발자 센터 구성](configure-xbl/windows-dev-center.md)
* [Xbox 개발 포털 구성](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/atoc-service-configuration) -이 링크를 액세스 하는 Microsoft 계정 (MSA)가 Xbox Live 전체 액세스 가능한 있어야 합니다.

구성 하는 제목, 이미 있는 경우에 타이틀을 설정 하는 데 필요한 다양 한 id를 가져오는 방법은 [사용자 Id](#get_ids) 를 아래로 스크롤할 수 있습니다.

### <a name="pcmobile-uwp-game-only"></a>PC/모바일 UWP 게임에만
Windows 개발자 센터는 Windows 10 Pc 및/또는 Windows 10 mobile 장치 에서만 실행 되는 UWP 게임 관리 및 구성에 대 한 것이 좋습니다.

#### <a name="using-xdp-to-configure-uwp-titles"></a>XDP를 사용 하 여 UWP 타이틀을 구성 하려면

XDP를 사용 하 여 다음 요구 사항 중 하나가 있다면 UWP 타이틀이 구성 수 있습니다.

1. 아레나를 사용 하 고 있습니다.
2. 계속 사용 하려는 XDP에서 기존 사용자, 그룹 및 권한 설정 했습니다.
3. 멀티 플레이어 세션 디렉터리 세션 기록 뷰어 토너먼트 도구 등 XDP 에서만 작동 하는 도구를 사용 중인 합니다.
4. Xbox One XDK 기반 게임와 같은 게임의 UWP PC/모바일 버전 사이의 플랫폼 간 플레이 않은 타이틀을 개발 하는.

이러한 범주 중 하나에 속하지 않는, 하는 경우 Windows 개발자 센터를 사용 해야 합니다. 그렇지 않으면 XDP를 사용 하 여 UWP 타이틀을 구성 하는 방법에 대 한 아래 볼 수 있습니다.

XDP를 사용 하 여 UWP 응용 프로그램에 대 한 Xbox Live 서비스를 구성 하는 몇 가지 중요 한 주의 사항:

* **게임의 Xbox Live 서비스 구성 XDP의 인증서/소매에 게시 되 면 기본적인 됩니다!** Xbox Live 서비스 구성에 대 한 게임 XDP 게임 타이틀의 수명 동안 유지 해야 합니다.
* **Windows 개발자 센터에서 XDP 마이그레이션 경로가 있습니다.** Xbox Live 구성과 XDP에서 시작 하면 수동으로 만들어야이 Windows 개발자 센터에서 이동 하려는 경우.

이러한 점을 고려 두 좋다고 권장 하였습니다 PC/모바일 게임의 경우 Windows 개발자 센터를 사용 하 여 위에서 설명한 범주 중 하나에 속하지 않는 한.

### <a name="cross-play-between-xbox-one-and-pcmobile-games"></a>Xbox One 및 PC/모바일 게임 간에 크로스 플레이 ###
Xbox One 및 크로스 플레이 라고 PC 간에 장치 간 게임은 Windows 10 쇼케이스 환경입니다. 이 시나리오에서는 게임의 버전은 Xbox One 및 PC를 모두 동일한 Xbox Live 구성을 공유 합니다.

이 시나리오는 또한 기존 Xbox One XDK 게임이, 있는 및 크로스 플레이 게임의 UWP 버전 지원을 추가 하려는 경우를 설명 합니다.

크로스 플레이 구현 하기 위해 다음을 수행 합니다.

* **구성 및 XDK 게임을 게시 하려면 XDP를 사용 합니다.** Windows 개발자 센터이 이번에 Xbox One XDK 게임을 지원 하지 않습니다.
* **단일 Xbox Live 서비스 구성의 XDK와 게임의 UWP 버전에 대 한 XDP에서 만든를 사용 합니다.** XDP은 XDK 버전과 게임의 UWP 버전 간 단일 Xbox Live 서비스 구성을 공유 하는 게임을 허용 하는 새로운 기능 되었습니다.
* **Windows 개발자 센터를 사용 하 여 수집을 UWP 게임을 게시 합니다.** 그러나 사용 하지 마십시오 Windows 개발자 센터 Xbox Live 서비스를 구성 하려면 게임 XDP에서 만든 서비스 구성을 사용 됩니다.
* **Xbox Live 서비스 구성 XDP 사이의 Windows 개발자 센터를 분할지 않습니다.** XDP 및 Windows 개발자 센터는 서로 인식 하 고 다른 소스에서 게시 구성을 덮어씁니다 게시 한 소스에서 서비스 구성. 이 인해 사용자 데이터가 (누락 된 도전 과제, 지워진된 게임 저장 등) 손실 될 수 있는 사용자 환경을 만들 수 있습니다. 이러한 이유로 **안길 Xbox Live 서비스 구성의 100% 했는지 XDP에서 크로스 플레이 XDK + UWP 게임을 요구 합니다.**

항목을 포함 하 여이 프로세스를 자세히 있으면 *하지* 셀프 서비스 [크로스 플레이 게임 시작](get-started-with-partner/get-started-with-cross-play-games.md) 가이드

### <a name="separate-versions-of-xbox-one-and-pcmobile-games-that-are-not-cross-play"></a>크로스 플레이 하지 않은 Xbox One 및 PC/모바일 게임의 개별 버전
Xbox One 게임의 버전을 동일한 게임의 PC/모바일 버전에서 별도로 유지 하도록 결정할 수 있습니다. 이 경우 두 개의 별도 제품을 만들고만 Xbox One XDK에 대 한 지침 및 PC/모바일 UWP 게임을 각각만 수행 합니다.

두 버전 모두 동일한 서비스 구성이 경우 사용할 수 없습니다 및 수동으로 만들어야 게임의 별도 각 버전에 대 한 서비스 구성을 XDP 또는 Windows 개발자 센터에서 적절 하 게 합니다.

<a name="get_ids"></a>

## <a name="get-your-ids"></a>사용자 id

Xbox Live 서비스를 사용 하려면 개발 키트 및 타이틀을 구성 하는 여러 개의 Id를 얻으려면 해야 합니다. 이러한 Xbox Live 서비스 구성을 수행 하 여 얻을 수 있습니다.

제목 XDP 또는 개발자 센터에 현재 없는 경우 지침에 대 한 [Xbox Live 서비스 구성 포털](#xbox_live_portals) 위의 섹션을 참조 하세요.

### <a name="critical-ids"></a>중요 한 Id

제목 및 Xbox One 용 응용 프로그램 개발에 대 한 중요 한 세 가지 Id는: 샌드박스 ID, 제목 ID 및 서비스를 안내 합니다.

개발 키트를 사용 하 여 샌드박스 ID가 하는 데 필요한 상태일 때 제목 ID 및 서비스 안내 초기 개발을 위해 필요는 없지만 Xbox Live 서비스를 사용할 필요 합니다. 따라서 세 모두를 한 번에 가져온 것이 좋습니다.

#### <a name="sandbox-ids"></a>샌드박스 Id

샌드박스 개발, 개발 및 타이틀 테스트를 위한 환경을 깨끗 있는지 확인 하는 동안 개발 키트에 대 한 콘텐츠 격리를 제공 합니다. 샌드박스 ID에 샌드박스를 식별합니다. 콘솔 하나의 샌드박스 여러 콘솔에 액세스할 수 있지만, 한 번에 하나의 샌드박스를 액세스할만 수 있습니다.

샌드박스 Id 대/소문자를 구분 합니다.

**개발자 센터**

개발자 센터에서 타이틀을 구성 하는 경우에 "Xbox Live" 루트 구성 페이지에서 아래와 같이 샌드박스 ID를 받기:

![](images/getting_started/devcenter_sandbox_id.png)

**XDP**

타이틀 XDP에를 구성 하는 경우 얻게 샌드박스 ID 개요 페이지에서 아래와 같이 제품에 대해:

![](images/getting_started/xdp_sandbox_id.png)

#### <a name="service-configuration-id-scid"></a>서비스 구성 ID (서비스 안내)

개발의 일부로, 이벤트, 도전 과제 및 다른 온라인 기능의 호스트 만듭니다. 이러한 모든 부분에 서비스 구성 되며 액세스는 서비스 안내 필요 합니다.

SCIDs 대/소문자를 구분 합니다.

**개발자 센터**

개발자 센터에서 서비스에 안내를 검색 하려면 Xbox Live 서비스 섹션으로 이동 하 고 *Xbox Live 설정* 으로 이동 합니다. 서비스 안내 하는 아래 표시 된 표에 표시 됩니다.

![](images/getting_started/devcenter_scid.png)

**XDP**

XDP에서 서비스에 안내를 검색 하려면 제목 아래 "제품 설치" 섹션으로 이동 하 고 제목 ID와 서비스 안내 표시 됩니다.

![](images/getting_started/xdp_scid.png)

#### <a name="title-id"></a>제목 ID

제목 ID는 고유 하 게 타이틀에 Xbox Live 서비스를 식별 합니다. 타이틀의 라이브 콘텐츠, 사용자 통계, 도전 과제 및 등, 액세스 하 고 라이브 멀티 플레이 기능을 사용 하도록 사용자를 사용 하도록 설정 하는 서비스 전체에서 사용 됩니다.

제목 Id 사용 되는 위치와 방법에 따라 대/소문자를 수 있습니다.

**개발자 센터**

개발자 센터에서 제목 ID는 *Xbox Live 설정* 페이지에서 서비스 안내 동일한 테이블에 있습니다.

**XDP**

서비스 안내 그대로 동일한 위치에서 XDP에 제목 ID는 가져옵니다.

<a name="xbox_live_portals"></a>
