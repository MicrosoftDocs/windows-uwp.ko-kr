---
title: POST /users/xuid(xuid)/lists/PINS/{listname}/ContainsItems
assetID: 86ee6d1a-fb1f-b918-f605-a9b494c0e787
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnamecontainsitemspost.html
description: " POST /users/xuid(xuid)/lists/PINS/{listname}/ContainsItems"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c18bf70773de60d4c831d4be891255f98750efa8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636878"
---
# <a name="post-usersxuidxuidlistspinslistnamecontainsitems"></a>POST /users/xuid(xuid)/lists/PINS/{listname}/ContainsItems
전체 목록을 검색 하지 않고 (itemId에 의해 지정 됨) 하는 항목 집합 목록에 포함 되는지 여부를 결정 합니다. 이러한 Uri에 대 한 도메인은 `eplists.xboxlive.com`합니다.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EAB)
  * [쿼리 문자열 매개 변수](#ID4EJC)
  * [요청 본문](#ID4EUC)
  * [HTTP 상태 코드](#ID4E6C)
  * [필요한 요청 헤더](#ID4EVAAC)
  * [응답 본문](#ID4ELCAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명 
 
이 끝점에는 호출자가 실제로 목록 가져오기 및 자체에 대 한 검색 하지 않고 지정된 된 목록에서 하나 이상의 항목이 존재 여부 확인을 수 있습니다. 항목 집합의 itemId (또는 공급자/providerId 콤보)으로 식별 됩니다 이며 반환 데이터 목록의 각 항목을 포함 하는지 여부를 ndicating true 또는 false의 부울 값으로 전달 되는 데이터입니다. 
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수 
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| XUID| 문자열| 사용자의 XUID 합니다.| 
| listname| 문자열| 조작 목록의 이름입니다.| 
  
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
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 200| 확인 | 요청이 완료 되었습니다. 응답 본문 (GET)에 대 한 요청된 된 리소스를 포함 해야 합니다. POST 및 PUT 요청 (목록 버전, count 등)의 최신 목록 메타 데이터를 받게 됩니다.| 
| 201| 만든 날짜 | 새 목록을 만들었습니다. 목록에 초기 삽입 시 반환 됩니다. 응답은 목록에 최신 메타 데이터를 포함 하 고 위치 헤더 목록에 대 한 URI를 포함 합니다.| 
| 304| 수정 안 됨| 반환 된 가져옵니다. 클라이언트에 최신 버전의 목록에 있음을 나타냅니다. 서비스의 값을 비교 합니다 <b>If-match</b> 목록 버전 헤더입니다. 같으면 304 데이터가 없는 반환을 가져옵니다. | 
| 400| 잘못된 요청 | 요청은 형식이 잘못 되었습니다. 잘못 된 값 또는 URI 또는 쿼리 문자열 매개 변수의 형식은 일 수 있습니다. 누락 된 필수 매개 변수 결과일 수도 있습니다 하거나 데이터 값 또는 요청 누락 되었거나 잘못 된 버전의 API 표시 합니다. 참조 된 <b>XBL 계약 버전 X</b> 헤더입니다. | 
| 401| 권한 없음 | 요청에 사용자 인증이 필요합니다.| 
| 403| 사용할 수 없음 | 사용자 또는 서비스에 대 한 요청이 허용 되지 않습니다.| 
| 404| 없음 | 호출자에 게 리소스에 액세스할 수 없습니다. 이러한 목록이 없는 만들어졌음을 나타냅니다.| 
| 412| 전제 조건 실패 | 목록의 버전이 변경 되었습니다. 수정 요청이 실패 했음을 나타냅니다. 수정 요청을 삽입, 업데이트 또는 제거 됩니다. 서비스 확인 합니다 <b>If-match</b> 목록 버전에 대 한 헤더입니다. 목록의 현재 버전은 일치 하지 않으면, 다음 작업이 실패 하는 및를 포함 하는 최신 버전은 현재 목록 메타 데이터와 함께 반환 됩니다. | 
| 500| 내부 서버 오류 | 서비스 서버 쪽 오류로 인해 요청이 거부 됩니다.| 
| 501| 구현 되지 않음 | 호출자에 게 서버에서 구현 되지 않았습니다 하는 URI를 요청 합니다. (현재 때 요청을 사용 하는 수 있는 허용 되지 않는 목록 이름.)| 
| 503| 서비스 이용 불가 | 서버에서 과도 한 부하 때문에 일반적으로 요청을 거부 됩니다. 잠시 후 (참조를 <b>retry-after</b> 헤더)에 요청을 다시 시도할 수 있습니다. | 
  
<a id="ID4EVAAC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 인증 요청을 인증 하는 데 사용 된 STS 토큰을 포함 합니다. XSTS 서비스에서 토큰 이어야 하 고 클레임 중 하나는 XUID 포함 해야 합니다. | 
| X-XBL-Contract-Version| API 버전 요청된 (양의 정수)를 지정 합니다. Pin 지원 버전 2입니다. 이 헤더가 누락 되 면 서비스는 400 – 반환 값 지원 되지 않습니다 경우 잘못 된 상태 설명에 "지원 되지 않는 없거나 계약 버전 헤더가"를 사용 하 여 요청 합니다. | 
| Content-Type| 요청/응답 본문의 콘텐츠는 json 또는 xml에 있는지 여부를 지정 합니다. 지원 되는 값은 "application/json" 및 "application/xml"| 
| If-Match| 이 헤더는 요청을 수정할 때는 목록의 현재 버전 번호를 포함 해야 합니다. 요청 사용 하 여 PUT, POST, 수정 하거나 동사를 삭제할. "If-match" 헤더에 버전 목록의 현재 버전을 일치 하지 않으면는 HTTP 412 요청이 거부 되며 – 전제 조건 실패 반환 코드입니다. (선택 사항) 사용할 수 있습니다도 get 경우 전달 된 버전과 현재 일치 현재 목록 버전 고 없습니다 목록 데이터는 HTTP 304 수정 되지 코드가 반환 됩니다. | 
  
<a id="ID4ELCAC"></a>

 
## <a name="response-body"></a>응답 본문 
 
호출에 성공 하면 함께 항목이 목록에서 인지 여부를 지정 하는 각 항목에 대 한 부울 항목 목록이 반환 됩니다. 
 
<a id="ID4EVCAC"></a>

 
### <a name="sample-response"></a>샘플 응답 
 

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

 
##### <a name="parent"></a>Parent 

[유니버설 리소스 식별자 (URI) 참조](../atoc-xboxlivews-reference-uris.md)

   