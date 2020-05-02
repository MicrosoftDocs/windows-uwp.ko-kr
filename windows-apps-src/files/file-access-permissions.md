---
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: 파일 액세스 권한
description: 앱은 기본적으로 특정 파일 시스템 위치에 액세스할 수 있습니다. 또한 앱은 파일 선택기를 통해서나 접근 권한 값을 선언하여 추가 위치에 액세스할 수도 있습니다.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- javascript
ms.openlocfilehash: b9529632fe582da438d6d17e31ffbdd963714a7c
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81608653"
---
# <a name="file-access-permissions"></a>파일 액세스 권한

UWP(유니버설 Windows 플랫폼) 앱은 기본적으로 특정 파일 시스템 위치에 액세스할 수 있습니다. 또한 앱은 파일 선택기를 통해서나 접근 권한 값을 선언하여 추가 위치에 액세스할 수도 있습니다.

## <a name="the-locations-that-all-apps-can-access"></a>모든 앱이 액세스할 수 있는 위치

새 앱을 만들면 기본적으로 다음 파일 시스템 위치에 액세스할 수 있습니다.

### <a name="application-install-directory"></a>애플리케이션 설치 디렉터리
사용자 시스템에서 앱이 설치된 폴더입니다.

다음과 같은 두 가지 기본 방법으로 앱의 설치 디렉터리에 있는 파일과 폴더에 액세스할 수 있습니다.

1. 아래와 같이 앱의 설치 디렉터리를 나타내는 [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder)를 검색할 수 있습니다.

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

    그런 다음 [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) 메서드를 사용하여 디렉터리의 파일 및 폴더에 액세스할 수 있습니다. 위 예제에서 이 **StorageFolder**는 `installDirectory` 변수에 저장됩니다. 앱 패키지로 작업하고 GitHub에 있는 [앱 패키지 정보 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Package)에서 디렉토리를 설치하는 방법에 대해 자세히 학습할 수 있습니다.

2. 아래와 같이 앱 URI를 사용하여 앱의 설치 디렉터리에서 직접 파일을 검색할 수 있습니다.

    ```csharp
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

    [**GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync)가 완료되면 앱 설치 디렉터리에 있는 `file.txt` 파일(이 예제에서는 `file`)을 나타내는 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)을 반환합니다.
    
    URI의 "ms-appx:///" 접두사는 앱의 설치 디렉터리를 나타냅니다. 앱 URI 사용에 대한 자세한 내용은 [URI를 사용하여 콘텐츠를 참조하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh781215(v=win.10))을 참조하세요.

또한 다른 위치와는 달리 일부 [UWP(유니버설 Windows 플랫폼) 앱용 Win32 및 COM](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps) 및 [Microsoft Visual Studio의 C/C++ 표준 라이브러리 기능](https://docs.microsoft.com/cpp/cpp/c-cpp-language-and-standard-libraries)을 사용하여 앱 설치 디렉터리에서 파일에 액세스할 수 있습니다.

앱의 설치 디렉터리는 읽기 전용 위치입니다. 파일 선택기를 통해서는 설치 디렉터리에 액세스할 수 없습니다.

### <a name="application-data-locations"></a>애플리케이션 데이터 위치
앱이 데이터를 저장할 수 있는 폴더입니다. 이러한 폴더(로컬, 로밍 및 임시)는 앱이 설치될 때 만들어집니다.

다음과 같은 두 가지 기본 방법으로 앱의 데이터 위치에 있는 파일과 폴더에 액세스할 수 있습니다.

1.  [  **ApplicationData**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData) 속성을 사용하여 앱 데이터 폴더를 검색합니다.

    예를 들어 [**ApplicationData**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData).[**LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder)를 사용하여 아래와 같이 앱의 로컬 폴더를 나타내는 [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder)를 검색할 수 있습니다.
    
    ```csharp
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
    
    앱의 로밍 또는 임시 폴더에 액세스하려면 대신 [**RoamingFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder) 또는 [**TemporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder) 속성을 사용합니다.
    
    앱 데이터 위치를 나타내는 [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder)를 검색한 후 **StorageFolder** 메서드를 사용하여 해당 위치의 파일과 폴더에 액세스할 수 있습니다. 위 예제에서 이 **StorageFolder** 개체는 `localFolder` 변수에 저장됩니다. [ApplicationData 클래스](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata) 페이지의 지침에서, 그리고 GitHub에서 [애플리케이션 데이터 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData)을 다운로드하여 앱 데이터 위치 사용에 대해 자세히 알아볼 수 있습니다.

2. 아래와 같이 앱 URI를 사용하여 앱의 로컬 폴더에서 직접 파일을 검색할 수 있습니다.
    
    ```csharp
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
    
    [**GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync)가 완료되면 앱의 로컬 폴더에 있는 `file.txt` 파일(이 예제에서는 `file`)을 나타내는 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)을 반환합니다.
    
    URI의 "ms-appdata:///local/" 접두사는 앱의 로컬 폴더를 나타냅니다. 앱의 로밍 또는 임시 폴더에 있는 파일에 액세스하려면 대신 "ms-appdata:///roaming/" 또는 "ms-appdata:///temporary/"를 사용합니다. 앱 URI 사용에 대한 자세한 내용은 [파일 리소스를 로드하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh781229(v=win.10))을 참조하세요.

또한 다른 위치와는 달리 일부 [UWP 앱용 Win32 및 COM](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps) 및 Visual Studio의 일부 C/C++ 표준 라이브러리 기능을 사용하여 앱 데이터 위치에서 파일에 액세스할 수 있습니다.

파일 선택기를 통해서는 로컬, 로밍 또는 임시 폴더에 액세스할 수 없습니다.

### <a name="removable-devices"></a>이동식 디바이스

또한 앱은 연결된 디바이스의 일부 파일에 기본적으로 액세스할 수 있습니다. 사용자가 카메라나 USB 썸 드라이브(thumb drive)와 같은 장치를 시스템에 연결할 때 앱이 [자동 실행 확장](https://docs.microsoft.com/previous-versions/windows/apps/hh464906(v=win.10))을 사용하여 자동으로 실행되는 경우 이것은 옵션입니다. 앱이 액세스할 수 있는 파일은 앱 매니페스트에서 파일 형식 연결 선언을 통해 지정한 특정 파일 형식으로 제한됩니다.

물론 이동식 장치의 파일과 폴더에도 액세스할 수 있는데, 이 경우에는 파일 선택기([**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 및 [**FolderPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker) 사용)를 호출하고 앱에서 액세스할 파일과 폴더를 사용자가 선택하게 합니다. [선택기를 사용하여 파일 및 폴더 열기](quickstart-using-file-and-folder-pickers.md)에서 파일 선택기를 사용하는 방법을 알아보세요.

> [!NOTE]
> 다른 이동식 디바이스에서 SD 카드에 액세스하는 방법에 대한 자세한 내용은 [SD 카드 액세스](access-the-sd-card.md)를 참조하세요.

## <a name="locations-that-uwp-apps-can-access"></a>UWP 앱이 액세스할 수 있는 위치

### <a name="users-downloads-folder"></a>사용자의 다운로드 폴더

다운로드된 파일이 기본적으로 저장되는 폴더입니다.

기본적으로 앱은 직접 만든 사용자의 다운로드 폴더에 있는 파일과 폴더에만 액세스할 수 있습니다. 그러나 파일 선택기([**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 또는 [**FolderPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker))를 호출하여 사용자 다운로드 폴더의 파일과 폴더에 액세스할 수 있으므로 앱에서 액세스할 파일이나 폴더를 사용자가 탐색하고 선택할 수 있습니다.

- 아래와 같이 사용자 다운로드 폴더에 파일을 만들 수 있습니다.

    ```csharp
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

    [**DownloadsFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.DownloadsFolder).[**CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.downloadsfolder.createfileasync)는 오버로드되므로 다운로드 폴더에 동일한 이름의 기존 파일이 이미 있는 경우 시스템에서 어떻게 할지를 지정할 수 있습니다. 이러한 메서드가 완료되면 만든 파일을 나타내는 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)을 반환합니다. 위 예제에서 이 파일은 `newFile`입니다.

- 아래와 같이 사용자 다운로드 폴더에 하위 폴더를 만들 수 있습니다.

    ```csharp
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

    [**DownloadsFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.DownloadsFolder).[**CreateFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.downloadsfolder.createfolderasync)는 오버로드되므로 다운로드 폴더에 동일한 이름의 기존 하위 폴더가 이미 있는 경우 시스템에서 어떻게 할지를 지정할 수 있습니다. 이러한 메서드가 완료되면 만든 하위 폴더를 나타내는 [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder)를 반환합니다. 위 예제에서 이 파일은 `newFolder`입니다.

다운로드 폴더에 파일이나 폴더를 만드는 경우 해당 항목의 앱의 [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist)에 추가하여 앱이 나중에 해당 항목에 쉽게 액세스할 수 있도록 하는 것이 좋습니다.

## <a name="accessing-additional-locations"></a>추가 위치 액세스

기본 위치 외에 앱은 [앱 매니페스트의 기능을 선언](../packaging/app-capability-declarations.md)하거나 [파일 선택기를 호출](quickstart-using-file-and-folder-pickers.md)하여 사용자가 앱에 액세스할 파일과 폴더를 선택할 수 있도록 하여 추가 파일 및 폴더에 액세스할 수 있습니다.

[AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) 확장을 선언하는 앱은 콘솔 창에서 앱이 실행되는 디렉토리(및 하위 디렉토리)에 대해 파일 시스템 권한이 있습니다.

다음 표에는 하나 이상의 기능을 선언하고 관련 [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) API를 사용하여 액세스할 수 있는 추가 위치가 나와 있습니다.

| 위치 | 기능 | Windows.Storage API |
|----------|------------|---------------------|
| 사용자에게 액세스 권한이 있는 모든 파일입니다. 예: 문서, 그림, 사진, 다운로드, 데스크톱, OneDrive 등. | **broadFileSystemAccess**<br><br>이는 제한된 접근 권한 값입니다. 액세스는 **설정** > **개인 정보** > **파일 시스템**에서 구성할 수 있습니다. 사용자가 언제든지 **설정**에서 권한을 부여 또는 거부할 수 있으므로 앱은 이러한 변경에 탄력적으로 반응할 수 있어야 합니다. 앱에 액세스 권한이 없으면 [Windows 10 파일 시스템 액세스 및 개인 정보](https://support.microsoft.com/help/4468237/windows-10-file-system-access-and-privacy-microsoft-privacy) 문서에 대한 링크를 제공하여 사용자가 설정을 변경할지 묻도록 선택할 수 있습니다. 사용자는 앱을 닫고, 설정을 전환하고, 앱을 다시 시작해야 합니다. 이러한 앱이 실행되는 동안 사용자가 설정을 전환하면 플랫폼은 사용자가 상태를 저장한 후 새 설정 저장을 위해 앱을 강제로 종료할 수 있도록 앱을 일시 중단합니다. 2018년 4월 업데이트에서 사용 권한의 기본값은 켜짐입니다. 2018년 10월 업데이트에서 기본값은 꺼짐입니다.<br /><br />이 접근 권한 값을 선언하는 Microsoft Store에 앱을 제출하는 경우 앱에 이 접근 권한 값이 필요한 이유와 이를 사용할 방법에 대한 추가 설명을 제공해야 합니다.<br/><br/>이 접근 권한 값은 [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) 네임스페이스의 API에 대해 작동합니다. 앱에서 이 접근 권한 값을 사용하도록 설정하는 방법의 예를 보려면 이 문서 끝에 나오는 **예제** 섹션을 참조하세요.<br/><br/>**참고:** 이 기능은 Xbox에서 지원되지 않습니다. | 해당 없음 |
| Documents | **documentsLibrary**<br><br>참고: 앱이 이 위치에서 액세스할 수 있는 특정 파일 형식을 선언하는 파일 형식 연결을 앱 매니페스트에 추가해야 합니다. <br><br>앱에서 다음 작업을 하려는 경우 이 접근 권한 값을 사용합니다.<br>- 유효한 OneDrive URL 또는 리소스 ID를 사용하여 특정 OneDrive 콘텐츠에 대한 플랫폼 간 오프라인 액세스를 용이하게 합니다.<br>- 오프라인에 있는 동안 열려 있는 파일을 사용자의 OneDrive에 자동으로 저장합니다. | [KnownFolders.DocumentsLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.documentslibrary) |
| 음악     | **musicLibrary** <br>[음악, 사진 및 비디오 라이브러리의 파일 및 폴더](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)도 참조하세요. | [KnownFolders.MusicLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.musiclibrary) |    
| 사진  | **picturesLibrary**<br> [음악, 사진 및 비디오 라이브러리의 파일 및 폴더](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)도 참조하세요. | [KnownFolders.PicturesLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.pictureslibrary) |  
| 동영상    | **videosLibrary**<br>[음악, 사진 및 비디오 라이브러리의 파일 및 폴더](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)도 참조하세요. | [KnownFolders.VideosLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.videoslibrary) |   
| 이동식 디바이스  | **removableStorage**  <br><br>참고: 앱이 이 위치에서 액세스할 수 있는 특정 파일 형식을 선언하는 파일 형식 연결을 앱 매니페스트에 추가해야 합니다. <br><br>[SD 카드 액세스](access-the-sd-card.md)도 참조하세요. | [KnownFolders.RemovableDevices](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.removabledevices) |  
| 홈 그룹 라이브러리  | 다음 접근 권한 값 중 하나 이상이 필요합니다. <br>- **musicLibrary** <br>- **picturesLibrary** <br>- **videosLibrary** | [KnownFolders.HomeGroup](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.homegroup) |      
| 미디어 서버 디바이스(DLNA) | 다음 접근 권한 값 중 하나 이상이 필요합니다. <br>- **musicLibrary** <br>- **picturesLibrary** <br>- **videosLibrary** | [KnownFolders.MediaServerDevices](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.mediaserverdevices) |
| UNC(범용 명명 규칙) 폴더 | 다음과 같은 접근 권한 값의 조합이 필요합니다. <br><br>홈 및 회사 네트워크 접근 권한 값: <br>- **privateNetworkClientServer** <br><br>하나 이상의 인터넷 및 공용 네트워크 접근 권한 값: <br>- **internetClient** <br>- **internetClientServer** <br><br>해당되는 경우 도메인 자격 증명 접근 권한 값:<br>- **enterpriseAuthentication** <br><br>**참고:** 앱이 이 위치에서 액세스할 수 있는 특정 파일 형식을 선언하는 파일 형식 연결을 앱 매니페스트에 추가해야 합니다. | 다음을 사용하여 폴더 검색: <br>[StorageFolder.GetFolderFromPathAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfolderfrompathasync) <br><br>다음을 사용하여 파일 검색: <br>[StorageFile.GetFileFromPathAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefrompathasync) |

### <a name="example"></a>예제

이 예제에서는 제한된 **broadFileSystemAccess** 기능을 추가합니다. 기능 지정 외에도 `rescap` 네임스페이스를 추가해야 하며 `IgnorableNamespaces`에도 추가됩니다.

```xaml
<Package
  ...
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp rescap">
...
<Capabilities>
    <rescap:Capability Name="broadFileSystemAccess" />
</Capabilities>
```

> [!NOTE]
> 앱 접근 권한 값의 전체 목록을 보려면 [앱 접근 권한 값 선언](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)을 참조하세요.
