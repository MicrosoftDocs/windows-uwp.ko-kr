---
title: 바코드 데이터 받기 및 인식하기
description: 검색 하는 바코드 데이터를 해석 하는 방법에 알아봅니다.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ece246ffd369ee21c089598f07b2566424757f55
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605968"
---
# <a name="obtain-and-understand-barcode-data"></a>바코드 데이터 받기 및 인식하기

바코드 스캐너를 설정한 후 검사 하는 데이터를 이해 하는 방법 이외에 필요 합니다. 바코드를 스캔할 때 합니다 [DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) 이벤트가 발생 합니다. 합니다 [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 이 이벤트를 구독 해야 합니다. 합니다 **DataReceived** 이벤트 전달를 [BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs) 바코드 데이터에 액세스 하는 데 사용할 수 있는 개체입니다.

## <a name="subscribe-to-the-datareceived-event"></a>DataReceived 이벤트 구독

했으면를 **ClaimedBarcodeScanner**를 구독 하 게 합니다 **DataReceived** 이벤트:

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

전달할 이벤트 처리기를 **ClaimedBarcodeScanner** 와 **BarcodeScannerDataReceivedEventArgs** 개체입니다. 이 개체를 통해 바코드 데이터에 액세스할 수 있습니다 [보고서](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) 유형인 속성인 [BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)합니다.

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>데이터 가져오기

했으면 합니다 **BarcodeScannerReport**, 액세스 및 바코드 데이터의 구문을 분석할 수 있습니다. **BarcodeScannerReport** 세 가지 속성이 있습니다.

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata): 전체, 원시 바코드 데이터입니다.
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel): 디코딩된 바코드 레이블-헤더, 체크섬 및 기타 정보도 포함 되지 않습니다.
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype): 디코딩된 바코드 레이블 형식입니다. 에 정의 된 가능한 값은 [BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) 클래스입니다.

액세스 하려는 경우 **ScanDataLabel** 또는 **ScanDataType**를 먼저 설정 해야 [IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) 하 **true**합니다.

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>검색 데이터 형식 가져오기

매우 간단 디코딩된 바코드 레이블 형식을 가져오는&mdash;를 호출 하 [GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) 온 **ScanDataType**합니다.

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>검색 데이터 레이블을 가져옵니다

디코딩된 바코드 레이블을 가져오려면 몇 가지 사항에 유의 해야 있습니다. 특정 데이터 형식 기호 문자열로 변환할 수 및 다음에서 얻게 버퍼를 변환 하는 경우 먼저 확인 해야 하므로 인코딩된 텍스트를 포함 하는 전용 **ScanDataLabel** u t F-8로 인코딩된 문자열로 합니다.

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

### <a name="get-the-raw-scan-data"></a>원시 검색 데이터 가져오기

바코드를 완전 한 원시 데이터를 활용 하려면 단순히 변환 버퍼에서 얻게 **ScanData** 문자열로 합니다.

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

이러한 데이터는, 일반적으로 스캐너에서 제공 되는 형식에서입니다. 그러나 응용 프로그램에 대 한 유용한 정보를 포함 하지 않는 스캐너 관련 될 가능성이 있으므로 메시지 헤더 및 트레일러 정보가 제거 됩니다.

일반적인 헤더 정보에는 (예: STX 문자) 접두사 문자입니다. 스캐너에서 생성 되는 일반적인 트레일러 정보는 종결자 문자 (예: ETX 또는 CR 문자) 되며 블록 검사 문자.

스캐너에서 반환 되 면이 속성 기호 문자를 포함 해야 합니다 (예를 들어를 **는** 에 대 한 UPC-A). 레이블에 있는 경우 검사 숫자 포함 해야 하 고 검색 프로그램으로 반환 합니다. (참고 검사 숫자 및 기호 문자 수 또는 없을 수도 있습니다, 스캐너 구성에 따라 합니다. 스캐너를 반환 하는 경우 존재 하지만 생성 않거나 없는 경우 자동으로 계산 합니다.)

일부 상품 추가 바코드를 사용 하 여 표시 될 수 있습니다. 이 바코드는 일반적으로 주 바코드의 오른쪽에 배치 및 정보의 추가 2 개 또는 5 개 문자로 구성 됩니다. 스캐너 상품을 읽는 경우 기본 및 보조 바코드를 포함 하는 보충 문자는 기본 문자를에 추가 됩니다 및 결과 하나 레이블로 응용 프로그램에 전달 됩니다. (참고 스캐너를 사용 하거나 추가 코드의 판독 하는 구성을 지원할 수 있습니다.)

일부 상품 라고도 하는 여러 레이블과 함께 표시 될 수 있습니다 *multisymbol 레이블을* 또는 *레이블을 계층화*합니다. 이 바코드는 일반적으로 세로 정렬 및 동일 하거나 다른 기호 될 수 있습니다. 스캐너는 여러 레이블을 포함 하는 상품을 읽고, 각 바코드 별도 레이블로 응용 프로그램에 배달 됩니다. 현재 이러한 유형의 바코드 표준화 부족으로 인해 되기 합니다. 개별 바코드 데이터를 기반으로 하는 모든 변형을 확인할 수 없는 합니다. 따라서 응용 프로그램 경우 여러 레이블 바코드 읽은 반환 되는 데이터에 따라 결정 해야 합니다. (참고 스캐너 수도 여러 레이블의 읽기를 지원 하지 않을 수 있습니다.)

이 값은 이전에 설정 된 **DataReceived** 응용 프로그램에 발생 한 이벤트입니다.

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>참고 항목
* [바코드 스캐너](pos-barcodescanner.md)
* [ClaimedBarcodeScanner 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [BarcodeScannerDataReceivedEventArgs 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [BarcodeScannerReport 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [BarcodeSymbologies 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
