---
description: Microsoft Store 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택적 필터에 대한 집계 오류 보고 데이터를 가져옵니다.
title: Xbox One 게임에 대한 오류 보고 데이터 가져오기
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API, 오류
ms.localizationpriority: medium
ms.openlocfilehash: 22dff391e787e1763cb730272ba9cea029758c99
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662078"
---
# <a name="get-error-reporting-data-for-your-xbox-one-game"></a>Xbox One 게임에 대한 오류 보고 데이터 가져오기

Xbox 개발자 포털 (XDP)를 통해 수집 된 및 XDP 분석 파트너 센터 대시보드에서 사용할 수 있는 게임에 Xbox One에 대 한 오류 보고 데이터를 집계를 가져올 수 있도록 Microsoft Store 분석 API에서에서이 메서드를 사용 합니다.

사용 하 여 추가 오류 정보를 검색할 수 있습니다 합니다 [게임에 Xbox One에 오류에 대 한 세부 정보를 가져옵니다](get-details-for-an-error-in-your-xbox-one-game.md)를 [Xbox One에 오류에 대 한 스택 추적을 게임 가져와서](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md), 및 [CAB 파일 다운로드 Xbox One 게임에서 오류가](download-the-cab-file-for-an-error-in-your-xbox-one-game.md) 메서드.

## <a name="prerequisites"></a>필수 구성 요소


이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failurehits``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** &lt; *토큰*&gt;합니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 형식   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 합니다 **Store ID** 는 오류를 검색 하는 Xbox One 게임 데이터를 보고 합니다. 합니다 **Store ID** 파트너 센터에서 앱 id 페이지에서 확인할 수 있습니다. 예로 **Store ID** 9WZDNCRFJ3Q8 됩니다. |  예  |
| startDate | date | 검색할 오류 보고 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜입니다. *aggregationLevel*이 **day**, **week** 또는 **month**인 경우 이 매개 변수는 ```mm/dd/yyyy``` 형식의 날짜를 지정해야 합니다. *aggregationLevel*이 **hour**인 경우 이 매개 변수는 ```mm/dd/yyyy``` 형식의 날짜를 지정하거나 ```yyyy-mm-dd hh:mm:ss``` 형식의 날짜 및 시간을 지정할 수 있습니다.  |  아니오  |
| endDate | date | 검색할 오류 보고 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다. *aggregationLevel*이 **day**, **week** 또는 **month**인 경우 이 매개 변수는 ```mm/dd/yyyy``` 형식의 날짜를 지정해야 합니다. *aggregationLevel*이 **hour**인 경우 이 매개 변수는 ```mm/dd/yyyy``` 형식의 날짜를 지정하거나 ```yyyy-mm-dd hh:mm:ss``` 형식의 날짜 및 시간을 지정할 수 있습니다. |  아니오  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니오  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색할 수 있습니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다. |  아니오  |
| filter |문자열  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 **eq** 또는 **ne** 연산자와 연결된 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul> | 아니오   |
| aggregationLevel | 문자열 | 집계 데이터를 검색할 시간 범위를 지정합니다. **hour**, **day**, **week** 또는 **month** 문자열 중 하나일 수 있습니다. 지정하지 않을 경우 기본값은 **day**입니다. **week** 또는 **month**를 지정하는 경우 *failureName* 및 *failureHash* 값은 1000개 버킷으로 제한됩니다.<p/><p/>**참고:**&nbsp;&nbsp;**hour**를 지정하면 지난 72시간의 오류 데이터만 검색할 수 있습니다. 72시간보다 오래된 오류 데이터를 검색하려면 **day**를 지정하거나 다른 집계 수준 중 하나를 지정합니다.  | 아니오 |
| orderby | 문자열 | 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 *orderby=field [order],field [order],...* 입니다. *field* 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul><p>*order* 매개 변수는 옵션이며 **asc** 또는 **desc**로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 **asc**입니다.</p><p>다음은 *orderby* 문자열 예입니다. *orderby=date,market*</p> |  아니오  |
| groupby | 문자열 | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 다음 필드를 지정할 수 있습니다.<ul><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li></ul><p>반환되는 데이터 행은 *groupby* 매개 변수에서 지정된 필드 및 다음을 포함합니다.</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**deviceCount**</li><li>**eventCount**</li></ul><p>*groupby* 매개 변수는 *aggregationLevel* 매개 변수와 함께 사용할 수 있습니다. 예: *&amp;groupby=failureName,market&amp;aggregationLevel=week*</p></p> |  아니오  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 Xbox One 게임 오류 보고 데이터를 가져오는 데 필요한 몇 개의 요청을 보여 줍니다. 대체는 *applicationId* 값을 **Store ID** 게임에 대 한 합니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failurehits?applicationId=BRRT4NJ9B3D1&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failurehits?applicationId=BRRT4NJ9B3D1&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'Console' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 형식    | 설명     |
|------------|---------|--------------|
| 값      | 배열   | 집계 오류 보고 데이터가 포함된 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 아래 [오류 값](#error-values) 섹션을 참조하세요.     |
| @nextLink  | 문자열  | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10000으로 설정되어 있지만 쿼리에 대한 오류의 행이 10000개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | 정수 | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.     |


### <a name="error-values"></a>오류 값

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값           | 형식    | 설명        |
|-----------------|---------|---------------------|
| date            | 문자열  | 오류 데이터에 대한 날짜 범위의 시작 날짜입니다. 형식은 ```yyyy-mm-dd```입니다. 요청에서 하루를 지정하는 경우 이 값은 해당 날짜입니다. 요청에서 더 긴 날짜 범위를 지정하는 경우 이 값은 해당 날짜 범위의 시작 날짜입니다. 지정 하는 요청에 대 한는 *aggregationLevel* 의 값 **시간**에이 값 형식으로 시간 값을 포함 ```hh:mm:ss``` 오류가 발생 한 현지 표준 시간대에서.  |
| applicationId   | 문자열  | 합니다 **Store ID** 오류 데이터를 검색 하려는 Xbox One 게임입니다.   |
| applicationName | 문자열  | 게임의 표시 이름입니다.   |
| failureName     | 문자열  | 오류 이름은 하나 이상의 문제 클래스, 예외/버그 확인 코드, 오류가 발생한 이미지의 이름 및 관련된 기능 이름 등 네 부분으로 구성됩니다.  |
| failureHash     | 문자열  | 오류의 고유 식별자입니다.   |
| symbol          | 문자열  | 이 오류에 할당된 기호입니다. |
| osVersion       | 문자열  | 오류가 발생한 OS 버전입니다. 이 값은 항상 **Windows 10**합니다.  |
| osRelease       | 문자열  |  지정 하는 Windows 10 운영 체제 릴리스 또는 플 라이팅 링 (OS 버전을 subpopulation) 오류가 발생 한 다음 문자열 중 하나입니다.<p/><ul><li><strong>버전 1507</strong></li><li><strong>버전 1511</strong></li><li><strong>버전 1607</strong></li><li><strong>버전 1703</strong></li><li><strong>버전 1709</strong></li><li><strong>버전 1803</strong></li><li><strong>Release Preview</strong></li><li><strong>참가자 Fast</strong></li><li><strong>느린 참가자</strong></li></ul><p>OS 릴리스 또는 플라이팅 링을 알 수 없는 경우 이 필드의 값은 <strong>알 수 없음</strong>입니다.</p>    |
| eventType       | 문자열  | 다음 문자열 중 하나입니다.<ul><li>**crash**</li><li>**hang**</li><li>**메모리 오류**</li></ul>      |
| 출시          | 문자열  | 디바이스 시장의 ISO 3166 국가 코드입니다.   |
| deviceType      | 문자열  | 오류가 발생한 디바이스 유형입니다. 이 값은 항상 **콘솔**합니다.    |
| packageName     | 문자열  | 이 오류와 연결 된 고유 이름을 게임 패키지입니다.      |
| packageVersion  | 문자열  | 이 오류와 연결 된 게임 패키지의 버전입니다.   |
| deviceCount     | 정수 | 지정된 집계 수준 중에 이 오류에 해당하는 고유 디바이스의 수입니다.  |
| eventCount      | 정수 | 지정된 집계 수준 중에 이 오류를 발생시킨 이벤트의 수입니다.      |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "date": "2017-03-09",
      "applicationId": "BRRT4NJ9B3D1",
      "applicationName": "Contoso Sports",
      "failureName": "APPLICATION_FAULT_8013150a_StoreWrapper.ni.DLL!70475e55",
      "failureHash": "5a6b2170-1661-ed47-24d7-230fed0077af",
      "symbol": "storewrapper_ni!70475e55",
      "osVersion": "Windows 10",
      "osRelease": "Version 1803",
      "eventType": "crash",
      "market": "US",
      "deviceType": "Console",
      "packageName": "",
      "packageVersion": "0.0.0.0",
      "deviceCount": 0.0,
      "eventCount": 1.0
    }
  ],
  "@nextLink": "failurehits?applicationId=BRRT4NJ9B3D1&aggregationLevel=week&startDate=2017/03/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>관련 항목

* [게임에 Xbox One에 오류에 대 한 세부 정보를 가져오기](get-details-for-an-error-in-your-xbox-one-game.md)
* [Xbox One에 오류에 대 한 스택 추적을 게임 가져오기](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
* [Xbox One 게임에서 오류에 대 한 CAB 파일 다운로드](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
* [Microsoft Store 서비스를 사용 하 여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
