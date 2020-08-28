---
title: 바코드 데이터 획득 및 이해
description: BarcodeScannerReport 개체의 바코드 스캐너에서 데이터를 가져오고 해당 형식 및 내용을 이해 하는 방법을 알아봅니다.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5d2ab873774ce8b48252b82ce6cf7521165554d4
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043445"
---
# <a name="obtain-and-understand-barcode-data"></a>바코드 데이터 획득 및 이해

바코드 스캐너를 설정한 후에는 검색 하는 데이터를 이해 하는 방법이 필요 합니다. 바코드를 스캔 하면 [Datareceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) 이벤트가 발생 합니다. [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 는이 이벤트를 구독 해야 합니다. **Datareceived** 이벤트는 바코드 데이터에 액세스 하는 데 사용할 수 있는 [BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs) 개체를 전달 합니다.

## <a name="subscribe-to-the-datareceived-event"></a>DataReceived 이벤트를 구독 합니다.

**ClaimedBarcodeScanner**가 있으면 **datareceived** 이벤트를 구독 하도록 합니다.

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

이벤트 처리기에 **ClaimedBarcodeScanner** 및 **BarcodeScannerDataReceivedEventArgs** 개체가 전달 됩니다. [BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)형식의이 개체의 [보고서](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) 속성을 통해 바코드 데이터에 액세스할 수 있습니다.

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>데이터 가져오기

**BarcodeScannerReport**가 있으면 바코드 데이터에 액세스 하 고 구문 분석할 수 있습니다. **BarcodeScannerReport** 에는 다음과 같은 세 가지 속성이 있습니다.

* [Scandata](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata): 전체 원시 바코드 데이터입니다.
* [Scandatalabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel): 헤더, 체크섬 및 기타 기타 정보를 포함 하지 않는 디코딩된 바코드 레이블입니다.
* [Scandatatype](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype): 디코딩된 바코드 레이블 유형입니다. 가능한 값은 [BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) 클래스에 정의 됩니다.

**Scandatalabel** 또는 **scandatalabel**에 액세스 하려는 경우 먼저 [IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) 를 **true**로 설정 해야 합니다.

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>검색 데이터 형식 가져오기

디코딩된 바코드 레이블 유형을 가져오는 것은 매우 간단 &mdash; 합니다 .이는 **scandatatype**에서 [GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) 을 호출 하는 것입니다.

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>검색 데이터 레이블 가져오기

디코딩된 바코드 레이블을 가져오려면 알아야 할 몇 가지 사항이 있습니다. 특정 데이터 형식만 인코딩된 텍스트를 포함 하므로 먼저 기호를 문자열로 변환할 수 있는지 확인 한 다음 **Scandatalabel** 에서 가져온 버퍼를 인코딩된 utf-8 문자열로 변환 해야 합니다.

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

바코드에서 전체 원시 데이터를 가져오려면 **스캔 데이터** 에서 가져오는 버퍼를 문자열로 변환 하기만 하면 됩니다.

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

이러한 데이터는 일반적으로 스캐너에서 배달 되는 형식입니다. 그러나 메시지 헤더와 트레일러 정보는 응용 프로그램에 대 한 유용한 정보를 포함 하지 않으며 스캐너와 관련 되어 있을 수 있기 때문에 제거 됩니다.

공용 헤더 정보는 접두사 문자 (예: STX 문자)입니다. 일반적인 트레일러 정보는 종결자 문자 (예: ETX 또는 CR 문자) 및 블록 확인 문자 (스캐너에서 생성 되는 경우)입니다.

스캐너에서 반환 되는 경우이 속성에 기호 문자가 포함 되어야 합니다 (예: UPC의 **a** ). 레이블에 표시 되 고 스캐너에서 반환 하는 경우에도 확인 숫자가 포함 되어야 합니다. (스캐너 구성에 따라 기호 문자와 확인 숫자가 모두 있을 수도 있고 없을 수도 있습니다. 스캐너는 있는 경우이를 반환 하지만, 없는 경우에는 해당 항목을 생성 하거나 계산 하지 않습니다.

일부 상품은 보충 바코드로 표시 될 수 있습니다. 이 바코드는 일반적으로 주 바코드의 오른쪽에 배치 되며, 두 가지 이상의 정보로 구성 됩니다. 스캐너가 주 바코드와 보충 바코드를 모두 포함 하는 상품을 읽는 경우 보조 문자가 기본 문자에 추가 되 고 결과가 응용 프로그램에 하나의 레이블로 전달 됩니다. (스캐너는 보충 코드 읽기를 사용 하거나 사용 하지 않도록 설정 하는 구성을 지원할 수 있습니다.)

일부 상품은 여러 레이블로 표시 될 수 있으며, 경우에 따라 *multisymbol 레이블* 또는 *계층화 된 레이블이*라고도 합니다. 일반적으로 이러한 바코드는 세로로 정렬 되며 같거나 다른 기호를 사용할 수 있습니다. 스캐너가 여러 레이블을 포함 하는 상품을 읽을 경우 각 바코드는 개별 레이블로 응용 프로그램에 전달 됩니다. 이러한 바코드 종류의 표준화가 부족 하기 때문에이 작업이 필요 합니다. 하나는 개별 바코드 데이터를 기반으로 모든 변형을 결정할 수 없습니다. 따라서 응용 프로그램은 반환 된 데이터에 따라 여러 레이블 바코드를 읽는 시기를 결정 해야 합니다. (스캐너가 여러 레이블 읽기를 지원 하거나 지원 하지 않을 수도 있습니다.)

이 값은 응용 프로그램에 대해 **Datareceived** 이벤트를 발생 하기 전에 설정 됩니다.

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>참고 항목
* [바코드 스캐너](pos-barcodescanner.md)
* [ClaimedBarcodeScanner 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [BarcodeScannerDataReceivedEventArgs 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [BarcodeScannerReport 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [BarcodeSymbologies 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
