---
title: /users/{ownerId}/clips
assetID: cc200b89-dc63-9ab5-b037-7a910f46dae9
permalink: en-us/docs/xboxlive/rest/uri-usersowneridclips.html
author: KevinAsgari
description: " /users/{ownerId}/clips"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 755d30796c7e948a2fe97e84986a74a461795f83
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6149507"
---
# <a name="usersowneridclips"></a>/users/{ownerId}/clips
사용자의 클립의 액세스 목록입니다. 이러한 Uri에 대 한 도메인은 `gameclipsmetadata.xboxlive.com` 및 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.
 
  * [URI 매개 변수](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| ownerId| string| 해당 리소스를 액세스 하는 사용자의 사용자 id입니다. 지원 되는 형식: "me" 또는 "xuid(123456789)"입니다. 최대 길이: 16입니다.| 
  
<a id="ID4EVB"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET (/users/{ownerId}/clips)](uri-usersowneridclipsget.md)

&nbsp;&nbsp;사용자의 클립의 목록을 검색 합니다.
 
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>부모 

[게임 DVR URI](atoc-reference-dvr.md)

   