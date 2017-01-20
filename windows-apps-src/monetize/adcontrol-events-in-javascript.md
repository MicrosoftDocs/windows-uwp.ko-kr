---
author: mcleanbyron
ms.assetid: 2383296e-c3d7-4b49-bcd2-621391228fdb
description: "AdControl 클래스의 이벤트를 처리하는 방법을 알아봅니다."
title: "JavaScript의 AdControl 이벤트"
translationtype: Human Translation
ms.sourcegitcommit: f88a71491e185aec84a86248c44e1200a65ff179
ms.openlocfilehash: e652fe6b5f295c0f4b4808e5a4605c13fdcfea68

---

# <a name="adcontrol-events-in-javascript"></a>JavaScript의 AdControl 이벤트

다음 예제에서는 [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.erroroccurred.aspx), [AdRefreshed](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.adrefreshed.aspx) 및 [IsEngagedChanged](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.isengagedchanged.aspx)와 같은 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 이벤트의 기본 이벤트 처리기를 보여 줍니다. 이러한 예제에서는 HTML 태그의 이벤트에 이벤트 처리기를 이미 할당했다고 가정합니다. 이 작업을 수행하는 방법에 대한 자세한 내용은 [HTML 속성 예제](html-properties-example.md)를 참조하세요.

JavaScript에서는 **AdControl** 이벤트를 [MarkSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx) 함수로 묶어야 합니다. JavaScript에서 이벤트를 처리하는 방법에 대한 자세한 내용은 [기본 앱 코딩(HTML)](https://msdn.microsoft.com/library/windows/apps/hh780660.aspx#adding-event-handlers)을 참조하세요.

## <a name="examples"></a>예제

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#EventHandlers)]

## <a name="related-topics"></a>관련 항목

* [GitHub의 광고 샘플](http://aka.ms/githubads)
* [AdControl 오류 처리](adcontrol-error-handling.md)
* [RoutedEventArgs 클래스](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 



<!--HONumber=Dec16_HO2-->


