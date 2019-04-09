---
description: Microsoft Store analytics API에서에서이 메서드를 사용 하 여 UWP 앱 및 Xbox 개발자 포털 (XDP)를 통해 수집 된 및 XDP 분석 파트너 센터 대시보드에서 사용할 수 있는 게임을 Xbox One에 대 한 JSON 형식으로 집계 추가 기능 취득 데이터를 가져오려고 합니다.
title: 게임 및 응용 프로그램에 대 한 추가 기능 구매 데이터를 가져오기
ms.date: 03/06/2019
ms.topic: article
keywords: windows 10, uwp, 광고 네트워크, 앱 메타데이터
ms.localizationpriority: medium
ms.openlocfilehash: 518648d52c613a3dd5f1bca0d34a7f533b59733f
ms.sourcegitcommit: df8e4143e81a1c5fe1aa5f14407b8dd5f155a12e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829911"
---
# <a name="get-add-on-acquisitions-data-for-your-games-and-apps"></a>게임 및 응용 프로그램에 대 한 추가 기능 구매 데이터를 가져오기 
Microsoft Store analytics API에서에서이 메서드를 사용 하 여 UWP 앱 및 Xbox 개발자 포털 (XDP)를 통해 수집 된 및 XDP 분석 파트너 센터 대시보드에서 사용할 수 있는 게임을 Xbox One에 대 한 JSON 형식으로 집계 추가 기능 취득 데이터를 가져오려고 합니다. 

## <a name="prerequisites"></a>사전 요구 사항
이 메서드를 사용하려면 다음을 먼저 수행해야 합니다. 

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md)을 완료합니다. 
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다. 

> [!NOTE]
> 이 API는 2016 년 10 월 1 일 전에 매일 집계 데이터를 제공 하지 않습니다. 

## <a name="request"></a>요청

### <a name="request-syntax"></a>요청 구문
| 메서드 | 요청 URI |
| --- | --- | 
| 가져오기 | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions` |

### <a name="request-header"></a>요청 헤더 
| 헤더 | 형식 | 설명 | 
| --- | --- | --- |
| Authorization | string | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** `<token>`합니다. |

### <a name="request-parameters"></a>요청 매개 변수
합니다 *applicationId* 하거나 *addonProductId* 매개 변수는 필수입니다. 앱에 등록된 모든 추가 기능의 구입 데이터를 검색하려면 *applicationId* 매개 변수를 지정합니다. 한 추가 기능에 대 한 취득 데이터를 검색 하려면 지정 된 *addonProductId* 매개 변수입니다. 둘 다 지정하는 경우 *applicationId* 매개 변수는 무시됩니다. 

| 매개 변수 | 형식 | 설명 | 필수 | 
| --- | --- | --- | --- |
| applicationId | string | 합니다 *productId* Xbox One 게임 취득 데이터를 검색 합니다. 가져오려는 합니다 *productId* 게임, 게임 XDP 분석 프로그램으로 이동 하 고 검색 합니다 *productId* URL에서 합니다. 또는 파트너 센터 분석 보고서에서 구매 데이터를 다운로드 하는 경우는 *productId* .tsv 파일에 포함 됩니다. | 예 |
| addonProductId | string | 합니다 *productId* 취득 데이터를 검색 하려는 추가 기능입니다. | 예 |
| startDate | date | 검색할 추가 기능 구입 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜입니다. | 아니요 |
| endDate | date | 검색할 추가 기능 구입 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다. | 아니요 |
| top | ssNoversion | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. | 아니요 |
| skip | ssNoversion | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색할 수 있습니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다. | 아니요 |
| filter | string | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에 포함 된 응답 본문 및 eq 또는 ne 연산자와 연관 된 값에서 필드 이름 및 문을 사용 하 여 결합할 수 있고 또는 합니다. 문자열 값 필터 매개 변수에 작은따옴표로 묶어야 합니다. 예를 들어, 필터 = 시장 eq 'u S' 및 성별 eq 고 '. <br/> 응답 본문의 다음과 같은 필드를 지정할 수 있습니다. <ul><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | 아니요 |
| aggregationLevel | string | 집계 데이터를 검색할 시간 범위를 지정합니다. **day**, **week** 또는 **month** 문자열 중 하나일 수 있습니다. 지정하지 않을 경우 기본값은 **day**입니다. | 아니요 |
| orderby | string | 각 추가 기능 구입에 대한 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 *orderby 필드 [순서] [순서] =...* *field* 매개 변수는 다음 문자열 중 하나일 수 있습니다. <ul><li>**date**</li><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**orderName**</li></ul> Order 매개 변수가 선택 사항이 며 수 **asc** 또는 **desc** 오름차순 또는 내림차순 키를 눌러 각 필드에 대해 지정할 수 있습니다. 기본값은 **asc**입니다. <br/> 다음은 *orderby* 문자열 예입니다. *orderby=date,market* | 아니요 |
| groupby | string | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 다음 필드를 지정할 수 있습니다. <ul><li>**date**</li><li>**applicationName**</li><li>**addonProductName**</li> <li>**acquisitionType**</li><li>**age**</li> <li>**storeClient**</li><li>**gender**</li> <li>**market**</li> <li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> 반환되는 데이터 행은 *groupby* 매개 변수에서 지정된 필드 및 다음을 포함합니다. <ul><li>**date**</li><li>**applicationId**</li><li>**addonProductId**</li><li>**acquisitionQuantity**</li></ul> Groupby 매개 변수를 사용 하 여 사용할 수는 *aggregationLevel* 매개 변수입니다. 예를 들어: *& groupby = 보존 기간, 시장 및 aggregationLevel = 주* | 아니요 |

### <a name="request-example"></a>요청 예제
다음 예제에서는 추가 기능 구입 데이터를 가져오는 데 필요한 몇 가지 요청을 보여 줍니다. 대체는 *addonProductId* 하 고 *applicationId* 추가 기능 또는 앱에 적절 한 Store ID 사용 하 여 값입니다. 

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0 HTTP/1.1 

Authorization: Bearer <your access token> 

 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0&filter=market eq 'GB' and gender eq 'm' HTTP/1.1 

Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

### <a name="response-body"></a>응답 본문

| 값 | 형식 | 설명 |
| --- | --- | --- |
| 값 | 배열 | 집계 추가 기능 구입 데이터가 포함된 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 아래 [추가 기능 구입 값](#add-on-acquisition-values) 섹션을 참조하세요. |
| @nextLink | string | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10000으로 설정되어 있지만 쿼리에 대한 추가 기능 구입 데이터의 행이 10000개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | ssNoversion | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다. |

### <a name="add-on-acquisition-values"></a>추가 기능 구입 값
값 배열의 요소는 다음 값을 포함 합니다.

| 값 | 형식 | 설명 | 
| --- | --- | --- |
| date | string | 구입 데이터의 날짜 범위에 대한 시작 날짜입니다. 요청에서 하루를 지정한 경우 이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정한 경우 이 값은 해당 날짜 범위의 시작 날짜입니다. |
| addonProductId | string | 합니다 *productId* 를 취득 데이터를 검색 하는 추가 기능입니다. |
| addonProductName | string | 추가 기능의 표시 이름입니다. 이 값 응답 데이터의 경우만 표시 합니다 *aggregationLevel* 매개 변수는 설정 **일**지정 하지 않는 한를 **addonProductName** 필드에 *groupby* 매개 변수입니다. |
| applicationId | string | 합니다 *productId* 추가 기능 취득 데이터를 검색 하려는 앱의 합니다. |
| applicationName | string | 게임의 표시 이름입니다. |
| deviceType | string | 취득을 완료한 장치 유형을 나타내는 다음 문자열 중 하나입니다. <ul><li>"PC"</li><li>"Phone"</li><li>"콘솔"</li><li>"IoT"</li><li>"Server"</li><li>"태블릿"</li><li>"Holographic"</li><li>"알 수 없음"</li></ul> |
| storeClient | string | 취득이 발생한 Microsoft Store의 버전을 나타내는 다음 문자열 중 하나입니다. <ul><li>"Windows Phone 스토어 (클라이언트)"</li><li>"Microsoft Store (클라이언트)" (또는 "Windows 스토어 (클라이언트)" 2018 년 3 월 23 일 하기 전에 데이터를 쿼리 하는 경우)</li><li>"Microsoft Store (웹)" (또는 "Windows 스토어 (웹)" 2018 년 3 월 23 일 하기 전에 데이터를 쿼리 하는 경우)</li><li>"조직에서 볼륨 구매"</li><li>"기타"</li></ul> |
| osVersion | string | 구입이 발생한 OS 버전입니다. 이 방법에서는이 값은 항상 "Windows 10"입니다. |
| 출시 | string | 구입이 발생한 시장의 ISO 3166 국가 코드입니다. |
| gender | string | 취득한 사용자의 성별을 나타내는 다음 문자열 중 하나입니다. <ul><li>"m"</li><li>"f"</li><li>"알 수 없음"</li></ul> |
| age | string | 취득한 사용자 연령 그룹을 나타내는 다음 문자열 중 하나입니다. <ul><li>"보다 작거나 13"</li><li>"13-17"</li><li>"18-24"</li><li>"25-34"</li><li>"35-44"</li><li>"44-55"</li><li>"55 보다 큼"</li><li>"알 수 없음"</li></ul> |
| acquisitionType | string | 취득 유형을 나타내는 다음 문자열 중 하나입니다. <ul><li>"무료" </li><li>"평가판" </li><li>"유료"</li><li>"프로 모션 코드" </li><li>"Iap"</li><li>"구독 Iap"</li><li>"개인 Audience"</li><li>"전 Order"</li><li>"Xbox Game Pass" (또는 "Game Pass" 2018 년 3 월 23 일 하기 전에 데이터를 쿼리 하는 경우)</li><li>"Disk"</li><li>"선불된 코드"</li><li>"사전 순서 청구 됩니다."</li><li>"이전 주문을 취소 되었습니다."</li><li>"사전 순서 실패 했습니다."</li></ul> |
| acquisitionQuantity | integer | 발생한 구입 횟수입니다. |
| inAppProductId | string | 이 추가 기능을 사용 하는 제품의 제품 ID입니다.  |
| inAppProductName | string | 이 추가 기능을 사용 하는 제품의 제품 이름입니다.  |
| paymentInstrumentType | string | 결제 방법 유형은 획득에 사용 합니다.  |
| sandboxId | string | 게임에 대해 만든 샌드박스 ID입니다. 이 값을 수 있습니다 **소매** 또는 개인 샌드박스 ID  |
| xboxTitleId | string | Xbox XDP에 해당 하는 경우에서 제품의 ID를 제목입니다.  |
| localCurrencyCode | string | 파트너 센터 계정 국가에 따라 로컬 통화 코드입니다.  |
| xboxProductId | string | Xbox XDP에 해당 하는 경우에서 제품의 제품 ID입니다.  |
| availabilityId | string | XDP에 해당 하는 경우에서 제품의 가용성 ID입니다.  |
| skuId | string | XDP에 해당 하는 경우에서 제품의 SKU ID입니다.  |
| skuDisplayName | string | XDP에 해당 하는 경우에서 제품의 SKU 표시 이름입니다.  |
| xboxParentProductId | string | 부모 제품 ID를 Xbox XDP에 해당 하는 경우에서 제품입니다.  |
| parentProductName | string | 부모 XDP에 해당 하는 경우에서 제품의 제품 이름입니다.  |
| productTypeName | string | XDP에 해당 하는 경우에서 제품의 제품 유형 이름입니다.  |
| purchaseTaxType | string | XDP에 해당 하는 경우에서 제품의 구매 세금 형식입니다.  |
| purchasePriceUSDAmount | number | 추가 기능을 위해 고객이 유료 크기 USD를 변환 합니다.  |
| purchasePriceLocalAmount | number | 추가 기능을 적용할 세 액입니다.  |
| purchaseTaxUSDAmount | number | 추가 기능을 적용할 세 액 USD를 변환 합니다.  |
| purchaseTaxLocalAmount | number | 구매 세금 로컬 양 XDP에 해당 하는 경우에서 제품입니다.  |

### <a name="response-example"></a>응답 예제
다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다. 

```JSON
{ 
  "Value": [ 
    { 
            "inAppProductId": "9NBLGGH1864K", 
            "inAppProductName": "866879", 
            "addonProductId": "9NBLGGH1864K", 
            "addonProductName": "866879", 
            "date": "2017-11-05", 
            "applicationId": "9WZDNCRFJ314", 
            "applicationName": "Tetris Blitz", 
            "acquisitionType": "Iap", 
            "age": "35-49", 
            "deviceType": "Phone", 
            "gender": "m", 
            "market": "US", 
            "osVersion": "Windows Phone 8.1", 
            "paymentInstrumentType": "Credit Card", 
            "sandboxId": "RETAIL", 
            "storeClient": "Windows Phone Store (client)", 
            "xboxTitleId": "", 
            "localCurrencyCode": "USD", 
            "xboxProductId": "00000000-0000-0000-0000-000000000000", 
            "availabilityId": "", 
            "skuId": "", 
            "skuDisplayName": "Full", 
            "xboxParentProductId": "", 
            "parentProductName": "Tetris Blitz", 
            "productTypeName": "Add-On", 
            "purchaseTaxType": "", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 1.08, 
            "purchasePriceLocalAmount": 0.09, 
            "purchaseTaxUSDAmount": 1.08, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": null, 
    
    "TotalCount": 7601 
} 
```