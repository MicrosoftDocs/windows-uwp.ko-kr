---
title: /users/xuid({xuid})/groups/{moniker}/broadcasting/count
assetID: 535c8d46-7001-c31e-3e9d-82ad275095ae
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerbroadcastingcount.html
author: KevinAsgari
description: " /users/xuid({xuid})/groups/{moniker}/broadcasting/count"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2d563fde1f5c7aa430547e16771fa920786cd739
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4686867"
---
# <a name="usersxuidxuidgroupsmonikerbroadcastingcount"></a>/users/xuid({xuid})/groups/{moniker}/broadcasting/count
액세스 그룹 모니커 하 여 지정 된 브로드캐스트 사용자의 수와 관련 된 URI에 표시 되는 XUID 합니다. 이러한 Uri에 대 한 도메인은 `userpresence.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| string| Xbox 사용자 ID (XUID)와 관련 된 그룹에 XUIDs 사용자의 합니다.| 
| 모니커| string| 사용자의 그룹을 정의 하는 문자열입니다. 현재 허용 된 유일한 모니커 대문자 'P'를 사용 하 여 "사람" 인 경우| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET (/users/xuid({xuid})/groups/{moniker}/broadcasting/count )](uri-usersxuidgroupsmonikerbroadcastingcountget.md)

&nbsp;&nbsp;지정 된 URI에 표시 되는 XUID와 관련 된 그룹 모니커 브로드캐스트 사용자의 수를 검색 합니다.
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>부모 

[상태 URI](atoc-reference-presence.md)

   