---
author: jnHs
Description: Select the base price for an app and schedule price changes. You can also customize these options for specific markets.
title: 앱 가격 설정 및 예약
ms.author: wdg-dev-content
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 가격 책정, 앱 가격 책정, 앱 가격, 앱 판매, 가격 변경, 가격 사용자 지정, 가격, 비용, 기본 가격 재정의, 자유 형식 가격, 자유 형식
ms.localizationpriority: medium
ms.openlocfilehash: ca37d0b360679a878cff3aeabd96f82016c36fde
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/30/2018
ms.locfileid: "3120810"
---
# <a name="set-and-schedule-app-pricing"></a>앱 가격 설정 및 예약

[가격 책정 및 가용성](set-app-pricing-and-availability.md) 페이지의 **가격 책정** 섹션에서 앱의 기본 가격을 선택할 수 있습니다. 또 [가격 변경 예약](#schedule-price-changes)으로 앱 가격이 변경되는 날짜와 시간을 지정할 수 있습니다. 또한 새 기준 가격을 선택하거나 시장의 현지 통화로 자유 형식 가격을 입력하여 [특정 시장에 대한 기본 가격을 재정의](#override-base-price-for-specific-markets)할 수 있는 옵션이 있습니다.

> [!NOTE]
> 이 항목은 앱을 가리키지만, 추가 기능 제출을 위한 가격 선택은 동일한 프로세스를 사용합니다. Note는 [구독 추가 기능](../monetize/enable-subscription-add-ons-for-your-app.md)대 한 선택한 기준 가격 적이 향상 될 수 없습니다 (여부를 변경 하 여 기본 가격 또는 가격 변경 예약 하 여), 있지만 저하 될 수 있습니다.

## <a name="base-price"></a>기본 가격

앱의 **기본 가격**을 선택하면 특정 시장(들)에 대한 기본 가격을 재정의하지 않는 한 해당 가격은 앱이 판매되는 모든 지역/국가에 사용됩니다.

**기본 가격**을 **무료**로 설정하거나, 사용 가능한 기준 가격을 선택해 앱을 배포하는 모든 지역/국가를 대상으로 가격을 설정할 수 있습니다. 기준 가격은 0.99USD에서 시작하며, 추가 기준은 증가된 가격(1.09USD, 1.19USD 등)으로 제공됩니다. 증가치는 일반적으로 가격이 높아질수록 커집니다. 

> [!NOTE]
> 이러한 기준 가격은 추가 기능에도 적용됩니다. 

각 기준 가격에는 Microsoft Store에서 제공하는 60가지가 넘는 통화 각각에 대한 값이 있습니다. 이러한 값을 사용하여 전 세계적으로 비슷한 가격 포인트로 앱을 판매하도록 도와드립니다. 어떤 통화로든 기본 가격을 선택할 수 있습니다. 또 Microsoft가 다른 지역/국가에는 해당되는 가격을 자동으로 사용합니다. 때때로 통화 환율의 변경에 맞게 특정 시장에서 해당 값을 조정할 수 있습니다.

**가격 책정** 섹션에서 **변환표 보기**를 클릭하면 모든 통화로 가격을 확인할 수 있습니다. 이는 각 기준 가격과 연결된 ID 번호를 표시합니다. [Microsoft Store 제출 API](../monetize/manage-app-submissions.md#price-tiers)를 사용하여 가격을 입력할 때 필요한 번호입니다. **다운로드**를 클릭하면 기준 가격표 사본을 .csv 파일로 다운로드 받을 수 있습니다.

선택한 기준 가격에는 고객이 지불해야 하는 판매세 또는 부가가치세가 포함될 수 있습니다. 선택한 지역/국가에서 앱의 세금 관련 정보에 대해 자세히 알아보려면 [유료 앱의 세금 세부 정보](tax-details-for-paid-apps.md)를 참조하세요. 또한 [특정 지역/국가의 가격 고려 사항](define-pricing-and-market-selection.md#price-considerations-for-specific-markets)을 검토하세요.

> [!NOTE]
> [표시 여부](choose-visibility-options.md#discoverability) 섹션의 **Store에서 사용할 수 있지만 검색 되지 않는 제품으로 설정** 아래에서 **취득 중지** 옵션을 선택하면 제출에 대한 가격을 설정할 수 없습니다(앱을 무료로 다운로드하기 위해 홍보 코드를 등록하지 않는 한 누구도 앱 취득이 불가능하기 때문).

## <a name="schedule-price-changes"></a>가격 변경 예약

특정 날짜와 시간에 앱의 기본 가격을 변경하기 위해 하나 이상의 가격 변경을 예약할 수 있는 옵션이 있습니다. 

> [!IMPORTANT]
> 가격 변경은 Windows 10 장치(Xbox 포함)고객에게만 표시됩니다. 앱이 이전 OS 버전을 지원하는 경우 가격 변경이 적용되지 않습니다. 
>
> - WIndows 8 고객의 경우, 추가 가격 변경을 예약한 경우에도 앱은 항상 **기본 가격**으로 제공됩니다(지역/국가에 특정적인 가격 없음). 
> - Windows 8.1 및 Windows Phone 8.1 이전 버전을 이용하는 고객의 경우, 지역/국가를 대상으로 추가 가격 변경을 예약한 경우에도 항상 해당 지역/국가의 처음 기준 가격으로 앱이 제공됩니다.
> 
> 가격 변경을 예약할 때 유의해야 할 내용입니다. 예를 들어, 처음에 낮은 기준 가격에 앱을 출시한 후 가격을 인상할 날짜를 예약한 경우, 앱을 구입한 이전 OS 버전 고객들은 인상된 가격보다 낮은 최초 가격을 지급합니다.

가격 변경 옵션을 확인하려면 **가격 변경 예약**을 클릭합니다. 사용할 기준 가격을 선택한 다음(또는 단일 시장 기본 가격 재정의를 위해 자유 형식의 가격을 입력한 다음) 날짜, 시간 및 표준 시간대를 선택합니다.

**다시 가격 변경 예약**을 클릭해 원하는 만큼 가격 변경을 예약할 수 있습니다.

> [!NOTE]
> 예약된 가격 변경은 [할인 가격 책정](put-apps-and-add-ons-on-sale.md)과 다르게 작동합니다. 앱을 할인 판매할 경우, Microsoft Store에서 가격은 취소선으로 표시되며 고객은 선택한 기간 동안 앱을 할인된 가격으로 구매할 수 있습니다. 할인 기간이 종료되면 판매 가격은 더 이상 적용되지 않으며 앱이 기본 가격(또는 해당하는 경우 특정 시장에 지정한 다른 가격)으로 제공됩니다.
>
> 예약된 가격 변경의 경우, 가격을 인상 또는 인하할 수 있습니다. 지정한 날짜에 가격이 변경되지만 Microsoft Store에 할인이 표시되거나 특별한 포맷이 적용되지 않습니다. 앱에 새로운 기본 가격이 적용될 뿐입니다. 


## <a name="override-base-price-for-specific-markets"></a>특정 시장에 대한 기본 가격 재정의

위에서 선택한 옵션이 앱을 제공하는 모든 지역/국가에 적용되는 것이 기본값입니다. 원하는 경우 다른 기준 가격을 선택하거나 특정 지역/국가의 현지 통화로 자유 형식 가격을 입력하여 하나 또는 둘 이상의 시장에 대한 가격을 변경할 수 있습니다.

> [!IMPORTANT]
> Windows 8 고객은 지역/국가에 다른 가격을 선택한 경우에도 항상 앱의 **기본 가격**만 볼 수 있습니다.

특정 지역/국가의 가격을 변경하려면 **특정 지역/국가에 대해 기준 가격 재지정**을 선택합니다. **지역/국가 선택** 팝업 창이 앱을 사용할 수 있도록 설정한 모든 지역/국가 목록을 표시합니다. (**지역/국가** 섹션에서 제외한 지역/국가는 표시되지 않습니다.) 

한 번에 하나의 지역/국가 또는 지역/국가 그룹에 대해 기본 가격을 재정의할 수 있습니다. 이를 완료한 후 **특정 지역/국가에 대해 기준 가격 재지정**를 다시 선택하고 아래에 설명된 프로세스를 반복하여 추가 시장(또는 추가 시장 그룹)에 대한 기본 가격을 재정의할 수 있습니다. 특정 시장(또는 시장 그룹)에 대해 지정한 가격 재정의를 제거하려면 **제거**를 클릭합니다.


### <a name="override-the-base-price-for-a-single-market"></a>단일 지역/국가에 대한 기본 가격 재정의

단일 지역/국가에 대한 가격을 변경하려면 해당 지역을 선택하고 **만들기**를 클릭합니다. 그러면 위에서 설명한 **기본 가격** 및 **가격 변경 예약**이 표시되지만, 특정 지역/국가에만 선택 사항이 적용됩니다. 단일 시장에 대한 기본 가격을 재정의하기 때문에 기준 가격은 해당 시장의 현지 통화로 표시됩니다. **변환표 보기**를 클릭하면 모든 통화로 가격을 확인할 수 있습니다. 

단일 지역/국가에 대한 기본 가격을 재정의하면 선택한 시장의 현지 통화로 자유 형식의 가격 또한 입력할 수 있습니다. 표준 기준 가격 중 하나에 해당하지 않는 경우에도 최소 및 최대 범위 내에서 원하는 어떤 가격도 입력할 수 있습니다. 선택한 지역/국가에서 Windows 10(Xbox 포함)의 고객에 대해 이 가격이 사용됩니다. 

> [!IMPORTANT]
> 자유 형식 가격을 입력한 경우 (변환율을 변경하더라도) 새로운 가격으로 업데이트를 제출하지 않으면 해당 가격은 조정되지 않습니다. 

### <a name="override-the-base-price-for-a-market-group"></a>지역/국가 그룹에 대한 기본 가격 재정의

여러 지역/국가에 대해 기준 가격을 재지정하려면 *지역/국가 그룹*을 만듭니다. 포함할 지역/국가를 선택하고 원하는 경우 해당 그룹의 이름을 입력합니다. (이 이름은 참고용으로 고객은 볼 수 없습니다.) 완료되면 **만들기**를 클릭합니다. 그러면 위에서 설명한 **기본 가격** 및 **가격 변경 예약** 옵션이 표시되지만, 특정 시장 그룹에만 선택 사항이 적용됩니다. 자유 형식 가격은 지역/국가 그룹과 함께 사용할 수 없습니다. 사용 가능한 기준 가격을 선택해야 합니다.

지역/국가 그룹에 포함된 지역/국가를 변경하려면 시장 그룹의 이름을 클릭하고 원하는 모든 지역/국가를 추가 또는 제거한 다음 **확인**을 클릭하여 변경 내용을 저장합니다. 

> [!NOTE]
> 하나의 시장은 **가격 책정** 섹션에서 여러 지역/국가 그룹에 속할 수 없습니다.




