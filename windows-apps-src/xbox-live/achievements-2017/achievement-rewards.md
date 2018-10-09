---
title: 도전 과제 보상
author: KevinAsgari
description: 보상을 제공 하는 성과 구성 하는 방법에 대해 설명 합니다.
ms.assetid: b6fc5bdb-ba7b-4687-985e-894182f066da
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 도전 과제, 보상
ms.localizationpriority: medium
ms.openlocfilehash: 67a84ac7296ccbb3aed82f676f09083dc9c16b17
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4430013"
---
# <a name="achievement-rewards"></a>도전 과제 보상

다음 다이어그램은 개발자 타이틀의 수명 주기 관리 방법을 보여 줍니다. 훨씬 더 많은 유연성 우리의 친숙 하며 때는 새로운 도전 과제 시스템 설계-어떻게 도전 과제 잠기지 않습니다에서 방법과 시기 추가 되 면 및 혜택은 사용자에 게 제공-개발자 제목으로 서비스로 실행 하는 강화 값을 추가 하 고 사용자의 참여 시간이 지남에 따라 유지 관리 합니다.

##### <a name="figure-1---how-a-title-might-drive-user-behavior"></a>그림 1.   어떻게 제목을 사용자 동작 드라이브 수 있습니다. #####
![rewarding_achievements](../images/omega/achievements_overview_01_drive_behavior.png)

### <a name="flexible-options-for-rewarding-achievement"></a>도전 과제 가치 유연한 옵션이 ###
Xbox One을 사용 하 여 도전 과제 시스템 보다 유연한 보상 옵션을 지원 하도록 확장 합니다. 게이머 점수에서 Xbox Live 생태계 하지만 이제 사용자에 대 한 단일, 일반적인 게임 점수를 추적 하는 중요 한 보상으로 남아-개발자나 게시자-도전 과제 보상 타이틀 내에서 훨씬 더 넓은 범위의 배달 메커니즘으로 사용할 수 있습니다 및 외부의 제목입니다.

성과 각 보상 유형의 하나의 보상까지 여러 보상을 사용 하 여 구성할 수 있습니다. 성과 없는 명시적 보상;를 사용 하 여 구성할 수 있습니다. 이러한 경우, 도전 과제의 아이콘 역할을 시각적 배지 도전 과제를 구입한 플레이어에 대 한 합니다.

Xbox Live 다음과 같은 유형의 보상을 지원합니다.

* 게이머 점수
* 아트
* 앱에서 보상

#### <a name="gamerscore"></a>게이머 점수 ####
Xbox Live 사용자로 작성 된 게이머 점수 값의 무결성을 유지 하기 위해 최선을 다하고 있습니다. 사용자 당 하나의 게이머 점수는! Xbox 360, Xbox One 또는 Windows 10 등의 기존 Xbox Live 플랫폼에서 사용자가 모든 게이머 해당 사용자에 대 한 단일 게이머 개수에 계산 됩니다.

사용자가 게이머 점수 도전 과제를 잠금 해제 구성 된 양만큼 사용자의 게이머 점수를 증가 자동으로 Xbox Live 합니다.

타이틀 제공할 수 게이머 점수에 자신의 도전 과제 보상으로 제한이 있습니다. 정책 문서를 참조 하세요. https://developer.xboxlive.com/ 에서 최신 정보.

#### <a name="art"></a>아트 ####
디자이너 그린 타이틀의 초기 단계 초기에 몇 가지 흥미로운 개념 아트 있으십니까? 아름, 고해상도 이미지를 플레이어에 방문 허브 응용 프로그램을 장식 수 있습니까? 아마도 앱이 지 원하는 여러 스킨? 아트 보상을 사용 하 여 무성, 멋진 환경을 제목에 플레이어를 획득 해야 하는 활용할 수 있습니다.

고해상도 개념 아트, 초기 디자인 드로잉 아트 자산을 특별히 만든 및 기타 디지털 아트 자산 제공 될 수 있습니다 보상으로 사용자에 게는 도전 과제를 잠금 해제 합니다. Xbox One 대시보드 내에서 이러한 자산을 표시 될 수 경험 하 고 간단 하 게 관련 메타 데이터를 검색할 도전 과제 서비스를 쿼리하여 도우미 경험에 표시할 수 있습니다.

#### <a name="in-app-rewards"></a>앱에서 보상 ####
앱에서 보상 훨씬 더 많은 유연성 및 제어 보상 성과 제공 하는 개발자에 게 제공 하기 위해 도입 합니다. 앱에서 보상을 사용 하 도전 과제를 사용 하 여 반드시 타이틀을 업데이트 하지 않고 사용자 지정 게임 보상으로 사용자에 게 직접 제공할 수 있습니다. 코드나 ID, 타이틀을 인식할 구를 사용 하 여 도전 과제 보상을 간단 하 게 구성 되며 사용자 도전 과제를 잠금 해제 Xbox LIVE 해당 코드에 제목에 있으므로 사용자에 게 제공 하도록 보상의 제목을 알리는.

보상 자체는 개발자에 달려 있습니다. 보상 아이디어는 다음과 같습니다.

* 추가 게임 내 통화 또는 지점
* 특수 문자, 무기, 또는 지도에 대 한 액세스
* 임시 경험 점수 합니다.

### <a name="configuring-in-app-rewards"></a>앱에서 보상 구성 ###
성과 대 한 앱에서 보상을 구성 하는 것은 매우 간단 합니다. 도전 과제 소유자 보상 이름, 보상 설명 및 보상 값 외에도 보상 아이콘을 제공 해야 합니다. 보상 값이 개발자가 결정 되며 하거나 제목 관련 보상 교환 환경의 일환으로 입력할 수 있는 게임 중 하나를 해석 하 고 올바르게 처리 수 있는 것 이어야 합니다.

게임 해석할 수 있는 보상 값의 예는 5 자리 숫자 또는 특수 문자열는 게임 또는 게임의 서비스 특정 게임 내 항목에 지도 알 수 있습니다. 개발자가 쉽게 게임을 읽는 방법 이해는 시간이 지남에 따라 새 보상 값을 추가 하려면 제목 관리 저장소 (TMS) 서비스를 활용 하고자 할 수 있습니다.

사용자가 제출 해야 하는 보상 값의 예는 특수 코드 또는 사용자가 제목, 도우미 앱 내에서 또는 개발자의 웹 사이트에서 내 교환 환경에 입력 문자열 수 있습니다.

### <a name="redeeming-in-app-rewards"></a>앱에서 보상을 통해 ###
앱에서 보상 사용자 시간부터 보상 게임 내에서 적용이 됩니다. 타이틀은 사용자가 제목을 사용자에 게는 보상을 제대로 제공할 수 있도록 앱에서 보상을 사용 하 여 구성 된는 도전 과제를 알고 있어야 합니다. 이렇게 하려면 제목 다음을 수행 해야 합니다.

1. 제목 시작 또는 앱에서 보상 잠금 해제 된 도전 과제를 확인 하 고 각각에 대 한 보상 코드를 가져오려면 일시 중단에서 제목 다시 시작 시 도전 과제 서비스를 쿼리 합니다. 이 없는 것 잠긴 제목 실행 되지 않은 동안 또는 다른 콘솔에서 모든 성과 catch 하는지 항상 수행 되어야 합니다.  

    쿼리할 Microsoft.Xbox.Services.Achievements Namespace에서 RESTful 도전 과제 Uri Uri 또는 Api를 사용할 수 있습니다.

2. 도전 과제 중 하나는 잠금 되 면 알림을 받도록 등록 합니다. 대부분의 제목에 가장 적합 하지만, 선택 사항입니다. Note 타이틀 제목 실제로 실행 하는 경우 발생 하는 잠금 해제 하는 경우이 알림을 받게만 됩니다. 이전 단계 중요 한 이유는 또 다른 이유입니다.

   도전 과제 알림을 등록 하려면 AchievementNotifier.GetTitleIdFilteredSource 메서드를 사용 합니다. 이 항목에서는 등록 하는 방법을 보여 주는 샘플 코드를 포함 합니다.