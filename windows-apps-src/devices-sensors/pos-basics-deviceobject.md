---
author: TerryWarwick
title: PointOfService 장치 개체
description: PointOfService 장치 개체 만들기에 대해 알아보기
ms.author: jken
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 31af943ab4a9231f58fb2e3d5489e9ae80d8d565
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5707364"
---
# <a name="pointofservice-device-objects"></a>PointOfService 장치 개체

## <a name="creating-a-device-object"></a>장치 개체 만들기
새로운 열거 또는 저장된 DeviceID에서 사용할 PointOfService 장치를 식별했으면 [**FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync)를 프로그래밍 방식으로 선택하거나 사용자가 선택한 [**DeviceID**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id)와 함께 호출하여 새 서비스 지점 장치 개체를 만듭니다.

이 샘플은 DeviceID를 사용하여 FromIdAsync로 새 BarcodeScanner 개체 만들기를 시도합니다. 개체 만드는 데 실패한 경우 디버그 메시지가 기록됩니다.

```Csharp

    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use and enable it to exchange data
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
    
```

장치 개체가 있으면 장치의 메서드, 속성 및 이벤트에 액세스할 수 있습니다.  

## <a name="device-object-lifecycle"></a>장치 개체 수명 주기
Windows8 이전에는 앱에 간단한 수명 주기가 있었습니다. Win32 및 .NET 앱은 실행되거나 실행되지 않으며 일반적으로 PointOfService 주변 장치는 전체 앱 수명 주기에 대해 클레임되었습니다. 사용자가 앱을 최소화하거나 벗어날 경우 계속 실행됩니다. 그래도 휴대용 장치와 전원 관리가 점점 중요해질 때까지는 문제가 되지 않았습니다.

Windows 8에서는 UWP 앱을 사용하는 새 응용 프로그램 모델이 도입되었습니다. 상위 수준에서 일시 중단 상태가 새로 추가되었습니다. 사용자가 앱을 최소화하거나 다른 앱으로 전환하면 즉시 UWP 앱이 일시 중단됩니다. 즉, 앱의 스레드가 중지되면 운영 체제가 리소스를 확보해야 하지 않는 이상 앱은 메모리에 남고 PointOfService 주변 장치를 나타내는 모든 장치 개체는 다른 응용 프로그램이 주변 장치에 액세스할 수 있도록 자동으로 닫힙니다. 사용자가 앱으로 전환하면 신속하게 실행 상태로 복원할 수 있으며 다시 시작할 때 계속 사용할 수 있는 경우 PointOfService 주변 장치 연결을 복원합니다.

어떤 이유로든 <DeviceObject>로 개체가 닫힐 때를 감지할 수 있습니다. 그러면 닫힌 이벤트 처리기는 나중에 연결을 다시 설정하기 위해 장치 ID를 기록합니다.   또는 앱 다시 시작 알림 시 장치 연결을 재설정하기 위해 장치 ID를 저장하도록 앱 일시 중단 알림에서 이를 처리하고자 할 수 있습니다.  이벤트 처리기에서 중복 수행하지 않고 닫힌 및 앱 일시 중단<DeviceObject> 모두에서 장치 개체에 대한 작업을 중복으로 수행하지 않도록 하십시오.

> [!TIP]
> Windows 10 유니버설 Windows 플랫폼(UWP) 응용 프로그램 수명 주기에 대한 자세한 내용은 다음 항목을 참조하세요.
> - [Windows 10 UWP(유니버설 Windows 플랫폼) 앱 수명 주기](../launch-resume/app-lifecycle.md)
> - [앱 일시 중단 처리](../launch-resume/suspend-an-app.md)
> - [앱 다시 시작 처리](../launch-resume/resume-an-app.md)
