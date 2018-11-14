---
author: eliotcowley
title: 플라이트 스틱
description: Windows.Gaming.Input 플라이트 스틱 API를 사용하여 스틱 플라이트에서 입력을 읽을 수 있습니다.
ms.assetid: DC633F6B-FDC9-4D6E-8401-305861F31192
ms.author: wdg-dev-content
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 입력, 플라이트 스틱
ms.localizationpriority: medium
ms.openlocfilehash: ebe7695b3f16271f3adedae658c0d62d38d7c078
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6200570"
---
# <a name="flight-stick"></a>플라이트 스틱

이 페이지에서는 UWP(유니버설 Windows 플랫폼)용 [Windows.Gaming.Input.FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) 및 관련 API를 사용하여 Xbox One 인증 플라이트 스틱용 프로그래밍의 기본 사항을 설명합니다.

이 페이지에서는 다음에 대해 알아봅니다.

* 연결된 플라이트 스틱 및 사용자 목록을 수집하는 방법
* 플라이트 스틱이 추가 또는 제거된 사실을 감지하는 방법
* 하나 이상의 플라이트 스틱에서 입력을 읽는 방법
* 플라이트 스틱이 UI 탐색 장치로서 작동하는 방법

## <a name="overview"></a>개요

플라이트 스틱은 평면 또는 우주선의 조종석에서 찾을 수 있는 플라이트 스틱의 느낌을 재현하도록 값이 입력된 게임 입력입니다. 신속하고 정확한 플라이트 제어를 위한 완벽한 입력 디바이스입니다. 플라이트 스틱은 Windows 10 및 Xbox One UWP 앱에서 [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) 네임스페이스를 통해 지원됩니다.

Xbox One 플라이트 스틱은 다음과 같은 컨트롤이 장착되어 있습니다.

* 롤, 피치 및 요가 가능한 비틀 수 있는 아날로그 조이스틱
* 아날로그 스로틀
* 두 개의 불 단추
* 8방향 모자 스위치
* **보기** 및 **메뉴** 단추

> [!NOTE]
> **보기** 및 **메뉴** 단추는 게임 플레이 명령이 아닌 UI 탐색을 지원하는 데 사용되므로 조이스틱 단추로 액세스하도록 준비되어 있지 않습니다.

### <a name="ui-navigation"></a>UI 탐색

사용자 인터페이스 탐색을 위해 다양한 입력 장치를 지원해야 하는 부담을 덜고 게임과 장치 간 일관성을 추구하기 위해 대부분의  _물리적_ 입력 장치는 [UI 탐색 컨트롤러](ui-navigation-controller.md)라는 별도의 _논리적_ 입력 장치 역할을 동시에 수행합니다. UI 탐색 컨트롤러는 여러 입력 장치의 UI 탐색 명령에 대한 공통 용어 모음집을 제공합니다.

UI 탐색 컨트롤러로 플라이트 스틱은 탐색 명령의 [필수 집합](ui-navigation-controller.md#required-set) 조이스틱과 **보기**, **메뉴**, **FirePrimary**, 및 **FireSecondary** 단추에 매핑합니다.

| 탐색 명령 | 플라이트 스틱 입력                  |
| ------------------:| ----------------------------------- |
|                 위 | 조이스틱 위                         |
|               아래 | 조이스틱 아래                       |
|               왼쪽 | 조이스틱 왼쪽                       |
|              오른쪽 | 조이스틱 오른쪽                      |
|               보기 | **보기** 버튼                     |
|               메뉴 | **메뉴** 버튼                     |
|             수락 | **FirePrimary** 단추              |
|             취소 | **FireSecondary** 단추            |

플라이트 스틱은 탐색 명령의 [선택 집합](ui-navigation-controller.md#optional-set)을 매핑하지 않습니다.

## <a name="detect-and-track-flight-sticks"></a>플라이트 스틱 검색 및 추적

플라이트 스틱의 감지 및 추적은 [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad) 클래스 대신 [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) 클래스를 제외하고 게임 패드에서와 똑같은 방식으로 작동합니다. 자세한 내용은 [게임 패드 및 진동](gamepad-and-vibration.md)을 참조하세요.

<!-- Flight sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected flight sticks and events to notify you when a flight stick is added or removed.

### The flight stick list

The [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) class provides a static property, [FlightSticks](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightSticks), which is a read-only list of flight sticks that are currently connected. Because you might only be interested in some of the connected flight sticks, we recommend that you maintain your own collection instead of accessing them through the `FlightSticks` property.

The following example copies all connected flight sticks into a new collection:

```cpp
auto myFlightSticks = ref new Vector<FlightStick^>();

for (auto flightStick : FlightStick::FlightSticks)
{
    // This code assumes that you're interested in all flight sticks.
    myFlightSticks->Append(flightStick);
}
```

### Adding and removing flight sticks

When a flight stick is added or removed, the [FlightStickAdded](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickAdded) and [FlightStickRemoved](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickRemoved) events are raised. You can register handlers for these events to keep track of the flight sticks that are currently connected.

The following example starts tracking a flight stick that's been added:

```cpp
FlightStick::FlightStickAdded += 
    ref new EventHandler<FlightStick^>([] (Platform::Object^, FlightStick^ args)
{
    // This code assumes that you're interested in all new flight sticks.
    myFlightSticks->Append(args);
});
```

The following example stops tracking a flight stick that's been removed:

```cpp
FlightStick::FlightStickRemoved += 
    ref new EventHandler<FlightStick^>([] (Platform::Object^, FlightStick^ args)
{
    unsigned int indexRemoved;

    if (myFlightSticks->IndexOf(args, &indexRemoved))
    {
        myFlightSticks->RemoveAt(indexRemoved);
    }
});
```

### Users and headsets

Each flight stick can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-flight-stick"></a>플라이트 스틱 읽기

관심 있는 플라이트 스틱을 식별하면 장치의 입력을 수집할 준비가 된 것입니다. 하지만 기존에 사용하던 다른 입력 유형과 달리 플라이트 스틱은 이벤트 발생을 통해 상태 변경을 알리지 않습니다. 대신, 플라이트 스틱을 직접 _폴링_하여 현재 상태의 규칙적인 판독값을 가져올 수 있습니다.

### <a name="polling-the-flight-stick"></a>플라이트 스틱 폴링

폴링은 정확한 시점에 플라이트 스틱의 스냅숏을 캡처합니다. 이 수집 입력의 논리는 일반적으로 이벤트 기반이 아니라 명확한 루프에서 실행되기 때문에 게임 대부분에 가장 잘 맞는 방법입니다. 일반적으로 상당한 시간 동안 수집된 여러 단일 입력보다 한 번에 수집된 입력에서 게임 명령을 해석하는 것이 더욱 간단한 방법이기도 합니다.

[FlightStick.GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick.GetCurrentReading)을 호출하여 플라이트 스틱을 폴링합니다. 이 기능은 플라이트 스틱의 상태를 포함하는 [FlightStickReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading) 반환합니다.

다음 예제에서는 플라이트 스틱의 현재 상태를 폴링합니다.

```cpp
auto flightStick = myFlightSticks->GetAt(0);
FlightStickReading reading = flightStick->GetCurrentReading();
```

플라이트 스틱 상태 외에도 각 판독값에는 상태가 검색된 시기를 정확히 나타내는 타임스탬프가 포함되어 있습니다. 타임스탬프는 이전 판독값의 타이밍 또는 게임 시뮬레이션의 타이밍을 이해하는 데 유용합니다.

### <a name="reading-the-joystick-and-throttle-input"></a>조이스틱 및 스로틀 입력 읽기

조이스틱은 X, Y, Z축(각각 롤, 피치, 요)에서 -1.0과 1.0 사이의 아날로그 읽기를 제공합니다. 롤에서 -1.0 값은 맨 왼쪽 조이스틱 위치에 해당하고, 1.0 값은 맨 오른쪽 위치에 해당합니다. 피치에서 -1.0 값은 맨 아래쪽 조이스틱 위치에 해당하고, 1.0 값은 맨 위쪽 위치에 해당합니다. 요의 경우 -1.0 값은 가장 반시계 방향인 뒤틀린 위치에 해당하며 1.0 값은 가장 시계 방향인 위치에 해당합니다.

모든 축에서 조이스틱이 중앙 위치에 있을 때 값은 약 0.0이지만 실제 값은 후속 판독값마다 달라질 수 있습니다. 이러한 변화를 완화하는 전략을 이 섹션의 뒷부분에서 설명합니다.

조이스틱 롤의 값은 [FlightStickReading.Roll](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Roll) 속성에서, 피치 값은 [FlightStickReading.Pitch](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Pitch) 속성에서, 요 값은 [FlightStickReading.Yaw](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Yaw) 속성에서 읽습니다.

```cpp
// Each variable will contain a value between -1.0 and 1.0.
float roll = reading.Roll;
float pitch = reading.Pitch;
float yaw = reading.Yaw;
```

조이스틱 값을 읽을 때 조이스틱이 중앙 위치에 있으면 중립 판독값 0.0이 안정적으로 생성되지 않지만 조이스틱이 이동하여 중앙 위치로 돌아갈 때마다 0.0에 가까운 다른 값이 생성된다는 사실을 알 수 있습니다. 이러한 변동을 완화하기 위해 무시되는 이상적 중앙 위치 근처의 값 범위인 _데드존_을 작게 구현할 수 있습니다.

데드존을 구현하는 한 가지 방법은 조이스틱이 중앙에서 얼마나 이동했는지 확인하고 선택한 거리보다 더 가까운 판독값을 무시하는 것입니다. 이 거리는 피타고라스의 정리만 사용해서 대략적으로 계산할 수 있습니다. 조이스틱 판독값은 기본적으로 평면이 아닌 양극 값이므로 정확하지 않습니다. 그러면 방사형 데드존이 생성됩니다.

다음 예제는 피타고라스의 원리를 사용하는 기본 방사형 데드존을 보여 줍니다.

```cpp
// Choose a deadzone. Readings inside this radius are ignored.
const float deadzoneRadius = 0.1f;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem: For a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
float oppositeSquared = pitch * pitch;
float adjacentSquared = roll * roll;

// Accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) < deadzoneSquared)
{
    // Input accepted, process it.
}
```

### <a name="reading-the-buttons-and-hat-switch"></a>단추와 모자 스위치 읽기

각 플라이트 스틱의 두 불 단추는 누름(아래) 또는 놓음(위) 여부를 나타내는 디지털 읽기를 제공합니다. 버튼 판독값은 효율을 위해 개별 부울 값으로 표현되지 않는 대신 [FlightStickButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickbuttons) 열거로 표현되는 단일 비트 필드로 모두 압축됩니다. 또한 8방향 모자 스위치는 [GameControllerSwitchPosition](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerswitchposition) 열거로 표시되는 단일 비트 필드로 패키지된 방향을 제공합니다.

> [!NOTE]
> 플라이트 스틱에는 UI 탐색에 사용되는 **보기** 및 **메뉴** 버튼과 같은 추가 버튼이 탑재되어 있습니다. 이러한 버튼은 `FlightStickButtons` 열거에 포함되지 않으며 UI 탐색 장치 역할을 하는 플라이트 스틱에 액세스해야만 읽을 수 있습니다. 자세한 내용은 [UI 탐색 컨트롤러](ui-navigation-controller.md)를 참조하세요.

단추 값은 [FlightStickReading.Buttons](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Buttons) 속성에서 읽습니다. 이러한 속성은 비트 필드이므로 해당 버튼의 값을 격리하기 위해 비트 마스킹이 사용됩니다. 해당 비트가 설정된 경우에는 버튼이 눌리고(아래), 그렇지 않은 경우에는 놓입니다(위).

다음 예제에서는 **FirePrimary** 버튼이 눌렸는지 확인합니다.

```cpp
if (FlightStickButtons::FirePrimary == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is pressed.
}
```

다음 예제에서는 **FirePrimary** 버튼이 놓였는지 확인합니다.

```cpp
if (FlightStickButtons::None == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is released (not pressed).
}
```

버튼이 눌렸다가 놓이거나, 반대로 놓였다가 눌렸을 때 여러 개의 버튼이 눌렸거나 놓였는지 또는 일련의 버튼이 특정 방식으로 정렬되었는지(일부는 눌리고 일부는 놓임) 확인해야 하는 경우가 있습니다. 이러한 각 상태를 검색하는 방법에 대한 자세한 내용은 [버튼 전환 검색](input-practices-for-games.md#detecting-button-transitions) 및 [복잡한 버튼 정렬 검색](input-practices-for-games.md#detecting-complex-button-arrangements)을 참조하세요.

모자 스위치 값은 [FlightStickReading.HatSwitch](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.HatSwitch) 속성에서 읽습니다. 이 속성은 비트 필드이기 때문에 모자 스위치의 위치를 격리하는 데 비트 마스킹이 다시 사용됩니다.

다음 예제는 모자 스위치가 위 위치에 있는지 확인합니다.

```cpp
if (GameControllerSwitchPosition::Up == (reading.HatSwitch & GameControllerSwitchPosition::Up))
{
    // The hat switch is in the up position.
}
```

다음 예제는 모자 스위치가 중앙 위치에 있는지 확인합니다.

```cpp
if (GameControllerSwitchPosition::Center == (reading.HatSwitch & GameControllerSwitchPosition::Center))
{
    // The hat switch is in the center position.
}
```

<!--## Run the InputInterfacingUWP sample

The [InputInterfacingUWP sample _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) demonstrates how to use flight sticks and different kinds of input devices in tandem, as well as how these input devices behave as UI navigation controllers.-->

## <a name="see-also"></a>참고 항목

* [Windows.Gaming.Input.UINavigationController 클래스](https://docs.microsoft.com/uwp/api/windows.gaming.input.uinavigationcontroller)
* [Windows.Gaming.Input.IGameController 인터페이스](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [게임용 입력 시스템](input-practices-for-games.md)