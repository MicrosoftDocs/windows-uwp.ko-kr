---
title: JSON blob 저장
author: KevinAsgari
description: JSON blob Xbox Live 타이틀 저장소에 저장 하는 방법을 알아봅니다.
ms.assetid: f424aca1-e671-4e31-84c6-098fda4a9558
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 타이틀 저장소
ms.localizationpriority: medium
ms.openlocfilehash: 629bc38e8d5e5afa4c9d78349587585d0701b3de
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6263136"
---
# <a name="storing-a-json-blob-in-xbox-live-title-storage"></a>Xbox Live 타이틀 저장소에 JSON blob 저장

1.  *PUT* 메서드를 사용 하 여 타이틀 저장소에 데이터를 보내도록 요청을 보냅니다.

        PUT https://titlestorage.xboxlive.com/json/users/xuid(1245111)/scids/{scid}/data/{pathAndFileName},json
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Content-Length: 240
        Connection: Keep-Alive



-   사용자는 업데이트를 세션에 있어야 합니다.

-   STSTokenString 편의 위해 자리 표시자 이며 인증 요청으로 반환 하는 토큰으로 대체 되어야 합니다.

2.  JSON 개체를 보냅니다.

        {
            "startlevel":"1",
            "expression":"smile"
        }

#### <a name="reference"></a>참조

**/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)**
