---
title: Bluetooth GATT 서버
description: 이 문서에서는 일반적인 사용 사례에 대한 샘플 코드와 함께 UWP(유니버설 Windows 플랫폼) 앱의 Bluetooth GATT(일반 특성 프로필) 서버에 대한 개요를 제공합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f59ae45486ee72f9d901898f6b03674e6b3e299c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370097"
---
# <a name="bluetooth-gatt-server"></a>Bluetooth GATT 서버


**중요 한 Api**
- [**Windows.Devices.Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)


이 문서에서는 일반적인 GATT 서버 작업에 대한 샘플 코드 함께 UWP(유니버설 Windows 플랫폼) 앱용 Bluetooth GATT(일반 특성 프로필) 서버 API에 대해 설명합니다. 
- 지원되는 서비스 정의
- 원격 클라이언트에서 서버를 찾을 수 있도록 서버 게시
- 서비스에 대한 지원 알림
- 읽기 및 쓰기 요청에 응답
- 구독한 클라이언트에 알림을 보냄

## <a name="overview"></a>개요
Windows는 일반적으로 클라이언트 역할로 작동합니다. 그러나 많은 경우 Windows에서 Bluetooth LE GATT 서버로서도 작동해야 합니다. 대부분의 IoT 장치 시나리오에서 대부분의 플랫폼 간 BLE 통신과 함께 GATT 서버로 Windows가 필요합니다. 또한 근처 착용식 디바이스에 알림을 보내는 데 이 기술을 사용해야 하는 시나리오가 흔하게 사용되는 경우입니다.  
> 계속하기 전에 [GATT 클라이언트 문서](gatt-client.md)의 모든 개념을 숙지해야 합니다.  

서버 운영은 서비스 공급자 및 GattLocalCharacteristic을 중심으로 이루어집니다. 이 두 클래스를 선언, 구현 및 원격 장치에는 데이터의 계층 구조를 노출 하는 데 필요한 기능을 제공 합니다.

## <a name="define-the-supported-services"></a>지원되는 서비스 정의
앱은 Windows에 의해 게시될 하나 이상의 서비스를 선언할 수 있습니다. 각 서비스는 UUID로 고유하게 식별됩니다. 

### <a name="attributes-and-uuids"></a>특성 및 UUID
각 서비스, 특성 및 설명자는 128비트 UUID로 정의됩니다.
> 모든 Windows API는 GUID 용어를 사용하지만 Bluetooth 표준은 이를 UUID로 정의합니다. 목적을 위해 이 두 용어는 상호 변경 가능하므로 계속해서 UUID라는 용어를 사용합니다. 

특성이 표준이며 Bluetooth SIG 정의에 정의된 경우 해당하는 약식 16비트 ID 또한 가질 수 있습니다(예: 배터리 수준 UUID는 0000**2A19**-0000-1000-8000-00805F9B34FB이며 약식 ID는 0x2A19임). 이러한 표준 UUID는 [GattServiceUuids](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids) 및 [GattCharacteristicUuids](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids)에서 볼 수 있습니다.

앱에서 고유사용자 지정 서비스를 구현하는 경우 사용자 지정 UUID를 생성해야 합니다. 이는 Visual Studio에서 Tools -> CreateGuid("xxxxxxxx-xxxx-... xxxx" 형식에서 이를 얻으려면 옵션 5를 사용)를 사용하여 쉽게 완료할 수 있습니다. 이제 이 UUID를 사용하여 새 로컬 서비스, 특성 또는 설명자를 선언할 수 있습니다.

#### <a name="restricted-services"></a>제한된 서비스
다음 서비스는 시스템에 의해 예약되며 현재로서 게시될 수 없습니다.
1. 장치 정보 서비스(DIS)
2. 일반 특성 프로필 서비스(GATT)
3. 일반 액세스 프로필 서비스(GAP)
4. 휴먼 인터페이스 장치 서비스(HOGP)
5. 검사 매개 변수 서비스(SCP)

> 차단된 서비스를 만들려고 시도할 때 CreateAsync에 대한 호출에서 BluetoothError.DisabledByPolicy 가 반환됩니다.

#### <a name="generated-attributes"></a>생성된 특성
다음 설명자는 특성을 생성하는 동안 제공된 GattLocalCharacteristicParameters에 따라 시스템에 의해 자동으로 생성됩니다.
1. 클라이언트 특성 구성(특징이 표시 가능 또는 알림 가능으로 표시된 경우).
2. 특성 사용자 설명(UserDescription 속성이 설정된 경우). 자세한 내용은 GattLocalCharacteristicParameters.UserDescription 속성을 참조하십시오.
3. 특성 형식(지정된 각 프레젠테이션 형식에 대한 하나의 설명자).  자세한 내용은 GattLocalCharacteristicParameters.PresentationFormats 속성을 참조하십시오.
4. 특성 집계 형식(한 개 이상의 프레젠테이션 형식이 지정된 경우).  GattLocalCharacteristicParameters.자세한 내용은 PresentationFormats 속성을 참조하십시오.
5. 특성 확장 속성(특성이 확장된 속성 비트로 표시된 경우).

> 확장 속성 설명자 값은 ReliableWrites 및 WritableAuxiliaries 특성 속성을 통해 결정됩니다.

> 예약된 설명자를 만들려고 시도하면 예외가 발생합니다.

> 브로드캐스트는 현재 지원되지 않습니다.  브로드캐스트 GattCharacteristicProperty를 지정하면 예외가 발생합니다.

### <a name="build-up-the-hierarchy-of-services-and-characteristics"></a>서비스 및 특성의 계층 구조를 구축
GattServiceProvider는 루트 기본 서비스 정의를 만들고 보급하는 데 사용됩니다.  각 서비스에는 GUID에서 사용되는 고유한 ServiceProvider 개체가 필요합니다. 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> 기본 서비스는 GATT 트리의 최상위 수준입니다. 기본 서비스에는 다른 서비스뿐만 아니라 특성 또한 포함됩니다('포함' 또는 보조 서비스라고 함). 

이제 필요한 특성과 설명자로 서비스를 채웁니다.

```csharp
GattLocalCharacteristicResult characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid1, ReadParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_readCharacteristic = characteristicResult.Characteristic;
_readCharacteristic.ReadRequested += ReadCharacteristic_ReadRequested;

characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid2, WriteParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_writeCharacteristic = characteristicResult.Characteristic;
_writeCharacteristic.WriteRequested += WriteCharacteristic_WriteRequested;

characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid3, NotifyParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_notifyCharacteristic = characteristicResult.Characteristic;
_notifyCharacteristic.SubscribedClientsChanged += SubscribedClientsChanged;
```
위에서 볼 수 있는 것과 같이 이는 각 특성이 지원하는 작업에 대한 이벤트 처리기를 선언하는 데 좋은 위치이기도 합니다.  요청에 제대로 응답하기 위해 앱은 특성이 지원하는 각 요청 유형별고 이벤트 처리기를 정의하고 설정해야 합니다.  처리기 등록에 실패하면 요청이 시스템에서 *UnlikelyError*로 즉시 완료됩니다.

### <a name="constant-characteristics"></a>상수 특성
경우에 따라, 앱의 수명 과정 동안 변경하지 않는 특성 값이 있습니다. 이러한 경우에 불필요한 앱 활성화를 방지하기 위해 상수 특성을 선언하는 것이 좋습니다. 

```csharp
byte[] value = new byte[] {0x21};
var constantParameters = new GattLocalCharacteristicParameters
{
    CharacteristicProperties = (GattCharacteristicProperties.Read),
    StaticValue = value.AsBuffer(),
    ReadProtectionLevel = GattProtectionLevel.Plain,
};

var characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid4, constantParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
```
## <a name="publish-the-service"></a>서비스 게시
서비스가 완전히 정의되면 다음 단계는 서비스에 대한 지원 게시입니다. 이는 원격 디바이스에서 서비스 검색을 수행할 때 서비스가 반환되어야 함을 OS에 알립니다.  IsDiscoverable 및 IsConnectable의 두 개의 속성을 설정해야 합니다.  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**: 원격 장치에 장치를 검색할 수 있도록 보급 알림 이름을 알립니다.
- **IsConnectable**:  주변 역할에서 사용 하기 위해 연결 가능한 광고를 알립니다.

> 서비스가 검색 가능하며 연결 가능할 때 시스템은 알림 패킷에 서비스 UUID를 추가합니다.  알림 패킷은 고작 31바이트이며 128비트 UUID는 이 중 16바이트를 차지합니다!

> 전면에 서비스가 게시되는 경우 응용 프로그램이 일시 중지될 때 응용 프로그램은 StopAdvertising을 호출해야 합니다.

## <a name="respond-to-read-and-write-requests"></a>읽기 및 쓰기 요청에 응답
위에서 설명한 것처럼 필요한 특성을 선언할 때 GattLocalCharacteristics는 ReadRequested, WriteRequested 및 SubscribedClientsChanged의 3가지 유형의 이벤트를 갖게 됩니다.

### <a name="read"></a>읽기
원격 디바이스에서 특성 값을 읽으려 할 때(상수 값이 아닌 경우), ReadRequested 이벤트가 호출됩니다. 인수(원격 장치에 대한 정보를 포함)와 함께 읽기가 호출된 특성이 대리자에게 전달됩니다. 

```csharp
characteristic.ReadRequested += Characteristic_ReadRequested;
// ... 

async void ReadCharacteristic_ReadRequested(GattLocalCharacteristic sender, GattReadRequestedEventArgs args)
{
    var deferral = args.GetDeferral();
    
    // Our familiar friend - DataWriter.
    var writer = new DataWriter();
    // populate writer w/ some data. 
    // ... 

    var request = await args.GetRequestAsync();
    request.RespondWithValue(writer.DetachBuffer());
    
    deferral.Complete();
}
``` 

### <a name="write"></a>Write
원격 디바이스가 특성에 대한 값을 쓰려고 할 때 특성이 쓰여지는 값 자체 등 원격 장치에 대한 정보와 함께 WriteRequested 이벤트가 호출됩니다. 

```csharp
characteristic.ReadRequested += Characteristic_ReadRequested;
// ...

async void WriteCharacteristic_WriteRequested(GattLocalCharacteristic sender, GattWriteRequestedEventArgs args)
{
    var deferral = args.GetDeferral();
    
    var request = await args.GetRequestAsync();
    var reader = DataReader.FromBuffer(request.Value);
    // Parse data as necessary. 

    if (request.Option == GattWriteOption.WriteWithResponse)
    {
        request.Respond();
    }
    
    deferral.Complete();
}
```
쓰기에는 응답 및 응답 안 함의 2가지 유형이 있습니다. 원격 디바이스에서 어떤 유형의 쓰기를 수행하는지 파악하기 위해 GattWriteOption(GattWriteRequest 개체의 속성)을 사용합니다. 

## <a name="send-notifications-to-subscribed-clients"></a>구독한 클라이언트에 알림을 보냄
GATT 서버 작업 중 가장 갖은 작업으로 알림이 원격 장치에 데이터를 전송하는 중요한 기능을 수행합니다. 경우에 따라 모든 구독한 클라이언트에 알리고자 할 수 있지만 새 값을 보낼 디바이스를 선택하고자 할 수 있습니다. 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

새 디바이스가 알림에 대해 구독을 시작하면 SubscribedClientsChanged 이벤트가 호출됩니다. 

```csharp
characteristic.SubscribedClientsChanged += SubscribedClientsChanged;
// ...

void _notifyCharacteristic_SubscribedClientsChanged(GattLocalCharacteristic sender, object args)
{
    List<GattSubscribedClient> clients = sender.SubscribedClients;
    // Diff the new list of clients from a previously saved one 
    // to get which device has subscribed for notifications. 

    // You can also just validate that the list of clients is expected for this app.  
}

```
> 응용 프로그램은 MaxNotificationSize 속성을 사용하여 특정 클라이언트에 대해 최대 알림 크기를 얻을 수 있습니다.  시스템에 의해 최대 크기보다 큰 모든 데이터는 잘립니다.
