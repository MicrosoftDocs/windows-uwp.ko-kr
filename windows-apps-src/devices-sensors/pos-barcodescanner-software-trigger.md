---
author: eliotcowley
title: 소프트웨어 트리거 사용
description: 소프트웨어에서의 스캔 작업을 제어 하는 방법을 알아봅니다.
ms.author: elcowle
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 03fbed5a0145a093b1e7a2535012077644aaf2e2
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6184381"
---
# <a name="use-a-software-trigger"></a>소프트웨어 트리거 사용

프레젠테이션 모드에서 바코드 스캐너를 사용 중인 경우나 스캐너에 카메라 기반 바코드 스캐너 같은 물리적 트리거가 없는 경우에 소프트웨어에서의 스캔 작업을 제어하는 데 유용할 수 있습니다. [StartSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync)을 호출하여 스캔 프로세스를 시작할 수 있습니다.

[IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 값에 따라 스캐너는 오직 하나의 바코드만 스캔한 다음 중지하거나 [StopSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync)가 호출될 때까지 계속해서 스캔을 할 수 있습니다.

바코드가 디코딩될 때 스캐너 동작을 제어하도록 [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived)를 원하는 값으로 설정합니다.

| 값 | 설명 |
| ----- | ----------- |
| True   | 오직 하나의 바코드만 스캔하고 중지 |
| False  | 중단 없이 지속적으로 바코드 스캔 |


> [!Important]
> [IsSoftwareTriggerSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported)라는 속성을 먼저 확인하여 바코드 스캐너에서 소프트웨어 트리거 사용이 지원되는지 확인합니다.

다음 예제에서는 하나의 바코드 스캔 되 면 스캔을 중지 하는 소프트웨어 트리거를 사용 하 여 검사를 시작 하는 방법을 보여 줍니다.

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