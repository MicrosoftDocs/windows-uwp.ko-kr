---
title: 타이틀에서 플레이어 피드백 보내기
author: KevinAsgari
description: 어떻게 타이틀 신뢰도 Xbox Live 서비스에 플레이어 피드백을 전송 하 여 플레이어 긍정적인 경험을 홍보 하기 수에 대해 알아봅니다.
ms.assetid: 49f8eb44-1e31-4248-b645-9123df6f8689
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 평판, 플레이어 피드백
ms.localizationpriority: medium
ms.openlocfilehash: 6a0d6693fb1d97a408a8b6559bb7c317a0af0030
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5812939"
---
# <a name="sending-player-feedback-from-your-title"></a>타이틀에서 플레이어 피드백 보내기
대부분의 Xbox Live 구성원은 훌륭한 하지만 극히 일부만 다른 사람의 게임 환경을 저하는 "잘못 된 사과". 사용자를 통해 사용자의 작은 이러한 비율을 식별 하 고 제출한 피드백 제목입니다. "잘못 된 사과" 수 있도록 제한 멀티 플레이어 경험 수 없는 방해가 있는 좋은 플레이어가 게임에서 인구의 나머지 부분을 보호는 데 도움이 됩니다. Xbox 사용자 시스템을 정확 하 게 유지 하 게 보고 크게 의존 있으며 타이틀에 Xbox One 직접 참여할 수 있습니다 크게 사용자층 신뢰도의 정확도 향상 시키는 데 도움이 됩니다.

## <a name="steps-to-submit-feedback-from-title-or-title-service"></a>제목이 나 제목 서비스에서 피드백을 제출 하는 단계
1. 피드백 순간 제목 또는 제목 서비스에 추가
2. 올바른 피드백 유형 확인
3. 피드백을 사용 하 여 신뢰도 피드백 Api 호출

### <a name="adding-feedback-moments-to-title-or-title-service"></a>피드백 순간 제목이 나 제목 서비스 추가
게이머 자신의 면, 방금 적극적으로 재생 하지 않고 주위 돋보이게 플레이어 또는 사용자가 게임을 파괴 된 cheaters 파괴 하는 동료와 잘못 된 환경을 했습니다. 이러한 문제가 있는 플레이어를 직접 보고를 사용 하면 Xbox Live 하지만 사용자 피드백 완벽 하지 않습니다. 타이틀 간단한 작업 플레이어가 게임에서 유휴을 조기에 종료 및 부정 행위는 사람을 결정 자기 수를 쉽게 확인할 수 있습니다. 타이틀에서 다양 한 좋은 모든 플레이어의 환경을 개선에 도움이 되는 피드백 순간 피드백을 제출할 수 있습니다.

### <a name="determining-the-correct-feedback-type"></a>올바른 피드백 유형 확인
신뢰도 시스템에 사용자 피드백을 보증 하는 다양 한 방법을 캡처를 위한 많은 피드백 유형이 있습니다. 아래 표 1에는 나열 완벽 하 게 됩니다. 부정적는 대부분 하지만 양수 피드백도를 사용 하 여 사용자의 평판을 개선 하기 위해 가능 합니다.

Xbox 시스템 UI는 플레이어가 게임에서 다른 사용자에 게 피드백을 제출 하는 방법을 제공 합니다. 해당 사용자 간 피드백 수행 하지 않습니다 훨씬 간단한 사용자는 griefing 이후 다른 때 사라집니다. 타이틀 해당 시스템 UI를 직접, 피드백을 제출 하려면 사용자에 대 한 UI를 제공를 보완할 수 있지만 대신 제목 피드백을 제출할 수 있는지 제목 자체를 대신 하 여 파트너 피드백을 사용 하 여 선호 하. 파트너 피드백은 매우 신뢰할 수 있는 합니다.

## <a name="example-partner-feedback-usage-scenarios"></a>예제 파트너 피드백 사용 시나리오

### <a name="user-quitting-a-game-in-the-middle-of-a-match"></a>사용자가 일치 하는 중에 게임을 종료 합니다.
플레이어 게임을 잃는 및 자신의 동료 abandoning 게임의 메뉴는 게임 종료를 사용 합니다. 사용 하 여 동작을 보고할 수 제목을이 동작을 감지 하는 경우 **FairPlayQuitter.**

### <a name="idling-after-match-found-in-game"></a>게임에 일치 유휴
플레이어 가져옵니다 재생 하는 데 다른 플레이어와 일치 하지만 팀에 도움이 되는 대신 게임에서 스탠드 유휴 일관 되 게 합니다. 제목을 사용 하 여 플레이어 동작에 보고할 수 있습니다 **FairplayIdler.**

### <a name="killing-teammates-in-game"></a>게임에서 중단 동료
게임에서 플레이어 재미를 위한 동료 중단 지속적으로 됩니다. 것 팀 중단을 사용 하 여 플레이어를 보고할 수 제목을 지속적으로 사용자가 감지 **FairPlayKillsTeammates.**

### <a name="title-has-community-kickvote-feature"></a>타이틀이 커뮤니티 시작/투표 기능
잘못 된 동작에 대 한 세션에서 제거할 라운드에서 플레이어가 플레이어 투표 되었습니다 했습니다. 제목 세션에서 해당 플레이어를 제거 하는 경우 사용자에 게 보고할 수 **FairPlayKicked.**

### <a name="helpful-player-community-vote"></a>유용한 플레이어 커뮤니티 응답
게임의 몇 가지 라운드 후 제목 가장 도움을 주었습니다 사람을 선택 하는 옵션을 제공 합니다. 사용 하 여 동작을 보고할 수이 작업을 인식 하는 타이틀 **PositiveHelpfulPlayer.**

### <a name="high-quality-ugc-user-generated-content"></a>높은 품질 UGC (사용자가 생성 콘텐츠)
타이틀은 플레이어 수단에 대 한 최고의 디자인을 선택할 수 있는 장면을 있습니다. 사용 하 여 동작을 보고할 수이 작업을 인식 하는 타이틀 **PositiveHighQualityUGC.**

### <a name="skilled-player"></a>숙련 된 플레이어
게임의 몇 가지 라운드 후 제목 최상의 플레이어 사용자는 MVP 있는 사람을 선택 하는 옵션을 제공 합니다. 제목을 표시는 플레이어가 consistenly 사항도 보고할 수 MVP 상태를 사용 하 여 동작 하는 경우 **PositiveSkilledPlayer.**

### <a name="user-reports-questionable-ugc-in-title"></a>사용자가 제목에서 의심 스러운 UGC를 보고합니다.
타이틀에 장면 플레이어 자동차 디자인 볼 수 있습니다. 플레이어는 불쾌 디자인 공지 및 보고 하려는 합니다. 이 작업을 인식 하는 제목을 사용 하 여 다르기 보고할 수 **UserContentInappropriateUGC.**

### <a name="title-wants-to-request-an-xbl-ban-review-of-a-player-in-their-title"></a>제목은 타이틀에서 플레이어의 XBL Ban 리뷰를 요청 하려는
타이틀의 커뮤니티 관리자 허브가 게임에 문제를 일으키는 일관 되 게 신뢰도 낮은 플레이어를 발견 되었습니다. XBL 정책 및 사용 하 여 적용 팀 검토 제목을 요청할 수 있습니다 **FairPlayUserBanRequest.**

## <a name="complete-behavior-feedback-options"></a>전체 동작 피드백 옵션
아래 표에 피드백 형식을 타이틀을 대신 하 여 사용자에 게 피드백을 제출 하는 데 사용할 수 있습니다. 신뢰도 서비스는 유연 하 고 타이틀의 요구를 충족 하지 않는 이러한 생각 되 면 새 피드백 유형 쉽게 추가할 수 있습니다. 알려주십시오 계정 관리자 추가 된 새 피드백 형식을 표시 하려는 경우.

표 1: 다양 한 파트너 피드백 형식 평판 서비스 지원 합니다.

**Fairplay 피드백 유형**               | **설명**
----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------
FairplayKillsTeammates                    | 자신의 동료 중단 의도적으로 플레이어가 보고
FairplayCheater                           | 보고서는 물론 부정 하는 플레이어
FairplayTampering                         | 그렇지 않으면 게임에서 소프트웨어 또는 하드웨어를 사용 하 여 변조 있거나 게임 디스크를 사용 하 여 meddled 확실히가 플레이어를 보고 합니다.
FairplayUserBanRequest                    | 신원을 플레이어를 일시 중단 얻었습니다 보고
FairplayConsoleBanRequest                 | Xbox Live에 연결에서 차단 해야 하는 것으로 예상 되는 콘솔 보고서
FairplayUnsporting                        | Unsportsmanlike 사항 확실히 참여 하는 플레이어가 보고
FairplayIdler                             | 멀티 플레이어 입력 플레이어가 일정 부분 일치 하지만 적극적으로 재생 되지 않는 보고
FairplayLeaderboardCheater                | 표시는 순위표 높은 부인을 확실히가 있는 플레이어를 보고 합니다.
**통신 피드백 유형**         |
CommsInappropriateVideo                   | 화상 채팅에 해당 되는 플레이어를 보고 합니다.
**사용자 생성 피드백 콘텐츠 형식** |
UserContentInappropriateUGC               | 타이틀에서 플레이어를 작성 하는 프로그램 부적절 한 콘텐츠의 보고
UserContentReviewRequest                  | XBLPET 팀은 검토할 수 있도록 사전 콘텐츠의 보고
UserContentReviewRequestBroadcast         | XBLPET 팀은 검토할 수 있도록 사전 브로드캐스트를 보고
UserContentReviewRequestGameDVR           | XBLPET 팀은 검토할 수 있도록 사전 GameDVR 클립을 보고
UserContentReviewRequestScreenshot        | XBLPET 팀은 검토할 수 있도록 사전 스크린샷을 보고합니다
**양수 피드백**                     |
PositiveSkilledPlayer                     | MVP 확인 하려면 사용자에 게 응답할 수, 플레이어 양수 피드백에 대해서는 특정 때 숙련 된 플레이어를 보고
PositiveHelpfulPlayer                     | 게임을 다른 유용한 했음을 보고 플레이어에 대 한 UI를 제공 하는 경우 유용한 플레이어가 보고
PositiveHighQualityUGC                    | 다른 사용자의 콘텐츠를 보완 하도록 플레이어에 대 한 UI를 제공 하는 게임을 하는 경우 콘텐츠를 긍정적인 보고

## <a name="feedback-api-calls"></a>피드백 API 호출
타이틀은 신뢰도 서비스 호출을 두 가지 전략을 사용할 수 있습니다. 하는 기본 방법은 집계에서 보고서 사용자에 게 서비스 토큰을 사용 하 여 인증을 위해에서 파트너 서비스입니다. 타이틀은 클라이언트에서 직접 사용자를 보고할 수도 있습니다. 클라이언트 API에 여러 보고서 사용자에 게 유효한 것으로 간주 해야 하는 기본 제공 사기 방지 기술이 있습니다. 두 Api 모두 일괄 처리 되며 동시에 여러 사용자를 보고할 수 있습니다.

제목 다음 Xbox Live Api를 사용 하 여 플레이어 평판 피드백을 제출 수 있습니다.

Language | API
-------- | --------------------------------------------------------------------------------------------
WinRT    | Microsoft::Xbox::Services::Social::ReputationService.SubmitBatchReputationFeedbackAsync(...)
C++      | xbox::services::social::reputation_service.submit_batch_reputation_feedback(...)

또는 제목 다음 직접 REST 메서드를 사용할 수 있습니다.

API          | URL                                                      | 인증 요구 사항
------------ | -------------------------------------------------------- | ---------------------------------------
POST 서비스 | https://reputation.xboxlive.com/users/batchfeedback      | 파트너와 샌드박스 클레임을 사용 하 여 S-토큰
클라이언트 게시물  | https://reputation.xboxlive.com/users/batchtitlefeedback | Xtoken 제목 및 샌드박스 클레임

## <a name="feedback-object"></a>피드백 개체
피드백 개체에는 최신 버전은 101 다음 사양을 있습니다. 두 Api는 다음 개체의 일괄 처리를 기대합니다.

멤버       | 유형   | 설명
------------ | ------ | -----------------------------------------------------------------------------------------------------------------------------------------------------------------
sessionRef   | 개체 | MPSD 세션을 설명 하는 개체 또는 null이 피드백와 관련이 있습니다.
feedbackType | string | 피드백의 형식입니다. 가능한 값은 ReputationFeedbackType 열거형에 정의
textReason   | string | 보낸 추가 피드백 이유를 설명 하는 사용자가 제공한 텍스트 제출 되었습니다. 정책 적용 팀에 매우 유용 합니다.
evidenceId   | string | 예를 들어 비디오 파일 전송 되는 피드백의 증거로 사용할 수 있는 리소스의 ID를 게임 플레이 또는 활동 피드 항목 중 기록 합니다.
titleID      | 문자열 | 재생 제목의 제목 ID 토큰에 없는 경우에 필요 합니다.
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

## <a name="feedback-qa"></a>피드백 q&a

### <a name="q-can-i-send-a-hint-to-the-system-to-help-with-humans-that-might-be-looking-at-the-player-report"></a>Q: 플레이어 보고서를 보게 될 발음에 도움이 되는 시스템에 대 한 힌트를 보낼 수 있습니까?
A: 예, 매우 유용 하 고! "TextReason" 매개 변수를 사용 하 여 실행자가 제출한 피드백 궁극적으로 확인 하는 데 도움이 됩니다. 예를 들어 있는 idler에 "받은 사용자 입력이 없으면이 플레이어의 게임의 첫 번째 5 초 후" 라고 표시 되는 텍스트 이유를 추가할 수 있습니다. 텍스트 그렇기 XBLPET 적용 에이전트 매우 유용할 수, 유용 하 고 설명 텍스트는 이유는 보장 수 있습니다.

### <a name="q-should-i-worry-about-how-often-i-send-in-feedback-on-a-user"></a>Q: 사용자에 대 한 피드백에 보낸 빈도 대 한 걱정 해야 하나요?
A: 타이틀은 사용자 피드백을 얻었습니다는 품질을 확인할 때 신뢰도 서비스를 호출 해야 합니다. 시스템에 제목과 사용자 과도 하 게 저하 되지 않도록 방지 하기 위해 여러 안전 catch 합니다.

### <a name="q-can-i-adjust-the-weight-of-the-feedback-being-sent"></a>Q: 전송 되는 피드백의 두께 조정할 수 있나요?
A: 아니요, 신뢰도 서비스 피드백의 두께 결정 합니다. 항상 시스템을 개선할 수 있도록 가중치를 조정 하는 것입니다.

### <a name="q-can-i-undo-feedback-ive-sent-on-a-user"></a>Q: 사용자에 보낸 피드백을 취소할 수 있나요?
A: 아니요, 피드백은 최종 합니다. 타이틀 버그가 및 잘못 된 피드백을 보내는 생각 되는 경우, 알려주세요 및 우리 됩니다 차단 목록에 지정 타이틀 버그를 해결 될 때까지 합니다.

### <a name="q-can-i-see-the-feedback-sent-for-my-title-from-users"></a>Q: 전송 사용자 로부터 내 제목에 대 한 피드백을 볼 수 있나요?
A: 현재 해당 정보를 사용할 수 없는 셀프 서비스. 계좌를 요청할 수 관리자 및 것을 나눌 수 제목 당 데이터 않았을 합니다. 곧, 직접 사용할 수 있도록 하 낮은 신뢰도 타이틀을 사용 하 여 사용자의 조합을 표시 하겠습니다.

### <a name="q-when-i-send-in-console-or-user-ban-review-request-how-do-i-know-what-happened"></a>Q: I 콘솔에서 보내거나 때 사용자 ban 검토 요청 하는 방법 무슨 일이 알 수 있나요?
A: XBL 정책 및 적용 팀 리뷰에 대 한 정보가 전송 되어 현재 하지만 현재 ban 검토 하면 하지 업데이트 작업을 수행 합니다.

### <a name="q-is-there-a-reputation-score-per-title"></a>Q: 제목 당 평판 점수는 무엇입니까?
A: 아니요. 현재 fairplay, 통신 및 콘텐츠 사용자 생성 되지만 제목 단위가 아니라 평판에 대 한 하위 점수 있습니다. 이 기능은 나중에 충분 한 수요가 경우 하므로 수 있도록 해당 기능을 요청 하려는 경우 알아야 하는 고객 담당 관리자에 추가 될 수 있습니다.
