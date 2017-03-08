---
author: mcleanbyron
ms.assetid: 48a1ef86-8514-4af8-9c93-81e869d36de7
description: "JavaScript를 사용하여 프로그래밍 방식으로 **AdControl**을 만드는 방법을 알아봅니다."
title: "Javascript에서 AdControl 만들기"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 광고, 광고, AdControl, javascript"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: d2ceafb9781ca2b9cd640e65d9bb420f0bf37928
ms.lasthandoff: 02/07/2017

---

# <a name="create-an-adcontrol-in-javascript"></a>Javascript에서 AdControl 만들기




이 문서의 예제에서는 JavaScript를 사용하여 프로그래밍 방식으로 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)을 만드는 방법을 보여 줍니다. 이 문서에서는 **AdControl**를 사용하는 데 필요한 참조가 프로젝트에 이미 추가되었다고 가정합니다. JavaScript 대신 HTML 태그에서 **AdControl**을 만들고 초기화하는 방법에 대한 자세한 안내를 비롯한 자세한 내용은 [HTML 5 및 Javascript의 AdControl](adcontrol-in-html-5-and-javascript.md)을 참조하세요.

## <a name="html-div-for-an-adcontrol"></a>AdControl용 HTML div

**AdControl**은 광고를 표시하는 html 페이지에 **div**가 있어야 합니다. 아래 코드는 이러한 **div**의 예제를 제공합니다.

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl">
</div>
```

## <a name="javascript-for-creating-an-adcontrol"></a>AdControl을 만들기 위한 JavaScript

다음 예제에서는 HTML에서 ID가 **myAd**인 기존 **div**를 사용하고 있다고 가정합니다.

**app.onactivated** 함수에서 **AdControl**을 인스턴스화합니다.

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#DeclareAdControl)]

이 예제에서는 **myAdError**, **myAdRefreshed** 및 **myAdEngagedChanged**라는 이벤트 처리기 메서드를 이미 선언했다고 가정합니다.

>**참고**&nbsp;&nbsp;이 예제에 표시된 *applicationId* 및 *adUnitId* 값은 [테스트 모드 값](test-mode-values.md)입니다. 앱을 제출하기 전에 Windows 개발자 센터에서 [이러한 값을 라이브 값으로 교체](set-up-ad-units-in-your-app.md)해야 합니다.

이 코드를 사용해도 광고가 표시되지 않으면 **AdControl**을 포함하는 **div**에 **position:relative**의 특성을 삽입할 수 있습니다. 이렇게 하면 **IFrame**의 기본 설정이 재정의됩니다. 광고는 이 특성의 값으로 인해 표시되지 않는 경우가 아니면 올바르게 표시됩니다. 최대 30분 동안 새 광고 단위를 사용하지 못할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [GitHub의 광고 샘플](http://aka.ms/githubads)

 

 

