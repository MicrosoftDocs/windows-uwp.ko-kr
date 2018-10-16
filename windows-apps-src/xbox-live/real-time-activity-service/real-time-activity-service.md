---
title: 실시간 활동 서비스
author: KevinAsgari
description: Xbox Live 실시간 활동 서비스에 알아봅니다.
ms.assetid: 50de262f-fc55-4301-83b5-0a8a30bc7852
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 실시간으로 활동 서비스.
ms.localizationpriority: medium
ms.openlocfilehash: 1969a36a363f86ba6eced1c27d2206de1160c7bc
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4681321"
---
# <a name="real-time-activity-service"></a>실시간 활동 서비스

실시간 활동 (RTA) 서비스 상태 데이터, 사용자 통계 및 현재 상태에 가입 하려면 모든 장치에서 응용 프로그램을 허용 합니다. 시스템에서는 자신의 개인 정보 설정에 따라 모든 제목을 소유의 데이터를 다른 사용자 데이터를 구독 수 있습니다. 이 정보의 흐름을 않고도 지속적으로 폴링하여 최신 데이터를 가져올 수 있습니다.


## <a name="developer-scenarios"></a>개발자 시나리오

RTA 지 많은 시나리오가 있습니다. 그 중 몇 여기에 나열 된 이지만 RTA의 강점은 경우도 있습니다 진행할 디바이스를 사용 하 여 아직 생각 하지 않은 것입니다. 사용자가 자주 갖는 언제 든 해당 Microsoft Surface 또는 Apple iPad 콘솔 제목 상호 작용 하는 동안 게임 플레이의 차세대 정의 하는 데 도움이 수 있습니다. RTA 주제 연습에서는 다양 한 Windows에서 제공 하는 WebSockets API를 사용 하 여 구현에 대 한 개요에 포함 되도록 WebSocket 기술을 사용 합니다.

RTA를 사용 하 여 타이틀에 대 한 생성할 수 있는 몇 가지 간단한 시나리오는 다음과 같습니다.

-   도전 과제 진행 중인 앱
-   게임 도움말 앱
-   Squad 뷰어 앱
-   통계 뷰어
-   현재 상태 뷰어


## <a name="achievements-progress-app"></a>도전 과제 진행 중인 앱

사용자는 특정 도전 과제, 특히 특정 횟수 작업을 수행 해야 하는 전적으로 자신의 진행 용어나 거의 항상. (Xbox Live 플레이어 통계 서비스에 집계 됩니다)는 사용자의 통계에 대 한 실시간 액세스를 사용 하 여 플레이어와 도전 과제 및 중요 시점, Xbox One에서 또는 사용자 하는 동안 도우미 장치를 향해 친구 들에 대 한 실시간 진행률을 표시할 수 있습니다. 타이틀을 재생 됩니다.


## <a name="game-help-app"></a>게임 도움말 앱

타이틀을 통해 사용자가 탐색, 실시간 데이터 액세스 표시할 수 게임 도움말 경험 옆에 있는 하나 Xbox One에서 또는 도우미 장치에서. 사용자가 새 지도, 새 트랙 또는 어려운 상사 클럽에 발생할 수 있습니다 및 사용자 생성 또는 개발자 생성 비디오 및 타이틀을 통해 사용자를 위해 텍스트에 도움이 되는 게임 도우미 표시할 수 있습니다.


## <a name="squad-viewer-app"></a>Squad 뷰어 앱

게임이 협동 멀티 플레이 게임, 플레이어와 자신의 동료 협력 공유 목표입니다. 너무 많은 플레이어를 사용 하 여 화면을 더 크게 추적 하기 어려울 수 있습니다. 실시간 데이터에 대 한 액세스를 사용 하 여 작업 수 있는의 개략적인 지도 및 열 지도 표시 하는 도우미 앱을 만들 수 있습니다.


## <a name="statistics-viewer"></a>통계 뷰어

RTA 고려할 때는 도우미 앱 생각 하는 일반적인 상태일 때 RTA 핵심 제목에도 사용할 수 있습니다. 예를 들어 RTA 간단 하 게 일치 하는 멀티 플레이어에서 컨트롤러에서 보기 단추를 누르면 플레이어에 의해 아마도 멀티 플레이어 게임의 플레이어의 모든 사용자의 현재 통계 게임 내에서 표시와 함께 제공 하기 위해 사용할 수 있습니다.


## <a name="presence-viewer"></a>현재 상태 뷰어

로비의 경우는 최신 보기가 온라인 상태인 친구와 동일한 제목 재생 하는 경우에 유용 합니다. 사용자의 친구의 존재 여부를 구독할 수 있으며 어떤 친구 실시간 모두 제목으로 재생을 시작할 때 경우에 온라인 상태가 표시.


## <a name="subscription-privacy-and-authorization"></a>구독 개인 정보 및 권한 부여

최신 버전 RTA의 개인 정보 및 권한 부여/콘텐츠 격리 포함 됩니다. 개인 정보 및 권한 부여 검사 만족할 경우으로 앱 RTA 사용 가능으로 표시 된 모든 통계 구독할 수 있습니다. (통계 RTA 지원 참조 하는 방법에 대 한 자세한 정보에 대 한 [통계 알림에 대 한 등록](register-for-stat-notifications.md)합니다. RTA 사용 설정할 수 있는 통계 종류에 제한이 없다는-는 것은 개발자에 게 합니다. 그러나 사용자가 앱 세션별 구독할 수 있는 통계 *수* 에 제한이 있습니다. 사용자가 해당 제한에 도달 하면 경우 자신이 다음 구독에서 오류가 받게 됩니다.


## <a name="in-this-section"></a>이 섹션 내용

[플레이어 통계 변경 알림 등록](register-for-stat-notifications.md)  
통계 또는 상태 정보에 대 한 실시간 활동 (RTA)를 사용 하는 방법에 설명 합니다.

[Winrt api를 사용 하 여 실시간 활동 (rta) 서비스 프로그래밍](programming-the-real-time-activity-service.md)  
WinRT Api를 사용 하 여 실시간 활동 (RTA) 서비스 하는 방법을 설명 합니다.

[Restful 인터페이스를 사용 하 여 실시간 활동 (rta) 서비스 프로그래밍](programming-the-real-time-activity-service.md)  
RESTful 인터페이스를 사용 하 여 실시간 활동 (RTA) 서비스 하는 방법을 설명 합니다.

[실시간으로 활동 (rta)에 대 한 유용한 정보](rta-best-practices.md)  
Xbox 서비스 실시간으로 활동 (RTA) 서비스를 사용 하 여 Xbox 데이터 플랫폼에서 통계와 상태 데이터를 구독할 수에 대 한 모범 사례입니다.
