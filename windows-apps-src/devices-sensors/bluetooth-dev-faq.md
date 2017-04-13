---
author: msatranjr
title: "Bluetooth 개발자 FAQ"
description: "이 문서에는 UWP Bluetooth API와 관련된 FAQ가 포함되어 있습니다."
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.openlocfilehash: 3dabc5ad2833eecfec1f397bdd5bf7f2b807a48d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="bluetooth-developer-faq"></a>Bluetooth 개발자 FAQ

이 문서에는 UWP Bluetooth API와 관련된 FAQ가 포함되어 있습니다.

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

Bluetooth 저에너지(GATT 클라이언트)에는 이 기능을 사용할 수 없으므로 이러한 디바이스에 액세스하려면 설정 페이지를 통해 또는 [Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) API를 사용하여 페어링해야 합니다.

