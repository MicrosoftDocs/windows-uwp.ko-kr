---
title: PointOfService 장치 개체
description: PointOfService 장치 개체를 만들고 UWP (유니버설 Windows 플랫폼) 응용 프로그램 모델에서 장치 개체 수명 주기에 대해 알아보는 방법에 대해 알아봅니다.
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 4c9a5008756831eed9819a3b323d167dcc4b2744
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043405"
---
# <a name="pointofservice-device-objects"></a>PointOfService 장치 개체

## <a name="creating-a-device-object"></a>디바이스 개체 만들기

새 열거 또는 저장 된 DeviceID에서 사용 하려는 PointOfService 장치를 식별 한 후에는 프로그래밍 방식으로 선택한[**deviceid**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 로 [**FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync) 를 호출 하거나 사용자가 새 서비스 장치 개체를 만들도록 선택 했습니다.

이 샘플은 DeviceID를 사용 하 여 FromIdAsync를 사용 하 여 새 바코드 스캐너 개체를 만들려고 시도 합니다. 개체를 만드는 동안 오류가 발생 하는 경우 디버그 메시지가 기록 됩니다.

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

장치 개체를 만들었으면 장치의 메서드, 속성 및 이벤트에 액세스할 수 있습니다.  

## <a name="device-object-lifecycle"></a>장치 개체 수명 주기

Windows 8 이전에는 앱이 간단한 수명 주기를가지고 있습니다. Win32 및 .NET 앱이 실행 되 고 있거나 실행 되 고 있지 않으며, 일반적으로 전체 앱 수명 주기에 대 한 서비스 주변 기기가 요청 되었습니다. 사용자가이를 최소화 하거나 다른 위치로 전환 하면 계속 실행 됩니다. 이는 휴대용 장치 및 전원 관리가 점점 더 중요할 때까지 문제가 되지 않습니다.

Windows 8에는 UWP 앱과 함께 새로운 응용 프로그램 모델이 도입 되었습니다. 높은 수준에서 새 일시 중단 됨 상태가 추가 되었습니다. 사용자가 최소화 하거나 다른 앱으로 전환 하면 UWP 앱이 즉시 일시 중단 됩니다. 즉, 응용 프로그램의 스레드가 중지 되 고, 운영 체제에서 리소스를 회수 해야 하는 경우가 아니면 응용 프로그램은 메모리에 남아 있으며, 다른 응용 프로그램에서 주변 장치에 액세스할 수 있도록, PointOfService 주변 장치를 나타내는 모든 장치 개체가 자동으로 닫힙니다. 사용자가 다시 앱으로 전환 하는 경우 다시 시작할 때 계속 제공 되는 경우 실행 중 상태로 신속 하 게 복원 하 고, 서비스 주변 장치 연결을 복원할 수 있습니다.

를 사용 하 여 어떤 이유로 든 개체가 닫히는 시기를 감지할 수 있습니다 \<DeviceObject\> . 닫힌 이벤트 처리기는 나중에 연결을 다시 설정 하기 위한 장치 ID를 기록 합니다.   또는 앱 다시 시작 알림에서 장치 연결을 다시 설정 하기 위한 장치 ID를 저장 하기 위해 앱 일시 중단 알림에서이를 처리할 수 있습니다.  이벤트 처리기에 대 한 두 가지 작업을 수행 하지 않아야 \<DeviceObject\> 합니다. 닫힘 및 응용 프로그램 일시 중단.

> [!TIP]
> UWP (Windows 10 유니버설 Windows 플랫폼) 응용 프로그램 수명 주기에 대 한 자세한 내용은 다음 항목을 참조 하십시오.
> - [UWP (Windows 10 유니버설 Windows 플랫폼) 앱 수명 주기](../launch-resume/app-lifecycle.md)
> - [앱 일시 중단 처리](../launch-resume/suspend-an-app.md)
> - [앱 다시 시작 처리](../launch-resume/resume-an-app.md)
