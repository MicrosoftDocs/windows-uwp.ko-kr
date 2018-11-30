---
title: 장치 포털 원격 입력 API 참조
description: Xbox에서 원격으로 컨트롤러, 키보드 및 마우스 입력을 전송하는 방법을 알아보세요.
ms.localizationpriority: medium
ms.openlocfilehash: e0db86ad50bfb1cb27f516243542a554e710c3ea
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8200518"
---
# <a name="remote-input-api-reference"></a>원격 입력 API 참조   
이 API를 통해 원격으로 실시간으로 컨트롤러, 키보드 및 마우스 입력을 보낼 수 있습니다.

**요청**

메서드      | 요청 URI
:------     | :-----
Websocket | /ext/remoteinput
<br />
**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청**

일련의 바이트 배열 메시지인 Websocket입니다. 각 메시지의 형식은 다음과 같습니다.

첫 번째 바이트는 입력 유형을 나타냅니다. 다음 입력 유형이 지원됩니다.

| 입력 유형        | 바이트 값 |
|------------|-------------|
Keyboard KeyCodes | 0x01
Keyboard ScanCodes | 0x02
Mouse | 0x03
Clear All | 0x04

KeyboardKeyCodes 및 KeyboardScanCodes의 경우 두 번째 바이트는 키 코드 또는 스캔 코드 값이며 세 번째 바이트는 아래쪽 누르기에 대해 0x01이며 놓기에 대해 0x00입니다.

마우스 메시지의 경우 다음 값은 네트워크 순서(2바이트)로 마우스 이벤트의 유형을 나타내는 UINT16입니다.

| 작업 유형        | UINT16 값 |
|------------|-------------|
Move | 0x0001
LeftDown | 0x0002
LeftUp | 0x0004
RightDown | 0x0008
RightUp | 0x0010
MiddleDown | 0x0020
MiddleUp | 0x0040
X1Down | 0x0080
X1Up | 0x0100
X2Down | 0x0200
X2Up | 0x0400
VerticalWheelMoved | 0x0800
HorizontalWheelMoved | 0x1000

이 바이트 뒤에는 네트워크 순서로 두 UINT32 값과 휠 동작에 대한 선택적 세 번째 UINT32가 옵니다. 처음 두 개의 값은 X와 Y 좌표이며 세 번째는 휠 델타입니다. X 및 Y 좌표는 가로 및 세로 평면에서 마우스의 상대적인 위치를 나타내는 0과 65535 사이의 값입니다.

ClearAll은 현재 누른 모든 키를 놓아야 함을 나타냅니다. 다른 바이트는 예상되지 않습니다.

게임 패드 입력의 경우 게임 패드 단추 누름을 나타내는 키 코드 값은 KeyboardKeyCodes로 사용할 수 있습니다. 해당 값은 다음과 같습니다.

| 게임 패드 유형        | 바이트 값 |
|------------|-------------|
VK_GAMEPAD_A                       |  0xC3
VK_GAMEPAD_B                       |  0xC4
VK_GAMEPAD_X                       |  0xC5
VK_GAMEPAD_Y                       |  0xC6
VK_GAMEPAD_RIGHT_SHOULDER          |  0xC7
VK_GAMEPAD_LEFT_SHOULDER           |  0xC8
VK_GAMEPAD_LEFT_TRIGGER            |  0xC9
VK_GAMEPAD_RIGHT_TRIGGER           |  0xCA
VK_GAMEPAD_DPAD_UP                 |  0xCB
VK_GAMEPAD_DPAD_DOWN               |  0xCC
VK_GAMEPAD_DPAD_LEFT               |  0xCD
VK_GAMEPAD_DPAD_RIGHT              |  0xCE
VK_GAMEPAD_MENU                    |  0xCF
VK_GAMEPAD_VIEW                    |  0xD0
VK_GAMEPAD_LEFT_THUMBSTICK_BUTTON  |  0xD1
VK_GAMEPAD_RIGHT_THUMBSTICK_BUTTON |  0xD2
VK_GAMEPAD_LEFT_THUMBSTICK_UP      |  0xD3
VK_GAMEPAD_LEFT_THUMBSTICK_DOWN    |  0xD4
VK_GAMEPAD_LEFT_THUMBSTICK_RIGHT   |  0xD5
VK_GAMEPAD_LEFT_THUMBSTICK_LEFT    |  0xD6
VK_GAMEPAD_RIGHT_THUMBSTICK_UP     |  0xD7
VK_GAMEPAD_RIGHT_THUMBSTICK_DOWN   |  0xD8
VK_GAMEPAD_RIGHT_THUMBSTICK_RIGHT  |  0xD9
VK_GAMEPAD_RIGHT_THUMBSTICK_LEFT   |  0xDA


**응답**   

- 없음

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 요청에 성공했습니다.
4XX | 오류 코드
5XX | 오류 코드

<br />
**사용 가능한 디바이스 패밀리**

* Windows Xbox