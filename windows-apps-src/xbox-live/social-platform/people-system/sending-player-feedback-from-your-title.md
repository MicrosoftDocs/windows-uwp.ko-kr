---
title: 제목에서 플레이어 사용자 의견 보내기
description: 제목 수 Xbox Live reputation service로 플레이어 피드백을 전송 하 여 양수 플레이어 환경을 촉진 하는 방법에 대해 알아봅니다.
ms.assetid: 49f8eb44-1e31-4248-b645-9123df6f8689
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 평판, 플레이어 피드백
ms.localizationpriority: medium
ms.openlocfilehash: 3e8d82fc9b195f174bf7a46a8d21fb20b2df9551
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592238"
---
# <a name="sending-player-feedback-from-your-title"></a>제목에서 플레이어 사용자 의견 보내기
Xbox Live 멤버의 대부분은 훌륭한 되지만 다른 사람의 게임 환경을 저하는 "잘못 된 Apples"의 작은 비율입니다. 사용자를 통해 사용자의 이러한 작은 비율을 식별 하 고 제출한 피드백 제목입니다. 우리는 "잘못 된 Apples" 하 게 제한 된 멀티 플레이어 좋은 플레이어의 게임에 지장이 없습니다 여기서 함으로써 모집단의 나머지를 보호할 수 있습니다. Xbox 정확 하 고, 시스템을 유지 하려면 다른 사용자를 보고 하는 사용자에 크게 의존 하지만 타이틀 Xbox One에 직접 참여를 크게 사용자 평판 모집단의 정확도 높이 데 도움이 합니다.

## <a name="steps-to-submit-feedback-from-title-or-title-service"></a>제목 또는 제목 서비스 사용자 의견을 제출 하는 단계
1. 제목 또는 제목 서비스 피드백 분 추가
2. 올바른 피드백 유형을 결정합니다
3. 피드백을 사용 하 여 평판 피드백 Api 호출

### <a name="adding-feedback-moments-to-title-or-title-service"></a>제목 또는 제목 서비스 피드백 분 추가
모든 게이머 자체 면, 오디오가 재생 하는 대신 가만히 주위 플레이어 또는 게임을 손상 하는 cheaters 만지고는 동료는 나쁜 경험 했습니다. Xbox Live를 사용 하면 이러한 문제가 플레이어를 직접 보고 하지만 사용자에 게 피드백은 완벽 하지 않습니다. 제목 간단한 것 플레이어 게임에서 유휴을 조기에 종료 등의 부정 행위는 사용자가 결정 되기도 수를 쉽게 확인할 수 있습니다. 제목 다양 한 모든 좋은 플레이어 경험 개선에 도움이 되도록 피드백 분에서에서 피드백을 제출할 수 있습니다.

### <a name="determining-the-correct-feedback-type"></a>올바른 피드백 유형 결정
평판 시스템에 사용자 피드백을 보증 하는 다양 한 방법을 캡처를 위한 많은 피드백 유형이 있습니다. 아래 표 1에서는 완벽 하 게 나열 됩니다. 그 중 대부분은 음수 이지만 긍정적인 피드백을 사용 하 여 사용자의 평판을 향상 시킬 수 있습니다.

Xbox 시스템 UI 플레이어의 게임 내 다른 사용자에 대 한 피드백을 제출 하는 방법을 제공 합니다. 사용자-사용자 피드백 수행 하지 않습니다 가중치, 사용자 griefing 발생 하기 쉬운 되므로 다른 사용자가 수 없게 하는 경우. 제목 UI를 직접 차례로에 대 한 피드백을 전송 하는 사용자에 대 한 UI를 제공 하는 시스템을 보완할 수 있지만 대신 것이 좋습니다는 타이틀 피드백 제출 자체 제목을 대신 하 여 파트너 의견을 사용 하 여 합니다. 파트너 의견을 매우 신뢰할 수 있는 합니다.

## <a name="example-partner-feedback-usage-scenarios"></a>파트너 사용자 의견 사용 예시 시나리오

### <a name="user-quitting-a-game-in-the-middle-of-a-match"></a>일치 하는 중에 게임을 종료 하는 사용자
플레이어가 게임을 잃는를 사용 하 여 게임의 메뉴 게임을 종료 그녀의 팀 동료를 중단 합니다. 사용 하 여 문제를 보고할 수 제목을이 동작을 감지 하면 **FairPlayQuitter 합니다.**

### <a name="idling-after-match-found-in-game"></a>게임에서 찾은 일치 항목 후 유휴
플레이어 가져옵니다을 재생 하려면 다른 플레이어와 일치 하지만 팀을 지원 하는 대신 게임에서은 유휴 일관 되 게 합니다. 제목을 사용 하 여 플레이어 행동을 보고할 수 **FairplayIdler 합니다.**

### <a name="killing-teammates-in-game"></a>게임에서 중지 팀원이
게임에서 플레이어는 지속적으로 종료 하는 중 재미 있는 작업에 대 한 팀 동료입니다. 해당 팀 중지 사용 하 여 플레이어를 보고할 수 제목을 사용자 일관 되 게 임을 감지할 때 **FairPlayKillsTeammates 합니다.**

### <a name="title-has-community-kickvote-feature"></a>제목에 커뮤니티 투표 시작 하는 기능
플레이어가는 라운드에서 잘못 된 동작에 대 한 세션에서 제거할 플레이어가 투표 했습니다. 제목 세션에서 해당 플레이어를 제거 하는 경우 사용 하 여 사용자를 보고할 수 있어 **FairPlayKicked 합니다.**

### <a name="helpful-player-community-vote"></a>유용한 플레이어 커뮤니티 응답
게임의 몇 가지 반올림 후 제목 사용자를 가장 많이 선택 하는 옵션도 제공 합니다. 이 작업을 인식 하는 제목을 사용 하 여 동작을 보고할 수 있어 **PositiveHelpfulPlayer 합니다.**

### <a name="high-quality-ugc-user-generated-content"></a>높은 품질 UGC (사용자 생성 콘텐츠)
제목에 장면 플레이어는 수단에 대 한 최상의 디자인을 선택할 수 있습니다. 이 작업을 인식 하는 제목을 사용 하 여 동작을 보고할 수 있어 **PositiveHighQualityUGC 합니다.**

### <a name="skilled-player"></a>숙련 된 플레이어
게임의 몇 가지 반올림 후 제목 최상의 플레이어가 있는 MVP는 사람을 선택 하는 옵션도 제공 합니다. 제목을 표시는 플레이어 consistenly 수입이 MVP 상태를 보고할 수 있어 사용 하 여 동작 하는 경우 **PositiveSkilledPlayer 합니다.**

### <a name="user-reports-questionable-ugc-in-title"></a>보고서 제목에 의심 스러운 UGC 사용자
제목에 장면 플레이어는 차량 디자인을 볼 수 있습니다. 플레이어가는 불쾌감을 주는 디자인을 확인 하 고 보고 하려고 합니다. 이 작업을 인식 하는 제목을 사용 하 여 표시를 보고할 수 있어 **UserContentInappropriateUGC 합니다.**

### <a name="title-wants-to-request-an-xbl-ban-review-of-a-player-in-their-title"></a>제목 직함에 플레이어의 XBL 금지 검토를 요청 하려고 합니다.
제목을 커뮤니티 관리자 낮은 신뢰도 플레이어가 게임에서 일관 되 게 하는 데 문제가 발생 시키는 발견 되었습니다. 제목을 XBL 정책 및 사용 하 여 적용 팀 검토를 요청할 수 **FairPlayUserBanRequest 합니다.**

## <a name="complete-behavior-feedback-options"></a>완료 동작 의견 옵션
아래 표에 피드백 유형의 제목을 대신 하 여 사용자에 게 피드백을 제출 하는 데 사용할 수 있습니다. Reputation service 유연한 이며 이러한 타이틀의 요구를 충족 하지 않는 경우 새 피드백 유형을 쉽게 추가할 수 있습니다. 알려주십시오 계정 관리자에 게 추가 된 새 피드백 형식을 참조 하려는 경우.

표 1: 다양 한 파트너 의견 평판 서비스가 지 원하는 형식.

**Fairplay 피드백 유형**               | **설명**
----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------
FairplayKillsTeammates                    | 플레이어는 의도적으로 종료 하는 중 자신의 동료는 보고서
FairplayCheater                           | 보고서는 건 반 칙 확실히는 플레이어
FairplayTampering                         | 플레이어가 게임 디스크와 meddled 확실히에 또는 게임 소프트웨어 또는 하드웨어 변조 그렇지 않은 경우 보고
FairplayUserBanRequest                    | 신원을 플레이어가 일시 중단 획득 한 보고
FairplayConsoleBanRequest                 | Xbox Live에 연결할에서 차단 해야 하는 보고서 생각 하는 콘솔
FairplayUnsporting                        | Unsportsmanlike 사용 규정에 확실히 매력적인는 플레이어가 보고서
FairplayIdler                             | 멀티 플레이 들어가는 플레이어는 일치 하지만 적극적으로 재생 되지 않는 보고서
FairplayLeaderboardCheater                | Leaderboard 높은 표시할 편법 확실히에 플레이어를 보고 합니다.
**통신 피드백 유형**         |
CommsInappropriateVideo                   | 플레이어가 비디오 채팅에 해당 되는 보고서
**사용자 생성 콘텐츠 사용자 의견 유형** |
UserContentInappropriateUGC               | 제목에 만들어지는 플레이어는 부적절 한 콘텐츠의 보고서
UserContentReviewRequest                  | XBLPET 팀에서 검토 됩니다 있도록 콘텐츠를 사전에 보고
UserContentReviewRequestBroadcast         | XBLPET 팀에서 검토 됩니다 있도록 브로드캐스트를 사전에 보고
UserContentReviewRequestGameDVR           | XBLPET 팀에서 검토 됩니다 있도록 GameDVR 클립을 사전에 보고
UserContentReviewRequestScreenshot        | XBLPET 팀에서 검토 됩니다 있도록 스크린 샷을 사전에 보고
**긍정적인 피드백**                     |
PositiveSkilledPlayer                     | 사용자는 MVP 결정할 투표할 수, 숙련 된 플레이어 때 플레이어 긍정적인 피드백을 가치는 특정 보고
PositiveHelpfulPlayer                     | 다른 유용한 했음을 보고 하는 플레이어에 대 한 UI를 제공 하는 게임을 보고 유용한 플레이어
PositiveHighQualityUGC                    | 다른 사용자의 콘텐츠를 보완 하는 플레이어에 대 한 UI를 제공 하는 게임의 콘텐츠를 긍정적 보고

## <a name="feedback-api-calls"></a>피드백 API 호출
제목 평판 서비스를 호출 하려면 두 가지 전략을 사용할 수 있습니다. 것이 좋습니다 집계에서 보고서 사용자에 게 파트너 서비스에서 인증에 대 한 서비스 토큰을 사용 하 여 합니다. 제목에는 클라이언트에서 직접 사용자가 보고할 수도 있습니다. 클라이언트 API에 여러 보고서에 유효한 사용자를 요구 하는 기본 제공 부패 기술 두 Api 일괄 처리 되며 여러 사용자를 동시에 보고할 수 있습니다.

제목 다음 Xbox Live Api를 사용 하 여 플레이어 평판 피드백을 전송할 수 있습니다.

외국어 | API
-------- | --------------------------------------------------------------------------------------------
WinRT    | Microsoft::Xbox::Services::Social::ReputationService.SubmitBatchReputationFeedbackAsync(...)
C++      | xbox::services::social::reputation_service.submit_batch_reputation_feedback(...)

또는 제목 다음 직접 REST 메서드를 사용할 수 있습니다.

API          | URL                                                      | 인증 요구 사항
------------ | -------------------------------------------------------- | ---------------------------------------
서비스 게시 | https://reputation.xboxlive.com/users/batchfeedback      | 파트너 및 샌드박스 클레임을 사용 하 여 S-토큰
클라이언트 POST  | https://reputation.xboxlive.com/users/batchtitlefeedback | 제목 및 샌드박스 클레임과 Xtoken

## <a name="feedback-object"></a>피드백 개체
피드백 개체에 101 최신 버전의 경우 다음과 같이 지정 합니다. 두 Api는 다음 개체의 일괄 처리를 기대합니다.

멤버       | 형식   | 설명
------------ | ------ | -----------------------------------------------------------------------------------------------------------------------------------------------------------------
sessionRef   | object | 이 피드백 MPSD 세션을 설명 하는 개체 또는 null와 관련 됩니다.
feedbackType | 문자열 | 피드백의 형식입니다. 가능한 값은 ReputationFeedbackType 열거형에 정의 된
textReason   | 문자열 | 보낸 사람에 게 추가 피드백 이유를 설명 하는 사용자 제공 텍스트 제출 되었습니다. 정책 적용 팀에 매우 유용합니다.
evidenceId   | 문자열 | 예를 들어, 비디오 파일을 전송할 피드백의 증명 정보로 사용할 수 있는 리소스의 ID, 게임 플레이 또는 작업 피드 항목 중 기록 합니다.
titleID      | 문자열 | 재생 제목의 제목 ID 토큰에 없는 경우에 필요 합니다.
targetXuid   | 문자열 | 보고 하려는 플레이어의 XUID 합니다.

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

## <a name="feedback-qa"></a>질문 및 답변 피드백

### <a name="q-can-i-send-a-hint-to-the-system-to-help-with-humans-that-might-be-looking-at-the-player-report"></a>Q: 플레이어 보고서를 보게 될 하는 사람에 도움이 되도록 시스템에 힌트를 보낼 수 있습니까?
A: 예, 매우 유용한 것! 피드백 제출에 대해 살펴보겠습니다 궁극적으로 실행자를 "textReason" 매개 변수를 사용 합니다. 예를 들어는 idler에 대 한 "접수 없는 사용자 입력이이 플레이어의 게임의 첫 번째 5 초 후" 라는 텍스트 이유를 추가할 수 있습니다. 이 텍스트 이유 XBLPET 적용 에이전트에 대 한 매우 유용할 수, 있으므로 텍스트 이유 및 설명이 포함 된 유용한 정보 인지를 확인 수 있습니다.

### <a name="q-should-i-worry-about-how-often-i-send-in-feedback-on-a-user"></a>Q: 사용자에 대 한 피드백의 전송 빈도 대 한 걱정 해야?
A: 사용자가 피드백을 획득 했는지 확신할 때 제목 Reputation service를 호출 해야 합니다. 시스템에 제목 및 사용자가 과도 한 영향 사용자에 게 못하도록 방지 하기 위해 몇 가지 보안 catch 합니다.

### <a name="q-can-i-adjust-the-weight-of-the-feedback-being-sent"></a>Q: 전송 되는 피드백의 가중치를 조정할 수 있나요?
A: 아니요, Reputation service 피드백의 두께 결정 합니다. 항상 시스템을 개선 하는 가중치를 조정 하는 것입니다.

### <a name="q-can-i-undo-feedback-ive-sent-on-a-user"></a>Q: 사용자에 대해 보낸 피드백 실행 취소할 수 있습니까?
A: 아니요, 의견은 최종 합니다. 제목 버그가 및 잘못 된 사용자 의견 보내기는 생각 되 면, 알려주세요 및 버그를 수정 될 때까지 제목을 블랙 리스트 됩니다 것입니다.

### <a name="q-can-i-see-the-feedback-sent-for-my-title-from-users"></a>Q: 사용자의 내 제목에 대 한 전송 피드백을 볼 수 있나요?
A: 해당 정보를 사용할 수 없는 현재 셀프 서비스입니다. 계정을 요청할 수 있습니다 관리자를 공유할 수 있는 제목에 따라 데이터를 기준으로 권한이 있습니다. 곧을 직접 사용할 수 있도록 하 고 또한 제목을 사용 하 여 낮은 신뢰도 사용 하 여 다양 한 사용자를 표시 하고자 합니다.

### <a name="q-when-i-send-in-console-or-user-ban-review-request-how-do-i-know-what-happened"></a>Q: 콘솔에서 보낼 때나 사용자 금지 검토 요청 하는 방법에 무슨 알 수 있습니까?
A: XBL 정책과 적용 팀에 검토에 대 한 정보는 전송 하는 현재 되지만 현재 있습니다 업데이트 되지 금지 검토 합니다.

### <a name="q-is-there-a-reputation-score-per-title"></a>Q: 제목 당 신뢰도 점수는 무엇입니까?
A: 아니요. 현재 fairplay, 통신 및 사용자 생성 콘텐츠 있지만 제목 단위가 아니라 평판에 대 한 하위 점수는 있습니다. 이 기능은 나중에 수요가 충분 한 경우 수 있습니다 알고 있어야 해당 기능을 요청 하려는 경우 계정 관리자 추가할 수 있습니다.
