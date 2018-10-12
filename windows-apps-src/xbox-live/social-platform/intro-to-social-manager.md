---
title: 소셜 관리자를 소개
author: aablackm
description: 온라인 친구를 추적 하기 위해 Xbox Live 소셜 관리자 API에 알아봅니다.
ms.assetid: d4c6d5aa-e18c-4d59-91f8-63077116eda3
ms.author: aablackm
ms.date: 03/26/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e022a2d2cdc9eb73877cafaa05d7b55c7e79c744
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4537081"
---
# <a name="introduction-to-social-manager"></a>소셜 관리자를 소개

## <a name="description"></a>설명

Xbox Live 타이틀은 다양 한 시나리오에 사용할 수 있는 다양 한 소셜 그래프를 제공 합니다.

가져와서 소셜 그래프에 대 한 정보를 유지 관리 Xbox Live API (XSAPI)에 소셜 Api를 사용 하 여은 복잡 하며,이 정보를 최신 상태로 유지 복잡할 수 있습니다.  이렇게 하지 않으면 올바르게 성능 문제, 오래 된 데이터 또는 Xbox Live 소셜 서비스에 필요한 것 보다 더 자주 호출 인해 조정 될 수 있습니다.

소셜 관리자 하 여이 문제를 해결합니다.

* 간단한 API 호출 만들기
* 백그라운드에서 실시간으로 활동 서비스를 사용 하 여 최신 정보 만들기
* 개발자는 서비스에 모든 추가 부담 없이 동기적으로 소셜 관리자 API를 호출할 수 있습니다.

소셜 관리자 여러 RTA 구독 및 사용자에 대 한 데이터를 새로 고쳐지지 처리 하는 복잡성을 마스크 및 개발자가 관심 있는 시나리오를 만들려는 최신 그래프에 쉽게 액세스할 수 있습니다.

소셜 관리자 메모리 및 성능에 대 한 특성 살펴보세요 [소셜 관리자 메모리 및 성능 개요](social-manager-memory-and-performance-overview.md)

## <a name="features"></a>기능

소셜 관리자는 다음 기능을 제공 합니다.

* 간소화 된 소셜 API
* 최신 소셜 그래프
* 자세한 정보 표시 되는 정보의 제어
* Xbox Live 서비스에 대 한 호출 횟수를 줄일
  * 이 데이터 구입에 전체 대기 시간 감소에 직접 연결
* 안전 하 게 보호
* 효율적으로 데이터를 최신 상태로 유지

## <a name="core-concepts"></a>핵심 개념

**소셜 그래프**: *소셜 그래프* 장치에 로컬 사용자에 대해 만들어집니다. 모든 사용자에 게 친구에 대 한 정보를 최신 상태로 유지 하는 구조를 만듭니다.

> [!NOTE]
> Windows에서 한 로컬 사용자만 될 수 있습니다.

**Xbox 소셜 사용자**: *소셜 Xbox 사용자* 는 그룹에서 사용자와 관련 된 공유 데이터의 전체 집합

**Xbox 소셜 사용자 그룹**: 그룹은 UI를 채우는 등의 작업에 사용 되는 사용자의 컬렉션입니다. 두 가지 방법으로 그룹

* **필터 그룹**: 필터 그룹은 로컬 (호출) 사용자의 *소셜 그래프* 및 사용자 지정된 필터 매개 변수에 따라 지속적으로 새 집합을 반환
* **사용자 그룹**: 사용자 그룹을 사용자 목록 하 고 사용자에 대 한 일관 되 게 새 보기를 반환 합니다. 이러한 사용자는 사용자의 친구 들이 목록 외부 될 수 있습니다.

함수 날짜 까지의 *소셜 사용자 그룹* 을 유지 하기 위해 `social_manager::do_work()` 매 프레임 마다 호출 해야 합니다.

## <a name="api-overview"></a>API 개요

가장 자주 다음 주요 클래스를 사용 합니다.

### <a name="social-manager"></a>소셜 관리자

* C + + API 클래스 이름: social_manager
* WinRT(C#) API 클래스 이름: [SocialManager](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager.socialmanager?view=xboxlive-dotnet-2017.11.20171204.01)

이것은 **Xbox 소셜 사용자 그룹** 보기를 가져오는 데 사용할 수 있는 단일 클래스 위에서 설명한 합니다.

소셜 관리자는 항상 최신 xbox 소셜 사용자 그룹, 및 사용자 그룹 또는 사용자에 게 관계를 필터링 할 수 있습니다.  예를 들어 사용자의 온라인 상태인 친구의 모두 포함 하 고 현재 타이틀에 xbox 소셜 사용자 그룹을 만들 수 있습니다.  이 유지 최신 친구 시작 또는 제목 재생이 중지 합니다.

### <a name="xbox-social-user-group"></a>Xbox 소셜 사용자 그룹

* C + + API 클래스 이름: xbox_social_user_group
* WinRT(C#) API 클래스 이름: [XboxSocialUserGroup](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager.xboxsocialusergroup?view=xboxlive-dotnet-2017.11.20171204.01)

위에서 설명한 대로 특정 조건에 해당 하는 사용자의 그룹입니다. Xbox 소셜 사용자 그룹에 어떤 유형의 있는 사용자를 추적 하는 그룹, 노출 또는 및 로컬 사용자 그룹에 속한 켜져 필터를 설정 합니다.

[Xbox Live API 참조](https://aka.ms/xboxliveuwpdocs)의 전체 설명은 소셜 관리자 Api를 찾을 수 있습니다.
WinRT Api [Microsoft.Xbox.Services.Social.Manager.Namespace 설명서](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager?view=xboxlive-dotnet-2017.11.20171204.01) 에서 찾을 수도 있습니다.

## <a name="usage"></a>사용

### <a name="creating-a-social-user-group-from-filters"></a>필터에서 소셜 사용자 그룹 만들기

이 시나리오에서는이 사용자가 다음 또는 즐겨찾기로 태그가 지정 된 모든 사람이 같은 필터에서 사용자 목록을 할 수 있습니다.

#### <a name="source-example-using-the-c-api"></a>C + + API를 사용 하 여 소스 예제

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();

socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::preferred_color_level | social_manager_extra_detail_level::title_history_level
    );

auto socialUserGroup = socialManager->create_social_user_group_from_filters(
    xboxLiveContext->user(),
    presence_filter::all,
    relationship_filter::friends
    );

while(true)
{
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
}
```

#### <a name="source-example-using-the-c-api"></a>C# API를 사용 하 여 소스 예제

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromFilters(
     xboxLiveContext.User,
     PresenceFilter.All,
     RelationshipFilter.Friends
     );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}

```

#### <a name="events-returned"></a>반환 되는 이벤트

`local_user_added`(C + +) | `LocalUserAdded`(C#)-사용자가 소셜 그래프의 로드가 완료 되었을 때 트리거합니다. 초기화 중에 오류가 발생 하는 경우 알려줍니다.

`social_user_group_loaded`(C + +) | `SocialUserGroupLoaded`(C#)-트리거합니다 소셜 사용자 그룹을 작성 하는 경우

`users_added_to_social_graph`(C + +) | `UsersAddedToSocialGraph`(C#)-사용자가 로드 되 면 트리거

#### <a name="additional-details"></a>추가적인 세부 정보

위의 예제에는 사용자에 대 한 소셜 관리자 초기화, 해당 사용자에 대 한 소셜 사용자 그룹을 만들고, 최신 상태로 유지 하는 방법을 보여 줍니다.

필터링 옵션에서 볼 수는 `presence_filter` 및 `relationship_filter` 열거형

게임 루프에는 `do_work` 함수 해당 그룹의 사용자의 최신 스냅숏을 사용 하 여 만든된 모든 보기를 업데이트 합니다.

보기에서 사용자를 호출 하 여 얻을 수는 `xbox_social_user_group::get_users()` 의 목록을 반환 하는 함수 `xbox_social_user` 개체입니다.  `xbox_social_user` 게이머 태그, gamerpic uri 등 소셜 정보가 포함 되어 있습니다.

### <a name="create-and-update-a-social-user-group-from-list"></a>만들고 목록에서 소셜 사용자 그룹을 업데이트 합니다.

이 시나리오에서는 멀티 플레이 세션의 사용자와 같은 사용자 목록이의 소셜 정보를 할 수 있습니다.

#### <a name="source-example-using-the-c-api"></a>C + + API를 사용 하 여 소스 예제

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();

socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::preferred_color_level | social_manager_extra_detail_level::title_history_level
    );

auto socialUserGroup = socialManager->create_social_user_group_from_list(
    xboxLiveContext->user(),
    userList  // this is a std::vector<string_t> (list of xuids)
    );

while(true)
{
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
}
```

#### <a name="source-example-using-the-c-api"></a>C# API를 사용 하 여 소스 예제

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromList(
     xboxLiveContext.User,
     userList //this is a IReadOnlyList<string> (list of xbox user ids a.k.a. xuids)
    );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}
```

#### <a name="events-returned"></a>반환 되는 이벤트

`local_user_added`(C + +) | `LocalUserAdded`(C#)-사용자가 소셜 그래프의 로드가 완료 되었을 때 트리거합니다. 초기화 중에 오류가 발생 하는 경우 알려줍니다.

`social_user_group_loaded`(C + +) | `SocialUserGroupLoaded`(C#)-소셜 사용자 그룹이 만들어지면 트리거

`users_added_to_social_graph`(C + +) | `UsersAddedToSocialGraph`(C#)-사용자가 로드 되 면 트리거합니다

### <a name="updating-social-user-group-from-list"></a>목록에서 사회 사용자 그룹을 업데이트

소셜 사용자 그룹에 사용자를 추적된 목록 update_social_user_group()를 호출 하 여 변경할 수도 있습니다.

#### <a name="source-example-using-the-c-api"></a>C + + API를 사용 하 여 소스 예제

```cpp
//#include "Social.h"

socialManager->update_social_user_group(
    xboxSocialUserGroup,
    newUserList    // std::vector<string_t> (list of xuids)
    );

    while(true)
    {
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
    }
```

#### <a name="source-example-using-the-c-api"></a>C# API를 사용 하 여 소스 예제

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.UpdateSocialUserGroup(
     xboxSocialUserGroup,
     newUserList //IReadOnlyList<string> (list of xbox user ids a.k.a. xuids)
     );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}
```

#### <a name="events-returned"></a>반환 되는 이벤트

`social_user_group_updated`(C + +) | `SocialUserGroupUpdated`(C#)-소셜 사용자 그룹 업데이트 완료 되 면을 트리거합니다.

`users_added_to_social_graph` | `UsersAddedToSocialGraph`(C#)-사용자가 로드 되 면 트리거합니다. 이 이벤트 트리거되지 않습니다 그래프에서 목록을 통해 추가 된 사용자가 이미 인 경우

### <a name="using-social-manager-events"></a>소셜 관리자 이벤트를 사용 하 여

소셜 관리자는 또한 알려 이벤트의 형태로 실행 된 작업입니다.  이러한 이벤트를 사용 하 여 UI를 업데이트 하거나 다른 논리를 수행 수 있습니다.

#### <a name="source-example-using-the-c-api"></a>C + + API를 사용 하 여 소스 예제

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();
socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::no_extra_detail
    );

auto socialUserGroup = socialManager->create_social_user_group_from_filters(
    xboxLiveContext->user(),
    presence_filter::all,
    relationship_filter::friends
    );

socialManager->do_work();
// TODO: initialize the game UI containing the friends list using game UI, socialUserGroup->users()

while(true)
{
    // some update loop in the game
    auto events = socialManager->do_work();
    for(auto evt : events)
    {
        auto affectedUsersFromGraph = socialUserGroup->get_users_from_xbox_user_ids(evt.users_affected());
        // TODO: render the changes to the friends list using game UI, passing in affectedUsersFromGraph
    }
}
```

##### <a name="source-example-using-the-c-api"></a>C# API를 사용 하 여 소스 예제

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromFilters(
     xboxLiveContext.User,
     PresenceFilter.All,
     RelationshipFilter.Friends
     );

socialManager.DoWork();
// TODO: initialize the game UI containing the friends list using game UI, socialUserGroup->users()

while(true)
{
    // some update loop in the game
    IReadOnlyList<SocialEvent> Events = socialManager.DoWork();
    IReadOnlyList<XboxSocialUser> affectedUsersFromGraph;
    foreach(SocialEvent managerEvent in Events)
    {
        affectedUsersFromGraph = socialUserGroup.GetUsersFromXboxUserIds(managerEvent.UsersAffected);
    }
}

```

#### <a name="events-returned"></a>반환 되는 이벤트

`local_user_added`(C + +) | `LocalUserAdded`(C#)-사용자가 소셜 그래프의 로드가 완료 되었을 때 트리거합니다. 초기화 중에 오류가 발생 하는 경우 알려줍니다.

`social_user_group_loaded`(C + +) | `SocialUserGroupLoaded`(C#)-소셜 사용자 그룹이 만들어지면 트리거

`users_added_to_social_graph`(C + +) | `UsersAddedToSocialGraph`(C#)-사용자가 로드 되 면 트리거합니다

#### <a name="additional-details"></a>추가적인 세부 정보

이 예제에서는 제공 추가 컨트롤의 일부를 보여 줍니다.  게임 루프 중 새 사용자 목록을 제공 하는 소셜 사용자 그룹 필터에 의존 하지 않고 소셜 그래프는 게임 루프 외부 초기화 됩니다.  제목에 의존 하는 다음의 `events` 에서 반환 되는 `socialManager->do_work()` 함수.  `events` 목록은 `social_event`, 및 각 `social_event` 변경 마지막 프레임 중에 발생 한 소셜 그래프에 포함 되어 있습니다.  예를 들어 `profiles_changed`, `users_added`등.  더 많은 정보를 찾을 수는 `social_event` API 설명서.

### <a name="cleanup"></a>정리

#### <a name="cleaning-up-social-user-groups"></a>소셜 사용자 그룹을 정리

```cpp
//#include "Social.h"

socialManger->destroy_social_user_group(
    socialUserGroup
    );
```

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.DestroySocialUserGroup(
     socialUserGroup
     );
```

생성 된 소셜 사용자 그룹을 정리 합니다. 호출자는 이제 오래 된 데이터가 포함 된 대로 모든 만들어진된 소셜 사용자 그룹을가지고 대 한 참조도 제거 해야 합니다.

#### <a name="cleaning-up-local-users"></a>로컬 사용자를 정리

```cpp
//#include "Social.h"

socialManger->remove_local_user(
    xboxLiveContext->user()
    );
```

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.RemoveLocalUser(
     xboxLiveContext.User
     );
```

로컬 사용자 제거도 로드 된 사용자가 소셜 그래프 해당 사용자를 사용 하 여 생성 된 소셜 사용자 그룹을 제거 합니다.

#### <a name="events-returned"></a>반환 되는 이벤트

`local_user_removed`(C + +) | `LocalUserRemoved`(C#)-로컬 사용자 제거 되었을 때 트리거