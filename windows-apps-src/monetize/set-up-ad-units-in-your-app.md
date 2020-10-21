---
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: 앱을 스토어에 제출 하기 전에 파트너 센터에서 응용 프로그램 ID 및 ad 단위 ID 값을 앱에 추가 하는 방법에 대해 알아봅니다.
title: 앱에서 광고 단위 설정
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, ad 단위, 테스트
ms.localizationpriority: medium
ms.openlocfilehash: c7bafdc7d21814a03d6f7da7132d8017d7f238e5
ms.sourcegitcommit: c2e4bbe46c7b37be1390cdf3fa0f56670f9d34e9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92253634"
---
# <a name="set-up-ad-units-in-your-app"></a>앱에서 광고 단위 설정

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세히 알아보기](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

UWP (유니버설 Windows 플랫폼) 앱의 모든 ad 컨트롤에는 서비스에서 컨트롤에 대 한 광고를 제공 하는 데 사용 하는 해당 하는 *ad 단위가* 있습니다. 각 ad 단위는 앱의 코드에 할당 해야 하는 *ad 단위 id* 와 *응용 프로그램 id* 로 구성 됩니다.

테스트 중에 앱이 테스트 광고를 표시 하는지 확인 하는 데 사용할 수 있는 [테스트 ad 단위 값](#test-ad-units) 을 제공 합니다. 이러한 테스트 값은 응용 프로그램의 테스트 버전 에서만 사용할 수 있습니다. 앱을 게시 한 후에 앱에서 테스트 값을 사용 하려고 하면 라이브 앱이 광고를 수신 하지 않습니다.

UWP 앱 테스트를 완료 하 고 파트너 센터에 제출할 준비가 되 면 파트너 센터의 [앱 내 광고](../publish/in-app-ads.md) 페이지에서 [live ad 단위를 만들고](#live-ad-units) 이 ad 단위에 대 한 응용 프로그램 ID 및 ad unit id 값을 사용 하도록 앱 코드를 업데이트 해야 합니다.

응용 프로그램 코드에서 응용 프로그램 ID 및 ad 단위 ID 값을 할당 하는 방법에 대 한 자세한 내용은 다음 문서를 참조 하세요.
* [XAML 및 .NET의 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 및 Javascript의 AdControl](adcontrol-in-html-5-and-javascript.md)
* [중간 광고](../monetize/interstitial-ads.md)
* [기본 광고](../monetize/native-ads.md)

<span id="test-ad-units" />

## <a name="test-ad-units"></a>테스트 ad 단위

앱을 개발 하는 동안 테스트 하는 동안 앱이 광고를 렌더링 하는 방법을 보려면이 섹션의 응용 프로그램 ID 및 ad 단위 ID 값 테스트를 사용 합니다.

### <a name="banner-ads-using-the-adcontrol-class"></a>배너 광고 (AdControl 클래스 사용)

* Ad 유닛 ID: ```test```
* 응용 프로그램 ID:  ```3f83fe91-d6be-434d-a0ae-7351c5a997f1```

    > [!IMPORTANT]
    > **Adcontrol**의 경우 라이브 광고의 크기는 **Width** 및 **Height** 속성으로 정의 됩니다. 최상의 결과를 위해 코드의 **Width** 및 **Height** 속성이 [배너 광고에 대해 지원 되는 ad 크기](supported-ad-sizes-for-banner-ads.md)중 하나 인지 확인 합니다. **Width** 및 **Height** 속성은 라이브 광고의 크기에 따라 변경 되지 않습니다.

### <a name="interstitial-ads-and-native-ads"></a>중간 광고 및 네이티브 광고

* Ad 유닛 ID: ```test```
* 응용 프로그램 ID:  ```d25517cb-12d4-4699-8bdc-52040c712cab```

<span id="live-ad-units" />

## <a name="live-ad-units"></a>라이브 광고 단위

파트너 센터에서 라이브 ad 단위를 가져와서 앱에서 사용 하려면:

1.  파트너 센터의 **앱 내 광고** 페이지에서 [Ad 단위를 만듭니다](../publish/in-app-ads.md#create-ad-unit) . 앱에서 사용 하는 ad 컨트롤의 올바른 ad 단위 유형을 지정 해야 합니다.
    > [!NOTE]
    > 필요에 따라 [중재 설정](../publish/in-app-ads.md#mediation) 섹션에서 설정을 구성 하 여 ad 장치에 대 한 ad 중재를 사용 하도록 설정할 수 있습니다. Ad 중재를 사용 하면 다른 유료 ad 네트워크의 광고 및 Microsoft 앱 홍보 행사 광고를 비롯 한 여러 ad 네트워크의 광고를 표시 하 여 ad 수익과 앱 승격 기능을 최대화할 수 있습니다. 기본적으로 앱이 지 원하는 시장에서 광고 수익을 극대화 하는 데 도움이 되는 기계 학습 알고리즘을 사용 하 여 앱에 대 한 ad 중재 설정을 자동으로 선택 하지만 필요에 따라 중재 설정을 수동으로 구성할 수 있습니다.

2.  새 ad 단위를 만든 후 **수익 창출** ads 페이지에서 사용 가능한 ad 단위 테이블의 ad 단위에 대 한 **응용 프로그램 id** 및 **ad 단위 id** 를 검색 합니다 &gt; **In-app ads** .
    > [!NOTE]
    > 테스트 ad 단위와 라이브 UWP ad 단위의 응용 프로그램 ID 값은 서로 다른 형식입니다. 테스트 응용 프로그램 ID 값은 Guid입니다. 파트너 센터에서 라이브 UWP ad 단위를 만들 때 ad 단위의 응용 프로그램 ID 값은 항상 앱에 대 한 저장소 ID와 일치 합니다. 예를 들어 매장 ID 값은 9NBLGGH4R315와 같습니다.

3.  응용 프로그램의 코드에 응용 프로그램 ID 및 ad 단위 ID 값을 할당 합니다. 자세한 내용은 다음 항목을 참조하세요.
    * [XAML 및 .NET의 AdControl](adcontrol-in-xaml-and--net.md)
    * [HTML 5 및 Javascript의 AdControl](adcontrol-in-html-5-and-javascript.md)
    * [중간 광고](../monetize/interstitial-ads.md)
    * [기본 광고](../monetize/native-ads.md)

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>앱에서 여러 ad 컨트롤의 ad 단위 관리

단일 앱에서 여러 배너, 중간 및 네이티브 ad 컨트롤을 사용할 수 있습니다. 이 시나리오에서는 각 컨트롤에 다른 ad 단위를 할당 하는 것이 좋습니다. 각 컨트롤에 대해 서로 다른 ad 단위를 사용 하면 [중재 설정을](../publish/in-app-ads.md#mediation) 개별적으로 구성 하 고 각 컨트롤에 대 한 개별 [보고 데이터](../publish/advertising-performance-report.md) 를 가져올 수 있습니다. 이를 통해 서비스는 앱에 제공 하는 광고를 더 효율적으로 최적화할 수 있습니다.

> [!IMPORTANT]
> 하나의 앱 에서만 각 ad 단위를 사용할 수 있습니다. 둘 이상의 앱에서 ad 단위를 사용 하는 경우 해당 ad 단위에 대 한 광고가 제공 되지 않습니다.

## <a name="related-topics"></a>관련 항목

* [XAML 및 .NET의 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 및 Javascript의 AdControl](adcontrol-in-html-5-and-javascript.md)
* [중간 광고](interstitial-ads.md)
* [기본 광고](native-ads.md)


 

 
