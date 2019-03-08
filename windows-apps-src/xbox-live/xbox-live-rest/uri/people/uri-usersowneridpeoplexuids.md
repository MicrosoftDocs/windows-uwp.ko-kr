---
title: /users/{ownerId}/people/xuids
assetID: db2faec7-9f6c-f240-586a-45d6ed596e88
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeoplexuids.html
description: " /users/{ownerId}/people/xuids"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 27b7695ba163bf0ca832a96df030868e646e0abc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626338"
---
# <a name="usersowneridpeoplexuids"></a>/users/{ownerId}/people/xuids
사람들 XUID에서 호출자의 사용자 컬렉션에서 액세스 됩니다. 이러한 Uri에 대 한 도메인은 `social.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| ownerId| 문자열| 해당 리소스에 액세스 하는 사용자의 식별자입니다. 인증된 된 사용자를 일치 해야 합니다. 가능한 값은 "me", xuid({xuid}), 또는 gt({gamertag})입니다.| 
  
<a id="ID4EOB"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[POST](uri-usersowneridpeoplexuidspost.md)

&nbsp;&nbsp;컬렉션을에서 호출자의 사용자 XUID 하 여 사용자를 가져옵니다.
 
<a id="ID4EYB"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E1B"></a>

 
##### <a name="parent"></a>Parent 

[유니버설 리소스 식별자 (URI) 참조](../atoc-xboxlivews-reference-uris.md)

   