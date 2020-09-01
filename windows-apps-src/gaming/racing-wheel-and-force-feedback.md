---
title: 레이스 휠 및 피드백
description: Windows 게이밍의 입력 레이스 휠 Api를 사용 하 여 레이스 휠에 대 한 기능을 검색 하 고, 기능을 확인 하 고, 읽고, 피드백을 보냅니다.
ms.assetid: 6287D87F-6F2E-4B67-9E82-3D6E51CBAFF9
ms.date: 05/09/2018
ms.topic: article
keywords: windows 10, uwp, 게임, 레이스 휠, 강제 피드백
ms.localizationpriority: medium
ms.openlocfilehash: 0d92fe45a008ee0c3864355faf8133ab8e1ebefb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163067"
---
# <a name="racing-wheel-and-force-feedback"></a>레이싱 휠 및 힘 피드백

이 페이지에서는 [RacingWheel][racingwheel] 를 사용 하는 Xbox one 경주 휠 프로그래밍의 기본 사항 및 UWP (유니버설 Windows 플랫폼) 관련 api에 대해 설명 합니다.

이 페이지를 읽으면 다음에 대해 알아봅니다.

* 연결 된 레이스 휠 및 해당 사용자 목록을 수집 하는 방법
* 경쟁 휠이 추가 또는 제거 되었는지 검색 하는 방법
* 하나 이상의 레이스 휠에서 입력을 읽는 방법
* force 피드백 명령을 보내는 방법
* 경주 휠이 UI 탐색 장치로 작동 하는 방식

## <a name="racing-wheel-overview"></a>레이스 휠 개요

경주 휠은 실제 racecar의 느낌이 비슷한 입력 장치입니다. 경주 휠은 자동차 또는 트럭을 지 원하는 아케이드 스타일 및 시뮬레이션 스타일의 게임을 위한 완벽 한 입력 장치입니다. 경주 휠 [은 windows 10](/uwp/api/windows.gaming.input) 및 XBOX one UWP 앱에서 지원 됩니다.

Xbox One 경주 휠은 다양 한 가격 점수에 제공 되며, 일반적으로 가격 점수가 증가 함에 따라 더 많은 입력을 제공 하 고 피드백 기능을 적용 합니다. 모든 레이스 휠에는 아날로그 조종 휠, 아날로그 스로틀 및 브레이크 컨트롤 및 일부 휠 단추가 장착 되어 있습니다. 일부 레이스 휠에는 아날로그 클러치 및 핸드 브레이크 컨트롤, 패턴 shifters 및 강제 피드백 기능이 추가로 장착 되어 있습니다. 모든 레이스 휠에 동일한 기능 집합을 제공 하는 것은 아니고, 특정 기능에 대 한 지원에 따라 달라질 수 있습니다 &mdash; . 예를 들어 조종 핸들은 다양 한 회전 범위를 지원할 수 있으며 패턴 shifters 다른 수의 기어를 지원할 수 있습니다.

### <a name="device-capabilities"></a>디바이스 성능

다른 Xbox One 레이스 휠은 다양 한 선택적 장치 기능 집합 및 해당 기능에 대 한 다양 한 지원 수준을 제공 합니다. 단일 종류의 입력 장치 간의 이러한 변형 수준은 [Windows. 게이밍](/uwp/api/windows.gaming.input) API에서 지원 되는 장치 간에 고유 합니다. 또한 대부분의 장치에서 하나 이상의 선택적 기능이 나 기타 변형을 지원 합니다. 따라서 각각의 연결 된 레이스 휠의 기능을 개별적으로 확인 하 고 게임에 적합 한 모든 기능을 지 원하는 것이 중요 합니다.

자세한 내용은 [레이스 휠 기능 결정](#determining-racing-wheel-capabilities)을 참조 하세요.

### <a name="force-feedback"></a>피드백 적용

일부 Xbox One 레이스 휠에서는 진정한 힘 피드백을 제공 합니다 &mdash; . 즉, 간단한 진동 뿐만 아니라 조정 휠 등의 제어 축에 실제 힘을 적용할 수 있습니다 &mdash; . 게임에서는이 기능을 사용 하 여 집중 교육 (시뮬레이션 된_크래시 손상_, "도로 느낌")을 만들고 원활 하 게 작동 하는 문제를 늘립니다.

자세한 내용은 [Force 피드백 개요](#force-feedback-overview)를 참조 하세요.

### <a name="ui-navigation"></a>UI 탐색

사용자 인터페이스 탐색을 위한 다양 한 입력 장치를 지원 하 고 게임과 장치 간에 일관성을 유지 하기 위해 대부분의 _물리적_ 입력 장치는 [UI 탐색 컨트롤러](ui-navigation-controller.md)라는 별도의 _논리적_ 입력 장치로 동시에 작동 합니다. UI 탐색 컨트롤러는 입력 장치에서 UI 탐색 명령에 대 한 일반적인 어휘를 제공 합니다.

아날로그 컨트롤에 대 한 고유한 중점 및 경쟁 바퀴 간의 변형 수준으로 인해 일반적으로 [게임 패드](gamepad-and-vibration.md)와 비슷한 디지털 D-패드, **보기**, **메뉴**, **a**, **B**, **X**및 **Y** 단추가 포함 됩니다. 이러한 단추는 게임 플레이 명령을 지원 하기 위한 것이 아니며 레이스 휠 단추로 쉽게 액세스할 수 없습니다.

UI 탐색 컨트롤러인, 경쟁 바퀴는 필요한 탐색 명령 [집합](ui-navigation-controller.md#required-set) 을 왼쪽 엄지 스틱, D-패드, **뷰**, **메뉴**, **a**및 **B** 단추에 매핑합니다.

| 탐색 명령 | 레이스 휠 입력 |
| ------------------:| ------------------ |
|                 위로 | D-패드 위로           |
|               아래로 | D-패드 다운         |
|               왼쪽 | D-패드 왼쪽         |
|              오른쪽 | D-패드 오른쪽        |
|               보기 | 보기 단추        |
|               메뉴 | 메뉴 버튼        |
|             동의함 | 단추           |
|             취소 | B 단추           |

또한 일부 레이스 휠에서는 몇 가지 선택적인 탐색 명령 [집합](ui-navigation-controller.md#optional-set) 을 지 원하는 다른 입력에 매핑할 수 있지만 명령 매핑은 장치 마다 다를 수 있습니다. 이러한 명령도 지원 하는 것이 좋습니다. 그러나 이러한 명령이 게임 인터페이스를 탐색 하는 데 반드시 필요한 것은 아닙니다.

| 탐색 명령 | 레이스 휠 입력    |
| ------------------:| --------------------- |
|            Page Up | _잠기기_              |
|          Page Down | _잠기기_              |
|          왼쪽 페이지 | _잠기기_              |
|         오른쪽 페이지 | _잠기기_              |
|          위로 스크롤 | _잠기기_              |
|        아래로 스크롤 | _잠기기_              |
|        왼쪽으로 스크롤 | _잠기기_              |
|       오른쪽으로 스크롤 | _잠기기_              |
|          컨텍스트 1 | X 단추 (_일반적으로_) |
|          컨텍스트 2 | Y 단추 (_일반적으로_) |
|          컨텍스트 3 | _잠기기_              |
|          컨텍스트 4 | _잠기기_              |

## <a name="detect-and-track-racing-wheels"></a>레이스 휠 검색 및 추적

Gamepads에 대해 수행 하는 것과 정확히 동일한 방법으로, 즉, [게임 패드](/uwp/api/Windows.Gaming.Input.Gamepad) 클래스 대신 [RacingWheel](/uwp/api/windows.gaming.input.racingwheel) 클래스를 사용 하 여 레이스 휠 검색 및 추적이 작동 합니다. 자세한 내용은 [게임 패드 및 진동](gamepad-and-vibration.md) (영문)을 참조 하세요.

<!-- Racing wheels are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected racing wheels and events to notify you when a racing wheel is added or removed.

### The racing wheels list

The [RacingWheel](/uwp/api/windows.gaming.input.racingwheel) class provides a static property, [RacingWheels](/uwp/api/windows.gaming.input.racingwheel.racingwheels#Windows_Gaming_Input_RacingWheel_RacingWheels), which is a read-only list of racing wheels that are currently connected. Because you might only be interested in some of the connected racing wheels, it's recommended that you maintain your own collection instead of accessing them through the `RacingWheels` property.

The following example copies all connected racing wheels into a new collection.
```cpp
auto myRacingWheels = ref new Vector<RacingWheel^>();

for (auto racingwheel : RacingWheel::RacingWheels)
{
    // This code assumes that you're interested in all racing wheels.
    myRacingWheels->Append(racingwheel);
}
```

### Adding and removing racing wheels

When a racing wheel is added or removed the [RacingWheelAdded](/uwp/api/windows.gaming.input.racingwheel.racingwheeladded) and [RacingWheelRemoved](/uwp/api/windows.gaming.input.racingwheel.racingwheelremoved) events are raised. You can register handlers for these events to keep track of the racing wheels that are currently connected.

The following example starts tracking an racing wheels that's been added.
```cpp
RacingWheel::RacingWheelAdded += ref new EventHandler<RacingWheel^>(Platform::Object^, RacingWheel^ args)
{
    // This code assumes that you're interested in all new racing wheels.
    myRacingWheels->Append(args);
}
```

The following example stops tracking a racing wheel that's been removed.
```cpp
RacingWheel::RacingWheelRemoved += ref new EventHandler<RacingWheel^>(Platform::Object^, RacingWheel^ args)
{
    unsigned int indexRemoved;

    if(myRacingWheels->IndexOf(args, &indexRemoved))
    {
        myRacingWheels->RemoveAt(indexRemoved);
    }
}
```

### Users and headsets

Each racing wheel can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-racing-wheel"></a>레이스 휠 읽기

관심이 있는 경주 바퀴를 확인 한 후에는 입력을 수집할 준비가 된 것입니다. 그러나 사용할 수 있는 다른 종류의 입력과 달리, 레이스 휠에서는 이벤트를 발생 시켜 상태 변경 통신을 하지 않습니다. 대신, 현재 상태를 _폴링하여_ 정기적으로 사용 합니다.

### <a name="polling-the-racing-wheel"></a>레이스 휠 폴링

폴링은 정확한 시점에서 경주 휠의 스냅숏을 캡처합니다. 입력 수집에 대 한이 방법은 일반적으로 이벤트 기반이 아니라 결정적 루프에서 실행 되기 때문에 대부분의 게임에 적합 합니다. 또한 시간이 지남에 따라 수집 된 여러 개의 단일 입력에서 가져온 것 보다 한 번에 모두 수집 된 입력에서 게임 명령을 해석 하는 것이 더 간단 합니다.

[GetCurrentReading](/uwp/api/windows.gaming.input.racingwheel.getcurrentreading#Windows_Gaming_Input_RacingWheel_GetCurrentReading)를 호출 하 여 레이스 휠를 폴링합니다. 이 함수는 레이스 휠의 상태를 포함 하는 [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) 를 반환 합니다.

다음 예제에서는 현재 상태에 대 한 경쟁 바퀴를 폴링합니다.

```cpp
auto racingwheel = myRacingWheels[0];

RacingWheelReading reading = racingwheel->GetCurrentReading();
```

경쟁 휠 상태 외에도 각 읽기는 상태가 검색 된 시기를 정확 하 게 나타내는 타임 스탬프를 포함 합니다. 타임 스탬프는 이전 판독값의 타이밍과 게임 시뮬레이션의 타이밍과 관련 된 경우에 유용 합니다.

### <a name="determining-racing-wheel-capabilities"></a>레이싱 휠 기능 확인

대부분의 레이스 휠 컨트롤은 선택 사항이 며 필요한 컨트롤 에서도 다른 변형을 지원 하기 때문에 각 레이스 휠의 기능을 개별적으로 확인 해야 경쟁 바퀴를 읽을 때마다 수집 된 입력을 처리할 수 있습니다.

선택적 컨트롤은 수동 브레이크, 클러치 및 패턴으로, 각각 경주 휠의 [Has핸드 브레이크](/uwp/api/windows.gaming.input.racingwheel.hashandbrake#Windows_Gaming_Input_RacingWheel_HasHandbrake), [Has클러치](/uwp/api/windows.gaming.input.racingwheel.hasclutch#Windows_Gaming_Input_RacingWheel_HasClutch)및 [HasPatternShifter](/uwp/api/windows.gaming.input.racingwheel.haspatternshifter#Windows_Gaming_Input_RacingWheel_HasPatternShifter) 속성을 읽어 연결 된 레이스 휠이 이러한 컨트롤을 지원 하는지 여부를 확인할 수 있습니다. 속성의 값이 **true**이면 컨트롤이 지원 됩니다. 그렇지 않으면 지원 되지 않습니다.

```cpp
if (racingwheel->HasHandbrake)
{
    // the handbrake is supported
}

if (racingwheel->HasClutch)
{
    // the clutch is supported
}

if (racingwheel->HasPatternShifter)
{
    // the pattern shifter is supported
}
```

또한 다를 수 있는 컨트롤은 조종 휠 및 패턴으로 구분 됩니다. 조종 핸들은 실제 휠이 지원할 수 있는 실제 회전의 정도에 따라 다를 수 있으며, 패턴 이동 기는 지 원하는 고유 전달 기어의 수에 따라 달라질 수 있습니다. 경쟁 휠의 속성을 읽어 실제 휠에서 지 원하는 최대 회전 각도를 결정할 수 있습니다 `MaxWheelAngle` . 값은 지원 되는 최대 실제 각도 (양수)로,이는 카운터 클록 지향 방향 (음수도) 에서도 지원 됩니다. 경주 휠의 속성을 읽어 패턴에서 지 원하는 가장 높은 전방 기어를 결정할 수 있습니다 `MaxPatternShifterGear` . 해당 값은 지원 되는 가장 높은 전방 기어 ( &mdash; 즉, 값이 4 인 경우)는 역방향, 중립, 첫 번째, 두 번째, 세 번째 및 네 번째 기어를 지원 합니다.

```cpp
auto maxWheelDegrees = racingwheel->MaxWheelAngle;
auto maxShifterGears = racingwheel->MaxPatternShifterGear;
```

마지막으로, 일부 레이스 휠은 조종 핸들을 통해 피드백을 지원 합니다. 레이스 휠의 [WheelMotor](/uwp/api/windows.gaming.input.racingwheel.wheelmotor#Windows_Gaming_Input_RacingWheel_WheelMotor) 속성을 읽어 연결 된 레이스 휠에서 강제 피드백을 지원 하는지 여부를 확인할 수 있습니다. 가 null이 아닌 경우에는 강제 피드백이 지원 됩니다. `WheelMotor` 그렇지 않으면 지원 되지 않습니다. **null**

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    // force feedback is supported
}
```

지원 되는 레이스 휠의 피드백 기능을 사용 하는 방법에 대 한 자세한 내용은 [force 피드백 개요](#force-feedback-overview)를 참조 하세요.

### <a name="reading-the-buttons"></a>단추 읽기

각 레이스 휠 단추는 &mdash; D-패드의 네 가지 방향, **이전 기어** 및 **다음 기어** 단추, 16 개 추가 단추를 통해 해당 단추를 &mdash; 눌린 (다운) 또는 릴리스 (up) 여부를 나타내는 디지털 읽기를 제공 합니다. 효율성을 위해 단추 판독값은 개별 부울 값으로 표시 되지 않습니다. 대신 [RacingWheelButtons](/uwp/api/windows.gaming.input.racingwheelbuttons) 열거형으로 표현 되는 단일 비트 필드에 압축 됩니다.

> [!NOTE]
> 경주 휠에는 **보기** 및 **메뉴** 단추와 같은 UI 탐색에 사용 되는 추가 단추가 있습니다. 이러한 단추는 열거형의 일부가 아니므로 `RacingWheelButtons` UI 탐색 장치로 경주 휠에 액세스 하 여 읽을 수만 있습니다. 자세한 내용은 [UI 탐색 장치](ui-navigation-controller.md)를 참조 하세요.

`Buttons` [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) 구조체의 속성에서 단추 값을 읽습니다. 이 속성은 비트 필드 이므로 관심이 있는 단추의 값을 격리 하기 위해 비트 마스킹을 사용 됩니다. 해당 비트가 설정 된 경우 단추가 눌러져 있습니다 (down). 그렇지 않으면 해제 됩니다 (up).

다음 예에서는 **다음 기어** 단추를 눌렀는지 여부를 확인 합니다.

```cpp
if (RacingWheelButtons::NextGear == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is pressed
}
```

다음 예에서는 다음 기어 단추가 해제 되어 있는지 여부를 확인 합니다.

```cpp
if (RacingWheelButtons::None == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is released (not pressed)
}
```

경우에 따라 단추를 눌렀다 놓은 상태에서 눌린 상태로 전환 하는 경우, 여러 단추를 눌렀는지 또는 눌렀다 놓았을 때 또는 단추 집합이 특정 방법으로 몇 번의 단추에 배치 되는지 여부를 확인 하는 것이 좋습니다 &mdash; . 이러한 조건을 검색 하는 방법에 대 한 자세한 내용은 [단추 전환 검색](input-practices-for-games.md#detecting-button-transitions) 및 [복잡 한 단추 배열 검색](input-practices-for-games.md#detecting-complex-button-arrangements)을 참조 하세요.

### <a name="reading-the-wheel"></a>휠 읽기

조종 휠은-1.0에서 + 1.0 사이의 아날로그 판독값을 제공 하는 필수 컨트롤입니다. 값-1.0은 가장 왼쪽의 휠 위치에 해당 합니다. + 1.0 값은 맨 오른쪽 위치에 해당 합니다. `Wheel` [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) 구조체의 속성에서 조종 핸들의 값을 읽습니다.

```cpp
float wheel = reading.Wheel;  // returns a value between -1.0 and +1.0.
```

휠 판독값은 실제 레이스 휠에서 지원 되는 회전 범위에 따라 실제 바퀴에서 다른 각도의 실제 회전에 해당 하지만 일반적으로 휠 판독값을 조정 하지 않으려고 합니다. 더 큰 회전을 지 원하는 바퀴는 더 높은 정밀도를 제공 합니다.

### <a name="reading-the-throttle-and-brake"></a>스로틀 및 브레이크 읽기

스로틀 및 브레이크는 모두 부동 소수점 값으로 표현 되는 0.0 (완전히 릴리스된) 및 1.0 (완전히 누름) 사이의 아날로그 판독값을 제공 하는 필수 컨트롤입니다. RacingWheelReading 구조체의 속성에서 스로틀 컨트롤의 값을 읽습니다 .이 경우에는 `Throttle` 속성에서 브레이크 컨트롤의 값을 [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) 읽습니다 `Brake` .

```cpp
float throttle = reading.Throttle;  // returns a value between 0.0 and 1.0
float brake    = reading.Brake;     // returns a value between 0.0 and 1.0
```

### <a name="reading-the-handbrake-and-clutch"></a>수동 브레이크 및 클러치 읽기

수동 브레이크 및 클러치는 각각 부동 소수점 값으로 표현 되는 0.0 (완전히 릴리스된) 및 1.0 (완전히 개입 됨) 사이의 아날로그 판독값을 제공 하는 선택적 컨트롤입니다. RacingWheelReading 구조체의 속성에서 수동 브레이크 컨트롤의 값을 읽습니다 .이 경우에는 `Handbrake` 속성에서 클러치 컨트롤의 값을 [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) 읽습니다 `Clutch` .

```cpp
float handbrake = 0.0;
float clutch = 0.0;

if(racingwheel->HasHandbrake)
{
    handbrake = reading.Handbrake;  // returns a value between 0.0 and 1.0
}

if(racingwheel->HasClutch)
{
    clutch = reading.Clutch;        // returns a value between 0.0 and 1.0
}
```

### <a name="reading-the-pattern-shifter"></a>패턴의 무늬를 읽습니다.

패턴 지정 기는 부호 있는 정수 값으로 표시 되는-1과 [MaxPatternShifterGear](/uwp/api/windows.gaming.input.racingwheel.maxpatternshiftergear) 사이의 디지털 읽기를 제공 하는 선택적 컨트롤입니다. -1 또는 0 값은 각각 _역방향_ 및 _중립_ 기어에 해당 합니다. 점점 더 증가 하는 양의 값은 **MaxPatternShifterGear**(포함)까지 전달 되는 더 많은 기어에 해당 합니다. 패턴 PatternShifterGear의 값은 [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) 구조체의 [PatternShifterGear](/uwp/api/windows.gaming.input.racingwheelreading.patternshiftergear) 속성에서 읽습니다.

```cpp
if (racingwheel->HasPatternShifter)
{
    gear = reading.PatternShifterGear;
}
```

> [!NOTE]
> 지원 되는 패턴의 무늬는 **이전 기어** 및 플레이어 자동차의 현재 기어에도 영향을 주는 **다음 기어** 단추와 함께 존재 합니다. 이러한 입력을 통합 하는 간단한 전략은 플레이어에서 자동차에 대 한 자동 전송을 선택 하는 경우 패턴 다시 시작 (및 클러치)을 무시 하 고, 플레이어에서 자동차에 대해 수동 전송을 선택할 때 **이전** 및 **다음 기어** 단추를 무시 하는 간단한 전략입니다. 게임에 적합 하지 않은 경우 다른 통합 전략을 구현할 수 있습니다.

## <a name="run-the-inputinterfacing-sample"></a>InputInterfacing 샘플 실행

[InputInterfacingUWP 샘플 _(github)_ ](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) 에서는 경쟁 휠 및 다양 한 종류의 입력 장치를 함께 사용 하는 방법 뿐만 아니라 이러한 입력 장치가 UI 탐색 컨트롤러로 동작 하는 방식을 보여 줍니다.

## <a name="force-feedback-overview"></a>강제 피드백 개요

많은 레이스 휠에는 더 몰입 형 및 까다로운 구동 환경을 제공 하기 위한 강제 피드백 기능이 있습니다. 강제 피드백을 지 원하는 레이스 휠에는 일반적으로 휠 회전의 축 인 단일 축을 따라 조종 핸들에 강제로 적용 되는 단일 모터가 있습니다. Force [사용자 의견은](/uwp/api/windows.gaming.input.forcefeedback) windows 10 및 XBOX one UWP 앱에서 지원 됩니다.

> [!NOTE]
> Force 피드백 Api는 force의 여러 축을 지원할 수 있지만 현재는 Xbox One 레이스 휠이 휠 회전과는 다른 피드백 축을 지원 하지 않습니다.

## <a name="using-force-feedback"></a>강제 피드백 사용

이 섹션에서는 Xbox One 레이스 휠에 대 한 force 피드백 효과를 프로그래밍 하는 기본 사항을 설명 합니다. 효과를 사용 하 여 피드백을 적용 합니다 .이 효과는 먼저 force 피드백 장치에 로드 된 다음, 소리 효과와 유사한 방식으로 시작, 일시 중지, 다시 시작 및 중지 될 수 있습니다. 그러나 먼저 레이스 휠의 피드백 기능을 결정 해야 합니다.

### <a name="determining-force-feedback-capabilities"></a>Force 피드백 기능 결정

레이스 휠의 [WheelMotor](/uwp/api/windows.gaming.input.racingwheel.wheelmotor#Windows_Gaming_Input_RacingWheel_WheelMotor) 속성을 읽어 연결 된 레이스 휠에서 강제 피드백을 지원 하는지 여부를 확인할 수 있습니다. 가 null 인 경우에는 강제 피드백이 지원 되지 않습니다 `WheelMotor` . 그렇지 않은 경우에는 강제 피드백이 지원 되며, 영향을 받을 수 있는 축과 같은 모터의 특정 피드백 기능을 계속 확인할 수 있습니다. **null**

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    auto axes = racingwheel->WheelMotor->SupportedAxes;

    if(ForceFeedbackEffectAxes::X == (axes & ForceFeedbackEffectAxes::X))
    {
        // Force can be applied through the X axis
    }

    if(ForceFeedbackEffectAxes::Y == (axes & ForceFeedbackEffectAxes::Y))
    {
        // Force can be applied through the Y axis
    }

    if(ForceFeedbackEffectAxes::Z == (axes & ForceFeedbackEffectAxes::Z))
    {
        // Force can be applied through the Z axis
    }
}
```

### <a name="loading-force-feedback-effects"></a>Force 피드백 효과를 로드 하는 중

Force 피드백 효과는 게임의 명령에서 자율적으로 "재생" 되는 피드백 장치에 로드 됩니다. 여러 가지 기본 효과가 제공 됩니다. [IForceFeedbackEffect](/uwp/api/windows.gaming.input.forcefeedback.iforcefeedbackeffect) 인터페이스를 구현 하는 클래스를 통해 사용자 지정 효과를 만들 수 있습니다.

| Effect 클래스         | 효과 설명                                                                     |
| -------------------- | -------------------------------------------------------------------------------------- |
| ConditionForceEffect | 장치 내에서 현재 센서에 대 한 응답으로 변수 force를 적용 하는 효과입니다. |
| ConstantForceEffect  | 벡터를 따라 일관 된 적용을 적용 하는 효과입니다.                                  |
| PeriodicForceEffect  | 벡터를 따라 파형에 의해 정의 된 변수 force를 적용 하는 효과입니다.           |
| RampForceEffect      | 벡터를 따라 선형으로 늘어나는/감소 하는 효과를 적용 하는 효과입니다.          |

```cpp
using FFLoadEffectResult = ForceFeedback::ForceFeedbackLoadEffectResult;

auto effect = ref new Windows.Gaming::Input::ForceFeedback::ConstantForceEffect();
auto time = TimeSpan(10000);

effect->SetParameters(Windows::Foundation::Numerics::float3(1.0f, 0.0f, 0.0f), time);

// Here, we assume 'racingwheel' is valid and supports force feedback

IAsyncOperation<FFLoadEffectResult>^ request
    = racingwheel->WheelMotor->LoadEffectAsync(effect);

auto loadEffectTask = Concurrency::create_task(request);

loadEffectTask.then([this](FFLoadEffectResult result)
{
    if (FFLoadEffectResult::Succeeded == result)
    {
        // effect successfully loaded
    }
    else
    {
        // effect failed to load
    }
}).wait();
```

### <a name="using-force-feedback-effects"></a>강제 피드백 효과 사용

로드 된 후에는 경쟁 휠의 속성에서 함수를 호출 `WheelMotor` 하거나 피드백 효과 자체에서 함수를 호출 하 여 개별적으로 효과를 시작, 일시 중지, 다시 시작 및 중지할 수 있습니다. 일반적으로 게임 플레이를 시작 하기 전에 피드백 장치에 사용할 모든 효과를 로드 한 다음 각 기능을 사용 하 여 `SetParameters` 게임 재생이 진행 됨에 따라 효과를 업데이트 해야 합니다.

```cpp
if (ForceFeedbackEffectState::Running == effect->State)
{
    effect->Stop();
}
else
{
    effect->Start();
}
```

마지막으로, 필요할 때마다 특정 레이스 휠에서 전체 강제 피드백 시스템을 비동기적으로 사용 하거나 사용 하지 않도록 설정 하거나 다시 설정할 수 있습니다.

## <a name="see-also"></a>참고 항목

* [Windows.Gaming.Input.UINavigationController](/uwp/api/windows.gaming.input.uinavigationcontroller)
* [IGameController.](/uwp/api/windows.gaming.input.igamecontroller)
* [게임용 입력 시스템](input-practices-for-games.md)

[Windows.Gaming.Input]: /uwp/api/Windows.Gaming.Input
[Windows.Gaming.Input.UINavigationController]: /uwp/api/Windows.Gaming.Input.UINavigationController
[Windows.Gaming.Input.IGameController]: /uwp/api/Windows.Gaming.Input.IGameController
[racingwheel]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheels]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheeladded]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheelremoved]: /uwp/api/Windows.Gaming.Input.RacingWheel
[haspatternshifter]: /uwp/api/Windows.Gaming.Input.RacingWheel
[hashandbrake]: /uwp/api/Windows.Gaming.Input.RacingWheel
[hasclutch]: /uwp/api/Windows.Gaming.Input.RacingWheel
[maxpatternshiftergear]: /uwp/api/Windows.Gaming.Input.RacingWheel
[maxwheelangle]: /uwp/api/Windows.Gaming.Input.RacingWheel
[getcurrentreading]: /uwp/api/Windows.Gaming.Input.RacingWheel
[wheelmotor]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheelreading]: /uwp/api/Windows.Gaming.Input.RacingWheelReading
[racingwheelbuttons]: /uwp/api/Windows.Gaming.Input.RacingWheelButtons