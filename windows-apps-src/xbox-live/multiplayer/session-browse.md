---
title: 멀티 플레이 세션 찾아보기
description: Xbox Live 멀티 플레이어를 사용 하 여 멀티 플레이 세션 찾아보기를 구현 하는 방법에 알아봅니다.
ms.assetid: b4b3ed67-9e2c-4c14-9b27-083b8bccb3ce
ms.date: 10/16/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 579c71ef9266fb9a1ee4ef0538d1beffec0bb4ea
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660678"
---
# <a name="multiplayer-session-browse"></a>멀티 플레이 세션 찾아보기

제목을 지정 된 조건을 충족 하는 열린 멀티 플레이 게임 세션의 목록을 쿼리할 수 있도록 멀티 플레이 세션 찾아보기 2016 년 11 월에에서 도입 된 새로운 기능입니다.

## <a name="what-is-session-browse"></a>세션 찾아보기 란?

세션 찾아보기 시나리오에서는 게임에서 플레이어는 조인 가능한 게임 세션 목록을 검색할 수 있습니다. 이 목록에 각 세션 항목 플레이어는 참가할 세션을 선택 하는 데 사용할 수 있는 게임을 하는 방법에 대 한 몇 가지 추가 메타 데이터를 포함 합니다.  또한 메타 데이터에 따라 세션 목록을 필터링 할 수 있습니다. 플레이어가 게임에 적합 합니다. 세션을 인식 하는 경우 되 면 세션을 가입할 수 있습니다.

또한 플레이어가 새 게임 세션을 만드는 하 고 세션 찾아보기 결혼 정보 회사 연결에 의존 하지 않고 추가 플레이어를 모집 하는 데 수 있습니다.

세션 찾아보기 기존의 결혼 정보 회사 연결 시나리오에서 다른 플레이어 게임 결혼 정보 회사에서 참여 하려면 세션을 자체 선택에 일반적으로 플레이어가에서 플레이어를 자동으로 배치를 시도 하는 "찾을 게임" 단추를 적절 한 게임 세션입니다. 세션 찾아보기 객관적으로 최상의 게임을 선택 하지 않을 수 있습니다 하는 수동 및 속도가 느린 프로세스 이지만, 플레이어에 게 더 많은 제어를 제공 하 고 주관적으로 더 나은 게임 선택 감지 될 수 있습니다.

이 게임에서 모두 세션 찾아보기 및 결혼 정보 회사 연결 시나리오를 포함 하도록 일반적입니다. 일반적으로 결혼 정보 회사는 일반적으로 사용자 지정 게임에 대 한 세션 찾아보기 사용 하는 동안 게임 모드를 재생에 사용 됩니다.

**예:** John은 hero 전투 arena 스타일 멀티 플레이 게임을 재생에 관심이 있을 수 있지만 위치 모든 플레이어 해당 hero 임의로 선택 게임을 실행 하려고 합니다. 그 열려 있는 게임 세션의 목록을 검색합니다 하 고 해당 설명에 "임의 heroes"를 포함 하는 것을 찾을 수 있고 게임 UI를 허용 하는 경우 그 수도 "임의 hero" 게임 모드를 선택 합니다. "RandomHero" gam 지를 나타내는 태그가 지정 된 세션만 검색 es 합니다.

그 가망이 게임을 찾으면 게임을 참석 합니다. 사용자 세션에 참여 했으므로 게임 세션의 호스트는 게임을 시작할 수 있습니다.

### <a name="roles"></a>역할

세션에서 게임을 특정 역할에 대 한 플레이어 모집 하려고 합니다. 예를 들어, 플레이어를 만들 수도 게임 세션을 지정 하 고 세션 5 개 이하의 공격 클래스가 포함 되어 있지만 2 개 이상의 힐 역할 및 1 개 이상 탱크 역할을 포함 해야 합니다.

다른 플레이어에 세션에 적용 될 때 해당 역할을 미리 선택할 수 있습니다 및 서비스는 선택한 역할에 대 한 열기 슬롯이 없는 경우 세션에 참가 하도록 허용 하지 않습니다.

또 다른 예로 플레이어를 조인할 친구 들에 대 한 2 슬롯 예약 하려는 경우 게임에는 "friends" 역할을 지정할 수 있으며 세션 호스트를 사용 하 여 친구 들을 수 있는 플레이어만 "friends" 역할에 전용된 2 슬롯을 채울 수 것입니다.

역할에 대 한 자세한 내용은 참조 하세요. [멀티 플레이 역할](multiplayer-roles.md)입니다.



## <a name="how-does-session-browse-work"></a>세션 찾아보기는 어떻게 작동 하나요?

세션 찾아보기 검색 핸들의 사용에 주로 적용 됩니다. 검색 핸들이 세션, 즉 검색 특성에 대 한 추가 메타 데이터 뿐만 아니라 세션에 대 한 참조를 포함 하는 데이터 패킷을 합니다.

제목을 만들면 세션에 대 한 적합 한 새 게임 세션 찾아보기, 세션에 대 한 검색 핸들을 만듭니다. 검색 핸들에는 멀티 플레이 서비스 디렉터리 (MPSD), 제목에 검색 핸들을 유지 관리 하는 저장 됩니다.

세션의 목록을 검색 해야 하는 제목, 제목 검색 쿼리를 검색 조건을 충족 하는 검색 핸들의 목록을 반환 하는 MPSD를 보낼 수 있습니다. 제목 수 다음 목록을 사용 하 여 세션의 플레이어에 게 조인 가능한 게임의 목록을 표시 합니다.

세션은 full 또는 조인할 수 없는 경우 제목을 않도록 제거할 수 검색 핸들을 MPSD에서 세션 세션 찾아보기 쿼리에 표시 되지 것입니다.

>[!NOTE]
> 검색을 처리 하는 데 세션의 목록을 표시 하는 경우 사용자에 게 표시할 수는 합니다. 백그라운드 매 치메이 킹 유효 하지 않습니다. 검색 핸들을 사용 하 고 대신 사용 하는 것이 좋습니다 [SmartMatch](multiplayer-manager/play-multiplayer-with-matchmaking.md)

## <a name="set-up-a-session-for-session-browse"></a>세션 찾아보기에 대 한 세션 설정

세션에 대 한 검색 핸들을 사용 하려면 세션에 다음 있어야 true로 기능 설정:

* `searchable`
* `userAuthorizationStyle`

>[!NOTE]
> `userAuthorizationStyle` 기능은 에서만 UWP 게임에 필요 하지만 좋습니다 모든 Xbox Live에 대 한 구현 향후 이식성을 보장 하므로 XDK 게임을 포함 하 여 게임을 합니다.

>[!NOTE]
> 설정 된 `userAuthorizationStyle` 기능 기본값 합니다 `readRestriction` 및 `joinRestriction` 세션의 `local` 대신 `none`합니다. 즉, 제목 검색 핸들을 사용 하거나 게임 세션에 참가 대 한 핸들을 전송 해야 합니다.

Xbox Live 서비스를 구성한 경우 세션 템플릿에 이러한 기능을 설정할 수 있습니다.

세션 찾아보기에 대 한 검색을 처리 하지 로비 세션에 대 한 실제 게임 플레이에 사용할 수 있는 세션에만 만들 해야 있습니다.

## <a name="what-does-it-mean-to-be-an-owner-of-a-session"></a>어떤 의미 세션의 소유자 여야 하 시겠습니까?

대부분의 게임 세션 SmartMatch 친구 들과 게임 등의 형식 소유자를 필요 하지 않습니다, 세션 찾아보기 세션에 대 한 소유자 할 수도 있습니다. 

소유자 관리 세션을 있으면 소유자에 대 한 몇 가지 이점이 있습니다. 소유자는 세션에서 다른 멤버를 제거 하거나 다른 멤버의 소유권 상태를 변경할 수 있습니다.

세션 소유자를 사용 하려면 세션에 다음 있어야 true로 기능 설정:

* `hasOwners`

세션의 소유자는 Xbox Live 멤버 차단 있으면 해당 멤버는 세션을 조인할 수 없습니다.

사용 하는 경우 [멀티 플레이 역할](multiplayer-roles.md), 소유자에 게 역할을 할당할 수만 있도록 설정할 수 있습니다.

모든 소유자 세션을 그대로 경우 기반 세션에서 작업을 수행 하는 서비스는 `ownershipPolicy.migration` 세션에 대해 정의 된 정책입니다. 정책 "가장 오래 된" 이면 된 세션에서 가장 긴 플레이어가 새 소유자가 되도록 설정 됩니다. 정책 "endsession" (제공 되지 않은 경우 기본값) 이면 서비스 세션을 종료 및 세션에서 모든 나머지 플레이어를 제거 합니다.


## <a name="search-handles"></a>검색을 처리 하지만

검색 핸들을 JSON 구조로 MSPD에 저장 됩니다. 세션에 대 한 참조를 포함 하는 것 외에도 검색 핸들도 검색의 검색 특성으로 알려진 경우 추가 메타 데이터를 포함 합니다.

세션을 언제 든 지에 생성 한 검색 핸들을 하나만 사용할 수 있습니다.

Xbox Live Api를 사용 하 여의 세션에 대 한 검색 핸들을을 만들려면, 먼저 만듭니다는 `multiplayer::multiplayer_search_handle_request` 개체를 전달한 다음 해당 개체는 `multiplayer::multiplayer_service::set_search_handle()` 메서드.

### <a name="search-attributes"></a>검색 특성

검색 특성 다음 구성 요소로 구성 됩니다.

`tags` -태그는 사용자는 해시 태그를 비슷합니다 게임 세션을 분류 하는 데 사용할 수 있는 문자열 설명자입니다. 태그는 문자로 시작 해야 하며, 공백을 포함할 수 없습니다 및 100 자 미만 이어야 합니다.
예제에서는 태그: "ProRankOnly", "norocketlaunchers", "cityMaps".

`strings` 문자열은 텍스트는 변수 및 문자열 이름 문자로 시작 해야, 공백을 포함할 수 없습니다. 100 자 미만 이어야 합니다.

예제에서는 문자열 메타 데이터: "무기"="칼 + 피스 + 라이플", "맵 이름" = "UrbanCityAssault", "재미 있는 보편적인 게임 설명"=", 새 사용자 환영 합니다."

`numbers` -숫자는 숫자 변수 및 숫자 이름을 문자로 시작 해야, 공백을 포함할 수 없습니다. 100 자 미만 이어야 합니다. Xbox Live Api float 형식으로 숫자 값을 검색합니다.

메타 데이터 예제 수: "MinLevel" = 25, "MaxRank" = 10.

>**참고:** 서비스에서 태그 및 문자열 값의 대/소문자를 구분 문자는 유지 되지만 태그를 쿼리할 때 tolower() 함수를 사용 해야 합니다. 이 태그 문자열 값은 현재 소문자 취급 모든 대문자를 포함 하는 경우에 의미 합니다.

Xbox Live Api에서 사용 하 여 검색 속성을 설정할 수 있습니다 합니다 `set_tags()`, `set_stringsmetadata()`, 및 `set_numbers_metadata()` 의 메서드는 `multiplayer_search_handle_request` 개체입니다.


### <a name="additional-details"></a>추가 정보

결과 추가 유용한 데이터를 포함 하는 검색 핸들을 검색 하는 경우 세션에 대 한 이러한 세션이 닫혀 있는 경우에 따라 제한 사항이 있나요 조인 세션, 등입니다.

검색 특성을 함께 이러한 세부 정보에 포함 된 Xbox Live api에서는 `multiplayer_search_handle_details` 검색 쿼리 후 반환 되는 합니다.

### <a name="remove-a-search-handle"></a>검색 핸들을 제거 합니다.

세션 전체 경우와 같이 세션 찾아보기에서 세션을 제거 하려고 할 때 또는 세션이 닫힐 경우 검색 핸들을 삭제할 수 있습니다.

Xbox Live Api에서 사용할 수 있습니다는 `multiplayer_service::clear_search_handle()` 검색 핸들을 제거 하는 방법입니다.

### <a name="example-create-a-search-handle-with-metadata"></a>예제: 메타 데이터를 사용 하 여 검색 핸들을 만들기

다음 코드에는 c + + Xbox Live 멀티 플레이어 Api를 사용 하 여 세션에 대 한 검색 핸들을 만드는 방법을 보여 줍니다.

```cpp
auto searchHandleReq = multiplayer_search_handle_request(sessionBrowseRef);

searchHandleReq.set_tags(std::vector<string_t> val);
searchHandleReq.set_numbers_metadata(std::unordered_map<string_t, double> metadata);
searchHandleReq.set_strings_metadata(std::unordered_map<string_t, string_t> metadata);

auto result = xboxLiveContext->multiplayer_service().set_search_handle(searchHandleReq)
.then([](xbox_live_result<void> result)
{
  if (result.err())
  {
    // handle error
  }
});
```


## <a name="create-a-search-query-for-sessions"></a>세션에 대 한 검색 쿼리 만들기

검색 처리의 목록을 검색 하는 경우 특정 조건을 충족 하는 세션에 결과 제한 하려면 검색 쿼리를 사용할 수 있습니다.

검색 쿼리 구문은 [OData](https://docs.oasis-open.org/odata/odata/v4.0/errata02/os/complete/part2-url-conventions/odata-v4.0-errata02-os-part2-url-conventions-complete.html#_Toc406398092) 구문에서 지원 되는 다음 연산자를 사용 하 여 스타일:

 Operator | 설명
 --- | ---
 eq | 같음
 ne | 같지 않음
 gt | 보다 큼
 ge | 크거나 같음
 lt | 미만
 le | 작거나 같음
 및 | 논리적 AND
 또는 | 논리 (아래 참고 참조) 또는

람다 식을 사용할 수도 있습니다 및 `tolower` 정식 함수입니다. 다른 OData 함수가 없는 현재 지원 됩니다.

태그 또는 문자열 값을 검색할 때에 소문자 문자열에 대 한 검색 서비스는 현재 지원 검색 쿼리에서 'tolower' 함수를 사용 해야 합니다.

Xbox Live 서비스는만 검색 쿼리와 일치 하는 처음 100 개의 결과 반환 합니다. 게임 결과 너무 광범위 한 경우 해당 검색 쿼리를 구체화 하는 플레이어를 허용 해야 합니다.

>[!NOTE]
>  논리 Or 필터 문자열; 쿼리에서 지원 됩니다. 그러나 하나의 OR 수 있으며 쿼리 루트 여야 합니다. 쿼리에서 여러 ORs 없습니다도 만들 수는 쿼리는 초래 되지 맨 위에 있는 대부분의 쿼리 구조 수준 또는 합니다.

### <a name="search-handle-query-examples"></a>검색 핸들 쿼리 예제

Restful 호출을 "필터"의 모든 검색 핸들에 대 한 쿼리에 실행 되는 OData 필터 언어 문자열을 지정 합니다.  
멀티 플레이 2015 api에 검색 필터 문자열을 지정할 수 있습니다 합니다 *searchFilter* 의 매개 변수는 `multiplayer_service.get_search_handles()` 메서드.  

현재, 다음 필터 시나리오 지원 됩니다.

 기준으로 필터링 | 검색 필터 문자열
 --- | ---
 단일 멤버 xuid '1234566' | "session/memberXuids/any(d:d eq '1234566')"
 단일 소유자 xuid '1234566' | "session/ownerXuids/any(d:d eq '1234566')"
 문자열 'classb' 같음 ' forzacarclass' | "tolower(strings/forzacarclass) eq 'classb'"
 숫자 'forzaskill' 6 | "숫자/forzaskill eq 6"
 'Halokdratio' 1.5 보다 큰 숫자 | "숫자/halokdratio gt 1.5"
 태그 'coolpeopleonly' | "tags/any(d:tolower(d) eq 'coolpeopleonly')"
 'Cursingallowed' 태그를 포함 하지 않는 세션 | "tags/any(d:tolower(d) ne 'cursingallowed')"
 'Rank' 하는 숫자를 포함 하지 않는 세션 0은 | "숫자/순위 0 ne"
 세션 'forzacarclass' 문자열을 포함 하지 않는 'classa'와 같은 | "tolower(strings/forzacarclass) ne 'classa'"
 태그 'coolpeopleonly' 및 'halokdratio' 7.5 같음 번호 | "tags/any(d:tolower(d) eq 'coolpeopleonly') eq true and numbers/halokdratio eq 7.5"
 숫자 'halodkratio' 보다 크거나 같은 숫자로 1.5 ' 순위 ' 60 미만이 고는 숫자 'customnumbervalue' 작거나 같거나 5 | "숫자/halokdratio ge 1.5 및 숫자/순위 lt 60 및 숫자/customnumbervalue le 5"
 성과 달성 하셨습니다 id가 '123456' | "achievementIds/any(d:d eq '123456')"
 'En' 언어 코드 | "language eq 'en'"
 예약 된 시간에 모든 예약 된 반환 시간이 지정된 된 시간 보다 작거나 같음 | "session/scheduledTime le '2009-06-15T13:45:30.0900000Z'"
 게시 된 시간, 반환 모든 게시 시간에는 지정된 된 시간 보다 작음 | "세션/postedTime lt ' 2009-06-15T13:45:30.0900000Z'"
 세션 등록 상태 | "session/registrationState eq 'registered'"
 세션 멤버 수가 5 인 | "session/membersCount eq 5"
 세션 멤버 대상 count가 1 보다 크면 | "session/targetMembersCount gt 1"
 여기서 세션 멤버의 최대 개수는 3 보다 작은 | "세션/maxMembersCount lt 3"
 세션 멤버 대상 개수와 세션 멤버의 수 간의 차이 5 보다 작거나 | "session/targetMembersCountRemaining le 5"
 세션 멤버의 최대 수 및 세션 멤버 수가 간의 차이점은 2 자리 보다 큰 위치 | "세션/maxMembersCountRemaining gt 2"
 여기서 세션 멤버 대상 개수와 세션 멤버의 수 간의 차이 15 보다 작거나 합니다.</br> 역할에 지정 된 대상을 찾을 수 없는 경우에이 쿼리 세션 멤버의 최대 개수와 세션 멤버의 수 간의 차이 대해 필터링 합니다. | "세션/le 15 필요"
 해당 역할을 사용 하 여 멤버 수가 5 인 lfg"역할 형식"의 "확인" 역할 | "session/roles/lfg/confirmed/count eq 5"
 "확인"는 해당 역할의 대상이 1 보다 큰 lfg"역할 형식"의 역할입니다.</br> 역할에 지정 된 대상 역할의 최대 대신 사용 됩니다. | "세션/역할/lfg/확인/대상 gt 1"
 역할 "확인" 역할의 역할의 대상 및 해당 역할을 사용 하 여 멤버의 수 간의 차이 15 보다 작거나 "lfg"를 입력 합니다.</br> 역할에 지정 된 대상을 찾을 수 없는 경우에이 쿼리는 최대 역할 및 해당 역할을 사용 하 여 멤버의 수 간의 차이 대해 필터링 합니다. | "session/roles/lfg/confirmed/needs le 15"
 특정 키워드를 포함 하는 세션을 가리키는 모든 검색 처리 | "session/keywords/any(d:tolower(d) eq 'level2')"
 서비스는 특정 안내에 속한 세션을 가리키는 모든 검색 처리 | "session/scid eq '151512315'"
 특정 템플릿 이름을 사용 하는 세션을 가리키는 모든 검색 처리 | "session/templateName eq 'mytemplate1'"
 모든 처리는 태그 'elite' 또는 '이' 15 보다 큰 숫자 및 '자주색' 같음 ' 부족' 문자열 검색 | "tags/any(a:tolower(a) eq 'elite') 또는 숫자/이 15 및 문자열/부족 eq '자주색'"

### <a name="refreshing-search-results"></a>검색 결과 새로 고치는 중

 게임에 자동으로 세션의 목록을 새로 고침을 방지 해야 하지만 대신 (가능한 경우 더 잘 결과 필터링 하려면 검색 조건을 구체화) 후 목록을 수동으로 새로 고치려면 플레이어 수 있도록 UI를 제공 합니다.

 플레이어가 해당 전체 또는 닫힌 세션이 세션에 참가 하려고, 게임도 검색 결과 새로 해야 합니다.

 너무 많은 검색 새로 고침을 제목 속도는 쿼리를 새로 고칠 수를 제한 해야 하므로 서비스 제한, 발생할 수 있습니다.

 서비스 호출량을 줄이기 위해 검색 처리에 저장 하 고 빠르게 변화 세션 특성 쿼리를 사용할 수 있는 사용자 지정 세션 속성이 포함 됩니다. 검색 특성에서 이러한 특성을 저장 되어야 합니다.

### <a name="example-query-for-search-handles"></a>검색 처리에 대 한 쿼리 예제:

 다음 코드에는 검색 핸들에 대 한 쿼리 하는 방법을 보여 줍니다. API의 컬렉션을 반환 합니다. `multiplayer_search_handle_details` 쿼리와 일치 하는 모든 검색 핸들을 나타내는 개체입니다.

```cpp
 auto result = multiplayer_service().get_search_handles(scid, template, orderBy, orderAscending, searchFilter)
 .then([](xbox_live_result<std::vector<multiplayer_search_handle_details>> result)
 {
   if (result.err())
   {
      // handle error
   }
   else
   {
      // parse result.payload
   }
 });

 /* Payload element properties

 multiplayer_search_handle_details
 {
   string_t& handle_id();
   multiplayer_session_reference& session_reference();
   std::vector<string_t>& session_owner_xbox_user_ids();
   std::vector<string_t>& tags();
   std::unordered_map<string_t, double>& numbers_metadata();
   std::unordered_map<string_t, string_t>& strings_metadata();
   std::unordered_map<string_t, multiplayer_role_type>& role_types();
 }
 */
```

## <a name="join-a-session-by-using-a-search-handle"></a>검색 핸들을 사용 하 여 세션에 참가

참가 하려는 세션에 대 한 검색 핸들을 검색 한 후 제목 사용할지 `MultiplayerService::WriteSessionByHandleAsync()` 또는 `multiplayer_service::write_session_by_handle()` 세션에 추가 되도록 합니다.

> [!NOTE]
> 합니다 `WriteSessionAsync()` 고 `write_session()` 찾아보기 세션을 조인 하는 메서드를 사용할 수 없습니다.

다음 코드를 검색 핸들을 검색 한 후 세션에 참가 하는 방법에 설명 합니다.

```cpp
void Sample::BrowseSearchHandles()
{
    auto context = m_liveResources->GetLiveContext();
    context->multiplayer_service().get_search_handles(...)
    .then([this](xbox_live_result<std::vector<multiplayer_search_handle_details>> searchHandles)
    {
        if (searchHandles.err())
        {
            LogErrorFormat( L"BrowseSearchHandles failed: %S\n", searchHandles.err_message().c_str() );
        }
        else
        {
            m_searchHandles = searchHandles.payload();

            // Join the game session

            auto handleId = m_searchHandles.at(0).handle_id();
            auto sessionRef = multiplayer_session_reference(m_searchHandles.at(0).session_reference());
            auto gameSession = std::make_shared<multiplayer_session>(m_liveResources->GetLiveContext()->xbox_live_user_id(), sessionRef);
            gameSession->join(web::json::value::null(), false, false, false);

            context->multiplayer_service().write_session_by_handle(gameSession, multiplayer_session_write_mode::update_existing, handleId)
            .then([this, sessionRef](xbox_live_result<std::shared_ptr<multiplayer_session>> writeResult)
            {
                if (!writeResult.err())
                {
                    // Join the game session via MPM
                    m_multiplayerManager->join_game(sessionRef.session_name(), sessionRef.session_template_name());
                }
            });
        }
    });
}
```
