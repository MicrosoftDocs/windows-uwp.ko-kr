---
author: eliotcowley
title: 얻어서 자기 띠 데이터 이해
description: 자기 띠에서 데이터를 해석 하는 방법에 알아봅니다.
ms.author: elcowle
ms.date: 10/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 서비스, pos, 자기 띠 판독기 가리킨
ms.localizationpriority: medium
ms.openlocfilehash: ad954e8c03d92307fa72ead236d5428ac2bdddab
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4498640"
---
# <a name="obtain-and-understand-magnetic-stripe-data"></a>얻어서 자기 띠 데이터 이해

[서비스 지점 시작](pos-basics.md)에 설명 된 단계를 사용 하 여 응용 프로그램에서 자기 띠 판독기에 설정한 후 여기에서 데이터를 가져오려는 시작 준비가 되었습니다.

## <a name="subscribe-to-datareceived-events"></a>구독 * DataReceived 이벤트

판독기 민된 카드를 인식 될 때마다 발생 세 가지 이벤트 중 하나:

* [AamvaCardDataReceived 이벤트](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): 자동차 카드 살짝 민 때 발생 합니다.
* [BankCardDataReceived 이벤트](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): 은행 카드로 살짝 민 때 발생 합니다.
* [VendorSpecificDataReceived 이벤트](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.vendorspecificdatareceived): 공급 업체별 카드 살짝 민 때 발생 합니다.

응용 프로그램만 자기 띠 판독기에 의해 지원 되는 이벤트를 구독 해야 합니다. 어떤 유형의 카드 [MagneticStripeReader.SupportedCardTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.supportedcardtypes
)사용 지원 되는 것을 볼 수 있습니다.

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

이벤트 처리기 [ClaimedMagneticStripeReader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader) 및 형식이 이벤트에 따라 달라 집니다 *args* 개체를 전달 합니다.

* **AamvaCardDataReceived** 이벤트: [MagneticStripeReaderAamvaCardDataReceivedEventArgs 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* **BankCardDataReceived** 이벤트: [MagneticStripeReaderBankCardDataReceivedEventArgs 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* **VendorSpecificDataReceived** 이벤트: [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)

## <a name="get-the-data"></a>데이터 가져오기

**AamvaCardDataReceived** / **BankCardDataReceived** 이벤트 *args* 개체에서 직접 일부 데이터를 얻을 수 있습니다. 다음 예제에서는 몇 가지 속성을 가져오고 멤버 변수를 할당 하는 방법을 보여 줍니다.

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

그러나 *인수* 매개 변수의 속성인 **보고서** 개체를 통해 **VendorSpecificDataReceived** 이벤트에서 모든 데이터를 포함 하 여 일부 데이터를 검색 해야 합니다. 이 [MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)형식입니다.

[CardType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.cardtype) 속성을 사용 하 여 어떤 유형의 카드 미 된를 파악 하 고를 사용 하 여에 [Track1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track1), [Track2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track2), [Track3](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track3)및 [Track4](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track4)에서 데이터를 해석 하는 방법을 알릴 수 있습니다.

각 트랙에서 데이터 [MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata) 개체로 표시 됩니다. 이 클래스에서 다음과 같은 유형의 데이터를 가져올 수 있습니다.

* [데이터](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.data): 원시 또는 디코딩된 데이터입니다.
* [DiscretionaryData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.discretionarydata): 임의 데이터입니다. 
* [EncryptedData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.encrypteddata): 암호화 된 데이터입니다.

다음 코드 조각은 보고서 및 추적 데이터를 가져오고 카드 종류를 확인 한 후:

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

* [자기 띠 판독기](pos-magnetic-stripe-reader.md)
* [ClaimedMagneticStripeReader 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)
* [MagneticStripeReaderAamvaCardDataReceivedEventArgs 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* [MagneticStripeReaderBankCardDataReceivedEventArgs 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs 클래스](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)
* [MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)
* [MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)