---
title: /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
assetID: 0983dad0-59b7-45b7-505d-603e341fe0cc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidstatnamepeople.html
author: KevinAsgari
description: " /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4c6967cb337da2188b0403450db373ee3c9088a2
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/23/2018
ms.locfileid: "7572698"
---
# <a name="usersxuidxuidscidsscidstatsstatnamepeopleallfavorite"></a>/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
소셜 (순위) 순위표에 액세스합니다.
이러한 Uri에 대 한 도메인은 `leaderboards.xboxlive.com`.

  * [URI 매개 변수](#ID4EV)

<a id="ID4EV"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| xuid| string| 사용자의 식별자입니다.|
| 서비스 안내| string| 액세스 되는 리소스를 포함 하는 서비스 구성의 식별자입니다.|
| statname| string| 액세스 되는 사용자 통계 리소스의 고유 식별자입니다.|
| all\ | favorite| 열거형| 현재 사용자의 알려진된 모든 연락처 또는 해당 사용자가 즐겨찾기 사용자 지정 된 연락처에만 (점수) 값의 상태를 순위 여부입니다.|

<a id="ID4EOC"></a>


## <a name="valid-methods"></a>유효한 메서드

[GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all\|favorite})](uri-usersxuidscidstatnamepeopleget.md)

&nbsp;&nbsp;소셜 순위표 순위 상태는 현재 사용자의 모든 알려진된 연락처 중 하나 또는 해당 사용자가 즐겨찾기 사용자 지정 된 연락처에만 (점수) 값을 반환 합니다.

<a id="ID4EYC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4E1C"></a>


##### <a name="parent"></a>부모

[순위표 URI](atoc-reference-leaderboard.md)
