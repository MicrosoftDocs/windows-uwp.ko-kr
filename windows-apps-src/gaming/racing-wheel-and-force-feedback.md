---
author: eliotcowley
title: 레이싱 휠 및 힘 피드백
description: Windows.Gaming.Input 레이싱 휠 API를 사용하여 여러 기능의 감지와 확인, 레이싱 휠에 대한 힘 피드백 명령 읽기와 보내기 작업을 수행할 수 있습니다.
ms.assetid: 6287D87F-6F2E-4B67-9E82-3D6E51CBAFF9
ms.author: wdg-dev-content
ms.date: 05/09/2018
ms.topic: article
keywords: Windows 10, uwp, 게임, 레이싱 휠, 힘 피드백
ms.localizationpriority: medium
ms.openlocfilehash: 20b4b35bb729ee49dbfd3f2b2b2a029a4319521c
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "6453650"
---
# <a name="racing-wheel-and-force-feedback"></a>레이싱 휠 및 힘 피드백

이 페이지에서는 UWP(유니버설 Windows 플랫폼)용 [Windows.Gaming.Input.RacingWheel][racingwheel] 및 관련 API를 사용하여 Xbox One 레이싱 휠용 프로그래밍의 기본 사항을 설명합니다.

이 페이지에서는 다음에 대해 알아봅니다.

* 연결된 레이싱 휠 및 사용자 목록을 수집하는 방법
* 레이싱 휠이 추가 또는 제거된 사실을 감지하는 방법
* 하나 이상의 레이싱 휠에서 입력을 읽는 방법
* 힘 피드백 명령을 보내는 방법
* 레이싱 휠이 UI 탐색 장치로 동작하는 방법

## <a name="racing-wheel-overview"></a>레이싱 휠 개요

레이싱 휠은 실제 경주용 자동차의 조종석과 비슷한 느낌을 주는 입력 장치로, 자동차 또는 트럭이 등장하는 아케이드 스타일 및 시뮬레이션 스타일의 레이싱 게임에 매우 적합한 입력 장치입니다. 레이싱 휠은 Windows 10 및 Xbox One UWP 앱에서 [Windows.Gaming.Input][] 네임스페이스로 지원됩니다.

Xbox One 레이싱 휠은 다양한 가격대로 제공되며, 일반적으로 가격대가 높아질수록 더욱 많은 입력 및 힘 피드백 기능을 갖추게 됩니다. 모든 레이싱 휠에는 아날로그 조종 핸들, 아날로그 스로틀, 브레이크 컨트롤, 몇 가지 핸들 부착 단추가 설치되어 있습니다. 일부 레이싱 휠에는 아날로그 클러치 및 핸드 브레이크 컨트롤, 패턴 변환기, 힘 피드백 기능이 추가로 설치됩니다. 모든 레이싱 휠이 동일한 기능 모음을 갖추고 있는 것은 아니며 특정 기능에 대한 지원도 다를 수 있습니다. 예를 들어 조종 핸들이 지원하는 회전 범위나 패턴 변환기가 지원하는 기어의 수가 다양할 수 있습니다.

### <a name="device-capabilities"></a>장치 접근 권한 값

Xbox One 레이싱 휠은 종류마다 다양한 옵션 장치 기능 모음을 제공하고 이러한 기능의 지원 수준도 폭넓게 제공합니다. 단일 기종 입력 장치 간 변형 수준은 [Windows.Gaming.Input][] API에서 지원하는 여러 장치 중에서 독보적입니다. 또 사용자가 접하는 대부분의 장치는 일부 옵션 기능 또는 기타 변형 기능을 최소한 지원합니다. 그렇기 때문에 연결된 레이싱 휠 각각의 기능을 개별적으로 확인하고 게임에 적합한 모든 변형 기능을 지원하는 것이 중요합니다.

자세한 내용은 [레이싱 휠 기능 확인](#determining-racing-wheel-capabilities)을 참조하세요.

### <a name="force-feedback"></a>힘 피드백

일부 Xbox One 레이싱 휠은 진정한 의미의 힘 피드백을 제공합니다. 즉 단순한 진동이 아니라 조종 핸들과 같은 제어 축에 실제로 힘을 가할 수 있습니다 이 기능의 사용으로 게임에 더 큰 몰입감이 생기며(_모의 충돌 충격_, "도로 주행 느낌") 능숙한 운전에 대한 도전 의식이 높아집니다.

자세한 내용은 [힘 피드백 개요](#force-feedback-overview)를 참조하세요.

### <a name="ui-navigation"></a>UI 탐색

사용자 인터페이스 탐색을 위해 다양한 입력 장치를 지원해야 하는 부담을 덜고 게임과 장치 간 일관성을 추구하기 위해 대부분의 _물리적_ 입력 장치는 [UI 탐색 컨트롤러](ui-navigation-controller.md)라고 하는 별도의 _논리적_ 입력 장치 역할을 동시에 수행합니다. UI 탐색 컨트롤러는 입력 장치 전반적으로 UI 탐색 명령에 대한 공통 어휘를 제공합니다.

아날로그 컨트롤과 다양한 레이싱 휠 간의 변형 정도에 특별히 중점을 두고 있기 때문에 일반적으로 디지털 D-패드와 **보기**, **메뉴**, **A**, **B**, **X**, **Y** 단추가 장착되어 있어 [게임 패드](gamepad-and-vibration.md)와 단추 구성이 흡사합니다. 이들 단추는 게임 플레이 명령을 지원하기 위한 것이 아니며 레이싱 휠 단추로 바로 액세스할 수 없습니다.

레이싱 휠은 UI 탐색 컨트롤러로서 탐색 명령의 [필수 집합](ui-navigation-controller.md#required-set)을 왼쪽 엄지스틱, D-패드, **보기**, **메뉴**, **A**, **B** 단추에 매핑합니다.

| 탐색 명령 | 레이싱 휠 입력 |
| ------------------:| ------------------ |
|                 위로 | D 패드 위로           |
|               아래로 | D 패드 아래로         |
|               왼쪽으로 | D 패드 왼쪽으로         |
|              오른쪽으로 | D 패드 오른쪽으로        |
|               보기 | 보기 버튼        |
|               메뉴 | 메뉴 버튼        |
|             수락 | A 버튼           |
|             취소 | B 버튼           |

또한 일부 레이싱 휠은 탐색 명령에 대한 [선택 집합](ui-navigation-controller.md#optional-set) 중 일부를 지원되는 다른 입력에 매핑할 수 있지만 명령 매핑은 장치마다 다를 수 있습니다. 그러한 명령의 지원도 고려해 볼 수 있으나 해당 명령이 게임 인터페이스의 탐색에 필수적이지 않아야 합니다.

| 탐색 명령 | 레이싱 휠 입력    |
| ------------------:| --------------------- |
|            한 페이지 위로 | _다양_              |
|          한 페이지 아래로 | _다양_              |
|          한 페이지 왼쪽으로 | _다양_              |
|         한 페이지 오른쪽으로 | _다양_              |
|          위로 스크롤 | _다양_              |
|        아래로 스크롤 | _다양_              |
|        왼쪽으로 스크롤 | _다양_              |
|       오른쪽으로 스크롤 | _다양_              |
|          컨텍스트 1 | X 버튼(_일반적_) |
|          컨텍스트 2 | Y 버튼(_일반적_) |
|          컨텍스트 3 | _다양_              |
|          컨텍스트 4 | _다양_              |

## <a name="detect-and-track-racing-wheels"></a>레이싱 휠 감지 및 추적

레이싱 휠의 감지 및 추적은 [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad) 클래스 대신 [RacingWheel][] 클래스를 제외하고 게임 패드에서와 똑같은 방식으로 작동합니다. 자세한 내용은 [게임 패드 및 진동](gamepad-and-vibration.md)을 참조하세요.

<!-- Racing wheels are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected racing wheels and events to notify you when a racing wheel is added or removed.

### The racing wheels list

The [RacingWheel][] class provides a static property, [RacingWheels][], which is a read-only list of racing wheels that are currently connected. Because you might only be interested in some of the connected racing wheels, it's recommended that you maintain your own collection instead of accessing them through the `RacingWheels` property.

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

When a racing wheel is added or removed the [RacingWheelAdded][] and [RacingWheelRemoved][] events are raised. You can register handlers for these events to keep track of the racing wheels that are currently connected.

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

## <a name="reading-the-racing-wheel"></a>레이싱 휠 읽기

관심 있는 레이싱 휠을 식별하면 장치의 입력을 수집할 준비가 된 것입니다. 하지만 기존에 사용하던 다른 입력 유형과 달리 레이싱 휠은 이벤트 발생을 통해 상태 변경을 알리지 않습니다. 대신, 플라이트 스틱을 직접 _폴링_하여 현재 상태의 규칙적인 판독값을 가져올 수 있습니다.

### <a name="polling-the-racing-wheel"></a>레이싱 휠 폴링

폴링은 정확한 시점에 레이싱 휠의 스냅숏을 캡처합니다. 이러한 입력 수집 방법은 대부분의 게임에 적합합니다. 게임의 논리는 일반적으로 이벤트 기반 방식이 아닌 결정적 루프로 실행되기 때문입니다. 또한 시간을 두고 수집된 여러 단일 입력보다 한 번에 수집된 입력에서 게임 명령을 해석하는 것이 일반적으로 더 간단합니다.

레이싱 휠은 [GetCurrentReading][]을 호출하여 폴링합니다. 이 함수는 레이싱 휠의 상태가 포함된 [RacingWheelReading][]을 반환합니다.

다음 예제에서는 레이싱 휠의 현재 상태에 대해 레이싱 휠을 폴링합니다.

```cpp
auto racingwheel = myRacingWheels[0];

RacingWheelReading reading = racingwheel->GetCurrentReading();
```

레이싱 휠 상태 외에도 각 판독값에는 상태가 검색된 시기를 정확히 나타내는 타임스탬프가 포함되어 있습니다. 타임스탬프는 이전 판독값의 타이밍 또는 게임 시뮬레이션의 타이밍을 이해하는 데 유용합니다.

### <a name="determining-racing-wheel-capabilities"></a>레이싱 휠 기능 확인

레이싱 휠 컨트롤 중 다수가 옵션이거나 필수 컨트롤에서 조차 다양한 변형을 지원하므로, 레이싱 휠의 각 판독값에서 수집된 입력을 처리하기 전에 각 레이싱 휠의 기능을 개별적으로 확인해야 합니다.

옵션 컨트롤에는 핸드브레이크, 클러치, 패턴 변환기가 있습니다. 레이싱 휠의 [HasHandbrake][], [HasClutch][], [HasPatternShifter][] 속성을 각각 읽어 연결된 레이싱 휠이 이러한 컨트롤을 지원하는지 여부를 확인할 수 있습니다. 속성 값이 **true**인 경우 컨트롤이 지원되며, 그렇지 않은 경우 지원되지 않습니다.

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

또한 조종 핸들과 패턴 변환기의 컨트롤도 달라질 수 있습니다. 조종 핸들은 실제 바퀴가 지원하는 물리적 회전 정도에 따라, 패턴 변환기는 지원하는 전진 기어의 수에 따라 달라집니다. 레이싱 휠의 `MaxWheelAngle` 속성을 읽어 실제 핸들이 지원하는 최대 회전 각도를 확인할 수 있습니다. 그 값은 시계 방향(플러스 각도)으로 지원되는 물리적 최대 각도이자 동일하게 반시계 방향(마이너스 각도)으로 지원되는 최대 각도를 나타냅니다. 레이싱 휠의 `MaxPatternShifterGear` 속성을 읽어 패턴 변환기에서 지원하는 전진 기어 최고 단계를 확인할 수 있습니다. 그 값은 전진 기어 최고 변속 단계를 나타내므로 값이 4이면 해당 패턴 변환기는 후진, 중립, 1단, 2단, 3단, 4단 기어를 지원하는 것입니다.

```cpp
auto maxWheelDegrees = racingwheel->MaxWheelAngle;
auto maxShifterGears = racingwheel->MaxPatternShifterGear;
```

마지막으로 일부 레이싱 휠의 경우 조종 핸들을 통해 힘 피드백을 지원합니다. 레이싱 휠의 [WheelMotor][] 속성을 읽어 연결된 레이싱 휠의 힘 피드백 지원 여부를 확인할 수 있습니다. 힘 피드백은 `WheelMotor`가 **null**이 아닌 경우 지원되며, 그렇지 않은 경우 지원되지 않습니다.

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    // force feedback is supported
}
```

레이싱 휠이 지원하는 힘 피드백 기능을 사용하는 방법에 대해 자세한 내용은 [힘 피드백 개요](#force-feedback-overview)를 참조하세요.

### <a name="reading-the-buttons"></a>버튼 읽기

각 레이싱 휠 버튼, 즉 4방향 D-패드, **이전 기어** 및 **다음 기어** 버튼, 16개의 추가 버튼은 버튼이 눌렸는지(아래쪽), 놓였는지(위쪽)를 나타내는 디지털 판독값을 제공합니다. 버튼 판독값은 효율을 위해 개별 부울 값으로 표현되지 않는 대신 [RacingWheelButtons][] 열거로 표현되는 단일 비트 필드로 모두 압축됩니다.

> [!NOTE]
> 레이싱 휠에는 **보기** 및 **메뉴** 버튼과 같이 UI 탐색에 사용되는 여러 추가 버튼이 설치되어 있습니다. 이러한 버튼은 `RacingWheelButtons` 열거에 포함되지 않으며 UI 탐색 장치 역할을 하는 레이싱 휠에 액세스해야만 읽을 수 있습니다. 자세한 내용은 [UI 탐색 장치](ui-navigation-controller.md)를 참조하세요.

버튼 값은 [RacingWheelReading][] 구조의 `Buttons` 속성에서 읽어들입니다. 이러한 속성은 비트 필드이므로 해당 버튼 값을 격리하기 위해 비트 마스킹이 사용됩니다. 해당 비트가 설정된 경우에는 버튼이 눌리고(아래), 그렇지 않은 경우에는 놓입니다(위).

다음 예제에서는 **다음 기어** 버튼이 눌렸는지 확인합니다.

```cpp
if (RacingWheelButtons::NextGear == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is pressed
}
```

다음 예제에서는 다음 기어 버튼이 놓였는지 확인합니다.

```cpp
if (RacingWheelButtons::None == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is released (not pressed)
}
```

버튼이 눌렸다가 놓이거나, 반대로 놓였다가 눌렸을 때 여러 개의 버튼이 눌렸거나 놓였는지 또는 일련의 버튼이 특정 방식으로 정렬되었는지(일부는 눌리고 일부는 놓임) 확인해야 하는 경우가 있습니다. 이러한 상태를 검색하는 방법에 대한 자세한 내용은 [버튼 전환 검색](input-practices-for-games.md#detecting-button-transitions) 및 [복잡한 버튼 정렬 검색](input-practices-for-games.md#detecting-complex-button-arrangements)을 참조하세요.

### <a name="reading-the-wheel"></a>핸들 읽기

조종 핸들은 -1.0에서 +1.0 사이의 아날로그 판독값을 제공하는 필수 컨트롤입니다. 값이 -1.0이면 핸들의 가장 왼쪽 위치, 값이 +1.0이면 핸들의 가장 오른쪽 위치에 해당합니다. 조종 핸들의 값은 [RacingWheelReading][] 구조의 `Wheel` 속성에서 읽어들입니다.

```cpp
float wheel = reading.Wheel;  // returns a value between -1.0 and +1.0.
```

핸들 판독값은 레이싱 휠이 물리적으로 지원하는 회전 범위에 따라 다양하게 나타나는 실제 핸들의 물리적 회전 정도를 말하는데, 핸들 판독값의 크기를 조정하지 않는 것이 일반적입니다. 회전 각도가 클수록 정밀도가 탁월하기 때문입니다.

### <a name="reading-the-throttle-and-brake"></a>스로틀 및 브레이크 읽기

스로틀과 브레이크는 각기 부동 소수점 값으로 표현된 0.0(완전히 놓음)~1.0(완전히 눌림) 사이의 아날로그 판독값을 제공하는 필수 컨트롤입니다. 스로틀 컨트롤의 값은 [RacingWheelReading][] 구조의 `Throttle` 속성에서, 브레이크 컨트롤의 값은 `Brake` 속성에서 읽어들입니다.

```cpp
float throttle = reading.Throttle;  // returns a value between 0.0 and 1.0
float brake    = reading.Brake;     // returns a value between 0.0 and 1.0
```

### <a name="reading-the-handbrake-and-clutch"></a>핸드브레이크 및 클러치 읽기

핸드 브레이크와 클러치는 각기 부동 소수점 값으로 표현된 0.0(완전히 놓음)~1.0(완전히 눌림) 사이의 아날로그 판독값을 제공하는 옵션 컨트롤입니다. 핸드브레이크 컨트롤의 값은 [RacingWheelReading][] 구조의 `Handbrake` 속성에서, 클러치 컨트롤의 값은 `Clutch` 속성에서 읽어들입니다.

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

### <a name="reading-the-pattern-shifter"></a>패턴 변환기 읽기

패턴 변환기는 부호 있는 정수 값으로 표시된 -1부터 [MaxPatternShifterGear][] 사이의 디지털 판독값을 제공하는 옵션 컨트롤입니다. 값이 -1 또는 0이면 각각 _후진_ 기어와 _중립_ 기어에 해당합니다. 값이 플러스 쪽으로 증가할수록 전진 기어 변속 단계가 최대 [MaxPatternShifterGear][]까지 커집니다. 패턴 변환기 값은 [RacingWheelReading][] 구조의 `PatternShifterGear` 속성에서 읽어들입니다.

```cpp
if (racingwheel->HasPatternShifter)
{
    gear = reading.PatternShifterGear;
}
```

> [!NOTE]
> 패턴 변환기가 지원되는 경우 그 위치는 필수 버튼인 **이전 기어** 및 **다음 기어** 버튼과 함께 배치되는데, 이러한 버튼 역시 플레이어 자동차의 현재 기어 상태에 영향을 미칩니다. 두 종류 모두 배치되어 있을 경우 이들 입력을 간단하게 통합하려면, 자동 변속기 선택 시 패턴 변환기(및 클러치)를 무시하고 수동 변속기 선택 시 **이전** 및 **다음 기어** 버튼을 무시하면 됩니다(레이싱 휠에 패턴 변환기 컨트롤이 설치되어 있는 자동차의 경우에만 해당). 이러한 방법이 사용자의 게임에 적합하지 않을 경우 다른 통합 방법을 써 볼 수도 있습니다.

## <a name="run-the-inputinterfacing-sample"></a>InputInterfacing 샘플 실행

[InputInterfacingUWP 샘플 _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP)에서는 레이싱 휠과 여러 종류의 입력 장치를 동시에 사용하는 방법과 이러한 입력 장치가 UI 탐색 컨트롤러로 동작하는 방법을 보여 줍니다.

## <a name="force-feedback-overview"></a>힘 피드백 개요

수많은 레이싱 휠이 힘 피드백 기능을 갖추고 있어 더욱 높은 몰입감과 도전 의지가 샘솟는 주행 경험을 제공합니다. 힘 피드백을 지원하는 레이싱 휠에는 일반적으로 단일 축, 즉 핸들 회전축을 따라 조종 핸들에 힘을 적용하는 단일 모터가 장착되어 있습니다. 힘 피드백은 Windows 10 및 Xbox One UWP 앱에서 [Windows.Gaming.Input.ForceFeedback][] 네임스페이스로 지원됩니다.

> [!NOTE]
> 힘 피드백 API는 힘을 받는 축을 여러 개 지원할 수 있지만 Xbox One 레이싱 휠은 현재 핸들 회전축 이외의 피드백 축을 지원하지 않습니다.

## <a name="using-force-feedback"></a>힘 피드백 사용

이 섹션에서는 Xbox One 레이싱 휠에 대한 힘 피드백 효과 프로그래밍의 기본 사항에 대해 설명합니다. 힘 피드백 장치에 처음 로드된 효과를 사용하여 피드백을 적용한 다음 소리 효과와 비슷한 방식으로 시작, 일시 중지, 다시 시작, 중지 작업을 수행할 수 있습니다. 그러나 먼저 레이싱 휠의 피드백 기능부터 확인해야 합니다.

### <a name="determining-force-feedback-capabilities"></a>힘 피드백 기능 확인

레이싱 휠의 [WheelMotor][] 속성을 읽어 연결된 레이싱 휠의 힘 피드백 지원 여부를 확인할 수 있습니다. `WheelMotor`가 **null**인 경우 힘 피드백이 지원되지 않습니다. 그렇지 않은 경우 힘 피드백이 지원되므로 모터의 특정 피드백 기능 확인 작업을 계속할 수 있습니다(예: 모터의 영향을 받는 축).

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

### <a name="loading-force-feedback-effects"></a>힘 피드백 효과 로딩

힘 피드백 효과는 피드백 장치에 로드되며 여기에서 게임의 명령에 자동으로 "재생"됩니다. 제공되는 기본 효과는 다양합니다. 사용자 지정 효과는 [IForceFeedbackEffect][] 인터페이스를 구현하는 클래스를 통해 만들 수 있습니다.

| 효과 클래스         | 효과 설명                                                                     |
| -------------------- | -------------------------------------------------------------------------------------- |
| ConditionForceEffect | 장치 내의 전류 센서에 반응하여 일정하지 않은 힘을 적용하는 효과입니다. |
| ConstantForceEffect  | 일정한 힘을 벡터에 따라 적용하는 효과입니다.                                  |
| PeriodicForceEffect  | 파형으로 정의된 일정하지 않은 힘을 벡터에 따라 적용하는 효과입니다.           |
| RampForceEffect      | 선형으로 증가/감소하는 힘을 벡터에 따라 적용하는 효과입니다.          |

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

### <a name="using-force-feedback-effects"></a>힘 피드백 효과 사용

일단 로드된 후에는 레이싱 휠의 `WheelMotor` 속성에서 함수를 호출하거나 피드백 효과 자체에서 함수를 호출하여 여러 효과를 모두 동기적으로 시작, 일시 중지, 다시 시작 및 중지할 수 있습니다. 일반적으로 게임 플레이가 시작되기 전에 피드백 장치에서 사용하고자 하는 모든 효과를 로드한 다음 각각의 `SetParameters` 함수를 사용하여 게임 플레이를 진행하면서 효과를 업데이트해야 합니다.

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

마지막으로 필요할 때마다 특정 레이싱 휠에서 힘 피드백 시스템 전체를 비동기적으로 사용 또는 사용 안 함, 초기화 상태로 전환할 수 있습니다.

## <a name="see-also"></a>참고 항목

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [게임 입력 시스템](input-practices-for-games.md)

[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[racingwheel]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.aspx
[racingwheels]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheels.aspx
[racingwheeladded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheeladded.aspx
[racingwheelremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheelremoved.aspx
[haspatternshifter]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.haspatternshifter.aspx
[hashandbrake]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.hashandbrake.aspx
[hasclutch]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.hasclutch.aspx
[maxpatternshiftergear]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.maxpatternshiftergear.aspx
[maxwheelangle]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.maxwheelangle.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.getcurrentreading.aspx
[wheelmotor]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.wheelmotor.aspx
[racingwheelreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheelreading.aspx
[racingwheelbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheelbuttons.aspx
