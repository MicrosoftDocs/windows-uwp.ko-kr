---
ms.assetid: 7b07a6ca-4be1-497c-a901-0a2da3762555
description: Microsoft Store 프로모션 API에서 이 메서드를 사용하여 프로모션 광고 캠페인을 제작, 편집하고 가져옵니다.
title: 광고 캠페인 관리
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 프로모션 API, 광고 캠페인
ms.localizationpriority: medium
ms.openlocfilehash: 6529c1a21865b2997d36e9b254b19f971f620490
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633228"
---
# <a name="manage-ad-campaigns"></a>광고 캠페인 관리

[Microsoft Store 프로모션 API](run-ad-campaigns-using-windows-store-services.md)에서 이 메서드를 사용하여 앱의 프로모션 광고 캠페인을 제작, 편집하여 가져옵니다. 이 메서드를 사용해 제작한 캠페인은 한 앱에만 연결할 수 있습니다.

>**참고**&nbsp;&nbsp;파트너 센터에서 프로그래밍 방식으로 만든 캠페인을 액세스할 수 있습니다 및에 만들기 및 파트너 센터를 사용 하 여 광고 캠페인을 관리할 수 있습니다. 파트너 센터에서 광고 캠페인을 관리 하는 방법에 대 한 자세한 내용은 참조 하세요. [앱에 대 한 광고 캠페인을 만들](../publish/create-an-ad-campaign-for-your-app.md)합니다.

이러한 메서드를 사용해 캠페인을 만들거나 업데이트하면 일반적으로 다음 메서드 중 하나 이상을 호출하여 캠페인에 연결된 *배달 라인*, *대상 프로필*, *크리에이티브*를 관리할 수 있습니다. 광고 캠페인, 배달 라인, 대상 프로필, 크리에이티브 간의 관계에 대한 자세한 내용은 [Microsoft Store 서비스를 사용하여 광고 캠페인 실행](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)을 참조하세요.

* [광고 캠페인에 대 한 배달 줄 관리](manage-delivery-lines-for-ad-campaigns.md)
* [광고 캠페인에 대 한 대상 프로필 관리](manage-targeting-profiles-for-ad-campaigns.md)
* [광고 캠페인에 대 한 소재를 관리 합니다.](manage-creatives-for-ad-campaigns.md)

## <a name="prerequisites"></a>필수 구성 요소

이 메서드를 사용하려면 먼저 다음 작업을 완료해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 프로모션 API의 [필수 조건](run-ad-campaigns-using-windows-store-services.md#prerequisites)을 모두 완료합니다.

  >**참고**&nbsp;&nbsp;필수 구성 요소의 일부로, 해야 했는지 [파트너 센터에서 하나 이상의 유료 광고 캠페인 만들기](../publish/create-an-ad-campaign-for-your-app.md) 및 파트너의 ad 캠페인에 대 한 하나 이상의 결제 방법을 추가 하는 중심입니다. 이 API를 사용 하 여 만든 광고 캠페인에 대 한 배달 줄에서 선택한 기본 결제를 자동으로 청구 됩니다 합니다 **광고 캠페인** 파트너 센터의 페이지입니다.

* 이 메서드의 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.


## <a name="request"></a>요청

이 메서드에는 다음 URI가 있습니다.

| 메서드 유형 | 요청 URI                                                      |  설명  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  새 AD 캠페인을 만듭니다.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  *campaignId*로 지정된 광고 캠페인을 편집합니다.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  *campaignId*로 지정된 광고 캠페인을 가져옵니다.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  광고 캠페인에 대해 쿼리합니다. 지원되는 쿼리 매개 변수는 [매개 변수](#parameters) 섹션을 참조하십시오.  |


### <a name="header"></a>헤더

| 헤더        | 형식   | 설명         |
|---------------|--------|---------------------|
| 권한 부여 | 문자열 | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** &lt; *토큰*&gt;합니다. |
| 추적 ID   | GUID   | 선택 사항. 호출 흐름을 추적하는 ID입니다.                                  |


<span id="parameters"/> 

### <a name="parameters"></a>매개 변수

광고 캠페인을 쿼리하기 위한 GET 메서드는 다음 옵션 쿼리 매개 변수를 지원합니다.

| 이름        | 형식   |  설명      |    
|-------------|--------|---------------|------|
| skip  |  int   | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 데이터 집합의 페이지를 탐색합니다. 예를 들어 fetch=10 및 skip=0이면 데이터의 처음 10개 행을 검색하고 top=10 및 skip=10이면 데이터의 다음 10개 행을 검색하는 방식입니다.    |       
| fetch  |  int   | 요청에서 반환할 데이터의 행의 수입니다.    |       
| campaignSetSortColumn  |  문자열   | 응답 본문의 [캠페인](#campaign) 개체의 순서를 지정된 필드로 지정합니다. 구문은 <em>CampaignSetSortColumn=field</em>이고 여기서 <em>field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.</p><ul><li><strong>id</strong></li><li><strong>createdDateTime</strong></li></ul><p>기본 값은 **createdDateTime**입니다.     |     
| isDescending  |  부울   | 응답 본문의 [캠페인](#campaign) 개체를 내림차순이나 오름차순으로 정렬합니다.   |         
| storeProductId  |  문자열   | 이 값을 사용해 지정된 [스토어 ID](in-app-purchases-and-trials.md#store-ids)의 앱에 연결된 광고 캠페인만 반환합니다. 제품에 대한 예시 스토어 ID는 9nblggh42cfd입니다.   |         
| 레이블  |  문자열   | 이 값을 사용해 [캠페인](#campaign) 개체에서 지정된 *레이블*을 포함하는 광고 캠페인만 반환합니다.    |       |    


### <a name="request-body"></a>요청 본문

POST 및 PUT 메서드는 [캠페인](#campaign) 개체의 필수 필드 및 설정하거나 변경하려는 추가 필드가 있는 JSON 요청 본문이 필요합니다.


### <a name="request-examples"></a>요청 예제

다음 예시에서는 POST 메서드를 호출하여 광고 캠페인을 만드는 방법을 보여줍니다.

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

다음 예제는 특정 광고 캠페인을 검색할 GET 메서드를 호출하는 방법을 보여줍니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/31043481  HTTP/1.1
Authorization: Bearer <your access token>
```

다음 예제는 만든 날짜로 정렬된 광고 캠페인 집합에 대해 쿼리하기 위해 GET 메서드를 호출하는 방법을 보여줍니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign?storeProductId=9nblggh42cfd&fetch=100&skip=0&campaignSetSortColumn=createdDateTime HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>응답

이러한 메서드는 호출한 메서드에 따라 하나 이상의 [캠페인](#campaign) 개체가 있는 JSON 응답 본문을 반환합니다. 다음 예제는 특정 캠페인의 GET 메서드 응답 본문을 보여줍니다.

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

이 메서드에 대한 요청 및 응답 본문은 다음 필드를 포함합니다. 이 표에서는 읽기 전용(PUT 메서드에서 변경할 수 없음을 의미) 필드와 POST 메서드에 대한 요청 본문에서 필요한 필드가 어떤 것인지 보여줍니다.

| 필드        | 형식   |  설명      |  읽기 전용  | 기본값  | POST에 필요한지 여부 |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  정수   |  광고 캠페인의 ID입니다.     |   예    |      |  아니오     |       
|  name   |  문자열   |   광고 캠페인의 이름입니다.    |    아니오   |      |  예     |       
|  configuredStatus   |  문자열   |  개발자가 지정한 광고 캠페인의 상태를 지정하는 다음 값들 중 하나입니다. <ul><li>**Active**</li><li>**비활성**</li></ul>     |  아니오     |  활성    |   예    |       
|  effectiveStatus   |  문자열   |   시스템 유효성 검사에 따라 광고 캠페인의 유효성 관련 상태를 지정하는 다음 값들 중 하나입니다. <ul><li>**Active**</li><li>**비활성**</li><li>**처리**</li></ul>    |    예   |      |   아니오      |       
|  effectiveStatusReasons   |  배열   |  광고 캠페인의 유효성 관련 상태에 대한 이유를 지정하는 다음 값들 중 한 개 이상입니다. <ul><li>**AdCreativesInactive**</li><li>**BillingFailed**</li><li>**AdLinesInactive**</li><li>**ValidationFailed**</li><li>**못했습니다.**</li></ul>      |  예     |     |    아니오     |       
|  storeProductId   |  문자열   |  이 광고 캠페인이 연결된 앱의 [스토어 ID](in-app-purchases-and-trials.md#store-ids)입니다. 제품에 대한 예시 스토어 ID는 9nblggh42cfd입니다.     |   예    |      |  예     |       
|  레이블   |  배열   |   캠페인의 사용자 지정 레이블을 표시하는 하나 이상의 문자열입니다. 이러한 레이블을 캠페인 검색 및 태그 지정에 사용할 수 있습니다.    |   아니오    |  null    |    아니오     |       
|  유형   | 문자열    |  캠페인 유형을 지정하는 다음 값 중 하나입니다. <ul><li>**유료**</li><li>**집**</li><li>**커뮤니티**</li></ul>      |   예    |      |   예    |       
|  목표   |  문자열   |  캠페인의 목표를 지정하는 다음 값 중 하나입니다. <ul><li>**DriveInstall**</li><li>**DriveReengagement**</li><li>**DriveInAppPurchase**</li></ul>     |   아니오    |  DriveInstall    |   예    |       
|  라인   |  배열   |   광고 캠페인과 연결된 [배달 라인](manage-delivery-lines-for-ad-campaigns.md#line)을 식별하는 하나 이상의 개체입니다. 이 필드의 각 개체는 배달 라인의 ID와 이름을 지정하는 *ID* 및 *이름* 필드로 구성됩니다.     |   아니오    |      |    아니오     |       
|  createdDate   |  문자열   |  ISO 8601 형식의 광고 캠페인이 만들어진 날짜와 시간입니다.     |  예     |      |     아니오    |       |


## <a name="related-topics"></a>관련 항목

* [Microsoft Store Services를 사용 하 여 광고 캠페인을 실행 합니다.](run-ad-campaigns-using-windows-store-services.md)
* [광고 캠페인에 대 한 배달 줄 관리](manage-delivery-lines-for-ad-campaigns.md)
* [광고 캠페인에 대 한 대상 프로필 관리](manage-targeting-profiles-for-ad-campaigns.md)
* [광고 캠페인에 대 한 소재를 관리 합니다.](manage-creatives-for-ad-campaigns.md)
* [광고 캠페인 성능 데이터 가져오기](get-ad-campaign-performance-data.md)
