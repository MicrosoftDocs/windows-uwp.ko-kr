---
title: 평판 URI
assetID: d1cb76c0-86a4-8c51-19f6-5f223b517d46
permalink: en-us/docs/xboxlive/rest/atoc-reference-reputation.html
author: KevinAsgari
description: " 평판 URI"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b869f87760498dc6a2224809a42380f1b8f5930b
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4388359"
---
# <a name="reputation-uris"></a>평판 URI
 
이 섹션에서는 **Microsoft.Xbox.Services.Social.ReputationService**에 대 한 Xbox Live 서비스에서 유니버설 URI (Resource Identifier) 주소 및 관련된 하이퍼텍스트 전송 프로토콜 (HTTP) 메서드에 대 한 세부 정보를 제공 합니다. 평판 Uri에 대 한 도메인 reputation.xboxlive.com입니다. 일반적인 URI 표현 수도 있습니다. https://reputation.xboxlive.com/users/xuid(2533274790412952)/feedback. 
 
신뢰도 서비스를 사용 하 여 피드백을 [피드백 (JSON)](../../json/json-feedback.md)설명 된 대로 평판 점수를 계산 합니다. 이 점수 ReputationOverall 키 아래에 있는 사용자에 대 한 통계 영역에 저장 됩니다. 사용자 통계를 검색 하는 방법에 대 한 자세한 내용은 참조 [가져오기 (/users/xuid({xuid})/scids/{scid}/stats)](../userstats/uri-usersxuidscidsscidstatsget.md). 
 
모든 플랫폼에서 게임 신뢰도 서비스를 사용할 수 있습니다.
 
<a id="ID4EMB"></a>

 
## <a name="in-this-section"></a>이 섹션 내용

[/users/xuid({xuid})/feedback](uri-reputationusersxuidfeedback.md)

&nbsp;&nbsp;피드백 옵션이 아니라 셸을 사용 하 여 게임에서 추가를 원하는 경우 타이틀에서 사용 합니다.

[/users/batchfeedback](uri-reputationusersbatchfeedback.md)

&nbsp;&nbsp;타이틀의 인터페이스 외부에서 일괄 처리 형태로 피드백을 보내려면 타이틀의 서비스에서 사용 합니다.

[/users/me/resetreputation](uri-usersmeresetreputation.md)

&nbsp;&nbsp;적용 팀을 현재 사용자의 평판 점수에 액세스할 수 있습니다.

[/users/xuid({xuid})/deleteuserdata](uri-usersxuiddeleteuserdata.md)

&nbsp;&nbsp;테스트 사용자에 대 한 안전도 데이터를 완전히 다시 설정합니다. 만 테스트 합니다.

[/users/xuid({xuid})/resetreputation](uri-usersxuidresetreputation.md)

&nbsp;&nbsp;적용 팀을 지정 된 사용자의 평판 점수에 액세스할 수 있습니다.
 
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>부모 

[URI(Universal Resource Identifier) 참조](../atoc-xboxlivews-reference-uris.md)

   