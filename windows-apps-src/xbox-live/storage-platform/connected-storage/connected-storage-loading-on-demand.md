---
title: 연결 시 저장소 로드
description: 대신 한 번에 모두 주문형 연결 된 저장소 데이터를 로드 하는 방법에 알아봅니다.
ms.assetid: a0797a14-c972-4017-864c-c6ba0d5a3363
ms.date: 02/27/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 연결 된 저장소, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 31c1893f09e6d56682b4e718ee8b905ce72c7ad8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631718"
---
# <a name="connected-storage-loading-on-demand"></a>연결 시 저장소 로드

`GetSyncOnDemandForUserAsync` 한 번에 대신 "주문형" 연결 된 저장소 공간에서 클라우드 기반 데이터를 로드할 수 있습니다. 이 통해 성능이 개선 `GetForUserAsync` 특히 큰 파일을 저장 하는 경우에 합니다.

## <a name="to-load-data-from-a-connected-storage-space-on-demand"></a>주문형 연결 된 저장소 공간에서 데이터를 로드 하려면

### <a name="1--call-getsyncondemandforuserasync"></a>1:  호출 `GetSyncOnDemandForUserAsync`

이 클라우드로 해당 내용이 없습니다에서 컨테이너 및 해당 메타 데이터의 목록을 다운로드 하는 부분 동기화를 트리거합니다. 이 작업은 빠른 하 고 적절 한 네트워크 환경에서는 사용자 UI 모든 로드 나타나지 않아야 합니다.

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


### <a name="2--perform-a-container-query-using-getcontainerinfo2async"></a>2:  사용 하 여 컨테이너 쿼리 수행 `GetContainerInfo2Async`

컬렉션을 반환 합니다이 `ContainerInfo2`, 3 개의 새 메타 데이터 필드를 포함 하는:

    -   `DisplayName`: 오버 로드를 사용 하 여 작성 한 모든 표시 이름 `SubmitUpdatesAsync` 표시 이름 문자열을 매개 변수로 사용 합니다. 항상이 필드를 사용 하 여 알기 쉬운 이름을 저장 하는 것이 좋습니다.
    -   `LastModifiedTime`: 이 컨테이너에서 수정 된 마지막 시간입니다. 충돌 하는 로컬 및 원격 타임 스탬프의 경우 원격 작업 사용은 note 합니다.
    -   `NeedsSync`: 클라우드에서 다운로드 할 나타내는 부울 값을이 컨테이너에 데이터가 있는 경우.

    이 추가 메타 데이터를 사용 하 여 제공할 수 있습니다 사용자가 게임 저장 하는 방법에 대 한 핵심 정보를 사용 하 여 (이름을 포함 하 여 마지막에 사용 되는 낮고 여부 하나를 선택 하는 다운로드) 실제로 클라우드에서 전체 다운로드를 수행 하지 않고 있습니다.

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

### <a name="3--trigger-a-sync"></a>3:  동기화 트리거

기존 연결 된 저장소 API를 다음 중 하나를 호출 하 여 연결 된 저장소 synce가 트리거됩니다.

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

이렇게 하면 사용자에 게 선택한 컨테이너에서 데이터를 다운로드 하는 대로 일반적인 동기화 UI 및 진행률 표시줄을 표시 합니다. 선택한 컨테이너의 데이터만 동기화 있는지; note 다른 컨테이너에서 데이터 다운로드 되지 않습니다.

에 필요 시 동기화의 컨텍스트에서 이러한 API를 호출할 때 이러한 작업 모두 생성할 수 다음 새 오류 코드:

-   `ConnectedStorageErrorStatus::ContainerSyncFailed`(`GameSaveErrorStatus.ContainerSyncFailed` UWP의 C# API): 이 오류는 작업이 실패 하 고 컨테이너를 클라우드로 동기화 할 수 없는 나타냅니다. 가장 가능성이 높은 원인은 동기화 실패를 발생 시킨 사용자의 네트워크 조건을 보여 줍니다. 되지 않은 컨테이너를 사용 하도록 선택할 수 있습니다 또는 해당 네트워크를 수정한 후 다시 시도 하려는 동기화 합니다. UI에 이러한 옵션 중 하나를 허용 해야 합니다. 대화 상자가 없습니다. 다시 시도 필요 이므로 이러한는 앞서 살펴본 시스템 UI 재시도 대화 상자입니다.

-   `ConnectedStorageErrorStatus::ContainerNotInSync`(`GameSaveErrorStatus.ContainerNotInSync` UWP의 C# API): 이 오류는 제목이 잘못 시도 동기화 되지 않은 컨테이너에 쓸 것을 나타냅니다. 호출 `ConnectedStorageContainer::SubmitUpdatesAsync`(`GameSaveContainer.SubmitUpdatesAsync` UWP의 C# API) NeedsSync 플래그가 있는 컨테이너에서 true로 설정 되지 않습니다. 먼저 컨테이너에 쓰기 전에 동기화를 트리거를 읽어야 합니다. 읽지도 않고 컨테이너를 쓸 경우 것을 제목에 버그가 있는 사용자 진행률 손실 될 수 있습니다.

이 동작은 경우 사용자는 오프 라인에서 서로 다릅니다. 하는 동안 오프 라인으로 표시가 없습니다 컨테이너 동기화 여부의 책임은 사용자를 나중에 충돌을 해결에 있기 때문입니다. 그러나이 경우 시스템 알고 사용자를 동기화 해야 하므로 (원하는 경우 여전히 제목을 다시 시작 하 고 수 오프 라인 재생) 하지만 오래 된 컨테이너를 사용 하 여 잘못 된 상태를 가져오기 위해 허용 되지 것입니다.

### <a name="4--use-the-rest-of-the-connected-storage-api-as-normal"></a>4:  정상적으로 연결 된 저장소 API의 나머지 부분을 사용 하 여

요청 시 동기화 할 때 연결 된 저장소 동작은 변경 되지 않습니다.
