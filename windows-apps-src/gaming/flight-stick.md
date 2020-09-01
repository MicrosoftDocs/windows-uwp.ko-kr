---
title: 플라이트 스틱
description: Windows 게이밍. 입력 비행 스틱 Api를 사용 하 여 비행 스틱에서 입력을 읽습니다.
ms.assetid: DC633F6B-FDC9-4D6E-8401-305861F31192
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 입력, 비행 스틱
ms.localizationpriority: medium
ms.openlocfilehash: b9b4353a2736feb7cdfde9871c29f61de52e0c9a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156427"
---
# <a name="flight-stick"></a>플라이트 스틱

이 페이지에서는 [FlightStick](/uwp/api/windows.gaming.input.flightstick) 를 사용 하는 Xbox one 인증 비행의 프로그래밍에 대 한 기본 사항 및 UWP (유니버설 Windows 플랫폼) 관련 api에 대해 설명 합니다.

이 페이지를 읽으면 다음에 대해 알아봅니다.

* 연결 된 비행 스틱 및 해당 사용자 목록을 수집 하는 방법
* 비행 스틱 추가 또는 제거를 검색 하는 방법
* 하나 이상의 비행 스틱에서 입력을 읽는 방법
* 비행 스틱이 UI 탐색 장치로 작동 하는 방식

## <a name="overview"></a>개요

비행 스틱은 비행기 또는 spaceship의 환경에서 찾을 수 있는 비행 스틱의 느낌을 재현 하기 위한 게임 입력 장치입니다. 이러한 장치는 항공편을 빠르고 정확 하 게 제어 하기 위한 완벽 한 입력 장치입니다. 비행 스틱 [은 windows 10](/uwp/api/windows.gaming.input) 및 XBOX one UWP 앱에서 지원 됩니다.

Xbox One 비행에는 다음 컨트롤이 장착 되어 있습니다.

* Roll, 피치 및 요를 지 원하는 twistable 아날로그 조이스틱
* 아날로그 제한
* 두 개의 화재 단추
* 8 방향 디지털 hat 스위치
* **보기** 및 **메뉴** 단추

> [!NOTE]
> **보기** 및 **메뉴** 단추는 게임 플레이 명령이 아니라 UI 탐색을 지 원하는 데 사용 되므로 조이스틱 단추로 쉽게 액세스할 수 없습니다.

### <a name="ui-navigation"></a>UI 탐색

사용자 인터페이스 탐색을 위한 다양 한 입력 장치를 지원 하 고 게임과 장치 간에 일관성을 유지 하기 위해 대부분의 _물리적_ 입력 장치는 [UI 탐색 컨트롤러](ui-navigation-controller.md)라는 별도의 _논리적_ 입력 장치로 동시에 작동 합니다. UI 탐색 컨트롤러는 입력 장치에서 UI 탐색 명령에 대 한 일반적인 어휘를 제공 합니다.

UI 탐색 컨트롤러인 비행 스틱은 필요한 탐색 명령 [집합](ui-navigation-controller.md#required-set) 을 조이스틱 및 **뷰**, **메뉴**, **FirePrimary**및 **FireSecondary** 단추에 매핑합니다.

| 탐색 명령 | 비행 스틱 입력                  |
| ------------------:| ----------------------------------- |
|                 위로 | 조이스틱                         |
|               아래로 | 조이스틱                       |
|               왼쪽 | 왼쪽 조이스틱                       |
|              오른쪽 | 조이스틱 오른쪽                      |
|               보기 | **보기** 단추                     |
|               메뉴 | **메뉴** 단추                     |
|             동의함 | **FirePrimary** 단추              |
|             취소 | **FireSecondary** 단추            |

비행 스틱은 선택적 탐색 명령 [집합](ui-navigation-controller.md#optional-set) 을 매핑하지 않습니다.

## <a name="detect-and-track-flight-sticks"></a>비행 스틱 검색 및 추적

비행 스틱 검색 및 추적은 gamepads에 대해 수행 하는 것과 정확히 동일한 방식으로 작동 합니다. 단, [게임 패드](/uwp/api/Windows.Gaming.Input.Gamepad) 클래스 대신 [FlightStick](/uwp/api/windows.gaming.input.flightstick) 클래스를 사용 하는 경우는 예외입니다. 자세한 내용은 [게임 패드 및 진동](gamepad-and-vibration.md) (영문)을 참조 하세요.

<!-- Flight sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected flight sticks and events to notify you when a flight stick is added or removed.

### The flight stick list

The [FlightStick](/uwp/api/windows.gaming.input.flightstick) class provides a static property, [FlightSticks](/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightSticks), which is a read-only list of flight sticks that are currently connected. Because you might only be interested in some of the connected flight sticks, we recommend that you maintain your own collection instead of accessing them through the `FlightSticks` property.

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

When a flight stick is added or removed, the [FlightStickAdded](/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickAdded) and [FlightStickRemoved](/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickRemoved) events are raised. You can register handlers for these events to keep track of the flight sticks that are currently connected.

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

## <a name="reading-the-flight-stick"></a>비행 스틱 읽기

관심이 있는 항공편을 식별 한 후에는 여기에서 입력을 수집할 준비가 된 것입니다. 그러나 사용할 수 있는 다른 종류의 입력과 달리 비행 스틱에서는 이벤트를 발생 시켜 상태 변경을 전달 하지 않습니다. 대신, 현재 상태를 _폴링하여_ 정기적으로 사용 합니다.

### <a name="polling-the-flight-stick"></a>비행 스틱 폴링

폴링은 정확한 시점에 비행 스틱의 스냅숏을 캡처합니다. 입력 수집에 대 한이 방법은 일반적으로 이벤트가 구동 되는 것이 아니라 결정적 루프에서 실행 되기 때문에 대부분의 게임에 적합 합니다. 또한 시간이 지남에 따라 수집 된 여러 개의 단일 입력에서 가져온 것 보다 한 번에 모두 수집 된 입력에서 게임 명령을 해석 하는 것이 더 간단 합니다.

GetCurrentReading를 호출 하 여 항공편 스틱을 폴링합니다. [FlightStick](/uwp/api/windows.gaming.input.flightstick.GetCurrentReading). 이 함수는 항공편 스틱 상태를 포함 하는 [FlightStickReading](/uwp/api/windows.gaming.input.flightstickreading) 를 반환 합니다.

다음 예제에서는 현재 상태에 대 한 비행 스틱을 폴링합니다.

```cpp
auto flightStick = myFlightSticks->GetAt(0);
FlightStickReading reading = flightStick->GetCurrentReading();
```

비행 스틱 상태 외에도 각 읽기는 상태가 검색 된 시기를 정확 하 게 나타내는 타임 스탬프를 포함 합니다. 타임 스탬프는 이전 판독값의 타이밍과 게임 시뮬레이션의 타이밍과 관련 된 경우에 유용 합니다.

### <a name="reading-the-joystick-and-throttle-input"></a>조이스틱 및 스로틀 입력 읽기

조이스틱은 X, Y 및 Z 축 (각각 roll, 피치 및 요)에서-1.0과 1.0 사이의 아날로그 읽기를 제공 합니다. 롤의 경우 값-1.0은 가장 왼쪽의 조이스틱 위치에 해당 하며, 값 1.0은 맨 오른쪽 위치에 해당 합니다. 피치의 경우 값-1.0은 최하위 조이스틱 위치에 해당 하 고, 값 1.0은 맨 위 위치에 해당 합니다. 요의 경우 값-1.0은 가장 반시계 방향으로 일치 하는 위치에 해당 하며, 값 1.0은 가장 시계 방향 위치에 해당 합니다.

모든 축에서 값은 조이스틱의 중심 위치에 약 0.0가 있지만 이후 판독값에도 불구 하 고 정확한 값은 매우 일반적입니다. 이 변형 완화 전략에 대해서는이 섹션의 뒷부분에서 설명 합니다.

[FlightStickReading](/uwp/api/windows.gaming.input.flightstickreading.Roll) 속성에서 조이스틱의 roll 값을 읽고, 피치 값을 [FlightStickReading](/uwp/api/windows.gaming.input.flightstickreading.Pitch) 속성에서 읽고, 요의 값을 FlightStickReading 속성에서 읽습니다. [요](/uwp/api/windows.gaming.input.flightstickreading.Yaw) 속성은 다음과 같습니다.

```cpp
// Each variable will contain a value between -1.0 and 1.0.
float roll = reading.Roll;
float pitch = reading.Pitch;
float yaw = reading.Yaw;
```

조이스틱 값을 읽을 때 조이스틱을 가운데 위치에 놓을 때 0.0의 중립 읽기를 안정적으로 생성 하지 않는 것을 알 수 있습니다. 대신, 조이스틱을 이동 하 여 가운데 위치로 반환할 때마다 0.0에 가까운 다른 값을 생성 합니다. 이러한 차이를 완화 하기 위해 무시 되는 이상적인 센터 위치 근처에 있는 값의 범위에 해당 하는 작은 _deadzone_을 구현할 수 있습니다.

Deadzone을 구현 하는 한 가지 방법은 조이스틱이 가운데에서 이동 하는 정도를 확인 하 고 선택한 거리 보다 가까운 판독값을 무시 하는 것입니다. &mdash;조이스틱 판독값은 기본적으로 &mdash; 숫자가 피타고라스 정리를 사용 하는 것이 아니라 극좌표를 사용 하기 때문에 정확 하지 않은 거리를 계산할 수 있습니다. 그러면 방사형 deadzone 생성 됩니다.

다음 예제에서는 숫자가 피타고라스 정리를 사용 하는 기본 방사형 deadzone를 보여 줍니다.

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

### <a name="reading-the-buttons-and-hat-switch"></a>단추 및 hat 스위치 읽기

각 비행 스틱의 두 번 단추는 프레스 (다운) 또는 릴리스 (up) 여부를 나타내는 디지털 읽기를 제공 합니다. 효율성을 위해 단추 판독값은 개별 부울 값으로 표시 되지 않습니다 &mdash; . 이러한 값은 모두 [FlightStickButtons](/uwp/api/windows.gaming.input.flightstickbuttons) 열거형으로 표현 되는 단일 비트 필드에 압축 됩니다. 또한 8 방향 hat 스위치는 [GameControllerSwitchPosition](/uwp/api/windows.gaming.input.gamecontrollerswitchposition) 열거형으로 표현 되는 단일 비트 필드에 압축 된 방향을 제공 합니다.

> [!NOTE]
> 비행 스틱에는 **보기** 및 **메뉴** 단추와 같은 UI 탐색에 사용 되는 추가 단추가 있습니다. 이러한 단추는 열거형의 일부가 아니므로 `FlightStickButtons` 비행 스틱에 UI 탐색 장치로 액세스 해야만 읽을 수 있습니다. 자세한 내용은 [UI 탐색 컨트롤러](ui-navigation-controller.md)를 참조 하세요.

단추 값은 [FlightStickReading](/uwp/api/windows.gaming.input.flightstickreading.Buttons) 속성에서 읽습니다. 이 속성은 비트 필드 이므로 관심이 있는 단추의 값을 격리 하기 위해 비트 마스킹을 사용 됩니다. 해당 비트가 설정 된 경우 단추가 눌러져 있습니다 (down). 그렇지 않으면 해제 됩니다 (up).

다음 예에서는 **FirePrimary** 단추를 눌렀는지 여부를 확인 합니다.

```cpp
if (FlightStickButtons::FirePrimary == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is pressed.
}
```

다음 예에서는 **FirePrimary** 단추가 해제 되어 있는지 여부를 확인 합니다.

```cpp
if (FlightStickButtons::None == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is released (not pressed).
}
```

경우에 따라 단추를 눌렀다 놓은 상태에서 눌린 상태로 전환 하는 경우, 여러 단추를 눌렀는지 또는 눌렀다 놓았을 때 또는 단추 집합이 특정 방법으로 몇 번의 단추에 배치 되는지 여부를 확인 하는 것이 좋습니다 &mdash; . 이러한 각 조건을 검색 하는 방법에 대 한 자세한 내용은 [단추 전환 검색](input-practices-for-games.md#detecting-button-transitions) 및 [복잡 한 단추 배열](input-practices-for-games.md#detecting-complex-button-arrangements)검색을 참조 하세요.

Hat 스위치 값은 [FlightStickReading. HatSwitch](/uwp/api/windows.gaming.input.flightstickreading.HatSwitch) 속성에서 읽습니다. 이 속성은 비트 필드 이기도 하므로 hat 스위치의 위치를 격리 하기 위해 비트 마스킹을 다시 사용 합니다.

다음 예에서는 hat 스위치가 위쪽 위치에 있는지 여부를 확인 합니다.

```cpp
if (GameControllerSwitchPosition::Up == (reading.HatSwitch & GameControllerSwitchPosition::Up))
{
    // The hat switch is in the up position.
}
```

다음 예에서는 hat 스위치가 가운데 위치에 있는지 여부를 확인 합니다.

```cpp
if (GameControllerSwitchPosition::Center == (reading.HatSwitch & GameControllerSwitchPosition::Center))
{
    // The hat switch is in the center position.
}
```

<!--## Run the InputInterfacingUWP sample

The [InputInterfacingUWP sample _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) demonstrates how to use flight sticks and different kinds of input devices in tandem, as well as how these input devices behave as UI navigation controllers.-->

## <a name="see-also"></a>참고 항목

* [UINavigationController 클래스입니다.](/uwp/api/windows.gaming.input.uinavigationcontroller)
* [IGameController 인터페이스입니다.](/uwp/api/windows.gaming.input.igamecontroller)
* [게임용 입력 시스템](input-practices-for-games.md)