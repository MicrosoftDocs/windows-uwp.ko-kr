---
title: Xbox Live를 호출 하기 위한 모범 사례
description: Xbox Live Api 호출에 대 한 모범 사례에 알아봅니다.
ms.assetid: f4c7156b-7736-41e5-9b3d-e87cc8dd2531
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 모범 사례, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 55e05ef7de2e2981f9f5af86623a8d8413ce2c99
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613708"
---
# <a name="best-practices-for-calling-xbox-live"></a>Xbox Live를 호출 하기 위한 모범 사례

Xbox Live 서비스는 두 가지 기본적인 방법에서 호출할 수 있습니다: Xbox 서비스 API (XSAPI)를 사용 하 여 또는 REST 끝점을 직접 호출 합니다. 호출 하는 방법에 코드 Xbox Live에 관계 없이 호출 패턴을 적절 한 재시도 논리를 반드시 합니다.

두 가지 유형의 REST 끝점에 알아야 할 필요는 적절 한 재시도 논리를 작성 하는 방법을 알아보려면 **idempotent** 하 고 **비 멱 등**합니다. 각각에 대해 이러한 아래
 
## <a name="non-idempotent-endpoints"></a>비 멱 등 끝점

시 의도 하지 않은 HTTP 메서드 호출으로 간주 됩니다 반복 **비 멱 등**합니다. 이 클라이언트 끝점을 호출 하 고 네트워크 시간 초과가 발생, 없는 경우 리소스 업데이트 되었지만 네트워크 않았습니다 성공 호출자에 게 알릴 수 있으므로 메서드를 다시 시도 하려면 안전 것을 의미 합니다. 다시 시도 하는 대신 오류 시 클라이언트 먼저 쿼리 호출이 성공 했는지 여부를 확인 합니다. 호출이 성공 했습니다. 경우에 다음 것을 다시 시도해 야 합니다.

Xbox 서비스 api에서 일부 Api는 비 멱 등 끝점 호출 내부적으로 표시 됩니다. 이 이러한 끝점을 호출 하는 경우 오류가 발생 한 경우 Api는 자동으로 재시도 하지 끝점을 의미 합니다.

비 멱 등 Api의 전체 목록은 다음과 같습니다.

* game\_server\_platform\_service::allocate\_cluster()
<br>
* game\_server\_platform\_service::allocate\_cluster\_inline()
<br>
* game\_server\_platform\_service::allocate\_session\_host()
<br>
* matchmaking\_service::create\_match\_ticket()
<br>
* multiplayer\_service::write\_session()
<br>
* multiplayer\_service::write\_session\_by\_handle()
<br>
* multiplayer\_service::send\_invites()
<br>
* reputation\_service::submit\_batch\_reputation\_feedback()
<br>
* reputation\_service::submit\_reputation\_feedback()
 

## <a name="idempotent-methods"></a>Idempotent 메서드

**Idempotent** HTTP 메서드 반면에 두지 마십시오 부작용입니다. 이 다시 재시도 안전한 지 의미 합니다. Xbox Services API를 모든 idempotent 메서드는 특정 조건에서 자동으로 다시 시도 됩니다.

Idempotent Api의 전체 목록에는 모든 Api 비 멱 등으로 위에 나열 되지 않은 것입니다.


## <a name="retry-logic-best-practices"></a>재시도 논리 모범 사례

Idempotent 호출에 대 한 이러한 조건은 자동으로 다시 시도해 야 합니다.

* 모든 네트워크 오류
* 401: 권한 없음
* 408: RequestTimeout
* 429: 너무 많은 요청
* 500: InternalError
* 502: BadGateway
* 503: ServiceUnavailable
* 504: GatewayTimeout


On UWP, 401: 권한이 없는 특수 한 처리입니다. Xbox 서비스 API 토큰을 새로 고치려면 OS로 호출 하 고 단일 다시 시도를 수행 하므로 Xbox Live 인증 토큰 만료를 나타냅니다.

재시도 수행 하는 경우 것이 좋습니다 "Retry-after" 헤더 시간에 도달 될 때까지 서비스를 호출 하지 않도록 합니다. 이제 XSAPI이 모범 사례를 구현합니다. 모든 API에 대 한 HTTP 상태 코드 오류 및 "Retry-after" 헤더를 반환 하는 경우 추가 호출 Retry-after 시간 전에 똑같은 API 즉시 반환 됩니다 원래 오류를 사용 하 여 서비스에 도달 하지 않고 있습니다.

호출을 다시 시도 때 가장 좋은 방법은 지 수 백오프는 임의 지터 서비스에 부하 분산을 사용 하 여 수행 합니다. XSAPI xbox를 사용 하 여 제어 되는 기본 지연 2 시간 (초) 시작\_live\_상황에 맞는\_settings::set\_http\_을 다시 시도\_delay() 합니다. 즉, 각 기본적으로 다시 시도 백오프 2, 4, 8 않습니다 등 초 하며 jitters 현재 및 다음 백오프 값 재시도 시도 하는 장치 집합에서 부하 아웃 추가적인 확산을 응답 시간에 따라 사이의 지연입니다.

제목 방법 제어에서 해야 합니다. 장기 호출을 다시 시도 소비 합니다. 를 사용 하면 XSAPI 개발자는이 직접 제어 함수 xbox를 사용 하 여\_live\_상황에 맞는\_settings::set\_http\_제한\_window() 합니다. 기본적으로이 20 초로 설정 됩니다. 재시도 논리 해제을 0 초로 설정 하는이 효과적으로 됩니다. XSAPI 이제 동적으로 조정 내부 HTTP 시간 제한 기반으로 http에 얼마나 많은 시간 왼쪽된 남아\_timeout\_window() 합니다. 내부 HTTP 시간 제한 기간 OS 컨트롤을 중단 하기 전에 HTTP 네트워크 작업을 수행 하를 소비 합니다. Http에서 왼쪽 5 초 이상 남게 됩니다 하지 않으면 호출 재시도 되지 것입니다\_시간 제한\_window() 완료에 대 한 호출에 대 한 충분 한 적절 한 시간을 제공 합니다. 이 규칙은 첫 번째 호출 하므로 http 설정에 적용 되지 않습니다\_timeout\_0 window() 양호 하 고 한 번만 호출 됩니다. 이 논리는 http는 영향을 주지\_timeout\_window()는 API 호출은 반환 하는 경우 훨씬 더 명확 합니다. "Retry-after" 헤더를 반환 하는 경우 없습니다 reties "Retry-after" 시간에 도달한 후까지 수행 됩니다. "Retry-after" 시간 http 이후 이면\_제한 시간\_http 끝 window(), 다음 호출 반환\_제한 시간\_window() 합니다.


## <a name="error-handling"></a>오류 처리

제목 개발자 해야 **항상** 적절 한 오류에 대 한 처리를 사용 하 여 **모든** 서비스 호출 하는 실패 한 응답을 제대로 처리 하는 확인 해야 합니다.
 
Xbox live와 같은 오류 코드를 반환 하려면 요청에서 발생할 수 있는 많은 실제 조건이

1.  네트워크를 사용할 수 없습니다. 예를 들어, 장치 분실 4g, Wi-fi, 손실 또는 네트워크 다운 합니다.
2.  부하 (503)를 통해 서비스에서 너무 많은 로드
3.  (500) 서비스에서 오류가 발생 했습니다.
4.  요청이 너무 많음 (429) 서비스로 전송 하는 경우
5.  작업이 충돌 합니다. (412)를 작성 합니다. 예를 들어, 멀티 플레이 세션에서 다른 플레이어 제출 된 변경 내용을 먼저
6.  차단 된 사용자나 권한을 갖지 않습니다.
7.  사용자가 서명 된 스케일 아웃

적절 한 오류 처리기는 이러한 조건에서 게임이 올바르게 작동 하는지 확인 하는 데 중요 합니다.

XSAPI에 두 가지 유형의 오류 패턴을 처리 합니다. C + WinRT Api를 사용 하는 경우 한 가지 패턴 + CX, C\#, Javascript 및 c + + Api를 사용 하는 경우 또 다른 패턴 또는 합니다. 전체 오류 처리 모범 사례에 "오류 처리" Xbox Live 문서 페이지를 참조 정보와이 포함 하는 비디오에서 말하는를 참조 하십시오 [ *Xfest 2015 비디오* ](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2015.aspx) 호출 *XSAPI: C + +에서 예외 없음!*


## <a name="best-calling-patterns"></a>최상의 호출 패턴

### <a name="usebatching-requests"></a>일괄 처리 요청을 사용 하 여

일부 끝점을 단일 호출으로 요청 집합의 집계 또는 일괄 처리를 지원 합니다. 예를 들어, Xbox Live 프로필 서비스를 사용 하 여 단일 사용자의 프로필 또는 사용자 프로필의 집합을 요청할 수 있습니다. 따라서 사용자 집합에 대 한 사용자 프로필을 할 경우 수 없기 각 사용자 프로필에 대해 한 번에 끝점이 나 하나는 API를 호출 하는 것이 효율적입니다. 각 호출에 많은 인증 오버 헤드를 추가합니다. 따라서 끝점에서 동시에 모든 사용자 프로필을 처리 하 고 단일 응답을 반환할 수 있도록 API에 한 번에 대 한 정보를 원하는 모든 사용자를 전달 합니다.

### <a name="use-the-real-time-activity-rta-service-instead-of-polling"></a>실시간 활동 (RTA) 서비스를 사용 하 여 폴링 대신

다른 모범 사례에는 실시간 활동 (RTA) 서비스 대신 정기 폴링을 데 사용 됩니다. 이 서비스는 서비스에서 대상 리소스 변경 될 때 클라이언트에 알림을 전송 하는 websocket을 노출 합니다. RTA이 서비스는 현재 상태 변경, 통계 변경, 멀티 플레이 세션 문서 변경 및 소셜 관계 변경 내용에 대 한 알림을 제공 합니다. 무엇이 있는지 아는 정보 클라이언트 관심이, 클라이언트 항목에는 websocket을 통한 구독 먼저 해야 합니다. 서비스를 폴링하는 항목이 변경 될 때 정확히 메시지가 나타납니다 하므로 변경 내용을 검색 하는 것이 이렇게 없습니다.

XSAPI 노출 집합으로 RTA 서비스 클라이언트가 사용할 수 있는 Api를 구독 합니다. 이러한 각 Api가 해당 \* \_변경\_처리기는 항목이 변경 될 때 호출 되는 콜백 함수에서 사용 하는 Api입니다.

* presence\_service::subscribe\_to\_device\_presence\_change
<br>
* presence\_service::subscribe\_to\_title\_presence\_change
<br>
* user\_statistics\_service::subscribe\_to\_statistic\_change
<br>
* social\_service::subscribe\_to\_social\_relationship\_change<br>
 

## <a name="use-xbox-live-client-side-managers"></a>클라이언트 쪽 관리자 Xbox Live를 사용 합니다.

새 XSAPI에서 이제 떼지 특정 시나리오 모든 많은 수행 하는 캐시 및 상태 시스템으로 작동 하는 관리자의 집합입니다.


### <a name="social-manager"></a>주민 등록 관리자

소셜 Manager 친구 목록 및 프로필에 대 한 작업을 수행합니다. 이 친구 목록, 해당 프로필 및 해당 현재 상태 데이터를 최신 상태로 유지 RTA를 사용 하 여 합니다. 관리자는 매우 게임 엔진을 식별 하는 동기 API를 노출 합니다. 게임 Api를 호출할 수는 서비스에서 최신 정보는 메모리 내 캐시를 유지 관리 하는 만큼 자주 합니다. 자세한 내용은 Xbox Live 설명서 페이지를 소셜 Manager 소개 "을 참조 하세요.

### <a name="multiplayer-manager"></a>멀티 플레이어 관리자

멀티 플레이 세션 관리에 대 한 다중 접속 관리자는 기존의 멀티 플레이 게임에 대 한 드롭인 솔루션입니다. 멀티 플레이 게임 관리자 API 진행 중, 결혼 정보 회사 연결을 조인 하는 게임 초대를 처리 하며 기존 네트워킹 솔루션에 연결 플레이어 명단 및 세션 관리를 포함 합니다. 기존의 멀티 플레이 흐름을 구현 하는 관련 한 작업을 수행 합니다. 자세한 내용은 "다중 접속 Manager 소개"는 Xbox Live 설명서 페이지를 참조 하세요.


## <a name="throttling-fine-grained-rate-limiting"></a>제한 (세밀 하 게 세분화 된 속도 제한)

Xbox Live 서비스는 서비스에 과도 한 부하를 배치에서 모든 단일 장치를 방지 하기 위한 제한에 있습니다. 것을 제목 제한 된 때를 알아야 합니다. 제목에 몇 가지에서 제한 된 경우를 알 수 있습니다.


### <a name="monitor-for-http-status-code-429"></a>HTTP 상태 코드 429 모니터링 합니다.

Fiddler를 사용할 수 있으며 보기는 HTTP 상태 코드 429 반환 됩니다. JSON 응답 끝점 제한 된 방법에 대 한 정보를 포함 됩니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

```json
{
  "version":1,
  "currentRequests":13,
  "maxRequests":10,
  "periodInSeconds":120,
  "limitType":"Rate"
}
```

XSAPI를 사용 하는 경우 Api는 http를 반환\_상태\_429\_너무\_많은\_오류를 요청 하 고 API를 제한 하는 방법에 대 한 정보를 오류 메시지를 설정 합니다.

### <a name="debug-asserts"></a>디버그 어설션

호출 제한 되는 동안 개발자 샌드박스 및 사실을 개발자 즉시 사용할 수 있도록 assert는 제목의 디버그 빌드를 사용 하는 경우 XSAPI를 사용 하는 경우 제한이 발생 했습니다. 잘못 작성된 된 코드로 인해 의도 하지 않게 누락 429 제한 오류를 방지 하기 위해서입니다. 사용 하지 않도록 설정 하려는 경우 이러한 문제를 일으키는 코드를 수정 하지 않고 작업을 계속 하려면 어설션,이 API를 호출할 수 있습니다.


> xboxLiveContext-&gt;settings ()-&gt;사용 안 함\_어설션\_에 대 한\_xbox\_라이브\_조정\_에서\_개발\_ 샌드박스 (xbox\_live\_상황에 맞는\_스로틀\_setting::this\_코드\_해야\_하\_수\_변경\_하\_방지\_제한).

그러나이 API에서 제한 되 고을 제목 해도 됩니다. 제목에 여전히 제한 됩니다. 이 간단히 사용 하지 않도록 설정 된 디버그 빌드를 사용 하는 동안 개발자 샌드박스에서 어설션 합니다. 

### <a name="xbox-live-trace-analyzer-tool"></a>Xbox Live 추적 분석기 도구

Xbox Live 호출 추적 레코드를 사용 하 여 해당 추적을 분석 하는 또 다른 방법은 [Xbox Live 추적 분석기 도구입니다.](https://docs.microsoft.com/windows/uwp/xbox-live/tools/analyze-service-calls)

추적을 기록 하려면 사용할 수 있습니다 하거나 Fiddler 기록 하는 합니다. SAZ 파일 또는 XSAPI의 기본 제공 추적 로깅을 사용 하 여 합니다. 자세한 내용은 Xbox Live 설명서 페이지 "분석에 대 한 호출 Xbox Live 서비스"를 참조 하십시오 XSAPI에서 추적 설정 사용 하는 방법입니다. 추적을 만든 후 Xbox Live 추적 분석기 도구 검색 시 경고가 표시 됩니다는 호출을 제한 합니다.

## <a name="is-xbox-live-up"></a>Xbox Live는?

Xbox Live는 프로필, 친구 및 현재 상태, 통계, 순위표, 성과, 멀티 플레이, Xbox Live 기능 및 매 치메이 킹을 노출 하는 마이크로 서비스의 컬렉션입니다. 단일 서버 또는 Xbox Live 중인지 여부를 정의 하는 끝점 없습니다. 단일 서버 다운 되 면 Xbox Live 마이크로 서비스의 rest 주로 독립적 이며 작동 되어야 합니다.

단일 서비스 중단이 임시 것이 중요 서비스 호출이 게임에 대 한 중요 업무용 인 경우 알고 있어야 합니다. 일시적인 네트워크 또는 서비스 문제가 합리적인 작업 환경을 제공 하려고 합니다. 예를 들어, 현재 상태를 반환 하는 경우 게임에 대 한 중요 업무용 가능성이 호출 하는 실패 하지 않습니다. 따라서 단순히 Xbox Live 종료 되었음을 보고 하는 대신 마지막으로 알려진된 있는지 사용자에 게 보고 합니다.

Xbox Live에 최종 일관성의 일관성 모델을 다음과 같습니다. 이 새로운 업데이트 내용이 최종적으로 해당 리소스에 대 한 모든 요청은 마지막 값을 보고 업데이트 하는 의미 합니다. 이 적용 정보 데이터로 유효 하지 않은 작은 기간 전파를 의미 합니다.
