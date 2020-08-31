---
ms.assetid: 23001DA5-C099-4C02-ACE9-3597F06ECBF4
title: AEP 서비스 클래스 ID
description: 연결 끝점 (AEP) 서비스는 지정 된 프로토콜을 통해 장치가 지 원하는 서비스에 대 한 프로그래밍 계약을 제공 합니다. 이러한 서비스 중 일부는이를 참조할 때 사용 해야 하는 식별자를 설정 합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 025db9ae6ed3b7ab2c532ddc140fd5279db58777
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168617"
---
# <a name="aep-service-class-ids"></a>AEP 서비스 클래스 ID

연결 끝점 (AEP) 서비스는 지정 된 프로토콜을 통해 장치가 지 원하는 서비스에 대 한 프로그래밍 계약을 제공 합니다. 이러한 서비스 중 일부는이를 참조할 때 사용 해야 하는 식별자를 설정 합니다. 이러한 계약은 **ServiceClassId** 속성을 사용 하 여 식별 됩니다. 이 항목에는 잘 알려진 여러 AEP service 클래스 Id가 나열 되어 있습니다. AEP 서비스 클래스 ID는 사용자 지정 클래스 Id가 있는 프로토콜에도 적용 됩니다.

앱 개발자는 클래스 Id에 기반한 AQS (고급 쿼리 구문) 필터를 사용 하 여 해당 쿼리를 사용 하려는 AEP 서비스로 제한 해야 합니다. 이렇게 하면 쿼리 결과가 관련 서비스로 제한 되 고 장치에 대 한 성능, 배터리 수명 및 서비스 품질이 크게 향상 됩니다. 예를 들어 응용 프로그램은 이러한 서비스 클래스 Id를 사용 하 여 장치를 Miracast 동기화 또는 DLNA 디지털 미디어 렌더러 (DMR)로 사용할 수 있습니다. 장치와 서비스가 서로 상호 작용 하는 방법에 대 한 자세한 내용은 [**Deviceinformationkind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)를 참조 하세요.

> **중요 API**
>
> - [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)

## <a name="bluetooth-and-bluetooth-le-services"></a>Bluetooth 및 Bluetooth LE 서비스

Bluetooth 서비스는 Bluetooth 프로토콜 또는 Bluetooth LE 프로토콜의 두 프로토콜 중 하나에 속합니다. 이러한 프로토콜에 대 한 식별자는 다음과 같습니다.

- Bluetooth 프로토콜 ID: {e0cbf06c-cd8b-4647-bb8a}
- Bluetooth LE 프로토콜 ID: {bb7bb05e-5972-42b5-94fc-76eaa7084d49}

Bluetooth 프로토콜은 모두 동일한 기본 형식을 따라 여러 서비스를 지원 합니다. GUID의 처음 4 자리는 서비스에 따라 다르지만 모든 Bluetooth Guid는 **0000-0000-1000-8000-00805F9B34FB**로 끝납니다. 예를 들어 RFCOMM 서비스의 기반이는 0x0003 이므로 전체 ID는 **00030000-0000-1000-8000-00805F9B34FB**입니다. 다음 표에서는 몇 가지 일반적인 Bluetooth 서비스를 보여 줍니다.

| 서비스 이름                         | GUID                                     |
|--------------------------------------|------------------------------------------|
| RFCOMM                               | **00030000-0000-1000-8000-00805F9B34FB** |
| GATT-경고 알림 서비스    | **18110000-0000-1000-8000-00805F9B34FB** |
| GATT-자동화 IO                 | **18150000-0000-1000-8000-00805F9B34FB** |
| GATT-배터리 서비스               | **180F0000-0000-1000-8000-00805F9B34FB** |
| GATT-블러드 압력                | **18100000-0000-1000-8000-00805F9B34FB** |
| GATT-본문 컴퍼지션              | **181B0000-0000-1000-8000-00805F9B34FB** |
| GATT-본드지 관리               | **181E0000-0000-1000-8000-00805F9B34FB** |
| GATT-지속적인 glucose 모니터링 | **181F0000-0000-1000-8000-00805F9B34FB** |
| GATT-현재 시간 서비스          | **18050000-0000-1000-8000-00805F9B34FB** |
| GATT-전원 순환                 | **18180000-0000-1000-8000-00805F9B34FB** |
| GATT-속도 및 흐름 순환     | **18160000-0000-1000-8000-00805F9B34FB** |
| GATT-장치 정보            | **180A0000-0000-1000-8000-00805F9B34FB** |
| GATT-환경 감지         | **181A0000-0000-1000-8000-00805F9B34FB** |
| GATT-일반 액세스                | **18000000-0000-1000-8000-00805F9B34FB** |
| GATT-Generic 특성             | **18010000-0000-1000-8000-00805F9B34FB** |
| GATT-Glucose                       | **18080000-0000-1000-8000-00805F9B34FB** |
| GATT-상태 온도계            | **18090000-0000-1000-8000-00805F9B34FB** |
| GATT-하트 요금                    | **180D0000-0000-1000-8000-00805F9B34FB** |
| GATT-휴먼 인터페이스 장치        | **18120000-0000-1000-8000-00805F9B34FB** |
| GATT-즉각적인 경고               | **18020000-0000-1000-8000-00805F9B34FB** |
| GATT-실내 위치 지정            | **18210000-0000-1000-8000-00805F9B34FB** |
| GATT-인터넷 프로토콜 지원     | **18200000-0000-1000-8000-00805F9B34FB** |
| GATT-링크 손실                     | **18030000-0000-1000-8000-00805F9B34FB** |
| GATT - 위치 및 탐색       | **18190000-0000-1000-8000-00805F9B34FB** |
| GATT-다음 DST 변경 서비스       | **18070000-0000-1000-8000-00805F9B34FB** |
| GATT-전화 경고 상태 서비스    | **180E0000-0000-1000-8000-00805F9B34FB** |
| GATT-펄스 oximeter                | **18220000-0000-1000-8000-00805F9B34FB** |
| GATT-참조 시간 업데이트 서비스 | **18060000-0000-1000-8000-00805F9B34FB** |
| GATT-실행 속도 및 흐름     | **18140000-0000-1000-8000-00805F9B34FB** |
| GATT-검색 매개 변수               | **18130000-0000-1000-8000-00805F9B34FB** |
| GATT-Tx 전원                      | **18040000-0000-1000-8000-00805F9B34FB** |
| GATT-사용자 데이터                     | **181C0000-0000-1000-8000-00805F9B34FB** |
| GATT-무게 눈금                  | **181D0000-0000-1000-8000-00805F9B34FB** |

사용 가능한 Bluetooth 서비스의 전체 목록은 [GATT services 사양](https://www.bluetooth.com/specifications/gatt/services/)을 참조 하세요. [**GattServiceUuids**](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile.GattServiceUuids) API를 사용 하 여 몇 가지 일반적인 GATT 서비스를 가져올 수도 있습니다.

## <a name="custom-bluetooth-le-services"></a>사용자 지정 Bluetooth LE 서비스

사용자 지정 Bluetooth LE 서비스는 프로토콜 식별자 {bb7bb05e-5972-42b5-94fc76eaa7084d49}를 사용 합니다.

사용자 지정 프로필은 고유 하 게 정의 된 Guid를 사용 하 여 정의 됩니다. 이 사용자 지정 GUID는 **ServiceClassId**에 사용 해야 합니다.

## <a name="upnp-services"></a>UPnP 서비스

UPnP 서비스는 다음 프로토콜 식별자를 사용 합니다. {0e261de4-12f0-46e6-91ba-428607ccef64}

일반적으로 모든 UPnP 서비스의 이름은 RFC 4122에 정의 된 알고리즘을 사용 하 여 GUID로 해시 됩니다. 다음 표에서는 Windows에 정의 된 몇 가지 일반적인 UPnP 서비스를 보여 줍니다.

| 서비스 이름                       | GUID                                      |
|------------------------------------|-------------------------------------------|
| ODBC 대상 편집기                 | **ba36014c-b51f-51cc-bf71-1ad779ced3c6**  |
| AV 전송                       | **deeacb78-707a-52df-b1c6-6f945e7e25bf**  |
| 렌더링 컨트롤                  | **cc7fe721-a3c7-5a14-8c49-4419dc895513**  |
| 3 계층 전달                 | **97d477fa-f403-577b-a714-b29a9007797f**  |
| WAN 공통 인터페이스 구성 | **e4c1c624-c3c4-5104-b72e-ac425d9d157c**  |
| WAP IP 연결                  | **e4ac1c23-b5ac-5c27-8814-6bd837d8832c**  |
| WFA WLAN 구성             | **23d5f7db-747f-5099-8f21-3ddfd0c3c688**  |
| 프린터 향상                   | **fb9074da-3d9f-5384-922e-9978ae51ef0c**  |
| 프린터 기본                      | **5d2a7252-d45c-5158-87a4-05212da327e1**  |
| 미디어 수신기 등록 기관           | **0b4a2add-d725-5198-b2ba-852b8bf8d183**  |
| 콘텐츠 디렉터리                  | **89e701dd-0597-5279-a31c-235991d0db1c**  |
| 다이얼                               | **085dfa4a-3948-53c7-a0d7-16d8ec26b29b**  |

## <a name="wsd-services"></a>WSD 서비스

WSD 서비스는 다음 프로토콜 식별자를 사용 합니다. {782232aa-a2f9-4993-971b-aedc551346b0}

일반적으로 모든 WSD 서비스의 이름은 RFC 4122에 정의 된 알고리즘을 사용 하 여 GUID로 해시 됩니다. 다음 표에서는 Windows에 정의 된 몇 가지 일반적인 WSD 서비스를 보여 줍니다.

| 서비스 이름 | GUID                                     |
|--------------|------------------------------------------|
| 프린터      | **65dca7bd-2611-583e-9a12-ad90f47749cf** |
| 스캐너      | **56ec8b9e-0237-5cae-aa3f-d322dd2e6c1e** |

## <a name="aqs-sample"></a>AQS 샘플

이 AQS는 전화 걸기를 지 원하는 모든 UPnP **Associationendpointservice** 개체를 필터링 합니다. 이 경우 [**Deviceinformationkind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 는 **Ationentationendpointservice**로 설정 됩니다.

``` syntax
System.Devices.AepService.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}" AND
System.Devices.AepService.ServiceClassId:="{085DFA4A-3948-53C7-A0D7-16D8EC26B29B}"
```