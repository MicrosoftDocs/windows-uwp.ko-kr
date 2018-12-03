---
title: GET (/users/xuid({xuid})/groups/{moniker} )
assetID: 63aa7e5d-0599-5850-756d-3079c0772238
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerget.html
description: " GET (/users/xuid({xuid})/groups/{moniker} )"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: df967677ce779fc128a8956f137a027a108d313d
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8348531"
---
# <a name="get-usersxuidxuidgroupsmoniker-"></a>GET (/users/xuid({xuid})/groups/{moniker} )
그룹의 PresenceRecord를 가져옵니다. 이러한 Uri에 대 한 도메인은 `userpresence.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4E5)
  * [쿼리 문자열 매개 변수](#ID4EJB)
  * [권한 부여](#ID4EKC)
  * [리소스의 개인 정보 설정의 효과](#ID4EQD)
  * [필요한 요청 헤더](#ID4EEH)
  * [선택적 요청 헤더](#ID4EMBAC)
  * [요청 본문](#ID4EMCAC)
  * [HTTP 상태 코드](#ID4EXCAC)
  * [필요한 응답 헤더](#ID4E3GAC)
  * [선택적 응답 헤더](#ID4EMJAC)
  * [응답 본문](#ID4E5KAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
URI에 사용자에 게 관련 모니커 하 여 지정 된 그룹의 사용자를 검색 하 고 해당 사용자에 대 한 PresenceRecord를 반환 합니다. 데이터 개인 정보 또는 콘텐츠 격리에 의해 제어를 간단 하 게 반환 되지 않습니다.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| string| Xbox 사용자 ID (XUID)와 관련 된 그룹에 XUIDs 사용자의 합니다.| 
| 모니커| string| 사용자의 그룹을 정의 하는 문자열입니다. 현재 허용 된 유일한 모니커 대문자 'P'를 사용 하 여 "사람" 인 경우| 
  
<a id="ID4EJB"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| level| 문자열| 이 쿼리 문자열을 기준으로 세부 정보 수준을 반환합니다. 유효한 옵션에는 "사용자", "장치", "제목" 및 "모든" 포함 됩니다. "사용자" 수준은 DeviceRecord 중첩 된 개체 없이 PresenceRecord 개체입니다. "장치" 수준은 TitleRecord 중첩 된 개체 없이 PresenceRecord 및 DeviceRecord 개체입니다. "Title 이라는" 수준은 ActivityRecord 중첩 된 개체 없이 PresenceRecord, DeviceRecord, 및 TitleRecord 개체입니다. "All" 수준은 중첩 된 모든 개체를 비롯 한 전체 레코드입니다. 이 매개 변수를 지정 하지 않은 서비스 기본적으로 제목 수준 (즉, 제목의 세부 정보로이 사용자에 대 한 상태 반환).| 
  
<a id="ID4EKC"></a>

 
## <a name="authorization"></a>권한 부여
 
권한 부여 클레임 사용 | 클레임| 유형| 필수 여부| 예제 값| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>Xuid</b>| 64 비트의 부호 있는 정수| 예| 1234567890| 
  
<a id="ID4EQD"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>리소스의 개인 정보 설정의 효과
 
서비스는 항상 반환 200 OK 자체 요청이 제대로 구성 된 경우 합니다. 그러나 개인 정보 보호 검사를 통과 하지 않는 경우 응답의 정보 필터링 됩니다.
 
리소스의 개인 정보 설정의 효과 | 사용자를 요청합니다.| 대상 사용자의 개인 정보 설정| 동작| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 옵션인 추가 정보| -| 설명한 합니다.| 
| 친구| 모든 사용자| 설명한 합니다.| 
| 친구| 친구만| 설명한 합니다.| 
| 친구| 차단| -설명한 서비스는 데이터를 필터링 합니다.| 
| 비 친구 사용자| 모든 사용자| 설명한 합니다.| 
| 비 친구 사용자| 친구만| -설명한 서비스는 데이터를 필터링 합니다.| 
| 비 친구 사용자| 차단| -설명한 서비스는 데이터를 필터링 합니다.| 
| 제 3 자 사이트| 모든 사용자| -설명한 서비스는 데이터를 필터링 합니다.| 
| 제 3 자 사이트| 친구만| -설명한 서비스는 데이터를 필터링 합니다.| 
| 제 3 자 사이트| 차단| -설명한 서비스는 데이터를 필터링 합니다.| 
  
<a id="ID4EEH"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: "XBL3.0 x =&lt;userhash >; &lt;토큰 > ".| 
| xbl 계약 버전 x| string| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 서비스의 인증 토큰을 클레임 헤더의 유효성을 확인 한 후에 있습니다. 예제 값: 3, vnext 합니다.| 
| 수락| string| 사용할 수 있는 콘텐츠-형식입니다. 하나의 존재 지원 <b>응용 프로그램/j</b>이지만 여전히 지정 해야 헤더에 있습니다.| 
| Accept Language| string| 응답에는 문자열에 대 한 허용 로캘입니다. 예제 값: EN-US 합니다.| 
| 호스트| 문자열| 도메인 이름 서버입니다. 예제 값: userpresence.xboxlive.com 합니다.| 
  
<a id="ID4EMBAC"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion|  | 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 서비스의 인증 토큰을 클레임 헤더의 유효성을 확인 한 후에 있습니다. 기본값: 1입니다.| 
  
<a id="ID4EMCAC"></a>

 
## <a name="request-body"></a>요청 본문
 
개체가이 요청의 본문에 전송 됩니다.
  
<a id="ID4EXCAC"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스 사용 되는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 세션을 검색 했습니다.| 
| 400| 잘못 된 요청| 서비스 잘못 된 요청을 이해 하지 못했습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음| 필요한 사용자 인증을 요청 합니다.| 
| 403| 금지| 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 404| 찾을 수 없음| 지정된 된 리소스를 찾을 수 없습니다.| 
| 405| 메서드 허용 안 함| HTTP 메서드는 지원 되지 않는 콘텐츠 형식에 사용 되었습니다.| 
| 406| 허용할 수 없음| 리소스 버전은 지원 되지 않습니다.| 
| 500| 요청 시간 제한| 서비스 잘못 된 요청을 이해 하지 못했습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 503| 요청 시간 제한| 서비스 잘못 된 요청을 이해 하지 못했습니다. 일반적으로 잘못 된 매개 변수입니다.| 
  
<a id="ID4E3GAC"></a>

 
## <a name="required-response-headers"></a>필요한 응답 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| xbl 계약 버전 x| string| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 서비스의 인증 토큰을 클레임 헤더의 유효성을 확인 한 후에 있습니다. 예제 값: 1, vnext 합니다.| 
| Content-Type| 문자열| 요청 본문의 mime 형식입니다. 예제 값: <b>응용 프로그램/j</b>합니다.| 
| 캐시 제어| string| 정중 요청 캐싱 동작을 지정 합니다. 예제 값: "아니요 캐시".| 
| X XblCorrelationId| string| 서버에서 반환 하는 어떤와 클라이언트에서 수신 되는 어떤 연결 하기 위해 서비스에서 생성 된 값입니다. 예제 값: "4106d0bc-1cb3-47bd-83fd-57d041c6febe".| 
| 콘텐츠 유형 옵션 X| string| SDL 규격 값을 반환합니다. 예제 값: "nosniff".| 
| Date| 문자열| 메시지를 보낸 날짜/시간 예제 값: "2012 년 11 월 17 일 화 10시 33분: 31 GMT".| 
  
<a id="ID4EMJAC"></a>

 
## <a name="optional-response-headers"></a>선택적 응답 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 다시 시도 후| string| 503 HTTP 오류에서 반환 됩니다. 클라이언트를 호출을 다시 시도 하기 전에 대기할 시간 알 수 있습니다. 예제 값: "120".| 
| Content-Length| string| 응답 본문의 길이입니다. 예제 값: "527".| 
| 콘텐츠 인코딩| string| 응답의 인코딩 형식입니다. 예제 값: "gzip".| 
  
<a id="ID4E5KAC"></a>

 
## <a name="response-body"></a>응답 본문
 
이 API에는 각 요청에서 XUIDs 하나 PresenceRecord 개체의 배열을 반환합니다.
 
<a id="ID4EGLAC"></a>

 
### <a name="sample-response"></a>예제 응답
 

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
                         name:"Contoso 5",
                         lastModified:"2012-09-17T07:15:23.4930000",
                         placement:"full",
                         state:"active",
                         activity:
                         {
                             richPresence:"Playing on Nirvana"
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
                         name:"Contoso Waypoint",
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
                         name:"Contoso Gamehelp",
                         state:"active",
                         timestamp:"2012-09-17T07:15:23.4930000",
                         activity:
                         {
                             richPresence:"Viewing help"
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

 
##### <a name="parent"></a>부모 

[/users/xuid({xuid})/groups/{moniker}](uri-usersxuidgroupsmoniker.md)

   