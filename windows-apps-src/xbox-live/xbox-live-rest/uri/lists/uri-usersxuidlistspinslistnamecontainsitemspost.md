---
title: POST /users/xuid(xuid)/lists/PINS/{listname}/ContainsItems
assetID: 86ee6d1a-fb1f-b918-f605-a9b494c0e787
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnamecontainsitemspost.html
author: KevinAsgari
description: " POST /users/xuid(xuid)/lists/PINS/{listname}/ContainsItems"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d1bacb3cf6a97cf72b03df10096f3ebf779b045b
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4427715"
---
# <a name="post-usersxuidxuidlistspinslistnamecontainsitems"></a>POST /users/xuid(xuid)/lists/PINS/{listname}/ContainsItems
전체 목록을 검색 하지 않고 목록에 항목 (itemId 지정한) 집합이 포함 되어 있는지 여부를 결정 합니다. 이러한 Uri에 대 한 도메인은 `eplists.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EAB)
  * [쿼리 문자열 매개 변수](#ID4EJC)
  * [요청 본문](#ID4EUC)
  * [HTTP 상태 코드](#ID4E6C)
  * [필요한 요청 헤더](#ID4EVAAC)
  * [응답 본문](#ID4ELCAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명 
 
이 끝점에 호출자를가 하나 이상의 항목 목록을 가져오고 자체에 대 한 검색 실제로 하지 않고 지정 된 목록에에서 존재 하는 경우를 확인할 수 있습니다. 일련의 항목은 itemId (또는 공급자/providerId 콤보)으로 식별 되며 반환 데이터 각 항목 목록에 포함 되어 있는지 여부는 부울 true 또는 false ndicating의으로 전달 되는 데이터입니다. 
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수 
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| XUID| string| 사용자의 XUID 합니다.| 
| listname| string| 조작 목록 이름입니다.| 
  
<a id="ID4EJC"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수 
 
쿼리 매개 변수는 지원 되지 않습니다.
  
<a id="ID4EUC"></a>

 
## <a name="request-body"></a>요청 본문 
 

```cpp
{
  "Items":
  [{"ItemId":  "ed591a0c-dde3-563f-99af-530278a3c402",
    "ProviderId":  null,
    "Provider": null
  }]
}


    
```

  
<a id="ID4E6C"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드 
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 200| 확인 | 요청이 성공적으로 완료 합니다. 응답 본문 (GET)에 대 한 요청 된 리소스를 포함 해야 합니다. POST 및 PUT 요청 (예: 목록 버전, 개수) 최신 목록을 메타 데이터를 받게 됩니다.| 
| 201| 생성 | 새 목록을 작성 되었습니다. 이 목록에 초기 삽입에 반환 됩니다. 응답 목록에 최신 메타 데이터를 포함 하 고 위치 헤더 목록에 대 한 URI를 포함 합니다.| 
| 304| 수정 되지 않음| 반환 된 가져옵니다. 클라이언트에 최신 버전의 목록 나타냅니다. 서비스는 <b>헤더를 목록 버전</b> 값과 비교 합니다. 같으면는 304 데이터가 없는 반환 가져옵니다. | 
| 400| 잘못 된 요청 | 요청이 잘못 되었습니다. 잘못 된 값 또는 URI 또는 쿼리 문자열 매개 변수의 형식을 사용할 수 없습니다. 필요한 매개 변수를 누락 결과 수 또는 데이터 값 또는 요청 누락 되거나 잘못 된 버전의 API 표시 합니다. <b>XBL 계약 버전 X</b> 헤더를 참조 하세요. | 
| 401| 권한 없음 | 필요한 사용자 인증을 요청 합니다.| 
| 403| 금지 | 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 404| 찾을 수 없음 | 호출자는 리소스에 액세스할 수 없습니다. 이러한 목록이 이미 만들었다고 나타냅니다.| 
| 412| 전제 조건 실패 | 버전의 목록 변경 하 고 수정 요청이 실패 했습니다 함을 나타냅니다. 요청은 수정 삽입, 업데이트 또는 제거 됩니다. 서비스는 <b>헤더 목록 버전</b> 을 확인합니다. 현재 버전의 목록 일치 하지 않으면, 다음 작업이 실패 하 고 (현재 버전 포함)는 현재 목록 메타 데이터와 함께 반환 됩니다. | 
| 500| 내부 서버 오류 | 서비스 요청 서버 쪽 오류로 인해 거부 됩니다.| 
| 501| 구현 되지 않음 | 호출자에 게 요청 URI를 서버에 구현 되지 않았습니다. (현재 때 요청 사용만 대상인 허용 목록에 포함 되지 않은 목록 이름.)| 
| 503| 사용할 수 없는 서비스 | 서버가 과도 한 부하도 인해 일반적으로 요청을 거부 됩니다. 된 지연 시간 후 (참조를 <b>다시 시도 후</b> 헤더), 요청을 다시 시도할 수 있습니다. | 
  
<a id="ID4EVAAC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 인증 하 고 요청을 인증 하는 데 STS 토큰이 포함 되어 있습니다. XSTS 서비스에서 토큰 이어야 하 고는 XUID 클레임 중 하나로 포함 해야 합니다. | 
| XBL 계약 버전 X| 요청 된 (양의 정수) 되 고 API 버전을 지정 합니다. 버전 2의 핀 지원 합니다. 이 헤더 없거나 서비스는 400 – 반환 하는 다음 값 지원 되지 않습니다 "지원 되지 않는 없거나 계약 버전 헤더" 상태 설명에 잘못 된 요청 합니다. | 
| Content-Type| 요청/응답 본문의 콘텐츠를 xml 또는 json 수 있는지 여부를 지정 합니다. 지원 되는 값은 "application/json" 및 "application/xml"| 
| If-Match| 이 헤더 수정 요청을 만들 때 목록의 현재 버전 번호를 포함 해야 합니다. 요청 사용 PUT, POST, 수정 하거나 동사를 삭제할. 버전은 "" 헤더에서 현재 버전의 목록 일치 하지 않으면, HTTP 412 요청이 거부 됩니다 – 전제 조건 반환 코드에 실패 했습니다. (선택 사항) 또한 사용할 수 가져옵니다에 대 한 수정 되지 코드가 반환-현재 및 전달된 버전 현재 목록 버전 다음 목록 데이터 및 HTTP 304와 일치 하는 경우. | 
  
<a id="ID4ELCAC"></a>

 
## <a name="response-body"></a>응답 본문 
 
성공적으로 호출 하는 경우 항목이 목록에 인지 여부를 지정 하는 각 항목에 대 한 부울와 함께 항목 목록이 반환 됩니다. 
 
<a id="ID4EVCAC"></a>

 
### <a name="sample-response"></a>예제 응답 
 

```cpp
{
  "ContainedItems":
  [{"Contained": "true",
    "Item":
   {"ItemId":  "ed591a0c-dde3-563f-99af-530278a3c402",
    "ProviderId": null,
    "Provider": null
   }
  }]
}


      
```

   
<a id="ID4EBDAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EDDAC"></a>

 
##### <a name="parent"></a>부모 

[URI(Universal Resource Identifier) 참조](../atoc-xboxlivews-reference-uris.md)

   