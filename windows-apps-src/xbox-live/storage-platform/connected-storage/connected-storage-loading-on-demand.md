---
title: 주문형 저장소 로드를 연결합니다.
description: 대신 한 번에 필요에 따라 연결 된 저장소 데이터를 로드 하는 방법을 알아봅니다.
ms.assetid: a0797a14-c972-4017-864c-c6ba0d5a3363
ms.date: 02/27/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 연결 된 저장소, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 31c1893f09e6d56682b4e718ee8b905ce72c7ad8
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8457511"
---
# <a name="connected-storage-loading-on-demand"></a>주문형 저장소 로드를 연결합니다.

`GetSyncOnDemandForUserAsync` 대신 한 번에 "주문형" 연결 된 저장소 공간에서 클라우드 기반 데이터를 로드할 수 있습니다. 위에 성능을 향상할 수 있습니다 `GetForUserAsync` 파일을 저장 하는 경우 특히 큰 됩니다.

## <a name="to-load-data-from-a-connected-storage-space-on-demand"></a>필요에 따라 연결 된 저장소 공간에서 데이터를 로드

### <a name="1--call-getsyncondemandforuserasync"></a>1: 호출`GetSyncOnDemandForUserAsync`

클라우드에 하지만 해당 콘텐츠 하지에서 컨테이너 목록과 해당 메타 데이터를 다운로드 하는 부분 동기화를 트리거합니다. 이 작업을 신속 하 고 사용자 좋은 네트워크 조건에서 모든 로드 UI를 표시 되지 않습니다.

```cpp
auto op = ConnectedStorageSpace::GetSyncOnDemandForUserAsync(user);
op->Completed = ref new AsyncOperationCompletedHandler<ConnectedStorageSpace^>(
    [=](IAsyncOperation<ConnectedStorageSpace^>^ operation, Windows::Foundation::AsyncStatus status)
{
    switch (status)
    {
    case Windows::Foundation::AsyncStatus::Completed:
        auto storageSpace = operation->GetResults();
        break;
    case Windows::Foundation::AsyncStatus::Error:
    case Windows::Foundation::AsyncStatus::Canceled:
        // Present user option: ?Would you like to continue without saving progress??
        if (GetUserInputYesOrNo())
            SetGameState(LoadSaveState::NO_SAVE_MODE);
        else
            SetGameState(LoadSaveState::RETRY_LOAD);
        break;
    }
});
```

```csharp
var users = await Windows.System.User.FindAllAsync();

GameSaveProvider gameSaveProvider;

GameSaveProviderGetResult gameSaveTask = await GameSaveProvider.GetSyncOnDemandForUserAsync(users[0], context.AppConfig.ServiceConfigurationId); 
//Paramaters
//Windows.System.User user
//string SCID

if(gameSaveTask.Status == GameSaveErrorStatus.Ok)
{
    gameSaveProvider = gameSaveTask.Value;
}
```


### <a name="2--perform-a-container-query-using-getcontainerinfo2async"></a>2: 사용 하 여 컨테이너 쿼리를 수행 합니다.`GetContainerInfo2Async`

컬렉션을 반환 합니다 `ContainerInfo2`,이 포함 된 새 메타 데이터 필드 3:

    -   `DisplayName`: 모든 표시의 오버 로드를 사용 하 여 작성 된 이름 `SubmitUpdatesAsync` 표시 이름이 문자열을 매개 변수로 사용 합니다. 항상이 필드를 사용 하 여 입력을 저장 하는 것이 좋습니다.
    -   `LastModifiedTime`:이 컨테이너 마지막으로 수정 되었습니다. Note 충돌 하는 로컬 및 원격 타임 스탬프의 경우 원격 스타일 사용 됩니다.
    -   `NeedsSync`: 나타내는 부울 값이 컨테이너에 데이터를 다운로드 해야 클라우드에서 합니다.

    이 추가 메타 데이터를 사용 하 여 표시할 수 있습니다 사용자가 게임 저장에 대 한 핵심 정보 (이름을 포함 하 여 마지막 시간에 사용 되는 않으며 여부 하나를 선택 하면 필요 다운로드) 실제로 클라우드에서 전체 다운로드 였습니까를 수행 하지 않고 합니다.

```cpp
auto containerQuery = storageSpace->CreateContainerInfoQuery(nullptr); //return list of containers in ConnectedStorageSpace
auto queryOperation = containerQuery->GetContainerInfo2Async();

queryOperation->Completed = ref new AsyncOperationCompletedHandler<IVectorView<ContainerInfo2>^ >( 
    [=] (IAsyncOperation<IVectorView<ContainerInfo2>^ >^ operation, Windows::Foundation::AsyncStatus status)
    {
        switch (status)
        {
        case Windows::Foundation::AsyncStatus::Completed:
            // get the resulting vector of container info
            auto infoVector = operation->GetResults();
            break;
        case Windows::Foundation::AsyncStatus::Error:
        case Windows::Foundation::AsyncStatus::Canceled:
            // handle error cases
            break;
        }
    });
```

```csharp
GameSaveContainerInfoQuery infoQuery = gameSaveProvider.createContainerInfoQuery();
GameSaveContainerInfoGetResult containerInfoResult = await infoQuery.GetContainerInfoAsync();
var containerInfoList;

if(containerInfoResult.Status == GameSaveErrorStatus.Ok)
{
    containerInfoList = containerInfoResult.value;
}

// Use the containerInfoList to inform further actions or display container data to user. 
```

### <a name="3--trigger-a-sync"></a>3: 동기화를 트리거하기

연결 된 저장소 synce 기존 연결 된 저장소 API 중 하나를 호출 하 여 트리거됩니다.

**C++**

    -   BlobInfoQueryResult::GetBlobInfoAsync
    -   BlobInfoQueryResult::GetItemCountAsync
    -   ConnectedStorageContainer::GetAsync
    -   ConnectedStorageContainer::ReadAsync
    -   ConnectedStorageSpace::DeleteContainerAsync

**C#**

    -   GameSaveBlobInfoQuery.GetBlobInfoAsync
    -   GameSaveBlobInfoQuery.GetItemCountAsync
    -   GameSaveContainer.GetAsync
    -   GameSaveContainer.ReadAsync
    -   GameSaveProvider.DeleteContainerAsync

이렇게 하면 사용자에 게 해당 선택한 컨테이너의 데이터를 다운로드 한 상태로 일반적인 동기화 UI와 진행률 표시줄을 표시 합니다. 선택된 된 컨테이너에서 데이터가; 동기화 note 다른 컨테이너에서 데이터 다운로드 되지 않습니다.

에 수동 동기화의 컨텍스트에서 이러한 API를 호출할 때 이러한 작업 수 결과가 다음과 같은 새 오류 코드:

-   `ConnectedStorageErrorStatus::ContainerSyncFailed`(`GameSaveErrorStatus.ContainerSyncFailed` UWP C# api에서):이 오류는 작업이 실패 하 고 컨테이너 클라우드와 동기화 할 수 없는 나타냅니다. 가장 일반적인 원인 실패 동기화 발생 하는 사용자의 네트워크 상태입니다. 네트워크를 고정 한 또는 없는 컨테이너를 사용 하도록 선택할 수 있습니다 후 다시 시도 동기화 합니다. UI는 이러한 옵션 중 하나를 허용 해야 합니다. 다시 시도 대화 상자가 없습니다. 필요한 이므로 것은 앞서 살펴본 시스템 UI 재시도 대화 상자입니다.

-   `ConnectedStorageErrorStatus::ContainerNotInSync`(`GameSaveErrorStatus.ContainerNotInSync` UWP C# api에서):이 오류는 타이틀을 잘못 시도 동기화 컨테이너에 쓰기를 나타냅니다. 호출 `ConnectedStorageContainer::SubmitUpdatesAsync`(`GameSaveContainer.SubmitUpdatesAsync` UWP C# api에서) NeedsSync 플래그를 사용 하는 컨테이너에 대해 true로 설정은 허용 되지 않습니다. 먼저에 쓰기 전에 동기화를 트리거하는 컨테이너를 읽어야 합니다. 여기에서 읽기 없이 컨테이너를 작성 하는 경우 가능성이 타이틀에 버그가 사용자 진행률을 손실 될 수 있습니다.

이 동작은 사용자가 오프 라인을 재생할 때 다릅니다. 동안 오프 라인으로 있으면 컨테이너 동기화 되는지 여부를 표시 하지 않으므로 사용자가 나중에 충돌을 해결 합니다. 하지만이 경우 시스템 해야 한다는 사실을 알고 사용자를 동기화 하도록 (원하는 경우 수 여전히 제목 다시 시작를 하지만 오프 라인으로 재생)는 오래 된 컨테이너를 사용 하 여 잘못 된 상태로 전환 가져올 수 있도록 메시지를 표시 하지 것입니다.

### <a name="4--use-the-rest-of-the-connected-storage-api-as-normal"></a>4: 정상적으로 연결 된 저장소 API의 나머지 부분을 사용 합니다.

필요에 따라 동기화 할 때 연결 된 저장소 동작이 변경 되지 않습니다.
