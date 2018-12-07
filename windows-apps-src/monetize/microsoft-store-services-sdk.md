---
Description: The Microsoft Store Services SDK provides libraries and tools that you can use to add features to your apps that help you make more money and gain customers.
title: Microsoft Store Services SDK를 사용하여 고객과 소통
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
ms.date: 08/21/2017
ms.topic: article
keywords: Windows 10, uwp, Microsoft Store Services SDK
ms.localizationpriority: medium
ms.openlocfilehash: c0c283f9edd33b8c39ebccd0a71019741a0d1448
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8788397"
---
# <a name="engage-customers-with-the-microsoft-store-services-sdk"></a>Microsoft Store Services SDK를 사용하여 고객과 소통

Microsoft Store Services SDK에 도움이 되는 기능 앱에 대상된 알림 보내기 A 실행 등, 유니버설 Windows 플랫폼 (UWP) 앱에서 고객을 확보 제공 / B 실험 앱. 이 SDK는 Visual Studio 2015 이상에 대한 확장입니다.

> [!NOTE]
> UWP 앱에서 광고를 표시하려면 Microsoft Store Services SDK 대신 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)를 사용합니다. Advertising 라이브러리가 Microsoft Store Services SDK에서 Microsoft Advertising SDK로 이동되었습니다. 자세한 내용은 [앱에서 광고 표시](display-ads-in-your-app.md)를 참조하세요.



## <a name="scenarios-supported-by-the-microsoft-store-services-sdk"></a>Microsoft Store Services SDK가 지원하는 시나리오

Microsoft Store Services SDK는 현재 UWP 앱에 대해 다음과 같은 시나리오를 지원합니다. API에 대한 참조 설명서는 [Microsoft Store Services SDK API 참조](https://docs.microsoft.com/uwp/api/overview/engagement)를 참조하세요.

|  시나리오  |  설명   |
|------------|----------------|
|  [A/B 테스트로 UWP 앱에서 실험 실행](run-app-experiments-with-a-b-testing.md)    |  UWP(유니버설 Windows 플랫폼) 앱에서 A/B 테스트를 실행하여, 모든 고객에게 기능을 릴리스하기 전에 일부 고객에 대한 기능의 효과를 측정합니다. 파트너 센터에서 실험을 정의한 후 실험에 대 한 변형 앱에서이 데이터를 사용 하 여, 테스트할 기능의 동작을 수정 하려면 다음 사용 하려면 LogForVariation [ [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) 클래스를 사용 ](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation)파트너 센터에 보기 이벤트 및 전환 이벤트를 전송 하는 방법. 마지막으로, 결과 보고 실험을 관리 하려면 파트너 센터를 사용 합니다.  |
|  [UWP 앱에서 피드백 허브 시작](launch-feedback-hub-from-your-app.md)    |  UWP 앱에서 [StoreServicesFeedbackLauncher](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) 클래스를 사용하여 문제, 제안 및 좋아요를 제출할 수 있는 피드백 허브로 Windows10 고객을 안내합니다. 그런 다음 파트너 센터에서 [피드백 보고서](../publish/feedback-report.md) 에이 피드백을 관리 합니다. |
|  [파트너 센터 푸시 알림을 받도록 UWP 앱 구성](configure-your-app-to-receive-dev-center-notifications.md)    |  UWP 앱에서 [StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) 클래스를 사용 하 여 파트너 센터를 사용 하 여 고객에 게 보내는 대상된 푸시 알림을 받도록 앱을 등록 합니다.  |
|   [파트너 센터에서 사용 보고서에 대 한 UWP 앱에 사용자 지정 이벤트 로깅](log-custom-events-for-dev-center.md)   |  파트너 센터의 앱과 관련 된 사용자 지정 이벤트를 로깅할 UWP 앱에서 [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 클래스를 사용 합니다. 파트너 센터에서 [사용 보고서](https://msdn.microsoft.com/windows/uwp/publish/usage-report) 의 **사용자 지정 이벤트** 섹션에서 사용자 지정 이벤트에 대 한 총 발생 횟수를 검토 합니다.  |

<span id="prerequisites" />

## <a name="prerequisites"></a>필수 구성 요소

Microsoft Store Services SDK에는 다음이 필요합니다.

* Visual Studio 2015 이상 버전
* 사용 중인 Visual Studio 버전과 함께 설치되는 유니버설 Windows 앱용 Visual Studio Tools

<span id="install" />

## <a name="install-the-sdk"></a>SDK 설치

개발용 컴퓨터에 두 가지 방법 중 하나로 Microsoft Store Services SDK를 설치할 수 있습니다.

* **MSI 설치 관리자**&nbsp;&nbsp;[여기](http://aka.ms/store-em-sdk)에서 사용 가능한 MSI 설치 관리자를 통해 SDK를 설치할 수 있습니다.
* **NuGet 패키지**&nbsp;&nbsp;SDK를 NuGet 패키지로 설치할 수 있습니다.

Microsoft는 성능 향상과 새로운 기능을 추가하여 주기적으로 새 버전의 Microsoft Store Services SDK를 릴리스합니다. 이 SDK를 사용하는 기존 프로젝트가 있고 최신 버전을 사용하려는 경우 개발 컴퓨터에 최신 버전의 SDK를 다운로드하고 설치합니다.

<span id="install-msi" />

### <a name="install-via-msi"></a>MSI를 통해 설치

MSI 설치 관리자를 통해 Microsoft Store Services SDK를 설치하려면

1.  모든 Visual Studio 인스턴스를 닫습니다.

2. 이전에 Microsoft Store Engagement, Monetization SDK, Universal Ad Client SDK, Ad Mediator 확장을 설치했다면, 지금 이러한 SDK를 제거합니다. **명령 프롬프트** 창을 열고 다음 명령을 실행하여 Visual Studio와 함께 설치되었을 수 있으나 컴퓨터에 설치된 프로그램 목록에 나타나지 않을 수 있는 이전 SDK 버전을 모두 정리하는 방법도 있습니다.
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)를 다운로드하여 설치합니다. 설치하는 데 몇 분 정도 걸릴 수 있습니다. 프로세스가 완료되었는지 확인하고 완료될 때까지 기다립니다.

4.  Visual Studio를 다시 시작합니다.

5.  이전 버전의 Microsoft Store Services SDK, Microsoft Advertising SDK, Universal Ad Client SDK 또는 Microsoft Store Engagement and Monetization SDK에 있는 라이브러리를 참조하는 기존 프로젝트가 있는 경우 Visual Studio에서 프로젝트를 열고 프로젝트를 정리한 후 다시 빌드합니다(**솔루션 탐색기**에서 프로젝트 노드를 마우스 오른쪽 단추로 클릭하고 **정리**를 선택한 다음 프로젝트 노드를 다시 마우스 오른쪽 단추로 클릭하고 **다시 빌드** 선택).

  기타 프로젝트에서 처음 SDK를 사용하는 경우에도, 이제 [프로젝트에 어셈블리 참조를 추가](#references)할 수 있습니다.

<span id="install-nuget" />

### <a name="install-via-nuget"></a>NuGet을 통해 설치

NuGet을 통해 Microsoft Store Services SDK 라이브러리를 설치하려면

1.  모든 Visual Studio 인스턴스를 닫습니다.

2. 이전에 Microsoft Store Engagement, Monetization SDK, Universal Ad Client SDK, Ad Mediator 확장을 설치했다면, 지금 이러한 SDK를 제거합니다. **명령 프롬프트** 창을 열고 다음 명령을 실행하여 Visual Studio와 함께 설치되었을 수 있으나 컴퓨터에 설치된 프로그램 목록에 나타나지 않을 수 있는 이전 SDK 버전을 모두 정리하는 방법도 있습니다.
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Visual Studio를 시작하고 Microsoft Store Services SDK를 사용하려는 프로젝트를 엽니다.
    > [!NOTE]
    > 프로젝트에 이 SDK의 이전 MSI 설치에 포함된 라이브러리 참조가 이미 들어 있으면 프로젝트에서 이러한 참조를 제거합니다. 이러한 참조는 참조하는 라이브러리가 이전 단계에서 제거되었으므로 옆에 경고 아이콘이 표시됩니다.

4. Visual Studio에서 **프로젝트** 및 **NuGet 패키지 관리**를 클릭합니다.

5. 검색 상자에 **Microsoft.Services.Store.Engagement**를 입력하고 Microsoft.Services.Store.Engagement 패키지를 설치합니다. 패키지 설치가 완료되면 솔루션을 저장합니다.
    > [!NOTE]
    > **출력** 창이 지정된 경로가 너무 길다는 것을 나타내는 *Install-Package* 오류를 보고하는 경우, 기본 위치보다 경로가 더 짧은 다른 위치로 패키지를 추출하도록 NuGet을 구성해야 할 수 있습니다. 이렇게 하려면 컴퓨터의 nuget.config 파일에 ```repositoryPath``` 값을 추가하고 NuGet 패키지를 추출할 수 있는 더 짧은 폴더에 할당합니다. 자세한 내용은 NuGet 설명서에서 [이 문서](http://docs.nuget.org/ndocs/consume-packages/configuring-nuget-behavior)를 참조하세요. 또는 Visual Studio 프로젝트를 더 짧은 경로의 대체 폴더로 이동하려고 할 수 있습니다. 문제가 너무 오래 되 고 글로벌 패키지 경로 때문일 수 있습니다. 이 경우 추가 합니다 ```globalPackagesFolder``` 값 nuget.config 파일에 복사 합니다.

6. 프로젝트가 포함된 Visual Studio 솔루션을 닫고, 솔루션을 다시 엽니다.

7.  프로젝트가 NuGet를 통해 설치된 이전 버전의 Microsoft Store Services SDK에 있는 라이브러리를 이미 참조하며 프로젝트를 최신 버전의 SDK로 업데이트한 경우 프로젝트를 정리한 후 다시 빌드하는 것이 좋습니다(**솔루션 탐색기**에서 프로젝트 노드를 마우스 오른쪽 단추로 클릭하고 **정리**를 선택한 다음 프로젝트 노드를 다시 마우스 오른쪽 단추로 클릭하고 **다시 빌드** 선택).

  기타 프로젝트에서 처음 SDK를 사용하는 경우에도, 이제 [프로젝트에 어셈블리 참조를 추가](#references)할 수 있습니다.

<span id="references" />

## <a name="add-the-assembly-reference-to-your-project"></a>프로젝트에 어셈블리 참조 추가

MSI 설치 관리자 또는 NuGet을 통해 Microsoft Store Services SDK를 설치한 후에는 다음 지침에 따라 UWP 프로젝트에서 해당 SDK 어셈블리를 참조합니다.

1. Visual Studio에서 프로젝트를 엽니다.
    > [!NOTE]
    > 프로젝트의 대상이 **모든 CPU**인 JavaScript 앱인 경우 아키텍처별 빌드 출력(예: **x86**)을 사용하도록 프로젝트를 업데이트합니다.

2. **솔루션 탐색기**에서 **참조**를 마우스 오른쪽 단추로 클릭한 다음 **참조 추가...** 를 선택합니다.

3. **참조 관리자**에서 **유니버설 Windows**를 확장하고, **확장**을 클릭한 후 **Microsoft Engagement Framework** 옆에 있는 확인란을 선택합니다. 그러면 [Microsoft.Services.Store.Engagement](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement) 네임스페이스에 API를 사용할 수 있습니다.

3. **확인**을 클릭합니다.

> [!NOTE]
> NuGet을 통해 SDK 라이브러리를 설치한 경우 프로젝트에 **Microsoft.Services.Store.Engagement** 참조가 포함됩니다. **Microsoft.Services.Store.Engagement** 참조는 NuGet 패키지(포함된 라이브러리가 아님)를 나타내며 이는 무시해도 됩니다.

<span id="framework" />

## <a name="understanding-framework-packages-in-the-sdk"></a>SDK의 프레임워크 패키지 이해

Microsoft Store Services SDK의 Microsoft.Services.Store.Engagement.dll 라이브러리는 *프레임워크 패키지*로 구성됩니다. 이 라이브러리는 [Microsoft.Services.Store.Engagement](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement) 네임스페이스의 API를 포함합니다.

이 라이브러리는 프레임워크 패키지입니다. 다시 말해서 사용자가 이 라이브러리를 사용하는 앱 버전을 설치하면 수정 및 성능 향상이 포함된 새 버전의 라이브러리가 게시될 때마다 Windows 업데이트를 통해 사용자 디바이스의 라이브러리가 자동으로 업데이트됩니다. 따라서 고객의 디바이스에 항상 사용 가능한 최신 버전의 라이브러리가 설치됩니다.

이 라이브러리에 새로운 API 또는 기능을 도입하는 새 버전의 SDK를 릴리스하는 경우 이러한 기능을 사용하려면 최신 버전의 SDK를 설치해야 합니다. 이 시나리오에서는 업데이트된 앱도 스토어에 게시해야 합니다.

## <a name="related-topics"></a>관련 항목

* [Microsoft Store Services SDK API 참조](https://docs.microsoft.com/uwp/api/overview/engagement)
* [A/B 테스트로 실험 실행](run-app-experiments-with-a-b-testing.md)
* [앱에서 피드백 허브 시작](launch-feedback-hub-from-your-app.md)
* [파트너 센터 푸시 알림을 받도록 앱 구성](configure-your-app-to-receive-dev-center-notifications.md)
* [파트너 센터에 대해 사용자 지정 이벤트 로깅](log-custom-events-for-dev-center.md)
