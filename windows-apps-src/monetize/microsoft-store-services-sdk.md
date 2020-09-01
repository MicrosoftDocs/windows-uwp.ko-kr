---
Description: Microsoft Store Services SDK는 앱에 기능을 추가 하 여 비용을 절감 하 고 고객을 확보 하는 데 사용할 수 있는 라이브러리 및 도구를 제공 합니다.
title: Microsoft Store Services SDK를 사용하여 고객과 소통
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
ms.date: 08/21/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 서비스 SDK
ms.localizationpriority: medium
ms.openlocfilehash: 7b8544f6d4f60b2f4ca91af35ff922fcfe089380
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155467"
---
# <a name="engage-customers-with-the-microsoft-store-services-sdk"></a>Microsoft Store Services SDK를 사용하여 고객과 소통

Microsoft Store Services SDK는 앱에 대상 알림을 보내고 앱에서 A/B 실험을 실행 하는 것과 같이 UWP (유니버설 Windows 플랫폼) 앱의 고객과 관련 된 기능을 제공 합니다. 이 SDK는 visual studio 2015 이상 버전의 Visual Studio에 대 한 확장입니다.

> [!NOTE]
> UWP 앱에서 광고를 표시 하려면 Microsoft Store Services SDK 대신 [MICROSOFT ADVERTISING sdk](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) 를 사용 합니다. 광고 라이브러리는 Microsoft Store Services SDK에서 Microsoft Advertising SDK로 이동 되었습니다. 자세한 내용은 [앱에서 광고 표시](display-ads-in-your-app.md)를 참조 하세요.



## <a name="scenarios-supported-by-the-microsoft-store-services-sdk"></a>Microsoft Store Services SDK에서 지 원하는 시나리오

Microsoft Store Services SDK는 현재 UWP 앱에 대해 다음과 같은 시나리오를 지원 합니다. API 참조 설명서는 [Microsoft Store SERVICES SDK API 참조](/uwp/api/overview/engagement)를 참조 하세요.

|  시나리오  |  Description   |
|------------|----------------|
|  [A/B 테스트를 사용 하 여 UWP 앱에서 실험 실행](run-app-experiments-with-a-b-testing.md)    |  모든 사용자에 게 기능을 출시 하기 전에 유니버설 Windows 플랫폼 (UWP) 앱에서 A/B 테스트를 실행 하 여 일부 고객의 기능 효과를 측정 합니다. 파트너 센터에서 실험을 정의한 후에는 [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) 클래스를 사용 하 여 앱에서 실험의 변형을 가져오고,이 데이터를 사용 하 여 테스트 중인 기능의 동작을 수정한 다음, [logforvariation](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) 메서드를 사용 하 여 보기 이벤트 및 변환 이벤트를 파트너 센터에 보냅니다. 마지막으로 파트너 센터를 사용 하 여 결과를 보고 실험을 관리 합니다.  |
|  [UWP 앱에서 피드백 허브 시작](launch-feedback-hub-from-your-app.md)    |  UWP 앱에서 [StoreServicesFeedbackLauncher](/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) 클래스를 사용 하 여 Windows 10 고객에 게 문제, 제안 및 upvotes를 제출할 수 있는 피드백 허브로 안내 합니다. 그런 다음, 파트너 센터의 [피드백 보고서](../publish/feedback-report.md)에서 이 피드백을 관리합니다. |
|  [파트너 센터 푸시 알림을 받도록 UWP 앱 구성](configure-your-app-to-receive-dev-center-notifications.md)    |  UWP 앱에서 [StoreServicesEngagementManager](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) 클래스를 사용 하 여 파트너 센터를 통해 고객에 게 보내는 대상 푸시 알림을 받도록 앱을 등록 합니다.  |
|   [파트너 센터의 사용량 보고서에 대해 UWP 앱에서 사용자 지정 이벤트 기록](log-custom-events-for-dev-center.md)   |  UWP 앱에서 [StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 클래스를 사용 하 여 파트너 센터에서 앱과 연결 된 사용자 지정 이벤트를 로깅합니다. 그런 다음 파트너 센터의 [사용량 보고서](../publish/usage-report.md) 의 **사용자 지정 이벤트** 섹션에서 사용자 지정 이벤트에 대 한 총 발생 횟수를 검토 합니다.  |

<span id="prerequisites" />

## <a name="prerequisites"></a>필수 구성 요소

Microsoft Store Services SDK에는 다음이 필요 합니다.

* Visual Studio 2015 이상 버전
* 사용 중인 Visual Studio 버전과 함께 설치 된 유니버설 Windows 앱에 대 한 Visual Studio Tools 합니다.

<span id="install" />

## <a name="install-the-sdk"></a>SDK 설치

개발 컴퓨터에 Microsoft Store Services SDK를 설치 하는 방법에는 다음 두 가지가 있습니다.

* **MSI 설치 관리자** &nbsp; &nbsp; [여기](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK)에서 사용할 수 있는 MSI 설치 관리자를 통해 SDK를 설치할 수 있습니다.
* **NuGet 패키지** &nbsp; &nbsp; SDK를 NuGet 패키지로 설치할 수 있습니다.

Microsoft는 성능 향상 및 새로운 기능을 통해 새로운 버전의 Microsoft Store Services SDK를 정기적으로 릴리스 합니다. SDK를 사용 하는 기존 프로젝트가 있고 최신 버전을 사용 하려는 경우 개발 컴퓨터에 최신 버전의 SDK를 다운로드 하 여 설치 합니다.

<span id="install-msi" />

### <a name="install-via-msi"></a>MSI를 통해 설치

MSI 설치 관리자를 통해 Microsoft Store Services SDK를 설치 하려면 다음을 수행 합니다.

1.  Visual Studio의 모든 인스턴스를 닫습니다.

2. 이전에 Microsoft Store Engagement 및 수익 화 SDK, 유니버설 Ad 클라이언트 SDK 또는 Ad Mediator 확장을 설치한 경우 이제 이러한 Sdk를 제거 합니다. 필요에 따라 **명령 프롬프트** 창을 열고 다음 명령을 실행 하 여 Visual Studio와 함께 설치 되었지만 컴퓨터의 설치 된 프로그램 목록에 표시 되지 않을 수 있는 이전 SDK 버전을 정리 합니다.
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  [Microsoft Store SERVICES SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK)를 다운로드 하 여 설치 합니다. 설치 하는 데 몇 분 정도 걸릴 수 있습니다. 프로세스가 완료 될 때까지 대기 해야 합니다.

4.  Visual Studio를 다시 시작합니다.

5.  Microsoft Store Services SDK, Microsoft Advertising SDK, 유니버설 Ad 클라이언트 SDK 또는 Microsoft Store Engagement 및 수익 화 SDK의 이전 버전에서 라이브러리를 참조 하는 기존 프로젝트가 있는 경우 Visual Studio에서 프로젝트를 열고 프로젝트를 정리 하 고 다시 빌드해야 합니다 ( **솔루션 탐색기**에서 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 프로젝트 노드를 다시 마우스 오른쪽 단추로 클릭 한 다음 다시 **빌드** **를 선택**).

  그렇지 않으면 프로젝트에서 처음으로 SDK를 사용 하는 경우 [프로젝트에 어셈블리 참조를 추가할](#references)준비가 된 것입니다.

<span id="install-nuget" />

### <a name="install-via-nuget"></a>NuGet을 통해 설치

NuGet을 통해 Microsoft Store Services SDK 라이브러리를 설치 하려면 다음을 수행 합니다.

1.  Visual Studio의 모든 인스턴스를 닫습니다.

2. 이전에 Microsoft Store Engagement 및 수익 화 SDK, 유니버설 Ad 클라이언트 SDK 또는 Ad Mediator 확장을 설치한 경우 이제 이러한 Sdk를 제거 합니다. 필요에 따라 **명령 프롬프트** 창을 열고 다음 명령을 실행 하 여 Visual Studio와 함께 설치 되었지만 컴퓨터의 설치 된 프로그램 목록에 표시 되지 않을 수 있는 이전 SDK 버전을 정리 합니다.
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Visual Studio를 시작 하 고 Microsoft Store Services SDK를 사용 하려는 프로젝트를 엽니다.
    > [!NOTE]
    > 프로젝트에 이미 SDK의 이전 MSI 설치에서 라이브러리 참조가 포함 된 경우 프로젝트에서 이러한 참조를 제거 합니다. 이러한 참조는 이전 단계에서 참조 하는 라이브러리가 제거 되었으므로 옆에 경고 아이콘이 있습니다.

4. Visual Studio에서 **프로젝트** 및 **NuGet 패키지 관리**를 클릭 합니다.

5. 검색 상자에 **microsoft** 서비스를 입력 하 고 microsoft. \engagement 패키지를 설치 합니다. 패키지 설치가 완료 되 면 솔루션을 저장 합니다.
    > [!NOTE]
    > **출력** 창에서 지정 된 경로가 너무 긴 것을 나타내는 *설치 패키지* 오류를 보고 하는 경우 기본 위치 보다 짧은 경로를 사용 하 여 대체 위치에 패키지를 추출 하도록 NuGet을 구성 해야 할 수 있습니다. 이렇게 하려면 `repositoryPath` 컴퓨터의 nuget.config 파일에 값을 추가 하 고 NuGet 패키지를 추출할 수 있는 짧은 폴더 경로에 할당 합니다. 자세한 내용은 NuGet 설명서에서 [이 문서](/nuget/consume-packages/configuring-nuget-behavior) 를 참조 하세요. 또는 더 짧은 경로를 사용 하 여 Visual Studio 프로젝트를 대체 폴더로 이동 하려고 할 수 있습니다. 글로벌 패키지 경로가 너무 길어서 문제가 발생할 수도 있습니다. 이 경우 `globalPackagesFolder` nuget.config 파일에 값을 추가 합니다.

6. 프로젝트를 포함 하는 Visual Studio 솔루션을 닫은 다음 솔루션을 다시 엽니다.

7.  프로젝트에서 NuGet을 통해 설치 된 Microsoft Store Services SDK의 이전 버전에서 라이브러리를 이미 참조 하 고 프로젝트를 최신 버전의 SDK로 업데이트 한 경우 프로젝트를 정리 하 고 다시 빌드하는 것이 좋습니다. **솔루션 탐색기**에서 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 **정리**를 선택한 다음 프로젝트 노드를 다시 마우스 오른쪽 단추로 클릭 하 고 다시 **빌드**를 선택 합니다.

  그렇지 않으면 프로젝트에서 처음으로 SDK를 사용 하는 경우 [프로젝트에 어셈블리 참조를 추가할](#references)준비가 된 것입니다.

<span id="references" />

## <a name="add-the-assembly-reference-to-your-project"></a>프로젝트에 어셈블리 참조 추가

MSI 설치 관리자 또는 NuGet을 통해 Microsoft Store Services SDK를 설치한 후에는 다음 지침에 따라 UWP 프로젝트에서 SDK 어셈블리를 참조 합니다.

1. Visual Studio에서 프로젝트를 엽니다.
    > [!NOTE]
    > 프로젝트가 **모든 CPU**를 대상으로 하는 JavaScript 앱 인 경우 아키텍처 관련 빌드 출력 (예: **x86**)을 사용 하도록 프로젝트를 업데이트 합니다.

2. **솔루션 탐색기**에서 **참조** 를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가** ...를 선택 합니다.

3. **참조 관리자**에서 **유니버설 Windows**를 확장 하 고 **확장**을 클릭 한 다음 **Microsoft Engagement 프레임 워크**옆의 확인란을 선택 합니다. 이를 통해 [Microsoft 서비스](/uwp/api/microsoft.services.store.engagement) 의 api를 사용할 수 있습니다.

3. **확인**을 클릭합니다.

> [!NOTE]
> NuGet을 통해 SDK 라이브러리를 설치한 경우 프로젝트에는 **Microsoft. Store.** **Microsoft. Store Engagement** 참조는 NuGet 패키지를 나타내며,이 패키지의 라이브러리는 무시 해도 됩니다.

<span id="framework" />

## <a name="understanding-framework-packages-in-the-sdk"></a>SDK의 프레임 워크 패키지 이해

Microsoft Store Services SDK의 Microsoft.Services.Store.Engagement.dll 라이브러리는 *프레임 워크 패키지로*구성 됩니다. 이 라이브러리는 [Microsoft](/uwp/api/microsoft.services.store.engagement) 의 api를 포함 합니다.

이 라이브러리는 프레임 워크 패키지 이므로 사용자가이 라이브러리를 사용 하는 앱 버전을 설치한 후 수정 및 성능 향상으로 라이브러리의 새 버전을 게시할 때마다 Windows 업데이트를 통해 장치에서이 라이브러리가 자동으로 업데이트 됩니다. 이렇게 하면 고객이 항상 사용 가능한 최신 버전의 라이브러리를 장치에 설치 하도록 할 수 있습니다.

이 라이브러리에서 새 Api 또는 기능을 도입 하는 새 버전의 SDK를 출시 하는 경우 해당 기능을 사용 하려면 최신 버전의 SDK를 설치 해야 합니다. 이 시나리오에서는 업데이트 된 앱을 스토어에도 게시 해야 합니다.

## <a name="related-topics"></a>관련 항목

* [Microsoft Store Services SDK API 참조](/uwp/api/overview/engagement)
* [A/B 테스트를 사용 하 여 실험 실행](run-app-experiments-with-a-b-testing.md)
* [앱에서 피드백 허브 시작](launch-feedback-hub-from-your-app.md)
* [파트너 센터 푸시 알림을 받도록 앱 구성](configure-your-app-to-receive-dev-center-notifications.md)
* [파트너 센터의 사용자 지정 이벤트 로깅](log-custom-events-for-dev-center.md)