---
description: Store 제품의 확장 된 JSON 데이터 스키마에 대해 설명 합니다.
title: Microsoft Store 제품용 데이터 스키마
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp, ExtendedJsonData, 매장 제품, 스키마
ms.localizationpriority: medium
ms.openlocfilehash: 46feac06745cd875aaf99985d45ea1b5b126b540
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175097"
---
# <a name="data-schemas-for-store-products"></a>Microsoft Store 제품용 데이터 스키마

스토어에 앱 또는 추가 기능과 같은 제품을 제출 하는 경우 스토어는 제품 및 라이선스에 대 한 포괄적인 데이터 집합을 유지 관리 합니다. 앱의 코드에서, [Windows. Store](/uwp/api/windows.services.store) 네임 스페이스의 속성을 사용 하 여이 데이터의 일부에 프로그래밍 방식으로 액세스할 수 있습니다. 예를 들어, 사용자의 제품을 사용 하 여 현재 앱에 대 한 현재 앱 또는 추가 기능에 대 한 설명 및 가격을 검색할 수 있습니다 [. description](/uwp/api/windows.services.store.storeproduct.Description) 및을 사용 하 여 [price](/uwp/api/windows.services.store.storeproduct.Price) 속성을 사용 합니다.

그러나 저장소의 제품에 대 한 대부분의 데이터는 [Windows. store](/uwp/api/windows.services.store) 네임 스페이스의 미리 정의 된 속성에 의해 노출 되지 않습니다. 코드의 제품에 대 한 전체 데이터에 액세스 하려면 다음과 같은 일반 속성을 대신 사용할 수 있습니다.

* [ExtendedJsonData](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData)
* [StoreSku.ExtendedJsonData](/uwp/api/windows.services.store.storesku.ExtendedJsonData)
* [ExtendedJsonData](/uwp/api/windows.services.store.storeavailability.ExtendedJsonData)
*   [StoreCollectionData.ExtendedJsonData](/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData)
*   [StoreAppLicense.ExtendedJsonData](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData)
* [StoreLicense.ExtendedJsonData](/uwp/api/windows.services.store.storelicense.ExtendedJsonData)
*   [StorePurchaseProperties.ExtendedJsonData](/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData)

이러한 속성은 해당 개체에 대 한 전체 데이터를 JSON 형식 문자열로 반환 합니다. 이 문서에서는 이러한 속성이 반환 하는 데이터에 대 한 전체 스키마를 제공 합니다.

> [!NOTE]
> 상점의 제품은 [제품](/uwp/api/windows.services.store.storeproduct), [SKU](/uwp/api/windows.services.store.storesku)및 [가용성](/uwp/api/windows.services.store.storeavailability) 개체의 계층 구조로 구성 됩니다. 자세한 내용은 [Products, sku 및 전반](in-app-purchases-and-trials.md#products-skus)을 참조 하세요.

## <a name="schema-for-storeproduct-storesku-storeavailability-and-storecollectiondata"></a>StoreSku Product,, StoreCollectionData, 및에 대 한 스키마

다음 스키마는 [ExtendedJsonData](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData)에서 반환 하는 JSON 형식 문자열을 설명 합니다. [ExtendedJsonData, StoreSku](/uwp/api/windows.services.store.storesku.ExtendedJsonData), [ExtendedJsonData](/uwp/api/windows.services.store.storeavailability.ExtendedJsonData)및 [StoreCollectionData](/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData) 속성은 `DisplaySkuAvailabilities\Sku` `DisplaySkuAvailabilities\Availabilities` 각각, 및 `DisplaySkuAvailabilities\Sku\CollectionData` 경로 아래에 정의 된 스키마 부분만 반환 합니다.

[ExtendedJsonData](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData)에서 반환 하는 JSON 형식 문자열의 예제는 [이 섹션](#product-example)을 참조 하세요.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonData.json#L1-L729)]

<span id="product-example" />

### <a name="example"></a>예제

다음 예제에서는 앱의 [ExtendedJsonData](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) 속성에서 반환 하는 JSON 형식 문자열을 보여 줍니다.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonDataExample.json#L1-L268)]

## <a name="schema-for-storeapplicense-and-storelicense"></a>StoreAppLicense 및 StoreLicense에 대 한 스키마

다음 스키마는 ExtendedJsonData에서 반환 하는 JSON 형식 문자열을 설명 합니다. [StoreAppLicense.](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) [StoreLicense ExtendedJsonData](/uwp/api/windows.services.store.storelicense.ExtendedJsonData) 속성은 경로 아래에 정의 된 스키마 부분만 반환 합니다. `productAddOns`

ExtendedJsonData에서 반환 하는 JSON 형식 문자열의 예는 [이 섹션](#license-example)을 참조 하세요. [StoreAppLicense.](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData)

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonData.json#L1-L80)]

<span id="license-example" />

### <a name="example"></a>예제

다음 예제에서는 [StoreAppLicense ExtendedJsonData](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) 속성에 의해 반환 되는 JSON 형식 문자열을 보여 줍니다.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonDataExample.json#L1-L28)]

## <a name="schema-for-storepurchaseproperties"></a>StorePurchaseProperties에 대 한 스키마

다음 스키마는 ExtendedJsonData에서 반환 하는 JSON 형식 문자열을 설명 합니다. [StorePurchaseProperties.](/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData)

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StorePurchaseProperties.ExtendedJsonData.json#L1-L12)]

## <a name="related-topics"></a>관련 항목

* [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)
* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱 및 추가 기능에 대한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [앱에서 바로 앱 및 추가 기능 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md)
* [소모성 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)
* [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)