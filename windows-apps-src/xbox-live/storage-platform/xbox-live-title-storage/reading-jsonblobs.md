---
title: JSON blob을 읽는 중
description: Xbox Live 제목 저장소에서 JSON blob을 읽는 방법에 알아봅니다.
ms.assetid: 3697af16-d054-4835-af7f-7fee8c628345
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 30d2b9f6539e73df1c5ea5ae18b3d1a6ca061d60
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613488"
---
# <a name="reading-a-json-blob-in-xbox-live-title-storage"></a>Xbox Live 제목 저장소에서 JSON blob을 읽는 중

1.  사용 하 여 요청을 전송 합니다 *가져오기* 제목 저장소에서 데이터를 읽으려고 하는 방법. 이 예제에서는 전역 제목 저장소를 사용합니다.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/surprise.json,json
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive

-   사용자 세션 업데이트 해야 합니다.

-   STSTokenString은 간략하게 표현 하기 위해 자리 표시자 이며 인증 요청에 의해 반환 된 토큰으로 바꿔야 합니다.

#### <a name="reference"></a>참고자료

**/global/scids/{scid}/data/{pathAndFileName},{type}**
