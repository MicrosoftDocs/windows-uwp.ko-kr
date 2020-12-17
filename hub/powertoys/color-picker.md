---
title: Windows 10 용 Powertoy 색 선택 유틸리티
description: 현재 실행 중인 응용 프로그램에서 색을 선택 하 고 16 진수 또는 RGB 값을 클립보드에 자동으로 복사할 수 있도록 하는 Windows 10 용 시스템 수준 색 선택 유틸리티입니다.
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3c76753d455d28dd44045edf9482b3de9ccd97b8
ms.sourcegitcommit: 46a7e9db64e17a645ee6e888f62a9b04632c56af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618572"
---
# <a name="color-picker-utility"></a>색 선택 유틸리티

현재 실행 중인 응용 프로그램에서 색을 선택 하 고 자동으로 구성 가능한 형식으로 클립보드로 복사할 수 있도록 하는 Windows 10 용 시스템 수준 색 선택 유틸리티입니다.

![ColorPicker](../images/pt-colorpicker-hex-editor.png)

## <a name="getting-started"></a>시작

### <a name="enable"></a>사용

색 선택 사용을 시작 하려면 먼저 Powertoy 설정 (색 선택 섹션)에서 사용할 수 있는지 확인 해야 합니다.

### <a name="activate"></a>활성화

활성화 된 후에는 활성화 바로 가기 <kbd>Win</kbd>Shift C를 사용 하 여 색 선택기를 시작할 때 실행 되는 다음 세 가지 동작 중 하나를 선택할 수 있습니다 + <kbd></kbd> + <kbd></kbd> .이 바로 가기는 설정 대화 상자에서 변경할 수 있습니다.

:::image type="content" source="../images/pt-colorpicker-behaviors.png" alt-text="ColorPicker 동작입니다.":::

- **편집기 모드를 사용 하도록 설정 된 색 선택** -색 선택을 선택 하면 편집기가 열리고 선택한 색이 클립보드에 복사 됩니다 (설정 대화 상자의 기본 형식 구성 가능).
- **편집기** 에서 직접 편집기를 엽니다. 여기에서 기록에서 색을 선택 하거나, 선택한 색을 세밀 하 게 조정 하거나, 색 선택을 열어를 사용 하 여 새 색을 캡처할 수 있습니다.
- **색 선택만** -색 선택기만 열고 선택한 색은 클립보드에 복사 됩니다.

### <a name="select-color"></a>색 선택

색 선택이 활성화 되 면 복사 하려는 색 위에 마우스 커서를 놓고 마우스 단추를 마우스 왼쪽 단추로 클릭 하 여 색을 선택 합니다. 커서 주위의 영역을 자세히 확인 하려면 위로 스크롤하여 확대 합니다.

복사 된 색은 설정에서 구성 된 형식 (기본적으로 HEX)으로 클립보드에 저장 됩니다.

![색 선택](../images/pt-colorpicker.gif)

## <a name="editor-usage"></a>편집기 사용

편집기를 사용 하면 선택한 색의 기록 (최대 20 개)을 확인 하 고 미리 정의 된 문자열 형식으로 표시를 복사할 수 있습니다. 표시 되는 순서와 함께 편집기에 표시 되는 색 형식을 구성할 수 있습니다. 이 구성은 Powertoy 설정에서 찾을 수 있습니다.

편집기를 사용 하 여 선택 된 색을 세밀 하 게 조정 하거나 비슷한 색을 새로 가져올 수도 있습니다. 편집기에서 현재 선택 된 색 2 보다 밝은 색 2의 다른 음영을 미리 봅니다.

이러한 대체 색 음영을 클릭 하면 선택 된 색의 기록 (색 기록 목록의 맨 위에 표시 됨)에 선택 항목이 추가 됩니다. 가운데 색은 색 기록에서 현재 선택 된 색을 나타냅니다. 이 단추를 클릭 하면 현재 색의 색상 또는 RGB 값을 변경할 수 있는 미세 조정 구성 컨트롤이 표시 됩니다. 확인을 누르면 새로 구성 된 색이 색 기록에 추가 됩니다.

![ColorPicker 편집기](../images/pt-colorpicker-editor.gif)

색 기록에서 색을 제거 하려면 원하는 색을 마우스 오른쪽 단추로 클릭 하 고 *제거* 를 선택 합니다.

## <a name="settings"></a>설정

색 선택을 통해 다음 설정을 변경할 수 있습니다.

- 활성화 바로 가기
- 활성화 바로 가기 동작
- 복사 된 색의 형식 (16 진수, RGB 등)
- 편집기에서 색 형식의 순서 및 모양

![ColorPicker 설정 스크린 샷](../images/pt-colorpicker-settings.png)

## <a name="limitations"></a>제한 사항

- 시작 메뉴 또는 작업 센터의 맨 위에 색 선택기를 표시할 수 없습니다 (색을 선택할 수 있음).
- 현재 포커스가 있는 응용 프로그램이 관리자 권한 상승을 사용 하 여 시작 된 경우 (관리자 권한으로 실행) Powertoy가 관리자 권한 상승을 사용 하 여 시작 하지 않으면 색 선택 활성화 바로 가기가 작동 하지 않습니다.
