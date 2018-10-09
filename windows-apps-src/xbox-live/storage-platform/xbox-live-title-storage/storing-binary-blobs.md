---
title: 이진 blob 저장
author: KevinAsgari
description: Xbox Live 타이틀 저장소의 이진 blob 저장 하는 방법을 알아봅니다.
ms.assetid: a0da36ef-5a5a-466d-80a8-e055ba7e4cdc
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 타이틀 저장소
ms.localizationpriority: medium
ms.openlocfilehash: b74dd1943764b7dcfc7a635e569ed9bbd5d8e24b
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4460537"
---
# <a name="storing-a-binary-blob-in-xbox-live-title-storage"></a>Xbox Live 타이틀 저장소의 이진 blob 저장

1.  사용 하 여 요청 보내기는 아래 메서드를 타이틀 저장소에 데이터를 보냅니다.

        PUT https://titlestorage.xboxlive.com/trustedplatform/users/xuid(1245111)/scids/{scid}/data/lastturn.bin,binary              
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Content-Length: 40
        Connection: Keep-Alive


-   사용자는 업데이트를 세션에 있어야 합니다.

-   STSTokenString 편의 위해 자리 표시자 및 인증 요청으로 반환 하는 토큰으로 대체 되어야 합니다.

2.  이진 데이터를 보냅니다. HTTP를 통해 데이터를 전송 하는 이후 데이터 허용 문자 집합 제약 되어야 합니다. 이미지 또는 오디오 데이터 등의 정보로 인코딩되어야 합니다. HTTP 호환 문자를 생성 하는 모든 인코딩 방법을 선택할 수 있습니다.
d
```
  01EAEFBAD05903A4
  1EA2311656677DFF
  CF00
```

#### <a name="reference"></a>참조

**/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}**
