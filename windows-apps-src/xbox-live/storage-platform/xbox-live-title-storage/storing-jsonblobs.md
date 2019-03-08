---
title: JSON blob을 저장합니다.
description: Xbox Live 제목 저장소에서 JSON blob을 저장 하는 방법에 알아봅니다.
ms.assetid: f424aca1-e671-4e31-84c6-098fda4a9558
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 제목 저장소
ms.localizationpriority: medium
ms.openlocfilehash: dabcd0634bb20bc6b82ae302c77338b5bb726a63
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595068"
---
# <a name="storing-a-json-blob-in-xbox-live-title-storage"></a>Xbox Live 제목 저장소에서 JSON blob을 저장합니다.

1.  사용 하 여 요청을 전송 합니다 *배치* 제목 저장소에 데이터를 보내기 위한 메서드.

        PUT https://titlestorage.xboxlive.com/json/users/xuid(1245111)/scids/{scid}/data/{pathAndFileName},json
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Content-Length: 240
        Connection: Keep-Alive



-   사용자 세션 업데이트 해야 합니다.

-   STSTokenString은 간략하게 표현 하기 위해 자리 표시자 이며 인증 요청에 의해 반환 된 토큰으로 바꿔야 합니다.

2.  JSON 개체를 보냅니다.

        {
            "startlevel":"1",
            "expression":"smile"
        }

#### <a name="reference"></a>참고자료

**/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)**
