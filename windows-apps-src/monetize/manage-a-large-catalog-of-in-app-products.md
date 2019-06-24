---
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: 앱에서 대규모 앱에서 바로 구매 제품 카탈로그를 제공하는 경우 이 항목에 설명된 프로세스를 선택적으로 수행하여 카탈로그를 관리할 수 있습니다.
title: 앱에서 바로 구매 제품의 큰 카탈로그 관리
ms.date: 08/25/2017
ms.topic: article
keywords: windows 10, uwp, 앱에서 바로 구매, IAP, 추가 기능, 카탈로그, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: e297c01aa19449691e775c4d7d2c33028b234cfa
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320179"
---
# <a name="manage-a-large-catalog-of-in-app-products"></a>앱에서 바로 구매 제품의 큰 카탈로그 관리

앱에서 대규모 앱에서 바로 구매 제품 카탈로그를 제공하는 경우 이 항목에 설명된 프로세스를 선택적으로 수행하여 카탈로그를 관리할 수 있습니다. Windows 10 이전 릴리스에서는 개발자 계정별로 200개의 제품 목록을 스토어에 표시하도록 제한되었으며 이 항목에 설명된 프로세스를 사용하여 이 제한 사항 문제를 해결할 수 있습니다. Windows 10 부터는 저장소 개발자 계정당 제품 목록 수에 제한이 있고이 문서에서 설명한 프로세스는 더 이상 필요 합니다.

> [!IMPORTANT]
> 이 문서에서는 [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) 네임스페이스의 멤버를 사용하는 방법을 보여줍니다. 이 네임스페이스는 더 이상 새 기능으로 업데이트되지 않으므로 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 네임스페이스를 대신 사용하는 것이 좋습니다. 합니다 **Windows.Services.Store** 네임 스페이스 최신 추가 기능 형식을 사용할 수 있는 추가 기능 저장소 관리 등 구독을 지원 하며 향후 유형의 제품 및 파트너를 지 원하는 기능을 사용 하 여 호환 되도록 설계 되었습니다 센터와 저장소를 제공 합니다. **Windows.Services.Store** 네임스페이스는 Windows 10 버전, 1607에 도입되었으며 **Windows 10 Anniversary Edition(10.0, 빌드 14393)** 또는 Visual Studio의 최신 릴리스를 대상으로 하는 프로젝트에만 사용할 수 있습니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)을 참조하세요.

이 접근 권한 값을 사용하려면 카탈로그 내에서 각각 수백 개의 제품을 나타낼 수 있는 특정 기준 가격에 대한 소수의 제품 항목을 만듭니다. 스토어에 나열된 앱에서 바로 구매 제품과 관련된 앱 정의 제품 서비스를 지정하는 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 메서드 오버로드를 사용합니다. 호출하는 동안 앱은 판매와 제품 연결을 지정할 뿐만 아니라 큰 카탈로그 판매 정보를 포함하는 [ProductPurchaseDisplayProperties](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.ProductPurchaseDisplayProperties) 개체도 전달해야 합니다. 이러한 세부 정보를 제공하지 않으면 나열된 제품에 대한 세부 정보가 대신 사용됩니다.

스토어는 결과 [PurchaseResults](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.PurchaseResults)에서 구매 요청의 *offerId*만 사용합니다. 이 프로세스는 [스토어에 앱에서 바로 구매 제품을 나열](../publish/add-on-submissions.md)할 때 원래 제공된 정보를 직접 수정하지 않습니다.

## <a name="prerequisites"></a>사전 요구 사항

-   이 항목에서는 스토어에 나열된 단일 앱에서 바로 구매 제품을 사용하여 여러 가지 앱에서 바로 판매를 표현하는 스토어 지원에 대해 설명합니다. 앱에서 바로 구매를 잘 모르는 경우 라이선스 정보 및 스토어에 앱에서 바로 구매를 제대로 나열하는 방법을 알아보려면 [앱에서 바로 구매 제품 사용](enable-in-app-product-purchases.md)을 검토하세요.
-   새로운 앱에서 바로 판매를 처음 코딩하고 테스트할 때는 [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 개체 대신 [CurrentAppSimulator](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 개체를 사용해야 합니다. 이렇게 하면 라이브 서버를 호출하는 대신 라이선스 서버 호출을 시뮬레이트하여 라이선스 논리를 확인할 수 있습니다. 이 작업을 수행 하려면 %userprofile%에서 WindowsStoreProxy.xml 라는 파일을 사용자 지정 해야\\AppData\\로컬\\패키지\\&lt;패키지 이름&gt;\\LocalState\\ Microsoft\\Windows 스토어\\ApiData 합니다. 처음으로 앱이 실행될 때 Microsoft Visual Studio 시뮬레이터에서 이 파일을 만듭니다. 또는 런타임에 사용자 지정 파일을 로드할 수도 있습니다. 자세한 내용은 [CurrentAppSimulator와 함께 WindowsStoreProxy.xml 파일 사용](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy)을 참조하세요.
-   이 항목에서는 [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)에 제공된 코드 예제도 참조합니다. 이 샘플은 UWP(유니버설 Windows 플랫폼) 앱에 제공된 다양한 수익 창출 옵션을 실습할 수 있는 좋은 방법입니다.

## <a name="make-the-purchase-request-for-the-in-app-product"></a>앱에서 바로 구매 제품을 위한 구매 요청 만들기

큰 카탈로그 내의 특정 제품에 대한 구매 요청은 앱 내에서의 다른 모든 구매 요청과 거의 동일한 방식으로 처리됩니다. 앱은 새로운 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 메서드 오버로드를 호출할 때 *OfferId*와 앱에서 바로 구매 제품 이름으로 채워진 [ProductPurchaseDisplayProperties](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productpurchasedisplayproperties) 개체를 둘 다 제공합니다.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#MakePurchaseRequest)]

## <a name="report-fulfillment-of-the-in-app-offer"></a>앱에서 바로 판매의 이행 보고

판매가 로컬에서 이행되는 즉시 앱은 제품 이행을 스토어에 보고해야 합니다. 큰 카탈로그 시나리오에서 앱이 판매 이행을 보고하지 않을 경우 사용자는 동일한 스토어 제품 목록을 사용하여 앱에서 바로 구매 제품을 구매할 수 없습니다.

앞에서 설명한 대로 스토어는 제공된 판매 정보만 사용하여 [PurchaseResults](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.PurchaseResults)를 채우며, 큰 카탈로그 판매와 스토어 제품 목록 간에 영구 연결을 만들지 않습니다. 따라서 제품에 대한 사용자 자격을 추적하고, [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 작업 외부에서 제품 관련 컨텍스트(예: 구매 항목 이름 또는 항목에 대한 세부 정보)를 사용자에게 제공해야 합니다.

다음 코드에서는 이행 호출과 특정 판매 정보가 삽입된 UI 메시지 패턴을 보여 줍니다. 특정 제품 정보가 없을 경우 예제에서는 제품 [ListingInformation](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.ListingInformation)의 정보를 사용합니다.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#ReportFulfillment)]

## <a name="related-topics"></a>관련 항목

* [앱에서 바로 제품 구매 사용](enable-in-app-product-purchases.md)
* [앱에서 바로 소모성 제품 구매 사용](enable-consumable-in-app-product-purchases.md)
* [Store 샘플 (평가판 및 앱 내 구매를 보여 줍니다.)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)
* [ProductPurchaseDisplayProperties](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.ProductPurchaseDisplayProperties)
