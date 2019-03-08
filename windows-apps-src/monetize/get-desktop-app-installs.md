---
description: 이 REST URI를 사용 하 여 지정된 된 날짜 범위 및 다른 선택적 필터 중 데스크톱 응용 프로그램에 대 한 집계 설치 데이터를 가져오려고 합니다.
title: 데스크톱 응용 프로그램 설치 가져오기
ms.date: 03/01/2018
ms.topic: article
keywords: windows 10 데스크톱 앱 설치, Windows 데스크톱 응용 프로그램
localizationpriority: medium
ms.openlocfilehash: f9beb73dafa96c489b8a4c9dddecf59d8a047109
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612978"
---
# <a name="get-desktop-application-installs"></a>데스크톱 응용 프로그램 설치 가져오기


이 REST URI를 사용 하 여에 추가 하는 데스크톱 응용 프로그램에 대 한 JSON 형식으로 집계 설치 데이터를 가져오려고 합니다 [Windows 데스크톱 응용 프로그램](https://msdn.microsoft.com/library/windows/desktop/mt826504)합니다. 이 URI를 사용 하면 지정된 된 날짜 범위 및 다른 선택적 필터 중 설치 데이터를 가져올 수 있습니다. 이 정보를 사용할 수 있습니다 합니다 [설치 보고서](https://msdn.microsoft.com/library/windows/desktop/mt826504) 파트너 센터에서 데스크톱 응용 프로그램에 대 한 합니다.

## <a name="prerequisites"></a>필수 구성 요소


이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily```|


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** &lt; *토큰*&gt;합니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 형식   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 설치 데이터를 검색 하려는 데스크톱 응용 프로그램의 제품 ID입니다. 열 데스크톱 응용 프로그램의 제품 ID를 가져오려면 [파트너 센터에서 데스크톱 응용 프로그램에 대 한 분석 보고서](https://msdn.microsoft.com/library/windows/desktop/mt826504) (같은 합니다 **보고서 설치**) URL에서 제품 ID를 검색 합니다. |  예  |
| startDate | date | 검색할 설치 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜 전 90 일. |  아니오  |
| endDate | date | 검색할 설치 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니오  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니오  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색할 수 있습니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다. |  아니오  |
| filter | 문자열  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 **eq** 또는 **ne** 연산자와 연결된 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li></ul> | 아니오   |
| orderby | 문자열 | 각 설치에 대한 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 <em>orderby=field [order],field [order],...</em>입니다. <em>필드</em> 매개 변수는 응답 본문의 다음 필드 중 하나가 될 수 있습니다.<p/><ul><li><strong>productName</strong></li><li><strong>date</strong><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>installBase</strong></li></ul><p><em>order</em> 매개 변수는 옵션이며 <strong>asc</strong> 또는 <strong>desc</strong>로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열 예입니다. <em>orderby=date,market</em></p> |  아니오  |
| groupby | 문자열 | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li></ul><p>반환되는 데이터 행은 <em>groupby</em> 매개 변수에서 지정된 필드 및 다음을 포함합니다.</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>productName</strong></li><li><strong>installBase</strong></li></ul></p> |  아니오  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 데스크톱 응용 프로그램에 대 한 여러 요청 데이터를 설치 하는 방법을 보여 줍니다. 대체는 *applicationId* 데스크톱 응용 프로그램에 대 한 제품 ID를 가진 값입니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily?applicationId=1234567890&startDate=2018-01-01&endDate=2018-02-01&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily?applicationId=1234567890&startDate=2018-01-01&endDate=2018-02-01&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 형식   | 설명                  |
|------------|--------|-------------------------------------------------------|
| 값      | 배열  | 집계 설치 데이터가 포함된 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 다음 표를 참조하세요. |
| @nextLink  | 문자열 | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10000으로 설정되어 있지만 쿼리에 대한 설치 데이터 행이 10000개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다. |


*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값               | 형식   | 설명                           |
|---------------------|--------|-------------------------------------------|
| date                | 문자열 | 설치 기본 값과 연결 된 날짜입니다. |
| applicationId       | 문자열 | 검색 하는 데스크톱 응용 프로그램의 제품 ID 데이터를 설치 합니다. |
| productName         | 문자열 | 연결된 실행 파일의 메타데이터에서 파생된 데스크톱 응용 프로그램의 표시 이름입니다. |
| applicationVersion  | 문자열 | 설치 된 응용 프로그램 실행 파일의 버전입니다. |
| deviceType          | 문자열 | 데스크톱 응용 프로그램 설치 되어 있는 장치 종류를 지정 하는 다음 문자열 중 하나입니다.<p/><ul><li><strong>PC</strong></li><li><strong>Server</strong></li><li><strong>Tablet</strong></li><li><strong>알 수 없음</strong></li></ul> |
| 출시              | 문자열 | 데스크톱 응용 프로그램을 설치할 때 시장의 ISO 3166 국가 코드입니다. |
| osVersion           | 문자열 | 데스크톱 응용 프로그램이 설치된 OS 버전을 나타내는 다음 문자열 중 하나입니다.<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>알 수 없음</strong></li></ul>   |
| osRelease           | 문자열 | 데스크톱 응용 프로그램이 설치된 OS 릴리스 또는 플라이팅 링(OS 버전 내 하위 집단)을 나타내는 다음 문자열 중 하나입니다.<p/><p>Windows 10용:</p><ul><li><strong>버전 1507</strong></li><li><strong>버전 1511</strong></li><li><strong>버전 1607</strong></li><li><strong>버전 1703</strong></li><li><strong>버전 1709</strong></li><li><strong>Release Preview</strong></li><li><strong>참가자 Fast</strong></li><li><strong>느린 참가자</strong></li></ul><p/><p>Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Windows Server 2016:</p><ul><li><strong>버전 1607</strong></li></ul><p>Windows 8.1:</p><ul><li><strong>업데이트 1</strong></li></ul><p>Windows 7:</p><ul><li><strong>서비스 팩 1</strong></li></ul><p>OS 릴리스 또는 플라이팅 링을 알 수 없는 경우 이 필드의 값은 <strong>알 수 없음</strong>입니다.</p> |
| installBase         | 숫자 | 지정 된 집계 수준에서 설치 된 제품에 고유 장치 수입니다. |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "date": "2018-01-24",
      "applicationId": "123456789",
      "productName": "Contoso Demo",
      "applicationVersion": "1.0.0.0",
      "deviceType": "PC",
      "market": "All",
      "osVersion": "Windows 10",
      "osRelease": "Version 1709",
      "installBase": 348218.0
    }
  ],
  "@nextLink": "desktop/installbasedaily?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>관련 항목

* [Windows 데스크톱 응용 프로그램](https://msdn.microsoft.com/library/windows/desktop/mt826504)
