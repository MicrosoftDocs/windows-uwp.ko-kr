---
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: 앱에서 대규모 앱에서 바로 구매 제품 카탈로그를 제공하는 경우 이 항목에 설명된 프로세스를 선택적으로 수행하여 카탈로그를 관리할 수 있습니다.
title: 앱에서 바로 구매 제품의 큰 카탈로그 관리
---

# 앱에서 바로 구매 제품의 큰 카탈로그 관리


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

앱에서 대규모 앱에서 바로 구매 제품 카탈로그를 제공하는 경우 이 항목에 설명된 프로세스를 선택적으로 수행하여 카탈로그를 관리할 수 있습니다. 카탈로그 내에서 각각 수백 개의 제품을 나타낼 수 있는 특정 기준 가격에 대한 소수의 제품 항목을 만들 수 있습니다.

이 접근 권한 값을 사용하려면 스토어에 나열된 앱에서 바로 구매 제품과 관련된 앱 정의 제품 서비스를 지정하는 [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382) 메서드 오버로드를 사용합니다. 호출하는 동안 앱은 판매와 제품 연결을 지정할 뿐만 아니라 큰 카탈로그 판매 세부 정보를 포함하는 [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263384) 개체도 전달해야 합니다. 이러한 세부 정보를 제공하지 않으면 나열된 제품에 대한 세부 정보가 대신 사용됩니다.

스토어는 결과 [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392)에서 구매 요청의 *offerId*만 사용합니다. 이 프로세스는 [스토어에 앱에서 바로 구매 제품을 나열](https://msdn.microsoft.com/library/windows/apps/mt148551)할 때 원래 제공된 정보를 직접 수정하지 않습니다.

**참고** Windows 10부터 스토어에 표시되는 개발자 계정별 제품 목록의 수가 제한되지 않습니다. 이전 릴리스에서는 개발자 계정별로 200개의 제품 목록을 스토어에 표시하도록 제한되었으며 이 항목에 설명된 프로세스를 사용하여 이 제한 사항 문제를 해결할 수 있습니다.

## 필수 조건

-   이 항목에서는 스토어에 나열된 단일 앱에서 바로 구매 제품을 사용하여 여러 가지 앱에서 바로 판매를 표현하는 스토어 지원에 대해 설명합니다. 앱에서 바로 구매에 익숙하지 않은 경우 라이선스 정보 및 스토어에 앱에서 바로 구매를 제대로 나열하는 방법을 알아보려면 [앱에서 바로 구매 제품 사용](enable-in-app-product-purchases.md)을 검토하세요.
-   새 앱에서 바로 판매를 처음 코딩하고 테스트할 때는 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 개체 대신 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 개체를 사용해야 합니다. 이렇게 하면 라이브 서버를 호출하는 대신 라이선스 서버에 대한 호출을 시뮬레이션하여 라이선스 논리를 확인할 수 있습니다. 이렇게 하려면 %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData에 있는 "WindowsStoreProxy.xml" 파일을 사용자 지정해야 합니다. 처음으로 앱이 실행될 때 Microsoft Visual Studio 시뮬레이터에서 이 파일을 만들거나 런타임에 사용자 지정 파일을 로드할 수도 있습니다. 자세한 내용은 **CurrentAppSimulator**를 참조하세요.
-   이 항목에서는 [스토어 샘플](http://go.microsoft.com/fwlink/p/?LinkID=627610)에 제공된 코드 예제도 참조합니다. 이 샘플은 UWP(유니버설 Windows 플랫폼) 앱에 제공된 다양한 수익 창출 옵션을 실습할 수 있는 좋은 방법입니다.

## 앱에서 바로 구매 제품을 위한 구매 요청 만들기

큰 카탈로그 내에서 특정 제품을 위한 구매 요청은 앱 내에서의 다른 모든 구매 요청과 거의 동일한 방식으로 처리됩니다. 앱은 새로운 [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382) 메서드 오버로드를 호출할 때 *OfferId*와 앱에서 바로 구매 제품 이름으로 채워진 [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263390) 개체를 모두 제공합니다.

```CSharp
string offerId = "1234";
string displayPropertiesName = "MusicOffer1";
var displayProperties = new ProductPurchaseDisplayProperties(displayPropertiesName);

try
{
    PurchaseResults purchaseResults = await CurrentAppSimulator.RequestProductPurchaseAsync("product1", offerId, displayProperties);
    switch (purchaseResults.Status)
    {
        case ProductPurchaseStatus.Succeeded:
            // Grant the user their purchase here, and then pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
            // To indicate local fulfillment to the Windows Store.
            break;
        case ProductPurchaseStatus.NotFulfilled:
            // First check for unfulfilled purchases and grant any unfulfilled purchases from an earlier transaction.
            // Once products are fulfilled pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
            // To indicate local fulfillment to the Windows Store.
            break;
        case ProductPurchaseStatus.NotPurchased:
            // Notify user that the purchase was not completed due to cancellation or error.
            break;
    }
}
catch (Exception)
{
    //Notify the user of the purchase error.
}
```

## 앱에서 바로 판매의 이행 보고

판매가 로컬에서 이행되는 즉시 앱은 제품 이행을 스토어에 보고해야 합니다. 큰 카탈로그 시나리오에서 앱이 판매 이행을 보고하지 않을 경우 사용자는 동일한 스토어 제품 목록을 사용하여 앱에서 바로 구매 제품을 구매할 수 없습니다.

앞에서 설명한 대로 스토어는 제공된 판매 정보만 사용하여 [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392)를 채우며, 큰 카탈로그 판매와 스토어 제품 목록 간에 영구 연결을 만들지 않습니다. 따라서 제품에 대한 사용자 자격을 추적하고 제품 관련 컨텍스트(예제: 구매할 항목의 이름 또는 항목에 대한 세부 정보)를 [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382) 작업 외부의 사용자에게 제공해야 합니다.

다음 코드에서는 이행 호출과 특정 판매 정보가 삽입되는 UI 메시지 패턴을 보여줍니다. 특정 제품 정보가 없을 경우 예제에서는 제품 [**ListingInformation**](https://msdn.microsoft.com/library/windows/apps/br225163)의 정보를 사용합니다.

```CSharp
string offerId = "1234";
product1ListingName = product1.Name;
string displayPropertiesName = "MusicOffer1";

if (String.IsNullOrEmpty(displayPropertiesName))
{
    displayPropertiesName = product1ListingName;
}
var offerIdMsg = " with offer id " + offerId;
if (String.IsNullOrEmpty(offerId))
{
    offerIdMsg = " with no offer id";
}

FulfillmentResult result = await CurrentAppSimulator.ReportConsumableFulfillmentAsync(productId, transactionId);
switch (result)
{
    case FulfillmentResult.Succeeded:
        Log("You bought and fulfilled " + displayPropertiesName + offerIdMsg, NotifyType.StatusMessage);
        break;
    case FulfillmentResult.NothingToFulfill:
        Log("There is no purchased product 1 to fulfill.", NotifyType.StatusMessage);
        break;
    case FulfillmentResult.PurchasePending:
        Log("You bought product 1. The purchase is pending so we cannot fulfill the product.", NotifyType.StatusMessage);
        break;
    case FulfillmentResult.PurchaseReverted:
        Log("You bought product 1. But your purchase has been reverted.", NotifyType.StatusMessage);
        // Since the user' s purchase was revoked, they got their money back.
        // You may want to revoke the user' s access to the consumable content that was granted.
        break;
    case FulfillmentResult.ServerError:
        Log("You bought product 1. There was an error when fulfilling.", NotifyType.StatusMessage);
        break;
}
```

## 관련 항목

* [앱에서 바로 제품 구매 사용](enable-in-app-product-purchases.md)
* [앱에서 바로 구매 소모성 제품 구매 사용](enable-consumable-in-app-product-purchases.md)
* [스토어 샘플(평가판 및 앱에서 바로 구매 설명)](http://go.microsoft.com/fwlink/p/?LinkID=627610)
* [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382)
* [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263384)


<!--HONumber=Mar16_HO1-->


