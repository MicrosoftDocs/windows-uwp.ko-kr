---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: "Microsoft Advertising SDK는 광고로 앱 수익을 창출하는 다양한 방법을 제공합니다."
title: "Microsoft Advertising SDK를 사용하여 앱에 광고 표시"
ms.author: mcleans
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 광고, 광고, 배너, 광고 관리, 중간 광고"
ms.openlocfilehash: 4730ebaf55af8e7063c444d5b3bbd973b0508db2
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/21/2017
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>Microsoft Advertising SDK를 사용하여 앱에 광고 표시

Microsoft Advertising SDK를 사용해 앱에 광고를 삽입하여 수익 기회를 늘리세요. 저희 광고 수익 창출 플랫폼은 다양한 종류의 광고를 제공하고, 많은 인기 광고 네트워크에서 광고 조정을 지원합니다.

앱에 광고를 표시하려면 다음 단계를 따르세요.

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>1단계: Microsoft Advertising SDK 설치

Microsoft Advertising SDK는 앱에서 여러 광고 종류를 표시하기 위해 사용할 수 있는 다양한 컨트롤을 제공합니다. 설치 지침은 [이 문서](install-the-microsoft-advertising-libraries.md)를 참조하세요.

## <a name="step-2-choose-your-ad-type-and-add-code-to-display-test-ads-in-your-app"></a>2단계: 광고 유형을 선택하고, 앱에 테스트 광고를 표시할 코드를 추가합니다.

Microsoft Advertising SDK는 앱에서 표시할 수 있는 여러 유형의 광고를 제공합니다. 시나리오에 가장 적합한 광고 유형을 선택한 후 이 광고를 표시할 코드를 앱에 추가합니다.

앱에 광고를 개제하는 코드의 응용 프로그램 ID와 광고 단위 ID를 지정해야 합니다. 앱을 개발하는 동안 해당 [테스트 응용 프로그램 ID 및 광고 단위 ID 값](test-mode-values.md)을 사용하여 테스트 동안 앱이 광고를 렌더링하는 방법을 확인합니다.

### <a name="banner-ads"></a>배너 광고

페이지 위쪽 또는 아래쪽을 중심으로 앱의 일부분을 사용하는 고정 표시 광고입니다.

지침과 코드 예제는 [XAML 및 .NET의 AdControl](adcontrol-in-xaml-and--net.md)과 [HTML 5 및 Javascript의 AdControl](adcontrol-in-html-5-and-javascript.md)을 참조하세요. C# 및 C++를 사용하여 JavaScript/HTML 앱 및 XAML 앱에 배너 광고를 추가하는 방법을 보여 주는 전체 샘플 프로젝트에 대해서는 [GitHub의 광고 샘플](http://aka.ms/githubads)을 참조하세요.

![addreferences](images/banner-ad.png)

### <a name="interstitial-ads"></a>중간 광고

일반적으로 사용자가 앱 또는 게임을 계속하기 위해 강제로 비디오를 시청하거나 광고를 클릭해야 하는 전체 화면 광고입니다. 저희는 비디오와 배너라는 두 가지 중간 광고를 지원합니다.

지침과 코드 예제는 [중간 광고](interstitial-ads.md)를 참조하세요. C# 및 C++를 사용하여 JavaScript/HTML 앱 및 XAML 앱에 중간 광고를 추가하는 방법을 보여 주는 전체 샘플 프로젝트에 대해서는 [GitHub의 광고 샘플](http://aka.ms/githubads)을 참조하세요.

![addreferences](images/interstitial-ad.png)

### <a name="native-ads"></a>기본 광고

구성 요소 기반 광고입니다. 방해가 없는 사용자 환경으로 통합하기 위해 자신의 글꼴, 색상, 기타 UI 구성 요소를 사용해 자신의 각 광고 크리에이티브 조각이 앱과 통합할 수 있는 개별 요소로 앱에 제공되는 구성 요소 기반 광고입니다.

지침과 코드 예제는 [기본 광고](native-ads.md)를 참조하세요.

![addreferences](images/native-ad.png)

## <a name="step-3-get-an-ad-unit-from-dev-center-and-configure-your-app-to-receive-live-ads"></a>3단계: 개발자 센터에서 광고 단위를 가져와 라이브 광고를 받으려는 앱을 구성

앱 테스트를 완료해 스토어에 제출할 준비를 마쳤으면, Windows 개발자 센터 대시보드의 [광고로 수익 창출](../publish/monetize-with-ads.md)페이지에 광고 단위를 생성합니다. 그런 다음 해당 광고 단위에 대한 응용 프로그램 ID와 단위 ID 값을 사용하기 위해 앱 코드를 업데이트합니다. 스토어에 게시된 앱 버전에 테스트 응용 프로그램 ID 및 광고 단위 ID 값을 사용하려 시도하면, 앱이 라이브 광고를 받지 못합니다. 자세한 내용은 [앱에서 광고 단위 설정](set-up-ad-units-in-your-app.md)을 참조하세요.

<span id="ad-mediation"/>
### <a name="configure-ad-mediation"></a>광고 조정 구성

기본적으로 배너 광고, 중간 광고, 기본 광고는 Microsoft 유료 광고 네트워크의 광고를 표시합니다. 광고 수익을 극대화하기 위해 Taboola 및 Smaato 등 추가 유료 광고 네트워크의 광고를 표시하도록 광고 단위에 대한 광고 조정을 사용할 수 있습니다. Microsoft 앱 홍보 캠페인에 광고를 제공하여 앱 홍보 기능을 향상시킬 수 있습니다.

UWP 앱의 광고 조정을 사용하기 시작하려면, 광고 단위의 [광고 조정 설정을 구성](../publish/monetize-with-ads.md#mediation)합니다. 개발자의 앱이 제공되는 시장에서 광고 수익을 극대화할 수 있도록 기본적으로 기계 학습 알고리즘을 사용하여 조정 설정이 자동으로 구성됩니다. 하지만 개발자가 사용하려는 네트워크를 수동으로 선택할 수 있는 옵션도 제공됩니다. 어떤 방식을 선택하든 조정 설정이 서비스 전체에서 구성되므로 개발자는 앱에서 코드를 변경할 필요가 없습니다.    

<span id="8.x-mediation"/>
### <a name="mediation-in-windows-81-and-windows-phone-8x-apps"></a>Windows 8.1 및 Windows Phone 8.x 앱의 조정

Windows 8.1 및 Windows Phone 8.x 앱에서는 배너 광고에만 광고 조정을 사용할 수 있습니다. 광고 조정을 사용하려면 **AdControl** 클래스 대신 [Windows 및 Windows Phone 8.x용 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)의 **AdMediatorControl** 클래스를 사용해야 합니다. 앱에 이 컨트롤을 추가한 후에는 Windows 개발자 센터 대시보드에서 광고 조정 설정을 수동으로 구성할 수 있습니다.

Windows 8.1 또는 Windows Phone 8.x 앱에서 광고 조정을 사용하는 방법에 대한 자세한 내용은 [이 문서](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx)를 참조하세요.

> [!NOTE]
> Windows 8.1 및 Windows Phone 8.x 앱의 광고 조정은 더 이상 개발되지 않습니다. 광고 수익을 최대화하려면 배너 광고, 중간 광고, 기본 광고에서 광고 조정을 사용하는 UWP 앱을 빌드하는 것이 좋습니다.

## <a name="step-4-submit-your-app-and-review-performance"></a>4단계: 앱을 제출하고 성능 검토

광고가 있는 앱 개발을 마친 후, 스토어에서 사용할 수 있도록 개발자 센터 대시보드에 [업데이트한 앱을 제출](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)할 수 있습니다. 광고를 표시하는 앱은 [Windows 스토어 정책 섹션 10.10](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) 및 [앱 개발자 계약 별첨 E](https://msdn.microsoft.com/library/windows/apps/hh694058.aspx)에 규정된 추가 요구 사항을 충족해야 합니다.

앱을 게시해 스토어에서 사용할 수 있는 상태가 되고 나면, 대시보드에서 [광고 성능 보고서](../publish/advertising-performance-report.md)를 검토하고, 광고 성능을 최적화하기 위해 조정 설정을 계속 변경할 수 있습니다. 광고 수익은 [지급액 요약](../publish/payout-summary.md)에 포함되어 있습니다.

<span id="additional-help" />
## <a name="additional-help"></a>추가 도움말

Microsoft Advertising SDK 사용에 대한 추가 도움말은 다음 리소스를 사용하세요.

|  작업    | 리소스 |               
|----------|-------|
| 버그 보고 또는 광고 지원 받기     | [지원 페이지](https://go.microsoft.com/fwlink/p/?LinkId=331508)를 방문하고 **앱에서 바로 광고**를 선택합니다.        |
| 커뮤니티 지원 받기     | [포럼](http://go.microsoft.com/fwlink/p/?LinkId=401266)을 방문하세요.       |
| 배너 및 중간 광고를 앱에 추가하는 방법을 보여 주는 샘플 프로젝트를 다운로드합니다.     | [GitHub의 광고 샘플](http://aka.ms/githubads)을 참조하세요.       |
| Windows 앱용 최신 수익 창출 기회에 대한 자세한 정보     | [앱으로 수익 창출](https://developer.microsoft.com/store/monetize)을 방문하세요.        |

## <a name="related-topics"></a>관련 항목

* [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)
* [광고로 앱 수익 창출](http://go.microsoft.com/fwlink/p/?LinkId=699559)
* [광고 성과 보고서](../publish/advertising-performance-report.md)
