---
author: msatranjr
title: Bluetooth 저 에너지
description: 이 항목에서는 Bluetooth LE UWP 앱에서 빠른 개요를 제공 합니다.
ms.author: misatran
ms.date: 03/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, bluetooth, bluetooth LE, 낮은 에너지, gatt, 간격이 있는 테두리, 중앙, 주변 장치, 클라이언트, 서버, 감시자, publisher
ms.localizationpriority: medium
ms.openlocfilehash: 0d6cc1fb5a0b3cb96748b99c490b23a9e1df128f
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "304862"
---
# <a name="bluetooth-low-energy"></a>Bluetooth 저 에너지
Bluetooth 낮은 에너지 (LE)는 검색 및 전원 효율적인 장치 간의 통신에 대 한 프로토콜을 정의 하는 사양입니다. 장치의 검색 일반 액세스 프로필 (간격) 프로토콜을 통해 수행 됩니다. 검색 한 후 장치를 통신 일반 특성 (GATT) 프로토콜을 통해 수행 됩니다. 이 항목에서는 Bluetooth LE UWP 앱에서 빠른 개요를 제공 합니다. Bluetooth LE 하는 방법에 대 한 자세한 정보를 보려면 Bluetooth LE 도입 된 여기서 [Bluetooth 핵심 사양](https://www.bluetooth.com/specifications/bluetooth-core-specification) 버전 4.0를 참조 합니다. 

![Bluetooth LE 역할](images/gatt-roles.png)

*GATT 및의 간격이 있는 테두리 역할 Windows 10 1703 버전에에서 도입 합니다.*

다음 네임 스페이스를 사용 하 여 UWP 앱에 GATT 및의 간격이 있는 테두리 프로토콜을 구현할 수 있습니다.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>중앙 및 주변 장치
검색의 두 기본 역할 중앙와 주변에 호출 됩니다. 일반적으로 Windows 중앙 모드에서 작동 하 고 다양 한 주변 장치에 연결 합니다. 

## <a name="attributes"></a>특성
Windows Bluetooth Api에 표시 되는 일반적인 머리글자어는 일반 특성 (GATT)입니다. GATT 프로필 데이터의 구조 및 Bluetooth LE 다음의 두 장치가 통신할 하는 작업의 모드를 정의 합니다. 특성은 GATT의 기본 빌딩 블록입니다. 특성의 기본 유형은 서비스, 특성 및 설명자입니다. 이러한 특성의 관련 섹션에서의 작용에 설명 하는 많은 유용한 이므로 클라이언트와 서버 간의 다르게 수행 합니다. 

![공통 프로 파일의 일반적인 특성 계층](images/gatt-service.png)

*하트 속도 서비스는 GATT 서버 API 양식 표시 됩니다.*

## <a name="client-and-server"></a>클라이언트 및 서버
대 한 연결을 설정한 후 장치 (일반적으로 소규모 IoT 센서 또는 착용 가능) 데이터를 포함 하는 서버 라고 합니다. 해당 데이터를 사용 하 여 기능을 수행 하는 장치는 클라이언트 라고 합니다. 예, Windows PC (클라이언트)에서 데이터를 읽습니다 하트 속도 모니터 (서버)을 추적 하는 사용자가 작업 최적으로 합니다. 자세한 내용은 [GATT 클라이언트](gatt-client.md) 와 [GATT 서버](gatt-server.md) 항목을 참조 합니다.

## <a name="watchers-and-publishers-beacons"></a>감시자 및 게시자 (탐지 장치)
중앙 및 주변 장치 역할 외에도 관찰자 및 브로드캐스트 역할이 있습니다. 방송국 일반적으로 탐지 장치 라고, 자신이 하지 통신할 때 GATT 통신을 위한 광고 패킷에서 제공 되는 제한 된 공간을 사용 하기 때문에 합니다. 마찬가지로, 감시자 데이터를 수신할 연결을 설정할 수 없는, 근처 광고를 검색 합니다. 광고 근처에 있는 관찰 하는 Windows를 구성 하려면 [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) 클래스를 사용 합니다. 탐지 장치 페이로드 브로드캐스트를 위해 [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) 클래스를 사용 합니다. 자세한 내용은 [광고](ble-beacon.md) 항목을 참조 하세요.

## <a name="see-also"></a>참고 항목
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Bluetooth 핵심 사양](https://www.bluetooth.com/specifications/bluetooth-core-specification)