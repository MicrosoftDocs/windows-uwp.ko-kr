---
title: Windows 10 용 Powertoy Run 유틸리티
description: 성능을 저하 시 키 지 않고 몇 가지 추가 기능을 포함 하는 고급 사용자를 위한 빠른 시작 관리자입니다.
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ce71ac5f4667952be8beb790b0890aadd0d8eb54
ms.sourcegitcommit: a1b251971f7ac574275d53bbe3e9ef4a3a9dc15c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2021
ms.locfileid: "103417114"
---
# <a name="powertoys-run-utility"></a>Powertoy Run 유틸리티

Powertoy Run은 성능을 저하 시 키 지 않고 몇 가지 추가 기능을 포함 하는 고급 사용자를 위한 빠른 시작 관리자입니다. 오픈 소스이며 추가 플러그 인을 위한 모듈식입니다.

Powertoy 실행을 사용 하려면 <kbd>Alt</kbd> + <kbd>Space</kbd> 를 선택 하 고 입력을 시작 합니다.

*바로 가기가 마음에 들지 않으면 설정에서 완전히 구성할 수 있습니다.*

![Powertoy 실행 데모 앱 열기](../images/pt-powerrun-demo.gif)

## <a name="requirements"></a>요구 사항

- Windows 10 버전 1903 이상
- 를 설치한 후에는이 유틸리티가 작동 하기 위해 백그라운드에서 Powertoy를 사용 하도록 설정 하 고 실행 해야 합니다.

## <a name="features"></a>기능

Powertoy 실행 기능은 다음과 같습니다.

- 응용 프로그램, 폴더 또는 파일 검색

- 실행 중인 프로세스 (이전에는 [WindowWalker](https://github.com/betsegaw/windowwalker/)로 알려짐)를 검색 합니다.

- 바로 가기 키가 있는 클릭 가능한 단추 (예: *관리자 권한* 또는 *포함 하는 폴더 열기*)

- 를 사용 하 여 셸 플러그 인 호출 `>`  (예: `> Shell:startup` Windows 시작 폴더를 열 때)

- 계산기를 사용 하 여 간단한 계산 수행

## <a name="settings"></a>설정

Powertoy 설정 메뉴에서 다음 실행 옵션을 사용할 수 있습니다.

  | **설정** |**작업** |
  | --- | --- |
  | Powertoy 실행 열기 | Powertoy 실행을 열거나 숨기도록 바로 가기 키를 정의 합니다. |
  | 전체 화면 모드에서 바로 가기 무시 |  전체 화면 (F11)에서 실행 되는 경우 바로 가기를 사용 하지 않습니다. |
  | 최대 결과 수 |  스크롤 없이 표시 되는 최대 결과 수 |
  | 시작 시 이전 쿼리 지우기 | 이전 검색은 시작 시 강조 표시 되지 않습니다. |
  | 드라이브 검색 사용 안 함 경고 | 모든 드라이브가 인덱싱되지 않은 경우 경고는 더 이상 표시 되지 않습니다. |

## <a name="keyboard-shortcuts"></a>바로 가기 키

  | **바로 가기** | **작업** |
  | --- | --- |
  | Alt + 스페이스바 | Powertoy 실행을 열거나 숨깁니다. |
  | Esc | Powertoy 실행 숨기기 |
  | Ctrl+Shift+Enter | (응용 프로그램에만 적용 됨) 선택한 응용 프로그램을 관리자 권한으로 엽니다. |
  | Ctrl+Shift+E | (응용 프로그램 및 파일에만 적용 됨) 파일 탐색기에서 포함 하는 폴더 열기 |
  | Ctrl+C | (폴더 및 파일에만 적용 됨) 경로 위치 복사 |
  | 탭 | 검색 결과 및 상황에 맞는 메뉴 단추를 탐색 합니다. |

## <a name="action-key"></a>작업 키

이러한 기본 활성화 구는 Powertoy가 대상 플러그인 으로만 실행 되도록 합니다.

  | **작업 키** | **작업** |
  | --- | --- |
  | `=` | 계산기만 해당 합니다. 예제 `=2+2` |
  | `?` | 파일만 검색 합니다. `?road`찾기 위한 예`roadmap.txt` |
  | `.` | 설치 된 프로그램만 `.code`Visual Studio Code을 가져오는 예제입니다. 프로그램 시작에 매개 변수를 추가 하는 옵션은 [프로그램 매개 변수](#program-parameters) 를 참조 하세요. |
  | `//` | Url만. `//docs.microsoft.com`기본 브라우저가 이동 하는 예https://docs.microsoft.com |
  | `<` | 실행 중인 프로세스만 `<outlook`Outlook을 포함 하는 모든 프로세스를 찾는 예제 |
  | `>` | Shell 명령만. `>ping localhost`Ping 쿼리를 수행 하는 예제 |
  | `:` | 레지스트리 키만 `:hkcu`HKEY_CURRENT_USER 레지스트리 키를 검색 하는 예제 |
  | `!` | Windows 서비스만 해당 됩니다. `!alg`시작 또는 중지할 응용 프로그램 계층 게이트웨이 서비스를 검색 하는 예제 |

## <a name="system-commands"></a>시스템 명령

Powertoy Run은 실행할 수 있는 시스템 수준 작업 집합을 활성화 합니다.

  | **작업 키**   |   **작업** |
  | ------------------ | ---------------------------------------------------------------------------------|
  | `Shutdown` | 컴퓨터를 종료 합니다. |
  | `Restart` | 컴퓨터를 다시 시작 합니다. |
  | `Sign Out` | 현재 사용자를 로그 아웃 합니다. |
  | `Lock` | 컴퓨터를 잠급니다. |
  | `Sleep` | 컴퓨터를 중지 합니다. |
  | `Hibernate` | 컴퓨터의 절전 모드를 |
  | `Empty Recycle Bin` | 휴지통을 비웁니다. |

## <a name="plugin-manager"></a>플러그 인 관리자

Powertoy v 0.33 및 on을 사용 하는 경우, Powertoy 실행 설정 메뉴에는 현재 사용할 수 있는 여러 플러그 인을 활성화/비활성화할 수 있는 플러그 인 관리자가 포함 되어 있습니다. 섹션을 선택 하 고 확장 하 여 각 플러그 인에서 사용 하는 활성화 문구를 사용자 지정할 수 있습니다. 또한 플러그 인이 전역 결과에 표시 되는지 여부와 사용 가능한 경우 추가 플러그 인 옵션을 설정할 수 있습니다. 

## <a name="program-parameters"></a>프로그램 매개 변수

Powertoy v 0.33 이상에서 Powertoy Run 프로그램 플러그 인을 사용 하면 응용 프로그램을 시작할 때 프로그램 인수를 추가할 수 있습니다. 프로그램 인수는 프로그램의 명령줄 인터페이스에 정의 된 대로 필요한 형식 이어야 합니다.

예를 들어 Visual Studio Code를 시작 하는 경우 다음을 사용 하 여 열 폴더를 지정할 수 있습니다.

`Visual Studio Code -- C:\myFolder`

또한 Visual Studio Code는 [명령줄 매개 변수](https://code.visualstudio.com/docs/editor/command-line)집합을 지원 하며, 예를 들어 파일 간의 차이점을 보기 위해 powertoy 실행에서 해당 인수를 사용할 수 있습니다.

`Visual Studio Code -d C:\foo.txt C:\bar.txt` 

프로그램 플러그 인의 "글로벌 결과에 포함" 옵션을 선택 하지 않은 경우에는 기본적으로 활성화 문구를 포함 하 여 `.` 플러그 인 동작을 호출 해야 합니다.

`.Visual Studio Code -- C:\myFolder`

## <a name="windows-search-settings"></a>Windows 검색 설정

Windows Search 플러그 인이 모든 드라이브를 포함 하도록 설정 되지 않은 경우 다음과 같은 경고가 표시 됩니다.

![Powertoy 인덱서 실행 경고](../images/pt-run-warning.png)

Powertoy Run plugin manager options for Windows Search에서 경고를 해제 하거나 경고를 선택 하 여 인덱싱되는 드라이브를 확장할 수 있습니다. 경고를 선택 하면 Windows 10 설정 "Windows 검색" 옵션 메뉴가 열립니다.

![인덱싱 설정](../images/pt-run-indexing.png)

이 "Windows 검색" 메뉴에서 다음을 수행할 수 있습니다.

- Windows 10 컴퓨터의 모든 드라이브에서 인덱싱을 사용 하도록 설정 하려면 "고급" 모드를 선택 합니다.
- 제외할 폴더 경로를 지정 하십시오.
- 메뉴 옵션의 아래쪽에 있는 "고급 검색 인덱서 설정"을 선택 하 여 고급 인덱스 설정을 설정 하 고, 검색 위치를 추가 또는 제거 하 고, 암호화 된 파일을 인덱싱합니다.

![고급 인덱싱 설정](../images/pt-run-indexing-advanced.png)

## <a name="known-issues"></a>알려진 문제

알려진 모든 문제 및 제안 목록은 [GitHub의 powertoy 제품 리포지토리 문제](https://github.com/microsoft/PowerToys/issues?q=is%3Aopen+is%3Aissue+label%3AProduct-Launcher)를 참조 하세요.

## <a name="attribution"></a>특성이

- [Wox](https://github.com/Wox-launcher/Wox/)

- [베타 Tadele의 창 Walker](https://github.com/betsegaw/windowwalker)
