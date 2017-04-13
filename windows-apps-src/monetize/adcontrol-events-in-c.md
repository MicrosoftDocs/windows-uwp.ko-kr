---
author: mcleanbyron
ms.assetid: 2fba38c4-11be-4058-bfa3-5f979390791c
description: "AdControl 클래스의 이벤트를 처리하는 방법을 알아봅니다."
title: "C의 AdControl 이벤트#"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 광고, 광고, AdControl, 이벤트"
ms.openlocfilehash: a0b1130f8177166eef5b524ac4c4f1b065176eeb
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="adcontrol-events-in-c"></a>C\의 AdControl 이벤트# #  


다음 예제에서는 [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.erroroccurred.aspx), [AdRefreshed](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.adrefreshed.aspx) 및 [IsEngagedChanged](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.isengagedchanged.aspx)와 같은 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 이벤트의 기본 이벤트 처리기를 보여 줍니다. 이러한 예제에서는 XAML 코드의 이벤트에 이벤트 처리기를 이미 할당했다고 가정합니다. 이 작업을 수행하는 방법에 대한 자세한 내용은 [XAML 속성 예제](xaml-properties-example.md)를 참조하세요.

C#에서 이벤트를 처리하는 방법에 대한 자세한 내용은 [이벤트 및 라우트된 이벤트 개요(C#/VB/C++ 및 XAML을 사용하는 유니버설 Windows 앱)](http://msdn.microsoft.com/library/windows/apps/hh758286)를 참조하세요.

## <a name="examples"></a>예제

> [!div class="tabbedCodeSnippets"]
[!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MainPage.xaml.cs#EventHandlers)]

## <a name="related-topics"></a>관련 항목

* [GitHub의 광고 샘플](http://aka.ms/githubads)
* [AdControl 오류 처리](adcontrol-error-handling.md)
* [RoutedEventArgs 클래스](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 
