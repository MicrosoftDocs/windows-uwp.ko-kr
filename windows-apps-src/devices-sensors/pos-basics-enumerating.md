---
author: TerryWarwick
title: PointOfService 장치 열거
description: PointOfService 디바이스를 열거하는 메서드를 알아봅니다.
ms.author: jken
ms.date: 08/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 4e42ebb2eba7b6465be271e6095100c03798826f
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4319637"
---
# <a name="enumerating-point-of-service-devices"></a>서비스 지점 장치 열거
이 섹션에서는 시스템에서 사용할 수 있는 장치를 쿼리하는 데 사용되는 [장치 선택기를 정의](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)하고 이 선택기를 사용하여 다음 메서드 중 하나를 사용하는 서비스 지점 장치를 열거하는 메서드를 배웁니다.

**방법 1:** [디바이스 선택기를 사용 하 여](#method-1:-use-a-device-picker)
<br/>
장치 선택기 UI를 표시 하 고 사용자가 연결 된 장치를 선택 합니다. 이 메서드는 장치를 연결 하 고 제거 때 목록 업데이트를 처리 하 고 더 간단 하 고 다른 방법 보다 안전 하 게 합니다.

**방법 2:** [첫 번째 사용할 수 있는 장치 가져오기](#Method-1:-get-first-available-device)<br />[GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync) 를 사용 하 여 특정 서비스 지점 디바이스 클래스에서 사용 가능한 첫 번째 장치에 액세스할 수 있습니다.

**메서드 3:** [디바이스의 스냅숏](#Method-2:-Snapshot-of-devices)<br />시간이 지정 된 지점에 시스템에 존재 하는 서비스 지점 디바이스의 스냅숏을 열거 합니다. 고유한 UI를 빌드하거나 사용자에게 UI를 표시하지 않고 장치를 열거하고자 할 때 유용합니다. [FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 는 결과 보유 전체 열거가 완료 될 때까지 합니다.

**방법 4:** [열거 및 감시](#Method-3:-Enumerate-and-watch)<br />[DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) 수 현재 존재 하는 장치를 열거 하 고 장치가 추가 되거나 시스템에서 제거 되 면 알림을 받을 수 있는 더 강력 하 고 유연한 열거형 모델입니다.  스냅샷이 표시되기까지 기다리는 것보다 UI에 표시하기 위해 백그라운드에서 디바이스의 현재 목록을 유지하려는 경우 유용합니다.

## <a name="define-a-device-selector"></a>장치 선택기 정의
장치 선택기는 디바이스를 열거할 때 검색하는 디바이스를 제한할 수 있습니다.  이렇게 하면 관련성 높은 결과만 가져오고 원하는 장치를 열거 하는 데 걸리는 시간을 줄일 수 있습니다.

해당 형식에 대 한 장치 선택기를 가져오려면 원하는 장치 유형에 대 한 **GetDeviceSelector** 메서드를 사용할 수 있습니다. 예를 들어 [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) 를 사용 하 여는 제공 USB, 네트워크, Bluetooth POS 프린터를 포함 하 여 시스템에 연결 된 모든 [Posprinter](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter) 를 열거 하는 선택기를 사용 하 여 있습니다.

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

다른 장치 유형에 대 한 **GetDeviceSelector** 방법은 다음과 같습니다.

* [BarcodeScanner.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

[PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes) 값을 매개 변수로 사용 하는 **GetDeviceSelector** 메서드를 사용 하 여 제한할 수 있습니다 선택기를 열거 로컬, 네트워크, 또는 Bluetooth 연결 된 POS 디바이스 쿼리가 완료 하는 데 걸리는 시간이 줄어듭니다.  아래 샘플 로컬로 지 원하는 선택기를 정의 하는이 메서드를 사용 하는 연결 POS 프린터를 보여 줍니다.

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> 추가 고급 선택기 문자열은 [장치 선택기 빌드](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)를 참조하세요.

## <a name="method-1-use-a-device-picker"></a>방법 1: 장치 선택기를 사용 합니다.

> [!NOTE]
> 이 메서드는 최신 [Windows SDK 참가자 미리 보기](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)에 필요합니다.

[DevicePicker](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker) 클래스를 사용 하면 사용자가 선택 하에 대 한 장치 목록을 포함 하는 선택기 플라이 아웃을 표시할 수 있습니다. 선택기를 표시 하는 장치 유형을 선택 하려면 [필터](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.filter) 속성을 사용할 수 있습니다. 이 속성 [DevicePickerFilter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter)형식입니다. 장치 유형 [중 하나](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses) 또는 [SupportedDeviceSelectors](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors) 속성을 사용 하 여 필터를 추가할 수 있습니다.

장치 선택기를 표시할 준비가 되 면 선택기 UI를 표시 되며 선택한 디바이스를 반환 하는 [PickSingleDeviceAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync) 메서드를 호출할 수 있습니다. 플라이 아웃이 표시 되는 위치를 결정 하는 [Rect](https://docs.microsoft.com/uwp/api/windows.foundation.rect) 를 지정 해야 합니다. 이 메서드는 반환 [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 개체를 사용 하 여는 서비스 지점 Api를 사용 하기 위해서는, 원하는 특정 장치 클래스에 대 한 **FromIdAsync** 메서드를 사용 해야 합니다. [DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 속성 메서드의 *deviceId* 매개 변수로 전달 하 고 반환 값으로 장치 클래스의 인스턴스를 가져옵니다.

다음 코드 조각은 **DevicePicker**만듭니다, 그리고 사용자 선택, 디바이스에, 바코드 스캐너 필터를 추가 하 다음 장치 ID에 따라 **BarcodeScanner** 개체를 만듭니다.

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

## <a name="method-2-get-first-available-device"></a>방법 2: 첫 번째 사용할 수 있는 장치 가져오기

서비스 지점 장치 하는 가장 간단한 방법은 서비스 지점 디바이스 클래스 내에서 사용 가능한 첫 번째 장치를 가져오려면 **GetDefaultAsync** 를 사용 하는 것입니다. 

아래 샘플에서는 [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)에 대 한 [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) 를 사용 합니다. 코딩 패턴은 모든 서비스 지점 장치 클래스에 대해 비슷합니다.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> **GetDefaultAsync** 으로 다음 세션 간에 다른 장치를 반환할 수 있습니다 주의 사용 되어야 합니다. 많은 이벤트가 이 열거에 영향을 주어 다음을 포함하여 첫 번째 사용할 수 있는 장치를 변경할 수 있습니다. 
> - 컴퓨터에 연결된 카메라 변경 
> - 컴퓨터에 연결 된 장치의 서비스 범위로 변경
> - 네트워크에서 사용할 수 있는 서비스 지점 장치 네트워크 연결의 변경
> - 컴퓨터의 범위 내 Bluetooth 서비스 지점 장치를 변경 합니다. 
> - 서비스 지점 구성 변경 
> - 드라이버 또는 OPOS 서비스 개체 설치
> - 서비스 지점 확장 설치
> - Windows 운영 체제 업데이트

## <a name="method-3-snapshot-of-devices"></a>메서드 3: 디바이스의 스냅숏

일부 시나리오에서 고유한 UI를 빌드하거나 사용자에게 UI를 표시하지 않고 장치를 열거해야 할 수 있습니다.  이러한 경우, [DeviceInformation.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)를 사용하여 현재 시스템과 연결 또는 페어링된 디바이스의 스냅숏을 열거할 수 있습니다.  이 메서드는 전체 열거가 완료될 때까지 모든 결과를 보유합니다.

> [!TIP]
> 원하는 연결 형식에 대 한 쿼리를 제한 하기 위해 **FindAllAsync** 를 사용할 때 **PosConnectionTypes** 매개 변수와 함께 **GetDeviceSelector** 메서드를 사용 하는 것이 좋습니다.  **FindAllAsync** 결과가 반환 되기 전에 열거를 완료 해야 하는 대로 네트워크 및 Bluetooth 연결은 결과 지연 시킬 수 있습니다.

> [!CAUTION] 
> **FindAllAsync** 는 일련의 장치를 반환합니다.  이 배열의 순서는 세션 간에 변경될 수 있기 때문에 배열에 하드 코드된 인덱스를 사용하여 특정 순서에 의존하는 것은 권장되지 않습니다.  [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 속성을 사용 하 여 결과 필터링 하거나 선택할 수 사용자에 대 한 UI를 제공 합니다.

이 샘플 **FindAllAsync** 를 사용 하 여 디바이스의 스냅숏을 위에서 정의한 선택기를 사용 하 여 다음 각 컬렉션에서 반환 된 항목을 통해 열거 하 고 장치 이름 및 ID를 디버그 출력을 씁니다. 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) API로 작업할 때 특정 디바이스에 대한 개체 정보를 얻기 위해 종종 [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)을 사용해야 합니다. 예를 들어를 복구 하 고 향후 세션에서 사용할 수 및 앱에서 표시 하기 위해 [DeviceInformation.Name](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) 속성을 사용 하는 경우 동일한 장치를 다시 사용할 [DeviceInformation.ID](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 속성을 사용할 수 있습니다.  사용할 수 있는 추가 속성에 대한 정보는 [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 참조 페이지를 참조하세요.

## <a name="method-4-enumerate-and-watch"></a>방법 4: 열거 및 감시

장치를 열거하는 더 강력하고 유연한 방법은 [DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)를 만드는 것입니다.  장치 감시자는 장치를 동적으로 열거합니다. 그러면 초기 열거가 완료된 후 장치가 추가, 제거 또는 변경되는 경우 응용 프로그램에서 알림을 수신합니다.  **DeviceWatcher** 를 통해 네트워크에 연결 된 장치가 온라인 상태가 될 때 감지 하, Bluetooth 디바이스가 범위에도으로 하는 경우 응용 프로그램 내에서 적절 한 조치를 취할 수 있도록 로컬로 연결 된 장치가 연결 되어 있지 않습니다.

이 샘플을 만드는 **DeviceWatcher** 위에서 정의한 선택기를 사용 하 여 뿐 아니라 [추가](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added), [제거](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed)및 [업데이트](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) 알림에 대 한 이벤트 처리기를 정의 합니다. 각 알림에 대해 수행하려는 작업의 세부 정보를 입력해야 합니다.

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
> **DeviceWatcher**사용에 대 한 자세한 내용은 [장치 열거 및 감시를]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) 참조 하세요.

## <a name="see-also"></a>참고 항목
* [서비스 지점 시작하기](pos-basics.md)
* [DeviceInformation 클래스](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)
* [PosPrinter 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)
* [PosConnectionTypes 열거형](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [BarcodeScanner 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [DeviceWatcher 클래스](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]