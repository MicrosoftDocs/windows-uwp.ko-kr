---
title: 게임 DVR URI
assetID: 472f705e-bf28-7894-b1ba-80933d8746a6
permalink: en-us/docs/xboxlive/rest/atoc-reference-dvr.html
description: " 게임 DVR URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b4bfd6e51efce4c6ec85db99a10a44a776dcb840
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617848"
---
# <a name="game-dvr-uris"></a>게임 DVR URI
 
이 섹션에서 Xbox Live 서비스에 대 한 유니버설 리소스 식별자 (URI) 주소 및 관련된 하이퍼텍스트 전송 프로토콜 (HTTP) 메서드에 대 한 세부 정보를 제공 *게임 DVR*합니다.
 
콘솔만 게임 클립을 녹음할 수 있지만 액세스할 수 있는 모든 장치는 클립을 표시할 수 있습니다.
 
해당 URI의 함수에 따라 이러한 Uri에 대 한 도메인 다음과 같습니다.
 
   *  gameclipsmetadata.xboxlive.com 
   *  gameclipstransfer.xboxlive.com 
  
<a id="ID4EZB"></a>

 
## <a name="in-this-section"></a>이 섹션의 내용

[/public/scids/{scid}/clips](uri-publicscidclips.md)

&nbsp;&nbsp;클립을 공개 하는 액세스 합니다. 이 URI 실제로 지정할 수 있습니다 두 가지 형태로 `/public/scids/{scid}/clips` 고 `/public/titles/{titleId}/clips`입니다. 자세한 내용은 아래를 참조하세요.

[/{uri}](uri-uri.md)

&nbsp;&nbsp;게임 클립 데이터에 액세스 합니다.

[/users/me/scids/{scid}/clips](uri-usersmescidclips.md)

&nbsp;&nbsp;초기 액세스 요청을 업로드 합니다.

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

&nbsp;&nbsp;게임 클립 데이터 액세스 및 메타 데이터입니다.

[/users/{ownerId}/clips](uri-usersowneridclips.md)

&nbsp;&nbsp;사용자의 클립의 액세스 목록입니다.

[/users/{ownerId}/scids/{scid}/clips/{gameClipId}](uri-usersowneridscidclipsgameclipid.md)

&nbsp;&nbsp;알고 있는 경우 해당 위치를 찾도록 모든 Id 시스템에서 단일 게임 클립에 액세스 합니다.
 
<a id="ID4EOC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EQC"></a>

 
##### <a name="parent"></a>Parent 

[유니버설 리소스 식별자 (URI) 참조](../atoc-xboxlivews-reference-uris.md)

   