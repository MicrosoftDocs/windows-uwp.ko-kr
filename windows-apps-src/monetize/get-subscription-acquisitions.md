---
description: Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 날짜 범위 및 기타 선택적 필터 중에 추가 기능 구독에 대 한 획득 데이터를 가져옵니다.
title: 구독 추가 기능 취득 가져오기
ms.date: 01/25/2018
ms.topic: article
keywords: windows 10, uwp, 스토어 서비스, Microsoft Store analytics API, 구독
ms.openlocfilehash: 4c307dbf7d17251e3d3c5f8792d8ea3d049924d9
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493548"
---
# <a name="get-subscription-add-on-acquisitions"></a>구독 추가 기능 취득 가져오기

Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 날짜 범위 및 기타 선택적 필터 중에 앱에 대 한 추가 기능 구독에 대 한 집계 획득 데이터를 가져올 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 분석 API에 대 한 모든 [필수 구성 요소](access-analytics-data-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | Description          |
|---------------|--------|--------------|
| 권한 부여 | 문자열 | 필수 사항입니다. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | Type   |  설명      |  필요한 공간  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 구독 추가 기능 획득 데이터를 검색 하려는 앱의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다. |  예  |
| subscriptionProductId  | 문자열 | 획득 데이터를 검색 하려는 구독 추가 기능에 대 한 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다. 이 값을 지정 하지 않으면이 메서드는 지정 된 앱에 대 한 모든 구독 추가 기능에 대 한 취득 데이터를 반환 합니다.  | 아니요  |
| startDate | date | 검색할 구독 추가 기능 획득 데이터의 날짜 범위에 있는 시작 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| endDate | date | 검색할 구독 추가 기능 획득 데이터의 날짜 범위에 있는 종료 날짜입니다. 기본값은 현재 날짜입니다. |  예  |
| top | int | 요청에 반환할 데이터 행 수입니다. 지정 되지 않은 경우 최대값 및 기본값은 100입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 다음 링크가 포함 됩니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용 하 여 많은 데이터 집합을 페이징 합니다. 예를 들어 top = 100과 skip = 0은 처음 100 행의 데이터를 검색 하 고, top = 100을 검색 하 고, skip = 100은 다음 100 행 데이터를 검색 하는 식입니다. |  아니요  |
| filter | 문자열  | 응답 본문을 필터링 하는 하나 이상의 문입니다. 각 문은 **eq** 또는 **ne** 연산자를 사용할 수 있으며, **및** 또는 **또는**를 사용 하 여 문을 조합할 수 있습니다. 필터 문에 다음 문자열을 지정할 수 있습니다 ( [응답 본문의 값](#subscription-acquisition-values)에 해당). <ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>시장</strong></li><li><strong>deviceType</strong></li></ul><p>Filter <em>= date eq ' 2017-07-08 '</em>의 예제 *필터* 매개 변수는 다음과 같습니다.</p> | 아니요   |
| aggregationLevel | 문자열 | 집계 데이터를 검색 하는 시간 범위를 지정 합니다. <strong>일</strong>, <strong>주</strong>또는 <strong>월</strong>문자열 중 하나일 수 있습니다. 지정 하지 않으면 기본값은 <strong>day</strong>입니다. | 아니요 |
| orderby | 문자열 | 각 구독 추가 기능 획득에 대 한 결과 데이터 값을 정렬 하는 문입니다. 구문은 <em>orderby = field [order], field [order],...</em>입니다. <em>Field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>시장</strong></li><li><strong>deviceType</strong></li></ul><p><em>Order</em> 매개 변수는 선택 사항이 며 <strong>asc</strong> 또는 <strong>desc</strong> 로 각 필드에 대해 오름차순 또는 내림차순을 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열의 예입니다. <em>orderby = date, market</em></p> |  아니요  |
| groupby | 문자열 | 지정 된 필드에만 데이터 집계를 적용 하는 문입니다. 다음 필드를 지정할 수 있습니다.<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>시장</strong></li><li><strong>deviceType</strong></li></ul><p><em>Groupby</em> 매개 변수는 <em>aggregationLevel</em> 매개 변수와 함께 사용할 수 있습니다. 예: <em>groupby = market &amp; aggregationLevel = week</em></p> |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예에서는 구독 추가 기능 획득 데이터를 가져오는 방법을 보여 줍니다. *ApplicationId* 값을 앱의 적절 한 상점 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions?applicationId=9NBLGGGZ5QDR&startDate=2017-07-07&endDate=2017-07-08 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 형식   | Description         |
|------------|--------|------------------|
| 값      | array  | 집계 구독 추가 기능 획득 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 아래의 [구독 획득 값](#subscription-acquisition-values) 섹션을 참조 하십시오.             |
| @nextLink  | 문자열 | 추가 데이터 페이지가 있는 경우이 문자열에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 URI가 포함 됩니다. 예를 들어 요청의 **top** 매개 변수가 100로 설정 되어 있지만 쿼리에 대 한 구독 추가 기능 획득 데이터의 행이 100 개를 초과 하는 경우이 값이 반환 됩니다. |
| TotalCount | int    | 쿼리의 데이터 결과에 있는 총 행 수입니다.       |


<span id="subscription-acquisition-values" />

### <a name="subscription-acquisition-values"></a>구독 획득 값

*값* 배열의 요소에는 다음 값이 포함 됩니다.

| 값               | 형식    | 설명        |
|---------------------|---------|---------------------|
| date                | 문자열  | 취득 데이터의 날짜 범위에 있는 첫 번째 날짜입니다. 요청에서 하루를 지정 하는 경우이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정 하는 경우이 값은 해당 날짜 범위의 첫 번째 날짜입니다. |
| subscriptionProductId      | 문자열  | 획득 데이터를 검색 하는 구독 추가 기능에 대 한 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다.    |
| subscriptionProductName    | 문자열  | 구독 추가 기능에 대 한 표시 이름입니다.         |
| applicationId       | 문자열  | 구독 추가 기능 획득 데이터를 검색 하는 앱의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다.   |
| applicationName     | 문자열  | 응용 프로그램의 표시 이름입니다.     |
| skuId     | 문자열  | 획득 데이터를 검색 하는 구독 추가 기능 [SKU](in-app-purchases-and-trials.md#products-skus) 의 ID입니다.     |
| deviceType          | 문자열  |  가져오기를 완료 한 장치 유형을 지정 하는 다음 문자열 중 하나입니다.<ul><li><strong>컴퓨터</strong></li><li><strong>내선</strong></li><li><strong>콘솔-Xbox One</strong></li><li><strong>콘솔-Xbox 시리즈 X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>알 수 없음</strong></li></ul>       |
| market           | 문자열  | 획득이 발생 한 시장의 ISO 3166 국가 코드입니다.     |
| currencyCode         | 문자열  | 세금 전 총 판매량에 대 한 ISO 4217 형식의 통화 코드입니다.       |
| grossSalesBeforeTax           | integer  | *CurrencyCode* 값으로 지정 된 현지 통화의 총 판매량입니다.     |
| totalActiveCount             | integer  | 지정 된 기간 동안의 총 활성 구독 수입니다. 이 값은 *goodStandingActiveCount*, *pendingGraceActiveCount*, *graceActiveCount*및 *lockedActiveCount* 값의 합계와 같습니다.  |
| totalChurnCount              | integer  | 지정 된 기간 동안 비활성화 된 총 구독 수입니다. 이 값은 *billingChurnCount*, *nonRenewalChurnCount*, *refundChurnCount*, *chargebackChurnCount*, *earlyChurnCount*및 *otherChurnCount* 값의 합계와 같습니다.   |
| newCount            | integer  | 지정 된 기간 동안 평가판을 포함 하 여 새 구독을 획득 한 횟수입니다.    |
| renewCount     | integer  | 지정 된 기간 동안 사용자가 시작한 갱신 및 자동 갱신을 포함 하 여 구독을 갱신 하는 횟수입니다.        |
| goodStandingActiveCount | integer | 지정 된 기간 동안 활성 상태이 고 만료 날짜가 >인 구독의 수는 쿼리에 대 한 *endDate* 값입니다.    |
| pendingGraceActiveCount | integer | 지정 된 기간 동안 활성 상태 이지만 청구 오류가 발생 한 구독 수와 구독 만료 날짜가 >= 쿼리의 *endDate* 값입니다.     |
| graceActiveCount | integer | 지정 된 기간 동안 활성 상태 이지만 대금 청구 오류가 발생 하 고 있는 구독 수입니다.<ul><li>구독 만료 날짜가 쿼리에 대 한 *endDate* 값 < 됩니다.</li><li>유예 기간의 끝은 *endDate* 값 >=입니다.</li></ul>        |
| lockedActiveCount | integer | *Dunning* 에 있었던 구독 수 (즉, 구독 만료가 임박 하 고 Microsoft는 지정 된 기간 동안 구독을 자동으로 갱신 하는 자금을 확보 하려고 시도 하 고 있습니다.)<ul><li>구독 만료 날짜가 쿼리에 대 한 *endDate* 값 < 됩니다.</li><li>유예 기간의 끝은 *endDate* 값 <=입니다.</li></ul>        |
| billingChurnCount | integer | 청구 요금을 처리 하는 데 실패 하 고 구독이 이전에 dunning 된 위치에서 지정 된 기간 동안 비활성화 된 구독 수입니다.        |
| nonRenewalChurnCount | integer | 갱신 되지 않았기 때문에 지정 된 기간 동안 비활성화 된 구독 수입니다.        |
| refundChurnCount | integer | 지정 된 기간에 환불 된 구독 수입니다.        |
| chargebackChurnCount | integer | 비용 정산 때문에 지정 된 기간 동안 비활성화 된 구독 수입니다.        |
| earlyChurnCount | integer | 지정 된 기간 동안 비활성화 된 구독 중 적절 한 시간 동안 비활성화 된 구독 수입니다.        |
| otherChurnCount | integer | 지정 된 기간 동안 다른 이유로 비활성화 된 구독 수입니다.        |


### <a name="response-example"></a>응답 예제

다음 예제에서는이 요청에 대 한 예제 JSON 응답 본문을 보여 줍니다.

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

* [추가 기능 인수 보고서](../publish/add-on-acquisitions-report.md)
* [Microsoft Store 서비스를 사용 하 여 분석 데이터 액세스](access-analytics-data-using-windows-store-services.md)

 

 
