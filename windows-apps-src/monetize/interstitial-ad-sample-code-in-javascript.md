---
author: Xansky
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: JavaScript/HTML을 사용하여 중간 광고를 실행하는 방법을 알아봅니다.
title: JavaScript의 중간 광고 샘플 코드
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, 광고, 중간, javascript, 샘플 코드
ms.localizationpriority: medium
ms.openlocfilehash: 42ff810808e0e7b8d83152f7d23e535c721d3405
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5823016"
---
# <a name="interstitial-ad-sample-code-in-javascript"></a>JavaScript의 중간 광고 샘플 코드

이 항목에서는 중간 광고를 게재하는 기본 JavaScript 및 HTML UWP(유니버설 Windows 플랫폼) 앱의 전체 샘플 코드를 제공합니다. 이 코드를 사용하도록 프로젝트를 구성하는 방법을 보여 주는 단계별 지침은 [중간 광고](interstitial-ads.md)를 참조하세요. 전체 샘플 프로젝트는 [GitHub의 광고 샘플](http://aka.ms/githubads)을 참조하세요.

## <a name="code-example"></a>코드 예제

이 섹션에서는 중간 광고를 게재하는 기본 앱의 HTML 및 JavaScript 파일 콘텐츠를 보여 줍니다. 이러한 예제를 사용하려면 이 코드를 Visual Studio의 JavaScript**WinJS 앱(유니버설 Windows)** 프로젝트로 복사합니다.

이 샘플 앱은 2개의 버튼을 사용하여 중간 광고를 요청한 다음 실행합니다. Visual Studio에서 생성된 main.js 및 index.html은 수정되었으며 아래와 같습니다. 아래의 script.js 파일에는 샘플의 코드 대부분이 포함되어 있으므로 이 파일을 프로젝트의 **js** 폴더에 추가해야 합니다.

앱을 Microsoft Store에 제출하기 전에 Windows 개발자 센터에서 ```applicationId``` 및 ```adUnitId``` 변수의 값을 라이브 값으로 바꿉니다. 자세한 내용은 [앱에서 광고 단위 설정](set-up-ad-units-in-your-app.md#live-ad-units)을 참조하세요.

> [!NOTE]
> 동영상 중간 광고 대신 배너 중간 광고를 표시하도록 이 예제를 변경하려면 [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 메서드의 첫 번째 매개 변수에 **InterstitialAdType.video** 대신 **InterstitialAdType.display** 값을 전달하세요. 자세한 내용은 [중간 광고](interstitial-ads.md)를 참조하세요.

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
[!code-html[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/index.html#L1-L21)]

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#script)]

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/main.js#main)]

## <a name="related-topics"></a>관련 항목

* [GitHub의 광고 샘플](http://aka.ms/githubads)

 
