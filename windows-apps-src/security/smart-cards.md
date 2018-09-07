---
title: 스마트 카드
description: 이 항목에서는 물리적 스마트 카드 판독기에 액세스하고, 가상 스마트 카드를 만들고, 스마트 카드와 통신하고, 사용자를 인증하고, 사용자 PIN을 다시 설정하고 스마트 카드를 제거하거나 분리하는 방법을 포함하여 UWP(유니버설 Windows 플랫폼) 앱에서 스마트 카드를 사용하여 사용자를 보안 네트워크 서비스에 연결할 수 있는 방법을 설명합니다.
ms.assetid: 86524267-50A0-4567-AE17-35C4B6D24745
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: dda2b580a82c72ad2e31c771a9c76f8d770049ec
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/07/2018
ms.locfileid: "3660931"
---
# <a name="smart-cards"></a>스마트 카드




이 항목에서는 물리적 스마트 카드 판독기에 액세스하고, 가상 스마트 카드를 만들고, 스마트 카드와 통신하고, 사용자를 인증하고, 사용자 PIN을 다시 설정하고 스마트 카드를 제거하거나 분리하는 방법을 포함하여 UWP(유니버설 Windows 플랫폼) 앱에서 스마트 카드를 사용하여 사용자를 보안 네트워크 서비스에 연결할 수 있는 방법을 설명합니다. 

## <a name="configure-the-app-manifest"></a>앱 매니페스트 구성


앱에서 스마트 카드 또는 가상 스마트 카드를 사용하여 사용자를 인증하려면 먼저 프로젝트 Package.appxmanifest 파일에서 **공유 사용자 인증서** 기능을 설정해야 합니다.

## <a name="access-connected-card-readers-and-smart-cards"></a>연결된 카드 판독기 및 스마트 카드 액세스


[**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393)에 지정된 디바이스 ID를 [**SmartCardReader.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn263890) 메서드에 전달하여 판독기 및 연결된 스마트 카드를 쿼리할 수 있습니다. 현재 반환된 판독기 디바이스에 연결된 스마트 카드에 액세스하려면 [**SmartCardReader.FindAllCardsAsync**](https://msdn.microsoft.com/library/windows/apps/dn263887)를 호출합니다.

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

또한 카드 삽입 시의 앱 동작을 처리하는 메서드를 구현하여 앱이 [**CardAdded**](https://msdn.microsoft.com/library/windows/apps/dn263866) 이벤트를 관찰할 수 있도록 해야 합니다.

```cs
private void reader_CardAdded(SmartCardReader sender, CardAddedEventArgs args)
{
  // A card has been inserted into the sender SmartCardReader.
}
```

그런 다음 반환된 각 [**SmartCard**](https://msdn.microsoft.com/library/windows/apps/dn297565) 개체를 [**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801)에 전달하여 앱이 해당 구성을 액세스하고 사용자 지정할 수 있도록 하는 메서드에 액세스할 수 있습니다.

## <a name="create-a-virtual-smart-card"></a>가상 스마트 카드 만들기


[**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801)을 사용하여 가상 스마트 카드를 만들려면 앱에서 먼저 이름, 관리자 키 및 [**SmartCardPinPolicy**](https://msdn.microsoft.com/library/windows/apps/dn297642)를 제공해야 합니다. 이름은 일반적으로 앱에 제공되지만, 앱이 3개 값을 모두 [**RequestVirtualSmartCardCreationAsync**](https://msdn.microsoft.com/library/windows/apps/dn263830)에 전달하기 전에 관리자 키를 제공하고 현재 **SmartCardPinPolicy**의 인스턴스를 생성해야 합니다.

1.  새 [**SmartCardPinPolicy**](https://msdn.microsoft.com/library/windows/apps/dn297642) 인스턴스 만들기
2.  서비스 또는 관리 도구에서 제공한 관리자 키 값에 대해 [**CryptographicBuffer.GenerateRandom**](https://msdn.microsoft.com/library/windows/apps/br241392)을 호출하여 관리자 키 값을 생성합니다.
3.  이러한 값을 *FriendlyNameText* 문자열과 함께 [**RequestVirtualSmartCardCreationAsync**](https://msdn.microsoft.com/library/windows/apps/dn263830)에 전달합니다.

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

[**RequestVirtualSmartCardCreationAsync**](https://msdn.microsoft.com/library/windows/apps/dn263830)에서 연결된 [**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801) 개체를 반환하면 가상 스마트 카드가 프로비전되고 사용할 준비가 됩니다.

## <a name="handle-authentication-challenges"></a>인증 질문 처리


스마트 카드 또는 가상 스마트 카드를 사용하여 인증하려면 앱에서 카드에 저장된 관리자 키 데이터와 인증 서버 또는 관리 도구에서 유지된 관리자 키 데이터 간의 질문을 완료하는 동작을 제공해야 합니다.

다음 코드에서는 물리적 카드 또는 가상 카드의 정보 수정이나 서비스에 대해 스마트 카드 인증을 지원하는 방법을 보여 줍니다. 카드의 관리자 키를 사용하여 생성된 데이터("challenge")가 서버 또는 관리 도구에서 제공한 관리 키 데이터("adminkey")와 같으면 인증에 성공합니다.

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

이 코드는 이 항목의 나머지 부분 전체에서 참조되며, 인증 작업을 완료하는 방법과 스마트 카드 및 가상 스마트 카드 정보 변경을 적용하는 방법을 검토합니다.

## <a name="verify-smart-card-or-virtual-smart-card-authentication-response"></a>스마트 카드 또는 가상 스마트 카드 인증 응답 검증


이제 인증 질문에 대한 논리가 정의되었으므로 판독기와 통신하여 인증을 위해 스마트 카드 또는 가상 스마트 카드에 액세스할 수 있습니다.

1.  질문을 시작하려면 스마트 카드와 연결된 [**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801) 개체에서 [**GetChallengeContextAsync**](https://msdn.microsoft.com/library/windows/apps/dn263811)를 호출합니다. 그러면 카드의 [**Challenge**](https://msdn.microsoft.com/library/windows/apps/dn297578) 값이 포함된 [**SmartCardChallengeContext**](https://msdn.microsoft.com/library/windows/apps/dn297570) 인스턴스가 생성됩니다.

2.  카드의 질문 값과 서비스 또는 관리 도구에서 제공한 관리자 키를 이전 예제에서 정의한 **ChallengeResponseAlgorithm**에 전달합니다.

3.  인증에 성공하면 [**VerifyResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn297627)에서 **true**를 반환합니다.

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

## <a name="change-or-reset-a-user-pin"></a>사용자 PIN 변경 또는 초기화


스마트 카드와 연결된 PIN을 변경하려면

1.  카드에 액세스하고 연결된 [**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801) 개체를 생성합니다.
2.  [**RequestPinChangeAsync**](https://msdn.microsoft.com/library/windows/apps/dn263823)를 호출하여 이 작업을 완료하는 UI를 사용자에게 표시합니다.
3.  PIN이 변경되었으면 호출에서 **true**를 반환합니다.

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinChangeAsync();
```

PIN 초기화를 요청하려면

1.  [**RequestPinResetAsync**](https://msdn.microsoft.com/library/windows/apps/dn263825)를 호출하여 작업을 시작합니다. 이 호출에는 스마트 카드와 PIN 초기화 요청을 나타내는 [**SmartCardPinResetHandler**](https://msdn.microsoft.com/library/windows/apps/dn297701) 메서드가 포함되어 있습니다.
2.  [**SmartCardPinResetHandler**](https://msdn.microsoft.com/library/windows/apps/dn297701)는 [**SmartCardPinResetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn297693) 호출에 래핑된 **ChallengeResponseAlgorithm**에서 요청을 인증하기 위해 카드의 질문 값과 서비스 또는 관리 도구에서 제공한 관리자 키를 비교하는 데 사용되는 정보를 제공합니다.

3.  질문에 성공하면 [**RequestPinResetAsync**](https://msdn.microsoft.com/library/windows/apps/dn263825) 호출이 완료되고, PIN이 초기화되었으면 **true**를 반환합니다.

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


물리적 스마트 카드를 제거하는 경우 카드를 삭제할 때 [**CardRemoved**](https://msdn.microsoft.com/library/windows/apps/dn263875) 이벤트가 발생합니다.

카드 또는 판독기 제거 시의 앱 동작을 이벤트 처리기로 정의하는 메서드를 사용하여 이 이벤트 발생을 카드 판독기와 연결합니다. 이 동작은 카드가 제거되었다는 알림을 사용자에게 제공하는 것처럼 간단할 수 있습니다.

```cs
reader = card.Reader;
reader.CardRemoved += HandleCardRemoved;
```

가상 스마트 카드 제거는 카드를 먼저 검색한 다음 [**SmartCardProvisioning**](https://msdn.microsoft.com/library/windows/apps/dn263801) 반환 개체에서 [**RequestVirtualSmartCardDeletionAsync**](https://msdn.microsoft.com/library/windows/apps/dn263850)를 호출하여 프로그래밍 방식으로 처리됩니다.

```cs
bool result = await SmartCardProvisioning
    .RequestVirtualSmartCardDeletionAsync(card);
```