---
author: TerryWarwick
title: PointOfService 장치 공유
description: PointOfService 주변 장치를 다른 사용자와 공유
ms.author: jken
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 591ba592d1c17b03ae29c6fb98ef546bc18b8bdc
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6268858"
---
# <a name="pointofservice-device-sharing"></a>PointOfService 장치 공유

각 컴퓨터에 연결 된 전용된 주변 아닌 공유 주변 장치에 여러 Pc 의존할 환경에서 다른 컴퓨터와 네트워크 또는 Bluetooth로 연결 된 주변 장치를 공유 하는 방법을 알아봅니다.

## <a name="device-sharing"></a>장치 공유

네트워크 및 Bluetooth 연결 된 PointOfService 주변 장치는 일반적으로 사용 환경 wheere 여러 클라이언트 장치 하루 종일 동일한 주변 장치를 공유 하 고 있습니다.  사용 중인 소매 또는 음식 서비스 환경에서 주변 장치에 연결 하는 클라이언트 장치에 대 한 수의 지연에 영향이 효율성 연결 고객과 거래를 닫을 수 및 다음 단계로 넘어갑니다. 준비 주방으로 고객의 주문 세부 정보를 전송 하는 영수증 프린터 주방 프린터로 사용 위치 빠른 서비스 식당 시나리오에서에 여러 클라이언트 장치에서 고객 주문 라인 생깁니다.  순서가 완료 되 면 각 클라이언트 장치 공유 프린터와 주방 순서를 즉시 인쇄할 수 있어야 합니다.

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
