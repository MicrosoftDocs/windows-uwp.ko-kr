---
title: /users/xuid({xuid})/scids/{scid}/stats
assetID: 3cf9ffd4-9a8b-2658-402b-2e933f7f6f1b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstats.html
author: KevinAsgari
description: " /users/xuid({xuid})/scids/{scid}/stats"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2fa886078d429719eb50aa8567bfe238768ba2e3
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4418921"
---
# <a name="usersxuidxuidscidsscidstats"></a>/users/xuid({xuid})/scids/{scid}/stats
지정된 된 사용자를 대신 하 여 사용자 통계 이름의 쉼표로 구분 된 목록에 의해 범위가 서비스 구성에 액세스 합니다. 이러한 Uri에 대 한 도메인은 `userstats.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| GUID| Xbox 사용자 ID (XUID) 서비스 구성에 액세스 하려면 대신 사용자의 합니다.| 
| 서비스 안내| GUID| 액세스 되는 리소스를 포함 하는 서비스 구성의 식별자입니다.| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>유효한 메서드

[GET](uri-usersxuidscidsscidstatsget.md)

&nbsp;&nbsp;지정된 된 사용자를 대신 하 여 사용자 통계 이름의 쉼표로 구분 된 목록에 의해 범위가 서비스 구성을 가져옵니다.

[메타 데이터 값을 사용 하 여 가져오기](uri-usersxuidscidsscidstatsgetvaluemetadata.md)

&nbsp;&nbsp;사용자 지정 된 서비스 구성에 대 한 통계 값을와 관련 된 메타 데이터를 포함 하 여 지정 된 통계 목록을 가져옵니다.
 
<a id="ID4EKC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EMC"></a>

 
##### <a name="parent"></a>부모 

[사용자 통계 URI](atoc-reference-userstats.md)

   