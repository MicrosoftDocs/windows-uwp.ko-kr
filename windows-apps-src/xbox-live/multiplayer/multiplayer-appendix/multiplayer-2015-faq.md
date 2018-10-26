---
title: 멀티 플레이 2015 FAQ 및 문제 해결
author: KevinAsgari
description: Xbox Live 멀티 플레이 2015 및 문제 해결에 대 한 질문과 대답 합니다.
ms.assetid: 75823f10-b342-4e20-b885-e5ad4392bc3d
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 멀티 플레이어
ms.localizationpriority: medium
ms.openlocfilehash: e9c6778ebc90d917a883a437d905112cf32a6829
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5558417"
---
# <a name="multiplayer-2015-faq-and-troubleshooting"></a>멀티 플레이 2015 FAQ 및 문제 해결

-   새 타이틀을 개발 하 고 있습니다. 멀티 플레이 API 요소를 사용 해야 하나요?
-   새 멀티 플레이 API 서비스에서 액세스할 수는 방법
-   내 제목에 가입할 수 변경 둘 이상의 세션에 대 한?
-   제거 됩니다. 사용자가 즉시 네트워크 연결이 끊어졌습니다 합리적인 또는 콘솔을 해제 하는 경우?
-   어떤 서비스 안내, 세션 템플릿 및 샌드박스를 사용 하는 방법을 확인 하려면?
-   I 내 제목에 대 한을 비교할 수 있는 요청 본문의 예로 나와 있습니다.
-   HTTP/403 코드 MPSD를 호출할 때 발생 합니다.
-   HTTP/404 코드 MPSD를 호출할 때 발생 합니다.
-   **MultiplayerService.WriteSessionByHandleAsync** 또는 **MultiplayerService.GetCurrentSessionByHandleAsync**를 호출할 때 HTTP/404 코드를 가져오는 하는 이유
-   HTTP/412 코드 MPSD를 호출할 때 발생 합니다.
-   HTTP/400, 405, 409, 503, 나타남 MPSD를 호출할 때 등 코드입니다.
-   할 수 또는 바꾸어야 세션 템플릿에서 내 제목에 대 한?
-   세션 초기화 되지 있다는 오류가 발생 합니다.
-   세션이 만들어지지 않습니다 및 HTTP/204 코드 발생 합니다.
-   MPSD 폴링하지 경우
-   발생 하는 경우 예약 하거나 초대 된 플레이어 세션에 조인 하지 않는 것?
-   이유는 생성된 된 세션에서 찾을 수 매치 메이 킹?
-   당사자에 게 2015 멀티 플레이어에서 제대로 사용 되는 방식과 2014 멀티 플레이어에서 사용 되는 방식으로 주요 차이점은 무엇입니까?
-   게임 세션 플레이어 슬롯에 진행 중인 이유는 사용자가 할 수 없습니다 시작 된 후 세션을 찾을 가입을 지 원하는 경우?
-   게임 세션 열려 있으면 게임이 방금 참가 한 사용자 세션에 참가 하 고 수 예약에 대 한 기다릴 필요 없이 재생이 시작?
-   큰 게임 세션 내 제목에서 재생 하는 경우 이유 세션의 모든 구성원 보이지 않는 게임 초대 알림?
-   게임 되지 않은 동작이 유발 표시 하 고 게임 타사 기능을 참조 하는 프로토콜 정품 인증 정보를 수신 합니다.
-   표시 되는 이유 v105 세션 문서에 대 한 구문 내 추적에 v107 세션 템플릿 들이 구성한 있지만?


### <a name="i-am-developing-a-new-title-which-multiplayer-api-elements-should-i-use"></a>새 타이틀을 개발 하 고 있습니다. 멀티 플레이 API 요소를 사용 해야 하나요?

2014 멀티 플레이 기능은 기존 제목에 적용 계속 될 가능성이 가장 높은 관련된 API 요소 사용 되지 않습니다. 2015에서 릴리스에 대 한 클라이언트를 준비 하는 경우 2015 멀티 플레이어의 사용 적극 권장 합니다.


### <a name="how-can-i-access-the-new-multiplayer-api-from-a-service"></a>새 멀티 플레이 API 서비스에서 액세스할 수는 방법

[MPSD 호출](multiplayer-session-directory.md)을 참조 하세요.


### <a name="can-my-title-subscribe-to-changes-for-more-than-one-session"></a>내 제목에 가입할 수 변경 둘 이상의 세션에 대 한?

예, 타이틀 연결 당 최대 10 세션에서 변경 내용을 수신 구독할 수 있습니다.


### <a name="will-a-user-be-removed-immediately-if-heshe-loses-a-network-connection-or-turns-off-the-console"></a>제거 됩니다. 사용자가 즉시 네트워크 연결이 끊어졌습니다 합리적인 또는 콘솔을 해제 하는 경우?

웹 소켓 연결 MPSD를 감지 하 여 신속 하 게 클라이언트 연결 끊기 비활성으로 클라이언트를 설정할 수 있습니다. 세션 멤버 비활성 제거 제한 시간이 만료 되는 즉시 제거 됩니다. 자세한 내용은 [세션 시간 제한](mpsd-session-details.md)을 참조 하세요.


### <a name="how-do-i-find-out-what-scid-session-template-and-sandbox-to-use"></a>어떤 서비스 안내, 세션 템플릿 및 샌드박스를 사용 하는 방법을 확인 하려면?

이 정보를 만들 때 제목의 초기 등록 관련 되지 않은,이 정보에 액세스할 수 없는 작업을 수행 합니다. 이 데이터를 가져올 수 있는 개발자 계정 관리자에 게 문의 합니다.


### <a name="is-there-an-example-of-a-request-body-that-i-can-compare-to-the-one-for-my-title"></a>I 내 제목에 대 한을 비교할 수 있는 요청 본문의 예로 나와 있습니다.

**MultiplayerSessionRequest (JSON)** 에서 요청 구조를 참조 하세요.


### <a name="i-am-getting-an-http403-code-when-calling-mpsd"></a>HTTP/403 코드 MPSD를 호출할 때 발생 합니다.

이 일반적으로 사용 권한이 나 범위 문제입니다. 자세한 내용을 보려면 다음 메시지를 확인 하는 Fiddler 추적 수집 일반적인 403 메시지에 대 한 HttpResponse 본문의 일환으로 반환 됩니다.

-   요청 된 서비스 구성에 액세스할 수 없습니다.
    1.  해당 샌드박스에 액세스 권한이 있는 계정을 사용 하 고 있는지 확인 합니다.
    2.  올바른 샌드박스 내에 있는지 확인 합니다.
     3.  인증서 인증을 사용 하 고이 오류가 발생 하는 경우 개발자 계정 관리자에 게 문의 합니다.   요청 된 세션에 액세스할 수 없습니다. 호출 하는 사용자의 멀티 플레이 권한이 있어야 합니다. 및 개인 세션 세션 구성원이 읽을 수 있습니다.

    개인의 가시성을 가진 세션에 액세스 하려고 하기 때문에 가능한 경우 세션을 볼 수가 필요는 없습니다. 가시성 Private로 설정 하는 경우 해당 세션의 구성원만 세션 문서를 읽을 수 있습니다.

| 참고                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 사용자가 새 세션을 등록 하기 위해 골드 계정이 있어야 합니다. 골드 재생 중인 사용자 계정 권한 없이 새 세션을 등록 하는 요청 HTTP/403를 반환 합니다. |

-   요청 본문 사용자 인증 서버를 포함 하지 않는 한 기존 멤버에 대 한 참조를 포함할 수 없습니다.

    사용자를 대신 하 여 다른 사용자 세션에 참가할 수 없습니다. 만 초대할 수 있습니다. 인덱스 설정 "reserve\_&lt;번호&gt;" 플레이어를 초대할 수 있습니다.


### <a name="i-am-getting-an-http404-code-when-calling-mpsd"></a>HTTP/404 코드 MPSD를 호출할 때 발생 합니다.

자세한 내용을 보려면 다음을 수행 하는 Fiddler 추적 정보를 수집 합니다.

1.  일반적인 404 메시지에 대 한 HttpResponse 본문의 일환으로 반환 된 메시지를 확인 합니다.
    -   세션에 대 한 잘못 된 또는 구성 되지 않음이 하는 요청 된 서비스 구성 합니다. 사용 중인 서비스 안내 올바른지 확인 합니다.
    -   요청 된 세션을 찾을 수 없습니다. 세션 만들고 검색 하기 전에 세션 템플릿 올바른지 확인 합니다. PUT 호출 하 여 세션을 만들 수 있습니다.

2.  사용 하는 URI를 확인 합니다.
3.  콘솔 다시 부팅 하거나 새 사용자를 사용 하 여 시도 합니다.
4.  오류 코드 또는 다른 솔루션에 대 한 게임 개발자 포럼을 확인 합니다.
5.  세션 비어의 결과로 삭제 되지 않았는지 확인 합니다. 세션 시간 제한 사용자 때문에 빈 될 수 있습니다. 이런 경우 자주 세션의 모든 구성원 시간 제한을 적용 되는, 예: "준비" 또는 "비활성" 상태 이므로 됩니다. 사용자 상태 세부 정보 [세션 사용자 상태](mpsd-session-details.md) 를 참조 하세요.


### <a name="why-am-i-getting-an-http404-code-when-calling-multiplayerservicewritesessionbyhandleasync-or-multiplayerservicegetcurrentsessionbyhandleasync"></a>**MultiplayerService.WriteSessionByHandleAsync** 또는 **MultiplayerService.GetCurrentSessionByHandleAsync**를 호출할 때 HTTP/404 코드를 가져오는 하는 이유

타이틀 MPSD 세션에 대 한 응답 핸들 ID가 포함 된 조인 프로토콜 활성화를 처리 하 여 액세스 하는 경우에 프로토콜 활성화 핸들 ID 부실 다음 중 하나에 대 한 다음과 같습니다.

-   제목 가입을 시작 하는 Xbox 셸을 UI 보기 만료 된 수도 있습니다. 열려 있는 동안 일부 UI 보기, 예를 들어, 사용자의 프로필 카드 가입 핸들을 자동으로 새로 고쳐지지 않습니다. HTTP/404 코드를 받지 않으려면 제목 해야 닫고 조인 하기 전에 데이터를 새로 고치려면 보기.
-   제목에 가입 하는 사용자 수 있는 전환 활동 제목 Xbox 셸 UI에서에서 조인 작업을 선택한 후 세션. 이러한 이유로 HTTP/404 코드에 대 한 거의 발생 하지 않아야 합니다.

두이 경우 모두에서 제목 코드 해야 사용자에 게 표시 조인 가입 실패 했음을 나타내는 오류 메시지입니다.


### <a name="i-am-getting-an-http412-code-when-calling-mpsd"></a>HTTP/412 코드 MPSD를 호출할 때 발생 합니다.

다음 요청 세션을 이미 존재 하는 경우 HTTP/412를 반환 합니다.

    PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/foo HTTP/1.1
    Content-Type: application/json
    If-None-Match: *


다음 요청 세션 etag는 헤더와 일치 하지 않으면 HTTP/412를 반환 합니다.

    PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/foo HTTP/1.1
    Content-Type: application/json
    If-Match: 9555A7DE-8B91-40E4-8CFB-0629312C9C7D


자세한 내용은 [세션 업데이트 동기화를](multiplayer-session-directory.md) 참조 하세요.


### <a name="i-am-getting-an-http400-405-409-503-etc-code-when-calling-mpsd"></a>HTTP/400, 405, 409, 503, 나타남 MPSD를 호출할 때 등 코드입니다.

자세한 내용을 보려면 Fiddler 추적을 수집 하 고 메시지 확인 HttpResponse 본문의 일환으로 반환 하는 다음 키를 누릅니다. 식별 하 고 오류를 해결 하려면 충분 한 정보를 제공 해야이 또는 해상도 대 한 개발자 포럼을 검색할 수 있습니다.

가져올 수도 있습니다 응답 본문 XSAPI를 사용 하는 경우 [Xbox Live 서비스 API 문제 해결](../../using-xbox-live/troubleshooting/troubleshooting-the-xbox-live-services-api.md)에 설명 된 대로 합니다. 또는 코드는 출력 타이틀 UI 보내도록 **XboxLiveContextSettings.EnableServiceCallRoutedEvents** 속성을 사용할 수 있습니다.


### <a name="what-can-or-should-i-change-in-the-session-templates-for-my-title"></a>할 수 또는 바꾸어야 세션 템플릿에서 내 제목에 대 한?

세션 템플릿 사용자 세션에 대 한 패턴 되며 템플릿에서 이미 설정 되어 상수를 재정의할 수 없습니다. 그러나 템플릿에 새 속성을 추가할 수 있습니다. [MPSD 세션 템플릿](multiplayer-session-directory.md) 세부 정보를 참조 하세요.


### <a name="im-getting-an-error-that-is-saying-my-session-isnt-initialized"></a>세션 초기화 되지 있다는 오류가 발생 합니다.

예제 응답 오류:

400-\[ResponseBody\]:이 세션은 2 개 이상의 구성원이 시작 하도록 요구 하는 관리 되는 초기화에 대해 구성 합니다.

True로 "초기화" 필드 집합을 사용 하 여 부족 세션 멤버 예약 요청에 포함 된 세션을 만들 수 없습니다. 코드는 **MultiplayerSession.Join** 메서드 또는 **MultiplayerSession.AddMemberReservation** 메서드에 대해 *initializeRequested* 매개 변수를 사용 하 여 구성원에 대 한이 필드를 설정할 수 있습니다.

세션 템플릿에서 초기화가 지정 되어 있는지 확인 "초기화": "true"로 설정 되어 충분히 멤버 예약에 대 한 QoS 매치를 전달 합니다. 자세한 내용은 [대상 세션 초기화 단계 및 QoS를](smartmatch-matchmaking.md)참조 하세요.


### <a name="my-session-is-not-being-created-and-im-getting-an-http204-code"></a>세션이 만들어지지 않습니다 및 HTTP/204 코드 발생 합니다.

이 상태 코드는 사용자가 만든 경우 세션에 추가 되었는지 나타냅니다. 생성 되 고 있는 경우 빈 세션 시간 제한을 0 (기본값) 세션에 사용자가 없는 경우 세션 만들어지지 않습니다.

세션을 만들 때 하나 이상의 플레이어를 포함 해야 합니다. 전용된 서버 시나리오에 대 한 사용자가 일치 하는 항목을 만드는 하려고 하거나 일치를 만들고 해당 플레이어 초기 플레이어 일치에서 해야 하는 플레이어를 가져옵니다. 또는 변경 하거나 세션 빈 제한 시간을 제거할 수도 있습니다. 자세한 내용은 [세션 시간 제한](mpsd-session-details.md)을 참조 하세요.


### <a name="when-should-i-poll-mpsd"></a>MPSD 폴링하지 경우

제목은 MPSD 폴링 피해 야 합니다. 타이틀 MPSD 세션에 변경 내용을 찾아야, 세션 변경 이벤트에 대 한 가입 해야 합니다. 자세한 내용은 참조 [하는 방법: MPSD 세션 변경 알림을 등록](multiplayer-how-tos.md).


### <a name="what-happens-if-a-player-who-was-reserved-or-invited-to-the-session-does-not-join-it"></a>발생 하는 경우 예약 하거나 초대 된 플레이어 세션에 조인 하지 않는 것?

플레이어는 게임 세션은 준비 하기 위해 알림 메시지를 받으면 제목이 실행 중인지 여부에 따라 다릅니다. 플레이어가 제목에는 제목 UI에서에서 게임 세션 알림을 받지 않습니다 경우 **MultiplayerSession.Leave** 메서드를 사용 하 여 게임 세션에서 플레이어를 제거 하려면 제목 최대 됩니다.

제목 제한 된 또는 실행 되지 않는 셸 알림을 제공 하는 경우는 플레이어에 게 알려는 슬롯을 사용할 수 있습니다. 플레이어가 거절 하거나 시스템 알림을 무시, MPSD 게임 세션에서 해당 사용자를 제거 합니다.


### <a name="why-would-a-created-session-not-be-found-by-matchmaking"></a>이유는 생성된 된 세션에서 찾을 수 매치 메이 킹?

Xbox One에서 세션을 간단 하 게 만드는 충분 하지 않습니다 매치 메이 킹 새 세션을 찾을 수 있습니다. 광고 매치 메이 킹 서비스 세션을 시작 하려면 일치 티켓을 만들어야 합니다. [스마트 매치](smartmatch-matchmaking.md)를 참조 하세요.


### <a name="what-is-the-key-difference-between-the-way-parties-are-properly-used-by-2015-multiplayer-and-the-way-they-were-used-in-2014-multiplayer"></a>당사자에 게 2015 멀티 플레이어에서 제대로 사용 되는 방식과 2014 멀티 플레이어에서 사용 되는 방식으로 주요 차이점은 무엇입니까?

2015 멀티 플레이어 멀티 플레이 API 없는 시스템 수준의 게임 파티, 전용 채팅 당사자를 정의합니다. 게임 당사자를 사용 하는 대신 제목 세션을 사용 하 여 가입 제어, 초대, 및 관련 기능입니다. 2014 멀티 플레이어에 대 한 Xbox One의 멀티 플레이 API 눈에 띄게 효과적으로 게임 초대 하는 대신 시스템 수준 가입 로비를 구현 하는 게임 타사 개념 (**타사** 클래스)를 사용 합니다.


### <a name="if-a-game-session-has-open-player-slots-and-supports-join-in-progress-why-would-users-not-be-able-to-find-the-session-once-it-has-started"></a>게임 세션 플레이어 슬롯에 진행 중인 이유는 사용자가 할 수 없습니다 시작 된 후 세션을 찾을 가입을 지 원하는 경우?

게임 세션을 시작할 때 매치 메이 킹 서비스에서 더 이상 보급 됩니다. 플레이어 슬롯 세션 내에서 사용할 수 있게 하는 경우 (호스트) 중재자 새로운 플레이어를 모집 하려는 중재자 해야 진행 중인 세션에 대 한 새 일치 티켓을 **MatchmakingService.CreateMatchTicketAsync** 메서드를 사용 하 여 만듭니다. 티켓 세션을 다시 보급 하 고 더 많은 플레이어를 찾습니다. [스마트 매치](smartmatch-matchmaking.md)를 참조 하세요.


### <a name="if-a-game-session-is-open-can-a-user-who-has-just-joined-a-game-simply-join-the-session-and-start-playing-without-having-to-wait-for-the-reservation"></a>게임 세션 열려 있으면 게임이 방금 참가 한 사용자 세션에 참가 하 고 수 예약에 대 한 기다릴 필요 없이 재생이 시작?

그렇습니다. 타이틀 게임 세션 내에서 플레이어의 하위 그룹을 추적 하려면 여러 세션을 사용 하는 경우에 특히 유용 합니다. 조인 사용자가 그룹을 나타내는 세션에 참가 한 더 큰 게임 세션에 참가 해야 수 있습니다.


### <a name="when-large-game-sessions-are-playing-in-my-title-why-arent-all-session-members-seeing-the-game-invite-toast"></a>큰 게임 세션 내 제목에서 재생 하는 경우 이유 세션의 모든 구성원 보이지 않는 게임 초대 알림?

항상 사용자 참여를 통해 세션을 추가 하는 제목, 제목 true로 **MultiplayerSessionMember.InitializeRequested** 속성을 설정 합니다. MPSD 세션 멤버의 나머지 부분에 대 한 초기화 단계에서 게임을 이동 하기 전에 대기할 인지를 나타냅니다. 그렇지 않으면 사용자가 가입 하는 매우 짧은 창 세션 변경 알림 놓칠 수 있습니다.


### <a name="i-am-seeing-inconsistent-game-behavior-and-have-received-protocol-activation-information-referencing-game-party-features"></a>게임 되지 않은 동작이 유발 표시 하 고 게임 타사 기능을 참조 하는 프로토콜 정품 인증 정보를 수신 합니다.

이 2014 멀티 플레이어 및 멀티 플레이 2015 기능을 혼합 된 것을 나타냅니다. 2015 멀티 플레이어에 대 한 API 2014 멀티 플레이어 용으로 작성 된 코드를 사용 하 여 사용할 수는 없습니다.


### <a name="why-am-i-seeing-the-syntax-for-v105-session-documents-in-my-traces-although-i-have-configured-a-v107-session-template"></a>표시 되는 이유 v105 세션 문서에 대 한 구문 내 추적에 v107 세션 템플릿 들이 구성한 있지만?

MPSD는 클라이언트 요청에 따라 세션 문서 버전 간에 자동 변환을 수행 합니다. 현재 모든 Xbox 서비스 Api 요청은 MPSD에 대 한 v105를 사용합니다. V107 세션 템플릿 간에 서로 다른 구문에 따라서 및 v105 추적 하지만 다른 기능 영향을 미치지 않습니다. 세션 템플릿 v107으로 구성 되어야 합니다.

Ⓒ 2015 Microsoft Corporation입니다. All rights reserved.
피드백에 제출 <https://forums.xboxlive.com/spaces/22/index.html>.
버전: 2.0.90731.0
