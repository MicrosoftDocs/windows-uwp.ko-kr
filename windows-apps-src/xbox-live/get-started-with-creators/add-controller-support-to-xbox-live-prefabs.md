---
title: Xbox Live Prefabs에 컨트롤러 지원 추가
description: Xbox Live Unity 플러그 인을 사용 하 여 Xbox Live Prefabs에 컨트롤러 지원 추가
ms.assetid: ''
ms.date: 07/14/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, unity, 컨트롤러 지원
ms.localizationpriority: medium
ms.openlocfilehash: 4d32ec62b8beec10256ed9a695866c2fd9bdd03e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622878"
---
# <a name="add-controller-support-to-xbox-live-prefabs"></a>Xbox Live prefabs에 컨트롤러 지원 추가

> [!IMPORTANT]
> Xbox Live Unity 플러그 인 성과 또는 멀티 플레이어 온라인 게임을 지원 하지 않으며에 권장 됩니다 [Xbox Live 크리에이터 스 프로그램](../developer-program-overview.md) 멤버입니다.

Xbox Live Unity 플러그 인 Prefabs의 모든 관리자에 지정 컨트롤러 입력을 지원합니다.

예를 들어 있다고 가정해 보겠습니다 호출 하는 게임 개체 `UserProfile1` 기준으로 하는 `UserProfile` prefab 합니다. 플레이어 1이 게임 개체를 연결 하 고 사용 하 여 로그인 하려는 경우는 `A` 가 Xbox 컨트롤러 단추, 간단 하 게 작성할 `joystick 1 button 0` 에 `Input Controller Button` 검사자의 필드입니다.

  ![UserProfile Prefab의 컨트롤러 지원](../images/unity/controller-support-example.png)

## <a name="all-prefab-controller-input-fields"></a>모든 Prefab 컨트롤러 입력된 필드
### <a name="userprofile-prefab"></a>UserProfile prefab
- **입력된 컨트롤러 Button:** 추가 하 고 Xbox Live 사용자를 로그인 합니다.

### <a name="social-prefab"></a>소셜 prefab
- **설정/해제 필터 컨트롤러 단추:** '모두' 친구 또는 '온라인' 친구를 표시 하도록 필터를 설정/해제 합니다.

### <a name="leaderboard-prefab"></a>순위표 prefab
- **첫 번째 컨트롤러 단추:** 플레이어 순위표 항목의 첫 페이지를 사용합니다.
- **마지막 컨트롤러 단추:** 플레이어 순위표 항목의 마지막 페이지를 사용합니다.
- **다음 컨트롤러 Button:** 플레이어 순위표 항목의 다음 페이지를 사용합니다.
- **이전 컨트롤러 Button:** 플레이어 순위표 항목의 이전 페이지를 사용합니다.
- **컨트롤러 단추를 새로 고칩니다.** 순위표 보기를 새로 고칩니다.


### <a name="game-save-ui-prefab"></a>게임 저장 UI prefab
- **새 컨트롤러 단추를 생성 합니다.** 데이터를 저장 하는 새 정수를 생성합니다.
- **데이터 컨트롤러 단추를 저장 합니다.** 연결 된 저장소에 현재 데이터를 저장합니다.
- **데이터 컨트롤러 단추를 로드 합니다.** 현재 연결 된 저장소에 저장 된 데이터를 로드 합니다.
- **정보 컨트롤러 단추를 가져옵니다.** 연결 된 저장소에 저장 된 컨테이너에 대 한 정보를 검색합니다.
- **컨트롤러 [컨테이너] 단추를 삭제 합니다.** 연결 된 저장소에서 저장된 된 컨테이너를 삭제합니다.

## <a name="xbox-controller-button-mappings"></a>Xbox 컨트롤러 단추 매핑

Unity에서 Xbox 컨트롤러 단추 매핑의 확인 합니다 [Unity 컨트롤러 Wiki 페이지](https://wiki.unity3d.com/index.php?title=Xbox360Controller)합니다.
