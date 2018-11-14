---
author: Xansky
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: Windows10(UWP)용 JavaScript/HTML 앱에서 AdControl 클래스를 사용하여 배너 광고를 표시하는 방법을 알아봅니다.
title: HTML 5 및 JavaScript의 AdControl
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, AdControl, 광고 관리, javascript, HTML
ms.localizationpriority: medium
ms.openlocfilehash: df5623b8c73dc6c96c2869156d22da64f6a6b58d
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6463529"
---
# <a name="adcontrol-in-html-5-and-javascript"></a>HTML 5 및 JavaScript의 AdControl

이 연습에서는 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 클래스를 사용하여 Windows 10용 UWP(유니버설 Windows 플랫폼) JavaScript/HTML 앱에서 배너 광고를 표시하는 방법을 보여줍니다.

JavaScript/HTML 앱에 배너 광고를 추가하는 방법을 보여 주는 전체 샘플 프로젝트는 [GitHub의 광고 샘플](http://aka.ms/githubads)을 참조하세요.

## <a name="prerequisites"></a>필수 구성 요소

* Visual Studio 2015 이상 릴리스와 함께 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)를 설치합니다. 설치 지침은 [이 문서](install-the-microsoft-advertising-libraries.md)를 참조하세요.

> [!NOTE]
> Windows 10 SDK 버전 10.0.14393 (1 주년 업데이트) 또는 이후 버전의 Windows SDK를 설치한 경우 [WinJS](https://github.com/winjs/winjs) 라이브러리도 설치 해야 합니다. 이전 Windows 10용 Windows SDK 버전에는 이 라이브러리가 포함되어 있었지만, 10.0.14393(1주년 업데이트) 버전부터는 라이브러리를 별도로 설치해야 합니다. 

## <a name="integrate-a-banner-ad-into-your-app"></a>앱에 배너 광고를 통합

1. Visual Studio에서 프로젝트를 열거나 새 프로젝트를 만듭니다.

    > [!NOTE]
    > 기존 프로젝트를 사용하고 있는 경우 프로젝트에서 Package.appxmanifest 파일을 열고 **인터넷(클라이언트)** 기능이 선택되었는지 확인합니다. 앱에서 테스트 광고와 라이브 광고를 수신하려면 이 기능이 필요합니다.

2. 프로젝트의 대상이 **모든 CPU**인 경우 아키텍처별 빌드 출력(예: **x86**)을 사용하도록 프로젝트를 업데이트합니다. 프로젝트의 대상이 **모든 CPU**인 경우 다음 단계에 따라 Microsoft Advertising 라이브러리에 대한 참조를 성공적으로 추가하지 못할 수 있습니다. 자세한 내용은 [프로젝트에서 모든 CPU를 대상으로 할 경우 발생하는 참조 오류](known-issues-for-the-advertising-libraries.md#reference_errors)를 참조하세요.

3. 프로젝트에 Microsoft Advertising SDK 참조를 추가합니다.

    1. **솔루션 탐색기** 창에서 **참조**를 마우스 오른쪽 단추로 클릭한 다음 **참조 추가**를 선택합니다.
    2.  **참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for JavaScript**(버전 10.0) 옆의 확인란을 선택합니다.
    3.  **참조 관리자**에서 확인을 클릭합니다.

6.  index.html 파일(또는 프로젝트에 해당하는 다른 html 파일)을 엽니다.

7.  **&lt;head&gt;** 섹션에서 default.css 및 main.js에 대한 프로젝트의 JavaScript 참조 다음에 ad.js에 대한 참조를 추가합니다.

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    > [!NOTE]
    > 이 줄은 **&lt;head&gt;** 섹션에서 main.js를 포함한 후에 추가해야 합니다. 그러지 않으면 프로젝트를 빌드할 때 오류가 발생합니다.

8.  default.html 파일(또는 프로젝트에 해당하는 다른 html 파일)의 **&lt;body&gt;** 섹션을 **AdControl**에 대한 **div**를 포함하도록 수정합니다. [테스트 광고 단위 값](set-up-ad-units-in-your-app.md#test-ad-units)에 **AdControl**의 **applicationId**와 **adUnitId** 속성을 할당합니다. 또한 [배너 광고에 지원되는 광고 크기](supported-ad-sizes-for-banner-ads.md) 중 하나가 되도록 컨트롤의 **높이**와 **너비**를 조정합니다.

    > [!NOTE]
    > 모든 **AdControl**에는 컨트롤할 광고를 지원하는 서비스가 사용하는 *광고 단위*가 있고, 모든 광고 단위는 *광고 단위 ID*와 *응용 프로그램 ID*로 구성되어 있습니다. 이 단계에서 컨트롤에 테스트 광고 단위 ID와 응용 프로그램 ID 값을 할당하세요. 이 테스트 값은 앱 테스트 버전에서만 사용할 수 있습니다. 해야 스토어에 앱을 게시 하기 전에 [대체 이러한 테스트 값을 라이브 값](#release) 파트너 센터에서.

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  앱을 컴파일하고 실행하여 광고와 함께 표시합니다.

다음 예제는 간단한 앱의 완전한 index.html을 보여줍니다.

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

이전 단계는 HTML 태그에서 **AdControl**을 선언하는 방법을 보여줍니다. 또는 JavaScript를 사용하여 프로그래밍 방식으로 **AdControl**을 만들 수 있습니다. 이 예제에서는 HTML에서 ID가 **myAd**인 기존 **div**를 사용하고 있다고 가정합니다.

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#DeclareAdControl)]

이 예제에서는 **myAdError**, **myAdRefreshed** 및 **myAdEngagedChanged**라는 이벤트 처리기 메서드를 이미 선언했다고 가정합니다.

이 코드를 사용해도 광고가 표시되지 않으면 **AdControl**을 포함하는 **div**에 **position:relative**의 특성을 삽입할 수 있습니다. 이렇게 하면 **IFrame**의 기본 설정이 재정의됩니다. 광고는 이 특성의 값으로 인해 표시되지 않는 경우가 아니면 올바르게 표시됩니다. 최대 30분 동안 새 광고 단위를 사용하지 못할 수 있습니다.

> [!NOTE]
> 이 예제에 표시된 *applicationId* 및 *adUnitId* 값은 [테스트 모드 값](set-up-ad-units-in-your-app.md#test-ad-units)입니다. 제출에 대 한 앱을 제출 하기 전에 파트너 센터에서 [이러한 값을 라이브 값을 대체](set-up-ad-units-in-your-app.md#live-ad-units) 을 해야 합니다.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>라이브 광고와 앱 릴리스

1. 앱에 사용할 배너 광고는 [배너 광고에 대한 지침](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)을 따라야 합니다.

1.  파트너 센터에서 이동 [인 앱 광고](../publish/in-app-ads.md) 페이지와 [광고 단위를 생성](set-up-ad-units-in-your-app.md#live-ad-units)합니다. 광고 단위 유형으로 **배너**를 지정합니다. 광고 단위 ID와 응용 프로그램 ID를 적어둡니다.
    > [!NOTE]
    > 테스트 광고 단위와 라이브 UWP 광고 단위의 응용 프로그램 ID 값은 형식이 서로 다릅니다. 테스트 응용 프로그램 ID 값은 GUID입니다. 파트너 센터에서 라이브 UWP 광고 단위를 만들 때 광고 단위에 대 한 응용 프로그램 ID 값에 항상 (예: Store ID 값은 9NBLGGH4R315와) 앱에 대 한 스토어 ID와 일치 합니다.

2. [인앱 광고](../publish/in-app-ads.md) 페이지의 [조정 설정](../publish/in-app-ads.md#mediation) 섹션에서 설정을 구성하여, **AdControl**의 광고 조정을 사용하는 방법도 있습니다. 광고 조정을 통해 Taboola 및 Smaato 같은 기타 유료 광고 네트워크와 Microsoft 앱 프로모션 캠페인에 대한 광고를 포함하여 여러 광고 네트워크의 광고를 표시하여 광고 수익과 앱 프로모션 기능을 최대화할 수 있습니다.

3.  코드에서 테스트 광고 단위 값을 (**applicationId** 및 **adUnitId**) 파트너 센터에서 생성 한 라이브 값으로 바꿉니다.

4.  파트너 센터를 사용 하 여 스토어에 [앱 제출](../publish/app-submissions.md)

5.  파트너 센터에서 [광고 성과 보고서](../publish/advertising-performance-report.md) 를 검토 합니다.             

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>앱에서 여러 광고 컨트롤에 대한 광고 단위 관리

단일 앱에서 여러 **AdControl** 개체를 사용할 수 있습니다(예를 들어, 앱의 각 페이지를 여러 **AdControl** 개체에 호스트할 수 있습니다). 이 경우, 각 컨트롤에 다른 광고 단위를 지정하는 것이 좋습니다. 각 컨트롤에 다른 광고 단위를 사용하면 별도로 [조정 설정을 구성](../publish/in-app-ads.md#mediation)할 수 있고, 각 컨트롤에 대해 별개의 [보고 데이터](../publish/advertising-performance-report.md)를 가져올 수 있습니다. 또 이렇게 하면 서비스가 지원하는 앱에서 더 효과적으로 광고를 최적화할 수 있습니다.

> [!IMPORTANT]
> 앱별로 하나의 광고 단위만 사용할 수 있습니다. 특정 광고 단위를 하나 이상의 앱에 사용하면, 해당 광고 단위에 광고가 제공되지 않습니다.

## <a name="related-topics"></a>관련 항목

* [배너 광고에 대한 지침](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [GitHub의 광고 샘플](http://aka.ms/githubads)
* [앱에서 광고 단위 설정](set-up-ad-units-in-your-app.md)
* [JavaScript에서 오류 처리 연습](error-handling-in-javascript-walkthrough.md)
