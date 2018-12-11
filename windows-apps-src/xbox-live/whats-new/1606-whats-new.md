---
title: 새로운 기능에 대 한 Xbox Live SDK-2016 년 6 월
description: 새로운 기능에 대 한 Xbox Live SDK-2016 년 6 월
ms.assetid: 306e7fa8-14f0-437f-a991-6693f5c0d6a8
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1d984d054d9e5fd7f9d34b42c1a224d53632e719
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8875280"
---
# <a name="whats-new-for-the-xbox-live-sdk---june-2016"></a>새로운 기능에 대 한 Xbox Live SDK-2016 년 6 월

추가 된 항목에 대 한 [새로운-2016 년 4 월](1604-whats-new.md) 문서를 참조 하십시오 2016 년 4 월 릴리스에서 합니다.

> **참고:** (XDK (Xbox 개발자 키트)를 사용 하는 경우 *새로운-2016 년 4 월* 문서에서는 3 월 XDK 릴리스 이후 추가 된 새로운 기능을 설명 합니다.

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
Xbox Live SDK는 이제 새 포함 어설션는 감지 단일 품목이 인스턴스에서 사용자 당 5 개 이상의 websocket이 생성 됩니다.

이 비활성화할 수를 호출 하 여 어설션 `disable_asserts_for_maximum_number_of_websockets_activated()`.

> **참고:** 소셜 관리자 및 멀티 플레이어 관리자 타이틀에서 이러한 기능을 사용 하는 경우 단일 결합 된 websocket을 사용 합니다.

## <a name="tools"></a>도구
* Xbox Live 복원 력 플러그 인에 대 한 Fiddler Xbox Live SDK에 포함 됩니다.  
추가 기능을 통해 개발자는 선택적으로 Xbox Live에 대 한 호출을 차단 하는 Fiddler입니다.
이 플러그 인을 사용 하 여, 개발자는 복원 력 부분 서비스 중단 게임 제목에 걸쳐 테스트할 수 있습니다.
이 도구는 여러를 형식의 Xbox Live 서비스 끝점 기본 제공 하 고 다른 오류에 대 한 테스트를 포함 되어 있습니다.
모든 Xbox One 및 UWP 타이틀이 지원 됩니다.
