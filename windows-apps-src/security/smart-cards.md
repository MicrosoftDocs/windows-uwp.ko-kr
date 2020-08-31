---
title: 스마트 카드
description: 이 항목에서는 UWP (유니버설 Windows 플랫폼) 앱에서 스마트 카드를 사용 하 여 사용자를 보안 네트워크 서비스에 연결 하는 방법을 설명 합니다. 예를 들어 실제 스마트 카드 판독기에 액세스 하 고, 가상 스마트 카드를 만들고, 스마트 카드와 통신 하 고, 사용자를 인증 하 고, 사용자 Pin을 다시 설정 하 고, 스마트 카드
ms.assetid: 86524267-50A0-4567-AE17-35C4B6D24745
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: 5c792cde951b2bf01585256f51f67a545d96510d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170837"
---
# <a name="smart-cards"></a>스마트 카드




이 항목에서는 UWP (유니버설 Windows 플랫폼) 앱에서 스마트 카드를 사용 하 여 사용자를 보안 네트워크 서비스에 연결 하는 방법을 설명 합니다. 예를 들어 실제 스마트 카드 판독기에 액세스 하 고, 가상 스마트 카드를 만들고, 스마트 카드와 통신 하 고, 사용자를 인증 하 고, 사용자 Pin을 다시 설정 하 고, 스마트 카드 

## <a name="configure-the-app-manifest"></a>앱 매니페스트 구성


앱이 스마트 카드 또는 가상 스마트 카드를 사용 하 여 사용자를 인증 하려면 먼저 appxmanifest.xml 파일에서 **공유 사용자 인증서** 기능을 설정 해야 합니다.

## <a name="access-connected-card-readers-and-smart-cards"></a>연결 된 카드 판독기 및 스마트 카드 액세스


[**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)에 지정 된 장치 ID를 [**smartFromIdAsync**](/uwp/api/windows.devices.smartcards.smartcardreader.fromidasync) 메서드에 전달 하 여 판독기 및 연결 된 스마트 카드를 쿼리할 수 있습니다. 반환 된 판독기 장치에 현재 연결 된 스마트 카드에 액세스 하려면 [**SmartFindAllCardsAsync**](/uwp/api/windows.devices.smartcards.smartcardreader.findallcardsasync)를 호출 합니다.

```cs
string selector = SmartCardReader.GetDeviceSelector();
DeviceInformationCollection devices =
    await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation device in devices)
{
    SmartCardReader reader =
        await SmartCardReader.FromIdAsync(device.Id);

    // For each reader, we want to find all the cards associated
    // with it.  Then we will create a SmartCardListItem for
    // each (reader, card) pair.
    IReadOnlyList<SmartCard> cards =
        await reader.FindAllCardsAsync();
}
```

또한 카드 삽입 시 앱 동작을 처리 하는 메서드를 구현 하 여 앱에서 [**추가**](/uwp/api/windows.devices.smartcards.smartcardreader.cardadded) 된 이벤트를 관찰할 수 있도록 해야 합니다.

```cs
private void reader_CardAdded(SmartCardReader sender, CardAddedEventArgs args)
{
  // A card has been inserted into the sender SmartCardReader.
}
```

그런 다음 반환 된 각 [**스마트 카드**](/uwp/api/Windows.Devices.SmartCards.SmartCard) 개체를 [**smart의 프로 비전**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) 에 전달 하 여 앱이 해당 구성에 액세스 하 고 사용자 지정할 수 있도록 하는 메서드에 액세스할 수 있습니다.

## <a name="create-a-virtual-smart-card"></a>가상 스마트 카드 만들기


[**Smart카드 프로 비전**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning)을 사용 하 여 가상 스마트 카드를 만들려면 앱에서 먼저 친근 한 이름, 관리자 키 및 [**SmartCardPinPolicy**](/uwp/api/Windows.Devices.SmartCards.SmartCardPinPolicy)을 제공 해야 합니다. 이름은 일반적으로 앱에 제공되지만, 앱이 3개 값을 모두 [**RequestVirtualSmartCardCreationAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync)에 전달하기 전에 관리자 키를 제공하고 현재 **SmartCardPinPolicy**의 인스턴스를 생성해야 합니다.

1.  SmartCardPinPolicy의 새 인스턴스를 만듭니다 [ **SmartCardPinPolicy** .](/uwp/api/Windows.Devices.SmartCards.SmartCardPinPolicy)
2.  서비스 또는 관리 도구에서 제공 하는 관리 키 값에 [**CryptographicBuffer. GenerateRandom**](/uwp/api/windows.security.cryptography.cryptographicbuffer.generaterandom) 을 호출 하 여 관리 키 값을 생성 합니다.
3.  이러한 값을 *FriendlyNameText* 문자열과 함께 [**RequestVirtualSmartCardCreationAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync)에 전달 합니다.

```cs
SmartCardPinPolicy pinPolicy = new SmartCardPinPolicy();
pinPolicy.MinLength = 6;

IBuffer adminkey = CryptographicBuffer.GenerateRandom(24);

SmartCardProvisioning provisioning = await
     SmartCardProvisioning.RequestVirtualSmartCardCreationAsync(
          "Card friendly name",
          adminkey,
          pinPolicy);
```

[**RequestVirtualSmartCardCreationAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync) 가 연결 된 [**smartcard 프로비저닝**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) 개체를 반환 하면 가상 스마트 카드가 프로 비전 되 고 사용할 준비가 됩니다.

>[!NOTE]
>UWP 앱을 사용 하 여 가상 스마트 카드를 만들려면 앱을 실행 하는 사용자가 administrators 그룹의 구성원 이어야 합니다. 사용자가 administrators 그룹의 멤버가 아닌 경우 가상 스마트 카드 만들기가 실패 합니다.

## <a name="handle-authentication-challenges"></a>인증 문제 처리


스마트 카드 또는 가상 스마트 카드를 사용 하 여 인증 하려면 앱에서 카드에 저장 된 관리 키 데이터와 인증 서버 또는 관리 도구에 의해 유지 관리 되는 관리 키 데이터 간의 문제를 해결 하는 동작을 제공 해야 합니다.

다음 코드에서는 서비스에 대 한 스마트 카드 인증을 지원 하거나 실제 또는 가상 카드 세부 정보를 수정 하는 방법을 보여 줍니다. 카드의 관리 키 ("챌린지")를 사용 하 여 생성 된 데이터가 서버 또는 관리 도구 ("adminkey")에서 제공 하는 관리 키 데이터와 동일한 경우 인증에 성공 합니다.

```cs
static class ChallengeResponseAlgorithm
{
    public static IBuffer CalculateResponse(IBuffer challenge, IBuffer adminkey)
    {
        if (challenge == null)
            throw new ArgumentNullException("challenge");
        if (adminkey == null)
            throw new ArgumentNullException("adminkey");

        SymmetricKeyAlgorithmProvider objAlg = SymmetricKeyAlgorithmProvider.OpenAlgorithm(SymmetricAlgorithmNames.TripleDesCbc);
        var symmetricKey = objAlg.CreateSymmetricKey(adminkey);
        var buffEncrypted = CryptographicEngine.Encrypt(symmetricKey, challenge, null);
        return buffEncrypted;
    }
}
```

이 항목의 나머지 부분에서이 코드를 참조 하는 것을 확인할 수 있습니다. 인증 작업을 완료 하는 방법 및 스마트 카드 및 가상 스마트 카드 정보에 변경 내용을 적용 하는 방법을 검토 합니다.

## <a name="verify-smart-card-or-virtual-smart-card-authentication-response"></a>스마트 카드 또는 가상 스마트 카드 인증 응답 확인


이제 인증 과제에 대 한 논리가 정의 되었으므로 판독기와 통신 하 여 스마트 카드에 액세스 하거나 인증을 위해 가상 스마트 카드에 액세스할 수 있습니다.

1.  챌린지를 시작 하려면 스마트 카드와 연결 된 [**SmartGetChallengeContextAsync 프로비저닝**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) 개체에서 [**GetChallengeContextAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.getchallengecontextasync) 를 호출 합니다. 그러면 카드의 [**챌린지**](/uwp/api/windows.devices.smartcards.smartcardchallengecontext.challenge) 값을 포함 하는 [**SmartCardChallengeContext**](/uwp/api/Windows.Devices.SmartCards.SmartCardChallengeContext)의 인스턴스가 생성 됩니다.

2.  그런 다음, 카드의 챌린지 값과 서비스 또는 관리 도구에서 제공 하는 관리자 키를 앞의 예제에서 정의한 **ChallengeResponseAlgorithm** 전달 합니다.

3.  [**Verifyresponseasync**](/uwp/api/windows.devices.smartcards.smartcardchallengecontext.verifyresponseasync) 는 인증에 성공한 경우 **true** 를 반환 합니다.

```cs
bool verifyResult = false;
SmartCard card = await rootPage.GetSmartCard();
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

using (SmartCardChallengeContext context =
       await provisioning.GetChallengeContextAsync())
{
    IBuffer response = ChallengeResponseAlgorithm.CalculateResponse(
        context.Challenge,
        rootPage.AdminKey);

    verifyResult = await context.VerifyResponseAsync(response);
}
```

## <a name="change-or-reset-a-user-pin"></a>사용자 PIN 변경 또는 다시 설정


스마트 카드와 연결 된 PIN을 변경 하려면:

1.  카드에 액세스 하 고 연결 된 [**smartcard 프로비저닝**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) 개체를 생성 합니다.
2.  [**RequestPinChangeAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinchangeasync) 를 호출 하 여이 작업을 완료 하는 UI를 사용자에 게 표시 합니다.
3.  PIN이 성공적으로 변경 되 면 호출에서 **true**를 반환 합니다.

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinChangeAsync();
```

PIN 재설정을 요청 하려면:

1.  [**Requestpinresetasync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinresetasync) 를 호출 하 여 작업을 시작 합니다. 이 호출에는 스마트 카드 및 pin 재설정 요청을 나타내는 [**SmartCardPinResetHandler**](/uwp/api/windows.devices.smartcards.smartcardpinresethandler) 메서드가 포함 됩니다.
2.  [**SmartCardPinResetHandler**](/uwp/api/windows.devices.smartcards.smartcardpinresethandler) 는 [**Smartcard pinreset지연**](/uwp/api/Windows.Devices.SmartCards.SmartCardPinResetDeferral) 호출에서 래핑된 **ChallengeResponseAlgorithm**에서 카드의 챌린지 값과 서비스 또는 관리 도구에서 제공 하는 관리자 키를 비교 하 여 요청을 인증 하는 데 사용 하는 정보를 제공 합니다.

3.  챌린지에 성공 하면 [**Requestpinresetasync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinresetasync) 호출이 완료 된 것입니다. PIN이 성공적으로 다시 설정 된 경우 **true** 를 반환 합니다.

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinResetAsync(
    (pinResetSender, request) =>
    {
        SmartCardPinResetDeferral deferral =
            request.GetDeferral();

        try
        {
            IBuffer response =
                ChallengeResponseAlgorithm.CalculateResponse(
                    request.Challenge,
                    rootPage.AdminKey);
            request.SetResponse(response);
        }
        finally
        {
            deferral.Complete();
        }
    });
}
```

## <a name="remove-a-smart-card-or-virtual-smart-card"></a>스마트 카드 또는 가상 스마트 카드 제거


실제 스마트 카드가 제거 되 면 카드가 삭제 될 때 [**제거**](/uwp/api/windows.devices.smartcards.smartcardreader.cardremoved) 이벤트가 발생 합니다.

카드 판독기 및 판독기 제거를 이벤트 처리기로 정의 하는 메서드와 카드 판독기를 사용 하 여이 이벤트의 발생을 연결 합니다. 이 동작은 사용자에 게 카드가 제거 되었다는 알림을 제공 하기만 하면 됩니다.

```cs
reader = card.Reader;
reader.CardRemoved += HandleCardRemoved;
```

가상 스마트 카드의 제거는 먼저 카드를 검색 한 다음 [**SmartRequestVirtualSmartCardDeletionAsync 프로 비전**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) 반환 개체에서 [**RequestVirtualSmartCardDeletionAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcarddeletionasync) 를 호출 하 여 프로그래밍 방식으로 처리 됩니다.

```cs
bool result = await SmartCardProvisioning
    .RequestVirtualSmartCardDeletionAsync(card);
```