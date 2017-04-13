---
author: IvorB
ms.assetid: E9ADC88F-BD4F-4721-8893-0E19EA94C8BA
title: "대역 외 페어링"
description: "대역 외 페어링을 사용하면 앱이 검색 없이 POS(Point-of-Service) 주변 장치에 연결할 수 있습니다."
ms.author: ivorb
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: ef9d4c390112be66035ab2ace6b6b799ee9d99ef
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="out-of-band-pairing"></a>대역 외 페어링

대역 외 페어링을 사용하면 앱이 검색 없이 POS(Point-of-Service) 주변 디바이스에 연결할 수 있습니다. 앱은 [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/windows.devices.pointofservice.aspx) 네임스페이스를 사용하고 구체적으로 서식이 지정된 문자열(대역 외 Blob)을 원하는 주변 디바이스에 적합한 **FromIdAsync** 메서드에 전달해야 합니다. **FromIdAsync**를 실행하면 작업이 호출자에게 반환되기 전에 호스트 디바이스가 페어링되고 주변 디바이스에 연결합니다.

## <a name="out-of-band-blob-format"></a>대역 외 Blob 형식

```json
    "connectionKind":"Network",
    "physicalAddress":"AA:BB:CC:DD:EE:FF",
    "connectionString":"192.168.1.1:9001",
    "peripheralKinds":"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}",
    "providerId":"{02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2}",
    "providerName":"PrinterProtocolProvider.dll"
```

**connectionKind** - 연결 유형입니다. 유효한 값은 "네트워크" 및 "Bluetooth"입니다.

**physicalAddress** - 주변 디바이스의 MAC 주소입니다. 예를 들어 네트워크 프린터의 경우 프린터 테스트 시트에서 제공하는 AA:BB:CC:DD:EE:FF 형식의 MAC 주소입니다.

**connectionString** - 주변 디바이스의 연결 문자열입니다. 예를 들어 네트워크 프린터의 경우 프린터 테스트 시트에서 제공하는 192.168.1.1:9001 형식의 IP 주소입니다. 모든 Bluetooth 주변 디바이스에 대해서는 이 필드가 생략됩니다.

**peripheralKinds** - 디바이스 유형의 GUID입니다. 유효한 값은 다음과 같습니다.

| 장치 유형 | GUID |
| ---- | ---- |
| *POS 프린터* | C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33 |
| *바코드 스캐너* | C243FFBD-3AFC-45E9-B3D3-2BA18BC7EBC5 |
| *현금 출납기* | 772E18F2-8925-4229-A5AC-6453CB482FDA |


**providerId** - 프로토콜 공급자 클래스의 GUID입니다. 유효한 값은 다음과 같습니다.

| 프로토콜 공급자 클래스 | GUID |
| ---- | ---- |
| *일반 ESC/POS 네트워크 프린터* | 02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2 |
| *일반 ESC/POS BT 프린터* | CCD5B810-95B9-4320-BA7E-78C223CAF418 |
| *Epson BT 프린터* | 94917594-544F-4AF8-B53B-EC6D9F8A4464 |
| *Epson 네트워크 프린터* | 9F0F8BE3-4E59-4520-BFBA-AF77614A31CE |
| *Star 네트워크 프린터* | 1E3A32C2-F411-4B8C-AC91-CC2C5FD21996 |
| *Socket BT 스캐너* | 6E7C8178-A006-405E-85C3-084244885AD2 |
| *APG 네트워크 용지함(서랍형)* | E619E2FE-9489-4C74-BF57-70AED670B9B0 |
| *APG BT 용지함(서랍형)* | 332E6550-2E01-42EB-9401-C6A112D80185 |


**공급자 이름** - 공급자 DLL의 이름입니다. 기본 공급자는 다음과 같습니다.

| 공급자 | DLL 이름 |
| ---- | ---- |
| 프린터 | PrinterProtocolProvider.dll |
| 현금 출납기 | CashDrawerProtocolProvider.dll |
| 스캐너 | BarcodeScannerProtocolProvider.dll |

## <a name="usage-example-network-printer"></a>사용 예: 네트워크 프린터

```csharp
String oobBlobNetworkPrinter =
  "{\"connectionKind\":\"Network\"," +
  "\"physicalAddress\":\"AA:BB:CC:DD:EE:FF\"," +
  "\"connectionString\":\"192.168.1.1:9001\"," +
  "\"peripheralKinds\":\"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}\"," +
  "\"providerId\":\"{02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2}\"," +
  "\"providerName\":\"PrinterProtocolProvider.dll\"}";

printer = await PosPrinter.FromIdAsync(oobBlobNetworkPrinter);
```

## <a name="usage-example-bluetooth-printer"></a>사용 예: Bluetooth 프린터

```csharp
string oobBlobBTPrinter =
    "{\"connectionKind\":\"Bluetooth\"," +
    "\"physicalAddress\":\"AA:BB:CC:DD:EE:FF\"," +
    "\"peripheralKinds\":\"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}\"," +
    "\"providerId\":\"{CCD5B810-95B9-4320-BA7E-78C223CAF418}\"," +
    "\"providerName\":\"PrinterProtocolProvider.dll\"}";

printer = await PosPrinter.FromIdAsync(oobBlobBTPrinter);

```
