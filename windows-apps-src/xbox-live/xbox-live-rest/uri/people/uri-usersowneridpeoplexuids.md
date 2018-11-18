---
title: /users/{ownerId}/people/xuids
assetID: db2faec7-9f6c-f240-586a-45d6ed596e88
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeoplexuids.html
author: KevinAsgari
description: " /users/{ownerId}/people/xuids"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d2b915e185462e65f68fefce009d6081e7cddf37
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7152051"
---
# <a name="usersowneridpeoplexuids"></a>/users/{ownerId}/people/xuids
XUID 하 여 호출자의 사람들이 컬렉션에서 사용자를 액세스 합니다. 이러한 Uri에 대 한 도메인은 `social.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| ownerId| string| 해당 리소스를 액세스 하는 사용자의 식별자입니다. 인증된 된 사용자와 일치 해야 합니다. 가능한 값은 "me", xuid({xuid}), 또는 gt({gamertag}) 합니다.| 
  
<a id="ID4EOB"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[POST](uri-usersowneridpeoplexuidspost.md)

&nbsp;&nbsp;컬렉션을 호출자의 사용자 로부터 XUID 사람을 가져옵니다.
 
<a id="ID4EYB"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E1B"></a>

 
##### <a name="parent"></a>부모 

[URI(Universal Resource Identifier) 참조](../atoc-xboxlivews-reference-uris.md)

   