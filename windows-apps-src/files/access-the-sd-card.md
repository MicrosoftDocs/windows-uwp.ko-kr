---
author: laurenhughes
ms.assetid: CAC6A7C7-3348-4EC4-8327-D47EB6E0C238
title: SD 카드에 액세스
description: 중요하지 않은 데이터는 옵션인 microSD 카드에 저장하고 액세스할 수 있습니다. 특히 내부 저장 용량이 제한적인 저가대의 장치에서는 이 기능이 유용합니다.
ms.author: lahugh
ms.date: 03/08/2017
ms.topic: article
keywords: windows 10, uwp, sd 카드, 저장소
ms.localizationpriority: medium
ms.openlocfilehash: 498b43dc82100102c90fc7a920bed1538a164afc
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6648619"
---
# <a name="access-the-sd-card"></a>SD 카드에 액세스



중요하지 않은 데이터는 옵션인 microSD 카드에 저장하고 액세스할 수 있습니다. 특히 내부 저장 용량이 제한적인 저가대의 장치에 SD 카드 슬롯이 있을 때 이 기능이 유용합니다.

앱에서 SD 카드에 파일을 저장하고 액세스하려면 먼저 앱 매니페스트 파일에서 **removableStorage** 접근 권한 값을 지정해야 합니다. 일반적으로 앱이 저장하고 액세스하는 파일의 형식을 처리하려면 등록해야 합니다.

다음 방법을 사용하여 선택 사항인 SD 카드에 파일을 저장하고 액세스할 수 있습니다.
- 파일 선택기
- [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) API

## <a name="what-you-can-and-cant-access-on-the-sd-card"></a>SD 카드에서 액세스할 수 있는 항목 및 액세스할 수 없는 항목

### <a name="what-you-can-access"></a>액세스할 수 있는 항목

- 앱은 앱 매니페스트 파일에서 앱이 처리하는 것으로 등록되어 있는 파일 형식의 파일만 읽고 쓸 수 있습니다.
- 앱은 폴더를 만들고 관리할 수도 있습니다.

### <a name="what-you-cant-access"></a>액세스할 수 없는 항목

- 앱은 시스템 폴더 및 이 폴더에 있는 파일을 표시하거나 액세스할 수 없습니다.
- 앱은 Hidden 특성으로 표시된 파일을 볼 수 없습니다. Hidden 특성은 일반적으로 데이터를 실수로 삭제하는 위험을 줄이는 데 사용됩니다.
- 앱은 [**KnownFolders.DocumentsLibrary**](https://msdn.microsoft.com/library/windows/apps/br227152)를 사용하여 문서 라이브러리를 보거나 액세스할 수 없습니다. 그러나 파일 시스템을 트래버스하여 SD 카드의 문서 라이브러리에 액세스할 수 있습니다.

## <a name="security-and-privacy-considerations"></a>보안 및 개인 정보 설정 고려 사항

앱이 SD 카드의 전역 위치에 파일을 저장하면 해당 파일은 암호화되지 않으므로 일반적으로 다른 앱에서 파일에 액세스할 수 있습니다.

- SD 카드가 장치에 있는 동안에는 동일한 파일 형식을 처리하도록 등록되어 있는 다른 앱에서 해당 파일에 액세스할 수 있습니다.
- SD 카드를 장치에서 빼고 PC에서 열면 파일을 파일 탐색기에서 볼 수 있으며 다른 앱에서 액세스할 수 있습니다.

SD 카드에 설치된 앱이 [**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621)에 파일을 저장하면 해당 파일은 암호화되고 다른 앱에서 파일에 액세스할 수 없습니다.

## <a name="requirements-for-accessing-files-on-the-sd-card"></a>SD 카드에 있는 파일에 액세스하기 위한 요구 사항

SD 카드에 있는 파일에 액세스하려면 일반적으로 다음 항목을 지정해야 합니다.

1.  앱 매니페스트 파일에서 **removableStorage** 기능을 지정해야 합니다.
2.  액세스할 미디어 형식과 관련된 파일 확장명을 처리하려면 등록해야 합니다.

**KnownFolders.MusicLibrary**과 같이 알려진 폴더를 참조하지 않고도 SD 카드에 있는 미디어 파일에 액세스하거나 미디어 라이브러리 폴더 외부에 저장된 미디어 파일에 액세스하려면 이전 메서드를 사용합니다.

알려진 폴더를 사용하여 미디어 라이브러리에 저장된 미디어 파일(음악, 사진 또는 동영상)에 액세스하려면 앱 매니페스트 파일에 관련 기능(**musicLibrary**, **picturesLibrary** 또는 **videoLibrary**)을 지정하기만 하면 됩니다. **removableStorage** 접근 권한 값은 지정하지 않아도 됩니다. 자세한 내용은 [음악, 사진 및 비디오 라이브러리의 파일 및 폴더](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)를 참조하세요.

## <a name="accessing-files-on-the-sd-card"></a>SD 카드에 있는 파일에 액세스

### <a name="getting-a-reference-to-the-sd-card"></a>SD 카드에 대한 참조 가져오기

[**KnownFolders.RemovableDevices**](https://msdn.microsoft.com/library/windows/apps/br227158) 폴더는 현재 장치에 연결되어 있는 이동식 장치에 대한 논리적 루트 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)입니다. SD 카드가 있는 경우 **KnownFolders.RemovableDevices** 폴더 아래에 있는 첫 번째이자 유일한 **StorageFolder**가 SD 카드를 나타냅니다.

SD 카드가 있는지 여부를 확인하고 이 카드에 대한 참조를 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)로 가져오려면 다음과 같이 코드를 사용합니다.

```csharp
using Windows.Storage;

// Get the logical root folder for all external storage devices.
StorageFolder externalDevices = Windows.Storage.KnownFolders.RemovableDevices;

// Get the first child folder, which represents the SD card.
StorageFolder sdCard = (await externalDevices.GetFoldersAsync()).FirstOrDefault();

if (sdCard != null)
{
    // An SD card is present and the sdCard variable now contains a reference to it.
}
else
{
    // No SD card is present.
}
```

> [!NOTE]
> SD 카드 판독기가 내장 판독기(예: 노트북이나 PC 자체의 슬롯)인 경우, KnownFolders.RemovableDevices를 통해 액세스할 수 없습니다.

### <a name="querying-the-contents-of-the-sd-card"></a>SD 카드의 콘텐츠 쿼리

SD 카드는 알려진 폴더로 인식되지 않는 파일과 많은 폴더를 포함할 수 있으며 [**KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151)에서 위치를 사용하여 쿼리할 수 없습니다. 파일을 찾으려면 앱이 파일 시스템을 재귀적으로 트래버스하여 카드의 콘텐츠를 열거해야 합니다. SD 카드의 콘텐츠를 효율적으로 가져오려면 [**GetFilesAsync (CommonFileQuery.DefaultQuery)**](https://msdn.microsoft.com/library/windows/apps/br227274) 및 [**GetFoldersAsync (CommonFolderQuery.DefaultQuery)**](https://msdn.microsoft.com/library/windows/apps/br227281)를 사용합니다.

백그라운드 스레드를 사용하여 SD 카드를 트래버스하는 것이 좋습니다. SD 카드는 수기가바이트의 데이터를 포함할 수 있습니다.

사용자에게 폴더 선택기를 사용하여 특정 폴더를 선택하도록 요청하는 메시지가 앱에서 표시될 수 있습니다.

[**KnownFolders.RemovableDevices**](https://msdn.microsoft.com/library/windows/apps/br227158)에서 파생된 경로를 사용하여 SD 카드에 있는 파일 시스템에 액세스하는 경우, 다음 메서드는 다음과 같은 방법으로 동작합니다.

-   [**GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227273) 메서드는 사용자가 처리하기 위해 등록한 파일 확장명과 사용자가 지정한 모든 미디어 라이브러리 접근 권한 값과 관련된 파일 확장명의 합집합을 반환합니다.
-   액세스하려는 파일의 파일 확장명을 처리하도록 등록하지 않은 경우 [**GetFileFromPathAsync**](https://msdn.microsoft.com/library/windows/apps/br227206) 메서드가 실패합니다.

## <a name="identifying-the-individual-sd-card"></a>개별 SD 카드 식별

SD 카드를 처음 끼우면 운영 체제에서 카드의 고유 식별자를 생성합니다. 이 ID는 카드 루트의 WPSystem 폴더에 있는 파일에 저장됩니다. 앱은 이 ID를 사용하여 카드 식별 여부를 결정합니다. 앱이 카드를 인식하는 경우 앱은 이전에 완료된 특정 작업을 지연할 수 있습니다. 하지만 앱에서 마지막으로 카드에 액세스한 이후로 카드의 콘텐츠가 변경되었을 수 있습니다.

예를 들어 전자책을 인덱싱하는 앱을 생각해 보겠습니다. 앱이 이전에 전체 SD 카드에서 전자책 파일을 검사하고 전자책의 색인을 만든 경우에는 카드를 다시 끼우고 앱이 카드를 인식하면 즉시 목록을 표시할 수 있습니다. 별도로, 우선 순위가 낮은 백그라운드 스레드를 시작하여 새로운 전자책을 검색할 수 있습니다. 또한 오류를 처리함으로써, 사용자가 삭제된 전자책에 액세스하려고 하면 이전에 있던 전자책을 찾을 수도 있습니다.

이 ID가 포함된 속성의 이름은 **WindowsPhone.ExternalStorageId**입니다.

```csharp
using Windows.Storage;

// Get the logical root folder for all external storage devices.
StorageFolder externalDevices = Windows.Storage.KnownFolders.RemovableDevices;

// Get the first child folder, which represents the SD card.
StorageFolder sdCard = (await externalDevices.GetFoldersAsync()).FirstOrDefault();

if (sdCard != null)
{
    var allProperties = sdCard.Properties;
    IEnumerable<string> propertiesToRetrieve = new List<string> { "WindowsPhone.ExternalStorageId" };

    var storageIdProperties = await allProperties.RetrievePropertiesAsync(propertiesToRetrieve);

    string cardId = (string)storageIdProperties["WindowsPhone.ExternalStorageId"];

    if (...) // If cardID matches the cached ID of a recognized card.
    {
        // Card is recognized. Index contents opportunistically.
    }
    else
    {
        // Card is not recognized. Index contents immediately.
    }
}
```

 

 
