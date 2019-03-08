---
title: /users/me/scids/{scid}/clips/{gameClipId}
assetID: f5bead69-4fc9-f551-39cb-c8754645ac88
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipid.html
description: " /users/me/scids/{scid}/clips/{gameClipId}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3cbe2d2c996b466fd94287129f1add0868f05b05
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661778"
---
# <a name="usersmescidsscidclipsgameclipid"></a>/users/me/scids/{scid}/clips/{gameClipId}
게임 클립 데이터 액세스 및 메타 데이터입니다. 이러한 Uri에 대 한 도메인이 `gameclipsmetadata.xboxlive.com` 고 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.
 
  * [URI 매개 변수](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| scid| 문자열| 서비스 액세스 되는 리소스의 ID를 구성 합니다. 인증된 된 사용자의 서비스 안내 일치 해야 합니다.| 
| gameClipId| 문자열| 액세스 하는 리소스의 ID를 클립 합니다.| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[삭제 (/ 사용자/me scids / {서비스 안내} /clips/ {gameClipId})](uri-usersmescidclipsgameclipiddelete.md)

&nbsp;&nbsp;게임 클립 삭제

[POST (/ 사용자/me scids / {서비스 안내} /clips/ {gameClipId})](uri-usersmescidclipsgameclipidpost.md)

&nbsp;&nbsp;사용자의 데이터에 대 한 게임 클립 메타 데이터를 업데이트 합니다.
 
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 

[게임 DVR Uri](atoc-reference-dvr.md)

   