---
title: 이진 blob을 저장
description: Xbox Live 제목 저장소에 이진 blob을 저장 하는 방법에 알아봅니다.
ms.assetid: a0da36ef-5a5a-466d-80a8-e055ba7e4cdc
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 제목 저장소
ms.localizationpriority: medium
ms.openlocfilehash: 70e620f2e992dfdd240f71b5c73165f13d4ef5cb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660098"
---
# <a name="storing-a-binary-blob-in-xbox-live-title-storage"></a>Xbox Live 제목 저장소에 이진 blob을 저장합니다.

1.  사용 하 여 요청 보내기는 title 저장소로 데이터를 보내기 위한 메서드를 아래.

        PUT https://titlestorage.xboxlive.com/trustedplatform/users/xuid(1245111)/scids/{scid}/data/lastturn.bin,binary              
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Content-Length: 40
        Connection: Keep-Alive


-   사용자 세션 업데이트 해야 합니다.

-   STSTokenString은 간략하게 표현 하기 위해 자리 표시자 이며 인증 요청에 의해 반환 된 토큰으로 바꿔야 합니다.

2.  이진 데이터를 보냅니다. HTTP를 통해 데이터를 전송 하므로 허용 가능한 문자 집합에 데이터를 제한 해야 합니다. 이미지나 오디오 데이터와 같은 정보를 인코딩해야 합니다. HTTP 호환 문자를 생성 하는 모든 인코딩 메서드를 선택할 수 있습니다.
d
```
  01EAEFBAD05903A4
  1EA2311656677DFF
  CF00
```

#### <a name="reference"></a>참고자료

**/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}**
