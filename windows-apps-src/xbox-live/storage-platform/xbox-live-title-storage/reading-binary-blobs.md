---
title: 이진 blob 읽기
author: KevinAsgari
description: Xbox Live 타이틀 저장소에서 이진 blob 읽기에 대해 알아봅니다.
ms.assetid: 9b8e0c35-0cea-4491-bf30-22fad224f11b
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 타이틀 저장소
ms.localizationpriority: medium
ms.openlocfilehash: f6f6165dafdff76c7a0e605a62b5bc89cb5f7558
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4618016"
---
# <a name="reading-a-binary-blob-in-xbox-live-title-storage"></a>Xbox Live 타이틀 저장소에서 이진 blob 읽기

1.  타이틀 저장소에서 데이터 읽기를 *GET* 메서드를 사용 하 여 요청을 보냅니다. 이 예제에서는 전역 타이틀 저장소를 사용 합니다.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/userinfo.bin,binary
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive



-   사용자가 업데이트를 세션에 있어야 합니다.

-   STSTokenString는 편의 위해 자리 및 인증 요청으로 반환 하는 토큰으로 대체 되어야 합니다.

#### <a name="reference"></a>참조

**/global/scids/{scid}/data/{pathAndFileName},{type}**
