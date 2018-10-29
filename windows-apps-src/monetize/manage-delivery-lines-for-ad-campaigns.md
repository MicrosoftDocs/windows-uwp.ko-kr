---
author: Xansky
ms.assetid: dc632a4c-ce48-400b-8e6e-1dddbd13afff
description: Microsoft Store 프로모션 API에서 이 메서드를 사용하여 홍보용 광고 캠페인 배달 라인을 관리합니다.
title: 배달 라인 관리
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 프로모션 API, 광고 캠페인
ms.localizationpriority: medium
ms.openlocfilehash: 346383504abd7927cf863afa59bcb574ddd2495d
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5750269"
---
# <a name="manage-delivery-lines"></a>배달 라인 관리

Microsoft Store 프로모션 API에서 이 메서드를 사용하여 하나 이상의 *배달 라인*을 만들어 인벤토리를 구입하고 홍보용 광고 캠페인 광고를 제공합니다. 각 배달 라인에 대해 타기팅을 설정하고 입찰 가격을 설정할 수 있으며, 예산을 설정하고 사용할 크리에이티브와 연결하여 지출할 금액을 결정할 수 있습니다.

배달 라인과 광고 캠페인, 대상 프로필, 크리에이티브 간의 관계에 대한 자세한 내용은 [Microsoft Store 서비스를 사용하여 광고 캠페인 실행](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)을 참조하세요.

>**참고**&nbsp;&nbsp;이 API를 사용하여 광고 캠페인의 배달 라인을 성공적으로 만들려면 먼저 [개발자 센터 대시보드의 **앱 홍보** 페이지를 사용하여 유료 광고 캠페인을 하나 생성](../publish/create-an-ad-campaign-for-your-app.md)하고 이 페이지에서 하나 이상의 결제 방법을 추가해야 합니다. 이렇게 하면 이 API를 사용하여 광고 캠페인의 청구 가능한 배달 라인을 성공적으로 만들 수 있습니다. API를 사용하여 만든 광고 캠페인은 대시보드의 **앱 홍보** 페이지에 선택된 기본 결제 방법에 자동으로 요금을 청구합니다.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 먼저 다음 작업을 완료해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 프로모션 API의 [필수 조건](run-ad-campaigns-using-windows-store-services.md#prerequisites)을 모두 완료합니다.

  > [!NOTE]
  > 필수 조건의 일부로, [개발자 센터 대시보드에서 하나 이상의 유료 광고 캠페인을 생성](../publish/create-an-ad-campaign-for-your-app.md)하고 대시보드에서 광고 캠페인에 대한 결제 방법을 하나 이상 추가해야 합니다. 이 API를 사용하여 만든 배달 라인은 대시보드의 **앱 홍보** 페이지에 선택된 기본 결제 방법에 자동으로 요금을 청구합니다.

* 이 메서드의 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 URI가 있습니다.

| 메서드 유형 | 요청 URI         |  설명  |
|--------|---------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line``` |  새 배달 라인을 만듭니다.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  *lineId*에서 지정한 배달 라인을 편집합니다.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  *lineId*가 지정한 배달 라인을 가져옵니다.  |


### <a name="header"></a>헤더

| 헤더        | 유형   | 설명         |
|---------------|--------|---------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |
| 추적 ID   | GUID   | 선택 사항입니다. 호출 흐름을 추적하는 ID입니다.                                  |


### <a name="request-body"></a>요청 본문

POST 및 PUT 메서드는 [배달 라인](#line) 개체의 필수 필드 및 설정하거나 변경하려는 추가 필드가 있는 JSON 요청 본문이 필요합니다.


### <a name="request-examples"></a>요청 예시

다음 예시에서는 POST 메서드를 호출하여 배달 라인을 만드는 방법을 보여줍니다.

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

다음 예시에서는 GET 메서드를 호출하여 배달 라인을 검색하는 방법을 보여줍니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/31019990  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

이러한 메서드는 만들거나 업데이트하거나 검색한 배달 라인에 대한 정보가 포함된 [배달 라인](#line) 개체가 있는 JSON 응답 본문을 반환합니다. 다음 예시에서는 이 메서드에 대한 응답 본문을 보여줍니다.

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

이 메서드에 대한 요청 및 응답 본문은 다음 필드를 포함합니다. 이 표에서는 읽기 전용(PUT 메서드에서 변경할 수 없음을 의미) 필드와 POST 또는 PUT 메서드에 대한 요청 본문에서 필요한 필드가 어떤 것인지 보여줍니다.

| 필드        | 유형   |  설명      |  읽기 전용  | 기본값  | POST/PUT에 필요한지 여부 |   
|--------------|--------|---------------|------|-------------|------------|
|  id   |  정수   |  배달 라인의 ID입니다.     |   예    |      |  아니요      |    
|  name   |  문자열   |   배달 라인의 이름입니다.    |    아니요   |      |  POST     |     
|  configuredStatus   |  문자열   |  개발자가 지정한 배달 라인의 상태를 지정하는 다음 값들 중 하나입니다. <ul><li>**활성**</li><li>**비활성**</li></ul>     |  아니요     |      |   POST    |       
|  effectiveStatus   |  문자열   |   시스템 유효성 검사에 따라 배달 라인의 유효성 관련 상태를 지정하는 다음 값들 중 하나입니다. <ul><li>**활성**</li><li>**비활성**</li><li>**처리 중**</li><li>**실패**</li></ul>    |    예   |      |  아니요      |      
|  effectiveStatusReasons   |  배열   |  배달 라인의 유효성 관련 상태에 대한 이유를 지정하는 다음 값들 중 한 개 이상입니다. <ul><li>**AdCreativesInactive**</li><li>**ValidationFailed**</li></ul>      |  예     |     |    아니요    |           
|  startDatetime   |  문자열   |  배달 라인의 시작 날짜 및 시간으로서, ISO 8601 형식으로 되어 있습니다. 이미 있는 경우에는 이 값을 변경할 수 없습니다.     |    아니요   |      |    POST, PUT     |
|  endDatetime   |  문자열   |  배달 라인의 종료 날짜 및 시간으로서, ISO 8601 형식으로 되어 있습니다. 이미 있는 경우에는 이 값을 변경할 수 없습니다.     |   아니요    |      |  POST, PUT     |
|  createdDatetime   |  문자열   |  배달 라인을 만든 날짜 및 시간으로서, ISO 8601 형식으로 되어 있습니다.      |    예   |      |  아니요      |
|  bidType   |  문자열   |  배달 라인의 입찰 유형을 지정하는 값입니다. 현재는 **CPM** 값만 지원합니다.      |    아니요   |  CPM    |  아니요     |
|  bidAmount   |  10진수   |  모든 광고 요청 입찰에 사용할 입찰 금액입니다.      |    아니요   |  대상 시장에 따른 평균 CPM 값입니다(이 값은 주기적으로 수정됨).    |    아니요    |  
|  dailyBudget   |  10진수   |  배달 라인의 일일 예산입니다. *dailyBudget* 또는 *lifetimeBudget*을 설정해야 합니다.      |    아니요   |      |   POST, PUT(*lifetimeBudget*을 설정하지 않은 경우)       |
|  lifetimeBudget   |  10진수   |   배달 라인의 수명 예산입니다. lifetimeBudget* 또는 *dailyBudget*을 설정해야 합니다.      |    아니요   |      |   POST, PUT(*dailyBudget*을 설정하지 않은 경우)    |
|  targetingProfileId   |  개체   |  이 배달 라인의 대상으로 지정할 사용자, 지역 및 인벤토리 유형을 설명하는 [타기팅 프로필](manage-targeting-profiles-for-ad-campaigns.md#targeting-profile)을 식별하는 개체입니다. 이 개체는 타기팅 프로필의 ID를 지정하는 단일 *id* 필드로 이루어져 있습니다.     |    아니요   |      |  아니요      |  
|  크리에이티브   |  배열   |  배달 라인과 연결된 [크리에이티브](manage-creatives-for-ad-campaigns.md#creative)를 나타내는 하나 이상의 개체입니다. 이 필드의 각 개체는 크리에이티브의 ID를 지정하는 단일 *id* 필드로 이루어져 있습니다.      |    아니요   |      |   아니요     |  
|  campaignId   |  정수   |  상위 광고 캠페인의 ID입니다.      |    아니요   |      |   아니요     |  
|  minMinutesPerImp   |  정수   |  이 배달 라인에서 동일 사용자에게 표시되는 두 광고 노출 간의 최소 시간 간격(분)을 지정합니다.      |    아니요   |  4000    |  아니요      |  
|  pacingType   |  문자열   |  속도 유형을 지정하는 다음 값들 중 하나입니다. <ul><li>**SpendEvenly**</li><li>**SpendAsFastAsPossible**</li></ul>      |    아니요   |  SpendEvenly    |  아니요      |
|  currencyId   |  정수   |  캠페인 통화의 ID입니다.      |    예   |  개발자 계정의 통화(POST 또는 PUT 호출에서 이 필드를 지정할 필요가 없음)    |   아니요     |      |


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 광고 캠페인 실행](run-ad-campaigns-using-windows-store-services.md)
* [광고 캠페인 관리](manage-ad-campaigns.md)
* [광고 캠페인 타기팅 프로필 관리](manage-targeting-profiles-for-ad-campaigns.md)
* [광고 캠페인 크리에이티브 관리](manage-creatives-for-ad-campaigns.md)
* [광고 캠페인 성과 데이터 가져오기](get-ad-campaign-performance-data.md)
