---
title: GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
assetID: dbd60c93-9d8e-609b-0ae3-b3f7ee26ba2d
permalink: en-us/docs/xboxlive/rest/uri-usersowneridscidclipsgameclipidget.html
author: KevinAsgari
description: " GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 13b96b0d2f1f674533dd2c070bd1a10884bb7370
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5440026"
---
# <a name="get-usersowneridscidsscidclipsgameclipid"></a>GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
찾으면 모든 Id에 알려진 경우 시스템에서 단일 게임 클립을 가져옵니다. 이러한 Uri에 대 한 도메인은 `gameclipsmetadata.xboxlive.com` 및 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.
 
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
 
클립에 대 한 쿼리를 모든 데이터는 모든 메타 데이터 쿼리에서 반환한 **GameClip** 개체에서 사용할 수 있습니다. 이 API 통해 단일 게임 클립을 가져오려면 **XUID**, **ServiceConfigId**, **GameClipId** 및 **SandboxId** 요청의 클레임에 필요 합니다. 게임 클립 적용 되도록 지정 또는 사용자를 특정 게임 클립을 가져올 권한이 없는 확인 검사 콘텐츠 격리 또는 개인 정보, API 404 (찾을 수 없음)의 HTTP 상태 코드를 반환 합니다.
 
이제 **SandboxId** 는 XToken 클레임이에서 검색 이며 적용 합니다. **SandboxId** 없으면 엔터테인먼트 검색 서비스 (EDS) 400 잘못 된 요청 오류를 throw 합니다.
 
이 API는 만료 된 Uri를 새로 고치려면도 사용 합니다. 쿼리가 완료 되 면 게임 클립에 대해 만료 된 모든 Uri가 새로 고쳐집니다 적절 하 게 합니다. 

> [!NOTE] 
> URI 새로 고침 최대 40 30 초가이 요청 작업이 끝나면 걸릴 수 있습니다. URI 만료 되 면 경우 스트리밍 작업에 즉시 사용 하려고 시도 하면 IIS 부드러운 스트리밍 서버에서 500 HTTP 상태 코드를 발생 합니다. 이 단축 하는 방법을 노력 하 고이 참고 해당 작업 진행 됨에 따라 그에 따라 업데이트 됩니다. 


  
<a id="ID4EVB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | 
| ownerId| string| 해당 리소스를 액세스 하는 사용자의 사용자 id입니다. 지원 되는 형식: "me" 또는 "xuid(123456789)". 최대 길이: 16입니다.| 
| 서비스 안내| string| 액세스 되는 리소스의 서비스 구성 ID입니다. 인증된 된 사용자의 서비스 안내 일치 해야 합니다.| 
| gameClipId| string| GameClip ID 액세스 되는 리소스입니다.| 
  
<a id="ID4EAC"></a>

 
## <a name="authorization"></a>권한 부여
 
사용 권한 부여 클레임 | 클레임| 유형| 필수 여부| 예제 값| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Xuid| 64 비트의 부호 있는 정수| 예| 1234567890|  | 
| TitleId| 64 비트의 부호 있는 정수| 예| 1234567890| <b>콘텐츠 격리</b> 검사에 사용 됩니다.| 
| SandboxId| 16 진수 이진 파일| 예|  | 시스템이, 조회에 대 한 올바른 영역 및 <b>콘텐츠 격리</b> 검사에 사용 합니다.| 
  
리소스의 개인 정보 설정의 효과 | 사용자를 요청합니다.| 대상 사용자의 개인 정보 설정| 동작| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 옵션인 추가 정보| -| 설명한 합니다.| 
| 친구| 모든 사용자| 사용할 수 없습니다.| 
| 친구| 친구만| 사용할 수 없습니다.| 
| 친구| 차단| 사용할 수 없습니다.| 
| 비 친구 사용자| 모든 사용자| 사용할 수 없습니다.| 
| 비 친구 사용자| 친구만| 사용할 수 없습니다.| 
| 비 친구 사용자| 차단| 사용할 수 없습니다.| 
| 제 3 자 사이트| 모든 사용자| 사용할 수 없습니다.| 
| 제 3 자 사이트| 친구만| 사용할 수 없습니다.| 
| 제 3 자 사이트| 차단| 사용할 수 없습니다.| 
 
<a id="ID4EUH"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: <b>Xauth =&lt;authtoken ></b>| 
| X RequestedServiceVersion| string| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 인증 토큰 등의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우트됩니다 됩니다. 예: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 수락| string| 콘텐츠 형식의 허용 되는 값입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 캐시 제어| string| 정중 요청 캐싱 동작을 지정 합니다.| 
  
<a id="ID4EBCAC"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| string| Encodings 허용 가능한 압축 합니다. 예제 값: gzip만 줄이기 identity 합니다.| 
| ETag| string| 캐시 최적화에 사용 됩니다. 예제 값: "686897696a7c876b7e".| 
| 범위| string|  | 
  
<a id="ID4ETDAC"></a>

 
## <a name="request-body"></a>요청 본문
 
개체가이 요청 본문에 전송 됩니다.
  
<a id="ID4E5DAC"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스에 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 세션을 검색 했습니다.| 
| 301| 영구적으로 이동|  | 
| 307| 임시 리디렉션|  | 
| 400| 잘못 된 요청| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음| 필요한 사용자 인증을 요청 합니다.| 
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 404| 찾을 수 없습니다.| 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용할 수 없음| 리소스 버전이 지원 되지 않습니다.| 
| 408| 요청 시간 제한| 요청을 완료 하는 데 너무 오래 걸렸습니다.| 
| 410| 최신 상태가 아닌| 요청 된 리소스는 더 이상 사용할 수 없습니다.| 
  
<a id="ID4EQIAC"></a>

 
## <a name="required-response-headers"></a>필요한 응답 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion| string| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 인증 토큰 등의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우트됩니다 됩니다. 예: 1, vnext 합니다.| 
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 수락| string| 콘텐츠 형식의 허용 되는 값입니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 캐시 제어| string| 정중 요청 캐싱 동작을 지정 합니다.| 
| 다시 시도 후| string| 클라이언트는 사용할 수 없는 서버의 경우 나중에 다시 시도 하도록 지시 합니다. 예: <b>응용 프로그램/j</b>합니다.| 
| 다| string| 응답을 캐시 하는 방법을 다운스트림 프록시에 지시 합니다. 예: <b>응용 프로그램/j</b>합니다.| 
  
<a id="ID4EJLAC"></a>

 
## <a name="optional-response-headers"></a>선택적 응답 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| string| 캐시 최적화에 사용 됩니다. 예제 값: "686897696a7c876b7e".| 
  
<a id="ID4EJMAC"></a>

 
## <a name="response-body"></a>응답 본문
 
<a id="ID4EPMAC"></a>

 
### <a name="sample-response"></a>예제 응답
 

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

 
##### <a name="parent"></a>부모 

[/users/{ownerId}/scids/{scid}/clips/{gameClipId}](uri-usersowneridscidclipsgameclipid.md)

  
<a id="ID4EFNAC"></a>

 
##### <a name="further-information"></a>자세한 정보 

[마켓플레이스 URI](../marketplace/atoc-reference-marketplace.md)

 [추가 참조](../../additional/atoc-xboxlivews-reference-additional.md)

   