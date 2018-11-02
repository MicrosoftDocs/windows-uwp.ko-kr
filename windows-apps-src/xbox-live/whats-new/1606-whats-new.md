---
title: 새로운 기능에 대 한 Xbox Live SDK-6 월 2016
author: KevinAsgari
description: 새로운 기능에 대 한 Xbox Live SDK-6 월 2016
ms.assetid: 306e7fa8-14f0-437f-a991-6693f5c0d6a8
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1e6be7c4f42ebca395100cf5dac677797d936719
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5926846"
---
# <a name="whats-new-for-the-xbox-live-sdk---june-2016"></a>새로운 기능에 대 한 Xbox Live SDK-6 월 2016

추가 된 항목에 대 한 [새로운 기능-2016 년 4 월](1604-whats-new.md) 문서를 참조 하십시오 4 월 2016 릴리스에서 합니다.

> **참고:** (XDK (Xbox 개발자 키트)를 사용 하는 경우 *제공 되는 새로운-2016 년 4 월* 문서 3 월 XDK 릴리스 이후 추가 된 새로운 기능을 설명 합니다.

## <a name="os-and-tool-support"></a>운영 체제 및 도구 지원
Xbox Live SDK는 Windows 10 RTM [버전 10.0.10240] 및 Visual Studio 2015 RTM [버전 14.0.23107.0]를 지원합니다.

## <a name="contextual-search"></a>상황에 맞는 검색
다음과 같은 새로운 클래스에 추가 된 합니다 `contextual_search` 게임 클립을 검색 하는 API:

* `contextual_search_game_clip`
* `contextual_search_game_clip_stat`
* `contextual_search_game_clip_thumbnail`
* `contextual_search_game_clip_uri_info`
* `contextual_search_game_clips_result`

자세한 내용은 참조 설명서를 참조 하세요.

## <a name="networking"></a>네트워킹
Xbox Live SDK는 이제 새 포함 어설션는 감지 단일 제목 인스턴스에서 사용자 당 5 이상의 websocket이 생성 됩니다.

이 비활성화할 수를 호출 하 여 어설션 `disable_asserts_for_maximum_number_of_websockets_activated()`.

> **참고:** 소셜 관리자 및 멀티 플레이어 관리자 제목에 이러한 기능을 사용 하는 경우 단일 결합 된 websocket을 사용 합니다.

## <a name="tools"></a>도구
* Xbox Live 복원 력 플러그 인에 대 한 Fiddler Xbox Live SDK에 포함 됩니다.  
개발자가 Xbox Live에 대 한 호출을 선택적으로 차단할 수 있도록 Fiddler 추가 기능입니다.
이 플러그인을 사용 하 여, 개발자는 복원 력 부분 서비스 중단 게임 제목에 걸쳐 테스트할 수 있습니다.
도구에는 다양 한 테스트 서비스 끝점 기본 제공 하 고 다른 오류 유형 Xbox Live 포함 됩니다.
모든 Xbox One 및 UWP 타이틀이 지원 됩니다.
