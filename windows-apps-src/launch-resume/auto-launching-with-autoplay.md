---
title: 자동 실행을 사용한 자동 시작
description: 사용자가 장치를 PC에 연결 하는 경우 자동 실행을 사용 하 여 앱을 옵션으로 제공할 수 있습니다. 여기에는 카메라 또는 미디어 플레이어와 같은 볼륨이 아닌 장치나 USB 플래시 드라이브, SD 카드, DVD 등의 볼륨 장치가 포함 됩니다.
ms.assetid: AD4439EA-00B0-4543-887F-2C1D47408EA7
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f1a3e2eae72da7eb45104a023ce0fb75a4e4451f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173017"
---
# <a name="span-iddev_launch_resumeauto-launching_with_autoplayspanauto-launching-with-autoplay"></a><span id="dev_launch_resume.auto-launching_with_autoplay"></span>자동 실행을 사용한 자동 시작

사용자가 장치를 PC에 연결 하는 경우 **자동 실행** 을 사용 하 여 앱을 옵션으로 제공할 수 있습니다. 여기에는 카메라 또는 미디어 플레이어와 같은 볼륨이 아닌 장치나 USB 플래시 드라이브, SD 카드, DVD 등의 볼륨 장치가 포함 됩니다. 사용자가 근접 (탭)을 사용 하 여 두 Pc 간에 파일을 공유 하는 경우 **자동 실행** 을 사용 하 여 앱을 옵션으로 제공할 수도 있습니다.

> **참고**    장치 제조업체에서 장치에 대 한 **자동 실행** 핸들러로 [Microsoft Store 장치 앱](/windows-hardware/drivers/devapps/) 을 연결 하려는 경우 장치 메타 데이터에서 해당 앱을 식별할 수 있습니다. 자세한 내용은 [Microsoft Store 장치 앱에 대 한 자동 실행](/windows-hardware/drivers/devapps/autoplay-for-uwp-device-apps)을 참조 하세요.

## <a name="register-for-autoplay-content"></a>자동 실행 콘텐츠 등록

**자동 실행** 콘텐츠 이벤트의 옵션으로 앱을 등록할 수 있습니다. **재생** 콘텐츠 이벤트는 카메라 메모리 카드, 엄지 드라이브 또는 DVD와 같은 볼륨 장치를 PC에 삽입할 때 발생 합니다. 여기서는 카메라의 볼륨 장치를 삽입할 때 앱을 **자동 실행** 옵션으로 식별 하는 방법을 보여 줍니다.

이 자습서에서는 이미지 파일을 표시 하거나 그림에 복사 하는 앱을 만들었습니다. **ShowPicturesOnArrival** content 이벤트를 자동으로 실행 하도록 앱을 등록 했습니다.

또한 자동 실행은 근접 (탭)을 사용 하 여 Pc 간에 공유 되는 콘텐츠에 대해 콘텐츠 이벤트를 발생 시킵니다. 이 섹션의 단계 및 코드를 사용 하 여 근접을 사용 하는 Pc 간에 공유 되는 파일을 처리할 수 있습니다. 다음 표에서는 근접을 사용 하 여 콘텐츠를 공유 하는 데 사용할 수 있는 자동 실행 콘텐츠 이벤트를 보여 줍니다.

| 작업         | 콘텐츠 자동 재생 이벤트  |
|----------------|-------------------------|
| 음악 공유  | PlayMusicFilesOnArrival |
| 비디오 공유 | PlayVideoFilesOnArrival |

 
근접성을 사용 하 여 파일을 공유 하는 경우 **FileActivatedEventArgs** 개체의 **files** 속성은 모든 공유 파일을 포함 하는 루트 폴더에 대 한 참조를 포함 합니다.

### <a name="step-1-create-a-new-project-and-add-autoplay-declarations"></a>1 단계: 새 프로젝트 만들기 및 자동 실행 선언 추가

1.  Microsoft Visual Studio를 열고 **파일** 메뉴에서 **새 프로젝트** 를 선택 합니다. **Visual c #** 섹션의 **Windows**에서 **비어 있는 앱 (유니버설 Windows)** 을 선택 합니다. 앱 이름을 **Autoplaydisplayorcopyimages** 로 만들고 **확인을 클릭 합니다.**
2.  Appxmanifest.xml 파일을 열고 **기능** 탭을 선택 합니다. **이동식 저장소** 및 **그림 라이브러리** 기능을 선택 합니다. 이를 통해 앱은 카메라 메모리의 이동식 저장 장치에 액세스 하 고 로컬 그림에 액세스할 수 있습니다.
3.  매니페스트 파일에서 **선언** 탭을 선택 합니다. **사용 가능한 선언** 드롭다운 목록에서 **자동 실행 콘텐츠** 를 선택 하 고 **추가**를 클릭 합니다. **지원 되는 선언** 목록에 추가 된 새 **자동 실행 콘텐츠** 항목을 선택 합니다.
4.  자동 **실행 콘텐츠** 선언은 자동 실행이 콘텐츠 이벤트를 발생 시킬 때 앱을 옵션으로 식별 합니다. 이 이벤트는 DVD 또는 엄지 드라이브와 같은 볼륨 장치의 콘텐츠를 기반으로 합니다. 자동 실행은 볼륨 장치의 콘텐츠를 검사 하 고 발생 시킬 콘텐츠 이벤트를 결정 합니다. 볼륨의 루트에 DCIM, AVCHD 또는 PRIVATE \\ achd 폴더가 포함 되어 있거나, 사용자가 사용 하도록 설정한 경우 자동 재생 제어판에서 **각 미디어 유형으로 수행할 작업을 선택** 하면 자동으로 **ShowPicturesOnArrival** 이벤트가 발생 합니다. **시작 작업** 섹션에서 첫 번째 시작 동작에 대해 아래 표 1의 값을 입력 합니다.
5.  **자동 재생 콘텐츠** 항목에 대 한 **시작 작업** 섹션에서 **새로 추가** 를 클릭 하 여 두 번째 시작 작업을 추가 합니다. 두 번째 시작 작업에 대해 아래 표 2의 값을 입력 합니다.
6.  **사용 가능한 선언** 드롭다운 목록에서 **파일 유형 연결** 을 선택 하 고 **추가**를 클릭 합니다. 새 **파일 유형 연결** 선언의 속성에서 **표시 이름** 필드를 **자동으로 복사 또는 이미지 표시** 로 설정 하 고 **이름** 필드를 **image \_ association1**로 설정 합니다. **지원 되는 파일 형식** 섹션에서 **새로 추가**를 클릭 합니다. **파일 형식** 필드를 **.jpg**로 설정 합니다. **지원 되는 파일 형식** 섹션에서 새 파일 연결의 **파일 형식** 필드를 **.png**로 설정 합니다. 콘텐츠 이벤트의 경우 자동으로 앱에 명시적으로 연결 되지 않은 파일 형식을 자동으로 필터링 합니다.
7.  매니페스트 파일을 저장하고 닫습니다.

**표 1**

| 설정             | 값                 |
|---------------------|-----------------------|
| 동사                | 표시                  |
| 작업 표시 이름 | 사진 표시         |
| 콘텐츠 이벤트       | ShowPicturesOnArrival |

**작업 표시 이름** 설정은 앱에 대해 자동 실행이 표시 하는 문자열을 식별 합니다. **동사** 설정은 선택 된 옵션에 대해 앱에 전달 되는 값을 식별 합니다. 자동 실행 이벤트에 대해 여러 실행 작업을 지정 하 고 **동사** 설정을 사용 하 여 사용자가 앱에 대해 선택한 옵션을 확인할 수 있습니다. 앱에 전달 된 startup 이벤트 인수의 **verb** 속성을 확인 하 여 사용자가 선택한 옵션을 확인할 수 있습니다. 을 사용 하는 경우를 제외 하 고는 예약 된 **open**을 사용 하 여 **동사** 설정에 모든 값을 사용할 수 있습니다.

**표 2**  

| 설정             | 값                      |
|--------------------:|----------------------------|
| 동사                | copy                       |
| 작업 표시 이름 | 라이브러리에 그림 복사 |
| 콘텐츠 이벤트       | ShowPicturesOnArrival      |

### <a name="step-2-add-xaml-ui"></a>2 단계: XAML UI 추가

MainPage .xaml 파일을 열고 기본 그리드 섹션에 다음 XAML을 추가 합니다 &lt; &gt; .

```xml
<TextBlock FontSize="18">File List</TextBlock>
<TextBlock x:Name="FilesBlock" HorizontalAlignment="Left" TextWrapping="Wrap"
           VerticalAlignment="Top" Margin="0,20,0,0" Height="280" Width="240" />
<Canvas x:Name="FilesCanvas" HorizontalAlignment="Left" VerticalAlignment="Top"
        Margin="260,20,0,0" Height="280" Width="100"/>
```

### <a name="step-3-add-initialization-code"></a>3 단계: 초기화 코드 추가

이 단계의 코드는 **Onfileactivated** 된 이벤트 중에 앱에 전달 된 시작 인수 중 하나인 **verb** 속성의 동사 값을 확인 합니다. 그런 다음 코드는 사용자가 선택한 옵션과 관련 된 메서드를 호출 합니다. 카메라 메모리 이벤트의 경우 자동 실행은 카메라 저장소의 루트 폴더를 앱에 전달 합니다. 이 폴더는 **Files** 속성의 첫 번째 요소에서 검색할 수 있습니다.

App.xaml.cs 파일을 열고 **App** 클래스에 다음 코드를 추가 합니다.

```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
    if (args.Verb == "show")
    {
        Frame rootFrame = (Frame)Window.Current.Content;
        MainPage page = (MainPage)rootFrame.Content;

        // Call DisplayImages with root folder from camera storage.
        page.DisplayImages((Windows.Storage.StorageFolder)args.Files[0]);
    }

    if (args.Verb == "copy")
    {
        Frame rootFrame = (Frame)Window.Current.Content;
        MainPage page = (MainPage)rootFrame.Content;

        // Call CopyImages with root folder from camera storage.
        page.CopyImages((Windows.Storage.StorageFolder)args.Files[0]);
    }

    base.OnFileActivated(args);
}
```

> **참고**    `DisplayImages`및 `CopyImages` 메서드는 다음 단계에서 추가 됩니다.

### <a name="step-4-add-code-to-display-images"></a>4 단계: 이미지를 표시 하는 코드 추가

MainPage.xaml.cs 파일에서 **Mainpage** 클래스에 다음 코드를 추가 합니다.

```cs
async internal void DisplayImages(Windows.Storage.StorageFolder rootFolder)
{
    // Display images from first folder in root\DCIM.
    var dcimFolder = await rootFolder.GetFolderAsync("DCIM");
    var folderList = await dcimFolder.GetFoldersAsync();
    var cameraFolder = folderList[0];
    var fileList = await cameraFolder.GetFilesAsync();
    for (int i = 0; i < fileList.Count; i++)
    {
        var file = (Windows.Storage.StorageFile)fileList[i];
        WriteMessageText(file.Name + "\n");
        DisplayImage(file, i);
    }
}

async private void DisplayImage(Windows.Storage.IStorageItem file, int index)
{
    try
    {
        var sFile = (Windows.Storage.StorageFile)file;
        Windows.Storage.Streams.IRandomAccessStream imageStream =
            await sFile.OpenAsync(Windows.Storage.FileAccessMode.Read);
        Windows.UI.Xaml.Media.Imaging.BitmapImage imageBitmap =
            new Windows.UI.Xaml.Media.Imaging.BitmapImage();
        imageBitmap.SetSource(imageStream);
        var element = new Image();
        element.Source = imageBitmap;
        element.Height = 100;
        Thickness margin = new Thickness();
        margin.Top = index * 100;
        element.Margin = margin;
        FilesCanvas.Children.Add(element);
    }
    catch (Exception e)
    {
       WriteMessageText(e.Message + "\n");
    }
}

// Write a message to MessageBlock on the UI thread.
private Windows.UI.Core.CoreDispatcher messageDispatcher = Window.Current.CoreWindow.Dispatcher;

private async void WriteMessageText(string message, bool overwrite = false)
{
    await messageDispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            if (overwrite)
                FilesBlock.Text = message;
            else
                FilesBlock.Text += message;
        });
}
```

### <a name="step-5-add-code-to-copy-images"></a>5 단계: 이미지를 복사 하는 코드 추가

MainPage.xaml.cs 파일에서 **Mainpage** 클래스에 다음 코드를 추가 합니다.

```cs
async internal void CopyImages(Windows.Storage.StorageFolder rootFolder)
{
    // Copy images from first folder in root\DCIM.
    var dcimFolder = await rootFolder.GetFolderAsync("DCIM");
    var folderList = await dcimFolder.GetFoldersAsync();
    var cameraFolder = folderList[0];
    var fileList = await cameraFolder.GetFilesAsync();

    try
    {
        var folderName = "Images " + DateTime.Now.ToString("yyyy-MM-dd HHmmss");
        Windows.Storage.StorageFolder imageFolder = await
            Windows.Storage.KnownFolders.PicturesLibrary.CreateFolderAsync(folderName);

        foreach (Windows.Storage.IStorageItem file in fileList)
        {
            CopyImage(file, imageFolder);
        }
    }
    catch (Exception e)
    {
        WriteMessageText("Failed to copy images.\n" + e.Message + "\n");
    }
}

async internal void CopyImage(Windows.Storage.IStorageItem file,
                              Windows.Storage.StorageFolder imageFolder)
{
    try
    {
        Windows.Storage.StorageFile sFile = (Windows.Storage.StorageFile)file;
        await sFile.CopyAsync(imageFolder, sFile.Name);
        WriteMessageText(sFile.Name + " copied.\n");
    }
    catch (Exception e)
    {
        WriteMessageText("Failed to copy file.\n" + e.Message + "\n");
    }
}
```

### <a name="step-6-build-and-run-the-app"></a>6 단계: 앱 빌드 및 실행

1.  F5 키를 눌러 디버그 모드에서 앱을 빌드하고 배포 합니다.
2.  앱을 실행 하려면 카메라의 카메라 메모리 카드 또는 다른 저장 장치를 PC에 삽입 합니다. 그런 다음 옵션의 자동 실행 목록에서 appxmanifest.xml 파일에 지정한 내용 이벤트 옵션 중 하나를 선택 합니다. 이 샘플 코드는 카메라 메모리 카드의 ECIM 폴더에 있는 사진만 표시 하거나 복사 합니다. 카메라 메모리 카드가 AVCHD 또는 비공개 achd 폴더에 사진을 저장 하는 경우 코드를 적절 하 게 \\ 업데이트 해야 합니다.
    **참고**    카메라 메모리 카드가 없는 경우 루트에 이름이 **Ecim** 인 폴더가 있고, d cim 폴더에 이미지를 포함 하는 하위 폴더가 있는 경우 플래시 드라이브를 사용할 수 있습니다.

## <a name="register-for-an-autoplay-device"></a>자동 재생 장치 등록


**자동 재생** 장치 이벤트에 대 한 옵션으로 앱을 등록할 수 있습니다. 장치가 PC에 연결 된 경우 장치 이벤트가 **자동** 으로 발생 합니다.

여기서는 카메라가 PC에 연결 된 경우 **자동 실행** 옵션으로 앱을 식별 하는 방법을 보여 줍니다. 앱은 **WPD \\ ImageSourceAutoPlay** 이벤트에 대 한 처리기로 등록 됩니다. 이 이벤트는 카메라 및 기타 이미징 장치가 MTP를 사용 하 여 ImageSource를 알릴 때 WPD (Windows 휴대용 장치) 시스템에서 발생 하는 일반적인 이벤트입니다. 자세한 내용은 [Windows 휴대용 장치](/previous-versions/ff597729(v=vs.85))를 참조 하세요.

**중요**    [**Windows. storagedevice**](/uwp/api/Windows.Devices.Portable.StorageDevice) api는 [데스크톱 장치 제품군](../get-started/universal-application-platform-guide.md)의 일부입니다. 앱은 Pc와 같은 데스크톱 장치 제품군의 Windows 10 장치 에서만 이러한 Api를 사용할 수 있습니다.

 

### <a name="step-1-create-a-new-project-and-add-autoplay-declarations"></a>1 단계: 새 프로젝트 만들기 및 자동 실행 선언 추가

1.  Visual Studio를 열고 **파일** 메뉴에서 **새 프로젝트** 를 선택 합니다. **Visual c #** 섹션의 **Windows**에서 **비어 있는 앱 (유니버설 Windows)** 을 선택 합니다. 앱의 이름을 **Autoplaydevice \_ Camera** 로, **확인을 클릭 합니다.**
2.  Appxmanifest.xml 파일을 열고 **기능** 탭을 선택 합니다. **이동식 저장소** 기능을 선택 합니다. 이렇게 하면 앱이 이동식 저장소 볼륨 장치로 카메라의 데이터에 액세스할 수 있습니다.
3.  매니페스트 파일에서 **선언** 탭을 선택 합니다. **사용 가능한 선언** 드롭다운 목록에서 **자동 실행 장치** 를 선택 하 고 **추가**를 클릭 합니다. **지원 되는 선언** 목록에 추가 된 새 **자동 실행 장치** 항목을 선택 합니다.
4.  **자동 재생 장치** 선언에서는 응용 프로그램이 알려진 이벤트에 대해 장치 이벤트를 발생 시킬 때 앱을 옵션으로 식별 합니다. **시작 작업** 섹션에서 첫 번째 시작 작업에 대해 아래 표의 값을 입력합니다.
5.  **사용 가능한 선언** 드롭다운 목록에서 **파일 유형 연결** 을 선택 하 고 **추가**를 클릭 합니다. 새 **파일 유형 연결** 선언의 속성에서 **표시 이름** 필드를 **카메라에서 이미지 표시** 로, **이름** 필드를 **camera \_ association1**로 설정 합니다. **지원 되는 파일 형식** 섹션에서 **새로 추가** (필요한 경우)를 클릭 합니다. **파일 형식** 필드를 **.jpg**로 설정 합니다. **지원 되는 파일 형식** 섹션에서 **새로 추가** 를 다시 클릭 합니다. 새 파일 연결의 **파일 형식** 필드를 **.png**로 설정 합니다. 콘텐츠 이벤트의 경우 자동으로 앱에 명시적으로 연결 되지 않은 파일 형식을 자동으로 필터링 합니다.
6.  매니페스트 파일을 저장하고 닫습니다.

| 설정             | 값            |
|---------------------|------------------|
| 동사                | 표시             |
| 작업 표시 이름 | 사진 표시    |
| 콘텐츠 이벤트       | WPD \\ ImageSource |

**작업 표시 이름** 설정은 앱에 대해 자동 실행이 표시 하는 문자열을 식별 합니다. **동사** 설정은 선택 된 옵션에 대해 앱에 전달 되는 값을 식별 합니다. 자동 실행 이벤트에 대해 여러 실행 작업을 지정 하 고 **동사** 설정을 사용 하 여 사용자가 앱에 대해 선택한 옵션을 확인할 수 있습니다. 앱에 전달 된 startup 이벤트 인수의 **verb** 속성을 확인 하 여 사용자가 선택한 옵션을 확인할 수 있습니다. 을 사용 하는 경우를 제외 하 고는 예약 된 **open**을 사용 하 여 **동사** 설정에 모든 값을 사용할 수 있습니다. 단일 앱에서 여러 동사를 사용 하는 예제는 [자동 실행 콘텐츠 등록](#register-for-autoplay-content)을 참조 하세요.

### <a name="step-2-add-assembly-reference-for-the-desktop-extensions"></a>2 단계: 데스크톱 확장에 대 한 어셈블리 참조 추가

Windows 휴대용 장치, [**windows**](/uwp/api/Windows.Devices.Portable.StorageDevice)의 저장소에 액세스 하는 데 필요한 api는 데스크톱 [데스크톱 장치 제품군](../get-started/universal-application-platform-guide.md)의 일부입니다. 즉, Api를 사용 하려면 특수 어셈블리가 필요 하며, 이러한 호출은 데스크톱 장치 제품군 (예: PC)의 장치 에서만 작동 합니다.

1.  **솔루션 탐색기**에서 **참조** 를 마우스 오른쪽 단추로 클릭 한 다음 **참조 추가**...를 클릭 합니다.
2.  **유니버설 Windows**를 확장하고 **확장**을 클릭합니다.
3.  그런 다음 **UWP에 대 한 Windows 데스크톱 확장** 을 선택 하 고 **확인**을 클릭 합니다.

### <a name="step-3-add-xaml-ui"></a>3 단계: XAML UI 추가

MainPage .xaml 파일을 열고 기본 그리드 섹션에 다음 XAML을 추가 합니다 &lt; &gt; .

```xml
<StackPanel Orientation="Vertical" Margin="10,0,-10,0">
    <TextBlock FontSize="24">Device Information</TextBlock>
    <StackPanel Orientation="Horizontal">
        <TextBlock x:Name="DeviceInfoTextBlock" FontSize="18" Height="400" Width="400" VerticalAlignment="Top" />
        <ListView x:Name="ImagesList" HorizontalAlignment="Left" Height="400" VerticalAlignment="Top" Width="400">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Vertical">
                        <Image Source="{Binding Path=Source}" />
                        <TextBlock Text="{Binding Path=Name}" />
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
            <ListView.ItemsPanel>
                <ItemsPanelTemplate>
                    <WrapGrid Orientation="Horizontal" ItemHeight="100" ItemWidth="120"></WrapGrid>
                </ItemsPanelTemplate>
            </ListView.ItemsPanel>
        </ListView>
    </StackPanel>
</StackPanel>
```

### <a name="step-4-add-activation-code"></a>4 단계: 활성화 코드 추가

이 단계의 코드는 카메라의 장치 정보 Id를 트 [**중부**](/uwp/api/windows.devices.portable.storagedevice.fromid) 메서드에 전달 하 여 카메라를 [**storagedevice**](/uwp/api/Windows.Devices.Portable.StorageDevice) 로 참조 합니다. 카메라의 장치 정보 Id는 먼저 이벤트 인수를 [**DeviceActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.DeviceActivatedEventArgs)로 캐스팅 한 다음 [**deviceinformationid**](/uwp/api/windows.applicationmodel.activation.deviceactivatedeventargs.deviceinformationid) 속성에서 값을 가져오는 방식으로 가져옵니다.

App.xaml.cs 파일을 열고 **App** 클래스에 다음 코드를 추가 합니다.

```cs
protected override void OnActivated(IActivatedEventArgs args)
{
   if (args.Kind == ActivationKind.Device)
   {
      Frame rootFrame = null;
      // Ensure that the current page exists and is activated
      if (Window.Current.Content == null)
      {
         rootFrame = new Frame();
         rootFrame.Navigate(typeof(MainPage));
         Window.Current.Content = rootFrame;
      }
      else
      {
         rootFrame = Window.Current.Content as Frame;
      }
      Window.Current.Activate();

      // Make sure the necessary APIs are present on the device
      bool storageDeviceAPIPresent =
      Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.Devices.Portable.StorageDevice");

      if (storageDeviceAPIPresent)
      {
         // Reference the current page as type MainPage
         var mPage = rootFrame.Content as MainPage;

         // Cast the activated event args as DeviceActivatedEventArgs and show images
         var deviceArgs = args as DeviceActivatedEventArgs;
         if (deviceArgs != null)
         {
            mPage.ShowImages(Windows.Devices.Portable.StorageDevice.FromId(deviceArgs.DeviceInformationId));
         }
      }
      else
      {
         // Handle case where APIs are not present (when the device is not part of the desktop device family)
      }

   }

   base.OnActivated(args);
}
```

> **참고**    `ShowImages`메서드는 다음 단계에서 추가 됩니다.

### <a name="step-5-add-code-to-display-device-information"></a>5 단계: 장치 정보를 표시 하는 코드 추가

[**Storagedevice**](/uwp/api/Windows.Devices.Portable.StorageDevice) 클래스의 속성에서 카메라에 대 한 정보를 가져올 수 있습니다. 이 단계의 코드는 앱이 실행 될 때 사용자에 게 장치 이름 및 기타 정보를 표시 합니다. 그런 다음,이 코드는 다음 단계에서 추가할 GetImageList 및 GetThumbnail 메서드를 호출 하 여 카메라에 저장 된 이미지의 미리 보기를 표시 합니다.

MainPage.xaml.cs 파일에서 다음 코드를 **Mainpage** 클래스에 추가 합니다.

```cs
private Windows.Storage.StorageFolder rootFolder;

internal async void ShowImages(Windows.Storage.StorageFolder folder)
{
    DeviceInfoTextBlock.Text = "Display Name = " + folder.DisplayName + "\n";
    DeviceInfoTextBlock.Text += "Display Type =  " + folder.DisplayType + "\n";
    DeviceInfoTextBlock.Text += "FolderRelativeId = " + folder.FolderRelativeId + "\n";

    // Reference first folder of the device as the root
    rootFolder = (await folder.GetFoldersAsync())[0];
    var imageList = await GetImageList(rootFolder);

    foreach (Windows.Storage.StorageFile img in imageList)
    {
        ImagesList.Items.Add(await GetThumbnail(img));
    }
}
```

> **참고**    `GetImageList`및 `GetThumbnail` 메서드는 다음 단계에서 추가 됩니다.

### <a name="step-6-add-code-to-display-images"></a>6 단계: 이미지를 표시 하는 코드 추가

이 단계의 코드는 카메라에 저장 된 이미지의 축소판 그림을 표시 합니다. 이 코드는 카메라에 대해 비동기 호출을 수행 하 여 미리 보기 이미지를 가져옵니다. 그러나 다음 비동기 호출은 이전 비동기 호출이 완료 될 때까지 발생 하지 않습니다. 이렇게 하면 한 번에 하나의 요청만 카메라에 적용 됩니다.

MainPage.xaml.cs 파일에서 다음 코드를 **Mainpage** 클래스에 추가 합니다.

```cs
async private System.Threading.Tasks.Task<List<Windows.Storage.StorageFile>> GetImageList(Windows.Storage.StorageFolder folder)
{
    var result = await folder.GetFilesAsync();
    var subFolders = await folder.GetFoldersAsync();
    foreach (Windows.Storage.StorageFolder f in subFolders)
        result = result.Union(await GetImageList(f)).ToList();

    return (from f in result orderby f.Name select f).ToList();
}

async private System.Threading.Tasks.Task<Image> GetThumbnail(Windows.Storage.StorageFile img)
{
    // Get the thumbnail to display
    var thumbnail = await img.GetThumbnailAsync(Windows.Storage.FileProperties.ThumbnailMode.SingleItem,
                                                100,
                                                Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale);

    // Create a XAML Image object bind to on the display page
    var result = new Image();
    result.Height = thumbnail.OriginalHeight;
    result.Width = thumbnail.OriginalWidth;
    result.Name = img.Name;
    var imageBitmap = new Windows.UI.Xaml.Media.Imaging.BitmapImage();
    imageBitmap.SetSource(thumbnail);
    result.Source = imageBitmap;

    return result;
}
```

### <a name="step-7-build-and-run-the-app"></a>7 단계: 앱 빌드 및 실행

1.  F5 키를 눌러 디버그 모드에서 앱을 빌드하고 배포 합니다.
2.  앱을 실행 하려면 카메라를 컴퓨터에 연결 합니다. 그런 다음 옵션의 자동 실행 목록에서 앱을 선택 합니다.
    **참고**    모든 카메라가 **WPD \\ ImageSource** 자동 재생 장치 이벤트에 대해 보급 하는 것은 아닙니다.

## <a name="configure-removable-storage"></a>이동식 저장소 구성

볼륨 장치가 PC에 연결 된 경우 메모리 카드나 thumb 드라이브와 같은 볼륨 장치를 **자동 재생** 장치로 식별할 수 있습니다. 볼륨 장치에 대 한 사용자에 게 제공할 특정 앱을 **자동** 으로 연결 하려는 경우에 특히 유용 합니다.

여기서는 볼륨 장치를 **자동 재생** 장치로 식별 하는 방법을 보여 줍니다.

볼륨 장치를 **자동 재생** 장치로 식별 하려면 장치의 루트 드라이브에 자동 실행 .inf 파일을 추가 합니다. 자동 실행 .inf 파일에서 **자동 실행** 섹션에 **CustomEvent** 키를 추가 합니다. 볼륨 장치가 PC에 연결 되 면 **자동 실행** 에서 자동으로 실행 되는 .inf 파일을 찾아 볼륨을 장치로 처리 합니다. **자동 실행** 은 **CustomEvent** 키에 대해 제공한 이름을 사용 하 여 **자동 재생** 이벤트를 만듭니다. 그런 다음 앱을 만들고 앱을 해당 **자동 재생** 이벤트에 대 한 처리기로 등록할 수 있습니다. 장치가 PC에 연결 된 경우 **자동 실행** 은 앱을 볼륨 장치에 대 한 처리기로 표시 합니다. 자동 실행 inf 파일에 대 한 자세한 내용은 [자동 실행 .inf 항목](/windows/desktop/shell/autorun-cmds)을 참조 하세요.

### <a name="step-1-create-an-autoruninf-file"></a>1 단계: 자동으로 실행 되는 .inf 파일 만들기

볼륨 장치의 루트 드라이브에 autorun .inf 라는 파일을 추가 합니다. 자동 실행 .inf 파일을 열고 다음 텍스트를 추가 합니다.

``` syntax
[AutoRun]
CustomEvent=AutoPlayCustomEventQuickstart
```

### <a name="step-2-create-a-new-project-and-add-autoplay-declarations"></a>2 단계: 새 프로젝트 만들기 및 자동 실행 선언 추가

1.  Visual Studio를 열고 **파일** 메뉴에서 **새 프로젝트** 를 선택 합니다. **Visual c #** 섹션의 **Windows**에서 **비어 있는 앱 (유니버설 Windows)** 을 선택 합니다. 응용 프로그램 이름을 **AutoPlayCustomEvent** 로 확인 하 고 확인을 클릭 **합니다.**
2.  Appxmanifest.xml 파일을 열고 **기능** 탭을 선택 합니다. **이동식 저장소** 기능을 선택 합니다. 이를 통해 앱은 이동식 저장 장치의 파일 및 폴더에 액세스할 수 있습니다.
3.  매니페스트 파일에서 **선언** 탭을 선택 합니다. **사용 가능한 선언** 드롭다운 목록에서 **자동 실행 콘텐츠** 를 선택 하 고 **추가**를 클릭 합니다. **지원 되는 선언** 목록에 추가 된 새 **자동 실행 콘텐츠** 항목을 선택 합니다.

    **참고**    또는 사용자 지정 자동 재생 이벤트에 대 한 **자동 실행 장치** 선언을 추가 하도록 선택할 수도 있습니다.  
4.  **자동 실행 내용** 이벤트 선언에 대 한 **시작 작업** 섹션에서 첫 번째 시작 동작에 대해 아래 표에 있는 값을 입력 합니다.
5.  **사용 가능한 선언** 드롭다운 목록에서 **파일 유형 연결** 을 선택 하 고 **추가**를 클릭 합니다. 새 **파일 유형 연결** 선언의 속성에서 **표시 이름** 필드를 **. m s 파일 표시** 로 설정 하 고 **이름** 필드를 **ms \_ association**으로 설정 합니다. **지원 되는 파일 형식** 섹션에서 **새로 추가**를 클릭 합니다. **파일 형식** 필드를 **. m**s s로 설정 합니다. 콘텐츠 이벤트의 경우 자동으로 앱에 명시적으로 연결 되지 않은 파일 형식을 자동으로 필터링 합니다.
6.  매니페스트 파일을 저장하고 닫습니다.

| 설정             | 값                         |
|---------------------|-------------------------------|
| 동사                | 표시                          |
| 작업 표시 이름 | 파일 표시                    |
| 콘텐츠 이벤트       | AutoPlayCustomEventQuickstart |

**Content 이벤트** 값은 autorun 파일에서 **CustomEvent** 키에 대해 입력 한 텍스트입니다. **작업 표시 이름** 설정은 앱에 대해 자동 실행이 표시 하는 문자열을 식별 합니다. **동사** 설정은 선택 된 옵션에 대해 앱에 전달 되는 값을 식별 합니다. 자동 실행 이벤트에 대해 여러 실행 작업을 지정 하 고 **동사** 설정을 사용 하 여 사용자가 앱에 대해 선택한 옵션을 확인할 수 있습니다. 앱에 전달 된 startup 이벤트 인수의 **verb** 속성을 확인 하 여 사용자가 선택한 옵션을 확인할 수 있습니다. 을 사용 하는 경우를 제외 하 고는 예약 된 **open**을 사용 하 여 **동사** 설정에 모든 값을 사용할 수 있습니다.

### <a name="step-3-add-xaml-ui"></a>3 단계: XAML UI 추가

MainPage .xaml 파일을 열고 기본 그리드 섹션에 다음 XAML을 추가 합니다 &lt; &gt; .

```xml
<StackPanel Orientation="Vertical">
    <TextBlock FontSize="28" Margin="10,0,800,0">Files</TextBlock>
    <TextBlock x:Name="FilesBlock" FontSize="22" Height="600" Margin="10,0,800,0" />
</StackPanel>
```

### <a name="step-4-add-activation-code"></a>4 단계: 활성화 코드 추가

이 단계의 코드는 메서드를 호출 하 여 볼륨 장치의 루트 드라이브에 폴더를 표시 합니다. 자동 실행 콘텐츠 이벤트의 경우 자동 실행은 **Onfileactivated** 된 이벤트 중에 응용 프로그램에 전달 된 시작 인수에서 저장 장치의 루트 폴더를 전달 합니다. 이 폴더는 **Files** 속성의 첫 번째 요소에서 검색할 수 있습니다.

App.xaml.cs 파일을 열고 **App** 클래스에 다음 코드를 추가 합니다.

```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
    var rootFrame = Window.Current.Content as Frame;
    var page = rootFrame.Content as MainPage;

    // Call ShowFolders with root folder from device storage.
    page.DisplayFiles(args.Files[0] as Windows.Storage.StorageFolder);

    base.OnFileActivated(args);
}
```

> **참고**    `DisplayFiles`메서드는 다음 단계에서 추가 됩니다.

 

### <a name="step-5-add-code-to-display-folders"></a>5 단계: 폴더를 표시 하는 코드 추가

MainPage.xaml.cs 파일에서 **Mainpage** 클래스에 다음 코드를 추가 합니다.

```cs
internal async void DisplayFiles(Windows.Storage.StorageFolder folder)
{
    foreach (Windows.Storage.StorageFile f in await ReadFiles(folder, ".ms"))
    {
        FilesBlock.Text += "  " + f.Name + "\n";
    }
}

internal async System.Threading.Tasks.Task<IReadOnlyList<Windows.Storage.StorageFile>>
    ReadFiles(Windows.Storage.StorageFolder folder, string fileExtension)
{
    var options = new Windows.Storage.Search.QueryOptions();
    options.FileTypeFilter.Add(fileExtension);
    var query = folder.CreateFileQueryWithOptions(options);
    var files = await query.GetFilesAsync();

    return files;
}
```

### <a name="step-6-build-and-run-the-qpp"></a>6 단계: qpp를 빌드하고 실행 합니다.

1.  F5 키를 눌러 디버그 모드에서 앱을 빌드하고 배포 합니다.
2.  앱을 실행 하려면 메모리 카드나 다른 저장 장치를 PC에 삽입 합니다. 그런 다음 자동 실행 핸들러 옵션 목록에서 앱을 선택 합니다.

## <a name="autoplay-event-reference"></a>자동 재생 이벤트 참조


**자동 실행** 시스템을 통해 앱은 다양 한 장치 및 볼륨 (디스크) 도착 이벤트를 등록할 수 있습니다. **자동 실행** 콘텐츠 이벤트를 등록 하려면 패키지 매니페스트에서 **이동식 저장소** 기능을 사용 하도록 설정 해야 합니다. 다음 표에서는에 대해 등록할 수 있는 이벤트 및 발생 시기를 보여 줍니다.

| 시나리오                                                           | 이벤트                              | Description   |
|--------------------------------------------------------------------|------------------------------------|---------------|
| 카메라에서 사진 사용                                           | **WPD\ImageSource**                | Windows 휴대용 장치로 식별 되며 ImageSource 기능을 제공 하는 카메라에 대해 발생 합니다. |
| 오디오 플레이어에서 음악 사용                                     | **WPD\AudioSource**                | Windows 휴대용 장치로 식별 되며 오디오 소스 기능을 제공 하는 미디어 플레이어에 대해 발생 합니다. |
| 비디오 카메라에서 비디오 사용                                     | **WPD\VideoSource**                | Windows 휴대용 장치로 식별 되며 비디오 원본 기능을 제공 하는 비디오 카메라에 대해 발생 합니다. |
| 연결 된 플래시 드라이브 또는 외부 하드 드라이브 액세스              | **StorageOnArrival**               | 드라이브 또는 볼륨이 PC에 연결 된 경우 발생 합니다.   드라이브 또는 볼륨에 디스크 루트의 DCIM, AVCHD 또는 PRIVATE\ACHD 폴더가 포함 되어 있으면 **ShowPicturesOnArrival** 이벤트가 대신 발생 합니다. |
| 대용량 저장소에서 사진 사용 (레거시)                            | **ShowPicturesOnArrival**          | 드라이브 또는 볼륨에 디스크 루트의 DCIM, AVCHD 또는 PRIVATE\ACHD 폴더가 포함 되어 있을 때 발생 합니다. 사용자가 사용 하도록 설정한 IIf 자동 재생 제어판에서 **각 미디어 유형으로 수행할 작업을 선택** 하면 자동 재생에서 PC에 연결 된 볼륨을 검사 하 여 디스크의 콘텐츠 유형을 확인 합니다. 그림이 있으면 **ShowPicturesOnArrival** 이 발생 합니다. |
| 근접 공유를 사용 하 여 사진 받기 (탭 하 여 보내기)             | **ShowPicturesOnArrival**          | 사용자가 근접 (탭 하 여 보내기)을 사용 하 여 콘텐츠를 보내는 경우 자동 실행은 콘텐츠 형식을 확인 하는 공유 파일을 검사 합니다. 그림이 있으면 **ShowPicturesOnArrival** 이 발생 합니다. |
| 대용량 저장소에서 음악 사용 (레거시)                             | **PlayMusicFilesOnArrival**        | 사용자가 사용 하도록 설정 된 경우 자동 실행 제어판에서 **각 미디어 유형에 대해 수행할 작업을 선택** 하면 자동으로 PC에 연결 된 볼륨을 검사 하 여 디스크의 콘텐츠 유형을 확인 합니다.  음악 파일이 있으면 **PlayMusicFilesOnArrival** 이 발생 합니다. |
| 근접 공유를 사용 하 여 음악 받기 (탭 하 여 보내기)              | **PlayMusicFilesOnArrival**        | 사용자가 근접 (탭 하 여 보내기)을 사용 하 여 콘텐츠를 보내는 경우 자동 실행은 콘텐츠 형식을 확인 하는 공유 파일을 검사 합니다. 음악 파일이 있는 경우 **PlayMusicFilesOnArrival** 이 발생 합니다. |
| 대용량 저장소에서 비디오 사용 (레거시)                            | **PlayVideoFilesOnArrival**        | 사용자가 사용 하도록 설정 된 경우 자동 실행 제어판에서 **각 미디어 유형에 대해 수행할 작업을 선택** 하면 자동으로 PC에 연결 된 볼륨을 검사 하 여 디스크의 콘텐츠 유형을 확인 합니다. 비디오 파일이 있으면 **Playvideofilesonarrival** 이 발생 합니다. |
| 근접 공유를 사용 하 여 비디오 받기 (탭 하 여 보내기)             | **PlayVideoFilesOnArrival**        | 사용자가 근접 (탭 하 여 보내기)을 사용 하 여 콘텐츠를 보내는 경우 자동 실행은 콘텐츠 형식을 확인 하는 공유 파일을 검사 합니다. 비디오 파일을 찾았으면 **Playvideofilesonarrival** 이 발생 합니다. |
| 연결 된 장치에서 혼합 된 파일 집합 처리               | **MixedContentOnArrival**          | 사용자가 사용 하도록 설정 된 경우 자동 실행 제어판에서 **각 미디어 유형에 대해 수행할 작업을 선택** 하면 자동으로 PC에 연결 된 볼륨을 검사 하 여 디스크의 콘텐츠 유형을 확인 합니다. 특정 콘텐츠 형식 (예: 그림)이 없으면 **MixedContentOnArrival** 이 발생 합니다. |
| 근접 공유를 사용 하 여 혼합 된 파일 집합 처리 (탭 하 여 보내기) | **MixedContentOnArrival**          | 사용자가 근접 (탭 하 여 보내기)을 사용 하 여 콘텐츠를 보내는 경우 자동 실행은 콘텐츠 형식을 확인 하는 공유 파일을 검사 합니다. 특정 콘텐츠 형식 (예: 그림)이 없으면 **MixedContentOnArrival** 이 발생 합니다. |
| 광학 미디어의 비디오 처리                                    | **PlayDVDMovieOnArrival**<br />**PlayBluRayOnArrival**<br />**PlayVideoCDMovieOnArrival**<br />**PlaySuperVideoCDMovieOnArrival** | 디스크가 광학 드라이브에 삽입 되 면 자동으로 파일을 검사 하 여 콘텐츠 형식을 확인 합니다. 비디오 파일이 있으면 광학 디스크 유형에 해당 하는 이벤트가 발생 합니다. |
| 광학 미디어에서 음악 처리                                    | **Playcd오디오 On도착**<br />**PlayDVDAudioOnArrival** | 디스크가 광학 드라이브에 삽입 되 면 자동으로 파일을 검사 하 여 콘텐츠 형식을 확인 합니다. 음악 파일이 있으면 광학 디스크 유형에 해당 하는 이벤트가 발생 합니다. |
| 향상 된 디스크 재생                                                | **PlayEnhancedCDOnArrival**<br />**PlayEnhancedDVDOnArrival** | 디스크가 광학 드라이브에 삽입 되 면 자동으로 파일을 검사 하 여 콘텐츠 형식을 확인 합니다. 향상 된 디스크가 있으면 광학 디스크 유형에 해당 하는 이벤트가 발생 합니다. |
| 쓰기 가능한 광학 디스크 처리                                     | **HandleCDBurningOnArrival** <br />**HandleDVDBurningOnArrival** <br />**HandleBDBurningOnArrival** | 디스크가 광학 드라이브에 삽입 되 면 자동으로 파일을 검사 하 여 콘텐츠 형식을 확인 합니다. 쓰기 가능한 디스크가 있으면 광학 디스크 유형에 해당 하는 이벤트가 발생 합니다. |
| 다른 장치 또는 볼륨 연결을 처리 합니다.                       | **UnknownContentOnArrival**        | 자동 실행 콘텐츠 이벤트와 일치 하지 않는 콘텐츠가 있는 경우 모든 이벤트에 대해 발생 합니다. 이 이벤트는 사용 하지 않는 것이 좋습니다. 처리할 수 있는 특정 자동 실행 이벤트에 대해서만 응용 프로그램을 등록 해야 합니다. |

볼륨에 대 한 자동 실행 .inf 파일의 **CustomEvent** 항목을 사용 하 여 자동 실행에서 사용자 지정 자동 실행 콘텐츠 이벤트를 발생 시 키도 록 지정할 수 있습니다. 자세한 내용은 [자동 실행 .inf 항목](/windows/desktop/shell/autorun-cmds)을 참조 하세요.

앱에 대 한 appxmanifest.xml 파일에 확장을 추가 하 여 앱을 자동 실행 콘텐츠 또는 자동 재생 장치 이벤트 처리기로 등록할 수 있습니다. Visual Studio를 사용 하는 경우에는 **선언** 탭에서 **자동으로 실행 콘텐츠** 또는 **자동 실행 장치** 선언을 추가할 수 있습니다. 앱에 대 한 appxmanifest.xml 파일을 직접 편집 하는 경우 **windows. autoPlayContent** 또는 **Windows. Autoplaycontent** 를 **범주로**지정 하는 [**확장**](/uwp/schemas/appxpackage/appxmanifestschema/element-1-extension) 요소를 패키지 매니페스트에 추가 합니다. 예를 들어 패키지 매니페스트의 다음 항목은 **ShowPicturesOnArrival** 이벤트에 대 한 처리기로 앱을 등록 하는 **자동 실행 콘텐츠** 확장을 추가 합니다.

```xml
  <Applications>
    <Application Id="AutoPlayHandlerSample.App">
      <Extensions>
        <Extension Category="windows.autoPlayContent">
          <AutoPlayContent>
            <LaunchAction Verb="show" ActionDisplayName="Show Pictures"
                          ContentEvent="ShowPicturesOnArrival" />
          </AutoPlayContent>
        </Extension>
      </Extensions>
    </Application>
  </Applications>
```

 

 