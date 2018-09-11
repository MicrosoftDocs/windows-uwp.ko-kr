---
author: eliotcowley
title: 가져와서 바코드 데이터 이해
description: 스캔 바코드 데이터를 해석 하는 방법에 알아봅니다.
ms.author: elcowle
ms.date: 08/29/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 0992ea54092063ba53f23871599905e58f1b456e
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/11/2018
ms.locfileid: "3848687"
---
# <a name="obtain-and-understand-barcode-data"></a>가져와서 바코드 데이터 이해

바코드 스캐너를 설정한 후 스캔 데이터를 이해 하는 방법이 물론 필요 합니다. 바코드를 스캔할 때 [DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) 이벤트가 발생 합니다. [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 이 이벤트를 구독 해야 합니다. **DataReceived** 이벤트는 바코드 데이터에 액세스 하는 데 사용할 수 있는 [BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs) 개체를 전달 합니다.

## <a name="subscribe-to-the-datareceived-event"></a>DataReceived 이벤트에 등록

**ClaimedBarcodeScanner**있으면 **DataReceived** 이벤트를 구독 했습니다.

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

이벤트 처리기는 **ClaimedBarcodeScanner** 및 **BarcodeScannerDataReceivedEventArgs** 개체에 전달 됩니다. 바코드 데이터 형식이 [BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)이 개체의 [보고서](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) 속성을 통해 액세스할 수 있습니다.

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>데이터 가져오기

**BarcodeScannerReport**있으면 바코드 데이터를 구문 분석 하 고 액세스할 수 있습니다. **BarcodeScannerReport** 에 세 가지 속성이 있습니다.

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata): 전체, 원시 바코드 데이터입니다.
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel): 헤더, 체크섬 및 기타 정보를 포함 하지 않는 디코딩된 바코드 레이블.
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype): 디코딩된 바코드 레이블 형식입니다. 가능한 값은 [BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) 클래스에서 정의 됩니다.

**ScanDataLabel** 또는 **ScanDataType**액세스 하려는 경우에 **true**로 먼저 [IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) 를 설정 해야 합니다.

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>검사 데이터 유형 가져오기

상당히 쉽습니다 디코딩된 바코드 레이블 형식을&mdash; **ScanDataType** [GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) 호출 하기만 하면 됩니다.

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>검사 데이터 레이블 가져오기

디코딩된 바코드 레이블을 가져오는 인식 해야 하는 몇 가지 있습니다. 특정 데이터 형식 경우 기호를 문자열로 변환 될 수 변환한 후 다시 인코딩된 u t F-8 문자열로 **ScanDataLabel** 에서 얻은 버퍼를 먼저 확인 해야 하므로 인코딩된 텍스트를 포함 합니다.

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

### <a name="get-the-raw-scan-data"></a>원시 검사 데이터 가져오기

전체를 가져오려면 원시 데이터는 바코드를 간단 하 게 변환 문자열로 **ScanData** 에서 얻은 버퍼 합니다.

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

이러한 데이터 형식 스캐너에서 제공 하는 대로 일반적으로 됩니다. 그러나 응용 프로그램에 대 한 유용한 정보가 포함 되지 않은 스캐너 특정 수 있으므로 메시지 머리글과 예고편 정보 제거 됩니다.

일반적인 헤더 정보는 접두사 문자 (예: STX 문자). 일반적인 예고편 정보 스캐너에서 생성 한 하는 경우 (예: ETX 또는 CR 문자)는 문자 및 블록 확인 문자입니다.

스캐너에서 반환 되 면이 속성 기호 문자를 포함 해야 합니다 (예를 들어 **A** 에 대 한 UPC A). 레이블의에 있는 경우에 확인 숫자를 포함 시켜야 하 고 스캐너에서 반환 합니다. (참고는 기호 문자 및 확인 숫자 수 그렇지 않을 수 있는 스캐너 구성에 따라 합니다. 스캐너 경우에 반환 하기를 제공 하지만 생성 하거나 않습니다 없는 경우 계산 합니다.)

일부 상품 추가 바코드를 사용 하 여 표시할 수 있습니다. 이 바코드 일반적으로 기본 바코드의 오른쪽에 배치 하는 및 정보의 추가 2 개 5 자 이루어져 있습니다. 스캐너 상품을 읽습니다 주 및 보조 바코드를 포함 하, 추가 문자는 주 문자에 추가 및 결과 하나의 레이블로 응용 프로그램에 전달 됩니다. (참고 스캐너 구성을 사용 하거나 추가 코드의 판독을 지원할 수 있습니다.)

*Multisymbol 레이블* 또는 *계층형된 레이블*을 라고도 하는 여러 레이블이 있는 일부 상품을 표시할 수 있습니다. 이러한 바코드는 세로로 정렬 되는 것이 일반적으로 하며 같거나 서로 다른 기호의 수 있습니다. 스캐너에 여러 레이블이 포함 된 상품이 나타나는 경우에 각 바코드 별도 레이블로 응용 프로그램에 제공 됩니다. 이러한 바코드 유형의 표준화 부족 하 여 현재 필요 합니다. 하나가 개별 바코드 데이터에 따라 모든 변형을 확인할 수 없습니다. 따라서 응용 프로그램 때 여러 레이블 바코드 읽은 반환 된 데이터에 따라 결정 해야 합니다. (참고 스캐너 수도 여러 레이블의 읽기를 지원 하지 않을 수 있습니다.)

이 값은 **DataReceived** 이벤트 응용 프로그램에 발생 하기 전에 설정 됩니다.

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>참고 항목
* [바코드 스캐너](pos-barcodescanner.md)
* [ClaimedBarcodeScanner 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [BarcodeScannerDataReceivedEventArgs 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [BarcodeScannerReport 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [BarcodeSymbologies 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)