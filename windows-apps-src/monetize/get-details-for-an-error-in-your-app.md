---
ms.assetid: f0c0325e-ad61-4238-a096-c37802db3d3b
description: Microsoft Store analytics API에서이 메서드를 사용 하 여 앱에 대 한 특정 오류에 대 한 자세한 데이터를 가져옵니다.
title: 앱에서 오류에 대한 세부 정보 가져오기
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, 저장소 서비스, Microsoft Store 분석 API, 오류, 세부 정보
ms.localizationpriority: medium
ms.openlocfilehash: 52e440fc7e1d174567b9b10c92e38acb3e2edc18
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492808"
---
# <a name="get-details-for-an-error-in-your-app"></a>앱에서 오류에 대한 세부 정보 가져오기

Microsoft Store analytics API에서이 메서드를 사용 하 여 JSON 형식으로 앱에 대 한 특정 오류에 대 한 자세한 데이터를 가져옵니다. 이 메서드는 지난 30 일 동안 발생 한 오류에 대 한 정보만 검색할 수 있습니다. 자세한 오류 데이터는 파트너 센터의 [상태 보고서](../publish/health-report.md) 의 **오류** 섹션 에서도 사용할 수 있습니다.

이 메서드를 사용 하려면 먼저 [get error reporting data](get-error-reporting-data.md) 메서드를 사용 하 여 자세한 정보를 가져올 오류의 ID를 검색 해야 합니다.

## <a name="prerequisites"></a>필수 구성 요소


이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 분석 API에 대 한 모든 [필수 구성 요소](access-analytics-data-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.
* 자세한 정보를 가져오려는 오류의 ID를 가져옵니다. 이 ID를 얻으려면 [get error reporting data](get-error-reporting-data.md) 메서드를 사용 하 고 해당 메서드의 응답 본문에서 **failureHash** 값을 사용 합니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 사항입니다. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | Type   |  설명      |  필요한 공간  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 자세한 오류 데이터를 검색할 앱의 스토어 ID입니다. 저장소 ID는 파트너 센터의 [앱 id 페이지](../publish/view-app-identity-details.md) 에서 사용할 수 있습니다. 예 매장 ID는 9WZDNCRFJ3Q8입니다. |  예  |
| failureHash | 문자열 | 자세한 정보를 가져올 오류에 대 한 고유 ID입니다. 관심이 있는 오류에 대 한이 값을 얻으려면 [get error reporting data](get-error-reporting-data.md) 메서드를 사용 하 고 해당 메서드의 응답 본문에서 **failureHash** 값을 사용 합니다. |  예  |
| startDate | date | 검색할 자세한 오류 데이터의 날짜 범위에 있는 시작 날짜입니다. 기본값은 현재 날짜의 30 일입니다.<p/><p/>**참고:** &nbsp; &nbsp; 이 메서드는 지난 30 일 동안 발생 한 오류에 대 한 정보만 검색할 수 있습니다. |  아니요  |
| endDate | date | 검색할 자세한 오류 데이터의 날짜 범위에 있는 종료 날짜입니다. 기본값은 현재 날짜입니다. |  예  |
| top | int | 요청에 반환할 데이터 행 수입니다. 지정 되지 않은 경우 최대값 및 기본값은 1만입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 다음 링크가 포함 됩니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용 하 여 많은 데이터 집합을 페이징 합니다. 예를 들어 top = 10 및 skip = 0은 처음 10 개의 데이터 행을 검색 하 고 top = 10 및 skip = 10은 다음 10 개의 데이터 행을 검색 하는 식입니다. |  아니요  |
| filter |문자열  | 응답의 행을 필터링 하는 하나 이상의 문입니다. 각 문에는 **eq** 또는 **ne** 연산자와 연결 된 응답 본문 및 값의 필드 이름이 포함 되며, **and** 또는 **or**를 사용 하 여 문을 결합할 수 있습니다. 문자열 값은 *필터* 매개 변수에서 작은따옴표로 묶어야 합니다. 응답 본문에서 다음 필드를 지정할 수 있습니다.<p/><ul><li><strong>시장</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul> | 아니요   |
| orderby | 문자열 | 결과 데이터 값을 정렬 하는 문입니다. 구문은 <em>orderby = field [order], field [order],...</em>입니다. <em>Field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li><strong>시장</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul><p><em>Order</em> 매개 변수는 선택 사항이 며 <strong>asc</strong> 또는 <strong>desc</strong> 로 각 필드에 대해 오름차순 또는 내림차순을 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열의 예입니다. <em>orderby = date, market</em></p> |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예에서는 자세한 오류 데이터를 가져오는 몇 가지 요청을 보여 줍니다. *ApplicationId* 값을 앱의 스토어 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'Windows.Desktop' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 형식    | Description    |
|------------|---------|------------|
| 값      | array   | 자세한 오류 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 아래의 [오류 정보 값](#error-detail-values) 섹션을 참조 하십시오.          |
| @nextLink  | 문자열  | 추가 데이터 페이지가 있는 경우이 문자열에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 URI가 포함 됩니다. 예를 들어 요청의 **top** 매개 변수가 10으로 설정 되어 있지만 해당 쿼리에 대해 10 개 이상의 행이 있는 경우이 값이 반환 됩니다. |
| TotalCount | integer | 쿼리의 데이터 결과에 있는 총 행 수입니다.        |


<span id="error-detail-values"/>

### <a name="error-detail-values"></a>오류 정보 값

*값* 배열의 요소에는 다음 값이 포함 됩니다.

| 값           | 형식    | 설명     |
|-----------------|---------|----------------------------|
| applicationId   | 문자열  | 자세한 오류 데이터를 검색 한 앱의 저장소 ID입니다.      |
| failureHash     | 문자열  | 오류에 대 한 고유 식별자입니다.     |
| failureName     | 문자열  | 오류 이름으로, 하나 이상의 문제 클래스, 예외/버그 검사 코드, 오류가 발생 한 이미지 이름 및 연결 된 함수 이름을 네 부분으로 구성 되어 있습니다.           |
| date            | 문자열  | 오류 데이터에 대 한 날짜 범위의 첫 번째 날짜입니다. 요청에서 하루를 지정 하는 경우이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정 하는 경우이 값은 해당 날짜 범위의 첫 번째 날짜입니다. |
| cabId           | 문자열  | 이 오류와 연결된 CAB 파일의 고유 ID입니다.   |
| cabExpirationTime  | 문자열  | CAB 파일이 만료 되어 더 이상 다운로드할 수 없는 날짜 및 시간 (ISO 8601 형식)입니다.   |
| market          | 문자열  | 장치 시장의 ISO 3166 국가 코드입니다.     |
| osBuild         | 문자열  | 오류가 발생 한 OS의 빌드 번호입니다.       |
| packageVersion  | 문자열  | 이 오류와 연결 된 응용 프로그램 패키지의 버전입니다.    |
| deviceModel           | 문자열  | 오류가 발생 했을 때 앱이 실행 되는 장치의 모델을 지정 하는 문자열입니다.   |
| osVersion       | 문자열  | 오류가 발생 한 OS 버전을 나타내는 다음 문자열 중 하나입니다.<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>알 수 없음</strong></li></ul>    |
| osRelease       | 문자열  |  오류가 발생 한 os 릴리스 또는 플 라이팅 링 (os 버전 내의 subpopulation)을 지정 하는 다음 문자열 중 하나입니다.<p/><p>Windows 10의 경우:</p><ul><li><strong>버전 1507</strong></li><li><strong>버전 1511</strong></li><li><strong>버전 1607</strong></li><li><strong>버전 1703</strong></li><li><strong>버전 1709</strong></li><li><strong>버전 1803</strong></li><li><strong>릴리스 미리 보기</strong></li><li><strong>빠른 참가자</strong></li><li><strong>참가자 느림</strong></li></ul><p/><p>Windows Server 1709:</p><ul><li><strong>출시</strong></li></ul><p>Windows Server 2016:</p><ul><li><strong>버전 1607</strong></li></ul><p>Windows 8.1의 경우:</p><ul><li><strong>Update 1</strong></li></ul><p>Windows 7의 경우:</p><ul><li><strong>서비스 팩 1</strong></li></ul><p>OS 릴리스 또는 플 라이팅 링을 알 수 없는 경우이 필드에 <strong>알 수 없는</strong>값이 있습니다.</p>    |
| deviceType      | 문자열  | 오류가 발생 했을 때 앱이 실행 되는 장치 유형을 지정 하는 다음 문자열 중 하나입니다.<ul><li><strong>컴퓨터</strong></li><li><strong>내선</strong></li><li><strong>콘솔-Xbox One</strong></li><li><strong>콘솔-Xbox 시리즈 X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>알 수 없음</strong></li></ul>     |
| cabDownloadable 가능           | Boolean  | 이 사용자에 대해 CAB 파일을 다운로드할 수 있는지 여부를 나타냅니다.   |


### <a name="response-example"></a>응답 예제

다음 예제에서는이 요청에 대 한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR ",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "STOWED_EXCEPTION_System.UriFormatException_exe!ContosoGame.GroupedItems+_ItemView_ItemClick_d__9.MoveNext",
      "date": "2018-02-05 09:11:25",
      "cabId": "133637331323",
      "cabExpirationTime": "2016-12-05 09:11:25",
      "market": "US",
      "osBuild": "10.0.10240",
      "packageVersion": "1.0.2.6",
      "deviceModel": "Contoso Computer",
      "osVersion": "Windows 10",
      "osRelease": "Version 1507",
      "deviceType": "PC",
      "cabDownloadable": false
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>관련 항목

* [상태 보고서](../publish/health-report.md)
* [Microsoft Store 서비스를 사용 하 여 분석 데이터 액세스](access-analytics-data-using-windows-store-services.md)
* [오류 보고 데이터 가져오기](get-error-reporting-data.md)
* [앱에서 오류에 대한 스택 추적 가져오기](get-the-stack-trace-for-an-error-in-your-app.md)
* [앱의 오류에 대한 CAB 파일 다운로드](download-the-cab-file-for-an-error-in-your-app.md)
