---
ms.assetid: A26A287C-B4B0-49E9-BB28-6F02472AE1BA
description: Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 날짜 범위 및 기타 선택적 필터에서 지정 된 응용 프로그램에 대 한 집계 ad 캠페인 성능 데이터를 가져올 수 있습니다.
title: 광고 캠페인 성과 데이터 가져오기
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 스토어 서비스, Microsoft Store analytics API, ad 캠페인
ms.localizationpriority: medium
ms.openlocfilehash: c40ab5d6aea67477e0900441cfb02b858dbef9b2
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878456"
---
# <a name="get-ad-campaign-performance-data"></a>광고 캠페인 성과 데이터 가져오기


Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 날짜 범위 및 기타 선택적 필터 중에 응용 프로그램에 대 한 프로 모션 광고 캠페인 성능 데이터의 집계 요약을 가져올 수 있습니다. 이 메서드는 JSON 형식으로 데이터를 반환 합니다.

이 메서드는 파트너 센터의 [Ad 캠페인 보고서](/windows/uwp/publish/ad-campaign-report) 에서 제공 하는 것과 동일한 데이터를 반환 합니다. Ad 캠페인에 대 한 자세한 내용은 [앱에 대 한 광고 캠페인 만들기](./index.md)를 참조 하세요.

Ad 캠페인에 대 한 세부 정보를 만들거나 업데이트 하거나 검색 하려면 [Microsoft Store 프로 모션 API](run-ad-campaigns-using-windows-store-services.md)에서 [ad 캠페인 관리](manage-ad-campaigns.md) 메서드를 사용할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 분석 API에 대 한 모든 [필수 구성 요소](access-analytics-data-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | Description                |
|---------------|--------|---------------|
| 권한 부여 | 문자열 | 필수 사항입니다. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

특정 앱에 대 한 광고 캠페인 성능 데이터를 검색 하려면 *applicationId* 매개 변수를 사용 합니다. 개발자 계정과 연결 된 모든 앱에 대 한 ad 성능 데이터를 검색 하려면 *applicationId* 매개 변수를 생략 합니다.

| 매개 변수     | Type   | 설명     | 필수 |
|---------------|--------|-----------------|----------|
| applicationId   | 문자열    | Ad 캠페인 성능 데이터를 검색 하려는 앱의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다. |    아니요      |
|  startDate  |  date   |  검색할 ad 캠페인 성능 데이터의 날짜 범위의 시작 날짜 (YYYY/MM/DD 형식)입니다. 기본값은 현재 날짜에서 30 일을 뺀 값입니다.   |   아니요    |
| endDate   |  date   |  검색할 ad 캠페인 성능 데이터의 날짜 범위에 있는 종료 날짜 이며 YYYY/MM/DD 형식입니다. 기본값은 현재 날짜에서 1 일을 뺀 값입니다.   |   예    |
| top   |  int   |  요청에 반환할 데이터 행 수입니다. 지정 되지 않은 경우 최대값 및 기본값은 1만입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 다음 링크가 포함 됩니다.   |   아니요    |
| skip   | int    |  쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용 하 여 많은 데이터 집합을 페이징 합니다. 예를 들어 top = 10000과 skip = 0은 처음 1만 개의 데이터 행을 검색 하 고 top = 10000 및 skip = 10000은 데이터의 다음 1만 행을 검색 하는 식입니다.   |   아니요    |
| filter   |  문자열   |  응답의 행을 필터링 하는 하나 이상의 문입니다. 유일 하 게 지원 되는 필터는 **campaignId**입니다. 각 문은 **eq** 또는 **ne** 연산자를 사용할 수 있으며, **및** 또는 **또는**를 사용 하 여 문을 조합할 수 있습니다.  예제 *필터* 매개 변수는 다음과 같습니다 ```filter=campaignId eq '100023'``` .   |   아니요    |
|  aggregationLevel  |  문자열   | 집계 데이터를 검색 하는 시간 범위를 지정 합니다. <strong>일</strong>, <strong>주</strong>또는 <strong>월</strong>문자열 중 하나일 수 있습니다. 지정 하지 않으면 기본값은 <strong>day</strong>입니다.    |   아니요    |
| orderby   |  문자열   |  <p>광고 캠페인 성능 데이터에 대 한 결과 데이터 값을 정렬 하는 문입니다. 구문은 <em>orderby = field [order], field [order],...</em>입니다. <em>Field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.</p><ul><li><strong>date</strong></li><li><strong>campaignId</strong></li></ul><p><em>Order</em> 매개 변수는 선택 사항이 며 <strong>asc</strong> 또는 <strong>desc</strong> 로 각 필드에 대해 오름차순 또는 내림차순을 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 orderby <em>= date, campaignId</em> 의 예제 <em>orderby</em> 문자열입니다.</p>   |   아니요    |
|  groupby  |  문자열   |  <p>지정 된 필드에만 데이터 집계를 적용 하는 문입니다. 다음 필드를 지정할 수 있습니다.</p><ul><li><strong>campaignId</strong></li><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>currencyCode</strong></li></ul><p><em>Groupby</em> 매개 변수는 <em>aggregationLevel</em> 매개 변수와 함께 사용할 수 있습니다. 예: <em> &amp; Groupby = applicationId &amp; aggregationLevel = week</em></p>   |   아니요    |


### <a name="request-example"></a>요청 예제

다음 예제에서는 ad 캠페인 성능 데이터를 가져오는 몇 가지 요청을 보여 줍니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?aggregationLevel=week&groupby=applicationId,campaignId,date  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?applicationId=9NBLGGH0XK8Z&startDate=2015/1/20&endDate=2016/8/31&skip=0&filter=campaignId eq '31007388' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | Type   | 설명  |
|------------|--------|---------------|
| 값      | array  | 집계 ad 캠페인 성능 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 아래의 [캠페인 성능 개체](#campaign-performance-object) 섹션을 참조 하세요.          |
| @nextLink  | 문자열 | 추가 데이터 페이지가 있는 경우이 문자열에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 URI가 포함 됩니다. 예를 들어 요청의 **맨 위** 매개 변수가 5로 설정 되어 있지만 쿼리에 대 한 데이터 항목은 5 개를 초과 하는 경우이 값이 반환 됩니다. |
| TotalCount | int    | 쿼리의 데이터 결과에 있는 총 행 수입니다.                                |


<span id="campaign-performance-object" />


### <a name="campaign-performance-object"></a>캠페인 성과 개체

*값* 배열의 요소에는 다음 값이 포함 됩니다.

| 값               | Type   | 설명            |
|---------------------|--------|------------------------|
| date                | 문자열 | 광고 캠페인 성능 데이터에 대 한 날짜 범위의 첫 번째 날짜입니다. 요청에서 하루를 지정 하는 경우이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정 하는 경우이 값은 해당 날짜 범위의 첫 번째 날짜입니다. |
| applicationId       | 문자열 | Ad 캠페인 성능 데이터를 검색 하는 앱의 저장소 ID입니다.                     |
| campaignId     | 문자열 | 광고 캠페인의 ID입니다.           |
| lineId     | 문자열 |    이 성능 데이터를 생성 한 광고 캠페인 [배달 라인](manage-delivery-lines-for-ad-campaigns.md) 의 ID입니다.        |
| currencyCode              | 문자열 | 캠페인 예산에 대 한 통화 코드입니다.              |
| 낭비          | 문자열 |  광고 캠페인에 소요 된 예산 금액입니다.     |
| 갖게           | long | 캠페인에 대 한 광고 광고 수입니다.        |
| 설치              | long | 캠페인과 관련 된 앱 설치 수입니다.   |
| clicks            | long | 캠페인에 대 한 광고 클릭 수입니다.      |
| iapInstalls            | long | 캠페인과 관련 된 추가 기능 (앱에서 구입 또는 IAP 라고도 함)의 수를 설치 합니다.      |
| activeUsers            | long | 캠페인의 일부 이며 앱에 반환 되는 광고를 클릭 한 사용자의 수입니다.      |


### <a name="response-example"></a>응답 예제

다음 예제에서는이 요청에 대 한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "date": "2015-04-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "4568",
      "lineId": "0001",
      "currencyCode": "USD",
      "spend": 700.6,
      "impressions": 200,
      "installs": 30,
      "clicks": 8,
      "iapInstalls": 0,
      "activeUsers": 0
    },
    {
      "date": "2015-05-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "1234",
      "lineId": "0002",
      "currencyCode": "USD",
      "spend": 325.3,
      "impressions": 20,
      "installs": 2,
      "clicks": 5,
      "iapInstalls": 0,
      "activeUsers": 0
    }
  ],
  "@nextLink": "promotion?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/1/20&endDate=2016/8/31&top=2&skip=2",
  "TotalCount": 1917
}
```

## <a name="related-topics"></a>관련 항목

* [앱 광고 캠페인 만들기](./index.md)
* [Microsoft Store 서비스를 사용 하 여 ad 캠페인 실행](run-ad-campaigns-using-windows-store-services.md)
* [Microsoft Store 서비스를 사용 하 여 분석 데이터 액세스](access-analytics-data-using-windows-store-services.md)