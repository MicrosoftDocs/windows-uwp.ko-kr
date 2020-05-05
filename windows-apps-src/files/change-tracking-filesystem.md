---
title: 백그라운드에서 파일 시스템 변경 추적
description: 사용자가 시스템에서 파일 및 폴더를 이동할 때 백그라운드에서 파일과 폴더의 변경 내용을 추적하는 방법을 설명합니다.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1cef2fb660681d3e382eb8ca7dcb92456756f627
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75685230"
---
# <a name="track-file-system-changes-in-the-background"></a>백그라운드에서 파일 시스템 변경 추적

**중요 API**

-   [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker)
-   [**StorageLibraryChangeReader**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader)
-   [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)
-   [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)

[**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker) 클래스를 사용하면 앱에서는 사용자가 시스템에서 파일 및 폴더를 이동할 때 변경 내용을 추적할 수 있습니다. **StorageLibraryChangeTracker** 클래스를 사용하면 앱은 다음을 추적할 수 있습니다.

- 추가, 삭제, 수정을 포함하는 파일 작업
- 이름 바꾸기 및 삭제와 같은 폴더 작업
- 드라이브에서의 파일 및 폴더 이동

이 가이드에서는 변경 추적 장치를 사용하기 위한 프로그래밍 모델을 학습하고, 몇 가지 샘플 코드를 확인하고, **StorageLibraryChangeTracker**에서 추적되는 다양한 유형의 파일 작업을 이해합니다.

**StorageLibraryChangeTracker**는 로컬 컴퓨터의 사용자 라이브러리 또는 폴더에 작동합니다. 여기에는 보조 드라이브 또는 이동식 드라이브가 포함되지만 NAS 드라이브 또는 네트워크 드라이브는 포함되지 않습니다.

## <a name="using-the-change-tracker"></a>변경 추적 장치 사용

변경 추적 장치는 마지막 *N*개 파일 시스템 작업을 저장하는 순환 버퍼로 시스템에서 구현됩니다. 앱은 버퍼에서 변경 내용을 읽은 다음, 고유한 환경으로 처리할 수 있습니다. 앱 변경이 완료되면 해당 변경을 처리됨으로 표시하고 다시 확인하지 않습니다.

폴더에 대해 변경 추적 장치를 사용하려면 다음 단계를 수행합니다.

1. 폴더에 변경 내용 추적을 설정합니다.
2. 변경될 때까지 기다립니다.
3. 변경 내용을 읽습니다.
4. 변경 내용을 적용합니다.

다음 섹션에서는 몇 가지 코드 예제를 사용하여 각 단계를 안내합니다. 전체 코드 샘플은 이 문서 끝에 제공됩니다.

### <a name="enable-the-change-tracker"></a>변경 추적 장치 사용

앱이 처음 수행해야 하는 작업은 지정된 라이브러리의 변경 추적에 관심이 있는다는 사실을 시스템에 알리는 것입니다. 이를 위해 관심 있는 라이브러리의 변경 추적 장치에 대해 [**Enable**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) 메서드를 호출합니다.

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

몇 가지 중요한 참고 사항:

- [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) 개체를 만들기 전에 매니페스트의 올바른 라이브러리에 대한 권한이 앱에 있는지 확인합니다. 자세한 내용은 [파일 액세스 권한](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)을 참조하세요.
- [**Enable**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable)은 스레드로부터 안전하고, 포인터를 다시 설정하지 않으며, 원하는 만큼 호출할 수 있습니다(이 문서 뒷부분에 자세히 설명됨).

![빈 변경 추적 장치 사용](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>변경될 때까지 대기

변경 추적 장치가 초기화되면 앱이 실행되고 있지 않을 때도 라이브러리 내에서 발생하는 모든 작업을 기록하기 시작합니다. [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger) 이벤트를 등록하여 변경 내용이 있을 때마다 앱이 활성화하도록 등록할 수 있습니다.

![변경 내용이 변경 추적 장치에 추가되기만 하고 앱에서 읽지 않음](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>변경 내용 읽기

그런 후에 앱은 변경 추적 장치에서 변경 내용을 폴링하고 마지막으로 확인한 후에 변경 내용 목록을 수신합니다. 아래 코드에서는 변경 추적 장치에서 변경 내용 목록을 가져오는 방법을 보여 줍니다.

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

앱은 필요에 따라 고유한 환경 또는 데이터베이스로 변경 내용을 처리합니다.

![변경 추적 장치에서 앱 데이터베이스로 변경 내용 읽기](images/changetracker-reading.png)

> [!TIP]
> 사용하도록 설정할 두 번째 호출은 앱이 변경 내용을 읽는 동안 사용자가 라이브러리에 다른 폴더를 추가하는 경우의 경합 상태를 방어하기 위한 것입니다. **Enable** 코드를 추가로 호출하지 않으면 사용자가 해당 라이브러리에서 폴더를 변경하는 경우 코드는 ecSearchFolderScopeViolation(0x80070490)을 나타내며 실패합니다.

### <a name="accept-the-changes"></a>변경 내용 적용

앱이 변경 내용 처리를 완료하면 [**AcceptChangesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) 메서드를 호출하여 해당 변경 내용을 다시 보지 않을 것임을 시스템에 알려야 합니다.

```csharp
await changeReader.AcceptChangesAsync();
```

![다시 표시되지 않도록 변경 내용을 읽음으로 표시](images/changetracker-accepting.png)

이제 앱은 앞으로 변경 추적 장치를 읽을 때만 새 변경 내용을 수신합니다.

- [**ReadBatchAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync) 및 [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) 호출 중간에 변경이 발생한 경우 포인터가 앱이 확인한 가장 최근 변경 내용으로만 이동합니다. 이러한 변경 내용은 다음번에 **ReadBatchAsync**를 호출할 때 계속 사용할 수 있습니다.
- 변경 내용을 수락하지 않으면 다음번에 앱이 **ReadBatchAsync**를 호출할 때 시스템은 동일한 변경 세트를 반환하게 됩니다.

## <a name="important-things-to-remember"></a>유의해야 할 중요한 사항

변경 추적 장치를 사용할 때는 아무 문제가 없도록 하기 위해 몇 가지 유지해야 할 사항이 있습니다.

### <a name="buffer-overruns"></a>버퍼 오버런

앱이 읽을 수 있을 때까지 시스템에서 발생하는 모든 작업을 변경 추적 장치에 저장할 충분한 공간을 예약하려고 하지만 순환 버퍼가 자체를 덮어쓰기 전에 앱이 변경 내용을 읽지 않는 시나리오를 쉽게 예상할 수 있습니다. 특히, 사용자가 백업에서 데이터를 복원하거나 휴대폰 카메라에서 대량의 사진 모음을 동기화하는 경우는 더욱 그렇습니다.

이 예에서 **ReadBatchAsync**는 오류 코드 [**StorageLibraryChangeType.ChangeTrackingLost**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype)를 반환합니다. 앱이 오류 코드를 수신하는 것은 다음과 같은 의미를 갖습니다.

* 사용자가 마지막으로 확인한 후에 버퍼가 자체적으로 덮어썼습니다. 최선의 작업 과정은 추적 장치의 정보가 불완전하므로 라이브러리를 다시 탐색하는 것입니다.
* 변경 추적 장치는 [**Reset**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset)을 호출할 때까지 추가 변경 내용을 반환하지 않습니다. 앱이 reset을 호출하면 포인터가 가장 최신 변경 내용으로 이동하며 추적이 정상적으로 다시 시작됩니다.

이러한 경우는 거의 드물지만, 사용자가 디스크에서 대량의 데이터를 이동하는 경우에는 변경 추적 장치가 커져서 너무 많은 스토리지를 차지하는 것은 원치 않을 것입니다. 그래야 앱이 Windows의 고객 환경을 손상시키지 않으면서 대규모 파일 시스템 작업에 반응할 수 있습니다.

### <a name="changes-to-a-storagelibrary"></a>StorageLibrary 변경

[**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) 클래스는 다른 폴더를 포함하는 루트 폴더의 가상 그룹으로 존재합니다. 파일 시스템 변경 추적 장치에 맞춰 조정하려면 다음 항목을 선택합니다.

- 루트 라이브러리 폴더의 하위 항목을 변경하면 변경 내용이 변경 추적 장치에 표시됩니다. 루트 라이브러리 폴더는 [**Folders**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) 속성을 사용하여 찾을 수 있습니다.
- **StorageLibrary**([**RequestAddFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync) 및 [**RequestRemoveFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)를 통해)에서 루트 폴더를 추가 또는 제거해도 변경 추적 장치에서 항목이 생성되지 않습니다. 이러한 변경 내용은 [**DefinitionChanged**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged) 이벤트를 통해 또는 [**Folders**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) 속성을 사용하여 라이브러리의 루트 폴더를 열거하여 추적할 수 있습니다.
- 라이브러리에 이미 있는 폴더를 라이브러리에 추가하면 변경 알림이 발생하지 않으며 변경 추적 장치 항목도 생성되지 않습니다. 나중에 해당 폴더의 하위 항목이 변경되면 알림 및 변경 추적 장치 항목이 생성됩니다.

### <a name="calling-the-enable-method"></a>Enable 메서드 호출

앱은 파일 시스템 추적을 시작하는 즉시, 변경 내용이 모두 열거되기 전에 해야 [**Enable**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable)을 호출해야 합니다. 이렇게 하면 변경 추적 장치가 모든 변경 내용을 캡처합니다.  

## <a name="putting-it-together"></a>통합

비디오 라이브러리의 변경 내용을 등록하고 변경 추적 장치에서 변경 내용을 끌어오는 데 사용하는 모든 코드는 다음과 같습니다.

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
