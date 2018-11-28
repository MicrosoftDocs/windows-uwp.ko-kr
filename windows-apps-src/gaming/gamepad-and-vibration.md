---
title: 게임 패드 및 진동
description: Windows.Gaming.Input 게임 패드 API를 사용하여 진동 및 임펄스 명령을 검색하고 읽고 게임 패드로 전송하세요.
ms.assetid: BB03BB8E-255F-4AE8-AC43-1E519CA860FE
ms.date: 09/06/2018
ms.topic: article
keywords: windows 10, uwp, 게임, 게임 패드, 진동
ms.localizationpriority: medium
ms.openlocfilehash: e65b22039c381bd333516bd9f98c60bbddb9621c
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7852838"
---
# <a name="gamepad-and-vibration"></a>게임 패드 및 진동

이 페이지에서는 UWP(유니버설 Windows 플랫폼)용 [Windows.Gaming.Input.Gamepad][gamepad] 및 관련 API를 사용하여 Xbox One 게임 패드용 프로그래밍의 기본 사항을 설명합니다.

이 페이지에서는 다음에 대해 알아봅니다.

* 연결된 게임 패드 및 사용자 목록을 수집하는 방법
* 게임 패드가 추가 또는 제거된 사실을 감지하는 방법
* 하나 이상의 게임 패드에서 입력을 읽는 방법
* 진동 및 임펄스 명령을 전송하는 방법
* 게임 패드 UI 탐색 장치로 동작 하는 방법

## <a name="gamepad-overview"></a>게임 패드 개요

Xbox 무선 컨트롤러 및 Xbox 무선 컨트롤러 S와 같은 게임 패드는 범용 게임 입력 장치입니다. 게임 패드는 Xbox One의 표준 입력 장치로, 키보드와 마우스를 선호하지 않는 Windows 게이머가 사용하는 경우가 많습니다. 게임 패드는 Windows 10 및 Xbox UWP 앱에서 [Windows.Gaming.Input][] 네임스페이스로 지원됩니다.

방향 패드 (또는 D 패드); Xbox One 게임 패드는 설치 되어 있습니다. **A**, **B**, **X**, **Y**, **보기**및 **메뉴** 단추입니다. 왼쪽 및 오른쪽 섬, 범퍼 및 트리거 한 총 4 개의 진동 모터가 합니다. 두 섬스틱(thumbstick)은 X 및 Y 축에서 이중 아날로그 판독값을 제공하며, 안쪽으로 눌리면 버튼 역할도 합니다. 각 트리거는 얼마나 것 끌어오는 다시 나타내는 아날로그 판독값을 제공 합니다.

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **Paddle** buttons on its underside. These can be used to provide redundant access to game commands that are difficult to use together (such as the right thumbstick together with any of the **A**, **B**, **X**, or **Y** buttons) or to provide dedicated access to additional commands. -->

> [!NOTE]
> `Windows.Gaming.Input.Gamepad` 표준 Xbox One 게임 패드와 동일한 컨트롤 레이아웃을 가진 Xbox 360 게임 패드도 지원 합니다.

### <a name="vibration-and-impulse-triggers"></a>진동 및 임펄스 트리거

Xbox One 게임 패드는 강력한 게임 패드 진동과 미세한 게임 패드 진동을 위한 두 개의 독립적 모터와 각 트리거에 선명한 진동을 제공하기 위한 전용 모터 두 개를 제공합니다(이 고유한 기능 때문에 Xbox One 게임 패드 트리거를 _임펄스 트리거_라고 함).

> [!NOTE]
> Xbox 360 게임 패드는 _임펄스 트리거_를 사용 하 여 탑재 되어 있지 않습니다.

자세한 내용은 [진동 및 임펄스 트리거 개요](#vibration-and-impulse-triggers-overview)를 참조하세요.

### <a name="thumbstick-deadzones"></a>섬스틱(thumbstick) 데드존

중앙 위치에 서 있는 섬스틱(thumbstick)은 매번 X 축과 Y 축에 동일한 중립 판독값을 이상적으로 생성합니다. 하지만 섬스틱(thumbstick)의 기계력과 민감도로 인해 중앙 위치의 실제 판독값은 이상적인 중립값의 근사치일 뿐이며 후속 판독값마다 달라질 수 있습니다. 이러한 이유로 항상 작은 _데드 존_를 사용 해야&mdash;무시 되는 이상적 중앙 위치 근처의 값 범위&mdash;제조상의 차이, 기계적 마모 또는 기타 게임 패드 문제를 보완 하기 위해 합니다.

큰 데드존은 의도한 입력과 의도치 않은 입력을 구분하기 위한 간단한 전략입니다.

자세한 내용은 [섬스틱(thumbstick) 읽기](#reading-the-thumbsticks)를 참조하세요.

### <a name="ui-navigation"></a>UI 탐색

사용자 인터페이스 탐색을 위해 다양한 입력 장치를 지원해야 하는 부담을 덜고 게임과 장치 간 일관성을 추구하기 위해 대부분의 _물리적_ 입력 장치는 [UI 탐색 컨트롤러](ui-navigation-controller.md)라고 하는 별도의 _논리적_ 입력 장치 역할을 동시에 수행합니다. UI 탐색 컨트롤러는 입력 장치 전반적으로 UI 탐색 명령에 대한 공통 어휘를 제공합니다.

UI 탐색 컨트롤러로 게임 패드는 탐색 명령의 [필수 집합](ui-navigation-controller.md#required-set) 왼쪽된 엄지 스틱, D-패드, **보기**, **메뉴**, **A**및 **B** 버튼에 매핑합니다.

| 탐색 명령 | 게임 패드 입력                       |
| ------------------:| ----------------------------------- |
|                 위쪽 | 왼쪽 섬스틱(thumbstick) 위쪽/D 패드 위쪽       |
|               아래쪽 | 왼쪽 섬스틱(thumbstick) 아래쪽/D 패드 아래쪽   |
|               왼쪽 | 왼쪽 섬스틱(thumbstick) 왼쪽/D 패드 왼쪽   |
|              오른쪽 | 왼쪽 섬스틱(thumbstick) 오른쪽/D 패드 오른쪽 |
|               보기 | 보기 버튼                         |
|               메뉴 | 메뉴 버튼                         |
|             수락 | A 버튼                            |
|             취소 | B 버튼                            |

또한 게임 패드는 탐색 명령의 모든 [선택 집합](ui-navigation-controller.md#optional-set)을 나머지 입력에 매핑합니다.

| 탐색 명령 | 게임 패드 입력          |
| ------------------:| ---------------------- |
|            Page Up | 왼쪽 트리거           |
|          Page Down | 오른쪽 트리거          |
|          Page Left | 왼쪽 범퍼            |
|         Page Right | 오른쪽 범퍼           |
|          Scroll Up | 오른쪽 섬스틱(thumbstick) 위쪽    |
|        Scroll Down | 오른쪽 섬스틱(thumbstick) 아래쪽  |
|        Scroll Left | 오른쪽 섬스틱(thumbstick) 왼쪽  |
|       Scroll Right | 오른쪽 섬스틱(thumbstick) 오른쪽 |
|          컨텍스트 1 | X 버튼               |
|          컨텍스트 2 | Y 버튼               |
|          컨텍스트 3 | 왼쪽 섬스틱(thumbstick) 누름  |
|          컨텍스트 4 | 오른쪽 섬스틱(thumbstick) 누름 |

## <a name="detect-and-track-gamepads"></a>게임 패드 감지 및 추적

게임 패드는 시스템에서 관리하므로 생성하거나 초기화할 필요가 없습니다. 게임 패드가 추가되거나 제거되면 시스템에서 연결된 게임 패드 및 이벤트 목록을 표시하여 알려 줍니다.

### <a name="the-gamepads-list"></a>게임 패드 목록

[Gamepad][] 클래스는 정적 속성 [Gamepads][]를 제공하는데, 이는 현재 연결된 게임 패드의 읽기 전용 목록입니다. 연결 된 게임 패드 중 일부에 관심이 있기, 때문에 것이 좋습니다 통해 액세스 하는 대신 자체 컬렉션을 유지 관리 하는 `Gamepads` 속성입니다.

다음 예제에서는 연결된 모든 게임 패드를 새 컬렉션에 복사합니다. 참고 백그라운드에서 다른 스레드에서 됩니다 ( [GamepadAdded][] 및 [GamepadRemoved][] 이벤트)에서이 컬렉션에 액세스 하기 때문에 할 경우 잠금을 읽거나 컬렉션을 업데이트 하는 코드를 배치 합니다.

```cpp
auto myGamepads = ref new Vector<Gamepad^>();
critical_section myLock{};

for (auto gamepad : Gamepad::Gamepads)
{
    // Check if the gamepad is already in myGamepads; if it isn't, add it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), gamepad);

    if (it == end(myGamepads))
    {
        // This code assumes that you're interested in all gamepads.
        myGamepads->Append(gamepad);
    }
}
```

```cs
private readonly object myLock = new object();
private List<Gamepad> myGamepads = new List<Gamepad>();
private Gamepad mainGamepad;

private void GetGamepads()
{
    lock (myLock)
    {
        foreach (var gamepad in Gamepad.Gamepads)
        {
            // Check if the gamepad is already in myGamepads; if it isn't, add it.
            bool gamepadInList = myGamepads.Contains(gamepad);

            if (!gamepadInList)
            {
                // This code assumes that you're interested in all gamepads.
                myGamepads.Add(gamepad);
            }
        }
    }   
}
```

### <a name="adding-and-removing-gamepads"></a>게임 패드 추가 및 제거

게임 패드 추가 되거나 제거 된 경우 [GamepadAdded][] 및 [GamepadRemoved][] 이벤트가 발생 합니다. 이러한 이벤트의 처리기를 등록하면 현재 연결된 게임 패드를 추적할 수 있습니다.

다음 예제에서는 추가된 게임 패드의 추적을 시작합니다.

```cpp
Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    // Check if the just-added gamepad is already in myGamepads; if it isn't, add
    // it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), args);

    if (it == end(myGamepads))
    {
        // This code assumes that you're interested in all new gamepads.
        myGamepads->Append(args);
    }
}
```

```cs
Gamepad.GamepadAdded += (object sender, Gamepad e) =>
{
    // Check if the just-added gamepad is already in myGamepads; if it isn't, add
    // it.
    lock (myLock)
    {
        bool gamepadInList = myGamepads.Contains(e);

        if (!gamepadInList)
        {
            myGamepads.Add(e);
        }
    }
};
```

다음 예제에서는 제거 된 게임 패드의 추적을 중지 합니다. 또한; 제거할 때 추적 하는 게임 패드 어떻게 처리 해야 이 코드만 한 게임 패드에서 입력을 추적 하 고 간단 하 게 설정 하는 예를 들어 `nullptr` 제거 됩니다. 모든 프레임에 게임 패드, 활성 상태 이면과 게임 패드 컨트롤러에 연결 되 고 연결이 끊어진 경우의 입력을 수집 중인 하면 업데이트를 확인 해야 합니다.

```cpp
Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;
    critical_section::scoped_lock lock{ myLock };

    if(myGamepads->IndexOf(args, &indexRemoved))
    {
        if (m_gamepad == myGamepads->GetAt(indexRemoved))
        {
            m_gamepad = nullptr;
        }

        myGamepads->RemoveAt(indexRemoved);
    }
}
```

```cs
Gamepad.GamepadRemoved += (object sender, Gamepad e) =>
{
    lock (myLock)
    {
        int indexRemoved = myGamepads.IndexOf(e);

        if (indexRemoved > -1)
        {
            if (mainGamepad == myGamepads[indexRemoved])
            {
                mainGamepad = null;
            }

            myGamepads.RemoveAt(indexRemoved);
        }
    }
};
```

자세한 내용은 [게임용 입력을](input-practices-for-games.md) 참조 하세요.

### <a name="users-and-headsets"></a>사용자 및 헤드셋

각 게임 패드를 사용자 계정과 연결하여 해당 ID를 게임 플레이에 연결할 수 있고, 음성 채팅 또는 게임 내 기능을 도와주는 헤드셋을 연결할 수 있습니다. 사용자 및 헤드셋 관련 작업에 대해 자세히 알아보려면 [사용자 및 장치 추적](input-practices-for-games.md#tracking-users-and-their-devices)과 [헤드셋](headset.md)을 참조하세요.

## <a name="reading-the-gamepad"></a>게임 패드 읽기

관심 있는 게임 패드를 식별하면 장치의 입력을 수집할 준비가 된 것입니다. 하지만 기존에 사용하던 다른 입력 유형과 달리 게임 패드는 이벤트 발생을 통해 상태 변경을 알리지 않습니다. 대신, 게임 패드를 직접 _폴링_하여 현재 상태의 규칙적인 판독값을 가져올 수 있습니다.

### <a name="polling-the-gamepad"></a>게임 패드 폴링

폴링은 정확한 시점에 탐색 장치의 스냅숏을 캡처합니다. 이러한 입력 수집 방법은 대부분의 게임에 적합합니다. 게임의 논리는 일반적으로 이벤트 기반 방식이 아닌 결정적 루프로 실행되기 때문입니다. 또한 시간을 두고 수집된 여러 단일 입력보다 한 번에 수집된 입력에서 게임 명령을 해석하는 것이 일반적으로 더 간단합니다.

게임 패드는 [GetCurrentReading][]을 호출하여 폴링합니다. 이 함수는 게임 패드의 상태가 포함된 [GamepadReading][]을 반환합니다.

다음 예제에서는 게임 패드의 현재 상태를 폴링합니다.

```cpp
auto gamepad = myGamepads[0];

GamepadReading reading = gamepad->GetCurrentReading();
```

```cs
Gamepad gamepad = myGamepads[0];

GamepadReading reading = gamepad.GetCurrentReading();
```

게임 패드 상태 외에도 각 판독값에는 상태가 검색된 시기를 정확히 나타내는 타임스탬프가 포함되어 있습니다. 타임스탬프는 이전 판독값의 타이밍 또는 게임 시뮬레이션의 타이밍을 이해하는 데 유용합니다.

### <a name="reading-the-thumbsticks"></a>섬스틱(thumbstick) 읽기

각 섬스틱(thumbstick)은 X 축과 Y 축에 -1.0과 +1.0 사이의 아날로그 판독값을 제공합니다. X 축에서 -1.0 값은 맨 왼쪽 섬스틱(thumbstick) 위치에 해당하고, +1.0 값은 맨 오른쪽 위치에 해당합니다. Y 축에서 -1.0 값은 맨 아래쪽 섬스틱(thumbstick) 위치에 해당하고, +1.0 값은 맨 위쪽 위치에 해당합니다. 양 축 값은 약 0.0 스틱이 중앙 위치에 있지만 달라 정밀 값은 후속 판독값;까지 이러한 변동을 완화 하기 위한 전략이이 섹션의 뒷부분에 설명 되어 있습니다.

왼쪽 섬스틱(thumbstick)의 X 축 값은 [GamepadReading][] 구조의 `LeftThumbstickX` 속성에서 읽어들이고, Y 축 값은 `LeftThumbstickY` 속성에서 읽어들입니다. 오른쪽 섬스틱(thumbstick)의 X 축 값은 `RightThumbstickX` 속성에서 읽어들이고, Y 축 값은 `RightThumbstickY` 속성에서 읽어들입니다.

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
float rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
float rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

```cs
double leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
double leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
double rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
double rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

섬스틱(thumbstick) 값을 읽을 때 섬스틱이 중앙 위치에 서 있으면 중립 판독값 0.0이 안정적으로 생성되지 않지만 섬스틱이 이동하여 중앙 위치로 돌아갈 때마다 0.0에 가까운 다른 값이 생성된다는 사실을 알 수 있습니다. 이러한 변동을 완화하기 위해 무시되는 이상적 중앙 위치 근처의 값 범위인 _데드존_을 작게 구현할 수 있습니다. 데드존을 구현하는 한 가지 방법은 섬스틱(thumbstick)이 중앙에서 얼마나 이동했는지 확인하고 선택한 거리보다 더 가까운 판독값을 무시하는 것입니다. 대략 거리를 계산할 수 있습니다&mdash;것 이므로 정확 하지 엄지 스틱 판독값은 기본적으로 평면이 아닌 하지 값&mdash;는 피타고라스의 원리를 사용 하 여 합니다. 그러면 방사형 데드존이 생성됩니다.

다음 예제는 피타고라스의 원리를 사용하는 기본 방사형 데드존을 보여 줍니다.

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0

// choose a deadzone -- readings inside this radius are ignored.
const float deadzoneRadius = 0.1;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem -- for a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

// accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) > deadzoneSquared)
{
    // input accepted, process it
}
```

```cs
double leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
double leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0

// choose a deadzone -- readings inside this radius are ignored.
const double deadzoneRadius = 0.1;
const double deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem -- for a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
double oppositeSquared = leftStickY * leftStickY;
double adjacentSquared = leftStickX * leftStickX;

// accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) > deadzoneSquared)
{
    // input accepted, process it
}
```

각 섬스틱(thumbstick)은 안쪽으로 눌릴 경우 버튼 역할도 합니다. 이러한 입력을 읽는 방법에 대한 자세한 내용은 [버튼 읽기](#reading-the-buttons)를 참조하세요.

### <a name="reading-the-triggers"></a>트리거 읽기

트리거는 0.0(완전히 놓임) 및 1.0(완전히 눌림) 사이의 부동 소수점 값으로 표현됩니다. 왼쪽 트리거의 값은 [GamepadReading][] 구조의 `LeftTrigger` 속성에서 읽어들이고, 오른쪽 트리거의 값은 `RightTrigger` 속성에서 읽어들입니다.

```cpp
float leftTrigger  = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
float rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

```cs
double leftTrigger = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
double rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

### <a name="reading-the-buttons"></a>버튼 읽기

각 게임 패드 버튼&mdash;4 방향 D-패드, 왼쪽 및 오른쪽 범퍼, 왼쪽 및 오른쪽 섬 스틱 누르기, **A**, **B**, **X**, **Y**, **보기**및 **메뉴**&mdash;디지털 읽기를 제공 합니다. 누름 (아래) 또는 놓음 (위)에 있는지 여부를 나타냅니다. 버튼 판독값은 효율을 위해 개별 부울 값으로 표현 되지 않는 대신, 모두 압축 [GamepadButtons][] 열거로 표현 되는 단일 비트 필드로 합니다.

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **paddle** buttons on its underside. These buttons are also represented in the `GamepadButtons` enumeration and their values are read in the same way as the standard gamepad buttons. -->

버튼 값은 [GamepadReading][] 구조의 `Buttons` 속성에서 읽어들입니다. 이러한 속성은 비트 필드이므로 해당 버튼의 값을 격리하기 위해 비트 마스킹이 사용됩니다. 해당 비트가 설정된 경우에는 버튼이 눌리고(아래), 그렇지 않은 경우에는 놓입니다(위).

다음 예제에서는 A 버튼이 눌렸는지 확인합니다.

```cpp
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}
```

```cs
if (GamepadButtons.A == (reading.Buttons & GamepadButtons.A))
{
    // button A is pressed
}
```

다음 예제에서는 A 버튼이 놓였는지 확인합니다.

```cpp
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // button A is released
}
```

```cs
if (GamepadButtons.None == (reading.Buttons & GamepadButtons.A))
{
    // button A is released
}
```

때로는 하려고 하에서 단추 전환 하는 시기를 결정에 릴리스된 놓았는지 누름에 여러 단추의 눌 렸 거 나 놓였는지 또는 일련의 버튼이 특정 방식으로 정렬 된 경우&mdash;리고 일부 합니다. 이러한 각 상태를 검색하는 방법에 대한 자세한 내용은 [버튼 전환 검색](input-practices-for-games.md#detecting-button-transitions) 및 [복잡한 버튼 정렬 검색](input-practices-for-games.md#detecting-complex-button-arrangements)을 참조하세요.

## <a name="run-the-gamepad-input-sample"></a>게임 패드 입력 샘플 실행

[GamepadUWP 샘플 _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadUWP)은 게임 패드에 연결하여 상태를 읽는 방법을 보여 줍니다.

## <a name="vibration-and-impulse-triggers-overview"></a>진동 및 임펄스 트리거 개요

게임 패드 내 진동 모터는 사용자에게 촉각 피드백을 제공하는 데 사용됩니다. 게임에서는 더 높은 몰입감을 이끌어내거나, 상태 정보(예: 공격 받는 중)를 알리는 데 도움을 주거나, 중요한 개체에 근접 신호를 전송하는 등 창의적 용도에 이 기능을 사용합니다.

Xbox One 게임 패드에는 총 4개의 독립적 진동 모터가 탑재되어 있습니다. 두 개는 게임 패드 본체;에 있는 대형 모터 왼쪽된 모터는 오른쪽 모터 보다 더 미세한 진동을 제공 하는 동안 진폭 진동을 제공 합니다. 다른 두 모터는 소형 모터입니다. 하나는 각 트리거 안에 있는 모터로, 사용자의 트리거 손가락에 즉시 선명하게 파열되는 진동을 제공합니다. Xbox One 게임 패드의 이러한 고유한 기능 때문에 Xbox One 게임 패드 트리거를 _임펄스 트리거_라고 합니다. 이러한 모터를 함께 오케스트레이션하면 광범위한 촉감을 재현할 수 있습니다.

## <a name="using-vibration-and-impulse"></a>진동 및 임펄스 사용

게임 패드 진동은 [Gamepad][] 클래스의 [Vibration][] 속성을 통해 제어됩니다. `Vibration` 부동 소수점 값 4개로 이루어진 [GamepadVibration][] 구조의 인스턴스입니다. 각 값은 모터 중 하나의 강도를 나타냅니다.

하지만의 멤버는 `Gamepad.Vibration` 속성을 직접 수정할 수 있는, 별도 초기화 하는 것이 좋습니다. `GamepadVibration` 값을 및에 복사 하는 인스턴스는 `Gamepad.Vibration` 실제 모터 강도 모두 한 번에 변경 하는 속성입니다.

다음 예제에서는 모터 강도를 모두 한 번에 변경하는 방법을 보여 줍니다.

```cpp
// get the first gamepad
Gamepad^ gamepad = Gamepad::Gamepads->GetAt(0);

// create an instance of GamepadVibration
GamepadVibration vibration;

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration;
```

```cs
// get the first gamepad
Gamepad gamepad = Gamepad.Gamepads[0];

// create an instance of GamepadVibration
GamepadVibration vibration = new GamepadVibration();

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration;
```

### <a name="using-the-vibration-motors"></a>진동 모터 사용

왼쪽 및 오른쪽 진동 모터는 0.0(진동 없음) 및 1.0(가장 강한 진동) 사이의 부동 소수점 값을 사용합니다. 왼쪽 모터의 강도는 [GamepadVibration][] 구조의 `LeftMotor` 속성으로 설정되고, 오른쪽 모터의 강도는 `RightMotor` 속성으로 설정됩니다.

다음 예제에서는 두 진동 모터의 강도를 설정하고 게임 패드 진동을 활성화합니다.

```cpp
GamepadVibration vibration;
vibration.LeftMotor = 0.80;  // sets the intensity of the left motor to 80%
vibration.RightMotor = 0.25; // sets the intensity of the right motor to 25%
gamepad.Vibration = vibration;
```

```cs
GamepadVibration vibration = new GamepadVibration();
vibration.LeftMotor = 0.80;  // sets the intensity of the left motor to 80%
vibration.RightMotor = 0.25; // sets the intensity of the right motor to 25%
mainGamepad.Vibration = vibration;
```

이러한 두 모터는 동일하지 않으므로 이러한 속성을 동일한 값으로 설정하면 한 모터에 다른 모터와 동일한 진동이 재현되지 않습니다. 모든 값 왼쪽된 모터는 오른쪽 모터 보다 더 낮은 주파수의 더 강력한 진동의 진동을&mdash;동일한 값&mdash;더 높은 주파수의 더 미세한 진동을 재현 합니다. 최대값에서도 왼쪽 모터는 오른쪽 모터의 고주파를 생성할 수 없으며, 오른쪽 모터도 왼쪽 모터의 강력한 힘을 재현할 수 없습니다. 하지만 모터는 게임 패드 본체로 견고하게 연결되어 있기 때문에 플레이어는 모터가 다른 특성을 가지고 다른 강도로 진동할 수 있더라도 진동이 완전히 독립적이라고 느끼지 못합니다. 이러한 정렬은 모터가 동일한 경우보다 더 광범위하고, 더 풍부한 촉감을 재현할 수 있습니다.

### <a name="using-the-impulse-triggers"></a>임펄스 트리거 사용

각 임펄스 트리거 모터는 0.0(진동 없음) 및 1.0(가장 강한 진동) 사이의 부동 소수점 값을 사용합니다. 왼쪽 트리거 모터의 강도는 [GamepadVibration][] 구조의 `LeftTrigger` 속성으로 설정되고, 오른쪽 트리거의 강도는 `RightTrigger` 속성으로 설정됩니다.

다음 예제에서는 두 임펄스 트리거의 강도를 설정하고 활성화합니다.

```cpp
GamepadVibration vibration;
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
gamepad.Vibration = vibration;
```

```cs
GamepadVibration vibration = new GamepadVibration();
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
mainGamepad.Vibration = vibration;
```

트리거 내 두 진동 모터는 다른 모터와 달리 동일하므로 동일한 값에서 동일한 진동을 재현합니다. 하지만 이러한 모터는 견고하게 연결되어 있지 않으므로 플레이어는 진동을 독립적으로 느낍니다. 이러한 정렬은 완전히 독립적인 촉감을 두 트리거에 동시에 전달할 수 있으며 게임 패드 본체의 모터보다 더 구체적인 정보를 전달할 수 있습니다.

## <a name="run-the-gamepad-vibration-sample"></a>게임 패드 진동 샘플 실행

[GamepadVibrationUWP 샘플 _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadVibrationUWP)은 게임 패드 진동 모터와 임펄스 트리거를 사용하여 다양한 효과를 생성하는 방법을 보여 줍니다.

## <a name="see-also"></a>참고 항목

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [게임 입력 시스템](input-practices-for-games.md)

[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[gamepad]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.aspx
[gamepads]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepads.aspx
[gamepadadded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepadadded.aspx
[gamepadremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepadremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.getcurrentreading.aspx
[vibration]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.vibration.aspx
[gamepadreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadreading.aspx
[gamepadbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadbuttons.aspx
[gamepadvibration]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadvibration.aspx
