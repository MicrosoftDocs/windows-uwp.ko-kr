---
author: Xansky
ms.assetid: B356C442-998F-4B2C-B550-70070C5E4487
description: Windows.Services.Store 네임스페이스를 사용하여 앱 또는 해당 추가 기능 중 하나를 구매하는 방법을 알아봅니다.
title: 앱에서 바로 앱 및 추가 기능 구매 사용
keywords: windows 10, uwp, 추가 기능, 앱에서 바로 구매, IAP, Windows.Services.Store
ms.author: mhopkins
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: beb7165586c62770fd6b18fff8c7ad0095bc78ba
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5981143"
---
# <a name="enable-in-app-purchases-of-apps-and-add-ons"></a>앱에서 바로 앱 및 추가 기능 구매 사용

이 문서는 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스의 멤버를 사용하여 현재 앱 또는 해당 추가 기능 중 하나를 구매 요청하는 방법을 설명합니다. 예를 들어 사용자에게 현재 앱의 평가판이 있는 경우 이 프로세스를 사용하여 사용자에 대한 정식 라이선스를 구매할 수 있습니다. 또는 이 프로세스를 사용하여 사용자에 대한 새 게임 수준과 같은 추가 기능을 구매할 수 있습니다.

[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스는 앱 또는 추가 기능 구매를 요청하는 데 다양한 방법을 제공합니다.
* 앱의 [스토어 ID](in-app-purchases-and-trials.md#store_ids)나 추가 기능을 알고 있는 경우, [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스의 [RequestPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestpurchaseasync) 메서드를 사용할 수 있습니다.
* 앱 또는 추가 기능을 나타내는 [**StoreProduct**](in-app-purchases-and-trials.md#products-skus), **StoreSku **또는 **StoreAvailability** 개체가 이미 있는 경우 이러한 개체의 **RequestPurchaseAsync **메서드를 사용할 수 있습니다. 코드에서 **StoreProduct**를 검색하는 여러 방법에 대한 예제는 [앱과 추가 기능에 대한 제품 정보 얻기](get-product-info-for-apps-and-add-ons.md)를 확인하세요.

각 메서드는 표준 구매 UI를 사용자에게 제공하고 트랜잭션을 마치면 비동기적으로 완료됩니다. 메서드가 트랜잭션이 성공했는지 여부를 나타내는 개체를 반환합니다.

> [!NOTE]
> **Windows.Services.Store** 네임스페이스는 Windows 10 버전, 1607에 도입되었으며 **Windows 10 Anniversary Edition(10.0, 빌드 14393)** 또는 Visual Studio의 최신 릴리스를 대상으로 하는 프로젝트에만 사용할 수 있습니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 **Windows.Services.Store** 네임스페이스 대신 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스를 사용해야 합니다. 자세한 내용은 [이 문서](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)를 참조하세요.

## <a name="prerequisites"></a>필수 구성 요소

이 예제의 필수 조건은 다음과 같습니다.
* **Windows 10 Anniversary Edition(10.0, 빌드 14393)** 이상 릴리스를 대상으로 하는 UWP(유니버설 Windows 플랫폼) 앱에 대한 Visual Studio 프로젝트.
* [앱 제출을 만들지](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) 파트너 센터에서 드라이버와이 앱은 스토어에서 게시 합니다. 테스트 하는 동안 스토어에서 검색이 되지 않도록 앱을 구성할 수도 있습니다. 자세한 내용은 [테스트 지침](in-app-purchases-and-trials.md#testing)을 참조하세요.
* 앱에 대 한 추가 기능에 대 한 앱에서 바로 구매를 사용 하도록 설정 하려는 해야 [파트너 센터에서 추가 기능을 생성](../publish/add-on-submissions.md)합니다.

이 예제의 코드에서는 다음과 같이 가정합니다.
* 코드는 ```workingProgressRing```이라는 [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx)과 ```textBlock```이라는 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)을 포함하는 [페이지](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx)의 컨텍스트에서 실행됩니다. 해당 개체를 사용하여 각각 비동기 작업이 발생함을 나타내고 출력 메시지를 표시합니다.
* 코드 파일에는 **Windows.Services.Store** 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md#api_intro)을 참조하세요.

> [!NOTE]
> [데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop)를 사용하는 데스크톱 응용 프로그램이 있는 경우 이 예에서 표시되지 않는 별도의 코드를 추가하여 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 개체를 구성해야 할 수도 있습니다. 자세한 내용은 [데스크톱 브리지를 사용하는 데스크톱 응용 프로그램에서 StoreContext 클래스 사용](in-app-purchases-and-trials.md#desktop)을 참조하세요.

## <a name="code-example"></a>코드 예제

이 예제에서는 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스의 [RequestPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestpurchaseasync) 메서드를 사용하여 알려진 [스토어 ID](in-app-purchases-and-trials.md#store-ids)로 앱 또는 추가 기능을 구매하는 방법을 보여 줍니다. 전체 샘플 응용 프로그램은 [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)을 참조하세요.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnablePurchases](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseAddOnPage.xaml.cs#PurchaseAddOn)]

## <a name="video"></a>비디오

앱에 앱 내 구매를 구현하는 방법에 대한 개요는 다음 비디오를 시청하세요.
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

## <a name="related-topics"></a>관련 항목

* [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)
* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱 및 추가 기능에 대한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [소모성 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)
* [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)
* [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
