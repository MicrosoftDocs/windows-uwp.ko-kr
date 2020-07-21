---
description: Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 날짜 범위 및 기타 선택적 필터 중에 추가 기능에 대 한 채널 데이터를 통해 집계 변환을 가져올 수 있습니다.
title: 채널별 추가 기능 변환 가져오기
ms.date: 08/04/2017
ms.topic: article
keywords: windows 10, uwp, 저장소 서비스, Microsoft Store 분석 API, 추가 기능 변환, 채널
ms.localizationpriority: medium
ms.openlocfilehash: d2458a880d4fb767eb0158cbeaf0041b58237b61
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493027"
---
# <a name="get-add-on-conversions-by-channel"></a>채널별 추가 기능 변환 가져오기

Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 날짜 범위 및 기타 선택적 필터를 사용 하는 동안 추가 기능에 대 한 채널로 집계 변환을 가져올 수 있습니다.

* *변환은* 고객이 (Microsoft 계정로 로그인 한) 추가 기능에 대 한 라이선스를 새로 획득 했음을 의미 합니다. (요금을 부과 했는지 아니면 무료로 제공 했는지 여부에 관계 없이)
* *채널* 은 고객이 앱의 목록 페이지 (예: 스토어 또는 [사용자 지정 앱 판촉 캠페인](../publish/create-a-custom-app-promotion-campaign.md)을 통해)에 도착 하는 방법입니다.

이 정보는 파트너 센터의 [추가 기능 구분 보고서](../publish/add-on-acquisitions-report.md#add-on-page-views-and-conversions-by-campaign-id) 에서도 사용할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 분석 API에 대 한 모든 [필수 구성 요소](access-analytics-data-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 사항입니다. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | Type   |  설명      |  필요한 공간  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 추가 기능 변환 데이터를 검색 하려는 앱의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다. 예 매장 ID는 9WZDNCRFJ3Q8입니다. |  예  |
| inAppProductId | 문자열 | 변환 데이터를 검색 하려는 추가 기능에 대 한 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다.  | 예  |
| startDate | date | 검색할 변환 데이터의 날짜 범위에 있는 시작 날짜입니다. 기본값은 1/1/2016입니다. |  아니요  |
| endDate | date | 검색할 변환 데이터의 날짜 범위에 있는 종료 날짜입니다. 기본값은 현재 날짜입니다. |  예  |
| top | int | 요청에 반환할 데이터 행 수입니다. 지정 되지 않은 경우 최대값 및 기본값은 1만입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 다음 링크가 포함 됩니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용 하 여 많은 데이터 집합을 페이징 합니다. 예를 들어 top = 10000과 skip = 0은 처음 1만 개의 데이터 행을 검색 하 고 top = 10000 및 skip = 10000은 데이터의 다음 1만 행을 검색 하는 식입니다. |  아니요  |
| filter | 문자열  | 응답 본문을 필터링 하는 하나 이상의 문입니다. 각 문은 **eq** 또는 **ne** 연산자를 사용할 수 있으며, **및** 또는 **또는**를 사용 하 여 문을 조합할 수 있습니다. 필터 문에 다음 문자열을 지정할 수 있습니다.  설명은이 문서의 [변환 값](#conversion-values) 섹션을 참조 하세요. <ul><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>유형별</strong></li><li><strong>deviceType</strong></li><li><strong>시장</strong></li></ul><p>*필터 매개 변수* <em>filter = DEVICETYPE eq ' PC '</em>를 예로 들 수 있습니다.</p> | 아니요   |
| aggregationLevel | 문자열 | 집계 데이터를 검색 하는 시간 범위를 지정 합니다. <strong>일</strong>, <strong>주</strong>또는 <strong>월</strong>문자열 중 하나일 수 있습니다. 지정 하지 않으면 기본값은 <strong>day</strong>입니다. | 아니요 |
| orderby | 문자열 | 각 변환에 대 한 결과 데이터 값을 정렬 하는 문입니다. 구문은 <em>orderby = field [order], field [order],...</em>입니다. <em>Field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>유형별</strong></li><li><strong>deviceType</strong></li><li><strong>시장</strong></li></ul><p><em>Order</em> 매개 변수는 선택 사항이 며 <strong>asc</strong> 또는 <strong>desc</strong> 로 각 필드에 대해 오름차순 또는 내림차순을 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열의 예입니다. <em>orderby = date, market</em></p> |  아니요  |
| groupby | 문자열 | 지정 된 필드에만 데이터 집계를 적용 하는 문입니다. 다음 필드를 지정할 수 있습니다.<p/><ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>유형별</strong></li><li><strong>deviceType</strong></li><li><strong>시장</strong></li></ul><p>반환 된 데이터 행에는 <em>groupby</em> 매개 변수 및 다음에 지정 된 필드가 포함 됩니다.</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>inAppProductId</strong></li><li><strong>inAppProductName</strong></li><li><strong>conversionCount</strong></li><li><strong>클릭 횟수</strong></li></ul><p><em>Groupby</em> 매개 변수는 <em>aggregationLevel</em> 매개 변수와 함께 사용할 수 있습니다. 예: <em>groupby = ageGroup, market &amp; aggregationLevel = week</em></p> |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 앱 변환 데이터를 가져오는 몇 가지 요청을 보여 줍니다. *ApplicationId* 값을 앱의 스토어 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=4/31/2017&skip=0&filter=market eq 'US'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 형식   | Description                  |
|------------|--------|-------------------------------------------------------|
| 값      | array  | 추가 기능에 대 한 집계 변환 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 아래의 [변환 값](#conversion-values) 섹션을 참조 하십시오.                     |
| @nextLink  | 문자열 | 추가 데이터 페이지가 있는 경우이 문자열에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 URI가 포함 됩니다. 예를 들어 요청의 **top** 매개 변수가 10으로 설정 되어 있지만 쿼리에 대 한 변환 데이터 행이 10 개를 초과 하는 경우이 값이 반환 됩니다. |
| TotalCount | int    | 쿼리의 데이터 결과에 있는 총 행 수입니다.                                                                                                                                                                                                                             |

### <a name="conversion-values"></a>변환 값

*값* 배열의 개체에는 다음 값이 포함 됩니다.

| 값               | 형식   | 설명                           |
|---------------------|--------|-------------------------------------------|
| date                | 문자열 | 변환 데이터에 대 한 날짜 범위의 첫 번째 날짜입니다. 요청에서 하루를 지정 하는 경우이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정 하는 경우이 값은 해당 날짜 범위의 첫 번째 날짜입니다. |
| inAppProductId      | 문자열  | 변환 데이터를 검색 하는 추가 기능에 대 한 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다.     |
| inAppProductName    | 문자열  | 변환 데이터를 검색 하는 추가 기능에 대 한 표시 이름입니다.   |
| applicationId       | 문자열 | 변환 데이터를 검색 하는 앱의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다.     |
| applicationName     | 문자열 | 변환 데이터를 검색 하는 응용 프로그램의 표시 이름입니다.        |
| appType          | 문자열 |  변환 데이터를 검색 하는 대상 제품의 유형입니다. 이 메서드의 경우 유일 하 게 지원 되는 값은 **추가 기능**입니다.            |
| customCampaignId           | 문자열 |  앱과 연결 된 [사용자 지정 앱 판촉 캠페인](../publish/create-a-custom-app-promotion-campaign.md) 의 ID 문자열입니다.   |
| referrerUriDomain           | 문자열 |  사용자 지정 앱 판촉 캠페인 ID를 사용 하 여 나열 된 앱이 활성화 된 도메인을 지정 합니다.   |
| channelType           | 문자열 |  변환의 채널을 지정 하는 다음 문자열 중 하나입니다.<ul><li><strong>CustomCampaignId</strong></li><li><strong>트래픽 저장</strong></li><li><strong>기타</strong></li></ul>    |
| 유형별         | 문자열 | 변환이 발생 한 저장소의 버전입니다. 현재 유일 하 게 지원 되는 값은 **SFC**입니다.    |
| deviceType          | 문자열 | Key, Input, Predict, PredictOnly, None<ul><li><strong>컴퓨터</strong></li><li><strong>내선</strong></li><li><strong>콘솔-Xbox One</strong></li><li><strong>콘솔-Xbox 시리즈 X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>알 수 없음</strong></li></ul>            |
| market              | 문자열 | 변환이 발생 한 시장의 ISO 3166 국가 코드입니다.    |
| 클릭 횟수              | number  |     앱 목록 링크에서 고객이 클릭 한 횟수입니다.      |           
| conversionCount            | number  |   고객 변환 수입니다.         |         


### <a name="response-example"></a>응답 예제

다음 예제에서는이 요청에 대 한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "date": "2016-01-01",
      "inAppProductId": "9NBLGGH3LHKL",
      "inAppProductName": "Contoso Add-On",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso App",
      "appType": "Add-On",
      "customCampaignId": "",
      "referrerUriDomain": "Universal Client Store",
      "channelType": "Store Traffic",
      "storeClient": "SFC",
      "deviceType": "PC",
      "market": "CN",
      "clickCount": 1,
      "conversionCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>관련 항목

* [추가 기능 인수 보고서](../publish/add-on-acquisitions-report.md)
* [Microsoft Store 서비스를 사용 하 여 분석 데이터 액세스](access-analytics-data-using-windows-store-services.md)
* [추가 기능 구입 가져오기](get-in-app-acquisitions.md)
