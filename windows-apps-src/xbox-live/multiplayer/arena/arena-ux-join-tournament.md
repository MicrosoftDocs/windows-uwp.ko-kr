---
title: 토너먼트 조인
description: 플레이어의 게임 토너먼트 참가 대 한 UI를 만드는 방법에 알아봅니다.
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, arena, 토너먼트, ux
ms.localizationpriority: medium
ms.openlocfilehash: 1cdcfc7243463a35eccfaeb13a3b9b92b616952b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613508"
---
# <a name="join-a-tournament-by-using-the-arena-ui"></a>토너먼트 Arena UI를 사용 하 여 조인

Arena Api를 등록, 체크 인 및 팀 정보 있지만 대 한 데이터를 사용 하 여 제목을 제공할 수 있습니다 *하지* 기능을 제공 *실행* 관련된 작업을 합니다. Arena UI 또는 제 3 자 토너먼트 구성 도우미 (TO)를 사용 하거나 자신의 토너먼트 관리 지원도 제목을 사용할 수 있습니다.

## <a name="xbox-arena-ui-team-formation"></a>Xbox Arena UI: 팀 구성

Arena UI 폼 이나 팀 조인을 수에 대 한 메서드를 제공합니다. 제목에 대 한 사항은 없습니다.

###### <a name="ui-example-create-a-team"></a>UI 예제: 팀 만들기

![팀 화면 구성](../../images/arena/arena-ux-create-team.png)

#### <a name="when-forming-a-team-a-gamer-can"></a>팀을 형성 하는 경우 게이머를 수행할 수 있습니다.

* 하나 이상의 친구 또는 club 멤버를 참가 초대를 보냅니다.
* LFG 광고를 게시 하 여 팀 멤버를 찾습니다.
* 등록 하거나 팀의 등록을 취소 합니다.
* 팀의 구성원을 제거 합니다.

## <a name="xbox-arena-ui-registration"></a>Xbox Arena UI: 등록

Arena UI 팀 등록 수에 대 한 메서드를 제공 합니다. 제목에 대 한 사항은 없습니다.

###### <a name="ui-example-register-a-team"></a>UI 예제: 등록 팀

![등록 팀 화면](../../images/arena/arena-ux-register-team.png)

#### <a name="when-registering-for-a-tournament-a-user-can"></a>토너먼트를 등록할 때 사용자 수: 있습니다.

* 토너먼트에 대 한 등록 취소 *하기 전에* 등록을 닫았습니다.
* 체크 인 또는 토너먼트 진행에서 중이면 팀의 등록을 취소 합니다.
* 전체 팀의 등록을 취소 합니다. *개별 사용자는 팀을 그대로 두고 전체 팀의 등록을 취소 하는 참고 합니다.*
* captain를 등록 합니다.
* 토너먼트 요구 사항 및 등록 하기 전에 업무 규칙을 이해 합니다.
* 등록에 성공 했음을 알림을 받습니다.
* 실시간으로 '등록' 변경 토너먼트 상태를 확인 합니다.
* 토너먼트 시작 될 때 대괄호를 미리 봅니다.
* 이미 등록 된 토너먼트 한 플레이어를 참조 하세요.
* 토너먼트 자격 요건을 충족 하지 않는 경우 등록에서 차단 됩니다.
* 플레이어가 차단 된 경우 등록에서 차단 됩니다.

> [!div class="nextstepaction"]
> [Engagement 일치](arena-ux-match-engagement.md)
