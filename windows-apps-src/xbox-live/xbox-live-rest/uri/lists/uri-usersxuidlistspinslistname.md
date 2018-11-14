---
title: /users/xuid(xuid)/lists/PINS/{listname}
assetID: b6421b11-fcd1-cfdb-c1fa-6cab3dab89d9
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistname.html
author: KevinAsgari
description: " /users/xuid(xuid)/lists/PINS/{listname}"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3b76637d0c88e4ef2bc8905ff1b4dd894fd819a0
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6274520"
---
# <a name="usersxuidxuidlistspinslistname"></a>/users/xuid(xuid)/lists/PINS/{listname}
항목을 목록에 액세스 합니다. 이러한 Uri에 대 한 도메인은 `eplists.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| string| Xbox 사용자 ID (XUID)입니다.| 
| listtype| string| 유형 (사용 하는 방법 및 작동 방식을) 목록입니다. 항상 이러한 "핀" 관련 메서드.| 
| listname| string| 목록 이름 (의 지정 된 listtype 어떤 목록). 항상 "XBLPins"에 대 한 Pin 항목입니다.| 
  
<a id="ID4EGC"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[DELETE](uri-usersxuidlistspinslistnamedelete.md)

&nbsp;&nbsp;목록에서 항목을 제거합니다.

[GET](uri-usersxuidlistspinslistnameget.md)

&nbsp;&nbsp;목록 내용을 반환합니다.

[POST](uri-usersxuidlistspinslistnamepost.md)

&nbsp;&nbsp;쿼리 문자열 매개 변수 **insertIndex**에 따라 인덱스 목록에 항목을 삽입 합니다.

[PUT](uri-usersxuidlistspinslistnameput.md)

&nbsp;&nbsp;요청 본문에서 각 항목에 대해 지정한 인덱스에 따라 목록에서 항목을 업데이트 합니다.
 
<a id="ID4EZC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>부모 

[URI(Universal Resource Identifier) 참조](../atoc-xboxlivews-reference-uris.md)

   