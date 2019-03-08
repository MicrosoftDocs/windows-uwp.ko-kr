---
title: PointOfService 장치 공유
description: PointOfService 주변 장치를 다른 사용자와 공유
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 53dc22b2aa35b5e69854f6fb489ff6a454c73bf6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618948"
---
# <a name="pointofservice-device-sharing"></a>PointOfService 장치 공유

여러 Pc 전용된 주변 장치가 각 컴퓨터에 연결 하지 않고 공유 주변 장치에 의존 하는 위치를 환경의 다른 컴퓨터와 네트워크 또는 연결 하는 Bluetooth 주변 장치를 공유 하는 방법에 알아봅니다.

## <a name="device-sharing"></a>공유 장치

네트워크 및 연결 된 PointOfService 주변 장치는 일반적으로 사용 되는 Bluetooth는 환경 wheere에서 여러 클라이언트 장치 하루 종일 같은 주변 장치를 공유 합니다.  사용량이 많은 일반 정품 또는 식품 서비스 환경에서 지연 주변 장치에 연결할 클라이언트 장치에 대 한 기능에 영향을 줍니다 효율성 연결을 닫습니다 고객 트랜잭션 수 및 다음 단계로 넘어갑니다. 여기서 수신 프린터도는 주방 프린터 준비를 위해 주방에 고객의 주문의 세부 정보를 전송할 빠른 서비스 식당 시나리오에 고객의 주문을 수행 하는 여러 클라이언트 장치 생깁니다.  순서 완료 되 면 각 클라이언트 장치에서 공유 프린터를 클레임 주방 순서를 즉시 인쇄 하 수 있어야 합니다.

이러한 환경에서는 응용 프로그램에 대 한 중요 한 완전히 **dispose** 장치 개체를 다른 동일한 장치에 클레임 할 수 있습니다.

'Using' 블록의 끝에 PosPrinter 삭제

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


Dispose ()를 명시적으로 호출 하 여는 PosPrinter 삭제

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>API 메서드 사용 

+ [BarcodeScanner.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]
