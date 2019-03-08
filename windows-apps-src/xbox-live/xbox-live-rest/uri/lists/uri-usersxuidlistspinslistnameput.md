---
title: PUT (/users/xuid(xuid)/lists/PINS/{listname})
assetID: f7964d2e-a8c8-2caa-ca97-e0d76ef586f4
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameput.html
description: " PUT (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 35527df28c2ca482d0c5ae2fe25637b3bc29f6ca
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622928"
---
# <a name="put-usersxuidxuidlistspinslistname"></a>PUT (/users/xuid(xuid)/lists/PINS/{listname})
요청 본문의 각 항목에 대해 지정 된 인덱스에 따라 목록에서 항목을 업데이트 합니다. 이러한 Uri에 대 한 도메인은 `eplists.xboxlive.com`합니다.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4E1B)
  * [권한 부여](#ID4EFC)
  * [HTTP 상태 코드](#ID4ESC)
  * [필요한 요청 헤더](#ID4EPH)
  * [요청 본문](#ID4EGBAC)
  * [응답 본문](#ID4EWBAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
이 호출에는 요청 본문의 각 항목에 대해 지정 된 인덱스에 따라 목록에서 항목 업데이트 됩니다. 이 호출 목록에 항목을 삽입 되지 않습니다 하 고 지정한 인덱스에 있는 항목을 존재 하지 않는 경우 다음 호출 400 잘못 된 요청 상태가 반환 합니다. 단일 호출에서 여러 항목을 업데이트할 수 있지만 모든 현재 목록에 존재 해야 합니다. 즉, 모든 업데이트 또는 업데이트 없음.
 
이 호출 하 여 지정할 수를 업데이트할 항목 수를 **itemId** of **인덱스**합니다. 이렇게 하려면 단순히 사용 하 여 "-1"의 인덱스에 대 한 합니다 **IndexedItems** 서비스에 전송 되는 구조입니다. 물론이 경우에에서는 **itemId** 변경 내용을 다른 메타 데이터 필드에만 사용할 수 있도록 업데이트의 일환으로 변경할 수 없습니다. **공급자**/**providerId** 콤보 대신 사용할 수 있습니다 **itemId** 항목을 식별 합니다. 내부적으로 서비스 검색은 이러한 항목 및 업데이트를 적절 한 인덱스를 파악 합니다. 그런 다음 항목을 찾을 수 없는 경우 400 잘못 된 요청 상태를 반환할 고 항목이 업데이트 됩니다.
 
이 호출은 필요는 **경우-일치: versionNumber** 인덱스를 사용 하 여 항목을 식별 하는 경우 요청에 포함 될 헤더입니다. 하는 경우 사용 하 여 항목 Id 항목을 식별할 (및 목록 중복을 허용 하지 않습니다)는 **If-match** 헤더는 선택 사항입니다. 있는 경우는 **If-match** 헤더는 항상 유효성을 검사할 수 있습니다. 헤더에는 **versionNumber** 목록의 현재 버전입니다. 하지 포함 (이며 필수), 않습니다 하지 현재 목록 버전 번호는 HTTP 412 사전 조건 실패 상태 코드는 반환 될 일치 하 고 응답의 본문은 최신 메타 데이터를 포함 목록의 경우 현재 버전 번호를 포함 하는 합니다. 이 서로에서 무시 되는 다른 클라이언트에서 업데이트에 대해 보호 됩니다.
  
<a id="ID4E1B"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid| 문자열| Xbox 사용자 ID (XUID)입니다.| 
| listtype| 문자열| 형식 (사용 방법 및 작업 방식) 목록입니다. 항상 "핀"에 대 한 관련 방법.| 
| listname| 문자열| 목록의 이름입니다 (작업할 지정된 listtype는 목록). 항상 "XBLPins"에 대 한 Pin의 항목입니다.| 
  
<a id="ID4EFC"></a>

 
## <a name="authorization"></a>권한 부여
 
이 호출 된 XSTS SAML 토큰에서 예상 된 **권한 부여** 헤더입니다. Xuid 클레임 호출자를 식별 하는 SAML 토큰 내에 있어야 합니다. 이 값은 호출자에 목록 데이터에 액세스 권한이 있는지 확인 하려면 사용 합니다. 목록 자체를 Xuid을도 식별 하 고 목록에 대 한 URI에 포함 됩니다. 이 사용에서는 수 목록에 대 한 공유 지원 액세스 나중에 있지만 없는 기능 지금은 수도 있습니다. 현재 사용자가 액세스 하는 모든 목록 자체 되며 공유 액세스 권한이 없습니다. 따라서 URI에 Xuid SAML 클레임 토큰에서 Xuid 일치 해야 합니다. 

> [!NOTE] 
> 현재 XBL 인증 2.0 및 3.0 토큰을 모두 지원 됩니다. 


  
<a id="ID4ESC"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 요청이 완료 되었습니다. 응답 본문 (GET)에 대 한 요청된 된 리소스를 포함 해야 합니다. POST 및 PUT 요청 (목록 버전, count 등)의 최신 목록 메타 데이터를 받게 됩니다.| 
| 201| 만든 날짜| 새 목록을 만들었습니다. 목록에 초기 삽입 시 반환 됩니다. 응답은 목록에 최신 메타 데이터를 포함 하 고 위치 헤더 목록에 대 한 URI를 포함 합니다.| 
| 304| 수정 안 됨| 반환 된 가져옵니다. 클라이언트에 최신 버전의 목록에 있음을 나타냅니다. 서비스의 값을 비교 합니다 <b>If-match</b> 목록 버전 헤더입니다. 같으면 304 데이터가 없는 반환을 가져옵니다.| 
| 400| 잘못된 요청| 요청은 형식이 잘못 되었습니다. 잘못 된 값 또는 URI 또는 쿼리 문자열 매개 변수의 형식은 일 수 있습니다. 누락 된 필수 매개 변수 결과일 수도 있습니다 하거나 데이터 값 또는 요청 누락 되었거나 잘못 된 버전의 API 표시 합니다. 참조 된 <b>XBL 계약 버전 X</b> 헤더입니다.| 
| 401| 권한 없음| 요청에 사용자 인증이 필요합니다.| 
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청이 허용 되지 않습니다.| 
| 404| 없음| 호출자에 게 리소스에 액세스할 수 없습니다. 이러한 목록이 없는 만들어졌음을 나타냅니다.| 
| 412| 전제 조건 실패| 목록의 버전이 변경 되었습니다. 수정 요청이 실패 했음을 나타냅니다. 수정 요청을 삽입, 업데이트 또는 제거 됩니다. 서비스 확인 합니다 <b>If-match</b> 목록 버전에 대 한 헤더입니다. 목록의 현재 버전은 일치 하지 않으면, 다음 작업이 실패 하는 및를 포함 하는 최신 버전은 현재 목록 메타 데이터와 함께 반환 됩니다.| 
| 500| 내부 서버 오류| 서비스 서버 쪽 오류로 인해 요청이 거부 됩니다.| 
| 501| 구현 되지 않음| 호출자에 게 서버에서 구현 되지 않았습니다 하는 URI를 요청 합니다. (현재 때 요청을 사용 하는 수 있는 허용 되지 않는 목록 이름.)| 
| 503| 서비스 이용 불가| 서버에서 과도 한 부하 때문에 일반적으로 요청을 거부 됩니다. 잠시 후 (참조를 <b>retry-after</b> 헤더)에 요청을 다시 시도할 수 있습니다.| 
  
<a id="ID4EPH"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 인증 요청을 인증 하는 데 사용 된 STS 토큰을 포함 합니다. XSTS 서비스에서 토큰 이어야 하 고 클레임 중 하나는 XUID 포함 해야 합니다. | 
| X-XBL-Contract-Version| API 버전 요청된 (양의 정수)를 지정 합니다. Pin 지원 버전 2입니다. 이 헤더가 누락 되 면 서비스는 400 – 반환 값 지원 되지 않습니다 경우 잘못 된 상태 설명에 "지원 되지 않는 없거나 계약 버전 헤더가"를 사용 하 여 요청 합니다.| 
| Content-Type| 요청/응답 본문의 콘텐츠는 json 또는 xml에 있는지 여부를 지정 합니다. 지원 되는 값은 "application/json" 및 "application/xml"| 
| If-Match| 이 헤더는 요청을 수정할 때는 목록의 현재 버전 번호를 포함 해야 합니다. 요청 사용 하 여 PUT, POST, 수정 하거나 동사를 삭제할. "If-match" 헤더에 버전 목록의 현재 버전을 일치 하지 않으면는 HTTP 412 요청이 거부 되며 – 전제 조건 실패 반환 코드입니다. (선택 사항) 사용할 수 있습니다도 get 경우 전달 된 버전과 현재 일치 현재 목록 버전 고 없습니다 목록 데이터는 HTTP 304 수정 되지 코드가 반환 됩니다. | 
  
<a id="ID4EGBAC"></a>

 
## <a name="request-body"></a>요청 본문
 
<a id="ID4EMBAC"></a>

 
### <a name="sample-request"></a>샘플 요청
 

```cpp
{"IndexedItems":
 [{ "Index": 0, 
     "Item": 
     {
    "ContentType": "Movie",
    "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
    "ProviderId": null,
    "Provider": null,
    "ImageUrl": "https://www.bing.com/thumb/...",
    "Title": "The Dark Knight",
    "SubTitle":null, 
    "Locale": "en-us",
    "DeviceType": "XboxOne"
}
}]}      
      
```

   
<a id="ID4EWBAC"></a>

 
## <a name="response-body"></a>응답 본문
 
<a id="ID4E3BAC"></a>

 
### <a name="sample-response"></a>샘플 응답
 

```cpp
{
  "ListVersion":  1,
  "ListCount":  1,
  "MaxListSize": 200,
  "AllowDuplicates": "true",
  "AccessSetting":  "OwnerOnly"
}        
         
```

   
<a id="ID4EGCAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EICAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

   