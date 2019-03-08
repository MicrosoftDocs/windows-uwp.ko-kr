---
title: 자기 띠 데이터 가져오기 및 이해
description: 마그네틱 띠에서 데이터를 해석 하는 방법에 알아봅니다.
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10, uwp, 서비스, pos, 마그네틱 띠 판독기 지점
ms.localizationpriority: medium
ms.openlocfilehash: 1805213c7c30ccbc67fb96098f11480703589bb4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651608"
---
# <a name="obtain-and-understand-magnetic-stripe-data"></a>자기 띠 데이터 가져오기 및 이해

에 설명 된 단계를 사용 하 여 응용 프로그램에서 마그네틱 띠 판독기에 설정한 후 [Point of Service 시작](pos-basics.md), 여기에서 데이터 가져오기를 시작할 준비가 되었습니다. 합니다.

## <a name="subscribe-to-datareceived-events"></a>구독할 * DataReceived 이벤트

판독기 인식 스와이프 카드 때마다 세 가지 이벤트 중 하나가 발생 합니다.

* [AamvaCardDataReceived 이벤트](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): 차량 카드 학습은 때 발생 합니다.
* [BankCardDataReceived 이벤트](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): 은행 카드 학습은 때 발생 합니다.
* [VendorSpecificDataReceived 이벤트](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.vendorspecificdatareceived): 공급 업체별 카드 학습은 때 발생 합니다.

응용 프로그램 에서만 마그네틱 띠 판독기에서 지원 되는 이벤트를 구독 해야 합니다. 카드 유형을 사용 하 여 지를 볼 수 있습니다 [MagneticStripeReader.SupportedCardTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.supportedcardtypes
)합니다.

다음 코드에서는 세 가지 구독 ***DataReceived** 이벤트:

```cs
private void SubscribeToEvents(ClaimedMagneticStripeReader claimedReader, MagneticStripeReader reader)
{
    foreach (var type in reader.SupportedCardTypes)
    {
        if (item == MagneticStripeReaderCardTypes.Aamva)
        {
            claimedReader.AamvaCardDataReceived += Reader_AamvaCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.Bank)
        {
            claimedReader.BankCardDataReceived += Reader_BankCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.ExtendedBase)
        {
            claimedReader.VendorSpecificDataReceived += Reader_VendorSpecificDataReceived;
        }
    }
}
```

전달할 이벤트 처리기를 [ClaimedMagneticStripeReader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader) 및 *args* 개체 형식의 이벤트에 따라 달라 집니다.

* **AamvaCardDataReceived** 이벤트: [MagneticStripeReaderAamvaCardDataReceivedEventArgs 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* **BankCardDataReceived** 이벤트: [MagneticStripeReaderBankCardDataReceivedEventArgs Class](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* **VendorSpecificDataReceived** 이벤트: [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs Class](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)

## <a name="get-the-data"></a>데이터 가져오기

에 대 한 합니다 **AamvaCardDataReceived** 및 **BankCardDataReceived** 이벤트를 가져올 수 있습니다에서 직접 데이터 중 일부를 *args* 개체입니다. 다음 예제에서는 몇 가지 속성을 가져오고 멤버 변수에 할당을 보여 줍니다.

```cs
private string _accountNumber;
private string _expirationDate;
private string _firstName;

private void Reader_BankCardDataReceived(
    ClaimedMagneticStripeReader sender, 
    MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    _accountNumber = args.AccountNumber;
    _expirationDate = args.ExpirationDate;
    _firstName = args.FirstName;
    // etc...
}
```

그러나 모든 데이터를 비롯 한 일부 데이터를는 **VendorSpecificDataReceived** 이벤트를 통해 검색 해야 합니다 **보고서** 속성인 개체의는 *args* 매개 변수입니다. 형식의 이것이 [MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)합니다.

사용할 수는 [CardType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.cardtype) 속성을 어떤 형식의 카드에 되었습니다 누르거나 살짝 밀거나 파악 하 고 다음에서 데이터를 해석 하는 방법을 알리는 데 사용 하는 하는 [Track1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track1)를 [Track2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track2)를 [ Track3](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track3), 및 [Track4](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track4)합니다.

각 트랙 데이터로으로 표시 됩니다 [MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata) 개체입니다. 이 클래스에서 다음과 같은 유형의 데이터를 가져올 수 있습니다.

* [데이터](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.data): 원시 또는 디코딩된 데이터입니다.
* [DiscretionaryData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.discretionarydata): 임의 데이터입니다. 
* [EncryptedData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.encrypteddata): 암호화 된 데이터입니다.

다음 코드 조각은 보고서 및 추적 데이터를 가져와서 카드 종류를 확인 합니다.

```cs
private void GetTrackData(MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    MagneticStripeReaderReport report = args.Report;
    IBuffer track1 = report.Track1.Data;
    IBuffer track2 = report.Track2.Data;
    IBuffer track3 = report.Track3.Data;
    IBuffer track4 = report.Track4.Data;

    if (report.CardType == MagneticStripeReaderCardTypes.Aamva)
    {
        // Card type is AAMVA
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Bank)
    {
        // Card type is bank card
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.ExtendedBase)
    {
        // Card type is vendor-specific
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Unknown)
    {
        // Card type is unknown
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>참고 항목

* [마그네틱 띠 판독기](pos-magnetic-stripe-reader.md)
* [ClaimedMagneticStripeReader 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)
* [MagneticStripeReaderAamvaCardDataReceivedEventArgs 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* [MagneticStripeReaderBankCardDataReceivedEventArgs 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs Class](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)
* [MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)
* [MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)