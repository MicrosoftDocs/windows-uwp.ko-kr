---
author: eliotcowley
title: 구하여 바코드 데이터 이해
description: 바코드 스캔 데이터를 해석 하는 방법에 알아봅니다.
ms.author: elcowle
ms.date: 08/29/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 0992ea54092063ba53f23871599905e58f1b456e
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4114521"
---
# <a name="obtain-and-understand-barcode-data"></a>구하여 바코드 데이터 이해

바코드 스캐너를 설정한 후 스캔 데이터를 이해 하는 방법이 물론 필요 합니다. 바코드를 스캔할 때 [DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) 이벤트가 발생 합니다. [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 는이 이벤트를 등록 해야 합니다. **DataReceived** 이벤트는 바코드 데이터에 액세스 하는 데 사용할 수 있는 [BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs) 개체를 전달 합니다.

## <a name="subscribe-to-the-datareceived-event"></a>DataReceived 이벤트

**ClaimedBarcodeScanner**는 일단 **DataReceived** 이벤트에 가입 했습니다.

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

이벤트 처리기는 **ClaimedBarcodeScanner** 및 **BarcodeScannerDataReceivedEventArgs** 개체에 전달 됩니다. 바코드 데이터 형식인 [BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)이 개체의 [보고서](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) 속성을 통해 액세스할 수 있습니다.

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>데이터 가져오기

**BarcodeScannerReport**를 만든 후 액세스할 수 있으며 바코드 데이터를 구문 분석 합니다. **BarcodeScannerReport** 에 속성이 3 개 있지만:

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata): 전체, 원시 바코드 데이터입니다.
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel): 디코딩된 바코드 레이블 머리글, 검사, 및 기타 다른 정보를 포함 하지 않는.
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype): 디코딩된 바코드 레이블 형식입니다. 가능한 값은 [BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) 클래스에 정의 됩니다.

**ScanDataLabel** 또는 **ScanDataType**중 하나에 액세스 하려면 **true**로 [IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) 을 먼저 설정 해야 합니다.

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>스캔 데이터 형식을 가져옵니다.

아주 평범한 일은 디코딩된 바코드 레이블 형식을&mdash;단순히 [GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) **ScanDataType**요청 합니다.

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>스캔 데이터 레이블을 가져옵니다.

디코딩된 바코드 라벨을 염두에 두어야 할 몇 가지 있습니다. 특정 데이터 형식만 인코딩된 텍스트를 포함할 경우 기호 문자열로 변환할 수 변환한 후 문자열 인코딩은 u t F-8로 **ScanDataLabel** 에서 받는 버퍼를 먼저 확인 해야 합니다.

```cs
private string GetDataLabel(BarcodeScannerDataReceivedEventArgs args)
{
    uint scanDataType = args.Report.ScanDataType;

    // Only certain data types contain encoded text.
    // To keep this simple, we'll just decode a few of them.
    if (args.Report.ScanDataLabel == null)
    {
        return "No data";
    }

    // This is not an exhaustive list of symbologies that can be converted to a string.
    else if (scanDataType == BarcodeSymbologies.Upca ||
        scanDataType == BarcodeSymbologies.UpcaAdd2 ||
        scanDataType == BarcodeSymbologies.UpcaAdd5 ||
        scanDataType == BarcodeSymbologies.Upce ||
        scanDataType == BarcodeSymbologies.UpceAdd2 ||
        scanDataType == BarcodeSymbologies.UpceAdd5 ||
        scanDataType == BarcodeSymbologies.Ean8 ||
        scanDataType == BarcodeSymbologies.TfStd)
    {
        // The UPC, EAN8, and 2 of 5 families encode the digits 0..9
        // which are then sent to the app in a UTF8 string (like "01234").
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanDataLabel);
    }

    // Some other symbologies (typically 2-D symbologies) contain binary data that
    // should not be converted to text.
    else
    {
        return "Decoded data unavailable.";
    }
}
```

### <a name="get-the-raw-scan-data"></a>원시 검사 데이터를 가져옵니다.

전체를 원시 데이터, 바코드를 간단 하 게 변환 **ScanData** 에서 문자열을 받는 버퍼 합니다.

```cs
private string GetRawData(BarcodeScannerDataReceivedEventArgs args)
{
    // Get the full, raw barcode data.
    if (args.Report.ScanData == null)
    {
        return "No data";
    }

    // Just to show that we have the raw data, we'll print the value of the bytes.
    else
    {
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanData);
    }
}
```

이러한 데이터와 스캐너에서 형식으로 일반적으로. 그러나 응용 프로그램에 대 한 유용한 정보가 들어 있지 않습니다과 스캐너 관련 될 수 있으므로 메시지 헤더와 트레일러 정보 제거 됩니다.

공용 헤더 정보는 접두사 (예: STX 문자)입니다. 트레일러는 공용 정보 이면 블록 확인 문자 및 문자 (예: ETX 또는 CR 문자)는 해당 검색 프로그램에 의해 생성 됩니다.

검색 프로그램으로 반환 되 면이 속성 기호 문자를 포함 합니다 (예를 들어, **A** 에 UPC A). 레이블에 있는 경우 검사 숫자 포함 해야 하 고 검색 프로그램으로 반환 합니다. (참고 하 기호 문자와 숫자 확인 되거나 없을 수도 있습니다, 스캐너 구성에 따라. 스캐너는 돌려보낼 경우 제시, 하지만 되거나 되지 것입니다 생성 없을 경우 자동으로 계산 됩니다.)

일부 상품 보충 바코드를 사용 하 여 표시할 수 있습니다. 이 바코드 일반적으로 주 바코드 오른쪽에 놓이고 정보의 추가 2 개 또는 다섯 개의 문자로 구성 됩니다. 주 및 보조 바코드를 포함 하 스캐너 상품 내용이 다음과 같은 경우 보조 문자는 기본 문자에 추가 됩니다 및 결과 한 레이블의 응용 프로그램에 전달 됩니다. (참고 스캐너 구성을 사용 하거나 보완 코드의 판독을 지원할 수 있습니다.)

일부 상품 *multisymbol 레이블* 또는 *레이블 계층 구성된*여러 레이블로 표시할 수 있습니다. 이러한 바코드는 일반적으로 수직으로 정렬 하며의 같거나 다른 기호입니다. 스캐너 1 레이블이 여러 개 포함 된 상품을 각 바코드 레이블과 별도 응용 프로그램에 전달 됩니다. 이러한 바코드 형식의 표준화의 현재 부족 하 여 필요한 경우 하나는 개별 바코드 데이터를 바탕으로 모든 변형을 확인할 수 수 없습니다. 따라서, 응용 프로그램 때 여러 바코드 레이블을 읽은 다음에 반환 되는 데이터에 따라 결정 해야 합니다. (메모 스캐너 수도의 다중 레이블 읽기를 지원 하지 않을 수도 있습니다.)

응용 프로그램에 발생 하는 **DataReceived** 이벤트를 하기 전에이 값을 설정 합니다.

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>참고 항목
* [바코드 스캐너](pos-barcodescanner.md)
* [ClaimedBarcodeScanner 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [BarcodeScannerDataReceivedEventArgs 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [BarcodeScannerReport 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [BarcodeSymbologies 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)