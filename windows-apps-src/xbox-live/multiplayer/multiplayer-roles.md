---
title: 멀티 플레이 역할
description: Xbox Live 멀티 플레이 게임에서 플레이어 역할 정의에 역할을 사용 하는 방법을 알아봅니다.
ms.date: 06/29/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one, 멀티 플레이 역할
ms.localizationpriority: medium
ms.openlocfilehash: ac5e7758bd8e068681d1c8dab2d47d11374c2616
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662088"
---
# <a name="roles"></a>역할

일부 게임 세션에 대 한 특정 멤버가 지원, medic, 공격 등의 특정 게임 플레이 역할을가지고 있음을 지정 하는 것이 좋습니다. Wat 특정 게임 플레이 역할을 채울 플레이어가 게임 슬롯을 예약할 수도 있습니다. Xbox Live 역할 기능을 사용 하 여 서비스 플레이어는 게임 플레이 역할 할당을 추적 하 고 플레이어 게임 플레이 특정 역할을 선택할 수 있는 최대 수를 적용할 수 있습니다.

역할의 가장 일반적인 용도 게임 세션에 대 한 게임 특정 역할을 결정 하는 것입니다. 예를 들어 1-2 지원 클래스, 1 개 이상 탱크/많은 클래스 및 5 개 이하의 공격 클래스 간에 필요한 게임 모드를 할 수 있습니다.

다른 가능한 시나리오에서는 게임 세션을 최대 8 spectators 및 1 announcer 정확히 4 게임 플레이어는 수를 지정 하는 것이 좋습니다.

또한 세션 찾아보기와 같은 다른 수단을 통해 나머지 슬롯을 채우는 동안 친구 들에 대해 슬롯을 예약할 역할을 사용할 수 있습니다.

## <a name="role-types"></a>역할 유형

역할 유형 역할 정의의 그룹을 나타냅니다. 모든 역할은 역할 유형의 일부분으로 정의 되어야 합니다. 역할 유형 멀티 플레이 세션 문서에서 정의 됩니다.

구성원만 할당할 수 있습니다 이상의 역할에서 지정 된 역할 유형. 예를 들어, "class" 역할 유형은 치료, 탱크, 및 손상에 포함 된 경우 다음 구성원만 할당할 수 있습니다 이러한 역할 중 하나.

여러 역할 유형을 정의할 수 있으며 멤버인 역할 유형별로에서 하나의 역할 할당할 수 있습니다. 이전 시나리오에서는 멤버 힐 역할을 선택할 수 있습니다 하지만 계 할당할 수도 있습니다 계 리더 역할을 별도 역할 형식에 정의 된 경우 리더 역할을 합니다.

> **중요:** Xbox Live SDK는 현재 지원 단일 역할 유형 및 각 멤버 마다 단일 역할.

## <a name="role-type-properties"></a>역할 유형 속성

역할 형식을 정의 하는 경우 역할 유형에 대 한 다음 정보를 지정 해야 합니다.

* 역할 유형의 이름입니다. 이름은은 영숫자, 소문자 및 100 개 이하의 자 여야 합니다.
* 역할 유형에 정의 된 역할 소유자 인지 관리 되는 경우입니다.
* 경우에 역할 유형이 정의 된 역할의 속성을 세션의 수명 동안 변경할 수 있습니다.
* 역할 형식에 포함 된 역할 정의 합니다.

역할 유형을 관리 하는 소유자 인 경우 즉, 세션의 소유자 인 멤버만 멤버에 해당 유형의 역할을 할당할 수는 있습니다. 관리 되는 소유자 역할 유형에 없는 경우 다음 멤버 역할을 할당할 수 자체입니다.

역할 유형을 설정 하는 "hasOwners" 기능이 있는 세션에서 관리 하는 소유자는만 지정할 수 있습니다.

> Xbox Live SDK는 다른 구성원에 게 역할을 할당 하는 소유자를 현재 지원 하지 않습니다.

## <a name="role-properties"></a>역할 속성

역할을 정의할 때 각 역할에 대해 다음 정보를 지정 해야 합니다.

* 역할의 이름입니다. 이름은은 영숫자, 소문자 및 100 개 이하의 자 여야 합니다.
* 역할에 허용 되는 멤버의 최대 수입니다. 0 보다 커야 합니다.
* 역할을 채워야 하는 멤버의 대상 수입니다. 대상 0 보다 커야 하 고 역할 수는 최대 멤버 수 보다 작거나 합니다.

세션 멤버인 역할에 할당 하면 멀티 플레이어 세션 문서의 멤버 역할에 해당 정보가 기록 됩니다.

서비스 역할에 할당할 수 있지만 대상 수를 적용 하지 않습니다 하는 멤버의 최대 수를 적용 합니다.

## <a name="create-roles"></a>역할 만들기

역할 및 역할 형식에서 일반적으로 정의 된 [세션 템플릿](service-configuration/session-templates.md)합니다. 하지만 서비스는 Xbox Live SDK 않습니다 역할 및 세션을 만드는 동안 역할 형식 정의 지원 합니다.

### <a name="define-role-types-and-roles-in-a-session-template"></a>세션 템플릿에서 역할 유형 및 역할 정의

Xbox Live 구성 하는 동안 세션 템플릿을 만들 때 역할 유형 및 역할을 정의할 수 있습니다.

역할 유형 및 역할 정보를 다음 형식으로 세션 템플릿에서 기본 수준 "roleTypes" 요소로 지정 됩니다.

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

역할에 대 한 정보를 얻을 수 유형, 역할 및 멀티 플레이 세션 중 하나에서 또는 멀티 플레이어 검색 핸들에서 얼마나 많은 멤버가 각 역할에 할당 됩니다.

Xbox Live SDK 역할 유형 및 역할에 대 한 정보는 맵 구조 내에서 저장 됩니다. C + + Api를 사용 합니다 `unordered_map` 클래스 및 WinRT Api를 사용 하 여는 `IMapView` 클래스입니다.

### <a name="get-the-role-information-from-a-search-handle"></a>검색 핸들에서 역할 정보 가져오기

에 `multiplayer_search_handle_details` 인덱싱하여 역할 형식 정보를 얻을 수는 검색 요청에서 반환 된 개체를 `role_types` 관심이 역할 유형의 이름 사용 하 여 맵을.

반환 된 `multiplayer_role_type` 개체입니다. 인덱싱 하 여 역할을 가져올 수 있습니다 합니다 `roles` 지도 반환 하는 `multiplayer_role_info` 개체입니다.

`multiplayer_role_info` 역할을 하는 방법에 대 한 정보를 포함 하는 개체 등 `max_members_count`, `member_xbox_user_ids`, `members_count`, 및 `target_count`합니다.

### <a name="get-the-role-information-from-a-search-handle"></a>검색 핸들에서 역할 정보 가져오기

세션에서 역할 정보를 가져오기 위한 흐름 검색 핸들을에서 정보를 가져오기 위한 흐름 유사 하지만 일부 다른 클래스를 사용 합니다.

에 `multiplayer_session` 개체를 참조 하 여 역할 형식 정보를 가져올 수 있습니다는 `session_role_types` 인 개체는 `multiplayer_session_role_types` 클래스. 이 개체에서 인덱싱할 수 있습니다는 `role_types` 맵으로 관심이 역할 형식의 이름입니다.

반환 된 `multiplayer_role_type` 개체입니다. 인덱싱 하 여 역할을 가져올 수 있습니다 합니다 `roles` 지도 반환 하는 `multiplayer_role_info` 개체입니다.

`multiplayer_role_info` 역할을 하는 방법에 대 한 정보를 포함 하는 개체 등 `max_members_count`, `member_xbox_user_ids`, `members_count`, 및 `target_count`합니다.

## <a name="change-mutable-role-settings"></a>변경 가능한 역할 설정 변경

역할 유형을 나타냅니다는 몇 가지 역할 설정을 변경할 수 있습니다 (mutable) 하는 경우 사용할 수 있습니다는 `multiplayer_role_type.set_roles()` 변경할 수 있는 설정을 수정 하는 방법입니다. 세션 소유자로 표시 된 멤버만 역할 설정을 변경할 수 있습니다.

## <a name="assign-a-role-to-a-member"></a>멤버에 게 역할 할당

현재 구성원만 Xbox Live SDK에는 자신의 역할을 할당할 수 있습니다. 에 `multiplayer_session` 개체를 호출할 수는 `set_current_user_role_info(role_type, role_name)` 현재 멤버에 대 한 역할 유형 및 역할을 지정 하는 방법.

역할 서비스에 세션을 작성 하려고 할 때에 이미 꽉 차면 MPSD 쓰기를 거부 합니다.
