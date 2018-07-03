---
author: TerryWarwick
title: 바코드 스캐너 시작하기
description: 유니버설 Windows 응용 프로그램에서 바코드 스캐너와 상호 작용하는 방법에 대한 자세한 내용
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 0fddfa3aa78c274735634315b1230b0893020805
ms.sourcegitcommit: dc3389ef2e2c94b324872a086877314d6f963358
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/11/2018
ms.locfileid: "1874201"
---
# <a name="getting-started-with-barcode-scanners"></a>바코드 스캐너(들) 시작하기

유니버설 Windows 응용 프로그램에서 바코드 스캐너와 상호 작용하는 방법을 자세히 살펴보세요.  이 항목에서는 바코드 스캐너 고유의 기능에 대한 정보를 제공합니다.

## <a name="configuring-your-barcode-scanner"></a>바코드 스캐너 구성
바코드 스캐너는 몇 가지 모드로 구성이 가능합니다.  바코드 스캐너를 원하는 응용 프로그램에 맞게 올바르게 구성하는 것이 중요합니다.  바코드 스캐너가 Windows에 키보드로 나타나도록 해주는 **키보드 웨지** 모드에서 다양한 바코드 스캐너를 구성할 수 있습니다.  따라서 메모장 같이 바코드 스캐너를 인식하지 못하는 응용 프로그램에 대해서도 바코드를 스캔할 수 있습니다.  이 모드에서 바코드를 스캔할 때 키보드를 사용하여 데이터를 입력하는 것처럼 바코드 스캐너에서 디코딩된 데이터가 삽입 지점에 삽입됩니다.  UWP 응용 프로그램에서 바코드 스캐너를 더 많이 제어하고 싶은 경우에는 키보드가 아닌 웨지 모드로 이를 구성해야 합니다.

### <a name="usb-barcode-scanner"></a>USB 바코드 스캐너
USB에 연결된 바코드 스캐너는 Windows에 포함된 바코드 스캐너 드라이버에서 작동하도록 **HID POS 스캐너** 모드로 구성해야 합니다. 이 드라이버는 [**USB-HID**](http://www.usb.org/developers/hidpage/)에 게시된 **HID POS(Point of Sale) 사용 표** 사양을 구현합니다.  바코드 스캐너 설명서를 참조하거나 바코드 스캐너 제조업체에게 **HID POS 스캐너** 모드를 활성화하기 위한 지침을 문의하세요.  **HID POS 스캐너**로 구성이 된 바코드 스캐너는 **POS 바코드 스캐너** 노드 아래의 장치 관리자에 **POS HID 바코드 스캐너**로 표시됩니다.
바코드 스캐너 제조업체는 **HID POS 스캐너** 이외의 모드를 사용하여 UWP 바코드 스캐너 API를 지원하는 공급업체별 드라이버도 가지고 있을 수 있습니다.  UWP 바코드 스캐너 API와 호환되는 제조업체 제공 드라이버를 이미 설치한 경우에는 장치 관리자의 **POS 바코드 스캐너** 아래에 나열되어 있는 공급업체별 장치를 참조할 수 있습니다.

### <a name="bluetooth-barcode-scanner"></a>Bluetooth 바코드 스캐너
Bluetooth에 연결된 바코드 스캐너는 UWP 바코드 스캐너 API에서 작동할 수 있도록 **직렬 포트 프로토콜 - 간단한 직렬 인터페이스(SPP-SSI)** 모드로 구성해야 합니다.o work with the UWP Barcode Scanner APIs.  바코드 스매너 설명서를 참조하거나 바코드 스캐너 제조업체에게 **SPP-SSI 모드**를 활성화하기 위한 지침을 문의하세요.  
Bluetooth 바코드 스캐너를 사용할 수 있으려면 먼저 설정 - 장치 - Bluetooth 및 기타 장치 - 다른 장치에 Bluetooth 추가를 사용하여 페어링을 해야 합니다.  
**Windows.Devices.Enumeration** 네임스페이스를 사용하여 페어링 공식 절차를 시작하고 제어할 수 있습니다.  자세한 내용은 [**장치 페어링**](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices)을 참조하세요.

## <a name="using-software-trigger-with-barcode-scanners"></a>바코드 스캐너에서 소프트웨어 트리거 사용
### <a name="initiate-scan-from-software"></a>소프트웨어에서 스캔 시작
프레젠테이션 모드에서 바코드 스캐너를 사용 중인 경우나 스캐너에 카메라 기반 바코드 스캐너 같은 물리적 트리거가 없는 경우에 소프트웨어에서의 스캔 작업을 제어하는 데 유용할 수 있습니다. [**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync)을 호출하여 스캔 프로세스를 시작할 수 있습니다.  
[**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 값에 따라 스캐너는 오직 하나의 바코드만 스캔한 다음 중지하거나 [**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync)가 호출될 때까지 계속해서 스캔을 할 수 있습니다.

바코드가 디코딩될 때 스캐너 동작을 제어하도록 [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived)를 원하는 값으로 설정합니다.

| 값 | 설명 |
| ----- | ----------- |
| True   | 오직 하나의 바코드만 스캔하고 중지 |
| False  | 중단 없이 지속적으로 바코드 스캔 |


> [!Important]
> [**IsSoftwareTriggerSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported)라는 속성을 먼저 확인하여 바코드 스캐너에서 소프트웨어 트리거 사용이 지원되는지 확인합니다.
