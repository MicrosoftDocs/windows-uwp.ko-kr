---
description: 이 REST URI를 사용 하 여 지정된 된 날짜 범위 및 다른 선택적 필터 중 데스크톱 응용 프로그램에 대 한 블록에서 데이터를 가져오려고 합니다.
title: 데스크톱 응용 프로그램에 대한 업그레이드 차단 가져오기
ms.date: 07/11/2018
ms.topic: article
keywords: windows 10 데스크톱 응용 프로그램 블록, Windows 데스크톱 응용 프로그램
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4bd57d61a3d0ee942742ab1688849ab181c8c71d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372101"
---
# <a name="get-upgrade-blocks-for-your-desktop-application"></a>데스크톱 응용 프로그램에 대한 업그레이드 차단 가져오기

데스크톱 응용 프로그램은 Windows 10 업그레이드를 실행에서을 차단 하는 데 기반이 되는 Windows 10 장치에 대 한 정보를 가져오려면이 REST URI를 사용 합니다. 이 URI에 추가 하는 데스크톱 응용 프로그램에 대해서만 사용할 수 있습니다 합니다 [Windows 데스크톱 응용 프로그램](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)합니다. 이 정보를 사용할 수 있습니다 합니다 [응용 프로그램 차단 보고서](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report) 파트너 센터에서 데스크톱 응용 프로그램에 대 한 합니다.

데스크톱 응용 프로그램에서 특정 실행 파일에 대 한 장치 블록에 대 한 세부 정보를 가져오려는 참조 [데스크톱 응용 프로그램에 대 한 세부 정보 가져오기 업그레이드할](get-desktop-block-data-details.md)합니다.

## <a name="prerequisites"></a>사전 요구 사항

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI       |
|--------|----------------------|
| 가져오기    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits```|


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** &lt; *토큰*&gt;합니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 형식   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | string | 블록에서 데이터를 검색 하려는 데스크톱 응용 프로그램의 제품 ID입니다. 열 데스크톱 응용 프로그램의 제품 ID를 가져오려면 [파트너 센터에서 데스크톱 응용 프로그램에 대 한 분석 보고서](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) (같은 합니다 **보고서 차단**) URL에서 제품 ID를 검색 합니다. |  예  |
| startDate | date | 검색할 블록에서 데이터를 날짜 범위의 시작 날짜입니다. 기본값은 현재 날짜 전 90 일. |  아니요  |
| endDate | date | 검색할 블록에서 데이터를 날짜 범위의 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| top | ssNoversion | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니요  |
| skip | ssNoversion | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색할 수 있습니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다. |  아니요  |
| filter | string  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 **eq** 또는 **ne** 연산자와 연결된 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>fileName</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>productName</strong></li><li><strong>targetOs</strong></li></ul> | 아니오   |
| orderby | string | 각 블록에 대 한 데이터 값 결과 정렬 하는 문입니다. 구문은 <em>orderby=field [order],field [order],...</em>입니다. <em>필드</em> 매개 변수는 응답 본문의 다음 필드 중 하나가 될 수 있습니다.<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>date</strong><li><strong>deviceType</strong></li><li><strong>fileName</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>productName</strong></li><li><strong>targetOs</strong></li><li><strong>deviceCount</strong></li></ul><p><em>order</em> 매개 변수는 옵션이며 <strong>asc</strong> 또는 <strong>desc</strong>로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열 예입니다. <em>orderby=date,market</em></p> |  아니요  |
| groupby | string | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>fileName</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>targetOs</strong></li></ul><p>반환되는 데이터 행은 <em>groupby</em> 매개 변수에서 지정된 필드 및 다음을 포함합니다.</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>productName</strong></li><li><strong>deviceCount</strong></li></ul></p> |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 데스크톱 응용 프로그램 블록에서 데이터를 가져오는 데 필요한 몇 개의 요청을 보여 줍니다. 대체는 *applicationId* 데스크톱 응용 프로그램에 대 한 제품 ID를 가진 값입니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits?applicationId=5126873772241846776&startDate=2018-05-01&endDate=2018-06-07&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits?applicationId=5126873772241846776&startDate=2018-05-01&endDate=2018-06-07&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 형식   | 설명                  |
|------------|--------|-------------------------------------------------------|
| 값      | 배열  | 배열 집계 블록에서 데이터를 포함 하는 개체입니다. 각 개체의 데이터에 대한 자세한 내용은 다음 표를 참조하세요. |
| @nextLink  | string | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 하는 경우이 값이 반환 하는 예를 들어 합니다 **위쪽** 10000 요청의 매개 변수는 설정 되지만 쿼리에 대 한 데이터 블록의 행을 10000 개. |
| TotalCount | ssNoversion    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다. |


*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값               | 형식   | 설명                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | 블록에서 데이터를 검색 하는 데스크톱 응용 프로그램의 제품 ID입니다. |
| date                | string | 블록 적중 값과 연결 된 날짜입니다. |
| productName         | string | 연결된 실행 파일의 메타데이터에서 파생된 데스크톱 응용 프로그램의 표시 이름입니다. |
| fileName            | string | 차단 된 실행 파일입니다. |
| applicationVersion  | string | 차단 된 응용 프로그램 실행 파일의 버전입니다. |
| osVersion           | string | 데스크톱 응용 프로그램은 현재 실행 중인 OS 버전을 지정 하는 다음 문자열 중 하나입니다.<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>알 수 없음</strong></li></ul>   |
| osRelease           | string | 지정 하는 OS 릴리스 또는 플 라이팅 링 (OS 버전을 subpopulation) 데스크톱 응용 프로그램 현재 실행 되는 다음 문자열 중 하나입니다.<p/><p>Windows 10용:</p><ul><li><strong>버전 1507</strong></li><li><strong>버전 1511</strong></li><li><strong>버전 1607</strong></li><li><strong>버전 1703</strong></li><li><strong>버전 1709</strong></li><li><strong>Release Preview</strong></li><li><strong>참가자 Fast</strong></li><li><strong>느린 참가자</strong></li></ul><p/><p>Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Windows Server 2016:</p><ul><li><strong>버전 1607</strong></li></ul><p>Windows 8.1:</p><ul><li><strong>업데이트 1</strong></li></ul><p>Windows 7:</p><ul><li><strong>서비스 팩 1</strong></li></ul><p>OS 릴리스 또는 플라이팅 링을 알 수 없는 경우 이 필드의 값은 <strong>알 수 없음</strong>입니다.</p> |
| 출시              | string | 데스크톱 응용 프로그램은 차단 된 시장의 ISO 3166 국가 코드입니다. |
| deviceType          | string | 데스크톱 응용 프로그램은 차단 하는 장치의 종류를 지정 하는 다음 문자열 중 하나입니다.<p/><ul><li><strong>PC</strong></li><li><strong>Server</strong></li><li><strong>Tablet</strong></li><li><strong>알 수 없음</strong></li></ul> |
| blockType            | string | 장치에서 발견 되는 블록의 형식을 지정 하는 다음 문자열 중 하나입니다.<p/><ul><li><strong>잠재적인 Sediment</strong></li><li><strong>임시 Sediment</strong></li><li><strong>런타임 알림</strong></li></ul><p/><p/>이러한 블록 형식 및 개발자와 사용자에 게 의미 하는 방법에 대 한 자세한 내용은 참조에 대 한 설명을 합니다 [응용 프로그램 차단 보고서](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report)합니다.  |
| architecture        | string | 블록 존재 하는 장치의 아키텍처: <p/><ul><li><strong>ARM64</strong></li><li><strong>X86</strong></li></ul> |
| targetOs            | string | 데스크톱 응용 프로그램 실행에서 차단 됩니다는 Windows 10 운영 체제 릴리스를 지정 하는 다음 문자열 중 하나입니다. <p/><ul><li><strong>버전 1709</strong></li><li><strong>버전 1803</strong></li></ul> |
| deviceCount         | number | 지정 된 집계 수준에서 블록에 있는 고유 장치의 수입니다. |


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
  "@nextLink": "desktop/blockhits?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>관련 항목

* [Windows 데스크톱 응용 프로그램](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [데스크톱 응용 프로그램에 대 한 업그레이드 블록 세부 정보 가져오기](get-desktop-block-data-details.md)
