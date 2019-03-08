---
title: DELETE (/users/xuid({xuid})/inbox/{messageId})
assetID: c54eede3-3e3b-2cbe-1be9-8bf3a48171bc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxmessageiddelete.html
description: " DELETE (/users/xuid({xuid})/inbox/{messageId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 80ec2a462648177cc6bfc846b9c84278821b0e5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594108"
---
# <a name="delete-usersxuidxuidinboxmessageid"></a>DELETE (/users/xuid({xuid})/inbox/{messageId})
사용자의 받은 편지함에서 사용자 메시지를 삭제합니다. 이러한 Uri에 대 한 도메인은 `msg.xboxlive.com`합니다.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4ECB)
  * [권한 부여](#ID4EPB)
  * [요청 본문](#ID4E1B)
  * [HTTP 상태 코드](#ID4EHC)
  * [JavaScript 개체 표기법 (JSON) 응답](#ID4EAE)
  * [리소스에 대 한 개인 정보 설정의 효과](#ID4EYF)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명 
 
삭제 작업은 idempotent입니다.
 
이 API는 지원만 content-type은 "application/json"이 각 호출의 HTTP 헤더에 필요 합니다. 
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수 
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid | 부호 없는 64 비트 정수 | Xbox 사용자 ID (XUID)은 요청을 수행 하는 플레이어입니다. | 
| messageId | string[50] | 검색 또는 삭제 되는 메시지의 ID입니다. | 
  
<a id="ID4EPB"></a>

 
## <a name="authorization"></a>권한 부여 
 
사용자 클레임을 사용자 메시지를 삭제 해야 합니다.
  
<a id="ID4E1B"></a>

 
## <a name="request-body"></a>요청 본문 
 
개체가이 요청의 본문에 전송 됩니다.
  
<a id="ID4EHC"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드 
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 설명| 
| --- | --- | --- | --- | --- | 
| 204| 성공 했습니다.| 
| 403| XUID 변환할 수 없는 또는 유효한 XUID 클레임을 찾을 수 없습니다.| 
| 404| URI에서 메시지 ID를 구문 분석할 수 없습니다 또는 XUID URI에 없습니다.| 
| 500| 일반 서버 쪽 오류입니다.| 
  
<a id="ID4EAE"></a>

 
## <a name="javascript-object-notation-json-response"></a>JavaScript 개체 표기법 (JSON) 응답 
 
오류가 하는 경우 서비스는 서비스의 환경에서 값을 포함할 수 있는 errorResponse 개체를 반환할 수 있습니다.
 
| 속성| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| errorSource| 문자열| 오류가 발생 한 위치를 나타냅니다.| 
| errorCode| int| (Null 일 수) 오류와 연결 된 숫자 코드입니다.| 
| errorMessage| 문자열| 세부 정보를 표시 하도록 구성 하는 경우 오류 세부 정보입니다.| 
  
<a id="ID4EYF"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>리소스에 대 한 개인 정보 설정의 효과 
 
만 고유한 사용자 메시지를 삭제할 수 있습니다. 
  
<a id="ID4EDG"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EFG"></a>

 
##### <a name="parent"></a>Parent  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)

  
<a id="ID4ETG"></a>

 
##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>참조 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)

   