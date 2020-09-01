---
title: 아케이드 스틱
description: Windows.Gaming.Input 아케이드 스틱 API를 사용하여 아케이드 스틱을 검색하고 읽으세요.
ms.assetid: 2E52232F-3014-4C8C-B39D-FAC478BA3E01
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 아케이드 스틱, 입력
ms.localizationpriority: medium
ms.openlocfilehash: e9a9a2b3622169b99a1b3a0c3607329c85c5785f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159417"
---
# <a name="arcade-stick"></a>아케이드 스틱

이 페이지에서는 [ArcadeStick][ArcadeStick] 를 사용 하는 Xbox one 아케이드 스틱 프로그래밍의 기본 사항을 설명 하 고 UWP (유니버설 Windows 플랫폼) 관련 api를 설명 합니다.

이 페이지를 읽으면 다음에 대해 알아봅니다.

* 연결 된 아케이드 스틱 및 해당 사용자 목록을 수집 하는 방법
* 아케이드 스틱 추가 또는 제거를 검색 하는 방법
* 하나 이상의 아케이드 스틱에서 입력을 읽는 방법
* 아케이드 스틱이 UI 탐색 장치로 작동 하는 방식

## <a name="arcade-stick-overview"></a>아케이드 스틱 개요

아케이드 스틱은 여는 아케이드 컴퓨터의 분위기를 재현 하 고 높은 정밀도 디지털 컨트롤에 대 한 입력 장치 값입니다. 아케이드 스틱은 헤드 및 기타 아케이드 스타일 게임을 위한 완벽 한 입력 장치 이며 모든 디지털 컨트롤에서 잘 작동 하는 게임에 적합 합니다. 아케이드 스틱 [은 windows 10][] 및 XBOX one UWP 앱에서 지원 됩니다.

Xbox One 아케이드 스틱에는 8 방향 디지털 조이스틱, 6 개의 **작업** 단추 (아래 이미지에서 A1-A6로 표시 됨) 및 두 개의 **특수** 단추 (S1 및 S2로 표시 됨)가 포함 되어 있습니다. 아날로그 컨트롤 또는 진동을 지원 하지 않는 모든 디지털 입력 장치입니다. Xbox One 아케이드 스틱에는 UI 탐색을 지 원하는 데 사용 되는 **보기** 및 **메뉴** 단추도 포함 되어 있지만 게임 플레이 명령을 지원 하기 위한 것이 아니며 조이스틱 단추로 쉽게 액세스할 수 없습니다.

![4 방향 조이스틱, 6 가지 작업 단추 (A1-A6) 및 2 개의 특수 단추 (S1 및 S2)를 포함 하는 아케이드 스틱](images/arcade-stick-1.png)

### <a name="ui-navigation"></a>UI 탐색

사용자 인터페이스 탐색을 위해 다양 한 입력 장치를 지원 하 고 게임과 장치 간에 일관성을 유지 하기 위해 대부분의 _물리적_ 입력 장치는 [UI 탐색 컨트롤러](ui-navigation-controller.md)라고 하는 별도의 _논리적_ 입력 장치로 동시에 작동 합니다. UI 탐색 컨트롤러는 입력 장치에서 UI 탐색 명령에 대 한 일반적인 어휘를 제공 합니다.

UI 탐색 컨트롤러인 아케이드 스틱은 [필요한](ui-navigation-controller.md#required-set) 탐색 명령 집합을 조이스틱이 나 **보기**, **메뉴**, **작업 1**및 **작업 2** 단추에 매핑합니다.

| 탐색 명령 | 아케이드 스틱 입력  |
| ------------------:| ------------------- |
|                 위로 | 스틱 위로            |
|               아래로 | 스틱 다운          |
|               왼쪽 | 왼쪽 스틱          |
|              오른쪽 | 오른쪽 스틱         |
|               보기 | 보기 단추         |
|               메뉴 | 메뉴 버튼         |
|             동의함 | 작업 1 단추     |
|             취소 | 작업 2 단추     |

아케이드 스틱은 선택적 탐색 명령 [집합](ui-navigation-controller.md#optional-set) 을 매핑하지 않습니다.

## <a name="detect-and-track-arcade-sticks"></a>아케이드 스틱 검색 및 추적

[ArcadeStick][] [클래스 대신](/uwp/api/Windows.Gaming.Input.Gamepad) gamepads를 사용 하는 경우를 제외 하 고 아케이드 스틱 검색 및 추적은 정확히 동일한 방법으로 작동 합니다. 자세한 내용은 [게임 패드 및 진동](gamepad-and-vibration.md) (영문)을 참조 하세요.

<!-- Arcade sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected arcades sticks and events to notify you when an arcade stick is added or removed.

### The arcade sticks list

The [ArcadeStick][] class provides a static property, [ArcadeSticks][], which is a read-only list of arcade sticks that are currently connected. Because you might only be interested in some of the connected arcade sticks, it's recommended that you maintain your own collection instead of accessing them through the `ArcadeSticks` property.

The following example copies all connected arcade sticks into a new collection. Note that because other threads in the background will be accessing this collection (in the [ArcadeStickAdded][] and [ArcadeStickRemoved][] events), you need to place a lock around any code that reads or updates the collection.

```cpp
auto myArcadeSticks = ref new Vector<ArcadeStick^>();
critical_section myLock{};

for (auto arcadeStick : ArcadeStick::ArcadeSticks)
{
    // Check if the arcade stick is already in myArcadeSticks; if it isn't, add
    // it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myArcadeSticks), end(myArcadeSticks), arcadeStick);

    if (it == end(myArcadeSticks))
    {
        // This code assumes that you're interested in all arcade sticks.
        myArcadeSticks->Append(arcadeStick);
    }
}
```

### Adding and removing arcade sticks

When an arcade stick is added or removed the [ArcadeStickAdded][] and [ArcadeStickRemoved][] events are raised. You can register handlers for these events to keep track of the arcade sticks that are currently connected.

The following example starts tracking an arcade stick that's been added.

```cpp
ArcadeStick::ArcadeStickAdded += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    // Check if the just-added arcade stick is already in myArcadeSticks; if it
    // isn't, add it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), args);

    // This code assumes that you're interested in all new arcade sticks.
    myArcadeSticks->Append(args);
}
```

The following example stops tracking an arcade stick that's been removed.

```cpp
ArcadeStick::ArcadeStickRemoved += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    unsigned int indexRemoved;

    if(myArcadeSticks->IndexOf(args, &indexRemoved))
    {
        myArcadeSticks->RemoveAt(indexRemoved);
    }
}
```

### Users and headsets

Each arcade stick can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-arcade-stick"></a>아케이드 스틱 읽기

관심이 있는 아케이드 스틱을 파악 한 후에는 여기에서 입력을 수집할 준비가 된 것입니다. 그러나 사용할 수 있는 다른 종류의 입력과 달리 아케이드 스틱에서는 이벤트를 발생 시켜 상태 변경을 전달 하지 않습니다. 대신, 현재 상태를 _폴링하여_ 정기적으로 사용 합니다.

### <a name="polling-the-arcade-stick"></a>아케이드 스틱 폴링

폴링은 정확한 시점에서 아케이드 스틱의 스냅숏을 캡처합니다. 입력 수집에 대 한이 방법은 일반적으로 이벤트 기반이 아니라 결정적 루프에서 실행 되기 때문에 대부분의 게임에 적합 합니다. 또한 시간이 지남에 따라 수집 된 여러 개의 단일 입력에서 가져온 것 보다 한 번에 모두 수집 된 입력에서 게임 명령을 해석 하는 것이 더 간단 합니다.

[GetCurrentReading][]를 호출 하 여 아케이드 스틱을 폴링합니다. 이 함수는 아케이드 스틱 상태를 포함 하는 [ArcadeStickReading][] 를 반환 합니다.

다음 예제에서는 현재 상태에 대 한 아케이드 스틱을 폴링합니다.

```cpp
auto arcadestick = myArcadeSticks[0];

ArcadeStickReading reading = arcadestick->GetCurrentReading();
```

아케이드 스틱 상태 외에도 각 읽기에는 상태가 검색 된 시기를 정확 하 게 나타내는 타임 스탬프가 포함 됩니다. 타임 스탬프는 이전 판독값의 타이밍과 게임 시뮬레이션의 타이밍과 관련 된 경우에 유용 합니다.

### <a name="reading-the-buttons"></a>단추 읽기

각 아케이드 스틱 단추는 &mdash; 조이스틱의 네 가지 방향, 6 개의 **작업** 단추 및 두 개의 **특수** 단추를 &mdash; 통해 눌린 (다운) 또는 릴리스 (up) 여부를 나타내는 디지털 읽기를 제공 합니다. 효율성을 위해 단추 판독값은 개별 부울 값으로 표시 되지 않습니다. 대신 [ArcadeStickButtons][] 열거형에 의해 표현 되는 단일 비트 필드에 모두 압축 됩니다.

> [!NOTE]
> 아케이드 스틱에는 **보기** 및 **메뉴** 단추와 같은 UI 탐색에 사용 되는 추가 단추가 있습니다. 이러한 단추는 열거형의 일부가 아니므로 `ArcadeStickButtons` UI 탐색 장치로 아케이드 스틱에 액세스 해야만 읽을 수 있습니다. 자세한 내용은 [UI 탐색 장치](ui-navigation-controller.md)를 참조 하세요.

`Buttons` [ArcadeStickReading][] 구조체의 속성에서 단추 값을 읽습니다. 이 속성은 비트 필드 이므로 관심이 있는 단추의 값을 격리 하기 위해 비트 마스킹을 사용 됩니다. 해당 비트가 설정 된 경우 단추가 눌러져 있습니다 (down). 그렇지 않으면 해제 됩니다 (up).

다음 예에서는 **작업 1** 단추를 눌렀는지 여부를 확인 합니다.

```cpp
if (ArcadeStickButtons::Action1 == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is pressed
}
```

다음 예에서는 **작업 1** 단추가 해제 되어 있는지 여부를 확인 합니다.

```cpp
if (ArcadeStickButtons::None == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is released (not pressed)
}
```

경우에 따라 단추를 눌렀다 놓은 상태에서 눌린 상태로 전환 하는 경우, 여러 단추를 눌렀는지 또는 눌렀다 놓았을 때 또는 단추 집합이 특정 방법으로 몇 번의 단추에 배치 되는지 여부를 확인 하는 것이 좋습니다 &mdash; . 이러한 조건을 검색 하는 방법에 대 한 자세한 내용은 [단추 전환 검색](input-practices-for-games.md#detecting-button-transitions) 및 [복잡 한 단추 배열 검색](input-practices-for-games.md#detecting-complex-button-arrangements)을 참조 하세요.

## <a name="run-the-inputinterfacing-sample"></a>InputInterfacing 샘플 실행

[InputInterfacingUWP 샘플 _(github)_ ](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) 에서는 아케이드 스틱 및 다양 한 종류의 입력 장치를 함께 사용 하는 방법 뿐만 아니라 이러한 입력 장치가 UI 탐색 컨트롤러로 동작 하는 방식을 보여 줍니다.

## <a name="see-also"></a>참고 항목

* [Windows.Gaming.Input.UINavigationController][]
* [IGameController.][]
* [게임용 입력 시스템](input-practices-for-games.md)

[Windows. 게임 입력]: /uwp/api/Windows.Gaming.Input
[IGameController.]: /uwp/api/Windows.Gaming.Input.IGameController
[Windows.Gaming.Input.UINavigationController]: /uwp/api/Windows.Gaming.Input.UINavigationController
[arcadestick]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[arcadesticks]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[arcadestickadded]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[arcadestickremoved]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[getcurrentreading]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[arcadestickreading]: /uwp/api/Windows.Gaming.Input.ArcadeStickReading
[arcadestickbuttons]: /uwp/api/Windows.Gaming.Input.ArcadeStickButtons