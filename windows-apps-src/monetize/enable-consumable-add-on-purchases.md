---
ms.assetid: FD381669-F962-465E-940B-AED9C8D19C90
description: Windows.Services.Store 네임스페이스를 사용하여 소모성 추가 기능으로 작업하는 방법을 알아봅니다.
title: 소모성 추가 기능 구매 사용
keywords: Windows 10, uwp, 소모성, 추가 기능, 앱에서 바로 구매, IAP, Windows.Services.Store
ms.date: 05/09/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 142c9f90161f4fd61946ccb7452af7ee91f66baa
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334821"
---
# <a name="enable-consumable-add-on-purchases"></a>소모성 추가 기능 구매 사용

이 문서는 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스에서 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스의 메서드를 사용해 UWP 앱에서 사용자의 소모성 추가 기능 이행을 관리하는 방법을 설명합니다. 구매하고 사용한 후 다시 구매할 수 있는 항목을 위한 소모성 추가 기능을 사용합니다. 이 기능은 구매한 후 특정 회복 아이템을 구매하는 데 사용할 수 있는 게임 내 통화(금, 동전 등) 등에 특히 유용합니다.

> [!NOTE]
> **Windows.Services.Store** 네임스페이스는 Windows 10 버전, 1607에 도입되었으며 **Windows 10 Anniversary Edition(10.0, 빌드 14393)** 또는 Visual Studio의 최신 릴리스를 대상으로 하는 프로젝트에만 사용할 수 있습니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 **Windows.Services.Store** 네임스페이스 대신 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스를 사용해야 합니다. 자세한 내용은 [이 문서](enable-consumable-in-app-product-purchases.md)를 참조하세요.

## <a name="overview-of-consumable-add-ons"></a>소모성 추가 기능 개요

앱은 처리 관리 방법이 서로 다른 두 가지 유형의 소모성 추가 기능을 제공할 수 있습니다.

* **개발자 관리 소모성**. 이 소모성 유형의 경우 개발자가 추가 기능이 나타내는 항목의 사용자 잔액을 추적하고 사용자가 항목을 모두 사용한 후 추가 기능 구매를 처리된 것으로 스토어에 보고해야 합니다. 사용자는 앱에서 이전 추가 기능 구매를 처리된 것으로 보고할 때까지 추가 기능을 다시 구매할 수 없습니다.

  예를 들어 게임에서 추가 기능이 100개 동전을 나타내고 사용자가 10개 동전을 사용한 경우 앱 또는 서비스에서 사용자의 남은 새 잔액인 90개 동전을 유지 관리해야 합니다. 사용자가 100개 동전을 모두 사용한 후 앱에서 추가 기능을 처리된 것으로 보고해야 하며, 그러면 사용자가 100개 동전 추가 기능을 다시 구매할 수 있습니다.

* **스토어 관리 소모성**. 이 소모성 유형의 경우 스토어에서 추가 기능이 나타내는 항목의 사용자 잔액을 추적합니다. 사용자는 항목을 사용할 때 해당 항목을 처리된 것으로 스토어에 보고해야 하며, 스토어에서 사용자 잔액을 업데이트합니다. 사용자는 원하는 만큼 추가 기능을 구입할 수 있습니다(항목을 먼저 소비할 필요는 없음). 앱은 언제든지 Microsoft Store에 사용자의 현재 잔액을 쿼리할 수 있습니다.

  예를 들어 게임에서 추가 기능이 초기 수량인 100개 동전을 나타내고 사용자가 50개 동전을 사용한 경우 앱은 추가 기능의 50개 단위가 처리되었다고 Microsoft Store에 보고하고 Microsoft Store에서 남은 잔액을 업데이트합니다. 그러면 사용자는 기능을 다시 구입하여 동전을 100개 획득하고, 동전을 총 150개 갖게 됩니다.
    > [!NOTE]
    > Microsoft Store 관리 소모성은 Windows 10 버전 1607에 도입되었습니다.

사용자에게 소모성 추가 기능을 제공하려면 다음과 같은 일반 프로세스를 따르세요.

1. 사용자가 앱에서 [추가 기능을 구매](enable-in-app-purchases-of-apps-and-add-ons.md)할 수 있게 합니다.
3. 사용자가 추가 기능을 사용하면(예: 게임에서 동전 소비) [추가 기능을 처리된 것으로 보고](enable-consumable-add-on-purchases.md#report_fulfilled)합니다.

언제든지 스토어 관리 소모성의 [남은 잔액을 확인](enable-consumable-add-on-purchases.md#get_balance)할 수도 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

이러한 예제의 필수 조건은 다음과 같습니다.
* **Windows 10 Anniversary Edition(10.0, 빌드 14393)** 이상 릴리스를 대상으로 하는 UWP(유니버설 Windows 플랫폼) 앱에 대한 Visual Studio 프로젝트.
* 있다고 [는 응용 프로그램 제출 생성](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) 파트너 센터에이 앱 스토어에 게시 됩니다. 테스트하는 동안 Store에서 검색이 되지 않도록 앱을 구성할 수도 있습니다. 자세한 내용은 [테스트 지침](in-app-purchases-and-trials.md#testing)을 참조하세요.
* 있다고 [앱에 대 한 추가 기능을 사용할 수 있도록 만든](../publish/add-on-submissions.md) 파트너 센터에서.

이러한 예제의 코드에서는 다음과 같이 가정합니다.
* 코드는 ```workingProgressRing```이라는 [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx)과 ```textBlock```이라는 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)을 포함하는 [페이지](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx)의 컨텍스트에서 실행됩니다. 해당 개체를 사용하여 각각 비동기 작업이 발생함을 나타내고 출력 메시지를 표시합니다.
* 코드 파일에는 **Windows.Services.Store** 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md#api_intro)을 참조하세요.

전체 샘플 응용 프로그램은 [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)을 참조하세요.

> [!NOTE]
> [데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop)를 사용하는 데스크톱 응용 프로그램이 있는 경우 이 예에서 표시되지 않는 별도의 코드를 추가하여 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 개체를 구성해야 할 수도 있습니다. 자세한 내용은 [데스크톱 브리지를 사용하는 데스크톱 응용 프로그램에서 StoreContext 클래스 사용](in-app-purchases-and-trials.md#desktop)을 참조하세요.

<span id="report_fulfilled" />

## <a name="report-a-consumable-add-on-as-fulfilled"></a>소모성 추가 기능을 처리된 것으로 보고

사용자가 앱에서 [추가 기능을 구매](enable-in-app-purchases-of-apps-and-add-ons.md)하고 추가 기능을 사용한 후 앱은 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스의 [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync) 메서드를 호출하여 추가 기능을 처리된 것으로 보고해야 합니다. 이 메서드에 다음 정보를 전달해야 합니다.

* 처리된 것으로 보고할 추가 기능의 [스토어 ID](in-app-purchases-and-trials.md#store-ids)
* 처리된 것으로 보고할 추가 기능의 단위.
  * 개발자 관리 소모성의 경우 *quantity* 매개 변수에 대해 1을 지정합니다. 그러면 소모성이 처리되었다고 스토어에 알리며 고객이 소모성을 다시 구매할 수 있습니다. 앱이 스토어에 처리되었다고 알리기 전에는 사용자가 소모성을 다시 구매할 수 없습니다.
  * 스토어 관리 소모성의 경우 사용된 실제 단위 수를 지정합니다. 스토어에서 소모성의 남은 잔액을 업데이트합니다.
* 처리의 추적 ID. 처리 작업이 연결된 특정 트랜잭션을 추적용으로 식별하는 개발자 제공 GUID입니다. 자세한 내용은 [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync)의 설명을 참조하세요.

이 예제에서는 스토어에서 관리하는 소모성을 처리된 것으로 보고하는 방법을 보여 줍니다.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/ConsumeAddOnPage.xaml.cs#ConsumeAddOn)]

<span id="get_balance" />

## <a name="get-the-remaining-balance-for-a-store-managed-consumable"></a>스토어에서 관리하는 소모성의 남은 잔액 가져오기

이 예제에서는 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스의 [GetConsumableBalanceRemainingAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getconsumablebalanceremainingasync) 메서드를 사용하여 스토어에서 관리하는 소모성 추가 기능의 남은 잔액을 가져오는 방법을 보여 줍니다.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/GetRemainingAddOnBalancePage.xaml.cs#GetRemainingAddOnBalance)]

## <a name="related-topics"></a>관련 항목

* [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)
* [앱 및 추가 기능에 대 한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱 및 추가 기능에 대 한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [앱 내 구매는 응용 프로그램 및 추가 기능을 사용 하도록 설정](enable-in-app-purchases-of-apps-and-add-ons.md)
* [앱의 평가판 버전을 구현 합니다.](implement-a-trial-version-of-your-app.md)
* [Store 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
