---
author: jnHs
Description: You can cross-promote your app with apps published by other developers. We call this feature community ads.
title: 커뮤니티 광고 정보
ms.assetid: F55CE478-99AF-4B70-90D1-D16419562136
ms.author: wdg-dev-content
ms.date: 10/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ada2e0610e81e986deba3ddab5e87547e05fe108
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/22/2018
ms.locfileid: "2788230"
---
# <a name="about-community-ads"></a>커뮤니티 광고 정보

하는 경우에 app [배너 또는 배너 중간 광고를 표시 하는](../monetize/display-ads-in-your-app.md), 승격할 수 있습니다 크로스-앱에 대 한 Microsoft 스토어에서 앱와 다른 개발자와 무료 합니다. 이 기능을 *커뮤니티 광고*라고 합니다.  

이 프로그램이 작동하는 방법은 다음과 같습니다.

* 후 하면 옵트인 커뮤니티 광고 아래 설명에 따라, [무료 커뮤니티 ad 캠페인을 만들](create-an-ad-campaign-for-your-app.md)수 있습니다. 그런 다음 앱 커뮤니티 광고에도 옵트인는 다른 개발자와 프로 모션 ad 공간을 공유 합니다. 개발자 앱에는 커뮤니티 광고에 참여하는 다른 개발자가 게시한 앱의 광고가 표시되고 개발자 앱의 광고가 다른 개발자의 앱에 표시됩니다.
* 앱에 커뮤니티 광고를 표시하여 다른 앱에서 홍보 광고 공간에 대한 크레딧을 획득합니다. 다음 프로세스에 따라 크레딧이 계산됩니다.
  * 커뮤니티 광고를 제공하는 앱을 사용할 수 있는 각 국가 또는 지역의 경우 국가 또는 지역의 현재 시장 환율 eCPM(유효 1,000회 노출당 비용) 값에 해당 국가 또는 지역에서 앱이 제공하는 커뮤니티 광고에 대한 요청 수를 곱합니다. 이 값은 해당 국가 또는 지역에서 앱으로 획득한 크레딧입니다.
  * 지정된 기간 내에 획득한 총 크레딧은 각 국가 또는 지역에서 커뮤니티 광고를 제공하는 각 앱으로 획득한 모든 크레딧의 합과 같습니다.
* 크레딧은 운영 중인 모든 커뮤니티 광고 캠페인에서 공평하게 분배되며 커뮤니티 광고 캠페인 대상 국가의 현재 시장 환율 eCPM 값에 따라 앱에 대한 광고 노출로 변환됩니다.
* 앱에서 커뮤니티 광고의 성과를 추적하려면 [광고 성과 보고서](advertising-performance-report.md)를 참조하세요.

### <a name="opt-in-to-community-ads"></a>커뮤니티 광고에 옵트인(opt in)

**Monetize** 에 옵트인 해야 앱 중 하나에 대 한 커뮤니티 ad 캠페인을 만들 수 있습니다, 전에 &gt; Windows 개발자 센터 대시보드에서 **app에서 ads** 페이지입니다.

커뮤니티 광고 UWP 응용 프로그램에 대 한 옵트인을:

1. **응용 프로그램에서 ads** 페이지에서 **중재 설정** 섹션에서 응용 프로그램에서 사용 하 여 ad 단위를 선택 합니다.
2. **수 있도록 Microsoft 응용 프로그램에 대 한 최상의 중재 설정을 선택** 옵션을 선택 하는 경우 ad 단위에 대 한 커뮤니티 ads 자동으로 활성화 됩니다. 그렇지 않은 경우 초기 구성 또는 시장 관련 구성 **대상** 드롭다운 목록에서 선택한 다음 **다른 ad 네트워크** 목록에서 **Microsoft 커뮤니티 ads** 상자를 선택 합니다.

    > [!NOTE]
    > 유료 네트워크 및 커뮤니티 광고를 포함 하 여 다른 ad 네트워크에서 표시 하려는 광고의 비율을 지정 하려면 **가중치** 필드를 사용할 수 있습니다.

Windows에 대 한 커뮤니티 광고에 참여 하도록 8.x 또는 Windows Phone 8.x 앱

1. **응용 프로그램에서 ads** 페이지에서 **응용 프로그램 내에서 커뮤니티 광고를 표시** 상자를 선택 합니다.

선택을 완료한 후 앱을 다시 게시 필요가 없습니다. 옵트인을 하면 [광고 캠페인을 만들](create-an-ad-campaign-for-your-app.md) 때 캠페인 유형으로 **커뮤니티 광고(무료)** 를 선택할 수 있습니다.

### <a name="related-topics"></a>관련 항목

* [인앱 광고](in-app-ads.md)
* [앱 광고 캠페인 만들기](create-an-ad-campaign-for-your-app.md)
