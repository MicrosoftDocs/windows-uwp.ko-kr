---
title: 멀티 플레이 세션 검색
author: KevinAsgari
description: Xbox Live 멀티 플레이어를 사용 하 여 멀티 플레이 세션 검색을 구현 하는 방법을 알아봅니다.
ms.assetid: b4b3ed67-9e2c-4c14-9b27-083b8bccb3ce
ms.author: kevinasg
ms.date: 10/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ea47c73cc882db4031f9c43ccbc64030ee039b3f
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4498472"
---
# <a name="multiplayer-session-browse"></a>멀티 플레이 세션 검색

멀티 플레이 세션 찾아보기는 2016 년 11 월에에서 도입 된 새로운 기능 목록 지정 된 조건을 충족 하는 열린 멀티 플레이 게임 세션에 대 한 쿼리를 제목입니다.

## <a name="what-is-session-browse"></a>세션 검색 란 무엇 인가요?

세션 검색 시나리오에서는 플레이어가 게임에서가 참여할 게임 세션의 목록을 검색할 수 있습니다. 이 목록의 각 세션 항목 플레이어를 가입 하는 세션을 선택 하는 데 사용할 수 있는 게임에 대 한 몇 가지 추가 메타 데이터가 포함 되어 있습니다.  또한 세션 메타 데이터에 따라 목록을 필터링 할 수 있습니다. 플레이어가 게임 세션을 들을 표시 되 면 세션에 참가할 수 있습니다.

또한 플레이어가 새 게임 세션 하 고 세션 검색을 사용 하 여 연결에 의존 하는 대신 추가 플레이어를 모집 수 있습니다.

세션 검색 기존 연결 시나리오에서 다른 플레이어 자체 매치 메이 킹에서 가입 하려는 게임 세션을 선택 하는 플레이어가 일반적으로 "게임 찾기"가 단추를 클릭할 플레이어에 자동으로 배치 하려고 하는 게임에 적합 합니다. 세션 검색 객관적으로 최상의 게임을 선택 하지 않을 수 있습니다 수동 부분과 느린 프로세스 이지만, 플레이어를 제어를 제공 하 고 오면서 더 나은 게임 선택 감지 될 수 있는 합니다.

게임에 모두 세션 검색 및 연결 시나리오를 포함 하는 것이 괜찮습니다. 일반적으로 매치 메이 킹 세션 검색 사용자 지정 게임에 사용 되는 동안 게임 모드를 일반적으로 재생에 사용 됩니다.

**예제:** John 영웅 배틀 아레나 스타일 멀티 플레이어 게임을 플레이 하는 데 관심이 있을 수 있지만 모든 플레이어 선택할 수 있는 해당 영웅 임의로 게임을 플레이 하려는 합니다. 게임 UI에서 지 원하는 경우에 "임의 영웅" 게임 모드를 선택 및 "RandomHero" gam 된다는를 태그가 지정 된 세션만 검색할 수 그 또는 그 열려 있는 게임 세션의 목록을 검색 하 고의 설명에 "임의 영웅"를 포함 하는 스타일을 찾을 수 있습니다. es 합니다.

저 게임을 찾으면 그 게임을 참석 합니다. 사용자 세션에 가입한 경우 게임 세션의 호스트는 게임을 시작할 수 있습니다.

### <a name="roles"></a>역할

게임 세션 검색에서 특정 역할에 대 한 플레이어를 모집 하고자 할 수 있습니다. 예를 들어 플레이어 세션 5 개 공격 클래스가 포함 되어 있지만 2 개 이상의 힐 역할 및 1 탱크 역할 있어야 지정 하는 게임 세션을를 만들려고 할 수 있습니다.

세션에 대 한 다른 플레이어를 적용 하면 미리 역할을 선택할 수 및 서비스는 선택한 역할에 대해 열린 슬롯 없는 경우 세션에 참가 하도록 허용 하지 않습니다.

또 다른 예는 플레이어를 가입 하는 친구에 대 한 2 슬롯 예약 하려는 경우 게임 "친구" 역할을 지정할 수 고 세션 호스트를 사용 하 여 친구 수 있는 플레이어 "친구" 역할에 전용된 2 슬롯을 채울 수입니다.

역할에 대 한 자세한 내용은 [멀티 플레이 역할](multiplayer-roles.md)을 참조 하세요.



## <a name="how-does-session-browse-work"></a>세션 검색은 어떻게 작동 합니까?

세션 검색은 주로 검색 핸들을 사용 합니다. 검색 핸들은 검색 속성, 즉 세션에 대 한 추가 메타 데이터가 뿐만 아니라 세션에 대 한 참조를 포함 하는 데이터의 패킷을 합니다.

제목이 세션에 해당 되는 새 게임 세션 검색, 세션에 대 한 검색 핸들을 만듭니다. 검색 핸들에는 멀티 플레이어 서비스 디렉터리 (MPSD), 제목에 대 한 검색 핸들을 유지 하는 저장 됩니다.

타이틀 세션의 목록을 검색 하는 경우 제목 검색 조건을 충족 하는 검색 핸들 목록이 반환 됩니다 MPSD를 검색 쿼리를 보낼 수 있습니다. 제목 세션의 목록을 사용 하 여 플레이어에 게 참여할 게임 목록을 표시할 수 있습니다.

세션 전체에 가입할 수 없을 때 제목을에서 제거할 수 검색 핸들 MPSD 나타나도록 세션은 더 이상 세션 검색 쿼리.

>[!NOTE]
> 검색 핸들 창을 사용자에 게 표시할 세션 목록을 표시할 때 사용할 수 있습니다. 가능한 경우 피해 야 백그라운드 연결에 대 한 검색 핸들을 사용 하 고 대신 [스마트](multiplayer-manager/play-multiplayer-with-matchmaking.md) 를 사용 하는 것이 좋습니다.

## <a name="set-up-a-session-for-session-browse"></a>세션 검색에 대 한 세션을 설정

세션에 대 한 검색 핸들을 사용 하려면 세션 다음 있어야 true로 설정 기능:

* `searchable`
* `userAuthorizationStyle`

>[!NOTE]
> `userAuthorizationStyle` 접근 권한 값은만 UWP 게임에 대 한 필요 하지만 권장 모든 Xbox Live에 대 한 구현 게임, XDK 게임 등 향후 이식성을 보장 합니다.

>[!NOTE]
> 설정의 `userAuthorizationStyle` 접근 권한 값은 기본값은 `readRestriction` 및 `joinRestriction` 세션의 `local` 대신 `none`. 즉, 타이틀 검색 핸들을 사용 하거나 게임 세션에 대 한 핸들을 전송 해야 합니다.

Xbox Live 서비스를 구성 하는 경우 세션 서식 파일에서 이러한 접근 권한이 값을 설정할 수 있습니다.

세션 검색에 대 한 검색 핸들 로비 세션에 대 한 실제 게임 플레이에 사용 되는 세션에서 만들 해야 있습니다.

## <a name="what-does-it-mean-to-be-an-owner-of-a-session"></a>무엇을 의미 세션의 소유자?

대부분의 게임 세션 스마트 또는 게임 친구와 같이 형식 소유자 필요 하지 않은, 세션 검색 세션에 대 한 소유자 할 수도 있습니다. 

소유자 관리 세션 것에 소유자에 대 한 몇 가지 이점이 있습니다. 소유자 세션에서 다른 멤버를 제거 하거나 다른 멤버의 소유권 상태를 변경할 수 있습니다.

세션에 대 한 소유자를 사용 하려면 세션 다음 있어야 true로 설정 기능:

* `hasOwners`

세션의 소유자 차단 하는 Xbox Live 멤버에 있는 경우 해당 멤버 세션에 참가할 수 없습니다.

[멀티 플레이 역할](multiplayer-roles.md)을 사용 하 여를 설정할 수 있습니다 작업만 소유자 사용자에 게 역할을 할당할 수 있습니다.

모든 소유자 세션을 종료 경우 서비스는 기반 세션에 대 한 작업은 `ownershipPolicy.migration` 세션에 대해 정의 된 정책입니다. 정책 "가장 오래 된" 인 경우 새 소유자에 있었던 세션 긴 플레이어 설정 됩니다. 정책 "endsession" (제공 하지 않으면 기본값) 인 경우 서비스 세션을 종료 고 세션에서 모든 나머지 플레이어를 제거 합니다.


## <a name="search-handles"></a>검색 핸들

검색 핸들 JSON 구조로 MSPD에 저장 됩니다. 세션에 대 한 참조를 포함 하는 것 외에도 검색 핸들 검색 특성으로 알려진 검색에 대 한 추가 메타 데이터가 포함 되어 있습니다.

세션을 언제 든 지 생성 한 검색 핸들을 하나만 사용할 수 있습니다.

검색 핸들에 세션에 대 한 Xbox Live Api를 사용 하 여를 만들려면 먼저 만들기는 `multiplayer::multiplayer_search_handle_request` 개체를 해당 개체를 전달 합니다 `multiplayer::multiplayer_service::set_search_handle()` 메서드.

### <a name="search-attributes"></a>검색 특성

검색 특성 다음 구성 요소로 이루어져 있습니다.

`tags` -태그는 사용자는 hashtag와 유사한 게임 세션을 분류 하는 데 사용할 수 있는 문자열 설명자 합니다. 태그 문자로 시작 해야 하 고, 공백을 포함할 수 100 자 미만 이어야 합니다.
예제 태그: "ProRankOnly", "norocketlaunchers", "cityMaps"입니다.

`strings` -문자열 텍스트 변수 되며 문자열 이름 문자로 시작, 공백을 포함할 수 및 100 자 미만 이어야 합니다.

문자열 메타 데이터 예: "무기"="칼 + 피스 + 라이플", "맵 이름" = "UrbanCityAssault", "하는 가벼운 게임과 재미 있는 설명"=", 새로운 사용자 환영 합니다."

`numbers` -숫자 숫자 변수 있으며 숫자 이름은 문자로 시작 해야 공백을 포함할 수 하 고 100 자 미만 이어야 합니다. Xbox Live Api float 형식으로 숫자 값을 검색 합니다.

숫자 메타 데이터 예: "MinLevel" = "MaxRank" 25 = 10.

>**참고:** 태그 및 문자열 값은 대/소문자를 서비스에서 보존 됩니다 하지만 태그 쿼리할 때 tolower() 함수를 사용 해야 합니다. 즉, 태그 및 문자열 값 현재 소문자, 국가로 모두 대문자로 포함 하는 경우에 합니다.

Xbox Live api에서를 사용 하 여 검색 특성을 설정할 수 있습니다는 `set_tags()`, `set_stringsmetadata()`, 및 `set_numbers_metadata()` 메서드는 `multiplayer_search_handle_request` 개체입니다.


### <a name="additional-details"></a>추가적인 세부 정보

결과 추가 유용한 데이터를 포함 하는 검색 핸들을 검색 하는 경우 세션에 대 한 같은 세션을 종료 하는 경우 제한 사항이 있습니까 조인 세션 등.

검색 특성과 함께 이러한 세부 정보에 포함 된 Xbox Live Api에는 `multiplayer_search_handle_details` 검색 쿼리 후 돌아갑니다.

### <a name="remove-a-search-handle"></a>검색 핸들을 제거

세션은 full 같은 세션 검색에서 세션을 제거 하려는 경우 또는 세션이 닫힙니다 경우 검색 핸들을 삭제할 수 있습니다.

Xbox Live Api를 사용할 수는 `multiplayer_service::clear_search_handle()` 검색 핸들을 제거 하는 방법.

### <a name="example-create-a-search-handle-with-metadata"></a>예: 메타 데이터를 사용 하 여 검색 핸들 만들기

다음 코드를 c + + Xbox Live 멀티 플레이 Api를 사용 하 여 세션에 대 한 검색 핸들을 만드는 방법을 보여 줍니다.

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


## <a name="create-a-search-query-for-sessions"></a>세션에 대 한 검색 쿼리를 작성 합니다.

목록을 검색 핸들을 검색할 때 특정 조건을 충족 하는 세션에 결과 제한 하려면 검색 쿼리를 사용할 수 있습니다.

검색 쿼리 구문은 지원 다음 연산자를 사용 하 여 [OData](http://docs.oasis-open.org/odata/odata/v4.0/errata02/os/complete/part2-url-conventions/odata-v4.0-errata02-os-part2-url-conventions-complete.html#_Toc406398092) 스타일 구문을 다음과 같습니다.

 연산자 | 설명
 --- | ---
 eq | 같음
 ne | 같지 않음
 gt | 보다 큼
 ge | 크거나 같음
 lt | 미만
 le | 작거나 같음
 및 | 논리
 또는 | 논리 (아래 참고 참조) 또는

람다 식 사용할 수도 있습니다 및 `tolower` 정식 함수입니다. 다른 없음 OData 함수는 현재 지원 됩니다.

태그 또는 문자열 값을 검색할 때 소문자 문자열에 대 한 검색만 현재 서비스를 지 원하는 'tolower' 함수에서 검색 쿼리를 사용 해야 합니다.

Xbox Live 서비스 검색 쿼리를 일치 하는 처음 100 결과 반환 합니다. 게임 결과 너무 광범위 하 게 하는 경우 해당 검색 쿼리를 구체화 하는 플레이어를 허용 해야 합니다.

>[!NOTE]
>  필터 문자열 쿼리에서; Or 논리는 지원 그러나 하나만 OR 수 있으며 쿼리의 루트에 있어야 합니다. 쿼리에 Or 여러 개의 및 하면 쿼리를 만들 수 있는 겹치는 또는 없는 맨 위에 있는 대부분의 수준의 쿼리 구조입니다.

### <a name="search-handle-query-examples"></a>검색 핸들 쿼리 예제

Restful 호출 "필터" 검색 핸들은 모두에 대해 쿼리에서 실행 되는 OData 필터 언어 문자열을 지정 합니다 됩니다.  
멀티 플레이 2015 Api 검색 필터 문자열의 *searchFilter* 매개 변수를 지정할 수는 `multiplayer_service.get_search_handles()` 메서드.  

현재 다음 필터 시나리오 지원 됩니다.

 필터링 기준 | 검색 필터 문자열
 --- | ---
 단일 구성원 xuid '1234566' | "세션/memberXuids/any(d:d eq '1234566')"
 단일 소유자 xuid '1234566' | "세션/ownerXuids/any(d:d eq '1234566')"
 'Forzacarclass' 'classb'과 같은 문자열 | "tolower(strings/forzacarclass) eq 'classb'"
 여러 'forzaskill' 6과 같은 | "숫자/forzaskill eq 6"
 'Halokdratio' 1.5 보다 큰 숫자 | "숫자/halokdratio gt 1.5"
 태그 'coolpeopleonly' | "tags/any(d:tolower(d) eq 'coolpeopleonly')"
 'Cursingallowed' 태그를 포함 하지 않는 세션 | "tags/any(d:tolower(d) ne 'cursingallowed')"
 '순위'는 번호를 포함 하지 않는 세션은 0 | "숫자/순위 ne 0"
 'Forzacarclass' 문자열을 포함 하지 않는 세션 같은지 'classa'를 | "tolower(strings/forzacarclass) ne 'classa'"
 태그 'coolpeopleonly'와 'halokdratio' 7.5 같지 번호 | "tags/any(d:tolower(d) eq 'coolpeopleonly') eq true 및 숫자/halokdratio eq 7.5"
 숫자 'halodkratio' 보다 크거나 1.5, 숫자를 ' 및 순위를 지정 ' 60 보다는 숫자 'customnumbervalue' 적게 5 이하일 | "숫자/halokdratio ge 1.5, 숫자/순위 lt 60 및 숫자/customnumbervalue le 5"
 도전 과제 id '123456' | "achievementIds/any(d:d eq '123456')"
 언어 코드 'en' | "언어 eq 'en'"
 예약 된 시간, 예약 된 모든 반환 시간이 지정 된 시간 보다 작거나 같아야 | "세션/scheduledTime le ' 2009-06-15T13:45:30.0900000Z'"
 게시 된 시간, 모두 게시 반환 시간 지정한 시간 보다 작은 | "세션/postedTime lt ' 2009-06-15T13:45:30.0900000Z'"
 세션 등록 상태 | "세션/registrationState eq '등록'"
 세션 멤버의 수는 5 | "세션/membersCount eq 5"
 세션 멤버 대상 개수는 1 보다 큰 | "세션/targetMembersCount gt 1"
 세션 멤버의 최대 수는 보다 작은 3 | "세션/maxMembersCount lt 3"
 세션 멤버 대상 개수와 세션 구성원 수가 간의 차이 5 보다 작거나 | "세션/targetMembersCountRemaining le 5"
 세션 멤버의 최대 수와 세션 구성원 수 간의 차이 2 이상인 | "세션/maxMembersCountRemaining gt 2"
 여기서 세션 멤버 대상 개수와 세션 구성원 수가 간의 차이 15 보다 작거나입니다.</br> 역할에 지정 된 대상이 없는 경우 세션 멤버의 최대 수와 세션 구성원 수의 차이 대해이 쿼리 필터링 합니다. | "세션 le 15 필요한 /"
 "확인" 역할 유형 "lfg" 해당 역할 멤버의 수는 5의 역할 | "세션/역할/lfg/확인/개수 eq 5"
 "확인" 해당 역할의 대상이 1 보다 큰 lfg"역할 유형"의 역할입니다.</br> 역할에 지정 된 대상이 없는 역할의 최대 대신 사용 됩니다. | "세션/역할/lfg/확인/대상 gt 1"
 역할 "확인" 역할의 역할의 대상 및 해당 역할 구성원 수가 간의 차이 15 보다 작거나 같아야 "lfg"를 입력 합니다.</br> 역할에 지정 된 대상이 없는 경우 해당 역할 멤버의 수와 역할의 최대 간의 차이 대해이 쿼리 필터링 합니다. | "세션/역할/lfg/확인/요구 le 15"
 특정 키워드를 포함 하는 세션을 가리키는 모든 검색 핸들 | "session/keywords/any(d:tolower(d) eq 'level2')"
 특정 서비스 안내에 속한 세션을 가리키는 모든 검색 핸들 | "세션/서비스 안내 eq '151512315'"
 특정 템플릿 이름을 사용 하는 세션을 가리키는 모든 검색 핸들 | "세션/templateName eq 'mytemplate1'"
 모든 핸들 태그 'elite' 또는 '포' 15 보다 큰 숫자를 '부족' '보라색'과 같은 문자열을 검색합니다 | "tags/any(a:tolower(a) eq 'elite') 또는 숫자/포 gt 15 및 문자열/부족 eq '보라색'"

### <a name="refreshing-search-results"></a>검색 결과 새로 고침

 게임 세션을 목록이 자동으로 새로 방지 하지만 플레이어를 수동으로 목록 (가능한 경우 더 나은 결과 필터링 하려면 검색 조건을 조정) 후 새로 고칠 수 있도록 UI를 제공 하는 대신 해야 합니다.

 플레이어 세션은 전체 또는 닫힌 하지만 해당 세션에 참가 하려고 시도 하면 게임도 검색 결과 복구 해야 합니다.

 너무 많은 검색 새로 고침 타이틀 속도는 쿼리를 새로 고칠 수를 제한 해야 하므로 서비스 제한 될 수 있습니다.

### <a name="example-query-for-search-handles"></a>검색 핸들에 대 한 예: 쿼리

 다음 코드는 검색 핸들에 대 한 쿼리 하는 방법을 보여 줍니다. 컬렉션을 반환 하는 API `multiplayer_search_handle_details` 쿼리를 일치 하는 모든 검색 핸들을 나타내는 개체.

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

제목을 사용 해야 참여 하고자 하는 세션에 대 한 검색 핸들을 검색 되 면 `MultiplayerService::WriteSessionByHandleAsync()` 또는 `multiplayer_service::write_session_by_handle()` 세션에 직접 추가 합니다.

> [!NOTE]
> `WriteSessionAsync()` 및 `write_session()` 메서드를 사용 하 여 세션 검색 세션에 참가할 수 없습니다.

다음 코드에서는 검색 핸들을 검색 한 후 세션에 참가 하는 방법을 보여 줍니다.

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
