---
author: msatranjr
title: Bluetooth 저 에너지
description: 이 항목에서는 UWP 앱에서 Bluetooth LE의 빠른 개요를 제공 합니다.
ms.author: misatran
ms.date: 03/15/2017
ms.topic: article
keywords: windows 10, uwp, bluetooth, bluetooth LE, 저 에너지, gatt, 간격, 중앙, 주변 장치, 클라이언트, 서버, 감시자, 게시자
ms.localizationpriority: medium
ms.openlocfilehash: 9e5bef16c76ee14c2abb7a5a41ab02d150a97333
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6199080"
---
# <a name="bluetooth-low-energy"></a>Bluetooth 저 에너지
Bluetooth LE (저 에너지) 및 전원 효율적인 장치 간에 통신 프로토콜을 정의 하는 사양입니다. 디바이스 검색 일반 액세스 프로필 (간격) 프로토콜을 통해 수행 됩니다. 검색 한 후 장치에 통신 일반 특성 (GATT) 프로토콜을 통해 수행 됩니다. 이 항목에서는 UWP 앱에서 Bluetooth LE의 빠른 개요를 제공 합니다. Bluetooth LE에 대 한 자세한 정보를 참조 하세요 [Bluetooth Core 사양](https://www.bluetooth.com/specifications/bluetooth-core-specification) 버전 4.0, Bluetooth LE 도입 되었습니다. 

![Bluetooth LE 역할](images/gatt-roles.png)

*GATT 및 간격 역할은 Windows 10 버전 1703에에서 도입 되었습니다.*

GATT 및 간격 프로토콜 다음 네임 스페이스를 사용 하 여 UWP 앱에 구현할 수 있습니다.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>중앙 및 주변 장치
검색의 두 가지 기본 역할 중앙 및 주변 장치 라고 합니다. 일반적으로 Windows는 중앙 모드에서 작동 하 고 다양 한 주변 장치에 연결 합니다. 

## <a name="attributes"></a>특성
Windows Bluetooth Api에 표시 되는 일반적인 머리글자어 일반 특성 (GATT)입니다. GATT 프로필 데이터의 구조 및 두 개의 Bluetooth LE 장치 통신 하는 작업의 모드를 정의 합니다. 특성은 GATT의 주 빌딩 블록입니다. 특성의 주요 형식은 서비스, 특징 및 설명자입니다. 이러한 특성의 상호 작용 관련 섹션에서 논의 하 더 유용 하므로 클라이언트와 서버 간의 다르게 수행 합니다. 

![일반적인 프로필의 일반적인 특성 계층](images/gatt-service.png)

*GATT 서버 API 형태로 심 박수 서비스 표시 됩니다.*

## <a name="client-and-server"></a>클라이언트 및 서버
연결을 설정한 후 서버 (일반적으로 작은 IoT 센서 또는 착용) 데이터를 포함 하는 장치 라고 합니다. 클라이언트는 기능을 수행 하려면 해당 데이터를 사용 하는 장치 라고 합니다. 예를 들어 Windows PC (클라이언트)에서 데이터를 읽고 심 박수 모니터 (서버)를 추적 하는 사용자가 작업 가장 적절 하 게 합니다. 자세한 내용은 [GATT 클라이언트](gatt-client.md) 및 [서버 GATT](gatt-server.md) 항목을 참조 하세요.

## <a name="watchers-and-publishers-beacons"></a>감시자 및 게시자 (탐지)
중앙 및 주변 장치 역할을 하는 것 외에도 관찰자 및 방송국 역할입니다. 방송국 일반적으로 탐지 라고, 이러한 알리지 않습니다 GATT을 통해 통신을 위한 광고 패킷에서 제공 되는 제한 된 공간을 사용 하기 때문에. 마찬가지로, 관찰자 데이터를 수신 하도록 연결을 설정 하지 않아도, 근처 광고를 검색 합니다. 근처 광고 관찰 하는 Windows를 구성 하려면 [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) 클래스를 사용 합니다. 페이로드 비콘 브로드캐스트 하기 위해 [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) 클래스를 사용 합니다. 자세한 내용은 [광고](ble-beacon.md) 항목을 참조 하세요.

## <a name="see-also"></a>참고 항목
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Bluetooth 핵심 사양](https://www.bluetooth.com/specifications/bluetooth-core-specification)