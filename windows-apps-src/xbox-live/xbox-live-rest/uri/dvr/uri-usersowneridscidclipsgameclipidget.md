---
title: GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
assetID: dbd60c93-9d8e-609b-0ae3-b3f7ee26ba2d
permalink: en-us/docs/xboxlive/rest/uri-usersowneridscidclipsgameclipidget.html
description: " GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5d2858b11bf8fb902ea07a016c8f41db375013f9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640958"
---
# <a name="get-usersowneridscidsscidclipsgameclipid"></a>GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
알고 있는 경우 해당 위치를 찾도록 모든 Id 시스템에서 단일 게임 클립을 가져옵니다. 이러한 Uri에 대 한 도메인이 `gameclipsmetadata.xboxlive.com` 고 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.
 
  * [설명](#ID4EX)
  * [URI 매개 변수](#ID4EVB)
  * [권한 부여](#ID4EAC)
  * [필요한 요청 헤더](#ID4EUH)
  * [선택적 요청 헤더](#ID4EBCAC)
  * [요청 본문](#ID4ETDAC)
  * [HTTP 상태 코드](#ID4E5DAC)
  * [필요한 응답 헤더](#ID4EQIAC)
  * [선택적 응답 헤더](#ID4EJLAC)
  * [응답 본문](#ID4EJMAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>설명
 
모든 데이터를 클립의 수에 대 한 쿼리를 **클립** 모든 메타 데이터 쿼리에서 반환 된 개체입니다. **XUID**, **ServiceConfigId**를 **GameClipId** 하 고 **SandboxId** 요청의 클레임에는 하는 데 필요한이 API 통해 단일 게임 클립 합니다. 게임 클립 적용에 대 한 플래그가 지정 된 경우 콘텐츠 격리 또는 개인 정보를 사용자에 특정 게임 클립을 가져올 수 있는 권한이 없는지 확인 확인 API는 HTTP 상태 코드 404 (찾을 수 없음)을 반환 합니다.
 
**SandboxId** 는 XToken에 클레임에서 검색 되 고 적용 합니다. 경우는 **SandboxId** 없을 엔터테인먼트 검색 서비스 (EDS) 400 잘못 된 요청 오류가 throw 됩니다.
 
만료 된 Uri를 새로 고치려면이 API는 사용할 수도 있어야 합니다. 쿼리가 완료 되 면 게임 클립에 대 한 만료 된 Uri가 새로 고쳐집니다 적절 하 게 합니다. 

> [!NOTE] 
> URI를 새로 고치는 30 ~ 40 초까지이 요청 작업이 완료 되 면 걸릴 수 있습니다. URI 만료 된 경우 스트리밍 작업에 대 한 즉시 사용 하려고 IIS 부드러운 스트리밍 서버에서 HTTP 500 상태 코드를 받게 됩니다. 이 단축 하는 방법을 노력 및이 해당 작업이 진행 됨에 따라 업데이트 됩니다. 


  
<a id="ID4EVB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | --- | 
| ownerId| 문자열| 해당 리소스가 액세스 되는 사용자의 사용자 id입니다. 지원 되는 형식: "me" 또는 "xuid(123456789)"입니다. 최대 길이: 16.| 
| scid| 문자열| 서비스 액세스 되는 리소스의 ID를 구성 합니다. 인증된 된 사용자의 서비스 안내 일치 해야 합니다.| 
| gameClipId| 문자열| 액세스 하는 리소스의 ID를 클립 합니다.| 
  
<a id="ID4EAC"></a>

 
## <a name="authorization"></a>권한 부여
 
권한 부여 클레임 사용 | 클레임| 형식| 필수 여부| 예제 값| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| xuid| 64 비트 부호 있는 정수| 예| 1234567890|  | 
| TitleId| 64 비트 부호 있는 정수| 예| 1234567890| 에 사용 되는 <b>콘텐츠 격리</b> 확인 합니다.| 
| SandboxId| 16 진수 이진| 예|  | 시스템에 대 한 조회 영역을 지시 및 사용 <b>콘텐츠 격리</b> 확인 합니다.| 
  
리소스에 대 한 개인 정보 설정의 효과 | 요청 하는 사용자| 대상 사용자의 개인 정보 설정| 동작| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Me| -| 설명한 합니다.| 
| friend| 모든 사용자| 사용할 수 없습니다.| 
| friend| 친구 들과| 사용할 수 없습니다.| 
| friend| 차단됨| 사용할 수 없습니다.| 
| friend가 아닌 사용자| 모든 사용자| 사용할 수 없습니다.| 
| friend가 아닌 사용자| 친구 들과| 사용할 수 없습니다.| 
| friend가 아닌 사용자| 차단됨| 사용할 수 없습니다.| 
| 타사 사이트| 모든 사용자| 사용할 수 없습니다.| 
| 타사 사이트| 친구 들과| 사용할 수 없습니다.| 
| 타사 사이트| 차단됨| 사용할 수 없습니다.| 
 
<a id="ID4EUH"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예제 값: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 예제: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>application/json</b>합니다.| 
| 수락| 문자열| Content-type의 허용 되는 값입니다. 예: <b>application/json</b>합니다.| 
| Cache-Control| 문자열| 캐싱 동작을 지정 하는 처리 완료 후 요청입니다.| 
  
<a id="ID4EBCAC"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 문자열| 허용 가능한 압축 인코딩입니다. 예제 값: gzip, deflate, identity입니다.| 
| ETag| 문자열| 캐시 최적화를 위해 사용 합니다. 예를 들어 값: "686897696a7c876b7e".| 
| 범위| 문자열|  | 
  
<a id="ID4ETDAC"></a>

 
## <a name="request-body"></a>요청 본문
 
개체가이 요청의 본문에 전송 됩니다.
  
<a id="ID4E5DAC"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 세션을 검색 했습니다.| 
| 301| 영구적으로 이동|  | 
| 307| 임시 리디렉션|  | 
| 400| 잘못된 요청| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음| 요청에 사용자 인증이 필요합니다.| 
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청이 허용 되지 않습니다.| 
| 404| 없음| 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용 되지 않음| 리소스 버전이 지원 되지 않습니다.| 
| 408| 요청 시간 초과| 요청이 너무 길어서 완료 합니다.| 
| 410| 완료| 요청된 된 리소스를 더 이상 사용할 수 없습니다.| 
  
<a id="ID4EQIAC"></a>

 
## <a name="required-response-headers"></a>필요한 응답 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 예제: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>application/json</b>합니다.| 
| 수락| 문자열| Content-type의 허용 되는 값입니다. 예: <b>application/json</b>합니다.| 
| Cache-Control| 문자열| 캐싱 동작을 지정 하는 처리 완료 후 요청입니다.| 
| Retry-after| 문자열| 서버를 사용할 수 없는 경우 나중에 다시 시도 하도록 클라이언트를 지시 합니다. 예: <b>application/json</b>합니다.| 
| 다| 문자열| 다운스트림 프록시 응답을 캐시 하는 방법을 지시 합니다. 예: <b>application/json</b>합니다.| 
  
<a id="ID4EJLAC"></a>

 
## <a name="optional-response-headers"></a>선택적 응답 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| 문자열| 캐시 최적화를 위해 사용 합니다. 예를 들어 값: "686897696a7c876b7e".| 
  
<a id="ID4EJMAC"></a>

 
## <a name="response-body"></a>응답 본문
 
<a id="ID4EPMAC"></a>

 
### <a name="sample-response"></a>샘플 응답
 

```cpp
{
 "gameClip": {
   "xuid": "2716903703773872",
   "clipName": "1234567890",
   "titleName": "",
   "gameClipId": "cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000",
   "state": "Published",
   "dateRecorded": "2013-05-08T21:32:17.4201279Z",
   "lastModified": "2013-05-08T21:34:48.8117829Z",
   "userCaption": "",
   "type": "DeveloperInitiated",
   "source": "Console",
   "visibility": "Public",
   "durationInSeconds": 30,
   "scid": "00000000-0000-0012-0023-000000000070",
   "titleId": 0,
   "rating": 0,
   "ratingCount": 0,
   "views": 0,
   "titleData": "",
   "systemProperties": "",
   "savedByUser": false,
   "thumbnails": [
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000\/thumbnails\/large",
       "fileSize": 0,
       "thumbnailType": "Large"
     },
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000\/thumbnails\/small",
       "fileSize": 0,
       "thumbnailType": "Small"
     }
   ],
   "gameClipUris": [
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000",
       "fileSize": 9332015,
       "uriType": "Download",
       "expiration": "9999-12-31T23:59:59.9999999"
     }
   ]
 }
}
         
```

   
<a id="ID4EZMAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E2MAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/{ownerId}/scids/{scid}/clips/{gameClipId}](uri-usersowneridscidclipsgameclipid.md)

  
<a id="ID4EFNAC"></a>

 
##### <a name="further-information"></a>추가 정보 

[Marketplace Uri](../marketplace/atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   