---
Description: 개발자 앱을 다른 개발자가 게시한 앱과 함께 서로 홍보할 수 있습니다. 이 기능을 커뮤니티 광고라고 합니다.
title: 커뮤니티 광고 정보
ms.assetid: F55CE478-99AF-4B70-90D1-D16419562136
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8da380cb49d584e56f584f3ad321d601d211faf0
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463445"
---
# <a name="about-community-ads"></a>커뮤니티 광고 정보

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 내용](https://aka.ms/ad-monetization-shutdown)

앱이 [배너 또는 배너 중간 광고를 표시](../monetize/display-ads-in-your-app.md)하는 경우 Microsoft Store의 앱을 사용 하는 다른 개발자와 앱을 동시에 승격 시킬 수 있습니다. 이 기능을 *커뮤니티 광고*라고 합니다.  

이 프로그램이 작동하는 방법은 다음과 같습니다.

* 아래에 설명 된 대로 커뮤니티 광고를 옵트인 한 후에 [는 무료 커뮤니티 ad 캠페인을 만들](create-an-ad-campaign-for-your-app.md)수 있습니다. 그러면 앱이 프로 모션 ad 공간을 커뮤니티 광고에 참여 하는 다른 개발자와 공유할 수 있습니다. 개발자 앱에는 커뮤니티 광고에 참여하는 다른 개발자가 게시한 앱의 광고가 표시되고 개발자 앱의 광고가 다른 개발자의 앱에 표시됩니다.
* 앱에 커뮤니티 광고를 표시하여 다른 앱에서 홍보 광고 공간에 대한 크레딧을 획득합니다. 다음 프로세스에 따라 크레딧이 계산됩니다.
  * 커뮤니티 광고를 제공하는 앱을 사용할 수 있는 각 국가 또는 지역의 경우 국가 또는 지역의 현재 시장 환율 eCPM(유효 1,000회 노출당 비용) 값에 해당 국가 또는 지역에서 앱이 제공하는 커뮤니티 광고에 대한 요청 수를 곱합니다. 이 값은 해당 국가 또는 지역에서 앱으로 획득한 크레딧입니다.
  * 지정된 기간 내에 획득한 총 크레딧은 각 국가 또는 지역에서 커뮤니티 광고를 제공하는 각 앱으로 획득한 모든 크레딧의 합과 같습니다.
* 크레딧은 운영 중인 모든 커뮤니티 광고 캠페인에서 공평하게 분배되며 커뮤니티 광고 캠페인 대상 국가의 현재 시장 환율 eCPM 값에 따라 앱에 대한 광고 노출로 변환됩니다.
* 앱에서 커뮤니티 광고의 성과를 추적하려면 [광고 성과 보고서](advertising-performance-report.md)를 참조하세요.

### <a name="opt-in-to-community-ads"></a>커뮤니티 광고에 옵트인(opt in)

앱 중 하나에 대 한 커뮤니티 ad 캠페인을 만들려면 먼저 [파트너 센터](https://partner.microsoft.com/dashboard)의 **수익 창출** &gt; **앱 내 광고** 페이지에서 옵트인 (opt in) 해야 합니다.

UWP 앱에 대 한 커뮤니티 광고를 옵트인 (opt in) 하려면 다음을 수행 합니다.

1. 앱에서 사용 중인 ad 단위를 선택 하 고 아래로 스크롤하여 **중재 설정**으로 이동 합니다.
2. Microsoft에서 [ **내 설정 최적화** ]를 선택 하면 커뮤니티 광고가 자동으로 ad 유닛에 사용 됩니다. 그렇지 않으면 **대상** 드롭다운에서 기준 구성 또는 시장 관련 구성을 선택 하 고 **다른 ad 네트워크** 목록에서 **Microsoft 커뮤니티 광고** 상자를 선택 합니다.

    > [!NOTE]
    > **가중치** 필드를 사용 하 여 유료 네트워크와 커뮤니티 광고를 비롯 한 다른 ad 네트워크에서 표시 하려는 광고 비율을 지정할 수 있습니다.

선택을 완료한 후 앱을 다시 게시 필요가 없습니다. 옵트인을 하면 **광고 캠페인을 만들** 때 캠페인 유형으로 [커뮤니티 광고(무료)](create-an-ad-campaign-for-your-app.md)를 선택할 수 있습니다.

### <a name="related-topics"></a>관련 항목

* [앱 내 광고](in-app-ads.md)
* [앱에 대 한 광고 캠페인 만들기](create-an-ad-campaign-for-your-app.md)
