---
title: /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
assetID: 0983dad0-59b7-45b7-505d-603e341fe0cc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidstatnamepeople.html
description: " /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 85a6470a64ceef3b154384d1ca859fb28733aad3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632578"
---
# <a name="usersxuidxuidscidsscidstatsstatnamepeopleallfavorite"></a>/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
소셜 (등급된) leaderboard에 액세스합니다.
이러한 Uri에 대 한 도메인은 `leaderboards.xboxlive.com`합니다.

  * [URI 매개 변수](#ID4EV)

<a id="ID4EV"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- |
| xuid| 문자열| 사용자의 식별자입니다.|
| scid| 문자열| 액세스 되는 리소스를 포함 하는 서비스 구성의 식별자입니다.|
| statname| 문자열| 액세스 하 고 사용자 상태 리소스의 고유 식별자입니다.|
| all\|favorite| 열거형| 순위는 stat 것인지 (점수)이 현재 사용자의 모든 알려진된 연락처 또는 해당 사용자가 즐겨 찾는 사용자로 지정 된 연락처만 대 한 값입니다.|

<a id="ID4EOC"></a>


## <a name="valid-methods"></a>올바른 메서드

[GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all\|favorite})](uri-usersxuidscidstatnamepeopleget.md)

&nbsp;&nbsp;소셜 leaderboard 순위는 stat 현재 사용자의 모든 알려진된 연락처 중 하나에 해당 사용자가 즐겨 찾는 사용자로 지정 된 연락처만 (점수) 값을 반환 합니다.

<a id="ID4EYC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4E1C"></a>


##### <a name="parent"></a>Parent

[순위표 Uri](atoc-reference-leaderboard.md)
