---
title: GET (/users/{userId}/profile/settings/people/{userList})
assetID: f6553499-89e2-f21b-a00f-7e5437c045ff
permalink: en-us/docs/xboxlive/rest/uri-usersuseridprofilesettingspeopleuserlistget.html
description: " GET (/users/{userId}/profile/settings/people/{userList})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f868fdf4f3d5cd36000784d9c5a3437fa5d67ffa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593858"
---
# <a name="get-usersuseridprofilesettingspeopleuserlist"></a>GET (/users/{userId}/profile/settings/people/{userList})
사용자에 대 한 프로필을 가져오거나 사용자 모니커를 사용 하 여 사용자를 지원 합니다. 이러한 Uri에 대 한 도메인은 `profile.xboxlive.com`합니다.
 
  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EKB)
  * [쿼리 문자열 매개 변수](#ID4EVB)
  * [필요한 요청 헤더](#ID4EQC)
  * [요청 본문](#ID4E2D)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
**userList** 하 고 **사용자 Id** 는 상호 배타적인 매개 변수입니다. 얻게 이거나 둘 다 지정 된 경우는 **BadRequest** 다시 합니다. **userList** 여러 명명 된 목록을 요청 하는 데 유용 시나리오에서는 향후 점검 하는 것에 대 한 배열입니다. **사용자 Id** 이루어집니다 JSON XUIDs-에 대 한 10 진수 문자열의 64 비트 부호 없는 정수를 직렬화 하는 작업에서 잘못 되었습니다. 마지막으로, Xbox One에 대 한 설정으로 명명 됩니다 설정을 일반 알기 쉬운 이름 보다는 64 비트 부호 없는 정수 또는 모호한 상수와 같은 **XONLINE_PROFILE_ASDF**합니다.
  
<a id="ID4EKB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| userId| 문자열| 'Xuid(12345)', 'gt(myGamertag)' 또는 'me' 수 있습니다.| 
| userList| 문자열| 명명 된 목록 설정을 가져올 사용자입니다. 현재 사용자는 지원 되는 유일한 목록입니다.| 
  
<a id="ID4EVB"></a>

 
## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | 
| 설정| 문자열| 쉼표로 구분 된 목록의 이름을 설정 합니다.| 
  
<a id="ID4EQC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 32 비트 부호 있는 정수| 값 = 2| 
| content-type| 문자열| Value = <code>application/json</code>| 
  
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
응답은는 **ReadMultiSettingsResponseV2** 개체입니다. 호출 하는 사용자를 가정 하나만 friend에 있습니다.
  

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

 
##### <a name="parent"></a>Parent 

[/users/{userId}/profile/settings/people/{userList}?settings={settings}](uri-usersuseridprofilesettingspeopleuserlist.md)

 [프로필 (JSON)](../../json/json-profile.md)

   