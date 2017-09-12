---
author: mcleanbyron
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: "Windows 10(UWP), Windows 8.1 또는 Windows Phone 8.1용 JavaScript/HTML 앱에서 AdControl 클래스를 사용하여 배너 광고를 표시하는 방법을 알아봅니다."
title: "HTML 5 및 JavaScript의 AdControl"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 광고, 광고, AdControl, 광고 관리, javascript, HTML"
ms.openlocfilehash: 44417516d773ea4faf103f6d4cdaf0bc8f290921
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/27/2017
---
# <a name="adcontrol-in-html-5-and-javascript"></a>HTML 5 및 JavaScript의 AdControl

이 연습에서는 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 클래스를 사용하여 Windows 10(UWP), Windows 8.1 또는 Windows Phone 8.1용 JavaScript/HTML 앱에서 배너 광고를 표시하는 방법을 보여 줍니다.

JavaScript/HTML 앱에 배너 광고를 추가하는 방법을 보여 주는 전체 샘플 프로젝트는 [GitHub의 광고 샘플](http://aka.ms/githubads)을 참조하세요.

## <a name="prerequisites"></a>필수 구성 요소


* UWP 앱의 경우 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp) 및 Visual Studio 2015 이상 릴리스를 설치합니다.
* Windows8.1 또는 Windows Phone 8.1 앱의 경우 [Microsoft Advertising SDK for Windows 및 Windows Phone 8.x](http://aka.ms/store-8-sdk)와 Visual Studio 2015 또는 Visual Studio 2013을 설치합니다.

> [!NOTE]
> Windows 10 SDK 10.0.14393 버전이나 이후의 Windows SDK 버전이 설치되어 있는 경우, UWP 앱에 배너 광고를 추가하고 싶다면 WinJS 라이브러리 또한 설치해야 합니다. 이전 Windows 10용 Windows SDK 버전에는 이 라이브러리가 포함되어 있었지만, 10.0.14393(1주년 업데이트) 버전부터는 라이브러리를 별도로 설치해야 합니다. WinJS를 설치하려면 [WinJS 다운로드](http://try.buildwinjs.com/download/GetWinJS/)를 참조하세요.

## <a name="code-development"></a>코드 개발

1. Visual Studio에서 프로젝트를 열거나 새 프로젝트를 만듭니다.

2. 프로젝트의 대상이 **모든 CPU**인 경우 아키텍처별 빌드 출력(예: **x86**)을 사용하도록 프로젝트를 업데이트합니다. 프로젝트의 대상이 **모든 CPU**인 경우 다음 단계에 따라 Microsoft Advertising 라이브러리에 대한 참조를 성공적으로 추가하지 못할 수 있습니다. 자세한 내용은 [프로젝트에서 모든 CPU를 대상으로 할 경우 발생하는 참조 오류](known-issues-for-the-advertising-libraries.md#reference_errors)를 참조하세요.

3.  **솔루션 탐색기** 창에서 **참조**를 마우스 오른쪽 단추로 클릭한 다음 **참조 추가...**를 선택합니다.

4.  **참조 관리자**에서 프로젝트 유형에 따라 다음 참조 중 하나를 선택합니다.

    -   UWP(유니버설 Windows 플랫폼) 프로젝트: **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for JavaScript**(버전 10.0) 옆의 확인란을 선택합니다.

    -   Windows 8.1 프로젝트: **Windows 8.1**을 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for Windows 8.1 Native(JS)** 옆의 확인란을 선택합니다.

    -   Windows Phone 8.1 프로젝트: **Windows Phone 8.1**을 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for Windows Phone 8.1 Native(JS)** 옆의 확인란을 선택합니다.

    ![javascriptaddreference](images/13-f7f6d6a6-161e-4f17-995d-1236d0b5d9f2.png)

5.  **참조 관리자**에서 확인을 클릭합니다.

6.  index.html 파일(또는 프로젝트에 해당하는 다른 html 파일)을 엽니다.

7.  **&lt;head&gt;** 섹션에서 default.css 및 main.js에 대한 프로젝트의 JavaScript 참조 다음에 ad.js에 대한 참조를 추가합니다.

    UWP 프로젝트에서 다음 코드를 추가합니다.

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    Windows 8.1 또는 Windows Phone 8.1 프로젝트에서 다음 코드를 추가합니다.

    ``` HTML
    <!-- Advertising required references -->
    <script src="/MSAdvertisingJS/ads/ad.js"></script>
    ```

    > [!NOTE]
    > 이 줄은 **&lt;head&gt;** 섹션에서 main.js를 포함한 후에 추가해야 합니다. 그러지 않으면 프로젝트를 빌드할 때 오류가 발생합니다. Windows 8.1 또는 Windows Phone 8.1 대상의 프로젝트인 경우 프로젝트의 기본 HTML 파일 이름이 index.html이 아닌 default.html이고 프로젝트의 기본 JavaScript 파일 이름이 main.js가 아닌 default.js입니다.

8.  default.html 파일(또는 프로젝트에 해당하는 다른 html 파일)의 **&lt;body&gt;** 섹션을 **AdControl**에 대한 div를 포함하도록 수정합니다. **AdControl**에서 [테스트 모드 값](test-mode-values.md)에 제공된 테스트 값에 **applicationId**와 **adUnitId** 속성을 할당합니다. 또한 [배너 광고에 지원되는 광고 크기](supported-ad-sizes-for-banner-ads.md) 중 하나가 되도록 컨트롤의 높이와 너비를 조정합니다.

    > [!NOTE]
    > 모든 **AdControl**에는 컨트롤할 광고를 지원하는 서비스가 사용하는 *광고 단위*가 있고, 모든 광고 단위는 *광고 단위 ID*와 *응용 프로그램 ID*로 구성되어 있습니다. 이 단계에서 컨트롤에 테스트 광고 단위 ID와 응용 프로그램 ID 값을 할당하세요. 이 테스트 값은 앱 테스트 버전에서만 사용할 수 있습니다. 앱을 스토어에 게시하기 전, Windows 개발자 센터에서 [이 테스트 값을 라이브 값으로](#release) 변경하세요.

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  앱을 컴파일하고 실행하여 광고와 함께 표시합니다.

다음 예제는 간단한 UWP 앱의 완전한 index.html을 보여 줍니다.

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

<span id="release" />
## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>Windows 개발자 센터를 사용하여 라이브 광고와 함께 앱 출시

1.  개발자 센터 대시보드에서 [앱의 광고 수익 창출](../publish/monetize-with-ads.md) 페이지로 이동한 후 [광고 단위를 만듭니다](../monetize/set-up-ad-units-in-your-app.md). 광고 단위 유형으로 **배너**를 지정합니다. 광고 단위 ID와 응용 프로그램 ID를 기록합니다.

2. 앱이 Windows 10용 UWP 앱인 경우, [광고 수익 창출](../publish/monetize-with-ads.md) 페이지의 [광고 조정](../publish/monetize-with-ads.md#mediation) 섹션에서 설정을 구성하여 **AdControl**에 대한 광고 조정을 활성화할 수 있습니다. 광고 조정을 통해 Taboola 및 Smaato 같은 기타 유료 광고 네트워크와 Microsoft 앱 프로모션 캠페인에 대한 광고를 포함하여 여러 광고 네트워크의 광고를 표시하여 광고 수익과 앱 프로모션 기능을 최대화할 수 있습니다.

3.  코드에서 테스트 광고 단위 값(**applicationId** 및 **adUnitId**)을 개발자 센터에서 생성한 라이브 값으로 바꿉니다.

4.  개발자 센터 대시보드를 사용하여 스토어에 [앱을 제출](../publish/app-submissions.md)합니다.

5.  개발자 센터 대시보드에서 [광고 성과 보고서](../publish/advertising-performance-report.md)를 검토합니다.             

<span id="manage" />
## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>앱에서 여러 광고 컨트롤에 대한 광고 단위 관리

단일 앱에서 여러 **AdControl** 개체를 사용할 수 있습니다(예를 들어, 앱의 각 페이지를 여러 **AdControl** 개체에 호스트할 수 있습니다). 이 경우, 각 컨트롤에 다른 광고 단위를 지정하는 것이 좋습니다. 각 컨트롤에 다른 광고 단위를 사용하면 별도로 [미디어 설정을 구성](../publish/monetize-with-ads.md#mediation)할 수 있고, 각 컨트롤에 별개의 [보고 데이터](../publish/advertising-performance-report.md)를 생성할 수 있습니다. 또 이렇게 하면 서비스가 지원하는 앱에서 더 효과적으로 광고를 최적화할 수 있습니다.

> [!IMPORTANT]
> 앱별로 하나의 광고 단위만 사용할 수 있습니다. 특정 광고 단위를 하나 이상의 앱에 사용하면, 해당 광고 단위에 광고가 제공되지 않습니다.

## <a name="related-topics"></a>관련 항목

* [GitHub의 광고 샘플](http://aka.ms/githubads)
* [앱에서 광고 단위 설정](../monetize/set-up-ad-units-in-your-app.md)
