---
description: Windows. Store 네임 스페이스를 사용 하 여 구독 추가 기능을 구현 하는 방법을 알아봅니다.
title: 앱에 구독 추가 기능을 사용하도록 설정
keywords: windows 10, uwp, 구독, 추가 기능, 앱 내 구매, IAPs, Windows. Service 스토어
ms.date: 12/06/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 844af95545e34dab8adb6698624fcd0dccb2ab30
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362806"
---
# <a name="enable-subscription-add-ons-for-your-app"></a>앱에 구독 추가 기능을 사용하도록 설정

UWP (유니버설 Windows 플랫폼) 앱은 고객에 게 *구독* 추가 기능의 앱 내 구매를 제공할 수 있습니다. 구독을 사용 하 여 앱의 디지털 제품 (예: 앱 기능 또는 디지털 콘텐츠)을 자동화 된 반복 청구 기간으로 판매할 수 있습니다.

> [!NOTE]
> 앱에서 구독 추가 기능을 구매할 수 있도록 하려면 프로젝트가 **Windows 10 기념일 Edition (10.0;)을 대상으로 해야 합니다. 빌드 14393)** 또는 Visual Studio의 이후 버전 (windows 10, 버전 1607에 해당) **및 Windows의** api를 사용 하 여 Windows. **applicationmodel. store** 네임 스페이스 대신 앱 내 구매 환경을 구현 해야 합니다. 이러한 네임 스페이스 간의 차이점에 대 한 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)을 참조 하세요.

## <a name="feature-highlights"></a>주요 기능

UWP 앱에 대 한 구독 추가 기능은 다음 기능을 지원 합니다.

* 구독 기간으로 1 개월, 3 개월, 6 개월, 1 년 또는 2 년 중에서 선택할 수 있습니다.
* 구독에 1 주 또는 1 개월의 무료 평가판 기간을 추가할 수 있습니다.
* Windows SDK는 앱에서 사용할 수 있는 구독 추가 기능에 대 한 정보를 가져오고 구독 추가 기능을 구매할 수 있도록 하는 데 사용할 수 있는 [api를 제공](#code-examples) 합니다. 또한 [사용자에 대 한 구독을 관리](#manage-subscriptions)하기 위해 서비스에서 호출할 수 있는 REST api를 제공 합니다.
* 지정 된 기간에 구독 획득, 활성 구독자 및 취소 된 구독 수를 제공 하는 분석 보고서를 볼 수 있습니다.
* 고객은 해당 [https://account.microsoft.com/services](https://account.microsoft.com/services) Microsoft 계정에 대 한 페이지에서 구독을 관리할 수 있습니다. 고객은이 페이지를 사용 하 여 획득 한 모든 구독을 확인 하 고, 구독을 취소 하 고, 구독과 연결 된 지불 형식을 변경할 수 있습니다.

## <a name="steps-to-enable-a-subscription-add-on-for-your-app"></a>앱에 대 한 구독 추가 기능을 사용 하도록 설정 하는 단계

앱에서 구독 추가 기능을 구매할 수 있도록 설정 하려면 다음 단계를 수행 합니다.

1. 파트너 센터에서 구독에 대 한 [추가 기능 제출을 만들고](../publish/add-on-submissions.md) 제출을 게시 합니다. 추가 기능 제출 프로세스를 수행 하는 동안 다음 속성에 주의를 기울여야 합니다.

    * [제품 유형](../publish/set-your-add-on-product-id.md#product-type): **구독**을 선택 했는지 확인 합니다.

    * [구독 기간](../publish/enter-add-on-properties.md#subscription-period): 구독에 대 한 되풀이 청구 기간을 선택 합니다. 추가 기능을 게시 한 후에는 구독 기간을 변경할 수 없습니다.

        각 구독 추가 기능에서 단일 구독 기간 및 평가 기간을 지원 합니다. 앱에서 제공 하려는 각 구독 유형에 대해 다른 구독 추가 기능을 만들어야 합니다. 예를 들어 평가판을 사용 하지 않고 월간 구독을 제공 하려는 경우 1 개월 평가판을 사용 하는 월간 구독, 평가판이 없는 연간 구독 및 1 개월 평가판을 사용 하는 연간 구독을 제공 하려면 4 개의 구독 추가 기능을 만들어야 합니다.

    * [평가 기간](../publish/enter-add-on-properties.md#free-trial): 사용자가 구독 콘텐츠를 구매 하기 전에 사용해 볼 수 있도록 구독에 대해 1 주 또는 1 개월 평가 기간을 선택 하는 것이 좋습니다. 구독 추가 기능을 게시 한 후에는 평가 기간을 변경 하거나 제거할 수 없습니다.

        구독에 대 한 무료 평가판을 얻으려면 사용자는 유효한 지불 형식을 포함 하 여 표준 앱 내 구매 프로세스를 통해 구독을 구입 해야 합니다. 평가 기간 중에는 비용이 청구 되지 않습니다. 평가판 기간이 종료 되 면 구독은 자동으로 전체 구독으로 변환 되 고, 사용자의 결제 방법은 유료 구독의 첫 번째 기간에 대해 요금이 청구 됩니다. 사용자가 평가 기간 동안 구독을 취소 하기로 선택 하는 경우 구독은 평가 기간이 끝날 때까지 활성 상태로 유지 됩니다. 일부 평가판 기간은 일부 구독 기간에 사용할 수 없습니다.

        > [!NOTE]
        > 각 고객은 한 번만 구독 추가 기능에 대 한 무료 평가판을 얻을 수 있습니다. 고객이 구독에 대 한 무료 평가판을 획득 한 후에는 동일한 고객이 동일한 무료 평가판 구독을 다시 획득 하지 못하도록 방지 합니다.

    * [표시 유형](../publish/set-add-on-pricing-and-availability.md#visibility): 구독에 대 한 앱 내 구매 환경을 테스트 하는 데만 사용 하는 테스트 추가 기능을 만드는 경우 **저장소 옵션에서 숨겨진** 중 하나를 선택 하는 것이 좋습니다. 그렇지 않으면 시나리오에 가장 적합 한 표시 유형 옵션을 선택할 수 있습니다.

    * [가격 책정](../publish/set-add-on-pricing-and-availability.md?#pricing):이 섹션에서 구독의 가격을 선택 합니다. 추가 기능을 게시 한 후에는 구독 가격을 높일 수 없습니다. 그러나 나중에 가격을 낮출 수 있습니다.
        > [!IMPORTANT]
        > 기본적으로 추가 기능을 만들 때 가격은 처음에 **Free**로 설정 됩니다. 추가 기능 제출을 완료 한 후에는 구독 추가 기능에 대 한 가격을 높일 수 없으므로 여기에서 구독의 가격을 선택 해야 합니다.

2. 앱에서 [**Windows**](/uwp/api/windows.services.store) 의 api를 사용 하 여 현재 사용자가 구독 추가 기능을 이미 확보 했는지 확인 한 다음 앱에서 바로 구매로 사용자를 판매 하도록 제공 합니다. 자세한 내용은이 문서의 [코드 예제](#code-examples) 를 참조 하세요.

3. 앱에서 구독의 앱 내 구매 구현을 테스트 합니다. 테스트에 라이선스를 사용 하려면 스토어에서 개발 장치로 앱을 한 번 다운로드 해야 합니다. 자세한 내용은 앱에서 바로 구매에 대 한 [테스트 가이드](in-app-purchases-and-trials.md#testing) 를 참조 하세요.  

4. 테스트 된 코드를 포함 하 여 업데이트 된 앱 패키지를 포함 하는 앱 제출을 만들고 게시 합니다. 자세한 내용은 [앱 제출](../publish/app-submissions.md)을 참조 하세요.

<span id="code-examples"/>

## <a name="code-examples"></a>코드 예제

이 섹션의 코드 예제에서는 현재 앱에 대 한 구독 추가 기능에 대 한 정보를 가져오고 현재 사용자를 대신 하 여 구독 추가 기능 구매를 요청 하는 데 사용 하는 방법을 보여 [**줍니다.**](/uwp/api/windows.services.store)

이러한 예제에는 다음과 같은 필수 구성 요소가 있습니다.
* **Windows 10 기념일 Edition (10.0;)을 대상으로 하는 UWP (유니버설 Windows 플랫폼) 앱 용 Visual Studio 프로젝트입니다. 빌드 14393)** 이상 릴리스
* 파트너 센터에서 [앱 제출을 만들었으며](../publish/app-submissions.md) 이 앱은 스토어에 게시 됩니다. 응용 프로그램을 테스트 하는 동안 저장소에서 검색할 수 없도록 앱을 선택적으로 구성할 수 있습니다. 자세한 내용은 [테스트 지침](in-app-purchases-and-trials.md#testing)을 참조 하세요.
* 파트너 센터에서 [앱에 대 한 구독 추가 기능을 만들었습니다](../publish/add-on-submissions.md) .

이 예제의 코드는 다음을 가정 합니다.
* 코드 파일은 **Windows. Store** 및 system.string 네임 스페이스에 대 한 문을 **사용** **합니다.**
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md#api_intro)을 참조 하세요.

> [!NOTE]
> 데스크톱 [브리지](https://developer.microsoft.com/windows/bridges/desktop)를 사용 하는 데스크톱 응용 프로그램이 있는 경우 다음 예제에 표시 되지 않은 코드를 추가 하 여 [**storecontext**](/uwp/api/Windows.Services.Store.StoreContext) 개체를 구성 해야 할 수 있습니다. 자세한 내용은 [데스크톱 브리지를 사용 하는 데스크톱 응용 프로그램에서 StoreContext 클래스 사용](in-app-purchases-and-trials.md#desktop)을 참조 하세요.

### <a name="purchase-a-subscription-add-on"></a>구독 추가 기능 구매

이 예제에서는 현재 고객을 대신 하 여 앱에 대 한 알려진 구독 추가 기능 구매를 요청 하는 방법을 보여 줍니다. 또한이 예제에서는 구독에 평가 기간이 있는 경우를 처리 하는 방법을 보여 줍니다.

1. Code first는 고객이 구독에 대해 활성 라이선스를 이미 보유 하 고 있는지 여부를 확인 합니다. 고객이 활성 라이선스를 이미 보유 한 경우에는 코드에서 필요에 따라 구독 기능을 잠금 해제 해야 합니다 .이는 앱 전용 이므로이는 예제에서 주석으로 식별 됩니다.
2. 그런 다음,이 코드는 고객을 대신 하 여 구매할 구독을 나타내는 복사본 [**제품**](/uwp/api/windows.services.store.storeproduct) 개체를 가져옵니다. 이 코드에서는 구매 하려는 구독 추가 기능에 대 한 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 를 이미 알고 있고이 값을 *subscriptionStoreId* 변수에 할당 했다고 가정 합니다.
3. 그런 다음 코드는 평가판을 구독에 사용할 수 있는지 여부를 확인 합니다. 필요에 따라 앱에서이 정보를 사용 하 여 고객에 게 사용 가능한 평가판 또는 전체 구독에 대 한 세부 정보를 표시할 수 있습니다.
4. 마지막으로,이 코드는 [**RequestPurchaseAsync**](/uwp/api/windows.services.store.storeproduct.RequestPurchaseAsync) 메서드를 호출 하 여 구독의 구매를 요청 합니다. 구독에 대 한 평가판을 사용할 수 있는 경우 평가판은 고객에 게 구매를 위해 제공 됩니다. 그렇지 않으면 전체 구독이 구매를 위해 제공 됩니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/PurchaseSubscriptionAddOnTrialPage.xaml.cs" id="PurchaseTrialSubscription":::

### <a name="get-info-about-subscription-add-ons-for-the-current-app"></a>현재 앱에 대 한 구독 추가 기능에 대 한 정보 가져오기

이 코드 예제에서는 앱에서 사용할 수 있는 모든 구독 추가 기능에 대 한 정보를 가져오는 방법을 보여 줍니다. 이 정보를 얻으려면 먼저 [**GetAssociatedStoreProductsAsync**](/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsAsync) 메서드를 사용 하 여 앱에 사용할 수 있는 각 추가 기능을 나타내는 복사본 [**제품**](/uwp/api/Windows.Services.Store.StoreProduct) 개체의 컬렉션을 가져옵니다. 그런 다음 각 제품에 대 한 [**StoreSku**](/uwp/api/windows.services.store.storesku) 을 가져오고 [**Issubscription**](/uwp/api/windows.services.store.storesku.IsSubscription) 및 [**subscriptioninfo**](/uwp/api/windows.services.store.storesku.SubscriptionInfo) 속성을 사용 하 여 구독 정보에 액세스 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/GetSubscriptionAddOnsPage.xaml.cs" id="GetSubscriptions":::

<span id="manage-subscriptions" />

## <a name="manage-subscriptions-from-your-services"></a>서비스에서 구독 관리

업데이트 된 앱이 스토어에 있고 고객이 구독 추가 기능을 구매할 수 있게 되 면 고객에 대 한 구독을 관리 해야 하는 시나리오가 있을 수 있습니다. 서비스에서 호출할 수 있는 REST Api를 제공 하 여 다음 구독 관리 작업을 수행 합니다.

* [사용자가 사용할 자격이 있는 구독을 가져옵니다](get-subscriptions-for-a-user.md). 구독이 플랫폼 간 서비스의 일부인 경우이 API를 호출 하 여 지정 된 사용자에 게 구독에 대 한 자격이 있는지 여부와 UWP 앱의 컨텍스트에서 구독 상태를 확인할 수 있습니다. 그런 다음이 정보를 사용 하 여 서비스에서 지 원하는 다른 플랫폼에서 구독의 상태를 업데이트할 수 있습니다.

* [지정 된 사용자에 대 한 구독의 청구 상태를 변경](change-the-billing-state-of-a-subscription-for-a-user.md)합니다. 이 API를 사용 하 여 구독에 대 한 자동 갱신을 취소, 확장 또는 해제할 수 있습니다.

## <a name="cancellations"></a>보내시겠습니까

고객은 [https://account.microsoft.com/services](https://account.microsoft.com/services) 해당 Microsoft 계정 페이지를 사용 하 여 획득 한 모든 구독을 확인 하 고, 구독을 취소 하 고, 구독과 연결 된 지불 형식을 변경할 수 있습니다. 고객이이 페이지를 사용 하 여 구독을 취소 하면 현재 청구 기간 동안 구독에 계속 액세스할 수 있습니다. 현재 청구 기간에 대 한 환불을 받지 않습니다. 현재 청구 기간이 종료 되 면 구독이 비활성화 됩니다.

또한 REST API를 사용 하 여 [지정 된 사용자에 대 한 구독의 청구 상태를 변경](change-the-billing-state-of-a-subscription-for-a-user.md)하 여 사용자를 대신 하 여 구독을 취소할 수 있습니다.

## <a name="subscription-renewals-and-grace-periods"></a>구독 갱신 및 유예 기간

각 청구 기간 중에는 다음 청구 기간 동안 고객의 신용 카드 요금을 청구 하려고 합니다. 요금이 청구 되지 않으면 고객의 구독은 *dunning* 상태로 전환 됩니다. 이는 해당 구독이 현재 청구 기간의 나머지 기간 동안 여전히 활성 상태 이지만 구독을 자동으로 갱신 하도록 신용 카드 요금을 정기적으로 시도 하는 것을 의미 합니다. 이 상태는 현재 청구 기간이 종료 될 때까지 최대 2 주 후, 다음 청구 기간에 대 한 갱신 날짜가 될 수 있습니다.

구독 청구에는 유예 기간을 제공 하지 않습니다. 현재 청구 기간이 종료 될 때까지 고객의 신용 카드를 성공적으로 부과할 수 없으면 구독이 취소 되 고 고객이 현재 청구 기간 후에 구독에 더 이상 액세스할 수 없게 됩니다.

## <a name="unsupported-scenarios"></a>지원되지 않는 시나리오

다음 시나리오는 현재 구독 추가 기능에 대해 지원 되지 않습니다.

* 지금은 스토어를 통해 고객에 게 구독을 직접 판매 하는 것은 지원 되지 않습니다. 구독은 디지털 제품의 앱 에서만 구매할 수 있습니다.
* 고객은 [https://account.microsoft.com/services](https://account.microsoft.com/services) 해당 Microsoft 계정에 대 한 페이지를 사용 하 여 구독 기간을 전환할 수 없습니다. 다른 구독 기간으로 전환 하려면 고객이 현재 구독을 취소 하 고 앱에서 다른 구독 기간을 사용 하 여 구독을 구매 해야 합니다.
* 계층 전환은 현재 구독 추가 기능에 대해 지원 되지 않습니다. 예를 들어 고객을 기본 구독에서 더 많은 기능을 제공 하는 프리미엄 구독으로 전환 하는 경우를 들 수 있습니다.
* [판매](../publish/put-apps-and-add-ons-on-sale.md) 및 [판촉 코드](../publish/generate-promotional-codes.md) 는 현재 구독 추가 기능에 대해 지원 되지 않습니다.
* **획득을 중지**하도록 구독 추가 기능에 대 한 표시 유형을 설정한 후 기존 구독을 갱신 합니다. 자세한 내용은 [추가 기능 가격 책정 및 가용성 설정](../publish/set-add-on-pricing-and-availability.md) 을 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)
* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
