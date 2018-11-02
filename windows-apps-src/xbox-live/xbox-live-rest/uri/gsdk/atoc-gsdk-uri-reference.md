---
title: 게임 서버 Universal Resource Identifier (URI) 참조
assetID: bbd7e3f3-77ac-6ffd-8951-fe4b8b48eb4c
permalink: en-us/docs/xboxlive/rest/atoc-gsdk-uri-reference.html
author: KevinAsgari
description: " 게임 서버 Universal Resource Identifier (URI) 참조"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9eff98593c122001cab591b9f45793aa6649736a
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5936801"
---
# <a name="game-server-universal-resource-identifier-uri-reference"></a>게임 서버 Universal Resource Identifier (URI) 참조
타이틀에 대 한 게임 서버 개발 키트 서버 인스턴스를 만드는 데 클라이언트에 의해 Uri입니다. 이러한 Uri에 대 한 도메인은 `gameserverds.xboxlive.com` 및 `gameserverms.xboxlive.com`.
 
<a id="ID4EY"></a>

 
## <a name="in-this-section"></a>이 섹션의 내용

[/qosservers](uri-qosservers.md)

&nbsp;&nbsp;URI를 사용 하 여 Xbox Live 계산 사용 하기 위해 사용할 수 있는 QoS 서버의 목록을 가져올 수 클라이언트에 의해 호출 합니다.

[/titles/{titleId}/clusters](uri-titlestitleidclusters.md)

&nbsp;&nbsp;클라이언트는 제목에 대 한 Xbox Live 계산 서버 인스턴스를 만들 수 있는 URI입니다.

[/titles/{titleId}/variants](uri-titlestitleidvariants.md)

&nbsp;&nbsp;URI는 제목에 대 한 사용 가능한 변형 가져오려면 클라이언트에 의해 호출 됩니다.

[/titles/{titleId}/sessionhosts](uri-titlestitleidsessionhosts.md)

&nbsp;&nbsp;지정 된 제목 id에 할당 하는 Xbox Live 계산 sessionhost를 요청 합니다.

[/titles/{titleId}/sessions/{sessionId}/allocationStatus](uri-titlestitleidsessionssessionidallocationstatus.md)

&nbsp;&nbsp;지정 된 제목 id와 세션 id가에 대 한 티켓 요청 상태를 가져옵니다.
 