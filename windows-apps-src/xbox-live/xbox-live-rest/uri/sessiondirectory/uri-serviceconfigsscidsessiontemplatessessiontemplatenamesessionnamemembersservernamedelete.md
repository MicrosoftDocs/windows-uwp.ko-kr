---
title: DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name})
assetID: 39c921d1-a166-74b9-fcbc-ea3c0c58cc40
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservernamedelete.html
author: KevinAsgari
description: " DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 12feca7761faa441023f2f242903d4fec2f0284d
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4532307"
---
# <a name="delete-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameserversserver-name"></a>DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name})
세션에서 지정된 된 서버를 제거합니다.

> [!IMPORTANT]
> 이 URI 메서드에 필요 X Xbl-계약 버전의 헤더 요소: 104/105 또는 나중에 모든 요청.

  * [URI 매개 변수](#ID4ET)
  * [HTTP 상태 코드](#ID4E5)
  * [요청 본문](#ID4EFB)
  * [응답 본문](#ID4EOB)

<a id="ID4ET"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| 서비스 안내| GUID| 서비스 구성 id (서비스 안내)입니다. 1 부 세션 식별자입니다.|
| sessionTemplateName| string| 현재 인스턴스 세션 서식 파일의 이름입니다. 2 부 세션 식별자입니다.|
| 세션 이름| GUID| 세션의 고유 ID입니다. 3 부 세션 식별자입니다.|

<a id="ID4E5"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4EFB"></a>


## <a name="request-body"></a>요청 본문
[MultiplayerSessionRequest (JSON)](../../json/json-multiplayersessionrequest.md)에서 요청 구조를 참조 하세요.  
<a id="ID4EOB"></a>


## <a name="response-body"></a>응답 본문
[MultiplayerSession (JSON)](../../json/json-multiplayersession.md)에 대 한 응답 구조를 참조 하세요.  
<a id="ID4E1B"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4E3B"></a>


##### <a name="parent"></a>부모

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservername.md)
