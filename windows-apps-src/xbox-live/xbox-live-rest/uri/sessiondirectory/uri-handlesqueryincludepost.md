---
title: POST (/handles/query?include=relatedInfo)
assetID: 66ecd1fe-24d4-4cd5-256d-8950ac658529
permalink: en-us/docs/xboxlive/rest/uri-handlesqueryincludepost.html
description: " POST (/handles/query?include=relatedInfo)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eb30561c91a085902dd9d79b6c4a2045dc709bfb
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8794750"
---
# <a name="post-handlesqueryincluderelatedinfo"></a>POST (/handles/query?include=relatedInfo)
세션 관련된 정보를 포함 하는 세션 핸들에 대 한 쿼리를 만듭니다.

> [!IMPORTANT]
> 이 메서드는 2015 멀티 플레이어에서 사용 되 고 및 나중 멀티 플레이 해당 버전에만 적용 됩니다. 템플릿 계약 104/105 이상을 사용 하 여 사용 하기 위한 하 고 X Xbl-계약 버전의 헤더 요소가: 104/105 또는 나중에 모든 요청.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4ECB)
  * [쿼리 문자열 매개 변수](#ID4EPB)
  * [HTTP 상태 코드](#ID4EAF)
  * [요청 본문](#ID4EHF)
  * [응답 본문](#ID4EZF)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명

이 HTTP/REST 메서드 "포함" 쿼리 문자열에 지정 된 세션 정보를 사용 하 여 핸들 데이터에 대 한 쿼리를 만듭니다. 쿼리 문자열 값은 쉼표로 구분 된 목록, 예를 들어, 지원 되는 앞으로 쿼리 옵션을 지원 하도록 확장할 수 있도록 "? 포함 = relatedInfo, 세션"입니다. POST 메서드를 **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetActivitiesForUsersAsync**하 여 줄 바꿈할 수 있습니다.

이 메서드에 대 한 요청 본문에 있는 종류 필드 "작업"으로 설정 되어 있어야 합니다. 응답 본문에 있는 항목의 **Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**속성에 직접 매핑됩니다.

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

<a id="ID4EPB"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

다음 표에 쿼리 문자열 매개 변수를 사용 하 여 쿼리를 수정할 수 있습니다.

| <b>매개 변수</b>| <b>유형</b>| <b>설명</b>|
| --- | --- | --- | --- |
| 키워드| string| 예를 들어 "foo"가, 키워드는는에 있어야 세션이 나 템플릿 검색 하려는 경우. |
| xuid| 64 비트 부호 없는 정수| 예를 들어 "123", 세션 쿼리에서 포함 하도록 하는 Xbox 사용자 ID입니다. 기본적으로 사용자를 포함 하는 세션에서 활성 상태 여야 합니다. |
| 예약| 부울| 세션을 포함 하려면 사용자 예약 된 플레이어로 설정 되어 있지만 활성 플레이어를 연결 하지 않습니다. 이 매개 변수는 자신의 세션 쿼리할 때 또는 특정 사용자의 세션-서버를 쿼리 하는 경우에 사용 됩니다. |
| 비활성| 부울| 사용자가 수락 하지만 적극적으로 재생 되지 세션을 포함 하는 경우 사용자가 "준비" 하지만 "비활성" 세션 비활성으로 계산 됩니다. |
| 개인| 부울| 개인 세션을 포함 하는 경우 이 매개 변수는 자신의 세션 쿼리할 때 또는 특정 사용자의 세션-서버를 쿼리 하는 경우에 사용 됩니다. |
| visibility| 문자열| 세션에 대 한 상태를 표시 합니다. 가능한 값은 <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionVisibility</b>으로 정의 됩니다. 이 매개 변수는 "열기"로 설정 되 면 쿼리만 열려 있는 세션을 포함 해야 합니다. <i>전용</i> 매개 변수를 설정 해야 "개인"으로 설정 되 면 true입니다. |
| 버전| 32 비트 부호 있는 정수| 포함 되어야 하는 최대 세션 버전입니다. 예를 들어 주요 세션 버전 2만 해당 세션을 지정 하는 값 2 또는 덜 포함 되어야 합니다. 버전 번호는 요청의 계약 버전 mod 100 보다 작거나 이어야 합니다. |
| 시험| 32 비트 부호 있는 정수| 검색할 세션의 최대 수입니다. 이 숫자는 0과 100 사이 여야 합니다. |


*개인* 또는 *예약* 을 true로 설정 하면 세션에 대 한 서버 수준 액세스에 필요 합니다. 또는 이러한 설정을 호출자의 XUID URI의 XUID 필터와 일치 하도록 요청 해야 합니다. 그렇지 않은 경우 HTTP/403 상태 코드는 반환 그러한 세션은 실제로 존재 하는지 여부.

<a id="ID4EAF"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4EHF"></a>


## <a name="request-body"></a>요청 본문

<a id="ID4ENF"></a>


### <a name="sample-request"></a>샘플 요청

사용자의 "즐겨찾기" 소셜 그래프에 대 한 모든 활동을 가져오려면 게시 본문은 다음과 같습니다.


```cpp
{
  "type": "activity",
  "scid": "B5B1F71F-A328-4371-89E0-C3AD222D0E92"  // optional filter on scid
  "owners": {
     "people": {
       "moniker": "favorites",
       "monikerXuid": "3210"
     }
  }
}

```


<a id="ID4EZF"></a>


## <a name="response-body"></a>응답 본문

각 핸들에 포함 된 고유 ID를 사용 하 여 핸들 구조의 배열로 결과가 반환 됩니다. 특정 세션 정보가 **relatedInfo** 개체가 반환 됩니다. 노트를이 URI에이 정보는 일반 POST 메서드에 대 한 반환 되지 않습니다.

<a id="ID4EDG"></a>


### <a name="sample-response"></a>예제 응답


```cpp
{
    "results": [{
        "id": "11111111-ebe0-42da-885f-033860a818f6",
        "type": "activity",
        "version": 1,
        "sessionRef": {
            "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
            "templateName": "party",
            "name": "e3a836aeac6f4cbe9bcab985494d3175"
        },

    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
        "relatedInfo": {
            "visibility": "open",
            "joinRestriction": "followed",
            "closed": true,
            "maxMembersCount": 8,
            "membersCount": 4,
        }
    },
    {
        "id": "11111111-ebe0-42da-885f-033860a818f7",
        "type": "activity",
        "version": 1,
        "sessionRef": {
            "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
            "templateName": "TitleStorageTestDefault",
            "name": "795fcaa7-8377-4281-bd7e-e86c12843632"
        },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
        "relatedInfo": {
            "visibility": "open",
            "joinRestriction": "followed",
            "closed": false,
            "maxMembersCount": 8,
            "membersCount": 4,
        }
    }]
}

```


<a id="ID4ENG"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EPG"></a>


##### <a name="parent"></a>부모

[/handles/query](uri-handlesquery.md)
