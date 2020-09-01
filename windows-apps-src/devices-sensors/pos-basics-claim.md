---
title: PointOfService 장치 클레임 및 모델 사용
description: 서비스 지점 장치 클레임을 사용 하 고 Api를 사용 하 여 장치를 클레임 하 고 i/o 작업에 사용 하도록 설정 합니다.
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 391acd1f4acf0620a87e77f3c540c3dd0faa6def
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165407"
---
# <a name="point-of-service-device-claim-and-enable-model"></a>서비스 지점 장치 클레임 및 모델 사용

## <a name="claiming-for-exclusive-use"></a>배타적 사용에 대 한 클레임

PointOfService 장치 개체를 성공적으로 만든 후에는 장치 유형에 대 한 적절 한 클레임 방법을 사용 하 여 입력 또는 출력에 장치를 사용 하도록 요청 해야 합니다.  클레임은 한 응용 프로그램이 다른 응용 프로그램에서 장치를 사용 하는 것을 방해 하지 않도록 응용 프로그램에 많은 장치 기능에 대 한 단독 액세스 권한을 부여 합니다.  한 번에 하나의 응용 프로그램만 독점 사용을 위해 PointOfService 장치를 요청할 수 있습니다. 

> [!Note]
> 클레임 작업은 장치에 대 한 배타적 잠금을 설정 하지만 작동 상태로 전환 하지 않습니다.  자세한 내용은 [장치에서 i/o 작업을 사용 하도록 설정](#enable-device-for-io-operations) 을 참조 하세요.

### <a name="apis-used-to-claim--release"></a>클레임/릴리스에 사용 되는 Api

|디바이스|클레임 | Release | 
|-|:-|:-|
|BarcodeScanner | [ClaimScannerAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | [ClaimedBarcodeScanner](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.close) |
|CashDrawer | [CashDrawer. ClaimDrawerAsync](/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | [ClaimedCashDrawer](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.close) | 
|LineDisplay | [LineDisplay. ClaimAsync](/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |  [ClaimedineDisplay](/uwp/api/windows.devices.pointofservice.claimedlinedisplay.close) | 
|MagneticStripeReader | [MagneticStripeReader](/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) |  [ClaimedMagneticStripeReader](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.close) | 
|PosPrinter | [PosPrinter 비동기](/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) |  [ClaimedPosPrinter](/uwp/api/windows.devices.pointofservice.claimedposprinter.close) | 
 | 

## <a name="enable-device-for-io-operations"></a>I/o 작업에 장치 사용

클레임 작업은 단순히 장치에 대 한 단독 권한만 설정 하지만 작동 상태로 전환 하지는 않습니다.  이벤트를 수신 하거나 출력 작업을 수행 하려면 **Enableasync**를 사용 하 여 장치를 사용 하도록 설정 해야 합니다.  반대로, **Disableasync** 를 호출 하 여 장치에서 이벤트 수신 대기를 중지 하거나 출력을 수행할 수 있습니다.  **IsEnabled** 를 사용 하 여 장치의 상태를 확인할 수도 있습니다.

### <a name="apis-used-enable--disable"></a>사용/사용 안 함 Api 사용

| 디바이스 | 사용 | 사용 안 함 | IsEnabled? |
|-|:-|:-|:-|
|ClaimedBarcodeScanner | [EnableAsync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.enableasync) | [DisableAsync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.disableasync) | [IsEnabled](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isenabled) | 
|ClaimedCashDrawer | [EnableAsync](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.enableasync) | [DisableAsync](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.disableasync) | [IsEnabled](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.isenabled) |
|ClaimedLineDisplay | 해당 없음 ¹ | 해당 없음 ¹ | 해당 없음 ¹ | 
|ClaimedMagneticStripeReader | [EnableAsync](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.enableasync) | [DisableAsync](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.disableasync) | [IsEnabled](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.isenabled) |  
|ClaimedPosPrinter | [EnableAsync](/uwp/api/windows.devices.pointofservice.claimedposprinter.enableasync) | [DisableAsync](/uwp/api/windows.devices.pointofservice.claimedposprinter.disableasync) | [IsEnabled](/uwp/api/windows.devices.pointofservice.claimedposprinter.isenabled) |
|

¹ 줄 표시에서는 i/o 작업을 위해 장치를 명시적으로 사용 하도록 설정할 필요가 없습니다.  을 사용 하도록 설정 하는 것은 i/o를 수행 하는 PointOfService LineDisplay Api에 의해 자동으로 수행 됩니다.

## <a name="code-sample-claim-and-enable"></a>코드 샘플: 클레임 및 사용

이 샘플에서는 바코드 스캐너 개체를 성공적으로 만든 후 바코드 스캐너 장치를 요청 하는 방법을 보여 줍니다.

```Csharp

    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
    
```

> [!Warning]
> 클레임은 다음과 같은 경우에 손실 될 수 있습니다.
> 1. 다른 앱이 동일한 장치의 클레임을 요청 했지만 앱이 **ReleaseDeviceRequested** 이벤트에 대 한 응답으로 **RetainDevice** 을 실행 하지 않았습니다.  자세한 내용은 아래의 [클레임 협상](#claim-negotiation) 을 참조 하세요.
> 2. 앱이 일시 중단 되었으며,이로 인해 장치 개체가 닫히고 클레임은 더 이상 유효 하지 않습니다. 자세한 내용은 [장치 개체 수명 주기](pos-basics-deviceobject.md#device-object-lifecycle) 를 참조 하세요.


## <a name="claim-negotiation"></a>클레임 협상

Windows는 다중 태스킹 환경 이므로 동일한 컴퓨터의 여러 응용 프로그램이 협조적 방식으로 주변 장치에 액세스 해야 할 수 있습니다.  PointOfService Api는 여러 응용 프로그램이 컴퓨터에 연결 된 주변 장치를 공유할 수 있도록 하는 협상 모델을 제공 합니다.

동일한 컴퓨터의 두 번째 응용 프로그램이 다른 응용 프로그램에서 이미 요청한 PointOfService 주변 장치에 대 한 클레임을 요청 하는 경우 **ReleaseDeviceRequested** 이벤트 알림이 게시 됩니다. 응용 프로그램이 현재 장치를 사용 하 고 있는 경우 클레임의 손실을 방지 하기 위해 활성 클레임을 사용 하는 응용 프로그램은 **RetainDevice** 를 호출 하 여 이벤트 알림에 응답 해야 합니다. 

활성 클레임이 있는 응용 프로그램이 **RetainDevice** 를 사용 하 여 응답 하지 않으면 응용 프로그램이 일시 중단 되었거나 장치가 필요 하지 않은 것으로 간주 되 고 클레임이 해지 되 고 새 응용 프로그램에 제공 됩니다. 

첫 번째 단계는 **RetainDevice**를 사용 하 여 **ReleaseDeviceRequested** 이벤트에 응답 하는 이벤트 처리기를 만드는 것입니다.  

```Csharp
    /// <summary>
    /// Event handler for the ReleaseDeviceRequested event which occurs when 
    /// the claimed barcode scanner receives a Claim request from another application
    /// </summary>
    void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner myScanner)
    {
        // Retain exclusive access to the device
        myScanner.RetainDevice();
    }
```

그런 다음, 요청 된 장치와 연결 된 이벤트 처리기를 등록 합니다.

```Csharp
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // register a release request handler to prevent loss of scanner during active use
            claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();          
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
```



### <a name="apis-used-for-claim-negotiation"></a>클레임 협상에 사용 되는 Api

|요청 받은 장치|릴리스 알림| 장치 보존 |
|-|:-|:-|
|ClaimedBarcodeScanner | [ReleaseDeviceRequested](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.releasedevicerequested) | [RetainDevice](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.retaindevice)
|ClaimedCashDrawer | [ReleaseDeviceRequested](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.releasedevicerequested) | [RetainDevice](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.retaindevice)
|ClaimedLineDisplay | [ReleaseDeviceRequested](/uwp/api/windows.devices.pointofservice.claimedlinedisplay.releasedevicerequested) | [RetainDevice](/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedMagneticStripeReader | [ReleaseDeviceRequested](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.releasedevicerequested) | [RetainDevice](/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedPosPrinter | [ReleaseDeviceRequested](/uwp/api/windows.devices.pointofservice.claimedposprinter.releasedevicerequested) | [RetainDevice](/uwp/api/windows.devices.pointofservice.claimedposprinter.retaindevice)
|