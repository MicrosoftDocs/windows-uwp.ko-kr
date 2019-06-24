---
title: Bluetooth 개발자 FAQ
description: 이 문서에는 UWP Bluetooth API와 관련된 FAQ가 포함되어 있습니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: f61f2a0889cd5a2b2b95063e6009530951b49cbd
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321599"
---
# <a name="bluetooth-developer-faq"></a>Bluetooth 개발자 FAQ

이 문서에는 UWP Bluetooth API와 관련된 FAQ가 포함되어 있습니다.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>어떤 API를 사용하나요? Bluetooth Classic(RFCOMM) 또는 Bluetooth 저에너지(GATT)가 있나요?
이 일반적인 항목에 대한 여러 온라인 논의가 있으므로 이에 대한 답변은 Windows와 관련한 차이점에 대해 정확하게 유지하도록 합니다. 몇 가지 일반적인 지침은 다음과 같습니다.

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE(Windows.Devices.Bluetooth.GenericAttributeProfile)

Bluetooth LE(저에너지)를 지원하는 디바이스와 통신하는 경우 GATT API를 사용합니다. 자주 사용하지 않는 경우, 낮은 대역폭 또는 저전력인 경우, Bluetooth LE(저에너지)가 해답입니다. 이 기능을 포함하는 주요 네임스페이스는 [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)입니다. 

**Bluetooth LE를 사용 하지 않는 경우**
- 높은 대역폭, 높은 주파수 시나리오. 많은 양의 데이터와 지속적으로 동기화하려는 경우 Bluetooth Classic 또는 Wi-Fi를 사용하는 것이 좋습니다. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth Classic(Windows.Devices.Bluetooth.Rfcomm)

RFCOMM API는 개발자에게 양방향 직렬 포트 스타일 통신을 수행하기 위한 소켓을 제공합니다. 소켓을 가져왔으면 이를 쓰고 읽는 메서드는 표준입니다. 이러한 구현은 [Rfcomm 채팅 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat)에서 보여 줍니다. 

**Bluetooth Rfcomm 사용 하지 않는 경우** 
- 알림. Bluetooth GATT 프로토콜은 이에 대한 특정 명령을 가지며 훨씬 더 적은 전력 및 빠른 응답 시간을 제공합니다. 
- 근접 또는 감지를 확인합니다. [광고 API](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement)를 사용하여 Bluetooth LE를 통해 연결하는 것이 좋습니다. 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>연결을 끊은 후 Bluetooth LE 디바이스가 응답하지 않는 이유는 무엇 때문인가요?

이 문제가 발생하는 일반적인 이유는 원격 디바이스에 페어링 정보가 없기 때문입니다. 이전의 많은 Bluetooth 디바이스에서는 인증이 필요하지 않습니다. 사용자를 보호하기 위해 설정 앱에서 수행되는 모든 페어링에 인증이 필요하며 일부 디바이스는 이 작업을 처리할 수 없습니다. 

Windows 10 릴리스 1511부터 개발자가 페어링을 제어할 수 있습니다. [ 열거 및 페어링 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing)에서 새 디바이스 연결의 다양한 측면을 자세히 설명합니다.

이 예제에서는 암호화를 사용하지 않고 디바이스와 페어링을 시작합니다. 이 방법은 원격 디바이스를 작동하는 데 암호화 또는 인증이 필요하지 않은 경우에만 사용할 수 있습니다.

```csharp
// Get ceremony type and protection level selections
// You must select at least ConfirmOnly or the pairing attempt will fail
    DevicePairingKinds ceremonySelected = DevicePairingKinds.ConfirmOnly;

//  Workaround remote devices losing pairing information
    DevicePairingProtectionLevel protectionLevel = DevicePairingProtectionLevel.None

    DeviceInformationCustomPairing customPairing = deviceInfoDisp.DeviceInformation.Pairing.Custom;

// Declare an event handler - you don't need to do much in PairingRequestedHandler since the ceremony is "None"
    customPairing.PairingRequested += PairingRequestedHandler;
    DevicePairingResult result = await customPairing.PairAsync(ceremonySelected, protectionLevel);
```

## <a name="do-i-have-to-pair-bluetooth-devices-before-using-them"></a>사용하기 전에 Bluetooth 디바이스를 페어링해야 하나요?

Bluetooth RFCOMM(클래식) 디바이스의 경우에는 페어링할 필요가 없습니다. Windows 10 릴리스 1607부터 주변 디바이스를 쿼리하고 연결하기만 하면 됩니다. 업데이트된 [RFCOMM 채팅 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat)에서 이 기능을 보여 줍니다. 

**(14393 및 이하)** Bluetooth 저에너지(GATT 클라이언트)에는 이 기능을 사용할 수 없으므로 이러한 디바이스에 액세스하려면 설정 페이지를 통해 또는 [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) API를 사용하여 페어링해야 합니다.

**(15030 및 이상)** Bluetooth 디바이스 페어링이 더 이상 필요 없습니다. GetGattServicesAsync 및 GetCharacteristicsAsync 같은 새로운 Async API를 사용하여 원격 디바이스의 현재 상태를 쿼리할 수 있습니다. 자세한 내용은 [클라이언트 문서](gatt-client.md)를 참조하세요. 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>통신하기 전에 언제 장치와 페어링을 해야 하나요?
일반적으로 디바이스에 대한 장기적인 신뢰가 필요한 경우 이를 페어링합니다(설정 페이지에서 사용자에게 지시하거나 디바이스 열거 및 페어링 API 사용). (온도 센서 또는 신호) 장치를 공개적으로 노출 하는 경우 단순히 정보를 읽어오려면, 연결 또는 장치를 쌍으로 연결 하려고 시도 하지 않고 보급 알림에 대 한 수신 대기 합니다. 이렇게 하면 디바이스의 호스트가 페어링을 지원하지 않으므로 장기적으로 상호 운용성 문제를 방지할 수 있습니다. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>모든 Windows 장치가 주변 역할을 지원하나요?

아니요. 이 기능은 하드웨어에 의존하지만 이 기능이 지원되는지 여부를 쿼리할 수 있는 메서드(BluetoothAdapter.IsPeripheralRoleSupported)가 제공됩니다.  현재 지원되는 디바이스에는 Windows Phone 8992+ 및 RPi3(Windows IoT)가 있습니다. 

## <a name="can-i-access-these-apis-from-win32"></a>Win32에서 이러한 API에 액세스할 수 있나요?

예, 이러한 모든 API가 작동합니다. 이 블로그는 [데스크톱 응용 프로그램의 Windows API](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/)를 호출하는 방법에 대해 자세히 설명합니다. 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>이 기능이 *-여기에 SKU 삽입-* 에 남아 있나요?

**Bluetooth LE**: 예, 모든 기능이 OneCore 이며 작동 Bluetooth LE 스택 통한 가장 최근의 장치에서 사용할 수 있어야 합니다. 
> 주의 사항: 주변 역할 하드웨어 종속 되며 몇 가지 Windows Server 버전에서 Bluetooth를 지원 하지 않습니다. 

**Bluetooth b R/EDR (클래식)** : 몇 가지 변형이 존재 하지만 전반적으로 매우 유사 프로필 수준 지원. [RFCOMM](send-or-receive-files-with-rfcomm.md)의 문서와 [PC](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles) 및 [휴대폰](https://support.microsoft.com/products/windows?os=windows-10-mobile)에서 지원되는 이러한 지원되는 프로필을 참조하세요.

