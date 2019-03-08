---
Description: 개발자 앱을 다른 개발자가 게시한 앱과 함께 서로 홍보할 수 있습니다. 이 기능을 커뮤니티 광고라고 합니다.
title: 커뮤니티 광고 정보
ms.assetid: F55CE478-99AF-4B70-90D1-D16419562136
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f8acf83e2b39ece5fcd46c3d89d921e4f3013b67
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635788"
---
# <a name="about-community-ads"></a>커뮤니티 광고 정보

하는 경우 앱 [배너 또는 중간 광고 배너를 표시](../monetize/display-ads-in-your-app.md), 크로스-수준을 올릴 수 있으며 앱 Microsoft Store 앱을 사용 하 여 다른 개발자와 무료입니다. 이 기능을 *커뮤니티 광고*라고 합니다.  

이 프로그램이 작동하는 방법은 다음과 같습니다.

* 옵트인 할 커뮤니티 광고를 아래 설명 된 대로, 수행할 수 있습니다 [무료 커뮤니티 ad 캠페인 만들기](create-an-ad-campaign-for-your-app.md)합니다. 앱이 커뮤니티 광고를 선택할 수도 있는 다른 개발자 홍보 ad 공간을 공유할 됩니다. 개발자 앱에는 커뮤니티 광고에 참여하는 다른 개발자가 게시한 앱의 광고가 표시되고 개발자 앱의 광고가 다른 개발자의 앱에 표시됩니다.
* 앱에 커뮤니티 광고를 표시하여 다른 앱에서 홍보 광고 공간에 대한 크레딧을 획득합니다. 다음 프로세스에 따라 크레딧이 계산됩니다.
  * 커뮤니티 광고를 제공하는 앱을 사용할 수 있는 각 국가 또는 지역의 경우 국가 또는 지역의 현재 시장 환율 eCPM(유효 1,000회 노출당 비용) 값에 해당 국가 또는 지역에서 앱이 제공하는 커뮤니티 광고에 대한 요청 수를 곱합니다. 이 값은 해당 국가 또는 지역에서 앱으로 획득한 크레딧입니다.
  * 지정된 기간 내에 획득한 총 크레딧은 각 국가 또는 지역에서 커뮤니티 광고를 제공하는 각 앱으로 획득한 모든 크레딧의 합과 같습니다.
* 크레딧은 운영 중인 모든 커뮤니티 광고 캠페인에서 공평하게 분배되며 커뮤니티 광고 캠페인 대상 국가의 현재 시장 환율 eCPM 값에 따라 앱에 대한 광고 노출로 변환됩니다.
* 앱에서 커뮤니티 광고의 성과를 추적하려면 [광고 성과 보고서](advertising-performance-report.md)를 참조하세요.

### <a name="opt-in-to-community-ads"></a>커뮤니티 광고에 옵트인(opt in)

에 옵트인 해야 앱 중 하나에 대 한 커뮤니티 광고 캠페인을 만들 수 있습니다, 전에 **Monetize** &gt; **앱 내 광고** 페이지에서 [파트너 센터](https://partner.microsoft.com/dashboard)합니다.

UWP 앱에 대 한 커뮤니티 광고에 옵트인 합니다.

1. 앱에서 사용 하 고 아래로 스크롤하여는 ad 단위를 선택 **중재 설정**합니다.
2. 하는 경우 **Microsoft에 알려주세요 내 설정을 최적화** 는 선택한 커뮤니티 광고 ad 단위에 대 한 자동으로 설정 됩니다. 이 고, 그렇지 기준 구성 또는에서 특정 시장 구성을 선택 합니다 **대상** 드롭 다운 한 다음 확인는 **Microsoft 커뮤니티 광고** 상자에 **다른 광고 네트워크** 목록입니다.

    > [!NOTE]
    > 사용할 수는 **가중치** 유료 네트워크에서 커뮤니티 광고를 포함 하 여 다른 광고 네트워크 표시할 광고의 비율을 지정 하는 필드입니다.

선택을 완료한 후 앱을 다시 게시 필요가 없습니다. 옵트인을 하면 [광고 캠페인을 만들](create-an-ad-campaign-for-your-app.md) 때 캠페인 유형으로 **커뮤니티 광고(무료)** 를 선택할 수 있습니다.

### <a name="related-topics"></a>관련 항목

* [앱 내 광고](in-app-ads.md)
* [앱에 대 한는 광고 캠페인 만들기](create-an-ad-campaign-for-your-app.md)
