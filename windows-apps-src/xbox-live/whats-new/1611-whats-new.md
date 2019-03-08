---
title: 새로운 기능-2016 년 11 월 Xbox Live SDK 용
description: 새로운 기능-2016 년 11 월 Xbox Live SDK 용
ms.assetid: 5cf9ba9d-5a15-4e62-bc1f-45ff8b8bf3b0
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4a9efeec63399916444de9ba33d4e587a914f2a7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658568"
---
# <a name="whats-new-for-the-xbox-live-sdk---november-2016"></a>새로운 기능-2016 년 11 월 Xbox Live SDK 용

참조 하세요 합니다 [새로운 기능-2016 년 8 월](1608-whats-new.md) 추가 된 기능에 대 한 문서는 2016 년 8 월 릴리스에서 합니다.

## <a name="xbox-services-api"></a>Xbox Services API

### <a name="simplified-achievements"></a>간소화 된 성과

* [성과 간체](../achievements-2017/simplified-achievements.md) 는 구성 및 성과 사용 하는 새롭고 간단한 방법입니다.  더 이상 해야 Xbox Live 서비스에 도전 과제 잠긴 한지 결정 했으며 이벤트를 전송 합니다.  제목 결정을 담당 합니다.
* 간소화 된 성과의 초기 파일럿의 일부로 포함 되어 있으면 오프 라인 지원을 추가 했습니다.
* Off를 사용 하 여 얼마나 쉬운지 보여 주는 SimplifiedAchievements 라는 새로운 샘플이 있습니다.

### <a name="multiplayer"></a>멀티 플레이

* [세션 찾아보기](../multiplayer/session-browse.md) 멀티 플레이 게임을 찾으려는 사용자에 게는 새로운 방법입니다.  세션 찾아보기 플레이어를를 지정 된 조건을 충족 하는 열린 멀티 플레이 게임 세션 목록을 검색할 수 있습니다.
* 합니다 [멀티 플레이 게임 Manager](../multiplayer/multiplayer-manager.md) 자동 채우기 기능 설정 되었습니다.  사용 하도록 설정 하는 경우 다중 접속 관리자는 게임 플레이 중 열기 슬롯에 맞게 결혼 정보 회사 연결을 통해 멤버를 찾습니다.
* 사전 프로덕션 버전의 [XIM (Xbox 통합 멀티 플레이 게임)](../multiplayer/xbox-integrated-multiplayer.md) XDK 개발에 대 한 출시 되었습니다.  XIM는 Xbox Live 서비스의 기능을 통해 게임에 실시간 다중 접속 네트워킹 및 채팅 통신을 쉽게 추가 하는 것에 대 한 자체 포함 된 인터페이스입니다.

### <a name="social-manager"></a>주민 등록 관리자

* 수많은 [소셜 Manager](../social-platform/intro-to-social-manager.md) API 변경 내용:
    * 소셜 manager 실험적 네임 스페이스를 떠났습니다. 특히 감사 드립니다 사용자 얼 리 어 되었으며 피드백을 제공 했습니다!
    * `xbox_social_user`
        * `string_t` 개체에 변경 된 `char*` 개체
        * 6 개 제한 사용자 지정 상태 레코드 `social_manager_presence_title_record` 당 `social_manager_presence_record`
    * `social_event`
        * 반환 된 `std::vector<xbox_user_id_container>` 대신 `std::vector<xbox_social_user>`
        * 이 벡터는 새로운 API에 전달할 수 있습니다. `xbox_social_user_group::get_users_from_xbox_user_ids()`
    * `xbox_social_user_group`
        * `users()` API 반환을 `std::vector<xbox_social_user*>`입니다. 이러한 포인터에 대 한 다음 호출에서 무효화 됩니다. `social_manager::do_work()`
        * `get_copy_of_users` 반환 된 `std::vector<xbox_social_user>` 호출자에 게 social_user_group의 현재 사용자의 복사본으로 합니다. 이 함수에 영향을 줄 수 호출 `do_work` 완료 시간입니다.
* 이제 주민 등록 관리자가 되지 초기화 후 되지 않습니다. 소셜 네트워크 관리자에서 다시 시도할 예정임 RTA 자동으로 연결 끊기를 기반으로 합니다. 합니다 `error` 고 `rta_disconnect_error` 사용 되지 않음 및 제거 된 이벤트
* 제목은 사용자 지정 메모리 할당자를 지정할 수 있습니다. 새 설명서를 참조 하세요. [소셜 관리자 메모리 및 성능](../social-platform/social-manager-memory-and-performance-overview.md)

### <a name="xbox-one-uwp"></a>Xbox One UWP
* TCUI Api 지원 Xbox One UWP 앱에 대 한 다중 사용자를 추가합니다.  XSAPI c + + 추가 새 Windows::System::User ^ 매개 변수 사용자 지정 및 XSAPI WinRT API ForUserAsync Api가 추가 되었습니다.
* Xbox에서 다중 사용자를 지원 하도록 업데이트 된 UWP 샘플

### <a name="other"></a>기타

* C + + /cli WinRT 지원이 추가 되었습니다.   자세한 내용을 확인할 수 있습니다 [여기](../introduction-to-xbox-live-apis.md)
* 새로운 기능 하려면 사용자 고유의 로깅 콜백을 추가 및 제거 합니다.  사용자의 동작을 미세 조정할 수 있도록 진단 수준은 콜백에 전달 됩니다.  참조 `add_logging_handler` 하 고 `remove_logging_handler` 에 `microsoft::xbox::services::system::xbox_live_services_settings` 네임 스페이스

## <a name="documentation"></a>설명서
* 새 설명서가 합니다 [멀티 플레이 게임 관리자](../multiplayer/multiplayer-manager.md), [XIM](../multiplayer/xbox-integrated-multiplayer.md), 및 [멀티 플레이 개념](../multiplayer/multiplayer-concepts.md) Xbox live.
* 합니다 [Xbox Live 소개](../get-started-with-partner/get-started-with-xbox-live-partner.md) 섹션 다시 작성 되었습니다.  만들 새 Xbox Live를 사용 하도록 설정 제목 또는 게임에 다른 Xbox Live 기능을 통합 하는 방법에 대 한 내용을 보려면, 새 문서를 볼 수 있습니다 [여기](../get-started-with-partner/get-started-with-xbox-live-partner.md)합니다.

## <a name="tools"></a>도구
* Xbox Live 추적 분석기에서 찾을 수 있는 [ https://aka.ms/XboxLiveTools ](https://aka.ms/XboxLiveTools) 인증서 모드 설정 되었습니다.  
* Xbox Live 추적 분석기에서 이기도 다중 콘솔 지원 합니다.  여러 콘솔에 대 한 HTTP 요청을 포함 하는 Fiddler 추적에 전달 하는 경우 별도 보고서 각각에 대해 생성 됩니다.
