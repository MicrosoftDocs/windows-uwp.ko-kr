---
title: GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)
assetID: 613ba53f-03cb-5ed3-a5ba-be59e5a146d1
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionssessionidallocationstatus-get.html
description: " GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 793d634bc1e3dc431b3797759751afb6dfd9c00a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650858"
---
# <a name="get-titlestitleidsessionssessionidallocationstatus"></a>GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)
해당 세션 Id로 식별 sessionhost의 할당 상태를 반환 합니다. 이러한 Uri에 대 한 도메인이 `gameserverds.xboxlive.com` 고 `gameserverms.xboxlive.com`입니다.
 
  * [필요한 요청 헤더](#ID4E4)
  * [필요한 응답 헤더](#ID4EEB)
  * [응답 본문](#ID4ELB)
 
<a id="ID4E4"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
없음.
  
<a id="ID4EEB"></a>

 
## <a name="required-response-headers"></a>필요한 응답 헤더
 
없음.
  
<a id="ID4ELB"></a>

 
## <a name="response-body"></a>응답 본문
 
호출이 성공 하면 서비스는 다음 멤버로 구성 된 JSON 개체를 반환 합니다.
 
| 멤버| 설명| 
| --- | --- | 
| description| 빈 문자열 (왼쪽에서 대 한 이전 버전과 호환성)를 반환 합니다.| 
| clusterId| 빈 문자열 (왼쪽에서 대 한 이전 버전과 호환성)를 반환 합니다.| 
| hostName| 세션 호스트의 URL입니다.| 
| status| 큐에 대기, 처리 또는 중단을 나타냅니다.| 
| sessionHostId| 세션 호스트 id입니다.| 
| sessionId| (할당 시) 제공 된 클라이언트 세션 id입니다.| 
| secureContext| 보안 장치 주소입니다.| 
| portMappings| 인스턴스에 대 한 포트 매핑입니다.| 
| 지역| 인스턴스의 위치입니다.| 
| ticketId| (왼쪽에서 대 한 이전 버전과 호환성) 현재 세션 ID입니다.| 
| gameHostId| (왼쪽에서 대 한 이전 버전과 호환성) 현재 sessionHostId 합니다.| 
 
<a id="ID4EGD"></a>

 
### <a name="sample-response"></a>샘플 응답
 

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
        "status",:"Fulfilled",
        "region": "WestUS",
        "secureContext": "AQDc8Hen/QCDJwWRPcW/1QEEAiABAACdOJU8JNujcXyUPwUBCnue+g==",
        "sessionId": "05328154-1bbe-4f5b-8caa-4e44106712f9",
        "description": "",
        "clusterId": "",
        "sessionHostId": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.GSDKAgent_IN_0.0",
        "ticketId": "05328154-1bbe-4f5b-8caa-4e44106712f9",
        "gameHostId": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.GSDKAgent_IN_0.0"

      
```

  
<a id="remarks"></a>

 
### <a name="remarks"></a>설명
 
제목을 다음 응답 코드를 받을 때 서비스에 대 한 호출 다시 시도 해야 합니다.
 
   * 200-성공 
   * 400-잘못 된 매개 변수를 포함 하는 요청 
   * 401-권한이 없음 
   * 404-제목 ID 또는 티켓 ID가 잘못 되었거나 찾을 수 없음 
   * 500-예기치 않은 서버 오류입니다. 
    