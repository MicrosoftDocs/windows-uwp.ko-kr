---
title: /scids/{scid}/leaderboards/{leaderboardname}
assetID: 16345a17-6025-5453-5694-eaf97f0e83e9
permalink: en-us/docs/xboxlive/rest/uri-scidsscidleaderboardsleaderboardname.html
description: " /scids/{scid}/leaderboards/{leaderboardname}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b73ffc2d6d6b80159651a90aabbf5595b146560d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645628"
---
# <a name="scidsscidleaderboardsleaderboardname"></a>/scids/{scid}/leaderboards/{leaderboardname}
미리 정의 된 전역 leaderboard에 액세스합니다. 이러한 Uri에 대 한 도메인은 `leaderboards.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| scid| GUID| 액세스 되는 리소스를 포함 하는 서비스 구성의 식별자입니다.| 
| leaderboardname| 문자열| 액세스 되는 미리 정의 된 순위표 리소스의 고유 식별자입니다.| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[GET](uri-scidsscidleaderboardsleaderboardnameget.md)

&nbsp;&nbsp; &nbsp;&nbsp;미리 정의 된 전체 순위표를 가져옵니다.


[메타 데이터 값을 사용 하 여 가져오기](uri-scidsscidleaderboardsleaderboardnamegetvaluemetadata.md)

&nbsp;&nbsp; &nbsp;&nbsp;순위표 값과 연결 된 모든 메타 데이터와 함께 미리 정의 된 전체 순위표를 가져옵니다.

 
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 

[순위표 Uri](atoc-reference-leaderboard.md)

   