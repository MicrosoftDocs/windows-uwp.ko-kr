---
title: Bluetooth GATT 서버
description: 이 문서에서는 일반적인 사용 사례에 대 한 샘플 코드와 함께 GATT (Bluetooth Generic Attribute Profile) for 유니버설 Windows 플랫폼 (UWP) 앱에 대 한 개요를 제공 합니다.
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b4cf26d4f4fe5fa33f9f214da32263031188c5f0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172257"
---
# <a name="bluetooth-gatt-server"></a>Bluetooth GATT 서버

이 문서에서는 일반적인 GATT 서버 작업에 대 한 샘플 코드와 함께 UWP (GATT) 유니버설 Windows 플랫폼 용 Bluetooth Generic 특성 (UWP) 앱을 보여 줍니다.

- 지원 되는 서비스 정의
- 원격 클라이언트에서 검색할 수 있도록 서버 게시
- 서비스에 대 한 지원 보급
- 읽기 및 쓰기 요청에 응답
- 구독 한 클라이언트에 알림 보내기

> [!Important]
> *Appxmanifest.xml*에서 "bluetooth" 기능을 선언 해야 합니다.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

**중요 API**

- [**Windows.Devices.Bluetooth**](/uwp/api/Windows.Devices.Bluetooth)
- [**Windows..**](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)

## <a name="overview"></a>개요

Windows는 일반적으로 클라이언트 역할에서 작동 합니다. 그럼에도 불구 하 고 Windows가 Bluetooth LE GATT 서버 역할을 해야 하는 많은 시나리오가 발생 합니다. IoT 장치에 대 한 거의 모든 시나리오에서 대부분의 플랫폼 간 통신에는 Windows가 GATT 서버가 필요 합니다. 또한 주변 wearable 장치에 알림을 보내는 것은이 기술이 필요한 인기 있는 시나리오입니다.  

서버 작업은 서비스 공급자 및 GattLocalCharacteristic을 중심으로 합니다. 이러한 두 클래스는 데이터 계층 구조를 원격 장치에 선언 하 고 구현 하 고 노출 하는 데 필요한 기능을 제공 합니다.

## <a name="define-the-supported-services"></a>지원 되는 서비스 정의
앱은 Windows에서 게시할 서비스를 하나 이상 선언할 수 있습니다. 각 서비스는 UUID에 의해 고유 하 게 식별 됩니다.

### <a name="attributes-and-uuids"></a>특성 및 Uuid
각 서비스, 특징 및 설명자는 고유한 128 비트 UUID로 정의 됩니다.
> Windows Api는 모두 용어 GUID를 사용 하지만 Bluetooth 표준은 이러한 용어를 Uuid로 정의 합니다. 이러한 두 가지 용어는 서로 바꿔 사용할 수 있으므로 UUID 라는 용어를 계속 사용할 수 있습니다. 

특성이 표준 이며 Bluetooth SIG 정의에 의해 정의 된 경우 해당 하는 16 비트 짧은 ID (예: 배터리 수준 UUID가 0000**2A19**-0000-1000-8000-00805F9B34FB이 고 short ID는 0x2a19)입니다. 이러한 표준 Uuid는 [GattServiceUuids](/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids) 및 [GattCharacteristicUuids](/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids)에서 볼 수 있습니다.

앱에서 고유한 사용자 지정 서비스를 구현 하는 경우 사용자 지정 UUID가 생성 되어야 합니다. 이 작업은 Visual Studio에서 도구-> CreateGuid를 통해 쉽게 수행할 수 있습니다 (옵션 5를 사용 하 여 "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx-...에서 가져오기 xxxx "형식). 이제이 uuid를 사용 하 여 새 로컬 서비스, 특성 또는 설명자를 선언할 수 있습니다.

#### <a name="restricted-services"></a>제한 된 서비스
다음 서비스는 시스템에 예약 되어 있으며 지금은 게시할 수 없습니다.
1. 되지 않는 (장치 정보 서비스)
2. GATT (Generic Attribute Profile Service)
3. 일반 액세스 프로필 서비스 (간격)
4. 휴먼 인터페이스 장치 서비스 (HOGP)
5. 검색 매개 변수 서비스 (SCP)

> 차단 된 서비스를 만들려고 하면 CreateAsync 호출에서 BluetoothError이 반환 됩니다.

#### <a name="generated-attributes"></a>생성 된 특성
다음 설명자는 특성을 만들 때 제공 된 GattLocalCharacteristicParameters를 기반으로 시스템에서 자동으로 생성 됩니다.
1. 클라이언트 특징 구성 (특징이 indicatable 또는 알려야 할로 표시 된 경우)
2. 특성 사용자 설명 (UserDescription 속성이 설정 된 경우) 자세한 내용은 GattLocalCharacteristicParameters 속성을 참조 하세요.
3. 특성 형식 (지정 된 각 프레젠테이션 형식에 대 한 하나의 설명자).  자세한 내용은 PresentationFormats 속성을 GattLocalCharacteristicParameters.
4. 특성 집계 형식 (둘 이상의 프레젠테이션 형식이 지정 된 경우)  GattLocalCharacteristicParameters 추가 정보는 PresentationFormats 속성을 참조 하세요.
5. 특성 확장 속성 (특징이 확장 속성 비트로 표시 된 경우)

> 확장 속성 설명자의 값은 ReliableWrites 및 WritableAuxiliaries 특성 속성을 통해 결정 됩니다.

> 예약 된 설명자를 만들려고 하면 예외가 발생 합니다.

> 지금은 브로드캐스트가 지원 되지 않습니다.  브로드캐스트 GattCharacteristicProperty를 지정 하면 예외가 발생 합니다.

### <a name="build-up-the-hierarchy-of-services-and-characteristics"></a>서비스 및 특성 계층 구조 구축
GattServiceProvider는 루트 기본 서비스 정의를 만들고 보급 하는 데 사용 됩니다.  각 서비스에는 GUID를 사용 하는 자체 ServiceProvider 개체가 필요 합니다. 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> 기본 서비스는 GATT 트리의 최상위 수준입니다. 기본 서비스에는 ' 포함 ' 또는 보조 서비스 라고 하는 기타 서비스 뿐만 아니라 특성도 포함 되어 있습니다. 

이제 필요한 특성 및 설명자를 사용 하 여 서비스를 채웁니다.

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
위와 같이 각 특성이 지 원하는 작업에 대 한 이벤트 처리기를 선언 하는 것도 좋은 장소입니다.  요청에 올바르게 응답 하려면 앱에서 특성이 지 원하는 각 요청 형식에 대 한 이벤트 처리기를 정의 하 고 설정 해야 합니다.  처리기를 등록 하지 못하면 시스템에서 *UnlikelyError* 를 사용 하 여 즉시 요청이 완료 됩니다.

### <a name="constant-characteristics"></a>상수 특징
응용 프로그램의 수명 동안 변경 되지 않는 특성 값이 있는 경우도 있습니다. 이 경우 불필요 한 앱 활성화를 방지 하기 위해 상수 특성을 선언 하는 것이 좋습니다. 

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
서비스가 완전히 정의 되 면 다음 단계는 서비스에 대 한 지원을 게시 하는 것입니다. 원격 장치에서 서비스 검색을 수행할 때 서비스가 반환 되어야 함을 OS에 알립니다.  IsDiscoverable 가능 및 Isdiscoverable 가능: 두 속성을 설정 해야 합니다.  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **Isdiscoverable**가능: 광고의 원격 장치에 친숙 한 이름을 보급 하 여 장치를 검색할 수 있도록 합니다.
- **Isconnectable**가능: 주변 역할에 사용할 연결 가능한 보급 알림을 보급 합니다.

> 서비스를 검색 가능 하 고 연결할 수 있는 경우 시스템은 서비스 Uuid를 보급 알림 패킷에 추가 합니다.  광고 패킷에는 31 바이트만 있고, 128 비트 UUID는 16 개를 차지 합니다.

> 서비스가 포그라운드에서 게시 될 때 응용 프로그램은 응용 프로그램이 일시 중단 될 때 StopAdvertising를 호출 해야 합니다.

## <a name="respond-to-read-and-write-requests"></a>읽기 및 쓰기 요청에 응답
위에서 언급 한 대로 필요한 특성을 선언 하는 동안 GattLocalCharacteristics 3 가지 유형의 이벤트 인 ReadRequested, WriteRequested 및 SubscribedClientsChanged를 포함 합니다.

### <a name="read"></a>읽기
원격 장치가 특성에서 값을 읽으려고 할 때 (상수 값이 아닌 경우) ReadRequested 된 이벤트가 호출 됩니다. 읽기가 호출 된 특성 및 인수 (원격 장치에 대 한 정보 포함)는 대리자에 게 전달 됩니다. 

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
원격 장치에서 특성에 값을 쓰려고 하면 원격 장치에 대 한 세부 정보를 포함 하는 WriteRequested 이벤트가 호출 됩니다. 여기에는 쓸 특성 및 값 자체에 대 한 정보가 포함 됩니다. 

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
쓰기에는 두 가지 유형이 있습니다 (응답 없음). GattWriteOption (GattWriteRequest 개체의 속성)를 사용 하 여 원격 장치에서 수행 하는 쓰기 유형을 파악 합니다. 

## <a name="send-notifications-to-subscribed-clients"></a>구독 한 클라이언트에 알림 보내기
GATT 서버 작업의 가장 빈번한 알림은 원격 장치로 데이터를 푸시하는 중요 한 기능을 수행 합니다. 경우에 따라 구독 되는 모든 클라이언트에 게 알리고, 새 값을 보낼 장치를 선택 하는 것이 좋습니다. 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

새 장치에서 알림을 구독할 때 SubscribedClientsChanged 이벤트가 호출 됩니다. 

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
> 응용 프로그램은 MaxNotificationSize 속성을 사용 하 여 특정 클라이언트에 대 한 최대 알림 크기를 가져올 수 있습니다.  최대 크기 보다 큰 데이터는 시스템에 의해 잘립니다.