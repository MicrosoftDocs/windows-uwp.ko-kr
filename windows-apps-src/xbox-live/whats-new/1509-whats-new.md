---
title: 새로운 기능-2015 년 9 월 Xbox Live SDK 용
description: 새로운 기능-2015 년 9 월 Xbox Live SDK 용
ms.assetid: 84b82fde-f6f3-4dc2-b2df-c7c7313a2cc3
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c618a738dc2670d3d3de1fa2f4c4108c24130eb0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602278"
---
# <a name="whats-new-for-the-xbox-live-sdk---september-2015"></a>새로운 기능-2015 년 9 월 Xbox Live SDK 용

참조 하세요 합니다 [새로운 기능-2015 년 8 월](1508-whats-new.md) 추가 된 기능에 대 한 문서는 2015 년 8 월 릴리스에서 합니다.

Xbox Live SDK의 9 월 릴리스를 다음 업데이트가 포함 되어 있습니다.

## <a name="os-and-tool-support"></a>OS 및 도구 지원 ##
Xbox Live SDK는 Windows 10 RTM [Version 10.0.10240] 및 Visual Studio 2015 RTM [Version 14.0.23107.0]를 지원합니다.

## <a name="contextual-search-apis"></a>상황별 검색 Api
* 제목 또는 브로드캐스트를 선택 해 실시간 통계를 사용 하 여 프로그램 game(s)에서 검색할 응용 프로그램을 사용 하도록 설정 합니다.
* Microsoft::Xbox::Services::ContextualSearch에서 새로운 Api를 참조 하세요

## <a name="app-insights-for-events"></a>이벤트에 대 한 app Insights

| 참고 |
|------|
| App Insights UWP 제목에만 적용 됩니다.  이 섹션에서는 XDK title을 개발 하는 경우에 적용 되지 않습니다. |

<p/>

* AppInsights를 사용 하 여 write_in_game_event()를 사용 하 여 작성 하는 이벤트를 볼 수 있습니다.
* 이 설명서를 제공 될 예정 나중에 그 동안 함께 하세요 액세스 하 여 파악

## <a name="logging"></a>로깅
* xbox::services에서 service_call_logging_config:: 실험적
* 시작 하 고 콘솔에 xbTrace.exe 통해 추적 중지를 register_for_protocol_activation service_call_logging_config 클래스에서 호출 해야 합니다.  게임 초기화 중에 한 번이 호출을 확인 합니다.

## <a name="resync-for-rta"></a>RTA에 대 한 다시 동기화
* 다시 동기화는 RTA 서비스 사용자 정보를 최신 수 있다고 인식 하는 경우에 발생할 수 있습니다.
* 제목에 가입 된 구독에 대 한 해당 HTTP 호출을 호출 해야 합니다.
* 제목을 재구독 할 필요가 없습니다.
* Added xbox::services::real_time_activity_service::add_resync_handler
* Removed xbox::services::real_time_activity_service::remove_resync_handler
* 추가 http_status_429_too_many_requests
* 제목이 너무 많은 http 요청을 보내는 제한 하려는 경우이 오류 상태가 표시 됩니다.

## <a name="documentation"></a>설명서
* Xbox Live 서비스 API 2.0으로 마이그레이션
* 오류 처리
* Windows 10의에서 Xbox Live 인증
* 상황에 맞는 검색
