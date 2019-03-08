---
title: 세션 디렉터리 URI
assetID: e3ba951d-b21f-0014-c358-2603d549d118
permalink: en-us/docs/xboxlive/rest/atoc-reference-sessiondirectory.html
description: " 세션 디렉터리 URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9492ff3272af830404a546c9b01d62178adbac96
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651198"
---
# <a name="session-directory-uris"></a>세션 디렉터리 URI

이 섹션에서는 Xbox Live 서비스에 대 한 다중 접속 세션 디렉터리 (MPSD)에서 유니버설 리소스 식별자 (URI) 주소 및 관련된 하이퍼텍스트 전송 프로토콜 (HTTP) 메서드에 대 한 세부 정보를 제공 합니다.


> [!NOTE] 
> Xbox 360, Windows Phone 장치 또는 Xbox.com에서 실행 되는 게임 타이틀만 세션 디렉터리 Uri를 사용할 수 있습니다.  


  * [도메인](#ID4EUB)
  * [서비스 버전](#ID4EZB)
  * [시스템 개체 및 속성](#ID4EAC)
  * [핸들](#ID4EBE)

<a id="ID4EUB"></a>


## <a name="domain"></a>도메인
sessiondirectory.xboxlive.com  
<a id="ID4EZB"></a>


## <a name="service-version"></a>서비스 버전

다음 REST Uri의 호출자에 게 전달 해야 값 104/105 이상 X-Xbl-계약-버전, 엔터테인먼트 검색 서비스 (EDS) 서비스 버전을 지정 하는 HTTP 헤더에 대 한 합니다.

<a id="ID4EAC"></a>


## <a name="system-objects-and-properties"></a>시스템 개체 및 속성

해당 세션 및 템플릿 구성에 대 한는 MPSD 디렉터리를 적용 하 고 해석 하는 고정된 스키마를 사용 하 여 따르는 세션 JSON 개체의 수를 사용 합니다. 다양 한 세션 디렉터리 Uri에서 지원 되는 메서드를 호출 하는 동안 이러한 개체의 유효성을 검사 하 고 병합을 지원 되는 스키마를 기반으로 합니다. 멀티 플레이 구성과 관련 된 것은 기본 JSON 개체입니다.

   *  [MultiplayerActivityDetails (JSON)](../../json/json-multiplayeractivitydetails.md)
   *  [MultiplayerSession (JSON)](../../json/json-multiplayersession.md)
   *  [MultiplayerSessionReference (JSON)](../../json/json-multiplayersessionreference.md)
   *  [MultiplayerSessionRequest (JSON)](../../json/json-multiplayersessionrequest.md)


게임으로 우려 하는 연결 된 JSON 개체입니다.

   *  [GameMessage (JSON)](../../json/json-gamemessage.md)
   *  [GameResult (JSON)](../../json/json-gameresult.md)
   *  [GameSession (JSON)](../../json/json-gamesession.md)
   *  [GameSessionSummary (JSON)](../../json/json-gamesessionsummary.md)


<a id="ID4EBE"></a>


## <a name="handles"></a>핸들

2015 멀티 플레이어 게임만, 세션 세션 핸들을 통해 액세스할 수 있습니다. 여러 Uri 처리를 지원 하기 위해 기능을 제공 하도록 추가 되었습니다.  
<a id="ID4EFE"></a>


## <a name="in-this-section"></a>이 섹션의 내용

[/handles](uri-handles.md)

&nbsp;&nbsp;Xbox One 대시보드 사용자 환경에서 표시 하 고 필요한 경우 세션 멤버를 초대 하는 사용자의 현재 활동에 대 한 세션을 설정 하는 POST 작업을 지원 합니다.

[/handles/{handleId}](uri-handleshandleid.md)

&nbsp;&nbsp;식별자로 지정 하는 세션 핸들에 대 한 DELETE 및 GET 작업을 지원 합니다.

[/handles/{handleId}/session](uri-handleshandleidsession.md)

&nbsp;&nbsp;PUT 및 GET 작업 세션에 대 한 핸들 역참조를 사용 하 여 지원 합니다.

[/handles/query](uri-handlesquery.md)

&nbsp;&nbsp;세션 핸들에 대 한 쿼리를 만드는 POST 작업을 지원 합니다.

[/serviceconfigs/{scid}/batch](uri-serviceconfigsscidbatch.md)

&nbsp;&nbsp;서비스 구성 식별자 수준에서 일괄 처리 쿼리에 대 한 POST 작업을 지원합니다.

[/serviceconfigs/{scid}/sessions](uri-serviceconfigsscidsessions.md)

&nbsp;&nbsp;세션 문서 집합을 검색 하는 GET 작업을 지원 합니다.

[/serviceconfigs/{scid}/sessiontemplates](uri-serviceconfigsscidsessiontemplates.md)

&nbsp;&nbsp;MPSD 세션 템플릿 집합을 검색 하는 GET 작업을 지원 합니다.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}](uri-serviceconfigsscidsessiontemplatessessiontemplatename.md)

&nbsp;&nbsp;세션 템플릿 이름 집합을 검색 하는 GET 작업을 지원 합니다.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch](uri-serviceconfigscidsessiontemplatessessiontemplatenamebatch.md)

&nbsp;&nbsp;세션 템플릿 수준에서 일괄 처리 쿼리를 만드는 POST 작업을 지원 합니다.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessions.md)

&nbsp;&nbsp;지정 된 템플릿 이름의 세션 템플릿 집합을 검색 하는 GET 작업을 지원 합니다.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)

&nbsp;&nbsp;PUT 및 GET 작업을 만들고 세션 검색을 지원 합니다.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindex.md)

&nbsp;&nbsp;지정 된 세션 구성원을 제거 하려면 삭제 작업을 지원 합니다.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme.md)

&nbsp;&nbsp;세션 구성원을 제거 하려면 삭제 작업을 지원 합니다.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservername.md)

&nbsp;&nbsp;세션의 지정 된 서버를 제거 하려면 삭제 작업을 지원 합니다.

<a id="ID4ESF"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EUF"></a>

   [결혼 정보 회사 연결 Uri](../matchtickets/atoc-reference-matchtickets.md)


<a id="ID4E1F"></a>


##### <a name="parent"></a>Parent

[유니버설 리소스 식별자 (URI) 참조](../atoc-xboxlivews-reference-uris.md)
