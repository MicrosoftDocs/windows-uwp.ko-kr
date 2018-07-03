---
author: TerryWarwick
title: PointOfService 장치 클레임 모델
description: PointOfService 장치 클레임 모델에 대해 알아봅니다.
ms.author: jken
ms.date: 06/4/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 202234530945e55ef9c0d0fb68cf9ca83d2e15c3
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983737"
---
# <a name="point-of-service-device-claim-model"></a>서비스 지점 장치 클레임 모델

## <a name="claiming-a-device-for-exclusive-use"></a>장치에 대한 단독 사용 클레임

성공적으로 PointOfService 장치 개체를 만들었으면 입력 또는 출력에 장치를 사용하기 전에 먼저 장치 유형에 대한 적절한 클레임 메서드를 사용하여 요청해야 합니다.  클레임은 응용 프로그램에 장치의 여러 기능에 대한 독점적인 액세스를 부여하여 한 응용 프로그램이 다른 응용 프로그램의 장치 사용을 방해하지 않도록 합니다.  한 번에 하나의 응용 프로그램만 PointOfService 장치에 대한 단독 사용을 주장할 수 있습니다. 

이 샘플은 성공적으로 바코드 스캐너 개체를 만든 후 바코드 스캐너 장치를 주장하는 방법을 보여 줍니다.

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}
```

> [!Warning]
> 클레임은 다음과 같은 환경에서 실패할 수 있습니다.
> 1. 다른 앱이 동일한 장치에 대한 클레임을 요청하고 앱이 **ReleaseDeviceRequested** 이벤트에 대한 응답으로 **RetainDevice**를 발급하지 않았습니다.  (자세한 내용은 아래 [클레임 협상](#Claim-negotiation) 섹션을 참조하세요.)
> 2. 앱이 일시 중단되어 장치 개체가 종료되고 그 결과로 클레임이 더 이상 유효하지 않습니다. (자세한 내용은 [장치 개체 수명 주기](pos-basics-deviceobject.md#device-object-lifecycle)를 참조하세요.)

### <a name="apis-used-for-claiming"></a>클레임에 사용되는 API

|장치|클레임 |
|-|:-|
|BarcodeScanner | [ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | 
|CashDrawer | [ClaimDrawerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | 
|LineDisplay | [ClaimAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |
|MagneticStripeReader | [ClaimReaderAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) | 
|PosPrinter | [ClaimPrinterAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) | 
|

## <a name="claim-negotiation"></a>클레임 협상

Windows는 멀티태스킹 환경이기 때문에 동일한 컴퓨터의 여러 응용 프로그램이 협력하는 방식으로 주변 장치에 액세스해야 합니다.  PointOfService API는 여러 응용 프로그램이 컴퓨터에 연결된 주변 장치를 공유할 수 있도록 협상 모델을 제공합니다.

동일한 컴퓨터의 두 번째 응용 프로그램이 이미 다른 응용 프로그램에서 요청된 PointOfService 주변 장치에 대한 클레임을 요청하면 **ReleaseDeviceRequested** 이벤트 알림이 게시됩니다. 활성 클레임이 있는 응용 프로그램이 장치를 사용하고 있는 경우 현재 클레임을 잃지 않기 위해 **RetainDevice**를 호출하여 이벤트 알림에 응답해야 합니다. 

활성 클레임이 있는 응용 프로그램이 즉시 **RetainDevice**로 응답하지 않는 경우 해당 응용 프로그램이 일시 중단되었거나 장치가 필요하지 않은 것으로 간주되며 클레임이 취소되고 새 응용 프로그램에 제공됩니다. 

이 예는 다른 앱이 디바이스를 요청했을 때 클레임한 바코드 스캐너를 유지하는 방법을 보여 줍니다.  

```Csharp
claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner myScanner)
{
    // Retain exclusive access to the device
    myScanner.RetainDevice();  
}
```
### <a name="apis-used-for-claim-negotiation"></a>클레임 협상에 사용되는 API

|클레임한 장치|릴리스 알림| 장치 보유 |
|-|:-|:-|
|ClaimedBarcodeScanner | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.retaindevice)
|ClaimedCashDrawer | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.retaindevice)
|ClaimedLineDisplay | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedMagneticStripeReader | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedPosPrinter | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.retaindevice)
|
