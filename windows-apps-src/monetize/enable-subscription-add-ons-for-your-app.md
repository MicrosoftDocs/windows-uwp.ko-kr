---
author: mcleanbyron
description: "Windows.Services.Store 네임스페이스를 사용하여 구독 추가 기능을 구현하는 방법을 알아봅니다."
title: "앱에 구독 추가 기능을 사용하도록 설정"
keywords: "windows 10, uwp, 구독, 추가 기능, 앱 내 구매, IAP, Windows.Services.Store"
ms.author: mcleans
ms.date: 08/01/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 16e2e8d160ad2236220dc6f19f578bbaa82c9dc0
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/17/2017
---
# <a name="enable-subscription-add-ons-for-your-app"></a>앱에 구독 추가 기능을 사용하도록 설정

> [!IMPORTANT]
> 현재 구독 추가 기능을 생성하는 기능은 초기 채택 프로그램에 참여한 개발자 계정의 집합에서만 사용할 수 있습니다. 향후 모든 개발자 계정에서 구독 추가 기능을 사용할 수 있도록 만들 계획입니다. 우리는 개발자들에게 기능 미리 보기를 제공하기 위해 이 예비 자료를 제공하고 있습니다.

UWP 앱이 Windows 10 버전 1067 이후를 대상으로 하는 경우, *구독* 추가 기능을 앱 내 구매하는 방식을 고객에게 제공할 수 있습니다. 앱에서 반복 자동 과금을 이용한 구독으로 디지털 제품(앱 기능 또는 디지털 콘텐츠)을 판매할 수 있습니다.

## <a name="feature-highlights"></a>기능 주요 사항

UWP 앱 구독 추가 기능은 다음 기능을 지원합니다.

* 1개월, 3개월, 6개월, 1년, 2년 중 구독 기간을 선택할 수 있습니다. 일부 앱의 경우 테스트용으로 6시간 구독도 가능합니다.
* 구독에 1주나 1개월 무료 평가판 사용 기간을 추가할 수 있습니다.
* Windows SDK는 앱에서 사용할 수 있는 구독 추가 기능에 대한 정보를 얻고, 구독 추가 기능을 구현하기 위해 앱에서 사용할 수 있는 [API를 제공](#code-examples)합니다. 저희는 또한 [사용자 구독을 관리할](#manage-subscriptions) 서비스를 호출할 수 있는 REST API를 제공합니다.
* 지정된 기간 동안 취득된 구독, 활성 구독자, 취소된 구독 수를 제공하는 분석 보고서도 확인할 수 있습니다.
* 고객은 Microsoft 계정의 [http://account.microsoft.com/services](http://account.microsoft.com/services) 페이지에서 자신의 구독을 관리할 수 있습니다. 고객은 이 페이지를 사용해 취득한 구독을 확인하고, 구독을 취소하고, 구독과 연결된 결제 유형을 변경할 수 있습니다.

> [!NOTE]
> 앱에 구독 추가 기능을 구현하려면, 앱에서 Windows 10 버전 1607 이상을 대상으로 지정해야 합니다. 또 **Windows.ApplicationModel.Store** 네임스페이스 대신 **Windows.Services.Store** 네임스페이스의 API를 사용해 앱 내 구매 환경을 구현해야 합니다. 이러한 네임스페이스 차이점에 대한 자세한 내용은 [앱 내 구매 및 평가판](in-app-purchases-and-trials.md)을 참조하세요.

## <a name="steps-to-enable-a-subscription-add-on-for-your-app"></a>앱에서 구독 추가 기능을 구현하는 단계

앱에서 구독 추가 기능 구매를 구현하려면 다음 단계를 따르세요.

1. 개발자 센터 대시보드에서 구독에 대한 [추가 기능 제출을 생성](../publish/add-on-submissions.md)하고, 제출을 게시합니다. 추가 기능 제출 프로세스를 적용할 때 다음 속성에 주의하세요.

  * [제품 유형](../publish/set-your-add-on-product-id.md#product-type): **구독**을 선택해야 합니다.

  * [구독 기간](../publish/enter-add-on-properties.md#subscription-period): 구독에 대한 반복 청구 기간을 선택합니다. 추가 기능을 게시한 후에는 구독 기간을 변경할 수 없습니다.

    각 구독 추가 기능에서 단일 구독 기간과 시험 평가 일 구독 기간과 평가 기간을 지원합니다. 앱에서 제공하려는 구독 유형 별로 다른 구독 추가 기능을 만들어야 합니다. 예를 들어 평가 기간이 없는 1개월 구독, 1개월 평가 기간이 제공되는 1개월 구독, 평가 기간이 없는 1년 구독, 1개월 평가 기간이 제공되는 1년 구독을 제공하고 싶다면 4가지 구독 추가 기능을 만들어야 합니다.
        > [!NOTE]
        > If you are in the process of implementing the in-app purchase experience for your subscription, we recommend that you create a test add-on with the **For testing only – 6 hours** subscription period to help you test the experience. You can choose this test period only if you select one of the **Hidden in the Store** [visibility options](../publish/set-add-on-pricing-and-availability.md#visibility) for your test add-on.

  * [평가 기간](../publish/enter-add-on-properties.md#free-trial): 구독에서 사용자가 구매 전에 구독 콘텐츠를 평가할 수 있도록 1주 또는 1개월 평가 기간을 선택합니다. 구독 추가 기능을 게시한 후에는 평가 기간을 변경하거나 제거할 수 없습니다.

    사용자는 구독 무료 평가 기간을 취득하려면, 유효한 결제 유형을 포함해 일반적인 앱 내 구매 프로세스를 통해 구독형 서비스를 구입해야 합니다. 평가 기간 동안에는 요금이 청구되지 않습니다. 평가 기간이 끝나면 구독은 자동으로 유료 구독으로 변경되고, 사용자의 결제 수단에 첫 유료 구독 기간에 대한 요금이 청구됩니다. 사용자가 평가 기간에 구독 취소를 선택해도 평가 기간이 끝날 때까지 구독이 유지됩니다.
        > [!NOTE]
        > Some trial durations are not available for all subscription periods.

  * [표시 여부](../publish/set-add-on-pricing-and-availability.md#visibility): 구독을 대상으로 앱 내 구매 환경을 테스트하기 위해 테스트 추가 기능을 생성하고 싶다면, **스토어에서 숨김** 옵션 중 하나를 선택하는 것이 좋습니다. 그렇지 않으면 해당 시나리오에서 가장 좋은 표시 옵션을 선택할 수 있습니다.

  * [가격](../publish/set-add-on-pricing-and-availability.md?#pricing): 이 섹션에서 구독 가격을 선택합니다. 추가 기능을 게시한 후에 구독 가격을 인상할 수 없습니다. 그러나 나중에 인하할 수는 있습니다.
      > [!IMPORTANT]
      > 기본적으로 추가 기능을 생성할 때 처음 가격은 **무료**로 설정됩니다. 추가 기능 제출이 완료된 후에는 구독 추가 기능 가격을 인상할 수 없기 때문에 여기에서 구독 가격을 선택해야 합니다.

2. 앱에서 [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) 네임스페이스의 API를 사용해, 현재 사용자가 구독 추가 기능을 이미 취득했는지 확인하고, 앱 내 구매 형식으로 사용자에게 판매용으로 제공합니다. 자세한 내용은 이 문서의 [코드 예제](#code-examples)를 참조합니다.

3. 앱에서 구독의 앱 내 구매를 테스트합니다. 개발자 장치가 테스트용 라이선스를 사용할 수 있도록 스토어에서 앱을 1회 다운로드해야 합니다. 자세한 내용은 앱 내 구매에 대한 [테스트 가이드](in-app-purchases-and-trials.md#testing)를 참조하세요.  
    > [!NOTE]
    > 앱이 **테스트 전용 - 6시간** 구독 기간에 액세스할 수 있는 경우, 환경 테스트에 도움이 되도록 이 기간에 대한 테스트 추가 기능을 만드는 것이 좋습니다. 테스트 추가 기능에서 **Hidden in the Store(스토어에서 숨김)** [표시 옵션](../publish/set-add-on-pricing-and-availability.md#visibility) 중 하나를 선택한 경우에만 이 테스트 기간을 선택할 수 있습니다.

4. 테스트된 코드 등 업데이트된 앱 패키지를 포함한 앱 제출을 생성해 게시합니다. 자세한 내용은 [앱 제출](../publish/app-submissions.md)을 참조하세요.

<span id="code-examples"/>
## <a name="code-examples"></a>코드 예제

이 섹션의 코드 예제는 [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) 네임스페이스의 API를 사용해 앱의 구독 추가 기능에 대한 정보를 얻고, 현재 사용자를 대신해 구독 추가 기능 구매를 요청하는 방법을 알려줍니다.

이러한 예제의 필수 조건은 다음과 같습니다.
* Windows10 버전 1607 이상을 대상으로 하는 UWP(유니버설 Windows 플랫폼) 앱에 대한 Visual Studio 프로젝트.
* Windows 개발자 센터 대시보드에서 [앱 제출](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)을 만들었으며, 이 앱은 스토어에서 게시되었습니다. 테스트 하는 동안 스토어에서 검색이 되지 않도록 앱을 구성할 수도 있습니다. 자세한 내용은 [테스트 가이드](in-app-purchases-and-trials.md#testing)를 참조하세요.
* 개발자 센터 대시보드에서 [앱에 사용 가능한 구독 추가 기능](../publish/add-on-submissions.md)을 만들었습니다.

이 예제의 코드는 다음과 같이 가정합니다.
* 코드는 ```workingProgressRing```이라는 이름의 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx)과 ```textBlock```이라는 이름의 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)을 포함한 [**페이지**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx)의 컨텍스트에서 실행됩니다. 해당 개체를 사용하여 각각 비동기 작업이 발생함을 나타내고 출력 메시지를 표시합니다.
* 코드 파일에는 **Windows.Services.Store** 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md#api_intro)을 참조하세요.

> [!NOTE]
> [데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop)를 사용하는 데스크톱 응용 프로그램이 있는 경우 이 예에서 표시되지 않는 별도의 코드를 추가하여 [**StoreContext**](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 개체를 구성해야 할 수도 있습니다. 자세한 내용은 [데스크톱 브리지를 사용하는 데스크톱 응용 프로그램에서 StoreContext 클래스 사용](in-app-purchases-and-trials.md#desktop)을 참조하세요.

### <a name="get-info-about-subscription-add-ons-for-the-current-app"></a>현재 앱에 사용할 수 있는 구독 추가 기능에 대한 정보 가져오기

이 코드 예제는 앱에서 사용할 수 있는 구독 추가 기능에 대한 정보를 가져오는 방법을 보여 줍니다. 이 정보를 가져오려면, 먼저 [**GetAssociatedStoreProductsAsync**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext#Windows_Services_Store_StoreContext_GetAssociatedStoreProductsAsync_) 메서드를 사용해 앱에서 사용할 수 있는 추가 기능 각각을 나타내는 [**StoreProduct**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct) 개체 컬렉션을 가져옵니다. 그런 다음 각 제품에 대한 [**StoreSku**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku)를 가져오고, [**IsSubscription**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku#Windows_Services_Store_StoreSku_IsSubscription_) 및 [**SubscriptionInfo**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku#Windows_Services_Store_StoreSku_SubscriptionInfo_) 속성을 사용해 구독 정보에 액세스 할 수 있습니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[구독](./code/InAppPurchasesAndLicenses_RS1/cs/GetSubscriptionAddOnsPage.xaml.cs#GetSubscriptions)]

### <a name="purchase-a-subscription-add-on"></a>구독 추가 기능 구매

이 예제는 [**StoreProduct**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) 클래스의 [**RequestPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_RequestPurchaseAsync_) 메서드를 사용해 구독 추가 기능을 구매하는 방법을 보여 줍니다. 이 예제는 구매를 원하는 구독 추가 기능의 [스토어 ID](in-app-purchases-and-trials.md#store-ids)를 이미 알고 있다고 가정합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[구독](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseSubscriptionAddOnPage.xaml.cs#PurchaseSubscription)]

<span id="manage-subscriptions" />
## <a name="manage-subscriptions-from-your-services"></a>서비스에서 구독 관리

업데이트된 앱이 스토어에 있고 고객이 구독 추가 기능을 구매할 수 있게 된 후, 고객을 위해 구독을 관리해야 하는 상황들이 있습니다. 저희는 다음과 같은 구독 관리 작업을 수행하기 위해 서비스에서 호출할 수 있는 REST API를 제공합니다.

* [사용자가 사용할 권리가 있는 구독을 가져옵니다](get-subscriptions-for-a-user.md). 구독이 플랫폼 간 서비스의 일부인 경우, 이 API를 호출해 특정 사용자가 구독 권리가 있는지 여부, UWP 앱 컨텍스트에서 구독 상태를 확인할 수 있습니다. 그런 다음 이러한 정보를 사용해 서비스가 지원하는 다른 플랫폼의 구독 상태를 업데이트 할 수 있습니다.

* [특정 사용자의 구독 청구 상태를 변경합니다](change-the-billing-state-of-a-subscription-for-a-user.md). 이 API를 사용해 구독을 취소, 연장하거나, 자동 갱신을 중지할 수 있습니다.

<span id="cancellations" />
## <a name="cancellations"></a>취소

고객은 Microsoft 계정으로 [http://account.microsoft.com/services](http://account.microsoft.com/services) 페이지에서 취득한 모든 구독을 확인하고, 구독을 취소하고, 구독과 연결된 결제 수단을 변경할 수 있습니다. 고객이 이 페이지를 사용해 구독을 취소하는 경우에도 현재 청구 기간 동안의 구독은 계속 액세스할 수 있습니다. 현재 청구 기간의 일부에 대해 환불을 받을 수 없습니다. 현재 청구 기간이 끝날 때 구독이 비활성화됩니다.

또한 [특정 사용자의 구독 청구 상태를 변경하는](change-the-billing-state-of-a-subscription-for-a-user.md) REST API를 사용, 사용자를 대신해 구독을 취소할 수 있습니다.

## <a name="unsupported-scenarios"></a>지원되지 않는 시나리오

다음 시나리오는 현재 구독 추가 기능에서 지원되지 않습니다.

* 현재 스토어를 통해 직접 고객에게 구독을 판매하는 기능은 지원되지 않습니다. 구독은 디지털 제품의 앱 내 구매에만 사용할 수 있습니다.
* 고객은 Microsoft 계정의 [http://account.microsoft.com/services](http://account.microsoft.com/services) 페이지에서 구독 기간을 변경할 수 없습니다. 다른 구독 기간으로 변경하려면, 고객은 앱에서 현재 구독을 취소한 다음 다른 구독 기간으로 구독을 구매해야 합니다.
* 현재 구독 추가 기능은 계층 전환을 지원하지 않습니다(예를 들어, 고객은 기본 구독에서 기능이 더 많은 프리미엄 구독으로 전환할 수 없음).
* 현재 구독 추가 기능에서는 [영업](../publish/put-apps-and-add-ons-on-sale.md) 및 [홍보 코드](../publish/generate-promotional-codes.md)를 지원하지 않습니다.


## <a name="related-topics"></a>관련 항목

* [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)
* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)