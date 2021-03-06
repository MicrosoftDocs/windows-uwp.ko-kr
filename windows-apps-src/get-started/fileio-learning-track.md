---
title: 파일 작업
description: UWP(유니버설 Windows 플랫폼) 앱에서 파일 읽기 및 쓰기를 시작하는 데 필요한 주요 API 및 유형에 대해 알아봅니다.
ms.date: 05/01/2018
ms.topic: article
keywords: 시작, uwp, windows 10, 학습 트랙, 파일, 파일 io, 파일 읽기, 파일 쓰기, 파일 만들기, 텍스트 쓰기, 텍스트 읽기
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: fea8437a533d3559c912e48241720bddd01ed96d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158967"
---
# <a name="work-with-files"></a>파일 작업

이 토픽에서는 유니버설 Windows 플랫폼(UWP) 앱에서 파일을 읽고 쓰기 위해 알아야 할 사항에 대해 다룹니다. 주요 API 및 형식을 소개하고, 자세한 내용을 알아볼 수 있는 링크를 제공합니다.

이 문서는 자습서가 아닙니다. 자습서를 원하는 경우 파일을 만들고 읽고 쓰는 방법과 버퍼 및 스트림을 사용하는 방법을 보여주는 [파일 만들기, 쓰기 및 읽기](../files/quickstart-reading-and-writing-files.md)를 참조하세요. 파일을 만들고 읽고 쓰고 복사하고 삭제하는 방법뿐 아니라 파일 속성을 검색하고 앱에서 쉽게 다시 액세스할 수 있도록 파일 또는 폴더를 기억하는 방법을 보여주는 [파일 액세스 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess)도 살펴볼 수 있습니다.

파일의/에 텍스트를 읽고 쓰는 코드와 앱의 로컬, 로밍 및 임시 폴더에 액세스하는 방법을 살펴보겠습니다.

## <a name="what-do-you-need-to-know"></a>알아야 할 사항

파일의/에 텍스트를 읽거나 쓰기 위해 알아야 할 주요 형식은 다음과 같습니다.

- [Windows.Storage.StorageFile](/uwp/api/windows.storage.storagefile)은 파일을 나타냅니다. 이 클래스에는 파일에 대한 정보를 제공하는 속성과 파일을 만들고, 열고, 복사하고, 삭제하고, 이름을 변경하는 메서드가 있습니다.
아마도 여러분은 문자열 경로를 다루는 데 익숙할 수 것입니다. 문자열 경로를 사용하는 일부 Windows 런타임 API가 있기는 하지만, UWP에서 작업하는 일부 파일에 경로가 없거나 다루기 어려운 경로가 있을 수 있기 때문에 **StorageFile**을 사용하여 파일을 나타내는 경우가 훨씬 더 많을 것입니다. [StorageFile.GetFileFromPathAsync()](/uwp/api/windows.storage.storagefile.getfilefrompathasync)를 사용하여 문자열 경로를 **StorageFile**로 변환합니다. 

- [FileIO](/uwp/api/windows.storage.fileio) 클래스는 쉽게 텍스트를 읽고 쓰는 방법을 제공합니다. 이 클래스는 바이트 배열 또는 버퍼 콘텐츠를 읽거나 쓸 수 있습니다. 이 클래스는 [PathIO](/uwp/api/windows.storage.pathio) 클래스와 매우 유사합니다. 문자열 경로 대신 **PathIO**처럼 **StorageFile**을 사용한다는 차이가 있습니다.
- [Windows.Storage.StorageFolder](/uwp/api/windows.storage.storagefolder)는 폴더(디렉터리)를 나타냅니다. 이 클래스에는 파일을 만들고, 폴더의 콘텐츠를 쿼리하고, 폴더를 만들고, 이름을 바꾸고, 삭제하는 메서드와 폴더에 대한 정보를 제공하는 속성이 있습니다. 

**StorageFolder**를 가져오는 일반적인 방법은 다음과 같습니다.

- 사용자가 사용하려는 폴더로 이동할 수 있는 [Windows.Storage.Pickers.FolderPicker](/uwp/api/windows.storage.pickers.folderpicker)
- 로컬, 로밍, 임시 폴더와 같은 앱에 로컬인 폴더 중 하나에 대한 **StorageFolder**를 제공하는 [Windows.Storage.ApplicationData.Current](/uwp/api/windows.storage.applicationdata.current)
- 음악이나 사진 라이브러리처럼 알려진 라이브러리에 대한 **StorageFolder**를 제공하는 [Windows.Storage.KnownFolders](/uwp/api/windows.storage.knownfolders)

## <a name="write-text-to-a-file"></a>파일에 텍스트 쓰기

 이 소개 섹션에서는 텍스트 읽기 및 쓰기의 간단한 시나리오를 집중적으로 다룹니다. **FileIO** 클래스를 사용하여 파일에 텍스트를 쓰는 코드를 살펴보는 것으로 시작하겠습니다.

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.CreateFileAsync("test.txt",
        Windows.Storage.CreationCollisionOption.OpenIfExists);

await Windows.Storage.FileIO.WriteTextAsync(file, "Example of writing a string\r\n");

// Append a list of strings, one per line, to the file
var listOfStrings = new List<string> { "line1", "line2", "line3" };
await Windows.Storage.FileIO.AppendLinesAsync(file, listOfStrings); // each entry in the list is written to the file on its own line.
```

먼저 파일이 있어야 할 위치를 확인합니다. `Windows.Storage.ApplicationData.Current.LocalFolder`는 앱을 설치할 때 생성되는 로컬 데이터 폴더에 대한 액세스를 제공합니다. 앱이 액세스할 수 있는 폴더에 대한 자세한 내용은 [파일 시스템 액세스](#access-the-file-system)를 참조하세요.

그런 다음, **StorageFolder**를 사용하여 파일을 만듭니다. 또는 파일이 이미 있는 경우 파일을 엽니다.

**FileIO** 클래스는 파일에 텍스트를 작성하는 편리한 방법을 제공합니다. `FileIO.WriteTextAsync()`는 파일의 전체 내용을 제공된 텍스트로 바꿉니다. `FileIO.AppendLinesAsync()`는 한 줄당 한 문자열을 작성하여 파일에 문자열 컬렉션을 추가합니다.

## <a name="read-text-from-a-file"></a>파일의 텍스트 읽기

파일 쓰기와 마찬가지로, 파일 읽기는 파일이 있는 위치를 지정하는 것으로 시작합니다. 위의 예제와 같은 위치를 사용하겠습니다. 그런 다음, **FileIO** 클래스를 사용하여 콘텐츠를 읽겠습니다.

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.GetFileAsync("test.txt");

string text = await Windows.Storage.FileIO.ReadTextAsync(file);
```

`IList<string> contents = await Windows.Storage.FileIO.ReadLinesAsync(sampleFile);`을 사용하여 파일의 각 줄을 컬렉션의 개별 문자열로 읽을 수도 있습니다.

## <a name="access-the-file-system"></a>파일 시스템 액세스

UWP 플랫폼에서는 사용자 데이터의 무결성과 프라이버시를 보장하기 위해 폴더 액세스가 제한됩니다. 

### <a name="app-folders"></a>앱 폴더

UWP 앱이 설치되면 c:\users\<user name>\AppData\Local\Packages\<app package identifier>\ 아래에 여러 폴더가 만들어져 특히 앱의 로컬, 로밍 및 임시 파일을 저장합니다. 이러한 폴더에 액세스하기 위해 앱은 접근 권한 (값)을 선언하지 않아도 되며, 다른 앱은 이러한 폴더에 액세스할 수 없습니다. 앱을 제거하면 이러한 폴더도 함께 제거됩니다.

다음은 일반적으로 사용되는 앱 폴더입니다.

- **LocalState**: 현재 디바이스에 로컬인 데이터를 저장합니다. 디바이스가 백업되면 이 디렉터리의 데이터는 OneDrive의 백업 이미지에 저장됩니다. 사용자가 디바이스를 초기화하거나 교체하면 데이터가 복원됩니다. `Windows.Storage.ApplicationData.Current.LocalFolder.`로 이 폴더에 액세스합니다. OneDrive에 백업하지 않기를 원하는 로컬 데이터는 `Windows.Storage.ApplicationData.Current.LocalCacheFolder`로 액세스할 수 있는 **LocalCacheFolder**에 저장합니다.

- **RoamingState**: 앱이 설치된 모든 디바이스에서 복제해야 하는 데이터를 저장합니다. Windows는 로밍할 데이터의 양을 제한하므로 이 폴더에는 사용자 설정과 작은 파일만 저장합니다. `Windows.Storage.ApplicationData.Current.RoamingFolder`로 로밍 폴더에 액세스합니다.

- **TempState**: 앱이 실행되지 않는 경우 삭제될 수 있는 데이터를 저장합니다. `Windows.Storage.ApplicationData.Current.TemporaryFolder`로 이 폴더에 액세스합니다.

### <a name="access-the-rest-of-the-file-system"></a>나머지 파일 시스템에 액세스

UWP 앱은 매니페스트에 해당 접근 권한 (값)을 추가하여 특정 사용자 라이브러리에 액세스하려는 의도를 선언해야 합니다. 그러면 앱을 설치할 때 지정된 라이브러리에 대한 액세스 권한을 부여할 것인지 확인하기 위한 메시지가 표시됩니다. 부여하지 않으면 앱이 설치되지 않습니다. 사진, 동영상 및 음악 라이브러리에 액세스하기 위한 접근 권한 (값)이 있습니다. 전체 목록은 [앱 접근 권한 (값) 선언](../packaging/app-capability-declarations.md)을 참조하세요. 이러한 라이브러리에 대한 **StorageFolder**를 가져오려면 [Windows.Storage.KnownFolders](/uwp/api/windows.storage.knownfolders) 클래스를 사용합니다.

#### <a name="documents-library"></a>문서 라이브러리

사용자의 문서 라이브러리에 액세스할 수 있는 접근 권한 (값)이 있지만 해당 접근 권한 (값)은 제한되어 있기 때문에 특별 승인을 얻기 위한 절차를 따르지 않으면 이를 선언하는 앱이 Microsoft Store에서 거부됩니다. 일반적인 용도로 사용할 수 없습니다. 대신 사용자가 폴더 또는 파일을 탐색하는 데 사용할 수 있는 파일 또는 폴더 선택기를 사용합니다([선택기를 사용하여 파일 및 폴더 열기](../files/quickstart-using-file-and-folder-pickers.md) 및 [선택기를 사용하여 파일 저장](../files/quickstart-save-a-file-with-a-picker.md) 참조). 사용자가 파일 또는 폴더로 이동할 때 앱이 파일 또는 폴더에 액세스할 수 있도록 사용자에게 암시적으로 권한이 부여되며 시스템에서 액세스를 허용합니다.

#### <a name="general-access"></a>일반 액세스

또는 앱에서 해당 매니페스트의 제한된 [broadFileSystem](../packaging/app-capability-declarations.md) 접근 권한 (값)을 선언할 수 있습니다. 이 경우에도 Microsoft Store 승인이 필요합니다. 그러면 앱은 파일 또는 폴더 선택기를 조작하지 않아도 사용자에게 액세스 권한이 있는 모든 파일에 액세스할 수 있습니다.

앱에서 액세스할 수 있는 위치의 전체 목록은 [파일 액세스 권한](../files/file-access-permissions.md)을 참조하세요.

## <a name="useful-apis-and-docs"></a>유용한 API 및 문서

여기서는 파일 및 폴더를 시작하는 데 도움이 되는 API에 대한 요약 정보와 기타 유용한 문서를 제공합니다.

### <a name="useful-apis"></a>유용한 API

| API | 설명 |
|------|---------------|
|  [Windows.Storage.StorageFile](/uwp/api/windows.storage.storagefile) | 파일에 대한 정보와 파일을 만들고, 열고, 복사하고, 삭제하고, 이름을 변경할 수 있는 메서드를 제공합니다. |
| [Windows.Storage.StorageFolder](/uwp/api/windows.storage.storagefolder) | 폴더에 대한 정보, 파일을 만들기 위한 메서드 그리고 폴더를 만들고, 열고, 이름을 변경하고, 삭제할 수 있는 메서드를 제공합니다. |
| [FileIO](/uwp/api/windows.storage.fileio) |  쉽게 텍스트를 읽고 쓰는 방법을 제공합니다. 이 클래스는 바이트 배열 또는 버퍼 콘텐츠를 읽거나 쓸 수 있습니다. |
| [PathIO](/uwp/api/windows.storage.pathio) | 문자열 경로가 지정된 파일의/에 텍스트를 읽고 쓰는 쉬운 방법을 제공합니다. 이 클래스는 바이트 배열 또는 버퍼 콘텐츠를 읽거나 쓸 수 있습니다. |
| [DataReader](/uwp/api/windows.storage.streams.datareader) & [DataWriter](/uwp/api/windows.storage.streams.datawriter) |  스트림에서 버퍼, 바이트, 정수, GUID, TimeSpan 등을 읽고 씁니다. |
| [Windows.Storage.ApplicationData.Current](/uwp/api/windows.storage.applicationdata.current) | 로컬 폴더, 로밍 폴더, 임시 파일 폴더 등 앱에 대해 생성된 폴더에 대한 액세스를 제공합니다. |
| [Windows.Storage.Pickers.FolderPicker](/uwp/api/windows.storage.pickers.folderpicker) |  사용자가 폴더를 선택할 수 있도록 하고 해당 폴더의 **StorageFolder**를 반환합니다. 이 방법으로 앱이 기본적으로 액세스할 수 없는 위치에 대한 액세스 권한을 얻을 수 있습니다. |
| [Windows.Storage.Pickers.FileOpenPicker](/uwp/api/windows.storage.pickers.fileopenpicker) | 사용자가 열 폴더를 선택할 수 있도록 하고 해당 폴더의 **StorageFile**을 반환합니다. 이 방법으로 앱이 기본적으로 액세스할 수 없는 파일에 대한 액세스 권한을 얻을 수 있습니다. |
| [Windows.Storage.Pickers.FileSavePicker](/uwp/api/windows.storage.pickers.filesavepicker) | 사용자가 파일의 이름, 확장명 및 스토리지 위치를 선택할 수 있습니다. **StorageFile**을 반환합니다. 이 방법으로 앱이 기본적으로 액세스할 수 없는 위치에 파일을 저장할 수 있습니다. |
|  [Windows.Storage.Streams 네임스페이스](/uwp/api/windows.storage.streams) | 스트림 읽기 및 쓰기를 다룹니다. 특히 버퍼, 바이트, 정수, GUID, TimeSpan 등을 읽고 쓰는 [DataReader](/uwp/api/windows.storage.streams.datareader) 및 [DataWriter](/uwp/api/windows.storage.streams.datawriter) 클래스를 잘 살펴보세요. |

### <a name="useful-docs"></a>유용한 문서

| 항목 | 설명 |
|-------|----------------|
| [Windows.Storage 네임스페이스](/uwp/api/windows.storage) | API 참조 문서 |
| [파일, 폴더 및 라이브러리](../files/index.md) | 개념 문서. |
| [파일 만들기, 쓰기 및 읽기](../files/quickstart-reading-and-writing-files.md) | 텍스트, 이진 데이터 및 스트림을 만들고 읽고 쓰는 것에 대해 설명합니다. |
| [로컬로 앱 데이터 저장 시작](https://blogs.windows.com/buildingapps/2016/05/10/getting-started-storing-app-data-locally/#pCbJKGjcShh5DTV5.97) | 로컬 데이터를 저장하는 모범 사례와 LocalSettings 및 LocalCache 폴더의 목적을 다룹니다. |
| [앱 데이터 로밍 시작](https://blogs.windows.com/buildingapps/2016/05/03/getting-started-with-roaming-app-data/#RgjgLt5OkU9DbVV8.97) | 앱 데이터 로밍을 사용하는 방법에 대한 두 부분으로 구성된 시리즈입니다. |
| [애플리케이션 데이터 로밍 지침](../design/app-settings/store-and-retrieve-app-data.md) | 앱을 디자인할 때 이러한 데이터 로밍 지침을 따릅니다. |
| [설정과 기타 앱 데이터의 저장 및 검색](../design/app-settings/store-and-retrieve-app-data.md) | 로컬, 로밍 및 임시 폴더와 같은 다양한 앱 데이터의 저장에 대한 개요를 제공합니다. 디바이스 간에 로밍하는 데이터를 쓰는 지침과 자세한 내용은 [데이터 로밍](../design/app-settings/store-and-retrieve-app-data.md#roaming-data) 섹션을 참조하세요. |
| [파일 액세스 권한](../files/file-access-permissions.md) | 앱이 액세스할 수 있는 파일 시스템 위치에 대한 정보입니다. |
| [선택기를 사용하여 파일 및 폴더 열기](../files/quickstart-using-file-and-folder-pickers.md) | 사용자가 선택기 UI를 통해 결정할 수 있도록 하여 파일 및 폴더에 액세스하는 방법을 보여줍니다. |
| [Windows.Storage.Streams](/uwp/api/windows.storage.streams) | 스트림을 읽고 쓰는 데 사용되는 형식입니다. |
| [음악, 사진 및 비디오 라이브러리의 파일 및 폴더](../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md) | 라이브러리의 폴더를 제거하고, 라이브러리의 폴더 목록을 가져오고, 저장된 사진, 음악 및 동영상을 검색하는 방법을 다룹니다. |

## <a name="useful-code-samples"></a>유용한 코드 샘플

| 코드 샘플 | 설명 |
|-----------------|---------------|
| [애플리케이션 데이터 샘플](/samples/microsoft/windows-universal-samples/applicationdata/) | 애플리케이션 데이터 API를 사용하여 각 사용자와 관련된 데이터를 저장 및 검색하는 방법을 보여줍니다. |
| [파일 액세스 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) | 파일을 만들고, 읽고, 쓰고, 복사하고 삭제하는 방법을 보여줍니다. |
| [파일 선택기 샘플](/samples/microsoft/windows-universal-samples/filepicker/) | 사용자가 UI를 통해 파일을 선택하도록 하여 파일 및 폴더에 액세스하는 방법과 사용자가 이름, 파일 형식 및 저장할 파일의 위치를 지정할 수 있도록 파일을 저장하는 방법을 보여줍니다. |
| [JSON 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Json) | [Windows.Data.Json 네임스페이스](/uwp/api/Windows.Data.Json)를 사용하여 JavaScript Object Notation(JSON) 개체, 배열, 문자열, 숫자 및 부울을 인코딩 및 디코딩하는 방법을 보여줍니다. |
| [추가 코드 샘플](https://developer.microsoft.com/windows/samples) | 범주 드롭다운 목록에서 **파일, 폴더 및 라이브러리**를 선택합니다. |