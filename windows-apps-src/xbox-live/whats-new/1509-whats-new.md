---
title: 새로운 기능-2015 년 9 월 Xbox Live SDK에 대 한
description: 새로운 기능-2015 년 9 월 Xbox Live SDK에 대 한
ms.assetid: 84b82fde-f6f3-4dc2-b2df-c7c7313a2cc3
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c618a738dc2670d3d3de1fa2f4c4108c24130eb0
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8782976"
---
# <a name="whats-new-for-the-xbox-live-sdk---september-2015"></a>새로운 기능-2015 년 9 월 Xbox Live SDK에 대 한

추가 된 항목에 대 한 [새로운 기능-2015 년 8 월](1508-whats-new.md) 문서를 참조 하십시오 2015 년 8 월 릴리스에서 합니다.

Xbox Live SDK의 9 월 릴리스는 다음과 같은 업데이트가 포함 됩니다.

## <a name="os-and-tool-support"></a>운영 체제 및 도구 지원 ##
Xbox Live SDK는 Windows 10 RTM [버전 10.0.10240] 및 Visual Studio 2015 RTM [버전 14.0.23107.0]를 지원합니다.

## <a name="contextual-search-apis"></a>상황에 맞는 검색 Api
* 제목이 나 응용 프로그램에서 사용자가 선택한 실시간으로 통계를 사용 하 여 사용자 game(s) 브로드캐스트 검색을 사용 하도록 설정 합니다.
* Microsoft::Xbox::Services::ContextualSearch의 새로운 Api 참조

## <a name="app-insights-for-events"></a>이벤트에 대 한 앱 정보

| 참고 |
|------|
| 앱 인 사이트는 UWP 제목에만 적용 됩니다.  XDK 타이틀을 개발 하는 경우이 섹션에는 적용 되지 않습니다. |

<p/>

* AppInsights를 사용 하 여 write_in_game_event()를 사용 하 여 작성 하는 이벤트를 볼 수 있습니다.
* 이 설명서 곧 제공 될 앞으로 그 하십시오 작동 방식으로 액세스 하 여 댐을

## <a name="logging"></a>로깅
* xbox::services에서 service_call_logging_config:: 실험적
* 시작 하 고 콘솔에 xbTrace.exe 통해 추적을 중지 하려면 register_for_protocol_activation theservice_call_logging_config 클래스에서 호출 해야 합니다.  한 번에 게임 초기화 하는 동안이 호출을 수행 합니다.

## <a name="resync-for-rta"></a>RTA에 대 한 재 동기화
* 재 동기화 RTA 서비스는 사용자가 정보를 만료 된 있을 때 발생할 수 있습니다.
* 타이틀 구독 하는 구독에 대 한 해당 HTTP 호출을 호출 해야
* 다시 하지 않아도 제목
* 추가 된 xbox::services::real_time_activity_service::add_resync_handler
* 제거 된 xbox::services::real_time_activity_service::remove_resync_handler
* 추가 된 http_status_429_too_many_requests
* 제목이 너무 많은 http 요청을 보내는 데 조정 됩니다. 하면이 오류가 표시 됩니다.

## <a name="documentation"></a>설명서
* Xbox Live 서비스 2.0 API로 마이그레이션
* 오류 처리
* Windows 10의에서 Xbox Live 인증
* 상황에 맞는 검색
