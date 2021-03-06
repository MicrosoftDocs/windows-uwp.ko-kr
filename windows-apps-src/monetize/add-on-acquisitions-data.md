---
description: Microsoft Store analytics API에서이 메서드를 사용 하 여 XDP (Xbox Developer Portal)를 통해 수집 하 고 XDP Analytics 파트너 센터 대시보드에서 사용할 수 있는 UWP 앱 및 Xbox One 게임의 JSON 형식으로 집계 추가 기능 획득 데이터를 가져올 수 있습니다.
title: 게임 및 앱에 대한 추가 기능 취득 데이터 가져오기
ms.date: 03/06/2019
ms.topic: article
keywords: windows 10, uwp, 광고 네트워크, 앱 메타 데이터
ms.localizationpriority: medium
ms.openlocfilehash: 406519bb6a38c3f7c8225d81fbd6fd37611ed1e0
ms.sourcegitcommit: 368753aea2792984857f6a57a22daed1035f1a33
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2020
ms.locfileid: "97349713"
---
# <a name="get-add-on-acquisitions-data-for-your-games-and-apps"></a>게임 및 앱에 대한 추가 기능 취득 데이터 가져오기 
Microsoft Store analytics API에서이 메서드를 사용 하 여 XDP (Xbox Developer Portal)를 통해 수집 하 고 XDP Analytics 파트너 센터 대시보드에서 사용할 수 있는 UWP 앱 및 Xbox One 게임의 JSON 형식으로 집계 추가 기능 획득 데이터를 가져올 수 있습니다. 

## <a name="prerequisites"></a>사전 요구 사항
이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다. 

* 아직 수행 하지 않은 경우 Microsoft Store 분석 API에 대 한 모든 [필수 구성 요소](access-analytics-data-using-windows-store-services.md) 를 완료 합니다. 
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다. 

> [!NOTE]
> 이 API는 2016 년 10 월 1 일 이전에 매일 집계 데이터를 제공 하지 않습니다. 

## <a name="request"></a>요청

### <a name="request-syntax"></a>요청 구문
| 방법 | 요청 URI |
| --- | --- | 
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions` |

### <a name="request-header"></a>요청 헤더 
| 헤더 | 형식 | Description | 
| --- | --- | --- |
| 권한 부여 | 문자열 | 필수 요소. **전달자** 형식의 Azure AD 액세스 토큰 `<token>` 입니다. |

### <a name="request-parameters"></a>요청 매개 변수
*ApplicationId* 또는 *addonProductId* 매개 변수는 필수입니다. 앱에 등록된 모든 추가 기능의 구입 데이터를 검색하려면 *applicationId* 매개 변수를 지정합니다. 단일 추가 기능에 대 한 획득 데이터를 검색 하려면 *addonProductId* 매개 변수를 지정 합니다. 둘 다 지정 하는 경우 *applicationId* 매개 변수는 무시 됩니다. 

| 매개 변수 | 형식 | 설명 | 필수 | 
| --- | --- | --- | --- |
| applicationId | 문자열 | 획득 데이터를 검색 하는 Xbox One 게임의 *productId* 입니다. 게임의 *productid* 를 얻으려면 XDP Analytics 프로그램에서 게임으로 이동 하 여 URL에서 *productid* 를 검색 합니다. 또는 파트너 센터 분석 보고서에서 획득 데이터를 다운로드 하는 경우 *productId* 는 tsv 파일에 포함 됩니다. | 예 |
| addonProductId | 문자열 | 획득 데이터를 검색 하려는 추가 기능 *productId* 입니다. | 예 |
| startDate | date | 검색할 추가 기능 획득 데이터의 날짜 범위에 있는 시작 날짜입니다. 기본값은 현재 날짜입니다. | 아니요 |
| endDate | date | 검색할 추가 기능 획득 데이터의 날짜 범위에 있는 종료 날짜입니다. 기본값은 현재 날짜입니다. | 아니요 |
| filter | 문자열 | 응답의 행을 필터링 하는 하나 이상의 문입니다. 각 문에는 eq 또는 ne 연산자와 연결 된 응답 본문 및 값의 필드 이름이 포함 되며, and 또는 or를 사용 하 여 문을 결합할 수 있습니다. 문자열 값은 필터 매개 변수에서 작은따옴표로 묶어야 합니다. 예: filter = market eq ' US ' 및 성별 eq m '. <br/> 응답 본문에서 다음 필드를 지정할 수 있습니다. <ul><li>**구매 유형**</li><li>**발전할**</li><li>**유형별**</li><li>**gender**</li><li>**시장**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | 아니요 |
| aggregationLevel | 문자열 | 집계 데이터를 검색 하는 시간 범위를 지정 합니다. **일**, **주** 또는 **월** 문자열 중 하나일 수 있습니다. 지정 하지 않으면 기본값은 **day** 입니다. | 아니요 |
| orderby | 문자열 | 각 추가 기능 획득에 대 한 결과 데이터 값을 정렬 하는 문입니다. 구문은 *orderby = field [order], field [order],...* *Field* 매개 변수는 다음 문자열 중 하나일 수 있습니다. <ul><li>**date**</li><li>**구매 유형**</li><li>**발전할**</li><li>**유형별**</li><li>**gender**</li><li>**시장**</li><li>**osVersion**</li><li>**deviceType**</li><li>**orderName**</li></ul> Order 매개 변수는 선택 사항이 며 **asc** 또는 **desc** 로 각 필드에 대해 오름차순 또는 내림차순을 지정할 수 있습니다. 기본값은 **asc** 입니다. <br/> 다음은 *orderby* 문자열의 예입니다. *orderby = date, market* | 아니요 |
| groupby | 문자열 | 지정 된 필드에만 데이터 집계를 적용 하는 문입니다. 다음 필드를 지정할 수 있습니다. <ul><li>**date**</li><li>**applicationName**</li><li>**addonProductName**</li> <li>**구매 유형**</li><li>**발전할**</li> <li>**유형별**</li><li>**gender**</li> <li>**시장**</li> <li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> 반환 된 데이터 행에는 *groupby* 매개 변수 및 다음에 지정 된 필드가 포함 됩니다. <ul><li>**date**</li><li>**applicationId**</li><li>**addonProductId**</li><li>**구매 수량**</li></ul> Groupby 매개 변수는 *aggregationLevel* 매개 변수와 함께 사용할 수 있습니다. 예: *&groupby = age, market&aggregationLevel = week* | 아니요 |

### <a name="request-example"></a>요청 예제
다음 예제에서는 추가 기능 획득 데이터를 가져오는 몇 가지 요청을 보여 줍니다. *AddonProductId* 및 *applicationId* 값을 추가 기능 또는 앱의 적절 한 상점 ID로 바꿉니다. 

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
| 값 | array | 집계 추가 기능 획득 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 아래의 [추가 기능 획득 값](#add-on-acquisition-values) 섹션을 참조 하십시오. |
| TotalCount | int | 쿼리의 데이터 결과에 있는 총 행 수입니다. |

### <a name="add-on-acquisition-values"></a>추가 기능 획득 값
값 배열의 요소에는 다음 값이 포함 됩니다.

| 값 | 형식 | 설명 | 
| --- | --- | --- |
| date | 문자열 | 취득 데이터의 날짜 범위에 있는 첫 번째 날짜입니다. 요청에서 하루를 지정 하는 경우이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정 하는 경우이 값은 해당 날짜 범위의 첫 번째 날짜입니다. |
| addonProductId | 문자열 | 획득 데이터를 검색 하는 추가 기능에 대 한 *productId* 입니다. |
| addonProductName | 문자열 | 추가 기능에 대 한 표시 이름입니다. 이 값은 *groupby* 매개 변수에 **addonProductName** 필드를 지정 하지 않는 한 *aggregationLevel* 매개 변수가 **day** 로 설정 된 경우에만 응답 데이터에 표시 됩니다. |
| applicationId | 문자열 | 추가 기능 획득 데이터를 검색 하려는 앱의 *productId* 입니다. |
| applicationName | 문자열 | 게임의 표시 이름입니다. |
| deviceType | 문자열 | 가져오기를 완료 한 장치 유형을 지정 하는 다음 문자열 중 하나입니다. <ul><li>컴퓨터</li><li>내선</li><li>"콘솔-Xbox One"</li><li>"콘솔-Xbox 시리즈 X"</li><li>IoT</li><li>서버인</li><li>표</li><li>Holographic</li><li>없습니다</li></ul> |
| 유형별 | 문자열 | 획득이 발생 한 저장소의 버전을 나타내는 다음 문자열 중 하나입니다. <ul><li>"Windows Phone 저장소 (클라이언트)"</li><li>"Microsoft Store (클라이언트)" (또는 "Windows 스토어 (클라이언트)", 2018 년 3 월 23 일 전에 데이터를 쿼리 하는 경우)</li><li>"Microsoft Store (웹)" (또는 "Windows 스토어 (웹)", 2018 년 3 월 23 일 전에 데이터를 쿼리 하는 경우)</li><li>"조직의 대량 구매"</li><li>다른</li></ul> |
| osVersion | 문자열 | 획득이 발생 한 OS 버전입니다. 이 메서드의 경우이 값은 항상 "Windows 10"입니다. |
| market | 문자열 | 획득이 발생 한 시장의 ISO 3166 국가 코드입니다. |
| gender | 문자열 | 가져오기를 수행한 사용자의 성별을 지정 하는 다음 문자열 중 하나입니다. <ul><li>"m"</li><li>"f"</li><li>없습니다</li></ul> |
| age | 문자열 | 가져오기를 수행한 사용자의 나이 그룹을 나타내는 다음 문자열 중 하나입니다. <ul><li>"13 보다 작음"</li><li>"13-17"</li><li>"18-24"</li><li>"25-34"</li><li>"35-44"</li><li>"44-55"</li><li>"55 보다 큼"</li><li>없습니다</li></ul> |
| 구매 유형 | 문자열 | 획득 유형을 나타내는 다음 문자열 중 하나입니다. <ul><li>늘릴 </li><li>기간인 </li><li>지급</li><li>"프로 모션 코드" </li><li>Iap</li><li>"Subscription Iap"</li><li>"개인 대상"</li><li>"사전 순서"</li><li>"Xbox 게임 Pass" (또는 3 월 23 일 전에 데이터를 쿼리 2018 하는 경우 "게임 패스")</li><li>디스크로</li><li>"선불 코드"</li><li>"청구 된 사전 주문"</li><li>"취소 된 사전 순서"</li><li>"실패 한 사전 순서"</li></ul> |
| 구매 수량 | integer | 발생 한 인수 수입니다. |
| inAppProductId | 문자열 | 이 추가 기능이 사용 되는 제품의 제품 ID입니다.  |
| inAppProductName | 문자열 | 이 추가 기능이 사용 되는 제품의 제품 이름입니다.  |
| paymentInstrumentType | 문자열 | 획득에 사용 되는 지불 수단 유형입니다.  |
| sandboxId | 문자열 | 게임에 대해 만든 샌드박스 ID입니다. 이 값은 **일반 정품** 또는 개인 샌드박스 ID 일 수 있습니다.  |
| xboxTitleId | 문자열 | 해당 하는 경우 XDP의 제품에 대 한 Xbox 타이틀 ID입니다.  |
| localCurrencyCode | 문자열 | 파트너 센터 계정의 국가를 기준으로 하는 현지 통화 코드입니다.  |
| xboxProductId | 문자열 | 해당 하는 경우 XDP의 제품에 대 한 Xbox 제품 ID입니다.  |
| availabilityId | 문자열 | 해당 하는 경우 XDP의 제품에 대 한 가용성 ID입니다.  |
| skuId | 문자열 | 해당 하는 경우 XDP의 제품 SKU ID입니다.  |
| 대만 | 문자열 | 해당 하는 경우 XDP의 제품에 대 한 SKU 표시 이름입니다.  |
| xboxParentProductId | 문자열 | 해당 하는 경우 XDP의 제품에 대 한 Xbox 부모 제품 ID입니다.  |
| parentProductName | 문자열 | XDP에서 제품의 부모 제품 이름입니다 (해당 하는 경우).  |
| 제품 형식 이름 | 문자열 | XDP 제품의 제품 유형 이름입니다 (해당 하는 경우).  |
| purchaseTaxType | 문자열 | 해당 하는 경우 XDP에서 제품의 세금 유형을 구매 합니다.  |
| purchasePriceUSDAmount | 숫자 | 추가 기능에 대 한 고객의 지불 금액으로, USD로 변환 됩니다.  |
| purchasePriceLocalAmount | 숫자 | 추가 기능에 적용 되는 세금입니다.  |
| purchaseTaxUSDAmount | 숫자 | USD로 변환 된 추가 기능에 적용 되는 세금입니다.  |
| purchaseTaxLocalAmount | 숫자 | 해당 하는 경우 XDP에서 제품의 세금 로컬 금액을 구매 합니다.  |

### <a name="response-example"></a>응답 예제
다음 예제에서는이 요청에 대 한 예제 JSON 응답 본문을 보여 줍니다. 

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