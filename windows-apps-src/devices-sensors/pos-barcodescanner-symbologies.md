---
title: 바코드 스캐너 기호 작업
description: 이 문서에는 바코드 스캐너 기호에 대한 정보가 나와 있습니다.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 690b6b8ee688f62dcae375ed48e07797c921bf43
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8781227"
---
# <a name="working-with-symbologies"></a>기호 처리
[바코드 기호](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)는 특정한 바코드 형식으로 데이터를 매핑한 것입니다. 일반적인 기호로 UPC, Code 128, QR 코드 및 등을 포함합니다.  유니버설 Windows 플랫폼 바코드 스캐너 Api는 응용 프로그램이 스캐너를 수동으로 구성 하지 않고도 스캐너가 이러한 기호 처리 하는 방법을 제어할 수 있음 

## <a name="determine-which-symbologies-are-supported"></a>지원되는 기호 확인 
여러 제조업체의 서로 다른 바코드 스캐너 모델에서 응용 프로그램이 사용될 수 있기 때문에 지원되는 기호 목록을 결정하기 위해 스캐너를 쿼리하고 싶을 것입니다.  응용 프로그램에서 일부 스캐너에서 지원되지 않는 특정 기호가 필요하거나 스캐너에서 수동 또는 프로그래밍 방식으로 비활성화를 한 기호를 활성화해야 하는 경우에 유용할 수 있습니다.

[BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync)를 사용하여 [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) 개체를 가져오고 나면 [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync)를 호출하여 장치에서 지원되는 기호 목록을 확보합니다.

다음 예제에서는 바코드 스캐너의 지원 되는 기호 목록을 가져오고 텍스트 블록에 표시 합니다.

```cs
private void DisplaySupportedSymbologies(BarcodeScanner barcodeScanner, TextBlock textBlock) 
{
    var supportedSymbologies = await barcodeScanner.GetSupportedSymbologiesAsync();

    foreach (uint item in supportedSymbologies)
    {
        string symbology = BarcodeSymbologies.GetName(item);
        textBlock.Text += (symbology + "\n");
    }
}
```

## <a name="determine-if-a-specific-symbology-is-supported"></a>특정 기호가 지원되는지 여부를 확인
스캐너가 특정 기호가 지원 하는지 확인 하려면 [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)를 호출할 수 있습니다.

다음 예제에서는 바코드 스캐너 **Code32** 기호를 지원 하는지 확인 합니다.

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>인식 된 기호 변경
경우에 따라 바코드 스캐너가 지원하는 기호의 하위 집합을 사용하고 싶을 수 있습니다.  이는 응용 프로그램에서 사용할 목적이 아니었던 기호를 차단하는 데 특히 유용합니다. 예를 들어, 사용자가 오른쪽 바코드를 스캔하도록 항목 SKU를 얻을 때 UPC 또는 EAN으로 스캔을 제한하고, 일련 번호를 얻을 때 코드 128로 스캔을 제한할 수 있습니다.

일단 스캐너에서 지원되는 기호를 파악하면 인식하기를 원하는 기호를 설정할 수 있습니다.  [ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync)를 사용 하 여 [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 개체를 설정한 후 수행할 수 있습니다. [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__)를 호출하여 목록에서 누락된 기호는 비활성화하고 특정 기호 집합은 활성화할 수 있습니다.

다음 예제에서는 [Code39](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39) 및 [Code39Ex](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex)을 때 클레임 한 바코드 스캐너의 활성 기호를 설정합니다.

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>바코드 기호 체계 특성
서로 다른 바코드 기호 지원 여러 길이, 원시 데이터의 일환으로 전송 하는 호스트에 확인 숫자를 디코드 및 숫자 유효성 검사와 같은 다른 속성이 있을 수 있습니다. [BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes) 클래스를 사용 하 여 가져올 특정된 [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 및 바코드 기호 체계에 대 한 이러한 특성을 설정 합니다.

[GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_)를 사용 하 여 특정된 기호 체계의 특성을 얻을 수 있습니다. 다음 코드 조각은 **ClaimedBarcodeScanner**에 대 한 Upca 기호의 특성을 가져옵니다.

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

해당 특성을 수정 완료 하 고 설정할 준비가 되 면 [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync)를 호출할 수 있습니다. 이 메서드는 특성이 성공적으로 설정 하는 경우 **true** 인 **부울**을 반환 합니다.

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>검사 데이터 길이 제한
일부 코드는 코드 39나 코드 128 같이 가변 길이입니다.  이 기호의 바코드는 특정 길이의 서로 다른 데이터가 포함 된 서로 가까이 있을 수 있습니다. 필요한 데이터의 특정 길이를 설정하면 잘못된 스캔을 방지할 수 있습니다.

디코드 길이 설정 하기 전에 바코드 기호 [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)를 사용 하 여 여러 길이 지원 하는지 여부를 확인 합니다. 지원 되는지 판단 합니다 [DecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind), 형식인 [BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)설정할 수 있습니다. 이 속성은 다음 값 중 하나일 수 있습니다.

* **AnyLength**: 상관 없이의 길이 디코딩합니다.
* **이산**: [DecodeLength1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1) 또는 [DecodeLength2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2) 싱글바이트 문자 길이 디코딩합니다.
* **범위**: **DecodeLength1** 및 **DecodeLength2** 싱글바이트 문자 사이의 길이 디코딩합니다. 순서 **DecodeLength1** 와 **DecodeLength2** (하나 수 다른 보다 높거나 낮은) 하는 중요 하지 않습니다.

마지막으로 필요한 데이터의 길이 제어 하는 **DecodeLength1** 과 **DecodeLength2** 값을 설정할 수 있습니다.

다음 코드 조각은 디코드 길이 설정 하는 방법을 보여 줍니다.

```cs
private async Task<bool> SetDecodeLength(
    ClaimedBarcodeScanner scanner,
    uint symbology, 
    BarcodeSymbologyDecodeLengthKind kind, 
    uint decodeLength1, 
    uint decodeLength2)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsDecodeLengthSupported)
    {
        attributes.DecodeLengthKind = kind;
        attributes.DecodeLength1 = decodeLength1;
        attributes.DecodeLength2 = decodeLength2;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-transmission"></a>숫자 전송 확인

기호에 설정한 다른 특성 여부 확인 숫자 전송 되는 호스트에 원시 데이터의 일환으로입니다. 이 설정 하기 전에 기호 확인을 지원 하는지 확인 [IsCheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported)된 숫자를 전송 합니다. 그런 다음 확인 숫자 전송 [IsCheckDigitTransmissionEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled)사용 하도록 설정 되었는지 여부를 설정 합니다.

다음 코드 조각은 설정을 확인 숫자 전송을 보여 줍니다.

```cs
private async Task<bool> SetCheckDigitTransmission(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitTransmissionSupported)
    {
        attributes.IsCheckDigitTransmissionEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-validation"></a>숫자 유효성 검사를 확인 합니다.

바코드 확인 숫자 유효성을 검사할 수 있는지 여부를 설정할 수 있습니다. 이 설정 하기 전에 기호 확인을 지원 하는지 확인 [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported)를 사용 하 여 숫자 유효성 검사 합니다. 그런 다음 [IsCheckDigitValidationEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled)를 사용 하 여 확인 숫자 유효성 검사를 사용할지 여부를 설정 합니다.

다음 코드 조각은 설정을 확인 숫자 유효성 검사를 보여 줍니다.

```cs
private async Task<bool> SetCheckDigitValidation(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitValidationSupported)
    {
        attributes.IsCheckDigitValidationEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>참고 항목

* [바코드 스캐너](pos-barcodescanner.md)
* [BarcodeSymbologies 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
* [BarcodeScanner 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [ClaimedBarcodeScanner 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)
* [BarcodeSymbologyAttributes 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)
* [BarcodeSymbologyDecodeLengthKind 열거형](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)