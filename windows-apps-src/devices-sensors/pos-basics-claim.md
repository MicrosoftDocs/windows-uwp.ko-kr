---
author: TerryWarwick
title: PointOfService 장치 클레임 및 모델을 사용 하도록 설정
description: PointOfService 클레임에 알아보고 모델을 사용 하도록 설정
ms.author: jken
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: df9c4764b8f7d752a132d6759054660f481cce55
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7429243"
---
# <a name="point-of-service-device-claim-and-enable-model"></a>서비스 지점 장치 클레임 및 모델을 사용 하도록 설정

## <a name="claiming-for-exclusive-use"></a>단독 사용 클레임

성공적으로 PointOfService 장치 개체를 만들었으면 입력 또는 출력에 장치를 사용하기 전에 먼저 장치 유형에 대한 적절한 클레임 메서드를 사용하여 요청해야 합니다.  클레임은 응용 프로그램에 장치의 여러 기능에 대한 독점적인 액세스를 부여하여 한 응용 프로그램이 다른 응용 프로그램의 장치 사용을 방해하지 않도록 합니다.  한 번에 하나의 응용 프로그램만 PointOfService 장치에 대한 단독 사용을 주장할 수 있습니다. 

> [!Note]
> 클레임 작업 장치에 대 한 단독 잠금을 설정 하지만 작동 상태에 배치 하지 않습니다.  자세한 내용은 [I/O 작업에 대 한 장치 사용](#Enable-device-for-I/O-operations) 을 참조 하세요.

### <a name="apis-used-to-claim--release"></a>클레임 / 해제 Api를 사용

|장치|클레임 | 릴리스 | 
|-|:-|:-|
|BarcodeScanner | [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | [ClaimedBarcodeScanner.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.close) |
|CashDrawer | [CashDrawer.ClaimDrawerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | [ClaimedCashDrawer.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.close) | 
|LineDisplay | [LineDisplay.ClaimAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |  [ClaimedineDisplay.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.close) | 
|MagneticStripeReader | [MagneticStripeReader.ClaimReaderAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) |  [ClaimedMagneticStripeReader.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.close) | 
|PosPrinter | [PosPrinter.ClaimPrinterAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) |  [ClaimedPosPrinter.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.close) | 
 | 

## <a name="enable-device-for-io-operations"></a>I/O 작업에 대 한 장치를 사용 하도록 설정

클레임 작업 설정 된 독점 권한을 장치에 작동 상태에 배치 하지 않습니다.  이벤트를 수신 하거나 모든 출력 작업을 수행 하려면 **EnableAsync**를 사용 하 여 장치를 활성화 해야 합니다.  반대로, 장치 또는 수행 출력에서 이벤트를 수신 대기를 중지 **DisableAsync** 호출할 수 있습니다.  디바이스의 상태를 확인 하려면 **IsEnabled** 를 사용할 수도 있습니다.

### <a name="apis-used-enable--disable"></a>사용 되는 Api 사용/사용 안 함

| 장치 | 사용 | 사용 안 함 | IsEnabled? |
|-|:-|:-|:-|
|ClaimedBarcodeScanner | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isenabled) | 
|ClaimedCashDrawer | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.isenabled) |
|ClaimedLineDisplay | 하지 Applicable¹ | 하지 Applicable¹ | 하지 Applicable¹ | 
|ClaimedMagneticStripeReader | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.isenabled) |  
|ClaimedPosPrinter | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.disableasyc) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.isenabled) |
|

¹ 라인 디스플레이 I/O 작업에 대 한 장치를 명시적으로 사용 하도록 설정 하 필요 하지 않습니다.  I/O를 수행 하는 PointOfService LineDisplay Api를 통해 사용 하면 자동으로 수행 됩니다.

## <a name="code-sample-claim-and-enable"></a>코드 샘플: 클레임 및 사용

이 샘플은 성공적으로 바코드 스캐너 개체를 만든 후 바코드 스캐너 장치를 주장하는 방법을 보여 줍니다.

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
> 클레임은 다음과 같은 환경에서 실패할 수 있습니다.
> 1. 다른 앱이 동일한 장치에 대한 클레임을 요청하고 앱이 **ReleaseDeviceRequested** 이벤트에 대한 응답으로 **RetainDevice**를 발급하지 않았습니다.  (자세한 내용은 아래 [클레임 협상](#Claim-negotiation) 섹션을 참조하세요.)
> 2. 앱이 일시 중단되어 장치 개체가 종료되고 그 결과로 클레임이 더 이상 유효하지 않습니다. (자세한 내용은 [장치 개체 수명 주기](pos-basics-deviceobject.md#device-object-lifecycle)를 참조하세요.)


## <a name="claim-negotiation"></a>클레임 협상

Windows는 멀티태스킹 환경이기 때문에 동일한 컴퓨터의 여러 응용 프로그램이 협력하는 방식으로 주변 장치에 액세스해야 합니다.  PointOfService API는 여러 응용 프로그램이 컴퓨터에 연결된 주변 장치를 공유할 수 있도록 협상 모델을 제공합니다.

동일한 컴퓨터의 두 번째 응용 프로그램이 이미 다른 응용 프로그램에서 요청된 PointOfService 주변 장치에 대한 클레임을 요청하면 **ReleaseDeviceRequested** 이벤트 알림이 게시됩니다. 활성 클레임이 있는 응용 프로그램이 장치를 사용하고 있는 경우 현재 클레임을 잃지 않기 위해 **RetainDevice**를 호출하여 이벤트 알림에 응답해야 합니다. 

활성 클레임이 있는 응용 프로그램이 즉시 **RetainDevice**로 응답하지 않는 경우 해당 응용 프로그램이 일시 중단되었거나 장치가 필요하지 않은 것으로 간주되며 클레임이 취소되고 새 응용 프로그램에 제공됩니다. 

첫 번째 단계와 **RetainDevice** **ReleaseDeviceRequested** 이벤트에 응답 하는 이벤트 처리기를 만드는 것입니다.  

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

그런 다음 클레임 장치와 관련에서 이벤트 처리기를 등록

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



### <a name="apis-used-for-claim-negotiation"></a>클레임 협상에 사용되는 API

|클레임한 장치|릴리스 알림| 장치 보유 |
|-|:-|:-|
|ClaimedBarcodeScanner | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.retaindevice)
|ClaimedCashDrawer | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.retaindevice)
|ClaimedLineDisplay | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedMagneticStripeReader | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedPosPrinter | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.retaindevice)
|
