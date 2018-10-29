---
author: Xansky
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: C#을 사용하여 중간 광고를 실행하는 방법을 알아봅니다.
title: C#의 중간 광고 샘플 코드
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, 광고, 중간, c#, 샘플 코드
ms.localizationpriority: medium
ms.openlocfilehash: 94baaa490859baf57e0bd12bc5a3800ed3b2312a
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5759100"
---
# <a name="interstitial-ad-sample-code-in-c"></a>C\#의 중간 광고 샘플 코드 #  

이 항목에서는 동영상 중간 광고를 게재하는 기본 C# 및 XAML UWP(유니버설 Windows 플랫폼) 앱의 전체 샘플 코드를 제공합니다. 이 코드를 사용하도록 프로젝트를 구성하는 방법을 보여 주는 단계별 지침은 [중간 광고](interstitial-ads.md)를 참조하세요. 전체 샘플 프로젝트는 [GitHub의 광고 샘플](http://aka.ms/githubads)을 참조하세요.

## <a name="code-example"></a>코드 예제

이 섹션에서는 중간 광고를 게재하는 기본 앱의 MainPage.xaml 및 MainPage.xaml.cs 파일 콘텐츠를 보여 줍니다. 이러한 예제를 사용하려면 코드를 Visual Studio에서 Visual C# **빈 앱(유니버설 Windows)** 프로젝트에 복사합니다.

이 샘플 앱은 2개의 버튼을 사용하여 중간 광고를 요청한 다음 실행합니다. 앱을 스토어에 제출하기 전에 Windows 개발자 센터에서 ```myAppId``` 및 ```myAdUnitId``` 필드의 값을 라이브 값으로 바꿉니다. 자세한 내용은 [앱에서 광고 단위 설정](set-up-ad-units-in-your-app.md#live-ad-units)을 참조하세요.

> [!NOTE]
> 동영상 중간 광고 대신 배너 중간 광고를 표시하도록 이 예제를 변경하려면 [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 메서드의 첫 번째 매개 변수에 **AdType.Video** 대신 **AdType.Display** 값을 전달하세요. 자세한 내용은 [중간 광고](interstitial-ads.md)를 참조하세요.

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>관련 항목

* [GitHub의 광고 샘플](http://aka.ms/githubads)
 
