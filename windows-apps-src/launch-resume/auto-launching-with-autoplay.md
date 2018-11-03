---
author: TylerMSFT
title: 자동 실행을 사용한 자동 시작
description: 자동 실행을 사용하면 사용자가 디바이스를 PC에 연결할 때 앱을 옵션으로 제공할 수 있습니다. 여기에는 카메라 또는 미디어 플레이어 등의 볼륨 이외의 디바이스나 USB 드라이브, SD 카드 또는 DVD 등의 볼륨 디바이스가 포함됩니다.
ms.assetid: AD4439EA-00B0-4543-887F-2C1D47408EA7
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 98c537ef3b2a5d002644cc554eae72b89a1799b0
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5980793"
---
# <a name="span-iddevlaunchresumeauto-launchingwithautoplayspanauto-launching-with-autoplay"></a><span id="dev_launch_resume.auto-launching_with_autoplay"></span>자동 실행을 사용한 자동 시작

**자동 실행**을 사용하면 사용자가 장치를 PC에 연결할 때 앱을 옵션으로 제공할 수 있습니다. 여기에는 카메라 또는 미디어 플레이어 등의 볼륨 이외의 장치나 USB 드라이브, SD 카드 또는 DVD 등의 볼륨 장치가 포함됩니다. 또한 **자동 실행**을 사용하면 사용자가 근접 연결(탭하기)을 사용하여 두 PC 간에 파일을 공유할 때 앱을 옵션으로 제공할 수 있습니다.

> **참고**장치 제조업체 이며 [Microsoft Store 장치 앱](http://go.microsoft.com/fwlink/p/?LinkID=301381) 을 장치에 대 한 **자동 실행** 처리기로 연결 하려는 경우 장치 메타 데이터에서 앱을 식별할 수 있습니다. 자세한 내용은 [Microsoft Store 장치 앱의 자동 실행](http://go.microsoft.com/fwlink/p/?LinkId=306684)을 참조하세요.

## <a name="register-for-autoplay-content"></a>자동 실행 콘텐츠 등록

**자동 실행** 콘텐츠 이벤트에 대한 옵션으로 앱을 등록할 수 있습니다. **자동 실행** 콘텐츠 이벤트는 카메라 메모리 카드, 썸 드라이브(thumb drive) 또는 DVD 같은 볼륨 장치가 PC에 삽입될 때 발생합니다. 여기서는 카메라에서 볼륨 장치가 삽입될 때 앱을 **자동 실행** 옵션으로 식별하는 방법을 보여 줍니다.

이 자습서에서는 이미지 파일을 표시하거나 사진에 복사하는 앱을 만들었습니다. 그리고 **ShowPicturesOnArrival** 콘텐츠 자동 실행 이벤트에 대해 앱을 등록했습니다.

또한 자동 실행에서는 근접 연결(탭하기)을 사용하여 PC 간에 공유된 콘텐츠에 대해 콘텐츠 이벤트를 발생시킵니다. 이 섹션의 단계와 코드를 사용하여 근접 연결을 사용하는 PC 간에 공유되는 파일을 처리할 수 있습니다. 다음 표에는 근접 연결을 사용하여 콘텐츠를 공유하는 데 사용할 수 있는 자동 실행 콘텐츠 이벤트가 나열되어 있습니다.

| 작업         | 콘텐츠 자동 실행 이벤트  |
|----------------|-------------------------|
| 음악 공유  | PlayMusicFilesOnArrival |
| 동영상 공유 | PlayVideoFilesOnArrival |

 
파일이 근접 연결을 사용하여 공유되는 경우 **FileActivatedEventArgs** 개체의 **Files** 속성에 모든 공유 파일이 있는 루트 폴더에 대한 참조가 포함됩니다.

### <a name="step-1-create-a-new-project-and-add-autoplay-declarations"></a>1단계: 새 프로젝트 만들기 및 자동 실행 선언 추가

1.  Microsoft Visual Studio를 열고 **파일** 메뉴에서 **새 프로젝트**를 선택합니다. **Visual C#** 섹션의 **Windows** 아래에서 **비어 있는 앱(범용 Windows)** 을 선택합니다. 앱의 이름을 **AutoPlayDisplayOrCopyImages**로 지정하고 **확인**을 클릭합니다.
2.  Package.appxmanifest 파일을 열고 **접근 권한 값** 탭을 엽니다. **이동식 저장소** 및 **그림 라이브러리** 접근 권한 값을 선택합니다. 이렇게 하면 앱이 카메라 메모리용 이동식 저장 장치와 로컬 사진에 액세스할 수 있습니다.
3.  매니페스트 파일에서 **선언** 탭을 선택합니다. **사용 가능한 선언** 드롭다운 목록에서 **자동 실행 콘텐츠**를 선택한 다음 **추가**를 클릭합니다. **지원되는 선언** 목록에 추가된 새 **자동 실행 콘텐츠** 항목을 선택합니다.
4.  **콘텐츠 자동 실행** 선언은 자동 실행에서 콘텐츠 이벤트를 발생시킬 때 옵션으로 앱을 식별합니다. 이벤트는 볼륨 장치(예: DVD 또는 USB 드라이브)의 콘텐츠를 기반으로 합니다. 자동 실행은 볼륨 장치의 콘텐츠를 검사한 후 발생시킬 콘텐츠 이벤트를 결정합니다. 볼륨의 루트에 DCIM, AVCHD 또는 PRIVATE\ACHD 폴더가 포함되어 있거나 사용자가 자동 실행 제어판에서 **각 미디어 유형으로 수행할 작업 선택**을 사용하도록 설정하고 볼륨의 루트에 사진이 있는 경우 자동 실행은 **ShowPicturesOnArrival** 이벤트를 발생시킵니다. **시작 작업** 섹션에서 첫 번째 시작 작업에 대해 아래 표 1에서 값을 입력합니다.
5.  **콘텐츠 자동 실행** 항목의 **시작 작업** 섹션에서 **새로 추가**를 클릭하여 두 번째 시작 작업을 추가합니다. 두 번째 시작 작업에 대해 아래 표 2의 값을 입력합니다.
6.  **사용 가능한 선언** 드롭다운 목록에서 **파일 형식 연결**을 선택하고 **추가**를 클릭합니다. 새 **파일 형식 연결** 선언에서 **표시 이름** 필드를 **AutoPlay Copy or Show Images**로 설정하고 **이름** 필드를 **image\_association1**로 설정합니다. **지원되는 파일 형식** 섹션에서 **새로 추가**를 클릭합니다. **파일 형식** 필드를 **.jpg**로 설정합니다. **지원되는 파일 형식** 섹션에서 새 파일 연결의 **파일 형식** 필드를 **.png**로 설정합니다. 콘텐츠 이벤트의 경우 자동 실행이 앱에 명시적으로 연결되지 않은 파일 형식을 필터링합니다.
7.  매니페스트 파일을 저장하고 닫습니다.

**표 1.**

| Setting             | 값                 |
|---------------------|-----------------------|
| 동사                | show                  |
| 작업 표시 이름 | 사진 표시         |
| 콘텐츠 이벤트       | ShowPicturesOnArrival |

**작업 표시 이름** 설정은 자동 실행이 앱에 대해 표시하는 문자열을 식별합니다. **동사** 설정은 선택한 옵션에 대해 앱에 전달된 값을 식별합니다. 자동 실행 이벤트에 대해 여러 개의 시작 작업을 지정하고 **동사** 설정을 사용하여 사용자가 앱에 대해 선택한 옵션을 확인할 수 있습니다. 앱에 전달된 시작 이벤트 인수의 **verb** 속성을 확인하여 사용자가 선택한 옵션을 알 수 있습니다. **동사** 설정에는 예약된 **open**을 제외한 모든 값을 사용할 수 있습니다.

**표 2.**  

| Setting             | 값                      |
|--------------------:|----------------------------|
| 동사                | copy                       |
| 작업 표시 이름 | Copy Pictures Into Library(사진을 라이브러리로 복사) |
| 콘텐츠 이벤트       | ShowPicturesOnArrival      |

### <a name="step-2-add-xaml-ui"></a>2단계: XAML UI 추가

MainPage.xaml 파일을 열고 다음 XAML을 기본 &lt;Grid&gt; 섹션에 추가합니다.

```xml
<TextBlock FontSize="18">File List</TextBlock>
<TextBlock x:Name="FilesBlock" HorizontalAlignment="Left" TextWrapping="Wrap"
           VerticalAlignment="Top" Margin="0,20,0,0" Height="280" Width="240" />
<Canvas x:Name="FilesCanvas" HorizontalAlignment="Left" VerticalAlignment="Top"
        Margin="260,20,0,0" Height="280" Width="100"/>
```

### <a name="step-3-add-initialization-code"></a>3단계: 초기화 코드 추가

이 단계의 코드는 **OnFileActivated** 이벤트 중 앱에 전달된 시작 인수에 중 하나인 **Verb** 속성에서 동사 값을 확인합니다. 그러면 코드는 사용자가 선택한 옵션과 관련된 메서드를 호출합니다. 카메라 메모리 이벤트의 경우 자동 실행은 카메라 저장소의 루트 폴더를 앱에 전달합니다. **Files** 속성의 첫 번째 요소에서 이 폴더를 검색할 수 있습니다.

App.xaml.cs 파일을 열고 다음 코드를 **App** 클래스에 추가합니다.

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

> **참고**의 `DisplayImages` 및 `CopyImages` 메서드는 다음 단계에서 추가 됩니다.

### <a name="step-4-add-code-to-display-images"></a>4단계: 이미지 표시 코드 추가

MainPage.xaml.cs 파일에서 **MainPage** 클래스에 다음 코드를 추가합니다.

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

### <a name="step-5-add-code-to-copy-images"></a>5단계: 이미지 복사 코드 추가

MainPage.xaml.cs 파일에서 **MainPage** 클래스에 다음 코드를 추가합니다.

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

### <a name="step-6-build-and-run-the-app"></a>6단계: 앱 빌드 및 실행

1.  F5 키를 눌러 앱을 빌드하고 배포합니다(디버그 모드).
2.  앱을 실행하려면 카메라 메모리 카드나 카메라의 다른 저장 장치를 PC에 삽입합니다. 그런 다음 자동 실행 옵션 목록에서 package.appxmanifest 파일에 지정한 콘텐츠 이벤트 옵션 중 하나를 선택합니다. 이 샘플 코드에서는 카메라 메모리 카드의 DCIM 폴더에 있는 사진을 표시하거나 복사하기만 합니다. 카메라 메모리 카드에서 사진을 AVCHD 또는 PRIVATE\\ACHD 폴더에 저장하는 경우 그에 따라 코드를 업데이트해야 합니다.
    **참고**루트에 **DCIM** 폴더 있으며 이미지 DCIM 폴더에 하위 폴더에 있는 경우 플래시 드라이브는 카메라 메모리 카드가 없는 경우 하는 경우 사용할 수 있습니다.

## <a name="register-for-an-autoplay-device"></a>장치 자동 실행을 위해 등록


앱을 **자동 실행** 장치 이벤트에 대한 옵션으로 등록할 수 있습니다. **자동 실행** 장치 이벤트는 장치가 PC에 연결될 때 발생합니다.

여기에서는 카메라가 PC에 연결될 때 앱을 **자동 실행** 옵션으로 식별하는 방법을 보여 줍니다. 앱은 **WPD\\ImageSourceAutoPlay** 이벤트에 대한 처리기로 등록됩니다. 이는 카메라 및 기타 이미징 장치가 MTP를 사용하는 ImageSource임을 알릴 경우 WPD(Windows 휴대용 장치)에서 발생하는 일반적인 이벤트입니다. 자세한 내용은 [Windows 휴대용 장치](https://msdn.microsoft.com/library/windows/hardware/ff597729)를 참조하세요.

**중요 한** [**Windows.Devices.Portable.StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654) Api는 [데스크톱 장치 제품군](https://msdn.microsoft.com/library/windows/apps/dn894631)의 일부입니다. 앱은 Pc와 같은 데스크톱 디바이스 패밀리의 Windows10 장치 에서만 이러한 Api를 사용할 수 있습니다.

 

### <a name="step-1-create-a-new-project-and-add-autoplay-declarations"></a>1단계: 새 프로젝트 만들기 및 자동 실행 선언 추가

1.  Visual Studio를 열고 **파일** 메뉴에서 **새 프로젝트**를 선택합니다. **Visual C#** 섹션의 **Windows** 아래에서 **비어 있는 앱(범용 Windows)** 을 선택합니다. 앱의 이름을 **AutoPlayDevice\_Camera**로 지정하고 **확인**을 클릭합니다.
2.  Package.appxmanifest 파일을 열고 **접근 권한 값** 탭을 엽니다. **이동식 저장소** 접근 권한 값을 선택합니다. 이렇게 하면 앱이 이동식 저장 볼륨 장치로 카메라의 데이터에 액세스할 수 있습니다.
3.  매니페스트 파일에서 **선언** 탭을 선택합니다. **사용 가능한 선언** 드롭다운 목록에서 **자동 실행 장치**를 선택한 다음 **추가**를 클릭합니다. **지원되는 선언** 목록에 추가된 새 **장치 자동 실행** 항목을 선택합니다.
4.  **장치 자동 실행** 선언은 자동 실행에서 알려진 이벤트에 대해 장치 이벤트를 발생시킬 때 옵션으로 앱을 식별합니다. **시작 작업** 섹션에서 첫 번째 시작 작업에 대해 아래 표의 값을 입력합니다.
5.  **사용 가능한 선언** 드롭다운 목록에서 **파일 형식 연결**을 선택하고 **추가**를 클릭합니다. 새 **파일 형식 연결** 선언에서 **표시 이름** 필드를 **Show Images from Camera**로 설정하고 **이름** 필드를 **camera\_association1**로 설정합니다. **지원되는 파일 형식** 섹션에서 필요한 경우 **새로 추가**를 클릭합니다. **파일 형식** 필드를 **.jpg**로 설정합니다. **지원되는 파일 형식** 섹션에서 **새로 추가**를 다시 클릭합니다. 새 파일 연결의 **파일 형식** 필드를 **.png**로 설정합니다. 콘텐츠 이벤트의 경우 자동 실행이 앱에 명시적으로 연결되지 않은 파일 형식을 필터링합니다.
6.  매니페스트 파일을 저장하고 닫습니다.

| Setting             | 값            |
|---------------------|------------------|
| 동사                | show             |
| 작업 표시 이름 | 사진 표시    |
| 콘텐츠 이벤트       | WPD\\ImageSource |

**작업 표시 이름** 설정은 자동 실행이 앱에 대해 표시하는 문자열을 식별합니다. **동사** 설정은 선택한 옵션에 대해 앱에 전달된 값을 식별합니다. 자동 실행 이벤트에 대해 여러 개의 시작 작업을 지정하고 **동사** 설정을 사용하여 사용자가 앱에 대해 선택한 옵션을 확인할 수 있습니다. 앱에 전달된 시작 이벤트 인수의 **verb** 속성을 확인하여 사용자가 선택한 옵션을 알 수 있습니다. **동사** 설정에는 예약된 **open**을 제외한 모든 값을 사용할 수 있습니다. 단일 앱에서 여러 동사를 사용하는 예제는 [콘텐츠 자동 실행을 위해 등록](#register-for-autoplay-content)을 참조하세요.

### <a name="step-2-add-assembly-reference-for-the-desktop-extensions"></a>2단계: 데스크톱 확장에 대한 어셈블리 참조 추가

Windows 휴대용 장치의 저장소에 액세스하는 데 필요한 API인 [**Windows.Devices.Portable.StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654)는 [데스크톱 디바이스 패밀리](https://msdn.microsoft.com/library/windows/apps/dn894631)의 일부입니다. 이는 API를 사용하려면 특수 어셈블리가 필요하고 해당 호출은 PC와 같은 데스크톱 디바이스 패밀리의 장치에서만 작동한다는 것입니다.

1.  **솔루션 탐색기**에서 **참조**를 마우스 오른쪽 단추로 클릭한 다음 **참조 추가...** 를 클릭합니다.
2.  **유니버설 Windows**를 확장하고 **확장**을 클릭합니다.
3.  그런 다음 **UWP용 Windows 데스크톱 확장**을 선택하고 **확인**을 클릭합니다.

### <a name="step-3-add-xaml-ui"></a>3단계: XAML UI 추가

MainPage.xaml 파일을 열고 다음 XAML을 기본 &lt;Grid&gt; 섹션에 추가합니다.

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

### <a name="step-4-add-activation-code"></a>4단계: 활성화 코드 추가

이 단계의 코드는 카메라의 장치 정보 ID를 [**FromId**](https://msdn.microsoft.com/library/windows/apps/br225655) 메서드에 전달하여 카메라를 [**StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654)로 참조합니다. 카메라의 장치 정보 ID는 먼저 이벤트 인수를 [**DeviceActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224710)로 캐스팅한 다음 [**DeviceInformationId**](https://msdn.microsoft.com/library/windows/apps/br224711) 속성에서 값을 가져와 얻을 수 있습니다.

App.xaml.cs 파일을 열고 다음 코드를 **App** 클래스에 추가합니다.

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

> **참고**의 `ShowImages` 메서드는 다음 단계에서 추가 됩니다.

### <a name="step-5-add-code-to-display-device-information"></a>5단계: 장치 정보 표시 코드 추가

[**StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654) 클래스의 속성에서 카메라에 대한 정보를 가져올 수 있습니다. 이 단계의 코드는 앱이 실행될 때 장치 이름 및 기타 정보를 사용자에게 표시합니다. 그런 후 다음 단계에서 추가할 GetImageList 및 GetThumbnail 메서드를 호출하여 카메라에 저장된 이미지의 미리 보기를 표시합니다.

MainPage.xaml.cs 파일에서 **MainPage** 클래스에 다음 코드를 추가합니다.

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

> **참고**의 `GetImageList` 및 `GetThumbnail` 메서드는 다음 단계에서 추가 됩니다.

### <a name="step-6-add-code-to-display-images"></a>6단계: 이미지 표시 코드 추가

이 단계의 코드는 카메라에 저장된 이미지의 미리 보기를 표시합니다. 이 코드는 카메라를 비동기 호출하여 미리 보기 이미지를 가져옵니다. 그러나 다음 비동기 호출은 이전 비동기 호출이 완료된 다음에야 실행됩니다. 따라서 카메라에 대해 한 번에 하나만 요청해야 합니다.

MainPage.xaml.cs 파일에서 **MainPage** 클래스에 다음 코드를 추가합니다.

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

### <a name="step-7-build-and-run-the-app"></a>7단계: 앱 빌드 및 실행

1.  F5 키를 눌러 앱을 빌드하고 배포합니다(디버그 모드).
2.  앱을 실행하려면 컴퓨터에 카메라를 연결합니다. 그런 다음 옵션의 자동 실행 목록에서 앱을 선택합니다.
    **참고** **WPD\\ImageSource** 자동 실행 장치 이벤트에 대 한 일부 카메라에서는 알립니다.

## <a name="configure-removable-storage"></a>이동식 저장소 구성

볼륨 장치가 PC에 연결된 경우 메모리 카드나 썸 드라이브(thumb drive) 같은 볼륨 장치를 **자동 실행** 장치로 식별할 수 있습니다. 이 기능은 **자동 실행**에 대한 특정 앱을 연결하여 사용자에게 볼륨 장치로 제공하려는 경우에 특히 유용합니다.

여기서는 볼륨 장치를 **자동 실행** 장치로 식별하는 방법을 보여 줍니다.

볼륨 디바이스를 **자동 실행** 디바이스로 식별하려면 디바이스의 루트 드라이브에 autorun.inf 파일을 추가합니다. autorun.inf 파일에서 **CustomEvent** 키를 **AutoRun** 섹션에 추가합니다. 볼륨 디바이스가 PC에 연결되면 **자동 실행**에서 autorun.inf 파일을 찾고 볼륨을 디바이스로 처리합니다. **자동 실행**에서 **CustomEvent** 키에 제공한 이름을 사용하여 **자동 실행** 이벤트를 만듭니다. 그런 다음 앱을 만들고 해당 **자동 실행** 이벤트에 대한 처리기로 등록할 수 있습니다. 장치가 PC에 연결되면 **자동 실행**에서 앱을 볼륨 장치에 대한 처리기로 표시합니다. autorun.inf 파일에 대한 자세한 내용은 [autorun.inf 항목](https://msdn.microsoft.com/library/windows/desktop/cc144200)을 참조하세요.

### <a name="step-1-create-an-autoruninf-file"></a>1단계: autorun.inf 파일 만들기

볼륨 디바이스의 루트 드라이브에서 autorun.inf라는 파일을 추가합니다. autorun.inf 파일을 열고 다음 텍스트를 추가합니다.

``` syntax
[AutoRun]
CustomEvent=AutoPlayCustomEventQuickstart
```

### <a name="step-2-create-a-new-project-and-add-autoplay-declarations"></a>2단계: 새 프로젝트 만들기 및 자동 실행 선언 추가

1.  Visual Studio를 열고 **파일** 메뉴에서 **새 프로젝트**를 선택합니다. **Visual C#** 섹션의 **Windows** 아래에서 **비어 있는 앱(범용 Windows)** 을 선택합니다. 응용 프로그램의 이름을 **AutoPlayCustomEvent**로 지정하고 **확인**을 클릭합니다.
2.  Package.appxmanifest 파일을 열고 **접근 권한 값** 탭을 엽니다. **이동식 저장소** 접근 권한 값을 선택합니다. 그러면 앱이 이동식 저장 장치의 파일 및 폴더에 액세스할 수 있습니다.
3.  매니페스트 파일에서 **선언** 탭을 선택합니다. **사용 가능한 선언** 드롭다운 목록에서 **자동 실행 콘텐츠**를 선택한 다음 **추가**를 클릭합니다. **지원되는 선언** 목록에 추가된 새 **자동 실행 콘텐츠** 항목을 선택합니다.

    **참고**또는 수도 있습니다 사용자 지정 자동 실행 이벤트에 대 한 **자동 실행 장치** 선언을 추가 합니다.  
4.  **콘텐츠 자동 실행** 이벤트 선언에 대한 **시작 작업** 섹션에서 첫 번째 시작 작업에 대해 아래 표의 값을 입력합니다.
5.  **사용 가능한 선언** 드롭다운 목록에서 **파일 형식 연결**을 선택하고 **추가**를 클릭합니다. 새 **파일 형식 연결** 선언에서 **표시 이름** 필드를 **Show .ms Files**로 설정하고 **이름** 필드를 **ms\_association**로 설정합니다. **지원되는 파일 형식** 섹션에서 **새로 추가**를 클릭합니다. **파일 형식** 필드를 **.ms**로 설정합니다. 콘텐츠 이벤트의 경우 자동 실행에서 앱에 명시적으로 연결되지 않은 파일 형식을 필터링합니다.
6.  매니페스트 파일을 저장하고 닫습니다.

| Setting             | 값                         |
|---------------------|-------------------------------|
| 동사                | show                          |
| 작업 표시 이름 | 파일 표시                    |
| 콘텐츠 이벤트       | AutoPlayCustomEventQuickstart |

**콘텐츠 이벤트** 값은 autorun.inf 파일에서 **CustomEvent** 키에 제공한 텍스트입니다. **작업 표시 이름** 설정은 자동 실행이 앱에 대해 표시하는 문자열을 식별합니다. **동사** 설정은 선택한 옵션에 대해 앱에 전달된 값을 식별합니다. 자동 실행 이벤트에 대해 여러 개의 시작 작업을 지정하고 **동사** 설정을 사용하여 사용자가 앱에 대해 선택한 옵션을 확인할 수 있습니다. 앱에 전달된 시작 이벤트 인수의 **verb** 속성을 확인하여 사용자가 선택한 옵션을 알 수 있습니다. **동사** 설정에는 예약된 **open**을 제외한 모든 값을 사용할 수 있습니다.

### <a name="step-3-add-xaml-ui"></a>3단계: XAML UI 추가

MainPage.xaml 파일을 열고 다음 XAML을 기본 &lt;Grid&gt; 섹션에 추가합니다.

```xml
<StackPanel Orientation="Vertical">
    <TextBlock FontSize="28" Margin="10,0,800,0">Files</TextBlock>
    <TextBlock x:Name="FilesBlock" FontSize="22" Height="600" Margin="10,0,800,0" />
</StackPanel>
```

### <a name="step-4-add-activation-code"></a>4단계: 활성화 코드 추가

이 단계의 코드에서는 메서드를 호출하여 볼륨 장치의 루트 드라이브에 폴더를 표시합니다. 콘텐츠 자동 실행 이벤트의 경우 자동 실행은 **OnFileActivated** 이벤트 중 응용 프로그램에 전달된 시작 인수에 저장 장치의 루트 폴더를 전달합니다. **Files** 속성의 첫 번째 요소에서 이 폴더를 검색할 수 있습니다.

App.xaml.cs 파일을 열고 다음 코드를 **App** 클래스에 추가합니다.

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

> **참고**의 `DisplayFiles` 메서드는 다음 단계에서 추가 됩니다.

 

### <a name="step-5-add-code-to-display-folders"></a>5단계: 폴더 표시 코드 추가

MainPage.xaml.cs 파일에서 **MainPage** 클래스에 다음 코드를 추가합니다.

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

### <a name="step-6-build-and-run-the-qpp"></a>6단계: 앱 빌드 및 실행

1.  F5 키를 눌러 앱을 빌드하고 배포합니다(디버그 모드).
2.  앱을 실행하려면 메모리 카드나 다른 저장 장치를 PC에 삽입합니다. 그런 다음 자동 실행 처리기 옵션 목록에서 앱을 선택합니다.

## <a name="autoplay-event-reference"></a>자동 실행 이벤트 참조


**자동 실행** 시스템을 사용하여 앱은 다양한 장치 및 볼륨(디스크) 도착 이벤트를 등록할 수 있습니다. **자동 실행** 콘텐츠 이벤트를 등록하려면 패키지 매니페스트에서 **이동식 저장소** 접근 권한 값을 사용하도록 설정해야 합니다. 다음 표에서는 등록할 수 있는 이벤트와 해당 이벤트가 발생하는 경우를 보여 줍니다.

| 시나리오                                                           | 이벤트                              | 설명   |
|--------------------------------------------------------------------|------------------------------------|---------------|
| 카메라에서 사진 사용                                           | **WPD\ImageSource**                | Windows 휴대용 장치로 식별된 카메라에 대해 발생하며 ImageSource 기능을 제공합니다. |
| 오디오 플레이어에서 음악 사용                                     | **WPD\AudioSource**                | Windows 휴대용 장치로 식별된 미디어 플레이어에 대해 발생하며 AudioSource 기능을 제공합니다. |
| 비디오 카메라에서 동영상 사용                                     | **WPD\VideoSource**                | Windows 휴대용 장치로 식별된 비디오 카메라에 대해 발생하며 VideoSource 기능을 제공합니다. |
| 연결된 플래시 드라이브 또는 외부 하드 드라이브 액세스              | **StorageOnArrival**               | 드라이브나 볼륨이 PC에 연결되면 발생합니다.   드라이브 또는 볼륨의 디스크 루트에 DCIM, AVCHD 또는 PRIVATE\ACHD 폴더가 포함되어 있으면 대신 **ShowPicturesOnArrival** 이벤트가 발생합니다. |
| 대용량 저장 장치(레거시)에서 사진 사용                            | **ShowPicturesOnArrival**          | 드라이브 또는 볼륨의 디스크 루트에 DCIM, AVCHD 또는 PRIVATE\ACHD 폴더가 포함되어 있을 경우 발생합니다. 사용자가 자동 실행 제어판에서 **각 미디어 유형으로 수행할 작업 선택**을 사용하도록 설정한 경우 자동 실행은 PC에 연결된 볼륨을 검사하여 디스크의 콘텐츠 유형을 확인합니다. 사진이 발견되면 **ShowPicturesOnArrival**이 발생합니다. |
| 근접 공유를 사용하여 사진 받기(탭하여 보내기)             | **ShowPicturesOnArrival**          | 사용자가 근접 연결(탭하여 보내기)을 사용하여 콘텐츠를 보내는 경우 자동 실행은 공유 파일을 검사하여 콘텐츠 유형을 확인합니다. 사진이 발견되면 **ShowPicturesOnArrival**이 발생합니다. |
| 대용량 저장 장치(레거시)에서 음악 사용                             | **PlayMusicFilesOnArrival**        | 사용자가 자동 실행 제어판에서 **각 미디어 유형으로 수행할 작업 선택**을 사용하도록 설정한 경우 자동 실행은 PC에 연결된 볼륨을 검사하여 디스크의 콘텐츠 유형을 확인합니다.  음악 파일이 발견되면 **PlayMusicFilesOnArrival**이 발생합니다. |
| 근접 공유를 사용하여 음악 받기(탭하여 보내기)              | **PlayMusicFilesOnArrival**        | 사용자가 근접 연결(탭하여 보내기)을 사용하여 콘텐츠를 보내는 경우 자동 실행은 공유 파일을 검사하여 콘텐츠 유형을 확인합니다. 음악 파일이 발견되면 **PlayMusicFilesOnArrival**이 발생합니다. |
| 대용량 저장 장치(레거시)에서 동영상 사용                            | **PlayVideoFilesOnArrival**        | 사용자가 자동 실행 제어판에서 **각 미디어 유형으로 수행할 작업 선택**을 사용하도록 설정한 경우 자동 실행은 PC에 연결된 볼륨을 검사하여 디스크의 콘텐츠 유형을 확인합니다. 동영상 파일이 발견되면 **PlayVideoFilesOnArrival**이 발생합니다. |
| 근접 공유를 사용하여 동영상 받기(탭하여 보내기)             | **PlayVideoFilesOnArrival**        | 사용자가 근접 연결(탭하여 보내기)을 사용하여 콘텐츠를 보내는 경우 자동 실행은 공유 파일을 검사하여 콘텐츠 유형을 확인합니다. 동영상이 발견되면 **PlayVideoFilesOnArrival**이 발생합니다. |
| 연결된 장치에서 혼합 파일 집합 처리               | **MixedContentOnArrival**          | 사용자가 자동 실행 제어판에서 **각 미디어 유형으로 수행할 작업 선택**을 사용하도록 설정한 경우 자동 실행은 PC에 연결된 볼륨을 검사하여 디스크의 콘텐츠 유형을 확인합니다. 특정 콘텐츠 형식(예: 사진)이 발견되지 않으면 **MixedContentOnArrival**이 발생합니다. |
| 근접 연결(탭하여 보내기)을 사용하여 혼합 파일 집합 처리 | **MixedContentOnArrival**          | 사용자가 근접 연결(탭하여 보내기)을 사용하여 콘텐츠를 보내는 경우 자동 실행은 공유 파일을 검사하여 콘텐츠 유형을 확인합니다. 특정 콘텐츠 형식(예: 사진)이 발견되지 않으면 **MixedContentOnArrival**이 발생합니다. |
| 광 미디어에서 동영상 처리                                    | **PlayDVDMovieOnArrival**<br />**PlayBluRayOnArrival**<br />**PlayVideoCDMovieOnArrival**<br />**PlaySuperVideoCDMovieOnArrival** | 광학 드라이브에 디스크를 삽입하면 자동 실행에서 파일을 검사하여 콘텐츠 종류를 확인합니다. 동영상 파일이 발견되면 광 디스크 종류에 해당하는 이벤트가 발생합니다. |
| 광 미디어에서 음악 처리                                    | **PlayCDAudioOnArrival**<br />**PlayDVDAudioOnArrival** | 광학 드라이브에 디스크를 삽입하면 자동 실행에서 파일을 검사하여 콘텐츠 종류를 확인합니다. 음악 파일이 발견되면 광 디스크 종류에 해당하는 이벤트가 발생합니다. |
| EDD 재생                                                | **PlayEnhancedCDOnArrival**<br />**PlayEnhancedDVDOnArrival** | 광학 드라이브에 디스크를 삽입하면 자동 실행에서 파일을 검사하여 콘텐츠 종류를 확인합니다. EDD가 발견되면 광 디스크 종류에 해당하는 이벤트가 발생합니다. |
| 쓰기 가능 광 디스크 처리                                     | **HandleCDBurningOnArrival** <br />**HandleDVDBurningOnArrival** <br />**HandleBDBurningOnArrival** | 광학 드라이브에 디스크를 삽입하면 자동 실행에서 파일을 검사하여 콘텐츠 종류를 확인합니다. 쓰기 가능한 디스크가 발견되면 광 디스크 종류에 해당하는 이벤트가 발생합니다. |
| 다른 장치 또는 볼륨 연결 처리                       | **UnknownContentOnArrival**        | 콘텐츠 자동 실행 이벤트와 일치하지 않는 콘텐츠가 발견되는 경우 모든 이벤트에 대해 발생합니다. 이 이벤트는 사용하지 않는 것이 좋습니다. 응용 프로그램이 처리할 수 있는 특정 자동 실행 이벤트에 대해서만 응용 프로그램을 등록해야 합니다. |

자동 실행이 볼륨의 autorun.inf 파일에 있는 **CustomEvent** 항목을 사용하여 사용자 지정 콘텐츠 자동 실행 이벤트를 발생시키도록 지정할 수 있습니다. 자세한 내용은 [Autorun.inf 항목](https://msdn.microsoft.com/library/windows/desktop/cc144200)을 참조하세요.

앱의 package.appxmanifest 파일에 확장명을 추가하여 앱을 콘텐츠 자동 실행 또는 장치 자동 실행 이벤트 처리기로 등록할 수 있습니다. Visual Studio를 사용하는 경우 **자동 실행 콘텐츠** 또는 **자동 실행 장치** 선언을 **선언** 탭에 추가할 수 있습니다. 앱의 package.appxmanifest 파일을 직접 편집하는 경우 **windows.autoPlayContent** 또는 **windows.autoPlayDevice**를 **범주**로 지정하는 패키지 매니페스트에 [**확장**](https://msdn.microsoft.com/library/windows/apps/br211400) 요소를 추가합니다. 예를 들어 패키지 매니페스트의 다음 항목은 **콘텐츠 자동 실행** 확장명을 추가하여 앱을 **ShowPicturesOnArrival** 이벤트에 대한 처리기로 등록합니다.

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

 

 
