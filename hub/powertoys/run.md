---
title: Windows 10 용 Powertoy Run 유틸리티
description: 성능을 저하 시 키 지 않고 몇 가지 추가 기능을 포함 하는 고급 사용자를 위한 빠른 시작 관리자입니다.
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9be1d54946ec2286d95dbe7d4518a631efd471e9
ms.sourcegitcommit: 46a7e9db64e17a645ee6e888f62a9b04632c56af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618532"
---
# <a name="powertoys-run-utility"></a>Powertoy Run 유틸리티

Powertoy Run은 성능을 저하 시 키 지 않고 몇 가지 추가 기능을 포함 하는 고급 사용자를 위한 빠른 시작 관리자입니다. 오픈 소스 이며 추가 플러그인 용 모듈식입니다.

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

이렇게 하면 Powertoy가 대상 플러그 인 으로만 실행 됩니다.

  | **작업 키** | **작업** |
  | --- | --- |
  | `=` | 계산기만 해당 합니다. 예제 `=2+2` |
  | `?` | 파일만 검색 합니다. `?road`찾기 위한 예`roadmap.txt` |
  | `.` | 설치 된 앱 검색만 `.code`Visual Studio Code 가져오는 예제 |
  | `//` | Url만. `//docs.microsoft.com`기본 브라우저가 이동 하는 예https://docs.microsoft.com |
  | `<` | 실행 중인 프로세스만 `<outlook`Outlook을 포함 하는 모든 프로세스를 찾는 예제 |
  | `>` | Shell 명령만. `>ping localhost`Ping 쿼리를 수행 하는 예제 |

## <a name="indexer-settings"></a>인덱서 설정

모든 드라이브를 포함 하도록 인덱서 설정을 설정 하지 않은 경우 다음과 같은 경고가 표시 됩니다.

![Powertoy 인덱서 실행 경고](../images/pt-run-warning.png)

Powertoy 설정에서 경고를 해제 하거나 경고를 선택 하 여 인덱싱되는 드라이브를 확장할 수 있습니다. 이 경고를 선택 하면 Windows 10 설정 "Windows 검색" 옵션이 열립니다.

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
