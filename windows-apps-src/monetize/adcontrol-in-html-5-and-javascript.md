---
author: mcleanbyron
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: Windows 10(UWP), Windows 8.1 또는 Windows Phone 8.1용 JavaScript/HTML 앱에서 AdControl 클래스를 사용하여 배너 광고를 표시하는 방법을 알아봅니다.
title: HTML 5 및 Javascript의 AdControl
---

# HTML 5 및 Javascript의 AdControl


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 연습에서는 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 클래스를 사용하여 Windows 10(UWP), Windows 8.1 또는 Windows Phone 8.1용 JavaScript/HTML 앱에서 배너 광고를 표시하는 방법을 보여 줍니다. 이 연습에서는 **AdMediatorControl** 또는 광고 조정을 사용하지 않습니다.

JavaScript/HTML 앱에 배너 광고를 추가하는 방법을 보여 주는 전체 샘플 프로젝트에 대해서는 [GitHub의 광고 샘플](http://aka.ms/githubads)을 참조하세요.

## 필수 조건


* Visual Studio 2015 또는 Visual Studio 2013과 함께 [Microsoft 스토어 참여 및 수익 창출 SDK](http://aka.ms/store-em-sdk)를 설치합니다.

> **참고** 나중에 Visual Studio 2015를 사용하여 Windows 10 Anniversary SDK Preview 빌드 14295 이상을 설치한 경우 WinJS 라이브러리도 설치해야 합니다. 이 라이브러리는 Windows 10용 Windows SDK 이전 버전에는 포함되었지만 Windows 10 Anniversary SDK Preview 빌드 14295부터는 별도로 설치해야 합니다. WinJS를 설치하려면 [WinJS 다운로드](http://try.buildwinjs.com/download/GetWinJS/)를 참조하세요.

## 코드 개발

1. Visual Studio에서 프로젝트를 열거나 새 프로젝트를 만듭니다.

2. 프로젝트의 대상이 **모든 CPU**인 경우 아키텍처별 빌드 출력(예: **x86**)을 사용하도록 프로젝트를 업데이트합니다. 프로젝트의 대상이 **모든 CPU**인 경우 다음 단계에 따라 Microsoft Advertising 라이브러리에 대한 참조를 성공적으로 추가하지 못할 수 있습니다. 자세한 내용은 [프로젝트에서 모든 CPU를 대상으로 할 경우 발생하는 참조 오류](known-issues-for-the-advertising-libraries.md#reference_errors)를 참조하세요.

3.  **솔루션 탐색기** 창에서 **참조**를 마우스 오른쪽 단추로 클릭한 다음 **참조 추가...**를 선택합니다.

4.  **참조 관리자**에서 프로젝트 유형에 따라 다음 참조 중 하나를 선택합니다.

    -   UWP(유니버설 Windows 플랫폼) 프로젝트: **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for JavaScript**(버전 10.0) 옆의 확인란을 선택합니다.

    -   Windows 8.1 프로젝트: **Windows 8.1**을 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for Windows 8.1 Native(JS)** 옆의 확인란을 선택합니다.

    -   Windows 8.1 프로젝트: **Windows Phone 8.1**을 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for Windows Phone 8.1 Native(JS)** 옆의 확인란을 선택합니다.

    ![javascriptaddreference](images/13-f7f6d6a6-161e-4f17-995d-1236d0b5d9f2.png)

    > **참고** 이 이미지는 Visual Studio 2015에서 Windows 10용 UWP 프로젝트를 빌드하는 방법을 보여 줍니다. Windows 8.1 또는 Windows Phone 8.1 앱을 빌드하거나 Visual Studio 2013을 사용하는 경우 화면이 다르게 나타납니다.

5.  **참조 관리자**에서 확인을 클릭합니다.

6.  default.html 파일(또는 프로젝트에 해당하는 다른 html 파일)을 엽니다.

7.  **
            &lt;head&gt;** 섹션에서 default.css 및 default.js에 대한 프로젝트의 JavaScript 참조 다음에 ad.js에 대한 참조를 추가합니다.

    UWP 프로젝트에서 다음 코드를 추가합니다.

    ``` syntax
    <!-- Microsoft advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    Windows 8.1 또는 Windows Phone 8.1 프로젝트에서 다음 코드를 추가합니다.

    ``` syntax
    <!-- Microsoft advertising required references -->
    <script src="/MSAdvertisingJS/ads/ad.js"></script>
    ```

    > **참고** 이 줄은 **&lt;head&gt;** 섹션에서 default.js를 포함한 후에 추가해야 합니다. 그러지 않으면 프로젝트를 빌드할 때 오류가 발생합니다.

8.  default.html 파일(또는 프로젝트에 해당하는 다른 html 파일)의 **&lt;body&gt;** 섹션을 **AdControl**에 대한 div를 포함하도록 수정합니다. **AdControl**의 **applicationId** 및 **adUnitId** 속성을 [테스트 모드 값](test-mode-values.md)에 제공된 테스트 값에 할당하고 컨트롤의 너비와 높이를 [배너 광고에 대해 지원되는 광고 크기](supported-ad-sizes-for-banner-ads.md) 중 하나로 조정합니다.

    > **참고**  
    앱을 제출하기 전에 테스트 **applicationId** 및 **adUnitId** 값을 라이브 값으로 바꿉니다.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
          data-win-control="MicrosoftNSJS.Advertising.AdControl"
          data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
    </div>
    ```

9.  앱을 컴파일하고 실행하여 광고와 함께 표시합니다.

## Windows 개발자 센터를 사용하여 라이브 광고와 함께 앱 출시


1.  개발자 센터 대시보드에서 앱의 **수익 창출**&gt;**광고로 수익 창출** 페이지로 이동한 후 [독립 실행형 Microsoft 광고 단위를 만듭니다](../publish/monetize-with-ads.md). 광고 단위 유형으로 **배너**를 지정합니다. 광고 단위 ID와 응용 프로그램 ID를 적어둡니다.

2.  코드에서 테스트 광고 단위 값(**applicationId** 및 **adUnitId**)을 개발자 센터에서 생성한 라이브 값으로 바꿉니다.

3.  개발자 센터 대시보드를 사용하여 스토어에 [앱을 제출](../publish/app-submissions.md)합니다.

4.  개발자 센터 대시보드에서 [광고 성과 보고서](../publish/advertising-performance-report.md)를 검토합니다.

## 샘플 UWP 프로젝트에 대한 default.html 완료


``` syntax
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>My_Windows_10_Ad_Funded_JavaScript_App</title>

    <!-- WinJS references -->
    <link href="//Microsoft.WinJS.2.0.Preview/css/ui-dark.css" rel="stylesheet" />
    <script src="//Microsoft.WinJS.2.0.Preview/js/base.js"></script>
    <script src="//Microsoft.WinJS.2.0.Preview/js/ui.js"></script>

    <!-- My_Windows_10_Ad_Funded_JavaScript_App references -->
    <link href="/css/default.css" rel="stylesheet" />
    <script src="/js/default.js"></script>

    <!-- Microsoft advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
</head>
<body>
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

## 관련 항목

* [GitHub의 광고 샘플](http://aka.ms/githubads)
 

 


<!--HONumber=May16_HO2-->


