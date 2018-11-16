---
title: POST (/titles/{Title Id}/sessionhosts)
assetID: 8558b336-1af9-8143-9752-477ceb3a8e4e
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionhosts-post.html
author: KevinAsgari
description: " POST (/titles/{Title Id}/sessionhosts)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dbbc7baf12daea485dec22389846e5e4acec16c1
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6853012"
---
# <a name="post-titlestitle-idsessionhosts"></a>POST (/titles/{Title Id}/sessionhosts)
새 클러스터 요청을 만듭니다. 이러한 Uri에 대 한 도메인은 `gameserverms.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EX)
  * [필요한 요청 헤더](#ID4EGB)
  * [요청 본문](#ID4E5B)
  * [필요한 응답 헤더](#ID4ELD)
  * [응답 본문](#ID4ESD)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 설명| 
| --- | --- | 
| titleId| 요청에서 작동 해야 하는 타이틀의 ID입니다.| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>호스트 이름

gameserverms.xboxlive.com
 
<a id="ID4EGB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
요청을 만들 때 다음 표에 표시 된 헤더는 필요 합니다.
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | 
| 콘텐츠 유형| application/json| 제출 되는 데이터 형식입니다.| 
  
<a id="ID4E5B"></a>

 
## <a name="request-body"></a>요청 본문
 
요청에는 다음 멤버가 포함 된 JSON 개체를 포함 해야 합니다.
 
| 멤버| 설명| 
| --- | --- | --- | --- | --- | --- | --- | 
| sessionId| 이 호출자가 지정한 식별자입니다. 할당 되 고 반환 되는 세션 호스트에 할당 됩니다. 나중에이 식별자가 특정 sessionhost를 참조할 수 있습니다. 고유한 이어야 합니다 (예: GUID).| 
| SandboxId| 샌드박스 원하는 세션 호스트에 할당 해야 합니다.| 
| cloudGameId| 클라우드 게임 식별자입니다.| 
| 위치| 순서가 지정 된 목록에서 할당할 세션 원하는 기본 위치입니다.| 
| sessionCookie| 이 지정 된 호출자 불투명 문자열입니다. sessionhost와 연결 하 고 게임 코드에서 참조할 수 있습니다. 이 멤버를 사용 하 여 (최대 크기는 4KB) 서버를 클라이언트에서 적은 양의 정보를 전달 합니다.| 
| gameModelId| 게임 모드 식별자입니다.| 
 
<a id="ID4EDD"></a>

 
### <a name="sample-request"></a>샘플 요청
 

```cpp
{
        "sessionId": "3536d3e6-e85d-4f47-b898-9617d19dabcd",
        "sandboxId": "ISST.0",
        "cloudGameId": "1b7f9925-369c-4301-b1f7-1125dce25776",
        "locations": [
        "West US",
        "East US",
        "West Europe"
        ],
        "sessionCookie": "Caller provided opaque string",
        "gameModeId": "2162d32c-7ac8-40e9-9b1f-56676b8b2513"
        }
      
```

   
<a id="ID4ELD"></a>

 
## <a name="required-response-headers"></a>필요한 응답 헤더
 
없음.
  
<a id="ID4ESD"></a>

 
## <a name="response-body"></a>응답 본문
 
호출 되 면 서비스는 다음 멤버가 포함 된 JSON 개체를 반환 합니다.
 
| 멤버| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 호스트 이름| 인스턴스의 호스트 이름입니다.| 
| portMappings| 포트 매핑을 합니다.| 
| 지역| 인스턴스 지역에서 호스팅됩니다.| 
| secureContext| 보안 장치 주소입니다.| 
 
<a id="ID4ESE"></a>

 
### <a name="sample-response"></a>예제 응답
 

```cpp
{
        "hostName": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.cloudapp.net",
        "portMappings": [
        {
        "Key": "GSDKTCPTest",
        "Value": {
        "internal": 10100,
        "external": 10103
        }
        },
        {
        "Key": "GSDKUDPTest",
        "Value": {
        "internal": 5000,
        "external": 5000
        }
        }
        ],
        "region": "WestUS",
        "secureContext": "AQDc8Hen/QCDJwWRPcW/1QEEAiABAACdOJU8JNujcXyUPwUBCnue+g=="
        }
      
```

   
<a id="remarks"></a>

 
## <a name="remarks"></a>설명
 
다음과 같은 응답 코드를 받는 경우 제목을 다시 호출 서비스에만 해야:
 
   * 200-성공-응답을 반환 합니다.
   * 400-잘못 된 요청 본문 또는 매개 변수가 잘못 되었습니다.
   * 401-권한이 없음
   * 404-제목 id에 할당 된 모든 구독 필요는 없습니다.
   * 409-대략 동시에 동일한 요청 (동일한 sessionId) 결정이 응답 가능한 됩니다. 할당 요청 작업이 수행 하 고 세션 호스트에 이미 지정된 sessionId 이미 활성 상태인 경우에서는 해당 sessionhost 자세히 설명 하는 정보를 반환 합니다. 하지만 세션 호스트 없는 경우 활성 아직 충돌이 발생 합니다.
   * 500-예기치 않은 서버 오류입니다.
   * 503-sessionhosts StandingBy 없습니다. 이러한 리소스 중 일부는 무료 때 요청을 다시 시도 합니다.
   
<a id="ID4EFG"></a>

 
## <a name="see-also"></a>참고 항목
 [/titles/{titleId}/sessionhosts](uri-titlestitleidsessionhosts.md)

  