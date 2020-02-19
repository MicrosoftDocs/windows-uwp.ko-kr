---
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: 앱을 스토어에 제출 하기 전에 파트너 센터에서 응용 프로그램 ID 및 ad 단위 ID 값을 앱에 추가 하는 방법에 대해 알아봅니다.
title: 앱에서 광고 단위 설정
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10 uwp, 광고, 광고, 광고 단위, 테스트
ms.localizationpriority: medium
ms.openlocfilehash: 37f04e8b112df23457c46e4f535309f2fe5dd847
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463905"
---
# <a name="set-up-ad-units-in-your-app"></a>앱에서 광고 단위 설정

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 내용](https://aka.ms/ad-monetization-shutdown)

유니버설 Windows 플랫폼(UWP) 앱의 모든 광고 컨트롤에는 저희 서비스가 컨트롤에 광고를 제공할 때 사용하는 *광고 단위*가 있습니다. 각 광고 단위는 앱의 코드에 할당해야 하는 *광고 단위 ID* 및 *응용 프로그램 ID*로 구성됩니다.

앱에 테스트 광고가 표시되는지 확인하기 위해 테스트 중에 사용할 수 있는 [테스트 광고 단위 값](#test-ad-units)이 제공됩니다. 이 테스트 값은 앱 테스트 버전에서만 사용할 수 있습니다. 게시한 후에 앱에서 테스트 값을 사용하려고 하면 앱에 광고가 수신되지 않습니다.

UWP 앱 테스트를 완료 하 고 파트너 센터에 제출할 준비가 되 면 파트너 센터의 [앱 내 광고](../publish/in-app-ads.md) 페이지에서 [live ad 단위를 만들고](#live-ad-units) 이 ad 단위에 대 한 응용 프로그램 ID 및 ad unit id 값을 사용 하도록 앱 코드를 업데이트 해야 합니다.

앱의 코드에서 응용 프로그램 ID와 광고 단위 ID 값을 할당하는 방법에 대한 자세한 내용은 다음 문서를 참조하세요.
* [XAML 및 .NET의 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 및 Javascript의 AdControl](adcontrol-in-html-5-and-javascript.md)
* [중간 광고](../monetize/interstitial-ads.md)
* [기본 광고](../monetize/native-ads.md)

<span id="test-ad-units" />

## <a name="test-ad-units"></a>테스트 광고 단위

앱을 개발하는 동안 이 섹션의 테스트 응용 프로그램 ID 및 광고 단위 ID 값을 사용하여 테스트 동안 앱이 광고를 렌더링하는 방법을 확인합니다.

### <a name="banner-ads-using-the-adcontrol-class"></a>배너 광고	(AdControl 클래스 사용)

* Ad 단위 ID: ```test```
* 응용 프로그램 ID: ```3f83fe91-d6be-434d-a0ae-7351c5a997f1```

    > [!IMPORTANT]
    > **AdControl**에서 라이브 광고의 크기는 **너비** 및 **높이** 속성으로 정의됩니다. 최상의 결과를 얻으려면 코드의 **Width** 및 **Height** 속성이 [배너 광고에 지원되는 광고 크기](supported-ad-sizes-for-banner-ads.md) 중 하나인지 확인합니다. **Width** 및 **Height** 속성은 라이브 광고의 크기에 따라 변경되지 않습니다.

### <a name="interstitial-ads-and-native-ads"></a>중간 광고와 기본 광고

* Ad 단위 ID: ```test```
* 응용 프로그램 ID: ```d25517cb-12d4-4699-8bdc-52040c712cab```

<span id="live-ad-units" />

## <a name="live-ad-units"></a>라이브 광고 단위

파트너 센터에서 라이브 ad 단위를 가져와서 앱에서 사용 하려면:

1.  파트너 센터의 **앱 내 광고** 페이지에서 [Ad 단위를 만듭니다](../publish/in-app-ads.md#create-ad-unit) . 엡에서 사용하는 광고 컨트롤에 대해 올바른 광고 단위 유형을 지정해야 합니다.
    > [!NOTE]
    > [조정 설정](../publish/in-app-ads.md#mediation) 섹션에서 설정을 구성하여, 광고 단위의 광고 조정을 사용하는 방법도 있습니다. 광고 조정을 통해 기타 유료 광고 네트워크의 광고와 Microsoft 앱 프로모션 캠페인 광고 등 여러 광고 네트워크의 광고를 표시하여 광고 수익과 앱 홍보 기능을 최대화할 수 있습니다. 기본적으로 앱에서 지원하는 시장에서 광고 수익을 극대화할 수 있도록 기계 학습 알고리즘을 사용하여 앱에 대한 광고 조정 설정이 자동으로 선택되지만, 원한다면 조정 설정을 수동으로 구성할 수 있습니다.

2.  새 ad 단위를 만든 후 **수익 창출** &gt; **in app ads** 페이지에서 사용 가능한 ad 단위 테이블의 ad 단위에 대 한 **응용 프로그램 id** 및 **ad 단위 id** 를 검색 합니다.
    > [!NOTE]
    > 테스트 광고 단위와 라이브 UWP 광고 단위의 응용 프로그램 ID 값은 형식이 서로 다릅니다. 테스트 응용 프로그램 ID 값은 GUID입니다. 파트너 센터에서 라이브 UWP ad 단위를 만들 때 ad 단위의 응용 프로그램 ID 값은 항상 앱에 대 한 저장소 ID와 일치 합니다. 예를 들어 매장 ID 값은 9NBLGGH4R315와 같습니다.

3.  앱 코드에서 응용 프로그램 ID 및 광고 단위 ID 값을 할당합니다. 자세한 내용은 다음 문서를 참조하세요.
    * [XAML 및 .NET의 AdControl](adcontrol-in-xaml-and--net.md)
    * [HTML 5 및 Javascript의 AdControl](adcontrol-in-html-5-and-javascript.md)
    * [중간 광고](../monetize/interstitial-ads.md)
    * [기본 광고](../monetize/native-ads.md)

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>앱에서 여러 광고 컨트롤에 대한 광고 단위 관리

단일 앱에서 여러 배너, 중간, 기본 광고 컨트롤을 사용할 수 있습니다. 이 경우, 각 컨트롤에 다른 광고 단위를 지정하는 것이 좋습니다. 각 컨트롤에 다른 광고 단위를 사용하면 별도로 [조정 설정을 구성](../publish/in-app-ads.md#mediation)할 수 있고, 각 컨트롤에 대해 별개의 [보고 데이터](../publish/advertising-performance-report.md)를 가져올 수 있습니다. 이렇게 하면 서비스를 통해 앱에 지원하는 광고를 더 효과적으로 최적화할 수 있습니다.

> [!IMPORTANT]
> 앱별로 하나의 광고 단위만 사용할 수 있습니다. 특정 광고 단위를 하나 이상의 앱에 사용하면, 해당 광고 단위에 광고가 제공되지 않습니다.

## <a name="related-topics"></a>관련 항목

* [XAML 및 .NET의 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 및 Javascript의 AdControl](adcontrol-in-html-5-and-javascript.md)
* [중간 광고](interstitial-ads.md)
* [기본 광고](native-ads.md)


 

 
