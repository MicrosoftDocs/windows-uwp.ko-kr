---
title: Bluetooth 저 에너지
description: 전원 효율적인 장치 간의 검색 및 통신을 위한 프로토콜을 정의 하는 UWP 앱에서 Bluetooth 저 에너지 (LE) 사양에 대해 알아봅니다.
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, bluetooth, bluetooth LE, 저 에너지, gatt, 차이, 중앙, 주변 장치, 클라이언트, 서버, 감시자, 게시자
ms.localizationpriority: medium
ms.openlocfilehash: 566e8e26e1a219a07dd5e7b539d96878a79f9163
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159767"
---
# <a name="bluetooth-low-energy"></a>Bluetooth 저 에너지
Bluetooth 저 에너지 (LE)는 전원 효율적인 장치 간의 검색 및 통신을 위한 프로토콜을 정의 하는 사양입니다. 장치 검색은 GAP (일반 액세스 프로필) 프로토콜을 통해 수행 됩니다. 검색 후 장치-장치 통신은 일반 특성 (GATT) 프로토콜을 통해 수행 됩니다. 이 항목에서는 UWP 앱의 Bluetooth LE에 대 한 간략 한 개요를 제공 합니다. Bluetooth LE에 대 한 자세한 정보를 보려면 bluetooth LE이 도입 된 [Bluetooth 코어 사양](https://www.bluetooth.com/specifications/bluetooth-core-specification/) 버전 4.0을 참조 하세요. 

![Bluetooth LE 역할](images/gatt-roles.png)

*GATT 및 GAP 역할은 Windows 10 버전 1703에서 도입 되었습니다.*

GATT 및 GAP 프로토콜은 다음 네임 스페이스를 사용 하 여 UWP 앱에서 구현할 수 있습니다.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](/uwp/api/windows.devices.bluetooth.advertisement)

## <a name="central-and-peripheral"></a>중앙 및 주변 장치
검색의 두 가지 주요 역할은 중앙 및 주변 장치 라고 합니다. 일반적으로 Windows는 중앙 모드에서 작동 하 고 다양 한 주변 장치에 연결 됩니다. 

## <a name="attributes"></a>특성
Windows Bluetooth Api에 표시 되는 일반적인 머리글자어는 일반 특성 (GATT)입니다. GATT 프로필은 두 개의 Bluetooth LE 장치에서 통신 하는 작업 모드와 데이터의 구조를 정의 합니다. 특성은 GATT의 주 빌딩 블록입니다. 특성의 기본 유형은 서비스, 특성 및 설명자입니다. 이러한 특성은 클라이언트와 서버 간에 다르게 수행 되므로 관련 섹션에서 상호 작용을 설명 하는 것이 더 유용 합니다. 

![공통 프로필의 일반적인 특성 계층](images/gatt-service.png)

*심장 rate service는 GATT Server API 형식으로 표현 됩니다.*

## <a name="client-and-server"></a>클라이언트 및 서버
연결이 설정 된 후에는 데이터를 포함 하는 장치 (일반적으로 작은 IoT 센서 또는 wearable)를 서버 라고 합니다. 이 데이터를 사용 하 여 기능을 수행 하는 장치를 클라이언트 라고 합니다. 예를 들어 Windows PC (클라이언트)는 하트 요금 모니터 (서버)에서 데이터를 읽어 사용자가 최적으로 작업 하 고 있음을 추적 합니다. 자세한 내용은 [GATT Client](gatt-client.md) And [GATT Server](gatt-server.md) 항목을 참조 하세요.

## <a name="watchers-and-publishers-beacons"></a>감시자 및 게시자 (탐지 장치)
중앙 및 주변 역할 외에도 관찰자 및 브로드캐스터 역할이 있습니다. 일반적으로 방송사는 통신을 위해 보급 알림 패킷에 제공 되는 제한 된 공간을 사용 하기 때문에 GATT를 통해 통신 하지 않습니다. 마찬가지로 관찰자는 데이터를 수신 하기 위해 연결을 설정할 필요가 없으며, 주변 광고를 검색 합니다. 근처의 보급 알림을 관찰 하도록 Windows를 구성 하려면 [BluetoothLEAdvertisementWatcher](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) 클래스를 사용 합니다. 신호 페이로드를 브로드캐스트하려면 [BluetoothLEAdvertisementPublisher](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) 클래스를 사용 합니다. 자세한 내용은 [보급 알림](ble-beacon.md) 항목을 참조 하세요.

## <a name="see-also"></a>관련 항목
- [Windows.Devices.Bluetooth.GenericAttributeProfile](/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](/uwp/api/windows.devices.bluetooth.advertisement)
- [Bluetooth 핵심 사양](https://www.bluetooth.com/specifications/bluetooth-core-specification)