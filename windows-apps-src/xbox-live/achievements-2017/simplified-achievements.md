---
title: 도전 과제 2017
description: 도전 과제 2017
ms.assetid: d424db04-328d-470c-81d3-5d4b82cb792f
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: bfc67f6aca27abf095a89c451111e6429bca82e1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590988"
---
# <a name="achievements-2017"></a>도전 과제 2017

성과 2017 게임 개발자 들이 Xbox One, Windows 10, Windows 10 Phone, Android 및 iOS에서 새로운 Xbox Live 게임에 대 한 도전 과제를 잠금 해제를 직접 호출 모델을 사용 하 여 사용할 수 있습니다.

## <a name="introduction"></a>소개

Xbox One을 사용 하 여 단순히 게임 내 원격 분석 이벤트를 전송 하 여 해당 사용자 통계와 성과, 다양 한 현재 상태, 멀티 플레이 게임을 Xbox Live 기능에 대 한 데이터를 드라이브 하도록 게임 개발자는 새로운 Cloud-Powered 성과 시스템을 도입 했습니다. 다양 한 새로운 혜택 수 있게이 – 단일 이벤트를 여러 Xbox Live 기능에 대 한 데이터를 업데이트할 수 있습니다. Xbox Live 구성 서버 대신 클라이언트에서 실행 그 밖의 기능입니다.

Xbox One 출시 이후 년 동안에서 게임 개발자 피드백을 밀접 하 게 귀가 되 고 개발자가 다음 공유 일관 되 게 갖습니다.

1.  **직접 호출 패턴을 통해 도전 과제를 잠금 해제 하려고 합니다.** 대부분의 개발자 Xbox의 이전 버전을 포함 하는 다양 한 플랫폼용 게임을 빌드하고 서비스도 도전 과제와 같은 시스템을 직접 호출 메서드를 사용 합니다. Xbox One에 대 한 호출이 잠금 해제 지원 직접 및 다른 현재 범용 Xbox 플랫폼 개발 시간 비용와 플랫폼 간 게임 개발 요구에 맞게 용이 하 게 됩니다.

2.  **구성 복잡성을 최소화 합니다.** 성과 Cloud-Powered 성과 시스템과 논리를 잠금 해제 타이틀의 통계 데이터를 해석 하는 방법 및 사용자에 대 한 도전 과제를 잠금 해제 하는 경우 서비스에서 알 수 있도록 Xbox Live에 구성 되어야 합니다. 이 작업은 이전에 존재 하지 않는 성과 구성의 새 도전 과제 규칙 섹션을 통해 수행 되었습니다. 클라우드에서 논리를 잠금 해제 하지 하는 것은 매우 강력 수 있지만,이 추가 구성 요구 사항을 디자인 및 제목의 성과의 생성에 복잡성을 추가 합니다.

3.  **문제 해결이 어렵습니다.** Cloud-Powered 성과 시스템 다양 한 유용한 기능을 도입 하는 동안 것도 더 어려울 수 있습니다 게임에 대 한 유효성을 검사 하 고 도전 과제를 잠금 해제 하므로 자신의 성과 사용 하 여 문제를 해결 하려면 개발자가 규칙에 의해 직접 트리거되는 서비스에서 라이브 것이 아니라 게임 자체에 의해 직접 제어 합니다.

게임 개발자가 반복적으로 공유 주셔서 감사 드리며 이러한 피드백과 값 Cloud-Powered 성과 시스템과 함께 도입 된 특정 기능 주목할 만한 가치가 있습니다.

1.  작업 피드로 성과 달성 하셨습니다 진행, 실시간 업데이트, 개념 아트 보상 및 게시와 같은 새 사용자 환경 기능은 잠금을 해제 합니다.

2.  게임 패키지에 포함 되어야 하는 로컬 구성 하는 대신 서비스 구성 같은 구성 개선 사항 (예: gameconfig, XLAST, SPA 등) 및 도전 과제를 쉽게 편집할 수는 문자열 & 게임 배송 되 면 이미지입니다.

성과 2017을 사용 하 여 대체에 빌드합니다 Xbox 게임 개발자 들이 도전 과제를 구성, 통합도 쉽게 사용 하도록 이후 타이틀에 대 한 기존 Cloud-Powered 성과 시스템의 도전 과제를 잠금 해제 &로 업데이트 합니다 게임 코드 및 성과 예상 대로 작동 하는지 유효성을 검사 합니다.

## <a name="whats-different-with-achievements-2017"></a>성과 2017을 사용 하 여 다른 점은

|                          | 성과 2017 시스템        | 클라우드 기반 성과 시스템      |
|--------------------------|---------------------------------------|----------------------------------------|
| 트리거를 잠금 해제           | API 호출을 통해 직접                 | 원격 분석 이벤트를 통해 간접적으로        |
| 소유자를 잠금 해제             | 제목                                 | Xbox Live                              |
| 구성            | 문자열, 이미지, 보상              | 문자열, 이미지, 보상에 규칙을 잠금 해제 \[통계 + 이벤트\]                    |
| 진행              | 지원함 <br>*API 호출을 통해 직접*                | 지원함 <br> *원격 분석 이벤트를 통해 간접적으로*       |
| (RTA)의 실시간 활동 | 지원함                             | 지원함                              |
| 과제               | 지원되지 않음   | 지원함                      |

## <a name="title-requirements"></a>제목 요구 사항

다음은 성과 2017 시스템을 사용 하는 모든 타이틀의 요구 사항입니다.

1.  **새 (출시 되지 않은) 제목을 이어야 합니다.** 이미 출시 된가 Cloud-Powered 성과 시스템을 사용 하는 제목을 자격이 없습니다. 자세한 내용은 참조 [이유 수 없습니다. 기존 제목 "마이그레이션" 새 성과 2017 시스템?](#_Why_can’t_existing)

2.  **2016 년 8 월 XDK를 사용 해야 이상.** Update_Achievement API는 XDK 2016 년 8 월에에서 출시 되었습니다.

3.  **XDK 또는 UWP 제목 이어야 합니다.** 성과 2017 시스템 Xbox 360, Windows를 포함 한 레거시 플랫폼을 사용할 수 없는 8.x 이상 또는 Windows Phone 8 이상입니다.

## <a name="updateachievement-api"></a>Update_Achievement API

자신의 성과 XDP를 통해 구성 되 면 또는 [UDC](../configure-xbl/dev-center/achievements-in-udc.md) 개발 샌드박스에 게시 된 프로그램 제목 잠금을 해제할 수 있습니다 하 Update_Achievement API를 호출 하 여 및 합니다.

API는 XDK 및 Xbox Live SDK에서 제공 됩니다.

### <a name="api-signature"></a>API 서명

API 시그니처는 다음과 같습니다.

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

`xbox::services::xbox_live_result<T>` c + + Xbox Live Services API에 대 한 모든 호출에 대 한 반환 호출이입니다.

자세한 내용은 체크 아웃 Xfest 2015 talk "XSAPI: C + + 예외 없이! "<br>
[video](https://go.microsoft.com/?linkid=9888207) |  [slides](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Documents/Xfest_2015/Xbox_Live_Track/XSAPI_Cpp_No_Exceptions.pptx)

### <a name="unlocking-via-updateachievement-api"></a>Update_Achievement API 통해 잠금 해제

도전 과제를 잠금 해제를 설정 합니다 *percentComplete* 100입니다.

사용자가 온라인 상태인 경우 요청 Xbox Live 성과 서비스로 바로 전송 되 고 다음과 같은 사용자 환경을 트리거할:

-   사용자 잠금 해제 된 성과 달성 하셨습니다 알림을; 받을 합니다.

-   지정 된 도전 과제 제목;에 대 한 사용자의 성과 달성 하셨습니다 목록의 Unlocked로 표시 됩니다.

-   잠기지 않은 도전 과제 피드 사용자의 활동에 추가 됩니다.

> *참고: 성과 2017 시스템을 사용 하는 성과 및 Cloud-Powered 성과 대 한 사용자 환경에서 차이가 표시 됩니다.*

사용자가 오프 라인 상태, 잠금 해제 요청 사용자의 장치에서 로컬로 대기 됩니다. 사용자의 장치 네트워크 연결에는 요청을 자동으로 다시 만들었습니다 때 성과 서비스로 전송할-참고: 동작은이 – 트리거에 게임에서 필요 하지 않습니다 하 고 위의 사용자 환경을 설명 된 대로 발생 합니다.

### <a name="updating-completion-progress-via-updateachievement-api"></a>Update_Achievement API를 통해 완료 진행 상태를 업데이트 하는 중

도전 과제를 잠금 해제에 대 한 사용자의 진행률을 업데이트 하려면 다음을 설정 합니다 *percentComplete* 1에서 100 사이의 적절 한 정수로 합니다.

성과 진행률만 늘릴 수 있습니다. 하는 경우 *percentComplete* 도전 과제의 마지막 보다 작은 수로 설정 된 *percentComplete* 값이 업데이트는 무시 됩니다. 예를 들어 경우 도전 과제의 *percentComplete* 75를 25 값을 사용 하 여 업데이트 무시 되 고 도전 과제 여전히 75% 완료로 표시할 전송으로 설정 된 이전 합니다.

하는 경우 *percentComplete* 설정 된 100으로 도전 과제는 잠금을 해제 합니다.

하는 경우 *percentComplete* 설정 된 100 보다 큰 숫자를 100 정확 하 게 설정 하는 경우에 따라 API 작동 합니다.

## <a name="frequently-asked-questions"></a>질문과 대답

### <a name="span-idwhyarechallenges-classanchorspancan-i-ship-my-title-using-the-achievements-2017-system-yet"></a><span id="_Why_are_Challenges" class="anchor"></span>아직 성과 2017 시스템을 사용 하 여 내 제목을 제공할 수 있나요?

당연히 있습니다. 모든 새 제목 반가운 일 되며 성과 2017를 사용 하는 것이 좋습니다 Cloud-Powered 성과 시스템 대신 시스템입니다.

### <a name="why-are-challenges-not-supported-in-the-achievements-2017-system"></a>왜 문제에서에서 지원 되지 않는 성과 2017 시스템?

Xbox 게임에서 사용 현황 데이터는 현재 구현 및 프레젠테이션 문제의 대부분의 게임 개발자에 대 한 요구가 만족 하지 않습니다 주었습니다. 개발자 입력과이 공간에 대 한 피드백을 모으고 있습니다. 계속 하 고 개발자 요구를 사용 하 여 지점에 자세히 설명 된 이후 기능을 제공 하도록 노력 합니다. 새로 출시 된 Xbox Arena Xbox에 대 한 새로운 경쟁력 있는 기능을 제공 하는 기능의 예로 게임 새롭 지만 마찬가지로 방향입니다.

### <a name="can-i-still-add-new-achievements-every-calendar-quarter-if-my-title-is-using-the-achievements-2017-system"></a>추가 새 성과 모든 calendar quarter 내 제목 성과 2017 시스템을 사용 하는 경우?

예. 성과 정책을 변경 되지 않습니다.

### <a name="span-idwhycantexisting-classanchorspanwhy-cant-existing-titles-migrate-onto-the-new-achievements-2017-system"></a><span id="_Why_can’t_existing" class="anchor"></span>왜 없습니다 기존 제목 "마이그레이션" 새 성과 2017 시스템 합니까?

기존 타이틀의 대부분에 대 한 '마이그레이션' 성과 2017 시스템에는 해당 서비스 구성 업데이트를 제한 되지 않습니다 및 단독으로 이러한 변경 내용은 매우 수는 있지만 호출 –를 잠금 해제 도전 과제에 대 한 이벤트 쓰기 대체 비용이 많이 드는 및 상당한 위험이 오류 및 손상 복구 중인 성과 인해 의도 하지 않은 동작이 수행 됩니다. 대신 대부분의 기존 제목 수도 기존 데이터를 사용 하 여 사용자가 있습니다. 시스템은 Cloud-Powered 성과 이미 사용은 라이브 게임을 변환 하는 동안 개발자와 Xbox에 대 한 매우 비용이 많이 드는 노력을 수 뿐만 아니라 기존 사용자의 프로필 및/또는 게임 환경을 크게 보안이 위험 해질 것입니다.

### <a name="if-my-title-was-released-using-the-cloud-powered-achievements-system-can-any-future-dlc-for-the-title-switch-to-achievements-2017"></a>내 제목을 눌렀다 놓으면 Cloud-Powered 성과 시스템을 사용 하 여 제목에 대 한 모든 향후 DLC 전환할 수 성과 2017?

제목에 대 한 모든 성과 같은 성과 시스템을 사용 해야 합니다. 어떤 성과 시스템 기본 게임의 성과에서 사용 되는 제목에 대 한 모든 향후 성과 대 한 사용 해야 하는 시스템.

### <a name="while-testing-achievements-in-my-dev-sandbox-can-i-mix-and-match-between-using-the-achievements-2017-system-and-the-cloud-powered-achievements-system"></a>내 개발 샌드박스에서 성과 테스트 하는 동안 수 혼합 및 일치 성과 2017 시스템과 Cloud-Powered 성과 시스템을 사용 하 여 간의 있나요?

아니요. 제목에 대 한 모든 성과 같은 성과 시스템을 사용 해야 합니다.

### <a name="does-achievements-2017-also-include-offline-unlocks"></a>성과 2017도는 오프 라인으로 잠금 해제?

장치가 오프 라인일 때 성과 해제 하는 제목, 경우 Update_Achievement API를 자동으로 오프 라인 잠금 해제 요청 큐를 장치에 네트워크 연결, 현재 비슷합니다를 다시 설정 하는 경우 Xbox Live에 자동으로 동기화 됩니다. 클라우드 기반 성과 시스템의 오프 라인 환경입니다. 도전 과제를 잠금 해제는 사용자가 오프 라인 수행 되지 않아야 합니다.

### <a name="i-see-a-new-achievementupdate-event-in-xdp-if-my-title-uses-that-event-does-that-mean-it-has-achievements-2017"></a>XDP 새 "AchievementUpdate" 이벤트가 표시 됩니다. 내 제목 해당 이벤트를 사용 하는 경우 메시지의 의미는 성과 2017가?

합니다 *AchievementUpdate* 백 엔드 목적에 대 한 기본 이벤트 Xbox live 필수입니다. 이 안전 하 게 무시할 수 있습니다. 제목이 종류의 기본 이벤트를 사용 하 여 이벤트를 구성 하는 경우 해당 이벤트 쓰기 Xbox Live에서 무시 됩니다. 제목 Cloud-Powered 성과 시스템 기반으로 작성 된 다른 기본 이벤트 형식을 사용 하 여 해당 이벤트를 구성 하려면 계속 해야 합니다. 성과 2017 시스템을 기반으로 하는 제목을 구성할 필요는 없습니다 *모든* 도전 과제를 위해 이벤트입니다.
