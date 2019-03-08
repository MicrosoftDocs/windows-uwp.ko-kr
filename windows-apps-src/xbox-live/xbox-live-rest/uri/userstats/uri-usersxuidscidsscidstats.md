---
title: /users/xuid({xuid})/scids/{scid}/stats
assetID: 3cf9ffd4-9a8b-2658-402b-2e933f7f6f1b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstats.html
description: " /users/xuid({xuid})/scids/{scid}/stats"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 53a6c7bb0e7390b024b01e221d8061316a80509e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650418"
---
# <a name="usersxuidxuidscidsscidstats"></a>/users/xuid({xuid})/scids/{scid}/stats
지정된 된 사용자를 대신 하 여 사용자 통계 이름의 쉼표로 구분 된 목록으로 범위가 지정 된 서비스 구성에 액세스 합니다. 이러한 Uri에 대 한 도메인은 `userstats.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid| GUID| Xbox 사용자 ID (XUID) 서비스 구성에 액세스 하는 사용자입니다.| 
| scid| GUID| 액세스 되는 리소스를 포함 하는 서비스 구성의 식별자입니다.| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>올바른 메서드

[GET](uri-usersxuidscidsscidstatsget.md)

&nbsp;&nbsp;지정된 된 사용자를 대신 하 여 사용자 통계 이름의 쉼표로 구분 된 목록으로 범위가 지정 된 서비스 구성을 가져옵니다.

[메타 데이터 값을 사용 하 여 가져오기](uri-usersxuidscidsscidstatsgetvaluemetadata.md)

&nbsp;&nbsp;지정 된 서비스 구성에서 사용자에 대 한 통계 값을 사용 하 여 연결 된 메타 데이터를 포함 하 여 지정 된 통계 목록을 가져옵니다.
 
<a id="ID4EKC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EMC"></a>

 
##### <a name="parent"></a>Parent 

[사용자 통계 Uri](atoc-reference-userstats.md)

   