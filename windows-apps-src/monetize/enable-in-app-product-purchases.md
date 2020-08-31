---
Description: '앱이 무료로 제공 되는지 여부에 관계 없이 앱 내에서 콘텐츠, 기타 앱 또는 새로운 앱 기능 (예: 게임의 다음 수준 잠금 해제)을 바로 판매할 수 있습니다. 앱에서 이러한 제품을 사용 하도록 설정 하는 방법을 보여 줍니다.'
title: 앱에서 바로 구매 제품 사용
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
keywords: uwp, 추가 기능, 앱 내 구매, IAPs, Windows. ApplicationModel 스토어
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ab2b5937746f006c0f5efd296e9b4a4f3cb82696
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171577"
---
# <a name="enable-in-app-product-purchases"></a>앱에서 바로 구매 제품 사용

앱이 무료로 제공 되는지 여부에 관계 없이 앱 내에서 콘텐츠, 기타 앱 또는 새로운 앱 기능 (예: 게임의 다음 수준 잠금 해제)을 바로 판매할 수 있습니다. 앱에서 이러한 제품을 사용 하도록 설정 하는 방법을 보여 줍니다.

> [!IMPORTANT]
> 이 문서에서는 Windows 앱 내 제품 구매를 사용 하도록 설정 하기 위해 Windows. r e s [. Store](/uwp/api/windows.applicationmodel.store) 네임 스페이스의 멤버를 사용 하는 방법을 보여 줍니다. 이 네임 스페이스는 더 이상 새 기능으로 업데이트 되지 않으므로 대신 [Windows. Store](/uwp/api/windows.services.store) 네임 스페이스를 사용 하는 것이 좋습니다. **Windows. store** 네임 스페이스는 스토어 관리 사용 가능 추가 기능 및 구독과 같은 최신 추가 기능을 지원 하며, 파트너 센터 및 스토어에서 지원 되는 이후 유형의 제품과 기능과 호환 되도록 설계 되었습니다. Windows 10, 버전 1607에 **는 windows 10** **기념일 10.0 버전을 대상으로 하는 프로젝트에만 사용 될 수 있습니다. 빌드 14393)** 또는 Visual Studio의 이후 릴리스. **Windows. Store** 네임 스페이스를 사용 하 여 앱 내 제품 구매를 사용 하도록 설정 하는 방법에 대 한 자세한 내용은 [이 문서](enable-in-app-purchases-of-apps-and-add-ons.md)를 참조 하세요.

> [!NOTE]
> 앱 내 제품은 평가판 앱에서 제공할 수 없습니다. 앱의 평가판을 사용 하는 고객은 앱의 전체 버전을 구입 하는 경우 앱 내 제품만 구매할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

-   고객이 구매할 기능을 추가할 수 있는 Windows 앱입니다.
-   새 앱 내 제품을 처음으로 코딩 하 고 테스트 하는 경우 [Currentapp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 개체 대신 [currentappsimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 개체를 사용 해야 합니다. 이러한 방식으로 라이브 서버를 호출 하는 대신 라이선스 서버에 대 한 시뮬레이션 된 호출을 사용 하 여 라이선스 논리를 확인할 수 있습니다. 이렇게 하려면% userprofile% \\ AppData \\ 로컬 \\ 패키지 \\ &lt; 패키지 이름 &gt; \\ localstate \\ Microsoft \\ Windows 스토어 \\ apidata에서 이름이 WindowsStoreProxy.xml 인 파일을 사용자 지정 해야 합니다. Microsoft Visual Studio 시뮬레이터는 앱을 처음 실행할 때이 파일을 만들거나 런타임에 사용자 지정 항목을 로드할 수도 있습니다. 자세한 내용은 [CurrentAppSimulator에서 WindowsStoreProxy.xml 파일 사용](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy)을 참조 하세요.
-   또한이 항목에서는 [Store 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)에 제공 된 코드 예제를 참조 합니다. 이 샘플은 유니버설 Windows 플랫폼 (UWP) 앱에 제공 되는 다양 한 수익 화 옵션을 사용 하 여 실습 환경을 얻는 좋은 방법입니다.

## <a name="step-1-initialize-the-license-info-for-your-app"></a>1 단계: 앱에 대 한 라이선스 정보 초기화

앱을 초기화 하는 경우 앱 내 제품을 구매할 수 있도록 [currentapp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 또는 [currentapp시뮬레이터](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 를 초기화 하 여 앱에 대 한 [LicenseInformation](/uwp/api/Windows.ApplicationModel.Store.LicenseInformation) 개체를 가져옵니다.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#InitializeLicenseTest)]

## <a name="step-2-add-the-in-app-offers-to-your-app"></a>2 단계: 앱에 앱 내 제품 추가

앱 내 제품을 통해 사용할 수 있도록 설정할 각 기능에 대해 제품을 만들고 앱에 추가 합니다.

> [!IMPORTANT]
> 스토어에 제출 하기 전에 고객에 게 제공할 앱 내 모든 제품을 앱에 추가 해야 합니다. 나중에 새 앱 내 제품을 추가 하려면 앱을 업데이트 하 고 새 버전을 다시 제출 해야 합니다.

1.  **앱 내 제품 토큰 만들기**

    토큰으로 앱에서 각 앱 내 제품을 식별 합니다. 이 토큰은 앱 및 스토어에서 정의 하 고 사용 하 여 특정 앱 내 제품을 식별 하는 문자열입니다. 코딩 하는 동안 표시 되는 올바른 기능을 신속 하 게 식별할 수 있도록 고유한 (앱) 및 의미 있는 이름을 제공 합니다. 다음은 이름에 대 한 몇 가지 예입니다.

    * "SpaceMissionLevel4"
    * "ContosoCloudSave"
    * "RainbowThemePack"

  > [!NOTE]
  > 코드에서 사용 하는 앱 내 제공 토큰은 [파트너 센터에서 앱에 대 한 해당 추가 기능을 정의할](../publish/add-on-submissions.md)때 지정 하는 [제품 ID](../publish/set-your-add-on-product-id.md#product-id) 값과 일치 해야 합니다.

2.  **조건부 블록의 기능 코딩**

    고객이 해당 기능을 사용할 수 있는 라이선스를 보유 하 고 있는지 테스트 하는 조건부 블록에서 앱 내 제품과 연결 된 각 기능에 대 한 코드를 입력 해야 합니다.

    라이선스 관련 조건부 블록에서 **featureName** 라는 제품 기능을 코딩 하는 방법을 보여 주는 예제는 다음과 같습니다. **FeatureName**문자열은 앱 내에서이 제품을 고유 하 게 식별 하는 토큰 이며 스토어에서 식별 하는 데에도 사용 됩니다.

    > [!div class="tabbedCodeSnippets"]
    [!code-csharp[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#CodeFeature)]

3.  **이 기능에 대 한 구매 UI 추가**

    앱은 고객이 앱 내 제품에서 제공 하는 제품이 나 기능을 구매할 수 있는 방법도 제공 해야 합니다. 전체 앱을 구매한 것과 같은 방식으로 스토어를 통해 구매할 수 없습니다.

    고객이 앱 내 제품을 이미 소유 하 고 있는지 확인 하기 위해 테스트 하는 방법은 다음과 같습니다. 고객이 구매할 수 있도록 구매 대화 상자를 표시 합니다. "구매 대화 상자 표시" 의견을 구매 대화 상자에 대 한 사용자 지정 코드로 바꿉니다 (예: "이 앱 구매"가 표시 된 페이지). 단추)를 클릭 합니다.

    > [!div class="tabbedCodeSnippets"]
    [!code-csharp[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#BuyFeature)]

## <a name="step-3-change-the-test-code-to-the-final-calls"></a>3 단계: 테스트 코드를 최종 호출로 변경

이는 간단한 단계입니다. 앱 코드에서 [Currentappsimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 에 대 한 모든 참조를 [currentapp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 로 변경 합니다. WindowsStoreProxy.xml 파일을 더 이상 제공 하지 않아도 되므로 앱의 경로에서 제거 해야 합니다 (다음 단계에서 앱 내 제품을 구성 하는 경우 참조를 위해 저장 해야 할 수 있음).

## <a name="step-4-configure-the-in-app-product-offer-in-the-store"></a>4 단계: 스토어에서 앱 내 제품 제품 구성

파트너 센터에서 앱으로 이동 하 여 앱 내 제품 제품과 일치 하는 [추가 기능을 만듭니다](../publish/add-on-submissions.md) . 추가 기능에 대 한 제품 ID, 유형, 가격 및 기타 속성을 정의 합니다. 테스트할 때 WindowsStoreProxy.xml에 설정 하는 구성에 동일 하 게 구성 해야 합니다.

  > [!NOTE]
  > 코드에서 사용 하는 앱 내 제공 토큰은 파트너 센터의 해당 추가 기능에 대해 지정한 [제품 ID](../publish/set-your-add-on-product-id.md#product-id) 값과 일치 해야 합니다.

## <a name="remarks"></a>설명

고객에 게 앱 내 제품 옵션 (구매 하 고, 사용 하 고, 원하는 경우 다시 구매할 수 있는 항목)을 제공 하는 데 관심이 있으면 [앱 내 제품 구매 사용](enable-consumable-in-app-product-purchases.md) 항목으로 이동 하세요.

수신 확인을 사용 하 여 사용자가 앱 내 구매를 확인 해야 하는 경우 확인 [메일을 검토 하 여 제품 구매를 확인](use-receipts-to-verify-product-purchases.md)해야 합니다.

## <a name="related-topics"></a>관련 항목


* [앱에서 바로 소모성 제품 구매 사용](enable-consumable-in-app-product-purchases.md)
* [앱에서 바로 구매 제품의 큰 카탈로그 관리](manage-a-large-catalog-of-in-app-products.md)
* [확인 메일을 사용하여 제품 구매 검증](use-receipts-to-verify-product-purchases.md)
* [스토어 샘플 (평가판 및 앱 내 구매 설명)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)