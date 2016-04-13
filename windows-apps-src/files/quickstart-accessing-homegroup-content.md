---
ms.assetid: 12ECEA89-59D2-4BCE-B24C-5A4DD525E0C7
title: 홈 그룹 콘텐츠 액세스
description: 사진, 음악, 동영상 등 사용자의 홈 그룹 폴더에 저장된 콘텐츠에 액세스합니다.
---
# 홈 그룹 콘텐츠 액세스

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


** 중요 API **

-   [**Windows.Storage.KnownFolders 클래스**](https://msdn.microsoft.com/library/windows/apps/br227151)

사진, 음악, 비디오 등 사용자의 홈 그룹 폴더에 저장된 콘텐츠에 액세스합니다.

## 필수 조건

-   **UWP(유니버설 Windows 플랫폼) 앱에 대한 비동기 프로그래밍 이해**

    C# 또는 Visual Basic에서 비동기 앱을 작성하는 방법에 대한 자세한 내용은 [C# 또는 Visual Basic에서 비동기식 API 호출](https://msdn.microsoft.com/library/windows/apps/mt187337)을 참조하세요. C++에서 비동기 앱을 작성하는 방법은 [C++의 비동기 프로그래밍](https://msdn.microsoft.com/library/windows/apps/mt187334)을 참조하세요.

-   **앱 기능 선언**

    홈 그룹 콘텐츠에 액세스하려면 사용자의 컴퓨터에 홈 그룹이 설정되어 있고 앱에는 **picturesLibrary**, **musicLibrary** 또는 **videosLibrary** 접근 권한 값 중 하나 이상이 있어야 합니다. 앱에서 홈 그룹 폴더에 액세스하면 앱의 매니페스트에 선언된 접근 권한 값에 해당하는 라이브러리만 표시됩니다. 자세한 내용은 [파일 액세스 권한](file-access-permissions.md)을 참조하세요.

    **참고** 홈 그룹의 문서 라이브러리에 있는 콘텐츠는 앱의 매니페스트에 선언된 접근 권한 값 및 사용자의 공유 설정에 관계없이 앱에 표시되지 않습니다.

     

-   **파일 선택기 사용 방법 이해**

    일반적으로 파일 선택기를 사용하여 홈 그룹의 파일 및 폴더에 액세스합니다. 파일 선택기 사용 방법에 대한 자세한 내용은 [선택기를 사용하여 파일 및 폴더 열기](quickstart-using-file-and-folder-pickers.md)를 참조하세요.

-   **파일 및 폴더 쿼리 이해**

    쿼리를 사용하여 홈 그룹의 파일 및 폴더를 열거할 수 있습니다. 파일 및 폴더 쿼리에 대한 자세한 내용은 [파일 및 폴더 열거 및 쿼리](quickstart-listing-files-and-folders.md)를 참조하세요.

## 홈 그룹에서 파일 선택기 열기

사용자가 홈 그룹에서 파일 및 폴더를 선택할 수 있도록 하는 파일 선택기 인스턴스를 열려면 다음 단계를 수행하세요.

1.  **파일 선택기 만들기 및 사용자 지정**

    [
            **FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)를 사용하여 파일 선택기를 만든 다음 선택기의 [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207854)을 [**PickerLocationId.HomeGroup**](https://msdn.microsoft.com/library/windows/apps/br207890)으로 설정합니다. 또는 사용자 및 앱과 관련된 다른 속성을 설정합니다. 파일 선택기 사용자 지정 방법을 결정하는 데 도움이 되는 지침은 [파일 선택기에 대한 지침 및 검사 목록](https://msdn.microsoft.com/library/windows/apps/hh465182)을 참조하세요.

    다음 예에서는 홈 그룹에서 열리는 파일 선택기를 만들고 모든 형식의 파일을 포함하며 파일을 미리 보기 이미지로 표시합니다.
    ```csharp
    Windows.Storage.Pickers.FileOpenPicker picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.HomeGroup;
    picker.FileTypeFilter.Clear();
    picker.FileTypeFilter.Add("*");
    ```
  
2.  **파일 선택기 표시 및 선택된 파일 처리**

    파일 선택기를 만들어 사용자 지정한 후 [**FileOpenPicker.PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275)를 호출하여 사용자가 파일 하나를 선택하거나 [**FileOpenPicker.PickMultipleFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br207851)를 호출하여 여러 파일을 선택하도록 할 수 있습니다.

    다음 예에서는 사용자가 파일 하나를 선택하도록 하는 파일 선택기를 표시합니다.
    ```csharp
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();

    if (file != null)
    {
        // Do something with the file.
    }
    else
    {
        // No file returned. Handle the error.
    }   
    ```

## 홈 그룹에서 파일 검색

이 섹션에서는 사용자가 제공한 쿼리 용어와 일치하는 홈 그룹 항목을 찾는 방법을 보여 줍니다.

1.  **사용자의 쿼리 용어를 가져옵니다.**

    여기서는 사용자가 `searchQueryTextBox`라는 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 컨트롤에 입력한 쿼리 용어를 가져옵니다.
    ```csharp
    string queryTerm = this.searchQueryTextBox.Text;    
    ```

2.  **쿼리 옵션 및 검색 필터를 설정합니다.**

    쿼리 옵션은 검색 결과를 정렬하는 방식을 결정하고 검색 필터는 검색 결과에 포함되는 항목을 결정합니다.

    다음 예에서는 검색 결과를 관련성순으로 정렬한 다음 수정한 날짜순으로 정렬하는 쿼리 옵션을 설정합니다. 검색 필터는 사용자가 이전 단계에서 입력한 쿼리 용어입니다.
    ```csharp
    Windows.Storage.Search.QueryOptions queryOptions = 
            new Windows.Storage.Search.QueryOptions
                (Windows.Storage.Search.CommonFileQuery.OrderBySearchRank, null);
    queryOptions.UserSearchFilter = queryTerm.Text;
    Windows.Storage.Search.StorageFileQueryResult queryResults = 
            Windows.Storage.KnownFolders.HomeGroup.CreateFileQueryWithOptions(queryOptions);    
    ```

3.  **쿼리를 실행하고 결과를 처리합니다.**

    다음 예에서는 홈 그룹에서 검색 쿼리를 실행하고 일치하는 파일의 이름으로 문자열 목록으로 저장합니다.
    ```csharp
    System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFile> files = 
        await queryResults.GetFilesAsync();

    if (files.Count > 0)
    {
        outputString += (files.Count == 1) ? "One file found\n" : files.Count.ToString() + " files found\n";
        foreach (Windows.Storage.StorageFile file in files)
        {
            outputString += file.Name + "\n";
        }
    }    
    ```


## 홈 그룹에서 특정 사용자의 공유 파일 검색

이 섹션에서는 특정 사용자가 공유하는 홈 그룹을 찾는 방법을 보여 줍니다.

1.  **홈 그룹 사용자 컬렉션을 가져옵니다.**

    홈 그룹에 있는 각각의 첫 번째 수준 폴더는 개별 홈 그룹 사용자를 나타냅니다. 따라서 홈 그룹 사용자 컬렉션을 가져오려면 [**GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br227279)를 호출하여 최상위 수준의 홈 그룹 폴더를 검색합니다.
    ```csharp
    System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFolder> hgFolders = 
        await Windows.Storage.KnownFolders.HomeGroup.GetFoldersAsync();    
    ```

2.  **대상 사용자의 폴더를 찾은 다음 범위가 해당 사용자의 폴더로 지정된 파일 쿼리를 만듭니다.**

    다음 예제에서는 검색된 폴더를 반복하여 대상 사용자의 폴더를 찾습니다. 그런 다음 폴더의 모든 파일을 찾아서 관련성순으로 정렬하고 나서 수정 날짜로 정렬하도록 쿼리 옵션을 설정합니다. 이 예제에서는 파일 이름과 함께 찾은 파일 수를 보고하는 문자열을 작성합니다.
    ```csharp
    bool userFound = false;
    foreach (Windows.Storage.StorageFolder folder in hgFolders)
    {
        if (folder.DisplayName == targetUserName)
        {
            // Found the target user's folder, now find all files in the folder.
            userFound = true;
            Windows.Storage.Search.QueryOptions queryOptions = 
                new Windows.Storage.Search.QueryOptions
                    (Windows.Storage.Search.CommonFileQuery.OrderBySearchRank, null);
            queryOptions.UserSearchFilter = "*";
            Windows.Storage.Search.StorageFileQueryResult queryResults =
                folder.CreateFileQueryWithOptions(queryOptions);
            System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFile> files =
                await queryResults.GetFilesAsync();

            if (files.Count > 0)
            {
                string outputString = "Searched for files belonging to " + targetUserName + "'\n";
                outputString += (files.Count == 1) ? "One file found\n" : files.Count.ToString() + " files found\n";
                foreach (Windows.Storage.StorageFile file in files)
                {
                    outputString += file.Name + "\n";
                }
            }
        }
    }    
    ```

## 홈 그룹에서 비디오 스트리밍

홈 그룹에서 비디오 콘텐츠를 스트리밍하려면 다음 단계를 수행하세요.

1.  **MediaElement를 앱에 포함합니다.**

    [
            **MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)를 사용하면 앱에서 오디오 및 비디오 콘텐츠를 재생할 수 있습니다. 오디오 및 비디오 재생에 대한 자세한 내용은 [사용자 지정 전송 컨트롤 만들기](https://msdn.microsoft.com/library/windows/apps/mt187271) 및 [오디오, 비디오 및 카메라](https://msdn.microsoft.com/library/windows/apps/mt203788)를 참조하세요.
    ```HTML
    <Grid x:Name="Output" HorizontalAlignment="Left" VerticalAlignment="Top" Grid.Row="1">
        <MediaElement x:Name="VideoBox" HorizontalAlignment="Left" VerticalAlignment="Top" Margin="0" Width="400" Height="300"/>
    </Grid>    
    ```

2.  **홈 그룹에서 파일 선택기를 열고 앱이 지원하는 형식으로 비디오 파일을 포함하는 필터를 적용합니다.**

    다음 예에서는 파일 열기 선택기에 .mp4 및 .wmv 파일을 포함합니다.
    ```csharp
    Windows.Storage.Pickers.FileOpenPicker picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.HomeGroup;
    picker.FileTypeFilter.Clear();
    picker.FileTypeFilter.Add(".mp4");
    picker.FileTypeFilter.Add(".wmv");
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();   
    ```

3.  **사용자가 선택한 파일을 읽을 수 있도록 열고 파일 스트림을** [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)의 원본으로 설정한 다음 파일을 재생합니다.
    ```csharp
    if (file != null)
    {
        var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.Read);
        VideoBox.SetSource(stream, file.ContentType);
        VideoBox.Stop();
        VideoBox.Play();
    }
    else
    {
        // No file selected. Handle the error here.
    }    
    ```

 

 






<!--HONumber=Mar16_HO1-->


