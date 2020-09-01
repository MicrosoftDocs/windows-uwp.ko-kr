---
ms.assetid: E0B9532F-1195-4927-99BE-F41565D891AD
title: 네트워크를 통해 디바이스 열거
description: 로컬에 연결 된 장치를 검색 하는 것 외에도, Windows. *. 열거 Api를 사용 하 여 무선 및 네트워크로 연결 된 프로토콜을 통해 장치를 열거할 수 있습니다.
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 11aa39ec4a5f4f4574f77ffc490bafe822616a6f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175467"
---
# <a name="enumerate-devices-over-a-network"></a>네트워크를 통해 디바이스 열거



**중요 API**

- [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)

로컬에 연결 된 장치를 검색 하는 것 외에도, [**Windows.**](/uwp/api/Windows.Devices.Enumeration) *. 열거 api를 사용 하 여 무선 및 네트워크로 연결 된 프로토콜을 통해 장치를 열거할 수 있습니다.

## <a name="enumerating-devices-over-networked-or-wireless-protocols"></a>네트워크 또는 무선 프로토콜을 통해 장치 열거

로컬에 연결 되지 않은 장치를 열거 해야 하며 무선 또는 네트워킹 프로토콜을 통해서만 검색할 수 있는 경우도 있습니다. 이 작업을 수행 하기 위해 [**Windows.**](/uwp/api/Windows.Devices.Enumeration) i n d. i n i o n api에는 **associationendpoint** (AEP), **Associationendpointcontainer** (AEP Container) 및 **associationendpointservice** (AEP Service)와 같은 세 가지 종류의 장치 개체가 있습니다. 그룹으로,이를 AEPs 또는 AEP 개체 라고 합니다.

일부 장치 Api는 사용 가능한 AEP 개체를 열거 하는 데 사용할 수 있는 선택기 문자열을 제공 합니다. 여기에는 쌍을 이루는 장치와 시스템과 페어링 되지 않는 두 장치가 모두 포함 될 수 있습니다. 일부 장치에는 페어링이 필요 하지 않을 수 있습니다. 이러한 장치 Api는 연결 하기 전에 필요한 경우 장치를 페어링 하 려 할 수 있습니다. Wi-fi Direct는이 패턴을 따르는 Api의 한 예입니다. 이러한 장치 Api가 자동으로 장치를 페어링 하지 않는 경우 [**Deviceinformation. 페어링**](/uwp/api/windows.devices.enumeration.deviceinformation.pairing)에서 사용할 수 있는 [**deviceinformationpairing**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing) 개체를 사용 하 여 해당 장치를 쌍으로 연결할 수 있습니다.

그러나 미리 정의 된 선택기 문자열을 사용 하지 않고 수동으로 장치를 검색 하려는 경우가 있을 수 있습니다. 예를 들어, AEP 장치에 대 한 정보를 수집 하거나, 미리 정의 된 선택기 문자열을 사용 하 여 검색 되는 것 보다 더 많은 AEP 개체를 찾으려고 할 수 있습니다. 이 경우 사용자 고유의 선택기 문자열을 빌드하고 [장치 선택기 빌드](build-a-device-selector.md)아래의 지침에 따라 사용 합니다.

사용자 고유의 선택기를 빌드하는 경우에는 원하는 프로토콜로 열거 범위를 제한 하는 것이 좋습니다. 예를 들어 UPnP 장치에 특히 관심이 있는 경우 wi-fi Direct 장치를 검색 하지 않으려는 경우를 들 수 있습니다. Windows는 열거 범위를 지정 하는 데 사용할 수 있는 각 프로토콜에 대 한 id를 정의 했습니다. 다음 표에서는 프로토콜 형식 및 식별자를 보여 줍니다.

| 프로토콜 또는 네트워크 장치 유형              | Id                                         |
|----------------------------------------------|--------------------------------------------|
| UPnP (전화 접속과 DLNA 포함)               | **{0e261de4-12f0-46e6-91ba-428607ccef64}** |
| 장치 (WSD)의 웹 서비스                | **{782232aa-a2f9-4993-971b-aedc551346b0}** |
| Wi-fi Direct                                 | **{0407d24e-53de-4c9a-9ba1-9ced54641188}** |
| Dns 서비스 검색 (DNS SD)               | **{4526e8c1-8aac-4153-9b16-55e86ada0e54}** |
| 서비스 지점                             | **{d4bf61b3-442e-4ada-882d-fa7B70c832d9}** |
| 네트워크 프린터 (active directory 프린터) | **{37aba761-2124-454c-8d82-c42962c2de2b}** |
| Windows connect now (c #)                    | **{4c1b1ef8-2f62-4b9f-9bc5-b21ab636138f}** |
| WiGig 도킹                                  | **{a277f3a5-8764-4f88-8045-4c5e962640b1}** |
| HP 프린터용 wi-fi 프로 비전           | **{c85ef710-f344-4792-bb6d-85a4346f1e69}** |
| Bluetooth                                    | **{e0cbf06c-cd8b-4647-bb8a}** |
| Bluetooth LE                                 | **{bb7bb05e-5972-42b5-94fc-76eaa7084d49}** |
| 네트워크 카메라                               | **{b8238652-b500-41eb-b4f3-4234f7f5ae99}** |

 

## <a name="aqs-examples"></a>AQS 예제

각 AEP kind에는 열거를 특정 프로토콜로 제한 하는 데 사용할 수 있는 속성이 있습니다. AQS 필터에 OR 연산자를 사용 하 여 여러 프로토콜을 결합할 수 있습니다. AEP 장치를 쿼리 하는 방법을 보여 주는 AQS 필터 문자열의 몇 가지 예는 다음과 같습니다.

이 AQS는 [**Deviceinformationkind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 가로 설정 된 경우 모든 UPnP **associationendpoint** 개체 **를 쿼리 합니다.**

``` syntax
System.Devices.Aep.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

이 AQS는 [**Deviceinformationkind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 가로 설정 된 경우 모든 UPNP 및 WSD **associationendpoint** 개체에 대 한 **쿼리를 합니다.**

``` syntax
System.Devices.Aep.ProtocolId:="{782232aa-a2f9-4993-971b-aedc551346b0}" OR
System.Devices.Aep.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

이 AQS는 [**Deviceinformationkind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 가로 설정 된 경우 모든 UPnP **associationendpointservice** 개체 **에 대해 쿼리 합니다.**

``` syntax
System.Devices.AepService.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

이 AQS는 [**Deviceinformationkind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 가 **associationendpointcontainer**로 설정 되어 있는 경우 **associationendpointcontainer** 개체를 쿼리 하지만 UPnP 프로토콜을 열거 하 여 해당 개체를 찾습니다. 일반적으로 한 프로토콜 에서만 제공 되는 컨테이너를 열거 하는 것은 유용 하지 않습니다. 그러나이는 장치를 검색할 수 있는 프로토콜로 필터를 제한 하 여 유용 하 게 사용할 수 있습니다.

``` syntax
System.Devices.AepContainer.ProtocolIds:~~"{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

 

 