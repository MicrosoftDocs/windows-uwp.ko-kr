---
title: 사용자 시스템에서 사용자를 표시 합니다.
description: Xbox Live 사용자 시스템을 사용 하 여 사용자를 표시 하는 코드 흐름에 대 한에 알아봅니다.
ms.assetid: c97b699f-ebc2-4f65-8043-e99cca8cbe0c
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 512de51f2a0e30a9b41a5e49f3dc3ababe30fc4d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658308"
---
# <a name="display-people-from-the-people-system"></a>사용자 시스템에서 사용자를 표시 합니다.

Xbox Live 전체 서비스 해당 서비스를 소유 하는 데이터만 반환 하 고을 사용자에 게 XUID 참조만 반환 예를 들어, 사용자를 소유 하 고 사용자의 사용자 목록 및 각 해당 XUIDs (예: 즐겨찾기 상태)에 대 한 기본적인 정보에 있는 XUIDs를 반환 합니다. 현재 상태를 XUIDs의 온라인 상태 정보에 대 한 데이터를 소유합니다. 순위표를 XUIDs의 목록에 대 한 순위 정보를 소유합니다. 표시 이름 및 게이머 태그 정보는 프로필 서비스 이외의 서비스에서 반환 되지 않습니다 하 고 따라서 여러 서비스를 호출 해야 하는 환경에 있는 사람의 목록을 렌더링 합니다.

먼저 가장 필터링 할 수 있는 서비스에서 XUIDs 목록을 가져오려면 이상의 왕복을 확인 하는 서비스에 대 한 일반 호출 패턴 Api 것 또는 정렬 목록에 다음 추가 메타 데이터를 얻는 데 필요한 다른 서비스에 동시 라운드트립 호출 하는 데 필요한 각 XIUD에 대 한. 이미지의 경우 호출의 세 번째 왕복 이미지 Url에서 이미지를 얻을 수 해야 할 수 있습니다.

사람들이 사용자의 사용자 목록에 대 한 데이터를 가져오는 데 필요한 왕복 횟수를 줄이는 *모니커* 관련 서비스에 도입 되 고 있습니다. 이 새로운 기능은 서비스 사용자 서비스에서 사용자의 사용자의 목록을 구하는 하며 다음 XUIDs 집합을 사용 하 여 반환 되는 범위를 지정 하는 기본 서비스를 추상적으로 표현 하는 호출자를 허용 합니다.

다음은 제목 사용자와 관련 된 서비스에서 데이터를 가져오는 방법을 보여 주는 일부 예제에서는 호출 흐름 시나리오.

-   게임에서 현재 사용자의 목록
-   Online은 현재 사용자의 사용자 목록
-   임의 사용자가 포함 된 전체 순위표
-   사용자의 사용자는 순위표


## <a name="list-of-users-currently-in-game"></a>게임에서 현재 사용자의 목록

| 제목에  | 목표  | 렌더링 하는 필드  | 호출 흐름
|-------------------------------------------------|----------------------------------------------------|--------------------|--------------------------------------|
| 게임 내에서 다른 사용자의 임의 XUIDs 목록 | 다른 사용자에 대해 최소한의 정보를 렌더링할 | GameDisplayName \[프로필\] | XUIDs 목록을 사용 하 여 프로필을 호출 합니다. |


## <a name="list-of-the-current-users-people-who-are-online"></a>Online은 현재 사용자의 사용자 목록

## <a name="title-has"></a>제목에 있습니다.
현재 사용자의 XUID

## <a name="goal"></a>목표
현재 사용자의 사용자 목록에 있는 사용자의 온라인의 다양 한 목록을 렌더링 하려면

## <a name="field-to-render-owning-service"></a>렌더링 하는 필드 \[서비스를 소유 합니다.\]
* 즐겨 찾는 지표 [사용자]
* 사진 표시 [프로필]
* GameDisplayName [프로필]
* 기본 온라인 상태 (녹색 공) [상태]

## <a name="call-flow"></a>호출 흐름
1. 현재 상태, 각 사용자의 사용자에 대 한 XUIDs 및 온라인 상태를 가져오려면 사용자 모니커 전달를 호출 합니다.
1. 병렬로
 1. 표시 이름을 가져오고 각각에 대 한 URL를 사진 XUIDs의 전체 목록을 전달 하는 프로필을 호출 합니다.
 1. 사용자, 모든 사용자의 즐겨찾기 인지 알아보려면 XUIDs 목록을 전달를 호출 합니다.
1. 프로필을 호출 합니다.
 1. 각 그림 URL에 대 한 이미지 가져오기

## <a name="global-leaderboard-containing-random-users"></a>임의 사용자가 포함 된 전체 순위표

## <a name="title-has"></a>제목에 있습니다.
순위표의 ID/이름

## <a name="goal"></a>목표
각 사용자는 순위표에 대 한 기본 정보를 렌더링 합니다.

## <a name="field-to-render-owning-service"></a>[소유 service]를 렌더링 하는 필드
* 즐겨 찾는 지표 [사용자]
* GameDisplayName [프로필]
* 순위 [순위표]
* 점수 [순위표]

## <a name="call-flow"></a>호출 흐름
1. 특정 순위표에 대 한 가져올 XUIDs, rank, 순위표 및 점수를 호출 합니다.
1. 병렬로
 1. 표시 이름을 가져오고 각각에 대 한 URL를 사진 XUIDs 목록을 전달 프로필을 호출 합니다.
 1. 사용자, 모든 사용자의 즐겨찾기 인지 알아보려면 XUIDs 목록을 전달를 호출 합니다.

## <a name="leaderboard-of-users-people"></a>사용자의 사용자는 순위표

## <a name="title-has"></a>제목에 있습니다.
* 순위표의 ID/이름
* 현재 사용자의 XUID

## <a name="goal"></a>목표
각 사용자는 순위표에 대 한 기본 정보를 렌더링 합니다.

## <a name="field-to-render-owning-service"></a>[소유 service]를 렌더링 하는 필드
* 즐겨 찾는 지표 [사용자]
* GameDisplayName [프로필]
* 순위 [순위표]
* 점수 [순위표]

## <a name="call-flow"></a>호출 흐름
1. 사용자의 사용자 목록으로 제한 된 특정 순위표에 대 한 순위표를 가져오려면 XUIDs, rank, 사람들이 모니커를 전달 하 고 점수를 호출 합니다.
1. 병렬로
 1. 표시 이름을 가져오고 각각에 대 한 URL를 사진 XUIDs 목록을 전달 프로필을 호출 합니다.
 1. 사용자, 모든 사용자의 즐겨찾기 인지 알아보려면 XUIDs 목록을 전달를 호출 합니다.
