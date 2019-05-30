---
ms.assetid: 3604524F-112A-474F-B0CA-0726DC8DB885
title: Microsoft OneDrive 파일의 가용성 확인
description: StorageFile.isAvailable 속성을 사용하여 Microsoft OneDrive 파일의 사용 가능 여부를 확인합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: effb28fa453ec884152dbc404245f00f4893ef5a
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369420"
---
# <a name="determining-availability-of-microsoft-onedrive-files"></a>Microsoft OneDrive 파일의 가용성 확인


**중요 한 Api**

-   [**FileIO 클래스**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
-   [**StorageFile 클래스**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)
-   [**StorageFile.IsAvailable 속성**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable)

[  **StorageFile.isAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable) 속성을 사용하여 Microsoft OneDrive 파일의 사용 가능 여부를 확인합니다.

## <a name="prerequisites"></a>사전 요구 사항

-   **유니버설 Windows 플랫폼 (UWP) 앱 용 비동기 프로그래밍 이해**

    C# 또는 Visual Basic에서 비동기 앱을 작성하는 방법에 대한 자세한 내용은 [C# 또는 Visual Basic에서 비동기식 API 호출](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic)을 참조하세요. C++에서 비동기 앱을 작성하는 방법은 [C++의 비동기 프로그래밍](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)을 참조하세요.

-   **앱 capabilty 선언**

    [파일 액세스 권한](file-access-permissions.md)을 참조하세요.

## <a name="using-the-storagefileisavailable-property"></a>StorageFile.IsAvailable 속성 사용

사용자는 OneDrive 파일을 '오프라인 사용 가능'(기본값) 또는 '온라인만'으로 표시할 수 있습니다. 이 기능을 통해 사용자는 큰 파일(예: 사진과 동영상)을 OneDrive로 이동하고 '온라인만'으로 표시하여 디스크 공간을 절약할 수 있습니다(메타데이터 파일만 로컬로 보관됨).

[**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable), 파일을 현재 사용할 수 있는지 확인 하는 데 사용 됩니다. 다음 표는 다양한 시나리오에서 **StorageFile.IsAvailable** 속성의 값을 보여 줍니다.

| 파일 형식                              | 온라인 | 데이터 통신 연결 네트워크        | 오프라인 |
|-------------------------------------------|--------|------------------------|---------|
| 로컬 파일                                | True   | True                   | True    |
| 오프라인 사용 가능으로 표시된 OneDrive 파일 | True   | True                   | True    |
| 온라인만으로 표시된 OneDrive 파일       | True   | 사용자 설정 기반 | False   |
| 네트워크 파일                              | True   | 사용자 설정 기반 | False   |

 

다음 단계에서는 파일이 현재 사용 가능한지를 확인하는 방법을 설명합니다.

1.  액세스하려는 라이브러리에 적절한 접근 권한 값을 선언합니다.
2.  [  **Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) 네임스페이스를 포함합니다. 이 네임스페이스에는 파일, 폴더 및 응용 프로그램 설정을 관리하기 위한 형식이 포함되어 있습니다. 또한 필요한 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 형식이 포함되어 있습니다.
3.  원하는 파일의 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 개체를 가져옵니다. 라이브러리를 열거하고 있는 경우 이 단계는 일반적으로 [**StorageFolder.CreateFileQuery**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfilequery) 메서드를 호출한 다음 결과 [**StorageFileQueryResult**](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.StorageFileQueryResult) 개체의 [**GetFilesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfilesasync) 메서드를 호출하여 수행됩니다. **GetFilesAsync** 메서드는 **StorageFile** 개체의 [IReadOnlyList](https://go.microsoft.com/fwlink/p/?LinkId=324970) 컬렉션을 반환합니다.
4.  원하는 파일을 나타내는 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 개체에 액세스할 수 있는 경우 [**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable) 속성의 값에는 파일의 사용 가능 여부가 반영됩니다.

다음 제네릭 메서드는 모든 폴더를 열거하고 해당 폴더에 대한 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 개체의 컬렉션을 반환하는 방법을 보여 줍니다. 그런 다음 호출한 메서드는 각 파일에 대한 [**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable) 속성을 참조하는 반환된 컬렉션에서 반복됩니다.

```cs
/// <summary>
/// Generic function that retrieves all files from the specified folder.
/// </summary>
/// <param name="folder">The folder to be searched.</param>
/// <returns>An IReadOnlyList collection containing the file objects.</returns>
async Task<System.Collections.Generic.IReadOnlyList<StorageFile>> GetLibraryFilesAsync(StorageFolder folder)
{
    var query = folder.CreateFileQuery();
    return await query.GetFilesAsync();
}

private async void CheckAvailabilityOfFilesInPicturesLibrary()
{
    // Determine availability of all files within Pictures library.
    var files = await GetLibraryFilesAsync(KnownFolders.PicturesLibrary);
    for (int i = 0; i < files.Count; i++)
    {
        StorageFile file = files[i];

        StringBuilder fileInfo = new StringBuilder();
        fileInfo.AppendFormat("{0} (on {1}) is {2}",
                    file.Name,
                    file.Provider.DisplayName,
                    file.IsAvailable ? "available" : "not available");
    }
}
```
