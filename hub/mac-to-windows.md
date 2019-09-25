---
title: Mac (Unix)에서 Windows로 이동 하는 데 도움을 줍니다.
description: Mac (Unix)에서 Windows 개발 환경으로 전환 하는 데 도움이 되는 가이드입니다. 바로 가기 키 매핑과 Mac과 Windows 간에 다른 개념에 대 한 간략 한 개요를 포함 합니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.prod: dev-environment
ms.technology: windows-python
keywords: Mac에서 Windows로 전환, 바로 가기 키 매핑, Unix에서 windows로 전환, windows에서 windows로 전환, Macintosh 사용자에 대해 Windows를 사용 하는 방법, macintosh 사용자에 게 Windows를 사용 하는 방법, windows에서 Windows로 전환 하는 방법, 도움을 주는 Mac OS X 도움말 Mac에서 PC로 이동
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 486742768bddff077c0f1b0e73e2f431ffdf1bba
ms.sourcegitcommit: 7104ad5d01ad1c69a4ea0b3ba6732c1b2a98ec09
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71251140"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>개발 환경을 Mac에서 Windows로 변경 하기 위한 가이드

다음 팁과 해당 항목은 Mac과 Windows (또는 WSL/Linux) 개발 환경 간에 전환 하는 데 도움이 됩니다.

앱 개발을 위해 Xcode에 가장 가까운 것은 [Visual Studio](https://visualstudio.microsoft.com)입니다. 뒤로 이동 해야 하는 경우에는 [Mac용 Visual Studio](https://visualstudio.microsoft.com/vs/mac/)버전도 있습니다. 플랫폼 간 소스 코드 편집 (및 매우 많은 수의 플러그 인)은 가장 인기 있는 선택 [Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432) .


## <a name="keyboard-shortcuts"></a>바로 가기 키

| **연산** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| 복사 | 명령 + C | Ctr + C |
| 잘라내기 | 명령 + X | Ctr + X |
| 붙여넣기 | 명령 + V | Ctr + V |
| 실행 취소 | 명령 + Z | Ctrl+Z |
| 저장 | 명령 + S | Ctrl+S |
| 열기 | 명령 + O | Ctrl+O |
| 컴퓨터 잠금 | 명령 + 컨트롤 + Q | Ctr + L |
| 바탕 화면 표시 | 명령 + F3 | Ctrl + D |
| 창 최소화 | Windows 키 + M | 명령 + M |
| 검색 | 명령 + 공백 | Windows 키 |
| 활성 창 닫기 | 명령 + W | 컨트롤 + W |
| 현재 작업 전환 | 명령 + 탭 | Alt+Tab |
| 화면 저장 | 명령 + Shift + 3 | Windows + Shift + S |
| 창 저장 | 명령 + Shift + 4 | Windows + Shift + S |
| 항목 정보 보기 또는 속성 | 명령 + I | Alt + Enter |
 | 모든 항목 선택 | 명령 + A | Ctrl+A |
| 목록에서 두 개 이상의 항목 선택 (인접 하지 않음) | 명령을 클릭 한 다음 각 항목을 클릭 합니다. | 컨트롤을 클릭 한 다음 각 항목을 클릭 합니다. |
| 특수 문자 입력 | Option + 문자 키 | Alt + 문자 키|

## <a name="trackpad-shortcuts"></a>트랙 패드 바로 가기

참고: 이러한 바로 가기 중 일부에는 Surface 장치의 트랙 패드 및 기타 타사 랩톱과 같은 "Precision 트랙 패드"가 필요 합니다.

 **연산** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Scroll | 두 손가락 세로 살짝 밀기 | 두 손가락 세로 살짝 밀기 |
| 확대/축소 | 손가락으로 두 손가락으로 | 손가락으로 두 손가락으로 |
| 뒤로 살짝 밀어 보기 간 이동 | 두 손가락 사이드 살짝 밀기 | 두 손가락 사이드 살짝 밀기 |
| 가상 작업 영역 전환 | 네 손가락 옆으로 살짝 밀기 | 네 손가락 옆으로 살짝 밀기 |
| 현재 열려 있는 앱 표시 | 네 손가락 위로 살짝 밀기 | 세 손가락 위로 살짝 밀기 |
| 앱 간 전환 | 해당 사항 없음 | 세 손가락 옆으로 살짝 밀기 |
| 바탕 화면으로 이동 | 네 손가락에 흩어져 있습니다. | 세 손가락 아래로 살짝 밀기 |
| Cortana/Action center 열기 | 오른쪽의 두 손가락 슬라이드 | 세 손가락 탭 |
| 추가 정보 열기 | 세 손가락 탭 | 해당 사항 없음 |
|실행 패드/앱 시작 표시 | 네 손가락으로 손가락 | 네 손가락으로 탭 |

참고: 트랙 패드 옵션은 두 플랫폼에서 구성할 수 있습니다.

## <a name="terminal-and-shell"></a>터미널 및 셸

Windows는 Mac의 터미널 에뮬레이터에 대 한 여러 가지 대안을 제공 합니다.

1. Windows 명령줄

Windows 명령줄은 DOS 명령을 수락 하 고 Windows에서 가장 일반적으로 사용 되는 명령줄 도구입니다. 열려면 다음을 수행 합니다. **Windows + R** 을 눌러 **실행** 상자를 열고 **Cmd** 를 입력 한 다음 **확인**을 클릭 합니다. 관리자 명령줄을 열려면 **cmd** 를 입력 한 다음 **ctrl + Shift + enter**를 누릅니다. 

2. PowerShell

[Powershell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6) 은 "powershell은 .net에서 빌드된 작업 기반 명령줄 셸 및 스크립트 언어입니다. PowerShell을 통해 시스템 관리자와 전원 사용자는 운영 체제를 관리 하는 작업을 신속 하 게 자동화할 수 있습니다. " 즉, 매우 강력한 명령줄 이며 특히 시스템 관리자가 좋아했던 합니다.

PowerShell은 [Mac 에서도 사용할 수](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6)있습니다.

3. Linux용 Windows 하위 시스템(WSL)

WSL을 사용 하면 Windows 내에서 Linux 셸을 실행할 수 있습니다. 즉, 선택 및 특정 Linux 배포판 설치에 따라 *bash** 또는 기타 셸을 실행할 수 있습니다. WSL을 사용 하면 Mac 사용자에 게 가장 익숙한 환경의 종류를 제공할 수 있습니다. 예를 들어 Windows 명령줄에서와 같이 **dir** 이 아닌 현재 디렉터리에 파일을 나열 하는 경우는 **ls** 입니다. Instaling 및 WSL 사용에 대 한 자세한 내용은 windows [10 용 Windows 하위 시스템 설치 가이드](https://docs.microsoft.com/en-us/windows/wsl/install-win10)를 참조 하세요.

## <a name="apps-and-utilities"></a>앱 및 유틸리티

 **다운로드** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| 설정 및 기본 설정 | 시스템 기본 설정 | 설정 |
| 작업 관리자 | 작업 모니터 | 작업 관리자 |
| 디스크 서식 지정 | 디스크 유틸리티 | 디스크 관리 |
| 텍스트 편집 | TextEdit | Windows 메모장 |
| 이벤트 보기 | 콘솔 | 이벤트 뷰어 |
| 파일/앱 찾기 | 명령 + 공백 | Windows 키 |
