---
description: Microsoft Store 분석 API에서 이 메서드를 사용하여 Xbox Live 상태 데이터를 가져옵니다.
title: Xbox Live 상태 데이터 가져오기
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API, Xbox Live 분석, 상태, 클라이언트 오류
ms.localizationpriority: medium
ms.openlocfilehash: 3b996d85776cb49d45cc5b699709b4eb107e7086
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8323261"
---
# <a name="get-xbox-live-health-data"></a>Xbox Live 상태 데이터 가져오기


Microsoft Store 분석 API에서 이 메서드를 사용하여 [Xbox Live 지원 게임](../xbox-live/index.md)의 상태 데이터를 가져옵니다. 이 정보는 파트너 센터에서 [Xbox 분석 보고서](../publish/xbox-analytics-report.md) 에 사용할 수 있습니다.

> [!IMPORTANT]
> 이 방법은 Xbox용 게임 또는 Xbox Live 서비스를 사용하는 게임만 지원합니다. 이러한 게임은 [Microsoft 파트너](../xbox-live/developer-program-overview.md#microsoft-partners)에 의해 게시된 게임 및 [ID@Xbox 프로그램](../xbox-live/developer-program-overview.md#id)을 통해 제출한 게임을 포함하는 [개념 승인 프로세스](../gaming/concept-approval.md)를 거쳐야 합니다. 이 방법은 현재 [Xbox Live 크리에이터스 프로그램](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)을 통해 게시된 게임을 지원하지 않습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수


| 매개 변수        | 형식   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | Xbox Live 상태 데이터를 검색하려는 게임의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다.  |  예  |
| metricType | 문자열 | 검색할 Xbox Live 분석 데이터의 유형을 지정하는 문자열입니다. 이 메서드의 경우 값 **callingpattern**을 지정합니다.  |  예  |
| startDate | 날짜 | 검색할 상태 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜보다 30일 전입니다. |  아니요  |
| endDate | 날짜 | 검색할 상태 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다. |  아니요  |
| filter | string  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 **eq** 또는 **ne** 연산자와 연결된 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul> | 아니요   |
| groupby | string | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li><strong>date</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul><p/>하나 이상의 *groupby* 필드를 지정하는 경우 응답 본문에서 지정하지 않은 다른 *groupby* 필드의 값은 **All**입니다. |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 Xbox Live 지원 게임의 상태 데이터를 가져오려는 요청에 대해 설명합니다. *applicationId* 값을 게임의 Store ID로 바꿉니다.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=callingpattern&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

| 값      | 유형   | 설명                  |
|------------|--------|-------------------------------------------------------|
| 값      | 배열  | 상태 데이터를 포함하는 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 다음 표를 참조하세요.                                                                                                                      |
| @nextLink  | string | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10000으로 설정되어 있지만 쿼리에 대한 데이터의 행이 10000개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.   |


*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값               | 유형   | 설명                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 문자열 | 상태 데이터를 검색할 게임의 Store ID입니다.     |
| date                | 문자열 | 상태 데이터에 대한 날짜 범위의 시작 날짜입니다. 요청에서 하루를 지정한 경우 이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정한 경우 이 값은 해당 날짜 범위의 시작 날짜입니다. |
| deviceType          | 문자열 | 사용자 게임이 사용된 장치 유형을 나타내는 다음 문자열 중 하나입니다.<p/><ul><li><strong>XboxOne</strong></li><li><strong>WindowsOneCore</strong>(이 값은 PC를 나타냄)</li><li><strong>알 수 없음</strong></li></ul>  |
| sandboxId     | 문자열 |   게임에 대해 생성된 샌드박스 ID입니다. 이는 값 RETAIL이거나 개인용 샌드박스의 ID일 수 있습니다.   |
| packageVersion     | 문자열 |  게임에 대한 네 부분으로 된 패키지 버전입니다.  |
| callingPattern     | 개체 |  지정된 날짜 범위 동안 게임에서 사용된 Xbox Live 서비스가 반환한 각 상태 코드에 대한 서비스 응답, 장치 및 사용자 데이터를 제공하는 [callingPattern](#callingpattern) 개체입니다.     |


### <a name="callingpattern"></a>callingPattern

| 값      | 유형   | 설명                  |
|------------|--------|-------------------------------------------------------|
| service      | 문자열  |   상태 데이터와 관련된 Xbox Live 서비스의 이름입니다.       |
| endpoint      | 문자열  |   상태 데이터와 관련된 Xbox Live 서비스의 끝점입니다.        |
| httpStatusCode      | 문자열  |  이 상태 데이터 세트의 HTTP 상태 코드입니다.<p/><p/>**참고**&nbsp;&nbsp;상태 코드 **429E**는 호출하는 동안 [세분화된 속도 제한](../xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md)이 단지 제외되었기 때문에 서비스 호출이 성공했음을 나타냅니다. 서비스가 대량으로 시행되는 경우 나중에 세분화된 속도 제한을 적용할 수 있으며, 이 경우 호출로 인해 [HTTP 429 상태 코드](../xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md#http-429-response-object)가 생성됩니다.         |
| serviceResponses      | 숫자  | 지정된 상태 코드를 반환한 서비스 응답 수입니다.         |
| uniqueDevices      | 숫자  |  서비스를 호출하고 지정된 상태 코드를 받은 고유한 장치 수입니다.       |
| uniqueUsers      | 숫자  |   지정된 상태 코드를 받은 고유한 사용자 수입니다.       |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다. 이 예제에 표시된 서비스 이름과 끝점은 실제가 아니며 예제에서만 사용됩니다.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "date": "2018-01-30",
      "deviceType": "All",
      "sandboxId": "RETAIL",
      "packageVersion": "Unknown",
      "callingpattern": [
        {
          "service": "userstats",
          "endpoint": "UserStats.BatchReadHandler.POST",
          "httpStatusCode": "200",
          "serviceResponses": 160891,
          "uniqueDevices": 410,
          "uniqueUsers": 410
        },
        {
          "service": "userstats",
          "endpoint": "UserStats.BatchStatReadHandler.GET",
          "httpStatusCode": "200",
          "serviceResponses": 422,
          "uniqueDevices": 0,
          "uniqueUsers": 30
        }
      ],
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [Xbox Live 분석 데이터 가져오기](get-xbox-live-analytics.md)
* [Xbox Live 도전 과제 데이터 가져오기](get-xbox-live-achievements-data.md)
* [Xbox Live 게임 허브 데이터 가져오기](get-xbox-live-game-hub-data.md)
* [Xbox Live 클럽 데이터 가져오기](get-xbox-live-club-data.md)
* [Xbox Live 멀티플레이 데이터 가져오기](get-xbox-live-multiplayer-data.md)
