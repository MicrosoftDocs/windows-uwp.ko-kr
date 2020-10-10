---
ms.assetid: dc632a4c-ce48-400b-8e6e-1dddbd13afff
description: Microsoft Store 프로 모션 API에서이 방법을 사용 하 여 판촉 광고 캠페인의 배달 선을 관리할 수 있습니다.
title: 배달 라인 관리
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 프로 모션 API, ad 캠페인
ms.localizationpriority: medium
ms.openlocfilehash: e7b370d8eea61033092d833cdf751f1dade86c6d
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878566"
---
# <a name="manage-delivery-lines"></a>배달 라인 관리

Microsoft Store 프로 모션 API에서 이러한 메서드를 사용 하 여 재고를 구입 하 고 판촉 광고 캠페인을 위한 광고를 배달 하는 하나 이상의 *배달 라인* 을 만들 수 있습니다. 각 배달 라인에 대해 대상을 설정 하 고, 입찰 가격을 설정 하 고, 사용 하려는 creatives에 대 한 예산 및 링크를 설정 하 여 비용을 결정 하는 데 사용할 수 있습니다.

배달 회선과 광고 캠페인, 대상 프로필 및 creatives 간의 관계에 대 한 자세한 내용은 [Microsoft Store services를 사용 하 여 ad 캠페인 실행](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)을 참조 하세요.

>**Note** &nbsp; 참고 &nbsp; 이 API를 사용 하 여 ad 캠페인의 배달 회선을 만들려면 먼저 [파트너 센터의 **ad 캠페인** 페이지를 사용 하 여 유료 ad 캠페인을 만들고](./index.md)이 페이지에서 결제 방법을 하나 이상 추가 해야 합니다. 이 작업을 수행한 후에는이 API를 사용 하 여 광고 캠페인에 대해 청구 가능한 배달 줄을 성공적으로 만들 수 있습니다. API를 사용 하 여 만든 ad 캠페인은 파트너 센터의 **ad 캠페인** 페이지에서 선택한 기본 결제 방법을 자동으로 청구 합니다.

## <a name="prerequisites"></a>필수 구성 요소

이러한 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 프로 모션 API에 대 한 모든 [필수 구성 요소](run-ad-campaigns-using-windows-store-services.md#prerequisites) 를 완료 합니다.

  > [!NOTE]
  > 필수 구성 요소의 일부로 [파트너 센터에서 하나 이상의 유료 ad 캠페인을 만들고](./index.md) 파트너 센터의 광고 캠페인에 대해 하나 이상의 결제 방법을 추가 해야 합니다. 이 API를 사용 하 여 만든 배달 줄은 파트너 센터의 **Ad 캠페인** 페이지에서 선택한 기본 결제 방법을 자동으로 청구 합니다.

* 이러한 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

## <a name="request"></a>요청

이러한 메서드에는 다음과 같은 Uri가 있습니다.

| 메서드 형식 | 요청 URI         |  설명  |
|--------|---------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line``` |  새 배달 선을 만듭니다.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  *Lineid*로 지정 된 배달 줄을 편집 합니다.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  *Lineid*로 지정 된 배달 라인을 가져옵니다.  |


### <a name="header"></a>헤더

| 헤더        | 유형   | Description         |
|---------------|--------|---------------------|
| 권한 부여 | 문자열 | 필수 사항입니다. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |
| 추적 ID   | GUID   | (선택 사항) 호출 흐름을 추적 하는 ID입니다.                                  |


### <a name="request-body"></a>요청 본문

POST 및 PUT 메서드에는 [배달 라인](#line) 개체의 필수 필드와 설정 하거나 변경 하려는 추가 필드가 있는 JSON 요청 본문이 필요 합니다.


### <a name="request-examples"></a>요청 예제

다음 예제에서는 POST 메서드를 호출 하 여 배달 선을 만드는 방법을 보여 줍니다.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/line HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign - Paid Line",
    "configuredStatus": "Active",
    "startDateTime": "2017-01-19T12:09:34Z",
    "endDateTime": "2017-01-31T23:59:59Z",
    "bidAmount": 0.4,
    "dailyBudget": 20,
    "targetProfileId": {
        "id": 310021746
    },
    "creatives": [
        {
            "id": 106851
        }
    ],
    "campaignId": 31043481,
    "minMinutesPerImp ": 1
}
```

다음 예제에서는 GET 메서드를 호출 하 여 배달 선을 검색 하는 방법을 보여 줍니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/31019990  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

이러한 메서드는 생성, 업데이트 또는 검색 된 배달 줄에 대 한 정보를 포함 하는 [배달 선](#line) 개체가 있는 JSON 응답 본문을 반환 합니다. 다음 예제에서는 이러한 메서드의 응답 본문을 보여 줍니다.

```json
{
    "Data": {
        "id": 31043476,
        "name": "Contoso App Campaign - Paid Line",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "startDateTime": "2017-01-19T12:09:34Z",
        "endDateTime": "2017-01-31T23:59:59Z",
        "createdDateTime": "2017-01-17T10:28:34Z",
        "bidType": "CPM",
        "bidAmount": 0.4,
        "dailyBudget": 20,
        "targetProfileId": {
            "id": 310021746
        },
        "creatives": [
            {
                "id": 106126
            }
        ],
        "campaignId": 31043481,
        "minMinutesPerImp ": 1,
        "pacingType ": "SpendEvenly",
        "currencyId ": 732
    }
}

```


<span id="line"/>

## <a name="delivery-line-object"></a>배달 라인 개체

이러한 메서드에 대 한 요청 및 응답 본문에는 다음 필드가 포함 됩니다. 이 표에서는 읽기 전용 필드 (PUT 메서드에서는 변경할 수 없음) 및 POST 또는 PUT 메서드의 요청 본문에 필요한 필드를 보여 줍니다.

| 필드        | Type   |  설명      |  읽기 전용  | 기본값  | POST/PUT에 필요 합니다. |   
|--------------|--------|---------------|------|-------------|------------|
|  id   |  정수   |  배달 줄의 ID입니다.     |   예    |      |  예      |    
|  name   |  문자열   |   배달 라인의 이름입니다.    |    아니요   |      |  POST     |     
|  configuredStatus   |  문자열   |  개발자가 지정한 배달 라인의 상태를 지정 하는 다음 값 중 하나입니다. <ul><li>**활성**</li><li>**비활성**</li></ul>     |  아니요     |      |   POST    |       
|  effectiveStatus   |  문자열   |   시스템 유효성 검사를 기반으로 하는 배달 줄의 유효 상태를 지정 하는 다음 값 중 하나입니다. <ul><li>**활성**</li><li>**비활성**</li><li>**처리 중**</li><li>**실패**</li></ul>    |    예   |      |  아니요      |      
|  effectiveStatusReasons   |  array   |  배달 라인의 유효 상태에 대 한 이유를 지정 하는 다음 값 중 하나 이상입니다. <ul><li>**AdCreativesInactive**</li><li>**ValidationFailed**</li></ul>      |  예     |     |    아니요    |           
|  startDatetime   |  문자열   |  배달 줄의 시작 날짜와 시간 (ISO 8601 형식)입니다. 이미 과거에는이 값을 변경할 수 없습니다.     |    아니요   |      |    POST, PUT     |
|  endDatetime   |  문자열   |  배달 줄의 종료 날짜 및 시간 (ISO 8601 형식)입니다. 이미 과거에는이 값을 변경할 수 없습니다.     |   아니요    |      |  POST, PUT     |
|  createdDatetime   |  문자열   |  배달 선이 생성 된 날짜와 시간 (ISO 8601 형식)입니다.      |    예   |      |  아니요      |
|  bidType   |  문자열   |  배달 선의 입찰 유형을 지정 하는 값입니다. 현재 유일 하 게 지원 되는 값은 **CPM**입니다.      |    아니요   |  CPM    |  아니요     |
|  bidAmount   |  decimal   |  모든 ad 요청을 입찰 하는 데 사용할 입찰 금액입니다.      |    아니요   |  대상 시장을 기반으로 하는 평균 CPM 값입니다 .이 값은 주기적으로 수정 됩니다.    |    아니요    |  
|  dailyBudget   |  decimal   |  배달 라인의 일별 예산입니다. *DailyBudget* 또는 *lifetimeBudget* 중 하나를 설정 해야 합니다.      |    아니요   |      |   POST, PUT ( *lifetimeBudget* 가 설정 되지 않은 경우)       |
|  lifetimeBudget   |  decimal   |   배달 라인의 수명 예산입니다. LifetimeBudget * 또는 *dailyBudget* 중 하나를 설정 해야 합니다.      |    아니요   |      |   POST, PUT ( *dailyBudget* 가 설정 되지 않은 경우)    |
|  targetingProfileId   |  object   |  이 배달 라인의 대상으로 지정할 사용자, 지역 및 재고 유형을 설명 하는 [대상 프로필](manage-targeting-profiles-for-ad-campaigns.md#targeting-profile) 을 식별 하는 개체입니다. 이 개체는 대상 프로필의 ID를 지정 하는 단일 *id* 필드로 구성 됩니다.     |    아니요   |      |  아니요      |  
|  creatives   |  array   |  배달 선과 연결 된 [creatives](manage-creatives-for-ad-campaigns.md#creative) 를 나타내는 하나 이상의 개체입니다. 이 필드의 각 개체는 소재 ID를 지정 하는 단일 *id* 필드로 구성 됩니다.      |    아니요   |      |   아니요     |  
|  campaignId   |  정수   |  부모 ad 캠페인의 ID입니다.      |    아니요   |      |   아니요     |  
|  minMinutesPerImp   |  정수   |  이 배달 선과 동일한 사용자에 게 표시 되는 두 개의 상대 사이의 최소 시간 간격 (분)을 지정 합니다.      |    아니요   |  4000    |  아니요      |  
|  pacingType   |  문자열   |  속도 유형을 지정 하는 다음 값 중 하나입니다. <ul><li>**SpendEvenly**</li><li>**SpendAsFastAsPossible**</li></ul>      |    아니요   |  SpendEvenly    |  아니요      |
|  currencyId   |  정수   |  캠페인의 통화 ID입니다.      |    예   |  개발자 계정의 통화 (POST 또는 PUT 호출에서는이 필드를 지정할 필요가 없음)    |   아니요     |      |


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 ad 캠페인 실행](run-ad-campaigns-using-windows-store-services.md)
* [광고 캠페인 관리](manage-ad-campaigns.md)
* [광고 캠페인의 대상 프로필 관리](manage-targeting-profiles-for-ad-campaigns.md)
* [Ad 캠페인에 대 한 creatives 관리](manage-creatives-for-ad-campaigns.md)
* [광고 캠페인 성과 데이터 가져오기](get-ad-campaign-performance-data.md)