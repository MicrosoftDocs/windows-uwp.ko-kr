---
title: Bluetooth GATT 클라이언트
description: 이 문서에서는 일반적인 사용 사례에 대 한 샘플 코드와 함께 GATT (Bluetooth Generic Attribute Profile) for 유니버설 Windows 플랫폼 (UWP) 앱에 대 한 개요를 제공 합니다.
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 339a154c3acf39c4f574d22907cf697db658552b
ms.sourcegitcommit: 74c2c878b9dbb92785b89f126359c3f069175af2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93122404"
---
# <a name="bluetooth-gatt-client"></a>Bluetooth GATT 클라이언트

이 문서에서는 일반적인 GATT 클라이언트 작업에 대 한 샘플 코드와 함께 UWP (GATT) 클라이언트 Api 앱에 대 유니버설 Windows 플랫폼 한 Bluetooth Generic 특성 ()을 사용 하는 방법을 보여 줍니다.

- 주변 장치에 대 한 쿼리
- 장치에 연결
- 장치에서 지원 되는 서비스 및 특성 열거
- 특성에 대 한 읽기 및 쓰기
- 특성 값이 변경 될 때 알림 구독

> [!Important]
> *Appxmanifest.xml* 에서 "bluetooth" 기능을 선언 해야 합니다.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

> **중요 API**
>
> - [**Windows.Devices.Bluetooth**](/uwp/api/Windows.Devices.Bluetooth)
> - [**Windows..**](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)

## <a name="overview"></a>개요

개발자는 [**Windows**](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile) 의 api를 사용 하 여 bluetooth LE 장치에 액세스할 수 있습니다. Bluetooth LE 장치는 다음 컬렉션을 통해 해당 기능을 노출 합니다.

- 서비스
- 특징
- 설명자

서비스는 LE 장치의 기능 계약을 정의 하 고 서비스를 정의 하는 특성의 컬렉션을 포함 합니다. 이러한 특성에는 특성을 설명 하는 설명자가 포함 됩니다. 이러한 3 가지 용어는 일반적으로 장치 특성 이라고 합니다.

Bluetooth LE GATT Api는 원시 전송에 액세스 하는 대신 개체 및 함수를 노출 합니다. 또한 개발자는 GATT Api를 사용 하 여 다음 작업을 수행할 수 있는 기능을 통해 Bluetooth LE 장치를 사용할 수 있습니다.

- 특성 검색 수행
- 특성 값 읽기 및 쓰기
- 특성 ValueChanged 이벤트에 대 한 콜백 등록

유용한 구현을 만들려면 개발자가 응용 프로그램에서 사용 하려는 GATT 서비스 및 특성에 대 한 사전 지식이 있어야 합니다 .이 특성 값은 API에서 제공 하는 이진 데이터가 사용자에 게 표시 되기 전에 유용한 데이터로 변환 됩니다. Bluetooth GATT Api는 Bluetooth LE 장치와 통신 하는 데 필요한 기본 기본 형식만 노출 합니다. 데이터를 해석 하려면 Bluetooth SIG 표준 프로필 또는 장치 공급 업체에서 구현 하는 사용자 지정 프로필에 의해 응용 프로그램 프로필을 정의 해야 합니다. 프로필은 교환 된 데이터가 나타내는 내용과이를 해석 하는 방법에 대 한 응용 프로그램과 장치 간에 바인딩 계약을 만듭니다.

편의를 위해 Bluetooth SIG는 사용 가능한 [공개 프로필의 목록을](https://www.bluetooth.com/specifications/adopted-specifications#gattspec) 유지 관리 합니다.

## <a name="examples"></a>예

전체 샘플은 [Bluetooth 저 에너지 샘플](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BluetoothLE)을 참조 하세요.

### <a name="query-for-nearby-devices"></a>주변 장치에 대 한 쿼리

주변 장치에 대해 쿼리 하는 데는 두 가지 주요 방법이 있습니다.

- Windows의 DeviceWatcher. 열거형
- AdvertisementWatcher의

두 번째 방법은 [광고](ble-beacon.md) 설명서의 길이에 설명 되어 있지만 여기서는 자세히 다루지 않지만 특정 [광고 필터](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter)를 충족 하는 주변 장치의 Bluetooth 주소를 찾는 것이 기본 개념입니다. 주소가 있으면 [BluetoothLEDevice. FromBluetoothAddressAsync](/uwp/api/windows.devices.bluetooth.bluetoothledevice.frombluetoothaddressasync) 를 호출 하 여 장치에 대 한 참조를 가져올 수 있습니다.

이제 DeviceWatcher 메서드로 돌아갑니다. Bluetooth LE 장치는 Windows의 다른 장치와 마찬가지로 [열거 api](/uwp/api/Windows.Devices.Enumeration)를 사용 하 여 쿼리할 수 있습니다. [Devicewatcher](/uwp/api/windows.devices.enumeration.devicewatcher) 클래스를 사용 하 여 찾을 장치를 지정 하는 쿼리 문자열을 전달 합니다.

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

DeviceWatcher를 시작 하면 해당 장치에 대해 [추가](/uwp/api/windows.devices.enumeration.devicewatcher.added) 된 이벤트에 대 한 처리기의 쿼리를 충족 하는 각 장치에 대 한 [devicewatcher](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 을 받게 됩니다. DeviceWatcher에 대 한 자세한 내용은 [Github에서](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing)전체 샘플을 참조 하세요.

### <a name="connecting-to-the-device"></a>장치에 연결

원하는 장치가 검색 되 면 [DeviceInformation.Id](/uwp/api/windows.devices.enumeration.deviceinformation.id) 를 사용 하 여 해당 장치에 대 한 Bluetooth LE 장치 개체를 가져옵니다.

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```

반면, 장치에 대 한 BluetoothLEDevice 개체에 대 한 모든 참조를 삭제 하면 (시스템의 다른 앱에 장치에 대 한 참조가 없는 경우) 작은 시간 제한 기간 후에 자동 연결 해제가 트리거됩니다.

```csharp
bluetoothLeDevice.Dispose();
```

앱에서 다시 장치에 액세스 해야 하는 경우 장치 개체를 다시 만들고 특성에 액세스 하기만 하면 (다음 섹션에서 설명) 필요한 경우 OS가 다시 연결 됩니다. 장치가 근처에 있으면 장치에 대 한 액세스 권한을 얻게 됩니다. 그렇지 않으면 DeviceUnreachable 수 없음 오류가 반환 됩니다.  

> [!NOTE]
> 이 메서드를 호출 하 여 [BluetoothLEDevice](/uwp/api/windows.devices.bluetooth.bluetoothledevice) 개체를 만드는 경우에는 연결을 시작 하지 않아도 됩니다. 연결을 시작 하려면 [Gattsession](/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattsession.maintainconnection) 을로 설정 `true` 하거나 **BluetoothLEDevice** 에서 캐시 되지 않은 service discovery 메서드를 호출 하거나 장치에 대해 읽기/쓰기 작업을 수행 합니다.
>
> - **MaintainConnection** 가 true로 설정 된 경우 시스템은 연결을 무기한 대기 하 고 장치를 사용할 수 있을 때 연결 됩니다. **Gattsession** 는 속성 이므로 응용 프로그램이 대기 하는 것은 없습니다.
> - GATT의 서비스 검색 및 읽기/쓰기 작업의 경우 시스템은 유한 하지만 변수 시간을 기다립니다. 순간부터 분의 중요 한 항목입니다. 스택에서 트래픽을은 하 고 요청을 큐에 대기 하는 방법을 결정 합니다. 보류 중인 다른 요청이 없고 원격 장치에 연결할 수 없는 경우 시스템은 시간이 초과 될 때까지 7 초 동안 대기 합니다. 보류 중인 다른 요청이 있는 경우 큐의 각 요청을 처리 하는 데 7 초 정도 소요 될 수 있으므로 나중에 큐의 끝 부분을 향해 대기 시간이 길어집니다.
>
> 현재는 연결 프로세스를 취소할 수 없습니다.

### <a name="enumerating-supported-services-and-characteristics"></a>지원 되는 서비스 및 특성 열거

이제 BluetoothLEDevice 개체를 만들었으므로 다음 단계는 장치에서 노출 하는 데이터를 검색 하는 것입니다. 이 작업을 수행 하는 첫 번째 단계는 서비스를 쿼리 하는 것입니다.

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();

if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```

관심이 있는 서비스를 확인 한 후 다음 단계는 특성을 쿼리 하는 것입니다.

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();

if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```

OS는 작업을 수행할 수 있는 GattCharacteristic 개체의 읽기 전용 목록을 반환 합니다.

### <a name="perform-readwrite-operations-on-a-characteristic"></a>특성에 대 한 읽기/쓰기 작업 수행

특성은 GATT 기반 통신의 기본 단위입니다. 장치에 있는 데이터의 고유 부분을 나타내는 값을 포함 합니다. 예를 들어 배터리 수준 특성에는 장치의 배터리 수준을 나타내는 값이 있습니다.

지원 되는 작업을 확인 하려면 특성 속성을 참조 하세요.

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

읽기가 지원 되는 경우 값을 읽을 수 있습니다.

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

특성에 대 한 쓰기는 비슷한 패턴을 따릅니다.

```csharp
var writer = new DataWriter();
// WriteByte used for simplicity. Other common functions - WriteInt16 and WriteSingle
writer.WriteByte(0x01);

GattCommunicationStatus result = await selectedCharacteristic.WriteValueAsync(writer.DetachBuffer());
if (result == GattCommunicationStatus.Success)
{
    // Successfully wrote to device
}
```

> **팁** : [DataReader](/uwp/api/windows.storage.streams.datareader) 및 [datawriter 여부](/uwp/api/windows.storage.streams.datawriter) 는 많은 Bluetooth api에서 가져온 원시 버퍼를 사용할 때 위한 됩니다.

### <a name="subscribing-for-notifications"></a>알림 구독

특성이 표시 또는 알림을 지원 하는지 확인 합니다 (특성 속성을 확인 하 여 확인).

> **제외** : 각 값 변경 이벤트는 클라이언트 장치의 승인에 연결 되므로 표시를 더 안정적으로 간주 합니다. 대부분의 GATT 트랜잭션은 매우 안정적이 지 않고 전력을 절약 하므로 알림이 더 많이 발생 합니다. 어떤 경우 든 모든는 컨트롤러 계층에서 처리 되므로 앱이 포함 되지 않습니다. 단순히 "알림" 이라고 통칭 하지만 이제 알고 있습니다.

알림을 받기 전에 다음 두 가지 작업을 수행 해야 합니다.

- 클라이언트 특성 구성 설명자 (CCCD)에 쓰기
- ValueChanged 이벤트를 처리 합니다.

CCCD에 쓰려면이 클라이언트에서 특정 특성 값이 변경 될 때마다이를 확인 하려고 하는 서버 장치에 지시 합니다. 가상 하드 디스크 파일에 대한 중요 정보를 제공하려면

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```

이제는 원격 장치에서 값이 변경 될 때마다 GattCharacteristic ValueChanged 이벤트가 호출 됩니다. 모든 것은 처리기를 구현 하는 것입니다.

```csharp
characteristic.ValueChanged += Characteristic_ValueChanged;

...

void Characteristic_ValueChanged(GattCharacteristic sender,
                                    GattValueChangedEventArgs args)
{
    // An Indicate or Notify reported that the value has changed.
    var reader = DataReader.FromBuffer(args.CharacteristicValue)
    // Parse the data however required.
}
```

