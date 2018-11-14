---
title: POST (/users/{ownerId}/people/xuids)
assetID: e20bfb58-9c3b-14ed-6462-85d42fa6fe1a
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeoplexuidspost.html
author: KevinAsgari
description: " POST (/users/{ownerId}/people/xuids)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 91bcae367e42b3dc728b794d1e68550e86dcfeaa
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6257224"
---
# <a name="post-usersowneridpeoplexuids"></a>POST (/users/{ownerId}/people/xuids)
컬렉션을 호출자의 사용자 로부터 XUID 사람을 가져옵니다. 이러한 Uri에 대 한 도메인은 `social.xboxlive.com`.
 
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
 
게시 작업 않으므로 한 번 또는 여러 번 실행 하는 경우 동일한 결과 재현 됩니다이 모든 리소스를 수정 하지 않습니다.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| ownerId| string| 해당 리소스를 액세스 하는 사용자의 식별자입니다. 인증된 된 사용자와 일치 해야 합니다. 가능한 값은 "me", xuid({xuid}), 또는 gt({gamertag}) 합니다.| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>권한 부여
 
| 형식| 필수| 설명| 응답 없는 경우| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| 예| 호출자가 사용자의 Xbox 사용자 ID (XUID).| 401 권한이 없음| 
  
<a id="ID4ERC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열입니다. Xbox LIVE에 대 한 데이터를 권한 부여 합니다. 일반적으로 암호화 된 XSTS 토큰입니다. 예제 값: XBL3.0 <b>x =&lt;userhash >;&lt; 토큰 ></b>합니다.| 
| Content-Length| 32 비트 부호 없는 정수입니다. 길이, 바이트, 요청 본문의입니다. 예제 값: 22.| 
| Content-Type| 문자열입니다. 요청 본문의 MIME 형식입니다. 이 <b>응용 프로그램/j</b>이어야 합니다.| 
  
<a id="ID4EBE"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 인증 토큰 등의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우트됩니다 됩니다. 기본값: 1입니다.| 
| 수락| 문자열입니다. 호출자는 응답에서 허용 하는 콘텐츠-형식입니다. 모든 응답은 <b>응용 프로그램/j</b>.| 
  
<a id="ID4EHF"></a>

 
## <a name="request-body"></a>요청 본문
 
<a id="ID4ENF"></a>

 
### <a name="required-members"></a>필수 멤버
 
| 멤버| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| XuidList| 호출자의 사람들이 컬렉션에서 반환 될 사용자를 식별 하는 XUIDs의 배열입니다. [XuidList (JSON)를](../../json/json-xuidlist.md)참조 하세요.| 
  
<a id="ID4EKG"></a>

 
### <a name="optional-members"></a>선택적 멤버
 
이 요청에 대 한 선택적 멤버인 없습니다.
  
<a id="ID4EVG"></a>

 
### <a name="prohibited-members"></a>금지 된 멤버
 
다른 모든 구성원 요청에 사용할 수 없습니다.
  
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
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| "가져오는" 방법은 때 성공 합니다.| 
| 204| 콘텐츠 없음| 성공 방법은 "추가" 또는 "제거" 합니다.| 
| 400| 잘못 된 요청| 메서드 매개 변수 누락 되거나 잘못 된 형식의 되었거나 사용자 Id가 잘못 된 형식의 합니다.| 
| 403| 금지| 권한 부여 헤더에서 XUID 클레임을 분석할 수 없습니다.| 
  
<a id="ID4ENBAC"></a>

 
## <a name="required-response-headers"></a>필요한 응답 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Length| 32 비트 부호 없는 정수| 길이, 바이트, 응답 본문의입니다. 예제 값: 22.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 이 <b>응용 프로그램/j</b>항상 됩니다.| 
  
<a id="ID4EZCAC"></a>

 
## <a name="response-body"></a>응답 본문
 
응답 본문을 요청 방법은 "가져오는" 하는 경우에 전송 됩니다. "추가" 또는 "제거"에 대 한 응답 본문이 없습니다.
 
"Get" 메서드 호출 되 면 서비스 컬렉션과 호출자의 사용자 컬렉션을 포함 하는 배열을 호출자의 사람들에 사용자의 총 수를 반환 합니다. "추가" 및 "제거" 메서드에 대 한 응답이 반환 됩니다. [PeopleList (JSON)를](../../json/json-peoplelist.md)참조 하세요.
 
<a id="ID4EHDAC"></a>

 
### <a name="sample-response"></a>예제 응답
 

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

 
##### <a name="parent"></a>부모 

[/users/{ownerId}/people/xuids](uri-usersowneridpeoplexuids.md)

   