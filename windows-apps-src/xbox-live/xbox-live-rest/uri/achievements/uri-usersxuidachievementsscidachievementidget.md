---
title: GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
assetID: 27318886-f084-d6a8-e582-3eb070ccbc38
permalink: en-us/docs/xboxlive/rest/uri-usersxuidachievementsscidachievementidget.html
description: " GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 19083937d48d8c312215f1734513d83c59f52f0d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627508"
---
# <a name="get-usersxuidxuidachievementsscidachievementid"></a>GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
도전 과제의 세부 정보를 가져옵니다. 이러한 Uri에 대 한 도메인은 `achievements.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EV)
  * [권한 부여](#ID4EAB)
  * [리소스에 대 한 개인 정보 설정의 효과](#ID4E4C)
  * [필요한 요청 헤더](#ID4EPG)
  * [선택적 요청 헤더](#ID4EPH)
  * [요청 본문](#ID4ECBAC)
  * [HTTP 상태 코드](#ID4ENBAC)
  * [응답 본문](#ID4EBGAC)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID) 해당 리소스에 액세스 하는 사용자입니다. 인증된 된 사용자의 XUID 일치 해야 합니다.| 
| scid| GUID| 해당 도전 과제에 액세스 하는 서비스 구성의 고유 식별자입니다.| 
| achievementid| 32 비트 부호 없는 정수| 액세스 하는 성과의 (내에서 지정된 된 서비스 안내) 고유 식별자입니다.| 
  
<a id="ID4EAB"></a>

 
## <a name="authorization"></a>권한 부여
 
권한 부여 클레임 사용 | 클레임| 필수 여부| 설명| 누락 된 경우 동작| 
| --- | --- | --- | --- | --- | --- | --- | 
| 사용자| 예| Xbox LIVE 요청이 수행 되 고 사용자의 유효한 사용자입니다.| 403 Forbidden| 
| 제목| 아니오| 호출 제목입니다.| AuthZ에 따라 달라 집니다. 2013 년 5 월 1 일자로 AuthZ에서 public으로 표시 되지 않은 모든 SCIDs에 액세스를 거부 하므로 누락 된 경우 클레임을 제공 하지 않습니다.| 
| 샌드박스| 아니오| 샌드박스 결과 검색 합니다.| AuthZ에 따라 달라 집니다. 2013 년 5 월 1 일자로 AuthZ 제공 하지 않는 기본 클레임 누락 된 경우.| 
  
<a id="ID4E4C"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>리소스에 대 한 개인 정보 설정의 효과
 
리소스에 대 한 개인 정보 설정의 효과 | 요청 하는 사용자| 대상 사용자의 개인 정보 설정| 동작| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Me| -| 설명한 합니다.| 
| friend| 모든 사용자| 확인| 
| friend| 친구 들과| 확인| 
| friend| 차단됨| 사용할 수 없습니다.| 
| friend가 아닌 사용자| 모든 사용자| 확인| 
| friend가 아닌 사용자| 친구 들과| 사용할 수 없습니다.| 
| friend가 아닌 사용자| 차단됨| 사용할 수 없습니다.| 
| 타사 사이트| 모든 사용자| 확인| 
| 타사 사이트| 친구 들과| 사용할 수 없습니다.| 
| 타사 사이트| 차단됨| 사용할 수 없습니다.| 
  
<a id="ID4EPG"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예를 들어 값: "XBL3.0 x=&lt;userhash>;&lt;token>".| 
  
<a id="ID4EPH"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 헤더, 인증 토큰의 클레임의 유효성을 확인 한 후 서비스는 합니다. 기본값: 1.| 
| x-xbl-contract-version| 문자열| V1에 대 한 기본값입니다.| 
| Accept-Language| 문자열| 원하는 로캘 및 대체 (예:-FR, fr, EN-GB, en WW, EN-US)의 목록입니다. 일치 하는 지역화 된 문자열을 찾으면 성과 서비스 목록을 통해 작동 합니다. 일치 항목이 없는 경우, 사용자의 IP 주소에서 제공 되는 사용자 토큰에 정의 된 위치와 일치 하도록 시도 합니다. 없는 일치 하는 지역화 된 문자열에 여전히 발견 되 면 제목 개발자/게시자가 제공 하는 기본 문자열을 사용 합니다. | 
  
<a id="ID4ECBAC"></a>

 
## <a name="request-body"></a>요청 본문
 
개체가이 요청의 본문에 전송 됩니다.
  
<a id="ID4ENBAC"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인| 세션을 검색 했습니다.| 
| 301| 영구적으로 이동| 서비스는 다른 URI로 이동 되었습니다.| 
| 307| 임시 리디렉션| 이 리소스에 대 한 URI 일시적으로 변경 되었습니다.| 
| 400| 잘못된 요청| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음| 요청에 사용자 인증이 필요합니다.| 
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청이 허용 되지 않습니다.| 
| 404| 없음| 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용 되지 않음| 리소스 버전이 지원 되지 않습니다. MVC 계층에서 거부 해야 합니다.| 
| 408| 요청 시간 초과| 요청이 너무 길어서 완료 합니다.| 
| 410| 완료| 요청된 된 리소스를 더 이상 사용할 수 없습니다.| 
  
<a id="ID4EBGAC"></a>

 
## <a name="response-body"></a>응답 본문
 
<a id="ID4EHGAC"></a>

 
### <a name="sample-response"></a>샘플 응답
 

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
                "url":"https://www.xbox.com"
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

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/achievements/{scid}/{achievementid}](uri-usersxuidachievementsscidachievementid.md)

   