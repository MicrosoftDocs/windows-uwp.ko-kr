---
title: 토너먼트 참가
author: KevinAsgari
description: 게임의 토너먼트 참가 하기 위한 UI를 만드는 방법을 알아봅니다.
ms.author: kevinasg
ms.date: 10-10-2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 아레나, 토너먼트, ux
ms.localizationpriority: medium
ms.openlocfilehash: 701545085f095bd9c26598ac063780c49ee94c0d
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5558780"
---
# <a name="join-a-tournament-by-using-the-arena-ui"></a>아레나 UI를 사용 하 여는 토너먼트 참가

아레나 Api 타이틀을 제공할 수 등록, 체크 인 및 팀 정보 있지만 대 한 데이터를 사용 하 여 제공 *하지* *실행할* 수 있는 기능을 관련된 작업 합니다. 타이틀은 아레나 UI 또는의 제 3 자 토너먼트 이끌이 (를)를 사용 하거나 빌드 자신의 토너먼트 관리를 지원 해야 합니다.

## <a name="xbox-arena-ui-team-formation"></a>Xbox 아레나 UI: 팀 형성

아레나 UI 게이머 나 가입 팀에 게 메서드를 제공합니다. 제목에 대 한 필요한 사항은 없습니다.

###### <a name="ui-example-create-a-team"></a>UI 예: 팀 만들기

![팀 화면을 형성 합니다.](../../images/arena/arena-ux-create-team.png)

#### <a name="when-forming-a-team-a-gamer-can"></a>팀을 형성 하는 게이머 수 있습니다.

* 친구 또는 클럽 멤버에 참가 초대를 보냅니다.
* LFG 광고를 게시 하 여 팀 구성원을 찾습니다.
* 등록 하거나 팀의 등록을 취소 합니다.
* 팀의 구성원을 제거 합니다.

## <a name="xbox-arena-ui-registration"></a>Xbox 아레나 UI: 등록

아레나 UI 게이머 팀을 등록 하는 방법을 제공 합니다. 제목에 대 한 필요한 사항은 없습니다.

###### <a name="ui-example-register-a-team"></a>UI 예: 팀 등록

![등록 팀 화면](../../images/arena/arena-ux-register-team.png)

#### <a name="when-registering-for-a-tournament-a-user-can"></a>토너먼트를 등록할 때 사용자가 수행할 수 있습니다.

* 닫힌 토너먼트 *전에* 등록에 대 한 등록을 취소 합니다.
* 체크 인 또는 토너먼트 진행 중인 경우 팀의 등록을 취소 합니다.
* 부서를 등록 취소 합니다. *참고 팀을 벗어나지 개별 사용자 등록 전체 팀을 취소 합니다.*
* 한 대로 등록 합니다.
* 토너먼트 요구 사항 및 등록 하기 전에 규칙의 참여를 이해 합니다.
* 등록 되었는지 알림을 받습니다.
* 실시간으로 '등록' 변경 토너먼트 상태를 참조 하세요.
* 미리 보기 토너먼트 참가 시간에 대괄호를 시작합니다.
* 이미 등록은 토너먼트 참가 한 플레이어를 참조 하세요.
* 토너먼트 요건을 충족 하지 못하는 경우 등록에서 차단 됩니다.
* 플레이어를 차단 하는 경우 등록에서 차단 됩니다.

> [!div class="nextstepaction"]
> [매치 연결](arena-ux-match-engagement.md)
