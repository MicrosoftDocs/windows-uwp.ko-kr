---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: "Microsoft 스토어 참여 및 수익 창출 SDK는 광고로 앱 수익을 창출하는 다양한 방법을 제공합니다."
title: "앱에서 광고 표시"
translationtype: Human Translation
ms.sourcegitcommit: 8a5b02dbc40f3f0cd9be32aa7d5184e60a3b2707
ms.openlocfilehash: c79ba96908cc7b52afefbe44c3f56ce009c87f16

---

# 앱에서 광고 표시


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

[Microsoft 스토어 참여 및 수익 창출 SDK](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md)는 광고로 앱 수익을 창출하는 다양한 방법을 제공합니다.

## Microsoft Advertising 라이브러리를 사용하여 배너 및 동영상 중간 광고 표시

배너 및 동영상 중간 광고를 포함하여 Windows 앱에서 수익을 창출할 수 있습니다. 광고는 PC, 태블릿 및 휴대폰용 Windows 앱에 표시됩니다. Windows 개발자 센터 대시보드를 사용하여 광고 성과를 실시간으로 모니터링할 수 있습니다.

앱에 광고를 포함하려면 Microsoft 스토어 참여 및 수익 창출 SDK에서 배포하는 Advertising 라이브러리에 있는 **AdControl** 및 **InterstitialAd** 컨트롤을 사용합니다. 이러한 컨트롤을 사용하여 Windows 10, Windows 8.1, Windows Phone 8.1 및 Windows Phone 8용 XAML 또는 JavaScript/HTML 앱에서 Microsoft의 배너 및 동영상 중간 광고를 표시할 수 있습니다.

자세한 내용은 [Microsoft Advertising 라이브러리를 사용하여 광고 표시](display-ads-using-the-microsoft-advertising-libraries.md)를 참조하세요. 광고가 포함된 앱을 게시하고 나면 [광고 성과 보고서](../publish/advertising-performance-report.md)를 사용하여 광고 성과를 추적합니다.                                           

## 여러 광고 네트워크의 배너 광고를 위해 광고 조정 사용

XAML 앱에서 **AdMediatorControl** 클래스를 사용하여 여러 광고 네트워크의 배너 광고를 표시함으로써 광고 수익을 최적화할 수 있습니다. 앱에 이 컨트롤을 추가하고 나면 Windows 개발자 센터 대시보드의 광고 조정 설정을 구성하고, Microsoft에서는 선택한 광고 네트워크에서 배너 광고 요청 조정을 담당합니다. 광고 조정에 대한 자세한 내용은 [광고 조정을 사용하여 광고 수익 최대화](use-ad-mediation-to-maximize-revenue.md)를 참조하세요.

## Microsoft Advertising 라이브러리와 광고 조정 간의 차이점

Microsoft 스토어 참여 및 수익 창출 SDK에는 Microsoft Advertising 및 광고 조정에 대한 라이브러리가 포함되어 있습니다. 하지만 이러한 라이브러리는 다양한 클래스를 제공하고 다양한 목적으로 사용됩니다.

* 배너 또는 동영상 중간 광고를 XAML 또는 JavaScript 앱에 표시하려면 Microsoft Advertising 라이브러리에 있는 **AdControl** 및 **InterstitialAd** 클래스를 사용합니다.
* XAML 앱에 여러 광고 네트워크의 배너 광고를 표시하려면 광고 조정 라이브러리에 있는 **AdMediatorControl** 클래스를 사용합니다.

자세한 내용은 [AdMediatorControl과 AdControl의 차이](what-is-the-difference-admediatorcontrol-or-adcontrol.md)를 참조하세요.

## 관련 항목

* [Microsoft 스토어 참여 및 수익 창출 SDK](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md)
* [광고로 앱 수익 창출]( http://go.microsoft.com/fwlink/p/?LinkId=699559)



<!--HONumber=Jun16_HO4-->


