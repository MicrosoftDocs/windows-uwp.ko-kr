---
title: 새로운 기능-2016 년 6 월 Xbox Live SDK 용
description: 새로운 기능-2016 년 6 월 Xbox Live SDK 용
ms.assetid: 306e7fa8-14f0-437f-a991-6693f5c0d6a8
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1d984d054d9e5fd7f9d34b42c1a224d53632e719
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595288"
---
# <a name="whats-new-for-the-xbox-live-sdk---june-2016"></a>새로운 기능-2016 년 6 월 Xbox Live SDK 용

참조 하세요 합니다 [새로운 기능-2016 년 4 월](1604-whats-new.md) 추가 된 기능에 대 한 문서는 2016 년 4 월 릴리스에서 합니다.

> **참고:** Xbox 개발자 키트 (XDK)를 사용 하는 경우 해당 *새로운 기능-2016 년 4 월* 년 3 월 XDK 릴리스 이후 추가 된 새로운 추가 기능을 다룹니다.

## <a name="os-and-tool-support"></a>OS 및 도구 지원
Xbox Live SDK는 Windows 10 RTM [Version 10.0.10240] 및 Visual Studio 2015 RTM [Version 14.0.23107.0]를 지원합니다.

## <a name="contextual-search"></a>상황에 맞는 검색
에 추가 된 다음 새 클래스를 `contextual_search` 게임 클립을 검색 하는 API:

* `contextual_search_game_clip`
* `contextual_search_game_clip_stat`
* `contextual_search_game_clip_thumbnail`
* `contextual_search_game_clip_uri_info`
* `contextual_search_game_clips_result`

자세한 참조 설명서를 참조 하세요.

## <a name="networking"></a>네트워킹
Xbox Live SDK는 이제 새 포함 어설션 5 개 이상의 websocket 단일 제목 인스턴스에서 사용자가 만들어질 경우를 검색 합니다.

이 비활성화할 수 있습니다를 호출 하 여 어설션 `disable_asserts_for_maximum_number_of_websockets_activated()`합니다.

> **참고:** 소셜 네트워크 관리자와 다중 접속 관리자 제목에 이러한 기능을 사용 하는 경우 단일 결합 된 websocket을 사용 합니다.

## <a name="tools"></a>도구
* 이제는 Xbox Live 복원 력 플러그 인에 대 한 Fiddler Xbox Live SDK에 포함 됩니다.  
개발자가 Xbox Live에 대 한 호출을 선택적으로 차단할 수 있는 Fiddler에 추가 된 기능 것입니다.
이 플러그 인을 사용 하 여 개발자는 복원 력 게임 제목에 부분 서비스 중단에서 테스트할 수 있습니다.
도구 형식의 Xbox Live 서비스 끝점 기본 제공 및 다른 오류에 대 한 테스트 수를 포함 합니다.
모든 Xbox One 및 UWP 제목은 지원 됩니다.
