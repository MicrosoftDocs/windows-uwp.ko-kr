---
title: Bluetooth 저 에너지
description: 이 항목에서는 UWP 앱의 Bluetooth LE에 대한 빠른 개요를 제공합니다.
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, uwp, bluetooth, bluetooth LE, 저 에너지, gatt, gap, 중앙, 주변, 클라이언트, 서버, 감시자, 게시자
ms.localizationpriority: medium
ms.openlocfilehash: 4859dfb540b252f379a0ec3cbfe52985c0776fd9
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684851"
---
# <a name="bluetooth-low-energy"></a>Bluetooth 저 에너지
Bluetooth LE(저 에너지)는 전력 효율적인 디바이스 간 검색 및 통신에 대한 프로토콜을 정의하는 사양입니다. 장치 검색은 GAP(일반 액세스 프로필) 프로토콜을 통해 수행됩니다. 검색 후 장치 간 통신은 GATT(일반 특성) 프로토콜을 통해 수행됩니다. 이 항목에서는 UWP 앱의 Bluetooth LE에 대한 빠른 개요를 제공합니다. Bluetooth LE에 대해 자세히 알아보려면 Bluetooth LE가 도입된 [Bluetooth Core 사양](https://www.bluetooth.com/specifications/bluetooth-core-specification/) 버전 4.0을 참조하세요. 

![Bluetooth LE 역할](images/gatt-roles.png)

*GATT 및 GAP 역할은 Windows 10 버전 1703에서 도입 되었습니다.*

다음 네임스페이스를 사용하여 UWP 앱에서 GATT 및 GAP 프로토콜을 구현할 수 있습니다.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows. 장치.](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement)

## <a name="central-and-peripheral"></a>중앙 및 주변 기기
검색의 두 가지 기본 역할을 중앙 및 주변 기기라고 합니다. 일반적으로 Windows는 중앙 모드에서 작동하고 다양한 주변 장치에 연결됩니다. 

## <a name="attributes"></a>특성
Windows Bluetooth API에서 볼 수 있는 일반적인 약어는 GATT(일반 특성)입니다. GATT 프로필은 두 Bluetooth LE 장치가 통신하는 데이터의 구조 및 작업 모드를 정의합니다. 특성은 GATT의 주요 구성 요소입니다. 주요 특성 유형은 서비스, 특징 및 설명자입니다. 이러한 특성은 클라이언트 및 서버 간에 다르게 수행되며 이의 상호 작용에 대해서는 관련 섹션에서 논의하는 것이 더 유용합니다. 

![공통 프로필의 일반적인 특성 계층](images/gatt-service.png)

*심장 rate service는 GATT Server API 형식으로 표현 됩니다.*

## <a name="client-and-server"></a>클라이언트 및 서버
연결이 설정된 후 데이터를 포함하는 디바이스(일반적으로 작은 IoT 센서 또는 웨어러블)를 서버라고 합니다. 기능을 수행하기 위해 이 데이터를 사용하는 디바이스를 클라이언트라고 합니다. 예를 들어, Windows PC(클라이언트)는 심박수 모니터(서버)에서 데이터를 읽어 최적으로 사용자의 운동을 추적합니다. 자세한 내용은 [GATT 클라이언트](gatt-client.md) 및 [GATT 서버](gatt-server.md) 항목을 참조하세요.

## <a name="watchers-and-publishers-beacons"></a>감시자 및 게시자(비콘)
중앙 및 주변 역할 외에 관찰자 및 브로드캐스터 역할이 있습니다. 브로드캐스터는 일반적으로 비콘이라고 하며 광고 패킷에서 제공되는 제한된 공간을 사용하기 때문에 GATT를 통해 통신하지 않습니다. 마찬가지로, 관찰자는 데이터를 수신하기 위해 연결을 설정하지 않아도 되며 근처의 광고를 검사합니다. Windows가 근처에 있는 광고를 관찰하도록 구성하려면 [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) 클래스를 사용합니다. 비콘 페이로드를 브로드캐스트하기 위해 [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) 클래스를 사용합니다. 자세한 내용은 [광고](ble-beacon.md) 항목을 참조하세요.

## <a name="see-also"></a>참고 항목
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows. 장치.](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement)
- [Bluetooth 핵심 사양](https://www.bluetooth.com/specifications/bluetooth-core-specification)
