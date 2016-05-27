---
author: mcleanbyron
ms.assetid: 9165f709-71d7-42cf-9b30-3190fe029fb4
description: Microsoft Advertising 라이브러리의 AdControl 클래스와 광고 조정 라이브러리의 AdMediatorControl 클래스 간 차이점을 알아봅니다.
title: AdMediatorControl과 AdControl의 차이
---

# AdMediatorControl과 AdControl의 차이


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

Microsoft의 배너 광고 또는 동영상 중간 광고를 표시하려는 경우 앱에서 XAML 및 JavaScript용 Microsoft Advertising 라이브러리를 사용합니다. 이러한 라이브러리는 여러 광고 네트워크에서 광고를 표시하는 데 사용하는 광고 조정 라이브러리와 다릅니다. 다음과 같은 경우 Microsoft Advertising 라이브러리(XAML 및 JavaScript)에 대한 이 설명서를 사용합니다.

* XAML 또는 JavaScript 앱에서 **AdMediatorControl**이 아닌 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 또는 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx)를 사용하고 있습니다.
* 광고 조정에서 사용되는 기본 **AdControl** API에 대한 참조 정보를 찾고 있습니다.

Microsoft Advertising 라이브러리 및 광고 조정 라이브러리 둘 다 Microsoft 스토어 참여 및 수익 창출 SDK에 포함되어 있습니다. 이 SDK와 이 SDK에 포함된 여러 다양한 Microsoft Advertising 라이브러리를 설치하는 방법에 대한 자세한 내용은 [Microsoft Advertising 라이브러리 설치](install-the-microsoft-advertising-libraries.md)를 참조하세요.

>**참고** 중간 광고를 표시하려면 **InterstitialAd** 컨트롤을 사용합니다. **AdControl** 및 **AdMediatorControl**은 중간 광고를 표시할 수 없습니다. 자세한 내용은 [중간 광고](interstitial-ads.md)를 참조하세요.

 

## 광고 조정


Microsoft의 배너 광고(중간 광고 아님)를 표시하는 권장 방법은 앱에 **AdMediatorControl**을 사용하는 것입니다. **AdMediatorControl**은 여러 광고 네트워크의 배너 광고를 표시합니다.

프로젝트에 **AdMediatorControl**을 사용하는 경우 Visual Studio의 **연결된 서비스** 기능을 사용하여 사용할 광고 네트워크를 지정해야 합니다. Visual Studio는 일부 광고 네트워크에 대해 필요한 어셈블리를 프로그래밍 방식으로 가져오려고 합니다. 자동으로 검색할 수 없는 어셈블리가 있는 경우 광고 네트워크별로 설치해야 합니다. 광고 조정에 대한 자세한 내용은 [광고 조정을 사용하여 수익 최대화](use-ad-mediation-to-maximize-revenue.md)를 참조하세요.

**AdMediatorControl**은 광고 단위 ID 및 응용 프로그램 ID의 사용을 추상화합니다. 이러한 ID는 Windows 개발자 센터 대시보드의 광고 조정 설정과 함께 **AdMediatorControl**에서 관리됩니다. 자세한 내용은 [앱 제출 및 광고 조정 구성](submit-your-app-and-configure-ad-mediation.md)을 참조하세요.

**AdMediatorControl**은 고유한 특성 및 구문을 사용하여 조정하는 각 광고 서비스에 대한 API를 지원합니다. 자세한 내용은 [광고 조정자 컨트롤 추가 및 사용](add-and-use-the-ad-mediator-control.md)을 참조하세요.

## AdControl


Microsoft(다른 광고 네트워크 없음)의 배너 광고만 표시하려면 XAML 및 JavaScript 앱에서 **AdControl**만 사용할 수 있습니다. **AdControl**을 사용하는 앱을 테스트하는 동안 응용 프로그램 ID 및 광고 단위 ID로 [테스트 모드 값](test-mode-values.md)을 사용합니다. 앱 테스트를 완료하고 Windows 개발자 센터에 제출할 준비가 되면 Windows 개발자 센터 대시보드를 사용하여 라이브 광고 단위 ID 및 응용 프로그램 ID를 가져오고 이러한 값을 사용하도록 코드를 업데이트합니다. 자세한 내용은 [앱에서 광고 단위 설정](set-up-ad-units-in-your-app.md)을 참조하세요.

 

 


<!--HONumber=May16_HO2-->


