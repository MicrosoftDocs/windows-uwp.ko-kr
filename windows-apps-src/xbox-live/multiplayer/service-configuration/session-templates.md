---
title: 멀티 플레이 세션 템플릿
author: KevinAsgari
description: Xbox Live 멀티 플레이 세션 템플릿 방법을 알아봅니다.
ms.assetid: 178c9863-0fce-4e6a-9147-a928110b53a2
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one 멀티 플레이어 세션 템플릿
ms.localizationpriority: medium
ms.openlocfilehash: be861f34293eea7b8cf9fbfb91620be29652fbff
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4393637"
---
# <a name="multiplayer-session-templates"></a>멀티 플레이 세션 템플릿

이 항목의 멀티 플레이 세션 템플릿 간략 한 개요를 제공 하 고 템플릿 복사 하 고 멀티 플레이 세션에 대 한 수정할 수 있는 몇 가지 예제를 제공 합니다.

멀티 플레이 세션 템플릿 멀티 플레이 세션을 만드는 데 사용 되는 청사진입니다. 모든 세션 미리 정의 된 템플릿을 기반 만들어야 합니다. 서식 있는 것은 동일한 템플릿에서 만들어진 모든 세션에 대 한 상수를 정의 합니다. 게임 세션 템플릿에서 만들면 게임에 추가할 수 및 세션에 추가 데이터를 수정 하지만 서식 파일에 정의 된 상수를 수정할 수 없습니다.

 자세한 내용은 [세션 개요](../multiplayer-appendix/mpsd-session-details.md)를 참조 하세요.

특정 세션 템플릿의 내용 뿐만 아니라 서비스 구성 id (서비스 안내)에 적용 되는 세션 템플릿 목록에서 멀티 플레이어 세션 디렉터리 (MPSD)를 검색할 수 있습니다.


## <a name="about-session-templates"></a>세션 템플릿 정보

세션 템플릿 만들거나 세션 수정 배치 하는 HTTP 요청을 동일한 형식을 사용 합니다. 차이점은 서식 파일은 상수 (없음 멤버, 서버 또는 속성)로 제한 됩니다. 상수는 사용자 지정 섹션과 시스템 상수 전체 범위를 비롯 한 모든 세션 상수를 수 있습니다.

### <a name="session-template-versions"></a>세션 템플릿 버전

이 항목에 정의 된 세션 템플릿 107 템플릿 계약 버전을 사용 하 여 생성 됩니다. 때 사용 하 여 새 템플릿을 만들, 107 계약 버전으로 입력이 있는지 확인 합니다.

XSAPI를 사용 하 고 디버거에서 결과 요청 시청 105 템플릿 계약 버전을 사용 하는 요청을 알 수 있습니다. MPSD는 효과적으로 "업그레이드" 이러한 요청 버전 107 런타임에 합니다.

> **참고:** 것에서 세션 템플릿에 사용 되는 요청에 다른 계약 버전을 사용 하 여 허용 합니다.

필요한 경우 버전 107 세션 템플릿 104/105 버전에서 변경할 수 있습니다. 지침은, [일반적인 문제는 경우 적응 Your 제목](../multiplayer-appendix/common-issues-when-adapting-multiplayer.md)에서 2015 멀티 플레이어에 대 한 지침을 마이그레이션하는 참조 하세요.


## <a name="session-template-default-values"></a>세션 템플릿 기본값

템플릿의 복사본으로 세션 템플릿으로 만든 각 세션을 시작 합니다. 세션 만들 때 템플릿을 포함 하지 않는 값을 제공할 수 있습니다. 다른 값이 설정 된 경우 기본 값이 경우에 따라 제공 됩니다. 예를 들어 107 계약 버전에 대 한 시간 제한의 기본 집합은:

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
Null을 지정 하 여 설정 되지 않은 상태로 유지 하는 값을 실행할 수 있습니다. 모든 기본 설정을 재정의 하 고 세션 생성 시 설정에서 값을 방지 합니다. 예를 들어 모든 구성원 없이 무기한 계속 세션 수 있도록 세션 빈 시간을 제거 하려면 다음 세션 템플릿 추가:
```json
    {
      "constants": {
        "system": {
          "sessionEmptyTimeout": null
        }
      }
    }
```
> **중요:** 템플릿을 통해 설정 된 상수 MPSD 쓰기를 통해 변경 될 수 없습니다. 값을 변경 하려면 만들기 하 고 새 템플릿을 필요한 변경 사항으로 제출 해야 합니다.


## <a name="example-session-templates"></a>세션 템플릿 예제
이 섹션에서는 다른 목적 및 네트워킹 토폴로지 세션 템플릿의 몇 가지 예를 보여 줍니다. 클라이언트에 가장 적합 한 선택 하기 전에 각 템플릿을 검토 하십시오. 수 다음 복사 하 템플릿을 필요한 변경한 후 잠재적으로 서비스 구성에 붙여넣습니다.

### <a name="standard-lobby-session"></a>표준 로비 세션
게임에 대 한 로비 세션을 만들려면 다음 템플릿을 시작 템플릿으로 사용할 수 있습니다.

* 변경 합니다 `maxMembersCount` 값 로비 세션에서 지원 하 고 플레이어의 최대 수입니다.  
* 제목 (예: Xbox One 및 PC) 여러 플랫폼에서 플레이어를 지원 하지 않는 경우 제거할 수 함께 재생 합니다 `crossPlay` 요소.  
* 다른 값도을 변경할 수 있지만 다음 값을 가지는 좋은 시작 하 여 필요한 확실 하지 않은 경우.


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

### <a name="standard-game-session-without-matchmaking"></a>매치 메이 킹 없이 표준 게임 세션
Starter 템플릿으로 다음 템플릿을 사용 하 여 게임 익명 매치 메이 킹 포함 하지 않는 고 100 개 이상의 멤버 필요 하지 않습니다 경우 게임에 대 한 게임 세션을 만들 수 있습니다.

다음은 표준 로비 세션 템플릿에서 지정 된 새 값을 참고:
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

### <a name="add-matchmaking-to-a-game-session-template-while-letting-the-multiplayer-service-handle-quality-of-service-checks"></a>멀티 플레이 서비스 확인 하 고 서비스 품질을 처리 하는 동안 게임 세션 템플릿 매치 메이 킹을 추가 합니다.

다음을 추가할 수 있습니다 `memberInitialization` 게임 플레이 템플릿에 매치 메이 킹 대 한 지원을 추가 하기 위해 json 요소입니다.

스마트 hopper을 만들 때에 hopper에 대 한 대상 세션 템플릿으로이 템플릿을 사용 합니다.

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

### <a name="add-matchmaking-to-a-game-session-template-where-quality-of-service-checks-are-handled-by-a-title-managed-data-center"></a>매치 메이 킹을 확인 하 고 서비스 품질 제목 관리 되는 데이터 센터에서 처리 하는 위치, 게임 세션 템플릿으로 추가 합니다.



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

### <a name="basic-session-template-for-client-server-game-session"></a>게임 세션에 클라이언트-서버에 대 한 기본 세션 템플릿

피어 투 피어 통신을 수행 하지 않는 및 Xbox Live 계산을 사용 하지 않지만 대신 제 3 자 호스트 서버에 연결 하는 클라이언트에는 제목에 대 한이 템플릿을 사용할 수 있습니다.
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

### <a name="lobbysmartmatch-ticket-session-template-for-peer-based-networking"></a>피어 기반 네트워킹 로비/스마트 티켓 세션 템플릿

로비 세션이 나 스마트 티켓 세션 매치 메이 킹에 플레이어의 그룹을 보내는 데 사용할에을 만들기 위한이 템플릿을 사용 합니다. 템플릿을 사용 하는 게임 세션 구성 되지 않습니다. 피어 투 피어 또는 피어와 호스트 네트워크 토폴로지를 사용 하 여 클라이언트에 의해 사용 하기 위해 위함입니다.
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

### <a name="quality-of-service-qos-templates"></a>품질 (Qos) 템플릿

클라이언트 매치 메이 킹 사용 하 고 QoS 평가, 일부 상수 MPSD 세션에 참가 하는 사용자를 관리 하는 클라이언트에 맞게 알리기 위해 세션 서식 파일에 추가 해야 합니다. 이 조정 품질의 게임 시작 준비가 된 사용자에 게 알리는 하기 전에 연결 상태를 확인 합니다. 클라이언트-서버 게임의 경우 조정 플레이어의 그룹 매치 메이 킹 전의 연결 품질 유효성을 검사 합니다.

#### <a name="peer-to-host-game-session-template-with-qos"></a>QoS 사용 하 여 호스트에 피어 게임 세션 템플릿

다음 예제에서는 QoS를 사용 하 여 게임 세션을 호스트 하려면 피어 템플릿을 제공합니다.
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

#### <a name="peer-to-peer-game-session-template-with-qos"></a>QoS 사용 하 여 피어 투 피어 게임 세션 템플릿

다음은 QoS 사용 하 여 피어 투 피어 게임 세션 템플릿의 예입니다.
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

#### <a name="client-server-xbox-live-compute-lobbymatchmaking-session-template-with-qos"></a>QoS 사용 하 여 클라이언트-서버 (Xbox Live 계산) 로비/매치 메이 킹 세션 템플릿

이 템플릿을 사용 하 여 로비 세션이 나 QoS를 사용 하 여 매치 메이 킹 세션을 만듭니다. 이 템플릿은 사용 하는 게임 세션 구성 되지 않습니다.
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

#### <a name="session-template-for-crossplay-between-xbox-one-and-windows-10"></a>Xbox One 및 Windows 10 crossplay에 대 한 세션 템플릿

이 템플릿을 사용 하 여 Xbox One 및 Windows 10 간 멀티 플레이어 crossplay를 사용 하도록 설정 합니다. UserAuthorizationStyle 접근 권한 값은 Windows 10에 액세스할 수 있습니다. 선택적 crossPlay 기능 타이틀 플랫폼 간에 초대 등 진행 중인 가입 상호 작용을 지원 하는지 나타냅니다.
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
