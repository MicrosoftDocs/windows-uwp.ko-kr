---
title: Bluetooth 광고
description: 이 섹션에는 AdvertisementWatcher 및 AdvertisementPublisher API 사용자를 통해 Bluetooth LE(저에너지) 광고를 UWP(유니버설 Windows 플랫폼) 앱에 통합하는 방법에 대한 문서가 포함되어 있습니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ff10bbc0-03a7-492c-b5fe-c5b9ce8ca32e
ms.localizationpriority: medium
ms.openlocfilehash: e9eafde0596ad3156f52a7a2f0a1566444a9836a
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7847473"
---
# <a name="bluetooth-le-advertisements"></a>Bluetooth LE 광고


**중요 API**

-   [**Windows.Devices.Bluetooth.Advertisement**](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.aspx)

이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱용 Bluetooth LE(저에너지) 광고 비콘의 개요를 제공합니다.  

## <a name="overview"></a>개요

개발자가 LE 광고 API를 사용하여 수행할 수 있는 두 가지 주요 기능은 다음과 같습니다.

-   [광고 감시자](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.aspx): 근처의 신호를 수신하고 페이로드 또는 근접성을 기준으로 필터링합니다.  
-   [광고 게시자](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher.aspx): Windows에서 개발자 대신 광고를 수행하기 위한 페이로드를 정의합니다.  

전체 샘플 코드는 [Bluetooth 광고 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619990)에서 찾을 수 있습니다.

## <a name="basic-setup"></a>기본 설정

유니버설 Windows 플랫폼 앱에서 기본 Bluetooth LE 기능을 사용하려면 Package.appxmanifest에서 Bluetooth 기능을 선택해야 합니다.

1. Package.appxmanifest 열기
2. 접근 권한 값 탭으로 이동
3. 왼쪽의 목록에서 Bluetooth를 찾아 옆에 있는 확인란을 선택합니다.

## <a name="publishing-advertisements"></a>광고 게시

Bluetooth LE 광고를 사용하면 디바이스가 광고라는 특정 페이로드 비콘을 지속적으로 보낼 수 있습니다. 이 특정 광고를 수신 대기하도록 설정된 경우 근처의 모든 Bluetooth LE 지원 디바이스에서 이 광고를 볼 수 있습니다.

> **참고**: 사용자 개인 정보에 대 한 광고 수명은 앱의 연결 됩니다. BluetoothLEAdvertisementPublisher를 만들고 광고에 대한 백그라운드 작업에서 Start를 호출할 수 있습니다. 백그라운드 작업에 대한 자세한 내용은 [실행, 다시 시작 및 백그라운드 작업](https://msdn.microsoft.com/windows/uwp/launch-resume/index)을 참조하세요.

### <a name="basic-publishing"></a>기본 게시

광고에 데이터를 추가하는 방법에는 여러 가지가 있습니다. 이 예제에서는 회사별 광고를 만드는 일반적인 방법을 보여 줍니다. 

첫 번째로, 디바이스에서 특정 광고 비콘을 보낼지 여부를 제어하는 광고 게시자를 만듭니다.

```csharp
BluetoothLEAdvertisementPublisher publisher = new BluetoothLEAdvertisementPublisher();
```

두 번째로, 사용자 지정 데이터 섹션을 만듭니다. 이 예제에서는 할당되지 않은 **CompanyId** 값 *0xFFFE*를 사용하고 광고에 텍스트 *Hello World*를 추가합니다. 

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

게시자를 만들고 설정했으므로 이제 **Start**를 호출하여 광고를 시작할 수 있습니다.

```csharp
publisher.Start();
```

## <a name="watching-for-advertisements"></a>광고 감시

### <a name="basic-watching"></a>기본 감시

다음 코드에서는 Bluetooth LE 광고 감시자를 만들고, 콜백을 설정하고, 모든 LE 광고 감시를 시작하는 방법을 보여 줍니다.

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
검사 응답 광고도 받으려면 감시자를 만든 후 다음을 설정합니다. 이 경우 전원이 많이 소모되며, 백그라운드 모드에서는 사용할 수 없습니다.

```csharp
watcher.ScanningMode = BluetoothLEScanningMode.Active;
```

### <a name="watching-for-a-specific-advertisement-pattern"></a>특정 광고 패턴 감시

특정 광고를 수신 대기하려는 경우도 있습니다. 여기서는 가상 회사(0xFFFE로 식별됨)의 페이로드와 문자열 *Hello World*가 포함된 광고를 수신 대기합니다. 이 예제를 기본 게시 예제와 연결하여 하나의 Windows 컴퓨터에서 광고하고 다른 컴퓨터에서 감시하도록 설정할 수 있습니다. 감시자를 시작하기 전에 이 광고 필터를 설정해야 합니다.

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

### <a name="watching-for-a-nearby-advertisement"></a>근처 광고 감시

디바이스 광고가 범위 내에 들어오는 경우에만 감시자를 트리거하려는 경우도 있습니다. 고유한 범위를 정의할 수 있지만 0에서 -128 사이의 범위로 값이 잘립니다. 

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

### <a name="gauging-distance"></a>거리 측정

Bluetooth LE 감시자의 콜백이 트리거되면 eventArgs에 수신된 신호 강도(Bluetooth 신호 강도)를 알려주는 RSSI 값이 포함됩니다.

```csharp
private async void OnAdvertisementReceived(BluetoothLEAdvertisementWatcher watcher, BluetoothLEAdvertisementReceivedEventArgs eventArgs)
{
    // The received signal strength indicator (RSSI)
    Int16 rssi = eventArgs.RawSignalStrengthInDBm;
}
```

이 값을 대략적으로 거리로 환산할 수는 있지만, 무선 송수신 장치마다 다르기 때문에 실제 거리를 측정하는 데 사용하면 안 됩니다. 벽, 라디오, 무선 송수신 장치 케이스, 공기 습도 등 다양한 환경 요인 때문에 거리를 측정하기 어려울 수 있습니다.

순수 거리를 파악하기 위한 대체 방법은 "버킷"을 정의하는 것입니다. 대체로 무선 송수신 장치는 매우 가까이 있을 때 0~-50DBm, 중간 거리에 있을 때 -50~-90, 멀리 떨어져 있을 때 -90 미만의 값을 보고합니다. 시행착오를 통해 앱에 적합한 버킷을 확인하는 것이 좋습니다.