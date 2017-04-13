---
author: mcleanbyron
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: "JavaScript/HTML을 사용하여 중간 광고를 실행하는 방법을 알아봅니다."
title: "JavaScript의 중간 광고 샘플 코드"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 광고, 중간, javascript, 샘플 코드"
ms.openlocfilehash: 192ea42e9d55bbafcd6c6dbd463681832ac28c86
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="interstitial-ad-sample-code-in-javascript"></a>JavaScript의 중간 광고 샘플 코드

이 항목에서는 중간 광고를 게재하는 기본 JavaScript 및 HTML UWP(유니버설 Windows 플랫폼) 앱의 전체 샘플 코드를 제공합니다. 이 코드를 사용하도록 프로젝트를 구성하는 방법을 보여 주는 단계별 지침은 [중간 광고](interstitial-ads.md)를 참조하세요. 전체 샘플 프로젝트는 [GitHub의 광고 샘플](http://aka.ms/githubads)을 참조하세요.

## <a name="code-example"></a>코드 예제

이 섹션에서는 중간 광고를 게재하는 기본 앱의 HTML 및 JavaScript 파일 콘텐츠를 보여 줍니다. 이들 예제를 사용하려면 이 코드를 Visual Studio 2015의 JavaScript**WinJS 앱(유니버설 Windows)** 프로젝트로 복사합니다.

이 샘플 앱은 2개의 버튼을 사용하여 중간 광고를 요청한 다음 실행합니다. Visual Studio에서 생성된 main.js 및 index.html은 수정되었으며 아래와 같습니다. 아래의 script.js 파일에는 샘플의 코드 대부분이 포함되어 있으므로 이 파일을 프로젝트의 **js** 폴더에 추가해야 합니다.

>**참고(Windows 8.x 및 Windows Phone 8.1의 경우)**&nbsp;&nbsp;Windows 8.1 또는 Windows Phone 8.1 대상의 프로젝트인 경우 프로젝트의 기본 HTML 파일 이름이 index.html이 아닌 default.html이고 프로젝트의 기본 JavaScript 파일 이름이 main.js가 아닌 default.js입니다.

앱을 스토어에 제출하기 전에 ```applicationId``` 및 ```adUnitId``` 변수의 값을 Windows 개발자 센터에서 라이브 값으로 바꿉니다. 자세한 내용은 [앱에서 광고 단위 설정](set-up-ad-units-in-your-app.md)을 참조하세요.

>**참고**&nbsp;&nbsp;동영상 중간 광고 대신 배너 중간 광고를 표시하도록 이 예제를 변경하려면  [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 메서드의 첫 번째 매개 변수에 **InterstitialAdType.video** 대신 **InterstitialAdType.display** 값을 전달하세요. 자세한 내용은 [중간 광고](interstitial-ads.md)를 참조하세요.

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
[!code-html[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/index.html#L1-L21)]

<span/>
>**참고(Windows 8.x 및 Windows Phone 8.1의 경우)**&nbsp;&nbsp;Windows 8.1 또는 Windows Phone 8.1 대상의 프로젝트인 경우 예제의 ```<script src="//Microsoft.Advertising.JavaScript/ad.js"></script>``` 줄을 ```<script src="/MSAdvertisingJS/ads/ad.js"></script>```로 바꿉니다.

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#script)]

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/main.js#main)]

## <a name="related-topics"></a>관련 항목

* [GitHub의 광고 샘플](http://aka.ms/githubads)

 
