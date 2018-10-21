---
title: 타이틀에서 플레이어 피드백 보내기
author: KevinAsgari
description: 어떻게 타이틀을 돕는 플레이어 피드백 신뢰도 Xbox Live 서비스에 전송 하 여 플레이어 긍정적인 경험을 홍보에 대해 알아봅니다.
ms.assetid: 49f8eb44-1e31-4248-b645-9123df6f8689
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 평판, 플레이어 피드백
ms.localizationpriority: medium
ms.openlocfilehash: e5239c04bd6a178133129f43fcd8a71c8e532b01
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2018
ms.locfileid: "5170712"
---
# <a name="sending-player-feedback-from-your-title"></a>타이틀에서 플레이어 피드백 보내기
대부분의 Xbox Live 구성원은 훌륭한 하지만 소수의 다른 사람의 게임 환경을 저하는 "잘못 된 사과". 사용자를 통해 사용자의 작은 이러한 비율을 식별 하 고 제출한 피드백 제목 합니다. 이러한 "잘못 된 사과" 수 있도록 제한 멀티 플레이어 경험 수 없는 방해가 있는 좋은 플레이어의 게임에서 인구의 나머지 부분을 보호 하도록 돕. Xbox 사용자 시스템을 정확 하 게 유지 하 게 보고 크게 의존 있으며 타이틀에 Xbox One 직접 참여 수 있습니다 크게 사용자층 신뢰도의 정확도 향상 시키는 데 도움이 됩니다.

## <a name="steps-to-submit-feedback-from-title-or-title-service"></a>제목이 나 제목 서비스에서 피드백을 제출 하는 단계
1. 피드백 순간 제목이 나 제목 서비스에 추가
2. 올바른 피드백 유형 확인
3. 피드백을 사용 하 여 신뢰도 피드백 Api 호출

### <a name="adding-feedback-moments-to-title-or-title-service"></a>추가 피드백 순간을 제목이 나 제목 서비스
게이머 자신의 측면, 누구 방금 주위를 재생 하는 대신 플레이어 또는 사용자가 게임을 파괴 된 cheaters 파괴 하는 동료와 잘못 된 환경을 했습니다. Xbox Live 수 있도록 이러한 문제가 있는 플레이어를 직접 보고 하지만 사용자 피드백 완벽 하지 않습니다. 타이틀 간단한 작업 플레이어가 게임에서 유휴을 조기에 종료 고 때로는 지정할 수도 부정 행위는 사용자를 쉽게 확인할 수 있습니다. 타이틀에서 좋은 모든 플레이어의 환경을 개선에 도움이 되는 피드백 분간의 다양 한 피드백을 제출할 수 있습니다.

### <a name="determining-the-correct-feedback-type"></a>올바른 피드백 유형 확인
신뢰도 시스템에 사용자 피드백을 보증 하는 다양 한 방법을 캡처를 위한 많은 피드백 유형이 있습니다. 아래 표 1에 완벽 하 게 나열 됩니다. 부정적는 대부분 이지만 양수 피드백도를 사용 하 여 사용자의 평판을 향상 시킬 수 있습니다.

Xbox 시스템 UI는 플레이어가 게임에서 다른 사용자에 게 피드백을 제출 하는 방법을 제공 합니다. 해당 사용자에 피드백 수행 하지 않으면 훨씬 두께 사용자는 griefing 이후 서로 때 사라집니다. 타이틀 해당 시스템 UI를 직접, 피드백을 제출 하려면 사용자에 대 한 UI를 제공 보완할 수 있지만 대신 제목 피드백을 제출할 수 있는지 제목 자체를 대신 하 여 파트너 피드백을 사용 하 여 선호 하 합니다. 파트너 피드백은 매우 신뢰할 수 있는 합니다.

## <a name="example-partner-feedback-usage-scenarios"></a>예제 파트너 피드백 사용 시나리오

### <a name="user-quitting-a-game-in-the-middle-of-a-match"></a>사용자가 일치 하는 중에 게임 종료
플레이어 게임 손실 되 고 자신의 동료 abandoning 게임을 종료 하 게임의 메뉴를 사용 합니다. 사용 하 여 해당 동작을 보고할 수는 제목이이 동작을 감지할 때 **FairPlayQuitter.**

### <a name="idling-after-match-found-in-game"></a>일치 하는 게임에서 후 유휴
플레이어 가져옵니다 재생 하는 데 다른 플레이어와 일치 하지만 팀에 도움이 되는 대신 게임에서 스탠드 유휴 일관 되 게 합니다. 제목을 사용 하 여 플레이어의 동작에 보고할 수 있습니다 **FairplayIdler.**

### <a name="killing-teammates-in-game"></a>게임에서 중단 동료
게임에서 플레이어 재미를 위한 동료 중단 지속적으로 됩니다. 제목을 지속적으로 사용자가 감지 것 팀 중단 사용 하 여 플레이어가 보고할 수 **FairPlayKillsTeammates.**

### <a name="title-has-community-kickvote-feature"></a>타이틀이 커뮤니티 시작/투표 기능
잘못 된 동작에 대 한 세션에서 제거 되 고 라운드에서 플레이어가 플레이어 투표 되었습니다 했습니다. 제목 세션에서 해당 플레이어를 제거 하는 경우 사용자에 게 보고할 수 **FairPlayKicked.**

### <a name="helpful-player-community-vote"></a>유용한 플레이어 커뮤니티 응답
게임의 몇 가지 라운드 후 제목 가장에는 사용자를 선택 하는 옵션을 제공 합니다. 사용 하 여 해당 동작을 보고할 수이 작업을 인식 하는 타이틀 **PositiveHelpfulPlayer.**

### <a name="high-quality-ugc-user-generated-content"></a>높은 품질 UGC (사용자가 생성 콘텐츠)
타이틀에 장면 플레이어는 수단에 대 한 최고의 디자인을 선택할 수 있습니다. 사용 하 여 해당 동작을 보고할 수이 작업을 인식 하는 타이틀 **PositiveHighQualityUGC.**

### <a name="skilled-player"></a>숙련 된 플레이어
게임의 몇 가지 라운드 후 제목 최상의 플레이어가 사용자는 MVP 있는 사람을 선택 하는 옵션을 제공 합니다. 타이틀은 플레이어가 consistenly 사항도 보고할 수 MVP 상태 사용 하 여 해당 동작을 볼 때 **PositiveSkilledPlayer.**

### <a name="user-reports-questionable-ugc-in-title"></a>사용자가 제목에서 의심 스러운 UGC 보고
타이틀은 플레이어 차량의 디자인 볼 수 있는 장면을 있습니다. 플레이어 불쾌 한 디자인 통지 이며 보고할 하고자 합니다. 이 작업을 인식 하는 제목을 사용 하 여 원인일 보고할 수 **UserContentInappropriateUGC.**

### <a name="title-wants-to-request-an-xbl-ban-review-of-a-player-in-their-title"></a>제목은 타이틀에서 플레이어의 XBL Ban 리뷰를 요청 하려는
타이틀의 커뮤니티 관리자가 게임에서 문제를 일으키는 일관 되 게 신뢰도 낮은 플레이어를 발견 되었습니다. XBL 정책 및 사용 하 여 적용 팀 검토 제목을 요청할 수 있습니다 **FairPlayUserBanRequest.**

## <a name="complete-behavior-feedback-options"></a>전체 동작 피드백 옵션
아래 표에 피드백 유형 타이틀을 대신 하 여 사용자에 게 피드백을 제출 하는 데 사용할 수 있습니다. 평판 서비스는 유연 하며 이러한 타이틀의 요구를 충족 하지 않는 경우 새 피드백 유형 쉽게 추가할 수 있습니다. 알려주십시오 계정 관리자에 추가 된 새 피드백 형식을 표시 하려는 경우.

표 1: 다양 한 파트너 피드백 형식 평판 서비스 지원 합니다.

**Fairplay 피드백 유형**               | **설명**
----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------
FairplayKillsTeammates                    | 자신의 동료 중단 의도적으로 플레이어가 보고
FairplayCheater                           | 부정 확실히는 플레이어가 보고
FairplayTampering                         | 게임 디스크를 사용 하 여 meddled 확실히에 있거나 게임 소프트웨어 또는 하드웨어 그렇지 변조 플레이어를 보고 합니다.
FairplayUserBanRequest                    | 신원을 플레이어는 일시 중단 얻었습니다 보고
FairplayConsoleBanRequest                 | Xbox Live에 연결에서 차단 해야 하는 것으로 예상 되는 콘솔 보고서
FairplayUnsporting                        | Unsportsmanlike 사항 확실히 참여 하는 플레이어가 보고
FairplayIdler                             | 멀티 플레이 입력 플레이어가 일정 부분 일치 하지만 적극적으로 재생 되지 않으면 보고
FairplayLeaderboardCheater                | 표시는 순위표에서 높은 부인을 확실히는 플레이어가 보고
**통신 피드백 유형**         |
CommsInappropriateVideo                   | 화상 채팅에 해당 되는 플레이어가 보고
**사용자 생성 피드백 콘텐츠 형식** |
UserContentInappropriateUGC               | 타이틀에서 플레이어를 작성 하는 프로그램 부적절 한 콘텐츠의 보고
UserContentReviewRequest                  | XBLPET 팀은 검토할 수 있도록 사전 콘텐츠의 보고
UserContentReviewRequestBroadcast         | XBLPET 팀은 검토할 수 있도록 사전 브로드캐스트 보고
UserContentReviewRequestGameDVR           | XBLPET 팀은 검토할 수 있도록 사전 GameDVR 클립을 보고
UserContentReviewRequestScreenshot        | XBLPET 팀은 검토할 수 있도록 사전 스크린샷을 보고합니다
**양수 피드백**                     |
PositiveSkilledPlayer                     | 사용자가 MVP 결정에 응답할 수, 플레이어 양수 피드백에 대해서는 특정 때 숙련 된 플레이어 보고
PositiveHelpfulPlayer                     | 게임을 다른 유용한 했음을 보고 플레이어에 대 한 UI를 제공 하는 경우 도움이 플레이어가 보고
PositiveHighQualityUGC                    | 게임을 다른 사용자의 콘텐츠를 보완 하는 플레이어에 대 한 UI를 제공 하는 경우 콘텐츠를 긍정적인 보고

## <a name="feedback-api-calls"></a>피드백 API 호출
타이틀 평판 서비스 호출에 두 전략을 사용할 수 있습니다. 것이 좋습니다 집계에서 보고서 사용자에 게 파트너 서비스에서 인증을 위해 서비스 토큰을 사용 하 여 합니다. 타이틀은 클라이언트에서 직접 사용자를 보고할 수도 있습니다. 클라이언트 API에 필요한 사용자에 게 유효한 것으로 간주 여러 보고서에 내장 된 백신 사기 기술이 있습니다. 두 Api 일괄 처리 되며 여러 사용자를 동시에 보고할 수 있습니다.

제목 다음 Xbox Live Api를 사용 하 여 플레이어 평판 피드백을 제출 수 있습니다.

Language | API
-------- | --------------------------------------------------------------------------------------------
WinRT    | Microsoft::Xbox::Services::Social::ReputationService.SubmitBatchReputationFeedbackAsync(...)
C++      | xbox::services::social::reputation_service.submit_batch_reputation_feedback(...)

또는 제목 다음 직접 REST 메서드를 사용할 수 있습니다.:

API          | URL                                                      | 인증 요구 사항
------------ | -------------------------------------------------------- | ---------------------------------------
POST 서비스 | https://reputation.xboxlive.com/users/batchfeedback      | 파트너 및 샌드박스 클레임을 사용 하 여 S-토큰
클라이언트 게시물  | https://reputation.xboxlive.com/users/batchtitlefeedback | Xtoken 제목 및 샌드박스 클레임

## <a name="feedback-object"></a>피드백 개체
피드백 개체에 최신 버전은 101 다음과 같이 지정 합니다. 두 Api는 다음 개체의 일괄 처리를 기대합니다.

멤버       | 유형   | 설명
------------ | ------ | -----------------------------------------------------------------------------------------------------------------------------------------------------------------
sessionRef   | 개체 | MPSD 세션을 설명 하는 개체 또는 null이 피드백와 관련이 있습니다.
feedbackType | string | 피드백의 형식입니다. 가능한 값은 ReputationFeedbackType 열거형에 정의 된
textReason   | string | 사용자가 제공한 추가 텍스트를 보낸 사람에 게 설명 하는 이유는 피드백을 제출 되었습니다. 정책 적용 팀에 매우 유용 합니다.
evidenceId   | string | 예를 들어 비디오 파일 ID가 제출한 피드백의 증거로 사용할 수 있는 리소스의 게임 플레이 또는 활동 피드 항목 도중 기록 합니다.
titleID      | 문자열 | 재생 제목의; 제목 ID 토큰에 없는 경우에 필요 합니다.
targetXuid   | 문자열 | 보고 하는 플레이어의 XUID 합니다.

예제:

```json
POST https://reputation.xboxlive.com/users/batchtitlefeedback
{
    "items" :
    [
        {
            "targetXuid": "33445566778899",
            "titleId" : null,
            "sessionRef": {
  "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
  "templateName": "CaptureFlag5",
  "name": "Title56932",
           },
            "feedbackType": "FairPlayKillsTeammates",
            "textReason": "Title detected this player killing team members 19 times",
            "evidenceId": null
        }
    ]
}
```

## <a name="feedback-qa"></a>Q&a 피드백

### <a name="q-can-i-send-a-hint-to-the-system-to-help-with-humans-that-might-be-looking-at-the-player-report"></a>Q: 힌트 플레이어 보고서를 보게 될 발음에 도움이 되는 시스템에 보낼 수 있나요?
A: 예, 매우 유용 하 고! 실행자 궁극적으로 제출 피드백을 확인 하는 데 "textReason" 매개 변수를 사용 합니다. 예를 들어는 idler에 대 한 "받은 사용자 입력이 없으면이 플레이어의 게임의 첫 번째 5 초 후" 이라는 텍스트 이유를 추가할 수 있습니다. 텍스트 그렇기 XBLPET 적용 에이전트에 대 한 매우 유용할 수, 하므로 유용 하 고 설명 텍스트는 이유는 되었는지 확인 합니다.

### <a name="q-should-i-worry-about-how-often-i-send-in-feedback-on-a-user"></a>Q: 사용자에 대 한 피드백에 보낸 빈도 대 한 걱정 해야 하나요?
A: 타이틀은 사용자 피드백을 얻었습니다 있는지 확신할 때 신뢰도 서비스를 호출 해야 합니다. 시스템에 제목과 사용자 과도 하 게 저하 되지 않도록 방지 하기 위해 여러 안전 catch 합니다.

### <a name="q-can-i-adjust-the-weight-of-the-feedback-being-sent"></a>Q: 전송 되는 피드백의 두께 조정할 수 있나요?
A: 아니요, 신뢰도 서비스 피드백의 두께 결정 합니다. 항상 시스템을 개선할 수 있도록 가중치를 조정 하는 것입니다.

### <a name="q-can-i-undo-feedback-ive-sent-on-a-user"></a>Q: 사용자에서 보낸 피드백을 취소할 수 있나요?
A: 아니요, 피드백은 최종 합니다. 타이틀 버그가 및 잘못 된 피드백을 보내는 경우, 알려주세요 및에서는 됩니다 차단 목록에 지정 타이틀 버그를 해결 될 때까지 합니다.

### <a name="q-can-i-see-the-feedback-sent-for-my-title-from-users"></a>Q: 전송 사용자 로부터 내 제목에 대 한 피드백을 볼 수 있나요?
A: 해당 정보를 사용할 수 없는 현재 셀프 서비스. 계정에 요청할 수 있습니다 관리자 및 것을 나눌 수 제목 당 데이터 않았을 합니다. 곧을 직접 사용할 수 있도록 하 여 낮은 신뢰도 타이틀을 사용 하 여 사용자의 조합을 표시 하겠습니다.

### <a name="q-when-i-send-in-console-or-user-ban-review-request-how-do-i-know-what-happened"></a>Q: 때 콘솔에서 보냅니까 또는 사용자 ban 검토 요청 하는 방법 무슨 알 수 있나요?
A: XBL 정책 및 적용 팀에 리뷰에 대 한 정보 전송 현재 되지만 현재 있습니다 수행할 업데이트 되지 ban 검토 합니다.

### <a name="q-is-there-a-reputation-score-per-title"></a>Q: 제목 당 평판 점수는 무엇입니까?
A: 아니요. 현재 fairplay, 통신 및 사용자가 생성 한 콘텐츠 있지만 제목 단위가 아니라 평판에 대 한 하위 점수 있습니다. 이 기능은 나중에 충분 한 수요가 경우 하므로 수 있도록 해당 기능을 요청 하려는 경우 알아야 하는 고객 담당 관리자에 추가 될 수 있습니다.
