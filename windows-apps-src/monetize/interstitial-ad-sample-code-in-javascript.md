---
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: JavaScript 및 HTML로 작성 된 유니버설 Windows 플랫폼 (UWP) 앱을 사용 하 여 중간 광고를 시작 하는 방법을 알아봅니다.
title: JavaScript의 중간 광고 샘플 코드
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, 중간, javascript, 샘플 코드
ms.localizationpriority: medium
ms.openlocfilehash: 07a61bfde1fc6a77f87617ee553408c71f79cbc3
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363637"
---
# <a name="interstitial-ad-sample-code-in-javascript"></a>JavaScript의 중간 광고 샘플 코드

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 정보](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

이 항목에서는 중간 광고를 게재하는 기본 JavaScript 및 HTML UWP(유니버설 Windows 플랫폼) 앱의 전체 샘플 코드를 제공합니다. 이 코드를 사용 하도록 프로젝트를 구성 하는 방법을 보여 주는 단계별 지침은 [중간 광고](interstitial-ads.md)를 참조 하세요. 전체 샘플 프로젝트는 [GitHub의 광고 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)을 참조 하세요.

## <a name="code-example"></a>코드 예제

이 섹션에서는 중간 광고를 표시 하는 기본 앱에서 HTML 및 JavaScript 파일의 내용을 보여 줍니다. 이러한 예제를 사용 하려면 Visual Studio에서 JavaScript **WinJS 앱 (유니버설 Windows)** 프로젝트에이 코드를 복사 합니다.

이 샘플 앱에서는 두 개의 단추를 사용 하 여 중간 광고를 요청 하 고 시작 합니다. Visual Studio에서 생성 된 main.js 및 index.html 파일은 다음과 같이 수정 되었습니다. 아래에 표시 된 script.js 파일에는 샘플의 코드가 대부분 포함 되어 있으며,이 파일을 프로젝트의 **js** 폴더에 추가 해야 합니다.

```applicationId``` ```adUnitId``` 앱을 스토어에 제출 하기 전에 및 변수의 값을 파트너 센터의 라이브 값으로 바꿉니다. 자세한 내용은 [앱에서 ad 단위 설정](set-up-ad-units-in-your-app.md#live-ad-units)을 참조 하세요.

> [!NOTE]
> 중간 비디오 ad 대신 중간 배너 광고를 표시 하도록이 예제를 변경 하려면 값을 **Interstitialadtype** . **비디오**대신 [requestad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 메서드의 첫 번째 매개 변수에 표시 합니다. 자세한 내용은 [중간 광고](interstitial-ads.md)를 참조 하세요.

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
:::code language="html" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/index.html" range="1-21":::

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
:::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/script.js" id="script":::

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
:::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/main.js" id="main":::

## <a name="related-topics"></a>관련 항목

* [GitHub의 광고 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)

 
