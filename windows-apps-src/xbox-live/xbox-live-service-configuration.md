---
title: Xbox Live 서비스 구성
description: Xbox Live 게임 서비스를 구성 하는 방법에 알아봅니다.
ms.assetid: 631c415b-5366-4a84-ba4f-4f513b229c32
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 서비스 구성
ms.localizationpriority: medium
ms.openlocfilehash: d12c66e61a189c13ddbcd96dd99caa351206ecf6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640908"
---
# <a name="xbox-live-service-configuration"></a>Xbox Live 서비스 구성

## <a name="what-is-service-configuration"></a>서비스 구성 이란?

수 Xbox Live 기능 중 일부를 사용 하 여 친숙 한와 같은 [성과](achievements-2017/achievements.md), [순위표](leaderboards-and-stats-2017/leaderboards.md) 하 고 [매 치메이 킹](multiplayer/multiplayer-concepts.md#smartmatch-matchmaking)합니다.

가 아닌 경우 예를 들어 순위표 간단 하 게 설명 됩니다. 순위표 수 다른 플레이어에 비해 실적을 나타내는 값을 참조 하세요.  예를 들어 고득점 레이싱 게임에서는 랩 시간 또는 1 인칭 슈팅에서 헤드샷, 아케이드 게임을 합니다. 이지만 해당 물리적 컴퓨터에서 재생 한 선수에서 최고 점수를 표시 하는 아케이드 컴퓨터를 달리 Xbox Live를 사용한 전 세계에서 최고 득점을 표시할 수 있습니다.

있지만 그러려면, Xbox Live에 순위표에 대 한 알 수 있도록 몇 가지 일회성 구성을 수행 해야 합니다. 예를 들어 오름차순 또는 내림차순 값에서 값이 정렬 되도록 해야 하는지 여부 및 데이터의 어느 부분이 해야 수 정렬 합니다.

이 구성이 이루어집니다 [파트너 센터](https://partner.microsoft.com/dashboard) 대부분의 시간입니다. 특정 개발자가 사용 되지만 [Xbox 개발자 포털 (XDP)](https://xdp.xboxlive.com)합니다.

Xbox Live 크리에이터 스 프로그램의 일부로 개발을 제목 인 경우 사용할 [파트너 센터](https://partner.microsoft.com/dashboard)를 참조할 수 있습니다 [가져오기 시작 된 Xbox Live](get-started-with-creators/get-started-with-xbox-live-creators.md) 설정 하는 방법을 알아보려면.

경우는 ID@Xbox 개발자 또는 Microsoft 파트너는 게시자를 사용 하 여 작업의을 참조 하세요.

## <a name="choose-your-development-portal"></a>개발 포털 선택

위에서 설명 했 듯이 Xbox Live Services를 구성 하는 데 사용할 수 있는 두 개의 서로 다른 포털 있습니다. 파트너 센터 [ https://partner.microsoft.com/dashboard ](https://partner.microsoft.com/dashboard) 와 Xbox 개발 포털 (XDP)에서 [ https://xdp.xboxlive.com ](https://xdp.xboxlive.com)합니다.

앞으로 모든 타이틀에 대 한 파트너 센터 좋지만 특정 기능을 계속 하려는 XDP를 사용 합니다. 이 섹션을 제목을 구성 하는 위치 검사가 하는 데 도움이 됩니다.

선택한 포털에 따라 특정 서비스 구성 페이지에 대 한 정보를 찾을 수 있습니다.

* [파트너 센터 구성](configure-xbl/windows-dev-center.md)
* [Xbox 개발 포털 구성](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/atoc-service-configuration) -이 링크에 액세스 하려면 Microsoft 계정 (MSA)가 Xbox Live에 완전히 액세스 가능한 있어야 합니다.

구성 제목에 이미 있는 경우 아래로 스크롤할 수 있습니다 [사용자 Id를 가져오려면](#get_ids) 제목을 설정 하는 데 필요한 다양 한 id를 가져오는 방법입니다.

### <a name="pcmobile-uwp-game-only"></a>PC/모바일 UWP 게임만
파트너 센터 구성 및 Windows 10 Pc 및/또는 Windows 10 mobile 장치에만 실행 되는 UWP 게임을 관리 하기 위한 것이 좋습니다.

#### <a name="using-xdp-to-configure-uwp-titles"></a>XDP를 사용 하 여 UWP 제목을 구성 하려면

XDP 다음 요구 사항이 있는 경우 UWP 타이틀을 구성 하는 데는 것이 좋습니다.

1. Arena를 사용 하 고 있습니다.
2. 계속 사용 하려는 XDP에 기존 사용자, 그룹 및 사용 권한 설정이 있습니다.
3. 다중 접속 세션 디렉터리 세션 기록 뷰어 등 토너먼트 도구 XDP 에서만 작동 하는 도구를 사용 합니다.
4. Xbox One XDK 기반 게임와 같은 게임의 UWP PC/모바일 버전 간에 크로스 플랫폼 play 갖게 되는 제목을 개발 하는입니다.

이러한 범주 중 하나에 해당 하지 않습니다, 경우 파트너 센터를 사용 해야 합니다. 그렇지 않으면 XDP를 사용 하 여 UWP 제목을 구성 하는 방법에 대 한 아래 볼 수 있습니다.

XDP를 사용 하 여 UWP 응용 프로그램에 대 한 Xbox Live Services를 구성 하는 몇 가지 중요 한 제한 사항:

* **게임의 Xbox Live 서비스 구성을 XDP에서 인증서 일반 정품으로 게시 되 면 기본적인 않았습니다!** Xbox Live 서비스 구성에 대 한 게임 XDP 게임 타이틀의 수명 동안 유지 해야 합니다.
* **파트너 센터에 XDP에서 마이그레이션 경로가 없는 경우** XDP에서 Xbox Live 구성을 시작 하는 경우 수동으로 다시 만들어야이 파트너 센터에서 이동 하려는 경우.

이러한 두 가지 고려 사항이 들어에서는 않는 것이 좋습니다 PC/모바일 게임에 대 한 파트너 센터를 사용 하 여 위에 설명 된 범주 중 하나에 속합니다.

### <a name="cross-play-between-xbox-one-and-pcmobile-games"></a>Xbox One 및 PC/모바일 게임 간에 크로스 플레이 ###
Xbox One 및 크로스-play 라고 PC 간에 장치 간 게임 쇼케이스 Windows 10 환경입니다. 이 시나리오에서는 Xbox One 및 PC 버전 게임의 동일한 Xbox Live 구성을 공유 합니다.

이 시나리오는 또한 기존 Xbox One XDK 게임을가 하 고 게임의 UWP 버전 간 play 지원을 추가 하려면 있는 경우를 다룹니다.

간 재생을 구현 하려면 다음을 수행 합니다.

* **구성 및 XDK 게임을 게시 하려면 XDP를 사용 합니다.** 파트너 센터는 현재 Xbox One XDK 게임을 지원 하지 않습니다.
* **XDK와 게임의 UWP 버전에 대 한 XDP에서 만든 Xbox Live는 단일 서비스 구성을 사용 합니다.** XDP 게임 XDK 버전 및 게임의 UWP 버전 간에 단일 Xbox Live 서비스 구성을 공유할 수 있는 기능이 새로 설정 되었습니다.
* **파트너 센터를 사용 하 여 수집 하 고 UWP 게임을 게시 합니다.** 그러나 사용 하지 마십시오 파트너 센터 Xbox Live 서비스를 구성으로 게임 XDP에서 만든 서비스 구성을 사용 합니다.
* **XDP 사이의 파트너 센터 Xbox Live 서비스 구성을 분할 되지 않습니다.** XDP 및 파트너 센터 서로 인식 되지 않으며 한 소스에서 서비스 구성을 게시 하면 다른 원본에서 게시 구성을 덮어씁니다. 사용자 데이터 (누락 된 성과, 게임 저장 지워진된 등) 손실 될 수는 잘못 된 사용자 환경을 만들 수 있습니다. 이러한 이유로 **는 Xbox Live 서비스 구성의 100% 이루어집니다 XDP 간 play XDK + UWP 게임 필요 합니다.**

항목을 포함 하 여이 프로세스에 자세한 내용이 포함 되어 *되지* 에서 셀프 서비스를 [간 play 게임 시작](get-started-with-partner/get-started-with-cross-play-games.md) 가이드

### <a name="separate-versions-of-xbox-one-and-pcmobile-games-that-are-not-cross-play"></a>Xbox One 및 PC/모바일 게임 플레이 교차 되지 않는 별도 버전
동일한 게임의 PC/모바일 버전에서 별도 게임에 Xbox One 버전을 유지 하도록 결정할 수 있습니다. 이 경우 두 가지 별도 제품을 만들고 PC/모바일 UWP 게임 및 Xbox One XDK 대 한 지침만를 각각만 수행 합니다.

두 버전 모두에 대 한이 경우 동일한 서비스 구성을 사용할 수 없습니다 하 고 수동으로 만들어야 게임을 별도 각 버전에 대 한 서비스 구성을 XDP 또는 파트너 센터에서 적절 하 게 합니다.

<a name="get_ids"></a>

## <a name="get-your-ids"></a>사용자 Id를 가져오려면

Xbox Live 서비스를 사용 하려면 개발 키트 및 제목을 구성 하려면 여러 Id를 가져오는 해야 합니다. 이러한 Xbox Live 서비스 구성을 수행 하 여 얻을 수 있습니다.

제목을 XDP 또는 파트너 센터에 현재 없는 경우 위의 섹션을 참조 하세요 [Xbox Live 서비스 구성 포털](#xbox_live_portals) 지침에 대 한 합니다.

### <a name="critical-ids"></a>중요 한 Id

제목 및 Xbox One에 응용 프로그램의 개발을 위한 중요 한 세 가지 Id가: 샌드박스 ID, 제목 ID 및 서비스를 안내 합니다.

샌드박스 id로 개발 키트를 사용 하는 데 필요한 상태인 제목 ID 및 서비스 안내 초기 개발에 대 한 필요는 없지만 Xbox Live 서비스 사용에 필요한 합니다. 따라서 세 가지 모두를 한 번에 가져오는 것이 좋습니다.

#### <a name="sandbox-ids"></a>샌드박스 Id

샌드박스 개발, 개발 및 테스팅을 제목에 정리 된 환경이 있는지 확인 하는 동안 개발 키트에 대 한 콘텐츠 격리를 제공 합니다. 샌드박스 ID 샌드박스에 식별합니다. 콘솔을 하나의 샌드박스 여러 콘솔에서 액세스할 수 있지만 한 시점 하나 샌드박스를 액세스할만 수 있습니다.

샌드박스 Id 대/소문자를 구분 합니다.

**파트너 센터**

파트너 센터에서 제목을 구성 하는 경우에 "Xbox Live" 루트 구성 페이지에서 아래와 같이 샌드박스 ID를 가져옵니다.

![](images/getting_started/devcenter_sandbox_id.png)

**XDP**

XDP에서 제목을 구성 하는 경우 얻게에 샌드박스 ID 개요 페이지에서 아래와 같이 제품:

![](images/getting_started/xdp_sandbox_id.png)

#### <a name="service-configuration-id-scid"></a>서비스 구성 ID (서비스 안내)

개발의 일부로, 이벤트, 성과, 및 기타 온라인 기능의 호스트를 만듭니다. 이러한 서비스 구성의 일부인 모든 및 액세스용 서비스를 안내 합니다.

SCIDs 대/소문자를 구분 합니다.

**파트너 센터**

파트너 센터에서 서비스에 안내를 검색 하려면 Xbox Live 서비스 섹션으로 이동 하 고 이동할 *Xbox Live 설치* 합니다. 사용자 서비스 안내는 아래 표에 표시 됩니다.

![](images/getting_started/devcenter_scid.png)

**XDP**

XDP에 서비스 사용자 안내를 검색 하려면 제목 아래에 있는 "제품 Setup" 섹션으로 이동 하 고 제목 ID와 서비스 안내 표시 됩니다.

![](images/getting_started/xdp_scid.png)

#### <a name="title-id"></a>제목 ID

제목 ID Xbox Live 서비스에 제목을 고유 하 게 식별합니다. 타이틀의 라이브 콘텐츠, 사용자 통계, 성과, 및 등을 액세스 하 고 실시간 다중 접속 기능을 사용 하 여 사용자를 사용 하도록 설정 하려면 서비스 전체에서 사용 됩니다.

제목 Id 사용 방법 및 위치에 따라 대/소문자를 수 있습니다.

**파트너 센터**

파트너 센터에서 제목 ID 하에서 서비스 안내와 동일한 테이블에서 발견 되는 *Xbox Live 설치* 페이지입니다.

**XDP**

서비스를 안내는 동일한 위치에서 XDP 프로그램 제목 ID는 가져옵니다.

<a name="xbox_live_portals"></a>
