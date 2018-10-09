---
title: JSON blob 읽기
author: KevinAsgari
description: Xbox Live 타이틀 저장소의 JSON blob 읽기 하는 방법을 알아봅니다.
ms.assetid: 3697af16-d054-4835-af7f-7fee8c628345
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d392269f9cde18f3fcf86bbe0e061aa957fd877a
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4415242"
---
# <a name="reading-a-json-blob-in-xbox-live-title-storage"></a>Xbox Live 타이틀 저장소에 JSON blob 읽기

1.  타이틀 저장소에서 데이터 읽기를 *GET* 메서드를 사용 하 여 요청을 보냅니다. 이 예제에서는 글로벌 타이틀 저장소를 사용 합니다.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/surprise.json,json
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive

-   사용자는 업데이트를 세션에 있어야 합니다.

-   STSTokenString 편의 위해 자리 표시자 및 인증 요청으로 반환 하는 토큰으로 대체 되어야 합니다.

#### <a name="reference"></a>참조

**/global/scids/{scid}/data/{pathAndFileName},{type}**