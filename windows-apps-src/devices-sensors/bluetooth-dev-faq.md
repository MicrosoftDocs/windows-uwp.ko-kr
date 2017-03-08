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
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 7394d211b580ad82689a79e7cbe4eb4dbf545f46
ms.lasthandoff: 02/08/2017

---
# <a name="bluetooth-developer-faq"></a>Bluetooth 개발자 FAQ

이 문서에는 UWP Bluetooth API와 관련된 FAQ가 포함되어 있습니다.

## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>연결을 끊은 후 Bluetooth LE 장치가 응답하지 않는 이유는 무엇 때문인가요?

이 문제가 발생하는 일반적인 이유는 원격 장치에 페어링 정보가 없기 때문입니다. 이전의 많은 Bluetooth 장치에서는 인증이 필요하지 않습니다. 사용자를 보호하기 위해 설정 앱에서 수행되는 모든 페어링에 인증이 필요하며 일부 장치는 이 작업을 처리할 수 없습니다. 

Windows 10 릴리스 1511부터 개발자가 페어링을 제어할 수 있습니다. [ 열거 및 페어링 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing)에서 새 장치 연결의 다양한 측면을 자세히 설명합니다.

이 예제에서는 암호화를 사용하지 않고 장치와 페어링을 시작합니다. 이 방법은 원격 장치를 작동하는 데 암호화 또는 인증이 필요하지 않은 경우에만 사용할 수 있습니다.

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

## <a name="do-i-have-to-pair-bluetooth-devices-before-using-them"></a>사용하기 전에 Bluetooth 장치를 페어링해야 하나요?

Bluetooth RFCOMM(클래식) 장치의 경우에는 페어링할 필요가 없습니다. Windows 10 릴리스 1607부터 주변 장치를 쿼리하고 연결하기만 하면 됩니다. 업데이트된 [RFCOMM 채팅 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat)에서 이 기능을 보여 줍니다. 

Bluetooth 저에너지(GATT 클라이언트)에는 이 기능을 사용할 수 없으므로 이러한 장치에 액세스하려면 설정 페이지를 통해 또는 [Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) API를 사용하여 페어링해야 합니다.


