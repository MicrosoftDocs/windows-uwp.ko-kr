---
title: 새로운 기능에 대 한 Xbox Live SDK-2016 년 11 월
author: KevinAsgari
description: 새로운 기능에 대 한 Xbox Live SDK-2016 년 11 월
ms.assetid: 5cf9ba9d-5a15-4e62-bc1f-45ff8b8bf3b0
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 86c17bad95cc309d39f50341d296dc5d54552ece
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5938651"
---
# <a name="whats-new-for-the-xbox-live-sdk---november-2016"></a>새로운 기능에 대 한 Xbox Live SDK-2016 년 11 월

추가 된 항목에 대 한 [새로운 기능-2016 년 8 월](1608-whats-new.md) 문서를 참조 하십시오 8 월 2016 릴리스에서 합니다.

## <a name="xbox-services-api"></a>Xbox 서비스 API

### <a name="simplified-achievements"></a>간소화 된 도전 과제

* [간체 도전 과제](../achievements-2017/simplified-achievements.md) 는 새롭고 더 간단 하 게 구성 및 도전 과제를 사용 합니다.  더 이상 해야에 도전 과제 잠금이 해제 인지 여부를 결정 하는 Xbox Live 서비스 있고, 이벤트를 전송 합니다.  타이틀은 해당 의사를 담당 합니다.
* 간체 도전 과제의 초기 파일럿의 일부로 포함 되어 있는 경우 오프 라인 지원을 추가 했습니다.
* 사용 하 여이 얼마나 쉬운지 해제를 보여 주는 SimplifiedAchievements 라는 새 샘플이 있습니다.

### <a name="multiplayer"></a>멀티 플레이

* [세션 찾아보기](../multiplayer/session-browse.md) 없으며 사용자가 멀티 플레이어 게임을 찾을 수는 새로운 방법입니다.  세션 검색 플레이어를 지정 된 조건을 충족 하는 열린 멀티 플레이 게임 세션의 목록을 검색할 수 있습니다.
* [멀티 플레이어 관리자](../multiplayer/multiplayer-manager.md) 는 자동 채우기 기능 되었습니다.  사용 하도록 설정 하는 경우 멀티 플레이어 관리자 연결을 통해 게임 플레이 하는 동안 열린 슬롯 채우기 구성원 찾을 있습니다.
* [XIM (Xbox 통합 멀티 플레이어)](../multiplayer/xbox-integrated-multiplayer.md) 의 사전 프로덕션 버전 XDK 개발에 대 한 출시 되었습니다.  XIM 쉽게 실시간 멀티 플레이어 네트워킹 및 채팅 통신 Xbox Live 서비스의 기능을 통해 게임에 추가 하기 위한 독립적인된 인터페이스입니다.

### <a name="social-manager"></a>소셜 관리자

* 다양 한 [소셜 관리자](../social-platform/intro-to-social-manager.md) API 변경 내용:
    * 소셜 관리자 요소가 실험적 네임 스페이스를 유지 합니다. 얼 리어 답 터 되 고 피드백을 준 나왔습니다 덕분 특수!
    * `xbox_social_user`
        * `string_t` 개체로 변경 되었습니다 `char*` 개체
        * 6의 제한 된 사용자 지정 존재 기록 `social_manager_presence_title_record` 당 `social_manager_presence_record`
    * `social_event`
        * 반환은 `std::vector<xbox_user_id_container>` 대신 `std::vector<xbox_social_user>`
        * 새로운 API에이 벡터를 전달할 수 있습니다. `xbox_social_user_group::get_users_from_xbox_user_ids()`
    * `xbox_social_user_group`
        * `users()` 이제 반환 API는 `std::vector<xbox_social_user*>`. 이러한 포인터 다음 호출에서 무효화 됩니다. `social_manager::do_work()`
        * `get_copy_of_users` 반환는 `std::vector<xbox_social_user>` 호출자에 게 social_user_group의 현재 사용자의 복사본으로 합니다. 이 함수에 영향을 미칠 수 호출 `do_work` 완료 시간.
* 이제 소셜 관리자는 되지 초기화 한 후 실패 됩니다. 소셜 관리자 연결 끊기에 RTA를 자동으로 다시 됩니다. `error` 및 `rta_disconnect_error` 이벤트 사용 중단 되 고 제거
* 제목은 사용자 지정 메모리 할당자 지정할 수 있습니다. 새 설명서를 참조 하세요: [소셜 관리자 메모리 및 성능](../social-platform/social-manager-memory-and-performance-overview.md)

### <a name="xbox-one-uwp"></a>Xbox One UWP
* Xbox One UWP 앱에 대 한 지원 다중 사용자를 추가 하는 TCUI Api입니다.  새 Windows::System::User를 추가 하는 c + + XSAPI ^ 매개 변수는 사용자 지정 하 고 XSAPI WinRT API를 ForUserAsync Api를 추가 합니다.
* Xbox 다중 사용자 지원 하도록 업데이트 된 UWP 샘플

### <a name="other"></a>기타

* C + + /winrt 지원은 추가 합니다.   세부 정보를 찾을 수 있습니다 [여기](../introduction-to-xbox-live-apis.md)
* 새로운 기능에 추가 하 고 고유한 로깅 콜백을 제거 합니다.  진단 수준에 동작을 미세 조정할 수 있도록 콜백에 전달 됩니다.  참고 `add_logging_handler` 및 `remove_logging_handler` 에 `microsoft::xbox::services::system::xbox_live_services_settings` 네임 스페이스

## <a name="documentation"></a>설명서
* Xbox live [멀티 플레이어 관리자](../multiplayer/multiplayer-manager.md), [XIM](../multiplayer/xbox-integrated-multiplayer.md)및 [멀티 플레이 개념](../multiplayer/multiplayer-concepts.md) 에 대 한 새 설명서가 있습니다.
* [Xbox Live 소개](../get-started-with-partner/get-started-with-xbox-live-partner.md) 섹션 재작성 했습니다.  새 문서를 볼 수 새 Xbox Live가 지원 타이틀을 만드는 된 게임에 Xbox Live 기능을 통합 하는 방법에 대 한 이름을 바꾼 경우, [다음과 같습니다](../get-started-with-partner/get-started-with-xbox-live-partner.md).

## <a name="tools"></a>도구
* Xbox Live 추적 분석기에서 찾을 수 있습니다 [http://aka.ms/XboxLiveTools](http://aka.ms/XboxLiveTools) 인증서 모드 되었습니다.  
* 또한 Xbox Live 추적 분석기에서 다중 콘솔 지원이입니다.  여러 콘솔에 대 한 HTTP 요청을 포함 하는 Fiddler 추적에 전달 하는 경우 각각에 대해 별도 보고서 생성 됩니다.
