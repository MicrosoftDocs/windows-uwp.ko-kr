---
title: GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName})
assetID: 81139619-dc27-1601-30ba-08f6c45aaaca
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenameget.html
author: KevinAsgari
description: " GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName})"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 25a2e4cd0ec13baea6ea14c44b277a8d14e1b3eb
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5812991"
---
# <a name="get-serviceconfigsscidsessiontemplatessessiontemplatename"></a>GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName})
세션 템플릿 이름으로 이루어진 집합을 가져옵니다.

> [!IMPORTANT]
> 이 URI 메서드에 필요 Xbl 계약 버전 X의 헤더 요소: 104/105 또는 나중에 모든 요청.

  * [URI 매개 변수](#ID4ET)
  * [HTTP 상태 코드](#ID4E5)
  * [요청 본문](#ID4EFB)
  * [응답 본문](#ID4EQB)

<a id="ID4ET"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| 서비스 안내| GUID| 서비스 구성 식별자 (서비스 안내)입니다. 파트 1 세션의 id.|
| sessionTemplateName| string| 현재 인스턴스의 세션 템플릿 이름입니다. 파트 2 세션의 id. |

<a id="ID4E5"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4EFB"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청 본문에 전송 됩니다.

<a id="ID4EQB"></a>


## <a name="response-body"></a>응답 본문


```cpp
{
    "results": [ {
            "xuid": "9876",    // If the session was found from a xuid, that xuid.
            "startTime": "2009-06-15T13:45:30.0900000Z",
            "sessionRef": {
                "scid": "foo",
                "templateName": "bar",
                "name": "session-seven"
            },
            "accepted": 4,    // Approximate number of non-reserved members.
            "status": "active",    // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
            "visibility": "open",    // or "private", "visible", or "full"
            "joinRestriction": "local",    // or "followed". Only present if 'visibility' is "open" or "full" and the session has a join restriction.
            "myTurn": true,    // Not present is the same as 'false'. Only present if the session was found using a xuid.
            "keywords": [ "one", "two" ]
        }
    ]
}

```


<a id="ID4EZB"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4E2B"></a>


##### <a name="parent"></a>부모

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}](uri-serviceconfigsscidsessiontemplatessessiontemplatename.md)
