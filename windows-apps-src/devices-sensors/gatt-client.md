---
author: msatranjr
title: Bluetooth GATT 클라이언트
description: 이 문서에서는 일반적인 사용 사례에 대 한 샘플 코드와 함께 유니버설 Windows 플랫폼 (UWP) 응용 프로그램에 대 한 Bluetooth 일반 특성 프로필 (GATT) 클라이언트에 대 한 개요를 제공 합니다.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 555fec6d534c07898acd911f9cd84a11ac66dcd8
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "304886"
---
# <a name="bluetooth-gatt-client"></a>Bluetooth GATT 클라이언트


**중요 API**

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

이 문서에서는 GATT 클라이언트의 일반 작업에 대 한 샘플 코드와 함께 유니버설 Windows 플랫폼 (UWP) 응용 프로그램에 대 한 Bluetooth 일반 특성 (GATT) 클라이언트 Api의 사용을 보여줍니다.
- 주변 장치에 대 한 쿼리
- 장치에 연결
- 지원 되는 서비스와 장치 특성을 열거
- 읽기 및 특성에 쓰기
- 알림을 표시 하는 경우 값이 변경에 대 한 구독

## <a name="overview"></a>개요
개발자는 [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) 네임 스페이스에는 Api를 사용 하 여 Bluetooth LE 장치에 액세스 하기 수 있습니다. Bluetooth LE 장치는 다음 컬렉션을 통해 장치의 기능을 표시합니다.

-   서비스
-   특성
-   설명자

LE 장치의 기능 계약을 정의 하 고 서비스를 정의 하는 특성의 컬렉션을 포함 하는 서비스. 그리고 이러한 특성에는 특성을 설명하는 설명자가 포함됩니다. 이러한 3 용어는 일반적으로 장치를의 특성으로 알려져 있습니다.

Bluetooth LE GATT Api 원시 전송에 대 한 액세스 아니라 개체 및 기능을 노출 합니다. GATT Api를 사용 하는 다음 작업을 수행 하는 기능 Bluetooth LE 장치를 사용 하는 개발자에도 사용 하도록 설정 합니다.

-   특성 검색을 수행 합니다.
-   읽기 및 쓰기 특성 값
-   필요한 경우 특성 재정의 대 한 콜백을 등록합니다

만들려면 유용한 구현 개발자 있어야 사전 지식 GATT 서비스 및 응용 프로그램 사용 (영문)를 적용 하려는 특성 및 프로세스에 특정 특성 값으로 API에 의해 제공 되는 이진 데이터는 변환 되지 않도록 사용자에 게 발표할 하기 전에 유용한 데이터입니다. Bluetooth GATT API는 Bluetooth LE 장치와 통신하는 데 필요한 기본 요소만 표시합니다. 데이터를 해석하려면 Bluetooth SIG 표준 프로필이나 장치 공급업체에서 구현한 사용자 지정 프로필을 통해 응용 프로그램 프로필을 정의해야 합니다. 프로필은 교환되는 데이터가 어떻게 표현되고 그 데이터를 어떻게 해석해야 하는지를 규정하는 응용 프로그램과 장치 간의 바인딩 계약을 만듭니다.

편의상 Bluetooth SIG는 [사용 가능한 공용 프로필](https://www.bluetooth.com/specifications/adopted-specifications#gattspec) 목록을 유지합니다.

## <a name="query-for-nearby-devices"></a>주변 장치에 대 한 쿼리
두 기본 방법 주변 장치에 대 한 쿼리할 수 있습니다.
- Windows.Devices.Enumeration의 DeviceWatcher
- Windows.Devices.Bluetooth.Advertisement의 AdvertisementWatcher

2 번째 메서드는 설명 길게 [광고](ble-beacon.md) 설명서에서 있으므로 설명 하지 않습니다 많은 여기 하지만 기본 개념은 특정 [광고 필터](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter.aspx)를 만족 하는 장치 근처의 Bluetooth 주소를 찾을 수 있습니다. 주소를 만든 후에 장치에 대 한 참조를 가져올 [BluetoothLEDevice.FromBluetoothAddressAsync](https://msdn.microsoft.com/en-us/library/windows/apps/mt608819.aspx) 호출할 수 있습니다. 

이제 DeviceWatcher 메서드를 다시 합니다. Bluetooth LE 장치 Windows의 다른 장치에서와 마찬가지로 이며 [열거형 Api](https://msdn.microsoft.com/library/windows/apps/BR225459)를 사용 하 여 쿼리할 수 있습니다. [DeviceWatcher](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher) 클래스를 사용 하 고 찾을 장치를 지정 하는 쿼리 문자열을 전달 합니다. 

```csharp
// Query for extra properties you want returned
string[] requestedProperties = { "System.Devices.Aep.DeviceAddress", "System.Devices.Aep.IsConnected" };

DeviceWatcher deviceWatcher =
            DeviceInformation.CreateWatcher(
                    BluetoothLEDevice.GetDeviceSelectorFromPairingState(false),
                    requestedProperties,
                    DeviceInformationKind.AssociationEndpoint);

// Register event handlers before starting the watcher.
// Added, Updated and Removed are required to get all nearby devices
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Updated += DeviceWatcher_Updated;
deviceWatcher.Removed += DeviceWatcher_Removed;

// EnumerationCompleted and Stopped are optional to implement.
deviceWatcher.EnumerationCompleted += DeviceWatcher_EnumerationCompleted;
deviceWatcher.Stopped += DeviceWatcher_Stopped;

// Start the watcher.
deviceWatcher.Start();
```
일단는 DeviceWatcher를 시작 했을 때에 문제의 장치에 대 한 [추가](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher.added) 이벤트 처리기에서 쿼리를 만족 하는 각 장치에 대 한 [DeviceInformation](https://msdn.microsoft.com/library/windows/apps/br225393) 를 받게 됩니다. DeviceWatcher에 대 한 보다 자세한 정보에 대 한 전체 [Github에](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing)예제를 참조 합니다. 

## <a name="connecting-to-the-device"></a>장치에 연결
원하는 장치를 검색 한 후에 [DeviceInformation.Id](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.id) 를 사용 하 여 개체를 가져오려면 Bluetooth LE 장치는 장치에 대 한 문제의: 

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```
반면에 BluetoothLEDevice에 대 한 모든 참조의 삭제 (영문) 장치에 대 한 개체 (및 시스템에 없는 다른 응용 프로그램에는 장치에 대 한 참조 하는 경우)을 트리거합니다 자동 작은 제한 시간 후의 연결을 끊습니다. 

```csharp
bluetoothLeDevice.Dispose();
```
응용 프로그램을 장치에 다시 액세스 해야하는 경우 단순히 다시 장치 개체를 만들고 (다음 섹션에서 설명) 특성에 액세스 하는 필요한 경우 다시 연결 하려면 OS를 트리거합니다. 장치 근처 있으면이 포함 된 DeviceUnreachable 오류를 반환 합니다 그렇지 않은 경우에 장치에 대 한 액세스를 얻을 수 있습니다.  

## <a name="enumerating-supported-services-and-characteristics"></a>지원 되는 서비스 및 특성을 열거합니다.
BluetoothLEDevice 개체를 설정 했으므로 다음 단계에서는 장치를 노출 데이터를 검색 하는 것입니다. 이 작업을 수행 하는 첫 단계에는 서비스에 대 한 쿼리: 

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```
다음 단계는 관심 서비스를 식별 한 후 특성에 대 한 쿼리 합니다. 

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```  
운영 체제 GattCharacteristic ReadOnly 목록이 작업에서 다음 수행할 수 있는 개체를 반환 합니다.

## <a name="perform-readwrite-operations-on-a-characteristic"></a>특성에 대 한 읽기/쓰기 작업을 수행 합니다.

특성은 GATT의 기본 단위 통신을 시작 합니다. 장치에는 데이터의 고유한 요소를 나타내는 값을 포함 합니다. 예 배터리 수준 특성은 장치의 배터리 수준을 나타내는 값입니다.

어떤 작업은 지원를 확인 하려면 characteristic 속성을 읽기:
```csharp
GattCharacteristicProperties properties = characteristic.CharacteristicProperties

if(properties.HasFlag(GattCharacteristicProperties.Read))
{
    // This characteristic supports reading from it.
}
if(properties.HasFlag(GattCharacteristicProperties.Write))
{
    // This characteristic supports writing to it.
}
if(properties.HasFlag(GattCharacteristicProperties.Notify))
{
    // This characteristic supports subscribing to notifications.
}
```

읽기, 지원 되는 경우 값을 읽을 수 있습니다. 
```csharp
GattReadResult result = await selectedCharacteristic.ReadValueAsync();
if (result.Status == GattCommunicationStatus.Success)
{
    var reader = DataReader.FromBuffer(result.Value);
    byte[] input = new byte[reader.UnconsumedBufferLength];
    reader.ReadBytes(input);
    // Utilize the data as needed
}
```
특성에 대 한 쓰기가 비슷한 패턴을 따릅니다. 
```csharp
var writer = new DataWriter();
// WriteByte used for simplicity. Other commmon functions - WriteInt16 and WriteSingle
writer.WriteByte(0x01);

GattReadResult result = await selectedCharacteristic.WriteValueAsync(writer.DetachBuffer());
if (result.Status == GattCommunicationStatus.Success)
{
    // Successfully wrote to device
}
```
> **팁**: [DataReader](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datareader.aspx) 및 [DataWriter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datawriter.aspx)를 사용 하 여 익숙해질 합니다. 다양 한 Bluetooth Api에서 가져올 원시 버퍼 함께 작업 하는 경우 해당 기능 dispensible 없게 됩니다. 
## <a name="subscribing-for-notifications"></a>알림 구독

표시 또는 Notify 특성을 지원 하는지 확인 (있는지 확인 하는 특성 속성을 확인 합니다.). 

> **별도로**: 나타내는 각 값이 변경 이벤트 클라이언트 장치에서 승인을 함께 사용할 때문에 안정적으로 간주 됩니다. 알림 되므로 보다 널리 사용 되기 대부분 GATT 트랜잭션을 아니라 절전 보다는 매우 안정적 이어야 합니다. 어떤 경우 든 그은 처리 컨트롤러 계층에서 되므로 앱 포함 되지 않습니다. 단순히 "알림"으로 자신에 지칭 전체적으로 하지만 알고 이제. 

두 가지 방법으로 알림 시작 하기 전에 처리를 수행 합니다.
- 클라이언트 특성 구성 설명자 (CCCD)에 쓰기
- Characteristic.ValueChanged 이벤트를 처리

CCCD에 대 한 쓰기가이 클라이언트가 때마다 해당 특정 특성 값이 변경 된 알고 서버 장치에 지시 합니다. 이렇게 하려면 다음을 수행합니다. 

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```
이제 GattCharacteristic의 경우 재정의 값을 가져옵니다 원격 장치에서 변경 된 때마다 호출 됩니다. 남아 있는 모든 처리기를 구현 하는 것: 

```csharp
characteristic.ValueChanged += Characteristic_ValueChanged;
// ... 

void Characteristic_ValueChanged(GattCharacteristic sender, 
                                    GattValueChangedEventArgs args)
{
    // An Indicate or Notify reported that the value has changed.
    var reader = DataReader.FromBuffer(args.CharacteristicValue)
    // Parse the data however required.
}
```

