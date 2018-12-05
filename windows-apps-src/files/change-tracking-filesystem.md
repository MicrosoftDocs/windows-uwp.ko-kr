---
title: 백그라운드에서 파일 시스템 변경 내용을 추적합니다
description: 사용자가 시스템 중심으로 이동할 때 백그라운드에서 폴더 및 파일의 변경 내용을 추적 하는 방법을 설명 합니다.
ms.date: 11/20/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e90692753924572a932767b9c188ed6d24f94593
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8701865"
---
# <a name="track-file-system-changes-in-the-background"></a>백그라운드에서 파일 시스템 변경 내용을 추적합니다

[StorageLibraryChangeTracker](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker) 클래스를 사용자가 이동 하는 시스템 파일 및 폴더 변경을 추적할 수 있습니다. **StorageLibraryChangeTracker** 클래스를 사용 하는 앱이 추적할 수 있습니다.

- 파일 추가 포함 하 여 작업을 하 고, 삭제 하 고, 수정 합니다.
- 이름 바꾸기, 삭제 등의 작업 폴더입니다.
- 파일 및 폴더는 드라이브를 이동 합니다.

이 가이드를 사용 하 여 변경 추적기를 사용 하기 위한 프로그래밍 모델 배우고 보기 몇 가지 샘플 코드는 다양 한 유형의 **StorageLibraryChangeTracker**에서 추적 되는 파일 작업을 이해 합니다.

**StorageLibraryChangeTracker** 사용자 라이브러리 또는 폴더에 대 한 로컬 컴퓨터에서 작동합니다. 이 보조 드라이브 또는 이동식 드라이브를 포함 되지만 NAS 드라이브 또는 네트워크 드라이브 포함 되지 않습니다.

## <a name="using-the-change-tracker"></a>변경 추적기를 사용 하 여

변경 추적기 마지막 *N* 파일 시스템 작업과 저장 순환 버퍼로 시스템에서 구현 됩니다. 앱은 버퍼의 변경 내용을 읽고 자신의 환경으로 처리 합니다. 변경 내용을 처리할 때 변경 내용을 표시 하 고 하지는 완료 되 면 앱을 다시 표시 합니다.

폴더에 변경 추적기를 사용 하려면 다음이 단계를 따르세요.

1. 변경 폴더에 대 한 추적을 사용 하도록 설정 합니다.
2. 변경 될 때까지 기다립니다.
3. 읽기를 변경합니다.
4. 변경 내용을 적용 합니다.

다음 섹션에서는 각 코드 예제를 사용 하 여 단계를 안내합니다. 전체 코드 샘플은 문서의 끝에 제공 됩니다.

### <a name="enable-the-change-tracker"></a>변경 추적기를 사용 하도록 설정

먼저 앱 작업을 수행 해야 하는 지정 된 라이브러리를 추적 하는 변경 관심 시스템에 알립니다 하는 것입니다. 관심 라이브러리에 대 한 변경 추적 장치에 [활성화](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) 메서드를 호출 하 여 수행 합니다.

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

몇 가지 중요 한 참고 사항:

- 앱 매니페스트에 올바른 라이브러리 권한을 [StorageLibrary](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) 개체를 만들기 전에 있는지 확인 합니다. 자세한 내용은 [파일 액세스 권한](https://docs.microsoft.com/en-us/windows/uwp/files/file-access-permissions) 을 참조 하세요.
- [활성화](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) 는 스레드로부터 안전, 포인터를 다시 설정 되지 않습니다 하 고 (뒤에서 자세히) 원하는 만큼 호출할 수 있습니다.

![빈 변경 추적기를 사용 하도록 설정](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>대기 시간 변경

변경 추적 장치를 초기화 한 후 앱이 실행 되지 않을 때에, 라이브러리 내에서 발생 하는 작업의 모든 기록 시작 됩니다. 앱은 [StorageLibraryChangedTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger) 이벤트를 등록 하 여 변경 될 때마다 활성화할지를 등록할 수 있습니다.

![읽기 앱 없이 변경 추적기에 추가 되 고 변경](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>변경 내용을 읽기

앱 수 하 고 변경 추적기에서 변경 내용을 폴링합니다, 마지막으로 선택이 변경 내용 목록이 표시. 아래 코드 변경 내용 목록이 변경 추적기에서 다운로드 하는 방법을 보여 줍니다.

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

앱은 자체 환경 또는 필요에 따라 데이터베이스에 변경 내용을 처리를 담당 합니다.

![앱 데이터베이스에 변경 내용을 변경 추적기에서 읽기](images/changetracker-reading.png)

> [!TIP]
> 사용 하도록 설정 하려면 두 번째 호출 앱 읽고 변경 하는 동안 사용자 라이브러리에 다른 폴더를 추가 하는 경우는 경합 상태를 방어 하는 것입니다. **사용** 에 대 한 추가 호출 하지 않고 코드 실패 ecSearchFolderScopeViolation (0x80070490)를 사용 하 여 사용자가 해당 라이브러리에 폴더를 변경 하는 경우

### <a name="accept-the-changes"></a>변경 내용을 적용합니다

앱 완료 된 후 변경 내용을 처리 시스템 되지 [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) 메서드를 호출 하 여 변경 내용을 다시 표시 지시 해야 것입니다.

```csharp
await changeReader.AcceptChangesAsync();
```

![표시 되지 다시 읽기로 변경 내용을 표시](images/changetracker-accepting.png)

앱에서 새 변경만 받을 이제 변경 추적기를 나중에 읽을 때 합니다.

- 포인터 수 변경 [ReadBatchAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync) 및 [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync)호출 간의 가능성이 있는, 하는 경우 앱에 표시 되는 가장 최근 변경에 고급만 합니다. 해당 변경 내용은 계속 사용할 수 있는 **ReadBatchAsync**호출 다음에 합니다.
- 변경 내용을 받아들이지 앱 호출 **ReadBatchAsync**다음에 동일한 변경 집합을 반환 하는 시스템을 발생 합니다.

## <a name="important-things-to-remember"></a>유의 해야 할 중요 한 사항

변경 추적기를 사용할 때는 모든 제대로 작동 하는지 확인 하기 위해 반드시에서 유지 해야 하는 몇 가지 있습니다.

### <a name="buffer-overruns"></a>버퍼 오버런

변경 추적기 앱을 읽을 수 있을 때까지 시스템에서 발생 하는 모든 작업을 저장 하기에 충분 한 공간을 확보 하려고 하는 있지만 앱 자체 순환 버퍼 덮어쓰기 전에 변경 내용을 읽지 않습니다 시나리오도를 매우 쉽습니다. 특히 경우 사용자가 데이터를 백업에서 복원 하거나 큰 카메라 휴대폰에서 사진 컬렉션을 동기화 합니다.

이 경우 **ReadBatchAsync** 는 [StorageLibraryChangeType.ChangeTrackingLost](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype)오류 코드를 반환 합니다. 앱이 오류 코드를 받은 경우 몇 가지 의미 합니다.

* 버퍼에 마지막으로 보았던 자체를 덮어씁니다. 최선의 작업의 라이브러리를 탐색 하려면 점에서 추적기에서 모든 정보를 완료 됩니다.
* 변경 추적 장치 [재설정](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset)을 호출할 때까지 더 많은 변경 반환 하지 않습니다. 앱에서 호출 하는 초기화 후 포인터 가장 최근 변경으로 이동 됩니다 및 추적 정상적으로 다시 시작 됩니다.

이러한 경우를 가져오려는 경우 거의 하지만 사용자가 이동 하는 많은 수의 디스크에 파일의 시나리오에서 너무 많은 저장소 벗기 급증 변경 추적기 싶지는 않을 것입니다. 이 없는 Windows에서 고객 환경을 손상 하는 동안 대용량 파일 시스템 작업에 반응 하는 앱을 허용 해야 합니다.

### <a name="changes-to-a-storagelibrary"></a>StorageLibrary 변경

[StorageLibrary](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) 클래스가 다른 폴더를 포함 하는 루트 폴더의 가상 그룹으로 존재 합니다. 이것을 파일 시스템 변경 추적기로 조정 하려면 다음 옵션을 제공 합니다.

- 루트 라이브러리 폴더의 하위 폴더에 모든 변경 사항 변경 추적기에 표시 됩니다. [폴더](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) 속성을 사용 하 여 루트 라이브러리 폴더를 찾을 수 있습니다.
- 추가 [RequestAddFolderAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync) 및 [RequestRemoveFolderAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)) (통해 **StorageLibrary** 에서 루트 폴더를 제거 또는 변경 추적기에 항목을 만들지 않습니다. [DefinitionChanged](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged) 이벤트를 통해 또는 [폴더](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) 속성을 사용 하는 라이브러리의 루트 폴더를 열거 하 여 이러한 변경 내용은 추적할 수 있습니다.
- 폴더에 이미 콘텐츠가 포함 된 라이브러리에 추가 되 면 되지 않습니다 변경 알림 또는 변경 추적기 항목 생성. 해당 폴더의 하위 항목 변경 알림을 생성 하 고 추적기 항목 변경 하겠습니다.

### <a name="calling-the-enable-method"></a>Enable 메서드를 호출합니다.

추적 파일 시스템을 시작할 때 즉시를 모든 열거형 변경 하기 전에 [사용](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) 앱이 호출 해야 합니다. 이렇게 하면 모든 변경 내용을 추적기를 변경 하 여 캡처됩니다.  

## <a name="putting-it-together"></a>정리

비디오 라이브러리의 변경에 대 한 등록 하 고 변경 추적기에서 변경 내용을 끌어오기를 시작 하는 데 사용 되는 모든 코드는 다음과 같습니다.

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
