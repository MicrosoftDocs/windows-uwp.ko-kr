---
title: 소프트웨어 트리거 사용
description: 비동기 소프트웨어 트리거를 사용 하 여 프로그래밍 방식으로 바코드 스캔 프로세스를 제어 하는 방법을 알아봅니다.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: baa556958750b4f88997746786a9b5fe92e601b4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168517"
---
# <a name="use-a-software-trigger"></a>소프트웨어 트리거 사용

프레젠테이션 모드에서 바코드 스캐너를 사용 하는 경우 또는 스캐너에 카메라 기반 바코드 스캐너와 같은 실제 트리거가 없는 경우 소프트웨어 스캔 작업을 제어 하는 것이 유용할 수 있습니다. [StartSoftwareTriggerAsync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync)를 호출 하 여 검색 프로세스를 시작할 수 있습니다.

[IsDisabledOnDataReceived](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 의 값에 따라 스캐너가 하나의 바코드를 검색 한 후 [StopSoftwareTriggerAsync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync)를 호출할 때까지 계속 해 서 중지 하거나 검색할 수 있습니다.

바코드가 디코딩되는 경우 스캐너 동작을 제어 하려면 [IsDisabledOnDataReceived](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 의 원하는 값을 설정 합니다.

| 값 | Description |
| ----- | ----------- |
| True   | 하나의 바코드만 검색 한 후 중지 |
| False  | 중지 하지 않고 계속 해 서 바코드 스캔 |


> [!Important]
> 먼저 [IsSoftwareTriggerSupported](/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported)속성을 확인 하 여 바코드 스캐너가 소프트웨어 트리거 사용을 지원 하는지 확인 합니다.

다음 예제에서는 하나의 바코드를 검사 한 후 검색을 중지 하는 소프트웨어 트리거를 사용 하 여 검색을 시작 하는 방법을 보여 줍니다.

```cs
private void SoftwareTrigger(BarcodeScanner barcodeScanner, ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    if (barcodeScanner.Capabilities.IsSoftwareTriggerSupported)
    {
        claimedBarcodeScanner.IsDisabledOnDataReceived = true;
        await claimedBarcodeScanner.StartSoftwareTriggerAsync();
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]