---
title: DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name})
assetID: 39c921d1-a166-74b9-fcbc-ea3c0c58cc40
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservernamedelete.html
description: " DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 65da52284d49d4d9384685d073f13bd93b10a30b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658318"
---
# <a name="delete-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameserversserver-name"></a>DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name})
세션에서 지정된 된 서버를 제거합니다.

> [!IMPORTANT]
> 이 URI 메서드를 헤더 요소의 X Xbl-계약 버전이 필요합니다. 104/105 또는 나중에 모든 요청 합니다.

  * [URI 매개 변수](#ID4ET)
  * [HTTP 상태 코드](#ID4E5)
  * [요청 본문](#ID4EFB)
  * [응답 본문](#ID4EOB)

<a id="ID4ET"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- | --- |
| scid| GUID| (서비스 안내) 식별자를 구성 하는 서비스입니다. 1 부 세션 식별자입니다.|
| sessionTemplateName| 문자열| 세션 템플릿의 현재 인스턴스의 이름입니다. 2 부를 선택 하면 세션 식별자입니다.|
| sessionName| GUID| 세션의 고유 ID입니다. 3 부 세션 식별자입니다.|

<a id="ID4E5"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4EFB"></a>


## <a name="request-body"></a>요청 본문
요청 구조를 볼 [MultiplayerSessionRequest (JSON)](../../json/json-multiplayersessionrequest.md)합니다.  
<a id="ID4EOB"></a>


## <a name="response-body"></a>응답 본문
응답 구조를 볼 [MultiplayerSession (JSON)](../../json/json-multiplayersession.md)합니다.  
<a id="ID4E1B"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4E3B"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservername.md)
