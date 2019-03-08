---
title: PointOfService 장치 열거
description: PointOfService 디바이스를 열거하는 메서드를 알아봅니다.
ms.date: 10/08/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 27d25864941b9d73c9b12e6329eab79fac1b15bf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620908"
---
# <a name="enumerating-point-of-service-devices"></a>서비스 지점 장치 열거
이 섹션에서는 알아봅니다 하는 방법 [장치 선택기 정의](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) 시스템에 사용할 수 있는 장치를 쿼리하고이 선택기를 사용 하 여 다음 방법 중 하나를 사용 하 여 서비스 지점 장치를 열거할 수 하는 데 사용 되는:

**방법 1:** [장치 선택기를 사용 하 여](#method-1-use-a-device-picker)
<br/>
장치 선택 UI를 표시 하 고 연결 된 장치 사용자에 게 키를 누릅니다. 이 메서드는 목록을 업데이트 하는 장치는 연결 되 고 제거 하는 경우를 처리 하 고 간단 하 고 다른 방법 보다 안전 하 게 됩니다.

**방법 2:** [첫 번째 사용 가능한 장치 가져오기](#method-2-get-first-available-device)<br />사용 하 여 [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync) 특정 서비스 지점 장치 클래스에서 사용 가능한 첫 번째 장치에 액세스할 수 있습니다.

**방법 3:** [장치의 스냅숏](#method-3-snapshot-of-devices)<br />스냅숏 시간에 지정된 된 시점에 시스템에 있는 서비스 지점 장치를 열거 합니다. 고유한 UI를 빌드하거나 사용자에게 UI를 표시하지 않고 장치를 열거하고자 할 때 유용합니다. [FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 보유할 다시 결과 열거 하는 완료 될 때까지 합니다.

**방법 4:** [열거 하 고 시청](#method-4-enumerate-and-watch)<br />[DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) 현재 존재 하는 장치를 열거 하 고 장치 추가 되거나 시스템에서 제거 되 면 알림을 받을 수도 할 수 있는 보다 강력 하 고 유연한 열거형 모델입니다.  스냅샷이 표시되기까지 기다리는 것보다 UI에 표시하기 위해 백그라운드에서 디바이스의 현재 목록을 유지하려는 경우 유용합니다.

## <a name="define-a-device-selector"></a>장치 선택기 정의
장치 선택기는 디바이스를 열거할 때 검색하는 디바이스를 제한할 수 있습니다.  이렇게 하면만 관련 결과 가져오고 원하는 장치를 열거 하는 데 걸리는 시간을 줄일 수 있습니다.

사용할 수는 **GetDeviceSelector** 메서드는 해당 형식에 대 한 장치 선택기에 가져오려는 찾고 있는 장치 유형에 대 한 합니다. 예를 들어,를 사용 하 여 [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) 선택기 모든 열거를 사용 하 여 제공 됩니다 [PosPrinters](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter) USB, 네트워크 및 Bluetooth POS 프린터를 포함 하 여 시스템에 연결 합니다.

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

합니다 **GetDeviceSelector** 다른 장치 유형에 대 한 메서드는:

* [BarcodeScanner.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

사용 하 여를 **GetDeviceSelector** 사용 하는 메서드를 [PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes) 값 매개 변수로 또는 제한할 수 있습니다에 선택기를 열거 로컬, 네트워크, Bluetooth 연결 POS 장치 감소는 쿼리가 완료 하는 데 걸리는 시간입니다.  아래 샘플 로컬로 지 원하는 선택기를 정의 하려면이 메서드를 사용 하 여 POS 프린터 연결을 보여줍니다.

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> 참조 [장치 선택기 빌드](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) 더 작성 하기 위한 고급 선택기 문자열입니다.

## <a name="method-1-use-a-device-picker"></a>방법 1: 장치 선택기를 사용 하 여

합니다 [DevicePicker](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker) 클래스를 사용 하면 선택할 사용자에 대 한 장치의 목록을 포함 하는 선택기 플라이 아웃을 표시할 수 있습니다. 사용할 수는 [필터](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.filter) 속성 선택기에 표시 하는 장치 유형을 선택 합니다. 이 속성은 형식의 [DevicePickerFilter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter)합니다. 장치 유형을 사용 하 여 필터를 추가할 수 있습니다 합니다 [SupportedDeviceClasses](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses) 하거나 [SupportedDeviceSelectors](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors) 속성입니다.

장치 선택기를 표시할 준비가 되었을 때를 호출할 수 있습니다 합니다 [PickSingleDeviceAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync) 메서드 선택기 UI를 표시 하 고 선택한 장치를 반환 합니다. 지정 해야 하는 [Rect](https://docs.microsoft.com/uwp/api/windows.foundation.rect) 플라이 아웃 나타나는 결정 하는 데는 합니다. 이 메서드는 반환 된 [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 개체를 사용 하 여 지점의 서비스 Api를 사용 하려면 사용 해야 하므로 **FromIdAsync** 원하는 특정 장치 클래스의 메서드를 합니다. 전달 된 [DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 메서드의 속성 *deviceId* 매개 변수 및 반환 값으로 장치 클래스의 인스턴스를 가져옵니다.

다음 코드 조각은 만듭니다는 **DevicePicker**, 바코드 스캐너 필터를 추가에 사용자가 장치를 선택 하 고 만듭니다는 **BarcodeScanner** 장치 ID를 기준으로 개체:

```cs
private async Task<BarcodeScanner> GetBarcodeScanner()
{
    DevicePicker devicePicker = new DevicePicker();
    devicePicker.Filter.SupportedDeviceSelectors.Add(BarcodeScanner.GetDeviceSelector());
    Rect rect = new Rect();
    DeviceInformation deviceInformation = await devicePicker.PickSingleDeviceAsync(rect);
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceInformation.Id);
    return barcodeScanner;
}
```

## <a name="method-2-get-first-available-device"></a>방법 2: 첫 번째 사용 가능한 장치 가져오기

다음은 장치에 대 한 가장 간단한 방법은 사용 하는 것 **GetDefaultAsync** Point of Service 장치 클래스 내에서 사용 가능한 첫 번째 장치를 가져오려고 합니다. 

아래 샘플의 사용법을 보여 줍니다 [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) 에 대 한 [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)합니다. 코딩 패턴을 모든 서비스 지점 장치 클래스에 대 한 것과 비슷합니다.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> **GetDefaultAsync** 사용 해야 합니다 신중 하 게 다음 세션 간에 다른 장치를 반환할 수 있습니다. 많은 이벤트가 이 열거에 영향을 주어 다음을 포함하여 첫 번째 사용할 수 있는 장치를 변경할 수 있습니다. 
> - 컴퓨터에 연결된 카메라 변경 
> - 컴퓨터에 연결 된 서비스 장치 범위로 변경
> - 네트워크에서 사용할 수 있는 서비스 지점 장치 네트워크 연결 변경
> - 컴퓨터의 범위 내에서 Bluetooth 서비스 지점 장치 변경 
> - Point of Service 구성의 변경 내용 
> - 드라이버 또는 OPOS 서비스 개체 설치
> - Point of Service 확장 설치
> - Windows 운영 체제 업데이트

## <a name="method-3-snapshot-of-devices"></a>방법 3: 장치의 스냅숏

일부 시나리오에서 고유한 UI를 빌드하거나 사용자에게 UI를 표시하지 않고 장치를 열거해야 할 수 있습니다.  이러한 상황에서 열거할 수 있습니다. 현재 연결 되어 있거나 시스템을 사용 하 여 연결 하는 장치의 스냅숏으로 [DeviceInformation.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)합니다.  이 메서드는 전체 열거가 완료될 때까지 모든 결과를 보유합니다.

> [!TIP]
> 사용 하는 것이 좋습니다.는 **GetDeviceSelector** 메서드를 **PosConnectionTypes** 사용 하는 경우 매개 변수 **FindAllAsync** 연결 형식으로 쿼리를 제한 하려면 원하는입니다.  네트워크 및 Bluetooth 연결 되기 전에 완료 해야 해당 열거형으로 결과 지연 될 수 있습니다 **FindAllAsync** 결과가 반환 됩니다.

> [!CAUTION] 
> **FindAllAsync** 장치 배열을 반환 합니다.  이 배열의 순서는 세션 간에 변경될 수 있기 때문에 배열에 하드 코드된 인덱스를 사용하여 특정 순서에 의존하는 것은 권장되지 않습니다.  사용 하 여 [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 결과 필터링 하려는 사용자에 대 한 UI를 제공 하는 속성입니다.

이 샘플에서는 스냅숏을 사용 하 여 장치의 위에 정의 된 선택기 **FindAllAsync** 그런 다음 각 컬렉션에 의해 반환 되는 항목을 열거 하 고 장치 이름 및 ID를 디버그 출력에 씁니다. 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> 작업할 때 합니다 [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) Api는 자주 사용 해야 [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 특정 장치에 대 한 정보를 가져올 개체입니다. 예를 들어 합니다 [DeviceInformation.ID](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 복구한 이후 세션에서 사용 가능한 경우 동일한 장치를 다시 사용할 속성을 사용할 수 있습니다 및 [DeviceInformation.Name](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) 속성을 표시 하기 위해 사용할 수 있습니다 앱의 목적으로 합니다.  참조 된 [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 사용할 수 있는 추가 속성에 대 한 정보에 대 한 참조 페이지입니다.

## <a name="method-4-enumerate-and-watch"></a>방법 4: 열거 하 고 시청

장치를 열거하는 더 강력하고 유연한 방법은 [DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)를 만드는 것입니다.  장치 감시자는 장치를 동적으로 열거합니다. 그러면 초기 열거가 완료된 후 장치가 추가, 제거 또는 변경되는 경우 응용 프로그램에서 알림을 수신합니다.  A **DeviceWatcher** 장치가 온라인 상태가 되는 Bluetooth 장치를 범위에서 네트워크에 연결 된 경우를 검색할 수 있습니다 로컬로 연결된 된 장치 내에서 적절 한 조치를 취할 수 있도록 하 여 연결 되어 있지 않습니다 처럼도 하 응용 프로그램입니다.

이 샘플에서는 만들려면 위에 정의 된 선택기를 사용을 **DeviceWatcher** 에 대 한 이벤트 처리기를 정의도 합니다 [Added](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added), [제거](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed), 및 [업데이트 ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) 알림. 각 알림에 대해 수행하려는 작업의 세부 정보를 입력해야 합니다.

```Csharp
using Windows.Devices.Enumeration;

DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Removed += DeviceWatcher_Removed;
deviceWatcher.Updated += DeviceWatcher_Updated;

void DeviceWatcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    // TODO: Add the DeviceInformation object to your collection
}

void DeviceWatcher_Removed(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Remove the item in your collection associated with DeviceInformationUpdate
}

void DeviceWatcher_Updated(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Update your collection with information from DeviceInformationUpdate
}
```

> [!TIP]
> 참조 [Enumerate 장치와 조사식]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) 사용에 대 한 자세한 내용은 **DeviceWatcher**합니다.

## <a name="see-also"></a>참고 항목
* [Point of Service 시작](pos-basics.md)
* [DeviceInformation 클래스](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)
* [PosPrinter 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)
* [PosConnectionTypes 열거형](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [BarcodeScanner 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [DeviceWatcher 클래스](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]