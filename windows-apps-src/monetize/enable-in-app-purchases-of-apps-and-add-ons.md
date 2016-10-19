---
author: mcleanbyron
ms.assetid: B356C442-998F-4B2C-B550-70070C5E4487
description: "Windows.Services.Store 네임스페이스를 사용하여 앱 또는 해당 추가 기능 중 하나를 구매하는 방법을 알아봅니다."
title: "앱에서 바로 앱 및 추가 기능 구매 사용"
keywords: "앱에서 바로 판매 코드 샘플"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 0347c3a72ccdf26ddd885a5bad944ae36e09a190

---

# 앱에서 바로 앱 및 추가 기능 구매 사용

Windows 10 버전 1607 이상을 대상으로 하는 앱은 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스의 멤버를 사용하여 현재 앱 또는 해당 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함) 중 하나를 구매 요청할 수 있습니다. 예를 들어 사용자에게 현재 앱의 평가판이 있는 경우 이 프로세스를 사용하여 사용자에 대한 정식 라이선스를 구매할 수 있습니다. 또는 이 프로세스를 사용하여 사용자에 대한 새 게임 수준과 같은 추가 기능을 구매할 수 있습니다.

[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)는 앱 또는 추가 기능 구매를 요청하는 데 다양한 방법을 제공합니다.
* 앱 또는 추가 기능의 [스토어 ID](in-app-purchases-and-trials.md#store_ids)를 알고 있는 경우 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스의 [RequestPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.requestpurchaseasync.aspx) 메서드를 사용할 수 있습니다.
* 앱 또는 추가 기능을 나타내는 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx), [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) 또는 [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) 개체가 이미 있는 경우 이러한 개체의 **RequestPurchaseAsync** 메서드를 사용할 수 있습니다.

각 메서드는 표준 구매 UI를 사용자에게 제공하고 트랜잭션을 마치면 비동기적으로 완료됩니다. 메서드가 트랜잭션이 성공했는지 여부를 나타내는 개체를 반환합니다.

>**참고**&nbsp;&nbsp;이 문서는 Windows 10 버전 1607 이상을 대상으로 하는 앱에 적용할 수 있습니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 **Windows.Services.Store** 네임스페이스 대신 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스를 사용해야 합니다. 자세한 내용은 [Windows.ApplicationModel.Store 네임스페이스를 사용하는 앱에서 바로 구매 및 평가판](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)을 참조하세요.

## 필수 조건

이 예제의 필수 조건은 다음과 같습니다.
* Windows 10 버전 1607 이상을 대상으로 하는 UWP(유니버설 Windows 플랫폼) 앱에 대한 Visual Studio 프로젝트.
* Windows 개발자 센터 대시보드에서 앱을 만들었으며, 이 앱은 스토어에서 게시되고 사용할 수 있습니다. 고객에게 릴리스하려는 앱일 수도 있고, 테스트용으로만 사용 중인 최소 [Windows 앱 인증 키트](https://developer.microsoft.com/windows/develop/app-certification-kit) 요구 사항을 충족하는 기본 앱일 수도 있습니다. 자세한 내용은 [테스트 지침](in-app-purchases-and-trials.md#testing)을 참조하세요.

이 예제의 코드에서는 다음과 같이 가정합니다.
* 코드는 ```workingProgressRing```이라는 [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx)과 ```textBlock```이라는 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)을 포함하는 [페이지](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx)의 컨텍스트에서 실행됩니다. 해당 개체를 사용하여 각각 비동기 작업이 발생함을 나타내고 출력 메시지를 표시합니다.
* 코드 파일에는 **Windows.Services.Store** 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md#api_intro)을 참조하세요.

## 코드 예제

이 예제에서는 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스의 [RequestPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.requestpurchaseasync.aspx) 메서드를 사용하여 알려진 [스토어 ID](in-app-purchases-and-trials.md#store_ids)로 앱 또는 추가 기능을 구매하는 방법을 보여 줍니다.

```csharp
private StoreContext context = null;

public async void PurchaseAddOn(string storeId)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    workingProgressRing.IsActive = true;
    StorePurchaseResult result = await context.RequestPurchaseAsync(storeId);
    workingProgressRing.IsActive = false;

    if (result.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {result.ExtendedError.Message}";
        return;
    }

    switch (result.Status)
    {
        case StorePurchaseStatus.AlreadyPurchased:
            textBlock.Text = "The user has already purchased the product.";
            break;

        case StorePurchaseStatus.Succeeded:
            textBlock.Text = "The purchase was successful.";
            break;

        case StorePurchaseStatus.NotPurchased:
            textBlock.Text = "The purchase did not complete. " +
                "The user may have cancelled the purchase.";
            break;

        case StorePurchaseStatus.NetworkError:
            textBlock.Text = "The purchase was unsuccessful due to a network error.";
            break;

        case StorePurchaseStatus.ServerError:
            textBlock.Text = "The purchase was unsuccessful due to a server error.";
            break;

        default:
            textBlock.Text = "The purchase was unsuccessful due to an unknown error.";
            break;
    }
}
```

전체 샘플 응용 프로그램은 [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)을 참조하세요.

## 관련 항목

* [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)
* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱 및 추가 기능에 대한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [소모성 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)
* [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)
* [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Aug16_HO5-->


