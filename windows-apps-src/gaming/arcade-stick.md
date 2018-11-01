---
author: eliotcowley
title: 아케이드 스틱
description: Windows.Gaming.Input 아케이드 스틱 API를 사용하여 아케이드 스틱을 검색하고 읽으세요.
ms.assetid: 2E52232F-3014-4C8C-B39D-FAC478BA3E01
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 아케이드 스틱, 입력
ms.localizationpriority: medium
ms.openlocfilehash: 13bc03559fb32156f5ff8bb29ed96f8a1e4ac84f
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5879838"
---
# <a name="arcade-stick"></a>아케이드 스틱

이 페이지에서는 UWP(유니버설 Windows 플랫폼)용 [Windows.Gaming.Input.ArcadeStick][arcadestick] 및 관련 API를 사용하여 Xbox One 아케이드 스틱용 프로그래밍의 기본 사항을 설명합니다.

이 페이지에서는 다음에 대해 알아봅니다.

* 연결된 아케이드 스틱 및 사용자 목록을 수집하는 방법
* 아케이드 스틱이 추가 또는 제거된 사실을 감지하는 방법
* 하나 이상의 아케이드 스틱에서 입력을 읽는 방법
* 아케이드 스틱이 UI 탐색 장치로 동작 하는 방법

## <a name="arcade-stick-overview"></a>아케이드 스틱 개요

아케이드 스틱은 스탠드형 아케이드 머신의 느낌을 재현하고 고정밀 디지털 컨트롤을 제공한다는 점에서 가치 있는 입력 장치입니다. 아케이드 스틱은 격투 및 기타 아케이드 스타일 게임에 적합한 입력 장치이며 디지털 전용 컨트롤과 함께 잘 작동하는 모든 게임에 적합합니다. 아케이드 스틱은 Windows 10 및 Xbox One UWP 앱에서 [Windows.Gaming.Input][] 네임스페이스를 통해 지원됩니다.

Xbox One 아케이드 스틱은 8 방향 디지털 조이스틱, 6 개의 **작업** 단추 (아래 이미지에서 A1 a 6으로 표시) 및 두 가지 **특별 한** 단추 (s 1과 S2로 표시 됨); 장착 되어 아날로그 컨트롤 또는 진동을 지원 하지 않는 디지털 전용 입력된 장치 일입니다. Xbox One 아케이드 스틱에 UI 탐색을 지 원하는 데 사용 되는 **보기** 및 **메뉴** 단추 장착 수도 있지만 게임 플레이 명령을 지원 하기 위해 아니며 조이스틱 버튼으로 즉시 액세스할 수 없습니다.

![아케이드 스틱 4 방향 조이스틱, 6 작업 단추 (A1-a 6) 및 2 특별 한 단추 (s 1과 S2)](images/arcade-stick-1.png)

### <a name="ui-navigation"></a>UI 탐색

사용자 인터페이스 탐색에 매우 다양한 입력 장치를 지원해야 하는 부담을 덜어주고 게임과 장치 간에 일관성을 도모하기 위해 대부분의 _물리적_ 입력 장치는 [UI 탐색 컨트롤러](ui-navigation-controller.md)라고 하는 별도의 _논리적_ 입력 장치 역할도 동시에 수행합니다. UI 탐색 컨트롤러는 여러 입력 장치의 UI 탐색 명령에 대한 공통 용어 모음집을 제공합니다.

UI 탐색 컨트롤러로 아케이드 스틱은 탐색 명령의 [필수 집합](ui-navigation-controller.md#required-set) 를 조이스틱 및 **보기**, **메뉴**, **동작 1**및 **2 작업** 단추에 매핑합니다.

| 탐색 명령 | 아케이드 스틱 입력  |
| ------------------:| ------------------- |
|                 위쪽 | 스틱 위쪽            |
|               아래쪽 | 스틱 아래쪽          |
|               왼쪽 | 스틱 왼쪽          |
|              오른쪽 | 스틱 오른쪽         |
|               보기 | 보기 버튼         |
|               메뉴 | 메뉴 버튼         |
|             수락 | 동작 1 버튼     |
|             취소 | 동작 2 버튼     |

아케이드 스틱은 탐색 명령의 [선택 집합](ui-navigation-controller.md#optional-set)을 매핑하지 않습니다.

## <a name="detect-and-track-arcade-sticks"></a>아케이드 스틱 검색 및 추적

검색 및 추적 아케이드 게임 패드를 제외 하 고와 [게임 패드](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad) 클래스 대신 [ArcadeStick][] 클래스와 동일한 방식으로 작동을 스틱 합니다. 자세한 내용은 [게임 패드 및 진동](gamepad-and-vibration.md)을 참조하세요.

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

관심 있는 아케이드 스틱을 식별하면 장치의 입력을 수집할 준비가 된 것입니다. 하지만 기존에 사용하던 다른 입력 유형과 달리 아케이드 스틱은 이벤트 발생을 통해 상태 변경을 알리지 않습니다. 대신, 아케이드 스틱을 직접 _폴링_하여 현재 상태의 규칙적인 판독값을 가져올 수 있습니다.

### <a name="polling-the-arcade-stick"></a>아케이드 스틱 폴링

폴링은 정확한 시점에 아케이드 스틱의 스냅숏을 캡처합니다. 이러한 입력 수집 방법은 대부분의 게임에 적합합니다. 게임의 논리는 일반적으로 이벤트 기반 방식이 아닌 결정적 루프로 실행되기 때문입니다. 또한 시간을 두고 수집된 여러 단일 입력보다 한 번에 수집된 입력에서 게임 명령을 해석하는 것이 일반적으로 더 간단합니다.

아케이드 스틱은 [GetCurrentReading][]을 호출하여 폴링합니다. 이 함수는 아케이드 스틱의 상태가 포함된 [ArcadeStickReading][]을 반환합니다.

다음 예제에서는 아케이드 스틱의 현재 상태를 폴링합니다.

```cpp
auto arcadestick = myArcadeSticks[0];

ArcadeStickReading reading = arcadestick->GetCurrentReading();
```

아케이드 스틱 상태 외에도 각 판독값에는 상태가 검색된 시기를 정확히 나타내는 타임스탬프가 포함되어 있습니다. 타임스탬프는 이전 판독값의 타이밍 또는 게임 시뮬레이션의 타이밍을 이해하는 데 유용합니다.

### <a name="reading-the-buttons"></a>버튼 읽기

각 아케이드 스틱 버튼&mdash;4 방향 조이스틱, 6 개의 **작업** 단추 및 두 가지 **특별 한** 단추&mdash;누름 (아래) 또는 놓음 (위)에 있는지 여부를 나타내는 디지털 판독값을 제공 합니다. 버튼 판독값은 효율을 위해 개별 부울 값으로 표현 되지 않는 대신, 모두 압축 [ArcadeStickButtons][] 열거로 표현 되는 단일 비트 필드로 합니다.

> [!NOTE]
> 아케이드 스틱에 **보기** 및 **메뉴** 단추와 같은 UI 탐색에 사용 되는 추가 버튼이 탑재 되어 있습니다. 이러한 버튼은 `ArcadeStickButtons` 열거에 포함되지 않으며 UI 탐색 장치 역할을 하는 아케이드 스틱에 액세스해야만 읽을 수 있습니다. 자세한 내용은 [UI 탐색 장치](ui-navigation-controller.md)를 참조하세요.

버튼 값은 [ArcadeStickReading][] 구조의 `Buttons` 속성에서 읽어들입니다. 이러한 속성은 비트 필드이므로 해당 버튼 값을 격리하기 위해 비트 마스킹이 사용됩니다. 해당 비트가 설정된 경우에는 버튼이 눌리고(아래), 그렇지 않은 경우에는 놓입니다(위).

다음 예제에서는 **동작** 1 버튼이 눌 렸 지를 결정 합니다.

```cpp
if (ArcadeStickButtons::Action1 == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is pressed
}
```

다음 예제에서는 **동작** 1 버튼이 놓였는지 확인 합니다.

```cpp
if (ArcadeStickButtons::None == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is released (not pressed)
}
```

버튼이 눌렸다가 놓이거나, 반대로 놓였다가 눌렸을 때 여러 개의 버튼이 눌렸거나 놓였는지 또는 일련의 버튼이 특정 방식으로 정렬되었는지(일부는 눌리고 일부는 놓임) 확인해야 하는 경우가 있습니다. 이러한 상태를 검색하는 방법에 대한 자세한 내용은 [버튼 전환 검색](input-practices-for-games.md#detecting-button-transitions) 및 [복잡한 버튼 정렬 검색](input-practices-for-games.md#detecting-complex-button-arrangements)을 참조하세요.

## <a name="run-the-inputinterfacing-sample"></a>InputInterfacing 샘플 실행

[InputInterfacingUWP 샘플 _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP)에서는 아케이드 스틱 및 여러 종류의 입력 장치를 동시에 사용하는 방법과 이러한 입력 장치가 UI 탐색 컨트롤러로 동작하는 방법을 보여 줍니다.

## <a name="see-also"></a>참고 항목

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [게임 입력 시스템](input-practices-for-games.md)

[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[arcadestick]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.aspx
[arcadesticks]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadesticks.aspx
[arcadestickadded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadestickadded.aspx
[arcadestickremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadestickremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.getcurrentreading.aspx
[arcadestickreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestickreading.aspx
[arcadestickbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestickbuttons.aspx
