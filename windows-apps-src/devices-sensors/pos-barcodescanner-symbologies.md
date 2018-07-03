---
author: TerryWarwick
title: 바코드 스캐너 기호 작업
description: 이 문서에는 바코드 스캐너 기호에 대한 정보가 나와 있습니다.
ms.author: jken
ms.date: 05/3/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: f6e03d62316a1b842330f39ac958e4471a895815
ms.sourcegitcommit: dc3389ef2e2c94b324872a086877314d6f963358
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/11/2018
ms.locfileid: "1874531"
---
# <a name="working-with-symbologies"></a>기호 처리
[바코드 기호](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)는 특정한 바코드 형식으로 데이터를 매핑한 것입니다. 일반적인 기호로는 UPC, Code 128, QR Code 등이 있습니다. UWP 바코드 스캐너 API를 사용하면 응용 프로그램이 스캐너를 수동으로 구성하지 않고도 스캐너가 이러한 기호를 처리하는 방법을 제어할 수 있습니다. 

## <a name="determine-which-symbologies-are-supported"></a>지원되는 기호 확인 
여러 제조업체의 서로 다른 바코드 스캐너 모델에서 응용 프로그램이 사용될 수 있기 때문에 지원되는 기호 목록을 결정하기 위해 스캐너를 쿼리하고 싶을 것입니다.  응용 프로그램에서 일부 스캐너에서 지원되지 않는 특정 기호가 필요하거나 스캐너에서 수동 또는 프로그래밍 방식으로 비활성화를 한 기호를 활성화해야 하는 경우에 유용할 수 있습니다.
[BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync)를 사용하여 [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) 개체를 가져오고 나면 [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync)를 호출하여 장치에서 지원되는 기호 목록을 확보합니다.

## <a name="determine-if-a-specific-symbology-is-supported"></a>특정 기호가 지원되는지 여부를 확인
스캐너에서 특정 기호가 지원되는지 여부를 확인하기 위해 [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)를 호출할 수 있음

## <a name="changing-which-symbologies-are-recognized"></a>인식된 기호 변경
경우에 따라 바코드 스캐너가 지원하는 기호의 하위 집합을 사용하고 싶을 수 있습니다.  이는 응용 프로그램에서 사용할 목적이 아니었던 기호를 차단하는 데 특히 유용합니다. 예를 들어, 사용자가 오른쪽 바코드를 스캔하도록 항목 SKU를 얻을 때 UPC 또는 EAN으로 스캔을 제한하고, 일련 번호를 얻을 때 코드 128로 스캔을 제한할 수 있습니다.
일단 스캐너에서 지원되는 기호를 파악하면 인식하기를 원하는 기호를 설정할 수 있습니다.  [ClaimScannerAsyc](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync)를 이용하여 [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 개체를 설정한 후에 이것이 가능합니다. [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__)를 호출하여 목록에서 누락된 기호는 비활성화하고 특정 기호 집합은 활성화할 수 있습니다.

## <a name="restricting-scan-data-by-data-length"></a>데이터 길이로 스캔 데이터를 제한
일부 코드는 코드 39나 코드 128 같이 가변 길이입니다.  이 기호의 바코드는 서로 가까이에 위치하고 특정 길이의 서로 다른 데이터를 포함할 수 있습니다. 필요한 데이터의 특정 길이를 설정하면 잘못된 스캔을 방지할 수 있습니다.

| 방법    | 설명 |
| :-------- | :---------- |
| [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) | 디코딩된 데이터의 원하는 길이 범위와 스캐너가 확인 숫자를 처리하는 방법을 구성할 수 있도록 해줍니다. |
| [GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) | 현재 길이를 검색하고 숫자 구성을 확인할 수 있도록 해줍니다. |

> [!Important] 
> 다음 속성을 먼저 확인하여 바코드 스캐너에서 기호 특성 사용이 지원되는지 확인합니다. 
> - [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)
> - [ICheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitTransmissionSupported)
> - [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitValidationSupported)
