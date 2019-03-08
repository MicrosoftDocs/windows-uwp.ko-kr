---
title: 소프트웨어 트리거 사용
description: 소프트웨어에서 검색 작업을 제어 하는 방법에 알아봅니다.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 2b6f06ea66767a1bcdd7e20fa05aa7af275eb892
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645528"
---
# <a name="use-a-software-trigger"></a>소프트웨어 트리거 사용

프레젠테이션 모드에서 바코드 스캐너를 사용 중인 경우나 스캐너에 카메라 기반 바코드 스캐너 같은 물리적 트리거가 없는 경우에 소프트웨어에서의 스캔 작업을 제어하는 데 유용할 수 있습니다. 호출 하 여 검색 프로세스를 시작할 수 있습니다 [StartSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync)합니다.

값에 따라 [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 스캐너 있습니다 하나만 바코드를 스캔을 중지 하거나 호출할 때까지 지속적으로 스캔 [StopSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync)합니다.

원하는 값을 설정 [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 바코드를 디코딩할 경우 스캐너 동작을 제어할 수 있습니다.

| 값 | 설명 |
| ----- | ----------- |
| True   | 오직 하나의 바코드만 스캔하고 중지 |
| False  | 중단 없이 지속적으로 바코드 스캔 |


> [!Important]
> 바코드 스캐너가 첫 번째 속성을 확인 하 여 소프트웨어 트리거 사용을 지원 하는지 확인 [IsSoftwareTriggerSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported)합니다.

다음 예제에서는 스캔 한 바코드를 검사 하 되 면 중지 됩니다 소프트웨어 트리거를 사용 하 여 검색을 시작 하는 방법을 보여 줍니다.

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