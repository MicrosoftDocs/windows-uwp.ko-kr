---
author: msatranjr
title: Bluetooth 저 에너지
description: 여기서는 Bluetooth LE UWP 응용 프로그램의 빠른 개요를 제공합니다.
ms.author: misatran
ms.date: 03/15/2017
ms.topic: article
keywords: 창 10, uwp, 블루투스, 블루투스 LE, 낮은 에너지, gatt, 간격, 중앙, 주변, 클라이언트, 서버, 감시자, 출판사
ms.localizationpriority: medium
ms.openlocfilehash: 9e5bef16c76ee14c2abb7a5a41ab02d150a97333
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5919131"
---
# <a name="bluetooth-low-energy"></a>Bluetooth 저 에너지
Bluetooth 낮은 에너지 (LE) 검색 및 전력 효율이 매우 높은 장치 간의 통신을 위한 프로토콜을 정의 하는 사양입니다. 장치의 검색 일반 액세스 프로 파일 (간격) 프로토콜을 통해 수행 됩니다. 검색을 한 후 장치를 통신 일반 특성 (GATT) 프로토콜을 통해 수행 됩니다. 여기서는 Bluetooth LE UWP 응용 프로그램의 빠른 개요를 제공합니다. LE Bluetooth에 대 한 자세한 내용을 참조 하십시오 [Bluetooth 코어 사양](https://www.bluetooth.com/specifications/bluetooth-core-specification) 버전 4.0, Bluetooth LE 도입 된 곳. 

![Bluetooth LE 역할](images/gatt-roles.png)

*GATT 및 간격 역할 Windows 10 1703 버전에서에서 소개 된*

다음 네임 스페이스를 사용 하 여 UWP 앱에 GATT 및 간격 프로토콜을 구현할 수 있습니다.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>중앙 및 주변 기기
검색의 두 가지 주요 역할인 중앙 및 주변 장치 라고 합니다. 일반적으로 중앙의 모드에서 작동 하는 창과 다양 한 주변 장치를 연결 합니다. 

## <a name="attributes"></a>특성
Windows Bluetooth Api에 표시 되는 일반적인 머리글자어는 일반 특성 (GATT). GATT 프로필 데이터의 구조와 작업 두 LE Bluetooth 장치는 통신 모드를 정의 합니다. 특성은 GATT의 기본 빌딩 블록입니다. 특성의 주요 유형은 서비스, 특징 및 설명자입니다. 이러한 특성 관련 부분에의 상호 작용에 더 유용 하므로 클라이언트와 서버 간에 다르게 수행 합니다. 

![공통 프로 파일의 일반적인 특성 계층](images/gatt-service.png)

*심장 박동수 서비스는 GATT 서버 API 형태로 표시*

## <a name="client-and-server"></a>클라이언트와 서버
연결이 설정 된 후 서버 (일반적으로 작은 IoT 센서 또는 착용 가능) 데이터를 포함 하는 장치 라고 합니다. 해당 데이터를 사용 하 여 기능을 수행 하는 장치를 클라이언트 라고 합니다. 예를 들어, Windows PC (클라이언트)에서 데이터를 읽고 심 박수 모니터 (서버)를 추적 하는 사용자가 작업 최적으로. 자세한 내용은 [GATT 클라이언트](gatt-client.md) 와 [GATT 서버](gatt-server.md) 항목을 참조 하십시오.

## <a name="watchers-and-publishers-beacons"></a>감시자와 게시자 (비콘)
중앙 및 주변 장치 역할 뿐만 아니라 방송국 및 관찰자 역할이 있습니다. 방송국은 일반적으로 탐지 장치 라고, 안 통신 GATT를 통해 통신에 대 한 알림 패킷에 제한 된 공간을 사용 하기 때문에. 마찬가지로, 관찰자 데이터에 연결할 필요는 없습니다, 그리고 근처 광고를 검사 합니다. 근처의 광고를 관찰 하는 Windows를 구성 하려면 [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) 클래스를 사용 합니다. 비콘 페이로드를 방송 하기 위해 [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) 클래스를 사용 합니다. 자세한 내용은 [알림](ble-beacon.md) 항목을 참조 하십시오.

## <a name="see-also"></a>참고 항목
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Bluetooth 코어 사양](https://www.bluetooth.com/specifications/bluetooth-core-specification)