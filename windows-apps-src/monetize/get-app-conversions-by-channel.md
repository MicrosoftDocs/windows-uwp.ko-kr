---
author: mcleanbyron
description: "Windows 스토어 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택 필터 동안 응용 프로그램 채널 데이터별 집계된 변환을 가져옵니다."
title: "채널별 앱 변환 가져오기"
ms.author: mcleans
ms.date: 08/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 저장소 서비스, Windows 스토어 분석 API, 앱 변환, 채널"
ms.openlocfilehash: f5ffce7c4bb1f8f08a6e33f212ba765eb0e70da7
ms.sourcegitcommit: 2b436dc5e5681b8884e0531ee303f851a3e3ccf2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/18/2017
---
# <a name="get-app-conversions-by-channel"></a>채널별 앱 변환 가져오기

Windows 스토어 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택 필터 동안 응용 프로그램 채널별 집계 변환을 가져옵니다.

* *변환*은 (Microsoft 계정으로 로그인한) 고객이 앱(금액 청구 또는 무료 제공 여부와는 관계 없음)에 대한 라이선스를 새로 획득했음을 의미합니다.
* *채널*은 고객이 앱의 목록 페이지(예, 상점이나 [사용자 지정 앱 프로 모션 캠페인](../publish/create-a-custom-app-promotion-campaign.md)을 통해)에 도착한 방법입니다.

이 정보는 Windows 개발자 센터 대시보드의 [구입 보고서](../publish/acquisitions-report.md#app-page-views-and-conversions-by-channel)를 통해서도 사용할 수 있습니다.

## <a name="prerequisites"></a>필수 조건


이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Windows 스토어 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions``` |

<span/>

### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |

<span/> 

### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 유형   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | string | 변환 데이터를 검색할 앱의 [스토어 ID](in-app-purchases-and-trials.md#store-ids)입니다. 스토어 ID의 예로는 9WZDNCRFJ3Q8이 있습니다. |  예  |
| startDate | date | 검색할 변환 데이터의 날짜 범위의 시작 날짜입니다. 기본값은 1/1/2016입니다. |  아니요  |
| endDate | date | 검색할 구입 데이터의 날짜 범위에서 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다. |  아니요  |
| filter | string  | 응답 본문을 필터링하는 하나 이상의 문장입니다. 각 문은 **eq** 또는 **ne** 연산자를 사용할 수 있으며, 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 필터 문에서 다음 문자열을 지정할 수 있습니다. 설명은 이 문서의 [변환 값](#conversion-values)을 참조하세요. <ul><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p>다음은 *필터* 매개변수 <em>filter=deviceType eq 'PC'</em>의 예입니다.</p> | 아니요   |
| aggregationLevel | 문자열 | 집계 데이터를 검색할 시간 범위를 지정합니다. <strong>day</strong>, <strong>week</strong> 또는 <strong>month</strong> 문자열 중 하나일 수 있습니다. 지정하지 않을 경우 기본값은 <strong>day</strong>입니다. | 아니요 |
| orderby | string | 각 변환에 대한 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 <em>orderby=field [order],field [order],...</em>입니다. <em>field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p><em>order</em> 매개 변수는 옵션이며 <strong>asc</strong> 또는 <strong>desc</strong>로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열 예입니다. <em>orderby=date,market</em></p> |  아니요  |
| groupby | 문자열 | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 다음 필드를 지정할 수 있습니다.<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p>반환되는 데이터 행은 <em>groupby</em> 매개 변수에서 지정된 필드 및 다음을 포함합니다.</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>conversionCount</strong></li><li><strong>clickCount</strong></li></ul><p><em>groupby</em> 매개 변수는 <em>aggregationLevel</em> 매개 변수와 함께 사용할 수 있습니다. 예: <em>groupby=ageGroup,marke</em>&amp;aggregationLevel=week</p> |  아니요  |

<span/>

### <a name="request-example"></a>요청 예제

다음 예제에서는 앱 변환 데이터를 가져오는 데 필요한 몇 가지 요청을 보여 줍니다. *applicationId* 값을 앱의 스토어 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=4/31/2017&skip=0&filter=market eq 'US'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 유형   | 설명                  |
|------------|--------|-------------------------------------------------------|
| 값      | array  | 앱의 집계 변환 데이터가 포함된 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 아래 [변환 값](#conversion-values) 섹션을 참조하세요.                                                                                                                      |
| @nextLink  | string | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10000으로 설정되어 있지만 쿼리에 대한 변환 데이터의 행이 10,000개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.                                                                                                                                                                                                                             |

<span/>
 
### <a name="conversion-values"></a>변환 값

*값* 배열의 개체에는 다음 값이 포함됩니다.

| 값               | 유형   | 설명                           |
|---------------------|--------|-------------------------------------------|
| date                | string | 변환 데이터의 날짜 범위에서 시작 날짜입니다. 요청에서 하루를 지정한 경우 이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정한 경우 이 값은 해당 날짜 범위의 시작 날짜입니다. |
| applicationId       | string | 변환 데이터를 검색할 앱의 [스토어 ID](in-app-purchases-and-trials.md#store-ids)입니다.     |
| applicationName     | string | 변환 데이터를 검색한 앱의 표시 이름입니다.        |
| appType          | string |  변환 데이터를 검색할 앱의 제품 유형입니다. 이 메서드는 **App**값만 지원합니다.            |
| customCampaignId           | string |  앱과 연결된 [사용자 지정 앱 프로 모션 캠페인](../publish/create-a-custom-app-promotion-campaign.md)의 ID 문자열입니다.   |
| referrerUriDomain           | string |  사용자 지정 앱 프로모션 캠페인 ID를 가진 앱 목록이 활성화되어 있는 도메인을 지정합니다.   |
| channelType           | string |  변환 채널을 지정하는 문자열은 다음 중 하나입니다.<ul><li><strong>CustomCampaignId</strong></li><li><strong>Store Traffic</strong></li><li><strong>기타</strong></li></ul>    |
| storeClient         | string | 변환이 발생한 스토어의 버전입니다. 현재는 **SFC** 값만 지원합니다.    |
| deviceType          | string | 다음 문자열 중 하나입니다.<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>알 수 없음</strong></li></ul>            |
| market              | string | 변환이 발생한 시장의 ISO 3166 국가 코드입니다.    |
| clickCount              | number  |     앱 목록 링크를 클릭한 고객의 수입니다.      |           
| conversionCount            | number  |   고객 변환 수         |          |


<span/> 

### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "date": "2016-01-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso App",
      "appType": "App",
      "customCampaignId": "",
      "referrerUriDomain": "Universal Client Store",
      "channelType": "Store Traffic",
      "storeClient": "SFC",
      "deviceType": "PC",
      "market": "US",
      "clickCount": 7,
      "conversionCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>관련 항목

* [구입 보고서](../publish/acquisitions-report.md)
* [Windows 스토어 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [앱 취득 가져오기](get-app-acquisitions.md)
