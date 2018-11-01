---
author: PatrickFarley
ms.assetid: 374D1983-60E0-4E18-ABBB-04775BAA0F0D
title: 앱에서 스캔
description: 여기서는 평판, 문서 공급 장치 또는 자동 구성된 스캔 소스를 사용하여 앱에서 콘텐츠를 스캔하는 방법에 대해 알아봅니다.
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f9128056cbb3b9218d164b243948d9dd16af0786
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5924392"
---
# <a name="scan-from-your-app"></a>앱에서 스캔


**중요 API**

-   [**Windows.Devices.Scanners**](https://msdn.microsoft.com/library/windows/apps/Dn264250)
-   [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)
-   [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381)

여기서는 평판, 문서 공급 디바이스 또는 자동 구성된 스캔 소스를 사용하여 앱에서 콘텐츠를 스캔하는 방법에 대해 알아봅니다.

**중요 한** [**Windows.Devices.Scanners**](https://msdn.microsoft.com/library/windows/apps/Dn264250) Api는 데스크톱 [장치 제품군](https://msdn.microsoft.com/library/windows/apps/Dn894631)의 일부입니다. 앱은 Windows10의 데스크톱 버전 에서만 이러한 Api를 사용할 수 있습니다.

앱에서 스캔하려면 먼저 새 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 개체를 선언하고 [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381) 형식을 가져와서 사용 가능한 스캐너를 나열해야 합니다. WIA 드라이버를 사용하여 로컬로 설치된 스캐너만 나열되고 앱에서 사용할 수 있습니다.

앱에서 사용 가능한 스캐너가 나열된 후에는 스캐너 형식에 따라 자동 구성된 스캔 설정을 사용하거나, 사용 가능한 평판 또는 문서 공급 장치 스캔 소스를 사용하여 스캔할 수 있습니다. 자동 구성된 설정을 사용하려면 스캐너에서 자동 구성이 지원되어야 하며 평판 스캐너와 문서 공급 장치 스캐너 중 하나만 장착되어 있어야 합니다. 자세한 내용은 [자동 구성된 스캔](https://msdn.microsoft.com/library/windows/hardware/Ff539393)을 참조하세요.

## <a name="enumerate-available-scanners"></a>사용할 수 있는 스캐너 열거

Windows에서는 스캐너를 자동으로 검색하지 않습니다. 앱이 스캐너와 통신하려면 이 단계를 수행해야 합니다. 이 예제에서는 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) 네임스페이스를 사용하여 스캐너 장치를 열거합니다.

1.  먼저 다음과 같은 using 문을 클래스 정의 파일에 추가합니다.

``` csharp
    using Windows.Devices.Enumeration;
    using Windows.Devices.Scanners;
```

2.  다음에는 장치 감시자를 구현하여 스캐너 열거를 시작합니다. 자세한 내용은 [디바이스 열거](enumerate-devices.md)를 참조하세요.

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

3.  스캐너가 추가되는 시점에 대한 이벤트 처리기를 만듭니다.

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

## <a name="scan"></a>스캔

1.  **ImageScanner 개체 가져오기**

각 [**ImageScannerScanSource**](https://msdn.microsoft.com/library/windows/apps/Dn264238) 열거 형식(**Default**, **AutoConfigured**, **Flatbed** 또는 **Feeder**)에 대해 먼저 다음과 같이 [**ImageScanner.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.scanners.imagescanner.fromidasync) 메서드를 호출하여 [**ImageScanner**](https://msdn.microsoft.com/library/windows/apps/Dn263806) 개체를 만들어야 합니다.

 ```csharp
    ImageScanner myScanner = await ImageScanner.FromIdAsync(deviceId);
 ```

2.  **스캔만**

기본 설정으로 스캔하려는 경우 앱에서는 [**Windows.Devices.Scanners**](https://msdn.microsoft.com/library/windows/apps/Dn264250) 네임스페이스에 의존하여 스캐너를 선택하고 해당 소스에서 스캔합니다. 스캔 설정은 변경되지 않습니다. 사용 가능한 스캐너로는 자동 구성, 평판 또는 공급 장치가 있습니다. 이러한 스캔 유형은 공급 디바이스 대신에 평판을 선택하는 등 잘못된 소스에서 스캔하더라도 스캔이 성공적으로 수행될 가능성이 높습니다.

**참고**사용자에 스캔할 문서 공급 장치에서 스캔을 스캐너가 평판에서 놓으면 스캐너가 스캔 합니다. 사용자가 빈 공급 디바이스에서 스캔하려고 하면 스캔된 파일이 생성되지 않습니다.
 
```csharp
    var result = await myScanner.ScanFilesToFolderAsync(ImageScannerScanSource.Default,
        folder).AsTask(cancellationToken.Token, progress);
```

3.  **자동 구성, 평판 또는 공급 장치 소스에서 스캔**

앱에서는 장치의 [자동 구성된 스캔](https://msdn.microsoft.com/library/windows/hardware/Ff539393)을 사용하여 최적의 스캔 설정으로 스캔할 수 있습니다. 이 옵션을 사용하면 스캔되는 콘텐츠에 따라 컬러 모드와 스캔 해상도 같은 최적의 스캔 설정을 장치 자체에서 결정할 수 있습니다. 디바이스는 각각의 새로운 스캔 작업마다 런타임 시 스캔 설정을 선택합니다.

**참고**모든 스캐너 앱 스캐너가이 설정을 사용 하기 전에이 기능을 지원 하는지 확인 해야 하므로이 기능을 지원 합니다.

이 예제에서 앱은 먼저 스캐너가 자동 구성 기능이 있는지 여부를 확인하고 나서 스캔합니다. 평판 스캐너 또는 문서 공급 디바이스 스캐너를 지정하려면 **AutoConfigured**를 **Flatbed** 또는 **Feeder**로 바꾸면 됩니다.

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

## <a name="preview-the-scan"></a>스캔 미리 보기

폴더로 스캔하기 전에 스캔을 미리 보는 코드를 추가할 수 있습니다. 아래 예제에서 앱은 **Flatbed** 스캐너가 미리 보기를 지원하는지 확인하고 나서 스캔 미리 보기를 표시합니다.

```csharp
if (myScanner.IsPreviewSupported(ImageScannerScanSource.Flatbed))
{
    rootPage.NotifyUser("Scanning", NotifyType.StatusMessage);
                // Scan API call to get preview from the flatbed.
                var result = await myScanner.ScanPreviewToStreamAsync(
                    ImageScannerScanSource.Flatbed, stream);
```

## <a name="cancel-the-scan"></a>스캔 취소

다음과 같이 사용자가 원할 경우 스캔 도중에 스캔 작업을 취소하도록 할 수 있습니다.

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

## <a name="scan-with-progress"></a>스캔 진행률 표시

1.  **System.Threading.CancellationTokenSource** 개체를 만듭니다.

```csharp
cancellationToken = new CancellationTokenSource();
```

2.  진행률 이벤트 처리기를 설정하고 스캔 진행률을 가져옵니다.

```csharp
    rootPage.NotifyUser("Scanning", NotifyType.StatusMessage);
    var progress = new Progress<UInt32>(ScanProgress);
```

## <a name="scanning-to-the-pictures-library"></a>사진 라이브러리로 스캔

사용자는 [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/BR207881) 클래스를 사용하여 동적으로 폴더로 스캔할 수 있지만, 사용자가 해당 폴더로 스캔할 수 있도록 하려면 매니페스트에 *사진 라이브러리* 기능을 선언해야 합니다. 앱 기능에 대한 자세한 내용은 [앱 기능 선언](https://msdn.microsoft.com/library/windows/apps/Mt270968)을 참조하세요.
