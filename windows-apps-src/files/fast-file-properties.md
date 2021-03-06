---
title: UWP의 파일 속성에 빠르게 액세스
description: UWP 앱에서 사용할 라이브러리에서 파일 및 해당 속성의 목록을 효율적으로 수집합니다.
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp, 파일, 속성
ms.localizationpriority: medium
ms.openlocfilehash: 75d835a5f7de6fd26e6a05a80b5f47a8fe0c0e1b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173227"
---
# <a name="fast-access-to-file-properties-in-uwp"></a>UWP의 파일 속성에 빠르게 액세스 

라이브러리에서 파일 및 속성 목록을 신속하게 수집하고 앱에서 해당 속성을 사용하는 방법에 대해 알아봅니다.  

필수 구성 요소 
- **UWP(유니버설 Windows 플랫폼) 앱에 대한 비동기 프로그래밍**   C# 또는 Visual Basic에서 비동기 앱을 작성하는 방법에 대한 자세한 내용은 [C# 또는 Visual Basic에서 비동기 API 호출](../threading-async/call-asynchronous-apis-in-csharp-or-visual-basic.md)을 참조하세요. C++에서 비동기 앱을 작성하는 방법은 [C++의 비동기 프로그래밍](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md)을 참조하세요. 
- **라이브러리에 대한 액세스 권한**   이 예제의 코드에는 **picturesLibrary** 기능이 필요하지만, 파일 위치에는 다른 기능이 필요하거나 기능이 전혀 필요하지 않을 수 있습니다. 자세한 내용은 [파일 액세스 권한](./file-access-permissions.md)을 참조하세요. 
- **단순 파일 열거**    이 예제에서는 [QueryOptions](/uwp/api/Windows.Storage.Search.QueryOptions)를 사용하여 몇 가지 고급 열거 속성을 설정합니다. 작은 디렉터리에 대한 단순 파일 목록을 가져오는 방법에 대한 자세한 내용은 [파일 및 폴더 열거 및 쿼리](./quickstart-listing-files-and-folders.md)를 참조하세요. 

## <a name="usage"></a>사용  
많은 경우 앱이 파일 그룹의 속성을 나열해야 하지만 항상 파일과 직접 상호 작용할 필요는 없습니다. 예를 들어, 뮤직 앱은 한 번에 하나의 파일을 재생하지만(열지만), 앱이 곡 대기열을 표시하거나 사용자가 유효한 재생 파일을 선택할 수 있도록 하기 위해 폴더의 모든 파일에 대한 속성이 필요합니다. 

모든 파일의 메타데이터를 수정할 앱 또는 속성을 읽지 않고 모든 결과 StorageFiles와 상호 작용하는 앱에 이 페이지의 예제를 사용해서는 안 됩니다. 자세한 내용은 [파일 및 폴더 열거 및 쿼리](./quickstart-listing-files-and-folders.md)를 참조하세요. 

## <a name="enumerate-all-the-pictures-in-a-location"></a>한 위치에 모든 사진 열거 
이 예제에서는
-  [QueryOptions](/uwp/api/Windows.Storage.Search.QueryOptions) 개체를 빌드하여 앱이 가능한 빨리 파일을 열거하도록 지정합니다.
-  StorageFile 개체를 앱으로 페이징하여 파일 속성을 가져옵니다. 파일을 페이징하면 앱에서 사용하는 메모리가 줄고 응답 속도가 빨라집니다.

### <a name="creating-the-query"></a>쿼리 만들기 
쿼리를 작성하기 위해, QueryOptions 개체를 사용하여 앱이 특정 유형의 이미지 파일만 열거하고 Windows Information Protection(System.Security.EncryptionOwners)으로 보호된 파일을 필터링하도록 지정합니다. 

[QueryOptions.SetPropertyPrefetch](/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch)를 사용하여 앱이 액세스할 속성을 설정하는 것이 중요합니다. 앱이 프리페치되지 않은 속성에 액세스할 경우 상당한 성능 저하가 발생합니다.

[IndexerOption.OnlyUseIndexerAndOptimzeForIndexedProperties](/uwp/api/Windows.Storage.Search.IndexerOption)를 설정하면 결과를 가능한 빨리 반환하되 [SetPropertyPrefetch](/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch)에 지정된 속성만 포함하도록 지정합니다. 

### <a name="paging-in-the-results"></a>결과 페이징 
사용자가 사진 라이브러리에 수천, 수백만 개의 파일을 가지고 있을 수 있으므로 [GetFilesAsync](/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync)를 호출하면 각 이미지에 해당하는 StorageFile이 만들어져 시스템에 부담이 될 것입니다. 한 번에 고정된 개수의 StorageFile을 생성하고 UI로 처리한 다음, 메모리를 해제하면 이 문제를 해결할 수 있습니다. 

이 예제에서는 [StorageFileQueryResult.GetFilesAsync(UInt32 StartIndex, UInt32 maxNumberOfItems)](/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync)를 사용하여 한 번에 100개의 파일만 가져옵니다. 그러면 앱이 파일을 처리하고 나중에 OS가 해당 메모리를 해제할 수 있습니다. 이 기법은 앱의 최대 메모리 상한을 지정하고, 시스템이 응답을 유지하도록 합니다. 물론 시나리오에 반환되는 파일 수를 조정해야 하지만 모든 사용자에게 신속한 응답을 제공하려면 한 번에 500개가 넘는 파일을 가져오지 않는 것이 좋습니다.


**예제**  
```csharp
StorageFolder folderToEnumerate = KnownFolders.PicturesLibrary; 
// Check if the folder is indexed before doing anything. 
IndexedState folderIndexedState = await folderToEnumerate.GetIndexedStateAsync(); 
if (folderIndexedState == IndexedState.NotIndexed || folderIndexedState == IndexedState.Unknown) 
{ 
    // Only possible in indexed directories.  
    return; 
} 
 
QueryOptions picturesQuery = new QueryOptions() 
{ 
    FolderDepth = FolderDepth.Deep, 
    // Filter out all files that have WIP enabled
    ApplicationSearchFilter = "System.Security.EncryptionOwners:[]", 
    IndexerOption = IndexerOption.OnlyUseIndexerAndOptimizeForIndexedProperties 
}; 

picturesQuery.FileTypeFilter.Add(".jpg"); 
string[] otherProperties = new string[] 
{ 
    SystemProperties.GPS.LatitudeDecimal, 
    SystemProperties.GPS.LongitudeDecimal 
}; 
 
picturesQuery.SetPropertyPrefetch(PropertyPrefetchOptions.BasicProperties | PropertyPrefetchOptions.ImageProperties, 
                                    otherProperties); 
SortEntry sortOrder = new SortEntry() 
{ 
    AscendingOrder = true, 
    PropertyName = "System.FileName" // FileName property is used as an example. Any property can be used here.  
}; 
picturesQuery.SortOrder.Add(sortOrder); 
 
// Create the query and get the results 
uint index = 0; 
const uint stepSize = 100; 
if (!folderToEnumerate.AreQueryOptionsSupported(picturesQuery)) 
{ 
    log("Querying for a sort order is not supported in this location"); 
    picturesQuery.SortOrder.Clear(); 
} 
StorageFileQueryResult queryResult = folderToEnumerate.CreateFileQueryWithOptions(picturesQuery); 
IReadOnlyList<StorageFile> images = await queryResult.GetFilesAsync(index, stepSize); 
while (images.Count != 0 || index < 10000) 
{ 
    foreach (StorageFile file in images) 
    { 
        // With the OnlyUseIndexerAndOptimizeForIndexedProperties set, this won't  
        // be async. It will run synchronously. 
        var imageProps = await file.Properties.GetImagePropertiesAsync(); 
 
        // Build the UI 
        log(String.Format("{0} at {1}, {2}", 
                    file.Path, 
                    imageProps.Latitude, 
                    imageProps.Longitude)); 
    } 
    index += stepSize; 
    images = await queryResult.GetFilesAsync(index, stepSize); 
} 
```

### <a name="results"></a>결과 
결과로 생성된 StorageFile 파일에는 요청된 속성만 포함되지만 다른 IndexerOptions에 비해 10배 빠릅니다. 앱은 쿼리에 아직 포함되지 않은 속성에 대한 액세스도 요청할 수 있지만 파일을 열고 속성을 검색할 때 성능이 저하될 수 있습니다.  

## <a name="adding-folders-to-libraries"></a>라이브러리에 폴더 추가 
앱은 [StorageLibrary.RequestAddFolderAsync](/uwp/api/Windows.Storage.StorageLibrary.RequestAddFolderAsync)를 통해 사용자에게 색인에 위치를 추가하도록 요청할 수 있습니다. 위치가 포함되면 자동으로 인덱싱되고 앱은 이 방법을 사용하여 파일을 열거할 수 있습니다.
 
## <a name="see-also"></a>참고 항목
[QueryOptions API 참조](/uwp/api/windows.storage.search.queryoptions)  
[파일과 폴더 열거 및 쿼리](./quickstart-listing-files-and-folders.md)  
[파일 액세스 권한](./file-access-permissions.md)  
 
 