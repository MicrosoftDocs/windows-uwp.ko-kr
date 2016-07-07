---
author: mcleanbyron
ms.assetid: fcebd659-438b-4d03-bc73-6b662ed6f1f3
description: "광고가 있는 앱을 개발하고 게시하기 위한 종단 간 프로세스에 알아봅니다."
title: "광고가 포함된 앱 만들기 워크플로"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 69dd1a17290b7ffbc14dbc58404868119403f7c0


---

# 광고가 포함된 앱 만들기 워크플로


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

앱에 광고를 표시하려면 앱이 광고 네트워크에서 광고를 수신할 수 있어야 합니다. Microsoft은 Windows 앱 개발자가 광고를 수신할 수 있도록 하는 웹 서비스를 제공합니다. 사용자가 앱에서 광고를 클릭하면 사용자(광고의 *게시자*)는 광고의 생성자(*광고주*)로부터 돈을 받습니다. 광고주로부터 번 돈은 사용자의 계정을 사용하여 사용자에게 지급됩니다.

다음 고급 단계에서는 광고가 포함된 앱을 개발하고 게시하는 일반적인 프로세스를 설명합니다.

1.  개발 단계:

    * Windows 개발자 센터 계정을 설정합니다.
    * 테스트 모드 값을 사용하여 앱을 개발합니다.

2.  해제 준비 완료:

    * 라이브 광고를 수신하도록 앱을 구성합니다.
    * Windows 개발자 센터에 앱을 제출하고 성능 데이터를 검토합니다.

각 단계에 대한 자세한 내용은 아래에서 해당 섹션을 읽어보세요.

## Windows 개발자 센터 계정 설정

앱을 게시하고 광고를 수신하기 위한 Windows 개발자 센터 계정이 있어야 합니다. 광고 조정을 사용하는지 여부는 관계가 없습니다. 광고 관련 앱 관리 또한 Windows 개발자 센터에서 수행됩니다. 이전에 Microsoft pubCenter를 사용하여 앱에서 광고를 관리한 경우 이 기능이 Windows 개발자 센터의 기능으로 대체되었습니다 

Windows 개발자 센터에서 계정을 설정하려면 [홈페이지](https://dev.windows.com/windows-apps)를 방문합니다. 추가 정보는 Windows 개발자 센터에서 [도움말 페이지](https://dev.windows.com/develop)에서 사용할 수 있습니다.

## 테스트 모드 값을 사용하여 앱 개발

다음 연습에 포함된 지침에 따라 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 또는 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx)를 추가하여 앱에 광고를 표시합니다.

-   [중간 광고](interstitial-ads.md)
-   [XAML 및 .NET의 AdControl](adcontrol-in-xaml-and--net.md)
-   [HTML 5 및 Javascript의 AdControl](adcontrol-in-html-5-and-javascript.md)
-   [Windows Phone Silverlight의 AdControl](adcontrol-in-windows-phone-silverlight.md)

**AdControl** 또는 **InterstitialAd**를 사용하여 앱에서 광고를 표시하려면 코드에 응용 프로그램 ID와 광고 단위 ID를 지정하여 앱을 Windows 개발자 센터 계정에 연결하고 광고를 제공해야 합니다. 앱을 개발하는 동안 테스트 응용 프로그램 ID 및 광고 단위 ID 값을 사용하여 테스트 동안 앱이 광고를 렌더링하는 방법을 확인합니다. 이렇게 하면 테스트하는 동안 앱이 광고를 수신하고 렌더링하는 방식을 확인할 수 있습니다. 자세한 내용은 [테스트 모드 값](test-mode-values.md)을 참조하세요.

C# 및 C++를 사용하여 JavaScript/HTML 앱 및 XAML 앱에 배너 및 동영상 중간 광고를 추가하는 방법을 보여 주는 전체 샘플 프로젝트에 대해서는 [GitHub의 광고 샘플](http://aka.ms/githubads)을 참조하세요.

## 라이브 광고를 수신하도록 앱 구성

앱 테스트를 완료하고 Windows 개발자 센터에 제출할 준비가 되면 [Windows 개발자 센터 대시보드](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx)의 응용 프로그램 ID 및 광고 단위 ID 값을 사용하도록 앱 코드를 업데이트해야 합니다. 라이브 앱에서 테스트 값을 사용하려고 하면 앱에 라이브 광고가 수신되지 않습니다. 자세한 내용은 [앱에서 광고 단위 설정](set-up-ad-units-in-your-app.md)을 참조하세요.

## 앱 제출

앱 개발이 완료되면 Windows 개발자 센터 대시보드를 사용하여 Windows 스토어에 앱을 게시할 수 있습니다. Windows 스토어의 모든 앱에 대한 요구 사항을 충족하는 것 외에, 광고를 표시하는 앱은 몇 가지 추가 요구 사항을 충족해야 합니다. 자세한 내용은 [Windows 스토어에 광고가 포함된 앱 제출](submit-an-app-with-ads-to-the-windows-store.md)을 참조하세요.

앱이 게시되고 Windows 스토어에서 사용할 수 있게 되면 개발자 센터 대시보드에서 [광고 성과 보고서](../publish/advertising-performance-report.md)를 검토할 수 있습니다.

 

 



<!--HONumber=Jun16_HO4-->


