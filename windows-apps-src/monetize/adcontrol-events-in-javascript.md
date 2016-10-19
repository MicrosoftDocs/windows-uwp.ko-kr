---
author: mcleanbyron
ms.assetid: 2383296e-c3d7-4b49-bcd2-621391228fdb
description: "AdControl 클래스의 이벤트를 처리하는 방법을 알아봅니다."
title: "JavaScript의 AdControl 이벤트"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: d24030dfae92451924000ba4f1ac19cf6c4d4abe


---

# JavaScript에서 AdControl 이벤트




다음 예제에서는 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 클래스의 이벤트를 처리하는 방법을 보여 줍니다. 이러한 예제에서는 이전에 **AdControl** 이벤트에 이벤트 처리기를 할당했다고 가정합니다. 이 작업을 수행하는 방법에 대한 자세한 내용은 [HTML 속성 예제](html-properties-example.md)를 참조하세요.

Javascript에서는 **AdControl** 이벤트를 [MarkSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx) 함수로 묶어야 합니다. JavaScript에서 이벤트를 처리하는 방법에 대한 자세한 내용은 [기본 앱 코딩(HTML)](https://msdn.microsoft.com/library/windows/apps/hh780660.aspx#adding-event-handlers)을 참조하세요.

## 예제

``` syntax
WinJS.Utilities.markSupportedForProcessing(
window.myAdError = function (sender, msg) {
  // place code here for when there is an error serving an ad.
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes.
});

WinJS.Utilities.markSupportedForProcessing(
window.myAdRefreshed = function (sender) {
  // place code here that you wish to execute when the ad refreshes.
});

WinJS.Utilities.markSupportedForProcessing(
window.myAdEngagedChanged = function (sender) {
  if (true == sender.isEngaged) {
    // code here for when user engaged with ad, e.g. if a game, pause it.
  }
  else {
    // user no longer engaged with ad, include code to unpause.
  }
});
```

## 관련 항목

* [GitHub의 광고 샘플](http://aka.ms/githubads)
* [AdControl 오류 처리](adcontrol-error-handling.md)
* [RoutedEventArgs 클래스](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 



<!--HONumber=Aug16_HO3-->


