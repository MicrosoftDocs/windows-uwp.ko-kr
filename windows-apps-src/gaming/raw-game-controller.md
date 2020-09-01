---
title: 원시 게임 컨트롤러
description: 거의 모든 유형의 게임 컨트롤러에서 입력을 읽으려면 game. raw game controller Api를 사용 합니다.
ms.assetid: 2A466C16-1F51-4D8D-AD13-704B6D3C7BEC
ms.date: 03/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 입력, raw 게임 컨트롤러
ms.localizationpriority: medium
ms.openlocfilehash: d21b411965da874cfb324fc2ee867e39bdcd0ded
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171987"
---
# <a name="raw-game-controller"></a>원시 게임 컨트롤러

이 페이지에서는 [RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller) 를 사용 하는 거의 모든 유형의 게임 컨트롤러 프로그래밍에 대 한 기본 사항 및 UWP (유니버설 Windows 플랫폼) 관련 api에 대해 설명 합니다.

이 페이지를 읽으면 다음에 대해 알아봅니다.

* 연결 된 원시 게임 컨트롤러 및 해당 사용자 목록을 수집 하는 방법
* 원시 게임 컨트롤러가 추가 또는 제거 되었는지 검색 하는 방법
* 원시 게임 컨트롤러의 기능을 가져오는 방법
* 원시 게임 컨트롤러에서 입력을 읽는 방법

## <a name="overview"></a>개요

원시 게임 컨트롤러는 다양 한 종류의 일반적인 게임 컨트롤러에서 제공 되는 입력을 사용 하는 게임 컨트롤러를 일반적으로 표현한 것입니다. 이러한 입력은 명명 되지 않은 단추, 스위치 및 축의 간단한 배열로 표시 됩니다. 원시 게임 컨트롤러를 사용 하 여 고객이 사용 중인 컨트롤러의 유형에 관계 없이 사용자 지정 입력 매핑을 만들 수 있습니다.

[RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller) 클래스는 실제로 다른 입력 클래스 ([ArcadeStick](/uwp/api/windows.gaming.input.arcadestick), FlightStick 등)가 요구 사항을 충족 하지 않는 경우에는 실제로 다른 입력 클래스 (, [FlightStick](/uwp/api/windows.gaming.input.flightstick)등)가 요구 사항을 충족 하지 않는 경우에만 &mdash; 사용 됩니다.

## <a name="detect-and-track-raw-game-controllers"></a>원시 게임 컨트롤러 검색 및 추적

[RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller) 클래스 대신 gamepads를 사용 [하는 경우](/uwp/api/Windows.Gaming.Input.Gamepad) 를 제외 하 고 raw 게임 컨트롤러를 검색 하 고 추적 하면 작동 하는 것과 정확히 동일한 방식으로 작동 합니다. 자세한 내용은 [게임 패드 및 진동](gamepad-and-vibration.md) (영문)을 참조 하세요.

<!-- Raw game controllers are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected raw game controllers and events to notify you when a raw game controller is added or removed.

### The raw game controller list

The [RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller) class provides a static property, [RawGameControllers](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllers), which is a read-only list of raw game controllers that are currently connected. Because you might only be interested in some of the connected raw game controllers, we recommend that you maintain your own collection instead of accessing them through the **RawGameControllers** property.

The following example copies all connected raw game controllers into a new collection:

```cpp
auto myRawGameControllers = ref new Vector<RawGameController^>();

for (auto rawGameController : RawGameController::RawGameControllers)
{
    // This code assumes that you're interested in all raw game controllers.
    myRawGameControllers->Append(rawGameController);
}
```

### Adding and removing raw game controllers

When a raw game controller is added or removed, the [RawGameControllerAdded](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerAdded) and [RawGameControllerRemoved](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerRemoved) events are raised. You can register handlers for these events to keep track of the raw game controllers that are currently connected.

The following example starts tracking a raw game controller that's been added:

```cpp
RawGameController::RawGameControllerAdded += 
    ref new EventHandler<RawGameController^>(
        [] (Platform::Object^, RawGameController^ args)
{
    // This code assumes that you're interested in all new raw game controllers.
    myRawGameControllers->Append(args);
});
```

The following example stops tracking a raw game controller that's been removed:

```cpp
RawGameController::RawGameControllerRemoved +=
    ref new EventHandler<RawGameController^>(
        [] (Platform::Object^, RawGameController^ args)
{
    unsigned int indexRemoved;

    if (myRawGameControllers->IndexOf(args, &indexRemoved))
    {
        myRawGameControllers->RemoveAt(indexRemoved);
    }
});
```

### Users and headsets

Each raw game controller can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="get-the-capabilities-of-a-raw-game-controller"></a>원시 게임 컨트롤러의 기능 가져오기

관심이 있는 원시 게임 컨트롤러를 확인 한 후에는 컨트롤러의 기능에 대 한 정보를 수집할 수 있습니다. [RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller.ButtonCount)를 사용 하 여 원시 게임 컨트롤러의 단추 수, [AxisCount](/uwp/api/windows.gaming.input.rawgamecontroller.AxisCount)를 사용 하는 아날로그 축 수 및 [RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller.SwitchCount)를 사용 하는 스위치 수를 가져올 수 있습니다. 또한 [RawGameController. GetSwitchKind](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_)를 사용 하 여 스위치의 형식을 가져올 수 있습니다.

다음 예에서는 원시 게임 컨트롤러의 입력 횟수를 가져옵니다.

```cpp
auto rawGameController = myRawGameControllers->GetAt(0);
int buttonCount = rawGameController->ButtonCount;
int axisCount = rawGameController->AxisCount;
int switchCount = rawGameController->SwitchCount;
```

다음 예에서는 각 스위치의 유형을 결정 합니다.

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    GameControllerSwitchKind mySwitch = rawGameController->GetSwitchKind(i);
}
```

## <a name="reading-the-raw-game-controller"></a>원시 게임 컨트롤러 읽기

원시 게임 컨트롤러의 입력 수를 확인 한 후에는 입력을 수집할 준비가 된 것입니다. 그러나 사용할 수 있는 다른 종류의 입력과 달리 원시 게임 컨트롤러는 이벤트를 발생 시켜 상태 변경을 전달 하지 않습니다. 대신 _폴링을_ 통해 현재 상태의 일반적인 판독값을 가져옵니다.

### <a name="polling-the-raw-game-controller"></a>원시 게임 컨트롤러 폴링

폴링은 정확한 시점에 원시 게임 컨트롤러의 스냅숏을 캡처합니다. 입력 수집에 대 한이 방법은 일반적으로 이벤트가 구동 되는 것이 아니라 결정적 루프에서 실행 되기 때문에 대부분의 게임에 적합 합니다. 또한 시간이 지남에 따라 수집 된 여러 개의 단일 입력에서 가져온 것 보다 한 번에 모두 수집 된 입력에서 게임 명령을 해석 하는 것이 더 간단 합니다.

[RawGameController. GetCurrentReading](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetCurrentReading_System_Boolean___Windows_Gaming_Input_GameControllerSwitchPosition___System_Double___)를 호출 하 여 원시 게임 컨트롤러를 폴링합니다. 이 함수는 원시 게임 컨트롤러의 상태를 포함 하는 단추, 스위치 및 축에 대 한 배열을 채웁니다.

다음 예제에서는 현재 상태에 대해 raw game controller를 폴링합니다.

```cpp
Platform::Array<bool>^ currentButtonReading =
    ref new Platform::Array<bool>(buttonCount);

Platform::Array<GameControllerSwitchPosition>^ currentSwitchReading =
    ref new Platform::Array<GameControllerSwitchPosition>(switchCount);

Platform::Array<double>^ currentAxisReading = ref new Platform::Array<double>(axisCount);

rawGameController->GetCurrentReading(
    currentButtonReading,
    currentSwitchReading,
    currentAxisReading);
```

각 배열의 어떤 위치에서 다양 한 유형의 컨트롤러 간에 입력 값을 보유할 수 있는 것은 아닙니다. 따라서 [RawGameController. GetButtonLabel](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) 및 [RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_)메서드를 사용 하는 입력을 확인 해야 합니다.

**Getbuttonlabel** 은 단추의 기능이 아니라 실제 단추에 인쇄 되는 텍스트 또는 기호를 알려 줍니다 &mdash; . 이러한 기능을 수행 하는 단추에 대 한 플레이어 힌트를 제공 하려는 경우에는 UI를 사용 하는 것이 가장 좋습니다. **Getswitchkind** 는 스위치의 유형 (즉, 포함 된 위치 수)을 알려 주지만 이름이 아닙니다.

축 또는 스위치의 레이블을 가져오는 표준화 된 방법이 없으므로 직접 테스트 하 여 입력을 확인 해야 합니다.

지원 하려는 특정 컨트롤러가 있는 경우 [RawGameController HardwareProductId](/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId) 및 [RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) 를 가져오고 해당 컨트롤러와 일치 하는지 확인할 수 있습니다. 각 배열의 각 입력 위치는 동일한 **HardwareProductId** 및 **HardwareVendorId**가 있는 모든 컨트롤러에 대해 동일 하므로 동일한 유형의 여러 컨트롤러 간에 일관 되지 않을 수 있는 논리에 대해 걱정할 필요가 없습니다.

Raw game controller 상태 외에도 각 읽기는 상태가 검색 된 시기를 정확 하 게 나타내는 타임 스탬프를 반환 합니다. 타임 스탬프는 이전 판독값의 타이밍과 게임 시뮬레이션의 타이밍과 관련 된 경우에 유용 합니다.

### <a name="reading-the-buttons-and-switches"></a>단추 및 스위치 읽기

각 원시 게임 컨트롤러의 단추는 프레스 (다운) 또는 릴리스 (up) 여부를 나타내는 디지털 읽기를 제공 합니다. 단추 판독값은 단일 배열에서 개별 부울 값으로 표현 됩니다. 배열의 부울 값 인덱스와 함께 [RawGameController 레이블을](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) 사용 하 여 각 단추의 레이블을 찾을 수 있습니다. 각 값은 [GameControllerButtonLabel](/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel)로 표시 됩니다.

다음 예제에서는 **Xboxa** 단추를 눌렀는지 여부를 확인 합니다.

```cpp
for (uint32_t i = 0; i < buttonCount; i++)
{
    if (currentButtonReading[i])
    {
        GameControllerButtonLabel buttonLabel = rawGameController->GetButtonLabel(i);

        if (buttonLabel == GameControllerButtonLabel::XboxA)
        {
            // XboxA is pressed.
        }
    }
}
```

경우에 따라 단추를 눌렀다 놓은 상태에서 눌린 상태로 전환 하는 경우, 여러 단추를 눌렀는지 또는 눌렀다 놓았을 때 또는 단추 집합이 특정 방법으로 몇 번의 단추에 배치 되는지 여부를 확인 하는 것이 좋습니다 &mdash; . 이러한 각 조건을 검색 하는 방법에 대 한 자세한 내용은 [단추 전환 검색](input-practices-for-games.md#detecting-button-transitions) 및 [복잡 한 단추 배열](input-practices-for-games.md#detecting-complex-button-arrangements)검색을 참조 하세요.

스위치 값은 [GameControllerSwitchPosition](/uwp/api/windows.gaming.input.gamecontrollerswitchposition)배열로 제공 됩니다. 이 속성은 비트 필드 이므로 스위치 방향을 격리 하는 데 비트 마스킹을 사용 됩니다.

다음 예에서는 각 스위치가 위쪽 위치에 있는지 여부를 확인 합니다.

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    if (GameControllerSwitchPosition::Up ==
        (currentSwitchReading[i] & GameControllerSwitchPosition::Up))
    {
        // The switch is in the up position.
    }
}
```

### <a name="reading-the-analog-inputs-sticks-triggers-throttles-and-so-on"></a>아날로그 입력 읽기 (스틱, 트리거, 제한 등)

아날로그 축은 0.0과 1.0 사이의 읽기를 제공 합니다. 여기에는 표준 스틱의 경우 X, Y와 같은 스틱의 각 차원과 비행 스틱에 대해 X, Y 및 Z 축 (각각 롤오버, 피치 및 요)이 포함 됩니다.

값은 아날로그 트리거, 제한 또는 기타 형식의 아날로그 입력을 나타낼 수 있습니다. 이러한 값은 레이블과 함께 제공 되지 않으므로 코드를 다양 한 입력 장치로 테스트 하 여 가정과 일치 하는지 확인 하는 것이 좋습니다.

모든 축에서 값은 중심 위치에 있는 경우 약 0.5이 고, 이후 판독값에도 불구 하 고 정확한 값이 변경 되는 것은 일반적입니다. 이 변형 완화 전략에 대해서는이 섹션의 뒷부분에서 설명 합니다.

다음 예제에서는 Xbox 컨트롤러에서 아날로그 값을 읽는 방법을 보여 줍니다.

```cpp
// Xbox controllers have 6 axes: 2 for each stick and one for each trigger.
float leftStickX = currentAxisReading[0];
float leftStickY = currentAxisReading[1];
float rightStickX = currentAxisReading[2];
float rightStickY = currentAxisReading[3];
float leftTrigger = currentAxisReading[4];
float rightTrigger = currentAxisReading[5];
```

스틱 값을 읽을 때 가운데 위치에 있는 경우 0.5의 중립 읽기가 안정적으로 생성 되지 않는다는 것을 알 수 있습니다. 대신, 스틱을 이동 하 여 가운데 위치로 반환할 때마다 0.5 근처에 서로 다른 값을 생성 합니다. 이러한 차이를 완화 하기 위해 무시 되는 이상적인 센터 위치 근처에 있는 값의 범위에 해당 하는 작은 _deadzone_을 구현할 수 있습니다.

Deadzone을 구현 하는 한 가지 방법은 스틱이 중심에서 이동 하는 정도를 확인 하 고 선택한 거리 보다 가까운 판독값을 무시 하는 것입니다. &mdash;스틱 판독값은 기본적으로 &mdash; 숫자가 피타고라스 정리를 사용 하는 것과 같이 평면이 아니라 극으로 계산 되기 때문에 정확 하지 않은 거리를 계산할 수 있습니다. 그러면 방사형 deadzone 생성 됩니다.

다음 예제에서는 숫자가 피타고라스 정리를 사용 하는 기본 방사형 deadzone를 보여 줍니다.

```cpp
// Choose a deadzone. Readings inside this radius are ignored.
const float deadzoneRadius = 0.1f;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem: For a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
float oppositeSquared = leftStickY * leftStickY;
float adjacentSquared = leftStickX * leftStickX;

// Accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) < deadzoneSquared)
{
    // Input accepted, process it.
}
```

<!--## Run the RawGameControllerUWP sample

The [RawGameControllerUWP sample (GitHub)](TODO: Link) demonstrates how to use raw game controllers. TODO: More information-->

## <a name="see-also"></a>참고 항목

* [게임용 입력 시스템](input-for-games.md)
* [게임용 입력 시스템](input-practices-for-games.md)
* [Windows 게이밍. 입력 네임 스페이스](/uwp/api/windows.gaming.input)
* [RawGameController 클래스입니다.](/uwp/api/windows.gaming.input.rawgamecontroller)
* [IGameController 인터페이스입니다.](/uwp/api/windows.gaming.input.igamecontroller)
* [IGameControllerBatteryInfo 인터페이스입니다.](/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo)