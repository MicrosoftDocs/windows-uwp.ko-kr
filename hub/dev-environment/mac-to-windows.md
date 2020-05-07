---
title: Mac(Unix)에서 Windows로 이동하도록 지원
description: Mac(Unix)에서 Windows 개발 환경으로 전환하는 데 유용한 가이드로, 바로 가기 키 매핑과 Mac 및 Windows 간의 다른 개념에 대해 간단히 설명합니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac에서 Windows로, 바로 가기 키 매핑, Unix에서 Windows로 이동, Mac에서 Windows로 전환, MacBook에서 Surface로의 이동 지원, Macintosh 사용자용 Windows 사용 방법, Macintosh에서 Windows로 전환, 개발 환경으로의 변경 지원, Mac OS X에서 Windows로, Mac에서 PC로 이동 지원
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 457abcec97247afcc0d63c983c8a6cda2de51c66
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81643696"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>개발 환경을 Mac에서 Windows로 변경하기 위한 가이드

다음 팁과 컨트롤은 Mac과 Windows(또는 WSL/Linux) 개발 환경 간에 전환하는 데 유용합니다.

앱 개발에서 Xcode와 가장 비슷한 프로그램은 [Visual Studio](https://visualstudio.microsoft.com)입니다. 다시 전환하려는 경우 [Mac용 Visual Studio](https://visualstudio.microsoft.com/vs/mac/) 버전을 사용할 수도 있습니다. 플랫폼 간 소스 코드 편집(및 다양한 플러그 인)에서 [Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432)는 가장 인기 있는 선택입니다.

## <a name="keyboard-shortcuts"></a>바로 가기 키

| **작업** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| 복사 | Command+C | Ctrl+C |
| 잘라내기 | Command+X | Ctrl+X |
| 붙여넣기 | Command+V | Ctrl+V |
| 실행 취소 | Command+Z | Ctrl+Z |
| 저장 | Command+S | Ctrl+S |
| 열기 | Command+O | Ctrl+O |
| 컴퓨터 잠금 | Command+Control+Q | Windows 키+L |
| 바탕 화면 표시 | Command+F3 | Windows 키+D |
| 파일 브라우저 열기 | Command+N | Windows 키+E |
| 창 최소화 | Command+M | Windows 키+M |
| 검색 | Command+스페이스 | Windows 키 |
| 활성 창 닫기 | Command+W | Control+W |
| 현재 작업 전환 | Command+Tab | Alt+Tab |
| 전체 화면으로 창 최대화 | Control+Command+F | Windows 키+Up |
| 화면 저장(스크린샷) | Command+Shift+3 | Windows 키+Shift+S |
| 창 저장 | Command+Shift+4 | Windows 키+Shift+S |
| 항목 정보 또는 속성 보기 | Command+I | Alt+Enter |
 | 모든 항목 선택 | Command+A | Ctrl+A |
| 목록에서 두 개 이상의 항목 선택(인접하지 않음) | Command, 각 항목 클릭 | Ctrl, 각 항목 클릭 |
| 특수 문자 입력 | Option+문자 키 | Alt+문자 키|

## <a name="trackpad-shortcuts"></a>트랙 패드 바로 가기

참고: 이러한 바로 가기 중 일부에는 Surface 디바이스의 트랙 패드 및 기타 타사 랩톱과 같은 "Precision Trackpad"가 필요합니다.

 **작업** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Scroll | 두 손가락으로 세로로 살짝 밀기 | 두 손가락으로 세로로 살짝 밀기 |
| 확대/축소 | 두 손가락 모으기 및 벌리기 | 두 손가락 모으기 및 벌리기 |
| 살짝 밀어 보기 간 전환 | 두 손가락으로 옆으로 살짝 밀기 | 두 손가락으로 옆으로 살짝 밀기 |
| 가상 작업 영역 전환 | 네 손가락으로 옆으로 살짝 밀기 | 네 손가락으로 옆으로 살짝 밀기 |
| 현재 열려 있는 앱 표시 | 네 손가락으로 위로 살짝 밀기 | 세 손가락으로 위로 살짝 밀기 |
| 앱 간 전환 | 해당 없음 | 세 손가락으로 옆으로 천천히 살짝 밀기 |
| 바탕 화면으로 이동 | 네 손가락으로 벌리기 | 세 손가락으로 아래로 살짝 밀기 |
| Cortana/알림 센터 열기 | 두 손가락으로 오른쪽에서 밀기 | 세 손가락으로 탭하기 |
| 추가 정보 열기 | 세 손가락으로 탭하기 | 해당 없음 |
|실행 패드 표시/앱 시작 | 네 손가락 모으기 | 네 손가락으로 탭하기 |

참고: 트랙 패드 옵션은 두 플랫폼에서 구성할 수 있습니다.

## <a name="terminal-and-shell"></a>터미널 및 셸

Windows는 Mac의 터미널 에뮬레이터에 대한 여러 가지 대안을 제공합니다.

1. Windows 명령줄

Windows 명령줄은 DOS 명령을 수락하고 Windows에서 가장 일반적으로 사용되는 명령줄 도구입니다. 열려면 다음을 수행합니다. **Windows 키+R**을 눌러 **실행** 상자를 열고 **cmd**를 입력한 다음, **확인**을 클릭합니다. 관리자 명령줄을 열려면 **cmd**를 입력한 다음, **Ctrl+Shift+Enter**를 누릅니다.

2. PowerShell

[PowerShell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6)은 .NET을 토대로 작성된 작업 기반 명령줄 셸 및 스크립트 언어입니다. PowerShell에서 시스템 관리자와 고급 사용자는 운영 체제를 관리하는 작업을 신속하게 자동화할 수 있습니다." 즉, 매우 강력한 명령줄이며 시스템 관리자가 특히 선호합니다.

PowerShell은 [Mac에서도 사용할 수 있습니다](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6).

3. WSL(Linux용 Windows 하위 시스템)

WSL을 사용하면 Windows 내에서 Linux 셸을 실행할 수 있습니다. 즉, 선택 항목 및 설치된 특정 Linux 배포판에 따라 **bash** 또는 기타 셸을 실행할 수 있습니다. WSL을 사용하면 Mac 사용자에게 가장 익숙한 환경이 제공됩니다. 예를 들어, Windows 명령줄을 사용할 때처럼 **dir**을 사용하는 것이 **ls**를 사용하여 현재 디렉터리에 있는 파일을 나열합니다. WSL의 설치 및 사용에 대해 자세히 알아보려면 [Windows 10에 Linux용 Windows 하위 시스템 설치 가이드](https://docs.microsoft.com/windows/wsl/install-win10)를 참조하세요.

4. Windows 터미널(미리 보기)

Windows 터미널은 기존 Windows 명령줄, PowerShell 및 Linux용 Windows 하위 시스템을 비롯한 다양한 소스의 명령줄 도구와 셸을 결합하는 애플리케이션입니다. 현재 미리 보기 상태에 있는 동안에도 여러 탭, 분할 창, 사용자 지정 테마 및 스타일, 전체 유니코드 지원 등 여러 가지 유용한 기능을 이미 사용할 수 있습니다. Windows 터미널은 [Windows 10의 Microsoft Store](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)에서 설치할 수 있습니다.

## <a name="apps-and-utilities"></a>앱 및 유틸리티

 **App** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| 설정 및 기본 설정 | 시스템 기본 설정 | Settings |
| 작업 관리자 | 활동 모니터 | 작업 관리자 |
| 디스크 포맷 | 디스크 유틸리티 | 디스크 관리 |
| 텍스트 편집 | TextEdit | 메모장 |
| 이벤트 보기 | 콘솔 | 이벤트 뷰어 |
| 파일/앱 찾기 | Command+스페이스 | Windows 키 |
