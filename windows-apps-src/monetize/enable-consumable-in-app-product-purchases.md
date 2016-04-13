---
Description: 스토어 상거래 플랫폼을 통해 앱에서 바로 구매 소모성 제품&\#8212;구매, 사용 및 필요에 따라 다시 구매할 수 있는 항목&\#8212;을 제공하여 강력하고 안정적인 구매 환경을 고객에게 제공합니다.
title: 앱에서 바로 소모성 제품 구매 사용
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
키워드: 앱에서 바로 판매
키워드: 소모성
키워드: 앱에서 바로 구매
키워드: 앱에서 바로 구매 제품
키워드: 앱에서 바로 지원 방법
키워드: 앱에서 바로 구매 코드 샘플
키워드: 앱에서 바로 판매 코드 샘플
---

# 앱에서 바로 소모성 제품 구매 사용


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

스토어 상거래 플랫폼을 통해 앱에서 바로 구매 소모성 제품(구매, 사용 및 필요에 따라 다시 구매할 수 있는 항목)을 제공하여 강력하고 안정적인 구매 환경을 고객에게 제공합니다. 이 기능은 특정 회복 아이템을 구매하여 사용할 수 있는 게임 내 통화(금, 동전 등) 등에 특히 유용합니다.

## 필수 조건

-   이 항목에서는 소모성 앱에서 바로 구매 제품의 구매 및 이행 보고에 대해 설명합니다. 앱에서 바로 구매 제품에 익숙하지 않은 경우 라이선스 정보 및 스토어에 앱에서 바로 구매 제품을 제대로 나열하는 방법을 알아보려면 [앱에서 바로 구매 제품 사용](enable-in-app-product-purchases.md)을 검토하세요.
-   새 앱에서 바로 구매 제품을 처음 코딩하고 테스트할 때는 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 개체 대신 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 개체를 사용해야 합니다. 이렇게 하면 라이브 서버를 호출하는 대신 라이선스 서버에 대한 호출을 시뮬레이션하여 라이선스 논리를 확인할 수 있습니다. 이렇게 하려면 %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData에 있는 "WindowsStoreProxy.xml" 파일을 사용자 지정해야 합니다. 처음으로 앱이 실행될 때 Microsoft Visual Studio 시뮬레이터에서 이 파일을 만들거나 런타임에 사용자 지정 파일을 로드할 수도 있습니다. 자세한 내용은 **CurrentAppSimulator**를 참조하세요.
-   이 항목에서는 [스토어 샘플](http://go.microsoft.com/fwlink/p/?LinkID=627610)에 제공된 코드 예제도 참조합니다. 이 샘플은 UWP(유니버설 Windows 플랫폼) 앱에 제공된 다양한 수익 창출 옵션을 실습할 수 있는 좋은 방법입니다.

## 1단계: 구매 요청

초기 구매 요청은 스토어를 통해 수행하는 다른 모든 구매와 마찬가지로 [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263381)를 사용하여 수행합니다. 소모성 앱에서 바로 구매 제품의 차이는 성공적인 구매 후 앱이 이전 구매가 성공적으로 이행되었음을 스토어에 알린 다음에야 고객이 동일한 제품을 구매할 수 있다는 점입니다. 구매한 소모성 항목을 이행하고 스토어에 해당 이행을 알리는 것은 앱의 책임입니다.

다음 예제에서는 앱에서 바로 소모성 제품 구매 요청을 보여 줍니다. 구매 요청이 성공한 경우와 동일한 제품의 구매 불이행으로 인해 구매 요청이 실패한 경우의 두 가지 다른 시나리오에서 앱이 앱에서 바로 구매 소모성 제품의 로컬 이행을 수행해야 하는 경우를 나타내는 코드 주석을 확인할 수 있습니다.

```CSharp
PurchaseResults purchaseResults = await CurrentAppSimulator.RequestProductPurchaseAsync("product1");
switch (purchaseResults.Status)
{
    case ProductPurchaseStatus.Succeeded:
        product1TempTransactionId = purchaseResults.TransactionId;

        // Grant the user their purchase here, and then pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
        // To indicate local fulfillment to the Windows Store.
        break;

    case ProductPurchaseStatus.NotFulfilled:
        product1TempTransactionId = purchaseResults.TransactionId;

        // First check for unfulfilled purchases and grant any unfulfilled purchases from an earlier transaction.
        // Once products are fulfilled pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
        // To indicate local fulfillment to the Windows Store.
        break;
}
```

## 2단계: 소모성 항목의 로컬 이행 추적

고객에게 소모성 앱에서 바로 구매 제품에 대한 액세스 권한을 부여하는 경우 이행되고 있는 제품(*productId*)과 이행이 연결된 거래(*transactionId*)를 추적해야 합니다.

**중요** 앱은 스토어에 이행을 정확하게 보고할 책임이 있습니다. 이 단계는 고객을 위해 공정하고 믿을 만한 구매 환경을 유지하는 데 필요합니다.

다음 예제에서는 이전 단계의 [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263381) 호출에서 [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392) 속성을 사용하여 이행할 구매한 제품을 확인하는 것을 보여 줍니다. 배열은 나중에 로컬 이행이 성공했는지 확인하기 위해 참조할 수 있는 위치에 제품 정보를 저장하는 데 사용됩니다.

```CSharp
private void GrantFeatureLocally(string productId, Guid transactionId)
{
    if (!grantedConsumableTransactionIds.ContainsKey(productId))
    {
        grantedConsumableTransactionIds.Add(productId, new List<Guid>());
    }
    grantedConsumableTransactionIds[productId].Add(transactionId);

    // Grant the user their content. You will likely increase some kind of gold/coins/some other asset count.
}
```

다음 예제에서는 이전 예제의 배열을 사용하여 제품 ID/거래 ID 쌍에 액세스하는 방법을 보여 줍니다. 이러한 쌍은 나중에 스토어에 이행을 보고할 때 사용됩니다.

**중요** 앱에서 이행을 추적하고 확인하는 데 사용하는 방법에 관계없이 고객이 받지 않은 항목에 대해 비용을 지불하지 않도록 하려면 앱이 적절한 노력을 기울여야 합니다.

```CSharp
private Boolean IsLocallyFulfilled(string productId, Guid transactionId)
{
    return grantedConsumableTransactionIds.ContainsKey(productId) &amp;&amp; grantedConsumableTransactionIds[productId].Contains(transactionId);
}
```

## 3단계: 스토어에 제품 이행 보고

로컬 이행이 완료되면 앱은 *productId* 및 제품 구매가 포함된 거래를 포함하는 [**ReportConsumableFulfillmentAsync**](https://msdn.microsoft.com/library/windows/apps/dn263380)를 호출해야 합니다.

**중요** 이행된 앱에서 바로 구매 소모성 제품을 스토어에 보고하지 못하면 이전 구매에 대한 이행이 보고될 때까지 사용자가 해당 제품을 다시 구매할 수 없습니다.

```CSharp
FulfillmentResult result = await CurrentAppSimulator.ReportConsumableFulfillmentAsync("product2", product2TempTransactionId);
```

## 4단계: 이행되지 않은 구매 확인

앱에서는 [**GetUnfulfilledConsumablesAsync**](https://msdn.microsoft.com/library/windows/apps/dn263379) 메서드를 사용하여 이행되지 않은 소모성 앱에서 바로 구매 제품을 언제든지 확인할 수 있습니다. 네트워크 연결 중단이나 앱 종료와 같은 예기치 않은 앱 이벤트로 인해 이행되지 않은 소모성 항목을 확인하려면 이 메서드를 정기적으로 호출해야 합니다.

다음 예제에서는 [**GetUnfulfilledConsumablesAsync**](https://msdn.microsoft.com/library/windows/apps/dn263379)를 사용하여 이행되지 않은 소모성 항목을 열거하는 방법 및 앱에서 로컬 이행을 완료하기 위해 이 목록을 반복하는 방법을 보여 줍니다.

```CSharp
private async void GetUnfulfilledConsumables()
{
    products = await CurrentApp.GetUnfulfilledConsumablesAsync();

    foreach (UnfulfilledConsumable product in products)
    {
        logMessage += "\nProduct Id: " + product.ProductId + " Transaction Id: " + product.TransactionId;
        // This is where you would pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
    // To indicate local fulfillment to the Windows Store.
    }
}
```

## 관련 항목

* [앱에서 바로 제품 구매 사용](enable-in-app-product-purchases.md)
* [스토어 샘플(평가판 및 앱에서 바로 구매 설명)](http://go.microsoft.com/fwlink/p/?LinkID=627610)
* [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197)
 

 






<!--HONumber=Mar16_HO1-->


