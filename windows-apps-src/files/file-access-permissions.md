---
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: 파일 액세스 권한
description: 앱은 기본적으로 특정 파일 시스템 위치에 액세스할 수 있습니다. 또한 앱은 파일 선택기를 통해서나 접근 권한 값을 선언하여 추가 위치에 액세스할 수도 있습니다.
ms.date: 06/28/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d960235e73ea9172fb966f227af9440923f3553e
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8196215"
---
# <a name="file-access-permissions"></a>파일 액세스 권한

유니버설 Windows 앱(앱)은 기본적으로 특정 파일 시스템 위치에 액세스할 수 있습니다. 또한 앱은 파일 선택기를 통해서나 접근 권한 값을 선언하여 추가 위치에 액세스할 수도 있습니다.

## <a name="the-locations-that-all-apps-can-access"></a>모든 앱이 액세스할 수 있는 위치

새 앱을 만들면 기본적으로 다음 파일 시스템 위치에 액세스할 수 있습니다.

### <a name="application-install-directory"></a>응용 프로그램 설치 디렉터리
사용자의 시스템에서 앱을 설치 하는 폴더입니다.

두 가지 기본 액세스 파일 및 폴더에 앱의 설치 디렉터리:

1. 아래와 같이 앱의 설치 디렉터리를 나타내는 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)를 검색할 수 있습니다.

```csharp
Windows.Storage.StorageFolder installedLocation = Windows.ApplicationModel.Package.Current.InstalledLocation;
```

```javascript
var installDirectory = Windows.ApplicationModel.Package.current.installedLocation;
```

```cppwinrt
#include <winrt/Windows.Storage.h>
...
Windows::Storage::StorageFolder installedLocation{ Windows::ApplicationModel::Package::Current().InstalledLocation() };
```

```cpp
Windows::Storage::StorageFolder^ installedLocation = Windows::ApplicationModel::Package::Current->InstalledLocation;
```

그런 다음 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 메서드를 사용하여 디렉터리의 파일 및 폴더에 액세스할 수 있습니다. 위 예제에서 이 **StorageFolder**는 `installDirectory` 변수에 저장됩니다. 앱 패키지로 작업하고 GitHub에 있는 [앱 패키지 정보 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Package)에서 디렉토리를 설치하는 방법에 대해 자세히 학습할 수 있습니다.

2. 아래와 같이 앱 URI를 사용하여 앱의 설치 디렉터리에서 직접 파일을 검색할 수 있습니다.

```cs
using Windows.Storage;            
StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///file.txt"));
```

```javascript
Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appx:///file.txt").done(
    function(file) {
        // Process file
    }
);
```

```cppwinrt
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    Windows::Storage::StorageFile file{
        co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{L"ms-appx:///file.txt"})
    };
    // Process file
}
```

```cpp
auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appx:///file.txt")));
getFileTask.then([](StorageFile^ file) 
{
    // Process file
});
```

[**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741)가 완료되면 앱 설치 디렉터리에 있는 `file.txt` 파일(이 예제에서는 `file`)을 나타내는 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)을 반환합니다.

URI의 "ms-appx:///" 접두사는 앱의 설치 디렉터리를 나타냅니다. 앱 URI 사용에 대한 자세한 내용은 [URI를 사용하여 콘텐츠를 참조하는 방법](https://msdn.microsoft.com/library/windows/apps/hh781215)을 참조하세요.

또한 다른 위치와는 달리 일부 [UWP(유니버설 Windows 플랫폼) 앱용 Win32 및 COM](https://msdn.microsoft.com/library/windows/apps/br205757) 및 [Microsoft Visual Studio의 C/C++ 표준 라이브러리 기능](http://msdn.microsoft.com/library/hh875057.aspx)을 사용하여 앱 설치 디렉터리에서 파일에 액세스할 수 있습니다.

앱의 설치 디렉터리는 읽기 전용 위치입니다. 파일 선택기를 통해서는 설치 디렉터리에 액세스할 수 없습니다.

### <a name="application-data-locations"></a>응용 프로그램 데이터 위치
앱이 데이터를 저장할 수 있는 폴더입니다. 이러한 폴더(로컬, 로밍 및 임시)는 앱이 설치될 때 만들어집니다.

앱의 데이터 위치에서 파일 및 폴더에 액세스 하는 두 가지 가지 있습니다.

1.  [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587) 속성을 사용하여 앱 데이터 폴더를 검색합니다.

예를 들어 [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587).[**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621)를 사용하여 아래와 같이 앱의 로컬 폴더를 나타내는 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)를 검색할 수 있습니다.

```cs
using Windows.Storage;
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
```

```javascript
var localFolder = Windows.Storage.ApplicationData.current.localFolder;
```

```cppwinrt
Windows::Storage::StorageFolder storageFolder{
    Windows::Storage::ApplicationData::Current().LocalFolder()
};
```

```cpp
using namespace Windows::Storage;
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
```

앱의 로밍 또는 임시 폴더에 액세스하려면 대신 [**RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623) 또는 [**TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629) 속성을 사용합니다.

앱 데이터 위치를 나타내는 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)를 검색한 후 **StorageFolder** 메서드를 사용하여 해당 위치의 파일과 폴더에 액세스할 수 있습니다. 위 예제에서 이 **StorageFolder** 개체는 `localFolder` 변수에 저장됩니다. [ApplicationData 클래스](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata) 페이지의 지침에서, 그리고 GitHub에서 [응용 프로그램 데이터 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData)을 다운로드하여 앱 데이터 위치 사용에 대해 자세히 알아볼 수 있습니다.

2. 예를 들어 아래와 같이 앱 URI를 사용하여 앱의 로컬 폴더에서 직접 파일을 검색할 수 있습니다.

```cs
using Windows.Storage;
StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appdata:///local/file.txt"));
```

```javascript
Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appdata:///local/file.txt").done(
    function(file) {
        // Process file
    }
);
```

```cppwinrt
Windows::Storage::StorageFile file{
    co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{ L"ms-appdata:///local/file.txt" })
};
// Process file
```

```cpp
using Windows::Storage;
auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appdata:///local/file.txt")));
getFileTask.then([](StorageFile^ file) 
{
    // Process file
});
```

[**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741)가 완료되면 앱의 로컬 폴더에 있는 `file.txt` 파일(이 예제에서는 `file`)을 나타내는 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)을 반환합니다.

URI의 "ms-appdata:///local/" 접두사는 앱의 로컬 폴더를 나타냅니다. 앱의 로밍 또는 임시 폴더에 있는 파일에 액세스하려면 대신 "ms-appdata:///roaming/" 또는 "ms-appdata:///temporary/"를 사용합니다. 앱 URI 사용에 대한 자세한 내용은 [파일 리소스를 로드하는 방법](https://msdn.microsoft.com/library/windows/apps/hh781229)을 참조하세요.

또한 다른 위치와는 달리 일부 [UWP 앱용 Win32 및 COM](https://msdn.microsoft.com/library/windows/apps/br205757) 및 Visual Studio의 일부 C/C++ 표준 라이브러리 기능을 사용하여 앱 데이터 위치에서 파일에 액세스할 수 있습니다.

파일 선택기를 통해서는 로컬, 로밍 또는 임시 폴더에 액세스할 수 없습니다.

### <a name="removable-devices"></a>이동식 장치
또한 앱은 연결된 디바이스의 일부 파일에 기본적으로 액세스할 수 있습니다. 사용자가 카메라나 USB 썸 드라이브(thumb drive)와 같은 장치를 시스템에 연결할 때 앱이 [자동 실행 확장](https://msdn.microsoft.com/library/windows/apps/xaml/hh464906.aspx#autoplay)을 사용하여 자동으로 실행되는 경우 이것은 옵션입니다. 앱이 액세스할 수 있는 파일은 앱 매니페스트에서 파일 형식 연결 선언을 통해 지정한 특정 파일 형식으로 제한됩니다.

물론 이동식 장치의 파일과 폴더에도 액세스할 수 있는데, 이 경우에는 파일 선택기([**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 및 [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881) 사용)를 호출하고 앱에서 액세스할 파일과 폴더를 사용자가 선택하게 합니다. [선택기를 사용하여 파일 및 폴더 열기](quickstart-using-file-and-folder-pickers.md)에서 파일 선택기를 사용하는 방법을 알아보세요.

> [!NOTE]
> 다른 이동식 장치에서 SD 카드에 액세스하는 방법에 대한 자세한 내용은 [SD 카드 액세스](access-the-sd-card.md)를 참조하세요.

## <a name="locations-that-uwp-apps-can-access"></a>UWP 앱에서 액세스할 수 있는 위치
### <a name="users-downloads-folder"></a>사용자의 다운로드 폴더

다운로드된 파일이 기본적으로 저장되는 폴더입니다.

기본적으로 앱은 직접 만든 사용자의 다운로드 폴더에 있는 파일과 폴더에만 액세스할 수 있습니다. 그러나 파일 선택기([**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 또는 [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881))를 호출하여 사용자 다운로드 폴더의 파일과 폴더에 액세스할 수 있으므로 앱에서 액세스할 파일이나 폴더를 사용자가 탐색하고 선택할 수 있습니다.

- 아래와 같이 사용자 다운로드 폴더에 파일을 만들 수 있습니다.

```cs
using Windows.Storage;
StorageFile newFile = await DownloadsFolder.CreateFileAsync("file.txt");
```

```javascript
Windows.Storage.DownloadsFolder.createFileAsync("file.txt").done(
    function(newFile) {
        // Process file
    }
);
```

```cppwinrt
Windows::Storage::StorageFile newFile{
    co_await Windows::Storage::DownloadsFolder::CreateFileAsync(L"file.txt")
};
// Process file
```

```cpp
using Windows::Storage;
auto createFileTask = create_task(DownloadsFolder::CreateFileAsync(L"file.txt"));
createFileTask.then([](StorageFile^ newFile)
{
    // Process file
});
```

[**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh996761)는 오버로드되므로 다운로드 폴더에 동일한 이름의 기존 파일이 이미 있는 경우 시스템에서 어떻게 할지를 지정할 수 있습니다. 이러한 메서드가 완료되면 만든 파일을 나타내는 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)을 반환합니다. 위 예제에서 이 파일은 `newFile`입니다.

- 아래와 같이 사용자 다운로드 폴더에 하위 폴더를 만들 수 있습니다.

```cs
using Windows.Storage;
StorageFolder newFolder = await DownloadsFolder.CreateFolderAsync("New Folder");
```

```javascript
Windows.Storage.DownloadsFolder.createFolderAsync("New Folder").done(
    function(newFolder) {
        // Process folder
    }
);
```

```cppwinrt
Windows::Storage::StorageFolder newFolder{
    co_await Windows::Storage::DownloadsFolder::CreateFolderAsync(L"New Folder")
};
// Process folder
```

```cpp
using Windows::Storage;
auto createFolderTask = create_task(DownloadsFolder::CreateFolderAsync(L"New Folder"));
createFolderTask.then([](StorageFolder^ newFolder)
{
    // Process folder
});
```

[**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFolderAsync**](https://msdn.microsoft.com/library/windows/apps/hh996763)는 오버로드되므로 다운로드 폴더에 동일한 이름의 기존 하위 폴더가 이미 있는 경우 시스템에서 어떻게 할지를 지정할 수 있습니다. 이러한 메서드가 완료되면 만든 하위 폴더를 나타내는 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)를 반환합니다. 위 예제에서 이 파일은 `newFolder`입니다.

다운로드 폴더에 파일이나 폴더를 만드는 경우 해당 항목의 앱의 [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457)에 추가하여 앱이 나중에 해당 항목에 쉽게 액세스할 수 있도록 하는 것이 좋습니다.

## <a name="accessing-additional-locations"></a>추가 위치 액세스

기본 위치 외에 앱은 앱 매니페스트에 접근 권한 값을 선언하거나([앱 접근 권한 값 선언](https://msdn.microsoft.com/library/windows/apps/mt270968) 참조), 파일 선택기를 호출하여 앱이 액세스할 파일과 폴더를 사용자가 선택할 수 있게 하는([선택기를 사용하여 파일 및 폴더 열기](quickstart-using-file-and-folder-pickers.md)) 방법으로 추가 파일과 폴더에 액세스할 수 있습니다.

[AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) 확장을 선언하는 앱은 콘솔 창에서 앱이 실행되는 디렉토리(및 하위 디렉토리)의 파일 시스템 권한을 가지고 있습니다.

다음 표에는 접근 권한 값을 선언하고 관련 [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) API를 사용하여 액세스할 수 있는 추가 위치가 나와 있습니다.

| 위치 | 접근 권한 값 | Windows.Storage API |
|----------|------------|---------------------|
| 사용자가 액세스 권한을 가지고 있는 모든 파일. 예: 문서, 그림, 사진, 다운로드, 데스크톱, OneDrive 등. | broadFileSystemAccess<br><br>이는 제한된 접근 권한 값입니다. 처음 사용할 때 시스템은 사용자에게 액세스를 허용할지를 묻는 메시지를 표시합니다. 액세스는 설정 > 개인 정보 > 파일 시스템에서 구성할 수 있습니다. 이 접근 권한 값을 선언하는 Microsoft Store에 앱을 제출하는 경우 앱에 이 접근 권한 값이 필요한 이유와 이를 사용할 방법에 대한 추가 설명을 제공해야 합니다.<br>이 접근 권한 값은 [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) 네임스페이스의 API에 대해 작동합니다. | 해당 없음 |
| 문서 | DocumentsLibrary <br><br>참고: 앱이 이 위치에서 액세스할 수 있는 특정 파일 형식을 선언하는 파일 형식 연결을 앱 매니페스트에 추가해야 합니다. <br><br>앱에서 다음 작업을 하려는 경우 이 접근 권한 값을 사용합니다.<br>- 유효한 OneDrive URL 또는 리소스 ID를 사용하여 특정 OneDrive 콘텐츠에 대한 플랫폼 간 오프라인 액세스를 용이하게 합니다.<br>-열려 있는 동안 자동으로 사용자의 OneDrive에 파일 저장 오프 라인 | [KnownFolders.DocumentsLibrary](https://msdn.microsoft.com/library/windows/apps/br227152) |
| 음악     | MusicLibrary <br>[음악, 사진 및 비디오 라이브러리의 파일 및 폴더](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)도 참조하세요. | [KnownFolders.MusicLibrary](https://msdn.microsoft.com/library/windows/apps/br227155) |    
| 사진  | PicturesLibrary<br> [음악, 사진 및 비디오 라이브러리의 파일 및 폴더](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)도 참조하세요. | [KnownFolders.PicturesLibrary](https://msdn.microsoft.com/library/windows/apps/br227156) |  
| 동영상    | VideosLibrary<br>[음악, 사진 및 비디오 라이브러리의 파일 및 폴더](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)도 참조하세요. | [KnownFolders.VideosLibrary](https://msdn.microsoft.com/library/windows/apps/br227159) |   
| 이동식 장치  | RemovableDevices <br><br>참고: 앱이 이 위치에서 액세스할 수 있는 특정 파일 형식을 선언하는 파일 형식 연결을 앱 매니페스트에 추가해야 합니다. <br><br>[SD 카드 액세스](access-the-sd-card.md)도 참조하세요. | [KnownFolders.RemovableDevices](https://msdn.microsoft.com/library/windows/apps/br227158) |  
| 홈 그룹 라이브러리  | 다음 접근 권한 값 중 하나 이상이 필요합니다. <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.HomeGroup](https://msdn.microsoft.com/library/windows/apps/br227153) |      
| 미디어 서버 장치(DLNA) | 다음 접근 권한 값 중 하나 이상이 필요합니다. <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.MediaServerDevices](https://msdn.microsoft.com/library/windows/apps/br227154) |
| UNC(범용 명명 규칙) 폴더 | 다음과 같은 접근 권한 값의 조합이 필요합니다. <br><br>홈 및 회사 네트워크 접근 권한 값: <br>- PrivateNetworkClientServer <br><br>하나 이상의 인터넷 및 공용 네트워크 접근 권한 값: <br>- InternetClient <br>- InternetClientServer <br><br>해당되는 경우 도메인 자격 증명 접근 권한 값:<br>- EnterpriseAuthentication <br><br>참고: 앱이 이 위치에서 액세스할 수 있는 특정 파일 형식을 선언하는 파일 형식 연결을 앱 매니페스트에 추가해야 합니다. | 다음을 사용하여 폴더 검색: <br>[StorageFolder.GetFolderFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227278) <br><br>다음을 사용하여 파일 검색: <br>[StorageFile.GetFileFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227206) |

**예**

이 예는 제한된 `broadFileSystemAccess` 접근 권한 값을 추가합니다. 접근 권한 값을 지정할 뿐 아니라 `rescap` 네임스페이스가 추가되어야 하고 `IgnorableNamespaces`에도 추가됩니다.

```xaml
<Package
  ...
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp uap5 rescap">
...
<Capabilities>
    <rescap:Capability Name="broadFileSystemAccess" />
</Capabilities>
```

> [!NOTE]
> 앱 접근 권한 값의 전체 목록을 보려면 [앱 접근 권한 값 선언](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)을 참조하세요.
