---
author: mcleanbyron
Description: "Microsoft Store Services SDK는 더 많은 수익을 창출하고 고객을 확보할 수 있게 해주는 기능을 앱에 추가하는 데 사용할 수 있는 라이브러리 및 도구를 제공합니다."
title: Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
translationtype: Human Translation
ms.sourcegitcommit: 011b370b7bd7ad7c7d8f60281261b6da954e2256
ms.openlocfilehash: 840a5e76d409f547d55e558262af09c8fa36a544

---

# Microsoft Store Services SDK

Microsoft Store Services SDK는 앱에 광고 표시, A/B 실험 실행 등 UWP(유니버설 Windows 플랫폼) 앱에서 더 많은 수익을 창출하고 고객을 확보하는 데 도움이 되는 기능을 제공합니다. 이 SDK는 Visual Studio 2015 이상에 대한 확장입니다.

## SDK에서 지원하는 시나리오

이 SDK는 현재 UWP 앱에 대해 다음과 같은 시나리오를 지원합니다. 이 SDK는 시간이 지남에 따라 진화하여 새로운 참여 및 수익 창출 시나리오를 지원할 것입니다. 이 SDK의 API에 대한 참조 설명서는 [Microsoft Store Services SDK API 참조](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)를 참조하세요.

|  시나리오  |  설명   |
|------------|----------------|
|  [A/B 테스트로 UWP 앱에서 실험 실행](run-app-experiments-with-a-b-testing.md)    |  UWP(유니버설 Windows 플랫폼) 앱에서 A/B 테스트를 실행하여, 모든 고객에게 기능을 릴리스하기 전에 일부 고객에 대한 기능의 효과를 측정합니다. 개발자 센터 대시보드에서 실험을 정의한 후 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 클래스를 사용하여 실험에 대한 변형을 가져오고, 이 데이터를 사용하여 테스트할 기능의 동작을 수정한 다음 [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) 메서드를 사용하여 보기 이벤트 및 전환 이벤트를 개발자 센터로 보냅니다. 마지막으로, 대시보드를 사용하여 결과를 보고 실험을 관리합니다.  |
|  [UWP 앱에서 피드백 허브 시작](launch-feedback-hub-from-your-app.md)    |  UWP 앱에서 [StoreServicesFeedbackLauncher](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesfeedbacklauncher.aspx) 클래스를 사용하여 문제, 제안 및 좋아요를 제출할 수 있는 피드백 허브로 Windows 10 고객을 안내합니다. 그런 다음, 개발자 센터 대시보드의 [피드백 보고서](../publish/feedback-report.md)에서 이 피드백을 관리합니다. |
|  [개발자 센터 푸시 알림을 받도록 UWP 앱 구성](configure-your-app-to-receive-dev-center-notifications.md)    |  UWP 앱에서 [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) 클래스를 사용하여 Windows 개발자 센터 대시보드를 통해 고객에게 보내는 대상 지정 푸시 알림을 받도록 앱을 등록합니다.  |
|   [개발자 센터에서 사용 보고서에 대한 UWP 앱에 사용자 지정 이벤트 로깅](log-custom-events-for-dev-center.md)   |  UWP 앱에서 [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) 클래스를 사용하여 개발자 센터의 앱과 연결된 사용자 지정 이벤트를 로깅합니다. 그런 다음 개발자 센터 대시보드에서 [사용 보고서](https://msdn.microsoft.com/windows/uwp/publish/usage-report)의 **사용자 지정 이벤트** 섹션에서 사용자 지정 이벤트의 총 발생 횟수를 검토합니다.  |
|  [UWP 앱에서 광고 표시](display-ads-in-your-app.md)    |  UWP 앱에서 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 또는 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 컨트롤을 사용하여 배너 광고 또는 중간 광고를 표시함으로써 수익을 늘립니다.<br/><br/>**참고**  Microsoft Store Services SDK는 Windows 10용 UWP 앱만 지원합니다. Windows 8.1 및 Windows Phone 8.x 앱에서 광고를 표시하려면 [Microsoft Advertising SDK for Windows 및 Windows Phone 8.x](http://aka.ms/store-8-sdk)를 사용합니다.  |

<span id="prerequisites" />
## 필수 조건

Microsoft Store Services SDK에는 다음이 필요합니다.

* Visual Studio 2015 이상 버전
* 사용 중인 Visual Studio 버전과 함께 설치되는 유니버설 Windows 앱용 Visual Studio Tools

>**참고**  Visual Studio 2015와 함께 이 SDK를 설치하려면 유니버설 Windows 앱용 Visual Studio Tools 버전 1.1 이상이 설치되어 있어야 합니다. 유니버설 Windows 앱용 Visual Studio Tools의 이 업데이트에 대한 자세한 내용은 [릴리스 정보](http://go.microsoft.com/fwlink/?LinkID=624516)를 참조하세요.

<span id="install" />
## SDK 설치

개발 컴퓨터에서 Visual Studio 2015(또는 이후 릴리스)와 함께 사용할 Microsoft Store Services SDK를 설치하기 위한 옵션에는 다음 두 가지가 있습니다.

* **MSI 설치 관리자**  [여기](http://aka.ms/store-em-sdk)에서 사용 가능한 MSI 설치 관리자를 통해 SDK를 설치할 수 있습니다. 이 옵션을 사용하면 Visual Studio에서 UWP 프로젝트에 의해 참조될 수 있도록 개발 컴퓨터의 공유 위치에 SDK 라이브러리가 설치됩니다.
* **NuGet 패키지**  NuGet을 사용하여 Visual Studio에서 특정 UWP 프로젝트에 대한 SDK 라이브러리를 설치할 수 있습니다. 이 옵션을 사용하면 NuGet 패키지를 설치한 프로젝트에 대해서만 SDK 라이브러리가 설치됩니다.

Microsoft는 성능 향상과 새로운 기능을 추가하여 주기적으로 새 버전의 Microsoft Store Services SDK를 릴리스합니다. 이 SDK를 사용하는 기존 프로젝트가 있고 최신 버전을 사용하려는 경우 개발 컴퓨터에 최신 버전의 SDK를 다운로드하고 설치합니다.

>**참고**  Visual Studio 2015와 함께 이 SDK를 설치하려면 유니버설 Windows 앱용 Visual Studio Tools 버전 1.1 이상이 설치되어 있어야 합니다. 유니버설 Windows 앱용 Visual Studio Tools의 이 업데이트에 대한 자세한 내용은 [릴리스 정보](http://go.microsoft.com/fwlink/?LinkID=624516)를 참조하세요.

<span id="install-msi" />
### MSI를 통해 설치

MSI 설치 관리자를 통해 Microsoft Store Services SDK를 설치하려면

1.  Visual Studio 2015(또는 이후 릴리스)의 인스턴스를 모두 닫습니다. 이전에 Microsoft Advertising SDK, Universal Ad Client SDK, Ad Mediator 확장 또는 Microsoft Store Engagement and Monetization SDK의 이전 버전을 설치한 경우 지금 이러한 SDK 버전을 제거합니다.

2.  **명령 프롬프트** 창을 열고 다음 명령을 실행하여 Visual Studio와 함께 설치되었을 수 있으나 컴퓨터에 설치된 프로그램 목록에 나타나지 않을 수 있는 이전 광고 SDK 버전을 모두 정리합니다.
  ```
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.  [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)를 다운로드하여 설치합니다. 설치하는 데 몇 분 정도 걸릴 수 있습니다. 프로세스가 완료되었는지 확인하고 완료될 때까지 기다립니다.

4.  Visual Studio를 다시 시작합니다.

5.  이전 버전의 Microsoft Store Services SDK, Microsoft Advertising SDK, Universal Ad Client SDK 또는 Microsoft Store Engagement and Monetization SDK에 있는 라이브러리를 참조하는 기존 프로젝트가 있는 경우 Visual Studio에서 프로젝트를 열고 프로젝트를 정리한 후 다시 빌드합니다(**솔루션 탐색기**에서 프로젝트 노드를 마우스 오른쪽 단추로 클릭하고 **정리**를 선택한 다음 프로젝트 노드를 다시 마우스 오른쪽 단추로 클릭하고 **다시 빌드** 선택).

  그렇지 않고 프로젝트에서 처음으로 이 SDK를 사용하는 경우 이제 [프로젝트에 해당 Microsoft Store Services SDK 라이브러리 참조를 추가](#references)할 수 있습니다.

<span id="install-nuget" />
### NuGet을 통해 설치

NuGet을 통해 특정 프로젝트에 대한 Microsoft Store Services SDK 라이브러리를 설치하려면

1.  Visual Studio 2015(또는 이후 릴리스)의 인스턴스를 모두 닫습니다. 이전에 Microsoft Advertising SDK, Universal Ad Client SDK, Ad Mediator 확장 또는 Microsoft Store Engagement and Monetization SDK의 이전 버전을 설치한 경우 지금 이러한 SDK 버전을 제거합니다.

2.  **명령 프롬프트** 창을 열고 다음 명령을 실행하여 Visual Studio와 함께 설치되었을 수 있으나 컴퓨터에 설치된 프로그램 목록에 나타나지 않을 수 있는 이전 광고 SDK 버전을 모두 정리합니다.
  ```
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.  Visual Studio를 시작하고 Microsoft Store Services SDK 라이브러리를 사용하려는 프로젝트를 엽니다.

  >**참고**  프로젝트에 이 SDK의 이전 MSI 설치에 포함된 라이브러리 참조가 이미 들어 있으면 프로젝트에서 이러한 참조를 제거합니다. 이러한 참조는 참조하는 라이브러리가 이전 단계에서 제거되었으므로 옆에 경고 아이콘이 표시됩니다.

4. Visual Studio에서 **프로젝트** 및 **NuGet 패키지 관리**를 클릭합니다.

5. 검색 상자에 **Microsoft.Services.Store.SDK**를 입력하고 Microsoft.Services.Store.SDK 패키지를 설치합니다.

  >**참고**  **출력** 창이 지정된 경로가 너무 길다는 것을 나타내는 *Install-Package* 오류를 보고하는 경우 기본 위치보다 경로가 더 짧은 다른 위치로 패키지를 추출하도록 NuGet을 구성해야 할 수 있습니다. 이렇게 하려면 컴퓨터의 nuget.config 파일에 ```repositoryPath``` 값을 추가하고 NuGet 패키지를 추출할 수 있는 더 짧은 폴더에 할당합니다. 자세한 내용은 NuGet 설명서에서 [이 문서](http://docs.nuget.org/ndocs/consume-packages/configuring-nuget-behavior)를 참조하세요. 또는 Visual Studio 프로젝트를 더 짧은 경로의 대체 폴더로 이동하려고 할 수 있습니다.

6. 프로젝트 닫았다가 다시 엽니다.

7.  프로젝트가 NuGet를 통해 설치된 이전 버전의 Microsoft Store Services SDK에 있는 라이브러리를 이미 참조하며 프로젝트를 최신 버전의 SDK로 업데이트한 경우 프로젝트를 정리한 후 다시 빌드하는 것이 좋습니다(**솔루션 탐색기**에서 프로젝트 노드를 마우스 오른쪽 단추로 클릭하고 **정리**를 선택한 다음 프로젝트 노드를 다시 마우스 오른쪽 단추로 클릭하고 **다시 빌드** 선택).

  그렇지 않고 프로젝트에서 처음으로 이 SDK를 사용하는 경우 이제 [프로젝트에 해당 Microsoft Store Services SDK 라이브러리 참조를 추가](#references)할 수 있습니다.

<span id="references" />
## 프로젝트에 SDK 라이브러리 참조 추가

MSI 설치 관리자 또는 NuGet을 통해 Microsoft Store Services SDK를 설치한 후에는 다음 지침에 따라 UWP 프로젝트에서 해당 SDK 라이브러리를 참조합니다.

1. Visual Studio에서 프로젝트를 엽니다.

  >**참고**  프로젝트의 대상이 **모든 CPU**인 JavaScript 앱인 경우 아키텍처별 빌드 출력(예: **x86**)을 사용하도록 프로젝트를 업데이트합니다.

2. **솔루션 탐색기**에서 **참조**를 마우스 오른쪽 단추로 클릭한 다음 **참조 추가...**를 선택합니다.

3. **참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 다음 항목 중 하나 옆에 있는 확인란을 선택합니다.

  * 고객 참여 유도 시나리오에 대한 [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.aspx) 네임스페이스의 API를 사용하여 **Microsoft 참여 프레임워크** 옆의 확인란을 선택합니다. [A/B 실험을 실행](run-app-experiments-with-a-b-testing.md)하거나, [피드백 허브를 시작하거나](launch-feedback-hub-from-your-app.md), [개발자 센터에서 대상 지정 푸시 알림을 수신하거나](configure-your-app-to-receive-dev-center-notifications.md) [개발자 센터에 사용자 지정 이벤트를 로깅하려면](log-custom-events-for-dev-center.md) 이 옵션을 선택합니다.

  * [앱에 배너 광고 또는 중간 광고를 표시](display-ads-in-your-app.md)하기 위한 API를 사용하려면 프로젝트 형식에 따라 **Microsoft Advertising SDK for XAML** 또는 **Microsoft Advertising SDK for JavaScript**를 사용합니다.

3. **확인**을 클릭합니다.

>**참고**  NuGet을 통해 SDK 라이브러리를 설치한 경우 프로젝트에 **Microsoft Advertising SDK for XAML** 또는 **Microsoft Advertising SDK for JavaScript** 외에 **Microsoft.Services.Store.SDK** 참조가 포함됩니다. **Microsoft.Services.Store.SDK** 참조는 NuGet 패키지(포함된 라이브러리가 아님)를 나타내며 이는 무시해도 됩니다.

<span id="framework" />
## SDK의 프레임워크 패키지 이해

Microsoft Store Services SDK의 다음 라이브러리는 *프레임워크 패키지*로 구성됩니다.

* Microsoft.Advertising.dll 이 라이브러리에는 [Microsoft.Advertising](https://msdn.microsoft.com/en-us/library/windows/apps/mt313187.aspx) 및 [Microsoft.Advertising.WinRT.UI](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.advertising.winrt.ui.aspx) 네임스페이스의 광고 API가 포함되어 있습니다.
* Microsoft.Services.Store.Engagement.dll 이 라이브러리는 [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.services.store.engagement.aspx) 네임스페이스의 API를 포함합니다.

즉, 사용자가 이 라이브러리를 사용하는 앱 버전을 설치하면 수정 및 성능 향상이 포함된 새 버전의 라이브러리를 게시할 때마다 Windows 업데이트를 통해 디바이스의 라이브러리도 자동으로 업데이트됩니다. 이렇게 하면 고객은 디바이스에 항상 사용 가능한 최신 버전의 라이브러리가 설치되도록 할 수 있습니다.

이 라이브러리에 새로운 API 또는 기능을 도입하는 새 버전의 SDK를 릴리스하는 경우 이러한 기능을 사용하려면 최신 버전의 SDK를 설치해야 합니다. 이 시나리오에서는 업데이트된 앱도 스토어에 게시해야 합니다.

## 관련 항목

* [Microsoft Store Services SDK API 참조](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [A/B 테스트로 실험 실행](run-app-experiments-with-a-b-testing.md)
* [앱에서 피드백 허브 시작](launch-feedback-hub-from-your-app.md)
* [개발자 센터 푸시 알림을 받도록 앱 구성](configure-your-app-to-receive-dev-center-notifications.md)
* [개발자 센터에 대한 사용자 지정 이벤트 로깅](log-custom-events-for-dev-center.md)
* [앱에서 광고 표시](display-ads-in-your-app.md)



<!--HONumber=Nov16_HO1-->


