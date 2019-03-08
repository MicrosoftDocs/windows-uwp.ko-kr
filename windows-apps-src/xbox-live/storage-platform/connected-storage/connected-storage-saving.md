---
title: 연결 된 저장소를 사용 하 여 데이터를 저장 합니다.
description: 연결 된 저장소를 사용 하 여 데이터를 저장 하는 방법에 알아봅니다.
ms.assetid: ccf7488c-5d55-480e-b3aa-412220d03104
ms.date: 02/27/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 연결 된 저장소, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 4140e3bbe0f0ab229e3637008e01892f4179292e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617618"
---
# <a name="use-connected-storage-to-save-data"></a>연결 된 저장소를 사용 하 여 데이터를 저장 합니다.


데이터를 만들어 비동기적으로 저장 됩니다는 `ConnectedStorageContainer` 에 `ConnectedStorageSpace` 호출을 사용자에 대 한를 `SubmitUpdatesAsync` 컨테이너에서 메서드.

> [!IMPORTANT]
> 연결 된 저장소 컨테이너 간에 종속성 데이터는 안전 하지 않습니다. 예를 들어, 하나의 컨테이너를 클라우드로 업로드 될 수 있습니다, 때는 완료할 다른 업로드에 대 한 큐에 대기 중인 남아 있을 수 있습니다. 사용자 다른 콘솔로 이동 하는 경우 동기화 작업은 첫 번째 컨테이너를 동기화 하 고 존재 하는 첫 번째 컨테이너 없이 두 번째 콘솔에서 액세스할 수를 허용 합니다.

## <a name="to-save-data-to-connected-storage"></a>연결 된 저장소에 데이터를 저장 하려면

1.  검색을 `ConnectedStorageSpace` 를 호출 하 여 사용자에 대 한 개체 `GetForUserAsync`합니다.

    반환 된 XDK 예와에서 `ConnectedStorageSpace` 개체의 간편한 관리를 사용 하도록 설정 하려면 지도에 추가 되 `ConnectedStorageSpace` 여러 사용자에 대 한 개체입니다.

2.  만들기는 `ConnectedStorageContainer` 개체를 호출 하 여 `CreateContainer` 에 `ConnectedStorageSpace` 개체입니다.
3.  호출 `SubmitUpdatesAsync` 에 `ConnectedStorageContainer` 사용 하 여 데이터 blob으로 저장 게임을 `blobsToWrite` 매개 변수입니다.

## <a name="c-xdk-sample"></a>C + + XDK 샘플

```cpp
auto gConnectedStorageSpaceForUsers = ref new Platform::Collections::Map<unsigned int, Windows::Xbox::Storage::ConnectedStorageSpace^>();

bool GetUserInputYesOrNo() {return true;};

User^ gCurrentUser;
IBuffer^ WrapRawBuffer( void* ptr, size_t size );

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

enum Color { RED, BLUE };
enum EngineSize { BIG, SMALL };

struct CarData
{
    Color color;
    bool hasWheels;
    bool hasFancyRims;
    EngineSize engineSize;
};


const int MAX_CARS = 10;

struct SaveData
{
    CarData cars[MAX_CARS];
    int numCars;
    int currentCar;
    int cash;
};

SaveData gMySaveData;

void SaveCheckpoint(Windows::Storage::Streams::IBuffer^ buffer, User^ user);

bool ItIsTimeToSaveACheckpoint() {return true;};

void Update()
{
    if (ItIsTimeToSaveACheckpoint())
        SaveCheckpoint(WrapRawBuffer(&gMySaveData,sizeof(SaveData)),gCurrentUser);
}


void SaveCheckpoint(Windows::Storage::Streams::IBuffer^ buffer, User^ user)
{
     auto storageSpace = gConnectedStorageSpaceForUsers->Lookup( user->Id );

     auto container = storageSpace->CreateContainer("Saves/Checkpoint");

     auto updates = ref new Platform::Collections::Map<Platform::String^, Windows::Storage::Streams::IBuffer^>();
     updates->Insert("data", buffer);

     auto op = container->SubmitUpdatesAsync(updates->GetView(), nullptr);

     //Save is happening here asynchronously

     op->Completed = ref new AsyncActionCompletedHandler(
               [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
                   //Save function has completed
                   //This area can be filled with further post save logic.
     });
}
```

XDK.chm 파일 경로 아래에 설명 된 XDK 연결 된 저장소 Api를 찾을 수 있습니다. **Xbox 하나 XDK >> API 참조 >> 플랫폼 API 참조 >> 시스템 API 참조 >> Windows.Xbox.Storage**합니다.
XDK Api에도 설명 되어는 [developer.microsoft.com 사이트](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n)합니다.
XDK Api에 대 한 링크는 Microsoft Account(MSA) Xbox 개발자 Kit(XDK) 액세스 가능 하도록 설정 되어 있어야 합니다.
Windows.Xbox.Storage에는 Xbox One 콘솔에 대 한 저장소 연결 된 네임 스페이스의 이름입니다.

## <a name="c-uwp-sample"></a>C#UWP 샘플

XDK 게임 및 UWP 앱에 다른 Api를 사용할 수 있습니다, UWP API XDK API 후 매우 밀접 하 게 모델링 됩니다. 데이터를 저장 하는 일부 네임 스페이스 및 클래스 이름 변경 기록 하는 동안 동일한 기본 단계를 수행 하려면 계속 해야 합니다. 네임 스페이스를 사용 하는 대신 `Windows::Xbox::Storage` 사용할지 `Windows.Gaming.XboxLive.Storage`합니다. 클래스 `ConnectedStorageSpace`에 해당 하는 `GameSaveProvider`합니다. 클래스 `ConnectedStorageContainer` 같습니다 `GameSaveContainer`합니다. 이러한 변경 내용은 연결 된 저장소 부분에서 자세히 설명 됩니다 [이식 Xbox Live 코드에서 XDK uwp](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md)합니다.

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
//string name

// To store a value in the container, it needs to be written into a buffer, then stored with
// a blob name in a Dictionary.

DataWriter writer = new DataWriter();

writer.WriteInt32(intData); //some number you want to save, in this case 23.

IBuffer dataBuffer = writer.DetachBuffer();

var blobsToWrite = new Dictionary<string, IBuffer>();

blobsToWrite.Add(c_saveBlobName, dataBuffer);

GameSaveOperationResult gameSaveOperationResult = await gameSaveContainer.SubmitUpdatesAsync(blobsToWrite, null, c_saveContainerDisplayName);
//IReadOnlyDictionary<String, IBuffer> blobsToWrite
//IEnumerable<string> blobsToDelete
//string displayName
```

에 설명 된 UWP 앱에 대 한 Storage Api를 연결 합니다 [Xbox Live API 참조](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage)합니다.
연결 된 저장소 체크 아웃을 사용 하는 다른 예제를 보려면 합니다 [Xbox Live API 샘플 게임 프로젝트 저장](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave)합니다.