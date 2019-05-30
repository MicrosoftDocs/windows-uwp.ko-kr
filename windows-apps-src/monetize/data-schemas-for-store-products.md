---
description: Windows.Services.Store 네임스페이스의 스토어 제품용 확장된 JSON 데이터 스키마에 대해 설명합니다.
title: 스토어 제품용 데이터 스키마
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp, ExtendedJsonData, Microsoft Store 제품, 스키마
ms.localizationpriority: medium
ms.openlocfilehash: 77f63ce409a576b3c873d95df0d2e8d0f0933808
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372530"
---
# <a name="data-schemas-for-store-products"></a>스토어 제품용 데이터 스키마

스토어에 앱이나 추가 기능 같은 제품을 제출할 때, 스토어는 제품과 라이선스에 대한 종합적인 데이터 세트를 유지합니다. 앱 코드에서 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 네임스페이스의 속성을 사용, 프로그래밍 방식으로 이런 데이터에 액세스할 수 있습니다. 예를 들어, [StoreProduct.Description](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.Description) 및 [StoreProduct.Price](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.Price) 속성을 사용, 해당 앱과 앱 추가 기능의 가격, 설명을 검색할 수 있습니다.

그러나 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 네임스페이스의 미리 정의된 속성들이 스토어 제품의 데이터를 많이 노출하지 않습니다. 코드에서 제품 데이터에 완전하게 액세스하기 위해, 대신 다음 일반 속성을 사용할 수 있습니다.

* [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData)
* [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.ExtendedJsonData)
* [StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.ExtendedJsonData)
*   [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData)
*   [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData)
* [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.ExtendedJsonData)
*   [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData)

이러한 속성들은 해당 개체의 모든 데이터를 JSON 형식 문자열로 반환합니다. 이 문서는 이들 속성이 반환하는 데이터에 대한 전체 스키마를 제공합니다.

> [!NOTE]
> 스토어의 제품들은 [제품](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct), [SKU](https://docs.microsoft.com/uwp/api/windows.services.store.storesku), 및 [가용성](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability) 개체를 기준으로 정렬됩니다. 자세한 내용은 [제품, SKU, 가용성](in-app-purchases-and-trials.md#products-skus)을 참조하세요.

## <a name="schema-for-storeproduct-storesku-storeavailability-and-storecollectiondata"></a>StoreProduct, StoreSku, StoreAvailability, StoreCollectionData 스키마

다음 스키마는 [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData)가 반환하는 JSON 형식 문자열을 설명합니다. [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.ExtendedJsonData), [StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.ExtendedJsonData),[StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData) 속성들은 각각 `DisplaySkuAvailabilities\Sku`, `DisplaySkuAvailabilities\Availabilities`, `DisplaySkuAvailabilities\Sku\CollectionData` 경로 아래 정의된 스키마만 반환합니다.

[StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData)이 반환하는 JSON-형식 문자열에 대한 예는 [여기 섹션](#product-example)을 참조하세요.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonData.json#L1-L729)]

<span id="product-example" />

### <a name="example"></a>예제

다음 예제는 앱의 [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) 속성이 반환하는 JSON 형식 문자열에 대해 설명합니다.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonDataExample.json#L1-L268)]

## <a name="schema-for-storeapplicense-and-storelicense"></a>StoreAppLicense 및 StoreLicense 스키마

다음 스키마는 [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData)가 반환하는 JSON 형식 문자열에 대해 설명합니다. [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.ExtendedJsonData) 속성은 `productAddOns` 경로 아래 정의된 스키마 부분만 반환합니다.

[StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData)가 반환하는 JSON 형식 문자열의 예는 [여기 섹션](#license-example)을 참조하세요.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonData.json#L1-L80)]

<span id="license-example" />

### <a name="example"></a>예제

다음 예제는 앱의 [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) 속성이 반환하는 JSON 형식 문자열에 대해 설명합니다.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonDataExample.json#L1-L28)]

## <a name="schema-for-storepurchaseproperties"></a>StorePurchaseProperties 스키마

다음 스키마는 [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData)가 반환하는 JSON 형식 문자열에 대해 설명합니다.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StorePurchaseProperties.ExtendedJsonData.json#L1-L12)]

## <a name="related-topics"></a>관련 항목

* [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)
* [앱 및 추가 기능에 대 한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱 및 추가 기능에 대 한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [앱 내 구매는 응용 프로그램 및 추가 기능을 사용 하도록 설정](enable-in-app-purchases-of-apps-and-add-ons.md)
* [사용할 수 있는 추가 기능 구매를 사용 하도록 설정](enable-consumable-add-on-purchases.md)
* [앱의 평가판 버전을 구현 합니다.](implement-a-trial-version-of-your-app.md)
