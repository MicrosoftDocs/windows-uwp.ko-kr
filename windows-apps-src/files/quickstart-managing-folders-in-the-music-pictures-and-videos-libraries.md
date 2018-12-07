---
ms.assetid: 1AE29512-7A7D-4179-ADAC-F02819AC2C39
title: 음악, 사진 및 비디오 라이브러리의 파일 및 폴더
description: 음악, 사진 또는 비디오의 기존 폴더를 해당 라이브러리에 추가합니다. 라이브러리에서 폴더를 제거하고, 라이브러리에 폴더 목록을 가져오고, 저장된 사진, 음악 및 동영상을 검색할 수도 있습니다.
ms.date: 06/18/2018
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8e04170fb8952ecd5802b6190816d44012f56d8a
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8796661"
---
# <a name="files-and-folders-in-the-music-pictures-and-videos-libraries"></a>음악, 사진 및 비디오 라이브러리의 파일 및 폴더

음악, 사진 또는 비디오의 기존 폴더를 해당 라이브러리에 추가합니다. 라이브러리에서 폴더를 제거하고, 라이브러리에 폴더 목록을 가져오고, 저장된 사진, 음악 및 동영상을 검색할 수도 있습니다.

라이브러리는 기본적으로 알려진 폴더와 사용자가 개발자의 앱 또는 기본 제공 앱 중 하나를 사용하여 라이브러리에 추가한 다른 폴더를 포함하는 가상 폴더 컬렉션입니다. 예를 들어 사진 라이브러리에는 기본적으로 알려진 사진 폴더가 포함되어 있습니다. 사용자는 개발자의 앱 또는 기본 제공 사진 앱을 사용하여 사진 라이브러리에서 폴더를 추가하거나 제거할 수 있습니다.

## <a name="prerequisites"></a>필수 조건


-   **UWP(유니버설 Windows 플랫폼) 앱에 대한 비동기 프로그래밍 이해**

    C# 또는 Visual Basic에서 비동기 앱을 작성하는 방법에 대한 자세한 내용은 [C# 또는 Visual Basic에서 비동기식 API 호출](https://msdn.microsoft.com/library/windows/apps/mt187337)을 참조하세요. C++에서 비동기 앱을 작성하는 방법은 [C++의 비동기 프로그래밍](https://msdn.microsoft.com/library/windows/apps/mt187334)을 참조하세요.

-   **위치에 대한 액세스 권한**

    Visual Studio의 매니페스트 디자이너에서 앱 매니페스트 파일을 엽니다. **기능** 페이지에서 앱이 관리하는 라이브러리를 선택합니다.

    -   **음악 라이브러리**
    -   **사진 라이브러리**
    -   **비디오 라이브러리**

    자세한 내용은 [파일 액세스 권한](file-access-permissions.md)을 참조하세요.

## <a name="get-a-reference-to-a-library"></a>라이브러리에 대한 참조 가져오기

> [!NOTE]
> 적절한 기능을 선언하는 것을 잊지 마세요. 자세한 내용은 [앱 기능 선언](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)을 참조하세요.
 

사용자의 음악, 그림, 비디오 라이브러리의 참조를 가져오려면 [**StorageLibrary.GetLibraryAsync**](https://msdn.microsoft.com/library/windows/apps/dn251725) 메서드를 호출합니다. [**KnownLibraryId**](https://msdn.microsoft.com/library/windows/apps/dn298399) 열거형에서 해당 값을 제공합니다.

-   [**KnownLibraryId.Music**](https://msdn.microsoft.com/library/windows/apps/br227155)
-   [**KnownLibraryId.Pictures**](https://msdn.microsoft.com/library/windows/apps/br227156)
-   [**KnownLibraryId.Videos**](https://msdn.microsoft.com/library/windows/apps/br227159)

```cs
var myPictures = await Windows.Storage.StorageLibrary.GetLibraryAsync(Windows.Storage.KnownLibraryId.Pictures);
```

## <a name="get-the-list-of-folders-in-a-library"></a>라이브러리에 폴더 목록 가져오기


라이브러리에 폴더 목록을 가져오려면 [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) 속성의 값을 가져옵니다.

```cs
using Windows.Foundation.Collections;
IObservableVector<Windows.Storage.StorageFolder> myPictureFolders = myPictures.Folders;
```

## <a name="get-the-folder-in-a-library-where-new-files-are-saved-by-default"></a>기본적으로 새 파일이 저장되는 폴더를 라이브러리에 가져오기


기본적으로 새 파일이 저장되는 폴더를 라이브러리에 가져오려면 [**StorageLibrary.SaveFolder**](https://msdn.microsoft.com/library/windows/apps/dn251728) 속성의 값을 가져옵니다.

```cs
Windows.Storage.StorageFolder savePicturesFolder = myPictures.SaveFolder;
```

## <a name="add-an-existing-folder-to-a-library"></a>라이브러리에 기존 폴더 추가

라이브러리에 폴더를 추가하려면 [**StorageLibrary.RequestAddFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251726)를 호출합니다. 예를 들어 사진 라이브러리의 경우 이 메서드를 호출하면 **사진에 이 폴더 추가** 단추가 포함된 폴더 선택기가 표시됩니다. 사용자가 폴더를 선택한 경우 폴더는 디스크의 원래 위치에 유지되고 [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) 속성(및 기본 제공 사진 앱)의 항목이 되지만 파일 탐색기의 사진 폴더 항목으로 표시되지는 않습니다.


```cs
Windows.Storage.StorageFolder newFolder = await myPictures.RequestAddFolderAsync();
```

## <a name="remove-a-folder-from-a-library"></a>라이브러리에서 폴더 제거

라이브러리에서 폴더를 제거하려면 [**StorageLibrary.RequestRemoveFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251727) 메서드를 호출하고 제거할 폴더를 지정합니다. [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) 및 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 컨트롤(또는 유사한 컨트롤)을 사용하여 사용자가 제거할 폴더를 선택하도록 할 수 있습니다.

[**StorageLibrary.RequestRemoveFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251727)를 호출한 경우 폴더가 “더 이상 사진에 표시되지 않지만 삭제되지 않음”을 나타내는 확인 대화 상자가 나타납니다. 이는 폴더가 디스크의 원래 위치에 유지되며, [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) 속성에서 제거되고 더 이상 기본 제공 사진 앱에 포함되지 않음을 의미합니다.

다음 예제에서는 사용자가 **lvPictureFolders**라는 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 컨트롤에서 제거할 폴더를 선택한 것으로 가정합니다.


```cs
bool result = await myPictures.RequestRemoveFolderAsync(folder);
```

## <a name="get-notified-of-changes-to-the-list-of-folders-in-a-library"></a>라이브러리의 폴더 목록 변경에 대한 알림 받기


라이브러리의 폴더 목록 변경에 대한 알림을 받으려면 라이브러리의 [**StorageLibrary.DefinitionChanged**](https://msdn.microsoft.com/library/windows/apps/dn251723) 이벤트에 대한 처리기를 등록합니다.


```cs
myPictures.DefinitionChanged += MyPictures_DefinitionChanged;

void HandleDefinitionChanged(Windows.Storage.StorageLibrary sender, object args)
{
    // ...
}
```

## <a name="media-library-folders"></a>미디어 라이브러리 폴더


장치에는 사용자 및 앱이 미디어 파일을 저장하도록 사전 정의된 5개의 위치가 있습니다. 기본 제공 앱은 사용자가 만들었거나 다운로드한 미디어를 모두 이러한 위치에 저장합니다.

위치는 다음과 같습니다.

-   **사진** 폴더. 사진이 있습니다.

    -   **카메라 앨범** 폴더. 기본 제공 카메라에서 촬영한 사진 및 동영상이 있습니다.

    -   **저장된 사진** 폴더. 사용자가 다른 앱에서 저장한 사진이 있습니다.

-   **음악** 폴더. 노래, 팟캐스트 및 오디오 책이 있습니다.

-   **비디오** 폴더. 비디오가 있습니다.

사용자 또는 앱이 SD 카드에 있는 미디어 라이브러리 폴더 외부에 미디어 파일을 저장할 수도 있습니다. SD 카드에서 안정적으로 미디어 파일을 찾으려면 SD 카드의 콘텐츠를 검사하거나 사용자에게 파일 선택기를 사용하여 파일 위치를 찾도록 요청합니다. 자세한 내용은 [SD 카드에 액세스](access-the-sd-card.md)를 참조하세요.

## <a name="querying-the-media-libraries"></a>미디어 라이브러리 쿼리

파일의 컬렉션을 가져오려면 원하는 파일 형식과 라이브러리를 지정합니다.

```cs
using Windows.Storage;
using Windows.Storage.Search;

private async void getSongs()
{
    QueryOptions queryOption = new QueryOptions
        (CommonFileQuery.OrderByTitle, new string[] { ".mp3", ".mp4", ".wma" });

    queryOption.FolderDepth = FolderDepth.Deep;

    Queue<IStorageFolder> folders = new Queue<IStorageFolder>();

    var files = await KnownFolders.MusicLibrary.CreateFileQueryWithOptions
      (queryOption).GetFilesAsync();

    foreach (var file in files)
    {
        // do something with the music files
    }
}
```

### <a name="query-results-include-both-internal-and-removable-storage"></a>내부 저장소 및 이동식 저장소를 포함하는 쿼리 결과

사용자는 기본적으로 옵션 SD 카드에 파일을 저장하도록 선택할 수 있습니다. 하지만 앱에서 SD 카드에 파일이 저장되는 것을 허용하지 않도록 선택할 수 있습니다. 결과적으로, 미디어 라이브러리를 장치 내부 저장소와 SD 카드로 분할할 수 있습니다.

이 옵션을 처리하기 위해 추가 코드를 작성할 필요가 없습니다. 알려진 폴더를 쿼리하는 [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) 네임스페이스의 메서드가 두 위치 모두의 쿼리 결과를 투명하게 결합합니다. 이와 같이 결합된 결과를 얻기 위해 앱 매니페스트 파일에 **removableStorage** 접근 권한 값을 지정할 필요가 없습니다.

디바이스 저장소의 상태가 다음 이미지에 나온 것과 같다고 가정합니다.

![휴대폰 및 SD 카드의 이미지](images/phone-media-locations.png)

`await KnownFolders.PicturesLibrary.GetFilesAsync()`을(를) 호출하여 사진 라이브러리의 콘텐츠를 쿼리하는 경우 결과에 internalPic.jpg 및 SDPic.jpg 모두가 포함됩니다.


## <a name="working-with-photos"></a>사진 작업

카메라가 모든 사진의 저해상도 이미지와 고해상도 이미지를 모두 저장하는 장치의 경우 심층 쿼리는 저해상도 이미지만 반환합니다.

카메라 롤과 저장된 사진 폴더는 심층 쿼리를 지원하지 않습니다.

**사진을 캡처한 앱으로 사진 열기**

나중에 사용자가 사진을 캡처한 앱으로 사진을 다시 열 수 있도록 하려면 다음 예제와 비슷한 코드를 사용하여 사진의 메타데이터로 **CreatorAppId**을(를) 저장할 수 있습니다. 이 예제에서 **testPhoto**는 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)입니다.

```cs
IDictionary<string, object> propertiesToSave = new Dictionary<string, object>();

propertiesToSave.Add("System.CreatorOpenWithUIOptions", 1);
propertiesToSave.Add("System.CreatorAppId", appId);

testPhoto.Properties.SavePropertiesAsync(propertiesToSave).AsyncWait();   
```

## <a name="using-stream-methods-to-add-a-file-to-a-media-library"></a>스트림 메서드를 사용하여 미디어 라이브러리에 파일 추가

**KnownFolders.PictureLibrary**과(와) 같은 알려진 폴더를 사용하여 미디어 라이브러리에 액세스하고 스트림 메서드를 사용하여 미디어 라이브러리에 파일을 추가할 때 코드가 연 모든 스트림을 닫아야 합니다. 그렇지 않으면 이러한 메서드가 미디어 라이브러리에 제대로 파일을 추가하지 못합니다. 하나 이상의 스트림에 여전히 파일에 대한 핸들이 있기 때문입니다.

예를 들어, 다음 코드를 실행할 때 파일이 미디어 라이브러리에 추가되지 않습니다. `using (var destinationStream = (await destinationFile.OpenAsync(FileAccessMode.ReadWrite)).GetOutputStreamAt(0))` 코드 줄에서 **OpenAsync** 메서드와 **GetOutputStreamAt** 메서드가 모두 스트림을 엽니다. 그러나 **using**문을 실행하면 **GetOutputStreamAt** 메서드가 연 스트림만 처리됩니다. 다른 스트림은 열린 상태로 남아 파일을 저장할 수 없게 됩니다.

```cs
StorageFolder testFolder = await StorageFolder.GetFolderFromPathAsync(@"C:\test");
StorageFile sourceFile = await testFolder.GetFileAsync("TestImage.jpg");
StorageFile destinationFile = await KnownFolders.CameraRoll.CreateFileAsync("MyTestImage.jpg");
using (var sourceStream = (await sourceFile.OpenReadAsync()).GetInputStreamAt(0))
{
    using (var destinationStream = (await destinationFile.OpenAsync(FileAccessMode.ReadWrite)).GetOutputStreamAt(0))
    {
        await RandomAccessStream.CopyAndCloseAsync(sourceStream, destinationStream);
    }
}
```

스트림 메서드를 사용하여 미디어 라이브러리에 파일을 추가하려면 다음 예제에 표시된 대로 코드가 연 모든 스트림을 닫아야 합니다.

```cs
StorageFolder testFolder = await StorageFolder.GetFolderFromPathAsync(@"C:\test");
StorageFile sourceFile = await testFolder.GetFileAsync("TestImage.jpg");
StorageFile destinationFile = await KnownFolders.CameraRoll.CreateFileAsync("MyTestImage.jpg");

using (var sourceStream = await sourceFile.OpenReadAsync())
{
    using (var sourceInputStream = sourceStream.GetInputStreamAt(0))
    {
        using (var destinationStream = await destinationFile.OpenAsync(FileAccessMode.ReadWrite))
        {
            using (var destinationOutputStream = destinationStream.GetOutputStreamAt(0))
            {
                await RandomAccessStream.CopyAndCloseAsync(sourceInputStream, destinationStream);
            }
        }
    }
}
```
