---
title: 이진 blob 읽기
description: Xbox Live 타이틀 저장소의 이진 blob 읽기 하는 방법을 알아봅니다.
ms.assetid: 9b8e0c35-0cea-4491-bf30-22fad224f11b
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 타이틀 저장소
ms.localizationpriority: medium
ms.openlocfilehash: e2a0be72142a3e11c7c680cfc998287f396c2afc
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2018
ms.locfileid: "8340450"
---
# <a name="reading-a-binary-blob-in-xbox-live-title-storage"></a>Xbox Live 타이틀 저장소의 이진 blob 읽기

1.  타이틀 저장소에서 데이터 읽기를 *GET* 메서드를 사용 하 여 요청을 보냅니다. 이 예제에서는 글로벌 타이틀 저장소를 사용 합니다.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/userinfo.bin,binary
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive



-   사용자는 업데이트를 세션에 있어야 합니다.

-   STSTokenString 편의 위해 자리 표시자 이며 인증 요청으로 반환 하는 토큰으로 대체 되어야 합니다.

#### <a name="reference"></a>참조

**/global/scids/{scid}/data/{pathAndFileName},{type}**
