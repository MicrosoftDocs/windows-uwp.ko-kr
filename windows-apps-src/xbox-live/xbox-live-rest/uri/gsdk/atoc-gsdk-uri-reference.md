---
title: 게임 서버 유니버설 리소스 식별자 (URI) 참조
assetID: bbd7e3f3-77ac-6ffd-8951-fe4b8b48eb4c
permalink: en-us/docs/xboxlive/rest/atoc-gsdk-uri-reference.html
description: " 게임 서버 유니버설 리소스 식별자 (URI) 참조"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a9a0a38cff9214485b2d7e8b1f8a28acb3207444
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662598"
---
# <a name="game-server-universal-resource-identifier-uri-reference"></a>게임 서버 유니버설 리소스 식별자 (URI) 참조
클라이언트에서 제목에 대 한 게임 서버 개발 키트 서버 인스턴스를 만드는 데는 Uri입니다. 이러한 Uri에 대 한 도메인이 `gameserverds.xboxlive.com` 고 `gameserverms.xboxlive.com`입니다.
 
<a id="ID4EY"></a>

 
## <a name="in-this-section"></a>이 섹션의 내용

[/qosservers](uri-qosservers.md)

&nbsp;&nbsp;Xbox Live Compute 사용에 대 한 QoS 서버의 목록을 가져오려면 클라이언트에서 호출 하는 URI입니다.

[/titles/{titleId}/clusters](uri-titlestitleidclusters.md)

&nbsp;&nbsp;제목에는 Xbox Live 계산 서버 인스턴스를 만들 수 있는 URI입니다.

[/titles/{titleId}/variants](uri-titlestitleidvariants.md)

&nbsp;&nbsp;제목에 대 한 사용 가능한 변형을 가져오려면 클라이언트에서 호출 하는 URI입니다.

[/titles/{titleId}/sessionhosts](uri-titlestitleidsessionhosts.md)

&nbsp;&nbsp;지정 된 제목 id가 할당 되는 Xbox Live Compute sessionhost를 요청 합니다.

[/titles/{titleId}/sessions/{sessionId}/allocationStatus](uri-titlestitleidsessionssessionidallocationstatus.md)

&nbsp;&nbsp;지정 된 제목 id 및 세션 id에 대해 티켓 요청의 상태를 가져옵니다.
 