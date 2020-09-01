---
ms.assetid: 374D1983-60E0-4E18-ABBB-04775BAA0F0D
title: 앱에서 스캔
description: 평판, 급지대 또는 자동 구성 된 스캔 소스를 사용 하 여 앱에서 콘텐츠를 검색 하는 방법에 대해 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c306c225d200fe0636b3195699afe0441bc252bf
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175437"
---
# <a name="scan-from-your-app"></a>앱에서 스캔


**중요 API**

-   [**Windows. 장치. 스캐너**](/uwp/api/Windows.Devices.Scanners)
-   [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)
-   [**DeviceClass**](/uwp/api/Windows.Devices.Enumeration.DeviceClass)

평판, 급지대 또는 자동 구성 된 스캔 소스를 사용 하 여 앱에서 콘텐츠를 검색 하는 방법에 대해 알아봅니다.

**중요**    [**Windows. 스캐너**](/uwp/api/Windows.Devices.Scanners) api는 데스크톱 [장치 제품군](../get-started/universal-application-platform-guide.md)의 일부입니다. 앱은 Windows 10 데스크톱 버전 에서만 이러한 Api를 사용할 수 있습니다.

앱에서 검색 하려면 먼저 새 [**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 개체를 선언 하 고 [**deviceclass**](/uwp/api/Windows.Devices.Enumeration.DeviceClass) 유형을 가져와 사용 가능한 스캐너를 나열 해야 합니다. WIA 드라이버를 사용 하 여 로컬로 설치 된 스캐너만 앱에 나열 되 고 사용할 수 있습니다.

앱이 사용 가능한 스캐너에 나열 된 후 스캐너 유형을 기반으로 자동 구성 된 검사 설정을 사용 하거나 사용 가능한 평판 또는 공급 장치 검색 원본을 사용 하 여 스캔할 수 있습니다. 자동 구성 된 설정을 사용 하려면 스캐너에서 자동 구성을 사용할 수 있도록 설정 해야 합니다. 평판 및 급지대 스캐너를 모두 사용 하도록 설정 해야 합니다. 자세한 내용은 [자동 구성 된 검색](/windows-hardware/drivers/image/auto-configured-scanning)을 참조 하세요.

## <a name="enumerate-available-scanners"></a>사용 가능한 스캐너 열거

Windows에서는 자동으로 스캐너를 검색 하지 않습니다. 앱이 스캐너와 통신 하기 위해이 단계를 수행 해야 합니다. 이 예제에서 스캐너 장치 열거는 [**Windows. Devices**](/uwp/api/Windows.Devices.Enumeration) 네임 스페이스를 사용 하 여 수행 됩니다.

1.  먼저 클래스 정의 파일에 이러한 using 문을 추가 합니다.

``` csharp
    using Windows.Devices.Enumeration;
    using Windows.Devices.Scanners;
```

2.  다음으로, 스캐너 열거를 시작 하려면 장치 감시자를 구현 합니다. 자세한 내용은 [장치 열거](enumerate-devices.md)를 참조 하세요.

```csharp
    void InitDeviceWatcher()
    {
       // Create a Device Watcher class for type Image Scanner for enumerating scanners
       scannerWatcher = DeviceInformation.CreateWatcher(DeviceClass.ImageScanner);

       scannerWatcher.Added += OnScannerAdded;
       scannerWatcher.Removed += OnScannerRemoved;
       scannerWatcher.EnumerationCompleted += OnScannerEnumerationComplete;
    }
```

3.  스캐너가 추가 된 경우에 대 한 이벤트 처리기를 만듭니다.

```csharp
    private async void OnScannerAdded(DeviceWatcher sender,  DeviceInformation deviceInfo)
    {
       await
       MainPage.Current.Dispatcher.RunAsync(
             Windows.UI.Core.CoreDispatcherPriority.Normal,
             () =>
             {
                MainPage.Current.NotifyUser(String.Format("Scanner with device id {0} has been added", deviceInfo.Id), NotifyType.StatusMessage);

                // search the device list for a device with a matching device id
                ScannerDataItem match = FindInList(deviceInfo.Id);

                // If we found a match then mark it as verified and return
                if (match != null)
                {
                   match.Matched = true;
                   return;
                }

                // Add the new element to the end of the list of devices
                AppendToList(deviceInfo);
             }
       );
    }
```

## <a name="scan"></a>검사

1.  **ImageScanner 개체 가져오기**

[**ImageScannerScanSource**](/uwp/api/Windows.Devices.Scanners.ImageScannerScanSource) 열거형 **형식 (** **기본값**, 자동 구성, **평판**또는 **공급 장치**)에 관계 없이 먼저 다음과 같이 [**FromIdAsync**](/uwp/api/windows.devices.scanners.imagescanner.fromidasync) 메서드를 호출 하 여 [**ImageScanner**](/uwp/api/Windows.Devices.Scanners.ImageScanner) 개체를 만들어야 합니다.

 ```csharp
    ImageScanner myScanner = await ImageScanner.FromIdAsync(deviceId);
 ```

2.  **검색만**

기본 설정을 사용 하 여 검색 하려면 앱이 [**Windows.**](/uwp/api/Windows.Devices.Scanners) n a t a. n a t 네임 스페이스를 사용 하 여 스캐너를 선택 하 고 해당 소스에서 스캔 합니다. 검색 설정이 변경 되지 않습니다. 가능한 스캐너는 자동 구성, 평판 또는 공급 장치입니다. 이 유형의 검색은 일반적으로 급지대 대신 평판 처럼 잘못 된 소스에서 검색 하는 경우에도 성공적인 검색 작업을 생성할 수 있습니다.

**참고**    사용자가 급지대에서 스캔할 문서를 배치 하면 스캐너에서 평판을 스캔 하 게 됩니다. 사용자가 빈 급지대에서 스캔 하려고 하면 검색 작업에서 검사 된 파일을 생성 하지 않습니다.
 
```csharp
    var result = await myScanner.ScanFilesToFolderAsync(ImageScannerScanSource.Default,
        folder).AsTask(cancellationToken.Token, progress);
```

3.  **자동 구성 된, 평판 또는 공급 장치 원본에서 스캔**

앱은 장치의 [자동 구성](/windows-hardware/drivers/image/auto-configured-scanning) 된 검색을 사용 하 여 가장 최적의 검색 설정을 검색할 수 있습니다. 이 옵션을 사용 하는 경우 장치 자체에서 검색 되는 콘텐츠를 기준으로 색 모드 및 스캔 해상도와 같은 최상의 검색 설정을 결정할 수 있습니다. 장치는 각 새 검색 작업에 대해 런타임에 검색 설정을 선택 합니다.

**참고**    모든 스캐너가이 기능을 지 원하는 것은 아니므로 앱은이 설정을 사용 하기 전에 스캐너가이 기능을 지원 하는지 확인 해야 합니다.

이 예제에서 앱은 먼저 스캐너에서 자동 구성을 수행할 수 있는지 확인 한 다음 스캔 합니다. 평판 또는 공급 장치 스캐너를 지정 하려면 **자동으로 구성 된를** **평판** 또는 **공급 장치**로 대체 하기만 하면 됩니다.

```csharp
    if (myScanner.IsScanSourceSupported(ImageScannerScanSource.AutoConfigured))
    {
        ...
        // Scan API call to start scanning with Auto-Configured settings.
        var result = await myScanner.ScanFilesToFolderAsync(
            ImageScannerScanSource.AutoConfigured, folder).AsTask(cancellationToken.Token, progress);
        ...
    }
```

## <a name="preview-the-scan"></a>검사 미리 보기

폴더를 검색 하기 전에 검사를 미리 보는 코드를 추가할 수 있습니다. 아래 예제에서 앱은 **평판** 스캐너가 미리 보기를 지원 하는지 확인 하 고 검색을 미리 봅니다.

```csharp
if (myScanner.IsPreviewSupported(ImageScannerScanSource.Flatbed))
{
    rootPage.NotifyUser("Scanning", NotifyType.StatusMessage);
                // Scan API call to get preview from the flatbed.
                var result = await myScanner.ScanPreviewToStreamAsync(
                    ImageScannerScanSource.Flatbed, stream);
```

## <a name="cancel-the-scan"></a>검사 취소

사용자가 다음과 같이 검색을 통해 중간에 검색 작업을 취소 하도록 할 수 있습니다.

```csharp
void CancelScanning()
{
    if (ModelDataContext.ScenarioRunning)
    {
        if (cancellationToken != null)
        {
            cancellationToken.Cancel();
        }                
        DisplayImage.Source = null;
        ModelDataContext.ScenarioRunning = false;
        ModelDataContext.ClearFileList();
    }
}
```

## <a name="scan-with-progress"></a>검사 진행 중

1.  **CancellationTokenSource** 개체를 만듭니다.

```csharp
cancellationToken = new CancellationTokenSource();
```

2.  진행률 이벤트 처리기를 설정 하 고 검사의 진행률을 가져옵니다.

```csharp
    rootPage.NotifyUser("Scanning", NotifyType.StatusMessage);
    var progress = new Progress<UInt32>(ScanProgress);
```

## <a name="scanning-to-the-pictures-library"></a>그림 라이브러리로 스캔 하는 중

사용자는 [**Folderpicker**](/uwp/api/Windows.Storage.Pickers.FolderPicker) 클래스를 사용 하 여 폴더를 동적으로 검색할 수 있지만 사용자가 해당 폴더를 검색할 수 있도록 매니페스트에서 *그림 라이브러리* 기능을 선언 해야 합니다. 앱 기능에 대 한 자세한 내용은 [앱 기능 선언](../packaging/app-capability-declarations.md)을 참조 하세요.