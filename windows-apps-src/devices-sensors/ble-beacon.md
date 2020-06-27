---
title: Bluetooth 보급 알림
description: 이 섹션에는 AdvertisementWatcher 및 AdvertisementPublisher Api의 사용자를 통해 Bluetooth 저 에너지 (LE) 보급 알림을 유니버설 Windows 플랫폼 (UWP) 앱에 통합 하는 방법에 대 한 문서가 포함 되어 있습니다.
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ff10bbc0-03a7-492c-b5fe-c5b9ce8ca32e
ms.localizationpriority: medium
ms.openlocfilehash: 2300871292e08588b0c2124c67a379d403ae53b3
ms.sourcegitcommit: 015291bdf2e7d67076c1c85fc025f49c840ba475
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469528"
---
# <a name="bluetooth-le-advertisements"></a>Bluetooth LE 광고


**중요 API**

-   [**Windows.Devices.Bluetooth.Advertisement**](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement)

이 문서에서는 유니버설 Windows 플랫폼 (UWP) 앱에 대 한 Bluetooth 저 에너지 (LE) 보급 알림 오류에 대 한 개요를 제공 합니다.  

> [!Important]
> *Appxmanifest.xml*에서 "bluetooth" 기능을 선언 해야 합니다.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

## <a name="overview"></a>개요

개발자가 LE 보급 Api를 사용 하 여 수행할 수 있는 두 가지 주요 함수는 다음과 같습니다.

-   [알림 감시자](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher): 근처의 탐지 장치를 수신 대기 하 고 페이로드 또는 근접성에 따라 필터링 합니다.  
-   [보급 알림 게시자](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher): 개발자에 게 보급을 대신 하 여 Windows에 대 한 페이로드를 정의 합니다.  

Github의 [Bluetooth 보급 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BluetoothAdvertisement) 에서 전체 샘플 코드를 찾을 수 있습니다.

## <a name="basic-setup"></a>기본 설정

유니버설 Windows 플랫폼 앱에서 기본 Bluetooth LE 기능을 사용 하려면 appxmanifest.xml에서 Bluetooth 기능을 확인 해야 합니다.

1. Appxmanifest.xml를 엽니다.
2. 기능 탭으로 이동
3. 왼쪽의 목록에서 Bluetooth를 찾아 옆의 상자를 선택 합니다.

## <a name="publishing-advertisements"></a>게시 알림

Bluetooth LE 보급 알림을 통해 장치는 보급 알림 이라는 특정 페이로드를 지속적으로 오류를 발생 시킬 수 있습니다. 이 보급 알림은 이러한 특정 advertisment을 수신 하도록 설정 된 경우 주변의 Bluetooth LE 지원 장치에서 볼 수 있습니다.

> **참고**: 사용자 개인 정보 보호를 위해 보급 알림의 수명은 앱의 수명에 연결 됩니다. BluetoothLEAdvertisementPublisher을 만들고 백그라운드에서 광고에 대해 백그라운드 작업에서 Start를 호출할 수 있습니다. 백그라운드 작업에 대 한 자세한 내용은 [시작, 다시 시작 및 백그라운드 작업](https://docs.microsoft.com/windows/uwp/launch-resume/index)을 참조 하세요.

### <a name="basic-publishing"></a>기본 게시

광고에 데이터를 추가 하는 방법에는 여러 가지가 있습니다. 이 예제에서는 회사 관련 광고를 만드는 일반적인 방법을 보여 줍니다. 

먼저 장치가 특정 광고를 제공할지 여부를 제어 하는 보급 알림 게시자를 만듭니다.

```csharp
BluetoothLEAdvertisementPublisher publisher = new BluetoothLEAdvertisementPublisher();
```

둘째, 사용자 지정 데이터 섹션을 만듭니다. 이 예제에서는 할당 되지 않은 **CompanyId** 값 *0xFFFE* 를 사용 하 여 광고에 *Hello World* 텍스트를 추가 합니다. 

```csharp
// Add custom data to the advertisement
var manufacturerData = new BluetoothLEManufacturerData();
manufacturerData.CompanyId = 0xFFFE;

var writer = new DataWriter();
writer.WriteString("Hello World");

// Make sure that the buffer length can fit within an advertisement payload (~20 bytes). 
// Otherwise you will get an exception.
manufacturerData.Data = writer.DetachBuffer();

// Add the manufacturer data to the advertisement publisher:
publisher.Advertisement.ManufacturerData.Add(manufacturerData);
```

이제 게시자를 만들고 설정 했으므로 **시작** 을 호출 하 여 광고를 시작할 수 있습니다.

```csharp
publisher.Start();
```

## <a name="watching-for-advertisements"></a>광고 감시

### <a name="basic-watching"></a>기본 감시

다음 코드에서는 Bluetooth LE 광고 감시자를 만들고, 콜백을 설정 하 고, 모든 LE 보급을 감시 하기 시작 하는 방법을 보여 줍니다.

```csharp
BluetoothLEAdvertisementWatcher watcher = new BluetoothLEAdvertisementWatcher();
watcher.Received += OnAdvertisementReceived;
watcher.Start();
``` 

```csharp
private async void OnAdvertisementReceived(BluetoothLEAdvertisementWatcher watcher, BluetoothLEAdvertisementReceivedEventArgs eventArgs)
{
    // Do whatever you want with the advertisement
}
```

#### <a name="active-scanning"></a>활성 검사
검색 응답 보급 알림도 받으려면 감시자를 만든 후 다음을 설정 합니다. 이렇게 하면 더 큰 전력이 소모 되며 백그라운드 모드에서는 사용할 수 없습니다.

```csharp
watcher.ScanningMode = BluetoothLEScanningMode.Active;
```

### <a name="watching-for-a-specific-advertisement-pattern"></a>특정 광고 패턴 감시

특정 광고를 수신 하려는 경우가 있습니다. 이 경우는 구성 된 회사 (0xFFFE로 식별 됨)가 있는 페이로드를 포함 하 고 광고에 *Hello World* 문자열을 포함 하는 광고를 수신 합니다. 이를 기본 게시 예제와 함께 사용 하 여 한 Windows 컴퓨터에서 광고 하 고 다른 감시를 수행할 수 있습니다. 감시자를 시작 하기 전에이 광고 필터를 설정 해야 합니다.

```csharp
var manufacturerData = new BluetoothLEManufacturerData();
manufacturerData.CompanyId = 0xFFFE;

// Make sure that the buffer length can fit within an advertisement payload (~20 bytes). 
// Otherwise you will get an exception.
var writer = new DataWriter();
writer.WriteString("Hello World");
manufacturerData.Data = writer.DetachBuffer();

watcher.AdvertisementFilter.Advertisement.ManufacturerData.Add(manufacturerData);
```

### <a name="watching-for-a-nearby-advertisement"></a>가까운 광고 감시

경우에 따라 장치 광고가 범위 내에 있는 경우에만 감시자를 트리거해야 합니다. 사용자 고유의 범위를 정의할 수 있습니다. 값은 0에서-128 사이로 잘립니다. 

```csharp
// Set the in-range threshold to -70dBm. This means advertisements with RSSI >= -70dBm 
// will start to be considered "in-range" (callbacks will start in this range).
watcher.SignalStrengthFilter.InRangeThresholdInDBm = -70;

// Set the out-of-range threshold to -75dBm (give some buffer). Used in conjunction 
// with OutOfRangeTimeout to determine when an advertisement is no longer 
// considered "in-range".
watcher.SignalStrengthFilter.OutOfRangeThresholdInDBm = -75;

// Set the out-of-range timeout to be 2 seconds. Used in conjunction with 
// OutOfRangeThresholdInDBm to determine when an advertisement is no longer 
// considered "in-range"
watcher.SignalStrengthFilter.OutOfRangeTimeout = TimeSpan.FromMilliseconds(2000);
```

### <a name="gauging-distance"></a>성취도 거리

Bluetooth LE 감시자의 콜백이 트리거되면 eventArgs에는 수신 된 신호 강도 (Bluetooth 신호의 강도)를 알려 주는 RSSI 값이 포함 됩니다.

```csharp
private async void OnAdvertisementReceived(BluetoothLEAdvertisementWatcher watcher, BluetoothLEAdvertisementReceivedEventArgs eventArgs)
{
    // The received signal strength indicator (RSSI)
    Int16 rssi = eventArgs.RawSignalStrengthInDBm;
}
```

이는 대략 거리로 변환 될 수 있지만 각 개별 라디오가 서로 다르기 때문에 실제 거리를 측정 하는 데 사용 하면 안 됩니다. 환경 요인에 따라 거리를 측정할 수 있습니다 (예: 벽, 케이스 주위의 케이스 또는 공기 습도).

심사 순수한 distance의 대안은 "버킷"을 정의 하는 것입니다. 라디오는 매우 근접 한 경우에는 0 ~-50 DBm을 보고 하는 경향이 있습니다. 즉, 보통 거리가 되 면-50에서-90이 고 멀리 떨어져 있는 경우에는-90입니다. 평가판 및 오류는 앱에 대 한 이러한 버킷을 원하는 대로 결정 하는 데 가장 적합 합니다.