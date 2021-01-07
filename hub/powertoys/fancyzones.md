---
title: Windows 10 용 Powertoy FancyZones 유틸리티
description: 효율적인 레이아웃으로 창을 정렬 하 고 맞추기 위한 창 관리자 유틸리티입니다.
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 00b849e19d3ae8fcf76e1f2a63dc1915353aad30
ms.sourcegitcommit: 5dac88ad541b71ebe85b78951e6b357a3db176cc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97612324"
---
# <a name="fancyzones-utility"></a>FancyZones 유틸리티

FancyZones는 워크플로 속도를 개선 하 고 레이아웃을 신속 하 게 복원 하기 위해 창을 효율적인 레이아웃으로 정렬 및 맞추기 위한 창 관리자 유틸리티입니다. 사용자는 FancyZones를 사용 하 여 windows의 끌기 대상인 데스크톱의 창 위치 집합을 정의할 수 있습니다.  사용자가 창을 영역으로 끌면 창의 크기가 조정 되 고 해당 영역을 채우도록 위치가 변경 됩니다.  

![FancyZones 스크린샷](../images/pt-fancy-zones2.png)

## <a name="getting-started"></a>시작

### <a name="enable"></a>사용

FancyZones 사용을 시작 하려면 Powertoy 설정에서 유틸리티를 사용 하도록 설정한 다음 FancyZones 편집기 UI를 호출 해야 합니다.  

### <a name="launch-zones-editor"></a>시작 영역 편집기

Powertoy 설정 메뉴의 단추를 사용 하거나 Win을 눌러 영역 편집기를 시작 <kbd></kbd> + <kbd>`</kbd> 합니다 (설정 대화 상자에서이 바로 가기는 변경 될 수 있음).  

![FancyZones 설정 UI](../images/pt-fancyzones-settings1.png) 

### <a name="elevated-permission-admin-apps"></a>상승 된 권한 관리 앱

상승 된 응용 프로그램이 있는 경우 관리자 모드에서를 실행 하 [고 powertoy를 읽고 관리자 권한으로 실행](administrator.md) 하 여 자세한 내용을 확인 합니다.

## <a name="choose-your-layout-layout-editor"></a>레이아웃 선택 (레이아웃 편집기)

영역 편집기는 처음 시작 될 때 모니터에 있는 창의 수를 기준으로 조정할 수 있는 레이아웃 목록을 제공 합니다. 레이아웃을 선택 하면 모니터의 해당 레이아웃 미리 보기가 표시 됩니다. 적용을 선택 하면 해당 레이아웃을 모니터로 설정 합니다.  

![FancyZones 선택 스크린샷](../images/pt-fancyzones-picker.png)

여러 표시를 사용 하는 경우 편집기에서 사용 가능한 모니터를 검색 하 여 사용자가 선택할 수 있도록 해당 모니터를 표시 합니다. 그러면 선택한 모니터가 적용 될 때 선택한 레이아웃의 대상이 됩니다.

![FancyZones 선택기 다중 모니터](../images/pt-fancyzones-multimon.png)

### <a name="space-around-zones"></a>영역 주변 공간

**영역 주위의 공간 표시** 확인란을 사용 하 여 각 Fccyzone 창에서 테두리 또는 여백 정렬을 결정할 수 있습니다. **영역 주위의 공간** 필드를 사용 하 여 테두리의 너비에 대 한 사용자 지정 값을 설정할 수 있습니다.

**인접 영역을 강조 표시 하는 거리** 를 사용 하면 함께 물릴 때까지 Fccyzone 창 사이의 공간 크기에 대 한 사용자 지정 값을 설정 하 고 함께 병합할 수 있도록 둘 다 강조 표시할 수 있습니다.

영역 편집기를 연 상태에서 값을 변경한 후 **영역 주위에 공간 표시** 상자를 선택 취소 하 여 적용 된 새 값을 확인 합니다.

![영역 주변 영역 주변 영역 스크린샷](../images/pt-fancyzones-spacearound.png)

### <a name="creating-a-custom-layout"></a>사용자 지정 레이아웃 만들기

영역 편집기는 사용자 지정 레이아웃 만들기 및 저장도 지원 합니다. 영역 편집기의 상단 메뉴에서 **템플릿** 옆의 **사용자 지정** 탭을 선택 합니다.
  
사용자 지정 영역 레이아웃을 만드는 두 가지 방법으로 창 레이아웃 및 테이블 레이아웃을 사용할 수 있습니다. 이러한 모델은 가감 및 subtractive 모델으로 간주할 수도 있습니다.  

가산 창 레이아웃 모델은 빈 레이아웃으로 시작 하 고 windows와 비슷하게 끌어서 크기를 조정할 수 있는 영역 추가를 지원 합니다.

![FancyZones 창 편집기 모드](../images/pt-fancyzones-windoweditor.png)

Subtractive 테이블 레이아웃 모델은 테이블 레이아웃으로 시작 하 고 영역을 분할 및 병합 한 다음 영역 사이에서 제본용 여백을 조정 하 여 영역을 만들 수 있습니다.

두 영역을 병합 하려면 마우스 왼쪽 단추를 선택 하 고 두 번째 영역이 선택 될 때까지 마우스를 끌고 단추를 놓으면 팝업 메뉴가 표시 됩니다.

![FancyZones 테이블 편집기 모드](../images/pt-fancyzones-tableeditor.png)

## <a name="snapping-a-window-to-two-or-more-zones"></a>둘 이상의 영역에 창 맞추기

두 영역이 인접해 있으면 해당 영역의 합계에 창을 스냅할 수 있습니다 (둘 다를 포함 하는 최소 사각형으로 반올림). 마우스 커서가 두 영역의 공통 가장자리 근처에 있는 경우 두 영역을 동시에 활성화 하 여 창을 두 영역으로 모두 삭제할 수 있습니다.  

원하는 수의 영역에 맞출 수도 있습니다. 즉, 먼저 한 영역이 활성화 될 때까지 창을 끌어 놓고 키를 길게 누른 채 `Control` 창을 끌어 여러 영역을 선택 합니다.

키보드만 사용 하 여 창을 여러 영역에 맞추려면 먼저 두 옵션인 및를 설정 `Override Windows Snap hotkeys (Win+Arrow) to move between zones` `Move windows based on their position` 합니다. 창을 한 영역에 연결한 후에는 <kbd>Win</kbd>  +  <kbd>컨트롤</kbd>  +  <kbd>Alt</kbd> + 화살표를 사용 하 여 창을 여러 영역으로 확장 합니다.

![두 영역 활성화 스크린샷](../images/pt-fancyzones-twozones.png)

## <a name="shortcut-keys"></a>바로 가기 키

| 바로 가기      | 작업 |
| ----------- | ----------- |
| Win ⊞ + \` | Windows 키 + 억음 (⊞ + \` )은 편집기를 시작 합니다 .이 바로 가기는 설정 대화 상자에서 편집할 수 있습니다. |
| 승 ⊞ + 왼쪽/오른쪽 화살표 | 영역 사이에 포커스가 있는 창 이동 (설정이 설정 된 경우에만 ()이 설정 된 경우에만, 및는 `Override Windows Snap hotkeys` `Windows ⊞ key + Left Arrow` `Windows key ⊞ + Right Arrow` `Win ⊞ + Up Arrow` `Win ⊞ + Down Arrow` 정상적으로 계속 작동 하는 경우에만) |

FancyZones는 Windows 10을 재정의 `Win ⊞ + Shift + Arrow` 하 여 창을 인접 모니터로 빠르게 이동 하지 않습니다.

## <a name="settings"></a>설정

| 설정 | Description |
| --------- | ------------- |
| 영역 편집기 바로 가기 키 구성 | 기본 바로 가기 키를 변경 하려면 텍스트 상자를 클릭 하 고 (텍스트를 선택 하거나 삭제할 필요가 없음) 키보드에서 원하는 키 조합을 누릅니다. |
| 드래그 하는 동안 Shift 키를 누르고 영역 활성화 | 끌기를 사용 하도록 설정 하는 동안 shift 키를 누르면 끌기 및 수동 맞춤 모드에서 자동 맞춤 모드를 전환 합니다. |
| 기본이 아닌 마우스 단추를 사용 하 여 영역 활성화 설정/해제 | 이 옵션이 설정 된 경우 기본이 아닌 마우스 단추를 클릭 하면 영역 활성화가 설정/해제 됩니다. |
| Windows 스냅 바로 가기 키 (Win ⊞ + 화살표)를 재정의 하 여 영역 사이를 이동 합니다. | 이 옵션을 on으로 설정 하 고 Fccyzones를 실행 하는 경우 다음 두 가지 Windows Snap 키를 재정의 `Win ⊞ + Left Arrow` 합니다. `Win ⊞ + Right Arrow` |
| 위치에 따라 창을 이동 합니다. | 에서 Win ⊞ + 위쪽/아래쪽/오른쪽/왼쪽 화살표를 사용 하 여 해당 위치에 따라 창을 가운데에 맞추려면 영역 레이아웃을 사용 합니다. |
| 모든 모니터에서 영역 간 창 이동 | 이 옵션을 off로 설정 하면 Win ⊞ + 화살표를 사용 하 여 맞추기를 사용 하는 경우 현재 모니터의 영역을 순환 하 고 모든 모니터의 모든 영역을 통해 창이 순환 됩니다. |
| 화면 해상도가 변경 될 때 해당 영역에 창 유지 | 화면 해상도를 변경한 후에는이 설정을 사용 하는 경우 FancyZones에서 이전에 있던 영역으로 창의 크기를 조정 하 고 위치를 변경 합니다. |
| 영역 레이아웃이 변경 되는 동안 영역에 할당 된 windows는 새 크기/위치와 일치 합니다. | 이 옵션을 on으로 설정 하면 FancyZones에서 각 창의 이전 영역 번호 위치를 유지 하 여 windows의 크기를 조정 하 고 새 영역 레이아웃으로 창을 배치 합니다. |
| 새로 만든 창을 마지막으로 알려진 영역으로 이동 | 새로 열린 창을 응용 프로그램이 있는 마지막 영역 위치로 자동 이동 |
| 새로 만든 창을 현재 활성 모니터로 이동 [실험적] | 이 옵션을 on으로 설정 하 고 "새로 만든 창에서 마지막으로 알려진 영역으로 이동"이 꺼져 있거나 응용 프로그램에 마지막으로 알려진 영역이 없는 경우 현재 활성 모니터에서 응용 프로그램을 유지 합니다. |
| 맞추지 않는 경우 windows의 원래 크기 복원 | 이 옵션을 on으로 설정 하면 창에 맞추기를 적용 하면 해당 크기가 스냅 되기 전의 크기로 복원 됩니다. |
| 다중 모니터 환경에서 편집기를 시작할 때 포커스가 아닌 마우스 커서를 따라 이동 합니다. | 이 옵션을 설정 하면 편집기 바로 가기 키가 마우스 커서가 있는 모니터에서 편집기를 시작 합니다 .이 옵션을 해제 하면 편집기 바로 가기 키를 누르면 현재 활성 창이 있는 모니터에서 편집기가 시작 됩니다.  |
| 창을 끄는 동안 모든 모니터에 영역 표시 | 기본적으로 FancyZones는 현재 모니터에서 사용할 수 있는 영역만 표시 하며,이 기능은 설정 된 경우 성능에 영향을 줄 수 있습니다. |
| 모니터가 모니터 전체에 걸쳐 있을 수 있도록 허용 (모든 모니터의 DPI 배율이 동일 해야 함) | 이 옵션을 사용 하면 연결 된 모든 모니터를 하나의 커다란 화면으로 취급할 수 있습니다. 제대로 작동 하려면 모든 모니터의 DPI 배율 인수가 동일 해야 합니다. |
| 끌어 온 창을 투명 하 게 만들기 | 영역이 활성화 되 면 영역 표시를 향상 시키기 위해 끌어 온 창이 투명 하 게 됩니다. |
| 영역 강조 색 (기본 #008CFF) | 창 끌기를 수행 하는 동안 활성 놓기 대상인 영역에 있는 색입니다. |
| 영역 비활성 색 (기본 #F5FCFF) | 창 끌기를 수행 하는 동안 활성 상태가 아닌 영역에 있는 색입니다. |
| 영역 테두리 색 (기본 #FFFFFF) | 활성 및 비활성 영역의 테두리 색입니다. |
| 영역 불투명도 (%) (기본값 50%) | 활성 및 비활성 영역의 불투명도 백분율 |
| 영역에 맞추기에서 응용 프로그램 제외 | 한 줄에 하나씩 응용 프로그램 이름 또는 이름의 일부를 추가 합니다 (예: 추가 `Notepad` 는 and와 일치 `Notepad.exe` 하 고 `Notepad++.exe` 확장만을 일치 시키기 위해 `Notepad.exe` `.exe` ). |

![Fit Yzones 설정 하단 스크린샷](../images/pt-fancyzones-settings2.png)