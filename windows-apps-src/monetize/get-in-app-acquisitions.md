---
ms.assetid: 1599605B-4243-4081-8D14-40F6F7734E25
description: Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 날짜 범위 및 기타 선택적 필터 중에 추가 기능에 대 한 집계 획득 데이터를 가져옵니다.
title: 추가 기능 구입 가져오기
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 스토어 서비스, Microsoft Store analytics API, 추가 기능 인수
ms.localizationpriority: medium
ms.openlocfilehash: 64605b044c93ee4744d09fe4f44d07bca39285ad
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493218"
---
# <a name="get-add-on-acquisitions"></a>추가 기능 구입 가져오기

Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 날짜 범위 및 기타 선택적 필터를 사용 하 여 앱의 추가 기능에 대 한 추가 기능에 대 한 집계 획득 데이터를 JSON 형식으로 가져올 수 있습니다. 이 정보는 파트너 센터의 [추가 기능 구분 보고서](../publish/add-on-acquisitions-report.md) 에서도 사용할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 분석 API에 대 한 모든 [필수 구성 요소](access-analytics-data-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | Description          |
|---------------|--------|--------------|
| 권한 부여 | 문자열 | 필수 사항입니다. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

*ApplicationId* 또는 *inappproductid* 매개 변수는 필수입니다. 앱에 등록된 모든 추가 기능의 구입 데이터를 검색하려면 *applicationId* 매개 변수를 지정합니다. 단일 추가 기능에 대 한 획득 데이터를 검색 하려면 *Inappproductid* 매개 변수를 지정 합니다. 둘 다 지정 하는 경우 *applicationId* 매개 변수는 무시 됩니다.

| 매개 변수        | Type   |  설명      |  필요한 공간  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 추가 기능 획득 데이터를 검색 하려는 앱의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다.  |  예  |
| inAppProductId | 문자열 | 획득 데이터를 검색 하려는 추가 기능에 대 한 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다.  | 예  |
| startDate | date | 검색할 추가 기능 획득 데이터의 날짜 범위에 있는 시작 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| endDate | date | 검색할 추가 기능 획득 데이터의 날짜 범위에 있는 종료 날짜입니다. 기본값은 현재 날짜입니다. |  예  |
| top | int | 요청에 반환할 데이터 행 수입니다. 지정 되지 않은 경우 최대값 및 기본값은 1만입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 다음 링크가 포함 됩니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용 하 여 많은 데이터 집합을 페이징 합니다. 예를 들어 top = 10000과 skip = 0은 처음 1만 개의 데이터 행을 검색 하 고 top = 10000 및 skip = 10000은 데이터의 다음 1만 행을 검색 하는 식입니다. |  아니요  |
| filter |문자열  | 응답의 행을 필터링 하는 하나 이상의 문입니다. 자세한 내용은 아래의 [필터 필드](#filter-fields) 섹션을 참조 하십시오. | 아니요   |
| aggregationLevel | 문자열 | 집계 데이터를 검색 하는 시간 범위를 지정 합니다. <strong>일</strong>, <strong>주</strong>또는 <strong>월</strong>문자열 중 하나일 수 있습니다. 지정 하지 않으면 기본값은 <strong>day</strong>입니다. | 아니요 |
| orderby | 문자열 | 각 추가 기능 획득에 대 한 결과 데이터 값을 정렬 하는 문입니다. 구문은 <em>orderby = field [order], field [order],...</em>입니다. <em>Field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li><strong>date</strong></li><li><strong>구매 유형</strong></li><li><strong>ageGroup</strong></li><li><strong>유형별</strong></li><li><strong>gender</strong></li><li><strong>시장</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p><em>Order</em> 매개 변수는 선택 사항이 며 <strong>asc</strong> 또는 <strong>desc</strong> 로 각 필드에 대해 오름차순 또는 내림차순을 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열의 예입니다. <em>orderby = date, market</em></p> |  아니요  |
| groupby | 문자열 | 지정 된 필드에만 데이터 집계를 적용 하는 문입니다. 다음 필드를 지정할 수 있습니다.<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>구매 유형</strong></li><li><strong>ageGroup</strong></li><li><strong>유형별</strong></li><li><strong>gender</strong></li><li><strong>시장</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>반환 된 데이터 행에는 <em>groupby</em> 매개 변수 및 다음에 지정 된 필드가 포함 됩니다.</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>inAppProductId</strong></li><li><strong>구매 수량</strong></li></ul><p><em>Groupby</em> 매개 변수는 <em>aggregationLevel</em> 매개 변수와 함께 사용할 수 있습니다. 예: <em> &amp; Groupby = agegroup, market &amp; aggregationLevel = week</em></p> |  아니요  |


### <a name="filter-fields"></a>필터 필드

요청의 *filter* 매개 변수에는 응답의 행을 필터링 하는 문이 하나 이상 포함 되어 있습니다. 각 문에는 **eq** 또는 **ne** 연산자와 연결 된 필드와 값이 포함 되며, **and** 또는 **or**를 사용 하 여 문을 결합할 수 있습니다. 몇 가지 예제 *필터* 매개 변수는 다음과 같습니다.

-   *filter = market eq ' US ' 및 성별 eq ' '*
-   *filter = (market ne ' US ') and (성별 ne ' Unknown ') and (성별 ne ' 없음 ') and (' n o n e n t '가 55 보다 큼 ' 또는 ageGroup ne ' 13 ' 미만)*

지원 되는 필드 목록은 다음 표를 참조 하세요. 문자열 값은 *필터* 매개 변수에서 작은따옴표로 묶어야 합니다.

| 필드        |  설명        |
|---------------|-----------------|
| 구매 유형 | Key, Input, Predict, PredictOnly, None<ul><li><strong>늘릴</strong></li><li><strong>기간인</strong></li><li><strong>지급</strong></li><li><strong>프로 모션 코드</strong></li><li><strong>iap</strong></li></ul> |
| ageGroup | Key, Input, Predict, PredictOnly, None<ul><li><strong>13 미만</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>55 보다 큼</strong></li><li><strong>알 수 없음</strong></li></ul> |
| 유형별 | Key, Input, Predict, PredictOnly, None<ul><li><strong>Windows Phone 스토어(클라이언트)</strong></li><li><strong>Microsoft Store (클라이언트)</strong></li><li><strong>Microsoft Store (웹)</strong></li><li><strong>조직에서 대량 구매</strong></li><li><strong>기타</strong></li></ul> |
| gender | Key, Input, Predict, PredictOnly, None<ul><li><strong>m</strong></li><li><strong>f</strong></li><li><strong>알 수 없음</strong></li></ul> |
| market | 획득이 발생 한 시장의 ISO 3166 국가 코드를 포함 하는 문자열입니다. |
| osVersion | Key, Input, Predict, PredictOnly, None<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>알 수 없음</strong></li></ul> |
| deviceType | Key, Input, Predict, PredictOnly, None<ul><li><strong>컴퓨터</strong></li><li><strong>내선</strong></li><li><strong>콘솔-Xbox One</strong></li><li><strong>콘솔-Xbox 시리즈 X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>알 수 없음</strong></li></ul> |
| orderName | 추가 기능을 획득 하는 데 사용 된 프로 모션 코드의 주문 이름을 지정 하는 문자열입니다 .이는 사용자가 교환 a 판촉 코드를 통해 추가 기능을 획득 한 경우에만 적용 됩니다. |


### <a name="request-example"></a>요청 예제

다음 예제에서는 추가 기능 획득 데이터를 가져오는 몇 가지 요청을 보여 줍니다. *Inappproductid* 및 *applicationId* 값을 추가 기능 또는 앱의 적절 한 상점 ID로 바꿉니다.

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

| 값      | 형식   | Description         |
|------------|--------|------------------|
| 값      | array  | 집계 추가 기능 획득 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 아래의 [추가 기능 획득 값](#add-on-acquisition-values) 섹션을 참조 하십시오.                                                                                                              |
| @nextLink  | 문자열 | 추가 데이터 페이지가 있는 경우이 문자열에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 URI가 포함 됩니다. 예를 들어 요청의 **top** 매개 변수가 1만로 설정 되어 있지만 쿼리에 대 한 추가 기능 획득 데이터의 행이 1만 개를 초과 하는 경우이 값이 반환 됩니다. |
| TotalCount | int    | 쿼리의 데이터 결과에 있는 총 행 수입니다.    |


<span id="add-on-acquisition-values" />

### <a name="add-on-acquisition-values"></a>추가 기능 획득 값

*값* 배열의 요소에는 다음 값이 포함 됩니다.

| 값               | 형식    | 설명        |
|---------------------|---------|---------------------|
| date                | 문자열  | 취득 데이터의 날짜 범위에 있는 첫 번째 날짜입니다. 요청에서 하루를 지정 하는 경우이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정 하는 경우이 값은 해당 날짜 범위의 첫 번째 날짜입니다. |
| inAppProductId      | 문자열  | 획득 데이터를 검색 하는 추가 기능에 대 한 저장소 ID입니다.                                                                                                                                                                 |
| inAppProductName    | 문자열  | 추가 기능에 대 한 표시 이름입니다. 이 값은 *groupby* 매개 변수에서 **inappproductname** 필드를 지정 하지 않는 한 *aggregationLevel* 매개 변수가 **day**로 설정 된 경우에만 응답 데이터에 표시 됩니다.                                                                                                                                                                                                            |
| applicationId       | 문자열  | 추가 기능 획득 데이터를 검색 하려는 앱의 저장소 ID입니다.                                                                                                                                                           |
| applicationName     | 문자열  | 응용 프로그램의 표시 이름입니다.                                                                                                                                                                                                             |
| deviceType          | 문자열  | 가져오기를 완료 한 장치의 유형입니다. 지원 되는 문자열 목록은 위의 [필터 필드](#filter-fields) 섹션을 참조 하십시오.                                                                                                  |
| orderName           | 문자열  | 주문의 이름입니다.                                                                                                                                                                                                                   |
| 유형별         | 문자열  | 획득이 발생 한 저장소의 버전입니다. 지원 되는 문자열 목록은 위의 [필터 필드](#filter-fields) 섹션을 참조 하십시오.                                                                                            |
| osVersion           | 문자열  | 획득이 발생 한 OS 버전입니다. 지원 되는 문자열 목록은 위의 [필터 필드](#filter-fields) 섹션을 참조 하십시오.                                                                                                   |
| market              | 문자열  | 획득이 발생 한 시장의 ISO 3166 국가 코드입니다.                                                                                                                                                                  |
| gender              | 문자열  | 취득 한 사용자의 성별입니다. 지원 되는 문자열 목록은 위의 [필터 필드](#filter-fields) 섹션을 참조 하십시오.                                                                                                    |
| ageGroup            | 문자열  | 가져오기를 수행한 사용자의 나이 그룹입니다. 지원 되는 문자열 목록은 위의 [필터 필드](#filter-fields) 섹션을 참조 하십시오.                                                                                                 |
| 구매 유형     | 문자열  | 취득 유형 (무료, 유료 등)입니다. 지원 되는 문자열 목록은 위의 [필터 필드](#filter-fields) 섹션을 참조 하십시오.                                                                                                    |
| 구매 수량 | integer | 발생 한 인수 수입니다.                        |


### <a name="response-example"></a>응답 예제

다음 예제에서는이 요청에 대 한 예제 JSON 응답 본문을 보여 줍니다.

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

* [추가 기능 인수 보고서](../publish/add-on-acquisitions-report.md)
* [Microsoft Store 서비스를 사용 하 여 분석 데이터 액세스](access-analytics-data-using-windows-store-services.md)
* [채널별 추가 기능 변환 가져오기](get-add-on-conversions-by-channel.md)
* [앱 인수 가져오기](get-app-acquisitions.md)
* [앱 취득 깔때기 데이터 가져오기](get-acquisition-funnel-data.md)
* [채널별 앱 변환 가져오기](get-app-conversions-by-channel.md)

 

 
