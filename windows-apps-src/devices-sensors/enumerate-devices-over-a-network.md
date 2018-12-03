---
ms.assetid: E0B9532F-1195-4927-99BE-F41565D891AD
title: 네트워크를 통해 디바이스 열거
description: 로컬로 연결된 디바이스를 검색하는 것 외에 Windows.Devices.Enumeration API를 사용하여 무선 및 네트워크 프로토콜을 통해 디바이스를 열거할 수 있습니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e80d16b3338291c756b543018812e9db1370a4ac
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8459915"
---
# <a name="enumerate-devices-over-a-network"></a>네트워크를 통해 디바이스 열거



**중요 API**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

로컬로 연결된 장치를 검색하는 것 외에 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API를 사용하여 무선 및 네트워크 프로토콜을 통해 장치를 열거할 수 있습니다.

## <a name="enumerating-devices-over-networked-or-wireless-protocols"></a>네트워크 또는 무선 프로토콜을 통해 디바이스 열거

경우에 따라 로컬로 연결되어 있지 않고 무선 또는 네트워킹 프로토콜을 통해서만 검색할 수 있는 장치를 열거해야 합니다. 이렇게 하기 위해 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API에는 세 가지 종류의 디바이스 개체인 **AssociationEndpoint**(AEP), **AssociationEndpointContainer**(AEP 컨테이너) 및 **AssociationEndpointService**(AEP 서비스)가 포함되어 있습니다. 이들을 묶어서 AEP 또는 AEP 개체라고 합니다.

일부 장치 API는 사용 가능한 AEP 개체를 열거하는 데 사용할 수 있는 선택기 문자열을 제공합니다. 여기에는 시스템과 페어링된 장치와 페어링되지 않은 장치가 모두 포함될 수 있습니다. 일부 장치는 페어링이 필요하지 않을 수도 있습니다. 이러한 장치 API는 장치를 조작하기 전에 페어링이 필요한 경우 장치를 페어링하려고 할 수 있습니다. Wi-Fi Direct는 이 패턴을 따르는 API의 예입니다. 이러한 장치 API가 장치를 자동으로 페어링하지 않는 경우 [**DeviceInformation.Pairing**](https://msdn.microsoft.com/library/windows/apps/Dn705960)서 사용 가능한 [**DeviceInformationPairing**](https://msdn.microsoft.com/library/windows/apps/Mt168396) 개체를 사용하여 페어링할 수 있습니다.

그러나 미리 정의된 선택기 문자열을 사용하지 않고 직접 장치를 수동으로 검색하려는 경우도 있을 수 있습니다. 예를 들어 AEP 장치에 대한 정보만 수집하고 장치를 조작하지 않거나 미리 정의된 선택기 문자열로 검색되는 것 외에 AEP 개체를 추가로 찾고 싶을 수 있습니다. 이 경우에 [장치 선택기 빌드](build-a-device-selector.md)에 있는 지침에 따라 고유한 선택기 문자열을 빌드하고 사용합니다.

고유한 선택기를 빌드할 때는 열거형 범위를 관심 있는 프로토콜로 제한하는 것이 좋습니다. 예를 들어 UPnP 장치에 특별히 관심이 있는 경우 Wi-Fi 무선으로 Wi-Fi Direct 장치를 검색하지 않을 수 있습니다. Windows에는 열거형의 범위를 지정하는 데 사용할 수 있는 각 프로토콜에 대한 ID가 정의되어 있습니다. 다음 표에는 프로토콜 유형 및 식별자가 나와 있습니다.

| 프로토콜 또는 네트워크 장치 유형              | Id                                         |
|----------------------------------------------|--------------------------------------------|
| UPnP(DIAL 및 DLNA 포함)               | **{0e261de4-12f0-46e6-91ba-428607ccef64}** |
| WSD(Web Services on Devices)                | **{782232aa-a2f9-4993-971b-aedc551346b0}** |
| Wi-Fi Direct                                 | **{0407d24e-53de-4c9a-9ba1-9ced54641188}** |
| DNS-SD(DNS Service Discovery)               | **{4526e8c1-8aac-4153-9b16-55e86ada0e54}** |
| 서비스 지점                             | **{d4bf61b3-442e-4ada-882d-fa7B70c832d9}** |
| 네트워크 프린터(Active Directory 프린터) | **{37aba761-2124-454c-8d82-c42962c2de2b}** |
| WCN(Windows Connect Now)                    | **{4c1b1ef8-2f62-4b9f-9bc5-b21ab636138f}** |
| WiGig dock                                  | **{a277f3a5-8764-4f88-8045-4c5e962640b1}** |
| HP 프린터용 Wi-Fi 프로비저닝           | **{c85ef710-f344-4792-bb6d-85a4346f1e69}** |
| Bluetooth                                    | **{e0cbf06c-cd8b-4647-bb8a-263b43f0f974}** |
| Bluetooth LE                                 | **{bb7bb05e-5972-42b5-94fc-76eaa7084d49}** |

 

## <a name="aqs-examples"></a>AQS 예제

각 AEP 종류에는 열거형을 특정 프로토콜로 제한하는 데 사용할 수 있는 속성이 있습니다. AQS 필터의 OR 연산자를 사용하여 여러 프로토콜을 결합할 수 있습니다. 다음은 AEP 장치 쿼리 방법을 보여 주는 AQS 필터 문자열의 몇 가지 예입니다.

이 AQS는 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)가 **AsssociationEndpoint**로 설정된 모든 UPnP **AssociationEndpoint** 개체를 쿼리합니다.

``` syntax
System.Devices.Aep.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

이 AQS는 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)가 **AsssociationEndpoint**로 설정된 모든 UPnP 및 WSD **AssociationEndpoint** 개체를 쿼리합니다.

``` syntax
System.Devices.Aep.ProtocolId:="{782232aa-a2f9-4993-971b-aedc551346b0}" OR
System.Devices.Aep.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

이 AQS는 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)가 **AsssociationEndpointService**로 설정된 경우 모든 UPnP **AssociationEndpointService** 개체를 쿼리합니다.

``` syntax
System.Devices.AepService.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

이 AQS는 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)가 **AssociationEndpointContainer**로 설정된 **AssociationEndpointContainer** 개체를 쿼리하지만 UPnP 프로토콜을 열거하는 방식으로만 개체를 찾습니다. 따라서 일반적으로 하나의 프로토콜에서만 제공되는 컨테이너를 열거하는 데에는 유용하지 않습니다. 그러나 장치를 검색할 수 있음을 알고 있는 프로토콜로 필터를 제한하면 유용할 수 있습니다.

``` syntax
System.Devices.AepContainer.ProtocolIds:~~"{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

 

 
