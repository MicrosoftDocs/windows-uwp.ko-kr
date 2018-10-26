---
title: GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
assetID: 27318886-f084-d6a8-e582-3eb070ccbc38
permalink: en-us/docs/xboxlive/rest/uri-usersxuidachievementsscidachievementidget.html
author: KevinAsgari
description: " GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c36b2c70f525029032c67208c9e2a9266d421010
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5570170"
---
# <a name="get-usersxuidxuidachievementsscidachievementid"></a>GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
도전 과제의 세부 정보를 가져옵니다. 이러한 Uri에 대 한 도메인은 `achievements.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
  * [권한 부여](#ID4EAB)
  * [리소스의 개인 정보 설정의 효과](#ID4E4C)
  * [필요한 요청 헤더](#ID4EPG)
  * [선택적 요청 헤더](#ID4EPH)
  * [요청 본문](#ID4ECBAC)
  * [HTTP 상태 코드](#ID4ENBAC)
  * [응답 본문](#ID4EBGAC)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID) 해당 리소스를 액세스 하는 사용자의 합니다. 인증된 된 사용자의 XUID 일치 해야 합니다.| 
| 서비스 안내| GUID| 해당 도전 과제를 액세스 하는 서비스 구성의 고유 식별자입니다.| 
| achievementid| 32 비트 부호 없는 정수| 액세스 되는 도전 과제의 (지정 된 서비스 안내) 내에서 고유 식별자입니다.| 
  
<a id="ID4EAB"></a>

 
## <a name="authorization"></a>권한 부여
 
사용 권한 부여 클레임 | 클레임| 필수 여부| 설명| 누락 된 동작| 
| --- | --- | --- | --- | --- | --- | --- | 
| 사용자| 예| Xbox LIVE는 요청 작업이 수행 누구를 위해에 유효한 사용자.| 403 사용할 수 없음| 
| 제목| 아니요| 호출 제목입니다.| 인증에 따라 다릅니다. 2013, 월 1 일 기준 인증 누락 하 고 공용으로 표시 되지 않은 모든 SCIDs에 대 한 액세스를 거부 따라서는 클레임을 제공 하지 않습니다.| 
| 샌드박스| 아니요| 결과 검색할 샌드박스입니다.| 인증에 따라 다릅니다. 2013, 월 1 일 기준 인증 누락 된 경우 기본 클레임을 제공 하지 않습니다.| 
  
<a id="ID4E4C"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>리소스의 개인 정보 설정의 효과
 
리소스의 개인 정보 설정의 효과 | 사용자를 요청합니다.| 대상 사용자의 개인 정보 설정| 동작| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 옵션인 추가 정보| -| 설명한 합니다.| 
| 친구| 모든 사용자| 확인| 
| 친구| 친구만| 확인| 
| 친구| 차단| 사용할 수 없습니다.| 
| 비 친구 사용자| 모든 사용자| 확인| 
| 비 친구 사용자| 친구만| 사용할 수 없습니다.| 
| 비 친구 사용자| 차단| 사용할 수 없습니다.| 
| 제 3 자 사이트| 모든 사용자| 확인| 
| 제 3 자 사이트| 친구만| 사용할 수 없습니다.| 
| 제 3 자 사이트| 차단| 사용할 수 없습니다.| 
  
<a id="ID4EPG"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: "XBL3.0 x =&lt;userhash >; &lt;토큰 > ".| 
  
<a id="ID4EPH"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion| string| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 인증 토큰의 클레임 헤더의 유효성을 확인 한 후 서비스는 합니다. 기본값: 1입니다.| 
| xbl 계약 버전 x| string| V1 기본값입니다.| 
| Accept Language| string| 원하는 로캘 및 대체 (예: FR-FR, fr, EN-US ~ EN-GB, en 전세계) 목록입니다. 지역화 된 문자열을 일치를 찾을 때까지 도전 과제 서비스 목록을 통해 작동 합니다. 발견 되 면 사용자의 IP 주소에서 제공 되는 사용자 토큰에 정의 된 위치와 일치 하도록 시도 합니다. 없는 일치 하는 지역화 된 문자열을 여전히 발견 되 면 제목 개발자/게시자가 제공 하는 기본 문자열을 사용 합니다. | 
  
<a id="ID4ECBAC"></a>

 
## <a name="request-body"></a>요청 본문
 
개체가이 요청 본문에 전송 됩니다.
  
<a id="ID4ENBAC"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스에 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 세션을 검색 했습니다.| 
| 301| 영구적으로 이동| 서비스는 다른 URI로 이동 합니다.| 
| 307| 임시 리디렉션| 이 리소스에 대 한 URI 일시적으로 바뀌었습니다.| 
| 400| 잘못 된 요청| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음| 필요한 사용자 인증을 요청 합니다.| 
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 404| 찾을 수 없습니다.| 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용할 수 없음| 리소스 버전이 지원 되지 않습니다. MVC 계층에 의해 거부 되어야 합니다.| 
| 408| 요청 시간 제한| 요청을 완료 하는 데 너무 오래 걸렸습니다.| 
| 410| 최신 상태가 아닌| 요청 된 리소스는 더 이상 사용할 수 없습니다.| 
  
<a id="ID4EBGAC"></a>

 
## <a name="response-body"></a>응답 본문
 
<a id="ID4EHGAC"></a>

 
### <a name="sample-response"></a>예제 응답
 

```cpp
{
    "achievement":
    {
        "id":"3",
        "serviceConfigId":"b5dd9daf-0000-0000-0000-000000000000",
        "name":"Default NameString for Microsoft Achievements Sample Achievement 3",
        "titleAssociations":
        [{
                "name":"Microsoft Achievements Sample",
                "id":3051199919,
                "version":"abc"
        }],
        "progressState":"Achieved",
        "progression":
        {
                "requirements":null,
                "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
        },
        "mediaAssets":
        [{
                "name":"Icon Name",
                "type":"Icon",
                "url":"http://www.xbox.com"
        }],
        "platform":"D",
        "isSecret":true,
        "description":"Default DescriptionString for Microsoft Achievements Sample Achievement 3",
        "lockedDescription":"Default UnachievedString for Microsoft Achievements Sample Achievement 3",
        "productId":"12345678-1234-1234-1234-123456789012",
        "achievementType":"Challenge",
        "participationType":"Individual",
        "timeWindow":
        {
                "startDate":"2013-02-01T00:00:00Z",
                "endDate":"2100-07-01T00:00:00Z"
        },
        "rewards":
        [{
                "name":null,
                "description":null,
                "value":"10",
                "type":"Gamerscore",
                "valueType":"Int"
        },
        {
                "name":"Default Name for InAppReward for Microsoft Achievements Sample Achievement 3",
                "description":"Default Description for InAppReward for Microsoft Achievements Sample Achievement 3",
                "value":"1",
                "type":"InApp",
                "valueType":"String"
        }],
        "estimatedTime":"06:12:14",
        "deeplink":"aWFtYWRlZXBsaW5r",
        "isRevoked":false
    }
}
         
```

   
<a id="ID4ERGAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ETGAC"></a>

 
##### <a name="parent"></a>부모 

[/users/xuid({xuid})/achievements/{scid}/{achievementid}](uri-usersxuidachievementsscidachievementid.md)

   