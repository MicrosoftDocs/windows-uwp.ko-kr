---
title: 게임 DVR URI
assetID: 472f705e-bf28-7894-b1ba-80933d8746a6
permalink: en-us/docs/xboxlive/rest/atoc-reference-dvr.html
author: KevinAsgari
description: " 게임 DVR URI"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b4da88a85535d6be97607663c96e416e226efb31
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6263996"
---
# <a name="game-dvr-uris"></a>게임 DVR URI
 
이 섹션에서는 *게임 DVR*에 대 한 Xbox Live 서비스에서 유니버설 리소스 식별자 (URI) 주소 및 관련된 하이퍼텍스트 전송 프로토콜 (HTTP) 메서드에 대 한 세부 정보를 제공 합니다.
 
만 콘솔 게임 클립을 기록할 수 있지만 액세스할 수 있는 모든 장치 클립을 표시할 수 있습니다.
 
해당 URI 함수에 따라 이러한 Uri에 대 한 도메인 다음과 같습니다.
 
   *  gameclipsmetadata.xboxlive.com 
   *  gameclipstransfer.xboxlive.com 
  
<a id="ID4EZB"></a>

 
## <a name="in-this-section"></a>이 섹션의 내용

[/public/scids/{scid}/clips](uri-publicscidclips.md)

&nbsp;&nbsp;공용 클립 액세스 합니다. 두 가지 형태로 지정 수 실제로이 URI `/public/scids/{scid}/clips` 및 `/public/titles/{titleId}/clips`. 자세한 내용은 아래를 참조하세요.

[/{uri}](uri-uri.md)

&nbsp;&nbsp;게임 클립 데이터에 액세스 합니다.

[/users/me/scids/{scid}/clips](uri-usersmescidclips.md)

&nbsp;&nbsp;액세스는 초기 요청을 업로드 합니다.

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

&nbsp;&nbsp;게임 클립 데이터 액세스 및 메타 데이터를 제공 합니다.

[/users/{ownerId}/clips](uri-usersowneridclips.md)

&nbsp;&nbsp;사용자의 클립의 액세스 목록입니다.

[/users/{ownerId}/scids/{scid}/clips/{gameClipId}](uri-usersowneridscidclipsgameclipid.md)

&nbsp;&nbsp;찾으면 모든 Id에 알려진 경우 시스템에서 단일 게임 클립에 액세스 합니다.
 
<a id="ID4EOC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EQC"></a>

 
##### <a name="parent"></a>부모 

[URI(Universal Resource Identifier) 참조](../atoc-xboxlivews-reference-uris.md)

   