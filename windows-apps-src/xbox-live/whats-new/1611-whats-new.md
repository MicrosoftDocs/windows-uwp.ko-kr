---
title: 새 기능을 Xbox Live sdk-2016 년 11 월
author: KevinAsgari
description: 새 기능을 Xbox Live sdk-2016 년 11 월
ms.assetid: 5cf9ba9d-5a15-4e62-bc1f-45ff8b8bf3b0
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 642d15a85837ed23ea98dc3f9bd39b8d50d860b2
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5483513"
---
# <a name="whats-new-for-the-xbox-live-sdk---november-2016"></a>새 기능을 Xbox Live sdk-2016 년 11 월

추가 된 내용에 대 한 [새로운-2016 년 8 월](1608-whats-new.md) 문서를 참조 하십시오에서 2016 년 8 월 출시.

## <a name="xbox-services-api"></a>Xbox 서비스 API

### <a name="simplified-achievements"></a>단순된 성과

* [간체 성과](../achievements-2017/simplified-achievements.md) 새롭고 더 간단한 방식으로 구성 하 여 성과 사용 합니다.  더 이상 이벤트를 보내고 Xbox Live 서비스를 달성 하면 잠금이 해제 되어 결정 해야 합니다.  프로그램 제목은 결정을 담당 합니다.
* 간체 업적의 초기 조종사의 일부가 되었을 경우 오프 라인 지원을 추가 했습니다.
* 높이 방법을 사용 하 여 표시 되는 SimplifiedAchievements 라는 새로운 샘플이 있습니다.

### <a name="multiplayer"></a>멀티 플레이

* [세션 검색](../multiplayer/session-browse.md) 사용자 들이 멀티 플레이 게임을 찾을 수는 새로운 방법입니다.  세션 찾아보기 플레이어가 지정한 조건에 맞는 열린 멀티 플레이어 게임 세션 목록을 검색할 수 있습니다.
* [멀티 플레이어 관리자](../multiplayer/multiplayer-manager.md) 자동 채우기 기능에 있습니다.  사용 하면 멀티 플레이어 관리자 열기 슬롯 게임 도중에 맞게 매치 메이크 통해 구성원 찾을 수 있습니다.
* [XIM (Xbox 통합 멀티 플레이어)](../multiplayer/xbox-integrated-multiplayer.md) 의 시 제품 버전이 XDK 개발 출시 되었습니다.  XIM은 Xbox Live 서비스의 강력한 기능을 통해 게임을 쉽게 실시간 멀티 네트워킹 및 채팅 통신을 추가 하기 위한 독립 된 인터페이스입니다.

### <a name="social-manager"></a>사회 관리자

* 수많은 [사회 관리자](../social-platform/intro-to-social-manager.md) API 변경 내용:
    * 소셜 매니저 실험 네임 스페이스를 떠났습니다. 특수가 채택한 의견을 준 사람 덕분에!
    * `xbox_social_user`
        * `string_t` 개체에 변경 된 `char*` 개체
        * 6의 한도 사용 하 여 사용자 정의 존재 기록 `social_manager_presence_title_record` 당 `social_manager_presence_record`
    * `social_event`
        * 반환 된 `std::vector<xbox_user_id_container>` 대신 `std::vector<xbox_social_user>`
        * 이 벡터는 새로운 API에 전달 될 수 있습니다. `xbox_social_user_group::get_users_from_xbox_user_ids()`
    * `xbox_social_user_group`
        * `users()` 이제 반환 API는 `std::vector<xbox_social_user*>`. 이러한 포인터 다음에 무효화 됩니다. `social_manager::do_work()`
        * `get_copy_of_users` 반환 된 `std::vector<xbox_social_user>` 호출자에 게 social_user_group에 있는 현재 사용자의 사본으로. 전화를이 기능에 영향을 미칠 수 있습니다 `do_work` 완료 시간입니다.
* 사회 관리자 이제 실패할 초기화 후. 사회 관리자 연결이 끊긴 상태에서 RTA 자동으로 시도 합니다. `error` 및 `rta_disconnect_error` 이벤트는 사용 되지 않으며 제거 된
* 제목은 사용자 지정 메모리 할당자를 지정할 수 있습니다. 새 설명서를 참조 하십시오: [사회 관리자 메모리 및 성능](../social-platform/social-manager-memory-and-performance-overview.md)

### <a name="xbox-one-uwp"></a>Xbox 1 UWP
* TCUI Api 지원 Xbox 한 UWP 응용 프로그램에 대 한 다중 사용자를 추가합니다.  새 Windows::System::User를 추가 하는 c + + XSAPI ^ XSAPI WinRT API ForUserAsync Api를 추가 하 고 매개 변수는 사용자 지정 합니다.
* Xbox에서 다중 사용자를 지원 하도록 업데이트 된 UWP 샘플

### <a name="other"></a>기타

* C + + /cli WinRT 지원 기능이 추가 되었습니다.   자세한 내용은 [여기](../introduction-to-xbox-live-apis.md)
* 새로운 기능에 추가 하 고 직접 로깅 콜백을 제거 합니다.  사용자가 작업을 미세 조정할 수 있도록 진단 로깅 수준은 콜백에 전달 됩니다.  참고 `add_logging_handler` 및 `remove_logging_handler` 에 있는 `microsoft::xbox::services::system::xbox_live_services_settings` 네임 스페이스

## <a name="documentation"></a>설명서
* Xbox Live에 대 한 [멀티 플레이어 관리자](../multiplayer/multiplayer-manager.md), [XIM](../multiplayer/xbox-integrated-multiplayer.md) [멀티 개념](../multiplayer/multiplayer-concepts.md) 에 새 문서가 있습니다.
* [Xbox Live 소개](../get-started-with-partner/get-started-with-xbox-live-partner.md) 섹션 다시 작성 되었습니다 했습니다.  새 Xbox Live 사용 제목을 만드는 게임에 다른 Xbox Live 기능을 통합 궁금한 경우 새 문서를 볼 수 있습니다 [여기](../get-started-with-partner/get-started-with-xbox-live-partner.md).

## <a name="tools"></a>도구
* Xbox Live Trace 분석기에서 찾을 수 있는 [http://aka.ms/XboxLiveTools](http://aka.ms/XboxLiveTools) 인증서 모드 되었습니다.  
* 또한 Xbox Live Trace 분석기에서는 멀티 콘솔 지원 합니다.  여러 콘솔에 대 한 HTTP 요청을 포함 하는 Fiddler 추적에 전달 하는 경우 각각에 대해 별도 보고서 생성 됩니다.
