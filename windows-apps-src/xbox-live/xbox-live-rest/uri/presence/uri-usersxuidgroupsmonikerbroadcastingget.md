---
title: GET (/users/xuid({xuid})/groups/{moniker}/broadcasting )
assetID: 8a3df075-ccdf-18f2-ab0c-275f25cc22e3
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerbroadcastingget.html
description: " GET (/users/xuid({xuid})/groups/{moniker}/broadcasting )"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fe9f0b880fb405b6374a1f7edb49f364def87bb7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640948"
---
# <a name="get-usersxuidxuidgroupsmonikerbroadcasting-"></a>GET (/users/xuid({xuid})/groups/{moniker}/broadcasting )
그룹 모니커가 XUID URI에 표시 되는 관련 된 지정 된 브로드캐스트 사용자의 현재 상태 레코드를 검색 합니다. 이러한 Uri에 대 한 도메인은 `userpresence.xboxlive.com`합니다.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4E5)
  * [쿼리 문자열 매개 변수](#ID4EJB)
  * [권한 부여](#ID4EKC)
  * [리소스에 대 한 개인 정보 설정의 효과](#ID4EQD)
  * [필요한 요청 헤더](#ID4EEH)
  * [선택적 요청 헤더](#ID4EMBAC)
  * [요청 본문](#ID4EMCAC)
  * [HTTP 상태 코드](#ID4EXCAC)
  * [필요한 응답 헤더](#ID4E3GAC)
  * [선택적 응답 헤더](#ID4EMJAC)
  * [응답 본문](#ID4E5KAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
액세스 그룹 모니커가 지정 된 브로드캐스트 사용자의 현재 상태 레코드를 URI에 표시 되는 XUID 관련이 있습니다.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid| 문자열| Xbox 사용자 ID (XUID)의 그룹 XUIDs와 관련 된 사용자입니다.| 
| 모니커| 문자열| 사용자 그룹을 정의 하는 문자열입니다. 현재 허용 된 유일한 모니커 대문자 'P'를 사용 하 여 "People" 됩니다.| 
  
<a id="ID4EJB"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | 
| level| 문자열| 이 쿼리 문자열에 지정 된 대로 정보의 수준을 반환합니다. 유효한 옵션 "user", "device", "title" 및 "all"을 포함 합니다. "User" 수준은 DeviceRecord 중첩된 개체 없이 PresenceRecord 개체입니다. "장치" 수준은 TitleRecord 중첩된 개체 없이 PresenceRecord 및 DeviceRecord 개체입니다. "Title" 수준은 ActivityRecord 중첩된 개체 없이 PresenceRecord, DeviceRecord, 및 TitleRecord 개체입니다. "All" 수준에는 모든 중첩 된 개체를 포함 하 여 전체 레코드입니다. 이 매개 변수를 제공 하지 않으면 서비스 기본값으로 제목 수준 (즉, title의 세부 정보까지이 사용자에 대 한 현재 상태 반환).| 
  
<a id="ID4EKC"></a>

 
## <a name="authorization"></a>권한 부여
 
권한 부여 클레임 사용 | 클레임| 형식| 필수 여부| 예제 값| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>Xuid</b>| 64 비트 부호 있는 정수| 예| 1234567890| 
  
<a id="ID4EQD"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>리소스에 대 한 개인 정보 설정의 효과
 
서비스는 항상 반환 200 확인 요청 자체에 올바르게 구성 된 경우. 그러나 개인 정보 검사를 전달 하지 않는 경우 응답에서 정보 필터링 됩니다.
 
리소스에 대 한 개인 정보 설정의 효과 | 요청 하는 사용자| 대상 사용자의 개인 정보 설정| 동작| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Me| -| 설명한 합니다.| 
| friend| 모든 사용자| 설명한 합니다.| 
| friend| 친구 들과| 설명한 합니다.| 
| friend| 차단됨| -설명 된 대로 서비스 데이터를 필터링 합니다.| 
| friend가 아닌 사용자| 모든 사용자| 설명한 합니다.| 
| friend가 아닌 사용자| 친구 들과| -설명 된 대로 서비스 데이터를 필터링 합니다.| 
| friend가 아닌 사용자| 차단됨| -설명 된 대로 서비스 데이터를 필터링 합니다.| 
| 타사 사이트| 모든 사용자| -설명 된 대로 서비스 데이터를 필터링 합니다.| 
| 타사 사이트| 친구 들과| -설명 된 대로 서비스 데이터를 필터링 합니다.| 
| 타사 사이트| 차단됨| -설명 된 대로 서비스 데이터를 필터링 합니다.| 
  
<a id="ID4EEH"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예를 들어 값: "XBL3.0 x=&lt;userhash>;&lt;token>".| 
| x-xbl-contract-version| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 헤더, 인증 토큰의 클레임의 유효성을 확인 한 후 서비스는 합니다. 예제 값: 3, vnext입니다.| 
| 수락| 문자열| 허용 되는 콘텐츠-유형입니다. 하나만 있으면 지원 됩니다 <b>application/json</b>, 것도 지정 해야 합니다 헤더에 있습니다.| 
| Accept-Language| 문자열| 응답에는 문자열에 대 한 허용 가능한 로캘입니다. 값 예: EN-US입니다.| 
| 호스트| 문자열| 서버의 도메인 이름입니다. 예제 값: userpresence.xboxlive.com 합니다.| 
  
<a id="ID4EMBAC"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 헤더, 인증 토큰의 클레임의 유효성을 확인 한 후 서비스는 합니다. 기본값: 1.| 
  
<a id="ID4EMCAC"></a>

 
## <a name="request-body"></a>요청 본문
 
개체가이 요청의 본문에 전송 됩니다.
  
<a id="ID4EXCAC"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 세션을 검색 했습니다.| 
| 400| 잘못된 요청| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음| 요청에 사용자 인증이 필요합니다.| 
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청이 허용 되지 않습니다.| 
| 404| 없음| 지정된 된 리소스를 찾을 수 없습니다.| 
| 405| 메서드가 허용 되지 않습니다| HTTP 메서드가 지원 되지 않는 콘텐츠 형식에서 사용 되었습니다.| 
| 406| 허용 되지 않음| 리소스 버전이 지원 되지 않습니다.| 
| 500| 요청 시간 초과| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 503| 요청 시간 초과| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
  
<a id="ID4E3GAC"></a>

 
## <a name="required-response-headers"></a>필요한 응답 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 헤더, 인증 토큰의 클레임의 유효성을 확인 한 후 서비스는 합니다. 예제 값: 1, vnext 합니다.| 
| Content-Type| 문자열| 요청 본문의 mime 형식입니다. 예제 값: <b>application/json</b>합니다.| 
| Cache-Control| 문자열| 캐싱 동작을 지정 하는 처리 완료 후 요청입니다. 예제 값: "no-cache"입니다.| 
| X-XblCorrelationId| 문자열| 서버에서 반환 하는 새로운 하 고 클라이언트에서 받은 내용을 상관 관계를 서비스에서 생성 된 값입니다. 예를 들어 값: "4106d0bc-1cb3-47bd-83fd-57d041c6febe".| 
| X-Content-Type-Option| 문자열| SDL 규격 값을 반환합니다. 예제 값: "nosniff"입니다.| 
| 날짜| 문자열| 메시지를 보낸 날짜/시간입니다. 예를 들어 값: "Tue, 17 Nov 2012 10:33:31 GMT".| 
  
<a id="ID4EMJAC"></a>

 
## <a name="optional-response-headers"></a>선택적 응답 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Retry-after| 문자열| 503 HTTP 오류에 반환 합니다. 클라이언트를 호출을 다시 시도 하기 전에 대기할 기간 알 수 있습니다. 예제 값: "120".| 
| Content-Length| 문자열| 응답 본문의 길이입니다. 예를 들어 값: "527".| 
| Content-Encoding| 문자열| 응답의 인코딩 형식입니다. 예제 값: "gzip"입니다.| 
  
<a id="ID4E5KAC"></a>

 
## <a name="response-body"></a>응답 본문
 
이 API는 XUID URI에 표시 되는 관련 된 그룹 모니커가 지정 된 브로드캐스트 사용자의 현재 상태 레코드를 검색 합니다.
 
<a id="ID4EGLAC"></a>

 
### <a name="sample-response"></a>샘플 응답
 

```cpp
[
     {
         xuid:"0123456789",
         state:"online",
         devices:
         [
             {
                 type:"D",
                 titles:
                 [
                     {
                         id:"12341234",
                         name:"Halo 5",
                         lastModified:"2012-09-17T07:15:23.4930000",
                         placement:"full",
                         state:"active",
                         activity:
                         {
                             richPresence:"Playing on Valhalla",    
                             broadcast:
                            {
                                "id":"broadcast id from broadcasting service",
                                "session":"3f2504e0-4f89-11d3-9a0c-0305e82c3301",
                                "provider":"Twitch",
                                "started":"2014-01-15T15:15:23.493Z",
                                "viewers":42
                           }
                         },
                     }
                 ]
             }
         ]
     },
     {
         xuid:"0123456780",
         state:"online",
         devices:
         [
             {
                 type:"D",
                 titles:
                 [
                     {
                         id:"12341235",
                         name:"Halo Waypoint",
                         lastModified:"2012-09-17T07:15:23.4930000",
                         placement;"full",
                         state:"active",
                         activity:
                         {
                             richPresence:"Viewing stats"
                         },
                     }
                 ]
             }
         ]
     },
     {
         xuid:"0123456781",
         state:"online",
         devices:
         [
             {
                 type:"Win8",
                 titles:
                 [
                     {
                         id:"23452345",
                         name:"Halo Gamehelp",
                         state:"active",
                         timestamp:"2012-09-17T07:15:23.4930000",
                         activity:
                         {
                             richPresence:"Viewing warthog help"
                         }
                     }
                 ]
             }
         ]
     }
 ]

         
```

   
<a id="ID4EQLAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ESLAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/groups/{moniker}](uri-usersxuidgroupsmoniker.md)

   