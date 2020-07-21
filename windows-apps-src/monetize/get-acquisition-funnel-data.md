---
description: Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 날짜 범위 및 기타 선택적 필터 중에 응용 프로그램에 대 한 획득 깔때기 데이터를 가져옵니다.
title: 앱 취득 깔때기 데이터 가져오기
ms.date: 08/04/2017
ms.topic: article
keywords: windows 10, uwp, 스토어 서비스, Microsoft Store 분석 API, 취득, 깔때기형
ms.localizationpriority: medium
ms.openlocfilehash: 817ad85bae642d6b09cf77bb5e7b4bf71b08f594
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493558"
---
# <a name="get-app-acquisition-funnel-data"></a>앱 취득 깔때기 데이터 가져오기

Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 날짜 범위 및 기타 선택적 필터 중에 응용 프로그램에 대 한 획득 깔때기 데이터를 가져옵니다. 이 정보는 파트너 센터의 [합병 보고서](../publish/acquisitions-report.md#acquisition-funnel) 에서도 사용할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소


이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 분석 API에 대 한 모든 [필수 구성 요소](access-analytics-data-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 사항입니다. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 형식   |  설명      |  필요한 공간  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 획득 깔때기 데이터를 검색 하려는 앱의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다. 예 매장 ID는 9WZDNCRFJ3Q8입니다. |  예  |
| startDate | date | 검색할 획득 깔때기 데이터의 날짜 범위에 있는 시작 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| endDate | date | 검색할 획득 깔때기 데이터의 날짜 범위에 있는 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| filter | 문자열  | 응답의 행을 필터링 하는 하나 이상의 문입니다. 자세한 내용은 아래의 [필터 필드](#filter-fields) 섹션을 참조 하십시오. | 아니요   |

 
### <a name="filter-fields"></a>필터 필드

요청의 *filter* 매개 변수에는 응답의 행을 필터링 하는 문이 하나 이상 포함 되어 있습니다. 각 문에는 **eq** 또는 **ne** 연산자와 연결 된 필드와 값이 포함 되며, **and** 또는 **or**를 사용 하 여 문을 결합할 수 있습니다.

지원 되는 필터 필드는 다음과 같습니다. 문자열 값은 *필터* 매개 변수에서 작은따옴표로 묶어야 합니다.

| 필드        |  설명        |
|---------------|-----------------|
| campaignId | 획득에 연결 된 [사용자 지정 앱 판촉 캠페인](../publish/create-a-custom-app-promotion-campaign.md) 의 ID 문자열입니다. |
| market | 획득이 발생 한 시장의 ISO 3166 국가 코드를 포함 하는 문자열입니다. |
| deviceType | 획득이 발생 한 장치 유형을 지정 하는 다음 문자열 중 하나입니다.<ul><li><strong>컴퓨터</strong></li><li><strong>내선</strong></li><li><strong>콘솔-Xbox One</strong></li><li><strong>콘솔-Xbox 시리즈 X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>알 수 없음</strong></li></ul> |
| ageGroup | 가져오기를 완료 한 사용자의 나이 그룹을 지정 하는 다음 문자열 중 하나입니다.<ul><li><strong>0 – 17</strong></li><li><strong>18 – 24</strong></li><li><strong>25 – 34</strong></li><li><strong>35 – 49</strong></li><li><strong>50 이상</strong></li><li><strong>알 수 없음</strong></li></ul> |
| gender | 가져오기를 완료 한 사용자의 성별을 지정 하는 다음 문자열 중 하나입니다.<ul><li><strong>M</strong></li><li><strong>350</strong></li><li><strong>알 수 없음</strong></li></ul> |


### <a name="request-example"></a>요청 예제

다음 예제에서는 앱에 대 한 획득 깔때기 데이터를 가져오는 몇 가지 요청을 보여 줍니다. *ApplicationId* 값을 앱의 스토어 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=8/1/2016&endDate=8/31/2016&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 형식   | 설명                  |
|------------|--------|-------------------------------------------------------|
| 값      | array  | 앱에 대 한 획득 깔때기 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 아래 [깔때기 값](#funnel-values) 섹션을 참조 하십시오.                  |
| TotalCount | int    | *값* 배열에 있는 개체의 총 수입니다.        |


### <a name="funnel-values"></a>깔때기 값

*값* 배열의 개체에는 다음 값이 포함 됩니다.

| 값               | 형식   | 설명                           |
|---------------------|--------|-------------------------------------------|
| MetricType                | 문자열 | 이 개체에 포함 된 [깔때기 데이터의 형식을](../publish/acquisitions-report.md#acquisition-funnel) 지정 하는 다음 문자열 중 하나입니다.<ul><li><strong>페이지 보기</strong></li><li><strong>취득</strong></li><li><strong>설치</strong></li><li><strong>사용 현황</strong></li></ul> |
| UserCount       | 문자열 | *MetricType* 값으로 지정 된 깔때기 단계를 수행한 사용자 수입니다.             |


### <a name="response-example"></a>응답 예제

다음 예제에서는이 요청에 대 한 예제 JSON 응답 본문을 보여 줍니다.

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

* [인수 보고서](../publish/acquisitions-report.md)
* [Microsoft Store 서비스를 사용 하 여 분석 데이터 액세스](access-analytics-data-using-windows-store-services.md)
* [앱 인수 가져오기](get-app-acquisitions.md)
