---
title: 카메라 바코드 스캐너 시작하기
description: 카메라 바코드 스캐너를 사용 하는 방법 학습
ms.date: 09/02/2019
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: b35ff6b183a6344fbc8da6b44a6cb81ea695a1c9
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243298"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>카메라 바코드 스캐너 시작하기

여기에 사용 된 코드 조각은 데모용 으로만 사용 됩니다. 작업 예제는 [바코드 스캐너 샘플](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)을 참조 하세요.

## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>1단계: 앱 매니페스트에 기능 선언 추가

1. Microsoft Visual Studio의 **솔루션 탐색기**에서 **package.appxmanifest** 항목을 두 번 클릭하여 응용 프로그램 매니페스트 디자이너를 엽니다.
2. **기능** 탭 선택
3. **웹캠** 및 **PointOfService** 확인란을 클릭

>[!NOTE]
> 소프트웨어 디코더가 카메라에서 프레임을 수신하여 디코딩을 수행하고 응용 프로그램에서 미리 보기를 제공하려면 **웹캠** 기능이 필요합니다.

## <a name="step-2-add-using-directives"></a>2단계: Using 지시문 추가

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;
```

## <a name="step-3-define-your-device-selector"></a>3단계: 장치 선택기 정의

### <a name="option-a-find-all-barcode-scanners"></a>**옵션 A: 모든 바코드 스캐너 찾기**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();
```

### <a name="option-b-scoping-device-selector-to-connection-type"></a>**옵션 B: 장치 선택기 범위를 연결 형식으로 지정**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-all-barcode-scanners"></a>4단계: 모든 바코드 스캐너 열거

응용 프로그램의 수명 동안 장치 목록이 변경 되지 않을 것으로 예상 되는 경우 FindAllAsync를 사용 하 여 스냅숏을 한 번만 열거할 수 있습니다 [.](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)그러나 바코드 스캐너 목록이 다음 기간 동안 변경 될 수 있다고 생각 되는 경우 응용 프로그램 대신 [Devicewatcher](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) 를 사용 해야 합니다.  

> [!Important]
> PointOfService 장치를 나열하기 위해 GetDefaultAsync를 사용하면 클래스에서 확인된 첫 번째 장치만 반환하면 세션 간의 변경이 가능하기 때문에 일관되지 않은 동작이 유발될 수 있습니다.

### <a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>**옵션 A: 바코드 스캐너의 스냅숏 열거**

```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> *FindAllAsync* 사용에 대한 자세한 내용은 [*장치의 스냅숏 나열*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-a-snapshot-of-devices)을 참조하세요.

### <a name="option-b-enumerate-available-barcode-scanners-and-watch-for-changes-to-the-available-scanners"></a>**옵션 B: 사용 가능한 바코드 스캐너를 열거 하 고 사용 가능한 스캐너에 대 한 변경 내용을 시청 하세요.**

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
watcher.Added += Watcher_Added;
watcher.Removed += Watcher_Removed;
watcher.Updated += Watcher_Updated;
watcher.Start();
```

> [!TIP]
> 자세한 내용은 [*장치 변경 사항 나열 및 모니터링*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices)과 [*DeviceWatcher*](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)를 참조하세요.

## <a name="step-5-identify-camera-barcode-scanners"></a>5단계: 카메라 바코드 스캐너 식별

카메라 바코드 스캐너는 Windows가 컴퓨터에 연결된 카메라(들)를 소프트웨어 디코더에 페어링하는 과정에서 역동적으로 생성됩니다.  카메라와 디코더를 페어링할 때마다 바코드 스캐너가 완벽하게 작동합니다.

결과 장치 컬렉션의 각 바코드 스캐너에 대해, 바코드 [*스캐너. VideoDeviceID*](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId) 속성을 선택 하 여 카메라 바코드 스캐너와 실제 바코드 스캐너를 구분할 수 있습니다.  NULL이 아닌 VideoDeviceID는 장치 컬렉션의 바코드 스캐너 개체가 카메라 바코드 스캐너 임을 나타냅니다.  카메라 바코드가 두 개 이상 있는 경우, 실제 바코드 스캐너를 제외 하는 별도의 컬렉션을 빌드할 수 있습니다.

Windows와 함께 제공 되는 디코더를 사용 하는 카메라 바코드 스캐너는 다음과 같이 식별 됩니다.

> Microsoft BarcodeScanner(*여기 카메라 이름*)

둘 이상의 카메라가 있고 컴퓨터 섀시에 기본 제공 되는 경우에는 이름이 *front* 카메라와 *후면* 카메라를 구분할 수 있습니다.

> [!NOTE]
> 나중에 다른 명명 스키마를 사용 하는 추가 소프트웨어 디코더가 해제 될 수 있습니다.

DeviceWatcher가 시작 되 면 (4 단계) 연결 된 각 장치를 열거 합니다. 여기서는 사용 가능한 스캐너를 바코드 스캐너 컬렉션에 추가 하 고 컬렉션을 ListBox에 바인딩합니다.

```csharp
ObservableCollection<BarcodeScannerInfo> barcodeScanners = new ObservableCollection<BarcodeScannerInfo>();

private async void Watcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        barcodeScanners.Add(new BarcodeScannerInfo(args.Name, args.Id));

        // Select the first scanner by default.
        if (barcodeScanners.Count == 1)
        {
            ScannerListBox.SelectedIndex = 0;
        }
    });
}
```

ListBox의 SelectedIndex가 변경 되 면 (이전 코드 조각에서 첫 번째 항목이 기본적으로 선택 됨) 장치 정보를 쿼리 합니다.

```csharp
private async void ScannerSelection_Changed(object sender, SelectionChangedEventArgs args)
{
    var selectedScannerInfo = (BarcodeScannerInfo)args.AddedItems[0];
    var deviceId = selectedScannerInfo.DeviceId;

    await SelectScannerAsync(deviceId);
}
```

## <a name="step-6-claim-the-camera-barcode-scanner"></a>6단계: 카메라 바코드 스캐너 클레임

[BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync)를 사용하여 카메라 바코드 스캐너에 대한 배타적 이용권을 획득합니다.

```csharp
private async Task SelectScannerAsync(string scannerDeviceId)
{
    selectedScanner = await BarcodeScanner.FromIdAsync(scannerDeviceId);

    if (selectedScanner != null)
    {
        claimedScanner = await selectedScanner.ClaimScannerAsync();
        if (claimedScanner != null)
        {
            await claimedScanner.EnableAsync();
        }
        else
        {
            rootPage.NotifyUser("Failed to claim the selected barcode scanner", NotifyType.ErrorMessage);
        }
    }
    else
    {
        rootPage.NotifyUser("Failed to create a barcode scanner object", NotifyType.ErrorMessage);
    }
}
```

## <a name="step-7-system-provided-preview"></a>7단계: 시스템 제공 미리 보기

사용자가 바코드에서 성공적으로 카메라를 겨냥하려면 카메라 미리 보기가 필요합니다.  Windows에서는 카메라 바코드 스캐너의 기본 컨트롤에 대 한 대화 상자를 시작 하는 간단한 카메라 미리 보기를 제공 합니다.  대화 상자를 열려면 [ClaimedBarcodeScanner.ShowVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync)를, 종료 시 대화 상자를 닫으려면 [ClaimedBarcodeScanner.HideVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview)를 호출하세요.

> [!TIP]
> 응용 프로그램에서 카메라 바코드 스캐너에 대한 미리 보기를 호스팅하는 방법은 [미리 보기 호스팅](pos-camerabarcode-hosting-preview.md)을 참조하세요.

## <a name="step-8-initiate-scan"></a>8단계: 검색 시작

[  **StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync)을 호출하여 스캔 프로세스를 시작할 수 있습니다.

[**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 의 값에 따라 스캐너가 단일 바코드를 검색 한 후 [**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync)를 호출할 때까지 계속 해 서 중지 하거나 검색할 수 있습니다.

바코드가 디코딩될 때 스캐너 동작을 제어하도록 [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived)를 원하는 값으로 설정합니다.

| 값 | 설명 |
| ----- | ----------- |
| True   | 오직 하나의 바코드만 스캔하고 중지 |
| False  | 중단 없이 지속적으로 바코드 스캔 |

## <a name="see-also"></a>참조

### <a name="samples"></a>샘플

- [바코드 스캐너 샘플](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
