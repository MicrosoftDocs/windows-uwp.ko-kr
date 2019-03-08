---
title: Feedback(JSON)
assetID: 726117c1-f01b-18c0-3b75-a7a7d27d84a2
permalink: en-us/docs/xboxlive/rest/json-feedback.html
description: " Feedback(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9cc1cb4ecc12219d54af1c4ab420671f2bbfa81f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644768"
---
# <a name="feedback-json"></a>Feedback(JSON)
플레이어에 대 한 사용자 의견 정보를 포함합니다.
<a id="ID4EN"></a>


## <a name="feedback"></a>사용자 의견

피드백 개체에 다음과 같이 지정 합니다.

| 멤버| 형식| 설명|
| --- | --- | --- |
| sessionRef| object | 이 피드백 MPSD 세션을 설명 하는 개체 또는 null와 관련 됩니다. |
| feedbackType| 문자열 | 피드백의 형식입니다. 에 정의 된 가능한 값은 <b>Microsoft.Xbox.Services.Social.ReputationFeedbackType</b>합니다. |
| textReason| 문자열| 보낸 사람에 게 추가 피드백 이유를 설명 하는 사용자 제공 텍스트 제출 되었습니다. |
| voiceReasonId| 문자열| 피드백 이유를 설명 하려면 추가 보낸 사람이 있는 Kinect의 음성 사용자가 제공한 파일의 ID (Base-64)을 제출 합니다. |
| evidenceId| 문자열| 예를 들어, 비디오 파일을 전송할 피드백의 증명 정보로 사용할 수 있는 리소스의 ID, 게임 플레이 중 기록 합니다. |

<a id="ID4EVC"></a>


### <a name="feedback-types"></a>피드백 유형

"에서 보낸" 열 피드백을 전송할 수 있는 사용자를 나타냅니다.

   * "User" 의미를 XToken를 사용 하 여 인증에 대 한 콘솔에서 제출할 수 있습니다 따라서 API를 사용할 수 있습니다 **SubmitFeedback**합니다.
   * 클레임 인증서를 사용 하 여 파트너가 제출할 수 있습니다 "파트너"를 의미, 따라서 API를 사용할 수 있습니다 **SubmitBatchFeedback**합니다.
   * "개인 정보 보호" SLS 개인 서비스만 피드백을 보낼 수 있습니다 의미 합니다.
   * "None" 피드백 감사 SLS 평판 서비스에 의해 내부적으로 생성 되 고 모든 호출자에 게 보낼 수 없는 의미 합니다.

| 형식| 보낸| 참고|
| --- | --- | --- | --- | --- | --- |
| CommsAbusiveVoice| 사용자| 사용자가 보고서 부적절 한 음성 통신에서 제목을 내 및 Xbox 대시보드에서 사용자 의견 보내기 |
| CommsInappropriateVideo| 사용자, 파트너| 사용자 및 파트너 내의 제목 및 Xbox 대시보드에서 부적절 한 비디오를 보고 하는 피드백을 보냅니다. |
| CommsMuted| 개인 정보| 사용자 다른 플레이어 음을 소거, 개인 정보 보호이 피드백 reputation service로 보냅니다. |
| CommsPhishing| 사용자| 사용자는 피싱 메시지를 보고 하려면이 피드백을 보냅니다. |
| CommsPictureMessage| 사용자| 받은 편지함 서비스 그림의 통신을 기반으로 보낸 사람 평판을 업데이트 하 고 적용 팀에 피드백을 보고 하는 평판 서비스를 호출 합니다. |
| CommsSpam| 사용자| 사용자 스팸 메시지를 보고 하려면이 피드백을 보냅니다. |
| CommsTextMessage| 사용자| 받은 편지함 서비스에 보낸 사람 평판을 업데이트 하 고 적용 팀에 피드백을 보고 하는 평판 서비스를 호출 합니다. **참고:** 받은 편지함 UI를 사용자가 메시지에 플래그 지정 하도록 허용 하는 단추가 있어야 합니다. |
  | CommsVoiceMessage | 사용자 | 받은 편지함 서비스 음성 메시지의 통신을 기반으로 보낸 사람 평판을 업데이트 하 고 적용 팀에 피드백을 보고 하는 평판 서비스를 호출 합니다.  |
  | FairPlayBlock | 개인 정보 | 개인 정보는 사용자가 다른 플레이어 차단 하는 경우 reputation service로이 피드백을 보냅니다.  |
  | FairPlayCheater | 사용자, 파트너 | 사용자는 건 반 칙을 결정 하는 제목 사용자 개입 없이이 피드백을 보낼 수 있습니다.  |
  | FairPlayConsoleBanRequest | 파트너 | 파트너는 Xbox Live에서 콘솔을 금지 하려면 권장 사항으로이 피드백을 보냅니다.  |
  | FairPlayIdler | 사용자, 파트너 | 제목 경우 사용자는 유휴 의도적으로 게임에서 라운드 후 일반적으로 라운드를 결정 하는 사용자 개입 없이이 피드백을 보낼 수 있습니다.  |
  | FairPlayKicked | 사용자, 파트너 | 제목 (시작) 게임에서 사용자가 결 있는지 검색 하는 사용자 개입 없이이 피드백을 보낼 수 있습니다.  |
  | FairPlayKillsTeammates | 사용자, 파트너 | 제목 때 플레이어 killls 자동으로 결정할 수 있는 그의 팀 동료 사용자 개입 없이이 피드백을 보낼 수 있습니다.  |
  | FairPlayQuitter | 사용자, 파트너 | 사용자는 게임을 조기에 종료를 결정 하는 제목 사용자 개입 없이이 피드백을 보낼 수 있습니다.  |
  | FairPlayTampering | 사용자, 파트너 | 제목 사용자 디스크 콘텐츠가 변조를 결정 하는 사용자 개입 없이이 피드백을 보낼 수 있습니다.  |
  | FairPlayUnblock | 개인 정보 | 개인 정보 보호 사용자 다른 플레이어를 차단 해제 하는 경우 reputation service로이 피드백을 보냅니다.  |
  | FairPlayUserBanRequest | 파트너 | 파트너는 Xbox Live에서 사용자를 차단 하려면 권장 사항으로이 피드백을 보냅니다.  |
  | InternalAmbassadorScoreUpdated | 없음 | 호출자가 사용할 수 있도록 하지는 내부 피드백 형식입니다.  |
  | InternalReputationReset | 없음 | 호출자가 사용할 수 있도록 하지는 내부 피드백 형식입니다.  |
  | InternalReputationUpdated | 없음 | 호출자가 사용할 수 있도록 하지는 내부 피드백 형식입니다.  |
  | PositiveHelpfulPlayer | 사용자, 파트너 | 사용자 및 파트너 게임, 포럼 및 등의 유용한 동료 플레이어에 대 한 양의 정보를 제출 하려면이 피드백을 보냅니다.  |
  | PositiveHighQualityUGC | 사용자, 파트너 | 사용자 및 파트너이 피드백을 보낼이 제목, 게임 내에서 공유 UGC에 긍정적인 피드백을 제출 하는 사용자를 허용 해야 함을 나타내려면 예를 들어 Forza에서 설정을 조정.  |
  | PositiveSkilledPlayer | 사용자, 파트너 | 사용자 및 파트너 제목 MPSD 세션의 끝에 MVP에 투표할 수 있도록 할 수 있는지를 나타내기 위해이 피드백을 보냅니다.  |
  | UserContentGamerpic | 사용자 | 사용자가 게이머 카드 직접에서 부적절 한 게이머 그림을 보고이 피드백을 보냅니다.  |
  | UserContentGamertag | 사용자 | 사용자가 게이머 카드 직접에서 부적절 한 게이머 태그를 보고 하려면이 피드백을 보냅니다.  |
  | UserContentInappropriateUGC | 사용자, 파트너 | 사용자 및 파트너 제목 Forza에서 그리기 작업 예를 들어, 게임 내에서 부적절 한 공유 UGC 플래그를 설정 하려면 사용자를 허용 해야 함을 나타내려면이 피드백을 보냅니다.  |
  | UserContentPersonalInfo | 사용자 | 사용자이 의견을 소개 및 기타 개인 정보 게이머 카드에서 직접 보고서를 보냅니다.  |

<a id="ID4EFEAC"></a>


## <a name="sample-json-syntax"></a>JSON 구문 예제


```json
{
"sessionRef": {
  "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
  "templateName": "CaptureFlag5",
  "name": "Halo556932",
  },
  "feedbackType": "CommsAbusiveVoice",
  "textReason": "He called me a doodoo-head!",
  "voiceReasonId": null,
  "evidenceId": null
}

```


<a id="ID4EOEAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EQEAC"></a>


##### <a name="parent"></a>Parent

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)
