---
Description: 앱에서 ad 단위에 대 한 성능 데이터를 보려면 파트너 센터의 광고 성능 보고서를 사용 합니다.
title: 광고 성과 보고서
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bb9c6a43b9411b2297cc4cbf35dfdb2177e5dab7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155377"
---
# <a name="advertising-performance-report"></a>광고 성과 보고서


[파트너 센터](https://partner.microsoft.com/dashboard) 의 **광고 성능 보고서** 에는 커뮤니티 광고를 비롯 하 여 [ad 단위가](in-app-ads.md) 어떻게 수행 되는지 표시 됩니다. 이 보고서에는 [ad 중재](in-app-ads.md#mediation)를 사용 하는 UWP 앱에서 여러 ad 공급자의 데이터가 포함 됩니다.

이 보고서를 보려면 왼쪽 탐색 메뉴에서 **분석** 을 확장 한 다음 **Ad 성능**을 선택 합니다. 파트너 센터에서이 데이터를 보거나 페이지의 화살표 아이콘을 클릭 하 여 보고서 데이터를 다운로드 하 여 오프 라인으로 볼 수 있습니다. 또는 [분석 REST API](../monetize/access-analytics-data-using-windows-store-services.md)에서 [get ad performance data](../monetize/get-ad-performance-data.md) 메서드를 사용 하 여 프로그래밍 방식으로이 데이터를 검색할 수 있습니다.

광고 성능 보고서를 볼 때 지난 3 일간의 보고 데이터는 다양 한 원본에서 새 데이터를 받고 처리할 때 변경 될 수 있습니다. 또한 데이터 다시 문은 과거에 최대 90 일까지 발생할 수 있습니다.

## <a name="apply-filters"></a>필터 적용

페이지 맨 위 근처에서 데이터를 표시 하려는 기간을 선택할 수 있습니다. 기본 선택 항목은 30D (30 일) 이지만 3, 6 또는 12 개월에 대 한 데이터 또는 지정한 사용자 지정 데이터 범위에 대 한 데이터를 표시 하도록 선택할 수 있습니다.

**필터** 를 확장 하 여 ad 단위, 앱, ad 공급자 및 장치 유형별로이 페이지의 모든 데이터를 필터링 할 수도 있습니다. 다음 옵션 중에서 선택할 수 있습니다.

* **집계**: 보고서 데이터를 집계 하는 방법 및 필터링 할 수 있는 방법을 선택 합니다. 기본적으로이 필터는 **모든 ad 단위**로 설정 됩니다. 필요에 따라 **모든 앱** 또는 **모든 ad 공급자**에 대해이 필터를 변경 하거나 광고를 사용 하는 특정 앱을 기준으로 집계 하도록 선택할 수 있습니다.
* **Ad 공급자**: 특정 [Ad 공급자](in-app-ads.md#paid-networks)에 대 한 성능 데이터에 대 한 보고서를 필터링 합니다. 기본적으로 보고서는 모든 ad 공급자의 데이터를 표시 합니다. **집계** 드롭다운에서 **모든 ad 공급자** 를 선택한 경우이 옵션을 사용할 수 없습니다.
* **장치**: 특정 장치 유형에 대 한 성능 데이터에 대 한 보고서를 필터링 합니다. 기본적으로 보고서는 모든 장치 유형에 대 한 데이터를 표시 합니다.

## <a name="overall-performance"></a>전반적인 성능

이 섹션에서는 보고서 필터에서 선택한 ad 단위, 앱 및 ad 공급자에 대 한 차트 또는 세계 지도 보기의 광고 성능 메트릭을 표시 합니다.

데이터의 다른 뷰로 전환 하려면 **전체 성능** 섹션의 위쪽에서 **차트** 또는 **지도** 를 클릭 합니다. 지도 보기에서 짙은 음영은 더 높은 값을 나타내고 더 밝은 음영은 더 낮은 값을 나타냅니다. 지도의 특정 국가나 지역을 마우스로 가리키면 선택한 메트릭의 값을 분석할 수 있습니다. 지도 영역을 확대 하 여 더 작은 국가의 데이터를 볼 수도 있습니다.

차트나 **지도** 드롭다운 옆에 있는 필터 아이콘을 클릭 하 여 차트 또는 지도에 표시 되는 **Chart** 데이터를 구체화할 수 있습니다. 이 필터를 사용 하면 차트 또는 지도 보기에서 비교할 최대 6 개의 다른 ad 단위, 앱 또는 ad 공급자를 선택할 수 있습니다. 이 필터에서 선택할 수 있는 데이터 유형은 보고서 위쪽의 **집계** 최상위 필터에 대해 선택한 내용에 따라 달라 집니다.


## <a name="overall-performance-breakdown"></a>전반적인 성능 분석

이 섹션에는 보고서에서 선택한 필터로 지정 된 데이터 집합에 대 한 모든 ad 성능 메트릭이 포함 된 테이블이 표시 됩니다.

## <a name="performance-metrics"></a>성능 메트릭

**광고 성능** 보고서에는 다음과 같은 성능 메트릭에 대 한 데이터가 포함 됩니다. 일부 메트릭은 특정 ad 공급자에 대해서만 표시 됩니다.

|  메트릭  |  설명  |
|----------|---------------|
| 예상 수익  |  앱에서 실행 되는 광고에서 받은 예상 금액입니다. |
| eCPM  |  천 단위 별 유효 비용. |
| 요청  | 앱에서 ad 요청이 전송 된 횟수입니다.  |
| Impressions  | 앱에서 광고가 표시 된 횟수입니다.  |
| 채우기 빈도  | Ad가 표시 된 앱에서 보낸 ad 요청 비율입니다.  |
| 클릭  |  앱에서 사용자가 광고를 클릭 한 횟수입니다. |
| CTR  |  광고를 클릭 한 횟수를 의미의 수로 나눈 값을 클릭 하 여 평가 합니다. |
| Viewability | 앱에서 볼 수 있는 광고 광고의 백분율입니다. 이 값을 계산 하는 방법에 대 한 자세한 내용은 [ad 단위의 Viewability 최적화](../monetize/optimize-ad-unit-viewability.md)를 참조 하세요. |
| 크레딧 획득  | [커뮤니티 ad](./about-community-ads.md) 캠페인을 실행 하는 경우 앱에 커뮤니티 광고를 표시 하 여 ad 공간 홍보를 위해 획득 한 크레딧 수를 나타냅니다.  |
| 크레딧을 소비한 시간  | [커뮤니티 ad](./about-community-ads.md) 캠페인을 실행 하는 경우 앱에 대 한 광고에 사용한 크레딧 수를 나타냅니다.  |

## <a name="related-topics"></a>관련 항목

* [앱 내 광고](in-app-ads.md)
* [Microsoft Advertising SDK를 사용하여 앱에 광고 표시](../monetize/display-ads-in-your-app.md)
* [광고 단위의 가시성 최적화](../monetize/optimize-ad-unit-viewability.md)


 