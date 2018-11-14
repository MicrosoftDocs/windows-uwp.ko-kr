---
title: Xbox Live 프리 팹에 컨트롤러 지원 추가
author: KevinAsgari
description: Xbox에 Live 프리 팹을는 Xbox Live Unity 플러그 인을 사용 하 여에 컨트롤러 지원 추가
ms.assetid: ''
ms.author: heba
ms.date: 07/14/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, unity, 컨트롤러 지원
ms.localizationpriority: medium
ms.openlocfilehash: b4fb8f710a04b2243f32daadc652aca8404d4577
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6154882"
---
# <a name="add-controller-support-to-xbox-live-prefabs"></a>Xbox Live 프리 팹에 컨트롤러 지원 추가

> [!IMPORTANT]
> Xbox Live Unity 플러그 인 도전 과제 또는 온라인 멀티 플레이어 지원 하지 않으며만 [Xbox Live 크리에이터 스 프로그램](../developer-program-overview.md) 구성원에 대 한 권장 합니다.

Xbox Live Unity 플러그 인 프리 팹의 모든 관리자에서 지정 컨트롤러 입력을 지원합니다.

예를 들어 있다고 가정해 보겠습니다 라고 하는 게임 개체 `UserProfile1` 기반으로 합니다 `UserProfile` prefab 합니다. 플레이어 1이 게임 개체를 연결 하 고 사용 하 여 로그인 하도록 하려는 경우는 `A` 자신의 Xbox 컨트롤러에서 단추를 간단 하 게 작성할 `joystick 1 button 0` 에 `Input Controller Button` 필드에 관리자.

  ![사용자 프로필 프리 팹의 컨트롤러 지원](../images/unity/controller-support-example.png)

## <a name="all-prefab-controller-input-fields"></a>모든 Prefab 컨트롤러 입력된 필드
### <a name="userprofile-prefab"></a>사용자 프로필 프리 팹
- **컨트롤러 단추 입력:** 추가 하 고 Xbox Live 사용자가 로그인 합니다.

### <a name="social-prefab"></a>소셜 프리 팹
- **필터 컨트롤러 단추:** '모두' 친구 또는 '온라인' 친구를 표시 하려면 필터를 전환 합니다.

### <a name="leaderboard-prefab"></a>순위표 프리 팹
- **첫 번째 컨트롤러 단추:** 순위표 항목의 첫 번째 페이지에 플레이어를 이동합니다.
- **컨트롤러 단추 마지막:** 플레이어가 순위표 항목의 마지막 페이지로 이동합니다.
- **다음 컨트롤러 단추:** 플레이어가 순위표 항목의 다음 페이지를 사용합니다.
- **이전 컨트롤러 단추:** 플레이어가 순위표 항목의 이전 페이지로 이동합니다.
- **새로 고침 컨트롤러 단추:** 순위표 보기를 새로 고칩니다.


### <a name="game-save-ui-prefab"></a>게임 저장 UI 프리 팹
- **새 컨트롤러 단추 생성:** 저장 데이터 새 정수를 생성합니다.
- **저장 데이터 컨트롤러 단추:** 연결 된 저장소에 현재 데이터를 저장합니다.
- **데이터 컨트롤러 단추 로드:** 현재 연결 된 저장소에 저장 된 데이터를 로드 합니다.
- **정보 컨트롤러 단추:** 연결 된 저장소에 저장 된 컨테이너에 대 한 정보를 검색합니다.
- **컨테이너 컨트롤러 단추를 삭제:** 연결 된 저장소에서 저장된 된 컨테이너를 삭제합니다.

## <a name="xbox-controller-button-mappings"></a>Xbox 컨트롤러 단추 매핑

Unity에서 Xbox 컨트롤러 버튼 매핑을에 대 한이 [Unity 컨트롤러 Wiki 페이지](http://wiki.unity3d.com/index.php?title=Xbox360Controller)를 확인 합니다.
