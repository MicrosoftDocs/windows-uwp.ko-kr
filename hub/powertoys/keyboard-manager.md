---
title: Windows 10 용 Powertoy 키보드 관리자 유틸리티
description: 사용자가 키보드에서 키를 다시 정의할 수 있도록 하는 유틸리티입니다.
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: eb17cd5a7ad76728e6b063f76369c8d194a5e12c
ms.sourcegitcommit: 1a997d7e0100e58886150f9fba33d7b205f41df1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2021
ms.locfileid: "97865468"
---
# <a name="keyboard-manager-utility"></a>키보드 관리자 유틸리티

Powertoy 키보드 관리자를 사용 하 여 키보드에서 키를 다시 정의할 수 있습니다.

예를 들어 키보드에서 문자 <kbd>D</kbd> 에 대 한 <kbd>A</kbd> 문자를 교환할 수 있습니다. <kbd>A</kbd> 키를 선택 하면 <kbd>D</kbd> 가 표시 됩니다.

![Powertoy 키보드 관리자 키 다시 매핑 스크린샷](../images/powertoys-keyboard-remap.png)

또한 바로 가기 키 조합을 교환할 수 있습니다. 예를 들어 바로 가기 키인 <kbd>Ctrl</kbd> + <kbd>C</kbd>는 Microsoft Word에서 텍스트를 복사 합니다. Powertoy 키보드 관리자 유틸리티를 사용 하 여 <kbd>⊞ Win</kbd>C에 대 한 바로 가기를 교환할 수 있습니다 + <kbd></kbd>. 이제 <kbd>⊞ Win</kbd> + <kbd>C</kbd>)에서 텍스트를 복사 합니다. Powertoy 키보드 관리자에서 대상 응용 프로그램을 지정 하지 않는 경우 바로 가기 교환은 Windows 전체에 적용 됩니다.

다시 매핑된 키와 바로 가기를 적용 하려면 Powertoy 키보드 관리자를 백그라운드에서 실행 하는 Powertoy를 사용 하도록 설정 해야 합니다. Powertoy가 실행 되 고 있지 않으면 키 다시 매핑은 더 이상 적용 되지 않습니다.

![Powertoy 키보드 관리자 다시 매핑 바로 가기 스크린샷](../images/powertoys-keyboard-shortcuts.png)

> [!NOTE]
> 운영 체제용으로 예약 된 몇 가지 바로 가기 키가 있으며 교체할 수 없습니다. 다시 매핑할 수 없는 키는 다음과 같습니다.
> - `⊞ Win`+`L`및는 `Ctrl`  +  `Alt`  +  `Del` Windows OS에서 예약 되어 있으므로 다시 매핑할 수 없습니다.
> - `Fn`(함수) 키를 다시 매핑할 수 없는 경우 (대부분의 경우) `F1` - `F12` (및 `F13` - `F24` ) 키를 매핑할 수 있습니다.
> - `Pause` 는 sngle keydown 이벤트만 보냅니다. 예를 들어 백스페이스 키에 대해 매핑하고 + 보유를 누르면 단일 문자만 삭제 됩니다.

## <a name="settings"></a>설정

키보드 관리자를 사용 하 여 매핑을 만들려면 다음 옵션을 사용할 수 있습니다.

- <kbd>키 다시 매핑</kbd> 을 선택 하 여 키보드 설정 다시 매핑 창을 시작 합니다.
- <kbd>바로 가기 다시 매핑</kbd> 을 선택 하 여 바로 가기 다시 매핑 설정 창을 시작 합니다.

## <a name="remap-keys"></a>키 다시 매핑

키를 다시 매핑하려면 새 값으로 변경 하 고 <kbd>키</kbd> 다시 매핑 단추를 사용 하 여 키보드 설정 다시 매핑 창을 시작 합니다. 처음 시작 하면 미리 정의 된 매핑이 표시 되지 않습니다. <kbd>+</kbd>새 다시 매핑을 추가 하려면 단추를 선택 해야 합니다.

새 다시 매핑 행이 표시 되 면 "키" 열에서 출력에 *를 **변경** 하려는 키를 선택 합니다. "매핑됨 To" 열에 할당할 새 키 값을 선택 합니다.

예 <kbd>를 들어를 누르고</kbd> <kbd>B</kbd>  가 표시 되 면 다음을 수행 합니다.

- 키: "A"
- 다음으로 매핑됨: "B"

"A"와 "B" 키 사이의 키 위치를 교환 하려면를 사용 하 여 다른 다시 매핑을 추가 합니다.

- 키: "B"
- 다음으로 매핑됨: "A"

![키보드 다시 매핑 키 스크린샷](../images/powertoys-keyboard-remap-a-b.png)

## <a name="key-to-shortcut"></a>바로 가기 키

키를 바로 가기 키 (키 조합)로 다시 매핑하려면 "매핑 대상" 열에 바로 가기 키 조합을 입력 합니다.

예를 들어 "C" 키를 선택 하 여 "Ctrl + V"를 발생 시키려면 다음을 수행 합니다.

- 키: "C"
- 다음으로 매핑됨: "Ctrl + V"

> [!NOTE]
> 키 다시 매핑은 다시 매핑된 키가 다른 바로 가기에서 사용 되는 경우에도 유지 됩니다. 예를 들어 "Alt + C" 바로 가기를 입력 하면 C 키가 "Ctrl + V"로 다시 매핑되고 "Alt + Ctrl + V"로 생성 됩니다.

## <a name="remap-shortcuts"></a>바로 가기 다시 매핑

"Ctrl + v"와 같은 바로 가기 키 조합을 다시 매핑하려면 <kbd>바로 가기 다시 매핑</kbd> 을 선택 하 여 바로 가기 다시 매핑 설정 창을 시작 합니다.

처음 시작 하면 미리 정의 된 매핑이 표시 되지 않습니다. <kbd>+</kbd>새 다시 매핑을 추가 하려면 단추를 선택 해야 합니다.

새 다시 매핑 행이 표시 되 면 "바로 가기" 열에서 해당 출력을 _*_변경_*_ 하려는 키를 선택 합니다. "매핑됨 To" 열에 할당할 새 바로 가기 값을 선택 합니다.

예를 들어 바로 가기 <kbd>Ctrl</kbd> + <kbd>C</kbd> 는 선택한 텍스트를 복사 합니다. <kbd>Ctrl</kbd> 키가 아닌 <kbd>Alt</kbd> 키를 사용 하 여 해당 바로 가기를 다시 매핑하려면 다음을 수행 합니다.

- 바로 가기: "Ctrl" + "C"
- 다음으로 매핑됨: "Alt" + "C"

![키보드 다시 매핑 바로 가기 스크린샷](../images/powertoys-keyboard-remap-shortcut.png)

바로 가기를 다시 매핑할 때 따라야 할 몇 가지 규칙이 있습니다. 이러한 규칙은 "바로 가기" 열에만 적용 됩니다.

- 바로 가기 키는 <kbd>Ctrl</kbd>, <kbd>Shift</kbd>, <kbd>Alt</kbd>또는 <kbd>⊞ Win</kbd> 의 보조키로 시작 해야 합니다.
- 바로 가기는 A, B, C, 1, 2, 3 등의 작업 키 (모든 보조키가 아닌 키)로 끝나야 합니다.
- 바로 가기 키는 3 개 보다 길 수 없습니다.  

## <a name="remap-a-shortcut-to-a-single-key"></a>단일 키에 대 한 바로 가기 다시 매핑

바로 가기 (키 조합)를 단일 키 누름으로 다시 매핑할 수 있습니다.

예를 들어 바로 가기 키인 <kbd>⊞ Win</kbd>  +  <kbd>D</kbd> (Windows 데스크톱 앱 표시/숨기기)를 단일 키로 바꾸려면 <kbd>alt</kbd>키를 누릅니다.

- 바로 가기: "⊞ Win" (Windows 키) + "D"
- 다음으로 매핑됨: "Alt"

> [!NOTE]
> 다시 매핑된 키가 다른 바로 가기에서 사용 되는 경우에도 바로 가기 다시 매핑은 유지 됩니다. 예를 들어 "alt" + "Tab" 바로 가기를 입력 하면 위와 같이 "Alt" 키를 다시 매핑하는 경우 "⊞ Win" + "D" + "Tab"이 발생 합니다.

## <a name="app-specific-shortcuts"></a>앱 특정 바로 가기

키보드 관리자를 사용 하면 Windows에서 전역적이 아닌 특정 앱에 대 한 바로 가기 키를 다시 매핑할 수 있습니다.

예를 들어 Outlook 전자 메일 앱에서 바로 가기 "Ctrl + E"는 전자 메일을 검색 하기 위해 기본적으로 설정 됩니다. 대신 "Ctrl + F"를 설정 하 여 전자 메일을 검색 하는 대신 (기본적으로 설정 된 대로 전자 메일을 전달 하는 대신) "대상 앱"으로 설정 된 "Outlook"으로 바로 가기를 다시 매핑할 수 있습니다.

키보드 관리자는 프로세스 이름 (응용 프로그램 이름 아님)을 사용 하 여 앱을 대상으로 합니다. 예를 들어 Microsoft Edge는 "Microsoft Edge" (응용 프로그램 이름)이 아니라 "msedge" (프로세스 이름)로 설정 됩니다. 응용 프로그램의 프로세스 이름을 찾으려면 PowerShell을 열고 명령을 입력 `get-process` 하거나 명령 프롬프트를 열고 명령을 입력 `tasklist` 합니다. 그러면 현재 열려 있는 모든 응용 프로그램에 대 한 프로세스 이름 목록이 생성 됩니다. 다음은 몇 가지 인기 있는 응용 프로그램 프로세스 이름 목록입니다.

  | _ *응용 프로그램**   | **프로세스 이름**|
  | ------------------| --------------|
  | Microsoft Edge    |  msedge.exe   |
  | OneNote           |  onenote.exe  |
  | Outlook           |  outlook.exe  |
  | Teams             |  Teams.exe    |
  | Adobe Photoshop   |  Photoshop.exe|
  | 파일 탐색기     |  explorer.exe |
  | Spotify 음악     |  spotify.exe  |
  | Google Chrome     |  chrome.exe   |
  | Excel             |  excel.exe    |
  | Word              |  winword.exe  |
  | Powerpoint        |  powerpnt.exe |

### <a name="keys-that-cannot-be-remapped"></a>다시 매핑할 수 없는 키

다시 매핑할 수 없는 특정 바로 가기 키가 있습니다. 이러한 위협은 다음과 같습니다.

- <kbd>Ctrl</kbd> + <kbd>Alt</kbd> +  <kbd>Del</kbd> (interupt 명령)
- <kbd>⊞ Win</kbd> + <kbd>L</kbd> (컴퓨터 잠금)
- 함수 키 <kbd>Fn</kbd>을 다시 매핑할 수 없는 경우 (대부분의 경우) <kbd>F1</kbd> - <kbd>F12</kbd> 를 매핑할 수 있습니다.

## <a name="how-to-select-a-key"></a>키를 선택 하는 방법

다시 매핑할 키 또는 바로 가기를 선택 하려면 다음을 수행할 수 있습니다.

- <kbd>형식 키</kbd> 단추를 사용 합니다.
- 드롭다운 메뉴를 사용 합니다.

<kbd>유형 키/바로 가기</kbd> 단추를 선택 하면 키보드를 사용 하 여 키 또는 바로 가기를 입력할 수 있는 대화 상자가 표시 됩니다. 출력에 만족 하는 경우 <kbd>Enter 키</kbd> 를 눌러 계속 합니다. 대화 상자를 나가려면 <kbd>Esc</kbd> 단추를 누릅니다.

드롭다운 메뉴를 사용 하 여 키 이름을 사용 하 여 검색할 수 있습니다. 그러면 추가 드롭다운 값이 진행 중으로 표시 됩니다. 그러나 드롭다운 메뉴가 열려 있는 동안에는 형식 키 기능을 사용할 수 없습니다.

## <a name="orphaning-keys"></a>분리 키

키를 분리 다른 키에 매핑하고 더 이상 매핑된 항목이 없다는 것을 의미 합니다.

예를 들어, 키가 A-> B에서 다시 매핑되는 경우 키보드에 키가 더 이상 존재 하지 않습니다.

이 문제를 해결 하려면 +를 사용 하 여의 결과에 매핑되는 다시 매핑되는 다른 키를 만듭니다. 실수로 인해이 문제가 발생 하지 않도록 하기 위해 분리 된 키에 대 한 경고가 표시 됩니다.

![Powertoy 키보드 관리자 분리 된 키](../images/powertoys-keyboard-remap-orphaned.png)

## <a name="frequently-asked-questions"></a>질문과 대답

### <a name="i-remapped-the-wrong-keys-how-can-i-stop-it-quickly"></a>잘못 된 키를 다시 매핑 했습니다. 신속 하 게 중지 하려면 어떻게 해야 하나요?

키를 다시 매핑하는 작업을 수행 하려면 Powertoy가 백그라운드에서 실행 중 이어야 하 고 키보드 관리자를 사용 하도록 설정 해야 합니다. 다시 매핑된 키를 중지 하려면 Powertoy를 닫거나 Powertoy 설정에서 키보드 관리자를 사용 하지 않도록 설정 합니다.

### <a name="can-i-use-keyboard-manager-at-my-log-in-screen"></a>내 로그인 화면에서 키보드 관리자를 사용할 수 있나요?

아니요. Powertoy가 실행 중이 고 관리자 권한으로 실행을 포함 하 여 암호 화면에서 작동 하지 않는 경우에만 키보드 관리자를 사용할 수 있습니다.

### <a name="do-i-have-to-turn-off-my-computer-for-the-remapping-to-take-effect"></a>다시 매핑을 적용 하려면 컴퓨터를 꺼야 하나요?

아니요, 다시 매핑은 **적용** 을 누를 때 즉시 수행 됩니다.

### <a name="where-are-the-maclinux-profiles"></a>Mac/Linux 프로필은 어디에 있나요?

현재 Mac 및 Linux 프로필은 포함 되지 않습니다.

### <a name="will-this-work-on-video-games"></a>이는 비디오 게임에서 작동 하나요?

게임에서 키에 액세스 하는 방법에 따라 달라 집니다. 특정 키보드 Api는 키보드 관리자에서 작동 하지 않습니다.

### <a name="will-remapping-work-if-i-change-my-input-language"></a>입력 언어를 변경 하면 작업을 다시 매핑 하나요?

예. 이제 영어 (미국) 키보드에서 <kbd>a</kbd> 를 <kbd>B</kbd> 로 다시 매핑한 후 언어 설정을 프랑스어로 변경 하는 경우 프랑스어 키보드 (미국 실제 키보드의<kbd>Q</kbd> )에 <kbd>를</kbd> 입력 하면 <kbd>B</kbd>는 Windows에서 다국어 입력을 처리 하는 방법과 일치 합니다.

## <a name="troubleshooting"></a>문제 해결

키 또는 바로 가기 키를 다시 매핑하려는 데 문제가 있는 경우 다음 문제 중 하나일 수 있습니다.

- **관리자 권한으로 실행:** 해당 창이 관리자 (관리자) 모드에서 실행 중이 고 Powertoy가 관리자 권한으로 실행 되 고 있지 않은 경우에는 앱/창에서 다시 매핑을 사용할 수 없습니다. 관리자 권한으로 Powertoy를 실행 해 보세요.

- **키 가로채기 안 함:** 키보드 관리자는 키를 다시 매핑하기 위해 키보드 후크를 차단 합니다. 이 작업을 수행 하 고 키보드 관리자를 방해할 수 있는 앱도 있습니다. 이 문제를 해결 하려면 설정으로 이동 하 여 키보드 관리자를 사용 하지 않도록 설정 하 고 다시 사용 하도록 설정 합니다.

## <a name="known-issues"></a>알려진 문제

- [Caps light 표시기가 올바르게 전환 되지 않음](https://github.com/microsoft/PowerToys/issues/1692)

- [FancyZones 및 바로 가기 안내선에 대 한 다시 매핑 작동 안 함](https://github.com/microsoft/PowerToys/issues/3079)

- [Win, Ctrl, Alt 또는 Shift와 같은 키를 다시 매핑하면 제스처와 일부 특수 단추가 중단 될 수 있습니다.](https://github.com/microsoft/PowerToys/issues/3703)

[열린 키보드 관리자 문제](https://github.com/microsoft/PowerToys/issues?q=is%3Aopen+is%3Aissue+label%3A%22Product-Keyboard+Shortcut+Manager%22)목록을 참조 하세요.
