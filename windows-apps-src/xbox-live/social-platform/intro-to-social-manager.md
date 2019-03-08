---
title: 소셜 Manager 소개
description: 온라인 친구를 추적 하기 위해 Xbox Live 소셜 관리자 API에 알아봅니다.
ms.assetid: d4c6d5aa-e18c-4d59-91f8-63077116eda3
ms.date: 03/26/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5dff3dcfd79fe43ff8af1513a4358bd0ff98b8d1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57609008"
---
# <a name="introduction-to-social-manager"></a>소셜 Manager 소개

## <a name="description"></a>설명

Xbox Live 제목 다양 한 시나리오에 사용할 수 있는 다양 한 소셜 그래프를 제공 합니다.

가져오고 소셜 그래프에 대 한 정보를 유지 관리를 Xbox Live API (XSAPI)에서 소셜 Api를 사용 하 여 복잡 하 고이 정보를 최신 상태로 유지 복잡할 수 있습니다.  이렇게 하지 않으면 올바르게 성능 문제, 오래 된 데이터 또는 Xbox Live 사회 복지를 필요한 것 보다 더 자주 호출로 인해 제한 되 고 발생할 수 있습니다.

소셜 Manager 하 여이 문제를 해결합니다.

* 간단한 API 호출 만들기
* 백그라운드에서 실시간 작업 서비스를 사용 하 여 최신 정보를 만드는 중
* 개발자는 서비스에서 모든 추가 부담 없이 동기적으로 소셜 Manager API를 호출할 수 있도록

주민 등록 관리자를 여러 RTA 구독 및 사용자에 대 한 데이터 새로 고침 처리의 복잡성을 마스크 하 고 개발자가 흥미로운 시나리오를 만들 것인지 최신 그래프를 쉽게 가져올 수 있습니다.

소셜 관리자 메모리 및 성능에 대 한 특성 살펴보겠습니다 [소셜 관리자 메모리 및 성능 개요](social-manager-memory-and-performance-overview.md)

## <a name="features"></a>기능

다음 기능을 제공 하는 소셜 관리자:

* 간소화 된 소셜 API
* 최신 소셜 그래프
* 표시 되는 정보의 자세한 정도 제어
* Xbox Live 서비스에 대 한 호출의 수 줄이기
  * 이 직접 상호 관련 데이터 습득의 전체 대기 시간 감소
* 스레드로부터 안전한 지 여부
* 효율적으로 데이터를 최신 상태로 유지

## <a name="core-concepts"></a>핵심 개념

**소셜 그래프**: A *소셜 그래프* 장치에서 로컬 사용자에 대해 생성 됩니다. 이 모든 사용자 친구 정보를 최신 상태로 유지 하는 구조를 만듭니다.

> [!NOTE]
> Windows에서 로컬 사용자만 수 있습니다.

**Xbox 소셜 사용자**: *Xbox 소셜 사용자* 그룹에서 사용자와 연결 된 공유 데이터의 전체 집합이

**Xbox 소셜 사용자 그룹**: 그룹은 UI 채우기 등을 위해 사용 되는 사용자의 컬렉션. 그룹의 두 종류가 있습니다.

* **그룹 필터링**: 필터 그룹은 로컬 (호출) 사용자 *소셜 그래프* 지속적으로 새로 지정 된 필터 매개 변수를 기반으로 사용자 집합을 반환 하 고
* **사용자 그룹**: 사용자 그룹을 사용자의 목록을 사용 하 고 해당 사용자의 지속적으로 새로운 보기를 반환 합니다. 이러한 사용자는 사용자의 친구 목록 외부 될 수 있습니다.

유지 하기 위해를 *소셜 사용자 그룹* 함수를 날짜 최대 `social_manager::do_work()` 프레임 마다 호출 해야 합니다.

## <a name="api-overview"></a>API 개요

다음 키 클래스를 가장 자주 사용 합니다.

### <a name="social-manager"></a>주민 등록 관리자

* C + + API 클래스 이름: social_manager
* WinRT (C#) API 클래스 이름: [SocialManager](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager.socialmanager?view=xboxlive-dotnet-2017.11.20171204.01)

이 클래스는 가져오는 데 사용할 수 있는 단일 클래스 **Xbox 소셜 사용자 그룹** 는 뷰는 위에 설명 되어 있습니다.

주민 등록 관리자를는 xbox 소셜 사용자 그룹을 최신 상태로 두고 있는지 또는 사용자에 게는 관계에서 사용자 그룹을 필터링 할 수 있습니다.  예를 들어, 모든 온라인 사용자의 친구를 포함 하 고 현재 제목을 재생 xbox 소셜 사용자 그룹을 만들 수 없습니다.  이 최신 상태로 유지 친구 시작 또는 제목 재생을 중지 합니다.

### <a name="xbox-social-user-group"></a>Xbox 소셜 사용자 그룹

* C + + API 클래스 이름: xbox_social_user_group
* WinRT (C#) API 클래스 이름: [XboxSocialUserGroup](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager.xboxsocialusergroup?view=xboxlive-dotnet-2017.11.20171204.01)

위에서 설명한 것 처럼 특정 조건을 충족 하는 사용자의 그룹입니다. Xbox 소셜 사용자 그룹 노출 어떤 유형의 추적 되 고 있는 사용자, 그룹 또는 및 로컬 그룹에 속한 사용자는 필터를 설정 하는 것입니다.

전체 설명은의 소셜 관리자 Api를 찾을 수 있습니다 합니다 [Xbox Live API 참조](https://aka.ms/xboxliveuwpdocs)합니다.
WinRT Api를 찾을 수도 있습니다는 [Microsoft.Xbox.Services.Social.Manager.Namespace 설명서](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager?view=xboxlive-dotnet-2017.11.20171204.01)

## <a name="usage"></a>용도

### <a name="creating-a-social-user-group-from-filters"></a>필터에서 소셜 사용자 그룹 만들기

이 시나리오에서 모든 사람이이 사용자가 팔 또는 즐겨찾기로 태그를 지정 하는 등 필터에서 사용자의 목록을 원하는 합니다.

#### <a name="source-example-using-the-c-api"></a>C + + API를 사용 하 여 원본 예

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

#### <a name="source-example-using-the-c-api"></a>사용 하 여 소스 예제는 C# API

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

`local_user_added`(C + +) | `LocalUserAdded`(C#)-사용자가 소셜 그래프의 로드가 완료 되 면 트리거합니다. 초기화 하는 동안 오류가 발생 하는 경우 표시 됩니다.

`social_user_group_loaded`(C + +) | `SocialUserGroupLoaded`(C#)-소셜 사용자 그룹이 생성 된 경우 트리거합니다.

`users_added_to_social_graph`(C + +) | `UsersAddedToSocialGraph`(C#)-사용자에 로드 된 경우 트리거합니다.

#### <a name="additional-details"></a>추가 정보

위의 예제에는 사용자에 대 한 소셜 네트워크 관리자를 초기화, 해당 사용자에 대 한 소셜 사용자 그룹을 만들고, 최신 상태로 유지 하는 방법을 보여 줍니다.

필터링 옵션에서 볼 수 있습니다 합니다 `presence_filter` 고 `relationship_filter` 열거형

게임 루프에는 `do_work` 함수는 해당 그룹의 사용자의 최신 스냅숏을 사용 하 여 생성된 된 모든 뷰를 업데이트 합니다.

호출 하 여 보기에서 사용자를 가져올 수 있습니다 합니다 `xbox_social_user_group::get_users()` 목록을 반환 하는 함수 `xbox_social_user` 개체입니다.  `xbox_social_user` 게이머 태그, gamerpic uri 등과 같은 소셜 정보를 포함 합니다.

### <a name="create-and-update-a-social-user-group-from-list"></a>만들기 및 소셜 사용자 그룹 목록에서 업데이트

이 시나리오에서 멀티 플레이 세션에서 사용자와 같은 사용자의 목록의 소셜 정보를 원하는합니다.

#### <a name="source-example-using-the-c-api"></a>C + + API를 사용 하 여 원본 예

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

#### <a name="source-example-using-the-c-api"></a>사용 하 여 소스 예제는 C# API

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

`local_user_added`(C + +) | `LocalUserAdded`(C#)-사용자가 소셜 그래프의 로드가 완료 되 면 트리거합니다. 초기화 하는 동안 오류가 발생 하는 경우 표시 됩니다.

`social_user_group_loaded`(C + +) | `SocialUserGroupLoaded`(C#)-소셜 사용자 그룹이 생성 된 경우 트리거합니다.

`users_added_to_social_graph`(C + +) | `UsersAddedToSocialGraph`(C#)-사용자에 로드 된 경우 트리거합니다.

### <a name="updating-social-user-group-from-list"></a>소셜 사용자 그룹 목록에서 업데이트

Update_social_user_group()를 호출 하 여 추적 된 사용자의 소셜 사용자 그룹에서 목록을 변경할 수도 있습니다.

#### <a name="source-example-using-the-c-api"></a>C + + API를 사용 하 여 원본 예

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

#### <a name="source-example-using-the-c-api"></a>사용 하 여 소스 예제는 C# API

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

`social_user_group_updated`(C + +) | `SocialUserGroupUpdated`(C#)-소셜 사용자 그룹 업데이트 완료 되 면 트리거합니다.

`users_added_to_social_graph` | `UsersAddedToSocialGraph`(C#)-사용자에 로드 된 경우 트리거합니다. 이 이벤트는 트리거되지 목록을 통해 추가 사용자가 이미 그래프에 있는 경우

### <a name="using-social-manager-events"></a>소셜 관리자 이벤트를 사용 하 여

소셜 관리자도 알려줍니다 이벤트의 형태로 내용.  UI를 업데이트 하거나 다른 논리를 수행 하려면 해당 이벤트를 사용할 수 있습니다.

#### <a name="source-example-using-the-c-api"></a>C + + API를 사용 하 여 원본 예

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

##### <a name="source-example-using-the-c-api"></a>사용 하 여 소스 예제는 C# API

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

`local_user_added`(C + +) | `LocalUserAdded`(C#)-사용자가 소셜 그래프의 로드가 완료 되 면 트리거합니다. 초기화 하는 동안 오류가 발생 하는 경우 표시 됩니다.

`social_user_group_loaded`(C + +) | `SocialUserGroupLoaded`(C#)-소셜 사용자 그룹이 생성 된 경우 트리거합니다.

`users_added_to_social_graph`(C + +) | `UsersAddedToSocialGraph`(C#)-사용자에 로드 된 경우 트리거합니다.

#### <a name="additional-details"></a>추가 정보

이 예제에서는 제공 되는 추가 컨트롤의 일부를 보여 줍니다.  게임 루프 동안 새로운 사용자 목록을 제공 하는 소셜 사용자 그룹 필터에 의존 하는 대신 소셜 그래프는 게임 루프 외부 초기화 됩니다.  제목에 의존 하는 다음의 `events` 반환한는 `socialManager->do_work()` 함수입니다.  `events` 목록은 `social_event`, 및 각 `social_event` 마지막 프레임 중에 발생 한 소셜 그래프에는 변경 내용을 포함 합니다.  예를 들어 `profiles_changed`, `users_added`등입니다.  자세한 정보를 찾을 수 있습니다는 `social_event` API 설명서입니다.

### <a name="cleanup"></a>정리

#### <a name="cleaning-up-social-user-groups"></a>소셜 사용자 그룹 정리

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

생성 된 소셜 사용자 그룹을 정리 합니다. 호출자에 게 참조 해야 하는 모든 소셜 사용자 그룹을 만들었습니다 이제 오래 된 데이터를 포함 하는 대로 제거 해야 합니다.

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

로드 사용자가 소셜 그래프 뿐만 아니라 해당 사용자를 사용 하 여 만든 모든 소셜 사용자 그룹을 제거 하는 로컬 사용자를 제거 합니다.

#### <a name="events-returned"></a>반환 되는 이벤트

`local_user_removed`(C + +) | `LocalUserRemoved`(C#)-로컬 사용자가 성공적으로 제거 된 경우 트리거합니다.