---
title: Xbox 아레나
description: Xbox Arena 토너먼트 게임에 대 한 실행을 사용 하는 방법에 알아봅니다.
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, arena, 토너먼트, ux
ms.localizationpriority: medium
ms.openlocfilehash: b08da01323d05c961005d562b70667dbbdf85437
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643768"
---
# <a name="xbox-arena"></a>Xbox 아레나

Xbox Arena는 만들고 광범위 한 대상 경쟁 게임의 재미를 전환 하는 목표를 사용 하 여 Xbox One 및 Windows 10에서 Xbox Live를 통해 온라인 토너먼트를 실행 하기 위한 플랫폼입니다.
경쟁 게임 및 Arena 최대한 유연 하 게 할 수 있어 모든 게시자가 마다 요구 사항이 있습니다. 따라서 Arena에서 제목에 토너먼트를 실행 하는 방법은 세 가지:

* 프랜차이즈 실행 토너먼트 Xbox Live 제공 도구 및 서비스를 설정 하 고 고유한 토너먼트를 실행 합니다.

* Xbox는 Arena 통해 제목을 대신 토너먼트를 실행 하는 데 사용할 수 있는 업계 최고의 토너먼트 주최측 (도움말)과 협력 했습니다. (지원 되는 토너먼트 주최측 목록, Microsoft 계정 관리자 문의).

* 플레이어를 만들고이 사용자가 생성 토너먼트 또는 UGTs를 사용 하 여 자신의 Arena 토너먼트 실행 수 있습니다.

가장 중요 한 제목을 분야에 대 한 지원을 추가 하면 세 가지를 모두 활용 하려면. 제목 토너먼트 게임을 촉진 하 고 일치 항목에서 참가자의 전환을 용이 하 게 사용할 수 있는 강력한 Api를 제공 합니다. Xbox Arena 허브 등록, 알림, 대괄호 및 시드 및 결과 보고와 같은 토너먼트-관리 작업을 지원 합니다.

> [!IMPORTANT]  
> Xbox Arena 관리 되는 파트너 에게만 제공 되 고 ID@Xbox 개발자. Xbox Live 크리에이터 스 프로그램을 사용할 수 있는 것입니다.

## <a name="a-titles-baseline-tournament-experience"></a>제목의 기준 토너먼트 환경

제목에 하나 이상의 토너먼트 구성 도우미를 사용 하 여 통합을 하는 경우 초기 토너먼트 경험을 제공 해야 합니다. 대회에 대 한 다양 한 환경을 제공 하도록 선택 하면 특정 토너먼트 도우미를 사용 하 여 보다 강력 하 게 통합 자유롭게 합니다. 하지만 여전히 제공 해야 초기 환경을 다른 토너먼트 주최측 및 UGTs와 프랜차이즈 실행 토너먼트 실행 하려는 경우.

### <a name="baseline-requirements-for-a-title"></a>제목에 대 한 초기 요구 사항

* 요구 사항의 전체 목록은 Microsoft 계정 관리자에 게 문의 합니다.

### <a name="ui-recommendations"></a>UI의 권장 사항

* 일치 하는 UI에서 토너먼트 인지 식별 합니다.

* 하면 로비 UI에 토너먼트 허브 및/또는 Xbox Arena shell UI에서에서 토너먼트 세부 정보 페이지 또는 토너먼트 도우미 앱 링크 사용자 UI 요소에 포함 됩니다.



초기 사용자 제목을 제공 해야 하는 환경은 간단 하 고 가능한 경쟁 형식의 많은 작업할 수 있을 만큼 유연 합니다. 사용자 환경 지침에 맞게 자유롭게 및 환경 요구 사항 타이틀의 UI 흐름에 맞게 및 원활한 사용자를 확인 합니다.

예를 들어 다음과 같은 가치를 제공해야 합니다.

* 필요한 정보를 제공는 세부 정보 페이지에서 같은 위치를 사용할 수 있거나 팝업 주 화면에 표시할 수 없습니다.

* 기존 게임 화면 또는 서로 결합 될 수 또는 여러 버전의 각 화면에 있을 수 있습니다. 예를 들어 많은 게임 포함 후 일치 "학살 보고서" 모두 "대기 결과"에 맞게 조정할 수 있는 화면 및 요구 사항 "준비" 화면.

* 화면 해당 단계에서 수행 하는 즉시 변경할 필요가 없습니다. 예를 들어 팀의 단계는 "준비" 화면에서 사용자는 "준비"에서 "재생"으로 전환, 경우에 제목 필요는 없습니다 게임 플레이를 즉시 이동 합니다. 이 수 (및 않아야 함) 스위치를 "기다리게 일치..." 단추에는 표시기-예를 들어, "준비 하려면 재생"-되도록 사용자 흐름을 제어 하므로 있을으로 더 잘 이해 합니다. 사용자 승인 전환 될 때까지 연기 "재생" 스테이지의 요구 사항에 대 한 괜찮습니다.


## <a name="arena-vs-title-roles"></a>Arena title 'roles' 비교

토너먼트 단계를 통해 진행 되는 상황 처럼 게임을 내부 및 외부로 이동 하는 것은 사용자에 대 한 복잡 해질 수 있습니다. 프로세스 재생 각 게임에 대 한 다른 경우 가능성이 더 적게을 이동할 위치 및 예상 되는 상황을 기억해 야 합니다.

> [!TIP]
> **UX 권장 사항**  
>
> 명확 하 게 구분할 수 유지 하 여 게임 및 Xbox Arena UI 간에 기능 역할을 간소화 합니다. 예를 들어, 모든 관리 관련 작업 영역에서 완료할 수 있습니다 하 고 게임 내에서 모든 게임 플레이 관련 작업 완료 됩니다.

Xbox Arena 역할 (한 토너먼트 설정)   | 타이틀의 역할 (게임 플레이)
--- | ---
<ul><li>등록 및 체크 인</li><li>알림</li><li>시드 및 대괄호</li><li>팀 구성</li></ul> |     <ul><li>Arena UI에서 참가자의 전환</li><li>멀티 플레이 로비 UI에서에서 토너먼트 관련 일치 항목을 식별합니다.</li><li>프로 모션 및/또는 토너먼트 게임 검색</li></ul>

## <a name="engineering-guidance"></a>엔지니어링 지침

문서 | description
--- | ---
[Arena 제목 통합](arena-title-integration.md) | 제목에 Xbox 분야에 대 한 지원을 통합 하는 방법에 알아봅니다.

## <a name="operations-guidance"></a>작업 지침

문서 | description
--- | ---
[Xbox Arena Operations 포털](operations-portal.md) | 만들기 및 Xbox Arena와 통합 되는 제목에 대 한 공식 토너먼트 관리를 사용할 수 있는 작업 포털을 설명 합니다.

## <a name="user-experience-guidance"></a>사용자 환경 지침

문서 | description
--- | ---
[Xbox 토너먼트 검색](discovering-xbox-tournaments.md) | 기존 토너먼트 검색에 대 한 팁 및 권장 사항을 만드는 훌륭한 사용자 환경을 제공 합니다.
[토너먼트 조인](arena-ux-join-tournament.md)  |  등록 하 고는 토너먼트 조인에 대 한 팁 및 권장 사항을 만드는 훌륭한 사용자 환경을 제공 합니다.
[Engagement 일치](arena-ux-match-engagement.md) | 플레이어는 토너먼트 거치 사용자 환경 단계를 설명 합니다.
[Arena API UI 메타 데이터](arena-apis-metadata.md)  | 게임에서 토너먼트 현재 상태에 대 한 정보를 표시 하는 데 사용할 수 있는 Arena Api에서 반환 하는 메타 데이터를 설명 합니다.
[Arena 알림](arena-notifications.md)  | Xbox Arena 토너먼트 참가자에 게 알림을 전송 하는 경우 조건을 설명 합니다.
[Arena 사용자 시나리오](arena-user-scenarios.md)  | 가장 일반적인 관련 동기 부여에 따라 게이머에 대 한 Arena 시나리오를 설명 합니다.
