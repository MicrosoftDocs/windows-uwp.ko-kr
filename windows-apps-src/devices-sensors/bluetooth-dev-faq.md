---
title: Bluetooth 개발자 FAQ
description: 이 문서에는 UWP bluetooth Api와 관련 하 여 자주 묻는 질문에 대 한 답변이 포함 되어 있습니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 7ff826be0f5b0b8e9a6723fbb1593663f1748c3d
ms.sourcegitcommit: d708ac4ec4fac0135dafc0d8c5161ef9fd945ce7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "85069471"
---
# <a name="bluetooth-developer-faq"></a>Bluetooth 개발자 FAQ

이 문서에는 자주 묻는 UWP Bluetooth API 질문에 대 한 답변이 포함 되어 있습니다.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>어떤 Api를 사용 하나요? Bluetooth 클래식 (RFCOMM) 또는 Bluetooth 저 에너지 (GATT)?
이 일반 항목에 대 한 다양 한 토론 온라인이 있으므로 Windows와 관련 된 차이점에 대해이 답변을 들. 다음은 몇 가지 일반적인 지침입니다.

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows. 장치.

Bluetooth 저 에너지를 지 원하는 장치와 통신 하는 경우 GATT Api를 사용 합니다. 사용 사례가 자주 나 낮은 대역폭 이거나 저전력이 필요한 경우 Bluetooth 저 에너지는 답입니다. 이 기능을 포함 하는 기본 네임 스페이스는 [Windows](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Bluetooth LE을 사용 하지 않는 경우**
- 높은 대역폭, 높은 빈도 시나리오. 지속적으로 많은 양의 데이터와 동기화를 유지 해야 하는 경우 Bluetooth 클래식 또는 WiFi를 사용 하는 것이 좋습니다. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth 클래식 (Rfcomm)

RFCOMM Api는 양방향 직렬 포트 스타일 통신을 수행할 수 있는 소켓을 개발자에 게 제공 합니다. 소켓이 있으면 쓰기 및 읽기에 대 한 메서드는 상당히 표준입니다. 이의 구현은 [Rfcomm Chat 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat)에 나와 있습니다. 

**Bluetooth Rfcomm를 사용 하지 않는 경우** 
- 알림을. Bluetooth GATT 프로토콜에는이에 대 한 특정 명령이 있으며,이로 인해 전원 그리기가 크게 줄어들고 응답 시간이 단축 됩니다. 
- 근접 또는 상태 검색을 확인 하는 중입니다. [보급 알림 api](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement) 를 사용 하 고 Bluetooth LE을 통해 연결 하는 것이 좋습니다. 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>연결을 끊은 후 Bluetooth LE 장치가 응답을 중지 하는 이유는 무엇 인가요?

이 문제가 발생 하는 가장 일반적인 이유는 원격 장치에서 페어링 정보를 손실 했기 때문입니다. 이전 Bluetooth 장치에는 인증이 필요 하지 않습니다. 사용자를 보호 하기 위해 설정 응용 프로그램에서 수행 하는 모든 페어링 트랜잭션은 인증을 요구 하 고 일부 장치는이를 염두에 두면 설계 되지 않았습니다. 

Windows 10 릴리스 1511부터 개발자는 페어링 핸드셰이크를 제어할 수 있습니다. [장치 열거 및 페어링 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) 은 새 장치 연결의 다양 한 측면을 자세히 설명 합니다.

이 예제에서는 암호화를 사용 하지 않고 장치와 페어링을 시작 합니다. 이는 원격 장치에서 암호화 또는 인증이 작동 하지 않아도 되는 경우에만 작동 합니다.

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

## <a name="do-i-have-to-pair-bluetooth-devices-before-using-them"></a>Bluetooth 장치를 사용 하기 전에 페어링 해야 하나요?

Bluetooth RFCOMM (클래식)를 활용 하는 경우 장치를 사용 하기 전에 장치를 페어링 하지 않아도 됩니다. Windows 10 릴리스 1607부터 주변 장치를 쿼리하고 연결 하기만 하면 됩니다. 업데이트 된 [RFCOMM Chat 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) 은이 기능을 보여 줍니다. 

**(14393 이상)** Bluetooth 저 에너지 (GATT 클라이언트)에는이 기능을 사용할 수 없기 때문에 이러한 장치에 액세스 하려면 설정 페이지를 통하거나 [Windows.](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) i n s. i n t api를 사용 해야 합니다.

**(15030 이상)** Bluetooth 장치 페어링은 더 이상 필요 하지 않습니다. 원격 장치의 현재 상태를 쿼리하려면 GetgattGetCharacteristicsAsync Async 및와 같은 새 비동기 Api를 사용 합니다. 자세한 내용은 [클라이언트 문서](gatt-client.md) 를 참조 하세요. 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>와 통신 하기 전에 장치와 쌍으로 연결 해야 하는 경우는 언제 인가요?
일반적으로 장치와 함께 신뢰할 수 있는 장기 연결을 사용 해야 하는 경우 사용자를 설정 페이지로 이동 하거나 장치 열거 및 페어링 Api를 사용 하 여 페어링 합니다. 공개적으로 노출 되는 장치 (온도 센서 또는 신호)에서 정보를 읽어야 하는 경우 장치와 연결 하지 않고도 보급 알림을 연결 하거나 수신 대기 합니다. 이렇게 하면 많은 수의 장치에서 페어링을 지원 하지 않기 때문에 긴 실행에서 상호 운용성 문제가 발생 하지 않습니다. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>모든 Windows 장치가 주변 역할을 지원 하나요?

아니요. 이는 하드웨어에 종속 된 기능이 며 지원 되는지 여부를 쿼리 하는 메서드가 제공 됩니다 (BluetoothAdapter. IsPeripheralRoleSupported).  현재 지원 되는 장치에는 8992 + and RPi3 (Windows IoT)에 Windows Phone 포함 됩니다. 

## <a name="can-i-access-these-apis-from-win32"></a>Win32에서 이러한 Api에 액세스할 수 있나요?

예, 이러한 모든 Api가 작동 합니다. 이 블로그에서는 [데스크톱 응용 프로그램에서 Windows api](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/)를 호출 하는 방법을 자세히 설명 합니다. 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>여기에이 기능이 *삽입 된 SKU*

**BLUETOOTH le**: 예, 모든 기능을 OneCore 하 고 작동 하는 bluetooth LE 스택이 있는 최신 장치에서 사용할 수 있어야 합니다. 
> 주의: 주변 역할은 하드웨어에 종속 되며 일부 Windows Server 버전은 Bluetooth를 지원 하지 않습니다. 

**BLUETOOTH BR/edr (클래식)**: 일부 변형이 있지만 대부분은 프로필 수준 지원이 매우 유사 합니다. [RFCOMM](send-or-receive-files-with-rfcomm.md) 에 대 한 문서 및 [PC](https://support.microsoft.com/help/10568/windows-10-supported-bluetooth-profiles) 및 [휴대폰](https://support.microsoft.com/help/10569/windows-10-mobile-supported-bluetooth-profiles) 에 대해 지원 되는 이러한 프로필 문서를 참조 하세요.
