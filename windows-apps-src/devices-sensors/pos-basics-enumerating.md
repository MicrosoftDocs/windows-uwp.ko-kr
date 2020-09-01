---
title: PointOfService 장치 열거
description: 장치 선택기를 사용 하 여 시스템에서 사용할 수 있는 PointOfService 장치를 쿼리하고 열거 하는 네 가지 방법에 대해 알아봅니다.
ms.date: 10/08/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 6d1767e49e14f424b7d21424fbfc02b76acd546a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159677"
---
# <a name="enumerating-point-of-service-devices"></a>서비스 장치 가리키기 열거
이 섹션에서는 시스템에 사용할 수 있는 장치를 쿼리 하는 데 사용 되는 [장치 선택기를 정의](./build-a-device-selector.md) 하 고 다음 방법 중 하나를 사용 하 여이 선택기를 사용 하 여 서비스 장치 지점을 열거 하는 방법을 알아봅니다.

**방법 1:** [장치 선택 사용](#method-1-use-a-device-picker)
<br/>
장치 선택 UI를 표시 하 고 사용자가 연결 된 장치를 선택 하도록 합니다. 이 메서드는 장치를 연결 및 제거 하는 경우 목록 업데이트를 처리 하 고, 다른 방법 보다 간단 하 고 안전 합니다.

**방법 2:** [사용 가능한 첫 번째 장치 가져오기](#method-2-get-first-available-device)<br />[GetDefaultAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync) 를 사용 하 여 특정 서비스 장치 클래스에서 사용 가능한 첫 번째 장치에 액세스 합니다.

**방법 3:** [장치 스냅숏](#method-3-snapshot-of-devices)<br />지정 된 시점에 시스템에 있는 서비스 장치 지점의 스냅숏을 열거 합니다. 이 기능은 사용자에 게 ui를 표시 하지 않고 장치를 열거 하거나 사용자에 게 ui를 작성 하려는 경우에 유용 합니다. [FindAllAsync](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 는 전체 열거가 완료 될 때까지 결과를 저장 합니다.

**방법 4:** [열거 및 조사식](#method-4-enumerate-and-watch)<br />[Devicewatcher](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) 는 현재 존재 하는 장치를 열거 하 고, 장치를 시스템에서 추가 하거나 제거할 때 알림을 받을 수 있는 보다 강력 하 고 유연한 열거 모델입니다.  이는 스냅숏이 발생할 때까지 기다리지 않고 UI에 표시 하기 위해 백그라운드에서 현재 장치 목록을 유지 하려는 경우에 유용 합니다.

## <a name="define-a-device-selector"></a>장치 선택기 정의
장치 선택기를 사용 하면 장치를 열거할 때 검색 하는 장치를 제한할 수 있습니다.  이렇게 하면 관련 된 결과만 얻고 원하는 장치를 열거 하는 데 걸리는 시간을 줄일 수 있습니다.

검색 하려는 장치 유형에 **Getdeviceselector** 메서드를 사용 하 여 해당 유형의 장치 선택기를 가져올 수 있습니다. 예를 들어 [PosPrinter](/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) 를 사용 하면 USB, 네트워크 및 Bluetooth POS 프린터를 비롯 하 여 시스템에 연결 된 모든 [PosPrinters](/uwp/api/windows.devices.pointofservice.posprinter) 를 열거 하는 선택기를 제공 합니다.

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

여러 장치 유형에 대 한 **Getdeviceselector** 메서드는 다음과 같습니다.

* [바 Code스캐너. GetDeviceSelector](/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer. GetDeviceSelector](/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay. GetDeviceSelector](/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader. GetDeviceSelector](/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter. GetDeviceSelector](/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

[PosConnectionTypes](/uwp/api/windows.devices.pointofservice.posconnectiontypes) 값을 매개 변수로 사용 하는 **getdeviceselector** 메서드를 사용 하 여 로컬, 네트워크 또는 Bluetooth 연결 POS 장치를 열거 하도록 선택기를 제한 하 여 쿼리가 완료 되는 데 걸리는 시간을 줄일 수 있습니다.  아래 샘플에서는이 메서드를 사용 하 여 로컬에 연결 된 POS 프린터만 지 원하는 선택기를 정의 하는 방법을 보여 줍니다.

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> 고급 선택기 문자열을 빌드하기 위한 [장치 선택기 빌드](./build-a-device-selector.md) 를 참조 하세요.

## <a name="method-1-use-a-device-picker"></a>방법 1: 장치 선택 사용

[DevicePicker](/uwp/api/windows.devices.enumeration.devicepicker) 클래스를 사용 하면 사용자가 선택할 수 있는 장치 목록이 포함 된 선택 플라이 아웃을 표시할 수 있습니다. [필터](/uwp/api/windows.devices.enumeration.devicepicker.filter) 속성을 사용 하 여 선택에 표시할 장치 유형을 선택할 수 있습니다. 이 속성은 [Devicepickerfilter](/uwp/api/windows.devices.enumeration.devicepickerfilter)유형입니다. [SupportedDeviceClasses](/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses) 또는 [supporteddeviceselectors](/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors) 속성을 사용 하 여 필터에 장치 유형을 추가할 수 있습니다.

장치 선택기를 표시할 준비가 되 면 [PickSingleDeviceAsync](/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync) 메서드를 호출 하 여 선택 UI를 표시 하 고 선택한 장치를 반환할 수 있습니다. 플라이 아웃이 표시 되는 위치를 결정 하는 [Rect](/uwp/api/windows.foundation.rect) 를 지정 해야 합니다. 이 메서드는 [Deviceinformation](/uwp/api/windows.devices.enumeration.deviceinformation) 개체를 반환 하므로 서비스 Api의 지점과 함께 사용 하려면 원하는 특정 장치 클래스에 대해 **FromIdAsync** 메서드를 사용 해야 합니다. [DeviceInformation.Id](/uwp/api/windows.devices.enumeration.deviceinformation.id) 속성을 메서드의 *deviceId* 매개 변수로 전달 하 고 장치 클래스의 인스턴스를 반환 값으로 가져옵니다.

다음 코드 조각에서는 **DevicePicker**을 만들고,이에 바코드 스캐너 필터를 추가 하 고, 사용자가 장치를 선택한 다음, 장치 ID를 기반으로 하는 **바 code스캐너** 개체를 만듭니다.

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

## <a name="method-2-get-first-available-device"></a>방법 2: 사용 가능한 첫 번째 장치 가져오기

서비스 장치를 가져오는 가장 간단한 방법은 **GetDefaultAsync** 를 사용 하 여 서비스 장치 클래스에서 사용 가능한 첫 번째 장치를 가져오는 것입니다. 

아래 샘플에서는 [바 Code스캐너](/uwp/api/windows.devices.pointofservice.barcodescanner)에 [GetDefaultAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) 를 사용 하는 방법을 보여 줍니다. 코딩 패턴은 모든 서비스 장치 클래스의 점에서 유사 합니다.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> **GetDefaultAsync** 는 한 세션에서 다음 세션으로 다른 장치를 반환할 수 있으므로 주의 해 서 사용 해야 합니다. 이 열거형에 영향을 줄 수 있는 여러 이벤트는 다음을 비롯 한 다른 첫 번째 사용 가능한 장치를 초래 합니다. 
> - 컴퓨터에 연결 된 카메라 변경 
> - 컴퓨터에 연결 된 서비스 장치 지점 변경
> - 네트워크에서 사용할 수 있는 네트워크 연결 된 서비스 지점의 변경
> - 컴퓨터 범위 내에서 Bluetooth 서비스 장치 변경 
> - 서비스의 시점 구성 변경 
> - 드라이버 또는 OPOS 서비스 개체 설치
> - 서비스 확장 지점 설치
> - Windows 운영 체제에 대 한 업데이트

## <a name="method-3-snapshot-of-devices"></a>방법 3: 장치 스냅숏

일부 시나리오에서는 사용자에 게 UI를 표시 하지 않고 사용자 고유의 UI를 빌드하거나 장치를 열거 해야 할 수도 있습니다.  이러한 경우, 현재 연결 되어 있거나 FindAllAsync를 사용 하 여 시스템과 연결 된 장치의 스냅숏을 열거할 수 있습니다 [.](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)  이 메서드는 전체 열거가 완료 될 때까지 모든 결과를 저장 합니다.

> [!TIP]
> **FindAllAsync** 를 사용 하 여 원하는 연결 형식으로 쿼리를 제한 하는 경우 **PosConnectionTypes** 매개 변수와 함께 **getdeviceselector** 메서드를 사용 하는 것이 좋습니다.  **FindAllAsync** 결과를 반환 하기 전에 해당 열거를 완료 해야 하므로 네트워크 및 Bluetooth 연결에서 결과를 연기할 수 있습니다.

> [!CAUTION] 
> **FindAllAsync** 는 장치 배열을 반환 합니다.  이 배열의 순서는 세션에서 세션으로 변경 될 수 있으므로 배열에 하드 코딩 된 인덱스를 사용 하 여 특정 순서를 사용 하지 않는 것이 좋습니다.  [Deviceinformation](/uwp/api/windows.devices.enumeration.deviceinformation) 속성을 사용 하 여 결과를 필터링 하거나 사용자가 선택할 수 있는 UI를 제공 합니다.

이 샘플에서는 위에 정의 된 선택기를 사용 하 여 **FindAllAsync** 를 사용 하는 장치의 스냅숏을 만든 다음, 컬렉션에서 반환 된 각 항목을 열거 하 고, 장치 이름 및 ID를 디버그 출력에 기록 합니다. 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> [Windows.](/uwp/api/Windows.Devices.Enumeration) . i w api를 사용 하는 경우에는 특정 장치에 대 한 정보를 얻기 위해 [deviceinformation](/uwp/api/windows.devices.enumeration.deviceinformation) 개체를 사용 해야 하는 경우가 자주 있습니다. 예를 들어 이후 세션에서 사용할 수 있는 경우 [DeviceInformation.ID](/uwp/api/windows.devices.enumeration.deviceinformation.id) 속성을 사용 하 여 동일한 장치를 복구 하 고 다시 사용할 수 있으며, [DeviceInformation.Name](/uwp/api/windows.devices.enumeration.deviceinformation.name) 속성을 앱의 표시 목적으로 사용할 수 있습니다.  사용 가능한 추가 속성에 대 한 자세한 내용은 [Deviceinformation](/uwp/api/windows.devices.enumeration.deviceinformation) 참조 페이지를 참조 하세요.

## <a name="method-4-enumerate-and-watch"></a>방법 4: 열거 및 조사식

장치를 열거 하는 강력 하 고 유연한 방법은 [Devicewatcher](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)를 만드는 것입니다.  장치 감시자는 초기 열거가 완료 된 후 장치를 추가, 제거 또는 변경 하는 경우 응용 프로그램에서 알림을 받을 수 있도록 장치를 동적으로 열거 합니다.  **Devicewatcher** 를 사용 하면 네트워크에 연결 된 장치를 온라인 상태로 전환 하 고, Bluetooth 장치가 범위 내에 있으며, 응용 프로그램 내에서 적절 한 조치를 취할 수 있도록 로컬로 연결 된 장치를 분리 하는 경우를 감지할 수 있습니다.

이 샘플에서는 위에 정의 된 선택기를 사용 하 여 **Devicewatcher** 를 만들고 [추가](/uwp/api/windows.devices.enumeration.devicewatcher.added), [제거](/uwp/api/windows.devices.enumeration.devicewatcher.removed)및 [업데이트](/uwp/api/windows.devices.enumeration.devicewatcher.updated) 된 알림에 대 한 이벤트 처리기를 정의 합니다. 각 알림에 대해 수행할 작업에 대 한 세부 정보를 입력 해야 합니다.

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
> **Devicewatcher**사용에 대 한 자세한 내용은 [장치 열거 및 감시]( ./enumerate-devices.md#enumerate-and-watch-devices) 를 참조 하세요.

## <a name="see-also"></a>참고 항목
* [서비스 지점 시작](pos-basics.md)
* [DeviceInformation 클래스](/uwp/api/windows.devices.enumeration.deviceinformation)
* [PosPrinter 클래스](/uwp/api/windows.devices.pointofservice.posprinter)
* [PosConnectionTypes 열거형](/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [바 Code스캐너 클래스](/uwp/api/windows.devices.pointofservice.barcodescanner)
* [DeviceWatcher 클래스](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]