---
title: GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)
assetID: 613ba53f-03cb-5ed3-a5ba-be59e5a146d1
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionssessionidallocationstatus-get.html
author: KevinAsgari
description: " GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1e351bed37e0761be1f884400f81a3da537967d2
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5483510"
---
# <a name="get-titlestitleidsessionssessionidallocationstatus"></a>GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)
해당 세션 Id로 식별 되는 sessionhost의 할당 상태를 반환 합니다. 이러한 Uri에 대 한 도메인은 `gameserverds.xboxlive.com` 및 `gameserverms.xboxlive.com`.
 
  * [필요한 요청 헤더](#ID4E4)
  * [필수 응답 헤더](#ID4EEB)
  * [응답 본문](#ID4ELB)
 
<a id="ID4E4"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
없음.
  
<a id="ID4EEB"></a>

 
## <a name="required-response-headers"></a>필수 응답 헤더
 
없음.
  
<a id="ID4ELB"></a>

 
## <a name="response-body"></a>응답 본문
 
호출이 성공 하면 서비스가 다음과 같은 멤버가 포함 된 JSON 개체가 반환 됩니다.
 
| 구성원| 설명| 
| --- | --- | 
| description| 빈 (왼쪽에 대 한 이전 버전과 호환성) 하는 문자열을 반환 합니다.| 
| clusterId| 빈 (왼쪽에 대 한 이전 버전과 호환성) 하는 문자열을 반환 합니다.| 
| 호스트 이름| 세션 호스트의 URL입니다.| 
| status| 대기, 수행, 또는 중단 되었습니다 나타냅니다.| 
| sessionHostId| 세션 호스트 id입니다.| 
| 세션 Id| (할당 시)에 제공 되는 클라이언트 세션의 id입니다.| 
| secureContext| 보안 장치 주소입니다.| 
| portMappings| 인스턴스에 대 한 포트 매핑을 합니다.| 
| 지역| 인스턴스의 위치입니다.| 
| ticketId| 현재 세션 ID (왼쪽에 대 한 이전 버전과 호환성).| 
| gameHostId| (왼쪽에 대 한 이전 버전과 호환성) 현재 sessionHostId.| 
 
<a id="ID4EGD"></a>

 
### <a name="sample-response"></a>샘플 응답입니다.
 

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
 
다음 응답 코드를 받았을 때 제목을 다시 호출 서비스에만 해야.
 
   * 200-성공 
   * 400-요청이 잘못 된 매개 변수를 포함 합니다. 
   * 401-권한 없음 
   * 404-제목 ID 또는 티켓 ID 잘못 되었거나 찾을 수 없습니다. 
   * 500-예기치 않은 서버 오류입니다. 
    