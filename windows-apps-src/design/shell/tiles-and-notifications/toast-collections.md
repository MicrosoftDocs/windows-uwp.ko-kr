---
title: 알림 컬렉션
description: 작업 센터에서 알림 컬렉션을 생성, 업데이트 또는 제거 하 여 앱에 대 한 알림 메시지를 구성 하는 방법에 대해 알아봅니다.
label: Toast Collections
template: detail.hbs
ms.date: 05/16/2018
ms.topic: article
keywords: windows 10, uwp, 알림, 컬렉션, 컬렉션, 그룹 알림, 그룹화 알림, 그룹, 구성, 동작 센터, 알림
ms.localizationpriority: medium
ms.openlocfilehash: 7ceeec7c84e67074e17d3885167f4be02741c1ff
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984620"
---
# <a name="grouping-toast-notifications-with-collections"></a>컬렉션을 사용 하 여 알림 메시지 그룹화
컬렉션을 사용 하 여 앱의 알림을을 Action Center에서 구성 합니다. 컬렉션을 통해 사용자는 더 쉽게 작업 센터 내에서 정보를 찾고 개발자가 알림을 보다 효율적으로 관리할 수 있습니다.  아래 Api를 사용 하 여 알림 컬렉션을 제거, 생성 및 업데이트할 수 있습니다.

> [!IMPORTANT]
> **작성자 업데이트 필요**: 알림 수집을 사용 하려면 SDK 15063을 대상으로 하 고 빌드 15063 이상을 실행 해야 합니다. 관련 Api에는 Windows [..](/uwp/api/windows.ui.notifications.toastcollectionmanager) s t a t e. [s t a](/uwp/api/windows.ui.notifications.toastcollection)t e.

채팅 그룹에 따라 알림을 분리 하는 메시징 앱을 사용 하 여 아래 예제를 볼 수 있습니다. 각 제목 (Comp 좋아하는 160A 프로젝트 채팅, 직접 메시지, Lacrosse 팀 채팅)은 별도의 컬렉션입니다.  알림이 동일한 앱의 모든 알림 인 경우에도 별도의 앱에서와 같이 개별적으로 그룹화 되는 방법을 확인 합니다.  알림을 구성 하는 더 미묘한 방법을 찾고 있는 경우 알림 [헤더](toast-headers.md)를 참조 하세요.  
![서로 다른 두 개의 알림 그룹을 사용 하는 컬렉션 예제](images/toast-collection-example.png)

## <a name="creating-collections"></a>컬렉션 만들기
각 컬렉션을 만들 때 위의 이미지에 표시 된 것 처럼 작업 센터 내에 컬렉션 제목의 일부로 표시 되는 표시 이름 및 아이콘을 제공 해야 합니다. 컬렉션에는 사용자가 컬렉션의 제목을 클릭할 때 앱 내에서 올바른 위치로 이동 하는 데 도움이 되는 시작 인수도 필요 합니다.  

### <a name="create-a-collection"></a>컬렉션 만들기

``` csharp 
// Create a toast collection
public async void CreateToastCollection()
{
    string displayName = "Work Email"; 
    string launchArg = "NavigateToWorkEmailInbox"; 
    Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/workEmail.png");

    // Constructor
    ToastCollection workEmailToastCollection = new ToastCollection(
        "MyToastCollection", 
        displayName,
        launchArg, 
        icon);

    // Calls the platform to create the collection
    await ToastNotificationManager.GetDefault().GetToastCollectionManager().SaveToastCollectionAsync(workEmailToastCollection);                                 
}
```

## <a name="sending-notifications-to-a-collection"></a>컬렉션에 알림 보내기
로컬, 예약 됨 및 푸시의 세 가지 알림 파이프라인에서 알림을 보내는 작업을 다룹니다.  이러한 각 예제에 대해 바로 아래 코드를 사용 하 여 보낼 샘플 알림을 만들고 각 파이프라인을 통해 컬렉션에 알림을 추가 하는 방법에 중점을 둡니다.

알림 콘텐츠를 생성 합니다.

``` csharp
// Construct the content
var content = new ToastContentBuilder()
    .AddText("Adam sent a message to the group")
    .GetToastContent();
```

### <a name="send-a-toast-to-a-collection"></a>컬렉션에 알림 보내기

```csharp
// Create the toast
ToastNotification toast = new ToastNotification(content.GetXml());

// Get the collection notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync("MyToastCollection");

// And show the toast
notifier.Show(toast);
```

### <a name="add-a-scheduled-toast-to-a-collection"></a>컬렉션에 예약 된 알림 추가

``` csharp
// Create scheduled toast from XML above
ScheduledToastNotification scheduledToast = new ScheduledToastNotification(content.GetXml(), DateTimeOffset.Now.AddSeconds(10));

// Get notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync("MyToastCollection");
    
// Add to schedule
notifier.AddToSchedule(scheduledToast);
```

### <a name="send-a-push-toast-to-a-collection"></a>컬렉션에 푸시 알림 보내기
푸시 알림을의 경우 게시 메시지에 X WNS 헤더를 추가 해야 합니다.
```csharp
// Add header to HTTP request
request.Headers.Add("X-WNS-CollectionId", collectionId); 

```

## <a name="managing-collections"></a>컬렉션 관리
#### <a name="create-the-toast-collection-manager"></a>알림 수집 관리자 만들기
이 ' 컬렉션 관리 ' 섹션에 있는 코드 조각의 나머지 부분에 대해서는 아래 collectionManager를 사용 합니다.
```csharp
ToastCollectionManger collectionManager = ToastNotificationManager.GetDefault().GetToastCollectionManager();
```

#### <a name="get-all-collections"></a>모든 컬렉션 가져오기

``` csharp
IReadOnlyList<ToastCollection> collections = await collectionManager.FindAllToastCollectionsAsync();
``` 

#### <a name="get-the-number-of-collections-created"></a>만든 컬렉션 수 가져오기

``` csharp
int toastCollectionCount = (await collectionManager.FindAllToastCollectionsAsync()).Count;
```

#### <a name="remove-a-collection"></a>컬렉션 제거

``` csharp
await collectionManager.RemoveToastCollectionAsync("MyToastCollection");
```

#### <a name="update-a-collection"></a>컬렉션 업데이트
동일한 ID를 사용 하 여 새 컬렉션을 만들고 컬렉션의 새 인스턴스를 저장 하 여 컬렉션을 업데이트할 수 있습니다.
``` csharp
string displayName = "Updated Display Name"; 
string launchArg = "UpdatedLaunchArgs"; 
Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/updatedPicture.png");

// Construct a new toast collection with the same collection id
ToastCollection updatedToastCollection = new ToastCollection(
    "MyToastCollection", 
    displayName,
    launchArg, 
    icon);

// Calls the platform to update the collection by saving the new instance
await collectionManager.SaveToastCollectionAsync(updatedToastCollection);                               
```
## <a name="managing-toasts-within-a-collection"></a>컬렉션 내에서 알림을 관리
#### <a name="group-and-tag-properties"></a>Group 및 tag 속성
Group 및 tag 속성은 컬렉션 내에서 알림을 고유 하 게 식별 합니다.  그룹 (및 태그)은 알림을 식별 하기 위해 복합 기본 키 (둘 이상의 식별자) 역할을 합니다. 예를 들어 알림을 제거 하거나 교체 하려는 경우에는 제거/교체할 *알림을* 지정할 수 있어야 합니다. 태그 및 그룹을 지정 하 여이 작업을 수행 합니다. 예를 들어 메시징 앱이 있습니다.  개발자는 대화 ID를 그룹으로 사용 하 고 메시지 ID를 태그로 사용할 수 있습니다.

#### <a name="remove-a-toast-from-a-collection"></a>컬렉션에서 알림 제거
태그 및 그룹 Id를 사용 하 여 개별 알림을을 제거 하거나 컬렉션의 모든 알림을을 지울 수 있습니다.
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync("MyToastCollection");

// Remove toast
collectionHistory.Remove(tag, group); 
```

#### <a name="clear-all-toasts-within-a-collection"></a>컬렉션 내의 모든 알림을 지우기
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync("MyToastCollection");

// Remove toast
collectionHistory.Clear();
```


## <a name="collections-in-notifications-visualizer"></a>알림 시각화 도우미의 컬렉션
[알림 시각화 도우미](notifications-visualizer.md) 도구를 사용 하 여 컬렉션을 디자인할 수 있습니다. 아래 단계를 따릅니다.

* 오른쪽 아래 모서리에서 기어 아이콘을 클릭 합니다. 
* ' 알림 모음 '을 선택 합니다.
* 알림 미리 보기 위에는 ' 알림 컬렉션 ' 드롭다운 메뉴가 있습니다. 컬렉션 관리를 선택 합니다.
* ' 컬렉션 추가 '를 클릭 하 고, 컬렉션에 대 한 세부 정보를 입력 하 고, 저장 합니다.
* 컬렉션을 더 추가 하거나 컬렉션 관리 상자를 클릭 하 여 주 화면으로 돌아갈 수 있습니다.
* ' 알림 컬렉션 ' 드롭다운 메뉴에서 알림을 추가 하려는 컬렉션을 선택 합니다.
* 알림을 실행 하면 알림 센터의 적절 한 컬렉션에 추가 됩니다.


## <a name="other-details"></a>기타 세부 정보
사용자가 만든 알림 컬렉션도 사용자의 알림 설정에 반영 됩니다.  사용자는 각 개별 컬렉션에 대 한 설정을 전환 하 여 이러한 하위 그룹을 설정 하거나 해제할 수 있습니다.  앱의 최상위 수준에서 알림이 해제 되 면 모든 컬렉션 알림도 꺼집니다.  또한 각 컬렉션에는 기본적으로 알림 센터에 3 개의 알림이 표시 되 고 사용자는 최대 20 개의 알림을 표시 하도록 확장할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [알림 콘텐츠](adaptive-interactive-toasts.md)
* [알림 헤더](toast-headers.md)
* [GitHub의 알림 라이브러리 (Windows 커뮤니티 도구 키트의 일부)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)