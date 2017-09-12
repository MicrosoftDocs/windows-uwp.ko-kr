---
author: mcleanbyron
description: "Windows 스토어 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택 필터 동안 응용 프로그램의 취득 깔때기형 데이터를 가져옵니다."
title: "앱 취득 깔때기 데이터 가져오기"
ms.author: mcleans
ms.date: 08/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 저장소 서비스, Windows 스토어 분석 API, 취득 깔때기"
ms.openlocfilehash: 5e09504eb7ed787fcf330a5cf92027eed5f6bc59
ms.sourcegitcommit: 2b436dc5e5681b8884e0531ee303f851a3e3ccf2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/18/2017
---
# <a name="get-app-acquisition-funnel-data"></a>앱 취득 깔때기 데이터 가져오기

Windows 스토어 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택 필터 동안 응용 프로그램의 취득 깔때기형 데이터를 가져옵니다. 이 정보는 Windows 개발자 센터 대시보드의 [구입 보고서](../publish/acquisitions-report.md#acquisition-funnel)를 통해서도 사용할 수 있습니다.

## <a name="prerequisites"></a>필수 조건


이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Windows 스토어 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel``` |

<span/>

### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |

<span/> 

### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 유형   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 취득 깔때기 데이터 검색을 원하는 앱의 [스토어 ID](in-app-purchases-and-trials.md#store-ids)입니다. 스토어 ID의 예로는 9WZDNCRFJ3Q8이 있습니다. |  예  |
| startDate | date | 검색할 취득 깔때기 데이터의 날짜 범위에서 시작 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| endDate | date | 검색할 취득 깔때기 데이터의 날짜 범위에서 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| filter | 문자열  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 자세한 내용은 아래의 [필터 필드](#filter-fields) 섹션을 참조하세요. | 아니요   |

<span/>
 
### <a name="filter-fields"></a>필드 필터링

요청의 *filter* 매개 변수에는 응답에서 행을 필터링하는 하나 이상의 문이 포함되어 있습니다. 각 문에는 **eq** 또는 **ne** 연산자와 연결된 필드 및 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다.

다음 필드를 지원합니다. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다.

| 필드        |  Description        |
|---------------|-----------------|
| campaignId | 취득과 연결된 [사용자 지정 앱 프로 모션 캠페인](../publish/create-a-custom-app-promotion-campaign.md)의 ID 문자열입니다. |
| market | 구입이 발생한 시장의 ISO 3166 국가 코드를 포함하는 문자열입니다. |
| deviceType | 취득이 발생한 장치 유형을 나타내는 다음 문자열 중 하나입니다.<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| ageGroup | 취득을 완료한 사용자 연령 그룹을 나타내는 다음 문자열 중 하나입니다.<ul><li><strong>0 – 17</strong></li><li><strong>18 – 24</strong></li><li><strong>25 – 34</strong></li><li><strong>35 – 49</strong></li><li><strong>50 이상</strong></li><li><strong>모름</strong></li></ul> |
| 성별 | 취득을 완료한 사용자의 성별을 나타내는 다음 문자열 중 하나입니다.<ul><li><strong>남자</strong></li><li><strong>여자</strong></li><li><strong>모름</strong></li></ul> |


다음은 *filter* 매개 변수의 몇 가지 예입니다.

```filter=market eq 'US' and gender eq 'M'```

```filter=(market ne 'US') and (gender ne 'Unknown') and (gender ne 'M') and (market ne 'NO') and (ageGroup ne '50 or over' or ageGroup ne ‘0 - 17’)```

<span/> 

### <a name="request-example"></a>요청 예제

다음 예제에서는 앱에 대한 취득 깔때기 데이터를 가져오는 데 필요한 몇 가지 요청을 보여 줍니다. *applicationId* 값을 앱의 스토어 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=8/1/2016&endDate=8/31/2016&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 유형   | 설명                  |
|------------|--------|-------------------------------------------------------|
| 값      | array  | 앱의 취득 깔때기 데이터가 포함된 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 아래 [깔때기 값](#funnel-values) 섹션을 참조하세요.                                                                                                                      |
| TotalCount | int    | *값* 배열의 개체 수 총계입니다.                         |
<span/>
 
### <a name="funnel-values"></a>깔때기 값

*값* 배열의 개체에는 다음 값이 포함됩니다.

| 값               | 유형   | Description                           |
|---------------------|--------|-------------------------------------------|
| MetricType                | string | 이 개체에 포함된 [깔때기 데이터 유형](../publish/acquisitions-report.md#acquisition-funnel)을 지정한 다음 문자열 중 하나입니다.<ul><li><strong>PageView</strong></li><li><strong>Acquisition</strong></li><li><strong>Install</strong></li><li><strong>Usage</strong></li></ul> |
| UserCount       | string | *MetricType* 값이 지정한 깔때기 단계를 수행할 수 있는 사용자의 수입니다.             |

<span/> 

### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "MetricType": "PageView",
      "UserCount": 100
    },
    {
      "MetricType": "Acquisition",
      "UserCount": 80
    },
    {
      "MetricType": "Install",
      "UserCount": 50
    },
    {
      "MetricType": "Usage",
      "UserCount": 10
    }
  ],
  "TotalCount": 4
}
```

## <a name="related-topics"></a>관련 항목

* [구입 보고서](../publish/acquisitions-report.md)
* [Windows 스토어 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [앱 취득 가져오기](get-app-acquisitions.md)
