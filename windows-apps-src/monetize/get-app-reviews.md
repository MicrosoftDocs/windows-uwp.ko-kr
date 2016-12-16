---
author: mcleanbyron
ms.assetid: 2967C757-9D8A-4B37-8AA4-A325F7A060C5
description: "Windows 스토어 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택적 필터에 대한 리뷰 데이터를 가져옵니다."
title: "앱 리뷰 가져오기"
translationtype: Human Translation
ms.sourcegitcommit: 7d05c8953f1f50be0b388a044fe996f345d45006
ms.openlocfilehash: 49d3f3cb608f3207306af443c67b684a0ae9f319

---

# <a name="get-app-reviews"></a>앱 리뷰 가져오기


Windows 스토어 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택적 필터에 대한 리뷰 데이터를 JSON 형식으로 가져옵니다. 이 정보는 Windows 개발자 센터 대시보드의 [검토 보고서](../publish/reviews-report.md)를 통해서도 사용할 수 있습니다.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Windows 스토어 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews``` |

<span/> 

### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|---------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |

<span/> 

### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 유형   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 리뷰 데이터를 검색할 앱의 스토어 ID입니다. 스토어 ID는 개발자 센터 대시보드의 [앱 ID 페이지](../publish/view-app-identity-details.md)에서 사용할 수 있습니다. 스토어 ID의 예로는 9WZDNCRFJ3Q8이 있습니다. |  예  |
| startDate | date | 검색할 리뷰 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| endDate | date | 검색할 리뷰 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다. |  아니요  |
| filter |문자열  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 자세한 내용은 아래의 [필터 필드](#filter-fields) 섹션을 참조하세요. | 아니요   |
| orderby | 문자열 | 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 <em>orderby=field [order],field [order],...</em>입니다. <em>field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li><strong>date</strong></li><li><strong>OSVersion</strong></li><li><strong>출시</strong></li><li><strong>deviceType</strong></li><li><strong>isRevised</strong></li><li><strong>packageVersion</strong></li><li><strong>deviceModel</strong></li><li><strong>productFamily</strong></li><li><strong>deviceScreenResolution</strong></li><li><strong>isTouchEnabled</strong></li><li><strong>reviewerName</strong></li><li><strong>reviewTitle</strong></li><li><strong>reviewText</strong></li><li><strong>helpfulCount</strong></li><li><strong>notHelpfulCount</strong></li><li><strong>responseDate</strong></li><li><strong>responseText</strong></li><li><strong>deviceRAM</strong></li><li><strong>deviceStorageCapacity</strong></li><li><strong>등급</strong></li></ul><p><em>order</em> 매개 변수는 옵션이며 <strong>asc</strong> 또는 <strong>desc</strong>로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열 예입니다. <em>orderby=date,market</em></p> |  아니요  |

<span/>
 
### <a name="filter-fields"></a>필드 필터링

요청의 *filter* 매개 변수에는 응답에서 행을 필터링하는 하나 이상의 문이 포함되어 있습니다. 각 문에는 **eq** 또는 **ne** 연산자와 연결된 필드 및 값이 포함되어 있으며 일부 필드는 **contains**, **gt**, **lt**, **ge** 및 **le** 연산자도 지원합니다. 문은 **and** 또는 **or**를 사용하여 결합할 수 있습니다.

다음은 *filter* 문자열 예입니다. *filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US'*

각 필드에 대해 지원되는 필드 및 지원 연산자의 목록은 다음 표를 참조하세요. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다.

| 필드        | 지원되는 연산자   |  설명        |
|---------------|--------|-----------------|
| 출시 | eq, ne | 디바이스 시장의 ISO 3166 국가 코드를 포함하는 문자열입니다. |
| OSVersion  | eq, ne  | 다음 문자열 중 하나입니다.<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>알 수 없음</strong></li></ul>  |
| deviceType  | eq, ne  | 다음 문자열 중 하나입니다.<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>콘솔</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>알 수 없음</strong></li></ul>  |
| isRevised  | eq, ne  | 수정된 리뷰를 필터링하려면 <strong>true</strong>를 지정하고, 그렇지 않으면 <strong>false</strong>를 지정합니다.  |
| packageVersion  | eq, ne  | 검토된 앱 패키지의 버전입니다.  |
| deviceModel  | eq, ne  | 앱이 검토된 디바이스 유형입니다.  |
| productFamily  | eq, ne  | 다음 문자열 중 하나입니다.<ul><li><strong>PC</strong></li><li><strong>태블릿</strong></li><li><strong>휴대폰</strong></li><li><strong>착용식</strong></li><li><strong>서버</strong></li><li><strong>공동 작업</strong></li><li><strong>기타</strong></li></ul>  |
| deviceRAM  | eq, ne, gt, lt, ge, le  | 실제 RAM(MB)입니다.  |
| deviceScreenResolution  | eq, ne  | &quot;<em>너비</em> x <em>높이</em>&quot; 형식의 디바이스 화면 해상도입니다.   |
| deviceStorageCapacity  | eq, ne, gt, lt, ge, le   | 기본 저장소 디스크의 용량(GB)입니다.  |
| isTouchEnabled  | eq, ne  | 터치 사용 디바이스를 필터링하려면 <strong>true</strong>를 지정하고, 그렇지 않으면 <strong>false</strong>를 지정합니다.   |
| reviewerName  | eq, ne  |  검토자 이름입니다. |
| rating  | eq, ne, gt, lt, ge, le  | 앱 평점(별 단위)입니다.  |
| reviewTitle  | eq, ne, contains  | 검토의 제목입니다.  |
| reviewText  | eq, ne, contains  |  리뷰의 텍스트 콘텐츠입니다. |
| helpfulCount  | eq, ne  |  리뷰가 유용하다고 표시된 횟수입니다. |
| notHelpfulCount  | eq, ne  | 리뷰가 유용하지 않다고 표시된 횟수입니다.  |
| responseDate  | eq, ne  | 응답이 제출된 날짜입니다.  |
| responseText  | eq, ne, contains  | 응답의 텍스트 콘텐츠입니다.  |


<span/> 

### <a name="request-example"></a>요청 예제

다음 예제에서는 리뷰 데이터를 가져오는 데 필요한 몇 가지 요청을 보여 줍니다. *applicationId* 값을 앱의 스토어 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 유형   | 설명                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 값      | 배열  | 리뷰 데이터를 포함하는 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 아래 [리뷰 값](#review-values) 섹션을 참조하세요.                                                                                                                                      |
| @nextLink  | 문자열 | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10000으로 설정되어 있지만 쿼리에 대한 리뷰 데이터 행이 10000개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.                                                                                                                                                                                                                             |

<span/>
 
### <a name="review-values"></a>리뷰 값

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값                  | 유형    | 설명                                                                                                                                                                                                                          |
|------------------------|---------|---------------------|
| date                   | 문자열  | 리뷰 데이터에 대한 날짜 범위의 시작 날짜입니다. 요청에서 하루를 지정한 경우 이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정한 경우 이 값은 해당 날짜 범위의 시작 날짜입니다.  |
| applicationId          | 문자열  | 리뷰 데이터를 검색할 앱의 스토어 ID입니다.        |
| applicationName        | 문자열  | 앱의 표시 이름   |
| 출시                 | 문자열  | 리뷰를 제출한 시장의 ISO 3166 국가 코드입니다.       |
| osVersion              | 문자열  | 리뷰를 제출한 OS 버전입니다. 지원되는 문자열 목록은 위의 [필드 필터링](#filter-fields) 섹션을 참조하세요.         |
| deviceType             | 문자열  | 리뷰를 제출한 디바이스 유형입니다. 지원되는 문자열 목록은 위의 [필드 필터링](#filter-fields) 섹션을 참조하세요.      |
| isRevised              | 부울 | 값 **true**는 수정된 리뷰를 나타내고, 그렇지 않으면 **false**입니다.         |
| packageVersion         | 문자열  | 검토된 앱 패키지의 버전입니다.   |
| deviceModel            | 문자열  | 앱이 검토된 디바이스 유형입니다.      |
| productFamily          | 문자열  | 디바이스 패밀리 이름입니다. 지원되는 문자열 목록은 위의 [필드 필터링](#filter-fields) 섹션을 참조하세요.  |
| deviceRAM              | 숫자  | 실제 RAM(MB)입니다.        |
| deviceScreenResolution | 문자열  | "*너비* x *높이*" 형식의 디바이스 화면 해상도입니다.        |
| deviceStorageCapacity  | 숫자  | 기본 저장소 디스크의 용량(GB)입니다.   |
| isTouchEnabled         | 부울 | 값 **true**는 터치를 사용할 수 있음을 나타내고, 그렇지 않으면 **false**입니다.      |
| reviewerName           | 문자열  | 검토자 이름입니다.      |
| rating                 | 숫자  | 앱 평점(별 단위)입니다.         |
| reviewTitle            | 문자열  | 검토의 제목입니다.       |
| reviewText             | 문자열  | 리뷰의 텍스트 콘텐츠입니다.     |
| helpfulCount           | 숫자  | 리뷰가 유용하다고 표시된 횟수입니다.     |
| notHelpfulCount        | 숫자  | 리뷰가 유용하지 않다고 표시된 횟수입니다.               |
| responseDate           | 문자열  | 응답이 제출된 날짜입니다.                 |
| responseText           | 문자열  | 응답의 텍스트 콘텐츠입니다.        |

<span/> 

### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

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
      "responseText": "1"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>관련 항목

* [리뷰 보고서](../publish/reviews-report.md)
* [Windows 스토어 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [앱 획득 가져오기](get-app-acquisitions.md)
* [추가 기능 구입 가져오기](get-in-app-acquisitions.md)
* [오류 보고 데이터 가져오기](get-error-reporting-data.md)
* [앱 평점 가져오기](get-app-ratings.md)



<!--HONumber=Dec16_HO1-->


