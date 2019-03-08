---
title: POST (/batch)
assetID: f5997c8e-82c7-0fba-9991-ce1116db4830
permalink: en-us/docs/xboxlive/rest/uri-batchpost.html
description: " POST (/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a854fc830c87afbf675a379599916bf3db919539
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645838"
---
# <a name="post-batch"></a>POST (/batch)
메서드를 통해 여러 제목을 여러 플레이어 통계에 대 한 복잡 한 일괄 처리 요청을 GET 메서드로 함수를 게시 합니다. 이러한 Uri에 대 한 도메인은 `userstats.xboxlive.com`합니다.
 
<a id="ID4ET"></a>

 
## <a name="remarks"></a>설명
 
제목 개발자에 따라 개방 또는 제한 XDP 또는 파트너 센터를 사용 하 여 통계를 표시할 수 있습니다. 순위표 통계가 엽니다. 사용자는 샌드박스에 허용으로 열기 통계 Smartglass, 뿐만 아니라 iOS, Android, Windows, Windows Phone 및 웹 응용 프로그램에서 액세스할 수 있습니다. 사용자 권한 부여는 샌드박스에 XDP 또는 파트너 센터를 통해 관리 됩니다.
  
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
 
호출자의 사용자, 서비스 구성 (SCIDs) Id 및 해당 통계를 검색할 SCIDs 당 통계 이름의 목록의 배열을 사용 하 여 메시지 본문을 제공 합니다.
 
것은 간단 하 고 단일 통계를 검토 하는 것이 유용한 [가져올](uri-usersxuidscidsscidstatsget.md) 이 더 복잡 한 일괄 처리 모드 페이지를 읽기 전에 메서드.
  
<a id="ID4EUB"></a>

 
## <a name="authorization"></a>권한 부여
 
콘텐츠 격리 및 액세스 제어 시나리오에 대해 구현 하는 권한 부여 논리가 있습니다.
 
   * 순위표 및 사용자 통계는 호출자에 게 유효한 XSTS 토큰 요청과 함께 전송 된 모든 플랫폼에서 클라이언트에서 읽을 수 있습니다. 쓰기는 물론에서 지 원하는 클라이언트를 제한 합니다.
   * 제목 개발자에 따라 개방 또는 제한 XDP 또는 파트너 센터를 사용 하 여 통계를 표시할 수 있습니다. 순위표 통계가 엽니다. 사용자는 샌드박스에 허용으로 열기 통계 Smartglass, 뿐만 아니라 iOS, Android, Windows, Windows Phone 및 웹 응용 프로그램에서 액세스할 수 있습니다. 사용자 권한 부여는 샌드박스에 XDP 또는 파트너 센터를 통해 관리 됩니다.
  
다음 예제에서는 의사 (pseudo) 코드 검사입니다.
 

```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.
         
```

  
<a id="ID4ETC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예를 들어 값: "XBL3.0 x=&lt;userhash>;&lt;token>".| 
  
<a id="ID4E3D"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | 빌드는이 요청을 보내야 하는 서비스의 이름/번호입니다. 요청만으로 라우팅되는 헤더, 인증 토큰의 클레임의 유효성을 확인 한 후 서비스는 합니다. 기본값: 1.| 
  
<a id="ID4EAF"></a>

 
## <a name="request-body"></a>요청 본문
 
<a id="ID4EIF"></a>

 
### <a name="sample-request"></a>샘플 요청
 
다음 POST 본문을 4 개의 통계에서 두 개의 다른 SCIDs 두 명의 사용자가 요청 되는 서비스에 알립니다.
 

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
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 세션을 검색 했습니다.| 
| 304| 수정 안 됨| 리소스 되지 마지막 요청 이후 수정 합니다.| 
| 400| 잘못된 요청| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음| 요청에 사용자 인증이 필요합니다.| 
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청이 허용 되지 않습니다.| 
| 404| 없음| 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용 되지 않음| 리소스 버전이 지원 되지 않습니다.| 
| 408| 요청 시간 초과| 리소스 버전이 지원 되지 않습니다. MVC 계층에서 거부 해야 합니다.| 
  
<a id="ID4ENBAC"></a>

 
## <a name="response-body"></a>응답 본문
 
<a id="ID4EXBAC"></a>

 
### <a name="sample-response"></a>샘플 응답
 

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

 
##### <a name="parent"></a>Parent 

[/batch](uri-batch.md)

   