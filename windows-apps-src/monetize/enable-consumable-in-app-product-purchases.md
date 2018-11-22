---
author: Xansky
Description: Offer consumable in-app products&\#8212;items that can be purchased, used, and purchased again&\#8212;through the Store commerce platform to provide your customers with a purchase experience that is both robust and reliable.
title: 앱에서 바로 구매 소모성 제품 사용
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: uwp, 소모성, 추가 기능, 앱에서 바로 구매, IAP, Windows.ApplicationModel.Store
ms.author: mhopkins
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c41801362a03bb8d1d5e06b3ada876014237b1f7
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7564735"
---
# <a name="enable-consumable-in-app-product-purchases"></a>앱에서 바로 구매 소모성 제품 사용

스토어 상거래 플랫폼을 통해 앱에서 바로 구매 소모성 제품(구매, 사용 및 필요에 따라 다시 구매할 수 있는 항목)을 제공하여 강력하고 안정적인 구매 환경을 고객에게 제공합니다. 이 기능은 구매한 후 특정 회복 아이템을 구매하는 데 사용할 수 있는 게임 내 통화(금, 동전 등) 등에 특히 유용합니다.

> [!IMPORTANT]
> 이 문서에서는 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스의 멤버를 사용하여 앱에서 바로 소모성 제품을 구매할 수 있도록 만드는 방법을 설명합니다. 이 네임스페이스는 더 이상 새 기능으로 업데이트되지 않으므로 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스를 대신 사용하는 것이 좋습니다. **Windows.Services.Store** 네임 스페이스는 스토어 관리 소모 성 추가 기능 및 구독 등의 최신 추가 기능 유형을 지원 하며 이후 제품 및 파트너 센터 및 스토어에서 지 원하는 기능 유형과 호환 되도록 설계 되었습니다. **Windows.Services.Store** 네임스페이스는 Windows 10 버전, 1607에 도입되었으며 **Windows 10 Anniversary Edition(10.0, 빌드 14393)** 또는 Visual Studio의 최신 릴리스를 대상으로 하는 프로젝트에만 사용할 수 있습니다. **Windows.Services.Store** 네임스페이스를 사용하여 앱에서 바로 소모성 제품을 구매할 수 있도록 하는 방법에 대한 자세한 내용은 [이 문서](enable-consumable-add-on-purchases.md)를 참조하세요.

## <a name="prerequisites"></a>필수 구성 요소

-   이 항목에서는 소모성 앱에서 바로 구매 제품의 구매 및 이행 보고에 대해 설명합니다. 앱에서 바로 구매 제품에 대해 잘 모르는 경우 라이선스 정보 및 스토어에 앱에서 바로 구매 제품을 제대로 나열하는 방법을 알아보려면 [앱에서 바로 구매 제품 사용](enable-in-app-product-purchases.md)을 검토하세요.
-   새로운 앱에서 바로 구매 제품을 처음 코딩하고 테스트할 때는 [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 개체 대신 [CurrentAppSimulator](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 개체를 사용해야 합니다. 이렇게 하면 라이브 서버를 호출하는 대신 라이선스 서버 호출을 시뮬레이트하여 라이선스 논리를 확인할 수 있습니다. 이렇게 하려면 %userprofile%\\AppData\\local\\packages\\&lt;패키지 이름&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData에 있는 WindowsStoreProxy.xml 파일을 사용자 지정해야 합니다. 처음으로 앱이 실행될 때 Microsoft Visual Studio 시뮬레이터에서 이 파일을 만듭니다. 또는 런타임에 사용자 지정 파일을 로드할 수도 있습니다. 자세한 내용은 [CurrentAppSimulator와 함께 WindowsStoreProxy.xml 파일 사용](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy)을 참조하세요.
-   이 항목에서는 [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)에 제공된 코드 예제도 참조합니다. 이 샘플은 UWP(유니버설 Windows 플랫폼) 앱에 제공된 다양한 수익 창출 옵션을 실습할 수 있는 좋은 방법입니다.

## <a name="step-1-making-the-purchase-request"></a>1단계: 구매 요청

초기 구매 요청은 스토어를 통한 다른 모든 구매와 마찬가지로 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)를 사용하여 수행됩니다. 앱에서 바로 구매 소모성 제품의 차이는 성공적인 구매 후 앱이 이전 구매가 성공적으로 이행되었음을 스토어에 알린 다음에야 고객이 동일한 제품을 구매할 수 있다는 점입니다. 구매한 소모성 항목을 이행하고 스토어에 해당 이행을 알리는 것은 앱의 책임입니다.

다음 예제에서는 앱에서 바로 소모성 제품 구매 요청을 보여 줍니다. 구매 요청이 성공한 경우와 동일한 제품의 구매 불이행으로 인해 구매 요청이 실패한 경우의 두 가지 다른 시나리오에서 앱이 앱에서 바로 구매 소모성 제품의 로컬 이행을 수행해야 하는 경우를 나타내는 코드 주석을 확인할 수 있습니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#MakePurchaseRequest)]

## <a name="step-2-tracking-local-fulfillment-of-the-consumable"></a>2단계: 소모성의 로컬 이행 추적

고객에게 앱에서 바로 구매 소모성 제품에 대한 액세스 권한을 부여하는 경우 이행되고 있는 제품(*productId*)과 이행이 연결된 거래(*transactionId*)를 추적해야 합니다.

> [!IMPORTANT]
> 앱은 스토어에 이행을 정확하게 보고할 책임이 있습니다. 이 단계는 고객을 위해 공정하고 믿을 만한 구매 환경을 유지하는 데 필요합니다.

다음 예제에서는 이전 단계에서 수행한 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 호출의 [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392) 속성을 사용하여 이행할 구매한 제품을 확인하는 방법을 보여 줍니다. 컬렉션은 나중에 로컬 이행이 성공했는지 확인하기 위해 참조할 수 있는 위치에 제품 정보를 저장하는 데 사용됩니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GrantFeatureLocally)]

다음 예제에서는 이전 예제의 배열을 사용하여 제품 ID/거래 ID 쌍에 액세스하는 방법을 보여 줍니다. 이러한 쌍은 나중에 스토어에 이행을 보고할 때 사용됩니다.

> [!IMPORTANT]
> 앱에서 이행을 추적하고 확인하는 데 사용하는 방법에 관계없이 고객이 받지 않은 항목에 대해 비용을 지불하지 않도록 하려면 앱이 적절한 노력을 기울여야 합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#IsLocallyFulfilled)]

## <a name="step-3-reporting-product-fulfillment-to-the-store"></a>3단계: Microsoft Store에 제품 이행 보고

로컬 이행이 완료되면 앱은 *productId* 및 제품 구매가 포함된 거래를 사용하여 [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync)를 호출해야 합니다.

> [!IMPORTANT]
> 이행된 앱에서 바로 구매 소모성 제품을 스토어에 보고하지 못하면 이전 구매에 대한 이행이 보고될 때까지 사용자가 해당 제품을 다시 구매할 수 없습니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#ReportFulfillment)]

## <a name="step-4-identifying-unfulfilled-purchases"></a>4단계: 이행되지 않은 구매 확인

앱에서 [GetUnfulfilledConsumablesAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) 메서드를 사용하여 이행되지 않은 앱에서 바로 구매 소모성 제품을 언제든지 확인할 수 있습니다. 네트워크 연결 중단이나 앱 종료와 같은 예기치 않은 앱 이벤트로 인해 이행되지 않은 소모성을 확인하려면 이 메서드를 정기적으로 호출해야 합니다.

다음 예제에서는 [GetUnfulfilledConsumablesAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync)를 사용하여 이행되지 않은 소모성을 열거하는 방법 및 앱에서 로컬 이행을 완료하기 위해 이 목록을 반복하는 방법을 보여 줍니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GetUnfulfilledConsumables)]

## <a name="related-topics"></a>관련 항목

* [앱에서 바로 제품 구매 사용](enable-in-app-product-purchases.md)
* [스토어 샘플(평가판 및 앱에서 바로 구매 설명)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/br225197)
 

 
