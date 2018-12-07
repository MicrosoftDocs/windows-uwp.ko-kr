---
title: /users/{ownerId}/people/{targetid}
assetID: 9dd19e75-3b48-d7e0-fc65-6760c15ddf62
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetid.html
description: " /users/{ownerId}/people/{targetid}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6238996abdeaca9b7a9a7a20d3f1ae9702e95a73
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/06/2018
ms.locfileid: "8730381"
---
# <a name="usersowneridpeopletargetid"></a>/users/{ownerId}/people/{targetid}
호출자의 사용자 컬렉션에서 대상 ID로 사람에 액세스 합니다. 이러한 Uri에 대 한 도메인은 `social.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| ownerId| string| 해당 리소스를 액세스 하는 사용자의 식별자입니다. 인증된 된 사용자와 일치 해야 합니다. 가능한 값은 "me", xuid({xuid}), 또는 gt({gamertag}) 합니다.| 
| targetid| string| Xbox 사용자 ID (XUID) 또는 게이머 태그 소유자의 사용자 목록에서 해당 데이터를 검색 하는 사용자의 식별자입니다. 예제 값: xuid(2603643534573581), gt(SomeGamertag) 합니다.| 
  
<a id="ID4EQB"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET](uri-usersowneridpeopletargetidget.md)

&nbsp;&nbsp;호출자의 사용자 컬렉션에서 대상 ID로 사용자를 가져옵니다.
 
<a id="ID4E1B"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E3B"></a>

 
##### <a name="parent"></a>부모 

[URI(Universal Resource Identifier) 참조](../atoc-xboxlivews-reference-uris.md)

   