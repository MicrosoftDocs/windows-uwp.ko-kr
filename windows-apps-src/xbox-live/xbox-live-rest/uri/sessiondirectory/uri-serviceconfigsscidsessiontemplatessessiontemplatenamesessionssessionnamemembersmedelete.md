---
title: DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me)
assetID: aa5de623-7787-a47c-b7e4-305693b9fe35
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersmedelete.html
author: KevinAsgari
description: " DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e67b48653b7cccb3a9ff30bc833df0f9d3189638
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5709563"
---
# <a name="delete-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme"></a>DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me)
세션의 구성원을 제거 합니다.

> [!IMPORTANT]
> 이 URI 메서드에 필요 Xbl 계약 버전 X의 헤더 요소: 104/105 또는 나중에 모든 요청.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4E3)
  * [HTTP 상태 코드](#ID4EHB)
  * [요청 본문](#ID4ENB)
  * [응답 본문](#ID4EYB)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명
모든 세션 멤버 리소스 작업에는 Xbox 사용자 ID (XUID) 인증이 필요합니다.  
<a id="ID4E3"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| 서비스 안내| GUID| 서비스 구성 id (서비스 안내)입니다. 파트 1 세션 식별자입니다.|
| sessionTemplateName| string| 현재 인스턴스의 세션 템플릿 이름입니다. 파트 2 세션 식별자입니다.|
| 세션 이름| GUID| 세션의 고유 ID입니다. 파트 3 세션 식별자입니다.|

<a id="ID4EHB"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4ENB"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청 본문에 전송 됩니다.

<a id="ID4EYB"></a>


## <a name="response-body"></a>응답 본문
[MultiplayerSession (JSON)](../../json/json-multiplayersession.md)에서 응답 구조를 참조 하세요.  
<a id="ID4EBC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EDC"></a>


##### <a name="parent"></a>부모

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme.md)
