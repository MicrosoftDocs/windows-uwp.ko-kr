---
author: TerryWarwick
title: PointOfService 장치 열거
description: PointOfService 디바이스를 열거하는 메서드를 알아봅니다.
ms.author: jken
ms.date: 06/8/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: be1fdc42295fc03ff86a69e287a4089abe547689
ms.sourcegitcommit: ee77826642fe8fd9cfd9858d61bc05a96ff1bad7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/11/2018
ms.locfileid: "2018441"
---
# <a name="enumerating-point-of-service-devices"></a>서비스 지점 장치 열거
이 섹션에서는 시스템에서 사용할 수 있는 장치를 쿼리하는 데 사용되는 [**장치 선택기를 정의**](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)하고 이 선택기를 사용하여 다음 메서드 중 하나를 사용하는 서비스 지점 장치를 열거하는 메서드를 배웁니다.

**메서드 1:**[**첫 번째 사용할 수 있는 장치 가져오기**](#Method-1:-get-first-available-device)<br />이 섹션에서 **GetDefaultAsync**를 사용하여 특정 PointOfService 디바이스 클래스에서 사용 가능한 첫 번째 장치에 액세스하는 메서드를 알아봅니다.

**메서드 2:**[**디바이스의 스냅숏**](#Method-2:-Snapshot-of-devices)<br />이 섹션에서 특정된 시간 지점에 시스템에 있는 PointOfService 디바이스의 스냅숏을 열거하는 메서드를 알아봅니다. 고유한 UI를 빌드하거나 사용자에게 UI를 표시하지 않고 장치를 열거하고자 할 때 유용합니다. FindAllAsync는 전체 열거가 완료될 때까지 결과를 보유합니다.

**메서드 3:** [**열거 및 감시**](#Method-3:-Enumerate-and-watch)<br />이 섹션에서 현재 있는 디바이스를 열거하고 장치가 시스템에 추가되거나 시스템에서 제거될 때 알림을 받을 수 있는 더 강력하고 유연한 열거형 모델에 대해 알아봅니다.  스냅샷이 표시되기까지 기다리는 것보다 UI에 표시하기 위해 백그라운드에서 디바이스의 현재 목록을 유지하려는 경우 유용합니다.
 

---
## <a name="define-a-device-selector"></a>장치 선택기 정의
장치 선택기는 디바이스를 열거할 때 검색하는 디바이스를 제한할 수 있습니다.  이렇게 하면 관련성 높은 결과만 가져오고 원하는 장치를 열거하는 데 걸리는 시간을 줄일 수 있습니다.  

[**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector)를 사용하면 USB, 네트워크, Bluetooth POSPrinter 등 시스템에 연결된 모든 POSPrinter를 열거하는 선택기를 사용할 수 있습니다.

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();   

```

[**PosConnectionTypes**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)를 매개 변수로 가지는 [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector_Windows_Devices_PointOfService_PosConnectionTypes_) 메서드를 사용하여 선택기가 로컬, 네트워크 또는 Bluetooth 연결 POSPrinter를 열거하도록 제한하여 쿼리가 완료되는 데 걸리는 시간을 절약할 수 있습니다.  아래의 예제는 이 메서드를 사용하여 로컬로 연결된 POSPrinter만 지원하는 선택기를 정의하는 작업을 보여 줍니다.

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);   

```
> [!TIP]
> 추가 고급 선택기 문자열은 [**장치 선택기 빌드**](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)를 참조하세요.

---

## <a name="method-1-get-first-available-device"></a>메서드 1: 첫 번째 사용할 수 있는 장치 가져오기

PointOfService 디바이스를 가져오는 가장 간단한 방법은 **GetDefaultAsync**를 사용하여 PointOfService 디바이스 클래스 내에서 사용 가능한 첫 번째 디바이스를 가져오는 것입니다. 

아래의 예제에서는 BarcodeScanner를 위한 [**GetDefaultAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) 사용을 설명합니다. 코딩 패턴은 모든 PointOfService 디바이스 클래스에 대해 비슷합니다.

```Csharp

using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();

```

> [!CAUTION]
> GetDefaultAsync는 한 세션에서 다음 세션으로 다른 장치를 반환하기 때문에 주의해서 사용해야 합니다. 많은 이벤트가 이 열거에 영향을 주어 다음을 포함하여 첫 번째 사용할 수 있는 장치를 변경할 수 있습니다. 
> - 컴퓨터에 연결된 카메라 변경 
> - 컴퓨터에 연결된 PointOfService 장치의 변경
> - 네트워크에서 사용할 수 있는 PointOfService 장치에 연결된 네트워크의 변경
> - 컴퓨터의 범위 내 Bluetooth PointOfService 장치의 변경 
> - PointOfService 구성 변경 
> - 드라이버 또는 OPOS 서비스 개체 설치
> - PointOfService 확장 설치
> - Windows 운영 체제 업데이트

---

## <a name="method-2-snapshot-of-devices"></a>메서드 2: 디바이스의 스냅숏

일부 시나리오에서 고유한 UI를 빌드하거나 사용자에게 UI를 표시하지 않고 장치를 열거해야 할 수 있습니다.  이러한 경우, [**DeviceInformation.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)를 사용하여 현재 시스템과 연결 또는 페어링된 디바이스의 스냅숏을 열거할 수 있습니다.  이 메서드는 전체 열거가 완료될 때까지 모든 결과를 보유합니다.

> [!TIP]
> 원하는 연결 형식에 대한 쿼리를 제한하기 위해 FindAllAsync를 사용할 때 PosConnectionTypes 매개 변수와 함께 GetDeviceSelector 메서드를 사용하는 것이 좋습니다.  FindAllAsync 결과가 반환되기 전에 열거를 완료해야 하므로 네트워크 및 Bluetooth 연결은 결과를 지연시킬 수 있습니다.

>[!CAUTION] 
>FindAllAsync는 일련의 장치를 반환합니다.  이 배열의 순서는 세션 간에 변경될 수 있기 때문에 배열에 하드 코드된 인덱스를 사용하여 특정 순서에 의존하는 것은 권장되지 않습니다.  DeviceInformation 속성을 사용하여 결과를 필터링하거나 선택할 수 있는 사용자에 대한 UI를 제공합니다.

이 샘플은 위에서 정의한 선택기를 사용하여 FindAllAsync를 사용하여 디바이스의 스냅숏을 찍은 다음 컬렉션에서 반환되는 각 항목을 통해 열거하고 디바이스 이름 및 ID를 디버그 출력에 기록합니다. 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) API로 작업할 때 특정 디바이스에 대한 개체 정보를 얻기 위해 종종 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)을 사용해야 합니다. 예를 들어, 향후 세션에서 사용할 수 있는 경우 동일한 장치를 복구하고 다시 사용하기 위해 [**DeviceInformation.ID**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 속성을 사용하고 앱의 용도를 표시하기 위해 [**DeviceInformation.Name**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) 속성을 사용할 수 있습니다.  사용할 수 있는 추가 속성에 대한 정보는 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 참조 페이지를 참조하세요.

---

## <a name="method-3-enumerate-and-watch"></a>메서드 3: 열거 및 감시

장치를 열거하는 더 강력하고 유연한 방법은 [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)를 만드는 것입니다.  장치 감시자는 장치를 동적으로 열거합니다. 그러면 초기 열거가 완료된 후 장치가 추가, 제거 또는 변경되는 경우 응용 프로그램에서 알림을 수신합니다.  DeviceWatcher를 통해 네트워크에 연결된 장치가 온라인 상태가 되었을 때, Bluetooth 디바이스가 범위 내에 있을 때, 그리고 로컬에 연결된 장치가 연결이 끊겼을 때 감지하여 응용 프로그램 내에서 적절한 조치를 취할 수 있습니다.

이 샘플은 위에서 정의한 선택기를 사용하여 DeviceWatcher를 만들고 추가, 제거 및 업데이트 알림에 대한 이벤트 처리기를 정의합니다. 각 알림에 대해 수행하려는 작업의 세부 정보를 입력해야 합니다.

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
> DeviceWatcher 사용에 대한 자세한 내용은 [**장치 열거 및 감시**]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices)를 참조하세요.
