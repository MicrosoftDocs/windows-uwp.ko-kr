---
title: 바코드 스캐너 기호 작업
description: 이 문서에는 바코드 스캐너 기호에 대한 정보가 나와 있습니다.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: ee78ffbc49fdcb7f8e87844dea1e2ce29297e9f3
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63816689"
---
# <a name="working-with-symbologies"></a>기호 처리
[바코드 기호](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)는 특정한 바코드 형식으로 데이터를 매핑한 것입니다. 몇 가지 일반적인 symbologies UPC, 코드 128, QR 코드 및 등을 포함 합니다.  유니버설 Windows 플랫폼 바코드 스캐너 Api 스캐너 스캐너를 수동으로 구성 하지 않고도 이러한 symbologies를 처리 하는 방법을 제어 하는 응용 프로그램을 허용 합니다. 

## <a name="determine-which-symbologies-are-supported"></a>지원되는 기호 확인 
여러 제조업체의 서로 다른 바코드 스캐너 모델에서 응용 프로그램이 사용될 수 있기 때문에 지원되는 기호 목록을 결정하기 위해 스캐너를 쿼리하고 싶을 것입니다.  응용 프로그램에서 일부 스캐너에서 지원되지 않는 특정 기호가 필요하거나 스캐너에서 수동 또는 프로그래밍 방식으로 비활성화를 한 기호를 활성화해야 하는 경우에 유용할 수 있습니다.

[BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync)를 사용하여 [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) 개체를 가져오고 나면 [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync)를 호출하여 장치에서 지원되는 기호 목록을 확보합니다.

다음 예제에서는 바코드 스캐너의 지원 되는 symbologies 목록을 가져와 텍스트 블록에 표시:

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
스캐너에서 호출할 수는 특정 기호를 지원 하는지 확인할 [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)합니다.

다음 예제에서는 바코드 스캐너를 지원 하는지 확인 합니다 **Code32** 기호:

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>인식 되는 symbologies 변경
경우에 따라 바코드 스캐너가 지원하는 기호의 하위 집합을 사용하고 싶을 수 있습니다.  이는 응용 프로그램에서 사용할 목적이 아니었던 기호를 차단하는 데 특히 유용합니다. 예를 들어, 사용자가 오른쪽 바코드를 스캔하도록 항목 SKU를 얻을 때 UPC 또는 EAN으로 스캔을 제한하고, 일련 번호를 얻을 때 코드 128로 스캔을 제한할 수 있습니다.

일단 스캐너에서 지원되는 기호를 파악하면 인식하기를 원하는 기호를 설정할 수 있습니다.  설정한 후이 수행할 수 있습니다는 [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 사용 하 여 개체 [ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync)합니다. [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__)를 호출하여 목록에서 누락된 기호는 비활성화하고 특정 기호 집합은 활성화할 수 있습니다.

다음 예제에서는 요청 된 바코드 스캐너의 현재 symbologies [Code39](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39) 하 고 [Code39Ex](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex):

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>바코드 기호 특성
다른 바코드 symbologies 지원 여러 호스트 확인 자리에서 원시 데이터의 일부로 전송 길이 디코딩하고 숫자 유효성 검사 확인 같은 다른 특성을 있습니다. 사용 하 여는 [BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes) 클래스를 가져오고 설정할 수 있습니다 이러한 특성에 대 한는 주어진 [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 및 바코드 기호.

특성을 사용 하 여 지정 된 기호를 가져올 수 있습니다 [GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_)합니다. 다음 코드 조각은 가져옵니다 Upca 기호에 대 한 특성을 **ClaimedBarcodeScanner**합니다.

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

호출할 수 있습니다 하는 특성 및 설정 준비가 수정 했으면 [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync)합니다. 이 메서드가 반환를 **bool**, 즉 **true** 특성을 성공적으로 설정 된 경우.

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>검색 데이터에서 길이 제한
일부 코드는 코드 39나 코드 128 같이 가변 길이입니다.  이러한 symbologies의 바코드 특정 길이의 종종 서로 다른 데이터가 포함 된 서로 가까이 있을 수 있습니다. 필요한 데이터의 특정 길이를 설정하면 잘못된 스캔을 방지할 수 있습니다.

바코드 기호를 사용 하 여 여러 길이 지원 하는지 여부를 확인 디코드 길이 설정 하기 전에 [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)합니다. 설정할 수 있습니다를 지원 하는지 알고 있다면 합니다 [DecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind), 형식인 [BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind). 이 속성은 다음 값 중 하나일 수 있습니다.

* **AnyLength**: 원하는 수의 길이 디코딩하십시오.
* **불연속**: 길이 디코딩 [DecodeLength1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1) 하거나 [DecodeLength2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2) 싱글바이트 문자입니다.
* **범위**: 사이의 길이 디코딩 **DecodeLength1** 하 고 **DecodeLength2** 싱글바이트 문자입니다. 순서 **DecodeLength1** 하 고 **DecodeLength2** 하나 일 수 있습니다 다른 보다 높거나 낮은 중요 하지 않습니다.

마지막으로 값을 설정할 수 있습니다 **DecodeLength1** 하 고 **DecodeLength2** 필요한 데이터의 길이 제어할 수 있습니다.

다음 코드 조각 디코딩 길이 설정 하는 방법을 보여 줍니다.

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

### <a name="check-digit-transmission"></a>숫자 전송을 확인 합니다.

기호를 설정할 수 있습니다 하는 다른 특성 여부 검사 숫자는 전송 호스트에 원시 데이터의 일부로입니다. 이 설정 하기 전에 기호 확인을 지원 하는지 확인 된 숫자를 전송 [IsCheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported)합니다. 그런 다음 확인 자리 전송으로 사용 되는지 여부를 설정 [IsCheckDigitTransmissionEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled)합니다.

다음 코드 조각 설정 확인 자리 전송을 보여 줍니다.

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

바코드 검사 숫자 유효성을 검사할 수 있는지 여부를 설정할 수 있습니다. 이 설정 하기 전에 기호 확인을 지원 하는지 확인을 사용한 숫자 유효성 검사 [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported)합니다. 그런 다음 사용 하 여 검사 숫자의 유효성 검사를 사용할지 여부를 설정 [IsCheckDigitValidationEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled)합니다.

다음 코드 조각 설정을 검사 숫자의 유효성 검사를 보여 줍니다.

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

## <a name="see-also"></a>참조

* [바코드 스캐너](pos-barcodescanner.md)
* [BarcodeSymbologies 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
* [BarcodeScanner 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [ClaimedBarcodeScanner 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)
* [BarcodeSymbologyAttributes 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)
* [BarcodeSymbologyDecodeLengthKind 열거형](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)