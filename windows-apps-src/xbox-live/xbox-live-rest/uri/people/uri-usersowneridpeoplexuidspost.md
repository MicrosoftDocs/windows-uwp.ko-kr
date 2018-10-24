---
title: POST (/users/{ownerId}/people/xuids)
assetID: e20bfb58-9c3b-14ed-6462-85d42fa6fe1a
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeoplexuidspost.html
author: KevinAsgari
description: " POST (/users/{ownerId}/people/xuids)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 27fbc0e209439fca01cf1e7d8c7c3bf98c4b9053
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5483535"
---
# <a name="post-usersowneridpeoplexuids"></a>POST (/users/{ownerId}/people/xuids)
컬렉션을에서 호출자의 사용자 XUID로 사람을 가져옵니다. 이러한 Uri의 도메인은 `social.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4E5)
  * [권한 부여](#ID4EJB)
  * [필요한 요청 헤더](#ID4ERC)
  * [선택적 요청 헤더](#ID4EBE)
  * [요청 본문](#ID4EHF)
  * [HTTP 상태 코드입니다.](#ID4EKH)
  * [필수 응답 헤더](#ID4ENBAC)
  * [응답 본문](#ID4EZCAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
게시 작업 한 번 또는 여러 번 실행 하는 경우 동일한 결과 생성 합니다이 리소스 수정 되지 않습니다.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| ownerId| string| 해당 리소스에 액세스 하 고 사용자 식별자입니다. 인증된 된 사용자를 일치 해야 합니다. 가능한 값에는 "나", xuid({xuid}), 또는 gt({gamertag})은.| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>권한 부여
 
| 형식| 필수| 설명| 응답이 없는 경우| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| 예| 호출자가 사용자의 Xbox 사용자 ID (XUID).| 401 권한이 없음| 
  
<a id="ID4ERC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열입니다. Xbox LIVE에 대 한 권한 부여 데이터입니다. 일반적으로 암호화 된 XSTS 토큰입니다. 예제 값: <b>XBL3.0 x =&lt;userhash >;&lt; 토큰 ></b>.| 
| Content-Length| 32 비트 부호 없는 정수입니다. 길이 바이트 단위로 요청 본문입니다. 예제 값: 22.| 
| Content-Type| 문자열입니다. 요청 본문의 MIME 형식입니다. <b>응용 프로그램/json</b>이어야 합니다.| 
  
<a id="ID4EBE"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion| 빌드 이름/번호는 Xbox LIVE 서비스를이 요청 하시기 바랍니다. 요청 헤더, 등 인증 토큰에서 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅되는. 기본값: 1.| 
| 수락| 문자열입니다. 콘텐츠 유형은 응답에서 호출자를 허용 하는. 모든 응답은 <b>응용 프로그램/json</b>입니다.| 
  
<a id="ID4EHF"></a>

 
## <a name="request-body"></a>요청 본문
 
<a id="ID4ENF"></a>

 
### <a name="required-members"></a>필수 멤버
 
| 구성원| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| XuidList| 호출자의 사용자 컬렉션에서 반환할 사용자를 식별 하는 XUIDs의 배열입니다. [(JSON) XuidList](../../json/json-xuidlist.md)을 참조 하십시오.| 
  
<a id="ID4EKG"></a>

 
### <a name="optional-members"></a>선택적 멤버
 
이 요청에 대 한 선택적 멤버가 있습니다.
  
<a id="ID4EVG"></a>

 
### <a name="prohibited-members"></a>금지 된 구성원
 
다른 멤버는 모든 요청에 사용할 수 없습니다.
  
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

 
## <a name="http-status-codes"></a>HTTP 상태 코드입니다.
 
서비스는이 리소스에는이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스 사용 하는 표준 HTTP 상태 코드의 전체 목록은, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하십시오.
 
| Code| 원인 문구| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| "Get" 방법은 성공 합니다.| 
| 204| 내용 없음| 성공 방법은 "추가" 또는 "사용 안 함"입니다.| 
| 400| 잘못 된 요청| 메서드 매개 변수 누락 되었거나, 잘못 되었거나 사용자 Id 형식이 잘못 되었습니다.| 
| 403| 사용할 수 없음| XUID 클레임 인증 헤더를 구문 분석 될 수 있습니다.| 
  
<a id="ID4ENBAC"></a>

 
## <a name="required-response-headers"></a>필수 응답 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Length| 32 비트 부호 없는 정수| 길이 바이트 단위로 응답 본문의. 예제 값: 22.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 항상 <b>응용 프로그램/json</b>됩니다.| 
  
<a id="ID4EZCAC"></a>

 
## <a name="response-body"></a>응답 본문
 
응답 본문 요청 메서드가 "get" 경우에 보내집니다. "추가" 또는 "제거"에 대 한 응답 본문이 없습니다.
 
"Get" 메서드 호출이 성공 하면 서비스와 호출자의 사용자 컬렉션을 포함 하는 배열에 호출자의 사람들의 사람들의 총 수를 반환 합니다. "추가" 및 "제거" 하는 방법에 대 한 응답이 반환 됩니다. [(JSON) PeopleList](../../json/json-peoplelist.md)을 참조 하십시오.
 
<a id="ID4EHDAC"></a>

 
### <a name="sample-response"></a>샘플 응답입니다.
 

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

   