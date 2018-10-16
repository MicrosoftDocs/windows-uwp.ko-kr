---
title: POST (/batch)
assetID: f5997c8e-82c7-0fba-9991-ce1116db4830
permalink: en-us/docs/xboxlive/rest/uri-batchpost.html
author: KevinAsgari
description: " POST (/batch)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: db832032a40b40d4b3a774a56487f7065d9cd8ff
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4619901"
---
# <a name="post-batch"></a>POST (/batch)
여러 제목 간 여러 플레이어 통계에 대 한 복잡 한 배치 요청에 대 한 GET 방법으로 작동 하는 메서드를 게시 합니다. 이러한 Uri에 대 한 도메인은 `userstats.xboxlive.com`.
 
<a id="ID4ET"></a>

 
## <a name="remarks"></a>설명
 
제목 개발자 열기 또는 XDP 또는 개발자 센터를 사용 하 여 제한 된 통계를 표시할 수 있습니다. 순위표 통계가 엽니다. 사용자가 해당 샌드박스에 인증으로 열기 통계 Smartglass, 뿐 아니라 iOS, Android, Windows, Windows Phone 및 웹 응용 프로그램에서 액세스할 수 있습니다. 사용자 권한 부여 샌드박스에 XDP 또는 개발자 센터를 통해 관리 됩니다.
  
  * [설명](#ID4ET)
  * [설명](#ID4EFB)
  * [권한 부여](#ID4EUB)
  * [필요한 요청 헤더](#ID4ETC)
  * [선택적 요청 헤더](#ID4E3D)
  * [요청 본문](#ID4EAF)
  * [HTTP 상태 코드](#ID4EWF)
  * [응답 본문](#ID4ENBAC)
 
<a id="ID4EFB"></a>

 
## <a name="remarks"></a>설명
 
호출자의 사용자, 서비스 구성 Id (SCIDs) 및 해당 통계를 검색 하려는 SCIDs 당 통계 이름 목록은 배열 메시지 본문을 제공 합니다.
 
하는 경우가 더 간단 하며 단일 통계를 검토 하는 데 유용 [GET](uri-usersxuidscidsscidstatsget.md) 메서드를 하기 전에 읽고이 더 복잡 한 배치 모드 페이지.
  
<a id="ID4EUB"></a>

 
## <a name="authorization"></a>권한 부여
 
콘텐츠 격리 및 액세스 제어 시나리오에 대 한 구현 된 권한 부여 논리가 있습니다.
 
   * 순위표와 사용자 통계 호출자가 요청을 사용 하 여 유효한 XSTS 토큰 전송 된 모든 플랫폼에서 클라이언트에서 읽을 수 있습니다. 클라이언트에서 지 원하는 쓰기 분명히 국한 합니다.
   * 제목 개발자 열기 또는 XDP 또는 개발자 센터를 사용 하 여 제한 된 통계를 표시할 수 있습니다. 순위표 통계가 엽니다. 사용자가 해당 샌드박스에 인증으로 열기 통계 Smartglass, 뿐 아니라 iOS, Android, Windows, Windows Phone 및 웹 응용 프로그램에서 액세스할 수 있습니다. 사용자 권한 부여 샌드박스에 XDP 또는 개발자 센터를 통해 관리 됩니다.
  
다음 예제에서는 검사에 대 한 의사 코드입니다.
 

```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.
         
```

  
<a id="ID4ETC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: "XBL3.0 x =&lt;userhash >; &lt;토큰 > ".| 
  
<a id="ID4E3D"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion|  | 이 요청 전달 되어야 하는 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 서비스의 인증 토큰을 클레임 헤더의 유효성을 확인 한 후에 있습니다. 기본값: 1.| 
  
<a id="ID4EAF"></a>

 
## <a name="request-body"></a>요청 본문
 
<a id="ID4EIF"></a>

 
### <a name="sample-request"></a>샘플 요청
 
POST 본문은 다음 두 가지 다른 사용자에 대 한 두 개의 다른 SCIDs에서 4 개의 통계 요청 되는 서비스에 알립니다.
 

```cpp
{    
"requestedusers": [
                1234567890123460,
                1234567890123234
            ],
            "requestedscids": [
                {
                    "scid": "c402ff50-3e76-11e2-a25f-0800200c1212",
                    "requestedstats": [
                        "Test4FirefightKills",
                        "Test4FirefightHeadshots"
                    ]
                },
                {
                    "scid": "c402ff50-3e76-11e2-a25f-0800200c0343",
                    "requestedstats": [
                        "OverallTestKills",
                        "TestHeadshots"
                    ]
                }
            ] 
}
      
```

   
<a id="ID4EWF"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스 사용 되는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 세션을 검색 했습니다.| 
| 304| 수정 되지 않음| 리소스 되지 요청 마지막으로 수정 합니다.| 
| 400| 잘못 된 요청| 서비스 잘못 된 요청을 이해 하지 못했습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음| 요청은 사용자 인증이 필요합니다.| 
| 403| 금지| 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 404| 찾을 수 없음| 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용할 수 없음| 리소스 버전은 지원 되지 않습니다.| 
| 408| 요청 시간 제한| 리소스 버전이 지원 되지 않습니다. MVC 계층에 의해 거부 되어야 합니다.| 
  
<a id="ID4ENBAC"></a>

 
## <a name="response-body"></a>응답 본문
 
<a id="ID4EXBAC"></a>

 
### <a name="sample-response"></a>예제 응답
 

```cpp
{    
   "users":[          
       {    
      "xuid": "123456789"
        "gamertag": "WarriorSaint",
        "scids":[
          {
             "scid":"c402ff50-3e76-11e2-a25f-0800200c1212",
             "stats":  [
          {
                 "statname":"Test4FirefightKills",
                 "type":"Integer",
                 "value":7
             },
          {
                 "statname":"Test4FirefightHeadshots",
                 "type":"Integer",
                 "value":4
             }]
                        },
          {
             "scid":"c402ff50-3e76-11e2-a25f-0800200c0343",
             "stats":  [
          {
                 "statname":"OverallTestKills",
                 "type":"Integer",
                 "value":3434
             },
          {
                 "statname":"TestHeadshots",
                 "type":"Integer",
                 "value":41
             }]
          }],
                   },
    {    
                   "gamertag":"TigerShark",
                   "xuid":1234567890123234
        "scids":[
          {
             "scid":"123456789",
             "stats":  [
          {
                 "statname":"Test4FirefightKills",
                 "type":"Integer",
                 "value":63
             },
          {
                 "statname":"Test4FirefightHeadshots",
                 "type":"Integer",
                 "value":12
             }]
                        },
          {
"scid":"987654321",
             "stats":  [
          {
                 "statname":"OverallTestKills",
                 "type":"Integer",
                 "value":375
             },
          {
                 "statname":"TestHeadshots",
                 "type":"Integer",
                 "value":34
             }]
          }],
                   }]
}
         
```

   
<a id="ID4EDCAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EFCAC"></a>

 
##### <a name="parent"></a>부모 

[/batch](uri-batch.md)

   