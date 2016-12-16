---
author: mcleanbyron
Description: "앱이 무료인지 여부와 상관없이, 앱 내에서 바로 콘텐츠, 기타 앱 또는 새 앱 기능(예: 게임의 다음 단계 잠금 해제)을 판매할 수 있습니다. 여기서는 앱에서 이러한 제품을 사용하도록 설정하는 방법을 보여 줍니다."
title: "앱에서 바로 구매 제품 사용"
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
keywords: "앱에서 바로 판매 코드 샘플"
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: 1cd748cd1b6ca7e85cfb86daba367540af25db88

---

# <a name="enable-in-app-product-purchases"></a>앱에서 바로 구매 제품 사용

>**참고**  이 문서에서는 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스의 멤버를 사용하는 방법을 보여 줍니다. 앱이 Windows 10 버전 1607 이상을 대상으로 하는 경우 **Windows.ApplicationModel.Store** 네임스페이스 대신 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스의 멤버를 사용하여 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함)을 관리하는 것이 좋습니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)을 참조하세요.

앱이 무료인지 여부와 상관없이, 앱 내에서 바로 콘텐츠, 기타 앱 또는 새 앱 기능(예: 게임의 다음 단계 잠금 해제)을 판매할 수 있습니다. 여기서는 앱에서 이러한 제품을 사용하도록 설정하는 방법을 보여 줍니다.

> **참고**  앱의 평가판에서는 앱에서 바로 구매 제품을 제공할 수 없습니다. 앱 평가판을 사용하는 고객은 처음 사용자용 앱 버전을 구매한 경우에만 앱에서 바로 구매 제품을 구입할 수 있습니다.

## <a name="prerequisites"></a>필수 조건

-   고객이 구매하는 기능을 추가할 Windows 앱
-   새로운 앱에서 바로 구매 제품을 처음 코딩하고 테스트할 때는 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) 개체 대신 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) 개체를 사용해야 합니다. 이렇게 하면 라이브 서버를 호출하는 대신 라이선스 서버 호출을 시뮬레이트하여 라이선스 논리를 확인할 수 있습니다. 이렇게 하려면 %userprofile%\\AppData\\local\\packages\\&lt;패키지 이름&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData에 있는 WindowsStoreProxy.xml 파일을 사용자 지정해야 합니다. 처음으로 앱이 실행될 때 Microsoft Visual Studio 시뮬레이터에서 이 파일을 만듭니다. 또는 런타임에 사용자 지정 파일을 로드할 수도 있습니다. 자세한 내용은 [CurrentAppSimulator와 함께 WindowsStoreProxy.xml 파일 사용](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy)을 참조하세요.
-   이 항목에서는 [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)에 제공된 코드 예제도 참조합니다. 이 샘플은 UWP(유니버설 Windows 플랫폼) 앱에 제공된 다양한 수익 창출 옵션을 실습할 수 있는 좋은 방법입니다.

## <a name="step-1-initialize-the-license-info-for-your-app"></a>1단계: 앱의 라이선스 정보 초기화

앱이 초기화되는 중이면 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) 또는 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) 초기화를 통해 앱에 대한 [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157) 개체를 가져와 앱에서 바로 구매 제품 구매를 사용하도록 설정합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#InitializeLicenseTest)]

## <a name="step-2-add-the-in-app-offers-to-your-app"></a>2단계: 앱에서 바로 판매를 앱에 추가

앱에서 바로 구매 제품을 통해 제공하려는 각 기능에 대해 판매를 만들어 앱에 추가합니다.

> **중요**  스토어에 앱을 제출하기 전에 고객에게 제공하려는 앱에서 바로 구매 제품을 앱에 모두 추가해야 합니다. 나중에 새로운 앱에서 바로 구매 제품을 추가하려면 앱을 업데이트하고 새 버전을 다시 제출해야 합니다.

1.  **앱에서 바로 판매 토큰 만들기**

    앱에서는 각 앱에서 바로 구매 제품을 토큰으로 식별합니다. 이 토큰은 사용자가 정의하고 앱과 스토어에서 특정 앱에서 바로 구매 제품을 식별하는 데 사용하는 문자열입니다. 코딩하는 동안 토큰이 나타내는 올바른 기능을 빨리 식별할 수 있도록 앱에서 고유하고 의미 있는 이름을 지정하세요. 다음은 이름의 몇 가지 예입니다.

    -   "SpaceMissionLevel4"

    -   "ContosoCloudSave"

    -   "RainbowThemePack"

2.  **조건부 블록에 기능 코딩**

    고객에게 이 기능을 사용할 수 있는 라이선스가 있는지 테스트하는 조건부 블록에 앱에서 바로 구매 제품과 연결된 각 기능의 코드 줄을 넣어야 합니다.

    다음 예제에서는 라이선스별 조건부 블록에 **featureName**이라는 제품 기능을 코딩하는 방법을 보여 줍니다. **featureName** 문자열은 앱에서 이 제품을 고유하게 식별하고 스토어에서 앱을 식별하는 데도 사용되는 토큰입니다.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#CodeFeature)]

3.  **이 기능의 구매 UI 추가**

    앱에서 바로 구매 제품을 통해 제공되는 제품 또는 기능을 고객이 구매할 방법도 앱에서 제공해야 합니다. 스토어에서 전체 앱을 구매한 것과 동일한 방법으로 구매할 수는 없습니다.

    고객이 이미 앱에서 바로 구매 제품을 소유하고 있는지 테스트하고, 없는 경우 구매할 수 있도록 구매 대화 상자를 표시하는 방법은 다음과 같습니다. "show the purchase dialog" 주석을 구매 대화 상자(예: 친숙한 "앱 구매!" 단추가 있는 페이지)의 사용자 지정 코드로 바꾸세요.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#BuyFeature)]

## <a name="step-3-change-the-test-code-to-the-final-calls"></a>3단계: 테스트 코드를 최종 호출로 변경

이 단계에서는 앱 코드의 모든 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) 참조를 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765)으로 변경합니다. WindowsStoreProxy.xml 파일은 더 이상 제공할 필요가 없으므로 앱 경로에서 제거하세요. 다음 단계에서 앱에서 바로 판매를 구성할 때 참조하기 위해 저장해 둘 수도 있습니다.

## <a name="step-4-configure-the-in-app-product-offer-in-the-store"></a>4단계: 스토어에서 앱에서 바로 제품 판매 구성

개발자 센터 대시보드에서 제품 ID, 유형, 가격 및 앱에서 바로 구매 제품에 대한 기타 속성을 정의합니다. 테스트할 때 WindowsStoreProxy.xml에서 설정한 구성과 동일하게 구성해야 합니다. 자세한 내용은 [IAP 제출](https://msdn.microsoft.com/library/windows/apps/mt148551)을 참조하세요.

## <a name="remarks"></a>설명

고객에게 소모성 앱에서 바로 구매 제품 옵션(구매하고 다 사용한 후 원하는 경우 다시 구매할 수 있는 항목)을 제공하려는 경우 [앱에서 바로 소모성 제품 구매 사용](enable-consumable-in-app-product-purchases.md) 항목을 진행하세요.

확인 메일을 사용하여 사용자가 앱에서 바로 구매했는지 확인해야 할 경우 [확인 메일을 사용하여 제품 구매 검증](use-receipts-to-verify-product-purchases.md)을 검토하세요.

## <a name="related-topics"></a>관련 항목


* [앱에서 바로 소모성 제품 구매 사용](enable-consumable-in-app-product-purchases.md)
* [앱에서 바로 구매 제품의 큰 카탈로그 관리](manage-a-large-catalog-of-in-app-products.md)
* [확인 메일을 사용하여 제품 구매 검증](use-receipts-to-verify-product-purchases.md)
* [스토어 샘플(평가판 및 앱에서 바로 구매 설명)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)



<!--HONumber=Dec16_HO1-->


