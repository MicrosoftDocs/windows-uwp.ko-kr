---
title: 자기 띠 데이터 획득 및 이해
description: UWP (유니버설 Windows 플랫폼) POS (Point of Service) Api를 사용 하 여 자기 stripe 판독기에서 데이터를 가져오고 해석 하는 방법에 대해 알아봅니다.
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos, 자기 줄무늬 판독기
ms.localizationpriority: medium
ms.openlocfilehash: 2a405a66fbc243925c4c9bbd5b1ee499a9a7b9f8
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238278"
---
# <a name="obtain-and-understand-magnetic-stripe-data"></a>자기 띠 데이터 획득 및 이해

[서비스 지점 시작](pos-basics.md)에 설명 된 단계를 사용 하 여 응용 프로그램에서 자기 줄무늬 판독기를 설정 하면 데이터 가져오기를 시작할 준비가 된 것입니다.

## <a name="subscribe-to-datareceived-events"></a>* DataReceived 이벤트를 구독 합니다.

판독기가 스와이프 카드를 인식할 때마다 다음과 같은 세 가지 이벤트 중 하나가 발생 합니다.

* [AamvaCardDataReceived Event](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): 화물 차량 카드가 스와이프 때 발생 합니다.
* [BankCardDataReceived Event](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): 은행 카드가 스와이프 때 발생 합니다.
* [VendorSpecificDataReceived Event](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.vendorspecificdatareceived): 공급 업체별 카드가 스와이프 때 발생 합니다.

응용 프로그램은 자기 줄무늬 판독기에서 지원 되는 이벤트를 구독 하기만 하면 됩니다. [MagneticStripeReader](/uwp/api/windows.devices.pointofservice.magneticstripereader.supportedcardtypes)에서 지원 되는 카드의 유형을 확인할 수 있습니다.

다음 코드는 세 개의 ***Datareceived** 이벤트를 구독 하는 방법을 보여 줍니다.

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

이벤트 처리기에는 이벤트에 따라 형식이 달라 지는 [ClaimedMagneticStripeReader](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader) 및 *args* 개체가 전달 됩니다.

* **AamvaCardDataReceived** 이벤트: [MagneticStripeReaderAamvaCardDataReceivedEventArgs 클래스](/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* **BankCardDataReceived** 이벤트: [MagneticStripeReaderBankCardDataReceivedEventArgs 클래스](/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* **VendorSpecificDataReceived** 이벤트: [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs 클래스](/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)

## <a name="get-the-data"></a>데이터 가져오기

**AamvaCardDataReceived** 및 **BankCardDataReceived** 이벤트의 경우 *args* 개체에서 직접 일부 데이터를 가져올 수 있습니다. 다음 예제에서는 몇 가지 속성을 가져오고 멤버 변수에 할당 하는 방법을 보여 줍니다.

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

그러나 **VendorSpecificDataReceived** 이벤트의 모든 데이터를 포함 한 일부 데이터는 *args* 매개 변수의 속성인 **Report** 개체를 통해 검색 되어야 합니다. [MagneticStripeReaderReport](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)유형입니다.

[Track1](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track1), [Track2](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track2), [Track3](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track3)및 [Track4](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track4)의 데이터를 해석 하는 방법을 알려주는 데 사용 [하 여 스와이프](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.cardtype) 된 카드 유형을 확인할 수 있습니다.

각 트랙의 데이터는 [MagneticStripeReaderTrackData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata) 개체로 표현 됩니다. 이 클래스에서 다음 형식의 데이터를 가져올 수 있습니다.

* [데이터](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.data): 원시 또는 디코딩된 데이터입니다.
* [DiscretionaryData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.discretionarydata): 임의 데이터입니다. 
* [EncryptedData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.encrypteddata): 암호화 된 데이터입니다.

다음 코드 조각에서는 보고서 및 트랙 데이터를 가져온 다음 카드 유형을 확인 합니다.

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
* [ClaimedMagneticStripeReader 클래스](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)
* [MagneticStripeReaderAamvaCardDataReceivedEventArgs 클래스](/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* [MagneticStripeReaderBankCardDataReceivedEventArgs 클래스](/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs 클래스](/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)
* [MagneticStripeReaderReport](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)
* [MagneticStripeReaderTrackData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)