---
author: mcleanbyron
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: "JavaScript/HTML을 사용하여 중간 광고를 실행하는 방법을 알아봅니다."
title: "JavaScript의 중간 광고 샘플 코드"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 5317895c2f28166c2905d0d49c5e98da5567943a


---

# JavaScript의 중간 광고 샘플 코드




이 항목에서는 JavaScript/HTML을 사용하여 중간 광고를 실행하는 방법을 보여 줍니다. 이 코드를 사용하도록 프로젝트를 구성하는 방법을 보여 주는 단계별 지침은 [중간 광고](interstitial-ads.md)를 참조하세요. JavaScript/HTML 앱에 동영상 중간 광고를 추가하는 방법을 보여 주는 전체 샘플 프로젝트에 대해서는 [GitHub의 광고 샘플](http://aka.ms/githubads)을 참조하세요.

## 코드 예제


이 JavaScript/HTML 샘플에서는 중간 광고를 구현하는 UWP(유니버설 Windows 플랫폼) 앱의 코드를 보여 줍니다. 이 샘플에서는 JavaScript 범용 Windows 비어 있는 앱 템플릿을 사용하여 새 Visual Studio 2015 프로젝트를 만듭니다.

이 코드는 단추를 사용하여 중간 광고를 시작하는 이벤트를 발생합니다. Visual Studio에서 생성된 default.js 및 default.html은 수정되었으며 아래와 같습니다. 아래에 표시된 script.js 파일은 프로젝트의 **js** 폴더/경로 아래에 추가됩니다.

**applicationId** 및 **adUnitId** 할당의 텍스트를 작업 값으로 바꿉니다.

### script.js

``` syntax
(function () {
    "use strict";

    // Assign applicationId and adUnitId.
    // Test values. Replace test values with live values before submitting an app to the store.
    var applicationId = "d25517cb-12d4-4699-8bdc-52040c712cab";
    var adUnitId = "11389925";

    var intAd = null;

    window.startInterstitial = function () {
        writeText("<br>Interstitial Ads in JavaScript Windows 10 Apps");
        registerEvents();

        // We know the opportunity to show an ad is coming soon (because this silly game is
        // so short), so prepare the ad now. In a real app, you should request the interstitial
        // close to when you think it will be shown, but with enough advance time to make the
        // request and prepare the ad (say 30 seconds to a few minutes).
        prepareInterstitial();

        writeText("Press the Button to show the interstitial ad.");
    };

    var registerEvents = function () {
        button1.addEventListener("click", onButtonClick);
    };

    var onButtonClick = function (evt) {
        if (intAd && intAd.state !== MicrosoftNSJS.Advertising.InterstitialAdState.showing) {
            showInterstitial();
        }
    }

    var restart = function () {
        if (intAd) {
            intAd.dispose();
        }
        intAd = null;
        window.startInterstitial();
    };

    var clearText = function (msg) {
        description.innerHTML = "";
    };

    var writeText = function (msg) {
        description.innerHTML = description.innerHTML + msg + "<br>";
        description.scrollTop = description.scrollHeight;
    };

    var prepareInterstitial = function () {
        if (!intAd) {
            intAd = new MicrosoftNSJS.Advertising.InterstitialAd();

            intAd.onErrorOccurred = errorOccurredHandler;
            intAd.onAdReady = adReadyHandler;
            intAd.onCancelled = cancelledHandler;
            intAd.onCompleted = completedHandler;

            intAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.video, applicationId, adUnitId);
        }
    };

    var showInterstitial = function () {
        if (intAd && intAd.state === MicrosoftNSJS.Advertising.InterstitialAdState.ready) {
            intAd.show();
        } else {
            // No ad is available to show. Allow user to try again anyway
            clearText();
            writeText("<br>Unable to show an ad. Check the error log. You can try again.");
            restart();
        }
    };

    var errorOccurredHandler = function (sender, args) {
        console.log("error: " + args.errorMessage + " (" + args.errorCode + ")");
        if (!isPlaying) {
            clearText();
            writeText("<br>Unable to show an ad. Check the error log. You can try again.");
            restart();
        }
    };

    var adReadyHandler = function (sender) {
        console.log("ad ready");
    };

    var cancelledHandler = function (sender) {
        console.log("ad cancelled");
        writeText("<br>You must watch the entire ad to continue. <b>Press the Button to watch the

ad.</b>");
        intAd.dispose();
        intAd = null;
        prepareInterstitial();
    };

    var completedHandler = function (sender) {
        console.log("ad complete");
        clearText();
        writeText("<br>Thanks for watching the ad! You can try again!");
        restart();
    };

})();
```

### default.js

``` syntax
// For an introduction to the Blank template, see the following documentation:
// http://go.microsoft.com/fwlink/?LinkId=232509
(function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                // TODO: This application has been newly launched. Initialize your application here.
            } else {
                // TODO: This application was suspended and then terminated.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
            }

            startInterstitial();

            args.setPromise(WinJS.UI.processAll());
        }
    };

    app.oncheckpoint = function (args) {
        // TODO: This application is about to be suspended. Save any state that needs to persist across suspensions here.
        // You might use the WinJS.Application.sessionState object, which is automatically saved and restored across suspension.
        // If you need to complete an asynchronous operation before your application is suspended, call args.setPromise().
    };

    app.start();
})();
```

### default.html(Windows 10)

``` syntax
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>App3JSInterstitial</title>

    <!-- WinJS references -->
    <link href="WinJS/css/ui-dark.css" rel="stylesheet" />
    <script src="WinJS/js/base.js"></script>
    <script src="WinJS/js/ui.js"></script>

    <!-- App3JSInterstitial references -->
    <link href="/css/default.css" rel="stylesheet" />
    <script src="/js/default.js"></script>
    <script src="/js/script.js"></script>

    <!-- ad.js src for Windows 10 -->
    <script src="//Microsoft.Advertising.JS/ad.js"></script>
</head>
<body class="win-type-body">
    <button id="button1" onclick="onButtonClick" class="win-button">Interstitial</button>
    <div id="description" style="height:100%; overflow:auto"></div>
</body>
</html>
```

### default.html(Windows 8.x)

``` syntax
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>App3JSInterstitial</title>

    <!-- WinJS references -->
    <link href="WinJS/css/ui-dark.css" rel="stylesheet" />
    <script src="WinJS/js/base.js"></script>
    <script src="WinJS/js/ui.js"></script>

    <!-- App3JSInterstitial references -->
    <link href="/css/default.css" rel="stylesheet" />
    <script src="/js/default.js"></script>
    <script src="/js/script.js"></script>

    <!-- ad.js src for Windows 8.x -->
    <script src="//Microsoft.Advertising.JS/ads/ad.js"></script>
</head>
<body class="win-type-body">
    <button id="button1" onclick="onButtonClick" class="win-button">Interstitial</button>
    <div id="description" style="height:100%; overflow:auto"></div>
</body>
</html>
```

## 관련 항목

* [GitHub의 광고 샘플](http://aka.ms/githubads)

 



<!--HONumber=Aug16_HO3-->


