---
author: mcleanbyron
ms.assetid: 571697B7-6064-4C50-9A68-1374F2C3F931
description: "Windows.Services.Store 네임스페이스를 사용하여 앱의 평가판을 구현하는 방법을 알아봅니다."
title: "앱의 평가판 구현"
keywords: "무료 체험 코드 샘플"
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: ea4c5637a970a63938da2b1bea9f11fd39de9cc8

---

# <a name="implement-a-trial-version-of-your-app"></a>앱의 평가판 구현

평가 기간에 고객이 앱을 무료로 사용할 수 있도록 [Windows 개발자 센터 대시보드에서 무료 평가판](../publish/set-app-pricing-and-availability.md#free-trial)으로 앱을 구성하면 평가 기간 동안 일부 기능을 제외하거나 제한하여 고객이 앱 정식 버전으로 업그레이드하도록 유도할 수 있습니다. 코딩을 시작하기 전에 제한할 기능을 결정한 다음 정식 라이선스를 구입한 다음에만 해당 기능이 작동하도록 해야 합니다. 또한 고객이 앱을 구매하기 전 체험 기간 동안에만 표시되는 배너 또는 워터마크와 같은 기능을 사용하도록 설정할 수도 있습니다.

Windows&nbsp;10 버전 1607 이상을 대상으로 하는 앱은 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스에서 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스의 멤버를 사용하여 사용자에게 앱의 평가판 라이선스가 있는지 확인하고 앱이 실행되는 동안 라이선스 상태가 변경되면 알림을 받을 수 있습니다.

>**참고** &nbsp;&nbsp;이 문서는 Windows&nbsp;10 버전 1607 이상을 대상으로 하는 앱에 적용할 수 있습니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 **Windows.Services.Store** 네임스페이스 대신 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스를 사용해야 합니다. 자세한 내용은 [Windows.ApplicationModel.Store 네임스페이스를 사용하는 앱에서 바로 구매 및 평가판](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)을 참조하세요.

## <a name="guidelines-for-implementing-a-trial-version"></a>평가판 구현 지침

앱의 현재 라이선스 상태는 [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) 클래스의 속성으로 저장됩니다. 일반적으로 다음 단계의 설명과 같이 라이선스 상태를 사용하는 기능을 조건부 블록에 넣습니다. 이러한 기능을 고려할 때 모든 라이선스 상태에서 작동하도록 구현할 수 있는지 확인하세요.

또한 앱이 실행되는 동안 앱의 라이선스 변경을 처리하는 방법을 결정하세요. 체험 앱에서 전체 기능을 제공할 수 있지만 구매한 버전에는 없는 앱에서 바로 구매 광고 배너가 있습니다. 또는 체험 앱에서 특정 기능을 사용하지 않도록 설정하거나 사용자에게 구매를 권유하는 메시지를 주기적으로 표시할 수 있습니다.

만들려는 앱의 유형과 적합한 체험 또는 만료 전략을 생각해 보세요. 게임 체험 버전의 경우 사용자가 플레이할 수 있는 게임 콘텐츠 양을 제한하는 것이 좋습니다. 유틸리티 체험 버전의 경우 만료 날짜를 설정하거나 잠재 구매자가 사용할 수 있는 기능을 제한할 수 있습니다.

게임이 아닌 대부분의 앱은 사용자가 전체 앱에 대해 잘 알 수 있으므로 만료 날짜 설정이 잘 작동합니다. 다음은 일반적인 만료 시나리오 및 이를 처리하는 옵션입니다.

-   **앱이 실행 중일 때 체험 라이선스 만료**

    앱이 실행 중일 때 체험이 만료되는 경우 앱은 다음을 할 수 있습니다.

    -   아무것도 하지 않습니다.
    -   고객에게 메시지를 표시합니다.
    -   닫힙니다.
    -   고객에게 앱을 구입할지 묻는 메시지를 표시합니다.

    앱을 구매할지 묻는 메시지를 표시하는 것이 좋습니다. 고객이 앱을 구매하면 계속해서 모든 기능을 사용할 수 있습니다. 사용자가 앱을 구매하지 않는 경우 앱을 닫거나 정기적으로 앱을 구매하도록 알립니다.

-   **앱을 실행하기 전에 체험 라이선스 만료**

    사용자가 앱을 실행하기 전에 체험이 만료되는 경우 앱은 실행되지 않습니다. 대신 스토어에서 앱을 구입할 수 있는 옵션을 제공하는 대화 상자가 표시됩니다.

-   **앱이 실행 중일 때 고객이 앱 구입**

    앱이 실행 중일 때 고객이 앱을 구입하는 경우 앱이 수행할 수 있는 몇 가지 작업은 다음과 같습니다.

    -   아무것도 하지 않고 고객이 앱을 다시 시작할 때까지 계속 체험 모드로 실행됩니다.
    -   구매에 대한 감사 인사를 표시하거나 메시지를 표시합니다.
    -   정식 라이선스에서 사용 가능한 기능을 자동으로 사용하도록 설정하거나 체험 버전 전용 알림을 사용하지 않도록 설정합니다.

고객이 앱의 동작에 놀라지 않도록 무료 체험 기간 동안 및 이후에 앱이 어떻게 동작하는지 고객에게 설명해야 합니다. 앱 설명 지정에 대한 자세한 내용은 [앱 설명 작성](https://msdn.microsoft.com/library/windows/apps/mt148529)을 참조하세요.

## <a name="prerequisites"></a>필수 조건

이 예제의 필수 조건은 다음과 같습니다.
* Windows&nbsp;10 버전 1607 이상을 대상으로 하는 UWP(유니버설 Windows 플랫폼) 앱에 대한 Visual Studio 프로젝트.
* 시간 제한이 없는 [무료 평가판](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability)으로 구성된 Windows 개발자 센터 대시보드에서 앱을 만들었으며, 이 앱은 스토어에서 게시되고 사용할 수 있습니다. 고객에게 릴리스하려는 앱일 수도 있고, 테스트용으로만 사용 중인 최소 [Windows 앱 인증 키트](https://developer.microsoft.com/windows/develop/app-certification-kit) 요구 사항을 충족하는 기본 앱일 수도 있습니다. 자세한 내용은 [테스트 지침](in-app-purchases-and-trials.md#testing)을 참조하세요.

이 예제의 코드에서는 다음과 같이 가정합니다.
* 코드는 ```workingProgressRing```이라는 [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx)과 ```textBlock```이라는 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)을 포함하는 [페이지](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx)의 컨텍스트에서 실행됩니다. 해당 개체를 사용하여 각각 비동기 작업이 발생함을 나타내고 출력 메시지를 표시합니다.
* 코드 파일에는 **Windows.Services.Store** 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md#api_intro)을 참조하세요.

>**참고**&nbsp;&nbsp;[데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop)를 사용하는 데스크톱 응용 프로그램이 있는 경우 이 예에서 표시되지 않는 별도의 코드를 추가하여 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 개체를 구성해야 할 수도 있습니다. 자세한 내용은 [데스크톱 브리지를 사용하는 데스크톱 응용 프로그램에서 StoreContext 클래스 사용](in-app-purchases-and-trials.md#desktop)을 참조하세요.

## <a name="code-example"></a>코드 예제

앱을 초기화하는 경우 앱의 [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) 개체를 가져와서 [OfflineLicensesChanged](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.offlinelicenseschanged.aspx) 이벤트를 처리하여 앱을 실행하는 동안 라이선스가 변경되면 알림을 받습니다. 예를 들어 체험 기간이 만료되거나 고객이 스토어를 통해 앱을 구매하면 앱의 라이선스가 변경될 수 있습니다. 라이선스가 변경될 경우 새 라이선스를 가져와서 앱의 기능을 사용 또는 사용 안 함으로 적절하게 설정합니다.

이때 사용자가 앱을 구매한 경우 라이선스 상태가 변경되었다는 피드백을 사용자에게 제공하는 것이 좋습니다. 코딩 방식에 따라 앱을 다시 시작하라는 메시지를 사용자에게 표시해야 할 수도 있습니다. 그러나 이러한 전환이 가능한 한 매끄럽고 불편 없이 진행되도록 하세요.

> [!div class="tabbedCodeSnippets"]
[!code-cs[ImplementTrial](./code/InAppPurchasesAndLicenses_RS1/cs/ImplementTrialPage.xaml.cs#ImplementTrial)]

전체 샘플 응용 프로그램은 [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)을 참조하세요.

## <a name="related-topics"></a>관련 항목

* [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)
* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱 및 추가 기능에 대한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [앱에서 바로 앱 및 추가 기능 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md)
* [소모성 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)
* [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Dec16_HO1-->


