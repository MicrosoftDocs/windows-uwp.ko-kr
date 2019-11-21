---
ms.assetid: 4e8cc0c0-b14c-472c-9e1c-4601d10289d2
description: Windows SDK, Microsoft Advertising SDK, Microsoft Store Services SDK 및 Microsoft Store는 앱에서 더 큰 수익을 창출하고 사용자 참여를 통해 고객을 확보할 수 있게 해주는 다양한 기능을 제공합니다.
title: 수익 창출, 참여 및 Microsoft Store 서비스
ms.date: 11/29/2017
ms.topic: article
keywords: Windows 10, uwp, 수익 창출, 참여, 프로모션, Microsoft Store 서비스
ms.localizationpriority: medium
ms.openlocfilehash: 7beee974bceceab02984ae6499a9c5db0b0281b9
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259298"
---
# <a name="monetization-engagement-and-store-services"></a>수익 창출, 참여 및 Microsoft Store 서비스

Windows SDK, Microsoft Advertising SDK, Microsoft Store Services SDK 및 Microsoft Store는 앱에서 더 큰 수익을 창출하고 사용자 참여를 통해 고객을 확보할 수 있게 해주는 다양한 기능을 제공합니다. 이 섹션의 항목에서는 이러한 기능을 앱에 빌드하는 방법을 보여 줍니다.

Microsoft Store에서 청구하는 수수료 및 앱으로 번 돈을 지급 받는 방법에 대한 자세한 내용은 [지급 받기](../publish/getting-paid-apps.md)를 참조하세요.

## <a name="choose-a-pricing-model"></a>가격 모델 선택

:::row:::
    :::column:::
        ![앱에 대한 요금 청구](images/pricing-charge-price.png)
    :::column-end:::
    :::column span="2":::
**앱에 대한 요금 청구**

앱에 대한 요금을 사전에 청구할 수 있습니다. 시장별 요금을 설정하는 옵션을 포함하여 광범위한 범위의 기준 가격을 지원합니다. 제한된 시간 동안 앱의 가격을 낮추는 할인 판매도 예약할 수 있습니다.

[앱 가격 책정 설정](../publish/set-app-pricing-and-availability.md)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![무료 평가판](images/pricing-free-trial.png)
    :::column-end:::
    :::column span="2":::
**무료 평가판**

더 많은 고객이 체험해 볼 수 있도록 무료 평가판 버전의 앱을 제공할 수 있습니다. 고객이 정품 버전을 구입하도록 유도하기 위해 게임의 첫 번째 단계만 포함하는 등 평가판 버전의 기능을 제한하고, 광고를 표시하거나, 제한된 시간 동안만 이용 가능하도록 지정할 수 있습니다.

[평가판 제공](in-app-purchases-and-trials.md)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![앱에서 바로 구매](images/pricing-in-app-purchases.png)
    :::column-end:::
    :::column span="2":::
**앱에서 바로 구매**

앱에 대한 비용을 청구하든 무료로 제공하든 앱에서 바로 구매를 사용하여 지속적인 매출을 올릴 수 있습니다. 앱에서 바로 구매를 사용하여 고객이 무료 버전에서 유료 버전으로 앱을 업그레이드하거나 앱 내에서 지속적이거나 사용할 수 있는 유료 추가 기능을 제공할 수 있습니다.

[앱에서 바로 구매 사용](in-app-purchases-and-trials.md)
    :::column-end:::
:::row-end:::

## <a name="monetize-your-app-with-ads"></a>광고로 앱 수익 창출

:::row:::
    :::column:::
        ![모든 상황에 맞는 광고](images/monetize-ads-every-context.png)
    :::column-end:::
    :::column span="2":::
**모든 상황에 맞는 광고**

배너 광고, 중간 광고(배너 및 동영상), 선형 비디오 광고, 재생 가능한 광고, 기본 광고 등 대부분의 요구에 맞는 다양한 광고 환경을 지원합니다. 이 플랫폼은 OpenRTB, VAST 2.x, MRAID 2 및 VPAID 3 표준을 준수하며 MOAT 및 IAS와 호환됩니다.

[광고 옵션 살펴보기](../publish/create-an-ad-campaign-for-your-app.md)
[광고 SDK 설치](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![광고 조정 서비스](images/monetize-ad-mediation-service.png)
    :::column-end:::
    :::column span="2":::
**광고 조정 서비스**

여러 인기 광고 네트워크에서 광고를 제공하도록 Microsoft 광고 조정 서비스를 사용하여 앱의 광고 수익을 최대화합니다. 코드 줄을 터치하지 않고 파트너 센터 대시보드에서 조정 설정을 구성할 수 있습니다. 조정을 Windows에서 구성하도록 하면 기계 학습 알고리즘이 앱에서 지원하는 시장에서 광고 수익을 최대화하는 데 도움이 됩니다.

[광고 서비스 사용](https://blogs.windows.com/windowsdeveloper/2017/05/08/announcing-microsofts-ad-mediation-service/)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![분석](images/monetize-analytics-pie-chart.png)
    :::column-end:::
    :::column span="2":::
**분석**

자세한 분석 보고서를 통해 앱에서 광고를 수행하는 방법을 확인할 수 있으며 필요한 정보를 제공하여 광고 수익을 극대화합니다. 또한 프로그래밍 방식으로 이 데이터를 가져오는 데 사용할 수 있는 RESTful API를 제공합니다.

[성과 검토](../publish/advertising-performance-report.md)
    :::column-end:::
:::row-end:::

## <a name="other-monetization-opportunities"></a>다른 수익 창출 기회

![다른 수익 창출 기회](images/monetize-other-opportunities.png)

수익 창출을 높일 다른 방법을 찾고 계신가요? 이러한 옵션을 고려해 보세요.

 항목                | 설명                 |
|--------------------|-----------------------------|
| [Microsoft 제휴 프로그램](https://www.microsoftaffiliates.com/) | 앱, 블로그, 웹 페이지 또는 기타 커뮤니케이션에서 Microsoft 제품으로 연결되도록 하고 수수료를 받으세요. Microsoft Store에서 판매하는 앱, 게임, 음악, 동영상, 하드웨어, 액세서리 및 기타 상품을 연결할 수 있습니다.
| [A/B 실험](https://docs.microsoft.com/windows/uwp/monetize/run-app-experiments-with-a-b-testing) | 모든 고객에 대해 기능을 변경하기 전에 앱에서 A/B 테스트를 실행하여 일부 고객에 대한 기능 변경 효과를 측정합니다.
| [Microsoft Store Services SDK를 사용하여 고객과 소통](microsoft-store-services-sdk.md) | Microsoft Store Services SDK는 고객과의 소통을 도와주는 기능을 앱에 추가할 때 사용하는 라이브러리 및 도구를 제공합니다. 이러한 기능으로는 대상 알림, A/B 테스트, 앱에서 피드백 허브 시작이 포함됩니다.
| [앱에서 피드백 허브 시작](launch-feedback-hub-from-your-app.md) | UWP 앱에 코드를 추가하여 문제, 제안 및 좋아요를 제출할 수 있는 피드백 허브로 Windows 10 고객을 안내합니다. 그런 다음, 파트너 센터의 [피드백 보고서](../publish/feedback-report.md)에서 이 피드백을 관리합니다. 이 기능은 Microsoft Store Services SDK가 필요합니다. 
| [파트너 센터 푸시 알림을 받도록 앱 구성](configure-your-app-to-receive-dev-center-notifications.md) | [파트너 센터 푸시 알림](../publish/send-push-notifications-to-your-apps-customers.md)을 수신할 수 있도록 UWP 앱용 알림 채널을 등록하고, 푸시 알림을 통해 앱 실행 속도 결과를 추적합니다. 이 기능은 Microsoft Store Services SDK가 필요합니다.
| [파트너 센터의 사용자 지정 이벤트 로깅](log-custom-events-for-dev-center.md) | UWP 앱의 사용자 지정 이벤트를 기록하고 파트너 센터의 [사용 보고서](../publish/usage-report.md)에서 이벤트를 검토합니다. 이 기능은 Microsoft Store Services SDK가 필요합니다.
| [평점 및 리뷰 요청](request-ratings-and-reviews.md) | 평점 및 리뷰 UI를 프로그래밍 방식으로 표시하여 앱에 대한 평점이나 리뷰를 남기도록 고객을 독려합니다.
| [Microsoft Store 서비스](using-windows-store-services.md) | RESTful API를 사용하여 스토어 제출을 자동화하고 앱 분석 데이터에 액세스하며 기타 스토어 관련 작업을 자동화하는 방법을 알아봅니다.
| [앱에 소매 데모(RDX) 기능 추가](retail-demo-experience.md) | 매장에서 PC 및 디바이스를 사용해 보려는 고객이 바로 시작할 수 있도록 Windows 앱에 소매 데모 모드를 포함하세요.

## <a name="monetization-analytics"></a>수익 창출 분석

![수익 창출 분석](images/monetize-analytics.png)

이러한 보고서를 사용하여 Microsoft Store에서 앱이 기능하는 방식을 지속적으로 감시합니다.

- [지급 요약](../publish/payout-summary.md)
- [취득 보고서](../publish/acquisitions-report.md)
- [추가 기능 구입 보고서](../publish/add-on-acquisitions-report.md)
- [광고 성과 보고서](../publish/advertising-performance-report.md)
- [REST API를 사용하여 분석 데이터 가져오기](access-analytics-data-using-windows-store-services.md)
- [고객층 만들기](../publish/create-customer-segments.md)
- [피드백 보고서](../publish/feedback-report.md)
- [사용 보고서](../publish/usage-report.md)