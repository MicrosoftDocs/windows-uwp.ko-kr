---
title: Unity에서 Leaderboard 예제 장면 사용
description: Unity 순위표 장면은 제대로 설정 하는 단계를 보여 줍니다.
ms.date: 04/24/2018
ms.topic: get-started-article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, unity, 순위표
ms.openlocfilehash: d931a0fdcbdec5dd9deb53876a19e1b475afcb93
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589748"
---
# <a name="the-leaderboard-example-scene-in-unity"></a>Unity에서 Leaderboard 예제 장면

[Unity 플러그 인](https://github.com/Microsoft/xbox-live-unity-plugin) Unity 개발자에 게 사용할 수 있는 Xbox Live 서비스를 보여 주기 위해 예제에서는 백그라운드에서 많은 호스트 합니다. 이러한 한 장면 leaderboard 예제 장면이입니다. 아래 스크린샷에 순위표를 포함 하는 패널 및 Xbox Live 사용자에 대 한 로그인 패널 leaderboard 예제 장면 단순히 표시 되는지 표시 됩니다. 에 추가 하지 않고이 시점에서 play를 누르면 로그인 패널을 보면이 장면 가짜 사용자 데이터로 채워집니다 되지만 겠죠 로드 정보가 없습니다. 이 예제에서는 장면은 실제 leaderboard 로드를 얻기 위해 몇 가지 추가 해야 합니다.

![순위표 장면은 스크린 샷](../images/unity/leaderboard-scene-1804.JPG)

## <a name="prerequisites"></a>필수 구성 요소

Xbox Live에서 순위표 통계 Xbox Live 서비스의 통계를 기반으로 합니다. 전에 해야는 일부 통계 테스트 계정과 연결 된 데이터를 사용 하 여 순위표를 채울 수 있습니다. 제목에 통계를 아직 추가 하지 않은 경우 참조 하 여 그렇게 하는 방법을 알아보십시오 [플레이어 통계 및 순위표를 Unity 프로젝트에 추가할](add-stats-and-leaderboards-in-unity.md)합니다. 해당 문서의 통계 섹션에서 작업을 수행한 후 여기로 돌아와서 예제 순위표 장면은에 상태를 표시 합니다.

## <a name="the-leaderboard-inspector"></a>순위표 검사기

순위표 prefab 순위표의 스크립트와 같은 구성 요소가 UI에 대 한 관리자 섹션에서 변경할 수 있는 설정의 개수가 *테마*, 순위표, xbox 컨트롤러 설정 연관 플레이어 및 기타 순위표 설정입니다. 순위표 설정 아래에 몇 가지 다른 섹션으로 분할 됩니다 것을 볼 수 있습니다.

![순위표 검사기 스크린 샷](../images/unity/leaderboard_script_inspector.JPG)

### <a name="theme-and-display-settings"></a>테마 및 표시 설정

이 섹션에서는 하나의 설정 호출 *테마*합니다. 간단한 드롭다운 아래쪽 prefab에 순위표에 대 한 어둡게 또는 밝게 테마를 사용할 수 있습니다. 이렇게 하면 배경, 글꼴 및를 prefab의 이미지 색 변경 됩니다. 장면 unity 플레이어에서 재생 하는 경우에 쉽게 효과 표시 됩니다.

![밝은 테마](../images/unity/leaderboard_light_theme.JPG) ![어두운 테마](../images/unity/leaderboard_dark_theme.JPG)

### <a name="stat-configuration"></a>상태 구성

이 섹션에서는 겠죠 행으로 채워질 때 검색 될 데이터의 종류를 확인할 수 있습니다.

- **플레이어 수** -는 플레이어는 순위표를 사용 하 여 연결 된 것을 나타냅니다.
- **Stat** -순위표 데이터를 채우는 데 사용 되는 상태입니다. 순위표 prefab 데이터 로드에 대 한 필수입니다.
- **순위표 형식** -이 드롭다운 메뉴는 로드 된 순위표 데이터에 필터를 적용 합니다. 하는 경우 *Global* 선택 겠죠 필터링 되지 것입니다 및 선택한 상태에 대 한 값을 사용 하 여 모든 플레이어를 표시 됩니다. 하는 경우 *친구* 선택 겠죠만 표시 플레이어에 순위표에 게 친구 목록에도 필터링 됩니다. 하는 경우 *즐겨 찾는* 선택 겠죠만 표시 플레이어는 순위표에서 즐겨찾기 목록에도 필터링 됩니다.
- **항목 수가** -1에서 100 사이의 순위표의 행 수를 한 번에 반환될지를 결정 하는 범위를 사용 하 여 슬라이더입니다. 숫자 집합 여기에 페이지당 표시 되는 순위표 행 수가 결정 됩니다.

### <a name="controller-configuration"></a>컨트롤러 구성

순위표 prefab 개발자가 Xbox 컨트롤러 사용을 쉽게 구성할 수 있습니다. 컨트롤러 구성 섹션의 prefab의 활성화 및 순위표 prefab을 제어 하는 단추를 선택 합니다. 수 있습니다.

- **컨트롤러 입력을 사용 하도록 설정** -간단한 라디오 단추 설정/해제 합니다. 이 확인란을 선택 합니다 prefab와 상호 작용 하는 Xbox 컨트롤러를 사용할 수 있습니다. 컨트롤러 지원에 필요합니다.
- **조이스틱 수** -prefab이 순위표를 사용 하 여 상호 작용할 수 있는 컨트롤러를 지정 합니다.
- **다음 페이지 컨트롤러 단추** -드롭다운 메뉴 단추 순위표 행의 다음 페이지를 로드 하는 컨트롤입니다.
- **이전 페이지 컨트롤러 단추** -드롭다운 메뉴 단추 순위표 행의 이전 페이지를 로드 하는 컨트롤입니다.
- **다음 보기 컨트롤러 단추** -간에 뷰 유형을 설정/해제 **모든** 하 고 **ME 가장 가까운**합니다.
- **이전 뷰 컨트롤러 단추** -간에 뷰 유형을 설정/해제 **모든** 하 고 **ME 가장 가까운**합니다.
- **세로 스크롤 입력 축** -어떤 컨트롤러 축이 연결 된 스크롤을 지정 하는 문자열입니다.
- **속도 승수를 스크롤하여** -컨트롤러 스크롤 속도 결정 합니다.

> [!NOTE]
> 페이지 또는 보기 순위표의 변경에 사용 되는 단추 변경 순위표의 각 함수에 사용 되는 단추와 연결 된 그림을 변경 됩니다. 그림 단추에 맞게 수동으로 UI 참조 섹션을 변경할 필요가 없습니다.

### <a name="ui-references"></a>UI 참조

이 섹션에는 이미지 및 순위표 prefab의 일반 구성을 제어합니다. 이 섹션에서는 순위표 prefab 성공적으로 사용에 대 한 변경할 필요가 없습니다. 그러나 고유한 목적를 prefab의 모양을 사용자 지정 하려면이 섹션에는 조정 해야 합니다.

## <a name="populating-the-unity-player-leaderboard-with-fake-data"></a>Unity 플레이어 순위표 (가짜 데이터로) 채우기

데이터를 사용 하 여 Unity 플레이어 순위표를 채우기 위해 순위표 prefab에 통계를 추가 해야 합니다. 순위표 보기 관리자의 prefab 줄이면 형식의 개체를 걸릴 수 있다는 것 `Stat Base` 해당 스크립트에 공용 매개 변수로 합니다. 중 하나를 끌 수는 `State Base` prefabs 입력 `IntegerStat`를 `DoubleStat`, 또는 `StringStat` prefabs 폴더의 Xbox Live Unity 플러그 인에서 prefab 순위 표에서이 위치에 배치 합니다.

![끌어서 놓기 Stat Prefab](../images/unity/stat-to-leaderbaord-drag.gif)

이제 unity에서 장면을 재생 하 고 아래와 같은 가짜 데이터가 채워진 겠죠 채워져 있는지 확인할 수 있습니다.

![가짜 데이터가 채워진 순위표 스크린 샷](../images/unity/leaderboard-fake-data-1804.JPG)

## <a name="populating-a-visual-studio-built-project-with-real-data"></a>실제 데이터를 사용 하 여 프로젝트를 빌드할 Visual Studio 채우기

제목에 대 한 실제 데이터로 순위표를 채우기 위해 컴퓨터에서 로컬로 실행 하 여 게임을 작성 해야 합니다. Unity 편집기에 Xbox Live에 액세스할 수 없기 때문에 로컬 빌드를 해야 합니다. 로컬로 실행 하도록 프로젝트를 만드는 것 외에 상태에 순위표 초기화 되 고 값을 제목에는 stat에서 구성 해야 합니다. 프로그램 순위표를 stat를 연결 하기 위해 stat 순위표 prefab 개체의 표시 이름과 ID를 수정 해야 합니다. ID를 구성 하는 상태와 일치 해야 합니다 [파트너 센터](https://partner.microsoft.com/dashboard)합니다. 이 작업을 수행한 후에 설명 된 대로 프로젝트를 빌드할 합니다 [빌드 섹션 Unity 문서의 Xbox Live 구성의](configure-xbox-live-in-unity.md#build-and-test-the-project)합니다. X64로이 프로젝트를 실행 합니다. 로컬 컴퓨터를 대상으로 하는 빌드를 사용 하 여를 로그인을 실제 게이머 태그를 사용 하 여 실제 데이터로 순위표를 채웁니다.