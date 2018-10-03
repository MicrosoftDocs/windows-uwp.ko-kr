---
author: jnHs
Description: The Pricing and availability page of the app submission process lets you determine how much your app will cost, whether you'll offer a free trial, and how, when, and where it will be available to customers.
title: 앱 가격 책정 및 가용성 설정
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
ms.author: wdg-dev-content
ms.date: 05/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, 가격, 사용 가능, 검색 가능, 무료 평가판, 평가판들, 평가판, 앱들, 출시 날짜
ms.localizationpriority: medium
ms.openlocfilehash: 20c52687b375f9bf33dd491eeb37d4142acace99
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2018
ms.locfileid: "4264938"
---
# <a name="set-app-pricing-and-availability"></a>앱 가격 책정 및 가용성 설정


[앱 제출 프로세스](app-submissions.md)의 **가격 책정 및 가용성** 페이지에서는 앱이 청구할 금액, 무료 평가판을 제공할지 여부, 고객에게 제공할 방법, 시간, 위치를 결정합니다. 여기에서 이 페이지의 옵션과 이 정보를 입력할 때 고려해야 하는 사항을 살펴봅니다.


## <a name="markets"></a>시장

Microsoft Store는 전 세계 200여 이상의 국가와 지역에 있는 고객에게 다가갑니다. 기본적으로 가능한 모든 지역/국가에 앱을 제공합니다. 원하는 경우, 앱을 제공할 특정 지역/국가를 선택할 수 있습니다. 

자세한 내용은 [지역/국가 선택 정의](define-pricing-and-market-selection.md)를 참조하세요.


## <a name="visibility"></a>표시 여부

**표시 여부** 섹션에서는 Store에서 앱을 찾거나 Store 목록을 볼 수 있는지 여부를 포함하여 앱을 검색 및 취득할 수 있는 방법에 대해 제한을 둘 수 있습니다.

자세한 내용은 [표시 여부 옵션 선택](choose-visibility-options.md)을 참조하세요.


## <a name="schedule"></a>일정

기본적으로 ([표시 여부](choose-visibility-options.md#discoverability) 섹션에서 **Store에서 사용할 수 있지만 검색되지 않는 앱으로 설정** 옵션을 선택한 경우를 제외하고) 앱 인증을 통과하고 게시 프로세스가 완료되는 즉시 고객이 앱을 사용할 수 있습니다. 다른 날짜를 선택 하려면 **옵션 표시**를 선택해 이 섹션을 확장하세요. 

자세한 내용은 [정확한 릴리스 일정 구성](configure-precise-release-scheduling.md)을 참조하세요.


## <a name="pricing"></a>가격 책정

**무료** 또는 사용 가능한 기준 가격 중 하나를 선택하여 앱의 기본 가격을 선택해야 합니다([표시 여부](choose-visibility-options.md#discoverability) 섹션의 **Store에서 사용할 수 있지만 검색되지 않는 앱으로 설정** 아래에서 **취득 중지** 옵션을 선택하지 않는 한). 또 가격 변경 예약으로 앱 가격이 변경되는 날짜와 시간을 지정할 수 있습니다. 여기에 더해, 특정 지역/국가의 가격 변경을 사용자 지정할 수 있는 옵션도 있습니다. 

자세한 내용은 [앱 가격 설정 및 예약](set-and-schedule-app-pricing.md)을 참조하세요.


## <a name="free-trial"></a>무료 평가판

대부분 개발자는 Microsoft Store에서 제공되는 평가판 기능을 통해 고객이 앱을 무료로 체험할 수 있도록 합니다. 기본적으로 **무료 평가판 없음**이 선택되어 있습니다. 앱에 무료 평가판이 없는 상태입니다. 무료 평가판을 제공하고 싶다면, **무료 평가판** 드롭다운에서 값을 선택할 수 있습니다.

두 가지 유형의 평가판을 선택할 수 있으며, 무료 평가판 시작/종료 날짜와 시간을 구성할 수 있는 옵션이 있습니다.

### <a name="time-limited"></a>시간 제한

**시간 제한**을 선택해 고객이 특정 기간 동안 앱을 무료로 사용할 수 있도록 지정할 수 있습니다. **1 일**, **7 일**, **15 일**, 또는 **30 일** 중에 선택할 수 있습니다. [평가판의 기능을 제한하거나 없애는](../monetize/in-app-purchases-and-trials.md) 코드를 추가해 기능을 제한할 수 있습니다. 또는 평가 기간에 고객이 모든 기능에 액세스할 수 있도록 구성할 수 있습니다. 
> [!NOTE]
> 시간 제한 평가판은 Windows 10 빌드 10.0.10586 이전, Windows Phone 8.1 이전 버전을 사용하는 고객에게는 표시되지 않습니다.

### <a name="unlimited"></a>제한 없음

**제한 없음**을 선택하면 고객이 제한 없이 앱을 무료로 사용할 수 있습니다. 그러나 개발자는 고객이 처음 사용자용 버전을 구입하도록 권장하기를 원할 것이므로 [평가판 버전에서 기능을 제외하거나 제한](../monetize/in-app-purchases-and-trials.md)하는 코드를 추가하는 것이 좋습니다.

### <a name="start-and-end-dates"></a>시작 및 종료 날짜

기본적으로 앱이 게시된 즉시 평가판을 사용할 수 있으며, 평가판이 계속 제공됩니다. 원할 경우, 평가판 제공을 시작하고 종료할 날짜 및 시간을 지정할 수 있습니다. 

>[!NOTE]
> 이 날짜는 Windows 10(Xbox 포함) 고객에게만 적용됩니다. 이전 OS 버전 고객이 앱을 사용할 수 있는 경우, 제품을 사용할 수 있는 한 해당 고객에게 계속 평가판이 제공됩니다. 

Windows 10의 고객에게 평가판을 제공할 날짜를 설정하려면 **시작** 및 **종료** 드롭다운 메뉴를 **at**으로 변경한 후, 날짜 및 시간을 선택합니다. 이렇게 할 경우, **UTC**를 선택해 선택 시간을 UTC(Universal Coordinated Time)으로 설정하거나, **로컬**을 선택해 특정 지역/국가의 표준 시간대에 사용되는 시간으로 설정할 수 있습니다. (시간대가 하나 이상인 지역/국가의 경우, 하나의 시간대만 사용합니다. 미국에서는 동부 표준 시간대를 사용 합니다.) 모든 지역/국가 대 한 다른 날짜를 설정 하려면 **특정 지역/국가 대 한 사용자 지정** 을 선택할 수 있습니다.


## <a name="sale-pricing"></a>할인 판매 가격 책정

한정된 기간 동안 앱을 할인된 가격으로 제공하려는 경우 할인 판매를 만들고 예약할 수 있습니다.

자세한 내용은 [앱 및 추가 기능 할인 판매](put-apps-and-add-ons-on-sale.md)를 참조하세요.


## <a name="organizational-licensing"></a>조직 라이선스

기본적으로 앱은 대량 구매하는 조직에 제공될 수 있습니다. 이 섹션에서는 앱을 제공할지 여부 및 제공하는 방법을 지정할 수 있습니다.

자세한 내용은 [조직 라이선스 옵션](organizational-licensing.md)을 참조하세요.


## <a name="publish-date"></a>게시 날짜

이전에는 **게시 날짜** 섹션이 이 페이지에 표시되었습니다. **게시 고정 옵션** 섹션의 [제출 옵션](manage-submission-options.md) 페이지에서 이 기능을 찾아볼 수 있습니다 (Store에 앱을 게시해야 하는 경우를 제어하려면 **가격 책정 및 가용성** 페이지의 [일정](configure-precise-release-scheduling.md) 섹션을 사용하는 것이 좋음).


