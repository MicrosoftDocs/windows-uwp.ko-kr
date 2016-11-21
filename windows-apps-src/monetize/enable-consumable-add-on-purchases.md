---
author: mcleanbyron
ms.assetid: FD381669-F962-465E-940B-AED9C8D19C90
description: "Windows.Services.Store 네임스페이스를 사용하여 소모성 추가 기능으로 작업하는 방법을 알아봅니다."
title: "소모성 추가 기능 구매 사용"
keywords: "앱에서 바로 판매 코드 샘플"
translationtype: Human Translation
ms.sourcegitcommit: 962bee0cae8c50407fe1509b8000dc9cf9e847f8
ms.openlocfilehash: eb188ed8e69f90727c5b57af1c407fac07eaf87d

---

# 소모성 추가 기능 구매 사용

Windows10 버전 1607 이상을 대상으로 하는 앱은 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스에 있는 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스의 메서드를 사용하여 UWP 앱의 소모성 추가 기능에 대한 사용자 처리를 관리할 수 있습니다. 구매하고 사용한 후 다시 구매할 수 있는 항목을 위한 소모성 추가 기능을 사용합니다. 이 기능은 특정 회복 아이템을 구매하여 사용할 수 있는 게임 내 통화(금, 동전 등) 등에 특히 유용합니다.

>
  **참고**
  &nbsp;&nbsp;이 문서는 Windows10 버전 1607 이상을 대상으로 하는 앱에 적용할 수 있습니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 **Windows.Services.Store** 네임스페이스 대신 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스를 사용해야 합니다. 자세한 내용은 [Windows.ApplicationModel.Store 네임스페이스를 사용하는 앱에서 바로 구매 및 평가판](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)을 참조하세요.

## 소모성 추가 기능 개요

Windows10 버전 1607 이상을 대상으로 하는 앱은 처리 관리 방법이 서로 다른 두 가지 유형의 소모성 추가 기능을 제공할 수 있습니다.

* **개발자 관리 소모성**. 이 소모성 유형의 경우 개발자가 추가 기능이 나타내는 항목의 사용자 잔액을 추적하고 사용자가 항목을 모두 사용한 후 추가 기능 구매를 처리된 것으로 스토어에 보고해야 합니다. 사용자는 앱에서 이전 추가 기능 구매를 처리된 것으로 보고할 때까지 추가 기능을 다시 구매할 수 없습니다.

  예를 들어 게임에서 추가 기능이 100개 동전을 나타내고 사용자가 10개 동전을 사용한 경우 앱 또는 서비스에서 사용자의 남은 새 잔액인 90개 동전을 유지 관리해야 합니다. 사용자가 100개 동전을 모두 사용한 후 앱에서 추가 기능을 처리된 것으로 보고해야 하며, 그러면 사용자가 100개 동전 추가 기능을 다시 구매할 수 있습니다.

* **스토어 관리 소모성**. 이 소모성 유형의 경우 스토어에서 추가 기능이 나타내는 항목의 사용자 잔액을 추적합니다. 사용자는 항목을 사용할 때 해당 항목을 처리된 것으로 스토어에 보고해야 하며, 스토어에서 사용자 잔액을 업데이트합니다. 앱은 언제든지 사용자의 현재 잔액을 쿼리할 수 있습니다. 사용자는 모든 항목을 사용한 후 추가 기능을 다시 구매할 수 있습니다.

  예를 들어 게임에서 추가 기능이 초기 수량인 100개 동전을 나타내고 사용자가 10개 동전을 사용한 경우 앱은 추가 기능의 10개 단위가 처리되었다고 스토어에 보고하고 스토어에서 남은 잔액을 업데이트합니다. 사용자는 100개 동전을 모두 사용한 후 100개 동전 추가 기능을 다시 구매할 수 있습니다.

  >
  **참고**
  &nbsp;&nbsp;스토어 관리 소모성은 Windows10 버전 1607부터 사용할 수 있습니다. Windows 개발자 센터 대시보드에서 스토어 관리 소모성을 만드는 기능이 곧 제공될 예정입니다.

사용자에게 소모성 추가 기능을 제공하려면 다음과 같은 일반 프로세스를 따르세요.

1. 사용자가 앱에서 [추가 기능을 구매](enable-in-app-purchases-of-apps-and-add-ons.md)할 수 있게 합니다.
3. 사용자가 추가 기능을 사용하면(예: 게임에서 동전 소비) [추가 기능을 처리된 것으로 보고](enable-consumable-add-on-purchases.md#report_fulfilled)합니다.

언제든지 스토어 관리 소모성의 [남은 잔액을 확인](enable-consumable-add-on-purchases.md#get_balance)할 수도 있습니다.

## 필수 조건

이러한 예제의 필수 조건은 다음과 같습니다.
* Windows10 버전 1607 이상을 대상으로 하는 UWP(유니버설 Windows 플랫폼) 앱에 대한 Visual Studio 프로젝트.
* 소모성 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함)을 사용하여 Windows 개발자 센터 대시보드에서 앱을 만들었으며, 이 앱은 스토어에서 게시되고 사용할 수 있습니다. 고객에게 릴리스하려는 앱일 수도 있고, 테스트용으로만 사용 중인 최소 [Windows 앱 인증 키트](https://developer.microsoft.com/windows/develop/app-certification-kit) 요구 사항을 충족하는 기본 앱일 수도 있습니다. 자세한 내용은 [테스트 지침](in-app-purchases-and-trials.md#testing)을 참조하세요.

이러한 예제의 코드에서는 다음과 같이 가정합니다.
* 코드는 ```workingProgressRing```이라는 [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx)과 ```textBlock```이라는 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)을 포함하는 [페이지](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx)의 컨텍스트에서 실행됩니다. 해당 개체를 사용하여 각각 비동기 작업이 발생함을 나타내고 출력 메시지를 표시합니다.
* 코드 파일에는 **Windows.Services.Store** 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md#api_intro)을 참조하세요.

전체 샘플 응용 프로그램은 [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)을 참조하세요.

>**참고**&nbsp;&nbsp;[데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop)를 사용하는 데스크톱 응용 프로그램이 있는 경우 이 예에서 표시되지 않는 별도의 코드를 추가하여 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 개체를 구성해야 할 수도 있습니다. 자세한 내용은 [데스크톱 브리지를 사용하는 데스크톱 응용 프로그램에서 StoreContext 클래스 사용](in-app-purchases-and-trials.md#desktop)을 참조하세요.

<span id="report_fulfilled" />
## 소모성 추가 기능을 처리된 것으로 보고

사용자가 앱에서 [추가 기능을 구매](enable-in-app-purchases-of-apps-and-add-ons.md)하고 추가 기능을 사용한 후 앱은 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스의 [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.reportconsumablefulfillmentasync.aspx) 메서드를 호출하여 추가 기능을 처리된 것으로 보고해야 합니다. 이 메서드에 다음 정보를 전달해야 합니다.

* 처리된 것으로 보고할 추가 기능의 [스토어 ID](in-app-purchases-and-trials.md#store_ids)
* 처리된 것으로 보고할 추가 기능의 단위.
  * 개발자 관리 소모성의 경우 *quantity* 매개 변수에 대해 1을 지정합니다. 그러면 소모성이 처리되었다고 스토어에 알리며 고객이 소모성을 다시 구매할 수 있습니다. 앱이 스토어에 처리되었다고 알리기 전에는 사용자가 소모성을 다시 구매할 수 없습니다.
  * 스토어 관리 소모성의 경우 사용된 실제 단위 수를 지정합니다. 스토어에서 소모성의 남은 잔액을 업데이트합니다.
* 처리의 추적 ID. 처리 작업이 연결된 특정 트랜잭션을 추적용으로 식별하는 개발자 제공 GUID입니다. 자세한 내용은 [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.reportconsumablefulfillmentasync.aspx)의 설명을 참조하세요.

이 예제에는 스토어 관리 소모성을 처리된 것으로 보고하는 방법을 보여 줍니다.

```csharp
private StoreContext context = null;

public async void ConsumeAddOn(string storeId)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
        // If your app is a desktop app that uses the Desktop Bridge, you
        // may need additional code to configure the StoreContext object.
        // For more info, see https://aka.ms/storecontext-for-desktop.
    }

    // This is an example for a Store-managed consumable, where you specify the actual number
    // of units that you want to report as consumed so the Store can update the remaining
    // balance. For a developer-managed consumable where you maintain the balance, specify 1
    // to just report the add-on as fulfilled to the Store.
    uint quantity = 10;
    string addOnStoreId = "9NBLGGH4TNNR";
    Guid trackingId = Guid.NewGuid();

    workingProgressRing.IsActive = true;
    StoreConsumableResult result = await context.ReportConsumableFulfillmentAsync(
        addOnStoreId, quantity, trackingId);
    workingProgressRing.IsActive = false;

    if (result.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {result.ExtendedError.Message}";
        return;
    }

    switch (result.Status)
    {
        case StoreConsumableStatus.Succeeded:
            textBlock.Text = "The fulfillment was successful. Remaining balance: " +
                result.BalanceRemaining;
            break;

        case StoreConsumableStatus.InsufficentQuantity:
            textBlock.Text = "The fulfillment was unsuccessful because the user " +
            "doesn't have enough remaining balance." + result.BalanceRemaining;
            break;

        case StoreConsumableStatus.NetworkError:
            textBlock.Text = "The fulfillment was unsuccessful due to a network error.";
            break;

        case StoreConsumableStatus.ServerError:
            textBlock.Text = "The fulfillment was unsuccessful due to a server error.";
            break;

        default:
            textBlock.Text = "The fulfillment was unsuccessful due to an unknown error.";
            break;
    }
}
```

<span id="get_balance" />
## 스토어 관리 소모성의 남은 잔액 가져오기

이 예제에서는 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스의 [GetConsumableBalanceRemainingAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getconsumablebalanceremainingasync.aspx) 메서드를 사용하여 스토어 관리 소모성 추가 기능의 남은 잔액을 가져오는 방법을 보여 줍니다.

```csharp
private StoreContext context = null;

public async void GetRemainingBalance(string storeId)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
        // If your app is a desktop app that uses the Desktop Bridge, you
        // may need additional code to configure the StoreContext object.
        // For more info, see https://aka.ms/storecontext-for-desktop.
    }

    string addOnStoreId = "9NBLGGH4TNNR";

    workingProgressRing.IsActive = true;
    StoreConsumableResult result = await context.GetConsumableBalanceRemainingAsync(addOnStoreId);
    workingProgressRing.IsActive = false;

    if (result.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {result.ExtendedError.Message}";
        return;
    }

    switch (result.Status)
    {
        case StoreConsumableStatus.Succeeded:
            textBlock.Text = "Remaining balance: " + result.BalanceRemaining;
            break;

        case StoreConsumableStatus.NetworkError:
            textBlock.Text = "Could not retrieve balance due to a network error.";
            break;

        case StoreConsumableStatus.ServerError:
            textBlock.Text = "Could not retrieve balance due to a server error.";
            break;

        default:
            textBlock.Text = "Could not retrieve balance due to an unknown error.";
            break;
    }
}
```

## 관련 항목

* [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)
* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱 및 추가 기능에 대한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [앱에서 바로 앱 및 추가 기능 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md)
* [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)
* [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Nov16_HO1-->


