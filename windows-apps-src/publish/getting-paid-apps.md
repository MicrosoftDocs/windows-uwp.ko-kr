---
Description: Learn about receiving payments for your apps, add-ons (in-app products), and advertising earnings.
title: 지급 받기
ms.assetid: 37D1EF45-C4A8-4849-8819-3D4A4898215C
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, 결제, 앱 판매, 앱 수익, 지급액, Microsoft Store 요금, 지급 보류, 백분율
ms.localizationpriority: medium
ms.openlocfilehash: 91289948f2f4745456b9cebe587cf75366a4877b
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8730726"
---
# <a name="getting-paid"></a>지급 받기
앱, 추가 기능 및 광고 수익에 대 한 지급을 수신 하는 방법에 대 한 중요 한 정보는 다음과 같습니다.

> [!IMPORTANT]
> Microsoft Store의 앱 판매에서 번 돈을 받으려면, 먼저 [지급 계좌를 설정 하 고 필요한 세금 양식을 입력](setting-up-your-payout-account-and-tax-forms.md)해야 합니다.

## <a name="store-fee"></a>Microsoft Store 수수료

[개발자 계정에 등록](http://go.microsoft.com/fwlink/p/?LinkID=615100)할 때 [앱 개발자 계약](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)에 동의합니다. 이 계약에서는 Microsoft에서 각 판매에 대해 청구하는 스토어 수수료를 포함하여 Microsoft Store에서 앱을 판매할 때 적용되는 개발자와 Microsoft 간의 관계에 대해 설명합니다.

대부분의 경우 Microsoft Store 수수료는 30%입니다. 수수료는 [앱 개발자 계약](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)에 공식적으로 정의됩니다. 질문이 있으면 항상 해당 문서를 검토하세요.

Microsoft Store 수수료는 추가 기능을 포함하여 Microsoft Store가 수금하는 모든 앱 매출에 적용됩니다.


## <a name="price-tiers"></a>기준 가격

선택한 기준 가격(들)은 앱을 배포하기로 한 모든 국가에서 [판매가](set-and-schedule-app-pricing.md#base-price)를 설정합니다. [시장마다 다른 가격 선택](set-and-schedule-app-pricing.md#override-base-price-for-specific-markets) 또는 [앱 판매](put-apps-and-add-ons-on-sale.md) 같은 추가적인 가격 기능을 사용할 수도 있습니다.

무료 앱을 제공하거나 고객이 앱 취득을 위해 지불해야 하는 가격을 선택할 수 있습니다. 기준 가격은 미화 0.99달러에서 시작하여 점차적으로 증가합니다(1.09달러, 1.19달러 등). 가격이 높아질수록 기준 가격 간 증분도 커집니다.

> [!NOTE] 
> 이러한 기준 가격은 앱 내에서 제공하는 추가 기능에도 적용됩니다.

각 기준 가격에는 Microsoft Store에서 제공하는 통화 각각에 해당하는 값이 포함되어 있습니다. 이러한 값을 사용하여 전 세계적으로 비슷한 가격 포인트로 앱을 판매하도록 도와드립니다. 그렇지만 환율 변동 때문에 정확한 판매가가 통화마다 약간 다를 수 있습니다.

사용자가 선택한 특정 지역/국가 현지 통화로 자유 형식 가격을 입력할 수 있는 옵션도 있습니다. 자유 형식 가격을 입력한 경우 (변환율을 변경하더라도) 새로운 가격으로 업데이트를 제출하지 않으면 해당 가격은 조정되지 않습니다. 

선택한 가격에는 고객이 지불해야 하는 판매세 또는 부가가치세가 포함될 수 있습니다. 자세한 내용은 [유료 앱의 세금 세부 정보](tax-details-for-paid-apps.md)를 참조하세요.


## <a name="payout-reporting"></a>지급 보고

결제 정보에 대 한 세부 정보에 액세스할 수 있으며 [파트너 센터](https://partner.microsoft.com/dashboard)의 **지급 요약** 에 보고서를 다운로드할 수 있습니다. 여기에 표시된 정보와 Microsoft에서 개발자가 번 돈을 분류하는 방법에 대한 자세한 내용은 [지급 요약](payout-summary.md)을 참조하세요.


## <a name="payout-timeframe"></a>지급 기간

해당 결제 임계값이 충족되고 아래에 설명된 대로 지급을 보류하지 않은 경우 결제는 월 단위로 이루어집니다. 일반적으로 해당 월의 15일에 지정된 월에 대한 결제 대금을 보냅니다. 결제는 일반적으로 영업일 3~10일 내에 지급 계좌로 입금됩니다. 자세한 내용은 [지급 임계값, 방법 및 기간](payment-thresholds-methods-and-timeframes.md)을 참조하세요.


##  <a name="payout-hold-status"></a>지급 보류 상태

기본적으로 위에서 설명한 대로 월 단위로 결제 대금을 보냅니다. 그러나 지급을 보류하여 결제 대금을 계좌로 보내지 않도록 할 수 있습니다. 지급을 보류하도록 선택하는 경우에도 계속해서 모든 수익을 기록하고 **지급 요약**에 세부 정보를 제공합니다. 그러나 보류를 제거할 때까지 결제 대금을 계좌로 보내지 않습니다. 

지급을 보류하려면 **계정 설정**으로 이동합니다. **재무 정보** 아래의 **지급 보류 상태** 섹션에서 슬라이더를 **켜짐**으로 전환합니다. 언제든지 지급 보류 상태를 변경할 수 있지만 결정은 다음 월별 지급에 적용된다는 것에 유의하세요. 예를 들어 4월 지급을 보류하려는 경우 3월 말 전에 지급 보류 상태를 **켜짐**으로 설정해야 합니다.

지급 보류 상태를 **켜짐**으로 설정한 후에는 슬라이더를 다시 **꺼짐**으로 전환할 때까지 모든 지급이 보류됩니다. 다시 전환하면 다음 월별 지급 주기 중에 포함됩니다(해당 지급 임계값이 충족된 경우). 예를 들어 지급을 보류했지만 6월에 지급이 생성되게 하려는 경우 5월 말 전에 지급 보류 상태를 **꺼짐**으로 전환해야 합니다.

> [!NOTE]
> **지급 보류 상태** 선택에 파트너 센터 (Microsoft Store, 광고, Azure Marketplace 등)에서 Windows 개발자 프로그램을 통해 지급 되는 **모든** 수입원에 적용 됩니다. 각 수입원에 대해 다른 보류 상태를 선택할 수 없습니다.


 

 




