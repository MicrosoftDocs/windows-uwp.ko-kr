---
title: Bluetooth GATT 클라이언트
description: 이 문서에서는 일반적인 사용 사례에 대한 샘플 코드와 함께 UWP(유니버설 Windows 플랫폼) 앱의 Bluetooth GATT(일반 특성 프로필) 클라이언트에 대한 개요를 제공합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3ae656b473a4dd5999588057b0ec970645703eec
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635088"
---
# <a name="bluetooth-gatt-client"></a>Bluetooth GATT 클라이언트


**중요 한 Api**

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

이 문서를 일반적인 GATT 클라이언트 작업에 대한 샘플 코드와 함께 UWP(유니버설 Windows 플랫폼) 앱용 Bluetooth GATT(일반 특성 프로필) 클라이언트 API의 사용을 설명합니다.
- 주변 장치에 대한 쿼리
- 디바이스에 연결
- 장치에 지원되는 서비스와 디바이스의 특징 열거
- 특성 읽기 및 쓰기
- 특성 값이 변경될 때 알림 구독

## <a name="overview"></a>개요
개발자는 [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) 네임스페이스의 API를 사용하여 Bluetooth LE 디바이스에 액세스할 수 있습니다. Bluetooth LE 장치는 다음 컬렉션을 통해 장치의 기능을 표시합니다.

-   서비스
-   특성
-   설명자

서비스는 LE 장치의 기능 계약을 정의하고 서비스를 정의하는 특성 컬렉션을 포함합니다. 그리고 이러한 특성에는 특성을 설명하는 설명자가 포함됩니다. 이 3개의 용어는 일반적으로 디바이스의 특성이라고 합니다.

Bluetooth LE GATT API는 원시 전송에 대한 액세스보다는 개체와 함수를 표시합니다. GATT API를 통해 개발자는 다음 작업을 수행할 수 있는 Bluetooth LED 장치도 사용할 수 있습니다.

-   특성 검색 수행
-   읽기 및 쓰기 특성 값
-   특성의 ValueChanged 이벤트에 대한 콜백 등록

유용한 구현을 만들려면 개발자가 응용 프로그램에서 사용하려는 GATT 서비스 및 특성에 대해 사전에 잘 알고 있어야 하며 API에서 제공하는 이진 데이터가 사용자에게 제공되기 전에 유용한 데이터로 변환되도록 특정 특성 값을 처리해야 합니다. Bluetooth GATT API는 Bluetooth LE 장치와 통신하는 데 필요한 기본 요소만 표시합니다. 데이터를 해석하려면 Bluetooth SIG 표준 프로필이나 장치 공급업체에서 구현한 사용자 지정 프로필을 통해 응용 프로그램 프로필을 정의해야 합니다. 프로필은 교환되는 데이터가 어떻게 표현되고 그 데이터를 어떻게 해석해야 하는지를 규정하는 응용 프로그램과 장치 간의 바인딩 계약을 만듭니다.

편의상 Bluetooth SIG는 [사용 가능한 공용 프로필](https://www.bluetooth.com/specifications/adopted-specifications#gattspec) 목록을 유지합니다.

## <a name="query-for-nearby-devices"></a>주변 장치에 대한 쿼리
주변 장치에 대해 쿼리하는 두 가지 주요 방법이 있습니다.
- Windows.Devices.Enumeration의 DeviceWatcher
- Windows.Devices.Bluetooth.Advertisement의 AdvertisementWatcher

두 번째 방법은 [알림](ble-beacon.md) 문서에서 자세히 다루므로 여기에서는 자세히 다루지 않지만 기본 개념은 특정 [알림 필터](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter.aspx)를 충족하는 근접 장치의 Bluetooth 주소를 찾는 것입니다. 주소를 가져오면 [BluetoothLEDevice.FromBluetoothAddressAsync](https://msdn.microsoft.com/en-us/library/windows/apps/mt608819.aspx)를 호출하여 장치에 참조를 가져올 수 있습니다. 

이제 DeviceWatcher 메서드로 돌아가겠습니다. Bluetooth LE 디바이스는 Windows의 다른 디바이스와 유사하며 [열거형 API](https://msdn.microsoft.com/library/windows/apps/BR225459)를 사용하여 쿼리할 수 있습니다. [DeviceWatcher](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher) 클래스를 사용하고 디바이스를 지정하는 쿼리 문자열을 전달하여 다음을 찾습니다. 

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
DeviceWatcher를 시작하면 의심스러운 장치에 대해 [추가된](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher.added) 이벤트에 대해 처리기에서 쿼리를 충족하는 각 장치에 대한 [DeviceInformation](https://msdn.microsoft.com/library/windows/apps/br225393) 알림을 받습니다. DeviceWatcher에 대한 더 자세한 내용은 [Github에 대한](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) 완전한 샘플을 참조하세요. 

## <a name="connecting-to-the-device"></a>디바이스에 연결
원하는 디바이스를 검색한 후 [DeviceInformation.Id](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.id)를 사용하여 의심스러운 디바이스에 대한 Bluetooth LE 디바이스 개체를 가져옵니다. 

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```
반면에 디바이스에 대한 BluetoothLEDevice 개체에 모든 참조를 삭제하면 (그리고 시스템의 다른 모든 앱에 디바이스에 대한 참조가 없는 경우) 짧은 시간 제한 이후 자동 분리가 트리거됩니다. 

```csharp
bluetoothLeDevice.Dispose();
```
앱에서 디바이스에 다시 액세스해야 할 경우 간단하게 다시 장치 개체 만들고 특성에 액세스하면(다음 섹션에서 설명) 필요할 경우 OS가 다시 연결되도록 트리거합니다. 장치가 근처에 있을 경우 장치에 액세스할 수 있으며 그렇지 않은 경우 DeviceUnreachable 오류가 반환됩니다.  

## <a name="enumerating-supported-services-and-characteristics"></a>지원되는 서비스와 특성 열거
이제 BluetoothLEDevice 개체를 가져왔으므로 다음 단계는 디바이스에서 어떤 데이터를 제공하는지 검색할 차례입니다. 이를 위한 첫 번째 단계는 서비스에 대해 쿼리를 수행하는 것입니다. 

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```
관심 있는 서비스를 확인한 후 다음 단계는 특징을 쿼리하는 것입니다. 

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```  
그러면 OS에서 작업을 수행할 수 있는 GattCharacteristic 개체의 읽기 전용 목록을 반환합니다.

## <a name="perform-readwrite-operations-on-a-characteristic"></a>특성에 읽기/쓰기 작업 수행

특성은 GATT의 기반 통신의 기본적인 단위입니다. 장치에서 고유한 데이터를 나타내는 값을 포함합니다. 예를 들어, 배터리 수준 특성에는 디바이스의 배터리 잔량을 나타내는 값이 있습니다.

어떤 작업이 지원되는지 확인하려면 특성 속성을 읽습니다.
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

읽기가 지원되는 경우 다음 값을 읽을 수 있습니다. 
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
특성에 대한 쓰기는 다음과 비슷한 패턴을 따릅니다. 
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
> **팁**: 사용 하 여 익숙해지는 [DataReader](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datareader.aspx) 하 고 [DataWriter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datawriter.aspx)합니다. 해당 기능은 Bluetooth API에서 상당수 가져온 원시 버퍼로 작업할 때 필요합니다. 
## <a name="subscribing-for-notifications"></a>알림 구독

특성이 표시 또는 알림을 지원하는지 확인합니다(확실하게 하기 위해 특성 속성 확인). 

> **따로**: 표시 된 클라이언트 장치에서 승인을 사용 하 여 각 값이 변경 됨 이벤트는 결합 때문에 더 신뢰할 수 있는 간주 됩니다. 알림은 대부분의 GATT 트랜잭션이 매우 안정적이기 보다는 전원을 절약하기 때문에 더욱 흔합니다. 어떤 경우든 이 모두 컨트롤러 계층에서 처리되므로 앱은 관여하지 않습니다. 간단히 이를 모두 "알림"으로 지칭하지만 이제 알 수 있습니다. 

알림을 가져오기 전해 수행해야 할 두 가지 작업이 있습니다.
- 클라이언트 특성 구성 설명자(CCCD)에 쓰기
- Characteristic.ValueChanged 이벤트 처리

CCCD에 대한 쓰기는 특정 특성 값을 변경할 때마다 이 클라이언트에서 알림을 받기 원한다는 것을 서버 장치에 알려줍니다. 이렇게 하려면 다음을 수행합니다. 

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```
이제 원격 디바이스에서 값이 변경될 때마다 GattCharacteristic의 ValueChanged 이벤트가 호출됩니다. 처리기를 구현하기 위해 남은 것은 다음과 같습니다. 

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


