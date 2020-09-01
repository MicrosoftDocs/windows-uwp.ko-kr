---
Description: 앱, 추가 기능 (앱 내 제품) 및 광고 수입에 대 한 지불을 받는 방법에 대해 알아봅니다.
title: 지급 받기
ms.assetid: 37D1EF45-C4A8-4849-8819-3D4A4898215C
ms.date: 05/29/2020
ms.topic: article
keywords: windows 10, uwp, 지불, 앱 판매, 앱 진행, 지급, 매장 요금, 지급 보류, 백분율
ms.localizationpriority: medium
ms.openlocfilehash: 5e2b67984c43d799f0f4e3a1c6662b57bdc6ae9d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167397"
---
# <a name="getting-paid"></a>지급 받기
앱, 추가 기능 및 광고 수입에 대 한 결제를 수신 하는 방법에 대 한 몇 가지 중요 한 정보는 다음과 같습니다.

> [!IMPORTANT]
> Microsoft Store의 앱 판매에서 돈을 받으려면 먼저 [지급 계정을 설정 하 고 필요한 세금 양식을 작성](setting-up-your-payout-account-and-tax-forms.md)해야 합니다.

> [!NOTE]
> 지급 계정 구성, 지급 누락, 지급 보류 등을 포함하여 지급과 관련된 지원이 필요한 경우 [여기](https://developer.microsoft.com/windows/support)의 지원에 문의하세요.

## <a name="store-fee"></a>매장 수수료

[개발자 계정에 등록](https://developer.microsoft.com/store/register)하면 [앱 개발자 계약](/legal/windows/agreements/app-developer-agreement)에 동의 하는 것입니다. 본 계약은 Microsoft에서 판매 하는 모든 판매에 대해 요금을 지불 하는 매장 요금을 포함 하 여 Microsoft Store의 앱 판매와 관련 하 여 사용자와 Microsoft 간의 관계를 설명 합니다.

요금은 [앱 개발자 계약](/legal/windows/agreements/app-developer-agreement)에 공식적으로 정의 되어 있습니다. 질문이 있는 경우 항상 해당 문서를 검토합니다.

매장 요금은 추가 기능을 포함 하 여 Microsoft Store에 의해 수집 된 모든 앱 판매에 적용 됩니다.


## <a name="price-tiers"></a>가격 책정 계층

선택한 가격 책정 계층은 앱을 배포 하도록 선택 하는 모든 국가의 [판매 가격](set-and-schedule-app-pricing.md#base-price) 을 설정 합니다. 또한  [다른 시장에 대해 다양 한 가격을 선택](set-and-schedule-app-pricing.md#override-base-price-for-specific-markets) 하거나 [판매에 앱을 배치](put-apps-and-add-ons-on-sale.md)하는 등의 추가 가격 책정 기능을 사용할 수 있습니다.

앱을 무료로 제공 하거나, 고객이 요금을 지불 해야 하는 가격을 선택 하 여 앱을 확보할 수 있습니다. 가격 책정 계층은 .99 USD부터 추가 증분(1.09 USD, 1.19 USD 등)으로 시작합니다. 가격 책정 계층 간의 증분은 가격이 높아질수록 증가합니다.

> [!NOTE] 
> 이러한 가격 책정 계층은 앱 내에서 제공 하는 추가 기능에도 적용 됩니다.

각 가격 책정 계층에는 매장에서 제공하는 각 통화에 해당하는 값이 있습니다. 이러한 값을 사용 하 여 전 세계의 비슷한 가격 지점에서 앱을 판매할 수 있습니다. 그러나 환율이 변경되기 때문에 정확한 판매량은 통화마다 약간씩 다를 수 있습니다. 환율이 매월 계산 됩니다. 트랜잭션이 발생 한 시기에 따라 적절 한 환율이 적용 됩니다. 환율 및이에 해당 하는 날짜 범위는 각각 exchangeRate 및 exchangeRateDate 열의 지급 보고서에 표시 됩니다.

또한 특정 시장의 현지 통화로 선택한 무료 형식 가격을 입력할 수 있는 옵션도 제공 됩니다. 이 작업을 수행하는 경우 새 가격으로 업데이트를 제출하지 않으면 가격은 조정되지 않습니다(전환율이 변경되는 경우에도). 

선택한 가격은 고객이 지불해야 하는 판매 또는 부가가치세를 포함할 수 있습니다. [유료 앱에 대 한 세금 세부 정보](tax-details-for-paid-apps.md) 를 참조 하세요.


## <a name="payout-reporting"></a>지급 보고

지급 정보에 대한 세부 정보에 액세스하고 [파트너 센터](https://partner.microsoft.com/dashboard)의 **지급 요약**에서 보고서를 다운로드할 수 있습니다. 여기에 표시 된 정보 및 얻게 되는 금액을 범주화 하는 방법에 대 한 자세한 내용은 [지급 summary](payout-summary.md)를 참조 하세요.


## <a name="payout-timeframe"></a>지급 기간

월 기준으로 지급 됩니다 (해당 결제 임계값이 충족 되 고 아래 설명 된 대로 지급을 보류 중으로 설정 하지 않은 경우). 일반적으로 해당 월의 15일을 기준으로 지정된 월에 발생한 모든 지급액을 보냅니다. 지급액은 일반적으로 지급 계정에 도달할 때까지 영업일 기준 3일에서 10일까지 소요됩니다. 자세한 내용은 [지불 임계값, 방법 및](payment-thresholds-methods-and-timeframes.md)기간을 참조 하세요.


##  <a name="payout-hold-status"></a>지급 보류 상태

기본적으로 위에서 설명한 대로 월 기준으로 지불을 보냅니다. 그러나 프로그램에 대한 지급을 보류하여 계정에 지급액을 보내지 못하게 하는 옵션이 있습니다. 지급를 보류 중으로 설정 하는 경우 계속 해 서 획득 한 수익을 기록 하 고 **지급 요약**에 세부 정보를 제공 합니다. 그러나 보류를 제거할 때 까지는 계정에 대 한 지불을 보내지 않습니다.

지급을 보류하려면 **개발자 설정**으로 이동합니다. **지급 및 세금 프로필 할당** 섹션의 **지급 및 세금**에서 지급을 보류할 프로그램을 찾습니다. **지급 보류** 확인란을 클릭하여 이 프로그램에 대한 지급을 보류합니다. 언제든지 지급 보류 상태를 변경할 수 있지만, 결정은 다음 월간 지급에 영향을 줍니다. 예를 들어 4 월의 지급을 유지 하려면 3 월의 끝 이전에 지급 보류 상태를 **On** 으로 설정 해야 합니다.

지급 보류 상태를 **설정**으로 지정하면 슬라이더를 다시 **해제**로 전환할 때까지 이 프로그램에 대한 모든 지급이 보류됩니다. 이렇게 하면 다음 월간 지급 주기 중에 포함 됩니다 (해당 하는 지불 임계값이 충족 되는 경우). 예를 들어 지급가 보류 중이지만 6 월에 지급을 생성 하려는 경우 끝이 끝나기 전에 지급 보류 상태를 **Off** 로 설정 해야 합니다.

> [!NOTE]
> **지급 보류 상태**는 각 프로그램에 개별적으로 적용됩니다(Microsoft Store, 광고, Azure Marketplace 등). 모든 프로그램에 대한 지급을 보류하려는 경우 각 프로그램에 대해 개별적으로 지급을 보류해야 합니다.


 

 