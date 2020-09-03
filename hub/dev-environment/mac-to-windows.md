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
ms.openlocfilehash: 1d94944037caf7cd909ea4799867f83bd4a6f887
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157587"
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

## <a name="command-line-shells-and-terminals"></a>명령줄 셸 및 터미널

Windows는 Mac의 BASH 셸이나 Terminal 및 iTerm 같은 터미널 에뮬레이터 앱과 약간 다르게 작동하는 여러 명령줄 셸 및 터미널을 지원합니다.

### <a name="windows-shells"></a>Windows 셸

Windows에는 다음과 같은 두 가지 기본 명령줄 셸이 있습니다.

1. **[PowerShell](/powershell/scripting/overview?view=powershell-7)** - PowerShell은 .NET 기반의 명령줄 셸 및 스크립트 언어로 구성된 플랫폼 간 작업 자동화 및 구성 관리 프레임워크입니다. 관리자, 개발자 및 고급 사용자는 PowerShell을 사용하여 복잡한 프로세스 및 프로세스가 실행되는 환경과 운영 체제의 다양한 측면을 관리하는 작업을 신속하게 제어하고 자동화할 수 있습니다. PowerShell은 [완전한 오픈 소스](https://github.com/powershell/powershell)이며, 플랫폼 간 도구이므로 [Mac 및 Linux에도 사용할 수 있습니다](/powershell/scripting/install/installing-powershell?view=powershell-7).

    **Mac 및 Linux BASH 셸 사용자**: PowerShell은 기존의 익숙한 여러 명령 별칭도 지원합니다. 예:
    - `ls`를 사용하여 현재 디렉터리의 내용을 나열
    - `mv`를 사용하여 파일 이동
    - `cd <path>`를 사용하여 새 디렉터리로 이동

    PowerShell과 BASH의 일부 명령 및 인수는 서로 다릅니다. PowerShell에서 [`get-help`](/powershell/scripting/learn/ps101/02-help-system?view=powershell-7)를 입력하여 자세히 알아보거나 문서의 [호환성 별칭](/powershell/scripting/samples/appendix-1---compatibility-aliases?view=powershell-7)을 확인하세요.

    관리자 권한으로 PowerShell을 실행하려면 Windows 시작 메뉴에서 "PowerShell"을 입력한 다음, "관리자 권한으로 실행"을 선택합니다.

2. **Windows 명령줄(Cmd)** : Windows는 기존 명령 프롬프트(및 콘솔 - 아래 참조)를 계속 제공하므로 현재 및 기존 MS-DOS와 호환되는 명령과 배치 파일을 제공합니다. Cmd는 기존/이전 배치 파일 또는 명령줄 작업을 실행할 때 유용하지만, 일반적으로 사용자는 PowerShell을 학습하여 사용하는 것이 좋습니다. Cmd는 현재 유지 관리 중이며, 향후 기능이 개선되거나 새 기능이 추가되지 않기 때문입니다.

### <a name="linux-shells"></a>Linux 셸

이제 WSL(Linux용 Windows 하위 시스템)을 설치하여 Windows에서 Linux 셸을 실행할 수 있습니다. 즉, 어떤 Linux 배포판을 선택하든 Windows 내부에 바로 통합되므로 **bash**를 실행할 수 있습니다. WSL을 사용하면 Mac 사용자에게 가장 익숙한 환경이 제공됩니다. 예를 들어 기존 Windows Cmd 셸을 사용할 때처럼 **dir**을 사용하는 것이 아니라 **ls**를 사용하여 현재 디렉터리에 있는 파일을 나열합니다. WSL의 설치 및 사용에 대해 자세히 알아보려면 [Windows 10에 Linux용 Windows 하위 시스템 설치 가이드](/windows/wsl/install-win10)를 참조하세요. WSL을 사용하여 Windows에 설치할 수 있는 Linux 배포판은 다음과 같습니다.

1. [Ubuntu 20.04 LTS](https://www.microsoft.com/store/apps/9n6svws3rx71)
2. [Kali Linux](https://www.microsoft.com/store/apps/9PKR34TNCV07)
3. [Debian GNU/Linux](https://www.microsoft.com/store/apps/9MSVKQC78PK6)
4. [openSUSE Leap 15.1](https://www.microsoft.com/store/apps/9NJFZK00FGKV)
5. [SUSE Linux Enterprise Server 15 SP1](https://www.microsoft.com/store/apps/9PN498VPMF3Z)

몇 가지 예를 살펴보겠습니다. [WSL 설치 문서](/windows/wsl/install-win10#install-your-linux-distribution-of-choice)에서 더 많은 배포판을 찾을 수 있으며 [Microsoft Store](https://www.microsoft.com/search/shop/apps?q=linux&category=Developer+tools)에서 바로 설치할 수 있습니다.

## <a name="windows-terminals"></a>Windows 터미널

Microsoft는 여러 타사 제품 외에도 명령줄 셸 및 애플리케이션에 액세스할 수 있는 두 개의 "터미널"(GUI 애플리케이션)을 제공합니다.

1. **[Windows 터미널](/windows/terminal/)** : 매우 유연하게 구성할 수 있는 최신 명령줄 터미널 애플리케이션인 Windows 터미널은 매우 강력한 성능, 짧은 대기 시간 명령줄 사용자 환경, 여러 탭, 분할 창, 사용자 지정 테마 및 스타일, 다양한 셸 또는 명령줄 앱에 대한 여러 "프로필"을 제공할 뿐 아니라 명령줄 사용자 환경의 여러 측면을 구성하고 개인 설정할 수 있는 많은 기회를 제공합니다.

    Windows 터미널을 사용하여 PowerShell, WSL 셸(예: Ubuntu 또는 Debian), 기존 Windows 명령 프롬프트 또는 다른 명령줄 앱(예: SSH, Azure CLI, Git Bash)에 연결된 탭을 열 수 있습니다.

2. **[콘솔](/windows/console/)** : Mac 및 Linux에서 사용자는 일반적으로 기본 설정된 터미널 애플리케이션을 시작합니다. 그러면 애플리케이션에서 사용자의 기본 셸(예: BASH)을 만들고 연결합니다.

    그러나 오랜 역사로 인해 전통적으로 Windows 사용자는 셸을 시작하고, Windows는 GUI 콘솔 앱을 자동으로 시작하고 연결합니다.

    앞으로도 계속해서 셸을 직접 시작하고 레거시 Windows 콘솔을 사용해도 되지만, 사용자가 Windows 터미널을 설치하고 사용한다면 가장 빠르고 가장 생산적인 최고의 명령줄 환경을 경험할 수 있습니다.

## <a name="apps-and-utilities"></a>앱 및 유틸리티

 **App** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| 설정 및 기본 설정 | 시스템 기본 설정 | Settings |
| 작업 관리자 | 활동 모니터 | 작업 관리자 |
| 디스크 포맷 | 디스크 유틸리티 | 디스크 관리 |
| 텍스트 편집 | TextEdit | 메모장 |
| 이벤트 보기 | 콘솔 | 이벤트 뷰어 |
| 파일/앱 찾기 | Command+스페이스 | Windows 키 |