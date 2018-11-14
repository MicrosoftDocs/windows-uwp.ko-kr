---
title: 연결 된 저장소를 사용 하 여 데이터를 로드 합니다.
author: aablackm
description: 연결 된 저장소를 사용 하 여 데이터를 로드 하는 방법을 알아봅니다.
ms.assetid: c660a456-fe7d-453a-ae7b-9ecaa2ba0a15
ms.author: aablackm
ms.date: 02/27/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 연결 된 저장소, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 5a54076f6105c4dbef257408a4af0cfe19f0574e
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6187424"
---
# <a name="use-connected-storage-to-load-data"></a>연결 된 저장소를 사용 하 여 데이터를 로드 합니다.

데이터를 비동기적으로 사용 하 여 읽기는 `ReadAsync` 또는 `GetAsync` 저장소 메서드를 연결 합니다.

### <a name="to-load-data-from-connected-storage"></a>연결 된 저장소에서 데이터를 로드

1.  검색 한 `ConnectedStorageSpace` 를 호출 하 여 사용자에 대 한 `GetForUserAsync`.

    반환 된 XDK 예제에서 `ConnectedStorageSpace` 쉽게 관리할 수 있도록 지도에 추가 되는 `ConnectedStorageSpace` 여러 사용자에 대 한 개체입니다.

2.  만들기는 `ConnectedStorageContainer` 를 호출 하 여 `CreateContainer` 에 `ConnectedStorageSpace`합니다.
3.  호출 하 여 데이터를 검색할 `ReadAsync` 또는 `GetAsync` 에 `ConnectedStorageContainer`합니다. `ReadAsync` 동안 버퍼에 전달 해야 `GetAsync` 읽을 수 있는 데이터를 저장 하는 새 버퍼를 할당 합니다.

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

void OnLoadCompleted(IAsyncAction^ action, Windows::Foundation::AsyncStatus status);

// Load data from a fixed container/blob name into an IBuffer
void Load(Windows::Storage::Streams::IBuffer^ buffer)
{

    auto reads = ref new Platform::Collections::Map<Platform::String^, Windows::Storage::Streams::IBuffer^>();
    reads->Insert("data", buffer);

    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(gCurrentUser->Id);
    auto container = storageSpace->CreateContainer("Saves/Checkpoint");

    //Save Data is Loading

    auto op = container->ReadAsync(reads->GetView());

    op->Completed = ref new AsyncActionCompletedHandler(OnLoadCompleted);
}

void OnLoadCompleted(IAsyncAction^ action, Windows::Foundation::AsyncStatus status)
{
    switch (action->Status)
    {
    case Windows::Foundation::AsyncStatus::Completed:
        //Successful load logic here.
        break;

    case Windows::Foundation::AsyncStatus::Error:
    case Windows::Foundation::AsyncStatus::Canceled:
        //Fail logic here
        break;

    default:
        //all other possible values of action->status are also failures, alternate fail logic here. 
        break;
    }
}
```

XDK 연결 된 저장소 Api XDK.chm 파일 경로 아래에 설명 된 찾을 수 있습니다: **Xbox ONE XDK >> API 참조 >> 플랫폼 API 참조 >> 시스템 API 참조 >> Windows.Xbox.Storage**합니다.
XDK Api도 [developer.microsoft.com 사이트](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n)에 문서화 되어 있습니다.
XDK Api에 대 한 링크는 Microsoft Account(MSA) Xbox 개발자 Kit(XDK) 액세스 가능 하도록 설정 되어 있어야 합니다.
Windows.Xbox.Storage에는 Xbox One 본체에 대 한 연결 된 저장소 네임 스페이스의 이름입니다.

## <a name="c-uwp-sample"></a>C# UWP 샘플

XDK 게임 및 UWP 앱 다른 Api을 사용할 수 있는 동안 XDK API 후 UWP API는 매우 밀접 하 게 모델링 됩니다. 데이터를 로드 하는 일부 네임 스페이스 및 클래스 이름 변경을 메모 하는 동안 같은 기본 단계를 수행 하려면 여전히 해야 합니다. 네임 스페이스를 사용 하는 대신 `Windows::Xbox::Storage` 사용 `Windows.Gaming.XboxLive.Storage`. 클래스 `ConnectedStorageSpace`을 하는 것 `GameSaveProvider`. 클래스 `ConnectedStorageContainer` 는 `GameSaveContainer`. 이러한 변경 내용은 [포팅 Xbox Live 코드에서 XDK에서 UWP로](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md)의 연결 된 저장소 섹션에 자세히 설명 됩니다.

```csharp
//Namespace Required
Windows.Gaming.XboxLive.Storage

//Get The User
var users = await Windows.System.User.FindAllAsync();

int intData = 0;
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
    //throw new Exception("Game Save Provider Initialization failed");;
}

//Now you have a GameSaveProvider
//Next you need to call CreateContainer to get a GameSaveContainer

GameSaveContainer gameSaveContainer = gameSaveProvider.CreateContainer(c_saveContainerName);
//Parameter
//string name (name of the GameSaveContainer Created)

//form an array of strings containing the blob names you would like to read.
string[] blobsToRead = new string[] { c_saveBlobName };

// GetAsync allocates a new Dictionary to hold the retrieved data. You can also use ReadAsync
// to provide your own preallocated Dictionary.
GameSaveBlobGetResult result = await gameSaveContainer.GetAsync(blobsToRead);

int loadedData = 0;

//Check status to make sure data was read from the container
if(result.Status == GameSaveErrorStatus.Ok)
{
    //prepare a buffer to receive blob
    IBuffer loadedBuffer;

    //retrieve the named blob from the GetAsync result, place it in loaded buffer.
    result.Value.TryGetValue(c_saveBlobName, out loadedBuffer);

    if(loadedBuffer == null)
    {

        throw new Exception(String.Format("Didn't find expected blob \"{0}\" in the loaded data.", c_saveBlobName));

    }
    DataReader reader = DataReader.FromBuffer(loadedBuffer);
    loadedData = reader.ReadInt32();
}
```

UWP 앱에 대 한 연결 된 저장소 Api는 [Xbox Live API 참조](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage)에 설명 되어 있습니다.
[Xbox Live API 샘플 게임 프로젝트 저장](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave)체크아웃 연결 된 저장소를 사용 하는 다른 샘플을 확인 합니다.