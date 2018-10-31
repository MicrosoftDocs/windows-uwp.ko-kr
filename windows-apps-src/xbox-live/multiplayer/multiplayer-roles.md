---
title: 멀티 플레이 역할
author: KevinAsgari
description: 역할을 사용 하 여 Xbox Live 멀티 플레이어에서 플레이어 역할을 정의 하는 방법을 알아봅니다.
ms.author: kevinasg
ms.date: 06/29/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one 멀티 플레이어, 역할
ms.localizationpriority: medium
ms.openlocfilehash: 7a61230d3ef857698fbe54fa528808d6166b2a3b
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5837772"
---
# <a name="roles"></a>역할

일부 게임 세션에 대 한 특정 구성원 있다고 지원, 된 medic이, 공격 등과 같은 특정 게임 플레이 역할을 지정 하는 것이 좋습니다. Wat 특정 게임 플레이 역할 채울 플레이어가 게임 슬롯을 예약할 수도 있습니다. 역할 Xbox Live 기능을 사용 하 여 서비스 플레이어는 게임 플레이 역할 할당을 추적 하 고 특정 게임 플레이 역할을 선택할 수 있는 플레이어의 최대 수를 적용할 수 있습니다.

역할의 가장 일반적인 용도 게임 세션에 대 한 게임의 특정 역할을 결정 하는 것입니다. 예를 들어 지원 클래스에서 1, 2, 1 탱크/어려운 클래스 및 5 개 공격 클래스 간에 필요한 게임 모드를 수도 있습니다.

다른 가능한 경우에는 게임 세션 최대 8 spectators 및 1 announcer 정확히 4 게임 플레이어는 지정 하는 것이 좋습니다.

역할을 사용 하 여 세션 검색 등 다른 방법을 통해 나머지 슬롯을 채우는 동안 친구를 위한 슬롯을 예약할 수 있습니다.

## <a name="role-types"></a>역할 유형

역할 유형 역할 정의의 그룹을 나타냅니다. 모든 역할 역할 형식의 일부로 정의 되어야 합니다. 역할 유형은 멀티 플레이 세션 문서에서 정의 됩니다.

구성원 수만 역할이 할당 되어야 하나의 특정된 역할 유형에 서. 예를 들어 "클래스" 역할 유형 치료, 탱크, 및 손상이 포함 되어 다음 구성원만에 할당할 수 이러한 역할 중 하나입니다.

여러 역할 유형을 정의와 구성원 각 역할 형식에서 하나 이상의 역할이 할당 될 수 있습니다. 이전 시나리오에서는 구성원이 힐 역할 선택한 있지만 squad 할당할 수도 있습니다 squad 리더 역할은 별도 역할 유형에 정의 된 경우 리더 역할을 합니다.

> **중요:** Xbox Live SDK 현재만 지원 단일 역할 유형 및 멤버 당 단일 역할을 합니다.

## <a name="role-type-properties"></a>역할 유형 속성

역할 유형을 정의 하는 경우 역할 형식에 대 한 다음 정보를 지정 해야 합니다.

* 이름 역할 유형입니다. 소문자 및 영숫자, 최대 100 자 이름 이어야 합니다.
* 역할 유형에 정의 된 역할 관리 소유자 경우.
* 경우 세션의 수명 동안 역할 형식에 정의 된 역할의 속성을 변경할 수 있습니다.
* 역할 형식에 포함 하는 역할 정의 합니다.

역할 형식이 관리 소유자 이면 소유자 세션의 구성원만 해당 유형의 역할 구성원에 게 할당할 수는 의미 합니다. 역할 유형 소유자 관리 되지 구성원을 자체 역할을 할당할 수 있습니다.

역할 유형이 설정 "hasOwners" 기능이 세션에서 관리 하는 소유자만 지정할 수 있습니다.

> Xbox Live SDK 현재 소유자 다른 구성원에 게 역할 할당을 지원 하지 않습니다.

## <a name="role-properties"></a>역할 속성

역할을 정의 하는 경우 각 역할에 대해 다음 정보를 지정 해야 합니다.

* 역할의 이름입니다. 소문자 및 영숫자, 최대 100 자 이름 이어야 합니다.
* 역할에 허용 되는 멤버의 최대 수입니다. 0 보다 커야 합니다.
* 역할을 채워야 하는 멤버의 대상 번호입니다. 대상 0 보다 커야 및 멤버의 최대 수 보다 작거나 역할에 허용 합니다.

세션 구성원 역할을 할당 한 정보는 멀티 플레이 세션 문서에서 멤버 역할에 기록 됩니다.

서비스는 멤버에 게 역할을 할당할 수 있지만 대상 번호를 적용 하지 않습니다의 최대 수를 적용 합니다.

## <a name="create-roles"></a>역할 만들기

일반적으로 역할 및 역할 유형을 [세션 템플릿](service-configuration/session-templates.md)정의 됩니다. Xbox Live SDK 하지 않는 서비스 지원 역할 및 세션 생성 하는 동안 역할 유형 정의 합니다.

### <a name="define-role-types-and-roles-in-a-session-template"></a>역할 유형 및 역할 세션 템플릿에서 정의

Xbox Live 구성 하는 동안 세션 템플릿을 만들 때 역할 유형 및 역할을 정의할 수 있습니다.

역할 유형 및 역할 정보 세션 템플릿에서 다음 형식의 기본 수준 "roleTypes" 요소로 지정 됩니다.

```json
"roleTypes": {
  "myroletype1": { // must be lowercase alpha-numeric.
    "ownerManaged": true, // Can only be true on sessions with the "hasOwners" capability set. If true, only the owner of the session can assign this role to members.
    "mutableRoleSettings": ["max", "target"], // Which role settings for roles in this role type can be modified throughout the life of the session. Exclude role settings to lock them.
    "roles": {
      "role1": { // must be lowercase alpha-numeric.
        "max": 3, // Max number of members assigned to this role at a time, enforced by MPSD.
        "target": 2 // Target number of members to assign this role to. Like max, but not enforced (can be exceeded).
      },
      "role2": {
        ...
      }
  },
  "myroletype2": {
    ...
  }
},
```

## <a name="retrieve-role-information-for-a-multiplayer-session"></a>멀티 플레이 세션에 대 한 역할 정보를 검색 합니다.

역할에 대 한 정보를 얻을 수 유형, 역할 및 멀티 플레이 검색 핸들 또는 두 멀티 플레이 세션에서 얼마나 많은 멤버 각 역할에 할당 됩니다.

Xbox Live SDK 역할 유형 및 역할에 대 한 정보는 맵 구조 안에 저장 됩니다. C + + Api를 사용 합니다 `unordered_map` 클래스 및 WinRT Api를 사용 하 여는 `IMapView` 클래스.

### <a name="get-the-role-information-from-a-search-handle"></a>역할 정보를 얻기 위해 검색 핸들

에 `multiplayer_search_handle_details` 검색 요청에서 반환 된 개체 인덱싱하여 역할 유형 정보를 얻을 수는 `role_types` 에 관심이 있는 역할 형식의 이름 사용 하 여 지도.

이 반환 하는 `multiplayer_role_type` 개체. 인덱싱하여 역할을 가져올 수 있습니다 합니다 `roles` 지도 반환 하는 `multiplayer_role_info` 개체입니다.

`multiplayer_role_info` 개체의 역할에 대 한 정보가 포함 하 여 `max_members_count`, `member_xbox_user_ids`, `members_count`, 및 `target_count`.

### <a name="get-the-role-information-from-a-search-handle"></a>역할 정보를 얻기 위해 검색 핸들

세션에서 역할 정보를 가져오는 데 필요한 흐름 검색 핸들에서 정보를 가져오는 데 필요한 흐름에 유사 하지만 일부 다른 클래스를 사용 합니다.

합니다 `multiplayer_session` 개체를 참조 하 여 역할 유형 정보를 얻을 수는 `session_role_types` 개체는 `multiplayer_session_role_types` 클래스. 이 개체에서 인덱싱할 수는 `role_types` 지도에 관심이 있는 역할 형식의 이름의 합니다.

이 반환 하는 `multiplayer_role_type` 개체. 인덱싱하여 역할을 가져올 수 있습니다 합니다 `roles` 지도 반환 하는 `multiplayer_role_info` 개체입니다.

`multiplayer_role_info` 개체의 역할에 대 한 정보가 포함 하 여 `max_members_count`, `member_xbox_user_ids`, `members_count`, 및 `target_count`.

## <a name="change-mutable-role-settings"></a>변경할 수 있는 역할 설정 변경

사용할 수는 일부 역할 설정을 변경할 수 있습니다 (변경 가능)을 나타내는 역할 유형 경우는 `multiplayer_role_type.set_roles()` 변경할 수 있는 설정을 수정 하는 방법. 세션 소유자도 표시 된 구성원만 역할 설정을 변경할 수 있습니다.

## <a name="assign-a-role-to-a-member"></a>구성원에 게 역할 할당

현재 구성원만 Xbox Live SDK에서 자신의 역할을 할당할 수 있습니다. 에 `multiplayer_session` 개체를 호출할 수는 `set_current_user_role_info(role_type, role_name)` 현재 구성원 역할 유형 및 역할을 지정 하는 메서드.

역할 서비스 세션을 작성 하려는 경우에 이미 꽉, MPSD 쓰기를 거부 합니다.
