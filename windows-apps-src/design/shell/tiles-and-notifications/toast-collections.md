---
Description: 컬렉션을 사용 하 여 관리 센터에서 알림을 그룹화 하는 방법에 알아봅니다.
title: 알림 컬렉션
label: Toast Collections
template: detail.hbs
ms.date: 05/16/2018
ms.topic: article
keywords: Windows 10, uwp, 알림, 컬렉션, 그룹 알림, 알림 그룹화, 그룹, 구성, 알림 센터
ms.localizationpriority: medium
ms.openlocfilehash: 9b6818f876c094298a0a6636faa00efa9a192545
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600688"
---
# <a name="grouping-toast-notifications-with-collections"></a>컬렉션으로 알림 메시지 그룹화
컬렉션을 사용하여 알림 센터에서 앱의 알림을 구성할 수 있습니다. 컬렉션은 사용자가 알림 센터에서 정보를 더 쉽게 찾고 개발자가 알림을 관리하는 데 도움이 됩니다.  아래 API를 사용하여 알림 컬렉션을 만들고, 제거하고 업데이트할 수 있습니다.

> [!IMPORTANT]
> **크리에이터 스 업데이트를 설치 해야**: SDK 15063 대상으로 해야 합니다을 실행 하 고 빌드 15063 알림 컬렉션을 사용 하려면. 관련 API는 [Windows.UI.Notifications.ToastCollection](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection) 및 [Windows.UI.Notifications.ToastCollectionManager](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollectionmanager) 등이 있습니다.

아래의 예제에서 채팅 그룹에 따라 알림을 구분하는 메시지 앱을 확인할 수 있습니다. 각 제목(Comp Sci 160A Project Chat, Direct Messages, Lacrosse Team Chat)은 별도 컬렉션입니다.  모든 알림이 동일한 앱의 알림이더라도 개별 앱의 알림인 것처럼 명확하게 알림을 그룹화하는 방법을 알아보세요.  알림을 구성하는 보다 섬세한 방법을 찾고자 하는 경우 [알림 헤더](toast-headers.md)를 참조하세요.  
![두 개의 다른 그룹의 알림을 사용 하 여 컬렉션 예제](images/toast-collection-example.png)

## <a name="creating-collections"></a>컬렉션 만들기
위 이미지에서와 같이 각 컬렉션을 만들 때 컬렉션 제목의 일부로 알림 센터 내에 표시되는 표시 이름 및 아이콘을 제공해야 합니다. 컬렉션에는 또한 사용자가 컬렉션의 제목을 클릭할 때 앱 내에서 앱이 오른쪽 위치로 이동하기 위해 실행 인수가 필요합니다.  

### <a name="create-a-collection"></a>컬렉션 만들기

``` csharp 
public const string toastCollectionId = "ToastCollection";

// Create a toast collection
public async void CreateToastCollection()
{
    string displayName = "Work Email"; 
    string launchArg = "NavigateToWorkEmailInbox"; 
    Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/workEmail.png");

    // Constructor
    ToastCollection workEmailToastCollection = new ToastCollection(MainPage.toastCollectionId, 
        displayName,
        launchArg, 
        icon);

    // Calls the platform to create the collection
    await ToastNotificationManager.GetDefault().GetToastCollectionManager().SaveToastCollectionAsync(workEmailToastCollection);                                 
}
```

## <a name="sending-notifications-to-a-collection"></a>컬렉션에 알림 보내기
로컬, 예약 및 푸시의 세 가지 다른 알림 파이프라인에서 알림 보내기를 다룹니다.  각 이러한 예에서 바로 아래에 있는 코드와 함께 보낼 샘플 알림을 만든 다음 각 파이프라인을 통해 컬렉션에 알림을 추가하는 방법에 대해 중점적으로 다루겠습니다.

알림 페이로드 구성

``` csharp
public const string toastCollectionId = "MyToastCollection";

public async void SendToastToToastCollection()
{
    // Construct the notification Content
    string toastXmlString = 
        $@"<toast launch=’’>
            <visual>
                <binding template=’ToastGeneric’>
                    <text>Hello,</text>
                    <text>it’s me</text>
                </binding>
            </visual>
        </toast>";
    // Convert to XML
    XmlDocument toastXml = new XmlDocment();
    toastXml.LoadXml(toastXmlString);
    ToastNotification toast = new ToastNotification(toastXml);
```

### <a name="send-a-toast-to-a-collection"></a>컬렉션에 알림 보내기

```csharp
// Get the collection notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync(MainPage.toastCollectionId);

// And show the toast
notifier.Show(toast);
```

### <a name="add-a-scheduled-toast-to-a-collection"></a>컬렉션에 예정된 알림 추가

``` csharp
// Create scheduled toast from XML above
ScheduledToastNotification scheduledToast = new ScheduledToastNotification(toastXml, DateTimeOffset.Now.AddSeconds(10));

// Get notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync(MainPage.toastCollectionId);
    
// Add to schedule
notifier.AddToSchedule(scheduledToast);
```

### <a name="send-a-push-toast-to-a-collection"></a>컬렉션에 푸시 알림 보내기
푸시 알림의 경우 POST 메시지에 X-WNS-CollectionId 헤더를 추가해야 합니다.
```csharp
// Add header to HTTP request
request.Headers.Add("X-WNS-CollectionId", collectionId); 

```

## <a name="managing-collections"></a>컬렉션 관리
#### <a name="create-the-toast-collection-manager"></a>알림 컬렉션 관리자 만들기
이 '컬렉션 관리' 섹션의 나머지 코드 조각에 대해 아래의 collectionManager를 사용합니다.
```csharp
ToastCollectionManger collectionManager = ToastNotificationManager.GetDefault().GetToastCollectionManager();
```

#### <a name="get-all-collections"></a>모든 컬렉션 가져오기

``` csharp
IReadOnlyList<ToastCollection> collections = await collectionManager.FindAllToastCollectionsAsync();
``` 

#### <a name="get-the-number-of-collections-created"></a>만든 컬렉션의 수 가져오기

``` csharp
int toastCollectionCount = (await collectionManager.FindAllToastCollectionsAsync()).Count;
```

#### <a name="remove-a-collection"></a>컬렉션 제거

``` csharp
await collectionManager.RemoveToastCollectionAsync(MainPage.toastCollectionId);
```

#### <a name="update-a-collection"></a>컬렉션 업데이트
동일한 ID를 사용하여 새 컬렉션을 만들고 새 컬렉션 인스턴스를 저장하여 컬렉션을 업데이트할 수 있습니다.
``` csharp
string displayName = "Updated Display Name"; 
string launchArg = "UpdatedLaunchArgs"; 
Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/updatedPicture.png");

// Construct a new toast collection with the same collection id
ToastCollection updatedToastCollection = new ToastCollection(MainPage.toastCollectionId, 
            displayName,
            launchArg, 
            icon);

// Calls the platform to update the collection by saving the new instance
await collectionManager.SaveToastCollectionAsync(updatedToastCollection);                               
```
## <a name="managing-toasts-within-a-collection"></a>컬렉션에서 알림 관리
#### <a name="group-and-tag-properties"></a>그룹 및 태그 속성
그룹 및 태그 속성은 함께 고유하게 컬렉션 내에서 알림을 식별합니다.  Group(및 Tag)은 알림을 식별하기 위해 복합 기본 키(2개 이상의 식별자) 역할을 합니다. 예를 들어 알림을 제거하거나 교체하려는 경우 제거/교체할 알림이 *어떤 알림*인지 지정해야 합니다. 이는 Tag 및 Group을 지정하여 수행합니다. 예를 들어 메시지 앱입니다.  개발자는 대화 ID를 Group으로, 메시지 ID를 Tag로 사용할 수 있습니다.

#### <a name="remove-a-toast-from-a-collection"></a>컬렉션에서 알림 제거
태그 또는 그룹 ID를 사용하여 개별 알림을 제거하거나 컬렉션에 있는 모든 알림을 지울 수 있습니다.
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Remove(tag, group); 
```

#### <a name="clear-all-toasts-within-a-collection"></a>컬렉션 내 모든 알림 지우기
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Clear();
```


## <a name="collections-in-notifications-visualizer"></a>알림 비주얼라이저의 컬렉션
[알림 비주얼라이저](notifications-visualizer.md) 도구를 사용하여 컬렉션을 디자인할 수 있습니다. 아래의 단계를 수행합니다.

* 오른쪽 하단 모서리에서 기어 아이콘을 클릭합니다. 
* '알림 컬렉션'을 선택합니다.
* 알림 미리 보기 위에 '알림 컬렉션' 드롭다운 메뉴가 있습니다. 컬렉션 관리를 선택합니다.
* '컬렉션 추가'를 클릭하고 컬렉션에 대한 세부 정보를 작성한 다음 저장합니다.
* 컬렉션을 더 추가하거나 컬렉션 관리 상자를 꺼 기본 화면으로 돌아갑니다.
* '알림 컬렉션' 드롭다운 메뉴에서 알림을 추가하려는 컬렉션을 선택합니다.
* 알림을 발생시키면 알림 센터에서 적절한 컬렉션에 추가됩니다.


## <a name="other-details"></a>기타 세부 정보
사용자가 만든 알림 컬렉션은 사용자의 알림 설정에도 반영됩니다.  사용자는 이러한 각 개별 컬렉션에 대한 설정을 토글하여 이러한 하위 그룹을 켜거나 끌 수 있습니다.  알림이 앱의 최상위 수준에서 꺼진 경우 모든 컬렉션 알림 도한 꺼집니다.  그리고 각 컬렉션은 기본적으로 알림 센터에서 3개의 알림을 표시하며 사용자는 최대 20개의 알림을 표시하도록 확장할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [알림 콘텐츠](adaptive-interactive-toasts.md)
* [알림 메시지 헤더](toast-headers.md)
* [GitHub (Windows 커뮤니티 도구 키트의 일부)에 대 한 알림 라이브러리](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)