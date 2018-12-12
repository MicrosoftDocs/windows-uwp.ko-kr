---
title: 원시 게임 컨트롤러
description: Windows.Gaming.Input 원시 게임 컨트롤러 API를 사용하여 거의 모든 유형의 게임 컨트롤러 입력을 읽을 수 있습니다.
ms.assetid: 2A466C16-1F51-4D8D-AD13-704B6D3C7BEC
ms.date: 03/08/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, 입력, 원시 게임 컨트롤러
ms.localizationpriority: medium
ms.openlocfilehash: 7b5f4d49ad49cf9f9065fe17788456e9dd2a4a4e
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8946799"
---
# <a name="raw-game-controller"></a>원시 게임 컨트롤러

이 페이지에서는 UWP(유니버설 Windows 플랫폼)용 [Windows.Gaming.Input.RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) 및 관련 API를 사용하여 거의 모든 유형의 게임 컨트롤러에 대한 프로그래밍의 기본 사항을 설명합니다.

이 페이지에서는 다음에 대해 알아봅니다.

* 연결된 원시 게임 컨트롤러 및 사용자 목록을 수집하는 방법
* 원시 게임 컨트롤러가 추가 또는 제거된 사실을 감지하는 방법
* 원시 게임 컨트롤러의 기능을 다운로드하는 방법
* 원시 게임 컨트롤러에서 입력을 읽는 방법

## <a name="overview"></a>개요

원시 게임 컨트롤러는 다양한 유형의 일반적인 게임 컨트롤러에서 발견된 입력으로 나타나는 게임 컨트롤러의 일반적인 표현입니다. 이러한 입력은 이름 없는 단추, 스위치 및 축의 단순한 배열로 표시됩니다. 원시 게임 컨트롤러를 사용하여 고객이 사용자는 컨트롤러 유형에 관계 없이 사용자 지정 입력 매핑을 만들도록 허용할 수 있습니다.

[RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) 클래스는 실제로 다른 클래스([ArcadeStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick), [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) 등)가 요구&mdash;를 충족하지 않는 경우를 위한 클래스입니다. 더 일반적인 항목을 원하는 경우 고객이 다양한 유형의 게임 컨트롤러를 사용할 것을 예측할 수 있으며 이 클래스가 적합합니다.

## <a name="detect-and-track-raw-game-controllers"></a>원시 게임 컨트롤러 검색 및 추적

원시 게임 컨트롤러의 감지 및 추적은 [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad) 클래스 대신[RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) 클래스를 제외하고 게임 패드에서와 똑같은 방식으로 작동합니다. 자세한 내용은 [게임 패드 및 진동](gamepad-and-vibration.md)을 참조하세요.

<!-- Raw game controllers are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected raw game controllers and events to notify you when a raw game controller is added or removed.

### The raw game controller list

The [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) class provides a static property, [RawGameControllers](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllers), which is a read-only list of raw game controllers that are currently connected. Because you might only be interested in some of the connected raw game controllers, we recommend that you maintain your own collection instead of accessing them through the **RawGameControllers** property.

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

When a raw game controller is added or removed, the [RawGameControllerAdded](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerAdded) and [RawGameControllerRemoved](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerRemoved) events are raised. You can register handlers for these events to keep track of the raw game controllers that are currently connected.

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

## <a name="get-the-capabilities-of-a-raw-game-controller"></a>원시 게임 컨트롤러의 기능 얻기

관심 있는 원시 게임 컨트롤러를 확인한 후 컨트롤러의 기능에 정보를 수집할 수 있습니다. [RawGameController.ButtonCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.ButtonCount)로 원시 게임 컨트롤러의 단추 수, [RawGameController.AxisCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.AxisCount)로 아날로그 축의 수, [RawGameController.SwitchCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.SwitchCount)로 스위치의 수를 얻을 수 있습니다. 또한 [RawGameController.GetSwitchKind](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_)를 사용하여 스위치 유형을 얻을 수 있습니다.

다음 예에서 원시 게임 컨트롤러의 입력 개수를 가져옵니다.

```cpp
auto rawGameController = myRawGameControllers->GetAt(0);
int buttonCount = rawGameController->ButtonCount;
int axisCount = rawGameController->AxisCount;
int switchCount = rawGameController->SwitchCount;
```

다음 예제는 각 스위치의 종류를 결정합니다.

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    GameControllerSwitchKind mySwitch = rawGameController->GetSwitchKind(i);
}
```

## <a name="reading-the-raw-game-controller"></a>원시 게임 컨트롤러 읽기

원시 게임 컨트롤러에서 입력 번호를 알면 입력을 수집할 준비가 된 것입니다. 하지만 기존에 사용하던 다른 입력 유형과 달리 원시 게임 컨트롤러는 이벤트 발생을 통해 상태 변경을 알리지 않습니다. 대신 이를 직접 _폴링_하여 현재 상태의 규칙적인 판독값을 가져올 수 있습니다.

### <a name="polling-the-raw-game-controller"></a>원시 게임 컨트롤러 폴링

폴링은 정확한 시점에 원시 게임 컨트롤러의 스냅숏을 캡처합니다. 이 수집 입력의 논리는 일반적으로 이벤트 기반이 아니라 명확한 루프에서 실행되기 때문에 게임 대부분에 가장 잘 맞는 방법입니다. 일반적으로 상당한 시간 동안 수집된 여러 단일 입력보다 한 번에 수집된 입력에서 게임 명령을 해석하는 것이 더욱 간단한 방법이기도 합니다.

[RawGameController.GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetCurrentReading_System_Boolean___Windows_Gaming_Input_GameControllerSwitchPosition___System_Double___)을 호출하여 원시 게임 컨트롤러를 폴링합니다. 이 기능은 의 상태를 포함하는 단추, 스위치를 및 축의 배열을 채웁니다.

다음 예제에서는 원시 게임 컨트롤러의 현재 상태를 폴링합니다.

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

다양한 유형의 컨트롤러 중 각 배열의 어떤 위치가 어떤 입력 값을 가지는지 보장하지 못하므로 [RawGameController.GetButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) 및 [RawGameController.GetSwitchKind](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_) 메서드를 사용하여 어떤 입력이 어떤 입력인지 확인해야 합니다.

**GetButtonLabel**은 단추의 기능이 아닌 실제 단추에 인쇄된 텍스트 또는 기호를 알려줍니다.&mdash; 따라서 이것은 플레이어에게 어떤 단추가 어떤 기능을 수행하는지에 대해 힌트를 제공하고자 하는 경우 UI에 대한 도움으로 사용하는 데 적합합니다. **GetSwitchKind**는 스위치의 종류(즉, 스위치의 위치 수)를 알려주지만 이름은 알려주지 않습니다.

축 또는 스위치의 레이블을 가져오는 표준화된 방법은 없으므로 어떤 입력이 어떤 것인지 확인하기 위해 직접 테스트해야 합니다.

지원하고자 하는 특정 컨트롤러가 있는 경우 [RawGameController.HardwareProductId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId) 및 [RawGameController.HardwareVendorId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId)를 가져와 컨트롤러와 일치하는지 확인합니다. 각 배열에서 각 입력의 위치는 동일한 **HardwareProductId** 및 **HardwareVendorId**를 가진 모든 컨트롤러에 대해 같으므로 같은 종류의 서로 다른 컨트롤러 간에 논리 불일치를 걱정할 필요가 없습니다.

원시 게임 컨트롤러 상태 외에도 각 판독값은 상태가 검색된 시점을 정확히 나타내는 타임스탬프를 반환합니다. 타임스탬프는 이전 판독값의 타이밍 또는 게임 시뮬레이션의 타이밍을 이해하는 데 유용합니다.

### <a name="reading-the-buttons-and-switches"></a>단추와 스위치 읽기

각 원시 게임 컨트롤러 단추는 누름(아래) 또는 놓음(위) 여부를 나타내는 디지털 읽기를 제공합니다. 단추 읽기는 단일 배열에서 개별 부울 값으로 표시됩니다. 각 단추의 레이블은 [RawGameController.GetButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_)과 배열의 부울 값 인덱스를 사용하여 찾을 수 있습니다. 각 값은 [GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel)로 표시됩니다.

다음 예제에서는 **XboxA** 버튼이 눌렸는지 확인합니다.

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

버튼이 눌렸다가 놓이거나, 반대로 놓였다가 눌렸을 때 여러 개의 버튼이 눌렸거나 놓였는지 또는 일련의 버튼이 특정 방식으로 정렬되었는지(일부는 눌리고 일부는 놓임) 확인해야 하는 경우가 있습니다. 이러한 각 상태를 검색하는 방법에 대한 자세한 내용은 [버튼 전환 검색](input-practices-for-games.md#detecting-button-transitions) 및 [복잡한 버튼 정렬 검색](input-practices-for-games.md#detecting-complex-button-arrangements)을 참조하세요.

스위치 값은 [GameControllerSwitchPosition](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerswitchposition)의 배열로 제공됩니다. 이 속성은 비트 필드이기 때문에 스위치를 파악하는 데 비트 마스킹이 사용됩니다.

다음 예제는 각 스위치가 위 위치에 있는지 확인합니다.

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

### <a name="reading-the-analog-inputs-sticks-triggers-throttles-and-so-on"></a>아날로그 입력 읽기(스틱, 트리거, 스로틀 등)

아날로그 축은 0.0과 1.0 사이의 읽기를 제공합니다. 이에는 표준 스틱에 대해 X 및 Y 또는 각 플라이트 스틱에 대해 X, Y, Z축까지(각각 롤, 피치, 요) 스틱의 각 차원을 포함합니다.

값은 아날로그 트리거, 스로틀, 또는 기타 모든 형식의 아날로그 입력을 나타낼 수 있습니다. 이 값은 레이블과 함께 제공되지 않으므로 가정과 정확하게 일치하는지 확인하기 위해 다양한 입력 디바이스에 코드를 테스트하는 것이 좋습니다.

모든 축에서 스틱이 중앙 위치에 있을 때 값은 약 0.5이지만 일반적으로 정밀 값은 후속 판독값마다 다릅니다. 이러한 변동을 완화하기 위한 전략은 이 섹션의 뒷부분에 설명되어 있습니다.

Xbox 컨트롤러에서 아날로그 값을 읽는 방법은 다음과 같습니다.

```cpp
// Xbox controllers have 6 axes: 2 for each stick and one for each trigger.
float leftStickX = currentAxisReading[0];
float leftStickY = currentAxisReading[1];
float rightStickX = currentAxisReading[2];
float rightStickY = currentAxisReading[3];
float leftTrigger = currentAxisReading[4];
float rightTrigger = currentAxisReading[5];
```

스틱 값을 읽을 때 스틱이 중앙 위치에 있으면 중립 판독값 0.5가 안정적으로 생성되지 않지만 스틱이 이동하여 중앙 위치로 돌아갈 때마다 0.5에 가까운 다른 값이 생성된다는 사실을 알 수 있습니다. 이러한 변동을 완화하기 위해 무시되는 이상적 중앙 위치 근처의 값 범위인 _데드존_을 작게 구현할 수 있습니다.

데드존을 구현하는 한 가지 방법은 스틱이 중앙에서 얼마나 이동했는지 확인하고 선택한 거리보다 더 가까운 판독값을 무시하는 것입니다. 이 거리는 피타고라스의 정리만 사용해서 대략적으로 계산할 수 있습니다. 스틱 판독값은 기본적으로 평면이 아닌 양극 값이므로 정확하지 않습니다. 그러면 방사형 데드존이 생성됩니다.

다음 예제는 피타고라스의 원리를 사용하는 기본 방사형 데드존을 보여 줍니다.

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

* [게임 입력 디바이스](input-for-games.md)
* [게임 입력 시스템](input-practices-for-games.md)
* [Windows.Gaming.Input 네임스페이스](https://docs.microsoft.com/uwp/api/windows.gaming.input)
* [Windows.Gaming.Input.RawGameController 클래스](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller)
* [Windows.Gaming.Input.IGameController 인터페이스](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Windows.Gaming.Input.IGameControllerBatteryInfo 인터페이스](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo)