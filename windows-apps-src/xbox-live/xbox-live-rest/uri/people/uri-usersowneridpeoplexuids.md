---
title: /users/{ownerId}/people/xuids
assetID: db2faec7-9f6c-f240-586a-45d6ed596e88
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeoplexuids.html
author: KevinAsgari
description: " /users/{ownerId}/people/xuids"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 715659b8bb001697fc9386be6ec587b3682793c5
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4415863"
---
# <a name="usersowneridpeoplexuids"></a>/users/{ownerId}/people/xuids
XUID 하 여 사람들이 호출자의 사람들이 컬렉션에서 액세스 합니다. 이러한 Uri에 대 한 도메인은 `social.xboxlive.com`.
 
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

   