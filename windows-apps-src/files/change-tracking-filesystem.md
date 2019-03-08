---
title: 배경에서 파일 시스템 변경 추적
description: 사용자가 이동 하는 시스템 백그라운드에서 폴더 및 파일의 변경 내용을 추적 하는 방법을 설명 합니다.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0ec7762fd64f0f0b8de65faa1aaf079bdaba3a3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621578"
---
# <a name="track-file-system-changes-in-the-background"></a>배경에서 파일 시스템 변경 추적

**중요 한 Api**

-   [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker)
-   [**StorageLibraryChangeReader**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader)
-   [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)
-   [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)

합니다 [ **StorageLibraryChangeTracker** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker) 클래스에는 앱 사용자가 이동할 시스템 파일 및 폴더의 변경 내용 추적을 허용 합니다. 사용 하는 **StorageLibraryChangeTracker** 클래스 앱을 추적할 수 있습니다.

- 추가 포함 하 여 작업 파일, 삭제, 수정 합니다.
- 폴더 이름 바꾸기 및 삭제 등의 작업입니다.
- 파일 및 폴더는 드라이브를 이동 합니다.

이 가이드를 사용 하 여 변경 추적 장치를 사용 하 여 작업에 대 한 프로그래밍 모델을 배우기를 몇 가지 샘플 코드를 확인 하 고 다양 한 유형의로 추적 되는 파일 작업 이해 **StorageLibraryChangeTracker**합니다.

**StorageLibraryChangeTracker** 로컬 컴퓨터의 모든 폴더 또는 사용자 라이브러리에 대 한 작동 합니다. 이 보조 드라이브 또는 이동식 드라이브를 포함 하지만 NAS 드라이브 또는 네트워크 드라이브를 포함 하지 않습니다.

## <a name="using-the-change-tracker"></a>변경 내용 추적기를 사용 하 여

마지막으로 저장을 순환 버퍼로 변경 추적 장치 시스템에서 구현 됩니다 *N* 파일 시스템 작업 합니다. 앱은 버퍼에서 변경 내용을 읽고 다음 자신의 환경에 처리할 수 있습니다. 앱 변경 내용 처리 변경 내용을 표시 하 고 두지으로 완료 되 면 해당 다시 표시 합니다.

변경 추적 장치에서 폴더를 사용 하려면 다음이 단계를 수행 합니다.

1. 폴더에 변경 내용 추적을 사용 하도록 설정 합니다.
2. 변경 될 때까지 기다립니다.
3. 변경 내용을 읽습니다.
4. 변경 내용을 적용 합니다.

다음 섹션에서는 각 코드 예제를 사용 하 여 단계를 안내합니다. 전체 코드 샘플은이 문서 끝에 제공 됩니다.

### <a name="enable-the-change-tracker"></a>변경 내용 추적기를 사용 하도록 설정

먼저 앱 작업을 수행 해야 하는 지정 된 라이브러리를 추적 하는 변경 내용에 관심이 있는 시스템에 알릴 하는 것입니다. 이 호출 하 여 수행 합니다 [ **사용 하도록 설정** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) 관심 라이브러리에 대 한 변경 추적 장치에서 메서드.

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

몇 가지 중요 한 참고 사항:

- 앱 권한이 매니페스트에서 올바른 라이브러리를 만들기 전에 있는지 확인 합니다 [ **StorageLibrary** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) 개체입니다. 참조 [파일 액세스 권한을](https://docs.microsoft.com/en-us/windows/uwp/files/file-access-permissions) 대 한 자세한 내용은 합니다.
- [**사용 하도록 설정** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) 은 스레드로부터 안전 하 고 포인터를 다시 설정 되지 것입니다 (보다 나중에) 원하는 만큼 호출할 수 있습니다.

![빈 변경 내용 추적기를 사용 하도록 설정](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>변경 내용에 대 한 대기

변경 추적 장치를 초기화 한 후에 모든 앱 실행 중이 아닌 동안에 라이브러리 내에서 발생 하는 작업 기록에 시작 됩니다. 앱을 등록 하 여 변경 될 때마다 활성화 등록할 수 있습니다 합니다 [ **StorageLibraryChangedTrigger** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger) 이벤트입니다.

![읽어와서 앱 없이 변경 추적 장치에 추가 되는 변경 내용](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>변경 내용을 읽지합니다

앱 변경 추적기에서 변경을 위한 폴링을 고 변경 내용의 목록은 마지막으로 확인 되기 이후에 수신 수 있습니다. 아래 코드에는 변경 추적 장치에서 변경 내용 목록을 가져오는 방법을 보여 줍니다.

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

앱이 다음 자체 환경 또는 필요에 따라 데이터베이스에 변경 내용을 처리 하는 일을 담당 합니다.

![응용 프로그램 데이터베이스에 변경 내용을 변경 추적기에서 읽기](images/changetracker-reading.png)

> [!TIP]
> 사용 하도록 설정 하려면 두 번째 호출 앱 변경 내용을 읽는 동안 사용자 라이브러리에 다른 폴더를 추가 하는 경우 경합을 방어 하는 것입니다. 추가 호출 하지 않고 **사용** 코드 ecSearchFolderScopeViolation (0x80070490)를 사용 하 여 실패는 사용자가 해당 라이브러리에서 폴더를 변경 하는 경우

### <a name="accept-the-changes"></a>변경 내용을 적용합니다

앱이 완료 되 면 변경 내용을 처리 시스템을 다시 표시 안 함 수정 사항을 호출 하 여 조언을 해야 하는 [ **AcceptChangesAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) 메서드.

```csharp
await changeReader.AcceptChangesAsync();
```

![다시 표시 되지 않습니다 되며 되므로 읽기로 변경 표시](images/changetracker-accepting.png)

앱을 이제 받습니다 새 변경 내용을 나중에 변경 내용 추적기를 읽을 때.

- 호출 간에 발생 했을 변경 하는 경우 [ **ReadBatchAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync) 및 [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync), 포인터가 됩니다. 가장 최근의 변경 내용이 앱에 표시 하려면 고급만 합니다. 이러한 변경 내용을 사용할 수 있습니다 다음에 호출한 **ReadBatchAsync**합니다.
- 앱 호출 시간을 변경 하면 다음 동일한 변경 집합을 반환 하려면 시스템을 수락 하지 않습니다 **ReadBatchAsync**합니다.

## <a name="important-things-to-remember"></a>기억해 야 할 중요 한 내용이

변경 내용 추적기를 사용 하면 모든 항목이 제대로 작동 하는지 확인 하기 위해 반드시에서 유지 해야 하는 몇 가지 있습니다.

### <a name="buffer-overruns"></a>버퍼 오버런

앱을 읽을 수 있을 때까지 시스템에서 발생 하는 모든 작업을 저장할 변경 추적기에서 충분 한 공간을 예약 하려고 노력 하지만 앱 순환 버퍼 자체를 덮어쓰기 전에 변경 내용을 읽지 않습니다 시나리오도 것이 쉽습니다. 특히 경우 사용자가 백업에서 데이터를 복원 하거나 자신의 카메라 폰의 사진 큰 컬렉션을 동기화 합니다.

이 예에서 **ReadBatchAsync** 오류 코드가 반환 됩니다 [ **StorageLibraryChangeType.ChangeTrackingLost**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype)합니다. 앱이 오류 코드를 수신 하는 경우에 몇 가지 작업을 의미 합니다.

* 버퍼에 해당 살펴보고 마지막으로 이후 자체를 덮어씁니다. 최선의 작업 라이브러리를 탐색 하려면 이므로 추적기에서 정보를 완료 됩니다.
* 변경 내용 추적기를 호출 하기 전에 자세한 내용을 반환 하지 것입니다 [ **재설정**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset)합니다. 재설정을 호출 하는 앱을 최신 변경에 포인터를 이동할 후 추적 정상적으로 다시 시작 됩니다.

이러한 경우를 가져오려는 경우 거의 하지만 사용자 들이 디스크의 파일 수가 많은 이동 하는 시나리오에서 매우 커질 너무 많은 저장소를 차지 하는 변경 추적 장치 않으려 합니다. 이 Windows에서 고객 환경 하지 주면서 대규모 파일 시스템 작업에 반응 하는 앱을 허용 해야 합니다.

### <a name="changes-to-a-storagelibrary"></a>StorageLibrary 변경

합니다 [ **StorageLibrary** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) 클래스가 다른 폴더를 포함 하는 루트 폴더의 가상 그룹으로 존재 합니다. 파일 시스템 변경 추적기를 사용 하 여이 조정, 다음과 같이 선택을 했습니다.

- 루트 라이브러리 폴더의 하위 항목의 변경 내용이 변경 추적기에서 표시 됩니다. 사용 하 여 루트 라이브러리 폴더를 찾을 수 있습니다 합니다 [ **폴더** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) 속성입니다.
- 추가 또는 제거에서 루트 폴더를 **StorageLibrary** (통해 [ **RequestAddFolderAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync) 고 [ **RequestRemoveFolderAsync**  ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)) 변경 추적기에서 항목을 생성 합니다. 이러한 변경 내용을 통해 추적할 수 있습니다 합니다 [ **DefinitionChanged** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged) 이벤트를 사용 하 여 라이브러리의 루트 폴더를 열거 하 여를 [ **폴더** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) 속성입니다.
- 폴더에 이미 내용이 포함 된 라이브러리에 추가 되 면 더 이상 없습니다 변경 알림 또는 변경 추적기 항목 생성 합니다. 해당 폴더의 하위 항목 이후에 변경 알림을 생성 되며 추적 항목을 변경 합니다.

### <a name="calling-the-enable-method"></a>Enable 메서드를 호출합니다.

앱에서 호출 해야 [ **사용 하도록 설정** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) 추적 파일 시스템을 시작 및 모든 열거형 변경 하기 전에 합니다. 이렇게 하면 모든 변경 내용을 변경 추적기에서 캡처됩니다.  

## <a name="putting-it-together"></a>과정

비디오 라이브러리의 변경 내용을 등록 하 고 변경 추적기에서 변경 내용을 풀링 시작에 사용 되는 모든 코드는 다음과 같습니다.

```csharp
private async void EnableChangeTracker()
{
    StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
    StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
    videoTracker.Enable();
}

private async void GetChanges()
{
    StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
    videosLibrary.ChangeTracker.Enable();
    StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
    IReadOnlyList changeSet = await changeReader.ReadBatchAsync();


    //Below this line is for the blog post. Above the line is for the magazine
    foreach (StorageLibraryChange change in changeSet)
    {
        if (change.ChangeType == StorageLibraryChangeType.ChangeTrackingLost)
        {
            //We are in trouble. Nothing else is going to be valid.
            log("Resetting the change tracker");
            videosLibrary.ChangeTracker.Reset();
            return;
        }
        if (change.IsOfType(StorageItemTypes.Folder))
        {
            await HandleFileChange(change);
        }
        else if (change.IsOfType(StorageItemTypes.File))
        {
            await HandleFolderChange(change);
        }
        else if (change.IsOfType(StorageItemTypes.None))
        {
            if (change.ChangeType == StorageLibraryChangeType.Deleted)
            {
                RemoveItemFromDB(change.Path);
            }
        }
    }
    await changeReader.AcceptChangesAsync();
}
```
