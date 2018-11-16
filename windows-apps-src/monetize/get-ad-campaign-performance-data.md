---
author: Xansky
ms.assetid: A26A287C-B4B0-49E9-BB28-6F02472AE1BA
description: Microsoft Store 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택 필터에 대해 지정된 응용 프로그램에 대한 광고 성과 집계 데이터를 가져옵니다.
title: 광고 캠페인 성과 데이터 가져오기
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API, 광고 캠페인
ms.localizationpriority: medium
ms.openlocfilehash: c9d348ffc0895d5adb16886c5a66af14a1884b8b
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6972962"
---
# <a name="get-ad-campaign-performance-data"></a>광고 캠페인 성과 데이터 가져오기


Microsoft Store 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택 필터에 대해 응용 프로그램에 대한 홍보성 광고 캠페인 성과 집계 요약 데이터를 가져옵니다. 이 메서드는 JSON 형식의 데이터를 반환합니다.

이 메서드는 파트너 센터에서 [광고 캠페인 보고서](../publish/app-install-ads-reports.md) 를 제공 하는 동일한 데이터를 반환 합니다. 광고 캠페인에 대한 자세한 내용은 [앱 광고 캠페인 만들기](../publish/create-an-ad-campaign-for-your-app.md)를 참조하세요.

광고 캠페인을 만들고, 업데이트하고, 세부 정보를 검색하려면 [Microsoft Store 프로모션 API](run-ad-campaigns-using-windows-store-services.md)의 [광고 캠페인 관리](manage-ad-campaigns.md) 메서드를 사용할 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                |
|---------------|--------|---------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

특정 앱에 대한 광고 캠페인 성과 데이터를 검색하려면 *applicationId* 매개 변수를 사용합니다. 개발자 계정과 연결된 모든 앱에 대한 광고 성과 데이터를 검색하려면 *applicationId* 매개 변수를 생략합니다.

| 매개 변수     | 유형   | 설명     | 필수 |
|---------------|--------|-----------------|----------|
| applicationId   | 문자열    | 광고 캠페인 성과 데이터를 검색할 앱의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다. |    아니요      |
|  startDate  |  date   |  검색할 광고 캠페인 성과 데이터의 날짜 범위에 대한 시작 날짜로 YYYY/MM/DD 형식입니다. 기본값은 현재 날짜에서 30일을 뺀 값입니다.   |   아니요    |
| endDate   |  date   |  검색할 광고 캠페인 성과 데이터의 날짜 범위에 대한 끝 날짜로 YYYY/MM/DD 형식입니다. 기본값은 현재 날짜에서 하루를 뺀 값입니다.   |   아니요    |
| top   |  int   |  요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다.   |   아니요    |
| skip   | int    |  쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다.   |   아니요    |
| filter   |  string   |  응답에서 행을 필터링하는 하나 이상의 문입니다. 지원되는 유일한 필터는 **campaignId**입니다. 각 문은 **eq** 또는 **ne** 연산자를 사용할 수 있으며, 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다.  다음은 *filter* 매개 변수: ```filter=campaignId eq '100023'```에 대한 예제입니다.   |   아니요    |
|  aggregationLevel  |  string   | 집계 데이터를 검색할 시간 범위를 지정합니다. <strong>day</strong>, <strong>week</strong> 또는 <strong>month</strong> 문자열 중 하나일 수 있습니다. 지정하지 않을 경우 기본값은 <strong>day</strong>입니다.    |   아니요    |
| orderby   |  문자열   |  <p>광고 캠페인 성과 데이터에 대한 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 <em>orderby=field [order],field [order],...</em>입니다. <em>field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.</p><ul><li><strong>date</strong></li><li><strong>campaignId</strong></li></ul><p><em>order</em> 매개 변수는 옵션이며 <strong>asc</strong> 또는 <strong>desc</strong>로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열 예입니다. <em>orderby=date,campaignId</em></p>   |   아니요    |
|  groupby  |  string   |  <p>지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 다음 필드를 지정할 수 있습니다.</p><ul><li><strong>campaignId</strong></li><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>currencyCode</strong></li></ul><p><em>groupby</em> 매개 변수는 <em>aggregationLevel</em> 매개 변수와 함께 사용할 수 있습니다. 예: <em>&amp;groupby=applicationId&amp;aggregationLevel=week</em></p>   |   아니요    |


### <a name="request-example"></a>요청 예제

다음 예제에서는 광고 캠페인 성과 데이터를 가져오는 데 필요한 몇 가지 요청을 보여 줍니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?aggregationLevel=week&groupby=applicationId,campaignId,date  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?applicationId=9NBLGGH0XK8Z&startDate=2015/1/20&endDate=2016/8/31&skip=0&filter=campaignId eq '31007388' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 유형   | 설명  |
|------------|--------|---------------|
| 값      | 배열  | 광고 캠페인 집계 성과 데이터가 포함된 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 아래 [캠페인 성과 개체](#campaign-performance-object) 섹션을 참조하세요.          |
| @nextLink  | string | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 5로 설정되어 있지만 쿼리에 대한 데이터가 5개 항목보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.                                |


<span id="campaign-performance-object" />


### <a name="campaign-performance-object"></a>캠페인 성과 개체

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값               | 유형   | 설명            |
|---------------------|--------|------------------------|
| date                | 문자열 | 광고 캠페인 성과 데이터의 날짜 범위에 대한 시작 날짜입니다. 요청에서 하루를 지정한 경우 이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정한 경우 이 값은 해당 날짜 범위의 시작 날짜입니다. |
| applicationId       | 문자열 | 광고 캠페인 성과 데이터를 검색 중인 앱의 스토어 ID입니다.                     |
| campaignId     | 문자열 | 광고 캠페인의 ID입니다.           |
| lineId     | 문자열 |    이 성능 데이터를 생성하는 광고 캠페인 ID[배달 라인](manage-delivery-lines-for-ad-campaigns.md)입니다.        |
| currencyCode              | 문자열 | 캠페인 예산의 통화 코드입니다.              |
| spend          | 문자열 |  광고 캠페인에 대해 소비한 예산 금액입니다.     |
| impressions           | long | 캠페인 광고 노출 수입니다.        |
| installs              | long | 캠페인 관련 앱 설치 수입니다.   |
| clicks            | long | 캠페인에서 광고를 클릭한 횟수입니다.      |
| iapInstalls            | long | 캠페인에 관련되어 설치된 추가 기능(앱에서 바로 구매 또는 IAP라고도 함)의 수입니다.      |
| activeUsers            | long | 캠페인의 일부인 광고를 클릭하고 앱으로 돌아온 사용자의 수입니다.      |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

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

* [앱 광고 캠페인 만들기](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app)
* [Microsoft Store 서비스를 사용하여 광고 캠페인 실행](run-ad-campaigns-using-windows-store-services.md)
* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
