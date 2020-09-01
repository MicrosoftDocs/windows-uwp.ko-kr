---
ms.assetid: FD381669-F962-465E-940B-AED9C8D19C90
description: 사용 가능한 추가 기능을 사용 하는 데 사용 하는 방법에 대해 알아봅니다.
title: 소모성 추가 기능 구매 사용
keywords: windows 10, uwp, 사용할 기능, 추가 기능, 앱 내 구매, IAPs, Windows. Service 스토어
ms.date: 05/09/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4f09b9a5c1f53c6a33f830c72514e061dc893348
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171587"
---
# <a name="enable-consumable-add-on-purchases"></a>소모성 추가 기능 구매 사용

이 문서에서는 [Windows. Store](/uwp/api/windows.services.store) 네임 스페이스에서 [storecontext](/uwp/api/windows.services.store.storecontext) 클래스의 메서드를 사용 하 여 UWP 앱에서 사용 가능한 추가 기능의 사용자를 관리 하는 방법을 보여 줍니다. 다시 구매, 사용 및 구매할 수 있는 항목에 대해 사용 가능한 추가 기능을 사용 합니다. 이 기능은 구입할 수 있는 게임 내 통화 (골드, 동전 등)와 특정 전원 공급 장치를 구입 하는 데 사용할 수 있는 작업에 특히 유용 합니다.

> [!NOTE]
> Windows 10, 버전 1607에 **는 windows 10** **기념일 10.0 버전을 대상으로 하는 프로젝트에만 사용 될 수 있습니다. 빌드 14393)** 또는 Visual Studio의 이후 릴리스. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우에는 windows. **store** 네임 스페이스 대신 [windows](/uwp/api/windows.applicationmodel.store) 를 사용 해야 합니다. 자세한 내용은 [이 문서](enable-consumable-in-app-product-purchases.md)를 참조하세요.

## <a name="overview-of-consumable-add-ons"></a>사용할 때 추가 기능 개요

앱은 fulfillments 관리 되는 방식과 다른 두 가지 유형의 사용할 수 있는 추가 기능을 제공할 수 있습니다.

* **개발자 관리 가능**. 이러한 유형의 사용을 위해 사용자는 추가 기능이 나타내는 항목의 균형을 추적 하 고 사용자가 모든 항목을 사용한 후에 저장소에 대 한 추가 기능 구매를 보고할 책임이 있습니다. 사용자는 앱이 이전 추가 기능 구매를 충족 된 것으로 보고 하기 전까지 추가 기능을 다시 구매할 수 없습니다.

  예를 들어, 추가 기능이 게임에서 100 동전을 나타내며 사용자가 10kb를 사용 하는 경우 앱 이나 서비스는 사용자에 대해 새로운 남은 잔액을 90로 유지 해야 합니다. 사용자가 모든 100을 사용한 후 앱에서 추가 기능을 충족 된 것으로 보고 해야 합니다. 그러면 사용자가 100 동전 추가 기능을 다시 구매할 수 있습니다.

* **저장소 관리 기능 가능**. 이러한 유형의 사용을 위해 저장소는 추가 기능이 나타내는 항목의 사용자 잔액을 추적 합니다. 사용자가 항목을 사용 하는 경우 해당 항목을 저장소에 대해 충족 된 것으로 보고 하 고, 스토어에서 사용자의 잔액을 업데이트 합니다. 사용자는 원하는 횟수 만큼 추가 기능을 구매할 수 있습니다 (항목을 먼저 사용할 필요는 없음). 앱은 언제 든 지 사용자의 현재 잔액에 대해 스토어를 쿼리할 수 있습니다.

  예를 들어 추가 기능이 게임에서 100 동전의 초기 수량을 표시 하 고 사용자가 50을 사용 하는 경우 앱은 추가 기능에 대 한 50 단위가 충족 된 상점에 보고 하 고, 저장소는 남은 잔액을 업데이트 합니다. 사용자가 추가 기능을 다시 구매 하 여 더 100 동전을 획득 하는 경우에는 이제 150 동전 합계가 발생 합니다.
    > [!NOTE]
    > 저장소 관리 된 소모품은 Windows 10 버전 1607에서 도입 되었습니다.

사용자에 게 사용 가능한 추가 기능을 제공 하려면 다음과 같은 일반 프로세스를 수행 합니다.

1. 사용자가 앱에서 [추가 기능을 구매할](enable-in-app-purchases-of-apps-and-add-ons.md) 수 있습니다.
3. 사용자가 추가 기능을 사용 하는 경우 (예: 게임에서 동전을 소비 하는 경우) [추가 기능을 충족 된 것으로 보고](enable-consumable-add-on-purchases.md#report_fulfilled)합니다.

언제 든 지 저장소 관리 사용할 수 있도록 [남은 잔액을 얻을](enable-consumable-add-on-purchases.md#get_balance) 수도 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

이러한 예제에는 다음과 같은 필수 구성 요소가 있습니다.
* **Windows 10 기념일 Edition (10.0;)을 대상으로 하는 UWP (유니버설 Windows 플랫폼) 앱 용 Visual Studio 프로젝트입니다. 빌드 14393)** 이상 릴리스
* 파트너 센터에서 [앱 제출을 만들었으며](../publish/app-submissions.md) 이 앱은 스토어에 게시 됩니다. 응용 프로그램을 테스트 하는 동안 저장소에서 검색할 수 없도록 앱을 선택적으로 구성할 수 있습니다. 자세한 내용은 [테스트 지침](in-app-purchases-and-trials.md#testing)을 참조 하세요.
* 파트너 센터에서 [앱에 대 한 사용할 수 있는 추가 기능을 만들었습니다](../publish/add-on-submissions.md) .

이 예제의 코드는 다음을 가정 합니다.
* 이 코드는 이름이 인 [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) TextBlock이 포함 된 [페이지](/uwp/api/windows.ui.xaml.controls.page) 의 컨텍스트에서 실행 됩니다 ```workingProgressRing``` [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) ```textBlock``` . 이러한 개체를 사용 하 여 비동기 작업이 진행 중임을 나타내고 출력 메시지를 각각 표시 합니다.
* 코드 파일은 **Windows. Store** 네임 스페이스에 대 한 **using** 문을 포함 합니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md#api_intro)을 참조 하세요.

전체 샘플 응용 프로그램은 [Store 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)을 참조 하세요.

> [!NOTE]
> 데스크톱 [브리지](https://developer.microsoft.com/windows/bridges/desktop)를 사용 하는 데스크톱 응용 프로그램이 있는 경우 다음 예제에 표시 되지 않은 코드를 추가 하 여 [storecontext](/uwp/api/windows.services.store.storecontext) 개체를 구성 해야 할 수 있습니다. 자세한 내용은 [데스크톱 브리지를 사용 하는 데스크톱 응용 프로그램에서 StoreContext 클래스 사용](in-app-purchases-and-trials.md#desktop)을 참조 하세요.

<span id="report_fulfilled" />

## <a name="report-a-consumable-add-on-as-fulfilled"></a>소비 추가 기능을 충족 된 것으로 보고

사용자가 앱에서 [추가 기능을 구입](enable-in-app-purchases-of-apps-and-add-ons.md) 하 고 추가 기능을 사용 하는 경우, 응용 프로그램은 [Storecontext](/uwp/api/windows.services.store.storecontext) 클래스의 [ReportConsumableFulfillmentAsync](/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync) 메서드를 호출 하 여 추가 기능을 충족 된 것으로 보고 해야 합니다. 이 메서드에 다음 정보를 전달 해야 합니다.

* 충족 된 것으로 보고 하려는 추가 기능에 대 한 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다.
* 충족 된 것으로 보고 하려는 추가 기능 단위입니다.
  * 개발자가 관리 하는 사용할 경우 *quantity* 매개 변수에 1을 지정 합니다. 이는 사용할 수 있는 저장소에 경고를 생성 하 고, 고객은이를 다시 구입할 수 있습니다. 사용자는 앱이 완료 되었음을 스토어에 알릴 때까지 사용 가능을 다시 구매할 수 없습니다.
  * 저장소 관리 사용 가능의 경우 사용 된 실제 단위 수를 지정 합니다. 저장소는 사용할 수 있도록 남은 잔액을 업데이트 합니다.
* 처리에 대 한 추적 ID입니다. 이는 추적을 위해 처리 작업이 연결 된 특정 트랜잭션을 식별 하는 개발자 제공 GUID입니다. 자세한 내용은 [ReportConsumableFulfillmentAsync](/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync)의 설명을 참조 하세요.

이 예제에서는 처리 된 것으로 저장소 관리 된 사용할 수를 보고 하는 방법을 보여 줍니다.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/ConsumeAddOnPage.xaml.cs#ConsumeAddOn)]

<span id="get_balance" />

## <a name="get-the-remaining-balance-for-a-store-managed-consumable"></a>저장소 관리 기능을 위한 잔여 잔액 가져오기

이 예제에서는 [Storecontext](/uwp/api/windows.services.store.storecontext) 클래스의 [GetConsumableBalanceRemainingAsync](/uwp/api/windows.services.store.storecontext.getconsumablebalanceremainingasync) 메서드를 사용 하 여 저장소 관리 사용 가능 추가 기능에 대 한 잔여 잔액을 가져오는 방법을 보여 줍니다.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/GetRemainingAddOnBalancePage.xaml.cs#GetRemainingAddOnBalance)]

## <a name="related-topics"></a>관련 항목

* [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)
* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱 및 추가 기능에 대한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [앱에서 바로 앱 및 추가 기능 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md)
* [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)
* [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)