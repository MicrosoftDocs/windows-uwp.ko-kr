---
title: PointOfService 장치 공유
description: PointOfService 주변 장치를 다른 사용자와 공유
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 53dc22b2aa35b5e69854f6fb489ff6a454c73bf6
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8827218"
---
# <a name="pointofservice-device-sharing"></a>PointOfService 장치 공유

각 컴퓨터에 연결 된 전용된 주변 아닌 공유 주변 장치에 여러 Pc 의존할 환경에서 다른 컴퓨터와 네트워크 또는 Bluetooth로 연결 된 주변 장치를 공유 하는 방법을 알아봅니다.

## <a name="device-sharing"></a>장치 공유

네트워크 및 Bluetooth 연결 된 PointOfService 주변 장치는 일반적으로 사용 되는 환경 wheere 여러 클라이언트 장치 하루 종일 동일한 주변 장치를 공유 하 고 있습니다.  사용 중인 정품 또는 음식 서비스 환경에서 주변 장치에 연결 하는 클라이언트 장치에 대 한 수의 지연에 영향이 효율성 동료 고객과 거래를 닫을 수 및 다음 단계로 넘어갑니다. 애니메이션이 사용 되는 영수증 프린터는 주방 프린터로 준비 주방으로 고객의 주문 세부 정보를 전송 하는 빠른 서비스 식당 시나리오에 고객의 주문 수행 하는 여러 클라이언트 장치 표시 됩니다.  순서가 완료 되 면 각 클라이언트 장치 공유 프린터와 즉시 주방 순서를 인쇄할 수 있어야 합니다.

이러한 환경에서 것이 중요 응용 프로그램을 완전히 **삭제** 를 장치 개체에 대 한 다른 동일한 장치를 요청할 수 있도록 합니다.

'사용' 블록의 끝에 PosPrinter 삭제

```Csharp 
using Windows.Devices.PointOfService;
using(PosPrinter printer = await PosPrinter.FromIdAsync("Device ID"))
{
    if (printer != null)
    {
        // Exercise the printer.
    }

    // When leaving this scope, printer.Dispose() is automatically invoked, 
    // releasing the session we have with the printer.
}
```


PosPrinter dispose ()를 명시적으로 호출 하 여 삭제

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>사용 되는 API 메서드 

+ [BarcodeScanner.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]
