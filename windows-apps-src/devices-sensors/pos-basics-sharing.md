---
title: PointOfService 장치 공유
description: 여러 Pc가 공유 주변 장치를 사용 하는 환경에서 네트워크 또는 Bluetooth 연결 주변 장치를 다른 컴퓨터와 공유 하는 방법을 알아봅니다.
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 5416628b88a070c7bd4f361f9f438fe690951d34
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054163"
---
# <a name="pointofservice-device-sharing"></a>PointOfService 장치 공유

여러 Pc가 각 컴퓨터에 연결 된 전용 주변 장치가 아닌 공유 주변 장치를 사용 하는 환경에서 네트워크 또는 Bluetooth 연결 주변 장치를 다른 컴퓨터와 공유 하는 방법에 대해 알아봅니다.

## <a name="device-sharing"></a>장치 공유

네트워크 및 Bluetooth 연결 된 PointOfService 주변 장치는 일반적으로 여러 클라이언트 장치가 하루 내내 동일한 주변 장치를 공유 하는 환경에서 사용 wheere.  사용 중인 소매 또는 음식 서비스 환경에서 클라이언트 장치를 주변 장치에 연결 하는 기능을 사용 하는 경우의 모든 지연은 고객이 보유 한 트랜잭션을 종료 하 고 다음으로 이동할 수 있는 효율성에 영향을 줍니다. 고객 주문에 대 한 세부 정보를 준비를 위해 부엌으로 전송 하기 위해 수신 프린터가 부엌 프린터로 사용 되는 신속한 서비스 식당 시나리오에서 고객의 주문을 가져오는 여러 클라이언트 장치가 있습니다.  주문이 완료 되 면 각 클라이언트 장치에서 공유 프린터를 요청 하 고 부엌의 주문을 즉시 인쇄할 수 있어야 합니다.

이러한 환경에서는 다른 장치에서 동일한 장치를 요청할 수 있도록 응용 프로그램이 장치 개체를 완전히 **삭제** 하는 것이 중요 합니다.

' Using ' 블록의 끝에 있는 PosPrinter의 삭제

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


Dispose ()를 명시적으로 호출 하 여 PosPrinter 삭제

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

+ [바 Code스캐너. Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer. Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay. Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]
