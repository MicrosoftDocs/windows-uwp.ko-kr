---
title: Xbox Live 소셜 플랫폼-게임 및 게이머
author: aablackm
description: Xbox Live 소셜 플랫폼 서비스에 대해 자세히 알아보려면 링크를 제공 합니다.
ms.assetid: 27b85218-60f3-4eb0-9f7e-fe90e027db5c
ms.author: aablackm
ms.date: 09/18/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c02fc7510f3856b7e31aad4d47fcf07d935dd126
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5930101"
---
# <a name="xbox-live-social-platform---for-games-and-gamers"></a>Xbox Live 소셜 플랫폼-게임 및 게이머

타이틀을 적용 하 고 계속 참여 게이머는 재생 하 고 다른 경쟁할 데 중요 합니다. Xbox Live 소셜 네트워크 50 백만 넘는 활성 사용자를 사용 하 여 게임 및 성장 하는 최상의 제공 합니다. 게이머 들이 모여를 하 고 눈에 새롭고 흥미로운 게임을 만드는 확실와 같은 도구 집합을 작성 했습니다. 타이틀에 Xbox Live 소셜 플랫폼을 통합 하는 것은 쉽습니다 이며 투자 수익률 엄청난 적인 싱글 플레이어 게임, 한 도우미 앱 또는 대규모 멀티 플레이어 게임을 개발 합니다.

## <a name="concepts-in-this-article"></a>이 문서의 개념
- [프로필](#profile)
- [게이머 점수와 도전 과제](#gamerscore-and-achievements)
- [Your 소셜 네트워크에 Xbox Live 피플 시스템](#the-people-system---your-social-network-on-xbox-live)
- [클럽](#clubs)
- [클립과 스크린샷을](#clips-and-screenshots)
- [활동 피드](#the-activity-feed)
- [추세](#trending)
- [다양한 상태](#rich-presence)
- [게임 허브](#game-hubs)
- [Xbox Live 소셜 Api를 사용 합니다.](#use-the-xbox-live-social-apis)

## <a name="bringing-gamers-together"></a>게이머를 함께 가져오기
Xbox Live는 활성 게이머 커뮤니티를 구축 하는 데 최선을 다하고 있습니다. 따라서 게이머에 게 다른 사용자와 경험을 공유 하 고 게임 소셜 활동을 찾기 위해 도구 집합을 제공 합니다. 따라서 게임을 플레이할 때 도움이 되는 수 있지만 게이머가 친구 들과 공유할 수 없습니다. 

### <a name="profile"></a>프로필
Xbox Live에 각 게이머 프로필을 갖고 있습니다. 프로필에는 다음 정보를 포함 합니다.

-   **게이머**: 게이머의 고유한 애칭. Xbox Live에 모든 사용자에 게 게이머 태그입니다.

-   **실제 이름**: 게이머의 첫 번째 및 마지막 이름. 사용자가 실제 이름 자신의 개인 정보 설정을 통해 공유 하지 않으려는 경우이 비어 있습니다.

-   **Gamerpic**: 사진 또는 아이콘 게이머가 선택이 표현입니다.

-   **상태**: (온라인, 오프 라인으로 등 게임을 재생 합니다.) 게이머의 현재 상태

-   **게이머 점수**: 사용자가 플레이 하는 Xbox Live 게임의 모든 누적 된 총 도전 과제 점수를 나타내는 단일 값.

### <a name="gamerscore-and-achievements"></a>게이머 점수와 도전 과제
각 게이머 게임에서 [도전 과제](../achievements-2017/achievements.md) 잠금 해제 하 여 게이머 점수를 획득할 수 있습니다.
도전 과제 타이틀 수 있도록 해당 능력을 보여 주셨습니다의 서와 게임 플레이 목표를 확장 하 여 참여 게이머를 유지 하는 가장 인기 있는 기능입니다. 플레이어의 도전 과제 프로필에서 해당 친구와 비교할 수 있습니다.

> [!NOTE]
> 게이머 점수와 도전 과제는 Xbox Live 크리에이터 스 프로그램에서 만든 게임에 사용할 수 없습니다.

### <a name="the-people-system---your-social-network-on-xbox-live"></a>사용자가 시스템에 Xbox Live 소셜 네트워크
각 Xbox Live 구성원 실제 친구, Xbox Live를 충족 하는 게이머 및 잘 알려진 게이머 Vip 주 Nelson 등을 포함할 수 있는 사람 목록이 개인 친구를 유지할 수 있습니다. 

사용자 간의 관계는 한 가지 방법은 *워* 관계 하 여 친구 목록에 추가 수 있습니다. 이 다른 시스템와 달리 시스템의 친구 수 전에 관계를 확인 하려면 수행 하려는 사람에 있는. 최대 1000 명의 친구 친구 들이 목록에 추가할 수 있으며 무제한 팔로 워:

-   *친구* 하 여 친구 목록에 추가한 사용자입니다.

-   *워* 에 추가한 친구 목록 수에 따라 사람입니다.

쉽게 좋아하는 게이머는 친구에서 찾을 수 목록, 각 게이머를 즐겨찾기로 최대 100 친구를 표시할 수 있습니다.

게이머는 게임을 재생 하는 경우 Xbox Live 소셜 그래프도 게임을 플레이 한가 게이머의 친구만 표시 됩니다.

[사용자가 시스템에 자세히 알아보기](people-system/xbox-live-people-system.md) 

### <a name="clubs"></a>클럽
게이머 공유 관심사에 따라 모든 규모의 소셜 그룹을 만들 수 있습니다. 이러한 그룹 클럽 이라고 합니다.
수많은 인사 및 대화 함께 재생을 선호 하는 게이머의 큰 커뮤니티에 친구의 작은 그룹에서 클럽 범위입니다.
또한 클럽 관심 및 게임 뿐만 아니라 카테고리에 속하는 특히 다룰 수 있습니다. 다음과 같은 몇 가지 주요 클럽 이름

- 멋진 스크린샷 클럽
- 버전 복고풍 게임
- Memes는 모든 항목
- 클럽 UFC

다양 한 클럽 Xbox Live 게이머 만든 몇 가지 뿐입니다.

클럽, 게임의 팬 수 공유 클립 및 질문에 서로 다른 사용자와 게임의 스크린샷 요청 함께 채팅 하 고 다른 플레이어를 통해 검색에 대 한 그룹 (LFG) 게임을 플레이할 찾을. 클럽 참여 하 고 있는 게이머 40%가 참여 늘리기 높이고 친구를 더 합니다.

> [!NOTE]
> 클럽 단독으로 플레이어에 의해 관리 됩니다는 자연스럽 게 작업이 요구에 fanbase 내에 표시 및 개발자 부분에 입력 합니다. 하지만 단어는 > 작성자 영향을 미칠 수 없습니다. 

## <a name="getting-eyes-on-games"></a>게임에서 눈을 받기
Xbox 소셜 시스템의 중요 한 부분 작업이 공유 합니다. 특별 한 추가 혜택의 게임에서 가져오는 모든 플레이어에 대 한 게임에 더 많은 노출 제공, 모든 친구에 Xbox Live 기능 확인 한 후 최대 알고 할당할 수 있습니다. 다음과 같은 기능 도움이 될 뿐 아니라 Xbox 사용자가 게임을 검색 하 고 다른 우수 팬 아직 되 고 나면 참여 상태로 유지 합니다. 그 보다 훨씬 더 될 때마다 게임을 플레이할 수 합니다 수 유인 군단에 가입 하는 모든 친구 합니다. 

### <a name="clips-and-screenshots"></a>클립과 스크린샷을
게이머는 게임 콘텐츠를 공유 하는 가장 쉬운 방법은 클립과 스크린샷을 통해 됩니다. 플레이어 녹화를 게임 플레이 비디오 및 Xbox Live에 친구와 공유할 스크린샷이 됩니다. 자동으로 영광 이러한 순간 플레이어의 피드에 소개 된 사항이 될 됩니다.

### <a name="the-activity-feed"></a>활동 피드
게이머 타이틀을 재생 하는 경우 해당 도전 과제 완료, 클립, 스크린샷 및 기타 활동에 볼 수 있도록 모든 게임에서 제공 하는 연속 친구와 공유 되는 활동 피드 포함 됩니다.

### <a name="trending"></a>추세
Xbox Live에서 게시 하는 가장 인기 있는 콘텐츠는 Xbox Live의 Trending 섹션에 표시 됩니다. 게이머는 게임 허브에서 흥미로운 질문을 게시 또는 게임의 좋은 클립을 공유 하는 경우에 Xbox Live에 추세 콘텐츠의 기대할 수 있습니다. 이것이 타이틀에 대 한 인식 하 여 다른 좋은 방법입니다.

### <a name="rich-presence"></a>다양한 상태
현재 상태, 플레이어의 온라인 상태 방금 되지 플레이어 온라인 인지 여부를 확인 합니다. 어떤 앱도 표시 또는 게임 플레이어가 현재 눌려져와 합니다. 또한 다양 한 상태 문자열을 사용 하면 사용자가 수행 게임 내 광고는 게임 또는 플레이 가운데에 대 한 대기 메뉴를 중심으로 puttering는 이러한 여부을 합니다. 

[다양 한 상태에 자세히 알아보기](rich-presence-strings/rich-presence-strings-overview.md)

> [!NOTE]
> 다양 한 상태는 Xbox Live 크리에이터 스 프로그램에서 만든 게임 사용할 수 없습니다.

### <a name="game-hubs"></a>게임 허브
Xbox Live를 통합 하 여 게임의 게임 허브로 알려진 허브 페이지를 자동으로 가져옵니다. 게임 허브에서 Xbox Live 게임에 대 한 공식 대상 이며 플레이어 게임 관련 활동, 도전 과제, 그룹, 클럽, 토너먼트, 등을 찾을 수 있습니다.

필요에 따라 피드를 통해 사용자와 상호 작용 하 여 게임 허브에 대 한 커뮤니티 관리자를 설정할 수 있습니다. 게이머는 게임을 재생 하는 자동으로 구독에 있는 Game Hub 활동 피드를 게이머에 게 도달 하는 강력한 메커니즘을 제공 합니다. 평균적으로 게임 허브 피드에 post 외부 인기 있는 소셜 미디어 플랫폼에서 10 배 유사한 게시물 보다 더 많은 참여를 받게 됩니다.

##  <a name="use-the-xbox-live-social-apis"></a>Xbox Live 소셜 Api를 사용 합니다.
게이머와 게이머가 친구 들을 유지 하기 위해 Api를 통해 Xbox Live 소셜 사용할 제목에 참여 합니다 `SocialManager` 클래스.  소개 [소셜 관리자를](intro-to-social-manager.md) 사용 하는 방법 학습