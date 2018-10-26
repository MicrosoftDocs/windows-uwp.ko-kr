---
title: POST (/users/batch/profile/settings)
assetID: 2a619148-a626-f413-bda1-a2790063075d
permalink: en-us/docs/xboxlive/rest/uri-usersbatchprofilesettingspost.html
author: KevinAsgari
description: " POST (/users/batch/profile/settings)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ed6d07a89b7ef2f18b254111d5edd6d7cdb94d45
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5562553"
---
# <a name="post-usersbatchprofilesettings"></a>POST (/users/batch/profile/settings)
사용자 또는 사용자에 대 한 프로필을 가져옵니다. 이러한 Uri에 대 한 도메인은 `profile.xboxlive.com`.
 
  * [설명](#ID4EV)
  * [권한 부여](#ID4EFB)
  * [필요한 요청 헤더](#ID4EOB)
  * [요청 본문](#ID4EZC)
  * [응답 본문](#ID4EJD)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>설명
 
이 정규화 된 이름만 프로필 URL에 사용할 수 있습니다. 클라이언트에서 다른 모든 프로필 Api 차단 됩니다.
  
<a id="ID4EFB"></a>

 
## <a name="authorization"></a>권한 부여
 
프로필에 액세스 하려면 일반 인증 토큰 및 클레임 필요 합니다.
  
<a id="ID4EOB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 유형| 설명| 
| --- | --- | --- | 
| xbl 계약 버전 x| 32 비트 부호 없는 정수| Xbox 360 API에서이 호출을 구분 하기 위해 계약 버전 2로 설정 해야 합니다.| 
| 콘텐츠 형식| string| 값 = <code>application/json</code>| 
  
<a id="ID4EZC"></a>

 
## <a name="request-body"></a>요청 본문
 
<a id="ID4E6C"></a>

 
### <a name="sample-request"></a>샘플 요청
 

```cpp
POST /users/batch/profile/settings
   {
      "userIds":[
         "2533274791381930"
       ],
      "settings":[
         "GameDisplayName",
         "GameDisplayPicRaw",
         "Gamerscore",
         "Gamertag"
      ]
   }
      
```

   
<a id="ID4EJD"></a>

 
## <a name="response-body"></a>응답 본문
 
<a id="ID4EPD"></a>

 
### <a name="sample-response"></a>예제 응답
 

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
                  "value":"http://images-eds.xboxlive.com/image?url=z951ykn43p4FqWbbFvR2Ec.8vbDhj8G2Xe7JngaTToBrrCmIEEXHC9UNrdJ6P7KIN0gxC2r1YECCd3mf2w1FDdmFCpSokJWa2z7xtVrlzOyVSc6pPRdWEXmYtpS2xE4F"
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

   
<a id="ID4EZD"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E2D"></a>

 
##### <a name="parent"></a>부모 

[/users/batch/profile/settings](uri-usersbatchprofilesettings.md)

  
<a id="ID4EFE"></a>

 
##### <a name="reference"></a>참조 

[Profile(JSON)](../../json/json-profile.md)

   