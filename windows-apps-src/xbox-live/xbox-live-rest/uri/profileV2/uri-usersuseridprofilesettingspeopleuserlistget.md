---
title: GET (/users/{userId}/profile/settings/people/{userList})
assetID: f6553499-89e2-f21b-a00f-7e5437c045ff
permalink: en-us/docs/xboxlive/rest/uri-usersuseridprofilesettingspeopleuserlistget.html
author: KevinAsgari
description: " GET (/users/{userId}/profile/settings/people/{userList})"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d8f04950b5aff14bf943f9827f0a2af52ddbcb9c
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5837586"
---
# <a name="get-usersuseridprofilesettingspeopleuserlist"></a>GET (/users/{userId}/profile/settings/people/{userList})
사용자 프로필을 가져오거나 사용자 모니커를 사용 하 여 사용자를 지원 합니다. 이러한 Uri에 대 한 도메인은 `profile.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EKB)
  * [쿼리 문자열 매개 변수](#ID4EVB)
  * [필요한 요청 헤더](#ID4EQC)
  * [요청 본문](#ID4E2D)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
**userList** 및 **userIds** 상호 배타적인 매개 변수가 있습니다. 하나 또는 둘 다를 지정 하는 경우에 **BadRequest** 다시 볼 수 있습니다. **userList** 여러 명명 된 목록을 요청에 유용한 시나리오에서 future 검사에 대 한 배열입니다. **userIds** XUIDs에 대해 10 진수 문자열의 구성 된-JSON 64 비트 부호 없는 정수를 직렬화 하는 작업에 잘못 되었습니다. 마지막으로, 일반 사용자를 읽을 수 있는 이름 대신 64 비트 부호 없는 정수로 또는 **XONLINE_PROFILE_ASDF**같은 모호한 상수를 사용 하 여 설정, Xbox One의 설정은 라는 됩니다.
  
<a id="ID4EKB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| 사용자 Id| string| 'Xuid(12345)', 'gt(myGamertag)' 또는 'm e' 수 있습니다.| 
| userList| string| 명명 된 목록에 대 한 설정을 가져오려면 사용자입니다. 현재 사용자가 지원 되는 유일한 목록입니다.| 
  
<a id="ID4EVB"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 설정| string| 이름 설정의 쉼표로 구분 된 목록입니다.| 
  
<a id="ID4EQC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| xbl 계약 버전 x| 32 비트 부호 있는 정수| 값 = 2| 
| 콘텐츠 형식| string| 값 = <code>application/json</code>| 
  
<a id="ID4E2D"></a>

 
## <a name="request-body"></a>요청 본문
 
<a id="ID4EBE"></a>

 
### <a name="sample-request"></a>샘플 요청
 

```cpp
GET /users/me/profile/settings/people/people?settings=GameDisplayName,GameDisplayPicRaw,Gamerscore,Gamertag
      
```

  
<a id="ID4EKE"></a>

  
 
<a id="ID4EME"></a>

 
##### <a name="response-body"></a>응답 본문 
응답은 **ReadMultiSettingsResponseV2** 개체입니다. 호출 하는 사용자를 가정 하나만 친구에 있습니다.
  

```cpp
{
      "profileUsers":[
         {
            "id":"2533274791381930",
            "settings":[
               {
                  "id":"GameDisplayName",
                  "value":"John Smith"
               },
               {
                  "id":"GameDisplayPicRaw",
                  "value":"http://images-eds.xboxlive.com/image?url=z951ykn43p4FqWbbFvR2Ec.8vbDhj8G2Xe7JngaTToBrrCmIEEXHC9UNrdJ6P7KIN0gxC2r1YECCd3mf2w1FDdmFCpSokJWa2z7xtVrlzOyVSc6pPRdWEXmYtpS2xE4F&format=png&w=64&h=64"
               },
               {
                  "id":"Gamerscore",
                  "value":"0"
               },
               {
                  "id":"Gamertag",
                  "value":"CracklierJewel9"
               }
            ]
         }
      ]
   }
         
```

   
<a id="ID4E3E"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E5E"></a>

 
##### <a name="parent"></a>부모 

[/users/{userId}/profile/settings/people/{userList}?settings={settings}](uri-usersuseridprofilesettingspeopleuserlist.md)

 [Profile(JSON)](../../json/json-profile.md)

   