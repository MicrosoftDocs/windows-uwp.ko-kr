---
title: 이진 blob 읽기
author: KevinAsgari
description: Xbox Live 타이틀 저장소의 이진 blob 읽기에 대해 알아봅니다.
ms.assetid: 9b8e0c35-0cea-4491-bf30-22fad224f11b
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 타이틀 저장소
ms.localizationpriority: medium
ms.openlocfilehash: 5dc5e429ab36621db1c5525ae7f1a8dc5da3b4fc
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6253536"
---
# <a name="reading-a-binary-blob-in-xbox-live-title-storage"></a>Xbox Live 타이틀 저장소의 이진 blob 읽기

1.  타이틀 저장소에서 데이터 읽기를 *GET* 메서드를 사용 하 여 요청을 보냅니다. 이 예제에서는 전역 타이틀 저장소를 사용 합니다.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/userinfo.bin,binary
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive



-   사용자는 업데이트를 세션에 있어야 합니다.

-   STSTokenString 편의 위해 자리 표시자 이며 인증 요청으로 반환 하는 토큰으로 대체 되어야 합니다.

#### <a name="reference"></a>참조

**/global/scids/{scid}/data/{pathAndFileName},{type}**
