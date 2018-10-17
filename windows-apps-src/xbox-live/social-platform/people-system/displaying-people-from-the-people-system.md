---
title: 사용자 시스템에서 사용자를 표시 합니다.
author: KevinAsgari
description: Xbox Live 사용자 시스템을 사용 하 여 사용자를 표시 하는 코드 흐름에 대해를 알아봅니다.
ms.assetid: c97b699f-ebc2-4f65-8043-e99cca8cbe0c
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fc6fd23d9113c4d0e3dbac6d3cf0c880fc71d6a9
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4749932"
---
# <a name="display-people-from-the-people-system"></a>사용자 시스템에서 사용자를 표시 합니다.

Xbox Live 전체 서비스 해당 서비스에서 소유 하는 유일한 데이터를 반환 하 고; 사용자에 게 XUID 참조만 반환 예를 들어, 사용자 서비스만 소유 하 고 사용자의 사용자 목록과 각 해당 XUIDs (예: 즐겨찾기 상태)에 대 한 몇 가지 기본적인 정보에 있는 XUIDs를 반환 합니다. 현재 상태 서비스 XUIDs의 온라인 상태 정보에 대 한 데이터를 소유합니다. 순위표 서비스 XUIDs의 목록에 대 한 순위 정보를 소유합니다. 표시 이름 및 게이머 정보는 프로필 서비스 이외의 모든 서비스에서 반환 되지 않습니다 이며, 따라서 여러 서비스 호출의 환경에서 사용자 목록을 렌더링 하는 데 필요한 합니다.

서비스에 대 한 일반 호출 패턴 Api 한 왕복 먼저 XUIDs 목록을 가장 잘 필터링 할 수 있는 서비스에서 얻을 수 있도록 인지 목록 정렬 다음 동시 왕복 호출 한 추가 메타 데이터를 가져오는 데 필요한 다른 서비스에 필요 각 XIUD에 대 한. 이미지의 경우 호출에 대 한 세 번째 왕복 이미지 Url에서 이미지를 얻으려면 필요할 수 있습니다.

사용자의 사용자 목록에 대 한 데이터를 얻는 데 필요한 왕복 수를 줄이기 위해 사용자 *모니커* 관련 서비스를 도입 되는 합니다. 이 새로운 기능 호출자를 서비스 사용자 서비스에서 사용자의 사람 목록이 받아야 하며 다음 해당 XUIDs 집합을 사용 하 여 반환 되는 범위를 지정 하는 기본 서비스 추상적으로 표현할 수 있습니다.

다음은 제목 사용자와 관련 된 서비스에서 데이터를 가져오는 방법을 보여 주는 몇 가지 예제 호출 흐름 시나리오입니다.

-   게임의 현재 사용자 목록
-   온라인 인 현재 사용자의 사용자 목록
-   임의 사용자를 포함 하는 전역 순위표
-   사용자의 사람의 순위표


## <a name="list-of-users-currently-in-game"></a>게임의 현재 사용자 목록

| 제목에 있습니다  | 목표  | 렌더링 하는 필드  | 호출 흐름
|-------------------------------------------------|----------------------------------------------------|--------------------|--------------------------------------|
| 게임에서 다른 사용자의 임의 XUIDs 목록 | 각 다른 사용자에 대 한 최소한의 정보를 렌더링 하려면 | GameDisplayName \[Profile\] | XUIDs 목록을 사용 하 여 프로필을 호출 합니다. |


## <a name="list-of-the-current-users-people-who-are-online"></a>온라인 인 현재 사용자의 사용자 목록

## <a name="title-has"></a>제목에 있습니다.
현재 사용자의 XUID

## <a name="goal"></a>목표
현재 사용자의 사용자 목록에 있는 온라인 사용자 목록을 풍부한 렌더링

## <a name="field-to-render-owning-service"></a>\[Owning service\ 렌더링 필드]
* 즐겨찾기 표시기 [사람]
* 공개 사진 [프로필]
* GameDisplayName [프로필]
* 기본 온라인 상태 (녹색 구슬이) [존재]

## <a name="call-flow"></a>호출 흐름
1. 각 사용자의 사용자에 대 한 XUIDs 및 온라인 상태를 가져오려면 사용자 모니커 전달 하 여 현재 상태를 호출 합니다.
1. 병렬로:
 1. 프로필을 전달 하 여 전체 목록을 표시 이름을 가져올 및 각 URL 그림 XUIDs 호출 합니다.
 1. 사용자, 사용자의 즐겨찾기가 되어 있는지 알아보려면 XUIDs 목록을 전달 하 여 호출 합니다.
1. 프로필을 호출 합니다.
 1. 각 그림 URL에 대 한 이미지 가져오기

## <a name="global-leaderboard-containing-random-users"></a>임의 사용자를 포함 하는 전역 순위표

## <a name="title-has"></a>제목에 있습니다.
순위표 I d/이름

## <a name="goal"></a>목표
각 사용자는 순위표에 대 한 기본 정보를 렌더링 하려면

## <a name="field-to-render-owning-service"></a>[소유 서비스]를 렌더링 하는 필드
* 즐겨찾기 표시기 [사람]
* GameDisplayName [프로필]
* 순위 [순위표]
* [순위표] 점수

## <a name="call-flow"></a>호출 흐름
1. 특정 순위표 가져오려면 XUIDs, 등급, 순위표 및 점수 호출 됩니다.
1. 병렬로:
 1. 표시 이름을 가져올 및 각 URL 그림 XUIDs 목록을 전달 하 여 프로필을 호출 합니다.
 1. 사용자, 사용자의 즐겨찾기가 되어 있는지 알아보려면 XUIDs 목록을 전달 하 여 호출 합니다.

## <a name="leaderboard-of-users-people"></a>사용자의 사람의 순위표

## <a name="title-has"></a>제목에 있습니다.
* 순위표 I d/이름
* 현재 사용자의 XUID

## <a name="goal"></a>목표
각 사용자는 순위표에 대 한 기본 정보를 렌더링 하려면

## <a name="field-to-render-owning-service"></a>[소유 서비스]를 렌더링 하는 필드
* 즐겨찾기 표시기 [사람]
* GameDisplayName [프로필]
* 순위 [순위표]
* [순위표] 점수

## <a name="call-flow"></a>호출 흐름
1. 사용자의 사용자 목록으로 제한 하는 특정 순위표 XUIDs, 순위를 가져오려면 사용자 모니커 전달, 순위표 및 점수 호출 됩니다.
1. 병렬로:
 1. 표시 이름을 가져올 및 각 URL 그림 XUIDs 목록을 전달 하 여 프로필을 호출 합니다.
 1. 사용자, 사용자의 즐겨찾기가 되어 있는지 알아보려면 XUIDs 목록을 전달 하 여 호출 합니다.
