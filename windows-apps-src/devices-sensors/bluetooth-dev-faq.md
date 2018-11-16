---
author: msatranjr
title: Bluetooth 개발자 FAQ
description: 이 문서에는 UWP Bluetooth API와 관련된 FAQ가 포함되어 있습니다.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 03ee8074a64b210d33498c8de135a76900d968f0
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6973803"
---
# <a name="bluetooth-developer-faq"></a>Bluetooth 개발자 FAQ

이 문서에는 UWP Bluetooth API와 관련된 FAQ가 포함되어 있습니다.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>어떤 Api 사용 합니까? Bluetooth 클래식 (RFCOMM) 또는 Bluetooth 저 에너지 (GATT)?
이제 Windows와 관련 된 차이점에 정사각형이 응답을 유지 일반이 항목에서는 다양 한 논의 온라인 가지 있습니다. 다음은 몇 가지 일반적인 지침입니다.

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

Bluetooth 저 에너지를 지 원하는 디바이스와 통신 하는 경우 GATT Api를 사용 합니다. 사용 중인 경우 드물게, 낮은 대역폭 되었거나 저전력 필요, Bluetooth 저 에너지 대답이 합니다. 이 기능을 포함 하는 기본 네임 스페이스 [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)입니다. 

**Bluetooth LE 사용 하지 않는 경우**
- 높은 대역폭을 고주파 시나리오 지속적으로 많은 양의 데이터를 사용 하 여 동기화 상태로 유지 해야 할 경우 클래식 또는 직장 동료 일 WiFi Bluetooth를 사용 하는 것이 좋습니다. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth 클래식 (Windows.Devices.Bluetooth.Rfcomm)

RFCOMM Api 개발자 양방향 직렬 포트 스타일의 통신을 수행 하는 소켓을 제공 합니다. 소켓 했으면 여기에서 읽기 및 쓰기를 메서드는 매우 표준. 이 구현의 [Rfcomm 채팅 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat)에 표시 됩니다. 

**Bluetooth Rfcomm을 사용 하지 않는 경우** 
- 알림입니다. Bluetooth GATT 프로토콜이 특정 명령에 있고 훨씬 더 적은 전원 그리기 및 응답 시간을 단축 됩니다. 
- 근접 연결 또는 상태 감지를 확인 합니다. [광고 Api](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement) 를 사용 하 여 Bluetooth LE를 통해 연결 합니다. 


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

**(14393 및 아래)** 이 기능은 없으므로 여전히 쌍 설정 페이지 또는 [Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) Api 사용 하 여 액세스 순서를 통해 이러한 장치 없습니다 Bluetooth 저 에너지 (GATT 클라이언트)에 사용할 수 있습니다.

**(15030 이상)** Bluetooth 장치를 페어링 더 이상 필요 합니다. 새로운 비동기 Api를 사용 하 여 원격 디바이스의 현재 상태를 쿼리 하기 위해 GetGattServicesAsync 및 GetCharacteristicsAsync 같은입니다. 자세한 내용은 [클라이언트 문서](gatt-client.md) 를 참조 하세요. 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>와 통신 하기 전에 디바이스에 페어링할 경우 해야 해야 하나요?
일반적으로 장치를 사용 하 여 신뢰할 수 있는, 장기 채권을 필요로 하는 경우 (설정 페이지를 사용자 지정 또는 장치 열거 및 페어링 Api를 사용 하 여)와 쌍입니다. (온도 센서 또는 신호) 장치는 공개적으로 노출 정보를 읽었는지 하기만 하는 경우, 연결 하거나 장치를 페어링할 교통과 하지 않고 광고에 대 한 수신 대기 합니다. 이렇게 하면 상호 운용성 문제 장기적으로 장치의 호스트 페어링을 지원 하지 않기 때문입니다. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>모든 Windows 장치 주변 장치를 지원 합니까?

더 – 하드웨어 종속 기능 이지만 메서드를 제공 됩니다 (BluetoothAdapter.IsPeripheralRoleSupported) 쿼리 여부 지원 여부 합니다.  현재 지원 되는 장치는 Windows Phone 8992 + 및 RPi3 (Windows IoT). 

## <a name="can-i-access-these-apis-from-win32"></a>Win32에서 이러한 Api에 액세스할 수 있나요?

예, 이러한 Api는 모든 작동 해야 합니다. 이 블로그 [데스크톱 응용 프로그램의 Windows Api](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/)를 호출 하는 방법을 자세히 설명 합니다. 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>이 기능은 *-삽입 SKU 여기-* 에 존재 하도록 되어 있나요?

**Bluetooth LE**: 예, 모든 OneCore 중인 기능과 작동 Bluetooth LE 스택을 지 원하는 최신 장치에서 사용할 수 있어야 합니다. 
> 주의 사항: 주변 장치는 하드웨어에 종속 및 일부 Windows Server 버전 Bluetooth를 지원 하지 않습니다. 

**Bluetooth BR/EDR (기본)**: 일부 변형 존재 하지만 매우 유사 프로필 수준 지원 전반적으로 갖습니다. [PC](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles) 와 [휴대폰](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles) 에 [RFCOMM](send-or-receive-files-with-rfcomm.md) 및 이러한 지원 되는 프로필 문서에 문서 보기

