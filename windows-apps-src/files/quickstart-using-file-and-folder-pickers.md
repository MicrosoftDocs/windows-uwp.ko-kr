---
author: laurenhughes
ms.assetid: F87DBE2F-77DB-4573-8172-29E11ABEFD34
title: 선택기를 사용하여 파일 및 폴더 열기
description: 사용자가 선택기를 조작할 수 있도록 하여 파일 및 폴더에 액세스합니다. FileOpenPicker 및 FileSavePicker 클래스를 사용하여 파일에 액세스하고 FolderPicker 클래스를 사용하여 폴더에 액세스할 수 있습니다.
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b79bfa9ecdf76d2d59e3c0991240d88599dbe6dd
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6159660"
---
# <a name="open-files-and-folders-with-a-picker"></a>선택기를 사용하여 파일 및 폴더 열기

**중요 API**

-   [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)
-   [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)
-   [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)

사용자가 선택기를 조작할 수 있도록 하여 파일 및 폴더에 액세스합니다. [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 및 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) 클래스를 사용하여 파일에 액세스하고 [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881) 클래스를 사용하여 폴더에 액세스할 수 있습니다.

> [!NOTE]
> 전체 샘플에 대해서는 [파일 선택기 샘플](http://go.microsoft.com/fwlink/p/?linkid=619994)을 참조하세요.

## <a name="prerequisites"></a>필수 조건


-   **UWP(유니버설 Windows 플랫폼) 앱에 대한 비동기 프로그래밍 이해**

    C# 또는 Visual Basic에서 비동기 앱을 작성하는 방법에 대한 자세한 내용은 [C# 또는 Visual Basic에서 비동기식 API 호출](https://msdn.microsoft.com/library/windows/apps/mt187337)을 참조하세요. C++에서 비동기 앱을 작성하는 방법은 [C++의 비동기 프로그래밍](https://msdn.microsoft.com/library/windows/apps/mt187334)을 참조하세요.

-   **위치에 대한 액세스 권한**

    [파일 액세스 권한](file-access-permissions.md)을 참조하세요.

## <a name="file-picker-ui"></a>파일 선택기 UI


파일 선택기는 사용자를 안내하고 파일을 열거나 저장할 때 일관된 환경을 제공하기 위해 정보를 표시합니다.

이러한 정보에는 다음이 포함됩니다.

-   현재 위치
-   사용자가 선택한 항목
-   사용자가 이동할 수 있는 위치의 트리. 이러한 위치에는 파일 시스템 위치(예: 음악 또는 다운로드 폴더) 파일 선택기 계약을 구현하는 앱(예: 카메라, 사진 및 Microsoft OneDrive)이 포함됩니다.

메일 앱은 사용자가 첨부 파일을 선택할 수 있도록 파일 선택기를 표시할 수 있습니다.

![두 개의 파일이 열기 위해 선택되어 있는 파일 선택기](images/picker-multifile-600px.png)

## <a name="how-pickers-work"></a>선택기 작동 방식


선택기를 통해 앱은 사용자 시스템의 파일 및 폴더를 액세스하고 찾아보고 저장할 수 있습니다. 앱은 사용자가 작업할 수 있도록 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 및 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 개체로 이러한 선택을 수신합니다.

선택기는 통합된 단일 인터페이스를 사용하여 사용자가 파일 시스템이나 다른 앱에서 파일 및 폴더를 선택할 수 있도록 합니다. 다른 앱에서 선택한 파일은 파일 시스템의 파일과 유사하며 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 개체로 반환됩니다. 일반적으로 앱은 다른 개체와 동일한 방식으로 해당 개체에서 작동할 수 있습니다. 다른 앱에서는 파일 선택기 계약에 참여함으로써 파일을 사용할 수 있게 합니다. 앱에서 다른 앱에 파일, 저장 위치 또는 파일 업데이트를 제공하도록 하려면 [파일 선택기 계약과 통합](https://msdn.microsoft.com/library/windows/apps/hh465192)을 참조하세요.

예를 들어 사용자가 파일을 열 수 있도록 앱에서 파일 선택기를 호출할 수 있습니다. 그러면 해당 앱이 호출 앱이 됩니다. 파일 선택기는 시스템 및/또는 다른 앱과 상호 작용하여 사용자가 파일을 탐색하고 선택할 수 있도록 합니다. 사용자가 파일을 선택하면 파일 선택기는 해당 파일을 앱에 반환합니다. 사용자가 OneDrive와 같은 제공 앱에서 파일을 선택할 경우의 프로세스는 다음과 같습니다.

![한 앱이 파일 선택기를 두 앱 간의 인터페이스로 사용하여 다른 앱에서 열 파일을 가져오는 프로세스를 보여 주는 다이어그램입니다.](images/app-to-app-diagram-600px.png)

## <a name="pick-a-single-file-complete-code-listing"></a>단일 파일 선택: 전체 코드 목록


```cs
var picker = new Windows.Storage.Pickers.FileOpenPicker();
picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.PicturesLibrary;
picker.FileTypeFilter.Add(".jpg");
picker.FileTypeFilter.Add(".jpeg");
picker.FileTypeFilter.Add(".png");

Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();
if (file != null)
{
    // Application now has read/write access to the picked file
    this.textBlock.Text = "Picked photo: " + file.Name;
}
else
{
    this.textBlock.Text = "Operation cancelled.";
}
```

## <a name="pick-a-single-file-step-by-step"></a>단일 파일 선택: 단계별


파일 선택기 사용에는 파일 선택기 개체를 만들고 사용자 지정한 다음 사용자가 하나 이상의 항목을 선택할 수 있도록 파일 선택기를 표시하는 과정이 포함됩니다.

1.  **FileOpenPicker 만들기 및 사용자 지정**

    ```cs
    var picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.PicturesLibrary;
    picker.FileTypeFilter.Add(".jpg");
    picker.FileTypeFilter.Add(".jpeg");
    picker.FileTypeFilter.Add(".png");
    ```
    사용자 및 앱과 관련된 파일 선택기 개체에서 속성을 설정합니다.

    이 예제에서는 세 가지 속성([**ViewMode**](https://msdn.microsoft.com/library/windows/apps/br207855), [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207854) 및 [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850))을 설정하여 사용자가 선택할 수 있는 편리한 위치에 사진의 풍부한 시각적 표시를 만듭니다.

    -   [**ViewMode**](https://msdn.microsoft.com/library/windows/apps/br207855)를 [**PickerViewMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.storage.pickers.pickerviewmode.aspx#thumbnail) **Thumbnail** 열거 값으로 설정하면 사진 미리 보기를 사용하여 파일 선택기에 파일을 표시함으로써 풍부한 시각적 표시가 만들어집니다. 이렇게 하려면 사진이나 비디오와 같은 시각적 파일을 선택합니다. 그렇지 않으면 [**PickerViewMode.List**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.storage.pickers.pickerviewmode.aspx#list)를 사용합니다. **사진 또는 비디오 첨부** 및 **문서 첨부** 기능이 있는 가상 메일 앱은 파일 선택기를 표시하기 전에 기능에 적합한 **ViewMode**를 설정합니다.

    -   [**PickerLocationId.PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br207854)를 사용하여 [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207890)을 사진으로 설정하면 사용자가 사진을 찾을 수 있을 것 같은 위치에서 시작됩니다. **SuggestedStartLocation**을 선택하려는 파일 형식(예: 음악, 사진, 동영상 또는 문서)에 적절한 위치로 설정합니다. 사용자는 시작 위치에서 다른 위치로 이동할 수 있습니다.

    -   [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850)를 사용하여 파일 형식을 지정하면 사용자가 관련 있는 파일을 선택하는 데 집중할 수 있습니다. **FileTypeFilter**의 이전 파일 형식을 새 항목으로 대체하려면 [**Add**](https://msdn.microsoft.com/library/windows/apps/br207844) 대신 [**ReplaceAll**](https://msdn.microsoft.com/library/windows/apps/br207834)을 사용합니다.

2.  **FileOpenPicker 표시**

    - **단일 파일을 선택하려면**

    ```cs
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();
    if (file != null)
    {
        // Application now has read/write access to the picked file
        this.textBlock.Text = "Picked photo: " + file.Name;
    }
    else
    {
        this.textBlock.Text = "Operation cancelled.";
    }
    ```

    - **여러 파일을 선택하려면**  

    ```cs
    var files = await picker.PickMultipleFilesAsync();
    if (files.Count > 0)
    {
        StringBuilder output = new StringBuilder("Picked files:\n");

        // Application now has read/write access to the picked file(s)
        foreach (Windows.Storage.StorageFile file in files)
        {
            output.Append(file.Name + "\n");
        }
        this.textBlock.Text = output.ToString();
    }
    else
    {
        this.textBlock.Text = "Operation cancelled.";
    }
    ```

## <a name="pick-a-folder-complete-code-listing"></a>폴더 선택: 전체 코드 목록


```cs
var folderPicker = new Windows.Storage.Pickers.FolderPicker();
folderPicker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.Desktop;
folderPicker.FileTypeFilter.Add("*");

Windows.Storage.StorageFolder folder = await folderPicker.PickSingleFolderAsync();
if (folder != null)
{
    // Application now has read/write access to all contents in the picked folder
    // (including other sub-folder contents)
    Windows.Storage.AccessCache.StorageApplicationPermissions.
    FutureAccessList.AddOrReplace("PickedFolderToken", folder);
    this.textBlock.Text = "Picked folder: " + folder.Name;
}
else
{
    this.textBlock.Text = "Operation cancelled.";
}
```

> [!TIP]
> 앱이 선택기를 통해 파일 또는 폴더에 액세스할 경우 해당 항목을 앱의 [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) 또는 [**MostRecentlyUsedList**](https://msdn.microsoft.com/library/windows/apps/br207458)에 추가하여 추적합니다. 이러한 목록을 사용하는 방법에 대한 자세한 내용은 [최근에 사용한 파일 및 폴더를 추적하는 방법](how-to-track-recently-used-files-and-folders.md)을 참조하세요.