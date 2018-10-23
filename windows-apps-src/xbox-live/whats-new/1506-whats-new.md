---
title: 새로운 기능-2015 년 6 월에 대 한 Xbox Live SDK
author: KevinAsgari
description: 새로운 기능-2015 년 6 월에 대 한 Xbox Live SDK
ms.assetid: 354bcd47-2564-4dd5-89e3-242bca462b35
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 29e31363a1afb0d01d24112a4fa8dd69f4f87bff
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5395206"
---
# <a name="whats-new-for-the-xbox-live-sdk---june-2015"></a>새로운 기능-2015 년 6 월에 대 한 Xbox Live SDK

Xbox Live SDK의 6 월 릴리스는 다음과 같은 업데이트가 포함 됩니다.

## <a name="os-and-tool-support"></a>운영 체제 및 도구 지원 ##
Xbox Live SDK는 이제 Windows 10 및 Visual Studio 2015 RC의 최신 참가자 빌드를 지원합니다.

## <a name="title-callable-ui-apis"></a>제목 호출할 UI Api

| 참고 |
|------|
| 이 섹션에서는 UWP 제목에만 적용 됩니다, 그리고 TCUI Windows.Xbox.UI.SystemUI 클래스를 나타내야 XDK 제목  |

Xbox Live SDK에는 이제 래퍼를 제목 호출할 UI (TCUI) 지 원하는 Windows 10 PC/모바일 장치에서 주식 UI를 표시 하기 위한 Api에 포함 되어 있습니다.

이러한 Api는 UWP 앱에 사용할 수만 있습니다.

TitleCallableUI (WinRT)와 title_callable_ui (c + +) 클래스 TCUI 래퍼를 호출할 수 있습니다.

재고 Ui 다음과 같습니다.
* 플레이어 선택기 UI
* 게임 초대 선택기 UI
* 플레이어 프로필 카드 UI
* 프로그램 추가/제거 친구 UI
* 표시 게임 타이틀 도전 과제 UI

새로운 Api의 사용 예를 들어 새 *TCUI* 샘플을 참조 하세요. {*SDK 소스 루트*} \Samples\TCUI에서 샘플을 찾을 수 있습니다.

## <a name="new-authentication-model-for-uwp-apps"></a>UWP 앱에 대 한 새로운 인증 모델
Xbox Live SDK는 이제 Microsoft 계정 (MSA)을 사용 하 여 Windows 10 PC/모바일 장치에서 Xbox Live 플레이어를 식별 하기 위한 지원 합니다.

이제 XboxLiveUser (WinRT) 및 xbox_live_user (c + +) 클래스를 사용 하 여 사용자에 서명할 수 있습니다.

## <a name="new-api-for-writing-events-in-uwp-apps"></a>UWP 앱에서 이벤트를 작성 하기 위한 새로운 API

| 참고 |
|------|
| 이 섹션은 UWP 제목에만 적용 됩니다.  XDK 개발자 XDK 특정 정보에 대 한 [게임 이벤트](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/game-events) 문서를 참조 해야  |

새 EventsService (WinRT) 및 events_service (c + +) 클래스 사용 하면 사용자 통계, 도전 과제, 순위표 등을 업데이트할 수 있는 게임에서 이벤트를 작성할 수 있습니다. 이러한 새로운 클래스는 UWP 앱에만 해당 합니다.

## <a name="breaking-change-to-event-handlers"></a>이벤트 처리기에 주요 변경 ##
C + + SDK의 모든 이벤트 처리기에서 단일 변경 된 `void set_*_handler()` 메서드 쌍에 `function_context add_*_handler()` 및 `void remove_*_handler(function_context context)` 메서드.

각 `add_*_handler()` 지금은 메서드가 반환 되는 `function_context` 개체입니다. 이벤트 처리기를 제거 하는 경우에 전달 해야 합니다 `function_context` 개체.

예:
```
function_context subscriptionLostContext = xbox_live_context()->multiplayer_service().add_multiplayer_subscription_lost_handler(...);

localUser->xbox_live_context()->multiplayer_service().remove_multiplayer_subscription_lost_handler(subscriptionLostContext);
```

## <a name="new-apis-for-managing-multiplayer-scenarios"></a>멀티 플레이어 시나리오를 관리 하기 위한 새로운 Api
이제 Xbox Live SDK는 일반적인 멀티 플레이어 시나리오를 관리 하기 위한 c + + Api 집합이 포함 됩니다. Api는 xbox.services.experimental.multiplayer 네임 스페이스에 포함 됩니다.

이러한 Api는 피드백에 따라 변경 될 수 있다는 것을 의미 하는 실험 네임 스페이스에 있습니다.  개발자가 사용 하 고 피드백을 제공 하는 것이 좋습니다.

이러한 새로운 Api의 사용 예를 들어 새 *MultiplayerManager* 샘플을 참조 하세요. {*SDK 소스 루트*} \Samples\MultiplayerManager에서 샘플을 찾을 수 있습니다.
