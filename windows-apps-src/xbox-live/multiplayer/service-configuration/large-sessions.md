---
title: 큰 세션
author: KevinAsgari
description: Xbox Live 멀티 플레이 플랫폼을 사용 하 여 큰 세션을 사용 하는 방법을 알아봅니다.
ms.author: kevinasg
ms.date: 07/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 멀티 플레이어, 큰 세션, 최근 플레이어
ms.localizationpriority: medium
ms.openlocfilehash: cead1a3ca1d56185ef97fe3f3271484bfbc58f18
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4415152"
---
# <a name="large-sessions"></a>큰 세션

100 개 이상의 멤버를 처리할 수 있는 멀티 플레이 세션에 필요 하면 큰 세션을 사용 해야 합니다. 이 시나리오는 온라인 멀티 플레이 (MMO) 게임 및 브로드캐스트 (대부분의 멤버는 spectators), 가장 일반적인 하지만 다른 스타일을 응용 프로그램의 게임도 있을 수 있습니다.

상황에 따라 라면도 큰 세션을 사용 하 여 플레이어의 작은 그룹으로 처리 하는 경우. 동일한 세션 하지만 반드시 알고 있어야 서로 게임에서 서로 발견 하지 않는 경우 여러 플레이어를 원하는 경우에 큰 세션의 "오류가" 속성을 사용할 수 있습니다.

큰 세션 현재 지원 되지 않는 [멀티 플레이어 관리자 (MPM)](../multiplayer-manager.md)또는 [Xbox 통합 멀티 플레이어 (XIM)](../xbox-integrated-multiplayer.md) 에서 하므로 직접 호출 멀티 플레이어 서비스 디렉터리 (MPSD)를 사용 하 여 멀티 플레이 2015 Api를 사용 해야 합니다.

큰 세션 일반 세션 보다 약간 다르게 처리 됩니다.

* 일반 세션 하지만 되어 더 효율적인 보다 더 적은 정보를 포함 합니다.
* 최대 10,000 구성원을 지원합니다.
* 큰 세션에 가입할 수 없습니다.
* 큰 세션의 구성원에 대 한 최근 플레이어 목록에 없는 자동 포함이 있습니다.

## <a name="recent-players"></a>최근 플레이어

Xbox Live의 기능 중 하나는 플레이어가 Xbox Live 멀티 플레이 게임 새로운 사람들과, 게임 후 볼 수 있습니다 해당 플레이어가 **최근 플레이어** 목록에서 해당 대시보드에서입니다. 플레이어가 게임에서 새로운 플레이어와 효율적인 환경을 있으면 친구로 추가 또는 게임을 다시 정의할 수 있습니다. 플레이어를 사용 하 여 저하 사용 했던 경우 나중에 가급적 및/또는 게임이 종료 되 면 잘못 된 문제를 보고 하고자 할 수도 있습니다.

일반 세션을 사용 하 여 Xbox Live에 자동으로 추가 플레이어 동일한 세션에서 최근 플레이어 목록. 그러나 큰 세션을 사용 하는 경우 최근 플레이어 목록이 제대로 채워집니다 몇 가지 추가 단계를 수행 해야 합니다.

## <a name="set-up-a-large-session"></a>큰 세션을 설정

를 설정 하려면 세션 크게 추가 `“large”: true` 접근 권한 값 섹션에 세션 템플릿. 설정할 수 있도록 하는 `maxMembersCount` 최대 10,000 합니다. 세션 템플릿 같은 아래 작동 해야 합니다.

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 2000,
            "visibility": "open",
            "capabilities": {
                "gameplay": true,
                "large": true
            },
            "timeouts": {
                "inactive": 0,
                "ready": 180000,
                "sessionEmpty": 0
            }
        },
        "custom": { }
    }
}
```

## <a name="working-with-large-sessions"></a>큰 세션 작업

큰 세션 MPSD를 작성할 때에 초당 10 쓰기를 넘지 않는 것이 좋습니다. 이것은 일반적으로 플레이어 당 평균 쓰기 2 분 마다 1000 플레이어 세션 (예: 조인/나가기).

다른 속성 하지 큰 세션에서 유지 관리 해야 합니다.

### <a name="associating-players-from-the-same-large-session"></a>동일한 큰 세션에서 플레이어를 연결합니다.

MPSD에서 큰 세션을 검색할 때 구성원 목록은 응답을 사용 하 여 다시 제공 되지 및 실제로 전체 목록을 가져올 수 있는 방법은 없습니다. 대신, 호출자에 게 세션에 있으면 해당 구성원 레코드 됩니다 "구성원" 컬렉션에 하나만 "me" (처럼 요청)으로 레이블이 지정 된 합니다.

즉, 클라이언트가 멤버를 세션에 직접 항목을 업데이트할 수만 및 Xbox Live 함께 재생 되는 플레이어를 연결 하는 데 사용할 수 있는 일반적인 식별자를 제공 하도록 서버에 의존 합니다.

두 가지 방법으로 세션에서 해당 사용자를 나타내기 위해 재생 함께 (신뢰도 및 최근 플레이어 상태 업데이트).

#### <a name="1-persistent-groups"></a>1. 영구 그룹

사용자 그룹에 남아 있는 경우 함께 지속적으로 잠재적으로, 들어오고 사람들과 이름 (예를 들어 guid – 일반 세션의 경우와 동일한 명명 규칙) 그룹 지정할 수 있습니다.  각 구성원 그룹에서 이동 하며, 이러한 해야 추가 또는 제거할 문자열의 배열에는 고유한 "그룹" 속성에 그룹 이름:

```json
{
    "members": {
        "me": {
            "properties": {
                "system": {
                    "groups": [ "boffins-posse" ]
                }
            }
        }
    }
}
```

#### <a name="2-brief-encounters"></a>2. 간단한 오류가

두 사람이 간략 한 일회성 발생할 경우 게임 "오류가" 배열 대신 사용할 수 있습니다. 각 부여 이름, 발생 하 고 발견, 후 모두 (또는 모든) 참가자가 이름에 쓰기 자신의 "에서" 속성:

```json
{
    "members": {
        "me": {
            "properties": {
                "system": {
                    "encounters": [ "trade.0c7bbbbf-1e49-40a1-a354-0a9a9e23d26a" ]
                }
            }
        }
    }
}
```

예를 들어 하나의 플레이어만 "거래 와" 그룹을 하는 경우 그룹의 구성원 필요가 없습니다 – "그룹" 및 "에서" 모두 동일한 이름을 사용할 수 있습니다 (해당 "그룹"에 그룹 이름을 이전에 추가 되었을 가정), 아무 작업도 수행할 수 있고 사용자에 게는 발견 했습니다 u pload "에서" 목록에 그룹 이름. 하는 개별 최근 플레이어로 이동 하 고 그 반대로 그룹의 모든 구성원을 확인 하면 됩니다.

그룹의 구성원 30 초 동안 계속 오류가 수입니다. 오류가 일회용 이벤트 고려 하는 경우 시작 하므로 "오류가" 배열은 항상 즉시 처리 하 고 세션에서 다음 지울 합니다.  응답에는 표시 되지 됩니다.  ("그룹" 배열 변경 되거나 제거 될 때까지 스틱 또는 멤버 세션을 둡니다.)