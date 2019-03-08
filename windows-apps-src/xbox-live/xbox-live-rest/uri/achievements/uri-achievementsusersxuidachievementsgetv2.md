---
title: GET (/users/xuid({xuid})/achievements)
assetID: 381d49d1-7a4b-4a1e-1baf-cf674f7e0d54
permalink: en-us/docs/xboxlive/rest/uri-achievementsusersxuidachievementsgetv2.html
description: " GET (/users/xuid({xuid})/achievements)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 720170e88808db7ef95b88896fbca4f1cda4a091
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655268"
---
# <a name="get-usersxuidxuidachievements"></a>GET (/users/xuid({xuid})/achievements)
제목, 사용자가 잠금 해제 된 또는 사용자가 진행 중에 이러한 정의 하는 성과의 목록을 가져옵니다. 이러한 Uri에 대 한 도메인은 `achievements.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EX)
  * [쿼리 문자열 매개 변수](#ID4ECB)
  * [권한 부여](#ID4ENF)
  * [필요한 요청 헤더](#ID4ESG)
  * [선택적 요청 헤더](#ID4ESH)
  * [요청 본문](#ID4EIBAC)
  * [응답 본문](#ID4ETBAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID)입니다 (리소스)를 액세스 하는 사용자입니다. 인증된 된 사용자의 XUID 일치 해야 합니다.| 
  
<a id="ID4ECB"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수
 
| 매개 변수| 필수| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | 
| <b>skipItems</b>| 아니오| 32 비트 부호 있는 정수| 항목 수가 지정 된 후 시작 하는 항목을 반환 합니다. 예를 들어 <b>skipItems = "3"</b> 항목을 검색 하는 네 번째 항목 부터는 검색 합니다. | 
| <b>continuationToken</b>| 아니오| 문자열| 지정 된 연속 토큰에서 시작 하는 항목을 반환 합니다. | 
| <b>maxItems</b>| 아니오| 32 비트 부호 있는 정수| 과 결합 될 수 있는 컬렉션에서 반환할 항목의 최대 수 <b>skipItems</b> 하 고 <b>continuationToken</b> 항목의 범위를 반환할 수 있습니다. 서비스 하는 경우 기본 값을 제공할 수 있습니다 <b>maxItems</b> 없 및 보다 더 적은 수를 반환할 수 있습니다 <b>maxItems</b>결과의 마지막 페이지 아직 반환 되지 않은 경우에 합니다. | 
| <b>titleId</b>| 아니오| 문자열| 반환된 된 결과 대 한 필터입니다. 하나 이상의 쉼표로 구분 된 10 진수 제목을 식별자를 허용합니다.| 
| <b>unlockedOnly</b>| 아니오| 부울 값| 반환된 된 결과 필터링 합니다. 경우 설정 <b>true</b>, 사용자에 대 한 잠금 해제 성과 반환 됩니다. 기본값으로 <b>false</b>합니다.| 
| <b>possibleOnly</b>| 아니오| 부울 값| 반환된 된 결과 필터링 합니다. 경우로 <b>true</b>모든 가능한 결과 반환 하지만 없습니다만 메타 데이터-잠금 해제, XMS의 성과 달성 하셨습니다 정보입니다. 기본값으로 <b>false</b>합니다.| 
| <b>types</b>| 아니오| 문자열| 반환된 된 결과 대 한 필터입니다. "Persistent" 또는 "챌린지" 수 있습니다. 기본값은 모든 지원 되는 형식입니다.| 
| <b>orderBy</b>| 아니오| 문자열| 결과 반환 하는 순서를 지정 합니다. "순서가 지정 되지 않은", "Title", "UnlockTime" 또는 "EndingSoon" 수 있습니다. 기본값은 "정렬 되지 않은"입니다.| 
  
<a id="ID4ENF"></a>

 
## <a name="authorization"></a>권한 부여
 
| 클레임| 필수 여부| 설명| 누락 된 경우 동작| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 사용자| 호출자에 게 권한이 부여 된 Xbox LIVE 사용자입니다.| 호출자는 Xbox LIVE의 유효한 사용자 해야 합니다.| 403 Forbidden| 
  
<a id="ID4ESG"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 권한 부여| 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예를 들어 값: "XBL3.0 x=&lt;userhash>;&lt;token>".| 
  
<a id="ID4ESH"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>X-RequestedServiceVersion</b>| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 기본값: 1.| 
| <b>x-xbl-contract-version</b>| 32 비트 부호 없는 정수| 있는 경우 2로이 API의 V2 버전 사용 됩니다. 그렇지 않으면 V1 합니다.| 
| <b>Accept-Language</b>| 문자열| 원하는 로캘 및 대체 (예:-FR, fr, EN-GB, en WW, EN-US)의 목록입니다. 일치 하는 지역화 된 문자열을 찾으면 성과 서비스 목록을 통해 작동 합니다. 일치 항목이 없는 경우, 사용자의 IP 주소에서 제공 되는 사용자 토큰에 정의 된 위치와 일치 하도록 시도 합니다. 없는 일치 하는 지역화 된 문자열에 여전히 발견 되 면 제목 개발자/게시자가 제공 하는 기본 문자열을 사용 합니다. | 
  
<a id="ID4EIBAC"></a>

 
## <a name="request-body"></a>요청 본문
 
개체가이 요청의 본문에 전송 됩니다.
  
<a id="ID4ETBAC"></a>

 
## <a name="response-body"></a>응답 본문
 
서비스의 배열을 반환 호출이 성공 하면 [도전 과제 (JSON)](../../json/json-achievementv2.md) 개체와 [PagingInfo (JSON)](../../json/json-paginginfo.md) 개체입니다.
 
<a id="ID4ECCAC"></a>

 
### <a name="sample-response"></a>샘플 응답
 

```cpp
{
    "achievements":
    [{
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
                "achievementState":"Achieved",
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
        }],
        "pagingInfo":
        {
                "continuationToken":null,
                "totalRecords":1
        }
}
         
```

   
<a id="ID4EPCAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ERCAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/achievements](uri-achievementsusersxuidachievementsv2.md)

  
<a id="ID4E2CAC"></a>

 
##### <a name="reference"></a>참고자료 

[도전 과제 (JSON)](../../json/json-achievementv2.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

 [페이징 매개 변수](../../additional/pagingparameters.md)

   