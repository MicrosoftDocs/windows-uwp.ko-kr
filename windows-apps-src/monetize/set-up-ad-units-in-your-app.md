---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "스토어에 앱을 제출하기 전에 Windows 개발자 센터 대시보드의 응용 프로그램 ID 및 광고 단위 ID 값을 앱에 추가하는 방법을 알아봅니다."
title: "앱에서 광고 단위 설정"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10 uwp, 광고, 광고, 광고 단위"
ms.openlocfilehash: daf0887462a4c84aa827a6261793a0eaf4d512ca
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="set-up-ad-units-in-your-app"></a>앱에서 광고 단위 설정




[AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 또는 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx)를 사용하여 앱에서 광고를 표시하려면 응용 프로그램 ID 및 광고 단위 ID를 지정해야 합니다. 앱을 개발하는 동안 해당 [테스트 응용 프로그램 ID 및 광고 단위 ID 값](test-mode-values.md)을 사용하여 테스트 동안 앱이 광고를 렌더링하는 방법을 확인합니다.

앱 테스트를 완료하고 Windows 개발자 센터에 제출할 준비가 되면 [Windows 개발자 센터 대시보드](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx)의 응용 프로그램 ID 및 광고 단위 ID 값을 사용하도록 앱 코드를 업데이트해야 합니다. 라이브 앱에서 테스트 값을 사용하려고 하면 앱에 라이브 광고가 수신되지 않습니다.

라이브 앱에 대한 응용 프로그램 ID 및 광고 단위를 설정하려면

1.  Windows 개발자 센터 대시보드에서 앱을 선택하고 **수익 창출 &gt; 광고로 수익 창출**을 클릭합니다.

2.  이 페이지의 **Microsoft Advertising 광고 단위** 섹션에서 광고 단위를 만듭니다. 광고 단위 유형에 대한 다음 옵션 중에서 선택합니다.

  * 앱에서 배너 광고를 표시하기 위해 **AdControl**을 사용하는 경우에는 광고 단위 유형에 **배너**를 선택합니다.

  * 앱에서 동영상 또는 배너 중간 광고를 표시하기 위해 **InterstitialAd**를 사용하는 경우에는 **동영상 중간 광고** 또는 **배너 중간 광고**를 선택합니다(표시하려는 중간 광고 유형에 적합한 옵션을 선택하세요).

3.  이 페이지에는 생성된 각 광고 단위에 대해 **응용 프로그램 ID** 및 **광고 단위 ID**가 표시됩니다. 앱에 광고를 표시하려면 다음과 같이 앱 코드에 이러한 값을 사용해야 합니다.

  * 앱에서 배너 광고를 표시하는 경우 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) 개체의 [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) 및 **AdUnitId** 속성에 이러한 값을 할당합니다. 자세한 내용은 [XAML 및 .NET의 AdControl](adcontrol-in-xaml-and--net.md) 및 [HTML 5 및 JavaScript의 AdControl](adcontrol-in-html-5-and-javascript.md)을 참조하세요.

  * 앱에서 중간 광고를 표시하는 경우 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 개체의 **RequestAd** 메서드에 이러한 값을 전달합니다. 자세한 내용은 [중간 광고](interstitial-ads.md)를 참조하세요.

**광고를 통한 수익 창출** 페이지에 대한 자세한 내용은 [광고를 통한 수익 창출](../publish/monetize-with-ads.md)을 참조하세요.

## <a name="related-topics"></a>관련 항목

* [테스트 모드 값](test-mode-values.md)
* [XAML 및 .NET의 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 및 JavaScript의 AdControl](adcontrol-in-html-5-and-javascript.md)
* [중간 광고](interstitial-ads.md)


 

 
