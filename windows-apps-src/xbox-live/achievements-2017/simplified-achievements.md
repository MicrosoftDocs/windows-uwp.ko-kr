---
title: 도전 과제 2017
description: 도전 과제 2017
ms.assetid: d424db04-328d-470c-81d3-5d4b82cb792f
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e7588639d9a83b50066e4dd313d6427d047c8482
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8931201"
---
# <a name="achievements-2017"></a>도전 과제 2017

도전 과제 2017 시스템 게임 개발자가 Xbox One, Windows 10, Windows 10 휴대폰, Android 및 iOS에서 새 Xbox Live 게임에 대 한 도전 과제를 잠금 해제를 직접 호출 모델을 사용할 수 있도록 합니다.

## <a name="introduction"></a>소개

Xbox One을 사용 하 여 간단 하 게 게임에서 원격 분석 이벤트를 전송 하 여 멀티 플레이어, 사용자 통계, 도전 과제, 다양 한 상태 등의 Xbox Live 기능에 대 한 데이터를 강화 하는 게임 개발자를 강화 하는 새 Cloud-Powered 도전 과제 시스템을 도입 했습니다. 이 다양 한 새로운 혜택 열렸습니다 – 단일 이벤트 여러 Xbox Live 기능;에 대 한 데이터를 업데이트할 수 있습니다. 대신 클라이언트, 서버에 거주 하 고 Xbox Live 구성 및 유도 합니다.

Xbox One 출시 이후 년에서 게임 개발자 피드백을 긴밀 하 게 수신할 수 있어야 하 고 개발자가 다음 공유 일관 되 게 합니다.

1.  **직접 호출 패턴을 통해 도전 과제를 잠금 해제를 요구 합니다.** 여러 개발자가 Xbox의 이전 버전을 비롯 한 다양 한 플랫폼에 대 한 게임 빌드하고 도전 과제와 같은 시스템 이러한 플랫폼에서 직접 호출한 메서드를 사용 합니다. Xbox One에 대 한 호출을 잠금 해제 직접 지원 및 해당 플랫폼 간 게임 개발 요구 사항 및 개발 시간 비용 다른 현재 생성 Xbox 플랫폼 쉽게 합니다.

2.  **구성 복잡성을 최소화 합니다.** 도전 과제의 Cloud-Powered 성과 시스템 논리를 잠금 해제는 서비스 사용자에 대 한 도전 과제를 잠금 해제 하는 시기와 타이틀의 통계 데이터를 해석 하는 방법을 알 수 있도록 Xbox Live에서 구성 해야 합니다. 이 작업은 이전에 존재 하지 않는 한 도전 과제 구성의 새 도전 과제 규칙 섹션을 통해 수행 되었습니다. 클라우드에서 논리를 잠금 해제 하지 매우 강력한 수 있지만, 디자인 및 도전 과제 타이틀의 생성에 복잡성이 추가 구성 요구 사항에 추가 합니다.

3.  **문제 해결 하기가 어렵습니다.** Cloud-Powered 성과 시스템 소개 다양 한 유용한 기능을 하는 동안도 어렵습니다 더 게임에 대 한 유효성 검사 및 도전 과제 완료 되므로 해당 도전 과제를 사용 하 여 문제를 해결 하려면 개발자가 규칙에 의해 직접 트리거되는 서비스에서 live 하지 않고 해당 게임 자체에서 직접 제어 됩니다.

게임 개발자가 반복적으로 공유 만족 하는 피드백 및 값 Cloud-Powered 성과 시스템 함께 도입 된 특정 기능에 나오는입니다.

1.  도전 과제 진행, 실시간 업데이트, 개념 아트 보상 게시 등의 새 사용자 환경 기능이 피드를 잠금 해제 합니다.

2.  게임 패키지에 포함 되어야 하는 로컬 구성 하는 대신 서비스 구성 등의 향상 된 기능 구성 (예: gameconfig, XLAST, SPA, 등)을 쉽게 도전 과제를 편집할 수 있는 문자열 및 게임에 제공 된 후 이미지입니다.

도전 과제 2017에서 대체를 빌드하는 것 사용 하 여 이후 타이틀에 Xbox 게임 개발자가 통합, 도전 과제를 구성 하는 데 훨씬 더 간단 하 게 하는 기존 Cloud-Powered 도전 과제 시스템의 도전 과제의 잠금을 해제 및 업데이트에 게임 코드 및 도전 과제는 예상 대로 작동 하는지 확인 합니다.

## <a name="whats-different-with-achievements-2017"></a>도전 과제 2017와 다른 점

|                          | 도전 과제 2017 시스템        | 클라우드 기반 성과 시스템      |
|--------------------------|---------------------------------------|----------------------------------------|
| 트리거를 잠금 해제           | API 호출을 통해 직접                 | 원격 분석 이벤트를 통해 간접적으로        |
| 소유자를 잠금 해제             | 제목                                 | Xbox Live                              |
| 구성            | 문자열, 이미지, 보상              | 문자열, 이미지, 보상, 규칙을 잠금 해제 \ [통계, + events\]                    |
| 진행              | 지원 <br>*API 호출을 통해 직접*                | 지원 <br> *원격 분석 이벤트를 통해 간접적으로*       |
| 실시간 활동 (RTA) | 지원                             | 지원                              |
| 문제               | 지원되지 않음   | 지원                      |

## <a name="title-requirements"></a>제목 요구 사항

다음은 도전 과제 2017 시스템 사용 하 여 모든 제목의 요구 사항입니다.

1.  **새 (아직 릴리스되지 않은) 타이틀 이어야 합니다.** 이미 해제가 Cloud-Powered 성과 시스템을 사용 하는 제목을 자격이 되지 않습니다. 자세한 내용을 보려면 참조 [이유는 기존 제목 "을 마이그레이션할 수" 새 도전 과제 2017 시스템?](#_Why_can’t_existing)

2.  **2016 년 8 월 XDK 사용 해야 이상.** Update_Achievement API는 2016 XDK 8 월에에서 출시 되었습니다.

3.  **XDK 또는 UWP는 제목 이어야 합니다.** 도전 과제 2017 시스템은 Xbox 360, Windows를 포함 한 레거시 플랫폼에 사용할 수 없습니다 8.x 고객만 또는 및 Windows Phone 8 또는 이전 합니다.

## <a name="updateachievement-api"></a>Update_Achievement API

도전 과제는 XDP 이나 [UDC](../configure-xbl/dev-center/achievements-in-udc.md) 구성 하 고 개발자 샌드박스에 게시, 타이틀 Update_Achievement API를 호출 하 여이 잠금 수 있습니다.

API가는 XDK와 Xbox Live SDK에서 사용할 수 있습니다.

### <a name="api-signature"></a>API 서명

API 서명이 다음과 같습니다.

```c++
/// <summary>
    /// Allow achievement progress to be updated and achievements to be unlocked.  
    /// This API will work even when offline. On PC and Xbox One, updates will be posted by the system when connection is re-established even if the title isn't running
    /// </summary>
    /// <param name="xboxUserId">The Xbox User ID of the player.</param>
    /// <param name="titleId">The title ID.</param>
    /// <param name="serviceConfigurationId">The service configuration ID (SCID) for the title.</param>
    /// <param name="achievementId">The achievement ID as defined by XDP or Partner Center.</param>
    /// <param name="percentComplete">The completion percentage of the achievement to indicate progress.
    /// Valid values are from 1 to 100. Set to 100 to unlock the achievement.  
    /// Progress will be set by the server to the highest value sent</param>
    /// <remarks>
    /// Returns a task<T> object that represents the state of the asynchronous operation.
    ///
    /// This method calls V2 GET /users/xuid({xuid})/achievements/{scid}/update
    /// </remarks>
    _XSAPIIMP pplx::task<xbox::services::xbox_live_result<void>> update_achievement(
        _In_ const string_t& xboxUserId,
        _In_ uint32_t titleId,
        _In_ const string_t& serviceConfigurationId,
        _In_ const string_t& achievementId,
        _In_ uint32_t percentComplete
        );
```

`xbox::services::xbox_live_result<T>` 모든 c + + Xbox Live 서비스 API 호출에 대 한 반환 호출이입니다.

자세한 내용은 체크 아웃 Xfest 2015 대화 하 고, "XSAPI: c + +, 예외 없이!"<br>
[비디오](http://go.microsoft.com/?linkid=9888207) |  [슬라이드](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Documents/Xfest_2015/Xbox_Live_Track/XSAPI_Cpp_No_Exceptions.pptx)

### <a name="unlocking-via-updateachievement-api"></a>Update_Achievement API 통해 잠금 해제

도전 과제를 잠금 해제를 *percentComplete* 100으로 설정 합니다.

사용자가 온라인으로 요청 Xbox Live 도전 과제 서비스에 즉시 전송 됩니다 하 고 다음 사용자 경험을 트리거합니다.

-   발생 한 도전 과제를 잠금 해제 된 알림;

-   지정 된 도전 과제 타이틀;에 대 한 사용자의 도전 과제 목록에 잠금 해제 됨으로 표시 됩니다.

-   잠금 해제 된 도전 과제 피드 사용자의 활동에 추가 됩니다.

> *참고: 있습니다 도전 과제 2017 시스템을 사용 하는 도전 과제 및 Cloud-Powered 도전 과제에 대 한 사용자 환경에서 표시 차이가 없습니다.*

사용자가 오프 라인으로 잠금 해제 요청 사용자의 장치에서 로컬로 대기 됩니다. 사용자의 장치에 네트워크 연결, 요청은 자동으로 다시 때 도전 과제 서비스로 전송 – 참고:이 – 트리거하는 게임에서 필요한 작업은 없습니다 및 위의 사용자 환경이 설명 된 대로 발생 합니다.

### <a name="updating-completion-progress-via-updateachievement-api"></a>Update_Achievement API를 통해 완료 진행률 업데이트

도전 과제를 잠금 해제에 대 한 사용자의 진행률을 업데이트 하려면 1-100 사이의 적절 한 정수로 *완료율* 을 설정 합니다.

도전 과제의 진행률만 늘릴 수 있습니다. *PercentComplete* 도전 과제의 마지막 *percentComplete* 값 보다 작거나 설정 되 면 업데이트 무시 됩니다. 예를 들어, 75 도전 과제의 *완료율* 이전에 설정 된, 25 값을 사용 하 여 업데이트를 보내지 무시 되 고 도전 과제 완료 75%로 표시 됩니다.

*PercentComplete* 100으로 설정 되 면 도전 과제 잠금을 해제 됩니다.

*PercentComplete* 100 보다 큰 수로 설정 되 면 정확 하 게 100으로 설정 해야 하는 경우 API 작동 합니다.

## <a name="frequently-asked-questions"></a>질문과 대답

### <a name="span-idwhyarechallenges-classanchorspancan-i-ship-my-title-using-the-achievements-2017-system-yet"></a><span id="_Why_are_Challenges" class="anchor"></span>아직 도전 과제 2017 시스템을 사용 하 여 내 제목을 제공할 수 있나요?

당연히 있습니다. 모든 새 타이틀은, 및 도전 과제 2017 사용 하는 것이 좋습니다 Cloud-Powered 성과 시스템 대신 시스템입니다.

### <a name="why-are-challenges-not-supported-in-the-achievements-2017-system"></a>왜 문제에서에서 지원 되지 않는 도전 과제 2017 시스템?

Xbox 게임에서 사용 현황 데이터에 대해서 설명 하였습니다 현재 구현 및 프레젠테이션 문제 중 대부분의 게임 개발자에 대 한 요구가 만족 하지 않습니다. 계속 개발자 입력과이 공간에 피드백을 수집 하 고 있는 개발자는 지점에서 더 많은 이자 향후 기능을 제공 하도록 노력 하겠습니다. 새로 릴리스된 Xbox 아레나 Xbox에 대 한 새로운 경쟁 기능을 도입 하는 기능 예로 게임 유사 하지만, 방향입니다.

### <a name="can-i-still-add-new-achievements-every-calendar-quarter-if-my-title-is-using-the-achievements-2017-system"></a>추가할 수 있습니까 여전히 새로운 도전 과제 모든 분기 내 제목 도전 과제 2017 시스템을 사용 하는 경우?

그렇습니다. 도전 과제 정책 변경 되지 않습니다.

### <a name="span-idwhycantexisting-classanchorspanwhy-cant-existing-titles-migrate-onto-the-new-achievements-2017-system"></a><span id="_Why_can’t_existing" class="anchor"></span>이유는 기존 제목 "을 마이그레이션할 수" 새 도전 과제 2017 시스템?

대부분의 기존 제목에 대 한 도전 과제 2017 시스템에 '마이그레이션' 업데이트 서비스 구성으로 제한 되지 않습니다 및 이러한 변경 내용을 만으로는 매우 수 있지만 호출-잠금 해제 도전 과제에 대 한 이벤트 쓰기 바꾸는 비용이 많이 드는 위험이 오류와 구분 되어 회복할 수 없게 되 고 도전 과제 야기 하는 의도 하지 않은 동작을 수행할 것입니다. 대신, 대부분의 기존 제목 기존 데이터를 사용 하 여 사용자에도 포함 합니다. 시스템은 Cloud-Powered 도전 과제에서 이미 사용 하는 라이브 게임으로 변환 하려고 뿐만 아니라 개발자와 Xbox는 매우 비용이 많이 드는 노력 수 기존 사용자의 프로필 및/또는 게임 환경을 피해갈 현저 하 게 됩니다.

### <a name="if-my-title-was-released-using-the-cloud-powered-achievements-system-can-any-future-dlc-for-the-title-switch-to-achievements-2017"></a>내 제목 Cloud-Powered 성과 시스템을 사용 하 여 릴리스된 경우 제목에 대 한 모든 향후 DLC 전환할 수 도전 과제 2017?

타이틀에 대 한 모든 성과 동일한 도전 과제 시스템을 사용 해야 합니다. 제목에 대 한 모든 향후 도전 과제에 사용 해야 하는 시스템은 기본 게임의 도전 과제 도전 과제 시스템 사용 됩니다.

### <a name="while-testing-achievements-in-my-dev-sandbox-can-i-mix-and-match-between-using-the-achievements-2017-system-and-the-cloud-powered-achievements-system"></a>개발자 샌드박스 내에 도전 과제를 테스트 하는 동안 도전 과제 2017 시스템과 Cloud-Powered 성과 시스템을 사용 하 여 간의 혼합 일치 I 수 있습니까?

아니요. 타이틀에 대 한 모든 성과 동일한 도전 과제 시스템을 사용 해야 합니다.

### <a name="does-achievements-2017-also-include-offline-unlocks"></a>도전 과제 2017도는 오프 라인으로 잠금 해제?

Update_Achievement API는 자동으로 오프 라인 잠금 해제 요청 큐 및 장치에 네트워크 연결, 현재 유사한를 다시 설정 하는 경우 Xbox Live에 자동으로 동기화 됩니다를 장치는 오프 라인 성과 제목에 잠금 해제 하는 경우 클라우드 기반 성과 시스템의 오프 라인 경험 합니다. 도전 과제를 잠금 해제 하는 동안 사용자가 오프 라인으로 발생 하지 합니다.

### <a name="i-see-a-new-achievementupdate-event-in-xdp-if-my-title-uses-that-event-does-that-mean-it-has-achievements-2017"></a>I XDP에서 새 "AchievementUpdate" 이벤트를 참조 하세요. 내 제목 해당 이벤트를 사용 하는 경우 있다는 뜻 도전 과제 2017가?

*AchievementUpdate* 기본 이벤트 백 엔드 목적에 대 한 Xbox Live에 필요 합니다. 안전 하 게이 무시할 수 있습니다. 타이틀이 종류의 기본 이벤트를 사용 하 여 이벤트를 구성 하는 경우 해당 이벤트 쓰기 Xbox Live에서 무시 됩니다. 타이틀 Cloud-Powered 성과 시스템에 내장 된 다른 기본 이벤트 형식을 사용 하 여 해당 이벤트를 구성 하려면 계속 해야 합니다. 도전 과제 2017 시스템에 내장 된 제목 필요 성과 목적에 대 *한* 이벤트를 구성 하지 않습니다.
