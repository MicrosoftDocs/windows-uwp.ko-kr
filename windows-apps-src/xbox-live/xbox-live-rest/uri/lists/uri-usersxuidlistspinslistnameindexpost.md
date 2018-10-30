---
title: POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
assetID: df61be42-c229-7408-5e4c-dbf4ae95b52b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameindexpost.html
author: KevinAsgari
description: " POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9e1ca061505efe06622302b11a9cd25cd35e93d7
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5764164"
---
# <a name="post-usersxuidxuidlistspinslistnameindexindexinsertindexinsertindex"></a>POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
목록에서 항목을 목록 내에서 다른 위치로 이동합니다. 이러한 Uri에 대 한 도메인은 `eplists.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EEB)
  * [쿼리 문자열 매개 변수](#ID4EWC)
  * [요청 본문](#ID4EVD)
  * [HTTP 상태 코드](#ID4EEE)
  * [필요한 요청 헤더](#ID4E1BAC)
  * [응답 본문](#ID4EQDAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명 
 
이 호출은 클라이언트가 항목의 목록에서 단일 작업을 다른 인덱스로 쉽게 이동할 수 있도록 제공 됩니다. 한 번에 하나의 항목만 이동할 수 있습니다. 이동할 수 있는 항목의 인덱스 존재 하지 않는 경우 HTTP 400 반환 됩니다. 삽입 지점에 대 한 인덱스는 표준 insert 동일한 규칙을 따릅니다. 기본값은 0-목록 맨 및 목록의 항목 수가 보다 큰 모든 숫자 끝 목록에 항목을 삽입 다시 합니다. 마찬가지로 insertIndex에 대 한 "최종"를 전달 하 여 목록의 끝을 나타낼 수 있습니다. 
 
이 호출 항목 itemId (또는 공급자/providerId 콤보)가 이동할 수를 확인할 수도 있습니다. 이 경우는 itemId 요청의 본문에 전달 해야 하 고 목록에서 기존 항목을 일치 해야 합니다. HTTP 400 오류를 설명 합니다; IndexOutOfRange로 반환 됩니다 하지 않는 경우 이 특수 버전 호출의 "-1" 항목의 인덱스도 이동할 사용 합니다. 
 
이 호출 해야는 "하는 경우-일치: versionNumber" 헤더 요청에 항목을 지정 하는 경우에 의해 포함 될 인덱스입니다. ItemId 이동 되는 항목을 나타내는 사용 하는 경우 "If 일치" 헤더는 선택 사항. 있는 경우 "If 일치" 헤더 항상 확인할 수 있습니다. "일치 경우" 헤더는 versionNumber 목록의 현재 버전 번호는 하는 경우 그 및 되지 않는 포함 (필수), 현재와 일치 하지 않는 또는 목록 버전 번호를 다음 HTTP 412 – 실패 한 상태 코드를 사전 조건 반환 되 고 응답 본문의 현재 버전 번호를 포함 하는 목록 최신 메타 데이터를 포함 됩니다 . 서로에서 무시 되는 다른 클라이언트에서 업데이트를 수행 됩니다. 
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수 
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| XUID| string| 사용자의 XUID 합니다.| 
| listname| string| 조작 목록의 이름입니다.| 
| 인덱스| string| 이동할 수 있는 항목의 현재 인덱스를 지정 합니다. 인덱스 값이 0 또는 양의 정수 인 경우이 항목의 현재 인덱스를 나타냅니다 및 호출의 요청 본문 비어 있어야 합니다. 그러나 인덱스 값은 "-1"을 이동할 항목 ItemId 또는 공급자/ProviderID 호출의 요청 본문에 의해 지정 되어야 합니다.| 
  
<a id="ID4EWC"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수 
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| insertIndex| string| 목록 항목을 삽입 위치를 지정 합니다. 허용 되는 값은 0, 양의 정수 및 "최종"입니다. 현재 목록 끝에 항목을 배치 하는 "최종". 지정 된 값 목록 끝에 있는 경우 항목 목록의 끝에 삽입 됩니다. | 
  
<a id="ID4EVD"></a>

 
## <a name="request-body"></a>요청 본문 
 
요청 본문 itemId 공급자/ProviderId의 이동 하려면 항목을 지정 하는 경우에 전송 됩니다.
 
<a id="ID4E6D"></a>

  
샘플 요청 

```cpp
{
  "Item":
  {
    "ItemId":  "ed591a0c-dde3-563f-99af-530278a3c402",
    "ProviderId":  null,
    "Provider": null
  }
}
    
```

  
<a id="ID4EEE"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드 
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스에 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인 | 요청이 성공적으로 완료 합니다. 응답 본문 (GET)에 대 한 요청 된 리소스를 포함 해야 합니다. POST 및 PUT 요청 (예: 목록 버전, 개수) 최신 목록을 메타 데이터를 받게 됩니다.| 
| 201| 생성 | 새 목록 작성 되었습니다. 목록에 초기 삽입 반환 됩니다. 응답 목록에 최신 메타 데이터를 포함 하 고 위치 헤더 목록에 대 한 URI를 포함 합니다.| 
| 304| 수정 되지 않음| 반환 된 가져옵니다. 클라이언트에 최신 버전의 목록 나타냅니다. 서비스 목록 버전 <b>일치 경우</b> 헤더에 있는 값을 비교합니다. 같으면는 304 데이터가 없는 반환 가져옵니다. | 
| 400| 잘못 된 요청 | 요청이 잘못 되었습니다. 잘못 된 값 또는 URI 또는 쿼리 문자열 매개 변수의 형식 수 있습니다. 누락 된 필수 매개 변수의 결과 수 또는 데이터 값 이나 요청 누락 되거나 잘못 된 버전의 API 표시 합니다. <b>XBL 계약 버전 X</b> 헤더를 참조 하세요. | 
| 401| 권한 없음 | 필요한 사용자 인증을 요청 합니다.| 
| 403| 사용할 수 없음 | 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 404| 찾을 수 없습니다. | 호출자는 리소스에 액세스할 수 없습니다. 이러한 목록이 이미 만들었다고 나타냅니다.| 
| 412| 전제 조건 실패 | 버전의 목록 변경 요청은 수정에 실패 한 되었음을 나타냅니다. 수정 요청 삽입, 업데이트 또는 제거 됩니다. 서비스 목록 버전에 대 한 <b>경우 일치</b> 헤더를 확인합니다. 현재 버전의 목록, 일치 하지 않으면 다음 작업이 실패 하 고 (현재 버전 포함)는 현재 목록 메타 데이터와 함께 반환 됩니다. | 
| 500| 내부 서버 오류 | 서비스 요청 서버 쪽 오류로 인해 거부 됩니다.| 
| 501| 구현 되지 않음 | 호출자에 게 요청 URI를 서버에 구현 되지 않았습니다. (현재 때 요청을 사용 하는 전용 대상인 허용 목록에 포함 되지 않은 목록 이름.)| 
| 503| 사용할 수 없는 서비스 | 서버가 과도 한 부하도 인해 일반적으로 요청을 거부 됩니다. 된 지연 시간 후 (참조를 <b>다시 시도 후</b> 헤더), 요청을 다시 시도할 수 있습니다. | 
  
<a id="ID4E1BAC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 인증 하 고 요청을 인증 하는 데 STS 토큰이 포함 되어 있습니다. XSTS 서비스에서 토큰 이어야 하 고 클레임 중 하나로 XUID를 포함 해야 합니다. | 
| XBL 계약 버전 X| 요청 된 (양의 정수) 되 고 API 버전을 지정 합니다. 버전 2의 핀 지원 합니다. 이 헤더 없거나 값 서비스 400-반환는 지원 되지 않습니다 "지원 되지 않는 없거나 계약 버전 헤더" 상태 설명에 잘못 된 요청 합니다.| 
| Content-Type| 요청/응답 본문의 콘텐츠를 json 또는 xml 수 있는지 여부를 지정 합니다. 지원 되는 값은 "application/json" 및 "application/xml"| 
| If-Match| 이 헤더 수정 요청을 만들 때 목록의 현재 버전 번호를 포함 해야 합니다. 요청 사용 PUT, POST, 수정 하거나 동사를 삭제할. 버전은 "" 헤더에서 현재 버전의 목록 일치 하지 않으면, HTTP 412 요청이 거부 됩니다 – 전제 조건 반환 코드에 실패 했습니다. (선택 사항) 사용할 수도 있습니다을 가져온에 대 한 필드에 현재 및 전달된 버전 일치 현재 목록 버전 다음 목록 데이터 없음 HTTP 304-수정 되지 코드가 반환 하는 경우. | 
  
<a id="ID4EQDAC"></a>

 
## <a name="response-body"></a>응답 본문 
 
호출 되 면 서비스 목록에 대 한 최신 메타 데이터를 반환 합니다. 
 
<a id="ID4E1DAC"></a>

 
### <a name="sample-response"></a>예제 응답 
 

```cpp
{ 
  "ListVersion":  1,
  "ListCount":  1,
  "MaxListSize": 200,
  "AllowDuplicates": "true",
  "AccessSetting":  "OwnerOnly"
}

      
```

   
<a id="ID4EIEAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EKEAC"></a>

 
##### <a name="parent"></a>부모 

[/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}](uri-usersxuidlistspinslistnameindex.md)

   