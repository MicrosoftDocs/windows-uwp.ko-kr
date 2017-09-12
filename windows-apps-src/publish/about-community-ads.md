---
author: jnHs
Description: "개발자 앱을 다른 개발자가 게시한 앱과 함께 서로 홍보할 수 있습니다. 이 기능을 커뮤니티 광고라고 합니다."
title: "커뮤니티 광고 정보"
ms.assetid: F55CE478-99AF-4B70-90D1-D16419562136
ms.author: wdg-dev-content
ms.date: 07/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: c16f474fe8242e2994350f5261c26c7148aea5e4
ms.sourcegitcommit: 10f8dcf69d37cdb61562fc9f4d268ccb499c368f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/07/2017
---
# <a name="about-community-ads"></a>커뮤니티 광고 정보

[배너 또는 배너 중간 광고를 표시하는](../monetize/display-ads-in-your-app.md) 앱을 사용하고 있다면, Windows 스토어에서 무료로 다른 개발자의 앱과 함께 서로 홍보할 수 있습니다. 이 기능을 *커뮤니티 광고*라고 합니다.  

이 프로그램이 작동하는 방법은 다음과 같습니다.

* [커뮤니티 광고에 옵트인(opt in)](#how-to-opt-in-to-community-ads)하고 [무료 커뮤니티 광고 캠페인을 만들면](create-an-ad-campaign-for-your-app.md) 앱은 커뮤니티 광고에 옵트인(opt in)한 다른 개발자와 홍보용 광고 공간을 공유합니다. 개발자 앱에는 커뮤니티 광고에 참여하는 다른 개발자가 게시한 앱의 광고가 표시되고 개발자 앱의 광고가 다른 개발자의 앱에 표시됩니다.
* 앱에 커뮤니티 광고를 표시하여 다른 앱에서 홍보 광고 공간에 대한 크레딧을 획득합니다. 다음 프로세스에 따라 크레딧이 계산됩니다.
  * 커뮤니티 광고를 제공하는 앱을 사용할 수 있는 각 국가 또는 지역의 경우 국가 또는 지역의 현재 시장 환율 eCPM(유효 1,000회 노출당 비용) 값에 해당 국가 또는 지역에서 앱이 제공하는 커뮤니티 광고에 대한 요청 수를 곱합니다. 이 값은 해당 국가 또는 지역에서 앱으로 획득한 크레딧입니다.
  * 지정된 기간 내에 획득한 총 크레딧은 각 국가 또는 지역에서 커뮤니티 광고를 제공하는 각 앱으로 획득한 모든 크레딧의 합과 같습니다.
* 크레딧은 운영 중인 모든 커뮤니티 광고 캠페인에서 공평하게 분배되며 커뮤니티 광고 캠페인 대상 국가의 현재 시장 환율 eCPM 값에 따라 앱에 대한 광고 노출로 변환됩니다.
* 앱에서 커뮤니티 광고의 성과를 추적하려면 [광고 성과 보고서](advertising-performance-report.md)를 참조하세요.

### <a name="opt-in-to-community-ads"></a>커뮤니티 광고에 옵트인(opt in)

Windows 개발자 센터 대시보드에서 앱에 대한 **수익 창출** &gt; **광고로 수익 창출** 페이지에 옵트인해야 앱 중 하나에 대한 커뮤니티 광고 캠페인을 만들 수 있습니다.

옵트인 하려면 다음 중 하나를 수행합니다.
  * Windows 10을 대상으로 하는 UWP 앱이라면 페이지의 **광고 조정** 섹션으로 가서 **Microsoft 커뮤니티 광고** 상자(**다른 광고 네트워크** 목록)에 표시하세요.
  * Windows 8.x나 Windows Phone 8.x를 대상으로 하는 앱이라면 페이지의 **커뮤니티 광고** 섹션으로 가서 **내 앱에 커뮤니티 광고 표시** 상자에 표시하세요.

선택을 완료한 후 앱을 다시 게시 필요가 없습니다. 옵트인을 하면 [광고 캠페인을 만들](create-an-ad-campaign-for-your-app.md) 때 캠페인 유형으로 **커뮤니티 광고(무료)**를 선택할 수 있습니다.

### <a name="related-topics"></a>관련 항목

* [광고를 통한 수익 창출](monetize-with-ads.md)
* [앱 광고 캠페인 만들기](create-an-ad-campaign-for-your-app.md)
