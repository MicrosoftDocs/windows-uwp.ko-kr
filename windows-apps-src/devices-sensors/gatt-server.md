---
author: msatranjr
title: Bluetooth GATT 서버
description: 이 문서에서는 일반적인 사용 사례에 대 한 샘플 코드와 함께 유니버설 Windows 플랫폼 (UWP) 응용 프로그램에 대 한 Bluetooth 일반 특성 프로필 (GATT) 서버에 대 한 개요를 제공 합니다.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 27154fbb535b76995fba97702e65a9c0b2a8291c
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "610766"
---
# <a name="bluetooth-gatt-server"></a>Bluetooth GATT 서버


**중요 API**
- [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)


이 문서에서는 일반적인 GATT 서버 작업에 대 한 샘플 코드와 함께 유니버설 Windows 플랫폼 (UWP) 응용 프로그램에 대 한 Bluetooth 일반 특성 (GATT) 서버 Api를 보여줍니다. 
- 지원 되는 서비스를 정의 합니다.
- 원격 클라이언트에서 검색할 수 있도록 서버를 게시 합니다.
- 광고 서비스에 대 한 지원
- 읽기 및 쓰기 요청에 응답
- 구독 된 클라이언트에 게 알림 보내기

## <a name="overview"></a>개요
Windows는 클라이언트 역할에서 일반적으로 작동 합니다. 그럼에도 불구 하 고, 다양 한 시나리오도 Bluetooth LE GATT 서버 역할을 하는 Windows를 필요로 하는 발생 합니다. 대부분의 플랫폼 BLE 통신 함께 IoT 장치에 대 한 거의 모든 시나리오에는 Windows GATT 서버 수를 수행 해야 합니다. 또한 wearable 장치 근처에 알림 메시지를 보내는도이 기술이 필요한 인기 있는 시나리오 해 왔습니다.  
> [GATT 클라이언트 문서](gatt-client.md) 에서 모든 개념 계속 하기 전에 지우기 없는지 확인 합니다.  

서버 작업은 서비스 공급자와의 GattLocalCharacteristic 중심으로 진행 합니다. 이러한 두 클래스에서 선언, 구현 및 원격 장치에는 데이터의 계층을 노출 하는 데 필요한 기능을 제공 합니다.

## <a name="define-the-supported-services"></a>지원 되는 서비스를 정의 합니다.
앱은 Windows 발행 하는 하나 이상의 서비스를 선언할 수 있습니다. 각 서비스 UUID 고유 하 게 식별 됩니다. 

### <a name="attributes-and-uuids"></a>특성 및 Uuid
각 서비스, 특성 및 설명자 정의 된 자신의 고유한 128 비트 UUID 것으로 합니다.
> GUID, 용어를 사용 하는 모든 Windows Api 하지만 Bluetooth 표준 Uuid로 이러한를 정의 합니다. 이 목적을 위해이 두 용어는 서로 바꿔서 사용할 수 있으므로 UUID 용어를 사용 하 여 계속 합니다. 

특성이 표준 및 Bluetooth SIG 정의 하 여 정의 된 경우 해당 16 비트 짧은 ID가 포함 될 (예: 배터리 수준 UUID는**2A19**0000-0000-1000-8000-00805F9B34FB 및 짧은 ID가 0x2A19). 이러한 표준 Uuid [GattServiceUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids.aspx) 및 [GattCharacteristicUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids.aspx)에서 볼 수 있습니다.

앱을 구현 하는 경우 자신의 사용자 지정 서비스, 사용자 지정 UUID 생성 해야 합니다. 이것은 쉽게 수행 도구를 통해 Visual Studio에서-> CreateGuid (사용 옵션 5 "xxxxxxxx-이며-... 이며" 형식에서 가져올). 이제이 uuid 새 로컬 서비스, 특징 또는 설명자를 선언 하는데 사용할 수 있습니다.

#### <a name="restricted-services"></a>제한 된 서비스
다음 서비스 시스템에서 예약 된 하 고이 이번에 게시할 수 없습니다.
1. 장치 정보 서비스 (DIS)
2. 일반 특성 프로필 서비스 (GATT)
3. 일반 액세스 프로필 서비스 (간격)
4. 휴먼 인터페이스 장치 서비스 (HOGP)
5. 매개 변수 서비스 (SCP)를 검색 합니다.

> 차단 된 서비스를 만들려고 하는 CreateAsync에 대 한 호출에서 반환 되는 BluetoothError.DisabledByPolicy에 발생 합니다.

#### <a name="generated-attributes"></a>생성 된 특성
다음 설명자는 특성을 만드는 동안 제공 GattLocalCharacteristicParameters 기반 시스템에 의해 자동으로 생성 합니다.
1. 클라이언트 (특성으로 indicatable 또는 notifiable 된다고 나타납니다) 하는 경우 특성 구성 합니다.
2. Characteristic 사용자 설명 (UserDescription 속성이 설정 된 경우). 추가 정보에 대 한 GattLocalCharacteristicParameters.UserDescription 속성을 참고 하십시오.
3. Characteristic 형식 (지정 된 각 프레젠테이션 형식에 대 한 하나의 설명자)입니다.  추가 정보에 대 한 GattLocalCharacteristicParameters.PresentationFormats 속성을 참고 하십시오.
4. Characteristic 집계 형식 (둘 이상의 프레젠테이션 형식 지정 된 경우).  추가 정보에 대 한 GattLocalCharacteristicParameters.See PresentationFormats 속성입니다.
5. Characteristic 확장 속성 (특성은 확장 된 속성 비트 기본적으로) 하는 경우.

> 확장 속성 설명자의 값은 ReliableWrites 및 WritableAuxiliaries 특성 속성을 통해 결정 됩니다.

> 예약 된 설명자를 만들 하려고 하면 예외가 발생 합니다.

> 이 이번에는 브로드캐스트 참고 지원 되지 않습니다.  브로드캐스트 GattCharacteristicProperty 지정 예외가 발생 합니다.

### <a name="build-up-the-heirarchy-of-services-and-characteristics"></a>서비스 및 특성의 계층 구조를 작성 합니다.
GattServiceProvider 만들고 광고 루트 기본 서비스 정의 하는데 사용 됩니다.  각 서비스에 직접 ServiceProvider 개체는 GUID를 수행 하는 것이 필요 합니다. 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> 기본 서비스는 GATT 트리의 최상위 수준입니다. 기본 서비스 ('Included' 또는 보조 서비스 라고 함) 다른 서비스와 특성을 포함 합니다. 

이제 필요한 특징 및 설명자를 사용 하 여 서비스를 채웁니다.

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
위에 표시 된 것과 같이 각 특성을 지원 하는 작업에 대 한 이벤트 처리기를 선언 하는 데 이기도 합니다.  앱 해야 올바르게 요청에 응답을 정의 하 고 각 요청 유형 특성의 지원에 대 한 이벤트 처리기를 설정 합니다.  실패 하는 처리기를 등록 하는 *UnlikelyError* 와 시스템에서 즉시 완료 되 고 요청에 발생 합니다.

### <a name="constant-characteristics"></a>상수 특성
경우에 따라서는 응용 프로그램의 수명 동안 변경 되지 것입니다 특성 값입니다. 이 경우 불필요 한 응용 프로그램 정품 인증을 방지 하기 위해 특성 상수를 선언 하는 것이 좋습니다. 

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
## <a name="publish-the-service"></a>서비스를 게시
서비스 완벽 하 게 정의 되 면 다음 단계는 서비스에 대 한 지원을 게시입니다. 이 통해 알립니다 운영 체제 서비스 검색을 수행 하는 원격 장치 서비스 반환 되도록 합니다.  IsDiscoverable 및 IsConnectable-두 속성을 설정 해야 합니다.  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**: 장치를 검색할 수 있도록 하는 광고에서 원격 장치 이름을 알립니다.
- **IsConnectable**: 주변 역할에서 사용 하기 위해 연결 가능한 광고를 알립니다.

> 서비스 검색 가능 하 고 Connectable 모두 되 면 시스템을 사용 하 여 서비스 Uuid 광고 패킷에 추가 합니다.  광고 패킷 중인 31 바이트만 하 고 그 중 16 차지 하는 128 비트 UUID!

> 메모는 서비스는 없이 게시 된 응용 프로그램 호출 해야 StopAdvertising 응용 프로그램을 일시 중단 하는 경우입니다.

## <a name="respond-to-read-and-write-requests"></a>읽기 및 쓰기 요청에 응답
필요한 특성을 선언 하는 동안 위에 살펴본 GattLocalCharacteristics 3 가지 유형의 이벤트-ReadRequested, WriteRequested 및 SubscribedClientsChanged 사용할 수 있습니다.

### <a name="read"></a>Read
원격 장치 특성을 사용 하 여 값을 읽어올 수 시도 (및 상수 값이 아닌) 하는 경우에 ReadRequested 이벤트 호출 됩니다. Args (원격 장치에 대 한 정보를 포함)와 읽기에서 호출 된 특성은 대리인에 게 전달 됩니다. 

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

### <a name="write"></a>쓰기
원격 장치를 특성에 값을 기록 하려고 하는 경우 WriteRequested 이벤트에 대 한 자세한 내용은 원격 장치에 쓸 수 있는 특성 및 값 자체와 호출 됩니다. 

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
와 응답 하지 않고 2 유형의 쓰기-있습니다. GattWriteOption (GattWriteRequest 개체에서 속성)를 사용 하 여 어떤 유형의 쓰기 원격 장치를 수행 하는 알아내야 합니다. 

## <a name="send-notifications-to-subscribed-clients"></a>구독 된 클라이언트에 게 알림 보내기
가장 자주 GATT 서버 작업을 알림 원격 장치 데이터 밀어넣기 (영문)의 중요 한 기능을 수행 합니다. 경우에 따라 모든 구독 된 클라이언트 하지만 새 값을 보낼 수 있는 장치를 선택 하려면 othertimes 알리도록에 하려고: 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

새 장치에 대해 알림을 구독할 때 SubscribedClientsChanged이 이벤트는 호출을 가져옵니다. 

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
> 참고 응용 프로그램가 MaxNotificationSize 속성을 사용 하 여 특정 클라이언트에 대 한 최대 알림 크기를 얻을 수 있습니다.  최대 크기 보다 큰 모든 데이터 시스템에 의해 잘립니다.
