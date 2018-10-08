---
title: DELETE (/users/xuid({xuid})/inbox/{messageId})
assetID: c54eede3-3e3b-2cbe-1be9-8bf3a48171bc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxmessageiddelete.html
author: KevinAsgari
description: " DELETE (/users/xuid({xuid})/inbox/{messageId})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e98608f8329407ccb728abb9490eeb341e72aec5
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4416169"
---
# <a name="delete-usersxuidxuidinboxmessageid"></a>DELETE (/users/xuid({xuid})/inbox/{messageId})
사용자의 받은 편지함에서 사용자 메시지를 삭제합니다. 이러한 Uri에 대 한 도메인은 `msg.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4ECB)
  * [권한 부여](#ID4EPB)
  * [요청 본문](#ID4E1B)
  * [HTTP 상태 코드](#ID4EHC)
  * [JavaScript Object Notation (JSON) 응답](#ID4EAE)
  * [리소스에 대 한 개인 정보 설정의 효과](#ID4EYF)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명 
 
삭제 작업 idempotent입니다.
 
이 API는 지원만 콘텐츠 형식은 "application/json", 각 호출의 HTTP 헤더에 필요한 합니다. 
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수 
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid | 64 비트의 부호 없는 정수 | Xbox 사용자 ID (XUID) 요청 하 고 있는 플레이어의 합니다. | 
| messageId | string [50] | 검색 되거나 삭제 되는 메시지의 ID입니다. | 
  
<a id="ID4EPB"></a>

 
## <a name="authorization"></a>권한 부여 
 
사용자는 사용자가 메시지를 삭제 하 고 있어야 합니다.
  
<a id="ID4E1B"></a>

 
## <a name="request-body"></a>요청 본문 
 
개체가이 요청 본문에 전송 됩니다.
  
<a id="ID4EHC"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드 
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| 코드| 설명| 
| --- | --- | --- | --- | --- | 
| 204| 성공 합니다.| 
| 403| XUID는 변환할 수 없으므로 또는 유효한 XUID 클레임을 찾을 수 없습니다.| 
| 404| URI의 메시지 ID를 구문 분석할 수 없는 또는 XUID URI에 없습니다.| 
| 500| 일반 서버 쪽 오류가 발생 했습니다.| 
  
<a id="ID4EAE"></a>

 
## <a name="javascript-object-notation-json-response"></a>JavaScript Object Notation (JSON) 응답 
 
오류가 발생 하는 경우 서비스는 서비스의 환경에서 값을 포함할 수 있는 errorResponse 개체를 반환할 수 있습니다.
 
| 속성| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| errorSource| string| 에 오류가 발생 나타냅니다.| 
| 오류 코드| int| (Null 일 수) 오류와 관련 된 숫자 코드입니다.| 
| errorMessage| string| 오류 세부 정보를 표시 하도록 구성 된 경우에 자세히 설명 합니다.| 
  
<a id="ID4EYF"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>리소스에 대 한 개인 정보 설정의 효과 
 
만 사용자 메시지를 직접 삭제할 수 있습니다. 
  
<a id="ID4EDG"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EFG"></a>

 
##### <a name="parent"></a>부모  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)

  
<a id="ID4ETG"></a>

 
##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>[표준 HTTP 상태 코드](../../additional/httpstatuscodes.md) 참조

   