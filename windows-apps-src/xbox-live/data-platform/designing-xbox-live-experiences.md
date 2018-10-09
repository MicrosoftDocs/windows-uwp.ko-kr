---
title: Xbox Live 경험을 디자인합니다.
author: KevinAsgari
description: 플레이어 통계, 순위표 및 도전 과제 타이틀에 대 한 계획 하 여 Xbox Live 구성원에 대 한 멋진 환경을 설계 하는 방법을 알아봅니다.
ms.assetid: d2a85621-7d02-4b11-a7fa-9ebaee0092d5
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 통계, 도전 과제, 순위표, 디자인
ms.localizationpriority: medium
ms.openlocfilehash: 112beacc2009f495da64475a0740560b7271b43d
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4417482"
---
# <a name="designing-xbox-live-experiences"></a>Xbox Live 경험을 디자인합니다.

이 항목에 대해 생각 하 고 게임에 Xbox Live 데이터 플랫폼 기능을 사용 하 여 사용자 통계, 도전 과제 및 순위표를 추가 하는 샘플 과정을 안내 합니다.

## <a name="example"></a>예
보여 주는 데이터 플랫폼 돕는 방법 Xbox Live 놀라운 환경을 조명 Xbox One 대시보드에서 Windows 10의 Xbox 앱 및 게임에는 가상의 게임을 구성 해 보겠습니다. 이 시나리오에서는 레이싱 게임 _경합이 2015_라는 가상 협력 합니다.

이러한 경험에서 대부분의 값을 얻기 위해 이러한 권장 계획 단계를 수행 합니다 했습니다.
1. XBL 환경 디자인
2. 디자인 하는 시나리오를 하는 데 필요한 통계
3. 필요에 따라 기능을 갖춘 통계, 도전 과제, 순위표 구성


## <a name="1-design-your-xbox-live-experiences"></a>1. Xbox Live 환경 디자인
_경합 2015_ 의 내부와 외부 게임에 참여 하는 사용자를 유지 하기 위해 Xbox Live 최대한 활용을 가져오려면 원하는 것입니다. 첫 번째 질문을 다음과 같습니다.

1. 수행할 작업 처럼 우리의 GameHub 도전 과제 탭 경험 우리가 원하는? (주요 통계 및 순위표 소셜)
2. 어떤 목표와 동기 게임에서 작업을 수행 하 고 플레이어에 게 하려는 것? (도전 과제)
3. 어떤 점수 또는 통계 플레이어가 게임에서 다른 플레이어 자체의 순위를 사용 하 여 하겠습니다. (순위표)


### <a name="gamehubs---featured-statistics-and-social-leaderboards"></a>주요 통계 및 순위표 소셜 GameHubs-
게임에 대 한 모든 사용자가 알아볼 수 있는 환경을 GameHubs에 방문 됩니다. 개발자 및/또는 게시자,이 한 _쇼케이스_ 좋은 방법에 대 한 완벽 한 장소 및 풍부한 게임은 게임 아직 구입 했는지 Xbox 사용자에 게 합니다. GameHubs 표시 하도록 진행률과 매력적인 메트릭을 게이머를 제목 이미 소유 하는 설계 되었습니다. 도전 과제 탭에서 사용자 게임 진행률 및 도전 과제, 영웅 통계 및 순위표 소셜 찾을 수 있습니다.

이 섹션의이 목표 게임에 대 한 가장 매력적인 메트릭을 캡처 및 게임을 사용 하 여 플레이어의 경험의 고유한 그림을 제공 하 고 소셜 순위표에서 친구 들에 대 한 순위를 지정 하 게 노출 하는 것입니다. 예를 들어, Forza 6 도전 과제 탭은 다음과 같이 표시 수 있습니다.

![Gamehub 이미지](../images/omega/forza_gamehub.png)


거리 기반 및 첫 번째 장소 완료 골드, 실버, 또는 청동 장식 친구에 대 한 플레이어의 순위가 여기서 설명 하는 등 일부 통계를 알 수 있습니다. 이러한 통계 조작 같이 전체 순위표를 표시 됩니다.

![순위표](../images/omega/progress_gamehub_lb.png)

 _경합 2015_, 다음은 좋은 순위 메트릭 뿐만 아니라 제목 플레이어의 경험을 나타내는 통계 집합이 좋은 생각:
 * 랩
 * 대부분의 1 일부 터 장소 wins
 * 운전


### <a name="achievements"></a>도전 과제
도전 과제는는 시스템 수준 메커니즘 지시 및 모든 게임에서 일관 된 방식으로 사용자의 게임에서 작업을 효과적입니다. 이 작업을 올바르게 디자인 가이드 사용자가 게임에서 최대한 하는 데 도움이 되 고 게임의 수명을 확장 합니다.

_경합 2015_, 구성 하려는 도전 과제의 하위 집합 다음과 같습니다.
* 랩 1 60 초 이내에 완료
* 1에서 10 경합이 완료
* 최소한 1 경합 각 트랙 완료
* 자체 5 자동차
* 10,000 거리에 대 한 드라이브
* ...


###  <a name="leaderboards"></a>순위표
순위표는 게이머에 대해 다른 게이머는 게임에서 가능 특정 작업에 대 한 자체 평가 하는 방법을 제공 합니다. 외에 GameHubs 소셜 순위표, 전역 순위표 게임에서 사용 하도록 구성할 수 있습니다. 다음은 일부는 순위표에서 사용자의 모든 순위 하려고 합니다.

* 가장 빠른 랩 시간
 * 메타 데이터: 발생이 추적
 * 메타 데이터: 자동차 사용
 * 메타 데이터: 날씨 조건
* 대부분의 1 일부 터 장소 wins
* 대부분의 자동차 수집

## <a name="next-steps"></a>다음 단계
이제 효과적으로 통계, 순위표 및 도전 과제를 디자인 하는 방법을 알게 타이틀에서 이러한 구현을 시작할 이제 시작할 수 있습니다.  다음의 몇몇 섹션 개발자 센터에서 구성으로 시작 하는 종단 간 프로세스를 설명 합니다.

[플레이어 통계](../leaderboards-and-stats-2017/player-stats.md) 를 참조 하세요.