---
title: Feedback(JSON)
assetID: 726117c1-f01b-18c0-3b75-a7a7d27d84a2
permalink: en-us/docs/xboxlive/rest/json-feedback.html
author: KevinAsgari
description: " Feedback(JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e9ef3bb19155199ae94b18b828fb40eb7a0a2ce6
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4418037"
---
# <a name="feedback-json"></a>Feedback(JSON)
플레이어에 대 한 피드백 정보를 포함합니다.
<a id="ID4EN"></a>


## <a name="feedback"></a>피드백

피드백 개체에는 다음 사양을 있습니다.

| 멤버| 유형| 설명|
| --- | --- | --- |
| sessionRef| 개체 | MPSD 세션을 설명 하는 개체 또는 null이 피드백와 관련이 있습니다. |
| feedbackType| string | 피드백 유형입니다. 가능한 값은 <b>Microsoft.Xbox.Services.Social.ReputationFeedbackType</b>에서 정의 됩니다. |
| textReason| string| 보낸 추가 피드백 이유를 설명 하는 사용자가 제공한 텍스트 제출 되었습니다. |
| voiceReasonId| string| 추가 피드백 이유를 설명 하기 위해 보낸 사람은 Kinect에서 사용자가 제공한 음성 파일의 ID (Base 64)을 제출 합니다. |
| evidenceId| string| 예를 들어 비디오 파일 ID가 제출한 피드백의 증거로 사용할 수 있는 리소스의 게임 플레이 하는 동안 기록 합니다. |

<a id="ID4EVC"></a>


### <a name="feedback-types"></a>피드백 유형

"보낸" 열 피드백을 전송할 수 있는 사용자를 나타냅니다.

   * "사용자" 의미 제출할 수 따라서는 XToken 인증을 사용 하는 콘솔에서 API **SubmitFeedback**수락할 수 있습니다.
   * "파트너" 의미 제출할 수 따라서 클레임 인증서를 사용 하 여 파트너가 API **SubmitBatchFeedback**수락할 수 있습니다.
   * "개인" 의미 SLS 개인 정보 보호 서비스에만 피드백을 보낼 수 있습니다.
   * "None" 의미 피드백 감사 SLS 신뢰도 서비스에서 내부적으로 생성 되 고 모든 호출자를 보낼 수 없습니다.

| 유형| 보낸| 메모|
| --- | --- | --- | --- | --- | --- |
| CommsAbusiveVoice| 사용자| 사용자에 게 보고서 부적절 한 음성 통신에서 Xbox 대시보드 및 제목 내 피드백 보내기. |
| CommsInappropriateVideo| 사용자, 파트너| 사용자 및 파트너 Xbox 대시보드 및 제목 내에서 부적절 한 비디오를 보고 하는 피드백을 보냅니다. |
| CommsMuted| Privacy| 사용자는 다른 플레이어 음소거, 개인 정보가이 피드백 신뢰도 서비스를 보냅니다. |
| CommsPhishing| 사용자| 사용자가 피싱 메시지를 보고 하려면이 피드백을 보냅니다. |
| CommsPictureMessage| 사용자| 받은 편지함 서비스 그림의 통신에 따라 발신자의 신뢰도 업데이트 하 고 적용 팀에 피드백을 보고 평판 서비스를 호출 합니다. |
| CommsSpam| 사용자| 사용자가이 피드백 스팸 메시지를 보고 합니다. |
| CommsTextMessage| 사용자| 받은 편지함 서비스는 발신자의 신뢰도 업데이트 하 고 적용 팀에 피드백을 보고 있는 신뢰도 서비스를 호출 합니다. **참고:** 받은 편지함 UI는 사용자가 메시지에 플래그 지정 하도록 허용 하는 단추가 있어야 합니다. |
  | CommsVoiceMessage | 사용자 | 받은 편지함 서비스 음성 메시지의 통신에 따라 발신자의 신뢰도 업데이트 하 고 적용 팀에 피드백을 보고 평판 서비스를 호출 합니다.  |
  | FairPlayBlock | Privacy | 개인 정보 사용자 차단 다른 플레이어 평판 서비스에이 피드백을 보냅니다.  |
  | FairPlayCheater | 사용자, 파트너 | 타이틀 사용자 부정가 결정 하는 사용자 개입 없이이 피드백을 보낼 수 있습니다.  |
  | FairPlayConsoleBanRequest | 파트너 | 파트너는 콘솔에서 Xbox Live 사용을 금지 하 권장 사항으로이 피드백을 보냅니다.  |
  | FairPlayIdler | 사용자, 파트너 | 타이틀 경우 사용자가 대표 유휴 의도적으로 반올림, 후 라운드 일반적으로 게임을 결정 하는 사용자 개입 없이이 피드백을 보낼 수 있습니다.  |
  | FairPlayKicked | 사용자, 파트너 | 제목 (시작) 게임에서 사용자가 투표를 검색 하는 사용자 개입 없이이 피드백을 보낼 수 있습니다.  |
  | FairPlayKillsTeammates | 사용자, 파트너 | 플레이어 killls 때 자동으로 확인할 수 있는 타이틀 자신의 팀 동료 사용자 개입 없이이 피드백을 보낼 수 있습니다.  |
  | FairPlayQuitter | 사용자, 파트너 | 사용자가 게임을 조기에 종료를 결정 하는 타이틀 사용자 개입 없이이 피드백을 보낼 수 있습니다.  |
  | FairPlayTampering | 사용자, 파트너 | 디스크에 콘텐츠를 사용 하 여 사용자를 변조를 결정 하는 타이틀 사용자 개입 없이이 피드백을 보낼 수 있습니다.  |
  | FairPlayUnblock | Privacy | 개인은 사용자는 다른 플레이어를 차단 하는 경우 평판 서비스에이 피드백을 보냅니다.  |
  | FairPlayUserBanRequest | 파트너 | 파트너는 Xbox Live에서 사용자를 금지 하 권장 사항으로이 피드백을 보냅니다.  |
  | InternalAmbassadorScoreUpdated | 없음 | 호출자가 사용에 대 한 하지는 내부 피드백 유형입니다.  |
  | InternalReputationReset | 없음 | 호출자가 사용에 대 한 하지는 내부 피드백 유형입니다.  |
  | InternalReputationUpdated | 없음 | 호출자가 사용에 대 한 하지는 내부 피드백 유형입니다.  |
  | PositiveHelpfulPlayer | 사용자, 파트너 | 사용자 및 파트너 게임, 포럼, 등에 내에서 유용한 동료 플레이어에 대 한 양의 정보를 제출 하려면이 피드백을 보냅니다.  |
  | PositiveHighQualityUGC | 사용자, 파트너 | 사용자 및 파트너가이 피드백을 보내기 타이틀에는 게임 내에서 공유 UGC 긍정적인 피드백을 제출 하는 사용자를 허용 해야 나타내는 예를 들어 Forza에서 설치를 조정 합니다.  |
  | PositiveSkilledPlayer | 사용자, 파트너 | 사용자 및 파트너 타이틀 MPSD 세션의 끝에 MVP에 응답할 수 있도록 할 수는 나타내기 위해이 피드백을 보냅니다.  |
  | UserContentGamerpic | 사용자 | 사용자는 부적절 한 게이머 사진 게이머 카드에서 직접 보고이 피드백을 보냅니다.  |
  | UserContentGamertag | 사용자 | 사용자가 보고 게이머 카드에서 직접 부적절 한 게이머 태그는이 피드백을 보냅니다.  |
  | UserContentInappropriateUGC | 사용자, 파트너 | 사용자 및 파트너 타이틀 Forza에서 그림판 작업 예를 들어 게임 내에서 부적절 한 공유 UGC 플래그 사용자를 허용 해야 나타내기 위해이 피드백을 보냅니다.  |
  | UserContentPersonalInfo | 사용자 | 사용자가 한 소개 및 게이머 카드에서 직접 개인 정보를 보고 하려면이 피드백을 보냅니다.  |

<a id="ID4EFEAC"></a>


## <a name="sample-json-syntax"></a>샘플 JSON 구문


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


##### <a name="parent"></a>부모

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)