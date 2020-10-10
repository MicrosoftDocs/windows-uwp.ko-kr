---
ms.assetid: 7b07a6ca-4be1-497c-a901-0a2da3762555
description: Microsoft Store 프로 모션 API에서이 방법을 사용 하 여 프로 모션 광고 캠페인을 만들고 편집 하 고 가져올 수 있습니다.
title: 광고 캠페인 관리
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 프로 모션 API, ad 캠페인
ms.localizationpriority: medium
ms.openlocfilehash: bf5945ca68a4ea060943cb2cc89ba907f597e4f5
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878406"
---
# <a name="manage-ad-campaigns"></a>광고 캠페인 관리

[Microsoft Store 프로 모션 API](run-ad-campaigns-using-windows-store-services.md) 에서 이러한 메서드를 사용 하 여 앱에 대 한 프로 모션 광고 캠페인을 만들고 편집 하 고 가져옵니다. 이 방법을 사용 하 여 만든 각 캠페인은 하나의 앱에만 연결 될 수 있습니다.

>**Note** &nbsp; 참고 &nbsp; 파트너 센터를 사용 하 여 ad 캠페인을 만들고 관리할 수도 있으며, 프로그래밍 방식으로 만드는 캠페인은 파트너 센터에서 액세스할 수 있습니다. 파트너 센터에서 ad 캠페인을 관리 하는 방법에 대 한 자세한 내용은 [앱에 대 한 광고 캠페인 만들기](./index.md)를 참조 하세요.

이러한 방법을 사용 하 여 캠페인을 만들거나 업데이트 하는 경우 일반적으로 다음 방법 중 하나 이상을 호출 하 여 캠페인에 연결 된 *배달 선*, *대상 프로필*및 *creatives* 를 관리 합니다. 캠페인, 배달 선, 대상 프로필 및 creatives 간의 관계에 대 한 자세한 내용은 [Microsoft Store services를 사용 하 여 ad 캠페인 실행](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)을 참조 하세요.

* [광고 캠페인의 배달 선 관리](manage-delivery-lines-for-ad-campaigns.md)
* [광고 캠페인의 대상 프로필 관리](manage-targeting-profiles-for-ad-campaigns.md)
* [Ad 캠페인에 대 한 creatives 관리](manage-creatives-for-ad-campaigns.md)

## <a name="prerequisites"></a>필수 구성 요소

이러한 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 프로 모션 API에 대 한 모든 [필수 구성 요소](run-ad-campaigns-using-windows-store-services.md#prerequisites) 를 완료 합니다.

  >**Note** &nbsp; 참고 &nbsp; 필수 구성 요소의 일부로 [파트너 센터에서 하나 이상의 유료 ad 캠페인을 만들고](./index.md) 파트너 센터의 광고 캠페인에 대해 하나 이상의 결제 방법을 추가 해야 합니다. 이 API를 사용 하 여 만든 ad 캠페인의 배달 줄은 파트너 센터의 **ad 캠페인** 페이지에서 선택한 기본 결제 방법을 자동으로 청구 합니다.

* 이러한 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.


## <a name="request"></a>요청

이러한 메서드에는 다음과 같은 Uri가 있습니다.

| 메서드 형식 | 요청 URI                                                      |  설명  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  새 광고 캠페인을 만듭니다.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  *CampaignId*로 지정 된 광고 캠페인을 편집 합니다.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  *CampaignId*로 지정 된 광고 캠페인을 가져옵니다.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  광고 캠페인을 쿼리 합니다. 지원 되는 쿼리 매개 변수는 [매개 변수](#parameters) 섹션을 참조 하세요.  |


### <a name="header"></a>헤더

| 헤더        | 유형   | Description         |
|---------------|--------|---------------------|
| 권한 부여 | 문자열 | 필수 사항입니다. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |
| 추적 ID   | GUID   | (선택 사항) 호출 흐름을 추적 하는 ID입니다.                                  |


<span id="parameters"/> 

### <a name="parameters"></a>매개 변수

Ad 캠페인에 대해 쿼리할 GET 메서드는 다음과 같은 선택적 쿼리 매개 변수를 지원 합니다.

| Name        | 유형   |  설명      |    
|-------------|--------|---------------|------|
| skip  |  int   | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용 하 여 데이터 집합을 페이징 합니다. 예를 들어 fetch = 10 이면 skip = 0 이면 처음 10 개의 데이터 행을 검색 하 고 top = 10 및 skip = 10은 다음 10 개의 데이터 행을 검색 하는 식입니다.    |       
| fetch  |  int   | 요청에 반환할 데이터 행 수입니다.    |       
| campaignSetSortColumn  |  문자열   | 지정 된 필드를 기준으로 응답 본문의 [캠페인](#campaign) 개체를 정렬 합니다. 구문은 <em>CampaignSetSortColumn = field</em>입니다. 여기서 <em>field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.</p><ul><li><strong>id</strong></li><li><strong>createdDateTime</strong></li></ul><p>기본값은 **createdDateTime**입니다.     |     
| isDescending  |  부울   | 응답 본문의 [캠페인](#campaign) 개체를 내림차순 또는 오름차순으로 정렬 합니다.   |         
| 제품 productid  |  문자열   | 지정 된 [저장소 ID](in-app-purchases-and-trials.md#store-ids)를 사용 하 여 앱과 연결 된 광고 캠페인만 반환 하려면이 값을 사용 합니다. 제품의 저장소 ID 예는 9nblggh42cfd입니다.   |         
| label  |  문자열   | 이 값을 사용 하 여 [캠페인](#campaign) 개체에 지정 된 *레이블을* 포함 하는 광고 캠페인만 반환 합니다.    |       |    


### <a name="request-body"></a>요청 본문

POST 및 PUT 메서드에는 [캠페인](#campaign) 개체의 필수 필드와 설정 하거나 변경 하려는 추가 필드가 있는 JSON 요청 본문이 필요 합니다.


### <a name="request-examples"></a>요청 예제

다음 예제에서는 POST 메서드를 호출 하 여 ad 캠페인을 만드는 방법을 보여 줍니다.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign",
    "storeProductId": "9nblggh42cfd",
    "configuredStatus": "Active",
    "objective": "DriveInstalls",
    "type": "Community"
}
```

다음 예제에서는 GET 메서드를 호출 하 여 특정 광고 캠페인을 검색 하는 방법을 보여 줍니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/31043481  HTTP/1.1
Authorization: Bearer <your access token>
```

다음 예제에서는 GET 메서드를 호출 하 여 생성 된 날짜별로 정렬 된 ad 캠페인 집합을 쿼리 하는 방법을 보여 줍니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign?storeProductId=9nblggh42cfd&fetch=100&skip=0&campaignSetSortColumn=createdDateTime HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>응답

이러한 메서드는 호출 된 메서드에 따라 하나 이상의 [캠페인](#campaign) 개체를 포함 하는 JSON 응답 본문을 반환 합니다. 다음 예제에서는 특정 캠페인의 GET 메서드에 대 한 응답 본문을 보여 줍니다.

```json
{
    "Data": {
        "id": 31043481,
        "name": "Contoso App Campaign",
        "createdDate": "2017-01-17T10:12:15Z",
        "storeProductId": "9nblggh42cfd",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "labels": [],
        "objective": "DriveInstalls",
        "type": "Paid",
        "lines": [
            {
                "id": 31043476,
                "name": "Contoso App Campaign - Paid Line"
            }
        ]
    }
}
```


<span id="campaign"/>

## <a name="campaign-object"></a>캠페인 개체

이러한 메서드에 대 한 요청 및 응답 본문에는 다음 필드가 포함 됩니다. 이 표에서는 읽기 전용 필드 (PUT 메서드에서는 변경할 수 없음) 및 POST 메서드의 요청 본문에 필요한 필드를 보여 줍니다.

| 필드        | Type   |  설명      |  읽기 전용  | 기본값  | POST에 필요 |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  정수   |  광고 캠페인의 ID입니다.     |   예    |      |  예     |       
|  name   |  문자열   |   광고 캠페인의 이름입니다.    |    아니요   |      |  예     |       
|  configuredStatus   |  문자열   |  개발자가 지정한 ad 캠페인의 상태를 지정 하는 다음 값 중 하나입니다. <ul><li>**활성**</li><li>**비활성**</li></ul>     |  아니요     |  활성    |   예    |       
|  effectiveStatus   |  문자열   |   시스템 유효성 검사를 기준으로 광고 캠페인의 유효 상태를 지정 하는 다음 값 중 하나입니다. <ul><li>**활성**</li><li>**비활성**</li><li>**처리 중**</li></ul>    |    예   |      |   아니요      |       
|  effectiveStatusReasons   |  array   |  Ad 캠페인의 유효 상태에 대 한 이유를 지정 하는 다음 값 중 하나 이상입니다. <ul><li>**AdCreativesInactive**</li><li>**BillingFailed**</li><li>**Ad회선 비활성**</li><li>**ValidationFailed**</li><li>**실패**</li></ul>      |  예     |     |    아니요     |       
|  제품 productid   |  문자열   |  이 광고 캠페인이 연결 된 앱의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다. 제품의 저장소 ID 예는 9nblggh42cfd입니다.     |   예    |      |  예     |       
|  레이블   |  array   |   캠페인의 사용자 지정 레이블을 나타내는 하나 이상의 문자열입니다. 이러한 레이블은 캠페인을 검색 하 고 태그를 지정 하는 데 사용 됩니다.    |   아니요    |  null    |    예     |       
|  type   | 문자열    |  캠페인 유형을 지정 하는 다음 값 중 하나입니다. <ul><li>**유료**</li><li>**집**</li><li>**커뮤니티**</li></ul>      |   예    |      |   예    |       
|  objective   |  문자열   |  캠페인의 목표를 지정 하는 다음 값 중 하나입니다. <ul><li>**드라이브 설치**</li><li>**DriveReengagement**</li><li>**드라이브**</li></ul>     |   아니요    |  드라이브 설치    |   예    |       
|  lines   |  array   |   광고 캠페인과 연결 된 [배달 선을](manage-delivery-lines-for-ad-campaigns.md#line) 식별 하는 하나 이상의 개체입니다. 이 필드의 각 개체는 배달 줄의 ID와 이름을 지정 하는 *id* 및 *이름* 필드로 구성 됩니다.     |   아니요    |      |    아니요     |       
|  createdDate   |  문자열   |  Ad 캠페인이 생성 된 날짜와 시간 (ISO 8601 형식)입니다.     |  예     |      |     아니요    |       |


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 ad 캠페인 실행](run-ad-campaigns-using-windows-store-services.md)
* [광고 캠페인의 배달 선 관리](manage-delivery-lines-for-ad-campaigns.md)
* [광고 캠페인의 대상 프로필 관리](manage-targeting-profiles-for-ad-campaigns.md)
* [Ad 캠페인에 대 한 creatives 관리](manage-creatives-for-ad-campaigns.md)
* [광고 캠페인 성과 데이터 가져오기](get-ad-campaign-performance-data.md)