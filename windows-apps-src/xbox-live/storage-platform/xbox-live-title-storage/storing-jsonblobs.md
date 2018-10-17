---
title: JSON blob 저장
author: KevinAsgari
description: JSON blob Xbox Live 타이틀 저장소에 저장 하는 방법을 알아봅니다.
ms.assetid: f424aca1-e671-4e31-84c6-098fda4a9558
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 타이틀 저장소
ms.localizationpriority: medium
ms.openlocfilehash: e98abcb9ab8738291efc40d5148b021ea95110fa
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4740320"
---
# <a name="storing-a-json-blob-in-xbox-live-title-storage"></a>Xbox Live 타이틀 저장소에서 JSON blob 저장

1.  *PUT* 메서드를 사용 하 여 타이틀 저장소에 데이터를 보내도록 요청을 보냅니다.

        PUT https://titlestorage.xboxlive.com/json/users/xuid(1245111)/scids/{scid}/data/{pathAndFileName},json
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Content-Length: 240
        Connection: Keep-Alive



-   사용자가 업데이트를 세션에 있어야 합니다.

-   STSTokenString는 편의 위해 자리 및 인증 요청으로 반환 하는 토큰으로 대체 되어야 합니다.

2.  JSON 개체를 보냅니다.

        {
            "startlevel":"1",
            "expression":"smile"
        }

#### <a name="reference"></a>참조

**/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)**
