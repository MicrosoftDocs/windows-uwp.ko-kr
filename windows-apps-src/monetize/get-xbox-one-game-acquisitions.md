---
author: mcleanbyron
ms.assetid: C1E42E8B-B97D-4B09-9326-25E968680A0F
description: Microsoft Store 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택 필터 조건에서 Xbox One 게임의 집계 취득 데이터를 가져옵니다.
title: Xbox One 게임 취득 가져오기
ms.author: mcleans
ms.date: 03/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API, Xbox One 게임 취득
ms.localizationpriority: medium
ms.openlocfilehash: 4b0caceae9f5b1e2fabb1b55de33d47e5980e2b5
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1691579"
---
# <a name="get-xbox-one-game-acquisitions"></a>Xbox One 게임 취득 가져오기

Microsoft Store 분석 API에서 이 메서드를 사용하여 Xbox 개발자 포털(XDP)을 통해 수집되고 XDP 분석 개발자 센터 대시보드에서 사용 가능한 Xbox One 게임에 대한 JSON 형식의 집계 취득 데이터를 가져옵니다.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 형식   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 취득 데이터를 검색할 Xbox One 게임의 제품 ID입니다. 게임의 제품 ID를 가져오려면 Xbox 개발자 포털(XDP)에서 사용자 게임으로 이동한 후 URL에서 제품 ID를 검색합니다. 또한 Windows 개발자 센터 분석 보고서에서 취득 데이터를 다운로드하는 경우 제품 ID는 .tsv 파일에 포함됩니다.  |  예  |
| startDate | date | 검색할 구입 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| endDate | date | 검색할 구입 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| top | int | 반환할 데이터의 행 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다. |  아니요  |
| filter | 문자열  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 **eq** 또는 **ne** 연산자와 연결된 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다. 예: *filter=market eq 'US' and gender eq 'm'*. <p/><p/>응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>OSVersion</strong></li><li><strong>deviceType</strong></li><li><strong>sandboxId</strong></li></ul> | 아니요   |
| aggregationLevel | 문자열 | 집계 데이터를 검색할 시간 범위를 지정합니다. <strong>day</strong>, <strong>week</strong> 또는 <strong>month</strong> 문자열 중 하나일 수 있습니다. 지정하지 않을 경우 기본값은 <strong>day</strong>입니다. | 아니요 |
| orderby | 문자열 | 각 구입에 대한 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 <em>orderby=field [order],field [order],...</em>입니다. <em>field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>OSVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li><li><strong>purchasePriceUSDAmount</strong></li><li><strong>taxUSDAmount</strong></li></ul><p><em>order</em> 매개 변수는 선택적이며 <strong>asc</strong> 또는 <strong>desc</strong>로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열 예입니다. <em>orderby=date,market</em></p> |  아니요  |
| groupby | 문자열 | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 다음 필드를 지정할 수 있습니다.<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>OSVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li><li><strong>purchasePriceUSDAmount</strong></li><li><strong>taxUSDAmount</strong></li></ul><p>반환되는 데이터 행은 <em>groupby</em> 매개 변수에서 지정된 필드 및 다음을 포함합니다.</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p><em>groupby</em> 매개 변수는 <em>aggregationLevel</em> 매개 변수와 함께 사용할 수 있습니다. 예: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 Xbox One 게임 취득 데이터를 가져오는 데 필요한 몇 가지 요청을 보여 줍니다. *applicationId* 값을 게임의 Store ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions?applicationId=BRRT4NJ9B3D1&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions?applicationId=BRRT4NJ9B3D1&startDate=8/1/2017&endDate=8/31/2017&skip=0&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 유형   | 설명                  |
|------------|--------|-------------------------------------------------------|
| 값      | 배열  | 게임의 집계 취득 데이터가 포함된 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 아래 [취득 값](#acquisition-values) 섹션을 참조하세요.                                                                                                                      |
| @nextLink  | string | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10000으로 설정되어 있지만 쿼리에 대한 구입 데이터의 행이 10000개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.              |


### <a name="acquisition-values"></a>구입 값

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값               | 유형   | 설명                           |
|---------------------|--------|-------------------------------------------|
| date                | 문자열 | 구입 데이터의 날짜 범위에 대한 시작 날짜입니다. 요청에서 하루를 지정한 경우 이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정한 경우 이 값은 해당 날짜 범위의 시작 날짜입니다. |
| applicationId       | 문자열 | 취득 데이터를 검색할 Xbox One 게임의 제품 ID입니다. |
| applicationName     | 문자열 | 게임의 표시 이름입니다.       |
| acquisitionType     | 문자열 | 취득 유형을 나타내는 다음 문자열 중 하나입니다.<ul><li><strong>무료</strong></li><li><strong>평가판</strong></li><li><strong>유료</strong></li><li><strong>프로모션 코드</strong></li><li><strong>사전 주문</strong></li><li>**Xbox Game Pass**(또는 2018년 3월 23일 이전에 데이터를 쿼리한 경우에는 **Game Pass**임)</li><li><strong>디스크</strong></li><li><strong>선불 코드</strong></li></ul>    |
| ageGroup            | 문자열 | 취득한 사용자 연령 그룹을 나타내는 다음 문자열 중 하나입니다.<ul><li><strong>13보다 적음</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>55보다 큼</strong></li><li><strong>알 수 없음</strong></li></ul>     |
| deviceType          | 문자열 | 취득을 완료한 장치 유형을 나타내는 다음 문자열 중 하나입니다.<ul><li><strong>PC</strong></li><li><strong>휴대폰</strong></li><li><strong>콘솔</strong></li><li><strong>IoT</strong></li><li><strong>서버</strong></li><li><strong>태블릿</strong></li><li><strong>홀로그램</strong></li><li><strong>알 수 없음</strong></li></ul>  |
| 성별              | 문자열 | 취득한 사용자의 성별을 나타내는 다음 문자열 중 하나입니다.<ul><li><strong>m</strong></li><li><strong>f</strong></li><li><strong>알 수 없음</strong></li></ul>     |
| 출시              | 문자열 | 취득이 발생한 지역의 ISO 3166 국가 코드입니다.  |
| osVersion           | 문자열 | 취득이 발생한 OS 버전입니다. 이 메서드에서 이 값은 항상 **Windows 10**입니다.</li></ul>    |
| paymentInstrumentType           | 문자열 | 취득에 사용된 결제 방법을 나타내는 다음 문자열 중 하나입니다.<ul><li><strong>신용 카드</strong></li><li><strong>직불 카드</strong></li><li><strong>유추 구매</strong></li><li><strong>MS 잔액</strong></li><li><strong>이동 통신사</strong></li><li><strong>온라인 계좌 이체</strong></li><li><strong>PayPal</strong></li><li><strong>분할 거래</strong></li><li><strong>토큰 상환</strong></li><li><strong>금액 0 지불</strong></li><li><strong>eWallet</strong></li><li><strong>알 수 없음</strong></li></ul>    |
| sandboxId              | 문자열 | 게임에 대해 생성된 샌드박스 ID입니다. 이는 값 **RETAIL**이거나 개인용 샌드박스의 ID일 수 있습니다.  |
| storeClient         | 문자열 | 취득이 발생한 Microsoft Store의 버전을 나타내는 다음 문자열 중 하나입니다.<ul><li>**Windows Phone Store(클라이언트)**</li><li>**Microsoft Store(클라이언트)**(또는 2018년 3월 23일 이전에 데이터를 쿼리한 경우에는 **Windows Store(클라이언트)** 임)</li><li>**Microsoft Store(웹)**(또는 2018년 3월 23일 이전에 데이터를 쿼리한 경우에는 **Windows Store(웹)** 임)</li><li>**조직에서 대량 구매**</li><li>**기타**</li></ul>                             |
| xboxTitleIdHex              | 문자열 | Xbox Live 지원 게임의 Xbox 개발자 포털(XDP)에서 할당한 Xbox Live 타이틀 ID(16진 값으로 표시)입니다.  |
| acquisitionQuantity | 숫자 | 지정된 집계 수준 중에 발생한 취득 횟수입니다.     |
| purchasePriceUSDAmount | 숫자 | 취득을 위해 고객이 지불한 금액입니다. 이 금액은 월별 환율을 사용하여 USD로 변환됩니다.    |
| taxUSDAmount     | 숫자 | 취득에 적용된 세금이며 USD로 변환됩니다. |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "date": "2017-02-01",
      "applicationId": "BRRT4NJ9B3D1 ",
      "applicationName": "Contoso Game",
      "acquisitionType": "Paid",
      "ageGroup": "35-49",
      "deviceType": "Console",
      "gender": "m",
      "market": "US",
      "osVersion": "Windows 10",
      "PaymentInstrumentType": "Credit Card ",
      "sandboxId": "RETAIL",
      "storeClient": "Windows Store (web)",
      "xboxTitleIdHex": "",
      "acquisitionQuantity": 1,
      "purchasePriceUSDAmount": 29.99,
      "taxUSDAmount": 2.99
    }
  ],
  "@nextLink": "xbox/acquisitions?applicationId=BRRT4NJ9B3D1&aggregationLevel=day&startDate=2017/02/01&endDate=2017/03/01&top=1&skip=1&orderby=date desc",
  "TotalCount": 39812
}
```

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)