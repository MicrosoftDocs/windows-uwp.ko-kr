---
title: /users/me/scids/{scid}/clips/{gameClipId}
assetID: f5bead69-4fc9-f551-39cb-c8754645ac88
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipid.html
author: KevinAsgari
description: " /users/me/scids/{scid}/clips/{gameClipId}"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 36be1a1f30c659bec70fdd67a02bbe5ad7ffad3d
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5697969"
---
# <a name="usersmescidsscidclipsgameclipid"></a>/users/me/scids/{scid}/clips/{gameClipId}
게임 클립 데이터 액세스 및 메타 데이터를 제공 합니다. 이러한 Uri에 대 한 도메인은 `gameclipsmetadata.xboxlive.com` 및 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.
 
  * [URI 매개 변수](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| string| 액세스 되는 리소스의 서비스 구성 ID입니다. 인증된 된 사용자의 서비스 안내 일치 해야 합니다.| 
| gameClipId| string| GameClip ID 액세스 되는 리소스입니다.| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[DELETE (/users/me/scids/{scid}/clips/{gameClipId})](uri-usersmescidclipsgameclipiddelete.md)

&nbsp;&nbsp;게임 클립 삭제

[POST (/users/me/scids/{scid}/clips/{gameClipId})](uri-usersmescidclipsgameclipidpost.md)

&nbsp;&nbsp;사용자의 데이터에 대 한 게임 클립 메타 데이터를 업데이트 합니다.
 
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>부모 

[게임 DVR URI](atoc-reference-dvr.md)

   