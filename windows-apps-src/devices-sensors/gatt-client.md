---
title: Bluetooth GATT 클라이언트
description: 이 문서에서는 유니버설 Windows 플랫폼 (UWP) 앱의 경우 일반적인 사용 사례에 대 한 샘플 코드와 함께 Bluetooth 일반 특성 프로필 (GATT) 클라이언트의 개요를 제공 합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3ae656b473a4dd5999588057b0ec970645703eec
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8339087"
---
# <a name="bluetooth-gatt-client"></a>Bluetooth GATT 클라이언트


**중요 API**

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

이 문서에서는 유니버설 Windows 플랫폼 (UWP) 앱의 경우 일반적인 GATT 클라이언트 작업에 대 한 샘플 코드와 함께 Bluetooth 일반 특성 (GATT) 클라이언트 Api의 사용 하는 방법을 보여 줍니다.
- 주변 장치에 대 한 쿼리
- 장치에 연결
- 지원 되는 서비스 및 특성에 장치를 열거 합니다.
- 읽기 및 쓰기 특성
- 알림을 표시 하는 경우 값이 변경에 대 한 구독

## <a name="overview"></a>개요
개발자는 Bluetooth LE 장치에 액세스할 수 [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) 네임 스페이스의 Api를 사용할 수 있습니다. Bluetooth LE 장치는 다음 컬렉션을 통해 장치의 기능을 표시합니다.

-   서비스
-   특성
-   설명자

서비스는 LE 장치의 기능 계약을 정의 하 고 서비스를 정의 하는 특성 컬렉션을 포함할 합니다. 그리고 이러한 특성에는 특성을 설명하는 설명자가 포함됩니다. 이러한 3 용어는 일반적으로 장치의 특성 라고도 합니다.

Bluetooth LE GATT Api 원시 전송에 대 한 액세스 대신 개체 및 기능을 노출 합니다. GATT Api Bluetooth LE 장치를 사용 하 여 다음 작업을 수행 하는 기능을 사용 하 여 작동 하도록 개발자가 사용 하도록 설정할 수도 있습니다.

-   특성 검색 수행
-   읽기 및 쓰기 특성 값
-   특성의 ValueChanged 이벤트에 대 한 콜백 등록

유용한 구현을 만들려면 개발자 이전 알고 있어야 GATT 서비스 및 응용 프로그램을 사용 하려는 특성 및 프로세스에 API에서 제공 되는 이진 데이터는 변환 되도록 특정 특성 값 사용자에 게 제공 되기 전에 유용한 데이터입니다. Bluetooth GATT API는 Bluetooth LE 장치와 통신하는 데 필요한 기본 요소만 표시합니다. 데이터를 해석하려면 Bluetooth SIG 표준 프로필이나 장치 공급업체에서 구현한 사용자 지정 프로필을 통해 응용 프로그램 프로필을 정의해야 합니다. 프로필은 교환되는 데이터가 어떻게 표현되고 그 데이터를 어떻게 해석해야 하는지를 규정하는 응용 프로그램과 장치 간의 바인딩 계약을 만듭니다.

편의상 Bluetooth SIG는 [사용 가능한 공용 프로필](https://www.bluetooth.com/specifications/adopted-specifications#gattspec) 목록을 유지합니다.

## <a name="query-for-nearby-devices"></a>주변 장치에 대 한 쿼리
주변 장치 쿼리 주요 방법은 두 가지 있습니다.
- Windows.Devices.Enumeration에서 DeviceWatcher
- Windows.Devices.Bluetooth.Advertisement에는 AdvertisementWatcher

두 번째 메서드는 설명 길게 [광고](ble-beacon.md) 설명서에서 설명 하지 않습니다 하므로 훨씬 여기 이지만 기본 개념은 특정 [광고 필터](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter.aspx)조건을 충족 하는 주변 장치의 Bluetooth 주소를 찾을 수 있습니다. 주소를 구성한 후 장치에 대 한 참조를 가져오려면 [BluetoothLEDevice.FromBluetoothAddressAsync](https://msdn.microsoft.com/en-us/library/windows/apps/mt608819.aspx) 호출할 수 있습니다. 

이제 DeviceWatcher 메서드를 다시 합니다. Bluetooth LE 장치는 Windows에서 다른 디바이스와 마찬가지로 이며 [열거형 Api](https://msdn.microsoft.com/library/windows/apps/BR225459)를 사용 하 여 쿼리할 수 있습니다. [DeviceWatcher](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher) 클래스를 사용 하 고 장치에 대 한 표시를 지정 하는 쿼리 문자열을 전달 합니다. 

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
DeviceWatcher를 시작한 후에 해당 장치에 대 한 [추가](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher.added) 이벤트 처리기에서 쿼리를 충족 하는 각 장치에 대 한 [DeviceInformation](https://msdn.microsoft.com/library/windows/apps/br225393) 를 받게 됩니다. DeviceWatcher 더 자세히 보기에 대 한 전체 [github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing)샘플을 참조 하세요. 

## <a name="connecting-to-the-device"></a>장치에 연결
원하는 장치를 검색 한 후 [DeviceInformation.Id](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.id) 사용 하 여 해당 장치에 대 한 Bluetooth LE 장치 개체를 가져옵니다. 

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```
반면에 BluetoothLEDevice에 대 한 모든 참조가 삭제 장치에 대 한 개체 (및 시스템에 없는 다른 앱에 장치에 대 한 경우)을 트리거합니다 자동 작은 제한 시간 후 분리 합니다. 

```csharp
bluetoothLeDevice.Dispose();
```
앱을 장치에 다시 액세스 하는 경우 간단 하 게 다시 장치 개체를 만들고 (다음 섹션에서 설명) 특성에 액세스을 트리거합니다 OS가 필요한 경우 다시 연결 합니다. 디바이스가 근처 DeviceUnreachable 오류를 지 원하는 반환 될 그렇지 않으면 장치에 대 한 액세스를 봅니다.  

## <a name="enumerating-supported-services-and-characteristics"></a>지원 되는 서비스 및 특성에 열거
BluetoothLEDevice 개체 했으므로 다음 단계 장치를 노출 하는 데이터를 검색 하는 것입니다. 이렇게 하려면 첫 번째 단계는 서비스에 대 한 쿼리 합니다. 

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```
다음 단계는 서비스의 관심을 식별 한 후 특성에 대 한 쿼리 합니다. 

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```  
OS는 GattCharacteristic의 읽기 전용 목록에서 작업을 수행한 다음 수 개체를 반환 합니다.

## <a name="perform-readwrite-operations-on-a-characteristic"></a>특성에 대 한 읽기/쓰기 작업 수행

특성은 GATT의 기본 단위 통신을 시작 합니다. 장치에서 데이터의 각각 다른 부분을 나타내는 값을 포함 합니다. 예를 들어 배터리 수준 특성 장치의 배터리 수준을 나타내는 값을 가집니다.

작업 지원 되는지 파악 하기 특성 속성을 읽습니다.
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

읽기 지원 되는 값을 읽을 수 있습니다. 
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
쓰기 특성을 비슷한 패턴을 따릅니다. 
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
> **팁**: [DataReader](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datareader.aspx) 및 [DataWriter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datawriter.aspx)사용 하 여에 익숙해지면 합니다. 다양 한 Bluetooth Api에서 얻은 원시 버퍼를 사용 하 여 작업을 할 때 기능 dispensible 없게 됩니다. 
## <a name="subscribing-for-notifications"></a>알림에 대 한 구독

표시 또는 알림 특성을 지원 하는지 확인 (특성 속성을 확인). 

> **별도로**: 나타내는 각 값 변경 이벤트는 클라이언트 장치에서 승인을 결합 하기 때문에 보다 안정적으로 간주 됩니다. 알림 아니라 있기 때문에 대부분의 GATT 거래는 대신 절전 매우 안정적 많이 됩니다. 어떤 경우에 모든 처리 컨트롤러 계층에서 되므로 앱 포함 되지 않습니다. 단순히 "notifications"로 통칭 지칭 하 하지만 지금까지 알아보았습니다. 

두 가지 방법으로 알림 가져오기 전에 다루려면.
- 클라이언트 특성 구성 설명자 (CCCD)에 작성 합니다.
- Characteristic.ValueChanged 이벤트 처리

쓰기는 CCCD에는이를 알고 싶을 때마다 해당 특정 특성 값 변경 서버 장치를 알려 줍니다. 이렇게 하려면 다음을 수행합니다. 

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```
이제 GattCharacteristic의 ValueChanged 이벤트 값 원격 디바이스에서 변경 될 때마다 호출 됩니다. 즉 처리기를 구현 하는 것. 

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


