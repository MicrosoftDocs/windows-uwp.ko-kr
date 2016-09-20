---
author: normesta
description: "소셜 피드를 피플 앱에 통합하는 방법을 보여 줍니다."
MSHAttr: PreferredLib:/library/windows/apps
title: "피플 앱에 소셜 피드 제공"
translationtype: Human Translation
ms.sourcegitcommit: 767acdc847e1897cc17918ce7f49f9807681f4a3
ms.openlocfilehash: c5b9666d8654a4065bc0e4e400d3e47de4773b8b

---

# 피플 앱에 소셜 피드 제공

데이터베이스의 소셜 피드 데이터를 피플 앱에 통합합니다.

피드 데이터는 피플 앱의 **새 소식** 페이지나 연락처의 **프로필** 페이지에 표시됩니다.

사용자는 피드 항목을 탭하여 앱을 열 수 있습니다.

![피플 앱의 소셜 피드](images/social-feeds.png)

시작하려면 소셜 피드를 위한 연락처에 태그를 지정하는 포그라운드 앱과 피드 데이터를 피플 앱에 보내는 백그라운드 에이전트를 만듭니다.

전체 샘플을 보려면 [소셜 정보 샘플](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp)을 참조하세요.

## 포그라운드 앱 만들기

먼저 UWP(유니버설 Windows 플랫폼) 프로젝트를 만들고 **UWP용 Windows 모바일 확장**을 추가합니다.

![모바일 확장](images/mobile-extensions.png)

### 연락처 찾기 또는 만들기

이름, 메일 주소 또는 전화 번호를 사용하여 연락처를 찾을 수 있습니다.

```cs
ContactStore contactStore = await ContactManager.RequestStoreAsync();

IReadOnlyList<Contact> contacts = null;

contacts = await contactStore.FindContactsAsync(emailAddress);

Contact contact = contacts[0];
```
연락처를 만든 다음 연락처 목록에 추가할 수도 있습니다.

```cs
Contact contact = new Contact();
contact.FirstName = "TestContact";

ContactEmail email = new ContactEmail();
email.Address = "TestContact@contoso.com";
email.Kind = ContactEmailKind.Other;
contact.Emails.Add(email);

ContactPhone phone = new ContactPhone();
phone.Number = "4255550101";
phone.Kind = ContactPhoneKind.Mobile;
contact.Phones.Add(phone);

ContactStore store = await
    ContactManager.RequestStoreAsync(ContactStoreAccessType.AppContactsReadWrite);

ContactList contactList;

IReadOnlyList<ContactList> contactLists = await store.FindContactListsAsync();

if (0 == contactLists.Count)
    contactList = await store.CreateContactListAsync("TestContactList");
else
    contactList = contactLists[0];

await contactList.SaveContactAsync(contact);
```

### 주석을 사용하여 각 연락처에 태그 지정

이 *주석* 때문에 피플 앱은 백그라운드 에이전트에서 연락처에 대한 피드 데이터를 요청합니다.

주석의 일부로, 앱이 해당 연락처를 식별하기 위해 내부적으로 사용하는 ID에 연락처 ID를 연결합니다.

```cs
ContactAnnotationStore annotationStore = await
   ContactManager.RequestAnnotationStoreAsync(ContactAnnotationStoreAccessType.AppAnnotationsReadWrite);

ContactAnnotationList annotationList;

IReadOnlyList<ContactAnnotationList> annotationLists = await annotationStore.FindAnnotationListsAsync();
if (0 == annotationLists.Count)
    annotationList = await annotationStore.CreateAnnotationListAsync();
else
    annotationList = annotationLists[0];

ContactAnnotation annotation = new ContactAnnotation();
annotation.ContactId = contact.Id;
annotation.RemoteId = "user22";

annotation.SupportedOperations = ContactAnnotationOperations.SocialFeeds;

await annotationList.TrySaveAnnotationAsync(annotation);

```
### 백그라운드 에이전트 프로비전

앱을 실행할 디바이스에서 [SocialInfoContract](https://msdn.microsoft.com/library/windows/apps/dn706146.aspx) API 계약을 사용할 수 있는지 확인합니다.

사용할 수 있으면 백그라운드 에이전트를 프로비전합니다.

```cs
if (Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
"Windows.ApplicationModel.SocialInfo.SocialInfoContract",
1))
{
    bool isProvisionSuccessful = await Windows.ApplicationModel.SocialInfo.Provider.SocialInfoProviderManager.ProvisionAsync();

    // Throw an exception if the app could not be provisioned
    if (!isProvisionSuccessful)
    {
        throw new Exception("Could not provision the app with the SocialInfoProviderManager");
    }
}
```
## 백그라운드 에이전트 만들기

백그라운드 에이전트는 피플 앱의 피드 요청에 응답하는 Windows 런타임 구성 요소입니다.

에이전트에서 데이터베이스의 피드 데이터를 피플 앱에 제공하여 해당 요청에 응답합니다.

### Windows 런타임 구성 요소 만들기

**Windows 런타임 구성 요소(유니버설 Windows)** 프로젝트를 솔루션에 추가합니다.

![Windows 런타임 구성 요소](images/windows-runtime-component.png)

### 백그라운드 에이전트를 앱 서비스로 등록

매니페스트의 ``Extensions`` 요소에 프로토콜 처리기를 추가하여 등록합니다.

```xml
<Extensions>
  <uap:Extension Category="windows.appService" EntryPoint="SocialFeeds.BackgroundAgent">
    <uap:AppService Name="com.microsoft.windows.social-feeds" />
  </uap:Extension>
</Extensions>
```
Visual Studio 매니페스트 디자이너의 **선언** 탭에서 이러한 처리기를 추가할 수도 있습니다.

![매니페스트 디자이너의 앱 서비스](images/manifest-designer-app-service.png)

### 피플 앱에서 작업 요청

다음에 원하는 데이터 유형을 피플 앱에 묻습니다. 피플 앱에서 데이터를 원하는 피드를 나타내는 코드를 사용하여 요청에 응답합니다.

다음 표에서는 각 피드를 설명합니다.

| 피드 | 설명 |
|-------|-------------|
| 홈 | 피플 앱의 새 소식 페이지에 표시되는 피드입니다. |
| 연락처 | 연락처의 새 소식 페이지에 표시되는 피드입니다. |
| 대시보드 | 연락처 카드에서 프로필 사진 옆에 표시되는 피드입니다. |
<br>
*작업*을 요청하여 피플 앱에 묻습니다. [IBackgroundTask](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.aspx) 인터페이스를 구현하고 [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 메서드를 재정의합니다.

[Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 메서드에서 두 개의 키 값 쌍을 피플 앱에 보냅니다. 한 쌍에는 프로토콜 버전이 포함되고 다른 한 쌍에는 작업 유형이 포함됩니다.

피플 앱의 응답을 수신 대기합니다. 해당 응답에는 코드가 포함됩니다.

```cs
public sealed class BackgroundAgent : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {
        BackgroundTaskDeferral backgroundTaskDeferral = taskInstance.GetDeferral();

        AppServiceTriggerDetails triggerDetails = taskInstance.TriggerDetails as AppServiceTriggerDetails;

        AppServiceConnection appServiceConnection = triggerDetails.AppServiceConnection;

        bool continueProcessing = true;

        while (continueProcessing)
        {
            // Get next operation.
            ValueSet fields =
                new ValueSet();

            fields.Add("Version",
                (1 << 16) | 0);

            fields.Add(
                "Type", 0x10);

            AppServiceResponse response =
                await appServiceConnection.SendMessageAsync(fields);

            if (response == null || response.Status != AppServiceResponseStatus.Success)
            { /* throw exception */ }

            UInt32 type = (UInt32)response.Message["Type"];

            switch (type)
            {
                //Get Next Operation.
                case 0x10:
                    break;
                //Download Home Feed Operation.
                case 0x11:
                    break;
                //Download Contact Feed Operation.
                case 0x13:
                    break;
                //Download Dashboard Feed Operation.
                case 0x15:
                    break;
                //Operation Result.
                case 0x80:
                    break;
                //Good Bye.
                case 0xF1:
                    continueProcessing = false;
                    break;
            }
        }
        // Complete the task deferral
        backgroundTaskDeferral.Complete();

        backgroundTaskDeferral = null;
        appServiceConnection = null;
    }

}
```

[AppServiceResponse.Message](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceresponse.message.aspx) 속성의 ``Type`` 요소를 참조하여 해당 코드를 가져옵니다. 코드의 전체 목록은 다음과 같습니다.

| 형식| 설명 |
|-----|-------------|
| 0x10 | 피플 앱으로 보내는 다음 작업 요청입니다. |
| 0X11 | 기본 사용자에 대한 홈 피드를 제공하라는 피플 앱의 요청입니다. |
| 0x13 | 선택한 연락처에 대한 연락처 피드를 가져오라는 피플 앱의 요청입니다. |
| 0x15 | 선택한 연락처의 대시보드 항목을 가져오라는 피플 앱의 요청입니다. |
| 0x80 | 작업이 완료되었음을 나타냅니다. 이제 데이터를 사용할 수 있음을 피플 앱에 알립니다. |
| 0xF1 | 다른 작업이 필요하지 않음을 나타내는 피플 앱의 메시지입니다. 이제 백그라운드 에이전트를 종료할 수 있습니다. |
<br>
[AppServiceResponse.Message](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceresponse.message.aspx) 속성은 응답을 설명하는 다른 키 값 쌍의 컬렉션도 반환합니다. 해당 목록은 다음과 같습니다.

| 키 | 형식 | 설명 |
|-----|------|-------------|
| Version | UINT32 | (필수) 메시지 프로토콜 버전을 식별합니다. 상위 16비트는 주 버전이고 하위 16비트는 부 버전입니다. |
| Type | UINT32 | (필수) 수행할 작업 유형입니다. 앞의 예제에서는 Type 키를 사용하여 피플 앱에서 요청하는 작업을 확인합니다.
| OperationId | UINT32 | 작업 ID입니다. |
| OwnerRemoteId | 문자열 | 앱에서 해당 연락처를 식별하기 위해 내부적으로 사용하는 ID입니다. |
| LastFeedItemTimeStamp | 문자열 | 검색된 마지막 피드 항목의 ID입니다. |
| LastFeedItemTimeStamp | DateTime | 검색된 마지막 피드 항목의 타임스탬프입니다. |
| ItemCount | UINT32 | 피플 앱에서 요청하는 항목 수입니다. |
| IsFetchMore | BOOLEAN | 내부 캐시를 업데이트할 시기를 결정합니다. |
| ErrorCode | UINT32 | 백그라운드 에이전트 작업과 관련된 오류 코드입니다. |
<br>
### 피플 앱에 데이터 피드 제공

``0x11``, ``0x13`` 또는 ``0x15``의 **Type** 값은 피드 데이터에 대한 피플 앱의 요청입니다.  

다음 몇 개의 조각은 피플 앱에 해당 데이터를 제공하는 방법을 보여 줍니다.

> [!NOTE]
> 이러한 코드 조각은 [소셜 정보 샘플](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp)에서 가져온 것입니다. 샘플의 다른 곳에서 정의된 인터페이스, 클래스 및 멤버에 대한 참조를 포함합니다. 이 항목의 다른 예제와 함께 이러한 조각을 사용하여 작업 흐름을 파악합니다. 인터페이스, 클래스 및 형식 스택을 자세히 살펴보려는 경우 샘플을 참조하세요.

**연락처 피드 항목 가져오기**

```cs
public override async Task DownloadFeedAsync()
{
    // Get the contact feeds
    IEnumerable<FeedItem> contactFeedItems =
        InMemorySocialCache.Instance.GetContactFeeds(OwnerRemoteId, ItemCount);

    // Check if the platform supports the SocialInfo APIs
    if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
         "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
    {

        return;
    }

    // Create the social feed updater.
    SocialFeedUpdater feedUpdater = await SocialInfoProviderManager.CreateSocialFeedUpdaterAsync(
        SocialFeedKind.ContactFeed,
        SocialFeedUpdateMode.Replace,
        OwnerRemoteId);

    // Translate the sample feed into Social info feed items.
    foreach (FeedItem fi in contactFeedItems)
    {
        SocialFeedItem item = new SocialFeedItem();

        item.Timestamp = fi.Timestamp;
        item.RemoteId = fi.Id;
        item.TargetUri = fi.TargetUri;
        item.Author.DisplayName = fi.AuthorDisplayName;
        item.Author.RemoteId = fi.AuthorId;
        item.PrimaryContent.Title = fi.Title;
        item.PrimaryContent.Message = fi.Message;

        if (fi.ImageUri != null)
        {
            item.Thumbnails.Add(new SocialItemThumbnail()
            {
                TargetUri = fi.TargetUri,
                ImageUri = fi.ImageUri
            });
        }

        feedUpdater.Items.Add(item);
    }

    await feedUpdater.CommitAsync();
}
```

**대시보드 항목 가져오기**

```cs
public override async Task DownloadFeedAsync()
{
    // Get the dashboard feed item from your database.
    FeedItem dashboardFeedItem = InMemorySocialCache.Instance.GetDashboardFeed(OwnerRemoteId);

    if (dashboardFeedItem != null)
    {
        // Check if the platform supports the social info APIs.
        if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
             "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
        {

            return;
        }

        SocialDashboardItemUpdater dashboard =
            await SocialInfoProviderManager.CreateDashboardItemUpdaterAsync(OwnerRemoteId);

        dashboard.Content.Message = dashboardFeedItem.Message;
        dashboard.Content.Title = dashboardFeedItem.Title;
        dashboard.Timestamp = dashboardFeedItem.Timestamp;

        // The TargetUri of the dashboard always has to be set.
        dashboard.TargetUri = dashboardFeedItem.TargetUri;

        // For a thumbnail, there must always be a target Uri.
        if ((dashboardFeedItem.ImageUri != null) && (dashboardFeedItem.TargetUri != null))
        {
            dashboard.Thumbnail = new SocialItemThumbnail()
            {
                ImageUri = dashboardFeedItem.ImageUri,
                TargetUri = dashboardFeedItem.TargetUri
            };
        }

        await dashboard.CommitAsync();
    }
}
```

**홈 피드 항목 가져오기**

```cs
public override async Task DownloadFeedAsync()
{
    // Query the "database" for the home feeds.
    IEnumerable<FeedItem> homeFeedItems =
        InMemorySocialCache.Instance.GetHomeFeeds(OwnerRemoteId, ItemCount);

    // Check if the platform supports the social info APIs.
    if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
         "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
    {

        return;
    }

    // Create the social feed updater.
    SocialFeedUpdater feedUpdater = await SocialInfoProviderManager.CreateSocialFeedUpdaterAsync(
        SocialFeedKind.HomeFeed,
        SocialFeedUpdateMode.Replace,
        OwnerRemoteId);

    // Generate each of the feed items.
    foreach (FeedItem fi in homeFeedItems)
    {
        SocialFeedItem item = new SocialFeedItem();

        item.Timestamp = fi.Timestamp;
        item.RemoteId = fi.Id;
        item.TargetUri = fi.TargetUri;
        item.Author.DisplayName = fi.AuthorDisplayName;
        item.Author.RemoteId = fi.AuthorId;
        item.PrimaryContent.Title = fi.Title;
        item.PrimaryContent.Message = fi.Message;

        if (fi.ImageUri != null)
        {
            item.Thumbnails.Add(new SocialItemThumbnail()
            {
                TargetUri = fi.TargetUri,
                ImageUri = fi.ImageUri
            });
        }

        feedUpdater.Items.Add(item);
    }

    await feedUpdater.CommitAsync();
}
```

### 성공 또는 실패 알림을 피플 앱으로 다시 보내기

피드 데이터를 제공한 후 try catch 블록에 호출을 캡슐화한 다음 성공 또는 실패 메시지를 피플 앱에 다시 전달합니다.

```cs
try
{
    // Calls to methods that fetch data and populate feed.
}
catch (Exception exception)
{
    errorCode = (UInt32)exception.HResult;
}

// Send status back to the people app.
ValueSet fields =
    new ValueSet();

fields.Add("ErrorCode", errorCode);

UInt32 operationID = (UInt32)response.Message["OperationId"];

fields.Add("OperationId", operationID);

await this.mAppServiceConnection.SendMessageAsync(fields);

```



<!--HONumber=Aug16_HO4-->


