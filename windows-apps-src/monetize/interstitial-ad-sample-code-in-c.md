---
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: C#을 사용하여 중간 광고를 실행하는 방법을 알아봅니다.
title: C#의 중간 광고 샘플 코드
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, 광고, 중간, c#, 샘플 코드
ms.localizationpriority: medium
ms.openlocfilehash: 8c05681d2f59c88643e8cd224bb7de554a35de8d
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259282"
---
# <a name="interstitial-ad-sample-code-in-c"></a>C\#의 중간 광고 샘플 코드 #  

이 항목에서는 동영상 중간 광고를 게재하는 기본 C# 및 XAML UWP(유니버설 Windows 플랫폼) 앱의 전체 샘플 코드를 제공합니다. 이 코드를 사용하도록 프로젝트를 구성하는 방법을 보여 주는 단계별 지침은 [중간 광고](interstitial-ads.md)를 참조하세요. 전체 샘플 프로젝트는 [GitHub의 광고 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)을 참조하세요.

## <a name="code-example"></a>코드 예제

이 섹션에서는 중간 광고를 게재하는 기본 앱의 MainPage.xaml 및 MainPage.xaml.cs 파일 콘텐츠를 보여 줍니다. 이러한 예제를 사용하려면 코드를 Visual Studio에서 Visual C# **빈 앱(유니버설 Windows)** 프로젝트에 복사합니다.

이 샘플 앱은 2개의 버튼을 사용하여 중간 광고를 요청한 다음 실행합니다. 앱을 스토어에 제출 하기 전에 ```myAppId``` 및 ```myAdUnitId``` 필드 값을 파트너 센터의 라이브 값으로 바꿉니다. 자세한 내용은 [앱에서 광고 단위 설정](set-up-ad-units-in-your-app.md#live-ad-units)을 참조하세요.

> [!NOTE]
> 동영상 중간 광고 대신 배너 중간 광고를 표시하도록 이 예제를 변경하려면 **RequestAd** 메서드의 첫 번째 매개 변수에 [AdType.Video](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 대신 **AdType.Display** 값을 전달하세요. 자세한 내용은 [중간 광고](interstitial-ads.md)를 참조하세요.

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-csharp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>관련 항목

* [GitHub의 광고 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
 
