---
title: POST (/handles/query)
assetID: a1a47d49-5c3f-8021-a213-13eb8bddf16a
permalink: en-us/docs/xboxlive/rest/uri-handlesquerypost.html
author: KevinAsgari
description: " POST (/handles/query)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a5caed8863133129c7bc2f0ee3dcb87f1c4a935e
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4430663"
---
# <a name="post-handlesquery"></a>POST (/handles/query)
세션 핸들에 대 한 쿼리를 만듭니다.

> [!IMPORTANT]
> 이 메서드는 2015 멀티 플레이어에서 사용 하 고 나중 및 멀티 플레이 해당 버전에만 적용 됩니다. 템플릿 계약 104/105 이상을 사용 하 여 사용 하기 위한 하 고 X Xbl-계약 버전의 헤더 요소가: 104/105 또는 나중에 모든 요청.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4EDB)
  * [쿼리 문자열 매개 변수](#ID4EQB)
  * [HTTP 상태 코드](#ID4EBF)
  * [요청 본문](#ID4EIF)
  * [응답 본문](#ID4ETF)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명

이 HTTP/REST 메서드 세션 정보 없이 핸들 데이터만 대 한 쿼리를 만듭니다. **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetActivitiesForSocialGroupAsync**래핑한 될 수 있습니다.

이 메서드에 대 한 요청 본문의 유형 필드 "작업"을 설정 해야 합니다. 응답 본문에 있는 항목의 **MultiplayerActivityDetails**속성에 직접 매핑됩니다.

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

<a id="ID4EQB"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

다음 표에 쿼리 문자열 매개 변수를 사용 하 여 쿼리를 수정할 수 있습니다.

| <b>매개 변수</b>| <b>유형</b>| <b>설명</b>|
| --- | --- | --- | --- |
| 키워드| string| 예를 들어 "foo"가, 키워드는는 해야 또는에서 찾을 수 세션 템플릿 검색 하려는 경우. |
| xuid| 64 비트 부호 없는 정수| 예를 들어 "123", 쿼리를 포함 하도록 세션에 대 한 하는 Xbox 사용자 ID입니다. 기본적으로 사용자를 포함 하는 세션에서 활성 상태 여야 합니다. |
| 예약| 부울| 세션을 포함 하려면 사용자 예약된 플레이어로 설정 되어 있지만 되는 활성 플레이어에 가입 되지 않은 했습니다. 이 매개 변수는 자신의 세션 쿼리할 때 또는 특정 사용자의 세션-서버를 쿼리 하는 경우에 사용 됩니다. |
| 비활성| 부울| 사용자는 수락 하지만 적극적으로 재생 되지 세션을 포함 하는 경우 사용자가 "준비" 하지만 "비활성" 세션 비활성으로 계산 됩니다. |
| 개인| 부울| 개인 세션을 포함 하는 경우 true입니다. 이 매개 변수는 자신의 세션 쿼리할 때 또는 특정 사용자의 세션-서버를 쿼리 하는 경우에 사용 됩니다. |
| visibility| 문자열| 세션에 대 한 상태를 표시 합니다. 가능한 값은 <b>MultiplayerSessionVisibility</b>정의 됩니다. 이 매개 변수는 "열기"로 설정 되 면 쿼리만 열려 있는 세션을 포함 해야 합니다. <i>개인</i> 매개 변수를 설정 해야 "개인"으로 설정 되 면 true입니다. |
| 버전| 32 비트 부호 있는 정수| 포함 되어야 하는 최대 세션 버전입니다. 예를 들어 2 값만 해당 세션 2의 주요 세션 버전을 지정 하거나 덜 포함 되어야 합니다. 버전 번호는 요청 계약 버전 mod 100 보다 작거나 이어야 합니다. |
| 시험| 32 비트 부호 있는 정수| 검색할 세션의 최대 수입니다. 이 숫자는 0과 100 사이 여야 합니다. |


*개인* 또는 *예약* 을 true로 설정 하면 세션에 대 한 서버 수준 액세스가 필요 합니다. 또는 이러한 설정을 호출자의 XUID URI에 XUID 필터와 일치 하도록 요청 해야 합니다. 그렇지 않은 경우 HTTP/403 상태 코드는 반환 그러한 세션은 실제로 존재 여부.

<a id="ID4EBF"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4EIF"></a>


## <a name="request-body"></a>요청 본문

사용자의 "즐겨찾기" 소셜 그래프에 대 한 모든 활동에 대 한 요청 본문은 다음과 같습니다.


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


<a id="ID4ETF"></a>


## <a name="response-body"></a>응답 본문

각 구조에 포함 된 고유 ID를 사용 하 여 핸들 핸들 구조의 배열을 검색 됩니다.


```cpp
{
  "results": [
    {
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
    }
  }]
}

```


<a id="ID4E4F"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4E6F"></a>


##### <a name="parent"></a>부모

[/handles/query](uri-handlesquery.md)