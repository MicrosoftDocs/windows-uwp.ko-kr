---
ms.assetid: BF929A68-9C82-4866-BC13-A32B3A550005
title: 최근에 사용한 파일 및 폴더 추적
description: 사용자가 자주 액세스하는 파일을 앱의 MRU(최근에 사용한 목록)에 추가하여 추적할 수 있습니다.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 21e0516e3de4085e9ad2f3b67cb5ed497d907f5c
ms.sourcegitcommit: 1cf708443d132306e6c99027662de8ec99177de6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/20/2018
ms.locfileid: "8980371"
---
# <a name="track-recently-used-files-and-folders"></a>최근에 사용한 파일 및 폴더 추적

**중요 API**

- [**MostRecentlyUsedList**](https://msdn.microsoft.com/library/windows/apps/br207458)
- [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/hh738369)

사용자가 자주 액세스하는 파일을 앱의 MRU(최근에 사용한 목록)에 추가하여 추적할 수 있습니다. 플랫폼은 마지막으로 액세스한 시간을 기반으로 항목을 정렬하고 목록의 25개 항목 제한에 도달한 경우 가장 오래된 항목을 제거하여 MRU를 자동으로 관리합니다. 모든 앱에는 자체 MRU가 있습니다.

앱의 MRU는 정적 [**StorageApplicationPermissions.MostRecentlyUsedList**](https://msdn.microsoft.com/library/windows/apps/br207458) 속성에서 가져오는 [**StorageItemMostRecentlyUsedList**](https://msdn.microsoft.com/library/windows/apps/br207475) 클래스로 표현됩니다. MRU 항목은 [**IStorageItem**](https://msdn.microsoft.com/library/windows/apps/br227129) 개체로 저장되므로 파일을 나타내는 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 개체와 폴더를 나타내는 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 개체를 모두 MRU에 추가할 수 있습니다.

> [!NOTE]
> 전체 샘플 [파일 선택기 샘플](http://go.microsoft.com/fwlink/p/?linkid=619994) 및 [파일 액세스 샘플](http://go.microsoft.com/fwlink/p/?linkid=619995)을 참조 하세요.

## <a name="prerequisites"></a>필수 조건

-   **UWP(유니버설 Windows 플랫폼) 앱에 대한 비동기 프로그래밍 이해**

    C# 또는 Visual Basic에서 비동기 앱을 작성하는 방법에 대한 자세한 내용은 [C# 또는 Visual Basic에서 비동기식 API 호출](https://msdn.microsoft.com/library/windows/apps/mt187337)을 참조하세요. C++에서 비동기 앱을 작성하는 방법은 [C++의 비동기 프로그래밍](https://msdn.microsoft.com/library/windows/apps/mt187334)을 참조하세요.

-   **위치에 대한 액세스 권한**

    [파일 액세스 권한](file-access-permissions.md)을 참조하세요.

-   [선택기를 사용하여 파일 및 폴더 열기](quickstart-using-file-and-folder-pickers.md)

    선택한 파일은 종종 사용자가 반복해서 열어 보는 파일과 동일한 파일입니다.

 ## <a name="add-a-picked-file-to-the-mru"></a>MRU에 선택한 파일 추가

-   사용자가 선택하는 파일은 해당 사용자가 반복적으로 돌아가는 파일인 경우가 많습니다. 따라서 사용자가 파일을 선택하는 즉시 해당 파일을 앱의 MRU에 추가하는 것이 좋습니다. 방법은 다음과 같습니다.

    ```cs
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();

    var mru = Windows.Storage.AccessCache.StorageApplicationPermissions.MostRecentlyUsedList;
    string mruToken = mru.Add(file, "profile pic");
    ```

    [**StorageItemMostRecentlyUsedList.Add**](https://msdn.microsoft.com/library/windows/apps/br207476)가 오버로드됩니다. 이 예제에서는 메타데이터를 파일과 연결할 수 있도록 [**Add(IStorageItem, String)**](https://msdn.microsoft.com/library/windows/apps/br207481)를 사용합니다. 메타데이터를 설정하면 항목의 용도(예: "프로필 사진")를 기록할 수 있습니다. [**Add(IStorageItem)**](https://msdn.microsoft.com/library/windows/apps/br207480)을 호출하면 메타데이터 없이 파일을 MRU에 추가할 수 있습니다. 항목을 MRU에 추가한 경우 메서드는 항목을 검색하는 데 사용되는 고유하게 식별되는 문자열(토큰)을 반환합니다.

> [!TIP]
> MRU에서 항목을 검색하려면 토큰이 필요하므로 다른 곳에 유지해야 합니다. 앱 데이터에 대한 자세한 내용은 [응용 프로그램 데이터 관리](https://msdn.microsoft.com/library/windows/apps/hh465109)를 참조하세요.

## <a name="use-a-token-to-retrieve-an-item-from-the-mru"></a>토큰을 사용하여 MRU에서 항목 검색

검색하려는 항목에 가장 적합한 검색 방법을 사용합니다.

-   파일은 [**GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br207486)를 사용하여 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)로 검색합니다.
-   폴더는 [**GetFolderAsync**](https://msdn.microsoft.com/library/windows/apps/br207489)를 사용하여 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)로 검색합니다.
-   파일 또는 폴더를 나타낼 수 있는 제네릭 [**IStorageItem**](https://msdn.microsoft.com/library/windows/apps/br227129)은 [**GetItemAsync**](https://msdn.microsoft.com/library/windows/apps/br207492)를 사용하여 검색합니다.

방금 추가한 파일로 돌아가는 방법은 다음과 같습니다.

```cs
StorageFile retrievedFile = await mru.GetFileAsync(mruToken);
```

모든 항목을 반복하여 토큰과 항목을 차례로 가져오는 방법은 다음과 같습니다.

```cs
foreach (Windows.Storage.AccessCache.AccessListEntry entry in mru.Entries)
{
    string mruToken = entry.Token;
    string mruMetadata = entry.Metadata;
    Windows.Storage.IStorageItem item = await mru.GetItemAsync(mruToken);
    // The type of item will tell you whether it's a file or a folder.
}
```

[**AccessListEntryView**](https://msdn.microsoft.com/library/windows/apps/br227349)를 사용하면 MRU에서 항목을 반복할 수 있습니다. 이러한 항목은 항목에 대한 토큰과 메타데이터가 포함된 [**AccessListEntry**](https://msdn.microsoft.com/library/windows/apps/br227348) 구조입니다.

## <a name="removing-items-from-the-mru-when-its-full"></a>가득 찬 경우 MRU에서 항목 제거

MRU의 25개 항목 제한에 도달한 후 새 항목을 추가하려고 하면 가장 오래 전에 액세스한 항목이 자동으로 제거됩니다. 따라서 새 항목을 추가하기 전에 항목을 제거할 필요가 없습니다.

## <a name="future-access-list"></a>향후 액세스 목록

MRU뿐만 아니라 앱에는 향후 액세스 목록도 있습니다. 파일 및 폴더를 선택하여 사용자는 액세스하지 못할 수 있는 항목에 대한 액세스 권한을 앱에 부여할 수 있습니다. 이러한 항목을 향후 액세스 목록에 추가하면 나중에서 앱이 이러한 항목에 다시 액세스하려는 경우 해당 권한이 유지됩니다. 앱의 향후 액세스 목록은 정적 [**StorageApplicationPermissions.FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) 속성에서 가져오는 [**StorageItemAccessList**](https://msdn.microsoft.com/library/windows/apps/br207459) 클래스로 표현됩니다.

사용자가 항목을 선택하면 MRU뿐만 아니라 향후 액세스 목록에도 추가하는 것이 좋습니다.

-   [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457)에는 최대 1000개의 항목을 저장할 수 있습니다. 파일뿐만 아니라 폴더도 유지할 수 있으므로 많은 폴더가 있을 수 있습니다.
-   플랫폼은 [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457)에서 항목을 자동으로 제거하지 않습니다. 1000개 항목 제한에 도달한 경우 [**Remove**](https://msdn.microsoft.com/library/windows/apps/br207497) 메서드로 공간을 확보할 때까지 다른 항목을 추가할 수 없습니다.
