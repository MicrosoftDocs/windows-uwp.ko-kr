---
author: Xansky
ms.assetid: 1599605B-4243-4081-8D14-40F6F7734E25
description: Microsoft Store 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택 필터 동안 추가 기능의 집계 취득 데이터를 가져옵니다.
title: 추가 기능 구입 정보 가져오기
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API, 추가 기능 취득
ms.localizationpriority: medium
ms.openlocfilehash: 4adb202df2806caeb0dc88469521b0f373886c43
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6044932"
---
# <a name="get-add-on-acquisitions"></a>추가 기능 구입 정보 가져오기

Microsoft Store 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택 필터 동안 앱 추가 기능의 집계 취득 데이터를 JSON 형식으로 가져옵니다. 이 정보는 파트너 센터에서 [추가 기능 구입 보고서](../publish/add-on-acquisitions-report.md) 에서 사용할 수 있습니다.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명          |
|---------------|--------|--------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

*applicationId* 또는 *inAppProductId* 매개 변수가 필요합니다. 앱에 등록된 모든 추가 기능의 구입 데이터를 검색하려면 *applicationId* 매개 변수를 지정합니다. 단일 추가 기능에 대한 구입 데이터를 검색하려면 *inAppProductId* 매개 변수를 지정합니다. 둘 다 지정하는 경우 *applicationId* 매개 변수는 무시됩니다.

| 매개 변수        | 형식   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 추가 기능 취득 데이터를 검색하려는 앱의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다.  |  예  |
| inAppProductId | 문자열 | 취득 데이터를 검색할 추가 기능의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다.  | 예  |
| startDate | date | 검색할 추가 기능 구입 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| endDate | date | 검색할 추가 기능 구입 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다. |  아니요  |
| filter |string  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 자세한 내용은 아래의 [필터 필드](#filter-fields) 섹션을 참조하세요. | 아니요   |
| aggregationLevel | string | 집계 데이터를 검색할 시간 범위를 지정합니다. <strong>day</strong>, <strong>week</strong> 또는 <strong>month</strong> 문자열 중 하나일 수 있습니다. 지정하지 않을 경우 기본값은 <strong>day</strong>입니다. | 아니요 |
| orderby | 문자열 | 각 추가 기능 구입에 대한 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 <em>orderby=field [order],field [order],...</em>입니다. <em>field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>OSVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p><em>order</em> 매개 변수는 옵션이며 <strong>asc</strong> 또는 <strong>desc</strong>로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열 예입니다. <em>orderby=date,market</em></p> |  아니요  |
| groupby | string | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 다음 필드를 지정할 수 있습니다.<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>OSVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>반환되는 데이터 행은 <em>groupby</em> 매개 변수에서 지정된 필드 및 다음을 포함합니다.</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>inAppProductId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p><em>groupby</em> 매개 변수는 <em>aggregationLevel</em> 매개 변수와 함께 사용할 수 있습니다. 예: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  아니요  |


### <a name="filter-fields"></a>필드 필터링

요청의 *filter* 매개 변수에는 응답에서 행을 필터링하는 하나 이상의 문이 포함되어 있습니다. 각 문에는 **eq** 또는 **ne** 연산자와 연결된 필드 및 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 다음은 *filter* 매개 변수의 몇 가지 예입니다.

-   *filter=market eq 'US' and gender eq 'm'*
-   *filter=(market ne 'US') and (gender ne 'Unknown') and (gender ne 'm') and (market ne 'NO') and (ageGroup ne 'greater than 55' or ageGroup ne 'less than 13')*

지원되는 필드 목록은 다음 표를 참조하세요. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다.

| 필드        |  설명        |
|---------------|-----------------|
| acquisitionType | 다음 문자열 중 하나입니다.<ul><li><strong>무료</strong></li><li><strong>평가판</strong></li><li><strong>유료</strong></li><li><strong>홍보 코드</strong></li><li><strong>iap</strong></li></ul> |
| ageGroup | 다음 문자열 중 하나입니다.<ul><li><strong>13보다 작음</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>55보다 큼</strong></li><li><strong>알 수 없음</strong></li></ul> |
| storeClient | 다음 문자열 중 하나입니다.<ul><li><strong>Windows Phone Store(클라이언트)</strong></li><li><strong>Microsoft Store(클라이언트)</strong></li><li><strong>Microsoft Store(웹)</strong></li><li><strong>조직에서 대량 구매</strong></li><li><strong>기타</strong></li></ul> |
| gender | 다음 문자열 중 하나입니다.<ul><li><strong>m</strong></li><li><strong>f</strong></li><li><strong>알 수 없음</strong></li></ul> |
| 출시 | 구입이 발생한 시장의 ISO 3166 국가 코드를 포함하는 문자열입니다. |
| OSVersion | 다음 문자열 중 하나입니다.<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows8</strong></li><li><strong>Windows8.1</strong></li><li><strong>Windows10</strong></li><li><strong>알 수 없음</strong></li></ul> |
| deviceType | 다음 문자열 중 하나입니다.<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>콘솔</strong></li><li><strong>IoT</strong></li><li><strong>홀로그램</strong></li><li><strong>알 수 없음</strong></li></ul> |
| orderName | 추가 기능을 구입하는 데 사용한 홍보 코드 주문 이름을 지정하는 문자열입니다(사용자가 홍보 코드를 사용하여 추가 기능을 구입한 경우에만 적용됨). |


### <a name="request-example"></a>요청 예제

다음 예제에서는 추가 기능 구입 데이터를 가져오는 데 필요한 몇 가지 요청을 보여 줍니다. *inAppProductId* 및 *applicationId* 값을 추가 기능 또는 앱에 대한 스토어 ID로 적절하게 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and ageGroup ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 유형   | 설명         |
|------------|--------|------------------|
| 값      | 배열  | 집계 추가 기능 구입 데이터가 포함된 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 아래 [추가 기능 구입 값](#add-on-acquisition-values) 섹션을 참조하세요.                                                                                                              |
| @nextLink  | string | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10000으로 설정되어 있지만 쿼리에 대한 추가 기능 구입 데이터의 행이 10000개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.    |


<span id="add-on-acquisition-values" />

### <a name="add-on-acquisition-values"></a>추가 기능 구입 값

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값               | 유형    | 설명        |
|---------------------|---------|---------------------|
| date                | 문자열  | 구입 데이터의 날짜 범위에 대한 시작 날짜입니다. 요청에서 하루를 지정한 경우 이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정한 경우 이 값은 해당 날짜 범위의 시작 날짜입니다. |
| inAppProductId      | 문자열  | 구입 데이터를 검색할 추가 기능의 스토어 ID입니다.                                                                                                                                                                 |
| inAppProductName    | 문자열  | 추가 기능의 표시 이름입니다. *aggregationLevel* 매개 변수가 **day**로 설정되어 있는 경우 *groupby* 매개 변수에서 **inAppProductName** 필드를 지정하지 않는 한 이 값은 응답 데이터에만 나타납니다.                                                                                                                                                                                                            |
| applicationId       | 문자열  | 추가 기능 구입 데이터를 검색하려는 앱의 스토어 ID입니다.                                                                                                                                                           |
| applicationName     | string  | 앱의 표시 이름                                                                                                                                                                                                             |
| deviceType          | 문자열  | 구입을 완료한 디바이스의 유형입니다. 지원되는 문자열의 목록은 위의 [필드 필터링](#filter-fields) 섹션을 참조하세요.                                                                                                  |
| orderName           | 문자열  | 주문의 이름입니다.                                                                                                                                                                                                                   |
| storeClient         | 문자열  | 구입이 발생한 스토어의 버전입니다. 지원되는 문자열의 목록은 위의 [필드 필터링](#filter-fields) 섹션을 참조하세요.                                                                                            |
| OSVersion           | 문자열  | 구입이 발생한 OS 버전입니다. 지원되는 문자열의 목록은 위의 [필드 필터링](#filter-fields) 섹션을 참조하세요.                                                                                                   |
| 출시              | 문자열  | 구입이 발생한 시장의 ISO 3166 국가 코드입니다.                                                                                                                                                                  |
| gender              | 문자열  | 구입한 사용자의 성별입니다. 지원되는 문자열의 목록은 위의 [필드 필터링](#filter-fields) 섹션을 참조하세요.                                                                                                    |
| ageGroup            | 문자열  | 구입한 사용자의 연령 그룹입니다. 지원되는 문자열의 목록은 위의 [필드 필터링](#filter-fields) 섹션을 참조하세요.                                                                                                 |
| acquisitionType     | 문자열  | 구입 형식(무료, 유료 등)입니다. 지원되는 문자열의 목록은 위의 [필드 필터링](#filter-fields) 섹션을 참조하세요.                                                                                                    |
| acquisitionQuantity | 정수 | 발생한 구입 횟수입니다.                        |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "date": "2015-01-02",
      "inAppProductId": "9NBLGGH3LHKL",
      "inAppProductName": "Contoso add-on 7",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "GB",
      "gender": "m",
      "ageGroup": "50orover",
      "acquisitionType": "iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "inappacquisitions?applicationId=9NBLGGGZ5QDR&inAppProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```

## <a name="related-topics"></a>관련 항목

* [추가 기능 취득 보고서](../publish/add-on-acquisitions-report.md)
* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [채널별 추가 기능 변환 가져오기](get-add-on-conversions-by-channel.md)
* [앱 획득 가져오기](get-app-acquisitions.md)
* [앱 취득 깔때기 데이터 가져오기](get-acquisition-funnel-data.md)
* [채널별 앱 변환 가져오기](get-app-conversions-by-channel.md)

 

 
