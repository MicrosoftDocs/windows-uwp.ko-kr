---
author: mcleanbyron
ms.assetid: 48a1ef86-8514-4af8-9c93-81e869d36de7
description: "JavaScript를 사용하여 프로그래밍 방식으로 **AdControl**을 만드는 방법을 알아봅니다."
title: "Javascript에서 AdControl 만들기"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 68bc124aea079bc60fa22e1e6a038caf95fe765c


---

# Javascript에서 AdControl 만들기




이 예제에서는 JavaScript를 사용하여 프로그래밍 방식으로 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)을 만드는 방법을 보여 줍니다.

## AdControl에 대한 HTML div

**AdControl**은 광고를 표시하는 html 페이지에 **div**가 있어야 합니다. 아래 코드는 이러한 **div**의 예제를 제공합니다.

``` syntax
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl">
</div>
```

## AdControl을 만들기 위한 JavaScript

다음 예제에서는 HTML에서 ID가 **myAd**인 기존 **div**를 사용하고 있다고 가정합니다.

**app.onactivated** 함수에서 **AdControl**을 인스턴스화합니다.

``` syntax
// TODO: This application has been newly launched. Initialize
// your application here.
var adDiv = document.getElementById("myAd");
var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
        applicationId: "3f83fe91-d6be-434d-a0ae-7351c5a997f1",
        adUnitId: "10865270"
    });
myAdControl.isAutoRefreshEnabled = false;
myAdControl.onErrorOccurred = myAdError;
myAdControl.onAdRefreshed = myAdRefreshed;
myAdControl.onEngagedChanged = myAdEngagedChanged;
```

값은 예제로 제공된 것입니다. 코드에서 이러한 함수 및 속성의 값을 앱에 적절하게 설정합니다.

이 코드를 사용해도 광고가 표시되지 않으면 **AdControl**을 포함하는 **div**에 **position:relative**의 특성을 삽입할 수 있습니다. 이렇게 하면 **IFrame**의 기본 설정이 재정의됩니다. 광고는 이 특성의 값으로 인해 표시되지 않는 경우가 아니면 올바르게 표시됩니다. 최대 30분 동안 새 광고 단위를 사용하지 못할 수 있습니다.

## 관련 항목

* [GitHub의 광고 샘플](http://aka.ms/githubads)

 

 



<!--HONumber=Aug16_HO3-->


