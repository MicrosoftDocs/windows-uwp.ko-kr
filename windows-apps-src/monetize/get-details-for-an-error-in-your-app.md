---
author: mcleanbyron
ms.assetid: f0c0325e-ad61-4238-a096-c37802db3d3b
description: "Windows 스토어 분석 API에서 이 메서드를 사용하여 앱의 특정 오류에 대한 자세한 데이터를 가져올 수 있습니다."
title: "앱에서 오류에 대한 세부 정보 가져오기"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 스토어 서비스, Windows 스토어 분석 API, 오류, 세부 정보"
ms.openlocfilehash: cfab1c8f5149d4c6d02a9fa94287a4e204a11a7f
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="get-details-for-an-error-in-your-app"></a>앱에서 오류에 대한 세부 정보 가져오기

Windows 스토어 분석 API에서 이 메서드를 사용하여 앱의 특정 오류에 대한 자세한 데이터를 JSON 형식으로 가져올 수 있습니다. 이 메서드는 지난 30일 동안 발생한 오류에 대한 세부 정보만 검색할 수 있습니다. 자세한 오류 데이터는 Windows 개발자 센터 대시보드에서 [상태 보고서](../publish/health-report.md)의 **오류** 섹션을 통해서도 확인할 수 있습니다.

이 메서드를 사용하려면 먼저 [오류 보고 데이터 가져오기](get-error-reporting-data.md) 메서드를 통해 자세한 정보를 가져오려는 오류 ID를 검색해야 합니다.

## <a name="prerequisites"></a>필수 조건


이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Windows 스토어 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 자세한 정보를 가져오려는 오류 ID를 가져옵니다. 이 ID를 가져오려면 [오류 보고 데이터 가져오기](get-error-reporting-data.md) 메서드를 사용하고 해당 메서드의 응답 본문에 **failureHash** 값을 사용합니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails``` |

<span/> 

### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |

<span/> 

### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 유형   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 자세한 오류 데이터를 검색할 앱의 스토어 ID입니다. 스토어 ID는 개발자 센터 대시보드의 [앱 ID 페이지](../publish/view-app-identity-details.md)에서 확인할 수 있습니다. 스토어 ID의 예로는 9WZDNCRFJ3Q8이 있습니다. |  예  |
| failureHash | 문자열 | 자세한 정보를 가져오려는 오류의 고유 ID입니다. 관심 있는 오류의 이 값을 가져오려면 [오류 보고 데이터 가져오기](get-error-reporting-data.md) 메서드를 사용하고 해당 메서드의 응답 본문에 **failureHash** 값을 사용합니다. |  예  |
| startDate | date | 검색할 자세한 오류 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜보다 30일 전입니다. |  아니요  |
| endDate | date | 검색할 자세한 오류 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색할 수 있습니다. 예를 들어 top=10 및 skip=0이면 데이터의 처음 10개 행을 검색하고 top=10 및 skip=10이면 데이터의 다음 10개 행을 검색하는 방식입니다. |  아니요  |
| filter |문자열  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 자세한 내용은 아래의 [필터 필드](#filter-fields) 섹션을 참조하세요. | 아니요   |
| orderby | 문자열 | 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 <em>orderby=field [order],field [order],...</em>입니다. <em>field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li><strong>date</strong></li><li><strong>market</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul><p><em>order</em> 매개 변수는 옵션이며 <strong>asc</strong> 또는 <strong>desc</strong>로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열 예입니다. <em>orderby=date,market</em></p> |  아니요  |

<span/>
 
### <a name="filter-fields"></a>필드 필터링

요청의 *filter* 매개 변수에는 응답에서 행을 필터링하는 하나 이상의 문이 포함되어 있습니다. 각 문에는 **eq** 또는 **ne** 연산자와 연결된 필드 및 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 다음은 *filter* 매개 변수의 몇 가지 예입니다.

-   *filter=market eq 'US' and osVersion eq 'Windows 10'*
-   *filter=market ne 'US' and osVersion ne 'Windows 8'*

지원되는 필드 목록은 다음 표를 참조하세요. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다.

| 필드        |  설명        |
|---------------|-----------------|
| osVersion | 다음 문자열 중 하나입니다.<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows8</strong></li><li><strong>Windows8.1</strong></li><li><strong>Windows10</strong></li><li><strong>알 수 없음</strong></li></ul> |
| osBuild | 오류가 발생했을 때 앱을 실행한 OS의 빌드 번호입니다. |
| market | 오류가 발생했을 때 앱을 실행한 디바이스의 지역/국가의 ISO 3166 국가 코드를 포함하는 문자열입니다. |
| deviceType | 오류가 발생했을 때 앱을 실행한 디바이스 유형을 지정하는 다음 문자열 중 하나입니다.<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>알 수 없음</strong></li></ul> |
| deviceModel | 오류가 발생했을 때 앱을 실행한 디바이스 모델을 지정하는 문자열입니다. |
| cabId | 이 오류와 연결된 CAB 파일의 고유 ID입니다. |
| cabExpirationTime | CAB 파일이 만료되고 더 이상 다운로드할 수 없는 ISO 8601 형식의 날짜 및 시간입니다. |
| packageVersion | 이 오류와 연결된 앱 패키지의 버전입니다. |

<span/> 

### <a name="request-example"></a>요청 예제

다음 예제에서는 자세한 오류 데이터를 가져오는 데 필요한 몇 가지 요청을 보여 줍니다. *applicationId* 값을 앱의 스토어 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'Windows.Desktop' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 유형    | 설명    |
|------------|---------|------------|
| 값      | array   | 자세한 오류 데이터를 포함하는 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 아래 [오류 정보 값](#error-detail-values) 섹션을 참조하세요.          |
| @nextLink  | string  | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10으로 설정되어 있지만 쿼리에 대한 오류의 행이 10개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | inumber | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.        |

<span id="error-detail-values"/>
### <a name="error-detail-values"></a>오류 정보 값

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값           | 유형    | 설명     |
|-----------------|---------|----------------------------|
| date            | 문자열  | 오류 데이터에 대한 날짜 범위의 시작 날짜입니다. 요청에서 하루를 지정한 경우 이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정한 경우 이 값은 해당 날짜 범위의 시작 날짜입니다. |
| applicationId   | 문자열  | 자세한 오류 데이터를 검색한 앱의 스토어 ID입니다.      |
| failureName     | 문자열  | 오류의 이름입니다. Windows 개발자 센터 대시보드에서 [상태 보고서](../publish/health-report.md)의 **오류** 섹션에 표시되는 이름과 같습니다.            |
| failureHash     | 문자열  | 오류의 고유 식별자입니다.     |
| osVersion       | 문자열  | 오류가 발생한 OS 버전입니다.    |
| market          | 문자열  | 디바이스 시장의 ISO 3166 국가 코드입니다.     |
| deviceType      | 문자열  | 오류가 발생한 디바이스 유형입니다.     |
| packageVersion  | 문자열  | 이 오류와 연결된 앱 패키지 버전입니다.    |
| osBuild         | 문자열  | 오류가 발생한 OS의 빌드 번호입니다.       |
| cabId           | 문자열  | 이 오류와 연결된 CAB 파일의 고유 ID입니다.   |
| cabExpirationTime  | 문자열  | CAB 파일이 만료되고 더 이상 다운로드할 수 없는 ISO 8601 형식의 날짜 및 시간입니다.   |
| deviceModel           | 문자열  | 오류가 발생했을 때 앱을 실행한 디바이스 모델을 지정하는 문자열입니다.   |
| cabDownloadable           | 부울  | 이 사용자에 대해 CAB 파일을 다운로드할 수 있는지 여부를 나타냅니다.   |

<span/> 

### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR ",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "STOWED_EXCEPTION_System.UriFormatException_exe!ContosoGame.GroupedItems+_ItemView_ItemClick_d__9.MoveNext",
      "date": "2015-02-05 09:11:25",
      "cabId": "133637331323",
      "cabExpirationTime": "2016-12-05 09:11:25",
      "market": "US",
      "osBuild": "10.0.10240",
      "packageVersion": "1.0.2.6",
      "deviceModel": "Contoso Computer",
      "osVersion": "Windows 10",
      "deviceType": "Windows.Desktop",
      "cabDownloadable": false
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>관련 항목

* [상태 보고서](../publish/health-report.md)
* [Windows 스토어 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [오류 보고 데이터 가져오기](get-error-reporting-data.md)
* [앱에서 오류에 대한 스택 추적 가져오기](get-the-stack-trace-for-an-error-in-your-app.md)
