---
description: 지정된 된 날짜 범위 및 다른 선택적 필터 중 추가 기능 구독 취득 데이터를 가져오려면 Microsoft Store 분석 API에서에서이 메서드를 사용 합니다.
title: 구독 추가 기능 취득 가져오기
ms.date: 01/25/18
ms.topic: article
keywords: Microsoft Store 분석 API 구독, 저장소 서비스, uwp, windows 10
ms.openlocfilehash: e33a3ded219fb4d223137b40ebe871f66589addf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594778"
---
# <a name="get-subscription-add-on-acquisitions"></a>구독 추가 기능 취득 가져오기

Microsoft Store 분석 API에에서이 메서드를 사용 하 여 지정된 된 날짜 범위 및 다른 선택적 필터 중 앱에 대 한 추가 기능 구독에 대 한 집계 취득 데이터를 가져오려고 합니다.

## <a name="prerequisites"></a>필수 구성 요소

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | 설명          |
|---------------|--------|--------------|
| 권한 부여 | 문자열 | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** &lt; *토큰*&gt;합니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 형식   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 합니다 [Store ID](in-app-purchases-and-trials.md#store-ids) 구독 추가 기능 취득 데이터를 검색 하려는 앱입니다. |  예  |
| subscriptionProductId  | 문자열 | 합니다 [Store ID](in-app-purchases-and-trials.md#store-ids) 취득 데이터를 검색 하려는 구독 추가 기능입니다. 이 값을 지정 하지 않는 경우이 메서드는 지정된 된 응용 프로그램에 대 한 모든 구독 추가 기능에 대 한 취득 데이터를 반환 합니다.  | 아니오  |
| startDate | date | 검색할 구독 추가 기능 취득 데이터의 날짜 범위의 시작 날짜입니다. 기본값은 현재 날짜입니다. |  아니오  |
| endDate | date | 검색할 구독 추가 기능 취득 데이터의 날짜 범위의 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니오  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 최 댓 값 및 지정 되지 않은 경우 기본값은 100입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니오  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색할 수 있습니다. 예를 들어 top=100 및 skip=0이면 데이터의 처음 100개 행을 검색하고 top=100 및 skip=100이면 데이터의 다음 100개 행을 검색하는 방식입니다. |  아니오  |
| filter | 문자열  | 응답 본문을 필터링하는 하나 이상의 문장입니다. 각 문은 **eq** 또는 **ne** 연산자를 사용할 수 있으며, 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 필터 문에 다음 문자열을 지정할 수 있습니다 (해당 [응답 본문에 값](#subscription-acquisition-values)): <ul><li><strong>날짜</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>ApplicationName</strong></li><li><strong>skuId</strong></li><li><strong>시장</strong></li><li><strong>장치 유형</strong></li></ul><p>예로 *필터* 매개 변수: <em>필터 = 날짜 eq ' 2017-07-08'</em>합니다.</p> | 아니오   |
| aggregationLevel | 문자열 | 집계 데이터를 검색할 시간 범위를 지정합니다. <strong>day</strong>, <strong>week</strong> 또는 <strong>month</strong> 문자열 중 하나일 수 있습니다. 지정하지 않을 경우 기본값은 <strong>day</strong>입니다. | 아니오 |
| orderby | 문자열 | 각 구독 추가 기능 구입에 대 한 데이터 값 결과 정렬 하는 문입니다. 구문은 <em>orderby=field [order],field [order],...</em>입니다. <em>field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li><strong>날짜</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>ApplicationName</strong></li><li><strong>skuId</strong></li><li><strong>시장</strong></li><li><strong>장치 유형</strong></li></ul><p><em>order</em> 매개 변수는 옵션이며 <strong>asc</strong> 또는 <strong>desc</strong>로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열 예입니다. <em>orderby=date,market</em></p> |  아니오  |
| groupby | 문자열 | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 다음 필드를 지정할 수 있습니다.<ul><li><strong>날짜</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>ApplicationName</strong></li><li><strong>skuId</strong></li><li><strong>시장</strong></li><li><strong>장치 유형</strong></li></ul><p><em>groupby</em> 매개 변수는 <em>aggregationLevel</em> 매개 변수와 함께 사용할 수 있습니다. 예를 들어: <em>groupby 시장 =&amp;aggregationLevel = 주</em></p> |  아니오  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 구독 추가 기능 취득 데이터를 가져오는 방법을 보여 줍니다. 대체는 *applicationId* 앱에 대 한 적절 한 Store ID 사용 하 여 값입니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions?applicationId=9NBLGGGZ5QDR&startDate=2017-07-07&endDate=2017-07-08 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 형식   | 설명         |
|------------|--------|------------------|
| 값      | 배열  | 배열 집계 구독 추가 기능 취득 데이터를 포함 하는 개체입니다. 각 개체의 데이터에 대 한 자세한 내용은 참조는 [구독 취득 값](#subscription-acquisition-values) 아래의 섹션입니다.             |
| @nextLink  | 문자열 | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 하는 경우이 값이 반환 하는 예를 들어 합니다 **위쪽** 요청의 매개 변수는 100으로 설정 되지만 구독 추가 기능 취득 데이터 쿼리에 대 한 100 개 이상의 행. |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.       |


<span id="subscription-acquisition-values" />

### <a name="subscription-acquisition-values"></a>구독 취득 값

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값               | 형식    | 설명        |
|---------------------|---------|---------------------|
| date                | 문자열  | 구입 데이터의 날짜 범위에 대한 시작 날짜입니다. 요청에서 하루를 지정한 경우 이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정한 경우 이 값은 해당 날짜 범위의 시작 날짜입니다. |
| subscriptionProductId      | 문자열  | 합니다 [Store ID](in-app-purchases-and-trials.md#store-ids) 구독 추가 기능을 취득 데이터를 검색 합니다.    |
| subscriptionProductName    | 문자열  | 구독 추가 기능의 표시 이름입니다.         |
| applicationId       | 문자열  | 합니다 [Store ID](in-app-purchases-and-trials.md#store-ids) 구독 추가 기능 취득 데이터를 검색 하는 앱입니다.   |
| applicationName     | 문자열  | 앱의 표시 이름     |
| skuId     | 문자열  | ID를 [SKU](in-app-purchases-and-trials.md#products-skus) 구독 추가 기능을 취득 데이터를 검색 합니다.     |
| deviceType          | 문자열  |  취득을 완료한 장치 유형을 나타내는 다음 문자열 중 하나입니다.<ul><li><strong>PC</strong></li><li><strong>전화</strong></li><li><strong>콘솔</strong></li><li><strong>IoT</strong></li><li><strong>홀로그램</strong></li><li><strong>알 수 없음</strong></li></ul>       |
| 출시           | 문자열  | 구입이 발생한 시장의 ISO 3166 국가 코드입니다.     |
| currencyCode         | 문자열  | 세금 전에 총 판매량에 대 한 형식으로 ISO 4217 통화 코드입니다.       |
| grossSalesBeforeTax           | 정수  | 지정 된 현지 통화에서 총 판매량을 *currencyCode* 값입니다.     |
| totalActiveCount             | 정수  | 지정 된 기간 동안 총 활성 구독 수입니다. 모두 합한 것과 같습니다 합니다 *goodStandingActiveCount*를 *pendingGraceActiveCount*를 *graceActiveCount*, 및 *lockedActiveCount* 값입니다.  |
| totalChurnCount              | 정수  | 지정 된 기간 동안 비활성화 된 구독의 총 수입니다. 모두 합한 것과 같습니다 합니다 *billingChurnCount*를 *nonRenewalChurnCount*를 *refundChurnCount*, *chargebackChurnCount*하십시오 *earlyChurnCount*, 및 *otherChurnCount* 값입니다.   |
| newCount            | 정수  | 지정 된 기간 동안, 평가판을 포함 하 여 새 구독 획득 횟수입니다.    |
| renewCount     | 정수  | 지정 된 시간 동안 사용자가 시작한 갱신 및 자동 갱신을 포함 하 여 구독 갱신 횟수입니다.        |
| goodStandingActiveCount | 정수 | 지정 된 기간 동안 활성화 된 및 만료 날짜가 있는 구독의 수 > = 합니다 *endDate* 쿼리의 값입니다.    |
| pendingGraceActiveCount | 정수 | 구독 및 구독 만료 날짜 위치가 지정 된 기간 동안 활성화 된는 했지만 청구 오류 수가 > = 합니다 *endDate* 쿼리의 값입니다.     |
| graceActiveCount | 정수 | 청구는 오류가 발생 했지만 지정 된 기간 동안 활성화 된 구독 수 및 위치:<ul><li>구독 만료 날짜가 < 합니다 *endDate* 쿼리의 값입니다.</li><li>유예 기간 종료는 > = 합니다 *endDate* 값입니다.</li></ul>        |
| lockedActiveCount | 정수 | 에 구독 수가 *독* (즉, 구독에 만료가 및 Microsoft 자동으로 구독을 갱신 하려고 자금을 획득 하려고) 동안 지정 된 시간, 기간 및 위치:<ul><li>구독 만료 날짜가 < 합니다 *endDate* 쿼리의 값입니다.</li><li>유예 기간이 완료 되 면 < = 합니다 *endDate* 값입니다.</li></ul>        |
| billingChurnCount | 정수 | 구독 하는 청구 요금을 처리 하는 데 오류로 인해 지정 된 기간 동안 비활성화 된 및 독에 구독을 이전에 있던 횟수입니다.        |
| nonRenewalChurnCount | 정수 | 갱신 하지 않은 때문에 지정 된 기간 동안 비활성화 된 구독 수입니다.        |
| refundChurnCount | 정수 | 환불 되었습니다 것 때문에 지정 된 기간 동안 비활성화 된 구독 수입니다.        |
| chargebackChurnCount | 정수 | 환불으로 인해 지정 된 기간 동안 비활성화 된 구독 수입니다.        |
| earlyChurnCount | 정수 | 양호한 상태 동안 지정 된 기간 동안 비활성화 된 구독 수입니다.        |
| otherChurnCount | 정수 | 다른 이유로 지정 된 기간 동안 비활성화 된 구독 수입니다.        |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "date": "2017-07-08",
      "subscriptionProductId": "9KDLGHH6R365",
      "subscriptionProductName": "Contoso App Subscription with One Month Free Trial",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso App",
      "skuId": "0020",
      "market": "Unknown",
      "deviceType": "PC",
      "currencyCode": "USD",
      "grossSalesBeforeTax": 0.0,
      "totalActiveCount": 1,
      "totalChurnCount": 0,
      "newCount": 0,
      "renewCount": 0,
      "goodStandingActiveCount": 1,
      "pendingGraceActiveCount": 0,
      "graceActiveCount": 0,
      "lockedActiveCount": 0,
      "billingChurnCount": 0,
      "nonRenewalChurnCount": 0,
      "refundChurnCount": 0,
      "chargebackChurnCount": 0,
      "earlyChurnCount": 0,
      "otherChurnCount": 0
    },
    {
      "date": "2017-07-08",
      "subscriptionProductId": "9JJFDHG4R478",
      "subscriptionProductName": "Contoso App Monthly Subscription",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso App",
      "skuId": "0020",
      "market": "US",
      "deviceType": "PC",
      "currencyCode": "USD",
      "grossSalesBeforeTax": 0.0,
      "totalActiveCount": 1,
      "totalChurnCount": 0,
      "newCount": 0,
      "renewCount": 0,
      "goodStandingActiveCount": 1,
      "pendingGraceActiveCount": 0,
      "graceActiveCount": 0,
      "lockedActiveCount": 0,
      "billingChurnCount": 0,
      "nonRenewalChurnCount": 0,
      "refundChurnCount": 0,
      "chargebackChurnCount": 0,
      "earlyChurnCount": 0,
      "otherChurnCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>관련 항목

* [추가 기능 구입 보고서](../publish/add-on-acquisitions-report.md)
* [Microsoft Store 서비스를 사용 하 여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)

 

 
