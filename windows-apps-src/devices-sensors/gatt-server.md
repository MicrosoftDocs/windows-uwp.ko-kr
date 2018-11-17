---
author: msatranjr
title: Bluetooth GATT 서버
description: 이 문서에서는 유니버설 Windows 플랫폼 (UWP) 앱의 일반적인 사용 사례에 대 한 샘플 코드와 함께 Bluetooth 일반 특성 프로필 (GATT) 서버에 대 한 개요를 제공합니다.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b8a941b7b80bd5d34e88798ec586d9c1d52e2887
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7163094"
---
# <a name="bluetooth-gatt-server"></a>Bluetooth GATT 서버


**중요 API**
- [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)


이 문서에서는 유니버설 Windows 플랫폼 (UWP) 앱의 경우 일반적인 GATT 서버 작업에 대 한 샘플 코드와 함께 Bluetooth 일반 특성 (GATT) 서버 Api를 보여 줍니다. 
- 지원 되는 서비스를 정의 합니다.
- 원격 클라이언트에서 검색할 수 있도록 서버를 게시 합니다.
- 지원 서비스에 대 한 광으십시오
- 읽기 및 쓰기 요청에 응답
- 구독 한 클라이언트 알림 보내기

## <a name="overview"></a>개요
일반적으로 Windows 클라이언트 역할을 수행 합니다. 그럼에도 불구 하 고 다양 한 시나리오 에서도 Bluetooth LE GATT 서버 역할을 하는 Windows 필요는 발생 합니다. IoT 디바이스, 대부분 플랫폼 간 BLE 통신에 대 한 거의 모든 시나리오는 Windows를 GATT 서버 필요 합니다. 또한 근처 착용 식 장치에 알림 보내기도이 기술이 필요로 하는 일반적인 시나리오 해 왔습니다.  
> [GATT 클라이언트 문서](gatt-client.md) 에 설명 된 모든 개념을 계속 하기 전에 명확 하 고 있는지 확인 합니다.  

서버 작업은 서비스 공급자와의 GattLocalCharacteristic를 중심으로 합니다. 이러한 두 클래스에서 선언 하 고, 구현, 원격 장치에 대 한 데이터의 계층 구조를 노출 하는 데 필요한 기능을 제공 합니다.

## <a name="define-the-supported-services"></a>지원 되는 서비스를 정의 합니다.
앱에는 Windows에서 게시할 수 있는 하나 이상의 서비스 선언할 수 있습니다. 각 서비스 UUID 고유 하 게 식별 됩니다. 

### <a name="attributes-and-uuids"></a>특성 및 Uuid
각 서비스, 특징 및 설명자 정의 된 고유의 자체 128 비트 UUID 것으로 합니다.
> GUID 라는 용어를 사용 하는 모든 Windows Api 하지만 이러한 Uuid Bluetooth 표준 정의 합니다. 우리 목적에 이러한 두 용어는 교체할 하므로 UUID 용어를 사용 하 여 계속 진행 됩니다. 

특성에 표준 및 Bluetooth SIG 정의에 정의 된 경우, 해당 16 비트 짧은 ID가 포함 될 (예: 배터리 수준 UUID는**2A19**0000-0000-1000-8000-00805F9B34FB 및 짧은 ID는 0x2A19). 이러한 표준 Uuid [GattServiceUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids.aspx) 및 [GattCharacteristicUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids.aspx)에서 볼 수 있습니다.

앱을 구현 하는 것이 고유한 사용자 지정 서비스, 사용자 지정 UUID를 생성 해야 합니다. 이 쉽게 수행 도구를 통해 Visual Studio에서-> CreateGuid (사용 옵션 5 "xxxxxxxx-xxxx-... xxxx" 형식으로 가져올 수)입니다. 이제 새 로컬 서비스, 특징 또는 설명자를 선언 하이 uuid는 사용할 수 있습니다.

#### <a name="restricted-services"></a>제한 된 서비스
다음 서비스가 시스템에 의해 예약 하 고이 시간에 게시할 수 없습니다.
1. 장치 정보 서비스 (DIS)
2. 일반 특성 프로필 (GATT) 서비스
3. 일반 액세스 프로필 서비스 (간격)
4. 휴먼 인터페이스 장치 서비스 (HOGP)
5. 매개 변수 (SCP) 서비스를 검색 합니다.

> 차단 된 서비스를 만들려면 CreateAsync 호출에서 반환 되는 BluetoothError.DisabledByPolicy 발생 합니다.

#### <a name="generated-attributes"></a>생성 된 특성
다음 설명자의 특성을 만드는 동안 제공 GattLocalCharacteristicParameters에 따라 시스템에 의해 자동으로 생성 같습니다.
1. 클라이언트 특성 구성 (경우 특성으로 indicatable 또는 notifiable로 표시).
2. 특성 사용자 설명 (UserDescription 속성이 설정 된 경우). 자세한 내용은 GattLocalCharacteristicParameters.UserDescription 속성을 참조 하세요.
3. 특성 형식 (지정 된 각 프레젠테이션 형식에 대 한 하나의 설명자)입니다.  자세한 내용은 GattLocalCharacteristicParameters.PresentationFormats 속성을 참조 하세요.
4. 특성 집계 형식 (경우 둘 이상의 프레젠테이션 형식 지정 됨).  자세한 내용은 GattLocalCharacteristicParameters.See PresentationFormats 속성입니다.
5. 특성 확장 속성 (특성은 확장된 속성 비트 기본적으로) 하는 경우.

> 확장 속성 설명자의 값은 ReliableWrites 및 WritableAuxiliaries 특성 속성을 통해 결정 됩니다.

> 예약 된 설명자를 만들려고 하면 예외가 발생 합니다.

> 브로드캐스트는 현재 지원 되지 않습니다.  브로드캐스트 GattCharacteristicProperty 지정 하면 예외가 발생 합니다.

### <a name="build-up-the-heirarchy-of-services-and-characteristics"></a>서비스 및 특성 계층 구조 구축
만들고 광고 루트 기본 서비스 정의 하는 GattServiceProvider 사용 됩니다.  각 서비스에 직접 ServiceProvider 개체 GUID를 이용 하는 것이 필요 합니다. 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> 기본 서비스는 GATT 트리의 최상위 수준입니다. 기본 서비스 ('포함' 또는 보조 서비스 라고 함)는 다른 서비스와 특성이 포함 되어 있습니다. 

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
위와 같이 각 특성에 원하는 작업에 대 한 이벤트 처리기를 선언 하기 좋은 위치 이기도 합니다.  앱 해야 올바르게 요청에 응답 하도록 정의 하 고 특성을 지 원하는 각 요청 형식에 대 한 이벤트 처리기를 설정 합니다.  처리기를 등록 하지 않으면 시스템에서 *UnlikelyError* 로 즉시 완료 되 고 요청 될 수 있습니다.

### <a name="constant-characteristics"></a>상수 특성
경우에 따라 앱의 수명 동안 변경 되지 않는 특성 값을 가지 있습니다. 이 경우 불필요 한 앱 활성화를 방지 하기 위해 상수 특성을 선언 하는 것이 좋습니다. 

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
## <a name="publish-the-service"></a>서비스를 게시 합니다.
서비스가 완벽 하 게 정의 되 면 다음 단계는 서비스에 대 한 지원을 게시입니다. 서비스 검색을 수행 하는 원격 장치 서비스 반환 되어야 함을 OS 알립니다.  IsDiscoverable 및 IsConnectable 두 속성을 설정 해야 합니다.  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**: 장치를 검색 하는 광고의 원격 장치에 이름을 알립니다.
- **IsConnectable**: 주변 장치에서 사용 가능한 광고를 알립니다.

> 서비스 검색 가능와 Connectable 되 면 시스템을 사용 하 여 서비스 Uuid 광고 패킷에 추가 됩니다.  광고 패킷에 31 바이트만 및 128 비트 UUID의 16 차지 있습니다!

> 참고 하는 서비스는 포그라운드에 게시 될 때 응용 프로그램 호출 해야 StopAdvertising 응용 프로그램을 일시 중단할 때 합니다.

## <a name="respond-to-read-and-write-requests"></a>읽기 및 쓰기 요청에 응답
필수 특성을 선언 하는 동안 위에서 살펴본 대로 GattLocalCharacteristics 3 가지 유형의 이벤트-ReadRequested, WriteRequested 및 SubscribedClientsChanged 했습니다.

### <a name="read"></a>Read
원격 장치에서 특성 값을 읽지 하려고 (및 상수 값이 아닌) ReadRequested 이벤트 호출 됩니다. 읽기 args (원격 장치에 대 한 정보를 포함) 뿐만 아니라에서 호출 된 특성은 대리자에 전달 됩니다. 

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
원격 장치에는 특성 값을 기록 하려고 하면 WriteRequested 이벤트에 쓸 수 있는 특성 및 값 자체가 원격 장치에 대 한 정보를 사용 하 여 호출 됩니다. 

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
응답 없이 쓰기-2 유형이 있습니다. GattWriteOption (GattWriteRequest 개체에서 속성)를 사용 하 여 원격 장치에서 수행 되 어떤 유형의 쓰기 파악 합니다. 

## <a name="send-notifications-to-subscribed-clients"></a>구독 한 클라이언트 알림 보내기
알림 GATT 서버 작업을 자주 원격 장치에 데이터를 푸시할 중요 한 기능을 수행 합니다. 경우에 따라 모든 가입된 클라이언트 하지만 새 값을 보낼 수 있는 장치를 선택 하는 것이 좋습니다 othertimes 알림 하려는: 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

알림에 대 한 새 장치 등록, SubscribedClientsChanged 이벤트 호출 가져옵니다. 

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
> 참고 응용 프로그램 MaxNotificationSize 속성을 사용 하 여 특정 클라이언트에 대 한 최대 알림 크기를 가져올 수 있습니다.  최대 크기 보다 큰 모든 데이터는 시스템에서 잘립니다.
