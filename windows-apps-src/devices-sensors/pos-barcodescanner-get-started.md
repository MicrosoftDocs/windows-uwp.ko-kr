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
ms.openlocfilehash: ed0fa79f5bbdfdaf8ca1f3273fa8d741f17efe1d
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1833294"
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

## <a name="working-with-symbologies"></a>기호 처리
[**바코드 기호**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)는 특정한 바코드 형식으로 데이터를 매핑한 것입니다. 일반적인 기호로는 UPC, Code 128, QR Code 등이 있습니다. UWP 바코드 스캐너 API를 사용하면 응용 프로그램이 스캐너를 수동으로 구성하지 않고도 스캐너가 이러한 기호를 처리하는 방법을 제어할 수 있습니다. 

### <a name="determine-which-symbologies-are-supported"></a>지원되는 기호 확인 
여러 제조업체의 서로 다른 바코드 스캐너 모델에서 응용 프로그램이 사용될 수 있기 때문에 지원되는 기호 목록을 결정하기 위해 스캐너를 쿼리하고 싶을 것입니다.  응용 프로그램에서 일부 스캐너에서 지원되지 않는 특정 기호가 필요하거나 스캐너에서 수동 또는 프로그래밍 방식으로 비활성화를 한 기호를 활성화해야 하는 경우에 유용할 수 있습니다.
[**BarcodeScanner.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync)를 사용하여 [**BarcodeScanner**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) 개체를 가져오고 나면 [**GetSupportedSymbologiesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync)를 호출하여 장치에서 지원되는 기호 목록을 확보합니다.

### <a name="determine-if-a-specific-symbology-is-supported"></a>특정 기호가 지원되는지 여부를 확인
스캐너에서 특정 기호가 지원되는지 여부를 확인하기 위해 [**IsSymbologySupportedAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)를 호출이 가능

### <a name="changing-which-symbologies-are-recognized"></a>인식된 기호 변경
경우에 따라 바코드 스캐너가 지원하는 기호의 하위 집합을 사용하고 싶을 수 있습니다.  이는 응용 프로그램에서 사용할 목적이 아니었던 기호를 차단하는 데 특히 유용합니다. 예를 들어, 사용자가 오른쪽 바코드를 스캔하도록 항목 SKU를 얻을 때 UPC 또는 EAN으로 스캔을 제한하고, 일련 번호를 얻을 때 코드 128로 스캔을 제한할 수 있습니다.
일단 스캐너에서 지원되는 기호를 파악하면 인식하기를 원하는 기호를 설정할 수 있습니다.  [**ClaimScannerAsyc**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync)을 이용하여 [**ClaimedBarcodeScanner**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 개체를 설정한 후에 이것이 가능합니다. [**SetActiveSymbologiesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__)를 호출하여 목록에서 누락된 기호는 비활성화하고 특정 기호 집합은 활성화할 수 있습니다.

### <a name="restricting-scan-data-by-data-length"></a>데이터 길이로 스캔 데이터를 제한
일부 코드는 코드 39나 코드 128 같이 가변 길이입니다.  이 기호의 바코드는 서로 가까이에 위치하고 특정 길이의 서로 다른 데이터를 포함할 수 있습니다. 필요한 데이터의 특정 길이를 설정하면 잘못된 스캔을 방지할 수 있습니다.

| 방법    | 설명 |
| :-------- | :---------- |
| [**SetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) | 디코딩된 데이터의 원하는 길이 범위와 스캐너가 확인 숫자를 처리하는 방법을 구성할 수 있도록 해줍니다. |
| [**GetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) | 현재 길이를 검색하고 숫자 구성을 확인할 수 있도록 해줍니다. |

> [!Important] 
> 바코드 스캐너에서 [**SetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) 또는 [**GetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) 같은 속성을 먼저 확인하여 기호 속성 사용이 지원되는지 확인하세요. | 현재 길이를 검색하고 숫자 구성을 확인할 수 있도록 해줍니다. :
> - [**IsDecodeLengthSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)
> - [**ICheckDigitTransmissionSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitTransmissionSupported)
> - [**IsCheckDigitValidationSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitValidationSupported)

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
