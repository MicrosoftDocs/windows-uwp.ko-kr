---
title: POST (/users/{ownerId}/people/xuids)
assetID: e20bfb58-9c3b-14ed-6462-85d42fa6fe1a
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeoplexuidspost.html
description: " POST (/users/{ownerId}/people/xuids)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1cb160f3276d215e3aba5dfd671c67fa17d883b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589868"
---
# <a name="post-usersowneridpeoplexuids"></a>POST (/users/{ownerId}/people/xuids)
컬렉션을에서 호출자의 사용자 XUID 하 여 사용자를 가져옵니다. 이러한 Uri에 대 한 도메인은 `social.xboxlive.com`합니다.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4E5)
  * [권한 부여](#ID4EJB)
  * [필요한 요청 헤더](#ID4ERC)
  * [선택적 요청 헤더](#ID4EBE)
  * [요청 본문](#ID4EHF)
  * [HTTP 상태 코드](#ID4EKH)
  * [필요한 응답 헤더](#ID4ENBAC)
  * [응답 본문](#ID4EZCAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
POST 작업 하므로 한 번 또는 여러 번 실행 하는 경우 동일한 결과 생성은이 모든 리소스를 수정 하지 않습니다.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| ownerId| 문자열| 해당 리소스에 액세스 하는 사용자의 식별자입니다. 인증된 된 사용자를 일치 해야 합니다. 가능한 값은 "me", xuid({xuid}), 또는 gt({gamertag})입니다.| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>권한 부여
 
| 형식| 필수| 설명| 응답 없는 경우| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| 예| 호출자가 사용자의 Xbox 사용자 ID (XUID).| 401 권한이 없음| 
  
<a id="ID4ERC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열입니다. Xbox LIVE에 대 한 권한 부여 데이터입니다. 이것이 일반적으로 암호화 된 XSTS 토큰입니다. 예를 들어 값: <b>XBL3.0 x=&lt;userhash>;&lt;token></b>.| 
| Content-Length| 32 비트 부호 없는 정수입니다. 길이 (바이트)를 요청 본문입니다. 예를 들어 값: 22.| 
| Content-Type| 문자열입니다. 요청 본문의 MIME 형식입니다. 이 여야 <b>application/json</b>합니다.| 
  
<a id="ID4EBE"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 기본값: 1.| 
| 수락| 문자열입니다. 호출자가 응답에 허용 하는 콘텐츠-형식입니다. 모든 응답이 <b>application/json</b>합니다.| 
  
<a id="ID4EHF"></a>

 
## <a name="request-body"></a>요청 본문
 
<a id="ID4ENF"></a>

 
### <a name="required-members"></a>필요한 멤버
 
| 멤버| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| XuidList| 호출자의 사용자 컬렉션에서 반환할 사용자를 식별 하는 XUIDs의 배열입니다. 참조 [XuidList (JSON)](../../json/json-xuidlist.md)합니다.| 
  
<a id="ID4EKG"></a>

 
### <a name="optional-members"></a>선택적 멤버
 
이 요청에 대 한 선택적 멤버가 없습니다.
  
<a id="ID4EVG"></a>

 
### <a name="prohibited-members"></a>허용 되지 않는 멤버
 
다른 모든 멤버를 요청에 사용할 수 없습니다.
  
<a id="ID4EAH"></a>

 
### <a name="sample-request"></a>샘플 요청
 

```cpp
{
    "xuids": [
        "2533274790395904", 
        "2533274792986770", 
        "2533274794866999"
    ]
}
      
```

   
<a id="ID4EKH"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 메서드는 "get" 때 성공 했습니다.| 
| 204| 내용 없음| 메서드는 "추가" 하는 경우 성공 하거나 "제거" 합니다.| 
| 400| 잘못된 요청| 메서드 매개 변수 누락 되었거나 형식이 잘못 되었거나 사용자 Id 형식이 잘못 되었습니다.| 
| 403| 사용할 수 없음| XUID 클레임 권한 부여 헤더에서 분석할 수 없습니다.| 
  
<a id="ID4ENBAC"></a>

 
## <a name="required-response-headers"></a>필요한 응답 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Length| 32 비트 부호 없는 정수| 길이 (바이트)를 응답 본문입니다. 예를 들어 값: 22.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 이 값은 항상 <b>application/json</b>합니다.| 
  
<a id="ID4EZCAC"></a>

 
## <a name="response-body"></a>응답 본문
 
응답 본문 요청 메서드가 "get" 경우에 전송 됩니다. "추가" 또는 "제거"에 대 한 응답 본문이 없습니다.
 
"Get" 메서드 호출에 성공한 경우 서비스 컬렉션 및 호출자의 사용자 컬렉션을 포함 하는 배열을 호출자의 사용자 들은 사용자의 총 수를 반환 합니다. "Add" 및 "제거" 메서드에 대 한 응답이 반환 됩니다. 참조 [PeopleList (JSON)](../../json/json-peoplelist.md)합니다.
 
<a id="ID4EHDAC"></a>

 
### <a name="sample-response"></a>샘플 응답
 

```cpp
{
    "people": [
        {
            "xuid": "2603643534573573",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573572",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573577",
            "isFavorite": false,
            "isFollowingCaller": false
        },
    ],
    "totalCount": 3
}
         
```

   
<a id="ID4ERDAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ETDAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/{ownerId}/people/xuids](uri-usersowneridpeoplexuids.md)

   