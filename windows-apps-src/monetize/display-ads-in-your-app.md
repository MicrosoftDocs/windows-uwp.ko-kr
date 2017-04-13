---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: "Microsoft Store Services SDK는 광고로 앱 수익을 창출하는 다양한 방법을 제공합니다."
title: "앱에서 광고 표시"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10 uwp, 광고, 광고, 배너, 중간"
ms.openlocfilehash: ceae17d34ba400876f912ba9932d76f59d773e63
ms.sourcegitcommit: d053f28b127e39bf2aee616aa52bb5612194dc53
translationtype: HT
---
# <a name="display-ads-in-your-app"></a>앱에서 광고 표시


UWP(유니버설 Windows 플랫폼) 및 Windows 스토어는 광고로 앱 수익을 창출하는 다양한 방법을 제공합니다.

## <a name="display-banner-and-interstitial-ads-using-the-microsoft-advertising-libraries"></a>Microsoft Advertising 라이브러리를 사용하여 배너 및 중간 광고 표시

앱에 배너 또는 중간 광고를 포함하여 앱으로 더 큰 수익을 창출하세요.

* *배너 광고*는 일반적으로 앱 페이지의 위쪽 또는 아래쪽 부분을 활용하는 작은 광고입니다.
* *중간 광고*는 일반적으로 사용자가 앱 또는 게임을 계속하기 위해 강제로 비디오를 시청하거나 광고를 클릭해야 하는 전체 화면 광고입니다. UWP 앱에는 비디오와 배너라는 두 가지 중간 광고가 지원됩니다.

앱에 이러한 유형의 광고를 포함하려면 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)(UWP 앱용)와 [Microsoft Advertising SDK for Windows 및 Windows Phone 8.x](http://aka.ms/store-8-sdk)(Windows 8.1 및 Windows Phone 8.x 앱용)에 배포된 광고 라이브러리의 **AdControl** 및 **InterstitialAd** 컨트롤을 사용합니다.

Windows 개발자 센터 대시보드의 [광고 성과 보고서](../publish/advertising-performance-report.md)를 사용하여 광고 성과를 실시간으로 모니터링할 수 있습니다.

다음 항목에서는 Windows Advertising 라이브러리와 관련된 일반적인 작업에 대한 정보를 제공합니다.

|  작업    | 항목 |               
|----------|-------|
| Microsoft Advertising 라이브러리를 설치하고 사용하기 시작합니다.     | [Microsoft Advertising 라이브러리 시작](get-started-with-microsoft-advertising-libraries.md)을 참조하세요.        |
| XAML/C# 앱에 배너 광고를 표시합니다.     | [XAML 및 .NET의 AdControl](adcontrol-in-xaml-and--net.md)을 참조하세요.        |
| HTML/JavaScript 앱에 배너 광고를 표시합니다.     | [HTML 5 및 Javascript의 AdControl](adcontrol-in-html-5-and-javascript.md)을 참조하세요.        |
| Windows Phone Silverlight 8.x 앱에 배너 광고를 표시합니다.     | [Windows Phone Silverlight에서의 AdControl](adcontrol-in-windows-phone-silverlight.md)을 참조하세요.        |
| 앱에 중간 광고를 표시합니다.     | [중간 광고](interstitial-ads.md)를 참조하세요.       |
| HTML과 함께 JavaScript를 사용하여 작성된 UWP(유니버설 Windows 플랫폼) 앱에서 비디오 콘텐츠에 광고를 추가합니다.   |  [HTML 5 및 JavaScript로 된 동영상 콘텐츠에 광고 추가](add-advertisements-to-video-content.md)를 참조하세요.  |
| 배너 및 중간 광고를 앱에 추가하는 방법을 보여 주는 샘플 프로젝트를 다운로드합니다.     |[GitHub의 광고 샘플](http://aka.ms/githubads)을 참조하세요.       |
| 앱의 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 오류를 처리합니다.     | [오류 처리](error-handling-with-advertising-libraries.md)와 [AdControl 오류 처리](adcontrol-error-handling.md) 아래의 연습을 참조하세요.       |
| Microsoft Advertising 라이브러리의 버그를 보고합니다.     | [지원 페이지](https://go.microsoft.com/fwlink/p/?LinkId=331508)를 방문하세요.        |
| 커뮤니티 지원을 받습니다.     | [포럼](http://go.microsoft.com/fwlink/p/?LinkId=401266)을 방문하세요.       |

                            

## <a name="use-ad-mediation-for-banner-ads-windows-81-and-windows-phone-8x"></a>배너 광고에 대한 광고 조정 사용(Windows 8.1 및 Windows Phone 8.x)

Windows 8.1 및 Windows Phone 8.x 앱의 경우 **AdMediatorControl** 클래스를 사용하여 여러 광고 네트워크의 배너 광고를 표시함으로써 광고 수익을 최적화할 수 있습니다. 앱에 이 컨트롤을 추가하고 나면 Windows 개발자 센터 대시보드의 광고 조정 설정을 구성하고, Microsoft에서는 선택한 광고 네트워크에서 배너 광고 요청 조정을 담당합니다. 자세한 내용은 [광고 조정을 사용하여 광고 수익 최대화](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx)를 참조하세요.

> [!NOTE]
> **AdMediatorControl** 클래스를 사용한 광고 조정은 현재 Windows 10용 UWP 앱에 지원되지 않습니다. 동일한 배너 광고용(**AdControl**) 및 동영상 중간 광고용(**InterstitialAd**) API를 사용하는 UWP 앱에 대한 서버 측 중재가 출시 예정입니다. UWP 앱에서 **AdMediatorControl**을 **AdControl**로 마이그레이션하는 방법은 [UWP 앱을 위해 AdMediatorControl을 AdControl로 마이그레이션](migrate-from-admediatorcontrol-to-adcontrol.md)을 참조하세요.

<span id="silverlight_support"/>
## <a name="advertising-support-for-windows-phone-8x-silverlight-projects"></a>Windows Phone 8.x Silverlight 프로젝트용 광고 지원

일부 개발자 시나리오는 Windows Phone 8.x Silverlight 프로젝트에서 더 이상 지원되지 않습니다. 자세한 내용은 다음 표를 참조하세요.

|  플랫폼 버전  |  기존 프로젝트    |   새 프로젝트  |
|-----------------|----------------|--------------|
| Windows Phone 8.0 Silverlight     |  Universal Ad Client SDK 또는 Microsoft Advertising SDK의 이전 릴리스에서 **AdControl** 또는 **AdMediatorControl**을 이미 사용하는 기존 Windows Phone 8.0 Silverlight 프로젝트가 있고 이 앱이 Windows 스토어에 이미 게시된 경우, 프로젝트를 수정 및 다시 빌드할 수 있으며 디바이스의 변경 내용을 디버그 또는 테스트할 수 있습니다. 에뮬레이터에서 프로젝트 디버깅 또는 테스트는 지원되지 않습니다.  |  지원되지 않음.  |
| Windows Phone 8.1 Silverlight    |  이전 SDK의 **AdControl** 또는 **AdMediatorControl**을 사용하는 기존 Windows Phone 8.1 Silverlight 프로젝트가 있는 경우 프로젝트를 수정하고 다시 빌드할 수 있습니다. 그러나 앱을 디버그하거나 테스트하려면 에뮬레이터에서 앱을 실행하고 응용 프로그램 ID와 광고 단위 ID에 [테스트 모드 값](test-mode-values.md)을 사용해야 합니다. 디바이스에서 앱 디버깅 또는 테스트는 지원되지 않습니다.  |   새 Windows Phone 8.1 Silverlight 프로젝트에 **AdControl** 또는 **AdMediatorControl**을 추가할 수 있습니다. 그러나 앱을 디버그하거나 테스트하려면 에뮬레이터에서 앱을 실행하고 응용 프로그램 ID와 광고 단위 ID에 [테스트 모드 값](test-mode-values.md)을 사용해야 합니다. 디바이스에서 앱 디버깅 또는 테스트는 지원되지 않습니다. |

## <a name="related-topics"></a>관련 항목

* [Microsoft 스토어 서비스 SDK](microsoft-store-services-sdk.md)
* [광고로 앱 수익 창출](http://go.microsoft.com/fwlink/p/?LinkId=699559)
* [광고 성과 보고서](../publish/advertising-performance-report.md)
