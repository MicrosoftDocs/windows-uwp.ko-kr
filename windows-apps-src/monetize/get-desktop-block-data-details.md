---
description: 이 REST URI를 사용 하 여 지정 된 날짜 범위 및 기타 선택적 필터에서 데스크톱 응용 프로그램에 대 한 블록 정보 데이터를 가져옵니다.
title: 앱에 대 한 업그레이드 블록 세부 정보 가져오기
ms.date: 07/11/2018
ms.topic: article
keywords: windows 10, 데스크톱 앱 블록, Windows 데스크톱 응용 프로그램 프로그램
localizationpriority: medium
ms.openlocfilehash: b30594ff3943d1ed9183190a0b5a43824816c080
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74735118"
---
# <a name="get-upgrade-block-details-for-your-desktop-application"></a>데스크톱 애플리케이션에 대한 업그레이드 차단 세부 정보 가져오기

이 REST URI를 사용 하 여 데스크톱 응용 프로그램의 특정 실행 파일이 Windows 10 업그레이드 실행을 차단 하는 Windows 10 장치에 대 한 세부 정보를 가져옵니다. [Windows 데스크톱 응용](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)프로그램에 추가한 데스크톱 응용 프로그램에만이 URI를 사용할 수 있습니다. 이 정보는 파트너 센터의 데스크톱 응용 프로그램에 대 한 [응용 프로그램 블록 보고서](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report) 에서도 사용할 수 있습니다.

이 URI는 [데스크톱 응용 프로그램에 대 한 업그레이드 블록을 가져오는](get-desktop-block-data.md)것과 비슷하지만 데스크톱 응용 프로그램에서 특정 실행 파일에 대 한 장치 블록 정보를 반환 합니다.

## <a name="prerequisites"></a>필수 구성 요소


이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails```|


### <a name="request-header"></a>요청 헤더

| 헤더        | 작업 표시줄의 검색 상자에   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **전달자** &lt;*토큰*&gt;의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 작업 표시줄의 검색 상자에   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 블록 데이터를 검색 하려는 데스크톱 응용 프로그램의 제품 ID입니다. 데스크톱 응용 프로그램의 제품 ID를 얻으려면 파트너 센터 (예: **블록 보고서**) [에서 데스크톱 응용 프로그램에 대 한 분석 보고서](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) 를 열고 URL에서 제품 id를 검색 합니다. |  예  |
| fileName | 문자열 | 차단 된 실행 파일의 이름입니다. |
| startDate | date | 검색할 블록 데이터의 날짜 범위에 있는 시작 날짜입니다. 기본값은 현재 날짜 이전 90 일입니다. |  아니요  |
| endDate | date | 검색할 블록 데이터의 날짜 범위에 있는 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색할 수 있습니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다. |  아니요  |
| filter | 문자열  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 **eq** 또는 **ne** 연산자와 연결된 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li><strong>applicationVersion</strong></li><li><strong>마이크로아키텍처</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>시장</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>제품</strong></li><li><strong>targetOs</strong></li></ul> | 아니요   |
| orderby | 문자열 | 각 블록에 대 한 결과 데이터 값을 정렬 하는 문입니다. 구문은 <em>orderby=field [order],field [order],...</em>입니다. <em>field</em> 매개 변수는 응답 본문의 다음 문자열 중 하나일 수 있습니다.<p/><ul><li><strong>applicationVersion</strong></li><li><strong>마이크로아키텍처</strong></li><li><strong>blockType</strong></li><li><strong>date</strong><li><strong>deviceType</strong></li><li><strong>시장</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>제품</strong></li><li><strong>targetOs</strong></li><li><strong>deviceCount</strong></li></ul><p><em>order</em> 매개 변수는 선택적이며 <strong>asc</strong> 또는 <strong>desc</strong>로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열 예입니다. <em>orderby=date,market</em></p> |  아니요  |
| groupby | 문자열 | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li><strong>applicationVersion</strong></li><li><strong>마이크로아키텍처</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>시장</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>targetOs</strong></li></ul><p>반환되는 데이터 행은 <em>groupby</em> 매개 변수에서 지정된 필드 및 다음을 포함합니다.</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>제품</strong></li><li><strong>deviceCount</strong></li></ul></p> |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 데스크톱 응용 프로그램 블록 데이터 가져오기에 대 한 몇 가지 요청을 보여 줍니다. *ApplicationId* 값을 데스크톱 응용 프로그램의 제품 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails?applicationId=10238467886765136388&fileName=contoso.exe&startDate=2018-05-01&endDate=2018-06-07&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails?applicationId=10238467886765136388&fileName=contoso.exe&startDate=2018-05-01&endDate=2018-06-07&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 작업 표시줄의 검색 상자에   | 설명                  |
|------------|--------|-------------------------------------------------------|
| 값      | 배열  | 집계 블록 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 다음 표를 참조하세요. |
| @nextLink  | 문자열 | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 1만로 설정 되어 있지만 쿼리에 대 한 블록 데이터 행이 1만 개를 초과 하는 경우이 값이 반환 됩니다. |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다. |


*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값               | 작업 표시줄의 검색 상자에   | 설명                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 문자열 | 블록 데이터를 검색 한 데스크톱 응용 프로그램의 제품 ID입니다. |
| date                | 문자열 | 블록 적중 값과 관련 된 날짜입니다. |
| productName         | 문자열 | 연결된 실행 파일의 메타데이터에서 파생된 데스크톱 응용 프로그램의 표시 이름입니다. |
| fileName            | 문자열 | 차단 된 실행 파일입니다. |
| applicationVersion  | 문자열 | 차단 된 응용 프로그램 실행 파일의 버전입니다. |
| osVersion           | 문자열 | 데스크톱 응용 프로그램이 현재 실행 중인 OS 버전을 지정 하는 다음 문자열 중 하나입니다.<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>없습니다</strong></li></ul>   |
| osRelease           | 문자열 | 데스크톱 응용 프로그램이 현재 실행 중인 os 릴리스 또는 플 라이팅 링 (os 버전 내에서 하위 채우기로)을 지정 하는 다음 문자열 중 하나입니다.<p/><p>Windows 10:</p><ul><li><strong>버전 1507</strong></li><li><strong>버전 1511</strong></li><li><strong>버전 1607</strong></li><li><strong>버전 1703</strong></li><li><strong>버전 1709</strong></li><li><strong>릴리스 미리 보기</strong></li><li><strong>빠른 참가자</strong></li><li><strong>참가자 느림</strong></li></ul><p/><p>Windows Server 1709:</p><ul><li><strong>출시</strong></li></ul><p>Windows Server 2016:</p><ul><li><strong>버전 1607</strong></li></ul><p>Windows 8.1:</p><ul><li><strong>업데이트 1</strong></li></ul><p>Windows 7:</p><ul><li><strong>서비스 팩 1</strong></li></ul><p>OS 릴리스 또는 플라이팅 링을 알 수 없는 경우 이 필드의 값은 <strong>알 수 없음</strong>입니다.</p> |
| 출시              | 문자열 | 데스크톱 응용 프로그램이 차단 되는 시장의 ISO 3166 국가 코드입니다. |
| deviceType          | 문자열 | 데스크톱 응용 프로그램이 차단 되는 장치 유형을 지정 하는 다음 문자열 중 하나입니다.<p/><ul><li><strong>컴퓨터</strong></li><li><strong>서버인</strong></li><li><strong>표</strong></li><li><strong>없습니다</strong></li></ul> |
| blockType            | 문자열 | 장치에 있는 블록의 유형을 지정 하는 다음 문자열 중 하나입니다.<p/><ul><li><strong>잠재적 sedidi</strong></li><li><strong>임시 Sediment</strong></li><li><strong>런타임 알림</strong></li></ul><p/><p/>이러한 블록 형식에 대 한 자세한 내용과 개발자와 사용자의 의미에 대 한 자세한 내용은 [응용 프로그램 블록 보고서](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report)의 설명을 참조 하세요. |
| architecture        | 문자열 | 블록이 있는 장치의 아키텍처는 다음과 같습니다. <p/><ul><li><strong>ARM64</strong></li><li><strong>X86</strong></li></ul> |
| targetOs            | 문자열 | 데스크톱 응용 프로그램의 실행이 차단 되는 Windows 10 OS 릴리스를 지정 하는 다음 문자열 중 하나입니다. <p/><ul><li><strong>버전 1709</strong></li><li><strong>버전 1803</strong></li></ul> |
| deviceCount         | 숫자 | 지정 된 집계 수준에 블록이 있는 고유 장치 수입니다. |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
     "applicationId": "10238467886765136388",
     "date": "2018-06-03",
     "productName": "Contoso Demo",
     "fileName": "contosodemo.exe",
     "applicationVersion": "2.2.2.0",
     "osVersion": "Windows 8.1",
     "osRelease": "Update 1",
     "market": "ZA",
     "deviceType": "All",
     "blockType": "Runtime Notification",
     "architecture": "X86",
     "targetOs": "RS4",
     "deviceCount": 120
    }
  ],
  "@nextLink": "desktop/blockdetails?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>관련 항목

* [Windows 데스크톱 응용 프로그램 프로그램](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [데스크톱 응용 프로그램에 대 한 업그레이드 블록 가져오기](get-desktop-block-data.md)
