---
author: laurenhughes
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: 파일 속성 가져오기
description: StorageFile 개체로 표시되는 파일의 최상위, 기본 및 확장 속성을 가져옵니다.
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8fc44300376efb5b56f390457e516f35a3ec4202
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5695924"
---
# <a name="get-file-properties"></a>파일 속성 가져오기



**중요 API**

-   [**StorageFile.GetBasicPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh701737)
-   [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh770652)

[**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 개체로 표시되는 파일의 최상위, 기본 및 확장 속성을 가져옵니다.

> [!NOTE]
> [파일 액세스 샘플](http://go.microsoft.com/fwlink/p/?linkid=619995)도 참조하세요.

 


## <a name="prerequisites"></a>필수 조건

-   **UWP(유니버설 Windows 플랫폼) 앱에 대한 비동기 프로그래밍 이해**

    C# 또는 Visual Basic에서 비동기 앱을 작성하는 방법에 대한 자세한 내용은 [C# 또는 Visual Basic에서 비동기식 API 호출](https://msdn.microsoft.com/library/windows/apps/mt187337)을 참조하세요. C++에서 비동기 앱을 작성하는 방법은 [C++의 비동기 프로그래밍](https://msdn.microsoft.com/library/windows/apps/mt187334)을 참조하세요.

-   **위치에 대한 액세스 권한**

    예를 들어 이 예제의 코드에서는 **picturesLibrary** 기능이 필요하지만 사용자 위치에는 다른 기능이 필요하거나 아무 기능도 필요하지 않을 수 있습니다. 자세한 내용은 [파일 액세스 권한](file-access-permissions.md)을 참조하세요.

## <a name="getting-a-files-top-level-properties"></a>파일의 최상위 속성 가져오기

많은 최상위 파일 속성은 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 클래스의 구성원으로 액세스할 수 있습니다. 이러한 속성에는 파일 특성, 콘텐츠 형식, 만든 날짜, 표시 이름, 파일 형식 등이 포함됩니다.

**참고**을 **picturesLibrary** 접근 권한 값을 선언 해야 합니다.

 

다음 예제에서는 사진 라이브러리의 모든 파일을 열거하고 각 파일의 몇 가지 최상위 속성에 액세스합니다.

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get top-level file properties.
    fileProperties.AppendLine("File name: " + file.Name);
    fileProperties.AppendLine("File type: " + file.FileType);
}
```

## <a name="getting-a-files-basic-properties"></a>파일의 기본 속성 가져오기

많은 기본 파일 속성은 먼저 [**StorageFile.GetBasicPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh701737) 메서드를 호출하여 가져옵니다. 이 메서드는 [**BasicProperties**](https://msdn.microsoft.com/library/windows/apps/br212113) 개체를 반환하며, 이 개체는 항목이 마지막으로 수정된 시간뿐만 아니라 항목(파일 또는 폴더)의 크기에 대한 속성을 정의합니다.

다음 코드에서는 사진 라이브러리의 모든 파일을 열거하고 각 파일의 몇 가지 기본 속성에 액세스합니다.

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get file's basic properties.
    Windows.Storage.FileProperties.BasicProperties basicProperties =
        await file.GetBasicPropertiesAsync();
    string fileSize = string.Format("{0:n0}", basicProperties.Size);
    fileProperties.AppendLine("File size: " + fileSize + " bytes");
    fileProperties.AppendLine("Date modified: " + basicProperties.DateModified);
}
 ```

## <a name="getting-a-files-extended-properties"></a>파일의 확장 속성 가져오기

최상위 및 기본 파일 속성 외에도 파일의 내용과 연결된 많은 속성이 있습니다. 이러한 확장 속성은 [**BasicProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br212124) 메서드를 호출하여 액세스합니다. [**BasicProperties**](https://msdn.microsoft.com/library/windows/apps/br212113) 개체는 [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225) 속성을 호출하여 가져옵니다. 최상위 및 기본 파일 속성은 각각 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 및 **BasicProperties** 클래스의 속성으로 액세스할 수 있지만 확장 속성은 검색할 속성의 이름을 나타내는 [String](http://go.microsoft.com/fwlink/p/?LinkID=325032) 개체의 [IEnumerable](http://go.microsoft.com/fwlink/p/?LinkID=313091) 컬렉션을 **BasicProperties.RetrievePropertiesAsync** 메서드로 전달하여 가져옵니다. 그러면 이 메서드에서 [IDictionary](http://go.microsoft.com/fwlink/p/?LinkId=325238) 컬렉션을 반환합니다. 그러면 컬렉션에서 이름 또는 인덱스로 각 확장 속성을 검색합니다.

다음 예제에서는 사진 라이브러리의 모든 파일을 열거하고 [List](http://go.microsoft.com/fwlink/p/?LinkID=325246) 개체에서 원하는 속성(**DataAccessed** 및 **FileOwner**)의 이름을 지정한 다음 해당 [List](http://go.microsoft.com/fwlink/p/?LinkID=325246) 개체를 [**BasicProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br212124)에 전달하여 해당 속성을 검색하고 반환된 [IDictionary](http://go.microsoft.com/fwlink/p/?LinkId=325238) 개체에서 이름으로 해당 속성을 검색합니다.

파일의 확장 속성 전체 목록은 [Windows 핵심 속성](https://msdn.microsoft.com/library/windows/desktop/mt805470)을 참조하세요.

```csharp
const string dateAccessedProperty = "System.DateAccessed";
const string fileOwnerProperty = "System.FileOwner";

// Enumerate all files in the Pictures library.
var folder = KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Define property names to be retrieved.
    var propertyNames = new List<string>();
    propertyNames.Add(dateAccessedProperty);
    propertyNames.Add(fileOwnerProperty);

    // Get extended properties.
    IDictionary<string, object> extraProperties =
        await file.Properties.RetrievePropertiesAsync(propertyNames);

    // Get date-accessed property.
    var propValue = extraProperties[dateAccessedProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("Date accessed: " + propValue);
    }

    // Get file-owner property.
    propValue = extraProperties[fileOwnerProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("File owner: " + propValue);
    }
}
```

 

 
