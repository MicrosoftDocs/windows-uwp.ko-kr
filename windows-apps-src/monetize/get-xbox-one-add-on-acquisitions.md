---
description: Microsoft Store 분석 API에서에서이 메서드를 사용 하 여 추가 기능의 집계 취득 데이터를 가져옵니다.
title: Xbox One 추가 기능 획득 가져오기
ms.date: 10/18/2018
ms.topic: article
keywords: windows 10, uwp, 스토어 서비스, Microsoft Store 분석 API, Xbox One 추가 기능 구입
ms.localizationpriority: medium
ms.openlocfilehash: f102d2d692a2307c25dcb95e66d612fc561dec70
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7717983"
---
# <a name="get-xbox-one-add-on-acquisitions"></a>Xbox One 추가 기능 획득 가져오기

이 메서드를 사용 하 여 Microsoft Store 분석 API에서 집계 추가 기능 구입 데이터를 JSON 형식에 대 한 가져오려면 Xbox One 게임을 Xbox 개발자 포털 (XDP)을 통해 수집 된 있으며 XDP 분석 파트너 센터 대시보드에서 사용할 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명          |
|---------------|--------|--------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

*ApplicationId* 또는 *addonProductId* 매개 변수가 필요 합니다. 앱에 등록된 모든 추가 기능의 구입 데이터를 검색하려면 *applicationId* 매개 변수를 지정합니다. 단일 추가 기능에 대 한 구입 데이터를 검색 하려면 *addonProductId* 매개 변수를 지정 합니다. 둘 다 지정하는 경우 *applicationId* 매개 변수는 무시됩니다.

| 매개 변수        | 유형   |  설명      |  필수  |
|---------------|--------|---------------|------|
| applicationId | string | 구입 데이터를 검색할 Xbox One 게임의 *제품 Id* 입니다. 게임의 *제품 Id* 를 가져오려면 XDP 분석 프로그램에 게임을 찾아 URL에서 *제품 Id* 를 검색 합니다. 또는 파트너 센터 분석 보고서에서 구입 데이터를 다운로드 하는 경우 *제품 Id* 는.tsv 파일에 포함 됩니다. |  예  |
| addonProductId | string | 추가 기능 구입 데이터를 검색 하려는 *productId* 합니다.  | 예  |
| startDate | date | 검색할 추가 기능 구입 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| endDate | date | 검색할 추가 기능 구입 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다. |  아니요  |
| filter |string  | <p>응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 eq 또는 ne 연산자와 연결된 값이 포함되어 있으며 문은 and 또는 or를 사용하여 결합될 수 있습니다. 문자열 값은 filter 매개 변수에서 단일 따옴표로 묶여야 합니다. 예: filter=market eq 'US' and gender eq 'm'.</p> <p>응답 본문의 다음과 같은 필드를 지정할 수 있습니다.</p> <ul><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>OSVersion</strong></li><li><strong>deviceType</strong></li><li><strong>sandboxId</strong></li></ul>| 아니요   |
| aggregationLevel | string | 집계 데이터를 검색할 시간 범위를 지정합니다. <strong>day</strong>, <strong>week</strong> 또는 <strong>month</strong> 문자열 중 하나일 수 있습니다. 지정하지 않을 경우 기본값은 <strong>day</strong>입니다. | 아니요 |
| orderby | 문자열 | 각 추가 기능 구입에 대한 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 <em>orderby = field [order], [order], 필드...</em> <em>필드</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>OSVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p><em>order</em> 매개 변수는 옵션이며 <strong>asc</strong> 또는 <strong>desc</strong>로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열 예입니다. <em>orderby=date,market</em></p> |  아니요  |
| groupby | string | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 다음 필드를 지정할 수 있습니다.<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>addonProductName</strong></li><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>OSVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li></ul><p>반환되는 데이터 행은 <em>groupby</em> 매개 변수에서 지정된 필드 및 다음을 포함합니다.</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>addonProductId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p><em>groupby</em> 매개 변수는 <em>aggregationLevel</em> 매개 변수와 함께 사용할 수 있습니다. 예: <em> &amp;groupby = 연령 지역/국가&amp;aggregationLevel = week</em></p> |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 추가 기능 구입 데이터를 가져오는 데 필요한 몇 가지 요청을 보여 줍니다. 추가 기능 또는 앱에 대 한 적절 한 스토어 ID로 *addonProductId* 및 *응용 프로그램 Id* 값을 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?addonProductId=BRRT4NJ9B3D2&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?applicationId=BRRT4NJ9B3D1&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?addonProductId=BRRT4NJ9B3D2&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and age ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 유형   | 설명         |
|------------|--------|------------------|
| 값      | 배열  | 집계 추가 기능 구입 데이터가 포함된 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 아래 [추가 기능 구입 값](#add-on-acquisition-values) 섹션을 참조하세요.                                                                                                              |
| @nextLink  | string | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10000으로 설정되어 있지만 쿼리에 대한 추가 기능 구입 데이터의 행이 10000개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.    |


<span id="add-on-acquisition-values" />

### <a name="add-on-acquisition-values"></a>추가 기능 구입 값

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값               | 유형    | 설명        |
|---------------------|---------|---------------------|
| date                | 문자열  | 구입 데이터의 날짜 범위에 대한 시작 날짜입니다. 요청에서 하루를 지정한 경우 이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정한 경우 이 값은 해당 날짜 범위의 시작 날짜입니다. |
| addonProductId      | string  | 구입 데이터를 검색할 추가 기능의 *제품 Id* 입니다.                                                                                                                                                                 |
| addonProductName    | 문자열  | 추가 기능의 표시 이름입니다. 이 값에에서만 나타납니다 응답 데이터 *aggregationLevel* 매개 변수는 **하루**설정 된 경우 *groupby* 매개 변수에서 **addonProductName** 필드를 지정 하지 않는 한 합니다.                                                                                                                                                                                                            |
| applicationId       | string  | 추가 기능 구입 데이터를 검색 하려는 앱의 *제품 Id* 입니다.                                                                                                                                                           |
| applicationName     | 문자열  | 게임의 표시 이름입니다.                                                                                                                                                                                                             |
| deviceType          | 문자열  | <p>취득을 완료한 장치 유형을 나타내는 다음 문자열 중 하나입니다.</p> <ul><li>"PC"</li><li>"Phone"</li><li>"콘솔"</li><li>"IoT"</li><li>"서버"</li><li>"태블릿"</li><li>"홀로그램"</li><li>"알 수 없음"</li></ul>                                                                                                  |
| storeClient         | 문자열  | <p>취득이 발생한 Microsoft Store의 버전을 나타내는 다음 문자열 중 하나입니다.</p> <ul><li>"Windows Phone 스토어 (클라이언트)"</li><li>"Microsoft Store (클라이언트)" (또는 "Windows 스토어 (클라이언트)" 2018 년 3 월 23 일에 대 한 데이터를 쿼리 하는 경우)</li><li>"Microsoft Store (웹)" (또는 "Windows 스토어 (웹)" 2018 년 3 월 23 일에 대 한 데이터를 쿼리 하는 경우)</li><li>"조직에서 대량 구매"</li><li>"기타"</li></ul>                                                                                            |
| osVersion           | 문자열  | 구입이 발생한 OS 버전입니다. 이 메서드에 대 한이 값은 항상 "Windows 10".                                                                                                   |
| market              | 문자열  | 구입이 발생한 시장의 ISO 3166 국가 코드입니다.                                                                                                                                                                  |
| gender              | 문자열  | <p>취득한 사용자의 성별을 나타내는 다음 문자열 중 하나입니다.</p> <ul><li>"m"</li><li>"f"</li><li>"알 수 없음"</li></ul>                                                                                                    |
| age            | 문자열  | <p>취득한 사용자 연령 그룹을 나타내는 다음 문자열 중 하나입니다.</p> <ul><li>"보다 작은 13"</li><li>"13 17"</li><li>"18-24"</li><li>"25 34"</li><li>"35-44"</li><li>"44-55"</li><li>"55 보다 큰"</li><li>"알 수 없음"</li></ul>                                                                                                 |
| acquisitionType     | 문자열  | <p>취득 유형을 나타내는 다음 문자열 중 하나입니다.</p> <ul><li>"무료"</li><li>"평가판"</li><li>"결제"</li><li>"홍보 코드"</li><li>"Iap"</li><li>"구독 Iap"</li><li>"개인 대상 그룹"</li><li>"이전 순서"</li><li>"Xbox Game Pass" (또는 "Game Pass" 2018 년 3 월 23 일에 대 한 데이터를 쿼리 하는 경우)</li><li>"디스크"</li><li>"선불된 코드"</li><li>"부과 전 순서"</li><li>"이전 순서를 취소"</li><li>"이전 순서 하지 못했습니다."</li></ul>                                                                                                    |
| acquisitionQuantity | 정수 | 발생한 구입 횟수입니다.                        |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "date": "2018-10-18",
      "addonProductId": " BRRT4NJ9B3D2",
      "addonProductName": "Contoso add-on 7",
      "applicationId": "BRRT4NJ9B3D1",
      "applicationName": "Contoso Demo",
      "deviceType": "Console",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows 10",
      "market": "GB",
      "gender": "m",
      "age": "50orover",
      "acquisitionType": "iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "addonacquisitions?applicationId=BRRT4NJ9B3D1&addonProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```
