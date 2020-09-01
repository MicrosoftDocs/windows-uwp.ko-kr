---
title: 게임 패드 및 진동
description: 임펄스 게임 패드 Api를 사용 하 여 진동 및 gamepads 명령을 검색 하 고 읽고 보냅니다.
ms.assetid: BB03BB8E-255F-4AE8-AC43-1E519CA860FE
ms.date: 09/06/2018
ms.topic: article
keywords: windows 10, uwp, 게임, 게임 패드, 진동
ms.localizationpriority: medium
ms.openlocfilehash: 66844b78893ffa8cb92b6b17bd11d87c1d4b1aa0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175327"
---
# <a name="gamepad-and-vibration"></a>게임 패드 및 진동

이 페이지에서는 gamepads [를 사용 하]는 Xbox one의 프로그래밍에 대 한 기본 사항 및 UWP (유니버설 Windows 플랫폼)에 대 한 관련[api를 설명] 합니다.

이 페이지를 읽으면 다음에 대해 알아봅니다.

* 연결 된 gamepads 및 해당 사용자 목록을 수집 하는 방법
* 추가 또는 제거 된 게임 패드를 검색 하는 방법
* 하나 이상의 gamepads에서 입력을 읽는 방법
* 진동 및 임펄스 명령을 보내는 방법
* gamepads가 UI 탐색 장치로 동작 하는 방식

## <a name="gamepad-overview"></a>게임 패드 개요

Xbox Wireless Controller 및 Xbox Wireless Controller와 같은 Gamepads는 범용 게임 입력 장치입니다. Xbox One의 표준 입력 장치 이며 키보드와 마우스를 선호 하지 않는 경우 Windows 게이머를 위한 일반적인 선택입니다. Gamepads [네임 스페이스는 windows 10][] 및 Xbox UWP 앱에서 지원 됩니다.

Xbox One gamepads에는 방향 패드 (또는 D-패드)가 장착 되어 있습니다. **A**, **B**, **X**, **Y**, **보기**및 **메뉴** 단추 왼쪽 및 오른쪽 thumbsticks, 범퍼 및 트리거 그리고 총 4 개의 진동 모터가 있습니다. 두 thumbsticks 모두 X 축과 Y 축에서 이중 아날로그 판독값을 제공 하 고 안쪽으로 누를 때 단추 역할을 합니다. 각 트리거는 다시 가져오는 정도를 나타내는 아날로그 읽기를 제공 합니다.

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **Paddle** buttons on its underside. These can be used to provide redundant access to game commands that are difficult to use together (such as the right thumbstick together with any of the **A**, **B**, **X**, or **Y** buttons) or to provide dedicated access to additional commands. -->

> [!NOTE]
> `Windows.Gaming.Input.Gamepad` 는 표준 Xbox One gamepads와 동일한 컨트롤 레이아웃을 가진 Xbox 360 gamepads도 지원 합니다.

### <a name="vibration-and-impulse-triggers"></a>진동 및 임펄스 트리거

Xbox One gamepads는 각 트리거에 선명한 진동 기능을 제공 하기 위한 두 가지 전용 모터 뿐만 아니라 강력 하 고 미묘한 게임 진동에 대해 두 개의 독립 된 모터를 제공 합니다 .이 고유한 기능은 Xbox One 게임 패드 트리거를 _임펄스 트리거_라고 하는 이유입니다.

> [!NOTE]
> Xbox 360 gamepads에는 _임펄스 트리거가_설치 되어 있지 않습니다.

자세한 내용은 [진동 및 임펄스 트리거 개요](#vibration-and-impulse-triggers-overview)를 참조 하세요.

### <a name="thumbstick-deadzones"></a>엄지 스틱 deadzones

가운데 위치에 있는 엄지 스틱은 항상 X 및 Y 축에 동일한 중립 읽기를 생성 합니다. 그러나 기계적 병력 및 엄지 스틱의 민감도 때문에 중심 위치의 실제 판독값은 이상적인 중립 값에 불과합니다. 이후 판독값에 따라 달라질 수 있습니다. 따라서 _deadzone_ &mdash; &mdash; 제조 차이, 기계적 마모 또는 기타 게임 패드 문제를 보완 하기 위해 무시 되는 이상적인 센터 위치 근처의 작은 deadzone 값 범위를 사용 해야 합니다.

큰 deadzones는 의도 하지 않은 입력 으로부터 의도적인 입력을 구분 하기 위한 간단한 전략을 제공 합니다.

자세한 내용은 [thumbsticks를](#reading-the-thumbsticks)참조 하세요.

### <a name="ui-navigation"></a>UI 탐색

사용자 인터페이스 탐색을 위한 다양 한 입력 장치를 지원 하 고 게임과 장치 간에 일관성을 유지 하기 위해 대부분의 _물리적_ 입력 장치는 [UI 탐색 컨트롤러](ui-navigation-controller.md)라는 별도의 _논리적_ 입력 장치로 동시에 작동 합니다. UI 탐색 컨트롤러는 입력 장치에서 UI 탐색 명령에 대 한 일반적인 어휘를 제공 합니다.

UI 탐색 컨트롤러인 gamepads는 필요한 탐색 명령 [집합](ui-navigation-controller.md#required-set) 을 왼쪽 엄지 스틱, D-패드, **뷰**, **메뉴**, **a**및 **B** 단추에 매핑합니다.

| 탐색 명령 | 게임 패드 입력                       |
| ------------------:| ----------------------------------- |
|                 위로 | 왼쪽 엄지 스틱 위쪽/D-패드 위로       |
|               아래로 | 왼쪽 엄지 스틱 아래쪽/D-패드 아래로   |
|               왼쪽 | 왼쪽 엄지 스틱 왼쪽/D-패드 왼쪽   |
|              오른쪽 | 왼쪽 엄지 스틱 오른쪽/D-패드 오른쪽 |
|               보기 | 보기 단추                         |
|               메뉴 | 메뉴 버튼                         |
|             동의함 | 단추                            |
|             취소 | B 단추                            |

또한 gamepads는 모든 선택적인 탐색 명령 [집합](ui-navigation-controller.md#optional-set) 을 나머지 입력에 매핑합니다.

| 탐색 명령 | 게임 패드 입력          |
| ------------------:| ---------------------- |
|            Page Up | 왼쪽 트리거           |
|          Page Down | 오른쪽 트리거          |
|          왼쪽 페이지 | 왼쪽 범퍼            |
|         오른쪽 페이지 | 오른쪽 범퍼           |
|          위로 스크롤 | 오른쪽 엄지 스틱 위쪽    |
|        아래로 스크롤 | 오른쪽 엄지 스틱 아래로  |
|        왼쪽으로 스크롤 | 오른쪽 엄지 스틱 왼쪽  |
|       오른쪽으로 스크롤 | 오른쪽 섬스틱(thumbstick) 오른쪽 |
|          컨텍스트 1 | X 단추               |
|          컨텍스트 2 | Y 단추               |
|          컨텍스트 3 | 왼쪽 엄지 스틱 누르기  |
|          컨텍스트 4 | 오른쪽 엄지 스틱 누르기 |

## <a name="detect-and-track-gamepads"></a>Gamepads 검색 및 추적

Gamepads는 시스템에서 관리 되므로이를 만들거나 초기화할 필요가 없습니다. 시스템은 게임 패드를 추가 하거나 제거할 때 사용자에 게 알리는 연결 된 gamepads 및 이벤트의 목록을 제공 합니다.

### <a name="the-gamepads-list"></a>Gamepads 목록

[게임 패드][] 클래스는 현재 연결 된 Gamepads의 읽기 전용 목록 인 정적 속성인 [Gamepads][]를 제공 합니다. 연결 된 gamepads 중 일부에만 관심이 있을 수 있으므로 속성을 통해 액세스 하는 대신 고유한 컬렉션을 유지 관리 하는 것이 좋습니다 `Gamepads` .

다음 예에서는 모든 연결 된 gamepads를 새 컬렉션에 복사 합니다. 배경의 다른 스레드가이 컬렉션에 액세스 하 게 되므로 ( [GamepadAdded][] 및 [GamepadRemoved][] 이벤트에서) 컬렉션을 읽거나 업데이트 하는 코드에 대 한 잠금을 만들어야 합니다.

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

### <a name="adding-and-removing-gamepads"></a>Gamepads 추가 및 제거

게임 패드를 추가 하거나 제거 하면 [GamepadAdded][] 및 [GamepadRemoved][] 이벤트가 발생 합니다. 이러한 이벤트에 대 한 처리기를 등록 하 여 현재 연결 된 gamepads를 추적할 수 있습니다.

다음 예제에서는 추가 된 게임 패드를 추적 하기 시작 합니다.

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

다음 예제에서는 제거 된 게임 패드 추적을 중지 합니다. 또한 제거 될 때 추적 하는 gamepads에 발생 하는 상황을 처리 해야 합니다. 예를 들어이 코드는 한 게임 패드의 입력만 추적 하 고, `nullptr` 제거 될 때로 설정 하기만 하면 됩니다. 게임 프로그램이 활성 상태 이면 모든 프레임을 확인 하 고, 컨트롤러가 연결 되 고 연결이 끊어질 때 입력을 수집 하는 게임 패드를 업데이트 해야 합니다.

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

자세한 내용은 [게임의 입력 방법](input-practices-for-games.md) 을 참조 하세요.

### <a name="users-and-headsets"></a>사용자 및 헤드셋

각 게임 패드는 사용자 계정에 연결 하 여 자신의 id를 게임에 연결 하 고, 음성 채팅 또는 게임 내 기능을 용이 하 게 하는 헤드셋을 연결할 수 있습니다. 사용자 및 헤드셋 사용에 대 한 자세한 내용은 [사용자 및 장치](input-practices-for-games.md#tracking-users-and-their-devices) 및 [헤드셋](headset.md)추적을 참조 하세요.

## <a name="reading-the-gamepad"></a>게임 패드 읽기

관심이 있는 게임 패드를 식별 하면 여기에서 입력을 수집할 준비가 된 것입니다. 그러나 사용할 수 있는 다른 종류의 입력과 달리 gamepads는 이벤트를 발생 시켜 상태 변경을 전달 하지 않습니다. 대신, 현재 상태를 _폴링하여_ 정기적으로 사용 합니다.

### <a name="polling-the-gamepad"></a>게임 패드 폴링

폴링은 정확한 시점에 탐색 장치의 스냅숏을 캡처합니다. 입력 수집에 대 한이 방법은 일반적으로 이벤트 기반이 아니라 결정적 루프에서 실행 되기 때문에 대부분의 게임에 적합 합니다. 또한 시간이 지남에 따라 수집 된 여러 개의 단일 입력에서 가져온 것 보다 한 번에 모두 수집 된 입력에서 게임 명령을 해석 하는 것이 더 간단 합니다.

[GetCurrentReading][]를 호출 하 여 게임 패드를 폴링합니다. 이 함수는 게임 패드의 상태를 포함 하는 [GamepadReading][] 를 반환 합니다.

다음 예제에서는 현재 상태에 대 한 게임 패드를 폴링합니다.

```cpp
auto gamepad = myGamepads[0];

GamepadReading reading = gamepad->GetCurrentReading();
```

```cs
Gamepad gamepad = myGamepads[0];

GamepadReading reading = gamepad.GetCurrentReading();
```

게임 패드 상태 외에도 각 읽기는 상태가 검색 된 시기를 정확 하 게 나타내는 타임 스탬프를 포함 합니다. 타임 스탬프는 이전 판독값의 타이밍과 게임 시뮬레이션의 타이밍과 관련 된 경우에 유용 합니다.

### <a name="reading-the-thumbsticks"></a>Thumbsticks 읽기

각 엄지 스틱은 X 축과 Y 축에서-1.0에서 + 1.0 사이의 아날로그 읽기를 제공 합니다. X 축에서-1.0 값은 가장 왼쪽 엄지 스틱 위치에 해당 합니다. 값 + 1.0은 맨 오른쪽 위치에 해당 합니다. Y 축에서-1.0 값은 가장 작은 엄지 스틱 위치에 해당 합니다. + 1.0 값은 맨 위 위치에 해당 합니다. 두 축에서 값은 약 0.0이 가운데 위치에 있을 때 약 이지만, 이후 판독값에도 불구 하 고 정확한 값이 변경 되는 것은 일반적입니다. 이 변형 완화 전략에 대해서는이 섹션의 뒷부분에서 설명 합니다.

왼쪽 엄지 스틱 X 축의 값은 `LeftThumbstickX` [GamepadReading][] 구조의 속성에서 읽습니다. Y 축의 값은 속성에서 읽습니다 `LeftThumbstickY` . 오른쪽 엄지 스틱 X 축의 값은 속성에서 읽어서 `RightThumbstickX` Y 축의 값은 속성에서 읽습니다 `RightThumbstickY` .

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

엄지 스틱 값을 읽을 때 엄지 스틱을 가운데 위치에 놓을 때 0.0의 중립 읽기를 안정적으로 생성 하지 않는 것을 알 수 있습니다. 대신, 엄지 스틱을 이동 하 여 가운데 위치로 반환할 때마다 0.0 근처에 서로 다른 값을 생성 합니다. 이러한 차이를 완화 하기 위해 무시 되는 이상적인 센터 위치 근처에 있는 값의 범위에 해당 하는 작은 _deadzone_을 구현할 수 있습니다. Deadzone을 구현 하는 한 가지 방법은 중앙에서 엄지 스틱이 이동한 거리를 확인 하 고 선택한 거리 보다 가까운 판독값을 무시 하는 것입니다. &mdash;엄지 스틱 판독값은 &mdash; 숫자가 피타고라스 정리를 사용 하는 것만이 아니라 본질적으로는 평면이 아니라 극으로 계산 되기 때문에 정확 하지 않은 거리를 계산할 수 있습니다. 그러면 방사형 deadzone 생성 됩니다.

다음 예제에서는 숫자가 피타고라스 정리를 사용 하는 기본 방사형 deadzone를 보여 줍니다.

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

각 엄지 스틱을 안쪽으로 누를 때에도 단추 역할을 합니다. 이 입력을 읽는 방법에 대 한 자세한 내용은 [단추 읽기](#reading-the-buttons)를 참조 하십시오.

### <a name="reading-the-triggers"></a>트리거 읽기

트리거는 0.0 (완전히 릴리스된) 및 1.0 (완전히 눌린) 사이의 부동 소수점 값으로 표현 됩니다. 왼쪽 트리거의 값은 `LeftTrigger` [GamepadReading][] 구조의 속성에서 읽습니다. 오른쪽 트리거의 값은 속성에서 읽습니다 `RightTrigger` .

```cpp
float leftTrigger  = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
float rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

```cs
double leftTrigger = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
double rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

### <a name="reading-the-buttons"></a>단추 읽기

각 게임 패드 단추에는 &mdash; D 패드, 왼쪽 및 오른쪽 범퍼, 왼쪽 및 오른쪽 엄지 스틱 누르기, **A**, **B**, **X**, **Y**, **보기**및 메뉴의 4 가지 방향이 **Menu** &mdash; 있습니다. 효율성을 위해 단추 판독값은 개별 부울 값으로 표시 되지 않습니다. 대신 [GamepadButtons][] 열거형에 의해 표현 되는 단일 비트 필드에 모두 압축 됩니다.

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **paddle** buttons on its underside. These buttons are also represented in the `GamepadButtons` enumeration and their values are read in the same way as the standard gamepad buttons. -->

`Buttons` [GamepadReading][] 구조체의 속성에서 단추 값을 읽습니다. 이 속성은 비트 필드 이므로 관심이 있는 단추의 값을 격리 하기 위해 비트 마스킹을 사용 됩니다. 해당 비트가 설정 된 경우 단추가 눌러져 있습니다 (down). 그렇지 않으면 해제 됩니다 (up).

다음 예에서는 단추를 눌렀는지 여부를 확인 합니다.

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

다음 예에서는 A 단추가 해제 되어 있는지 여부를 확인 합니다.

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

경우에 따라 단추를 눌렀다 놓은 상태에서 눌린 상태로 전환 하는 경우, 여러 단추를 눌렀는지 또는 눌렀다 놓았을 때 또는 단추 집합이 특정 방법으로 몇 번의 특정 방식으로 정렬 되는지 여부를 결정할 수 있습니다 &mdash; . 이러한 각 조건을 검색 하는 방법에 대 한 자세한 내용은 [단추 전환 검색](input-practices-for-games.md#detecting-button-transitions) 및 [복잡 한 단추 배열](input-practices-for-games.md#detecting-complex-button-arrangements)검색을 참조 하세요.

## <a name="run-the-gamepad-input-sample"></a>게임 패드 입력 샘플 실행

[GamepadUWP 샘플 _(github)_ ](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadUWP) 은 게임 패드에 연결 하 고 해당 상태를 읽는 방법을 보여 줍니다.

## <a name="vibration-and-impulse-triggers-overview"></a>진동 및 임펄스 트리거 개요

게임 패드 내 진동 모터는 사용자에 게 tactile 피드백을 제공 하기 위한 것입니다. 게임에서는이 기능을 사용 하 여 상태 정보 (예: 손상 발생)를 전달 하 고, 중요 한 개체에 근접 한 신호를 보내거나, 다른 독창적인 사용을 위해이 기능을 사용 합니다.

Xbox One gamepads에는 총 4 개의 독립 진동 모터가 장착 되어 있습니다. 2 개는 게임 패드 본문에 위치한 매우 클 수 있는 모터입니다. 왼쪽 모터는 최고 진폭 진폭 효과를 제공 하는 반면, 오른쪽 모터는 gentler 더 미묘한 진동을 제공 합니다. 다른 두 개는 각 트리거 내에 하나씩 작은 모터가 며 사용자의 트리거 손가락에 직접 진동의 급격 한 버스트를 제공 합니다. Xbox One 게임 패드의 이러한 고유한 기능은 해당 트리거를 _임펄스 트리거_라고 하는 이유입니다. 이러한 모터를 함께 오케스트레이션 하 여 광범위 한 tactile sensations를 생성할 수 있습니다.

## <a name="using-vibration-and-impulse"></a>진동 및 임펄스 사용

게임 패드 진동은 [게임 패드][] 클래스의 [진동][] 속성을 통해 제어 됩니다. `Vibration` 는 네 개의 부동 소수점 값으로 구성 된 [GamepadVibration][] 구조체의 인스턴스입니다. 각 값은 모터 중 하나의 강도를 나타냅니다.

속성의 멤버를 `Gamepad.Vibration` 직접 수정할 수 있지만, 개별 `GamepadVibration` 인스턴스를 원하는 값으로 초기화 한 다음 속성에 복사 하 여 `Gamepad.Vibration` 실제 모터 강도를 한 번에 변경 하는 것이 좋습니다.

다음 예제에서는 모터 강도를 한 번에 변경 하는 방법을 보여 줍니다.

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

왼쪽 및 오른쪽 진동 모터는 0.0 (진동 없음) 및 1.0 (가장 강한 진동) 사이의 부동 소수점 값을 사용 합니다. 왼쪽 모터의 강도는 GamepadVibration 구조의 속성에 의해 설정 됩니다. `LeftMotor` 오른쪽 모터의 강도는 속성으로 설정 됩니다 [GamepadVibration][] `RightMotor` .

다음 예제에서는 두 진동 모터의 강도를 설정 하 고 게임 패드 진동을 활성화 합니다.

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

이러한 두 모터가 동일 하지 않으므로 이러한 속성을 동일한 값으로 설정 하는 것은 다른 모터에서 동일한 진동을 생성 하지 않습니다. 모든 값의 경우 왼쪽 모터는 동일한 값에 대 한 오른쪽 모터 보다 낮은 빈도로 더 강력한 진동을 생성 하 고 &mdash; &mdash; 더 높은 빈도로 gentler 진동을 생성 합니다. 최 댓 값에도 왼쪽 모터가 오른쪽 화물의 높은 주파수를 생성할 수 없으며, 올바른 모터가 왼쪽 모터의 최고 병력을 생성할 수 없습니다. 그러나 엄격가 게임 패드 본문에 의해 연결 되어 있기 때문에, 모터가 다른 특성을 가지 며 다른 강도로 진동 수 있는 경우에도 vibrations는 완전히 독립적으로 발생 하지 않습니다. 이러한 배열을 통해 모터가 동일한 경우 보다 더 광범위 하 고 더 표현 되는 sensations을 생성할 수 있습니다.

### <a name="using-the-impulse-triggers"></a>임펄스 트리거 사용

각 임펄스 트리거 모터는 0.0 (진동 없음) 및 1.0 (가장 강한 진동) 사이의 부동 소수점 값을 사용 합니다. 왼쪽 트리거 모터의 강도는 GamepadVibration 구조의 속성으로 설정 됩니다. `LeftTrigger` 오른쪽 [GamepadVibration][] 트리거의 강도는 속성으로 설정 됩니다 `RightTrigger` .

다음 예제에서는 두 임펄스 트리거의 강도를 설정 하 고 활성화 합니다.

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

다른 메서드와 달리 트리거 내의 두 진동 모터가 동일 하므로 동일한 값에 대 한 두 모터에서 동일한 진동을 생성 합니다. 그러나 이러한 모터가 엄격 연결 되지 않으므로 플레이어는 vibrations를 독립적으로 경험 합니다. 이러한 배열을 통해 완전히 독립 된 sensations를 두 트리거로 동시에 전달할 수 있으며, 게임 패드 본문의 모터 보다 더 구체적인 정보를 전달 하는 데 도움이 됩니다.

## <a name="run-the-gamepad-vibration-sample"></a>게임 패드 진동 샘플 실행

[GamepadVibrationUWP 샘플 _(github)_ ](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadVibrationUWP) 은 다양 한 효과를 생성 하는 데 게임 패드 진동 및 임펄스 트리거를 사용 하는 방법을 보여 줍니다.

## <a name="see-also"></a>참고 항목

* [Windows.Gaming.Input.UINavigationController][]
* [IGameController.][]
* [게임용 입력 시스템](input-practices-for-games.md)

[Windows. 게임 입력]: /uwp/api/Windows.Gaming.Input
[Windows.Gaming.Input.UINavigationController]: /uwp/api/Windows.Gaming.Input.UINavigationController
[IGameController.]: /uwp/api/Windows.Gaming.Input.IGameController
[gamepad]: /uwp/api/Windows.Gaming.Input.Gamepad
[gamepads]: /uwp/api/Windows.Gaming.Input.Gamepad
[gamepadadded]: /uwp/api/Windows.Gaming.Input.Gamepad
[gamepadremoved]: /uwp/api/Windows.Gaming.Input.Gamepad
[getcurrentreading]: /uwp/api/Windows.Gaming.Input.Gamepad
[vibration]: /uwp/api/Windows.Gaming.Input.Gamepad
[gamepadreading]: /uwp/api/Windows.Gaming.Input.GamepadReading
[gamepadbuttons]: /uwp/api/Windows.Gaming.Input.GamepadButtons
[gamepadvibration]: /uwp/api/Windows.Gaming.Input.GamepadVibration