---
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: AdControl 클래스를 사용 하 여 Windows 10 용 JavaScript/HTML 앱 (UWP)에 배너 광고를 표시 하는 방법을 알아봅니다.
title: HTML 5 및 JavaScript의 AdControl
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, AdControl, ad 컨트롤, javascript, HTML
ms.localizationpriority: medium
ms.openlocfilehash: b99432c40bf2b4633e8902a5bb7b7eedab3119dc
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364056"
---
# <a name="adcontrol-in-html-5-and-javascript"></a>HTML 5 및 JavaScript의 AdControl

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 정보](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

이 연습에서는 [Adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 클래스를 사용 하 여 Windows 10 용 UWP (유니버설 Windows 플랫폼) JAVASCRIPT/HTML 앱에서 배너 광고를 표시 하는 방법을 보여 줍니다.

JavaScript/HTML 앱에 배너 광고를 추가 하는 방법을 보여 주는 전체 샘플 프로젝트는 [GitHub의 광고 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)을 참조 하세요.

## <a name="prerequisites"></a>사전 요구 사항

* Visual studio 2015 이상 버전의 Visual Studio를 사용 하 여 [MICROSOFT ADVERTISING SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) 를 설치 합니다. 설치 지침은 [이 문서](install-the-microsoft-advertising-libraries.md)를 참조 하세요.

> [!NOTE]
> Windows 10 SDK 버전 10.0.14393 (기념일 업데이트) 또는 이후 버전의 Windows SDK을 설치한 경우 [WinJS](https://github.com/winjs/winjs) 라이브러리도 설치 해야 합니다. 이 라이브러리는 Windows 10 용 Windows SDK의 이전 버전에 포함 되어 있지만 Windows 10 SDK 버전 10.0.14393 (기념일 업데이트)부터이 라이브러리를 별도로 설치 해야 합니다. 

## <a name="integrate-a-banner-ad-into-your-app"></a>배너 광고를 앱에 통합

1. Visual Studio에서 프로젝트를 열거나 새 프로젝트를 만듭니다.

    > [!NOTE]
    > 기존 프로젝트를 사용 하는 경우 프로젝트에서 appxmanifest.xml 파일을 열고 **인터넷 (클라이언트)** 기능이 선택 되어 있는지 확인 합니다. 앱에서 테스트 광고와 라이브 광고를 수신 하려면이 기능이 필요 합니다.

2. 프로젝트가 **모든 CPU**를 대상으로 하는 경우 프로젝트를 업데이트 하 여 아키텍처 관련 빌드 출력 (예: **x86**)을 사용 합니다. 프로젝트가 **모든 CPU**를 대상으로 하는 경우에는 다음 단계에서 Microsoft 광고 라이브러리에 대 한 참조를 성공적으로 추가할 수 없습니다. 자세한 내용은 [프로젝트의 모든 CPU를 대상으로 하 여 발생 한 참조 오류](known-issues-for-the-advertising-libraries.md#reference_errors)를 참조 하세요.

3. 프로젝트에서 Microsoft Advertising SDK에 대 한 참조를 추가 합니다.

    1. **솔루션 탐색기** 창에서 **참조**를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가** ...를 선택 합니다.
    2.  **참조 관리자**에서 **유니버설 Windows**를 확장 하 고 **확장**을 클릭 한 후 **Microsoft Advertising SDK for JavaScript** (버전 10.0) 옆의 확인란을 선택 합니다.
    3.  **참조 관리자**에서 확인을 클릭 합니다.

6.  index.html 파일 (또는 프로젝트에 적합 한 다른 html 파일)을 엽니다.

7.  ** &lt; 헤드 &gt; ** 섹션에서 프로젝트의 JavaScript 참조 인 기본 .css 및 main.js 뒤에 ad.js에 대 한 참조를 추가 합니다.

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    > [!NOTE]
    > 이 줄은 main.js 포함 후 ** &lt; head &gt; ** 섹션에 배치 해야 합니다. 그렇지 않으면 프로젝트를 빌드할 때 오류가 발생 합니다.

8.  default.html 파일 (또는 프로젝트에 적합 한 다른 html 파일)의 ** &lt; body &gt; ** 섹션을 수정 하 여 **adcontrol**의 **div** 를 포함 합니다. **Adcontrol** 의 **ApplicationId** 및 **adunit id** 속성을 [테스트 광고 단위 값](set-up-ad-units-in-your-app.md#test-ad-units)에 할당 합니다. 또한 컨트롤이 [배너 광고에 대해 지원 되는 광고 크기](supported-ad-sizes-for-banner-ads.md)중 하나를 갖도록 **높이** 와 **너비** 를 조정 합니다.

    > [!NOTE]
    > 모든 **Adcontrol** 에는 서비스에서 컨트롤에 대 한 광고를 제공 하는 데 사용 되는 해당 하는 *ad 단위가* 있으며 모든 ad 단위는 *ad 단위 id* 와 *응용 프로그램 id*로 구성 됩니다. 이러한 단계에서는 테스트 ad 단위 ID 및 응용 프로그램 ID 값을 컨트롤에 할당 합니다. 이러한 테스트 값은 응용 프로그램의 테스트 버전 에서만 사용할 수 있습니다. 스토어에 앱을 게시 하기 전에 이러한 테스트 값을 파트너 센터의 [라이브 값으로 바꾸어야](#release) 합니다.

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  앱을 컴파일하고 실행 하 여 광고를 확인 합니다.

다음 예제에서는 간단한 앱에 대 한 전체 index.html을 보여 줍니다.

``` HTML
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>AdControlExampleApp</title>
    <!-- WinJS references -->
    <link href="lib/winjs-4.0.1/css/ui-light.css" rel="stylesheet" />
    <script src="lib/winjs-4.0.1/js/base.js"></script>
    <script src="lib/winjs-4.0.1/js/ui.js"></script>
    <!-- AdControlExampleApp references -->
    <link href="css/default.css" rel="stylesheet" />
    <script src="js/main.js"></script>
    <!-- Required reference for AdControl -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
</head>
<body>
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

### <a name="create-an-adcontrol-programmatically-in-javascript"></a>Javascript에서 프로그래밍 방식으로 AdControl 만들기

이전 단계에서는 HTML 태그에서 **Adcontrol** 을 선언 하는 방법을 보여 줍니다. 또는 JavaScript를 사용 하 여 프로그래밍 방식으로 **Adcontrol** 을 만들 수 있습니다. 이 예에서는 ID가 **Myad**인 HTML에서 기존 **div** 를 사용 하 고 있다고 가정 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/AdControlSamples/js/main.js" id="DeclareAdControl":::

이 예에서는 **Myaderror**, **Myaderror**및 **myAdEngagedChanged**라는 이벤트 처리기 메서드를 이미 선언 했다고 가정 합니다.

이 코드를 사용 하 고 광고가 표시 되지 않는 경우 **Adcontrol**이 포함 된 **div** 에 **상대적인 위치의** 특성을 삽입 해 볼 수 있습니다. 이렇게 하면 **IFrame**의 기본 설정이 재정의 됩니다. 이 특성의 값으로 인해 표시 되지 않는 한 광고는 올바르게 표시 됩니다. 새 ad 단위는 최대 30 분 동안 사용 하지 못할 수 있습니다.

> [!NOTE]
> 이 예제에 표시 된 *applicationId* 및 *ad단위 id* 값은 [테스트 모드 값](set-up-ad-units-in-your-app.md#test-ad-units)입니다. 제출 하기 위해 앱을 제출 하기 전에 [이러한 값을 파트너 센터의 라이브 값으로 바꾸어야](set-up-ad-units-in-your-app.md#live-ad-units) 합니다.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>라이브 광고를 사용 하 여 앱 릴리스

1. 앱에서 배너 광고를 사용 하는 것이 [배너 광고에 대 한 지침](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)을 따라야 합니다.

1.  파트너 센터에서 [앱 내 광고](../publish/in-app-ads.md) 페이지로 이동 하 여 [ad 단위를 만듭니다](set-up-ad-units-in-your-app.md#live-ad-units). Ad 단위 형식에 **배너**를 지정 합니다. Ad 단위 ID와 응용 프로그램 ID를 모두 기록해 둡니다.
    > [!NOTE]
    > 테스트 ad 단위와 라이브 UWP ad 단위의 응용 프로그램 ID 값은 서로 다른 형식입니다. 테스트 응용 프로그램 ID 값은 Guid입니다. 파트너 센터에서 라이브 UWP ad 단위를 만들 때 ad 단위의 응용 프로그램 ID 값은 항상 앱에 대 한 저장소 ID와 일치 합니다. 예를 들어 매장 ID 값은 9NBLGGH4R315와 같습니다.

2. 필요 [에 따라 앱 내 광고](../publish/in-app-ads.md) 페이지의 [중재 설정](../publish/in-app-ads.md#mediation) 섹션에서 설정을 구성 하 여 **adcontrol** 에 대해 ad 중재를 사용 하도록 설정할 수 있습니다. Ad 중재를 통해 여러 ad 네트워크에서 광고를 표시 하 여 ad 수익과 앱 승격 기능을 최대화할 수 있습니다. 여기에는 Taboola 및 Smaato와 같은 다른 유료 ad 네트워크의 광고를 비롯 하 여 Microsoft 앱 판촉 캠페인에 대 한 광고 등이 있습니다.

3.  코드에서 테스트 ad 단위 값 (**applicationId** 및 **adunit Id**)을 파트너 센터에서 생성 한 라이브 값으로 바꿉니다.

4.  파트너 센터를 사용 하 여 스토어에 [앱을 제출](../publish/app-submissions.md) 합니다.

5.  파트너 센터에서 [광고 성능 보고서](../publish/advertising-performance-report.md) 를 검토 합니다.             

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>앱에서 여러 ad 컨트롤의 ad 단위 관리

단일 앱에서 여러 **Adcontrol** 개체를 사용할 수 있습니다. 예를 들어 앱의 각 페이지에서 다른 **adcontrol** 개체를 호스트할 수 있습니다. 이 시나리오에서는 각 컨트롤에 다른 ad 단위를 할당 하는 것이 좋습니다. 각 컨트롤에 대해 서로 다른 ad 단위를 사용 하면 [중재 설정을](../publish/in-app-ads.md#mediation) 개별적으로 구성 하 고 각 컨트롤에 대 한 개별 [보고 데이터](../publish/advertising-performance-report.md) 를 가져올 수 있습니다. 이를 통해 서비스는 앱에 제공 하는 광고를 더 효율적으로 최적화할 수 있습니다.

> [!IMPORTANT]
> 하나의 앱 에서만 각 ad 단위를 사용할 수 있습니다. 둘 이상의 앱에서 ad 단위를 사용 하는 경우 해당 ad 단위에 대 한 광고가 제공 되지 않습니다.

## <a name="related-topics"></a>관련 항목

* [배너 광고 지침](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [GitHub의 광고 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
* [앱에 대 한 ad 단위 설정](set-up-ad-units-in-your-app.md)
* [JavaScript에서 오류 처리 연습](error-handling-in-javascript-walkthrough.md)
