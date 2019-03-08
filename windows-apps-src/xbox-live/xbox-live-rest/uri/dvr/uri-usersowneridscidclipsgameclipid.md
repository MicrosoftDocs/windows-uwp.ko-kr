---
title: /users/{ownerId}/scids/{scid}/clips/{gameClipId}
assetID: 49b68418-71f1-c5a2-3a9b-869fd1fa663c
permalink: en-us/docs/xboxlive/rest/uri-usersowneridscidclipsgameclipid.html
description: " /users/{ownerId}/scids/{scid}/clips/{gameClipId}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e7ea92e89d54df17e8d82084d840a7ee9ef7d032
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658338"
---
# <a name="usersowneridscidsscidclipsgameclipid"></a>/users/{ownerId}/scids/{scid}/clips/{gameClipId}
알고 있는 경우 해당 위치를 찾도록 모든 Id 시스템에서 단일 게임 클립에 액세스 합니다. 이러한 Uri에 대 한 도메인이 `gameclipsmetadata.xboxlive.com` 고 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.
 
  * [URI 매개 변수](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| ownerId| 문자열| 해당 리소스가 액세스 되는 사용자의 사용자 id입니다. 지원 되는 형식: "me" 또는 "xuid(123456789)"입니다. 최대 길이: 16.| 
| scid| 문자열| 서비스 액세스 되는 리소스의 ID를 구성 합니다. 인증된 된 사용자의 서비스 안내 일치 해야 합니다.| 
| gameClipId| 문자열| 액세스 하는 리소스의 ID를 클립 합니다.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})](uri-usersowneridscidclipsgameclipidget.md)

&nbsp;&nbsp;알고 있는 경우 해당 위치를 찾도록 모든 Id 시스템에서 단일 게임 클립을 가져옵니다.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Parent 

[게임 DVR Uri](atoc-reference-dvr.md)

   