---
title: POST (/handles/query)
assetID: a1a47d49-5c3f-8021-a213-13eb8bddf16a
permalink: en-us/docs/xboxlive/rest/uri-handlesquerypost.html
description: " POST (/handles/query)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7540c117931c01c24c79cef6c8ab6540cb65bbcb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651508"
---
# <a name="post-handlesquery"></a>POST (/handles/query)
세션 핸들에 대 한 쿼리를 만듭니다.

> [!IMPORTANT]
> 이 메서드는 2015 멀티 플레이 게임에서 사용 되 고 이상 멀티 플레이 버전에만 적용 됩니다. 템플릿 계약 104/105 이상을 사용 하 여 사용 하기 위한 하 고 X-Xbl-계약-버전 헤더 요소를 필요 합니다. 104/105 또는 나중에 모든 요청 합니다.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4EDB)
  * [쿼리 문자열 매개 변수](#ID4EQB)
  * [HTTP 상태 코드](#ID4EBF)
  * [요청 본문](#ID4EIF)
  * [응답 본문](#ID4ETF)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명

이 HTTP/REST 메서드 세션 정보 없이 데이터를만 처리에 대 한 쿼리를 만듭니다. 래핑할 수 있습니다 **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetActivitiesForSocialGroupAsync**합니다.

이 메서드에 대 한 요청 본문에 유형 필드는 "활동"으로 설정 되어야 합니다. 응답 본문에 있는 항목의 속성에 직접 매핑되는 **MultiplayerActivityDetails**합니다.

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

<a id="ID4EQB"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

다음 표에 쿼리 문자열 매개 변수를 사용 하 여 쿼리를 수정할 수 있습니다.

| <b>매개 변수</b>| <b>Type</b>| <b>설명</b>|
| --- | --- | --- | --- |
| keyword| 문자열| 예를 들어, "foo" 키워드를 검색할 경우 세션 또는 서식 파일에서 발견 되어야 하는 합니다. |
| xuid| 64 비트 부호 없는 정수| 예를 들어 "123", 쿼리에 포함 하는 세션에 대 한 하 여 Xbox 사용자 ID입니다. 기본적으로 사용자를 포함 하는 세션에서 활성 상태 여야 합니다. |
| 예약| boolean| 세션을 포함 하려면 사용자 예약된 플레이어로 설정 되어 있지만 활성 플레이어를 조인 되지 않았습니다. 이 매개 변수는 고유한 세션을 쿼리할 때 또는 특정 사용자의 세션 서버-투-서버를 쿼리 하는 경우에 사용 됩니다. |
| 비활성| boolean| 사용자가 수락 하지만 적극적으로 재생 되지 않으면 세션을 포함 하려면 true입니다. 사용자가 "ready" 하지만 "비활성" 세션 비활성으로 계산 합니다. |
| 개인| boolean| 개인 세션을 포함 하려면 true입니다. 이 매개 변수는 고유한 세션을 쿼리할 때 또는 특정 사용자의 세션 서버-투-서버를 쿼리 하는 경우에 사용 됩니다. |
| visibility| 문자열| 세션에 대 한 표시 여부 상태입니다. 으로 정의 된 가능한 값은 <b>MultiplayerSessionVisibility</b>합니다. 이 매개 변수를 "열린"로 하는 경우 쿼리만 열려 있는 세션을 포함 해야 합니다. 에 "개인"으로 설정 되어 있으면 합니다 <i>개인</i> 매개 변수를 설정 해야 true로 합니다. |
| version| 32 비트 부호 있는 정수| 포함 되어야 하는 최대 세션 버전입니다. 예를 들어 2의 주요 세션 버전과 해당 세션을 지정 하는 값이 2 또는 작은 포함 되어야 합니다. 버전 번호를 요청 계약 버전 mod 100 보다 작거나 여야 합니다. |
| Take| 32 비트 부호 있는 정수| 검색할 세션의 최대 수입니다. 이 번호는 0과 100 사이 여야 합니다. |


설정 중 하나 *사설* 또는 *예약* true 세션에 대 한 서버 수준 액세스를 해야 합니다. 또는 이러한 설정은 URI에 XUID 필터와 일치 하는 호출자의 XUID 클레임 필요 합니다. 그렇지 않은 경우 HTTP/403 상태 코드가 반환 됩니다 이러한 세션을 실제로 존재 하는지 여부.

<a id="ID4EBF"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4EIF"></a>


## <a name="request-body"></a>요청 본문

사용자의 "즐겨찾기" 소셜 그래프에 대 한 모든 작업을 위해 요청 본문은 다음과 같습니다.


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

핸들은 핸들 구조체의 배열로 각 구조에 포함 된 고유 ID를 사용 하 여 검색 됩니다.


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


##### <a name="parent"></a>Parent

[/handles/query](uri-handlesquery.md)
