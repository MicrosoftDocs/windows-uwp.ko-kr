---
title: 바코드 스캐너로 작업 symbologies
description: 스캐너를 수동으로 구성 하지 않고 UWP 바코드 스캐너 Api를 사용 하 여 바코드 symbologies를 처리 합니다.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 96014dfeb0b160d9cb94afef5ba4251b3c8bf8aa
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054383"
---
# <a name="working-with-symbologies"></a>기호 처리
[바코드 기호](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) 는 특정 바코드 형식에 대 한 데이터의 매핑입니다. 몇 가지 일반적인 symbologies는 UPC, 코드 128, QR 코드 등을 포함 합니다.  유니버설 Windows 플랫폼 바코드 스캐너 Api를 통해 응용 프로그램은 스캐너를 수동으로 구성 하지 않고도 스캐너에서 이러한 symbologies를 처리 하는 방법을 제어할 수 있습니다. 

## <a name="determine-which-symbologies-are-supported"></a>지원 되는 symbologies 확인 
응용 프로그램은 여러 제조업체의 다른 바코드 스캐너 모델과 함께 사용 될 수 있으므로 스캐너를 쿼리하여 지원 되는 symbologies 목록을 확인할 수 있습니다.  이는 응용 프로그램에 모든 스캐너에서 지원 되지 않는 특정 기호를 요구 하거나 스캐너에서 수동으로 또는 프로그래밍 방식으로 사용 하지 않도록 설정 해야 하는 경우에 유용할 수 있습니다.

[FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync)를 사용 하 여 [바 Code스캐너](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) 개체가 있으면 [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) 를 호출 하 여 장치에서 지 원하는 symbologies 목록을 가져옵니다.

다음 예제에서는 바코드 스캐너의 지원 되는 symbologies 목록을 가져온 다음 텍스트 블록에 표시 합니다.

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

## <a name="determine-if-a-specific-symbology-is-supported"></a>특정 기호를 사용할 수 있는지 확인
스캐너가 특정 기호를 지원 하는지 확인 하려면 [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)를 호출 하면 됩니다.

다음 예에서는 바코드 스캐너가 **Code32** 기호를 지원 하는지 확인 합니다.

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>인식 되는 symbologies 변경
일부 경우에는 바코드 스캐너에서 지 원하는 symbologies의 하위 집합을 사용 하는 것이 좋습니다.  응용 프로그램에서 사용 하지 않으려는 symbologies을 차단 하는 데 특히 유용 합니다. 예를 들어 사용자가 올바른 바코드를 검색 하도록 하기 위해 항목 Sku를 획득 하 고 일련 번호를 가져올 때 코드 128에 대 한 검사를 제한할 때 검색을 UPC 또는 e로 제한할 수 있습니다.

스캐너가 지 원하는 symbologies을 알고 있으면 인식할 symbologies를 설정할 수 있습니다.  [ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync)를 사용 하 여 [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 개체를 설정한 후이 작업을 수행할 수 있습니다. [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) 를 호출 하 여 목록에서 생략 된 특정 symbologies 집합을 사용할 수 있습니다.

다음 예제에서는 요청한 바코드 스캐너의 활성 symbologies를 [Code39](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39) 및 [Code39Ex](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex)로 설정 합니다.

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>바코드 기호 특성
바코드 symbologies 여러 가지 특성을 가질 수 있습니다. 예를 들어 여러 디코드 길이 지원, 원시 데이터의 일부로 호스트에 확인 숫자 전송, 숫자 유효성 검사 등이 있습니다. [BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes) 클래스를 사용 하 여 지정 된 [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 및 바코드 기호에 대 한 이러한 특성을 가져오고 설정할 수 있습니다.

[GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_)를 사용 하 여 지정 된 기호 특성을 가져올 수 있습니다. 다음 코드 조각에서는 **ClaimedBarcodeScanner**에 대 한 upca 기호 특성을 가져옵니다.

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

특성 수정을 마치고 이러한 특성을 설정할 준비가 되 면 [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync)를 호출할 수 있습니다. 이 메서드는 **부울**값을 반환 합니다 .이는 특성이 성공적으로 설정 된 경우 **true** 입니다.

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>데이터 길이로 검색 데이터 제한
일부 symbologies는 코드 39 또는 코드 128와 같은 가변 길이입니다.  이러한 symbologies의 바코드는 특정 길이가 자주 다른 데이터를 포함 하는 서로 가까이 배치할 수 있습니다. 필요한 데이터의 특정 길이를 설정 하면 잘못 된 검색을 방지할 수 있습니다.

디코드 길이를 설정 하기 전에 바코드 기호에서 [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)를 사용 하 여 여러 길이를 지원 하는지 확인 합니다. 지원 되는 것을 알고 있으면 [BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)형식의 [DecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind)을 설정할 수 있습니다. 이 속성은 다음 값 중 하나일 수 있습니다.

* **Anylength**: 임의 개수의 길이를 디코드 합니다.
* **불연속**: [DecodeLength1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1) 또는 [DecodeLength2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2) 싱글바이트 문자의 디코드 길이입니다.
* **범위**: **DecodeLength1** 와 **DecodeLength2** 싱글바이트 문자 사이의 디코드 길이입니다. **DecodeLength1** 및 **DecodeLength2** 의 순서는 중요 하지 않습니다 (다른 보다 높거나 낮을 수 있음).

마지막으로 **DecodeLength1** 및 **DecodeLength2** 값을 설정 하 여 필요한 데이터의 길이를 제어할 수 있습니다.

다음 코드 조각에서는 디코드 길이를 설정 하는 방법을 보여 줍니다.

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

기호에 대해 설정할 수 있는 다른 특성은 확인 숫자가 원시 데이터의 일부로 호스트에 전송 되는지 여부입니다. 이를 설정 하기 전에 [IsCheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported)를 사용 하 여 기호 확인 숫자 전송을 지원 하는지 확인 합니다. 그런 다음 [IsCheckDigitTransmissionEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled)를 사용 하 여 확인 숫자 전송을 사용할지 여부를 설정 합니다.

다음 코드 조각에서는 확인 숫자 전송을 설정 하는 방법을 보여 줍니다.

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

### <a name="check-digit-validation"></a>숫자 유효성 검사 확인

바코드 확인 숫자의 유효성을 검사할지 여부를 설정할 수도 있습니다. 이를 설정 하기 전에 [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported)를 사용 하 여 기호 확인 숫자 유효성 검사를 지원 하는지 확인 합니다. 그런 다음 [IsCheckDigitValidationEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled)를 사용 하 여 확인 숫자 유효성 검사를 사용 하도록 설정할지 여부를 설정 합니다.

다음 코드 조각에서는 확인 숫자 유효성 검사를 설정 하는 방법을 보여 줍니다.

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
* [바 Code스캐너 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [ClaimedBarcodeScanner 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)
* [BarcodeSymbologyAttributes 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)
* [BarcodeSymbologyDecodeLengthKind 열거형](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)