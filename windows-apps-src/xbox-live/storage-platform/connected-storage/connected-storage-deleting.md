---
title: 연결 된 저장소를 사용 하 여 데이터를 삭제 하려면
description: 연결 된 저장소 blob 및 컨테이너 데이터 삭제를 사용 하는 방법을 알아봅니다.
ms.assetid: ccf7488c-5d55-480e-b3aa-412220d03104
ms.date: 02/27/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 연결 된 저장소, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 756de46d05cdbf64d85491b4e8c6f783122f2356
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601318"
---
# <a name="use-connected-storage-to-delete-data"></a>연결 된 저장소를 사용 하 여 데이터를 삭제 하려면

데이터 blob 만들기에 의해 비동기적으로 삭제 됩니다는 `ConnectedStorageContainer` 에 `ConnectedStorageSpace` 호출을 사용자에 대 한는 `SubmitUpdatesAsync` blobsToDelete 매개 변수에 대해 삭제할 명명 된 blob을 나타내는 문자열의 목록을 제공 하는 컨테이너의 메서드.

데이터 컨테이너를 만들어 비동기적으로 삭제 되는 `ConnectedStorageContainer` 호출 및 해당 `DeleteContainerAsync` 메서드.

## <a name="to-delete-blob-data-from-connected-storage"></a>연결 된 저장소에서 blob 데이터를 삭제 하려면

1.  검색을 `ConnectedStorageSpace` 를 호출 하 여 사용자에 대 한 개체 `GetForUserAsync`합니다.

    반환 된 XDK 예와에서 `ConnectedStorageSpace` 개체의 간편한 관리를 사용 하도록 설정 하려면 지도에 추가 되 `ConnectedStorageSpace` 여러 사용자에 대 한 개체입니다.

2.  만들기는 `ConnectedStorageContainer` 개체를 호출 하 여 `CreateContainer` 에 `ConnectedStorageSpace` 개체입니다.
3.  호출 `SubmitUpdatesAsync` 에 `ConnectedStorageContainer` 개체입니다.

## <a name="to-delete-a-container-from-connected-storage"></a>연결 된 저장소에서 컨테이너를 삭제 하려면

1.  검색을 `ConnectedStorageSpace` 를 호출 하 여 사용자에 대 한 개체 `GetForUserAsync`합니다.

    반환 된 XDK 예와에서 `ConnectedStorageSpace` 개체의 간편한 관리를 사용 하도록 설정 하려면 지도에 추가 되 `ConnectedStorageSpace` 여러 사용자에 대 한 개체입니다.

2.  호출 된 `DeleteContainerAsync` ConnectedStorageSpace 메서드의 메서드.

## <a name="c-xdk-sample"></a>C + + XDK 샘플
```cpp
auto gConnectedStorageSpaceForUsers = ref new Platform::Collections::Map<unsigned int, Windows::Xbox::Storage::ConnectedStorageSpace^>();

bool GetUserInputYesOrNo() { return true; };

User^ gCurrentUser;
IBuffer^ WrapRawBuffer(void* ptr, size_t size);

// Acquire a Connected Storage space for a user. A Connected Storage space is required to manipulate Connected Storage Data.
void PrepareConnectedStorage(User^ user)
{
  auto op = ConnectedStorageSpace::GetForUserAsync(user);
  op->Completed = ref new AsyncOperationCompletedHandler<ConnectedStorageSpace^>(
    [=](IAsyncOperation<ConnectedStorageSpace^>^ operation, Windows::Foundation::AsyncStatus status)
    {
      switch(status)
      {
        case Windows::Foundation::AsyncStatus::Completed:
          gConnectedStorageSpaceForUsers->Insert(user->Id, operation->GetResults());
          break;
        case Windows::Foundation::AsyncStatus::Error:
        case Windows::Foundation::AsyncStatus::Canceled:
          // Present user option: ?Would you like to continue without saving progress??
          if( GetUserInputYesOrNo() )
            //If the users opts yes, continue in offline mode
          else
            //If the users opts no, retry.
          break;
      }
    });
}

// Delete data from a fixed container/blob name into an IBuffer
void DeleteBlob(User^ user)
{

    //Create a list of blob names to delete
    std::vector<Platform::String^> blobsToDelete;

    //Add the blob name to your Generic Iterable
    blobsToDelete.push_back(L"someBlobName");

    auto blobNames = ref new Platform::Collections::VectorView<Platform::String^>(blobsToDelete);

    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(user->Id);
    auto container = storageSpace->CreateContainer("Saves/Checkpoint");

    //Delete is occurring asynchronously at this point.

    auto op = container->SubmitUpdatesAsync(nullptr, blobNames);

    op->Completed = ref new AsyncActionCompletedHandler(
        [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
            switch(status)
            {
                case Windows::Foundation::AsyncStatus::Completed:
                //Delete was successful no need for action
                break;
                case Windows::Foundation::AsyncStatus::Error:
                case Windows::Foundation::AsyncStatus::Canceled:
                // Handle failed deletion
            }
     });
}

void DeleteContainer(User^ user)
{
    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(user->Id);

    auto deleteOperation = storageSpace->DeleteContainerAsync("Saves/Checkpoint");

    deleteOperation->Completed = ref new AsyncActionCompletedHandler(
        [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
            switch(status)
            {
                case Windows::Foundation::AsyncStatus::Completed:
                //Delete was successful no need for action
                break;
                case Windows::Foundation::AsyncStatus::Error:
                case Windows::Foundation::AsyncStatus::Canceled:
                // Handle failed deletion
            }
     });

}
```

XDK.chm 파일 경로 아래에 설명 된 XDK 연결 된 저장소 Api를 찾을 수 있습니다. **Xbox 하나 XDK >> API 참조 >> 플랫폼 API 참조 >> 시스템 API 참조 >> Windows.Xbox.Storage**합니다.
XDK Api에도 설명 되어는 [developer.microsoft.com 사이트](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n)합니다.
XDK Api에 대 한 링크는 Microsoft Account(MSA) Xbox 개발자 Kit(XDK) 액세스 가능 하도록 설정 되어 있어야 합니다.
Windows.Xbox.Storage에는 Xbox One 콘솔에 대 한 저장소 연결 된 네임 스페이스의 이름입니다.


## <a name="c-uwp-sample"></a>C#UWP 샘플

XDK 게임 및 UWP 앱에 다른 Api를 사용할 수 있습니다, UWP API XDK API 후 매우 밀접 하 게 모델링 됩니다. 데이터를 삭제 하는 일부 네임 스페이스 및 클래스 이름 변경 기록 하는 동안 동일한 기본 단계를 수행 하려면 계속 해야 합니다. 네임 스페이스를 사용 하는 대신 `Windows::Xbox::Storage` 사용할지 `Windows.Gaming.XboxLive.Storage`합니다. 클래스 `ConnectedStorageSpace`에 해당 하는 `GameSaveProvider`합니다. 클래스 `ConnectedStorageContainer` 같습니다 `GameSaveContainer`합니다. 이러한 변경 내용은 연결 된 저장소 부분에서 자세히 설명 됩니다 [이식 Xbox Live 코드에서 XDK uwp](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md)합니다.

```csharp
//Namespace Required
Windows.Gaming.XboxLive.Storage

//Get The User
var users = await Windows.System.User.FindAllAsync();

int intData = 23;
const string c_saveBlobName = "Jersey";
const string c_saveContainerDisplayName = "GameSave";
const string c_saveContainerName = "GameSaveContainer";
GameSaveProvider gameSaveProvider;

GameSaveProviderGetResult gameSaveTask = await GameSaveProvider.GetForUserAsync(users[0], context.AppConfig.ServiceConfigurationId);
//Parameters
//Windows.System.User user
//string SCID

if(gameSaveTask.Status == GameSaveErrorStatus.Ok)
{
    gameSaveProvider = gameSaveTask.Value;
}
else
{
    return;
    //throw new Exception("Game Save Provider Initialization failed");
}

//Now you have a GameSaveProvider (formerly ConnectedStorageSpace)
//Next you need to call CreateContainer to get a GameSaveContainer (formerly ConnectedStorageContainer)

GameSaveContainer gameSaveContainer = gameSaveProvider.CreateContainer(c_saveContainerName); // this will create a new named game save container with the name = to the input name
//Parameter
//string name (name of container to be created) 

//DELETE A BLOB
//form an array of strings containing the blob names you would like to delete.
string[] blobsToDelete = new string[] { c_saveBlobName };

//Call SubmitUpdatesAsync with the dictionary of the names of blobs to delete as the second parameter.
GameSaveOperationResult gameSaveOperationResult = await gameSaveContainer.SubmitUpdatesAsync(null, blobsToDelete, c_saveContainerDisplayName);
//Parameters
//IReadOnlyDictionary<String, IBuffer> blobsToWrite
//IEnumerable<string> blobsToDelete
//string displayName

//Check status of the operation to ensure successful delete.
if(gameSaveOperationResult.Status == GameSaveErrorStatus.Ok)
{
    //Successful Deletion Logic
}
else
{
    //handle failed deletion logic. 
}

//DELETE A SAVE CONTAINER
GameSaveOperationResult deleteContainerResult = await gameSaveProvider.DeleteContainerAsync(c_saveContainerName);
//Parameter
//string name (name of container to be deleted)

//Check status of the operation to ensure successful delete.
if(deleteContainerResult.Status == GameSaveErrorStatus.Ok)
{
    //Successful Deletion Logic
}
else
{
    //handle failed deletion logic. 
}
```

에 설명 된 UWP 앱에 대 한 Storage Api를 연결 합니다 [Xbox Live API 참조](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage)합니다.
연결 된 저장소 체크 아웃을 사용 하는 다른 예제를 보려면 합니다 [Xbox Live API 샘플 게임 프로젝트 저장](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave)합니다.
