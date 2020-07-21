---
ms.assetid: 2967C757-9D8A-4B37-8AA4-A325F7A060C5
description: Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 날짜 범위 및 기타 선택적 필터에 대 한 검토 데이터를 가져옵니다.
title: 앱 리뷰 가져오기
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 스토어 서비스, Microsoft Store 분석 API, 리뷰
ms.localizationpriority: medium
ms.openlocfilehash: 01a22d22245882454fce6eb53b67c4fec0f8072b
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493108"
---
# <a name="get-app-reviews"></a>앱 리뷰 가져오기


Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 날짜 범위 및 기타 선택적 필터에 대 한 JSON 형식으로 검토 데이터를 가져옵니다. 이 정보는 파트너 센터의 [리뷰 보고서](../publish/reviews-report.md) 에서도 사용할 수 있습니다.

검토를 검색 한 후에는 [앱 검토에 대 한 응답 정보 가져오기](get-response-info-for-app-reviews.md) 및 MICROSOFT STORE 리뷰 API의 [앱 검토에 대 한 응답 제출](submit-responses-to-app-reviews.md) 방법을 사용 하 여 프로그래밍 방식으로 검토에 응답할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 분석 API에 대 한 모든 [필수 구성 요소](access-analytics-data-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

## <a name="request"></a>요청

### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | Description                                                                 |
|---------------|--------|---------------------|
| 권한 부여 | 문자열 | 필수 사항입니다. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | Type   |  설명      |  필요한 공간  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 검토 데이터를 검색 하려는 앱의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다.  |  예  |
| startDate | date | 검색할 검토 데이터의 날짜 범위에 있는 시작 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| endDate | date | 검색할 검토 데이터의 날짜 범위에 있는 종료 날짜입니다. 기본값은 현재 날짜입니다. |  예  |
| top | int | 요청에 반환할 데이터 행 수입니다. 지정 되지 않은 경우 최대값 및 기본값은 1만입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 다음 링크가 포함 됩니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용 하 여 많은 데이터 집합을 페이징 합니다. 예를 들어 top = 10000과 skip = 0은 처음 1만 개의 데이터 행을 검색 하 고 top = 10000 및 skip = 10000은 데이터의 다음 1만 행을 검색 하는 식입니다. |  아니요  |
| filter |문자열  | 응답의 행을 필터링 하는 하나 이상의 문입니다. 자세한 내용은 아래의 [필터 필드](#filter-fields) 섹션을 참조 하십시오. | 아니요   |
| orderby | 문자열 | 결과 데이터 값을 정렬 하는 문입니다. 구문은 <em>orderby = field [order], field [order],...</em>입니다. <em>Field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li><strong>date</strong></li><li><strong>osVersion</strong></li><li><strong>시장</strong></li><li><strong>deviceType</strong></li><li><strong>isRevised 됨</strong></li><li><strong>packageVersion</strong></li><li><strong>deviceModel</strong></li><li><strong>Productfamily가</strong></li><li><strong>deviceScreenResolution</strong></li><li><strong>isTouchEnabled</strong></li><li><strong>reviewerName</strong></li><li><strong>reviewTitle</strong></li><li><strong>reviewText</strong></li><li><strong>helpfulCount</strong></li><li><strong>notHelpfulCount</strong></li><li><strong>responseDate</strong></li><li><strong>responseText</strong></li><li><strong>deviceRAM</strong></li><li><strong>deviceStorageCapacity</strong></li><li><strong>점수</strong></li></ul><p><em>Order</em> 매개 변수는 선택 사항이 며 <strong>asc</strong> 또는 <strong>desc</strong> 로 각 필드에 대해 오름차순 또는 내림차순을 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열의 예입니다. <em>orderby = date, market</em></p> |  아니요  |


### <a name="filter-fields"></a>필터 필드

요청의 *filter* 매개 변수에는 응답의 행을 필터링 하는 문이 하나 이상 포함 되어 있습니다. 각 문에는 **eq** 또는 **ne** 연산자와 연결된 필드 및 값이 포함되어 있으며 일부 필드는 **contains**, **gt**, **lt**, **ge** 및 **le** 연산자도 지원합니다. **And** 또는 **or**를 사용 하 여 문을 결합할 수 있습니다.

*필터 문자열의* 예는 filter *= contains (reviewText, ' reviewText ') 및 contains (, ' ads ') 2048 및 포함 (' US ')* 입니다.

각 필드에 대해 지원 되는 필드 및 지원 연산자 목록은 다음 표를 참조 하세요. 문자열 값은 *필터* 매개 변수에서 작은따옴표로 묶어야 합니다.

| 필드        | 지원되는 연산자   |  설명        |
|---------------|--------|-----------------|
| market | eq, ne | 장치 시장의 ISO 3166 국가 코드를 포함 하는 문자열입니다. |
| osVersion  | eq, ne  | Key, Input, Predict, PredictOnly, None<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>알 수 없음</strong></li></ul>  |
| deviceType  | eq, ne  | Key, Input, Predict, PredictOnly, None<ul><li><strong>컴퓨터</strong></li><li><strong>내선</strong></li><li><strong>콘솔-Xbox One</strong></li><li><strong>콘솔-Xbox 시리즈 X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>알 수 없음</strong></li></ul>  |
| isRevised 됨  | eq, ne  | 수정 된 검토를 필터링 하려면 <strong>true</strong> 를 지정 합니다. 그렇지 않으면 <strong>false</strong>입니다.  |
| packageVersion  | eq, ne  | 검토 한 응용 프로그램 패키지의 버전입니다.  |
| deviceModel  | eq, ne  | 앱이 검토된 디바이스 유형입니다.  |
| Productfamily가  | eq, ne  | Key, Input, Predict, PredictOnly, None<ul><li><strong>컴퓨터</strong></li><li><strong>태블릿</strong></li><li><strong>내선</strong></li><li><strong>Wearable</strong></li><li><strong>Server</strong></li><li><strong>작업</strong></li><li><strong>기타</strong></li></ul>  |
| deviceRAM  | eq, ne, gt, lt, ge, le  | 실제 RAM (MB)입니다.  |
| deviceScreenResolution  | eq, ne  | &quot; <em>너비</em> x <em>높이</em>형식의 장치 화면 해상도 &quot; 입니다.   |
| deviceStorageCapacity  | eq, ne, gt, lt, ge, le   | 주 저장소 디스크의 용량 (GB)입니다.  |
| isTouchEnabled  | eq, ne  | 터치 사용 장치를 필터링 하려면 <strong>true</strong> 를 지정 합니다. 그렇지 않으면 <strong>false</strong>입니다.   |
| reviewerName  | eq, ne  |  검토자 이름입니다. |
| rating  | eq, ne, gt, lt, ge, le  | 앱 등급 (별)입니다.  |
| reviewTitle  | eq, ne, contains  | 검토의 제목입니다.  |
| reviewText  | eq, ne, contains  |  검토의 텍스트 내용입니다. |
| helpfulCount  | eq, ne  |  검토가 유용 하 게 표시 된 횟수입니다. |
| notHelpfulCount  | eq, ne  | 검토가 유용 하지 않은 것으로 표시 된 횟수입니다.  |
| responseDate  | eq, ne  | 응답이 전송 된 날짜입니다.  |
| responseText  | eq, ne, contains  | 응답의 텍스트 내용입니다.  |
| id  | eq, ne  | 검토 ID (GUID)입니다.        |


### <a name="request-example"></a>요청 예제

다음 예에서는 검토 데이터를 가져오는 몇 가지 요청을 보여 줍니다. *ApplicationId* 값을 앱의 스토어 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 형식   | Description      |
|------------|--------|------------------|
| 값      | array  | 검토 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 아래의 [값 검토](#review-values) 섹션을 참조 하십시오.       |
| @nextLink  | 문자열 | 추가 데이터 페이지가 있는 경우이 문자열에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 URI가 포함 됩니다. 예를 들어 요청의 **top** 매개 변수가 1만로 설정 되어 있지만 쿼리에 대 한 데이터 검토 행이 1만 개를 초과 하는 경우이 값이 반환 됩니다. |
| TotalCount | int    | 쿼리의 데이터 결과에 있는 총 행 수입니다.  |

 
### <a name="review-values"></a>값 검토

*값* 배열의 요소에는 다음 값이 포함 됩니다.

| 값           | 형식    | 설명       |
|-----------------|---------|-------------------|
| date            | 문자열  | 검토 데이터의 날짜 범위에 있는 첫 번째 날짜입니다. 요청에서 하루를 지정 하는 경우이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정 하는 경우이 값은 해당 날짜 범위의 첫 번째 날짜입니다. |
| applicationId   | 문자열  | 검토 데이터를 검색 중인 앱의 저장소 ID입니다.         |
| applicationName | 문자열  | 응용 프로그램의 표시 이름입니다.    |
| market          | 문자열  | 검토가 제출 된 시장의 ISO 3166 국가 코드입니다.        |
| osVersion       | 문자열  | 검토가 제출 된 OS 버전입니다. 지원 되는 문자열 목록은 위의 [필터 필드](#filter-fields) 섹션을 참조 하십시오.            |
| deviceType      | 문자열  | 검토가 제출 된 장치의 유형입니다. 지원 되는 문자열 목록은 위의 [필터 필드](#filter-fields) 섹션을 참조 하십시오.            |
| isRevised 됨       | Boolean | **True** 값은 검토가 수정 되었음을 나타냅니다. 그렇지 않으면 **false**입니다.   |
| packageVersion  | 문자열  | 검토 한 응용 프로그램 패키지의 버전입니다.        |
| deviceModel        | 문자열  |앱이 검토된 디바이스 유형입니다.     |
| Productfamily가      | 문자열  | 장치 제품군 이름입니다. 지원 되는 문자열 목록은 위의 [필터 필드](#filter-fields) 섹션을 참조 하십시오.   |
| deviceRAM       | number  | 실제 RAM (MB)입니다.    |
| deviceScreenResolution       | 문자열  | "*너비* x *높이*" 형식의 장치 화면 해상도입니다.    |
| deviceStorageCapacity | number | 주 저장소 디스크의 용량 (GB)입니다. |
| isTouchEnabled | Boolean | **True** 값은 터치가 사용 됨을 나타냅니다. 그렇지 않으면 **false**입니다. |
| reviewerName | 문자열 | 검토자 이름입니다. |
| rating | number | 앱 등급 (별)입니다. |
| reviewTitle | 문자열 | 검토의 제목입니다. |
| reviewText | 문자열 | 검토의 텍스트 내용입니다. |
| helpfulCount | number | 검토가 유용 하 게 표시 된 횟수입니다. |
| notHelpfulCount | number | 검토가 유용 하지 않은 것으로 표시 된 횟수입니다. |
| responseDate | 문자열 | 응답이 전송 된 날짜입니다. |
| responseText | 문자열 | 응답의 텍스트 내용입니다. |
| id | 문자열 | 검토 ID (GUID)입니다. 앱 [검토에 대 한 응답 정보 가져오기](get-response-info-for-app-reviews.md) 및 [앱 리뷰에 응답 제출](submit-responses-to-app-reviews.md) 메서드에이 ID를 사용할 수 있습니다. |


### <a name="response-example"></a>응답 예제

다음 예제에서는이 요청에 대 한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "date": "2015-07-29",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "US",
      "osVersion": "10.0.10240.16410",
      "deviceType": "PC",
      "isRevised": true,
      "packageVersion": "",
      "deviceModel": "Microsoft Corporation-Virtual Machine",
      "productFamily": "PC",
      "deviceRAM": -1,
      "deviceScreenResolution": "1024 x 768",
      "deviceStorageCapacity": 51200,
      "isTouchEnabled": false,
      "reviewerName": "ALeksandra",
      "rating": 5,
      "reviewTitle": "Love it",
      "reviewText": "Great app for demos and great dev response.",
      "helpfulCount": 0,
      "notHelpfulCount": 0,
      "responseDate": "2015-08-07T01:50:22.9874488Z",
      "responseText": "1",
      "id": "6be543ff-1c9c-4534-aced-af8b4fbe0316"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>관련 항목

* [검토 보고서](../publish/reviews-report.md)
* [Microsoft Store 서비스를 사용 하 여 분석 데이터 액세스](access-analytics-data-using-windows-store-services.md)
* [앱 리뷰에 대 한 응답 정보 가져오기](get-response-info-for-app-reviews.md)
* [앱 리뷰에 응답 제출](submit-responses-to-app-reviews.md)
* [앱 인수 가져오기](get-app-acquisitions.md)
* [추가 기능 구입 가져오기](get-in-app-acquisitions.md)
* [오류 보고 데이터 가져오기](get-error-reporting-data.md)
* [앱 평점 가져오기](get-app-ratings.md)
