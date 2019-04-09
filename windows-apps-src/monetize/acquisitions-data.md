---
description: Microsoft Store analytics API에서에서이 메서드를 사용 하 여 UWP 앱 및 Xbox 개발자 포털 (XDP)를 통해 수집 된 및 XDP 분석 대시보드에서 사용할 수 있는 게임을 Xbox One에 대 한 JSON 형식으로 집계 취득 데이터를 가져오려고 합니다.
title: 게임 및 앱 구매 데이터를 가져오기
ms.date: 03/06/2019
ms.topic: article
keywords: windows 10, uwp, 광고 네트워크, 앱 메타데이터
ms.localizationpriority: medium
ms.openlocfilehash: beca5620f25713e8a07e5dbaf64e955d920702a7
ms.sourcegitcommit: df8e4143e81a1c5fe1aa5f14407b8dd5f155a12e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829916"
---
# <a name="get-acquisitions-data-for-your-games-and-apps"></a>게임 및 앱 구매 데이터를 가져오기 
Microsoft Store analytics API에서에서이 메서드를 사용 하 여 UWP 앱 및 Xbox 개발자 포털 (XDP)를 통해 수집 된 및 XDP 분석 대시보드에서 사용할 수 있는 게임을 Xbox One에 대 한 JSON 형식으로 집계 취득 데이터를 가져오려고 합니다. 

> [!NOTE]
> 이 API는 2016 년 10 월 1 일 전에 매일 집계 데이터를 제공 하지 않습니다. 

## <a name="prerequisites"></a>사전 요구 사항
이 메서드를 사용하려면 다음을 먼저 수행해야 합니다. 

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md)을 완료합니다. 
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다. 

## <a name="request"></a>요청
### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI |
| --- | --- |
| 가져오기 | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions` |

### <a name="request-header"></a>요청 헤더

| 헤더 | 형식 | 설명 |
| --- | --- | --- |
| Authorization | string | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** `<token>`합니다. |

### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수 | 형식 | 설명 | 필수 |
| --- | --- | --- | --- |
| applicationId | string | 취득 데이터를 검색할 Xbox One 게임의 제품 ID입니다. 게임의 제품 ID를 가져오려면 XDP 분석 프로그램의 게임을 찾아 URL에서 제품 ID를 검색 합니다. 또는 파트너 센터 분석 보고서에서 구매 데이터를 다운로드 하는 경우 제품 ID.tsv 파일에 포함 됩니다.  | 예 |
| startDate | date | 검색할 구입 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜입니다.  | 아니요 |
| endDate | date | 검색할 구입 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다.  | 아니요 |
| top | integer | 반환할 데이터의 행 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다.  | 아니요 |
| skip | integer | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색할 수 있습니다. 예를 들어 *위쪽 건너뛰고 = 10000 = 0* 먼저 10000 행의 데이터를 검색 *위쪽 건너뛰고 = 10000 = 10000* 데이터의 다음 10000 행을 검색 합니다.  | 아니요 |
| filter | string | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 **eq** 또는 **ne** 연산자와 연결된 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 문자열 값 필터 매개 변수에 작은따옴표로 묶어야 합니다. 예: *filter=market eq 'US' and gender eq 'm'*.  <br/> 응답 본문의 다음과 같은 필드를 지정할 수 있습니다. <ul><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | 아니요 |
| aggregationLevel | string | 집계 데이터를 검색할 시간 범위를 지정합니다. **day**, **week** 또는 **month** 문자열 중 하나일 수 있습니다. 지정하지 않을 경우 기본값은 **day**입니다.  | 아니요 |
| orderby | string | 각 구입에 대한 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 *orderby 필드 [순서] [순서] =...* *field* 매개 변수는 다음 문자열 중 하나일 수 있습니다. <ul><li>**date**</li><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> *order* 매개 변수는 옵션이며 **asc** 또는 **desc**로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 **asc**입니다. 다음은 *orderby* 문자열 예입니다. *orderby=date,market*  | 아니요 |
| groupby | string | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 다음 필드를 지정할 수 있습니다. <ul><li>**date**</li><li>**applicationName**</li><li>**acquisitionType**</li><li>**ageGroup**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> 반환되는 데이터 행은 *groupby* 매개 변수에서 지정된 필드 및 다음을 포함합니다. <ul><li>**date**</li><li>**applicationId**</li><li>**acquisitionQuantity**</li></ul> 합니다 *groupby* aggregationLevel 매개 변수를 사용 하 여 매개 변수를 사용할 수 있습니다. 예를 들어: *& groupby = 연령대, 시장 및 aggregationLevel = 주*  | 아니요 |

### <a name="request-example"></a>요청 예제
다음 예제에서는 Xbox One 게임 취득 데이터를 가져오는 데 필요한 몇 가지 요청을 보여 줍니다. 대체는 *applicationId* 게임에 대 한 제품 ID 가진 값입니다.  

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&top=10&skip=0 HTTP/1.1 
Authorization: Bearer <your access token> 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&skip=0&filter=market eq 'US' and gender eq 'm' HTTP/1.1 
Authorization: Bearer <your access token> 
```

## <a name="response"></a>응답

### <a name="response-body"></a>응답 본문
| 값 | 형식 | 설명 |
| --- | --- | --- |
| 값 | 배열 | 게임의 집계 취득 데이터가 포함된 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 아래 [구입 값](#acquisition-values) 섹션을 참조하세요. |
| @nextLink | string | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10000으로 설정되어 있지만 쿼리에 대한 구입 데이터의 행이 10000개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | integer | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다. |

### <a name="acquisition-values"></a>구입 값 
*값* 배열의 요소에는 다음 값이 포함됩니다. 

| 값 | 형식 | 설명 |
| --- | --- | --- |
| date | string | 구입 데이터의 날짜 범위에 대한 시작 날짜입니다. 요청에서 하루를 지정한 경우 이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정한 경우 이 값은 해당 날짜 범위의 시작 날짜입니다. |
| applicationId | string | 취득 데이터를 검색할 Xbox One 게임의 제품 ID입니다. |
| applicationName | string | 게임의 표시 이름입니다. |
| acquisitionType | string | 취득 유형을 나타내는 다음 문자열 중 하나입니다.  <ul><li>**무료**</li><li>**평가판**</li><li>**유료**</li><li>**프로 모션 코드**</li><li>**Iap**</li><li>**구독 Iap**</li><li>**개인 사용자**</li><li>**사전 순서**</li><li>**Xbox Game Pass**(또는 2018년 3월 23일 이전에 데이터를 쿼리한 경우에는 **Game Pass**임)</li><li>**디스크**</li><li>**선불된 코드**</li><li>**청구 전 순서**</li><li>**취소 된 사전 순서**</li><li>**실패 한 이전 순서**</li></ul> |
| age | string | 취득한 사용자 연령 그룹을 나타내는 다음 문자열 중 하나입니다. <ul><li>**보다 작거나 13**</li><li>**13-17**</li><li>**18-24**</li><li>**25-34**</li><li>**35-44**</li><li>**44-55**</li><li>**55 보다 큼**</li><li>**알 수 없음**</li></ul> |
| deviceType | string | 취득을 완료한 장치 유형을 나타내는 다음 문자열 중 하나입니다. <ul><li>**PC**</li><li>**Phone**</li><li>**콘솔**</li><li>**IoT**</li><li>**Server**</li><li>**Tablet**</li><li>**홀로그램**</li><li>**알 수 없음**</li></ul> |
| gender | string | 취득한 사용자의 성별을 나타내는 다음 문자열 중 하나입니다. <ul><li>**m**</li><li>**f**</li><li>**알 수 없음**</li></ul> |
| 출시 | string | 구입이 발생한 시장의 ISO 3166 국가 코드입니다. |
| osVersion | string | 구입이 발생한 OS 버전입니다. 이 메서드에서 이 값은 항상 **Windows 10**입니다. |
| paymentInstrumentType | string | 취득에 사용된 결제 방법을 나타내는 다음 문자열 중 하나입니다. <ul><li>**신용 카드**</li><li>**직접 직불 카드**</li><li>**유추 된 구매**</li><li>**MS 잔액**</li><li>**모바일 운영자**</li><li>**온라인 계좌 이체**</li><li>**PayPal**</li><li>**트랜잭션 분합니다**</li><li>**토큰 상환**</li><li>**지불 금액 0**</li><li>**eWallet**</li><li>**알 수 없음**</li></ul> |
| sandboxId | string | 게임에 대해 생성된 샌드박스 ID입니다. 이 값을 수 있습니다 **소매** 또는 개인 샌드박스 ID |
| storeClient | string | 취득이 발생한 Microsoft Store의 버전을 나타내는 다음 문자열 중 하나입니다. <ul><li>**Windows Phone 스토어 (클라이언트)**</li><li>**Microsoft Store(클라이언트)**(또는 2018년 3월 23일 이전에 데이터를 쿼리한 경우에는 **Windows Store(클라이언트)** 임) </li><li>**Microsoft Store(웹)**(또는 2018년 3월 23일 이전에 데이터를 쿼리한 경우에는 **Windows Store(웹)** 임) </li><li>**조직에서 볼륨 구매**</li><li>**기타**</li></ul> |
| xboxTitleId | string | Xbox Live 지원 게임의 Xbox 개발자 포털(XDP)에서 할당한 Xbox Live 타이틀 ID(16진 값으로 표시)입니다. |
| acquisitionQuantity | number | 지정된 집계 수준 중에 발생한 구입 횟수입니다. |
| purchasePriceUSDAmount | number | 취득을 위해 고객이 지불한 금액입니다. 이 금액은 월별 환율을 사용하여 USD로 변환됩니다. |
| purchaseTaxUSDAmount | number | 취득에 적용된 세금이며 USD로 변환됩니다. |
| localCurrencyCode | string | 파트너 센터 계정 국가에 따라 로컬 통화 코드입니다.  |
| xboxProductId | string | Xbox XDP에 해당 하는 경우에서 제품의 제품 ID입니다.  |
| availabilityId | string | XDP에 해당 하는 경우에서 제품의 가용성 ID입니다.  |
| skuId | string | XDP에 해당 하는 경우에서 제품의 SKU ID입니다.  |
| skuDisplayName  | string | SKU는 해당 하는 경우 XDP에서 제품의 이름을 표시 합니다.  |
| xboxParentProductId | string | 부모 제품 ID를 Xbox XDP에 해당 하는 경우에서 제품입니다.  |
| parentProductName | string | 부모 XDP에 해당 하는 경우에서 제품의 제품 이름입니다.  |
| productTypeName | string | XDP에 해당 하는 경우에서 제품의 제품 유형 이름입니다.  |
| purchaseTaxType | string | XDP에 해당 하는 경우에서 제품의 구매 세금 형식입니다.  |
| purchasePriceLocalAmount | number | 해당 하는 경우 XDP에서 제품의 가격 로컬 금액을 구입 합니다.  |
| purchaseTaxLocalAmount | number | 구매 세금 로컬 양 XDP에 해당 하는 경우에서 제품입니다.  |

### <a name="response-example"></a>응답 예제
다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다. 

```JSON
{ 
    "Value": [ 
        { 
            "date": "2019-01-15T01:00:00.0000000Z", 
            "applicationId": "9WZDNCRFHXHT", 
            "applicationName": null, 
            "acquisitionType": "Paid", 
            "age": null, 
            "deviceType": "Phone", 
            "gender": null, 
            "market": "US", 
            "osVersion": "Windows 10", 
            "paymentInstrumentType": null, 
            "sandboxId": "RETAIL", 
            "storeClient": "Microsoft Store (client)", 
            "xboxTitleId": null, 
            "localCurrencyCode": "USD", 
            "xboxProductId": null, 
            "availabilityId": "B42LRTSZ2MCJ", 
            "skuId": "0010", 
            "skuDisplayName": null, 
            "xboxParentProductId": null, 
            "parentProductName": null, 
            "productTypeName": "Game", 
            "purchaseTaxType": "TaxesNotIncluded", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 3.08, 
            "purchasePriceLocalAmount": 3.08, 
            "purchaseTaxUSDAmount": 0.09, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": "acquisitions?applicationId=9WZDNCRFHXHT&aggregationLevel=day&startDate=2017-01-01T08:00:00.0000000Z&endDate=2019-01-16T08:44:15.6045249Z&top=1&skip=1", 
    
    "TotalCount": 12221 
} 
```

 
