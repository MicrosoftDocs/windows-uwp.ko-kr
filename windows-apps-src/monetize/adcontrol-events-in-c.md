---
author: mcleanbyron
ms.assetid: 2fba38c4-11be-4058-bfa3-5f979390791c
description: "AdControl 클래스의 이벤트를 처리하는 방법을 알아봅니다."
title: "C의 AdControl 이벤트#"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: cc5dfbfacefb4be32153f3266878be41a3b923f8

---

# C\의 AdControl 이벤트# #  


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

다음 예제에서는 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 클래스의 이벤트를 처리하는 방법을 보여 줍니다. 이러한 예제에서는 이전에 XAML의 **AdControl** 이벤트에 이벤트 처리기를 할당했다고 가정합니다. 이 작업을 수행하는 방법에 대한 자세한 내용은 [XAML 속성 예제](xaml-properties-example.md)를 참조하세요.

C#에서 이벤트를 처리하는 방법에 대한 자세한 내용은 [이벤트 및 라우트된 이벤트 개요(C#/VB/C++ 및 XAML을 사용하는 유니버설 Windows 앱)](http://msdn.microsoft.com/library/windows/apps/hh758286)를 참조하세요.

## 예제


``` syntax
private void OnAdError(object sender, AdErrorEventArgs e) {
  // place code here for when there is an error serving an ad
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes
  return;
}

private void OnAdRefresh(object sender, RoutedEventArgs e) {
  // place code here that you wish to execute when the ad refreshes.
 return;
}

private void OnAdEngagedChanged(object sender, RoutedEventArgs e) {
  // place code here for when there is an error serving an ad
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes
  return;
}
```

## 관련 항목

* [GitHub의 광고 샘플](http://aka.ms/githubads)
* [AdControl 오류 처리](adcontrol-error-handling.md)
* [RoutedEventArgs 클래스](http://msdn.microsoft.com/en-us/library/system.windows.routedeventargs.aspx)

 

 



<!--HONumber=Jun16_HO4-->


