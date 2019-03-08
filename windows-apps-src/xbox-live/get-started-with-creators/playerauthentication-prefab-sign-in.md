---
title: PlayerAuthentication Prefab으로 로그인
description: Unity 플러그인 PlayerAuthentication Prefab의 개요
ms.date: 05/08/2018
ms.topic: get-started-article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, unity
ms.openlocfilehash: ea161ff801e2004569d88d53c78ae963e91b4ce6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605738"
---
# <a name="easy-sign-in-with-the-playerauthentication-prefab"></a>간편한 로그인 PlayerAuthentication Prefab을 사용 하 여

PlayerAuthentication prefab은 Xbox Live 인증을 제목을 추가 하는 가장 쉬운 방법은 구성 합니다. 만 세 가지 간단한 단계로 새 장면에서의 로그인 페이지로 이동 됩니다.

1. 장면 PlayerAuthentication prefab 끌어
2. 장면 XboxLiveServices prefab을 끌어
3. EventSystem (PlayerAuthentication 기술적 만들어집니다 하나는 EventSystem 없는 경우는 습관도 추가 합니다.) 장면에 추가

됐습니다. 로그인 할 수 있습니다 이제 플레이어 XboxLive에 제목 PlayerAuthentication prefab 장면에서를 클릭 하 여 합니다. 재생 단추를 클릭 하 여 Unity 장면 테스트 하면 모조 데이터를 생성 하 여 prefab, 이므로이 Unity 플레이어 Xbox Live 서비스에 연결할 수 없습니다. 실제 로그인을 확인 하기 위해 Visual Studio에서 로컬로 실행 하도록 프로젝트를 작성 해야 합니다. 제목에 구성 된 경우 파트너 센터 하는 Microsoft 계정/게이머 태그를 제목에 로그인 할 권한이 다음 로그인 Visual Studio 빌드 시에서 권한 있는 계정 중 하나를 수 있습니다.

PlayerAuthentication prefab의 스크립트에 관리자에서 해당 보기에서 조작할 수 있는 몇 가지 설정이 있습니다.

![PlayerAuthentication 검사기 스크린 샷](../images/unity/playerauthentication_prefab_inspector.JPG)

* 플레이어 수-로그인 패널에 연결 된 플레이어를 결정
* 테마-사용자가 로그인 또는 로그 아웃 [로그인] 패널에 대 한 색 구성표를 변경 합니다. 이 설정에는 밝은 테마 또는 어두운 옵션이 있습니다.
* 사용할 수 있게 컨트롤러 입력-플레이어를 위해이 확인란을 사용 하는 Xbox 컨트롤러를 로그인 하 고 PlayerAuthentication prefab을 사용 하 여 로그 아웃 합니다.
* 조이스틱 Number-로그인 할 수는 제한 하는 컨트롤러를 결정 합니다 prefab을 사용 하 여 합니다.
* 로그인 단추-는 드롭다운을 사용 하면 사용자를 로그인으로 Xbox 컨트롤러에는 단추를 선택할 수 있습니다.
* 로그 아웃 단추-는 드롭다운을 사용 하면 Xbox 컨트롤러에는 단추 사용자 로그 아웃을 선택할 수 있습니다.

## <a name="multiplayer-scenario"></a>멀티 플레이 시나리오

단일 플레이어 로그인 외에도 Xbox One 콘솔 제목에서 로컬 멀티 플레이 게임을 구현 하려면 여러 PlayerAuthentication prefabs을 사용할 수 있습니다. prefab의 여러 인스턴스를 추가 하 고 각 플레이어 수 속성을 변경 하 여 프로그램 제목에 여러 사용자에 서명할 수 있습니다.

> [!WARNING]
> 로그인 여러 게이머 태그는 Windows 10 Pc에서 허용 되지 않습니다. 여러 사용자가 로그인 하기 위해는 Xbox 1 본체에서 게임을 테스트 해야 합니다.

멀티 플레이 있도록 장면 만들기 어렵습니다만 치중 PlayerAuthentication prefab을 사용 하 여 합니다.

1. 장면 PlayerAuthentication prefab의 인스턴스를 끌어
2. 확인 합니다 **컨트롤러 입력을 사용 하도록 설정** prefab의 검사기에서 상자입니다.
3. 있는지 확인 합니다 **플레이어 수** 하 고 **조이스틱 수** 1로 설정 됩니다.
4. 할당 된 **기호에 단추** 드롭다운 메뉴에서에서.
5. 할당 된 **기호 아웃 단추** 드롭다운 메뉴에서에서.
6. 끌어서를 *두 번째* 장면에 PlayerAuthentication prefab의 인스턴스.
7. 확인 합니다 **컨트롤러 입력을 사용 하도록 설정** prefab의 검사기에서 상자입니다.
8. 있는지 확인 합니다 **플레이어 수** 하 고 **조이스틱 수** 2로 설정 됩니다.
9. 할당 된 **기호에 단추** 드롭다운 메뉴에서에서.
10. 할당 된 **기호 아웃 단추** 드롭다운 메뉴에서에서.
11. 장면 XboxLiveServices prefab을 끌어
12. 장면에는 EventSystem 추가

prefabs Unity 플레이어에서 키를 눌러 재생 하 여 제대로 작동 되는 prefabs 클릭 지 확인 합니다. Unity 플레이어 Xbox Live에 연결할 수 없습니다. 예상 되는 가짜 데이터가 반환 됩니다. 다른 플레이어와 하므로 Visual Studio에서 게임을 빌드할 준비가 되었습니다. 조이스틱 하도록 PlayerAuthentication prefab의 두 인스턴스를 사용 하 여 테스트할 수 있습니다 제대로 Xbox 콘솔에서. 게임 빌드되면 게임에 대 한 다중 사용자 지원을 사용 하도록 설정 해야 하는 Visual Studio에서 솔루션 파일을 엽니다.
이렇게 하려면 다음을 수행합니다.

1. Package.appxmanifest.xml 파일에 대 한 솔루션 탐색기 검색
2. 파일을 마우스 오른쪽 단추로 클릭 하 고 코드 보기를 선택 합니다.
3. 아래는 `<Properties><\/Properties>` 섹션에서 다음 줄을 추가 합니다. ' < uap:SupportedUsers > 여러 <\/uap:SupportedUsers >.
4. Visual Studio에서 원격 디버깅 빌드를 시작 하 여 Xbox로 게임을 배포 합니다. Xbox에 제목을 설정 하는 지침을 찾을 수 있습니다 합니다 [Xbox 개발 환경에서 프로그램 UWP 설정](../../xbox-apps/development-environment-setup.md) 문서.

> [!NOTE]
> 구성이 변경 되었습니다. 부분에는 여전히 단일 플레이어 시나리오에서 게임을 실행 해야 하는 멀티 플레이어를 사용 하는 것 처럼 보일 수 있습니다.

했으면는 PlayerAuthentication prefab 작동 수에 대 한 자세한 스크립팅 로그인 아티클로 [Unity의 로그인 관리자를 사용 하 여 한 로그인](sign-in-manager.md)합니다.