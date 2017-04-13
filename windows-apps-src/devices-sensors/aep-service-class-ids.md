---
author: DBirtolo
ms.assetid: 23001DA5-C099-4C02-ACE9-3597F06ECBF4
title: "AEP 서비스 클래스 ID"
description: "AEP(연결 끝점) 서비스는 디바이스에서 지정된 프로토콜을 통해 지원하는 서비스에 대한 프로그래밍 계약을 제공합니다. 이러한 서비스 중 일부에는 참조할 때 사용해야 하는 식별자가 설정되어 있습니다."
ms.author: dbirtolo
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 354ed6c8d2a58bff68c798e66bfde0be03c5b140
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="aep-service-class-ids"></a>AEP 서비스 클래스 ID

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

AEP(연결 끝점) 서비스는 디바이스에서 지정된 프로토콜을 통해 지원하는 서비스에 대한 프로그래밍 계약을 제공합니다. 이러한 서비스 중 일부에는 참조할 때 사용해야 하는 식별자가 설정되어 있습니다. 이러한 계약은 **System.Devices.AepService.ServiceClassId** 속성으로 식별됩니다. 이 항목에서는 널리 알려진 몇 가지 AEP 서비스 클래스 ID를 설명합니다. AEP 서비스 클래스 ID는 사용자 지정 클래스 ID를 사용하는 프로토콜에도 적용됩니다.

앱 개발자는 클래스 ID를 기반으로 하는 AQS(고급 쿼리 구문) 필터를 사용하여 해당 쿼리를 사용하려는 AEP 서비스로 제한해야 합니다. 이는 쿼리 결과를 관련 서비스로 제한하며, 디바이스의 성능, 배터리 수명 및 서비스 품질을 크게 증가시킵니다. 예를 들어 응용 프로그램에서는 이러한 서비스 클래스 ID를 사용하여 디바이스를 Miracast 동기화 또는 DLNA DMR(디지털 미디어 렌더러)로 사용할 수 있습니다. 디바이스와 서비스가 상호 작용하는 방식에 대한 자세한 내용은 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)를 참조하세요.

## <a name="bluetooth-and-bluetooth-le-services"></a>Bluetooth 및 Bluetooth LE 서비스

Bluetooth 서비스는 Bluetooth 프로토콜과 Bluetooth LE 프로토콜의 두 가지 프로토콜 중 하나에 속합니다. 이러한 프로토콜의 식별자는 다음과 같습니다.

-   Bluetooth 프로토콜 ID: {e0cbf06c-cd8b-4647-bb8a263b43f0f974}
-   Bluetooth LE 프로토콜 ID: {bb7bb05e-5972-42b5-94fc76eaa7084d49}

Bluetooth 프로토콜은 여러 서비스를 지원하며, 모두 동일한 기본 형식을 따릅니다. GUID의 처음 4자리는 서비스에 따라 달라질 수 있지만 모든 Bluetooth GUID는 **0000-0000-1000-8000-00805F9B34FB**로 끝납니다. 예를 들어 RFCOMM 서비스는 0x0003으로 시작하므로 전체 ID는 **00030000-0000-1000-8000-00805F9B34FB**가 됩니다. 다음 표에는 몇 가지 일반적인 Bluetooth 서비스가 나와 있습니다.

| 서비스 이름                         | GUID                                     |
|--------------------------------------|------------------------------------------|
| RFCOMM                               | **00030000-0000-1000-8000-00805F9B34FB** |
| GATT - 경고 알림 서비스    | **18110000-0000-1000-8000-00805F9B34FB** |
| GATT - 자동화 IO                 | **18150000-0000-1000-8000-00805F9B34FB** |
| GATT - 배터리 서비스               | **180F0000-0000-1000-8000-00805F9B34FB** |
| GATT - 혈압                | **18100000-0000-1000-8000-00805F9B34FB** |
| GATT - 체성분              | **181B0000-0000-1000-8000-00805F9B34FB** |
| GATT - 채권 관리               | **181E0000-0000-1000-8000-00805F9B34FB** |
| GATT - 연속 혈당 모니터링 | **181F0000-0000-1000-8000-00805F9B34FB** |
| GATT - 현재 시간 서비스          | **18050000-0000-1000-8000-00805F9B34FB** |
| GATT - 순환 전원                 | **18180000-0000-1000-8000-00805F9B34FB** |
| GATT - 순환 속도 및 흐름     | **18160000-0000-1000-8000-00805F9B34FB** |
| GATT - 디바이스 정보            | **180A0000-0000-1000-8000-00805F9B34FB** |
| GATT - 환경 감지         | **181A0000-0000-1000-8000-00805F9B34FB** |
| GATT - 일반 액세스                | **18000000-0000-1000-8000-00805F9B34FB** |
| GATT - 일반 특성             | **18010000-0000-1000-8000-00805F9B34FB** |
| GATT - 혈당                       | **18080000-0000-1000-8000-00805F9B34FB** |
| GATT - 건강 온도계            | **18090000-0000-1000-8000-00805F9B34FB** |
| GATT - 심박수                    | **180D0000-0000-1000-8000-00805F9B34FB** |
| GATT - 휴먼 인터페이스 디바이스        | **18120000-0000-1000-8000-00805F9B34FB** |
| GATT - 즉각적 경고               | **18020000-0000-1000-8000-00805F9B34FB** |
| GATT - 실내 위치 지정            | **18210000-0000-1000-8000-00805F9B34FB** |
| GATT - 인터넷 프로토콜 지원     | **18200000-0000-1000-8000-00805F9B34FB** |
| GATT - 링크 끊어짐                     | **18030000-0000-1000-8000-00805F9B34FB** |
| GATT - 위치 및 탐색       | **18190000-0000-1000-8000-00805F9B34FB** |
| GATT - 다음 DST 변경 서비스       | **18070000-0000-1000-8000-00805F9B34FB** |
| GATT - 전화 알림 상태 서비스    | **180E0000-0000-1000-8000-00805F9B34FB** |
| GATT - 산소포화도 측정기                | **18220000-0000-1000-8000-00805F9B34FB** |
| GATT - 기준 시간 업데이트 서비스 | **18060000-0000-1000-8000-00805F9B34FB** |
| GATT - 실행 속도 및 흐름     | **18140000-0000-1000-8000-00805F9B34FB** |
| GATT - 검사 매개 변수               | **18130000-0000-1000-8000-00805F9B34FB** |
| GATT - 송신 전력                      | **18040000-0000-1000-8000-00805F9B34FB** |
| GATT - 사용자 데이터                     | **181C0000-0000-1000-8000-00805F9B34FB** |
| GATT - 체중계                  | **181D0000-0000-1000-8000-00805F9B34FB** |

 

사용 가능한 Bluetooth 서비스의 전체 목록은 Bluetooth의 프로토콜 및 서비스 페이지([여기](http://go.microsoft.com/fwlink/p/?LinkID=619586) 및 [여기](http://go.microsoft.com/fwlink/p/?LinkID=619587))를 참조하세요. [**GattServiceUuids**](https://msdn.microsoft.com/library/windows/apps/Dn297571) API를 사용하여 몇 가지 일반적인 GATT 서비스를 가져올 수도 있습니다.

## <a name="custom-bluetooth-le-services"></a>사용자 지정 Bluetooth LE 서비스

사용자 지정 Bluetooth LE 서비스는 프로토콜 식별자 {bb7bb05e-5972-42b5-94fc76eaa7084d49}를 사용합니다.

사용자 지정 프로필은 자체 정의된 GUID로 정의됩니다. 이 사용자 지정 GUID를 **System.Devices.AepService.ServiceClassId**에 사용해야 합니다.

## <a name="upnp-services"></a>UPnP 서비스

UPnP 서비스는 프로토콜 식별자 {0e261de4-12f0-46e6-91ba428607ccef64}를 사용합니다.

일반적으로 모든 UPnP 서비스는 RFC 4122에 정의된 알고리즘을 사용하여 GUID로 해시된 이름을 가집니다. 다음 표에는 Windows에 정의된 몇 가지 일반적인 UPnP 서비스가 나와 있습니다.

| 서비스 이름                       | GUID                                     |
|------------------------------------|------------------------------------------|
| 연결 관리자                 | **ba36014c-b51f-51cc-bf711ad779ced3c6**  |
| AV 전송                       | **deeacb78-707a-52df-b1c66f945e7e25bf**  |
| 렌더링 제어                  | **cc7fe721-a3c7-5a14-8c494419dc895513**  |
| 계층 3 전달                 | **97d477fa-f403-577b-a714b29a9007797f**  |
| WAN 공용 인터페이스 구성 | **e4c1c624-c3c4-5104-b72eac425d9d157c**  |
| WAP IP 연결                  | **e4ac1c23-b5ac-5c27-88146bd837d8832c**  |
| WFA WLAN 구성             | **23d5f7db-747f-5099-8f213ddfd0c3c688**  |
| 고급 프린터                   | **fb9074da-3d9f-5384-922e9978ae51ef0c**  |
| 기본 프린터                      | **5d2a7252-d45c-5158-87-a405212da327e1** |
| 미디어 수신기 등록자           | **0b4a2add-d725-5198-b2ba852b8bf8d183**  |
| 콘텐츠 디렉터리                  | **89e701dd-0597-5279-a31c235991d0db1c**  |
| DIAL                               | **085dfa4a-3948-53c7-a0d716d8ec26b29b**  |

 

## <a name="wsd-services"></a>WSD 서비스

WSD 서비스는 프로토콜 식별자 {782232aa-a2f9-4993-971baedc551346b0}을 사용합니다.

일반적으로 모든 WSD 서비스는 RFC 4122에 정의된 알고리즘을 사용하여 GUID로 해시된 이름을 가집니다. 다음 표에는 Windows에 정의된 몇 가지 일반적인 WSD 서비스가 나와 있습니다.

| 서비스 이름 | GUID                                    |
|--------------|-----------------------------------------|
| 프린터      | **65dca7bd-2611-583e-9a12ad90f47749cf** |
| 스캐너      | **56ec8b9e-0237-5cae-aa3fd322dd2e6c1e** |

 

## <a name="aqs-sample"></a>AQS 샘플

이 AQS는 DIAL을 지원하는 모든 UPnP **AssociationEndpointService** 개체를 필터링합니다. 이 경우 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)는 **AsssociationEndpointService**로 설정됩니다.

``` syntax
System.Devices.AepService.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}" AND
System.Devices.AepService.ServiceClassId:="{085DFA4A-3948-53C7-A0D716D8EC26B29B}"
```

 

 
