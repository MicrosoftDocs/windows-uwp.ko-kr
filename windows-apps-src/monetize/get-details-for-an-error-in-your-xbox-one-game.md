---
author: Xansky
description: Microsoft Store 분석 API에서에서이 메서드를 사용 하 여 자세한 데이터를 가져올 특정 오류에 대 한 Xbox One에 대 한 게임입니다.
title: 게임에 Xbox One에서 오류에 대 한 세부 정보를 가져오기
ms.author: mhopkins
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API, 오류, 세부 정보
ms.localizationpriority: medium
ms.openlocfilehash: 33733af7f323817bc82d49800c2dc17c5f7b9887
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6978352"
---
# <a name="get-details-for-an-error-in-your-xbox-one-game"></a>게임에 Xbox One에서 오류에 대 한 세부 정보를 가져오기

이 메서드를 사용 하 여 Microsoft Store 분석 API에서 자세한 데이터를 가져올 특정 오류에 대 한 Xbox One에 게임을 Xbox 개발자 포털 (XDP)을 통해 수집 된 있으며 XDP 분석 개발자 센터 대시보드에서 사용할 수 있습니다. 이 메서드는 지난 30일 동안 발생한 오류에 대한 세부 정보만 검색할 수 있습니다.

이 메서드를 사용 하려면 먼저에 자세한 정보를 원하는 오류 ID를 검색 먼저 [Xbox One 게임 한 오류 보고 데이터를 get](get-error-reporting-data-for-your-xbox-one-game.md) 메서드를 사용 해야 합니다.

## <a name="prerequisites"></a>사전 요구 사항


이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 자세한 정보를 가져오려는 오류 ID를 가져옵니다. 이 ID를 가져오려면 [Xbox One에 대 한 오류 보고 데이터를 게임 get](get-error-reporting-data-for-your-xbox-one-game.md) 메서드를 사용 하 고 해당 메서드의 응답 본문에 **failureHash** 값을 사용 합니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 유형   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | string | 오류 세부 정보를 검색할 Xbox One 게임의 제품 ID입니다. 게임의 제품 ID를 가져오려면 Xbox 개발자 포털(XDP)에서 사용자 게임으로 이동한 후 URL에서 제품 ID를 검색합니다. 또는 Windows 개발자 센터 분석 보고서에서 상태 데이터를 다운로드 하는 경우 제품 ID는.tsv 파일에 포함 됩니다. |  예  |
| failureHash | 문자열 | 자세한 정보를 가져오려는 오류의 고유 ID입니다. 관심 있는 오류의이 값을 얻기 위해 [게임에 Xbox One에 대 한 오류 보고 데이터를 get](get-error-reporting-data-for-your-xbox-one-game.md) 메서드를 사용 하 고 해당 메서드의 응답 본문에 **failureHash** 값을 사용 합니다. |  예  |
| startDate | date | 검색할 자세한 오류 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜보다 30일 전입니다. |  아니요  |
| endDate | date | 검색할 자세한 오류 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색할 수 있습니다. 예를 들어 top=10 및 skip=0이면 데이터의 처음 10개 행을 검색하고 top=10 및 skip=10이면 데이터의 다음 10개 행을 검색하는 방식입니다. |  아니요  |
| filter |string  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 **eq** 또는 **ne** 연산자와 연결된 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul> | 아니요   |
| orderby | string | 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 <em>orderby=field [order],field [order],...</em>입니다. <em>field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul><p><em>order</em> 매개 변수는 옵션이며 <strong>asc</strong> 또는 <strong>desc</strong>로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열 예입니다. <em>orderby=date,market</em></p> |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 게임에서 Xbox One에 대 한 자세한 오류 데이터를 가져오는 데 필요한 몇 가지 요청을 보여 줍니다. 게임에 대 한 제품 ID를 사용 하 여 *응용 프로그램 Id* 값을 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails?applicationId=BRRT4NJ9B3D1&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails?applicationId=BRRT4NJ9B3D1&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'Windows.Desktop' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 유형    | 설명    |
|------------|---------|------------|
| 값      | array   | 자세한 오류 데이터를 포함하는 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 아래 [오류 정보 값](#error-detail-values) 섹션을 참조하세요.          |
| @nextLink  | string  | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10으로 설정되어 있지만 쿼리에 대한 오류의 행이 10개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | 정수 | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.        |


<span id="error-detail-values"/>

### <a name="error-detail-values"></a>오류 정보 값

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값           | 유형    | 설명     |
|-----------------|---------|----------------------------|
| applicationId   | string  | 자세한 오류 데이터를 검색 하는 Xbox One 게임의 제품 ID입니다.      |
| failureHash     | string  | 오류의 고유 식별자입니다.     |
| failureName     | 문자열  | 오류 이름은 하나 이상의 문제 클래스, 예외/버그 확인 코드, 오류가 발생한 이미지의 이름 및 관련된 기능 이름 등 네 부분으로 구성됩니다.           |
| date            | string  | 오류 데이터에 대한 날짜 범위의 시작 날짜입니다. 요청에서 하루를 지정한 경우 이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정한 경우 이 값은 해당 날짜 범위의 시작 날짜입니다. |
| cabId           | string  | 이 오류와 연결된 CAB 파일의 고유 ID입니다.   |
| cabExpirationTime  | string  | CAB 파일이 만료되고 더 이상 다운로드할 수 없는 ISO 8601 형식의 날짜 및 시간입니다.   |
| market          | string  | 장치 지역의 ISO 3166 국가 코드입니다.     |
| osBuild         | 문자열  | 오류가 발생한 OS의 빌드 번호입니다.       |
| packageVersion  | string  | 이 오류와 연결 된 게임 패키지의 버전입니다.    |
| deviceModel           | 문자열  | 게임이 오류가 발생할 때 실행 된 Xbox One 콘솔을 나타내는 다음 문자열 중 하나입니다.<p/><ul><li><strong>Microsoft Xbox 하나</strong></li><li><strong>Microsoft Xbox One S</strong></li><li><strong>Microsoft Xbox One X</strong></li></ul>  |
| osVersion       | 문자열  | 오류가 발생한 OS 버전입니다. 항상 **Windows 10**값입니다.    |
| osRelease       | 문자열  |  Windows 10 OS 릴리스 또는 플 라이팅 링 (집단는 OS 버전 내) 오류가 발생 하는 다음 문자열 중 하나입니다.<p/><ul><li><strong>버전 1507</strong></li><li><strong>버전 1511</strong></li><li><strong>버전 1607</strong></li><li><strong>버전 1703</strong></li><li><strong>버전 1709</strong></li><li><strong>버전 1803</strong></li><li><strong>릴리스 미리 보기</strong></li><li><strong>초기 참가자</strong></li><li><strong>이후 참가자</strong></li></ul><p>OS 릴리스 또는 플라이팅 링을 알 수 없는 경우 이 필드의 값은 <strong>알 수 없음</strong>입니다.</p>    |
| deviceType      | 문자열  | 오류가 발생한 디바이스 유형입니다. 이것이 항상 **콘솔**값입니다.     |
| cabDownloadable           | 부울  | 이 사용자에 대해 CAB 파일을 다운로드할 수 있는지 여부를 나타냅니다.   |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "applicationId": "BRRT4NJ9B3D1",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "STOWED_EXCEPTION_System.UriFormatException_exe!ContosoSports.GroupedItems+_ItemView_ItemClick_d__9.MoveNext",
      "date": "2018-02-05 09:11:25",
      "cabId": "133637331323",
      "cabExpirationTime": "2016-12-05 09:11:25",
      "market": "US",
      "osBuild": "10.0.17134",
      "packageVersion": "1.0.2.6",
      "deviceModel": "Microsoft-Xbox One",
      "osVersion": "Windows 10",
      "osRelease": "Version 1803",
      "deviceType": "Console",
      "cabDownloadable": false
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [Xbox One에 대 한 데이터를 보고 하는 오류를 게임 가져오기](get-error-reporting-data-for-your-xbox-one-game.md)
* [Xbox One에서 오류에 대 한 스택 추적을 게임 가져오기](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
* [Xbox One 게임에서 오류에 대 한 CAB 파일 다운로드](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
