---
author: mcleanbyron
ms.assetid: 9F0A59A1-FAD7-4AD5-B78B-C1280F215D23
description: "Windows 스토어 대상 제품 API를 사용하여 앱에 제공되는 대상 제품을 요청합니다."
title: "스토어 서비스를 사용하여 대상 제품 관리"
ms.author: mcleans
ms.date: 05/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 스토어 서비스, Windows 스토어 대상 제품 API, 대상 제품"
ms.openlocfilehash: 684c37d4439f415ad607b7f3e6a166966cc9f835
ms.sourcegitcommit: eaacc472317eef343b764d17e57ef24389dd1cc3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/17/2017
---
# <a name="manage-targeted-offers-using-store-services"></a>스토어 서비스를 사용하여 대상 제품 관리

Windows 개발자 센터 대시보드에서, 앱의 **참여 > 대상 제품** 페이지에서 *대상 제품*을 만들 때 앱 코드에 *Windows 스토어 대상 제품 API*를 사용하여 대상 제품에 대한 앱 내 환경을 구현합니다. 대상 제품 및 대시보드에서 대상 제품을 만드는 방법에 대한 자세한 내용은 [대상 제품을 사용하여 참여 및 변환 최대화](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)를 참조하세요.

대상 제품 API는 다음 작업을 수행하는 데 사용할 수 있는 REST API입니다.

* 사용자가 대상 제품의 고객 세그먼트에 포함되는지 여부에 따라 현재 사용자에게 제공되는 대상 제품을 가져옵니다.
* 사용자가 대상 제품을 구매하는 경우 스토어에 요청을 제출하여 해당 대상 제품과 연결된 보너스를 받을 수 있습니다. 이 절차는 변환에 성공할 때마다 개발자에게 보너스가 지급되는 Microsoft 후원 프로모션과 대상 제품이 연결된 경우에만 필요합니다.

이 API를 앱 코드에 사용하려면 다음 단계를 따릅니다.

1.  현재 앱에 로그인한 사용자의 [Microsoft 계정 토큰을 가져옵니다](#obtain-a-microsoft-account-token).
2.  [현재 사용자의 대상 제품을 가져옵니다](#get-targeted-offers).
3.  [대상 제품과 연결된 추가 기능을 구매합니다](#purchase-add-on).
3.  [대상 제품을 요청합니다](#claim-targeted-offer)(변환에 성공할 때마다 개발자에게 보너스가 지급되는 Microsoft 후원 프로모션과 대상 제품이 연결된 경우).

> [!NOTE]
> 대상 제품을 Microsoft 후원 프로모션과 연결하고 해당 제품에 대한 요청을 제출하는 기능은 현재 일부 개발자 계정에서만 사용할 수 있습니다.

이러한 모든 단계를 보여주는 완전한 코드 예제는 이 문서 끝 부분 [코드 예제](#code-example)를 참조합니다. 다음 섹션에서는 이러한 각 단계에 대한 자세한 내용을 제공합니다.

<span id="obtain-a-microsoft-account-token" />
## <a name="get-a-microsoft-account-token-for-the-current-user"></a>현재 사용자의 Microsoft 계정 토큰 가져오기

앱의 코드에서 현재 로그인한 사용자의 MSA(Microsoft 계정) 토큰을 가져옵니다. Windows 스토어 대상 제품 API의 각 메서드에 대한 ```Authorization``` 요청 헤더에 이 토큰을 전달해야 합니다. 이 토큰은 스토어에서 현재 사용자에게 제공되는 대상 제품을 검색하는 데 사용됩니다.

MSA 토큰을 가져오려면 [WebAuthenticationCoreManager](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webauthenticationcoremanager) 클래스를 사용하여 ```devcenter_implicit.basic,wl.basic``` 범위를 사용하는 토큰을 요청합니다. 다음 예에서는 이 작업을 수행하는 방법을 보여 줍니다. [완전한 예제](#code-example)에서 발췌한 예제입니다. 완전한 예제에서 제공되는 **using** 문이 필요합니다.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetMSAToken)]

MSA 토큰을 가져오는 방법에 대한 자세한 내용은 [웹 계정 관리자](../security/web-account-manager.md)를 참조하세요.

<span id="get-targeted-offers" />
## <a name="get-the-targeted-offers-for-the-current-user"></a>현재 사용자의 대상 제품 가져오기

현재 사용자의 MSA 토큰을 확보한 후에는 ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` URI의 GET 메서드를 호출하여 현재 사용자에게 제공되는 대상 제품을 가져옵니다. 이 REST 메서드에 대한 자세한 내용은 [대상 제품 가져오기](get-targeted-offers.md)를 참조하세요.

이 메서드는 현재 사용자에게 제공되는 대상 제품과 연결된 추가 기능의 제품 ID를 반환합니다. 이 정보를 사용하여 사용자에게 하나 이상의 대상 제품을 앱에서 바로 구매로 제공할 수 있습니다. 또한 이 메서드는 대상 제품 중 하나와 연결된 보너스를 받을 수 있도록 스토어에 [요청을 제출](#claim-targeted-offer)하는 데 사용되는 추적 ID를 반환합니다.

다음 예제는 현재 사용자에게 대상 제품을 제시하는 방법을 보여 줍니다. 이 예제는 [완전한 예제](#code-example)에서 발췌했습니다. Newtonsoft의 [Json.NET](http://www.newtonsoft.com/json) 라이브러리와 추가 클래스, 완전한 예제에서 제공되는 **using** 문이 필요합니다.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffers)]

<span id="purchase-add-on" />
## <a name="purchase-the-add-on-that-is-associated-with-a-targeted-offer"></a>대상 제품과 연결된 추가 기능 구매

다음으로 사용자에게 구매할 대상 제품 중 하나를 제공합니다. 사용자가 대상 제품 구매에 동의할 경우 다음 메서드 중 하나를 사용하여 대상 제품과 연결된 추가 기능을 프로그래밍 방식으로 구매합니다. 이전 단계에서 검색한 제품 ID 값을 사용하여 구매할 추가 기능을 확인합니다.

* 앱이 Windows 10 버전 1607 이상을 대상으로 하는 경우 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) 네임스페이스에 **RequestPurchaseAsync** 메서드 중 하나를 사용하는 것이 좋습니다. 이러한 메서드 사용에 대한 자세한 내용은 [앱에서 바로 앱 및 추가 기능 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md)을 참조하세요.

* 앱이 Windows 10 이전 버전을 대상으로 하는 경우 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스에 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_RequestProductPurchaseAsync_System_String_) 메서드를 사용하여 추가 기능을 구매합니다. 이 메서드 사용에 대한 자세한 내용은 [앱에서 바로 구매 제품 구매 사용](enable-in-app-product-purchases.md)을 참조하세요.

각 방법을 보여주는 코드 예제는 이 문서 끝 부분 [코드 예제](#code-example)를 참조합니다.

> [!NOTE]
> UWP 앱에서 앱 내 구매를 구현하는 두 가지 네임스페이스가 있는데, 하나는 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)(Windows 10 버전 1607에서 도입)이고 다른 하나는 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)(모든 Windows 10 버전에서 사용 가능)입니다. 이러한 네임스페이스 차이점에 대한 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)을 참조하세요.

<span id="claim-targeted-offer" />
## <a name="submit-a-claim-for-a-targeted-offer"></a>대상 제품에 대한 요청 제출

변환에 성공할 때마다 개발자에게 보너스가 지급되는 Microsoft 후원 프로모션과 연결된 대상 제품을 사용자가 구매하는 경우 보너스를 받으려면 스토어에 요청을 제출해야 합니다. 클레임을 제출할 때, 구매 확인을 나타내는 값을 제공해야 합니다. 구매 확인 방법은 앱이 추가 기능을 구매할 때 **Windows.ApplicationModel.Store** 네임스페이스, 또는 **Windows.Services.Store** 네임스페이스의 API를 사용하는지에 따라 달라집니다.

> [!NOTE]
> 대상 제품을 Microsoft 후원 프로모션과 연결하고 해당 제품에 대한 요청을 제출하는 기능은 현재 일부 개발자 계정에서만 사용할 수 있습니다.

### <a name="apps-that-use-the-windowsapplicationmodelstore-namespace"></a>Windows.ApplicationModel.Store 네임스페이스를 사용하는 앱

1. **Windows.ApplicationModel.Store** 네임스페이스에 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_RequestProductPurchaseAsync_System_String_) 메서드를 사용하여 추가 기능을 구매한 후, [구매 영수증](use-receipts-to-verify-product-purchases.md)을 검색합니다. 이 영수증은 **RequestProductPurchaseAsync** 메서드에서 반환한 [PurchaseResults.ReceiptXml](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.purchaseresults#Windows_ApplicationModel_Store_PurchaseResults_ReceiptXml_) 개체에 제공됩니다. 이 영수증은 다음 단계에서 구매 확인으로 사용됩니다.

2. ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user```URI로 POST 메시지를 보내서 대상 제품 변환에 대한 요청을 제출합니다. 이 REST 메서드에 대한 자세한 내용은 [대상 제품 요청](claim-a-targeted-offer.md)을 참조하세요. 요청 분문에 다음 데이터를 전달합니다.

  * *storeOffer* 필드: [대상 제품 가져오기](get-targeted-offers.md) 메서드의 요청 본문에서 받은 JSON 개체 중 하나를 전달합니다(이 개체에는 *offers* 및 *trackingId* 필드가 포함되어야 합니다).

  * *receipt* 필드: 전 단계에서 검색한 영수증 문자열을 전달합니다(**RequestProductPurchaseAsync** 메서드가 반환한 **PurchaseResults.ReceiptXml** 개체에서 제공됩니다).

다음 예제는 **Windows.ApplicationModel.Store** 네임스페이스의 API를 사용해 대상 제품을 구매 및 클레임하는 방법을 보여 줍니다. 이 예제는 [완전한 예제](#code-example)에서 발췌했습니다. Newtonsoft의 [Json.NET](http://www.newtonsoft.com/json) 라이브러리와 추가 클래스, 완전한 예제에서 제공되는 **using** 문이 필요합니다.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#ClaimOfferOnAnyVersionWindows10)]

### <a name="apps-that-use-the-windowsservicesstore-namespace"></a>Windows.Services.Store 네임스페이스를 사용하는 앱

1. [Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) 네임스페이스의 **RequestPurchaseAsync** 메서드 중 하나를 사용해 추가 기능을 구매한 후, 다음 단계에 따라 구매에 대한 *주문 ID*를 가져옵니다. 이 값은 구매 확인을 나타냅니다.

  1. [GetUserCollectionAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext#Windows_Services_Store_StoreContext_GetUserCollectionAsync_Windows_Foundation_Collections_IIterable_System_String__) 메서드를 호출, 사용자가 취득한 추가 기능을 나타내는 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 컬렉션을 가져옵니다.

  2. 이 컬렉션에서 대상 제품에 대해 구매된 추가 기능을 나타내는 **StoreProduct** 개체를 가져옵니다.

  3. **StoreProduct** 개체의 [ExtendedJsonData](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_) 속성 값을 가져옵니다.  이 속성은 추가 기능에 대한 스토어 관련 데이터 전체가 포함된 JSON 형식 문자열을 반환합니다.

  4. JSON 문자열을 구문 분석하고, 이 문자열의 *orderId* 필드 값을 검색합니다.

2. ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user```URI로 POST 메시지를 보내서 대상 제품 변환에 대한 요청을 제출합니다. 이 REST 메서드에 대한 자세한 내용은 [대상 제품 요청](claim-a-targeted-offer.md)을 참조하세요. 요청 분문에 다음 개체를 전달합니다.

  * *storeOffer* 필드: [대상 제품 가져오기](get-targeted-offers.md) 메서드의 요청 본문에서 받은 JSON 개체 중 하나를 전달합니다(이 개체에는 *offers* 및 *trackingId* 필드가 포함되어야 합니다).

  * *receipt* 필드: 앞 단계에서 검색한 *orderId* 값을 전달합니다.

다음 예제는 **Windows.Service.Store** 네임스페이스의 API를 사용해 대상 제품을 구매 및 클레임하는 방법을 보여 줍니다. 이 예제는 [완전한 예제](#code-example)에서 발췌했습니다. Newtonsoft의 [Json.NET](http://www.newtonsoft.com/json) 라이브러리와 추가 클래스, 완전한 예제에서 제공되는 **using** 문이 필요합니다.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#ClaimOfferOnWindows1607AndLater)]

<span id="code-example" />
## <a name="complete-code-example"></a>전체 코드 예제

다음 코드 예제는 다음과 같은 작업을 보여 줍니다.

* 현재 사용자의 MSA 토큰을 가져옵니다.
* [Get targeted offers](get-targeted-offers.md) 메서드를 사용하여 현재 사용자에게 제공되는 대상 제품을 모두 가져옵니다.
* 대상 제품과 연결된 추가 기능을 구매합니다.
* [Claim a targeted offer](claim-a-targeted-offer.md) 메서드를 사용하여 대상 제품을 요청합니다.

이 예제는 Newtonsoft의 [Json.NET](http://www.newtonsoft.com/json) 라이브러리가 필요합니다. 이 예에서는 이 라이브러리를 사용하여 JSON 형식의 데이터를 직렬화 및 역직렬화합니다.

> [!NOTE]
> UWP 앱에서 앱 내 구매를 구현하는 두 가지 네임스페이스가 있는데, 하나는 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)(Windows 10 버전 1607에서 도입)이고 다른 하나는 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)(모든 Windows 10 버전에서 사용 가능)입니다. 이 예제는 [버전 적응 코드](../debug-test-perf/version-adaptive-code.md)를 사용, 추가 기능 구매와 대상 제품 클레임 제출에 동일한 앱의 네임스페이스를 사용합니다. 앱을 이전 버전 Windows 10에서 실행시키고 싶다면 최소 OS 버전이 이전 버전일 수 있지만, 이 코드를 사용하려면 프로젝트 대상 OS 버전이 **Windows 10 1주년 에디션(10.0, 빌드 14394)** 이상이어야 합니다. 이러한 네임스페이스 차이점에 대한 자세한 내용은 [앱 내 구매 및 평가판](in-app-purchases-and-trials.md)을 참조하세요.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffersSample)]

## <a name="related-topics"></a>관련 항목

* [대상 제품을 사용하여 참여 및 변환 최대화](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)
* [대상 제품 가져오기](get-targeted-offers.md)
* [대상 제품 요청](claim-a-targeted-offer.md)
