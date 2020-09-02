---
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: 'C # 및 XAML을 사용 하 여 UWP (유니버설 Windows 플랫폼) 앱에서 중간 광고를 표시 하 고 시작 하는 방법을 알아봅니다.'
title: C#의 중간 광고 샘플 코드
ms.date: 02/18/2020
ms.topic: article
keywords: 'windows 10, uwp, 광고, 광고, 중간, c #, 샘플 코드'
ms.localizationpriority: medium
ms.openlocfilehash: 63804fea6fc1657ead6c658814dfc7e0694f114f
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304735"
---
# <a name="interstitial-ad-sample-code-in-c"></a>C의 중간 광고 샘플 코드\# #  

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 정보](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

이 항목에서는 중간 비디오 광고를 표시 하는 기본 c # 및 UWP (XAML 유니버설 Windows 플랫폼) 앱에 대 한 전체 샘플 코드를 제공 합니다. 이 코드를 사용 하도록 프로젝트를 구성 하는 방법을 보여 주는 단계별 지침은 [중간 광고](interstitial-ads.md)를 참조 하세요. 전체 샘플 프로젝트는 [GitHub의 광고 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)을 참조 하세요.

## <a name="code-example"></a>코드 예제

이 섹션에서는 중간 광고를 표시 하는 기본 앱에서 MainPage 및 파일의 내용을 보여 줍니다. 이러한 예제를 사용 하려면 visual Studio의 Visual c # **Blank App (유니버설 Windows)** 프로젝트에 코드를 복사 합니다.

이 샘플 앱에서는 두 개의 단추를 사용 하 여 중간 광고를 요청 하 고 시작 합니다. ```myAppId``` ```myAdUnitId``` 앱을 스토어에 제출 하기 전에 및 필드 값을 파트너 센터의 라이브 값으로 바꿉니다. 자세한 내용은 [앱에서 ad 단위 설정](set-up-ad-units-in-your-app.md#live-ad-units)을 참조 하세요.

> [!NOTE]
> 중간 비디오 ad 대신 중간 배너 광고를 표시 하도록이 예제를 변경 하려면 adtype. **video**대신 [requestad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 메서드의 첫 번째 매개 변수에 **adtype. Display** 값을 전달 합니다. 자세한 내용은 [중간 광고](interstitial-ads.md)를 참조 하세요.

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-csharp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>관련 항목

* [GitHub의 광고 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
 