---
title: 실시간 활동 서비스
description: Xbox Live 실시간 작업 서비스에 알아봅니다.
ms.assetid: 50de262f-fc55-4301-83b5-0a8a30bc7852
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 실시간 작업 서비스입니다.
ms.localizationpriority: medium
ms.openlocfilehash: 36389fac3bd6dea2d2e24c0935087781118d8046
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592088"
---
# <a name="real-time-activity-service"></a>실시간 활동 서비스

활동 RTA (실시간) 서비스 상태 데이터, 사용자 통계 및 현재 상태를 구독할 수 있는 모든 장치에서 응용 프로그램을 허용 합니다. 해당 개인 정보 설정에 따라 모든 제목에 자신의 데이터를 다른 사용자 데이터를 구독을 허용 하는 시스템입니다. 따라서를 가져오려면 최신 데이터를 지속적으로 폴링하지 않고도 정보의 흐름입니다.


## <a name="developer-scenarios"></a>개발자 시나리오

RTA를 지 원하는 많은 시나리오가 있습니다. 그 중 일부에 지나지 여기에 나열 됩니다 이지만 RTA의 진가 답을 제시 했습니다 아직 상상 하지 않은 많은 시나리오입니다. 차세대 사용자 종종 있는 당면한 해당 Microsoft Surface 또는 Apple iPad을 콘솔 제목과 상호 작용할 때 게임 플레이 정의할 수 있습니다. RTA는 다양 한 하위 연습에서는 Windows에서 제공 하는 Websocket API를 사용 하 여 구현 되므로 WebSocket 기술을 사용 합니다.

다음은 제목에 대 한 RTA를 사용 하 여 만들 수 있는 몇 가지 간단한 시나리오입니다.

-   성과 진행률 앱
-   도움말 게임 앱
-   계 뷰어 앱
-   통계 뷰어
-   상태 뷰어


## <a name="achievements-progress-app"></a>성과 진행률 앱

사용자는 거의 항상 해당 진행을 특정 횟수 만큼 작업을 수행 해야 하는 성과 특히 특정 성과 대해 궁금한입니다. 사용자 통계 (Xbox Live Player 통계를 집계 됩니다.)에 대 한 실시간 액세스를 사용 하 여 플레이어에 게 친구 성과 및 마일스 톤을 도우미 장치는 사용자 또는 Xbox One에 실시간 진행률을 제공할 수 있습니다. 재생을 제목입니다.


## <a name="game-help-app"></a>도움말 게임 앱

를 사용자의 탐색을 제목 통해 데이터에 대 한 실시간 액세스를 사용 하면 모든 관련 장치 또는 Xbox One에서 게임 도움말 환경을 옆에 있는 하나 제공 수 있습니다. 사용자가 새 맵, 새 추적 또는 어려운 상사 퇴치 떠오를 수도 있습니다 및 게임 도움말 도우미를 사용 하 여 사용자가 생성 또는 개발자에서 생성 된 비디오 및 텍스트 제목에 환경을 통해 사용자를 표시할 수 있습니다.


## <a name="squad-viewer-app"></a>계 뷰어 앱

Co-op 멀티 플레이 게임에서 플레이어와 자신의 동료 협력 공동된 목표에 대 한 합니다. 많은 플레이어를 사용 하 여 전체적인을 추적 하는 것이 어려울 수 있습니다. 실시간 데이터에 액세스 하기를 사용 하 여 작업을 수 있는의 높은 수준의 맵 및 열 지도 보여 주는 도우미 앱을 만들 수 있습니다.


## <a name="statistics-viewer"></a>통계 뷰어

RTA를 고려할 때는 도우미 앱 생각 하는 일반적인 이지만, core 제목에 RTA를 사용할 수도 있습니다. 예를 들어, 멀티 플레이 일치 항목에 있는 동안 컨트롤러에서 뷰 단추를 누르는 player에서 모든 사용자의 현재 통계의 게임 내 표시를 사용 하 여 멀티 플레이 게임의 플레이어 아마도 제공 RTA를 사용할 수 있습니다.


## <a name="presence-viewer"></a>상태 뷰어

로비에 있는 경우는 친구 들과 온라인 상태이 고 동일한 제목 재생 하는 경우 최신 보기 있어야 유용 합니다. 사용자의 친구의 현재 상태를 구독 하 고는 친구 들과 실시간 모든 제목으로 재생 시작 하는 경우에 온라인 상태가 표시 수 있습니다.


## <a name="subscription-privacy-and-authorization"></a>구독 개인 정보 및 권한 부여

개인 정보 및 권한 부여/content 격리에 대 한 검사를 포함 하는 RTA의 최신 버전입니다. 개인 정보 및 권한 부여 검사를 충족으로 앱 RTA 사용 가능으로 표시 된 모든 통계 구독할 수 있습니다. (통계 RTA 지원 참조 하는 방법에 대 한 자세한 내용은 [stat 알림에 등록](register-for-stat-notifications.md)합니다. RTA 사용 될 수 있는 통계의 종류에 대 한 제한은 없습니다-개발자에 게 달려 있습니다. 하지만에 제한 됩니다 합니다 *번호* 사용자 앱 세션당 구독할 수 있는 통계. 사용자가 해당 한계에 도달 하면 자신이 다음 구독에 오류가 표시 됩니다.


## <a name="in-this-section"></a>이 섹션의 내용

[플레이어 상태 변경 알림을 등록 합니다.](register-for-stat-notifications.md)  
통계 또는 상태 정보에 대 한 활동 RTA (실시간)를 사용 하도록 설정 하는 방법에 설명 합니다.

[Winrt api를 사용 하 여 실시간 활동 (rta) 서비스를 프로그래밍 합니다.](programming-the-real-time-activity-service.md)  
WinRT Api를 사용 하는 활동 RTA (실시간) 서비스를 프로그래밍 하는 방법을 설명 합니다.

[Restful 인터페이스를 사용 하는 실시간 활동 (rta) 서비스를 프로그래밍 합니다.](programming-the-real-time-activity-service.md)  
RESTful 인터페이스를 사용 하는 활동 RTA (실시간) 서비스를 프로그래밍 하는 방법을 설명 합니다.

[실시간 활동 (rta)에 대 한 유용한 정보](rta-best-practices.md)  
Xbox 실시간 활동 (RTA) 서비스를 사용 하 여 Xbox 데이터 플랫폼에서 통계 및 상태 데이터를 구독할 수에 대 한 모범 사례입니다.
