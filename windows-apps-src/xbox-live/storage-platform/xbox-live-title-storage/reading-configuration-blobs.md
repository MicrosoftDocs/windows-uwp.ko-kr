---
title: 구성 blob 읽기
author: KevinAsgari
description: Xbox Live 타이틀 저장소에서 구성 blob 읽기는 방법을 알아봅니다.
ms.assetid: ee62d221-69b9-4f52-9b5d-5a44d04de548
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8f4ceed1c1258f2a53d1c5cb6306f27c7e8818a5
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5400718"
---
# <a name="reading-a-configuration-blob-in-xbox-live-title-storage"></a>Xbox Live 타이틀 저장소에서 구성 blob 읽기

*구성 blob* 은 게임 데이터를 저장 하는 Xbox Live 타이틀 저장소에서 파일입니다. 데이터는 게임에 전달 되 고 하기 전에 필터링 할 수 있는 가상 노드를 포함 하는 JSON 개체입니다. 구성 blob에 대 한 자세한 내용은 **타이틀 저장소 Uri**를 참조 하세요.

### <a name="to-read-a-configuration-blob"></a>구성 blob 읽기

1.  사용 하 여 요청 보내기는 타이틀 저장소에서 데이터를 읽을 하려면 아래 합니다.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/config.json,config              
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive


-   사용자는 업데이트를 세션에 있어야 합니다.
-   STSTokenString 편의 위해 자리 표시자 및 인증 요청으로 반환 하는 토큰으로 대체 되어야 합니다.

#### <a name="reference"></a>참조

**글로벌 / / scids / {서비스 안내} /data/ {pathAndFileName} {유형}**
**가져오기 (전역 / / scids / {서비스 안내} /data/ {pathAndFileName} {유형})**
