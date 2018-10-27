---
author: jnHs
Description: You can cross-promote your app with apps published by other developers. We call this feature community ads.
title: 커뮤니티 광고 정보
ms.assetid: F55CE478-99AF-4B70-90D1-D16419562136
ms.author: wdg-dev-content
ms.date: 10/04/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 20f0f06d0927d61c062a0514d84bef993da6e932
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5704474"
---
# <a name="about-community-ads"></a>커뮤니티 광고 정보

경우에 앱 [배너 또는 배너 중간 광고를 표시](../monetize/display-ads-in-your-app.md)하면 홍보할 수 앱에 대 한 Microsoft 스토어에서 다른 개발자와 무료입니다. 이 기능을 *커뮤니티 광고*라고 합니다.  

이 프로그램이 작동하는 방법은 다음과 같습니다.

* 후 하면 옵트인 커뮤니티 광고 아래에 설명 된 대로, [무료 커뮤니티 광고 캠페인을 만들](create-an-ad-campaign-for-your-app.md)수 있습니다. 앱은 또한 커뮤니티 광고에 옵트인 한 다른 개발자와 홍보용 광고 공간을 공유 합니다. 개발자 앱에는 커뮤니티 광고에 참여하는 다른 개발자가 게시한 앱의 광고가 표시되고 개발자 앱의 광고가 다른 개발자의 앱에 표시됩니다.
* 앱에 커뮤니티 광고를 표시하여 다른 앱에서 홍보 광고 공간에 대한 크레딧을 획득합니다. 다음 프로세스에 따라 크레딧이 계산됩니다.
  * 커뮤니티 광고를 제공하는 앱을 사용할 수 있는 각 국가 또는 지역의 경우 국가 또는 지역의 현재 시장 환율 eCPM(유효 1,000회 노출당 비용) 값에 해당 국가 또는 지역에서 앱이 제공하는 커뮤니티 광고에 대한 요청 수를 곱합니다. 이 값은 해당 국가 또는 지역에서 앱으로 획득한 크레딧입니다.
  * 지정된 기간 내에 획득한 총 크레딧은 각 국가 또는 지역에서 커뮤니티 광고를 제공하는 각 앱으로 획득한 모든 크레딧의 합과 같습니다.
* 크레딧은 운영 중인 모든 커뮤니티 광고 캠페인에서 공평하게 분배되며 커뮤니티 광고 캠페인 대상 국가의 현재 시장 환율 eCPM 값에 따라 앱에 대한 광고 노출로 변환됩니다.
* 앱에서 커뮤니티 광고의 성과를 추적하려면 [광고 성과 보고서](advertising-performance-report.md)를 참조하세요.

### <a name="opt-in-to-community-ads"></a>커뮤니티 광고에 옵트인(opt in)

**수익 창출** 에 옵트인 해야 앱 중 하나에 대 한 커뮤니티 광고 캠페인을 만들면 전에 &gt; Windows 개발자 센터 대시보드의 **인 앱 광고** 페이지.

UWP 앱에 대 한 커뮤니티 광고에 옵트인 하려면:

1. **인 앱 광고** 페이지에서 **조정 설정** 섹션에서는 앱에서 사용 중인 광고 단위를 선택 합니다.
2. **Microsoft는 앱에 대 한 가장 좋은 조정 설정을 선택** 옵션을 선택 하는 경우 광고 단위에 대 한 커뮤니티 광고 자동으로 활성화 됩니다. 그렇지 않은 경우 **대상** 드롭다운 목록에서 기준 구성 또는 지역/국가 특정 구성을 선택 하 고 **다른 광고 네트워크** 목록에서 **Microsoft 커뮤니티 광고** 확인란을 선택 합니다.

    > [!NOTE]
    > 유료 네트워크 및 커뮤니티 광고를 포함 하 여 다른 광고 네트워크에서 표시 하려는 광고의 비율을 지정 하려면 **가중치** 필드를 사용할 수 있습니다.

Windows에 대 한 커뮤니티 광고에 옵트인 하려면 8.x 또는 Windows Phone 8.x 앱

1. **인 앱 광고** 페이지에서 **내 앱에 커뮤니티 광고 표시** 상자를 선택 합니다.

선택을 완료한 후 앱을 다시 게시 필요가 없습니다. 옵트인을 하면 [광고 캠페인을 만들](create-an-ad-campaign-for-your-app.md) 때 캠페인 유형으로 **커뮤니티 광고(무료)** 를 선택할 수 있습니다.

### <a name="related-topics"></a>관련 항목

* [인앱 광고](in-app-ads.md)
* [앱 광고 캠페인 만들기](create-an-ad-campaign-for-your-app.md)
