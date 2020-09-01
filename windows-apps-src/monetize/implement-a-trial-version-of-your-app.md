---
ms.assetid: 571697B7-6064-4C50-9A68-1374F2C3F931
description: Windows. Store 네임 스페이스를 사용 하 여 앱의 평가판을 구현 하는 방법을 알아봅니다.
title: 앱의 평가판 구현
keywords: windows 10, uwp, 평가판, 앱에서 바로 구매, Windows. 서비스 스토어
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8cac33f36e66c1a5f22fc246daab192298e9f876
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167557"
---
# <a name="implement-a-trial-version-of-your-app"></a>앱의 평가판 구현

고객이 평가판 기간 동안 앱을 무료로 사용할 수 있도록 [파트너 센터에서 무료 평가판으로 앱을 구성](../publish/set-app-pricing-and-availability.md#free-trial) 하는 경우, 평가판 기간 동안 일부 기능을 제외 하거나 제한 하 여 고객이 앱의 전체 버전으로 업그레이드 하도록 유도할 수 있습니다. 코딩을 시작 하기 전에 제한 되어야 하는 기능을 결정 한 다음, 전체 라이선스를 구매한 경우에만 앱이 작동 하도록 허용 해야 합니다. 고객이 앱을 구입 하기 전에 평가판 동안에만 표시 되는 배너 또는 워터 마크와 같은 기능을 사용 하도록 설정할 수도 있습니다.

이 문서에서는 [Windows. Store](/uwp/api/windows.services.store) 네임 스페이스에서 [storecontext](/uwp/api/windows.services.store.storecontext) 클래스의 멤버를 사용 하 여 사용자에 게 앱에 대 한 평가판 라이선스가 있는지 확인 하 고 앱이 실행 되는 동안 라이선스의 상태가 변경 되 면이를 알리도록 하는 방법을 보여 줍니다. 

> [!NOTE]
> Windows 10, 버전 1607에 **는 windows 10** **기념일 10.0 버전을 대상으로 하는 프로젝트에만 사용 될 수 있습니다. 빌드 14393)** 또는 Visual Studio의 이후 릴리스. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우에는 windows. **store** 네임 스페이스 대신 [windows](/uwp/api/windows.applicationmodel.store) 를 사용 해야 합니다. 자세한 내용은 [이 문서](exclude-or-limit-features-in-a-trial-version-of-your-app.md)를 참조하세요.

## <a name="guidelines-for-implementing-a-trial-version"></a>평가판을 구현 하기 위한 지침

앱의 현재 라이선스 상태는 [StoreAppLicense](/uwp/api/windows.services.store.storeapplicense) 클래스의 속성으로 저장 됩니다. 일반적으로 다음 단계에서 설명 하는 것 처럼 조건부 블록의 라이선스 상태에 종속 된 함수를 배치 합니다. 이러한 기능을 고려 하는 경우 모든 라이선스 상태에서 작동 하는 방식으로 구현할 수 있는지 확인 합니다.

또한 앱이 실행 되는 동안 앱 라이선스에 대 한 변경 내용을 처리 하는 방법을 결정 합니다. 평가판 앱은 전체 기능을 사용할 수 있지만 유료 버전의 앱 내 ad 배너를 사용할 수 있습니다. 또는 평가판 앱에서 특정 기능을 사용 하지 않도록 설정 하거나 사용자에 게 구입 하 라는 일반 메시지를 표시할 수 있습니다.

앱의 유형과 적절 한 평가판 또는 만료 전략이 무엇 인지 생각해 보겠습니다. 게임 평가판의 경우 사용자가 재생할 수 있는 게임 콘텐츠의 양을 제한 하는 것이 좋습니다. 유틸리티의 평가판을 사용 하는 경우 만료 날짜를 설정 하거나 잠재적 구매자가 사용할 수 있는 기능을 제한할 수 있습니다.

사용자가 전체 앱을 잘 이해할 수 있으므로, 대부분의 비 게임 앱에 대해 만료 날짜를 설정 하는 것이 잘 작동 합니다. 다음은 몇 가지 일반적인 만료 시나리오와 이러한 시나리오를 처리 하는 옵션입니다.

-   **앱을 실행 하는 동안 평가판 라이선스가 만료 됩니다.**

    앱이 실행 되는 동안 평가판이 만료 되 면 앱에서 다음 작업을 수행할 수 있습니다.

    -   아무 작업도 하지 않습니다.
    -   고객에 게 메시지를 표시 합니다.
    -   거의 정확합니다.
    -   고객에 게 앱을 구입 하 라는 메시지를 표시 합니다.

    앱을 구매할지 묻는 메시지를 표시하는 것이 좋습니다. 고객이 앱을 구매하면 계속해서 모든 기능을 사용할 수 있습니다. 사용자가 앱을 구입 하지 않기로 결정 한 경우 앱을 닫거나 정기적으로 앱을 구입 하도록 알려 주십시오.

-   **평가판 라이선스는 앱이 시작 되기 전에 만료 됩니다.**

    사용자가 앱을 시작 하기 전에 평가판이 만료 되 면 앱이 시작 되지 않습니다. 대신 사용자가 스토어에서 앱을 구매할 수 있는 옵션을 제공 하는 대화 상자가 표시 됩니다.

-   **고객이 실행 중인 앱을 구입 합니다.**

    고객이 실행 중인 앱을 구입 하는 경우 앱에서 수행할 수 있는 몇 가지 작업은 다음과 같습니다.

    -   아무것도 하지 말고 앱을 다시 시작할 때까지 평가판 모드에서 계속 진행 합니다.
    -   구매 하거나 메시지를 표시 해 주셔서 감사 합니다.
    -   전체 라이선스에서 사용할 수 있는 기능을 자동으로 사용 하도록 설정 (또는 평가판 전용 알림을 사용 하지 않도록 설정) 합니다.

무료 평가 기간 동안 및 후에 앱이 어떻게 동작 하는지 설명 해야 합니다. 그러면 고객이 앱의 동작에 영향을 주지 않습니다. 앱을 설명 하는 방법에 대 한 자세한 내용은 [앱 설명 만들기](../publish/create-app-store-listings.md)를 참조 하세요.

## <a name="prerequisites"></a>필수 구성 요소

이 예제에는 다음과 같은 필수 구성 요소가 있습니다.
* **Windows 10 기념일 Edition (10.0;)을 대상으로 하는 UWP (유니버설 Windows 플랫폼) 앱 용 Visual Studio 프로젝트입니다. 빌드 14393)** 이상 릴리스
* 파트너 센터에서 시간 제한이 없는 [무료 평가판](../publish/set-app-pricing-and-availability.md) 으로 구성 되 고이 앱이 스토어에 게시 된 앱을 만들었습니다. 응용 프로그램을 테스트 하는 동안 저장소에서 검색할 수 없도록 앱을 선택적으로 구성할 수 있습니다. 자세한 내용은 [테스트 지침](in-app-purchases-and-trials.md#testing)을 참조 하세요.

이 예제의 코드는 다음을 가정 합니다.
* 이 코드는 이름이 인 [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) TextBlock이 포함 된 [페이지](/uwp/api/windows.ui.xaml.controls.page) 의 컨텍스트에서 실행 됩니다 ```workingProgressRing``` [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) ```textBlock``` . 이러한 개체를 사용 하 여 비동기 작업이 진행 중임을 나타내고 출력 메시지를 각각 표시 합니다.
* 코드 파일은 **Windows. Store** 네임 스페이스에 대 한 **using** 문을 포함 합니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md#api_intro)을 참조 하세요.

> [!NOTE]
> 데스크톱 [브리지](https://developer.microsoft.com/windows/bridges/desktop)를 사용 하는 데스크톱 응용 프로그램이 있는 경우이 예제에 표시 되지 않은 코드를 추가 하 여 [storecontext](/uwp/api/windows.services.store.storecontext) 개체를 구성 해야 할 수 있습니다. 자세한 내용은 [데스크톱 브리지를 사용 하는 데스크톱 응용 프로그램에서 StoreContext 클래스 사용](in-app-purchases-and-trials.md#desktop)을 참조 하세요.

## <a name="code-example"></a>코드 예제

앱을 초기화 하는 경우 앱에 대 한 [StoreAppLicense](/uwp/api/windows.services.store.storeapplicense) 개체를 가져오고 [OfflineLicensesChanged](/uwp/api/windows.services.store.storecontext.offlinelicenseschanged) 이벤트를 처리 하 여 앱이 실행 되는 동안 라이선스가 변경 될 때 알림을 수신 합니다. 예를 들어 평가 기간이 만료 되거나 고객이 스토어를 통해 앱을 구입 하는 경우 앱의 라이선스가 변경 될 수 있습니다. 라이선스가 변경 되 면 새 라이선스를 확보 하 고 앱의 기능을 적절 하 게 사용 하거나 사용 하지 않도록 설정 합니다.

이 시점에서 사용자가 앱을 구매한 경우 라이선스 상태가 변경 되었음을 사용자에 게 피드백을 제공 하는 것이 좋습니다. 앱이 코딩 된 방법 인 경우 사용자에 게 앱을 다시 시작 하도록 요청 해야 할 수도 있습니다. 하지만 가능한 한 원활 하 고 간편 하 게 전환 합니다.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[ImplementTrial](./code/InAppPurchasesAndLicenses_RS1/cs/ImplementTrialPage.xaml.cs#ImplementTrial)]

전체 샘플 응용 프로그램은 [Store 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)을 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)
* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱 및 추가 기능에 대한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [앱에서 바로 앱 및 추가 기능 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md)
* [소모성 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)
* [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)