---
author: mcleanbyron
ms.assetid: 89178FD9-850B-462F-9016-1AD86D1F6F7F
description: "Windows.Services.Store 네임스페이스를 사용하여 현재 앱이나 추가 기능 중 하나에 대한 스토어 관련 제품 정보를 가져오는 방법을 알아봅니다."
title: "앱 및 추가 기능에 대한 제품 정보 가져오기"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 앱에서 바로 구매, IAP, 추가 기능, Windows.Services.Store"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7e486c451174cd24429dc35cda07d22fe2b28745
ms.lasthandoff: 02/07/2017

---

# <a name="get-product-info-for-apps-and-add-ons"></a>앱 및 추가 기능에 대한 제품 정보 가져오기

Windows 10 버전 1607 이상을 대상으로 하는 앱은 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스에 있는 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스의 메서드를 사용하여 현재 앱이나 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함) 중 하나에 대한 스토어 관련 정보에 액세스할 수 있습니다. 이 문서의 다음 예제에서는 다양한 시나리오에서 이 작업을 수행하는 방법을 보여 줍니다. 전체 샘플은 [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)을 참조하세요.

>**참고**&nbsp;&nbsp;이 문서는 Windows 10 버전 1607 이상을 대상으로 하는 앱에 적용할 수 있습니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 **Windows.Services.Store** 네임스페이스 대신 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스를 사용해야 합니다. 자세한 내용은 [Windows.ApplicationModel.Store 네임스페이스를 사용하는 앱에서 바로 구매 및 평가판](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)을 참조하세요.

## <a name="prerequisites"></a>필수 조건

이러한 예제의 필수 조건은 다음과 같습니다.
* Windows 10 버전 1607 이상을 대상으로 하는 UWP(유니버설 Windows 플랫폼) 앱에 대한 Visual Studio 프로젝트.
* Windows 개발자 센터 대시보드에서 앱을 만들었으며, 이 앱은 스토어에서 게시되고 사용할 수 있습니다. 고객에게 릴리스하려는 앱일 수도 있고, 테스트용으로만 사용 중인 최소 [Windows 앱 인증 키트](https://developer.microsoft.com/windows/develop/app-certification-kit) 요구 사항을 충족하는 기본 앱일 수도 있습니다. 자세한 내용은 [테스트 지침](in-app-purchases-and-trials.md#testing)을 참조하세요.

이러한 예제의 코드에서는 다음과 같이 가정합니다.
* 코드는 ```workingProgressRing```이라는 [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx)과 ```textBlock```이라는 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)을 포함하는 [페이지](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx)의 컨텍스트에서 실행됩니다. 해당 개체를 사용하여 각각 비동기 작업이 발생함을 나타내고 출력 메시지를 표시합니다.
* 코드 파일에는 **Windows.Services.Store** 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md#api_intro)을 참조하세요.

전체 샘플 응용 프로그램은 [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)을 참조하세요.

>**참고**&nbsp;&nbsp;[데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop)를 사용하는 데스크톱 응용 프로그램이 있는 경우 이 예에서 표시되지 않는 별도의 코드를 추가하여 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 개체를 구성해야 할 수도 있습니다. 자세한 내용은 [데스크톱 브리지를 사용하는 데스크톱 응용 프로그램에서 StoreContext 클래스 사용](in-app-purchases-and-trials.md#desktop)을 참조하세요.

## <a name="get-info-for-the-current-app"></a>현재 앱에 대한 정보 가져오기

현재 앱에 대한 스토어 제품 정보를 가져오려면 [GetStoreProductForCurrentAppAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getstoreproductforcurrentappasync.aspx) 메서드를 사용합니다. 이는 가격 등의 정보를 가져오는 데 사용할 수 있는 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 개체를 반환하는 비동기 메서드입니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAppInfoPage.xaml.cs#GetAppInfo)]

## <a name="get-info-for-products-with-known-store-ids"></a>스토어 ID를 알고 있는 제품에 대한 정보 가져오기

[스토어 ID](in-app-purchases-and-trials.md#store_ids)를 이미 알고 있는 앱 또는 추가 기능에 대한 스토어 제품 정보를 가져오려면 [GetStoreProductsAsync](https://msdn.microsoft.com/library/windows/apps/mt706579.aspx) 메서드를 사용합니다. 이는 각 앱이나 추가 기능을 나타내는 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 개체 컬렉션을 반환하는 비동기 메서드입니다. 스토어 ID 외에도 추가 기능 유형을 식별하는 문자열 목록을 이 메서드에 전달해야 합니다. 지원되는 문자열 값 목록은 [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx) 속성을 참조하세요.

다음 예제에서는 지정한 스토어 ID를 가진 지속형 추가 기능에 대한 정보를 검색합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetProductInfoPage.xaml.cs#GetProductInfo)]

## <a name="get-info-for-add-ons-that-are-available-for-the-current-app"></a>현재 앱에 사용할 수 있는 추가 기능에 대한 정보 가져오기

현재 앱에 사용할 수 있는 추가 기능에 대한 스토어 제품 정보를 가져오려면 [GetAssociatedStoreProductsAsync](https://msdn.microsoft.com/library/windows/apps/mt706571.aspx) 메서드를 사용합니다. 이는 사용 가능한 각 추가 기능을 나타내는 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 개체 컬렉션을 반환하는 비동기 메서드입니다. 검색할 추가 기능 유형을 식별하는 문자열 목록을 이 메서드에 전달해야 합니다. 지원되는 문자열 값 목록은 [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx) 속성을 참조하세요.

>**참고**&nbsp;&nbsp;앱에 많은 추가 기능이 있는 경우 [GetAssociatedStoreProductsWithPagingAsync](https://msdn.microsoft.com/library/windows/apps/mt706572.aspx) 메서드를 통해 페이징을 사용하여 추가 기능 결과를 반환할 수도 있습니다.

다음 예제에서는 모든 지속형 추가 기능, 스토어에서 관리하는 소모성 추가 기능 및 개발자가 관리하는 소모성 추가 기능에 대한 정보를 검색합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAddOnInfoPage.xaml.cs#GetAddOnInfo)]


## <a name="get-info-for-add-ons-for-the-current-app-that-the-current-user-is-entitled-to-use"></a>현재 사용자가 사용할 수 있는 현재 앱용 추가 기능에 대한 정보 가져오기

현재 사용자가 사용할 수 있는 추가 기능에 대한 스토어 제품 정보를 가져오려면 [GetUserCollectionAsync](https://msdn.microsoft.com/library/windows/apps/mt706580.aspx) 메서드를 사용합니다. 이는 각 추가 기능을 나타내는 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 개체 컬렉션을 반환하는 비동기 메서드입니다. 검색할 추가 기능 유형을 식별하는 문자열 목록을 이 메서드에 전달해야 합니다. 지원되는 문자열 값 목록은 [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx) 속성을 참조하세요.

>**참고**&nbsp;&nbsp;앱에 많은 추가 기능이 있는 경우 [GetUserCollectionWithPagingAsync](https://msdn.microsoft.com/library/windows/apps/mt706581.aspx) 메서드를 통해 페이징을 사용하여 추가 기능 결과를 반환할 수도 있습니다.

다음 예제에서는 지정한 스토어 ID를 가진 지속형 추가 기능에 대한 정보를 검색합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetUserCollectionPage.xaml.cs#GetUserCollection)]

## <a name="related-topics"></a>관련 항목

* [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)
* [앱 및 추가 기능에 대한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [앱에서 바로 앱 및 추가 기능 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md)
* [소모성 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)
* [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)
* [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)

