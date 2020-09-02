---
Description: '\# \# 스토어 상거래 플랫폼을 통해 고객에 게 강력 하 고 신뢰할 수 있는 구매 환경을 제공 하기 위해 스토어 상거래 플랫폼을 통해 8212 제품&8212;를&다시 구매, 사용 및 구매할 수 있는 항목을 제공 합니다.'
title: 앱에서 바로 소모성 제품 구매 사용
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: uwp, 소비재, 추가 기능, 앱 내 구매, IAPs, Windows. ApplicationModel 스토어
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fb4119296b11e805fa72ff027383d13e6fb43818
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363696"
---
# <a name="enable-consumable-in-app-product-purchases"></a>앱에서 바로 소모성 제품 구매 사용

스토어 상거래 플랫폼을 통해 고객에 게 강력 하 고 안정적인 구매 환경을 제공 하기 위해 스토어 상거래 플랫폼을 통해 구매, 사용 및 구매할 수 있는 앱 내 제품을 제공 합니다. 이 기능은 구입할 수 있는 게임 내 통화 (골드, 동전 등)와 특정 전원 공급 장치를 구입 하는 데 사용할 수 있는 작업에 특히 유용 합니다.

> [!IMPORTANT]
> 이 문서에서는 Windows 앱 내 제품 구매를 사용 하도록 설정 하기 위해 Windows. r u s [. Store](/uwp/api/windows.applicationmodel.store) 네임 스페이스의 멤버를 사용 하는 방법을 보여 줍니다. 이 네임 스페이스는 더 이상 새 기능으로 업데이트 되지 않으므로 대신 [Windows. Store](/uwp/api/windows.services.store) 네임 스페이스를 사용 하는 것이 좋습니다. **Windows. store** 네임 스페이스는 스토어 관리 사용 가능 추가 기능 및 구독과 같은 최신 추가 기능을 지원 하며, 파트너 센터 및 스토어에서 지원 되는 이후 유형의 제품과 기능과 호환 되도록 설계 되었습니다. Windows 10, 버전 1607에 **는 windows 10** **기념일 10.0 버전을 대상으로 하는 프로젝트에만 사용 될 수 있습니다. 빌드 14393)** 또는 Visual Studio의 이후 릴리스. **Windows. Store** 네임 스페이스를 사용 하 여 앱 내 제품 구매를 사용 하도록 설정 하는 방법에 대 한 자세한 내용은 [이 문서](enable-consumable-add-on-purchases.md)를 참조 하세요.

## <a name="prerequisites"></a>사전 요구 사항

-   이 항목에서는 소비재 in 앱 제품에 대 한 구매 및 상품 보고에 대해 설명 합니다. 앱 내 제품에 익숙하지 않은 경우 [앱 내 제품 구매 사용](enable-in-app-product-purchases.md) 을 검토 하 여 라이선스 정보에 대해 알아보고 스토어에서 앱 내 제품을 적절 하 게 나열 하는 방법을 검토 하세요.
-   새 앱 내 제품을 처음으로 코딩 하 고 테스트 하는 경우 [Currentapp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 개체 대신 [currentappsimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 개체를 사용 해야 합니다. 이러한 방식으로 라이브 서버를 호출 하는 대신 라이선스 서버에 대 한 시뮬레이션 된 호출을 사용 하 여 라이선스 논리를 확인할 수 있습니다. 이렇게 하려면% userprofile% \\ AppData \\ 로컬 \\ 패키지 \\ &lt; 패키지 이름 &gt; \\ localstate \\ Microsoft \\ Windows 스토어 \\ apidata에서 이름이 WindowsStoreProxy.xml 인 파일을 사용자 지정 해야 합니다. Microsoft Visual Studio 시뮬레이터는 앱을 처음 실행할 때이 파일을 만들거나 런타임에 사용자 지정 항목을 로드할 수도 있습니다. 자세한 내용은 [CurrentAppSimulator에서 WindowsStoreProxy.xml 파일 사용](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy)을 참조 하세요.
-   또한이 항목에서는 [Store 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)에 제공 된 코드 예제를 참조 합니다. 이 샘플은 유니버설 Windows 플랫폼 (UWP) 앱에 제공 되는 다양 한 수익 화 옵션을 사용 하 여 실습 환경을 얻는 좋은 방법입니다.

## <a name="step-1-making-the-purchase-request"></a>1 단계: 구매 요청 만들기

초기 구매 요청은 스토어를 통해 수행 된 다른 구매와 마찬가지로 [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 를 사용 하 여 수행 됩니다. 앱에서 사용할 수 있는 제품의 차이점은 성공적으로 구매한 후에 앱이 이전 구매가 성공적으로 충족 되었음을 스토어에 알릴 때까지 동일한 제품을 다시 구매할 수 없다는 것입니다. 앱은 구매한 소모품을 충족 하 고 스토어에 배송을 알리는 것이 좋습니다.

다음 예제에서는 앱 내 제품 구매 요청을 보여 줍니다. 응용 프로그램에서 두 가지 시나리오 (요청이 성공 했을 때, 동일한 제품의 수행 되지 않은 구매로 인해 요청이 성공 하지 않은 경우)에 대 한 앱 내 제품의 로컬 처리를 수행 해야 하는 경우를 나타내는 코드 주석을 확인할 수 있습니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="MakePurchaseRequest":::

## <a name="step-2-tracking-local-fulfillment-of-the-consumable"></a>2 단계: 소비 가능의 로컬 처리 추적

사용자에 게 사용 가능한 앱 제품에 대 한 액세스 권한을 부여 하는 경우 달성 된 제품 (*productId*) 및 해당 제품이 연결 된 트랜잭션 (*transactionId*)을 추적 하는 것이 중요 합니다.

> [!IMPORTANT]
> 앱은 저장소에 대 한 정확한 보고를 담당 합니다. 이 단계는 고객에 게 안정적이 고 안정적인 구매 환경을 유지 하는 데 필수적입니다.

다음 예제에서는 이전 단계의 [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 호출에서 [PurchaseResults](/uwp/api/Windows.ApplicationModel.Store.PurchaseResults) 속성을 사용 하 여 구매한 제품을 확인 하는 방법을 보여 줍니다. 컬렉션은 제품 정보를 나중에 참조할 수 있는 위치에 저장 하는 데 사용 됩니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="GrantFeatureLocally":::

다음 예제에서는 이전 예제의 배열을 사용 하 여 나중에 저장소에 대 한 처리를 보고할 때 사용 되는 제품 ID/트랜잭션 ID 쌍에 액세스 하는 방법을 보여 줍니다.

> [!IMPORTANT]
> 앱에서 앱을 추적 하 고 등록 하는 데 사용 하는 방법에 관계 없이 앱은 고객이 수신 하지 않은 항목에 대해 요금이 청구 되지 않도록 하기 위해 기한 성실을 시연 해야 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="IsLocallyFulfilled":::

## <a name="step-3-reporting-product-fulfillment-to-the-store"></a>3 단계: 제품을 스토어에 보고

로컬 처리를 완료 한 후에는 앱이 *productId* 와 제품 구매가 포함 된 트랜잭션을 포함 하는 [ReportConsumableFulfillmentAsync](/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync) 호출을 수행 해야 합니다.

> [!IMPORTANT]
> 앱에서 발생 한 앱 내 제품을 스토어에 보고 하지 못하면 이전 구매에 대 한 배송이 보고 될 때까지 사용자가 해당 제품을 다시 구매할 수 없게 됩니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="ReportFulfillment":::

## <a name="step-4-identifying-unfulfilled-purchases"></a>4 단계: 충족 되지 않은 구매 식별

앱은 [GetUnfulfilledConsumablesAsync](/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) 메서드를 사용 하 여 언제 든 지 처리 되지 않은 앱 내 제품을 확인할 수 있습니다. 네트워크 연결 중단 또는 앱 종료와 같은 예기치 않은 앱 이벤트로 인해 존재 하는 처리 되지 않은 소비를 확인 하려면 정기적으로이 메서드를 호출 해야 합니다.

다음 예제에서는 [GetUnfulfilledConsumablesAsync](/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) 를 사용 하 여 처리 되지 않은 소비를 열거 하는 방법 및 앱이이 목록을 반복 하 여 로컬 처리를 완료 하는 방법을 보여 줍니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="GetUnfulfilledConsumables":::

## <a name="related-topics"></a>관련 항목

* [앱에서 바로 구매 제품 사용](enable-in-app-product-purchases.md)
* [스토어 샘플 (평가판 및 앱 내 구매 설명)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Windows.ApplicationModel.Store](/uwp/api/Windows.ApplicationModel.Store)
 

 
