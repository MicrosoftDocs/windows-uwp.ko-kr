---
title: 구성 blob을 읽는 중
description: Xbox Live 제목 저장소의 구성 blob을 읽는 방법에 알아봅니다.
ms.assetid: ee62d221-69b9-4f52-9b5d-5a44d04de548
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 71607f05f07ee4a98f73a19c28c85dc325041a10
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599398"
---
# <a name="reading-a-configuration-blob-in-xbox-live-title-storage"></a>Xbox Live 제목 저장소의 구성 blob을 읽는 중

*구성 blob* 은 제목 저장소 Xbox Live 게임 데이터를 보유 하는 파일입니다. 데이터는 게임으로 배달 하기 전에 필터링 할 수 있는 가상 노드를 포함 하는 JSON 개체입니다. 구성 blob에 대 한 자세한 내용은 참조 하세요. **Title 저장소 Uri**합니다.

### <a name="to-read-a-configuration-blob"></a>구성 blob를 읽으려면

1.  사용 하 여 요청 보내기는 title 저장소에서 데이터를 읽는 데 메서드 아래.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/config.json,config              
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive


-   사용자 세션 업데이트 해야 합니다.
-   STSTokenString은 간략하게 표현 하기 위해 자리 표시자 이며 인증 요청에 의해 반환 된 토큰으로 바꿔야 합니다.

#### <a name="reference"></a>참고자료

**/global/scids/{scid}/data/{pathAndFileName},{type}**
**GET (/global/scids/{scid}/data/{pathAndFileName},{type})**
