---
title: 멀티 플레이 세션 템플릿
description: 멀티 플레이 세션 템플릿 Xbox Live 정보에 대해 알아봅니다.
ms.assetid: 178c9863-0fce-4e6a-9147-a928110b53a2
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one, 멀티 플레이 세션 템플릿
ms.localizationpriority: medium
ms.openlocfilehash: 0bbe4f6a3afe2d39fb18b4d4bad13e2aa91d246e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599438"
---
# <a name="multiplayer-session-templates"></a>멀티 플레이 세션 템플릿

이 항목에서는 멀티 플레이 세션 템플릿의 간략 한 개요를 제공 하 고 복사 하 고 멀티 플레이 세션에 대 한 수정할 수 있는 서식 파일의 몇 가지 예제를 제공 합니다.

멀티 플레이 세션 템플릿은 멀티 플레이 세션을 만드는 데 사용 되는 청사진과입니다. 모든 세션은 미리 정의 된 템플릿을 기반으로 생성 되어야 합니다. 템플릿으로 될 동일한 템플릿에서 만들어진 모든 세션에 대 한 상수를 정의 합니다. 세션 템플릿에서 만듭니다 게임, 게임 추가할 수는 세션에 추가 데이터를 수정 하지만 템플릿에 정의 된 상수를 수정할 수 없습니다.

 자세한 내용은 [세션 개요](../multiplayer-appendix/mpsd-session-details.md)합니다.

특정 세션 템플릿의 내용을 뿐만 아니라 서비스 구성 식별자 (서비스 안내)에 적용 되는 세션 템플릿 목록에서 다중 접속 세션 디렉터리 (MPSD) 검색할 수 있습니다.


## <a name="about-session-templates"></a>세션 템플릿 정보

세션 템플릿을 만들거나 세션을 수정 하려면 HTTP PUT 요청으로 동일한 형식을 사용 합니다. 템플릿을 상수 (없습니다 멤버, 서버 또는 속성)으로 제한 된다는 점이 다릅니다. 상수에는 사용자 지정 섹션 및 광범위 한 시스템 상수를 포함 하 여 모든 세션 상수 수 있습니다.

### <a name="session-template-versions"></a>세션 템플릿 버전

이 항목에 정의 된 세션 템플릿은 템플릿 계약 버전 107을 사용 하 여 생성 됩니다. 새 템플릿 만들기를 사용 하는 경우는 107 계약 버전으로 입력 되어 있는지 있는지 확인 해야 합니다.

디버거에서 결과 요청, 기능과 XSAPI를 사용 하는 경우 템플릿 계약 버전 105을 사용 하는 요청을 확인할 수 있습니다. MPSD는 효과적으로 "업그레이드" 이러한 요청 버전 107 런타임에 합니다.

> **참고:** 세션 템플릿에서 사용 되는 것의 요청에 서로 다른 계약 버전을 사용 하도록 허용 되는 것입니다.

필요한 경우 버전 107 세션 템플릿을 104/105 버전에서 변경할 수 있습니다. 자세한 내용은의 마이그레이션 지침을 참조 하십시오 [공통 문제 때 조정 Your 제목을 2015 멀티 플레이어 게임](../multiplayer-appendix/common-issues-when-adapting-multiplayer.md)합니다.


## <a name="session-template-default-values"></a>세션 템플릿 기본값

각 세션 세션 템플릿에서 만든 서식 파일의 복사본으로 시작 합니다. 세션을 만들 때 템플릿을 포함 하지 않는 값을 제공할 수 있습니다. 기본값 설정 된 다른 값이 없는 경우 일부 경우에 제공 됩니다. 예를 들어, 107 계약 버전에 대 한 시간 제한의 기본 집합을 다음과 같습니다.

```json
    {
      "constants": {
        "system": {
          "reservedRemovalTimeout": 30000,
          "inactiveRemovalTimeout": 0,
          "readyRemovalTimeout": 180000,
          "sessionEmptyTimeout": 0
        }
      }
    }
```
Null을 지정 하 여 설정 되지 않은 상태로 유지 하는 값을 적용할 수 있습니다. 이 모든 기본 설정을 재정의 하 고 값은 세션 생성 시 설정 되는 것을 방지 합니다. 예를 들어 세션 빈 제한 시간, 모든 멤버 없이 무기한 계속 하려면 세션을 허용을 제거 하려면 다음 세션 템플릿에 추가 합니다.
```json
    {
      "constants": {
        "system": {
          "sessionEmptyTimeout": null
        }
      }
    }
```
> **중요:** 템플릿을 통해 설정 되는 상수 MPSD에 쓰기를 통해 변경할 수 없습니다. 값을 변경 하려면 해야 만들고 필요한 변경 작업을 사용 하 여 새 템플릿을 제출 합니다.


## <a name="example-session-templates"></a>예제에서는 세션 템플릿
이 섹션에서는 토폴로지에 대 한 다양 한 용도 및 네트워킹 세션 템플릿의 몇 가지 예를 보여 줍니다. 클라이언트에 가장 적합 한 것을 선택 하기 전에 각 서식 파일을 검사 하십시오. 복사 고 템플릿에 필요한 내용을 변경한 후 잠재적으로 서비스 구성에 붙여넣습니다.

### <a name="standard-lobby-session"></a>표준 로비 세션
게임에 대 한 로비 세션을 만들려면 다음 템플릿을 시작 템플릿으로 사용할 수 있습니다.

* 변경 된 `maxMembersCount` 로비 세션에서 지원 하려는 플레이어의 최대 수 값입니다.  
* 제목 (예: Xbox One 및 PC) 서로 다른 플랫폼에서 플레이어를 지원 하지 않는 경우 함께 재생 하 고, 제거할 수 있습니다는 `crossPlay` 요소입니다.  
* 다른 값도 변경할 수 있지만 다음 값을 적절 한 값으로 시작 하려면 필요한 확실 하지 않은 경우.


```json
{
   "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 8,
            "visibility": "open",
            "capabilities": {
                "connectivity": true,
                "connectionRequiredForActiveMembers": true,
                "crossPlay": true,
                "userAuthorizationStyle": true
            },
        },
        "custom": {}
    }
}
```

### <a name="standard-game-session-without-matchmaking"></a>결혼 정보 회사 연결 없이 표준 게임 세션
세션을 만드는 게임 게임, 게임은 익명 결혼 정보를 포함 하지 않으며 100 개가 넘는 멤버를 필요 하지 않습니다 하는 경우 다음 템플릿을 시작 템플릿으로 사용할 수 있습니다.

표준 로비 세션 템플릿에서 지정 된 새 값이 다음에는 참고 합니다.
* `constants.system.inviteProtocol : "game"`
* `constants.system.capabilities.gameplay : true`

```json
{
   "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 8,
            "visibility": "open",
            "inviteProtocol": "game",
            "capabilities": {
                "connectivity": true,
                "connectionRequiredForActiveMembers": true,
                "gameplay" : true,
                "crossPlay": true,
                "userAuthorizationStyle": true
            }
        },
        "custom": {}
    }
}
```

### <a name="add-matchmaking-to-a-game-session-template-while-letting-the-multiplayer-service-handle-quality-of-service-checks"></a>멀티 플레이 서비스 품질의 서비스 검사를 처리할 수 있도록 하는 동안 게임 세션 템플릿으로 결혼 정보 회사 연결을 추가 합니다.

다음을 추가할 수 있습니다 `memberInitialization` 매 치메이 킹에 대 한 지원을 추가 하기 위해 게임 플레이 템플릿에 대 한 json 요소입니다.

프로그램 SmartMatch hopper를 만들면 hopper 프로그램에 대 한 대상 세션 서식 파일로이 템플릿을 사용 합니다.

```json
{
   "constants": {
        "system": {
            "memberInitialization": {
               "joinTimeout": 20000,
               "measurementTimeout": 15000,
               "membersNeededToStart": 2
            }
        }
    }
}
```

### <a name="add-matchmaking-to-a-game-session-template-where-quality-of-service-checks-are-handled-by-a-title-managed-data-center"></a>매 치메이 킹 제목 관리 되는 데이터 센터에서 서비스 검사의 품질은 처리 하는 위치, 게임 세션 템플릿에 추가 합니다.



```json
{
   "constants": {
        "system": {
            "peerToHostRequirements": {  
                "latencyMaximum": 250,
                "bandwidthDownMinimum": 256,
                "bandwidthUpMinimum": 256,
                "hostSelectionMetric": "latency"
            },
            "memberInitialization": {
               "joinTimeout": 15000,
               "measurementTimeout": 15000,
               "membersNeededToStart": 2
            }
        },
        "custom": {}
    }
}
```

### <a name="basic-session-template-for-client-server-game-session"></a>클라이언트-서버 게임 세션에 대 한 기본 세션 템플릿

피어-투-피어 통신을 수행 하지 않습니다 및 Xbox Live Compute을 사용 하지 않고 대신 타사 호스트 서버에 연결 하는 클라이언트에는 제목에 대 한이 템플릿을 사용할 수 있습니다.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "inviteProtocol": "game",
          "capabilities": {
            "connectionRequiredForActiveMembers": true,
            "gameplay" : true,
          },
        },
        "custom": {}
      }
    }
```

### <a name="lobbysmartmatch-ticket-session-template-for-peer-based-networking"></a>피어 기반 네트워킹 로비/SmartMatch 티켓 세션 템플릿

이 템플릿을 사용 하 여 로비 세션을 만들거나입니다만 매 치메이 킹으로 플레이어의 그룹을 보내는 데 사용할 SmartMatch 티켓 세션에 대 한 합니다. 템플릿은 게임 세션을 구성 하는 데 사용 될 필요가입니다. 피어-투-피어 나 피어와 호스트 간 네트워크 토폴로지를 사용 하 여 클라이언트에서 사용할 것입니다.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 10,
          "visibility": "open",
          "capabilities": {
            "connectionRequiredForActiveMembers": true,
          },
          "memberInitialization": {
            "membersNeededToStart": 1
          },
        },
        "custom": {}
      }
    }
```

### <a name="quality-of-service-qos-templates"></a>품질 (QoS) 서비스 템플릿

클라이언트 결혼 정보 회사 연결을 사용 하 고 QoS를 평가 하는 경우에 세션에 참가 하는 사용자를 관리 하는 클라이언트와 조정 하기 위해 MPSD 알리기 위해 세션 템플릿에 일부 상수를 추가 해야 합니다. 이렇게이 하려면 품질 게임을 시작할 준비가 된 사용자에 게 알리는 이전 연결 상태를 확인 합니다. 클라이언트-서버 게임의 경우 조정의 플레이어 그룹 결혼 정보 회사를 시작 하기 전에 연결 품질을 확인 합니다.

#### <a name="peer-to-host-game-session-template-with-qos"></a>QoS 사용 하 여 게임 세션 호스트 피어 간 템플릿

다음 예제에서는 QoS를 사용 하 여 피어와 호스트 간 게임 세션 템플릿을 제공합니다.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "inviteProtocol": "game",
          "capabilities": {
            "connectivity": true,
            "connectionRequiredForActiveMembers": true,
            "gameplay" : true
          },
          "memberInitialization": {
            "membersNeededToStart": 2
          },
          "peerToHostRequirements": {
            "latencyMaximum": 350,
            "bandwidthDownMinimum": 1000,
            "bandwidthUpMinimum": 1000,
            "hostSelectionMetric": "latency"
          }
        },
        "custom": { }
      }
    }
```

#### <a name="peer-to-peer-game-session-template-with-qos"></a>QoS 사용 하 여 피어-투-피어 게임 세션 템플릿

다음은 QoS 사용 하 여 피어-투-피어 게임 세션 템플릿의 예입니다.
```json
    {
    "constants": {
      "system": {
        "version": 1,
        "maxMembersCount": 12,
        "visibility": "open",
        "inviteProtocol": "game",
        "capabilities": {
          "connectivity": true,
          "connectionRequiredForActiveMembers": true,
          "gameplay" : true
        },
        "memberInitialization": {
          "membersNeededToStart": 2
        },
        "peerToPeerRequirements": {
          "latencyMaximum": 250,
          "bandwidthMinimum": 10000
        }
      },
      "custom": { }
     }
    }
```

#### <a name="client-server-xbox-live-compute-lobbymatchmaking-session-template-with-qos"></a>QoS 사용 하 여 클라이언트-서버 (Xbox Live Compute) 로비/매 치메이 킹 세션 템플릿

이 템플릿을 사용 하 여 로비 세션 또는 QoS를 사용 하 여 매 치메이 킹 세션을 만듭니다. 이 템플릿은 게임 세션을 구성 하는 데 사용할 아닙니다.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "memberInitialization": {
            "membersNeededToStart": 1
          }
        },
        "custom": {}
      }
    }
```

#### <a name="session-template-for-crossplay-between-xbox-one-and-windows-10"></a>Xbox One 및 Windows 10 사이의 crossplay에 대 한 세션 템플릿

이 템플릿을 사용 하 여 crossplay Xbox One 및 Windows 10 사이의 멀티 플레이어를 사용 하도록 설정 합니다. UserAuthorizationStyle 기능에는 Windows 10에 액세스할 수 있습니다. 선택적 crossPlay 기능을 제목에서 플랫폼 간 초대 등 진행 중인 조인 상호 작용을 지원 하는지 나타냅니다.
```json
    {
      "constants": {
        "system": {
          "capabilities": {
            "crossPlay": true,
            "userAuthorizationStyle": true
          },
        },
        "custom": {}
      }
    }
```
