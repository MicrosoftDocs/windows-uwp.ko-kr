---
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: 앱이 규모가 크고 앱 내 제품 카탈로그를 제공 하는 경우 필요에 따라이 항목에 설명 된 프로세스에 따라 카탈로그를 쉽게 관리할 수 있습니다.
title: 앱에서 바로 구매 제품의 큰 카탈로그 관리
ms.date: 08/25/2017
ms.topic: article
keywords: windows 10, uwp, 앱 내 구매, IAPs, 추가 기능, 카탈로그, Windows ApplicationModel. 스토어
ms.localizationpriority: medium
ms.openlocfilehash: e3eb35e2fccede9dc6f0412a3762d3d6245847c0
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364096"
---
# <a name="manage-a-large-catalog-of-in-app-products"></a>앱에서 바로 구매 제품의 큰 카탈로그 관리

앱이 규모가 크고 앱 내 제품 카탈로그를 제공 하는 경우 필요에 따라이 항목에 설명 된 프로세스에 따라 카탈로그를 쉽게 관리할 수 있습니다. Windows 10 이전 릴리스에서 스토어는 개발자 계정 당 200 제품 목록으로 제한 되어 있으며이 항목에서 설명 하는 프로세스를 사용 하 여 해당 제한을 해결할 수 있습니다. Windows 10부터 스토어는 개발자 계정 당 제품 목록 수에 제한이 없으며,이 문서에서 설명 하는 프로세스는 더 이상 필요 하지 않습니다.

> [!IMPORTANT]
> 이 문서에서는 [Windows. ApplicationModel. Store](/uwp/api/windows.applicationmodel.store) 네임 스페이스의 멤버를 사용 하는 방법을 보여 줍니다. 이 네임 스페이스는 더 이상 새 기능으로 업데이트 되지 않으므로 대신 [Windows. Store](/uwp/api/windows.services.store) 네임 스페이스를 사용 하는 것이 좋습니다. **Windows. store** 네임 스페이스는 스토어 관리 사용 가능 추가 기능 및 구독과 같은 최신 추가 기능을 지원 하며, 파트너 센터 및 스토어에서 지원 되는 이후 유형의 제품과 기능과 호환 되도록 설계 되었습니다. Windows 10, 버전 1607에 **는 windows 10** **기념일 10.0 버전을 대상으로 하는 프로젝트에만 사용 될 수 있습니다. 빌드 14393)** 또는 Visual Studio의 이후 릴리스. 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)을 참조 하세요.

이 기능을 사용 하도록 설정 하려면 특정 가격 책정 계층에 대 한 몇 가지 제품 항목을 만들고 각각은 카탈로그 내에서 수백 개의 제품을 나타낼 수 있습니다. 스토어에 나열 된 앱 내 제품과 연결 된 앱 정의 제안을 지정 하는 [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 메서드 오버 로드를 사용 합니다. 호출 하는 동안 제품 및 제품 연결을 지정 하는 것 외에도 앱은 많은 카탈로그 제품 세부 정보를 포함 하는 [ProductPurchaseDisplayProperties](/uwp/api/Windows.ApplicationModel.Store.ProductPurchaseDisplayProperties) 개체를 전달 해야 합니다. 이러한 세부 정보를 제공 하지 않으면 나열 된 제품에 대 한 세부 정보가 대신 사용 됩니다.

이 저장소는 결과 [PurchaseResults](/uwp/api/Windows.ApplicationModel.Store.PurchaseResults)의 구매 요청에서 *offerId* 만 사용 합니다. 이 프로세스는 [스토어에 앱 내 제품을 나열할](../publish/add-on-submissions.md)때 원래 제공 된 정보를 직접 수정 하지 않습니다.

## <a name="prerequisites"></a>사전 요구 사항

-   이 항목에서는 스토어에 나열 된 단일 앱 내 제품을 사용 하 여 여러 앱에서 제공 하는 표현에 대 한 저장소 지원을 다룹니다. 앱에서 바로 구매를 잘 모르는 경우 [앱에서 제품 구매 사용](enable-in-app-product-purchases.md) 을 검토 하 여 라이선스 정보에 대해 알아보고 스토어에서 앱 내 구매를 적절 하 게 나열 하는 방법을 검토 하세요.
-   새 앱 내 제품을 처음으로 코딩 하 고 테스트 하는 경우 [Currentapp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 개체 대신 [currentappsimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 개체를 사용 해야 합니다. 이러한 방식으로 라이브 서버를 호출 하는 대신 라이선스 서버에 대 한 시뮬레이션 된 호출을 사용 하 여 라이선스 논리를 확인할 수 있습니다. 이렇게 하려면% userprofile% \\ AppData \\ 로컬 \\ 패키지 \\ &lt; 패키지 이름 &gt; \\ localstate \\ Microsoft \\ Windows 스토어 \\ apidata에서 이름이 WindowsStoreProxy.xml 인 파일을 사용자 지정 해야 합니다. Microsoft Visual Studio 시뮬레이터는 앱을 처음 실행할 때이 파일을 만들거나 런타임에 사용자 지정 항목을 로드할 수도 있습니다. 자세한 내용은 [CurrentAppSimulator에서 WindowsStoreProxy.xml 파일 사용](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy)을 참조 하세요.
-   또한이 항목에서는 [Store 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)에 제공 된 코드 예제를 참조 합니다. 이 샘플은 유니버설 Windows 플랫폼 (UWP) 앱에 제공 되는 다양 한 수익 화 옵션을 사용 하 여 실습 환경을 얻는 좋은 방법입니다.

## <a name="make-the-purchase-request-for-the-in-app-product"></a>앱 내 제품에 대 한 구매 요청 만들기

큰 카탈로그 내의 특정 제품에 대 한 구매 요청은 앱 내에서 다른 구매 요청과 거의 동일한 방식으로 처리 됩니다. 앱에서 새 [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 메서드 오버 로드를 호출 하면 앱에서 *OfferId* 및 [ProductPurchaseDisplayProperties](/uwp/api/windows.applicationmodel.store.productpurchasedisplayproperties) 개체를 모두 제공 하 여 앱 내 제품의 이름으로 채웁니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/ManageCatalog.cs" id="MakePurchaseRequest":::

## <a name="report-fulfillment-of-the-in-app-offer"></a>앱 내 제품의 보고서 처리

앱이 로컬에서 제공 되는 즉시 스토어에 제품을 보고 해야 합니다. 앱에서 제공 하는 제품을 보고 하지 않으면 사용자는 동일한 매장 제품 목록을 사용 하 여 앱 내 제품을 구매할 수 없습니다.

앞서 언급 했 듯이, 스토어는 제공 된 제안 정보를 사용 하 여 [PurchaseResults](/uwp/api/Windows.ApplicationModel.Store.PurchaseResults)을 채운 다음, 대량 카탈로그 제품 및 매장 제품 목록 간에 영구 연결을 만들지 않습니다. 결과적으로 제품에 대 한 사용자 자격을 추적 하 고 [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 작업 외부의 사용자에 게 제품별 컨텍스트 (예: 구입 중인 항목의 이름 또는이에 대 한 세부 정보)를 제공 해야 합니다.

다음 코드에서는 특정 제안 정보를 삽입 하는 UI 메시징 패턴 및 처리 호출을 보여 줍니다. 특정 제품 정보가 없을 경우이 예에서는 product [Listinginformation](/uwp/api/Windows.ApplicationModel.Store.ListingInformation)의 정보를 사용 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/ManageCatalog.cs" id="ReportFulfillment":::

## <a name="related-topics"></a>관련 항목

* [앱에서 바로 구매 제품 사용](enable-in-app-product-purchases.md)
* [앱에서 바로 소모성 제품 구매 사용](enable-consumable-in-app-product-purchases.md)
* [스토어 샘플 (평가판 및 앱 내 구매 설명)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)
* [ProductPurchaseDisplayProperties](/uwp/api/Windows.ApplicationModel.Store.ProductPurchaseDisplayProperties)
