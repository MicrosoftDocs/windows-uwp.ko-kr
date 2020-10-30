---
description: 평가판 기간 동안 고객이 앱을 무료로 사용할 수 있도록 하는 경우 평가판 기간 중 일부 기능을 제외 하거나 제한 하 여 고객이 앱의 전체 버전으로 업그레이드 하도록 유도할 수 있습니다.
title: 평가판의 기능 제외 또는 제한
ms.assetid: 1B62318F-9EF5-432A-8593-F3E095CA7056
keywords: windows 10, uwp, 평가판, 앱 내 구매, IAP, Windows. ApplicationModel 스토어
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 701769386f9637574cfb7f38f7458a1434796730
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033976"
---
# <a name="exclude-or-limit-features-in-a-trial-version"></a>평가판의 기능 제외 또는 제한

평가판 기간 동안 고객이 앱을 무료로 사용할 수 있도록 하는 경우 평가판 기간 중 일부 기능을 제외 하거나 제한 하 여 고객이 앱의 전체 버전으로 업그레이드 하도록 유도할 수 있습니다. 코딩을 시작 하기 전에 제한 되어야 하는 기능을 결정 한 다음, 전체 라이선스를 구매한 경우에만 앱이 작동 하도록 허용 해야 합니다. 고객이 앱을 구입 하기 전에 평가판 동안에만 표시 되는 배너 또는 워터 마크와 같은 기능을 사용 하도록 설정할 수도 있습니다.

> [!IMPORTANT]
> 이 문서에서는 [Windows. ApplicationModel. Store](/uwp/api/windows.applicationmodel.store) 네임 스페이스의 멤버를 사용 하 여 평가판 기능을 구현 하는 방법을 보여 줍니다. 이 네임 스페이스는 더 이상 새 기능으로 업데이트 되지 않으므로 대신 [Windows. Store](/uwp/api/windows.services.store) 네임 스페이스를 사용 하는 것이 좋습니다. **Windows. store** 네임 스페이스는 스토어 관리 사용 가능 추가 기능 및 구독과 같은 최신 추가 기능을 지원 하며, 파트너 센터 및 스토어에서 지원 되는 이후 유형의 제품과 기능과 호환 되도록 설계 되었습니다. Windows 10, 버전 1607에 **는 windows 10** **기념일 10.0 버전을 대상으로 하는 프로젝트에만 사용 될 수 있습니다. 빌드 14393)** 또는 Visual Studio의 이후 릴리스. **Windows. Store** 네임 스페이스를 사용 하 여 평가판 기능을 구현 하는 방법에 대 한 자세한 내용은 [이 문서](implement-a-trial-version-of-your-app.md)를 참조 하세요.

## <a name="prerequisites"></a>사전 요구 사항

고객이 구매할 기능을 추가할 수 있는 Windows 앱입니다.

## <a name="step-1-pick-the-features-you-want-to-enable-or-disable-during-the-trial-period"></a>1 단계: 평가 기간 동안 사용 하거나 사용 하지 않도록 설정할 기능 선택

앱의 현재 라이선스 상태는 [LicenseInformation](/uwp/api/Windows.ApplicationModel.Store.LicenseInformation) 클래스의 속성으로 저장 됩니다. 일반적으로 다음 단계에서 설명 하는 것 처럼 조건부 블록의 라이선스 상태에 종속 된 함수를 배치 합니다. 이러한 기능을 고려 하는 경우 모든 라이선스 상태에서 작동 하는 방식으로 구현할 수 있는지 확인 합니다.

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

라이선스 변경을 검색 하 고 앱에서 몇 가지 작업을 수행 하려면 다음 단계에 설명 된 대로이에 대 한 이벤트 처리기를 추가 해야 합니다.

## <a name="step-2-initialize-the-license-info"></a>2 단계: 라이선스 정보 초기화

앱을 초기화 하는 경우이 예제와 같이 앱에 대 한 [LicenseInformation](/uwp/api/Windows.ApplicationModel.Store.LicenseInformation) 개체를 가져옵니다. **LicenseInformation** 가 **licenseInformation** 형식의 전역 변수 또는 필드인 것으로 가정 합니다.

지금은 [Currentapp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp)대신 [currentappsimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 를 사용 하 여 시뮬레이트된 라이선스 정보를 얻게 됩니다. 앱의 릴리스 버전을 **스토어** 에 제출 하기 전에 코드의 모든 **currentappsimulator** 참조를 **currentapp** 으로 바꾸어야 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/TrialVersion.cs" id="InitializeLicenseTest":::

다음으로, 앱이 실행 되는 동안 라이선스가 변경 될 때 알림을 수신 하는 이벤트 처리기를 추가 합니다. 평가판 기간이 만료 되거나 고객이 스토어를 통해 앱을 구매 하는 경우 (예:) 앱의 라이선스가 변경 될 수 있습니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/TrialVersion.cs" id="InitializeLicenseTestWithEvent":::

## <a name="step-3-code-the-features-in-conditional-blocks"></a>3 단계: 조건부 블록의 기능 코딩

라이선스 변경 이벤트가 발생 하면 앱은 라이선스 API를 호출 하 여 평가판 상태가 변경 되었는지 확인 해야 합니다. 이 단계의 코드는이 이벤트에 대 한 처리기를 구성 하는 방법을 보여 줍니다. 이 시점에서 사용자가 앱을 구매한 경우 라이선스 상태가 변경 되었음을 사용자에 게 피드백을 제공 하는 것이 좋습니다. 앱이 코딩 된 방법 인 경우 사용자에 게 앱을 다시 시작 하도록 요청 해야 할 수도 있습니다. 하지만 가능한 한 원활 하 고 간편 하 게 전환 합니다.

이 예제에서는 응용 프로그램의 기능을 적절 하 게 사용 하거나 사용 하지 않도록 설정할 수 있도록 앱의 라이선스 상태를 평가 하는 방법을 보여 줍니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/TrialVersion.cs" id="ReloadLicense":::

## <a name="step-4-get-an-apps-trial-expiration-date"></a>4 단계: 앱 평가판 만료 날짜 가져오기

앱의 평가판 만료 날짜를 확인 하는 코드를 포함 합니다.

이 예제의 코드는 앱 평가판 라이선스의 만료 날짜를 가져오는 함수를 정의 합니다. 라이선스가 여전히 유효한 경우 평가판이 만료 될 때까지 남은 일 수를 사용 하 여 만료 날짜를 표시 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/TrialVersion.cs" id="DisplayTrialVersionExpirationTime":::

## <a name="step-5-test-the-features-using-simulated-calls-to-the-license-api"></a>5단계: 라이선스 API 호출을 시뮬레이트하여 기능 테스트

이제 시뮬레이션 된 데이터를 사용 하 여 앱을 테스트 합니다. **Currentappsimulator** 는 WindowsStoreProxy.xml 이라는 XML 파일에서 테스트 관련 라이선스 정보를 가져옵니다 .이 파일 은% UserProfile \\ % \\ AppData \\ local \\ &lt; Package package name &gt; \\ localstate \\ Microsoft \\ Windows Store \\ apidata에 있습니다. WindowsStoreProxy.xml을 편집 하 여 앱 및 해당 기능에 대 한 시뮬레이션 된 만료 날짜를 변경할 수 있습니다. 모든 가능한 만료 및 라이선스 구성을 테스트 하 여 모든 것이 의도 한 대로 작동 하는지 확인 합니다. 자세한 내용은 [CurrentAppSimulator에서 WindowsStoreProxy.xml 파일 사용](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy)을 참조 하세요.

이 경로 및 파일이 존재 하지 않는 경우 설치 하는 동안 또는 런타임에 만들어야 합니다. 특정 위치에 WindowsStoreProxy.xml 없는 상태로 [Currentappsimulator](/uwp/api/windows.applicationmodel.store.currentappsimulator.licenseinformation) 속성에 액세스 하려고 하면 오류가 발생 합니다.

## <a name="step-6-replace-the-simulated-license-api-methods-with-the-actual-api"></a>6 단계: 시뮬레이트된 라이선스 API 메서드를 실제 API로 바꾸기

시뮬레이트된 라이선스 서버를 사용 하 여 앱을 테스트 한 후 인증을 위해 스토어에 앱을 제출 하기 전에 다음 코드 샘플과 같이 **Currentappsimulator** 를 **currentapp** 으로 바꿉니다.

> [!IMPORTANT]
> 앱을 스토어에 제출할 때 앱이 **currentapp** 개체를 사용 해야 하며, 그렇지 않으면 인증에 실패 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/TrialVersion.cs" id="InitializeLicenseRetailWithEvent":::

## <a name="step-7-describe-how-the-free-trial-works-to-your-customers"></a>7 단계: 고객에 게 무료 평가판이 어떻게 작동 하는지 설명

무료 평가 기간 동안 및 후에 앱이 어떻게 동작 하는지 설명 해야 합니다. 그러면 고객이 앱의 동작에 영향을 주지 않습니다.

앱을 설명 하는 방법에 대 한 자세한 내용은 [앱 설명 만들기](../publish/create-app-store-listings.md)를 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [스토어 샘플 (평가판 및 앱 내 구매 설명)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [앱 가격 책정 및 가용성 설정](../publish/set-app-pricing-and-availability.md)
* [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp)
* [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator)
 

 
