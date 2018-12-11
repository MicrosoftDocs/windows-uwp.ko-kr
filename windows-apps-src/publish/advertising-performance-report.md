---
Description: To view performance data for the ad units in your apps, use the advertising performance report in Partner Center.
title: 광고 성과 보고서
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a96f6f6593a8ccc6714f67b6f825a6416750b432
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8892989"
---
# <a name="advertising-performance-report"></a>광고 성과 보고서


[파트너 센터](https://partner.microsoft.com/dashboard) 에서 **광고 성과 보고서** 커뮤니티 광고를 포함 하 여 [광고 단위](in-app-ads.md) 에 수행 하는 방법을 보여 줍니다. 이 보고서에는 [광고 조정](in-app-ads.md#mediation)을 사용하는 UWP 앱의 여러 광고 공급자의 데이터가 포함되어 있습니다.

이 보고서를 보려면 왼쪽 탐색 메뉴에서 **분석**을 확장하고 **광고 성과**를 선택합니다. 파트너 센터에서이 데이터를 보거나 보고서 데이터를 다운로드 하는 페이지에 있는 화살표 아이콘을 클릭 하 여 오프 라인으로 볼 수 있습니다. 또는 [분석 REST API](../monetize/access-analytics-data-using-windows-store-services.md)에서 [광고 성과 데이터 가져오기](../monetize/get-ad-performance-data.md) 메서드를 사용하여 프로그래밍 방식으로 이 데이터를 검색할 수 있습니다.

광고 성과 보고서를 볼 때는 다양한 소스에서 새 데이터를 수신하고 처리하기 때문에 지난 3일간의 보고 데이터가 변경될 수 있다는 사실에 유의하세요. 뿐만 아니라 최대 90일 이전까지 데이터를 다시 지정할 수 있습니다.

## <a name="apply-filters"></a>필터 적용

페이지 위쪽에서 데이터가 표시되는 기간을 선택할 수 있습니다. 기본 설정은 30D (30일)이지만, 3개월, 6개월 또는 12개월 동안 데이터를 표시하거나 사용자가 지정한 데이터 범위에서 데이터를 표시하도록 선택할 수 있습니다.

또한 **필터**를 확장하여 광고 단위, 앱, 광고 공급자, 장치 유형을 기준으로 이 페이지의 모든 데이터를 필터링할 수 있습니다. 다음 옵션 중에서 선택할 수 있습니다.

* **집계**: 보고서 데이터 집계 방법 및 추가 필터링 방법을 선택할 수 있습니다. 기본적으로 이 필터는 **모든 광고 단위**로 설정되어 있습니다. 원하는 경우 이 필터를 **모든 앱** 또는 **모든 광고 공급자**로 변경하거나 광고를 사용하는 특정 앱별로 집계하도록 선택할 수도 있습니다.
* **광고 공급자**: 특정 [광고 공급자](in-app-ads.md#paid-networks)에 대한 성과 데이터를 보고하도록 필터링합니다. 기본적으로 보고서는 모든 광고 공급자의 데이터를 표시합니다. **집계** 드롭다운 목록에서 **모든 광고 공급자**를 선택하면 이 옵션이 비활성화됩니다.
* **장치**: 특정 장치 유형에 대한 성과 데이터를 보고하도록 필터링합니다. 기본적으로 보고서는 모든 장치 유형의 데이터를 표시합니다.

## <a name="overall-performance"></a>전반적인 성과

이 섹션은 보고서 필터에서 선택한 광고 단위, 앱 및 광고 공급자에 대해 광고 성과 메트릭을 차트 또는 세계 지도 보기로 표시합니다.

데이터의 여러 보기 간에 전환하려면 **전반적인 성과** 섹션 맨 위에 있는 **차트** 또는 **지도**를 클릭합니다. 맵 보기에서 그림자가 어두울수록 값이 크고 그림자가 밝을수록 값이 작습니다. 선택한 메트릭의 값을 분석하려면 맵에서 특정 국가나 지역 위를 가리키면 됩니다. 또한 맵의 어떤 영역을 확대하여 더 작은 국가의 데이터를 볼 수도 있습니다.

**차트** 또는 **지도** 드롭다운 옆에 있는 필터 아이콘을 클릭하여 차트 또는 지도에 표시되는 데이터를 상세 검색할 수 있습니다. 이 필터로 최대 6개의 다른 광고 단위, 앱, 또는 광고 제공자에서 선택하여 차트 또는 지도 보기에서 비교할 수 있습니다. 이 필터에서 선택할 수 있는 데이터의 종류는 보고서의 상단의 **집계** 최상위 필터에 대해 선택한 것에 따라 달라집니다.


## <a name="overall-performance-breakdown"></a>전반적인 성과 분석

이 섹션에 보고서에서 선택한 필터로 지정된 데이터 집합에 대한 모든 광고 성과 메트릭이 포함된 테이블 표시됩니다.

## <a name="performance-metrics"></a>성과 메트릭

**광고 성과** 보고서에는 다음 성과 메트릭에 대한 데이터가 포함됩니다. 일부 메트릭은 특정 광고 공급자에 대해서만 표시됩니다.

|  메트릭  |  설명  |
|----------|---------------|
| 예상 수입  |  예상 수입은 앱에서 실행 중인 광고를 통해 받는 예상 금액입니다. |
| eCPM  |  효과적인 웹 페이지 광고 노출률입니다. |
| 요청  | 앱에서 광고 요청을 보낸 횟수입니다.  |
| 광고 노출  | 앱에 광고가 표시된 횟수입니다.  |
| 유효 노출률  | 광고가 표시된 앱에서 보낸 광고 요청의 백분율입니다.  |
| 클릭 수  |  앱에서 광고를 클릭한 횟수입니다. |
| CTR  |  광고 클릭률(Click-through rate)을 나타내며 광고가 클릭된 횟수를 광고가 노출된 수로 나눕니다. |
| 가시성 | 앱에서 볼 수 있는 광고 노출의 백분율입니다. 이 값은 계산 하는 방법에 대 한 자세한 내용은 [광고 단위 가시성 최적화](../monetize/optimize-ad-unit-viewability.md)를 참조 하세요. |
| 획득한 크레딧  | [커뮤니티 광고](https://docs.microsoft.com/windows/uwp/publish/about-community-ads) 캠페인을 실행하는 경우, 이는 앱에서 커뮤니티 광고를 표시해 광고 공간을 홍보해 획득한 크레딧의 수를 나타냅니다.  |
| 사용한 크레딧  | [커뮤니티 광고](https://docs.microsoft.com/windows/uwp/publish/about-community-ads) 캠페인을 실행하는 경우, 이는 앱에서 광고에 지출한 크레딧 수를 나타냅니다.  |

## <a name="related-topics"></a>관련 항목

* [인앱 광고](in-app-ads.md)
* [Microsoft Advertising SDK를 사용하여 앱에 광고 표시](../monetize/display-ads-in-your-app.md)
* [광고 단위의 가시성 최적화](../monetize/optimize-ad-unit-viewability.md)


 
