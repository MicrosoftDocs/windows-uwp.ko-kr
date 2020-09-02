---
ms.assetid: B356C442-998F-4B2C-B550-70070C5E4487
description: Windows. Store 네임 스페이스를 사용 하 여 앱 또는 추가 기능 중 하나를 구매 하는 방법을 알아봅니다.
title: 앱에서 바로 앱 및 추가 기능 구매 사용
keywords: windows 10, uwp, 추가 기능, 앱 내 구매, IAPs, Windows. Service 스토어
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3064a651715329a3602c0c3539394d2ce72f90a7
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362816"
---
# <a name="enable-in-app-purchases-of-apps-and-add-ons"></a>앱에서 바로 앱 및 추가 기능 구매 사용

이 문서에서는 [Windows. Store](/uwp/api/windows.services.store) 네임 스페이스의 멤버를 사용 하 여 현재 앱 또는 사용자에 대 한 추가 기능 중 하나를 구매 하도록 요청 하는 방법을 보여 줍니다. 예를 들어 사용자가 현재 앱의 평가판을 사용 하는 경우이 프로세스를 사용 하 여 사용자에 대 한 정식 라이선스를 구매할 수 있습니다. 또는이 프로세스를 사용 하 여 추가 기능 (예: 사용자에 대 한 새 게임 수준)을 구입할 수 있습니다.

앱 또는 추가 기능 구매를 요청 하기 위해 [Windows. Store](/uwp/api/windows.services.store) 네임 스페이스는 다양 한 메서드를 제공 합니다.
* 앱 또는 추가 기능에 대 한 [저장소 ID](in-app-purchases-and-trials.md#store_ids) 를 알고 있는 경우 [Storecontext](/uwp/api/windows.services.store.storecontext) 클래스의 [RequestPurchaseAsync](/uwp/api/windows.services.store.storecontext.requestpurchaseasync) 메서드를 사용할 수 있습니다.
* 앱 또는 추가 기능을 나타내는 [StoreSku **product**, **StoreSku**또는 RequestPurchaseAsync **availability** 개체가](in-app-purchases-and-trials.md#products-skus) 이미 있는 경우 이러한 개체의 **RequestPurchaseAsync** 메서드를 사용할 수 있습니다. 코드에서 사용자 **제품** 을 검색 하는 다양 한 방법의 예제는 [앱 및 추가 기능에 대 한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)를 참조 하세요.

각 메서드는 사용자에 게 표준 구매 UI를 제공 하 고 트랜잭션이 완료 된 후에는 비동기적으로 완료 됩니다. 메서드는 트랜잭션이 성공 했는지 여부를 나타내는 개체를 반환 합니다.

> [!NOTE]
> Windows 10, 버전 1607에 **는 windows 10** **기념일 10.0 버전을 대상으로 하는 프로젝트에만 사용 될 수 있습니다. 빌드 14393)** 또는 Visual Studio의 이후 릴리스. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우에는 windows. **store** 네임 스페이스 대신 [windows](/uwp/api/windows.applicationmodel.store) 를 사용 해야 합니다. 자세한 내용은 [이 문서](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)를 참조하세요.

## <a name="prerequisites"></a>사전 요구 사항

이 예제에는 다음과 같은 필수 구성 요소가 있습니다.
* **Windows 10 기념일 Edition (10.0;)을 대상으로 하는 UWP (유니버설 Windows 플랫폼) 앱 용 Visual Studio 프로젝트입니다. 빌드 14393)** 이상 릴리스
* 파트너 센터에서 [앱 제출을 만들었으며](../publish/app-submissions.md) 이 앱은 스토어에 게시 됩니다. 응용 프로그램을 테스트 하는 동안 저장소에서 검색할 수 없도록 앱을 선택적으로 구성할 수 있습니다. 자세한 내용은 [테스트 지침](in-app-purchases-and-trials.md#testing)을 참조 하세요.
* 앱에 대 한 추가 기능에 대 한 앱에서 바로 구매를 사용 하도록 설정 하려면 [파트너 센터에서 추가 기능을 만들어야](../publish/add-on-submissions.md)합니다.

이 예제의 코드는 다음을 가정 합니다.
* 이 코드는 이름이 인 [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) TextBlock이 포함 된 [페이지](/uwp/api/windows.ui.xaml.controls.page) 의 컨텍스트에서 실행 됩니다 ```workingProgressRing``` [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) ```textBlock``` . 이러한 개체를 사용 하 여 비동기 작업이 진행 중임을 나타내고 출력 메시지를 각각 표시 합니다.
* 코드 파일은 **Windows. Store** 네임 스페이스에 대 한 **using** 문을 포함 합니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md#api_intro)을 참조 하세요.

> [!NOTE]
> 데스크톱 [브리지](https://developer.microsoft.com/windows/bridges/desktop)를 사용 하는 데스크톱 응용 프로그램이 있는 경우이 예제에 표시 되지 않은 코드를 추가 하 여 [storecontext](/uwp/api/windows.services.store.storecontext) 개체를 구성 해야 할 수 있습니다. 자세한 내용은 [데스크톱 브리지를 사용 하는 데스크톱 응용 프로그램에서 StoreContext 클래스 사용](in-app-purchases-and-trials.md#desktop)을 참조 하세요.

## <a name="code-example"></a>코드 예제

이 예제에서는 [Storecontext](/uwp/api/windows.services.store.storecontext) 클래스의 [RequestPurchaseAsync](/uwp/api/windows.services.store.storecontext.requestpurchaseasync) 메서드를 사용 하 여 알려진 [저장소 ID](in-app-purchases-and-trials.md#store-ids)를 사용 하 여 앱 또는 추가 기능을 구입 하는 방법을 보여 줍니다. 전체 샘플 응용 프로그램은 [Store 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)을 참조 하세요.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/PurchaseAddOnPage.xaml.cs" id="PurchaseAddOn":::

## <a name="video"></a>동영상

앱에서 앱에서 바로 구매 기능을 구현 하는 방법에 대 한 개요는 다음 비디오를 시청 하세요.
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
