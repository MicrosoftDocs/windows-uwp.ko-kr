---
title: GET (/users/{userId}/profile/settings/people/{userList})
assetID: f6553499-89e2-f21b-a00f-7e5437c045ff
permalink: en-us/docs/xboxlive/rest/uri-usersuseridprofilesettingspeopleuserlistget.html
author: KevinAsgari
description: " GET (/users/{userId}/profile/settings/people/{userList})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d57a6620115d5f009c054210a50548c3da7e47d5
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4340628"
---
# <a name="get-usersuseridprofilesettingspeopleuserlist"></a>GET (/users/{userId}/profile/settings/people/{userList})
사용자 프로필을 가져오거나 사람들이 모니커와 사용자를 지원 합니다. 이러한 Uri에 대 한 도메인은 `profile.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EKB)
  * [쿼리 문자열 매개 변수](#ID4EVB)
  * [필요한 요청 헤더](#ID4EQC)
  * [요청 본문](#ID4E2D)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
**userList** 및 **userIds** 상호 배타적인 매개 변수가 있습니다. 하나 또는 둘 다를 지정 하는 경우 다시 **BadRequest** 를 볼 수 있습니다. **userList** 은 여러 명명 된 목록을 요청에 유용한 시나리오에서 future 검사에 대 한 배열입니다. **userIds** XUIDs에 대해 10 진수 문자열의 구성 된-JSON 직렬화 64 비트 부호 없는 정수로에 잘못 되었습니다. 마지막으로, 일반 읽을 이름 보다는 64 비트 부호 없는 정수로 또는 **XONLINE_PROFILE_ASDF**같은 모호한 상수를 사용 하 여 설정, Xbox One에서 설정은 라는 됩니다.
  
<a id="ID4EKB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| userId| string| 'Xuid(12345)', 'gt(myGamertag)' 또는 'm e' 수 있습니다.| 
| userList| string| 명명 된 목록에 대 한 설정을 가져올 수입니다. 현재 사용자가 지원 되는 유일한 목록입니다.| 
  
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

   