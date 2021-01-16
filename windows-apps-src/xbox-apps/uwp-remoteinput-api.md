---
title: 장치 포털 원격 입력 API 참조
description: Xbox에서 컨트롤러, 키보드 및 마우스 입력을 원격으로 보내는 방법에 대해 알아봅니다.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 34546f5aa7a1fd5bddd9122d24fc38dd2026700d
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254248"
---
# <a name="remote-input-api-reference"></a>원격 입력 API 참조   
이 API를 통해 원격으로 컨트롤러, 키보드 및 마우스 입력을 실시간으로 보낼 수 있습니다.

**요청**

| 방법 | 요청 URI |
|--------|-------------|
| 서버당 | /ext/remoteinput |

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청**

연속 하는 바이트 배열 메시지의 websocket입니다. 각 메시지에 대 한 형식은 다음과 같습니다.

첫 번째 바이트는 입력 유형을 나타냅니다. 다음 입력 형식이 지원 됩니다.

| 입력 형식 | 바이트 값 |
|------------|------------|
| 키보드 키 코드 | 0x01 |
| 키보드 스캔 코드 | 0x02 |
| 마우스 | 0x03 |
| 모두 지우기 | 0x04 |

KeyboardKeyCodes 및 KeyboardScanCodes의 경우 두 번째 바이트는 keycode 또는 scancode의 값이 고, 세 번째 바이트는 중단 된 경우 두 번째 바이트이 고, 릴리스에 대해 0x00 인 경우 0x00입니다.

마우스 메시지의 경우 다음 값은 마우스 이벤트의 유형을 나타내는 네트워크 순서 (2 바이트)의 UINT16입니다.

| 액션 유형 | UINT16 값 |
|-------------|--------------|
| 이동 | 0x0001 |
| 왼쪽 아래로 | 0x0002 |
| 왼쪽 위 | 0x0004 |
| 오른쪽 아래 | 0x0008 |
| RightUp | 0x0010 |
| MiddleDown | 0x0020 |
| MiddleUp | 0x0040 |
| X1Down | 0x0080 |
| X1Up | 0x0100 |
| X2Down | 0x0200 |
| X2Up | 0x0400 |
| VerticalWheelMoved | 0x0800 |
| HorizontalWheelMoved | 0x1000 |

이 바이트 다음에는 네트워크 순서에서 두 개의 UINT32 값이 오고, 휠 동작에 대 한 선택적 세 번째 UINT32가 있습니다. 처음 두 값은 X 및 Y 좌표이 고, 세 번째 값은 휠 델타입니다. X 및 Y 좌표는 가로 및 세로 평면에서 마우스의 상대 위치를 나타내는 0에서 65535 사이의 값 이어야 합니다.

ClearAll은 현재 보관 된 키를 해제 해야 함을 나타냅니다. 다른 바이트는 필요 하지 않습니다.

게임 패드 입력을 보내기 위해 게임 패드 단추 누름을 나타내는 keycode 값을 KeyboardKeyCodes과 함께 사용할 수 있습니다. 이러한 값은 다음과 같습니다.

| 게임 패드 유형 | 바이트 값 |
|--------------|------------|
| VK_GAMEPAD_A                       |  0xC3을 지정합니다. |
| VK_GAMEPAD_B                       |  0xC4 |
| VK_GAMEPAD_X                       |  0xC5 |
| VK_GAMEPAD_Y                       |  0xC6 |
| VK_GAMEPAD_RIGHT_SHOULDER          |  0xC7 |
| VK_GAMEPAD_LEFT_SHOULDER           |  0xC8 |
| VK_GAMEPAD_LEFT_TRIGGER            |  0xC9 |
| VK_GAMEPAD_RIGHT_TRIGGER           |  0xCA |
| VK_GAMEPAD_DPAD_UP                 |  0xCB |
| VK_GAMEPAD_DPAD_DOWN               |  0xCC |
| VK_GAMEPAD_DPAD_LEFT               |  0xCD |
| VK_GAMEPAD_DPAD_RIGHT              |  0xCE |
| VK_GAMEPAD_MENU                    |  0xCF |
| VK_GAMEPAD_VIEW                    |  0xD0 |
| VK_GAMEPAD_LEFT_THUMBSTICK_BUTTON  |  0xD1 |
| VK_GAMEPAD_RIGHT_THUMBSTICK_BUTTON |  0xD2 |
| VK_GAMEPAD_LEFT_THUMBSTICK_UP      |  0xD3 |
| VK_GAMEPAD_LEFT_THUMBSTICK_DOWN    |  0xD4 |
| VK_GAMEPAD_LEFT_THUMBSTICK_RIGHT   |  0xD5 |
| VK_GAMEPAD_LEFT_THUMBSTICK_LEFT    |  0xD6 |
| VK_GAMEPAD_RIGHT_THUMBSTICK_UP     |  0xD7 |
| VK_GAMEPAD_RIGHT_THUMBSTICK_DOWN   |  0X8 |
| VK_GAMEPAD_RIGHT_THUMBSTICK_RIGHT  |  0xD9 |
| VK_GAMEPAD_RIGHT_THUMBSTICK_LEFT   |  0xDA |

**Response**   

- 없음

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

HTTP 상태 코드 | Description |
|----------------|-------------|
| 200 | 요청 성공 |
| 4XX | 오류 코드 |
| 5XX | 오류 코드 |

**사용 가능한 장치 패밀리**

* Windows Xbox
