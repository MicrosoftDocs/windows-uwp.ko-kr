---
title: Xbox Live를 호출 하기 위한 모범 사례
author: KevinAsgari
description: Xbox Live Api를 호출 하기 위한 모범 사례에 알아봅니다.
ms.assetid: f4c7156b-7736-41e5-9b3d-e87cc8dd2531
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 모범 사례, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 1a28595b74fc316950d6d5b93b234ffa7a26e550
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7146382"
---
# <a name="best-practices-for-calling-xbox-live"></a>Xbox Live를 호출 하기 위한 모범 사례

두 가지 기본 방법으로에서 Xbox Live 서비스를 호출할 수 있습니다: 또는 Xbox 서비스 API (XSAPI)를 사용 하 여 REST 끝점을 직접 호출 합니다. 이 방법을 Xbox Live 호출에 관계 없이 호출 패턴에 적절 한 및 다시 시도 논리에 중요 합니다.

적절 한 다시 시도 논리를 작성 하는 방법을 알아보려면 두 가지 유형의 REST 끝점- **idempotent** 및 **비 idempotent**에 대해 알아야 할 필요 합니다. 살펴봅니다 이러한 각 아래
 
## <a name="non-idempotent-endpoints"></a>비 idempotent 끝점

부작용 시 HTTP 메서드 호출은 **비 idempotent**간주 반복 합니다. 즉, 클라이언트 끝점을 호출 하는 네트워크 시간 초과가 발생 하는 경우 아님을 안전 리소스 업데이트 하지만 네트워크에 성공 했는지 호출자에 게 알리려면 못했습니다. 때문에 메서드를 다시 시도 합니다. 다시 시도 하는 대신 오류 발생 시 클라이언트가 먼저 쿼리해야 호출 성공 했는지 확인 합니다. 호출에 성공 하는 경우에 다음 그 다시 시도해 야 합니다.

Xbox 서비스 API에서 일부 Api 호출 비 idempotent 끝점으로 내부적으로 표시 됩니다. 이 이러한 끝점을 호출할 때 오류가 발생 하는 경우 Api 시도 하지 않습니다 자동으로 끝점을 의미 합니다.

비 idempotent Api의 전체 목록은 다음과 같습니다.

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

**Idempotent** HTTP 메서드 부작용 안됩니다 반면 합니다. 그러면 다시 10-eb 의미 합니다. Xbox 서비스 API 모든 idempotent 메서드는 특정 조건에서 자동으로 다시 시도 됩니다.

Idempotent Api의 전체 목록은 위에 idempotent 아닌 것으로 나열 되지 않은 모든 Api가 있습니다.


## <a name="retry-logic-best-practices"></a>다시 시도 논리 모범 사례

Idempotent 호출에 대 한 이러한 조건 자동으로 다시 시도해 야 합니다.

* 모든 네트워크 오류
* 401: 무단
* 408: RequestTimeout
* 429: 요청이 너무 많이
* 500: InternalError
* 502: BadGateway
* 503: ServiceUnavailable
* 504: GatewayTimeout


UWP에서, 401: 특별 한 처리는 무단 합니다. Xbox Live 인증 토큰이 만료 Xbox 서비스 API 토큰을 새로 고치는 OS로 호출 되어 단일 재시도으로 다음을 수행 하므로 나타냅니다.

다시 시도 수행할 때 것이 좋습니다 "다시 시도 후" 헤더 시간에 도달할 때까지 서비스를 호출 하지 못하도록 합니다. 이제 XSAPI이 모범 사례를 구현합니다. 오류 HTTP 상태 코드 및 "다시 시도 후" 헤더는 모든 API에 대 한 반환 된 경우 경우 추가 시간 후 다시 시도 하기 전에 해당 동일한 API 호출 바로 반환 됩니다 원래 오류와 함께 서비스에 부딪힐 위험 없이.

호출 시도, 것이 좋습니다 지 백오프 서비스 부하를 분산 임의 떨림을 사용 하 여 수행할 수 있습니다. XSAPI 2 초 xbox\_live\_context\_settings::set\_http\_retry\_delay()를 사용 하 여 제어 되는 기본 지연으로 시작 합니다. 이것은 다시 시도는 지 백오프 2, 4, 8, 등 초 있어 jitters 현재 및 다음 백오프 값 집합 다시 시도 시도 하는 장치 간에 추가적인 확산 부하에 대 한 응답 시간에 따라 간 지연을 각 기본적으로 의미 합니다.

제목은 해야 하는 방법에 대 한 제어 호출 시도 오래 소요 됩니다. 개발자 함수 xbox\_live\_context\_settings::set\_http\_timeout\_window()를 사용 하 여 직접 제어는 XSAPI를 사용 합니다. 기본적으로이 20 초로 설정 됩니다. 다시 시도 논리 해제이 값을 0 초 설정 효과적으로 됩니다. XSAPI 이제 동적으로 조정는 http\_timeout\_window()에 얼마나 많은 시간 왼쪽된 남아에 따라 내부 HTTP 시간이 초과 되었습니다. 내부 HTTP 시간 제한 제어 점으로부터 OS 많아질 중단 되기 전에 HTTP 네트워크 작업을 수행 합니다. 호출 남게 됩니다 5 초 이상에 http\_timeout\_window() 남아 호출이 완료에 대 한 적절 한 충분 한 시간을 제공 하지 않는 한 다시 시도 하지 않습니다. 이 규칙은 양호 하 고 한 번만 호출 하면는 http\_timeout\_window()을 0으로 설정 되므로 첫 번째 호출에 적용 되지 않습니다. 이 논리에 효과 http\_timeout\_window() 훨씬 더 명확한 경우 API 호출에서 반환 됩니다. "다시 시도 후" 헤더 반환 된 경우 "다시 시도 후" 시간에 도달 하면 없는 reties까지 적용 됩니다. "다시 시도 후" 시간이 http\_timeout\_window() 후 이면 호출의 http\_timeout\_window()의 끝에 반환 합니다.


## <a name="error-handling"></a>오류 처리

이러한 실패 한 응답을 제대로 처리 되어 있는지 확인 해야 제목 개발자는 **항상** 사용 하 여 적절 한 오류 처리 서비스 호출 시 **마다** 합니다.
 
Xbox live와 같은 오류 코드를 반환 하는 요청에서 발생할 수 있는 많은 실제 조건 가지

1.  네트워크를 사용할 수 있습니다. 예를 들어 장치 분실 Wi Fi 손실 4g 또는 네트워크 다운 합니다.
2.  로드 (503)을 통해 서비스에 너무 많은 부하
3.  (500) 서비스에 오류가 발생
4.  요청이 너무 많이 (429) 서비스에 전달 하는 경우
5.  쓰기 작업 충돌 (412). 예를 들어 멀티 플레이 세션에서 다른 플레이어 제출 변경 먼저
6.  사용자가 차단 된 또는 권한이 없는
7.  사용자가 서명 아웃

적절 한 오류 처리기가 게임 이러한 조건에서 올바르게 작동 하는지 확인 하는 데 매우 중요 합니다.

XSAPI에 오류 처리 패턴의 두 가지 유형이 있습니다. 패턴 중 하나에서 C + WinRT Api를 사용 하는 경우 + CX, C\ # 또는 Javascript 및 다른 패턴은 새로운 c + + Api를 사용 하는 경우. 전체 세부 정보 오류 처리의 모범 사례에 "오류 처리" Xbox Live doc 페이지를 참조 하 고이 설명 하는 비디오 [*Xfest 2015 동영상*](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2015.aspx) 호출의 주제를 참조 하십시오 *XSAPI: c + +, 예외 없음!*


## <a name="best-calling-patterns"></a>최상의 호출 패턴

### <a name="usebatching-requests"></a>Usebatching 요청

일부 끝점 일괄 처리 또는 일련의 요청을 한 번만 호출에 집계를 지원 합니다. 예를 들어 프로필 Xbox Live 서비스를 사용 하 여 단일 사용자의 프로필 또는 사용자 프로필 집합에 대해 요청할 수 있습니다. 따라서 사용자 집합에 대 한 사용자 프로필을 해야 하는 경우 하기란 매우 효율적 각 사용자 프로필에 대 한 번에 끝점이 나 하나 API를 호출 합니다. 각 호출 많은 인증 오버 헤드를 추가합니다. 따라서 그 대신 끝점에서 동시에 모든 사용자 프로필을 처리 하 고 단일 응답을 반환할 수 있도록 API에 한 번에 대 한 정보를 원하는 모든 사용자를 전달 합니다.

### <a name="use-the-real-time-activity-rta-service-instead-of-polling"></a>폴링 대신 실시간으로 활동 (RTA) 서비스를 사용 합니다.

또 다른 최선의 실시간으로 활동 (RTA) 서비스 대신에 정기 폴링이 사용 하 여. 이 서비스는 서비스에서 대상 리소스 변경 될 때 클라이언트에 알림을 보내는 websocket을 노출 합니다. RTA 서비스에서 상태 변경, 통계 변경, 멀티 플레이 세션 문서 변경 및 소셜 관계 변경 알림을 제공합니다. 정보 클라이언트 란 관심이 알아야 클라이언트 항목에는 websocket을 통해 구독 먼저 해야 합니다. 이렇게 하면 항목이 변경 될 때 정확 하 게 메시지가 나타납니다 이후 변경 내용을 검색 하는 서비스를 폴링합니다.

RTA 서비스 집합으로 가입 클라이언트에서 사용할 수 있는 Api XSAPI 노출 합니다. 이러한 Api의 각각에 해당 \*\_changed\_handler 항목 변경 될 때 호출 되는 콜백 함수를 사용 하는 Api입니다.

* presence\_service::subscribe\_to\_device\_presence\_change
<br>
* presence\_service::subscribe\_to\_title\_presence\_change
<br>
* user\_statistics\_service::subscribe\_to\_statistic\_change
<br>
* social\_service::subscribe\_to\_social\_relationship\_change<br>
 

## <a name="use-xbox-live-client-side-managers"></a>Xbox Live 클라이언트 쪽 관리자를 사용 합니다.

새 XSAPI에 드디어 떼지 특정 시나리오 모든 어려운 작업을 수행 하는 캐시 및 상태 시스템으로 작용 관리자의 집합입니다.


### <a name="social-manager"></a>소셜 관리자

소셜 관리자 친구 목록과 프로필 주위 모든 번거로운 작업을 수행합니다. 이 친구 목록, 파트너가 프로필 및 해당 상태 데이터를 최신 상태로 유지 RTA 서비스를 사용 하 여 합니다. 관리자는 매우 게임 엔진을 식별 하는 동기 API를 노출 합니다. 게임에서 해당 Api를 호출할 수 자주 서비스에서 최신 정보는 메모리에 캐시를 유지 합니다. 자세한 내용은 "소셜 관리자에 소개"의 Xbox Live 설명서 페이지를 참조 하세요.

### <a name="multiplayer-manager"></a>멀티 플레이어 관리자

멀티 플레이 세션 관리를 위한 멀티 플레이어 관리자는 기존의 멀티 플레이어 게임에 대 한 방법 솔루션입니다. 멀티 플레이어 관리자 API 플레이어 명단 및 세션 관리를 포함 하 고 조인 매치 메이 킹, 진행 중인 게임 초대를 처리 기존 네트워킹 솔루션에 연결 합니다. 기존의 멀티 플레이 흐름을 구현 하는 주변 모든 번거로운 작업을 수행 합니다. 자세한 내용은 "멀티 플레이어 관리자를 소개"의 Xbox Live 설명서 페이지를 참조 하세요.


## <a name="throttling-fine-grained-rate-limiting"></a>조절 (세분화 된 속도 제한)

Xbox Live 서비스 단일 장치 서비스에서 과도 한 부하를 배치 하는 것을 방지에 제한 했습니다. 것 타이틀 제한 된 경우 알아야 합니다. 타이틀 몇 가지 방법으로 제한 된 경우를 알 수 있습니다.


### <a name="monitor-for-http-status-code-429"></a>HTTP 상태 코드 429에 대 한 모니터링

Fiddler를 사용 하 고 HTTP 상태 코드 429 없으면 시청할 수 있습니다. JSON 응답 된 끝점을 제한 하는 방법에 대 한 세부 정보를 포함 됩니다. 예를 들면 다음과 같습니다.

```json
{
  "version":1,
  "currentRequests":13,
  "maxRequests":10,
  "periodInSeconds":120,
  "limitType":"Rate"
}
```

XSAPI를 사용 하는 경우 Api는 http\_status\_429\_too\_many\_requests 오류를 반환 하 고 오류 메시지 API를 제한 하는 방법에 대 한 세부 정보를 설정 합니다.

### <a name="debug-asserts"></a>디버그 어설션

개발자 샌드박스에 있는 동안 호출 제한 되어 하 고 개발자는 알 수 있도록 즉시 어설션 됩니다 제목의 디버그 빌드를 사용 하 여 XSAPI를 사용 하 여 스로틀 발생 합니다. 이 잘못 작성된 된 코드 인해 의도치 않게 누락 429 스로틀 오류가 발생 하지 않도록 하는 것입니다. 사용 하지 않도록 설정 하려는 경우 이러한 문제를 일으키는 코드를 수정 하지 않고 작업을 계속 하려면 어설션,이 API를 호출할 수 있습니다.


> xboxLiveContext-&gt;settings()-&gt;disable\_asserts\_for\_xbox\_live\_throttling\_in\_dev\_sandboxes (xbox\_live\_context\_throttle\_setting::this\_code\_needs\_to\_be\_changed\_to\_avoid\_throttling);

그러나이 API에서 제한 되 고 타이틀 차단 되지 것입니다. 타이틀을 제한 계속 됩니다. 이 정책은 단순히는 디버그 빌드를 사용 하는 동안 개발자 샌드박스에 있을 때 어설션 합니다. 

### <a name="xbox-live-trace-analyzer-tool"></a>Xbox Live 추적 분석기 도구

다른 옵션은 Xbox Live 호출의 추적을 기록 하 고 사용 하 여 해당 추적을 분석 하 고 [Xbox Live 추적 분석기 도구.](https://docs.microsoft.com/windows/uwp/xbox-live/tools/analyze-service-calls)

추적을 기록 하려면 사용 Fiddler 녹음할 수는 있습니다. SAZ 파일 또는 XSAPI의 기본 제공 추적 로그를 사용 하 여 합니다. 자세한 내용은 XSAPI 추적 켜기 사용 하 여 Xbox Live 설명서 페이지 "Xbox Live 서비스에 대 한 분석 호출"를 참조 하는 방법. 추적을 구성한 후에 Xbox Live 추적 분석기 도구에서 검색 시 경고 메시지가 호출을 제한 합니다.

## <a name="is-xbox-live-up"></a>Xbox Live는?

Xbox Live는 매치 메이 킹 및 프로필, 친구 및 상태, 통계, 순위표, 도전 과제, 멀티 플레이어와 같은 Xbox Live 기능을 노출 하는 마이크로 서비스의 컬렉션입니다. 단일 서버 또는 Xbox Live 켜져 있는 경우를 정의 하는 끝점의 경우 없습니다. 단일 서버 다운 되 면 Xbox Live 마이크로 서비스의 나머지는 거의 독립적, 작동 있어야 합니다.

임시 중단이 발생 하는 단일 서비스, 있으면이 서비스 호출 게임에 대 한 중요 업무용 인지 알아야 합니다. 일시적인 네트워크 또는 서비스 문제 상태일 적절 한 환경을 제공 하려고 합니다. 예를 들어 상태 서비스는 반환 하는 경우 게임에 대 한 중요 업무용 가능성이 호출 실패 하지 않습니다. Xbox Live 종료 되었음을 보고 하는 대신 마지막으로 알려진된 상태 사용자에 게 보고 하면 됩니다.

Xbox Live에 최종 일관성의 일관성 모델은 다음과 같습니다. 이 새로운 업데이트 내용이 결국 해당 리소스에 대 한 모든 요청은 보고 마지막으로 업데이트 된 값을 의미 합니다. 이 사실 정보 데이터로 부실 작은 기간 전파를 의미 합니다.
