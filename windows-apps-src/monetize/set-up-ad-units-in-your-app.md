---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "스토어에 앱을 제출하기 전에 Windows 개발자 센터 대시보드의 응용 프로그램 ID 및 광고 단위 ID 값을 앱에 추가하는 방법을 알아봅니다."
title: "앱에서 광고 단위 설정"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10 uwp, 광고, 광고, 광고 단위"
ms.openlocfilehash: f96e81079764682a9f603fe93a9c123a69690507
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/27/2017
---
# <a name="set-up-ad-units-in-your-app"></a>앱에서 광고 단위 설정

앱의 모든 배너 광고, 중간 광고, 기본 광고 컨트롤에는 저희 서비스가 컨트롤에 광고를 제공할 때 사용하는 *광고 단위*가 있습니다. 각 광고 단위는 *광고 단위 ID* 및 *응용 프로그램 ID*로 구성되어 있습니다. 앱을 개발하는 동안 [테스트 응용 프로그램 ID 및 광고 단위 ID 값](test-mode-values.md)을 컨트롤에 할당, 앱이 테스트 광고를 표시하는지 확인합니다. 이 테스트 값은 앱 테스트 버전에서만 사용할 수 있습니다.

앱 테스트를 완료하고 Windows 개발자 센터에 제출할 준비가 되면, Windows 개발자 센터 대시보드의 [광고로 수익 창출](../publish/monetize-with-ads.md) 페이지에서 응용 프로그램 ID와 광고 단위 ID 값을 사용해 앱 코드를 업데이트해야 합니다. 라이브 앱에서 테스트 값을 사용하려고 하면 앱에 라이브 광고가 수신되지 않습니다.

라이브 앱에 대한 광고 단위를 설정하려면,

1.  Windows 개발자 센터 대시보드에서 앱을 선택하고 **수익 창출 > 광고로 수익 창출**을 클릭합니다.

2.  **광고 단위 만들기** 섹션에서 **광고 단위 이름** 필드에 광고 단위의 이름을 입력합니다.

3. **광고 단위 유형** 드롭다운 목록에서 컨트롤에 표시 중인 광고에 해당하는 광고 단위 유형을 선택합니다.

    -   앱에서 배너 광고를 표시하기 위해 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)을 사용하는 경우에는 **배너**를 선택합니다.

    -   앱에서 동영상 또는 배너 중간 광고를 표시하기 위해 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx)를 사용하는 경우에는 **동영상 중간 광고** 또는 **배너 중간 광고**를 선택합니다(표시하려는 중간 광고 유형에 적합한 옵션을 선택하세요).

    -   앱에서 배너 광고를 표시하기 위해 [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx)를 사용하는 경우에는 **기본**을 선택합니다.
      > [!NOTE]
      > 현재 **기본** 광고 단위를 생성하는 기능은 파일럿 프로그램에 참여하고 있는 일부 개발자만 사용할 수 입습니다. 그러나 조만간 모든 개발자가 이 기능을 사용할 수 있도록 지원할 계획입니다. 파일럿 프로그램에 참여하고 싶다면 aiacare@microsoft.com에 문의하세요.

4.  **장치 패밀리** 드롭다운 목록에서 광고 단위가 사용될 앱의 대상이 되는 장치 패밀리를 선택합니다. 사용 가능한 옵션은 **UWP(Windows 10)**, **PC/태블릿(Windows 8.1)** 또는 **모바일(Windows Phone 8.x)**입니다.

5.  **광고 단위 만들기**를 클릭합니다. 이 페이지의 **사용 가능한 광고 단위** 섹션에 있는 목록 맨 위에 새로운 광고 단위가 표시됩니다.

6.  생성된 광고 단위마다 **응용 프로그램 ID** 및 **광고 단위 ID**가 표시됩니다. 앱에 광고를 표시하려면 다음과 같이 앱 코드에 이러한 값을 사용해야 합니다.

    -   앱에서 배너 광고를 표시하는 경우 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) 개체의 [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) 및 **AdUnitId** 속성에 이러한 값을 할당합니다. 자세한 내용은 [XAML 및 .NET의 AdControl](adcontrol-in-xaml-and--net.md) 및 [HTML 5 및 JavaScript의 AdControl](adcontrol-in-html-5-and-javascript.md)을 참조하세요.

    -   앱에서 중간 광고를 표시하는 경우 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 개체의 **RequestAd** 메서드에 이러한 값을 전달합니다. 자세한 내용은 [중간 광고](interstitial-ads.md)를 참조하세요.

    -   앱에서 기본 광고를 표시하는 경우 이러한 값을 [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx) 생성자의 *applicationId* 및 *adUnitId* 매개 변수로 전달합니다. 자세한 내용은 [기본 광고](../monetize/native-ads.md)를 참조하세요.

7. Windows 10용 UWP 앱의 경우 필요에 따라 **광고 조정** 섹션에서 설정을 구성하여 [AdControl](../publish/monetize-with-ads.md#mediation)에 광고 조정을 사용할 수 있습니다. 광고 조정을 통해 Taboola 및 Smaato 같은 기타 유료 광고 네트워크와 Microsoft 앱 프로모션 캠페인에 대한 광고를 포함하여 여러 광고 네트워크의 광고를 표시하여 광고 수익과 앱 프로모션 기능을 최대화할 수 있습니다. 기본적으로 앱에서 지원하는 시장에서 광고 수익을 극대화할 수 있도록 기계 학습 알고리즘을 사용하여 앱에 대한 광고 조정 설정이 자동으로 선택되지만, 원한다면 조정 설정을 수동으로 구성할 수 있습니다.

<span id="manage" />
## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>앱에서 여러 광고 컨트롤에 대한 광고 단위 관리

단일 앱에서 여러 배너, 중간, 기본 광고 컨트롤을 사용할 수 있습니다. 이 경우, 각 컨트롤에 다른 광고 단위를 지정하는 것이 좋습니다. 각 컨트롤에 다른 광고 단위를 사용하면 별도로 [미디어 설정을 구성](../publish/monetize-with-ads.md#mediation)할 수 있고, 각 컨트롤에 별개의 [보고 데이터](../publish/advertising-performance-report.md)를 생성할 수 있습니다. 또 이렇게 하면 서비스가 지원하는 앱에서 더 효과적으로 광고를 최적화할 수 있습니다.

> [!IMPORTANT]
> 앱별로 하나의 광고 단위만 사용할 수 있습니다. 특정 광고 단위를 하나 이상의 앱에 사용하면, 해당 광고 단위에 광고가 제공되지 않습니다.

## <a name="related-topics"></a>관련 항목

* [테스트 모드 값](test-mode-values.md)
* [XAML 및 .NET의 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 및 JavaScript의 AdControl](adcontrol-in-html-5-and-javascript.md)
* [중간 광고](interstitial-ads.md)
* [기본 광고](../monetize/native-ads.md)


 

 
