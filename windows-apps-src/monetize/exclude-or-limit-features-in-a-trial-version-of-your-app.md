---
author: Xansky
Description: If you enable customers to use your app for free during a trial period, you can entice your customers to upgrade to the full version of your app by excluding or limiting some features during the trial period.
title: 평가판의 기능 제외 또는 제한
ms.assetid: 1B62318F-9EF5-432A-8593-F3E095CA7056
keywords: windows 10, uwp, 평가판, 앱에서 바로 구매, IAP, Windows.ApplicationModel.Store
ms.author: mhopkins
ms.date: 08/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 18d9fb3ba5b0fbd1e964450a75d8e0e417265e7f
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5163876"
---
# <a name="exclude-or-limit-features-in-a-trial-version"></a>평가판의 기능 제외 또는 제한

평가 기간 동안 고객이 앱을 무료로 사용할 수 있게 하는 경우 평가 기간 동안 일부 기능을 제외하거나 제한하여 고객이 앱 정식 버전으로 업그레이드하도록 유도할 수 있습니다. 코딩을 시작하기 전에 제한할 기능을 결정한 다음 정식 라이선스를 구입한 다음에만 해당 기능이 작동하도록 해야 합니다. 또한 고객이 앱을 구매하기 전 체험 기간 동안에만 표시되는 배너 또는 워터마크와 같은 기능을 사용하도록 설정할 수도 있습니다.

> [!IMPORTANT]
> 이 문서에서는 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스의 멤버를 사용하여 평가판 기능을 구현하는 방법을 설명합니다. 이 네임스페이스는 더 이상 새 기능으로 업데이트되지 않으므로 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스를 대신 사용하는 것이 좋습니다. **Windows.Services.Store** 네임스페이스는 Store 관리 소모성 추가 기능 및 구독 등의 최신 추가 기능 유형을 지원하며 Windows 개발자 센터 및 Store에서 지원하는 이후 제품 및 기능 유형과 호환되도록 설계되었습니다. **Windows.Services.Store** 네임스페이스는 Windows 10 버전, 1607에 도입되었으며 **Windows 10 Anniversary Edition(10.0, 빌드 14393)** 또는 Visual Studio의 최신 릴리스를 대상으로 하는 프로젝트에만 사용할 수 있습니다. **Windows.Services.Store** 네임스페이스를 사용하여 평가판 기능을 구현하는 방법에 대한 자세한 내용은 [이 문서](implement-a-trial-version-of-your-app.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

고객이 구매하는 기능을 추가할 Windows 앱

## <a name="step-1-pick-the-features-you-want-to-enable-or-disable-during-the-trial-period"></a>1단계: 체험 기간 중에 사용하거나 사용하지 않도록 설정할 기능 선택

앱의 현재 라이선스 상태는 [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157) 클래스의 속성으로 저장됩니다. 일반적으로 다음 단계의 설명과 같이 라이선스 상태를 사용하는 기능을 조건부 블록에 넣습니다. 이러한 기능을 고려할 때 모든 라이선스 상태에서 작동하도록 구현할 수 있는지 확인하세요.

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

앱에서 라이선스 변경을 감지하고 조치를 취하도록 하려면 다음 단계에서 설명하는 대로 이를 위한 이벤트 처리기를 추가해야 합니다.

## <a name="step-2-initialize-the-license-info"></a>2단계: 라이선스 정보 초기화

앱을 초기화하는 경우 이 예제와 같이 앱의 [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157) 개체를 가져옵니다. **licenseInformation**은 **LicenseInformation** 유형의 전역 변수 또는 필드로 가정됩니다.

지금은 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) 대신 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766)를 사용하여 시뮬레이트된 라이선스 정보를 가져옵니다. 앱의 릴리스 버전을 **스토어**에 제출하기 전에 코드에서 모든 **CurrentAppSimulator** 참조를 **CurrentApp**으로 바꾸어야 합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseTest)]

앱이 실행되는 동안 라이선스가 변경될 경우 알림을 받는 이벤트 처리기를 추가합니다. 예를 들어 체험 기간이 만료되거나 고객이 Microsoft Store를 통해 앱을 구매하면 앱의 라이선스가 변경될 수 있습니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseTestWithEvent)]

## <a name="step-3-code-the-features-in-conditional-blocks"></a>3단계: 조건부 블록에 기능 코딩

라이선스 변경 이벤트가 발생할 경우 앱이 라이선스 API를 호출하여 체험 상태가 변경되었는지 확인해야 합니다. 이 단계의 코드는 이 이벤트의 처리기를 구성하는 방법을 보여 줍니다. 이때 사용자가 앱을 구매한 경우 라이선스 상태가 변경되었다는 피드백을 사용자에게 제공하는 것이 좋습니다. 코딩 방식에 따라 앱을 다시 시작하라는 메시지를 사용자에게 표시해야 할 수도 있습니다. 그러나 이러한 전환이 가능한 한 매끄럽고 불편 없이 진행되도록 하세요.

이 예제에서는 앱의 라이선스 상태를 평가하고, 그에 따라 앱의 기능을 사용하거나 사용하지 않도록 설정할 수 있는 방법을 보여 줍니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#ReloadLicense)]

## <a name="step-4-get-an-apps-trial-expiration-date"></a>4단계: 앱의 체험 만료 날짜 확인

앱의 체험 만료 날짜를 확인하는 코드를 포함합니다.

이 예의 코드는 앱 체험 라이선스의 만료 날짜를 가져오는 함수를 정의합니다. 라이선스가 유효하면 만료 날짜가 체험 만료 시까지 남은 일수와 함께 표시됩니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#DisplayTrialVersionExpirationTime)]

## <a name="step-5-test-the-features-using-simulated-calls-to-the-license-api"></a>5단계: 라이선스 API 호출을 시뮬레이트하여 기능 테스트

이제 시뮬레이트된 데이터를 사용하여 앱을 테스트합니다. **CurrentAppSimulator**는 %UserProfile%\\AppData\\local\\packages\\&lt;패키지 이름&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData에 있는 WindowsStoreProxy.xml이라는 XML 파일에서 테스트 관련 라이선스 정보를 가져옵니다. WindowsStoreProxy.xml을 편집하여 앱과 해당 기능에 대해 시뮬레이트된 만료 날짜를 변경할 수 있습니다. 가능한 만료 및 라이선스 구성을 모두 테스트하여 모든 것이 예상대로 작동하는지 확인하세요. 자세한 내용은 [CurrentAppSimulator와 함께 WindowsStoreProxy.xml 파일 사용](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy)을 참조하세요.

이 경로와 파일이 없는 경우 설치 중에 또는 런타임에 만들어야 합니다. 이 특정 위치에 WindowsStoreProxy.xml이 없는 상태에서 [CurrentAppSimulator.LicenseInformation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.licenseinformation) 속성에 액세스하면 오류가 발생합니다.

## <a name="step-6-replace-the-simulated-license-api-methods-with-the-actual-api"></a>6단계: 시뮬레이트된 라이선스 API 메서드를 실제 API로 바꾸기

시뮬레이트된 라이선스 서버로 앱을 테스트한 후, 인증을 위해 앱을 스토어로 제출하기 전에 다음 코드 샘플과 같이 **CurrentAppSimulator**를 **CurrentApp**으로 바꾸세요.

> [!IMPORTANT]
> 앱을 Microsoft Store에 제출할 때 앱에서 **CurrentApp** 개체를 사용해야 합니다. 그러지 않으면 앱 인증에 실패합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseRetailWithEvent)]

## <a name="step-7-describe-how-the-free-trial-works-to-your-customers"></a>7단계: 고객에게 무료 평가판이 작동하는 방식 설명

고객이 앱의 동작에 놀라지 않도록 무료 체험 기간 동안 및 이후에 앱이 어떻게 동작하는지 고객에게 설명해야 합니다.

앱 설명 지정에 대한 자세한 내용은 [앱 설명 작성](https://msdn.microsoft.com/library/windows/apps/mt148529)을 참조하세요.

## <a name="related-topics"></a>관련 항목

* [스토어 샘플(평가판 및 앱에서 바로 구매 설명)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [앱 가격 책정 및 가용성 설정](https://msdn.microsoft.com/library/windows/apps/mt148548)
* [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765)
* [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766)
 

 
