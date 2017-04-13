---
author: mithom
title: "아케이드 스틱"
description: "Windows.Gaming.Input 아케이드 스틱 API를 사용하여 아케이드 스틱을 검색하고 읽으세요."
ms.assetid: 2E52232F-3014-4C8C-B39D-FAC478BA3E01
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 게임, 아케이드 스틱, 입력"
ms.openlocfilehash: b0411dcf1fd75ec7dc31d29a39e95f5c26073953
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="arcade-stick"></a>아케이드 스틱

이 페이지에서는 UWP(유니버설 Windows 플랫폼)용 [Windows.Gaming.Input.ArcadeStick][arcadestick] 및 관련 API를 사용하여 Xbox One 아케이드 스틱용 프로그래밍의 기본 사항을 설명합니다.

이 페이지에서는 다음에 대해 알아봅니다.
* 연결된 아케이드 스틱 및 사용자 목록을 수집하는 방법
* 아케이드 스틱이 추가 또는 제거된 사실을 감지하는 방법
* 하나 이상의 아케이드 스틱에서 입력을 읽는 방법
* 아케이드 스틱이 탐색 장치로 동작하는 방법


## <a name="arcade-stick-overview"></a>아케이드 스틱 개요

아케이드 스틱은 스탠드형 아케이드 머신의 느낌을 재현하고 고정밀 디지털 컨트롤을 제공한다는 점에서 가치 있는 입력 장치입니다. 아케이드 스틱은 격투 및 기타 아케이드 스타일 게임에 적합한 입력 장치이며 디지털 전용 컨트롤과 함께 잘 작동하는 모든 게임에 적합합니다. 아케이드 스틱은 Windows 10 및 Xbox One UWP 앱에서 [Windows.Gaming.Input][] 네임스페이스를 통해 지원됩니다.

Xbox One 아케이드 스틱은 8방향 디지털 조이스틱, **동작** 버튼 6개, **특수** 버튼 2개를 탑재하고 있으며 아날로그 컨트롤 또는 진동을 지원하지 않는 디지털 전용 입력 장치입니다. Xbox One 아케이드 스틱에는 UI 탐색을 지원하는 데 사용되는 **보기** 및 **메뉴** 버튼도 탑재되어 있지만 게임 플레이 명령을 지원하기 위한 것은 아니며 조이스틱 버튼으로 즉시 액세스할 수 없습니다.

### <a name="ui-navigation"></a>UI 탐색

사용자 인터페이스 탐색에 매우 다양한 입력 장치를 지원해야 하는 부담을 덜어주고 게임과 장치 간에 일관성을 도모하기 위해 대부분의 _물리적_ 입력 장치는 [UI 탐색 컨트롤러](ui-navigation-controller.md)라고 하는 별도의 _논리적_ 입력 장치 역할도 동시에 수행합니다. UI 탐색 컨트롤러는 여러 입력 장치의 UI 탐색 명령에 대한 공통 용어 모음집을 제공합니다.

UI 탐색 컨트롤러 역할을 하는 아케이드 스틱은 탐색 명령의 [필수 집합](ui-navigation-controller.md#required-set)을 조이스틱 및 **보기**, **메뉴**, **동작 1** 및 **동작 2** 버튼에 매핑합니다.

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

아케이드 스틱은 시스템에서 관리하므로 생성하거나 초기화할 필요가 없습니다. 아케이드 스틱이 추가되거나 제거되면 시스템에서 연결된 아케이드 스틱 및 이벤트 목록을 표시하여 알려 줍니다.

### <a name="the-arcade-sticks-list"></a>아케이드 스틱 목록

[ArcadeStick][] 클래스는 정적 속성 [ArcadeSticks][]를 제공하는데, 이는 현재 연결된 아케이드 스틱의 읽기 전용 목록입니다. 개발자는 연결된 아케이드 스틱 중 일부에만 관심이 있기 때문에 `ArcadeSticks` 속성을 통해 액세스하는 대신 자체 컬렉션을 유지 관리하는 것이 좋습니다.

다음 예제에서는 연결된 모든 아케이드 스틱을 새 컬렉션에 복사합니다.
```cpp
auto myArcadeSticks = ref new Vector<ArcadeStick^>();

for (auto arcadestick : ArcadeStick::ArcadeSticks)
{
    // This code assumes that you're interested in all arcade sticks.
    myArcadeSticks->Append(arcadestick);
}
```

### <a name="adding-and-removing-arcade-sticks"></a>아케이드 스틱 추가 및 제거

아케이드 스틱이 추가되거나 제거되면 [ArcadeStickAdded][] 및 [ArcadeStickRemoved][] 이벤트가 발생합니다. 이러한 이벤트의 처리기를 등록하면 현재 연결된 아케이드 스틱을 추적할 수 있습니다.

다음 예제에서는 추가된 아케이드 스틱의 추적을 시작합니다.
```cpp
ArcadeStick::ArcadeStickAdded += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    // This code assumes that you're interested in all new arcade sticks.
    myArcadeSticks->Append(args);
}
```

다음 예제에서는 제거된 아케이드 스틱의 추적을 중지합니다.
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

### <a name="users-and-headsets"></a>사용자 및 헤드셋

각 아케이드 스틱을 사용자 계정과 연결하여 해당 ID를 게임 플레이에 연결할 수 있고, 음성 채팅 또는 게임 내 기능을 도와주는 헤드셋을 연결할 수 있습니다. 사용자 및 헤드셋 관련 작업에 대해 자세히 알아보려면 [사용자 및 장치 추적](input-practices-for-games.md#tracking-users-and-their-devices)과 [헤드셋](headset.md)을 참조하세요.


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

각 아케이드 스틱 버튼, 즉 4방향 조이스틱, **동작** 버튼 6개, **특수** 버튼 2개는 버튼이 눌렸는지(아래쪽), 놓였는지(위쪽)를 나타내는 디지털 판독값을 제공합니다. 버튼 판독값은 효율을 위해 개별 부울 값으로 표현되지 않는 대신 [ArcadeStickButtons][] 열거로 표현되는 단일 비트 필드로 모두 압축됩니다.

> **참고**    아케이드 스틱에는 UI 탐색에 사용되는 **보기** 및 **메뉴** 버튼과 같은 추가 버튼이 탑재되어 있습니다. 이러한 버튼은 `ArcadeStickButtons` 열거에 포함되지 않으며 UI 탐색 장치 역할을 하는 아케이드 스틱에 액세스해야만 읽을 수 있습니다. 자세한 내용은 [UI 탐색 장치](ui-navigation-controller.md)를 참조하세요.

버튼 값은 [ArcadeStickReading][] 구조의 `Buttons` 속성에서 읽어들입니다. 이러한 속성은 비트 필드이므로 해당 버튼의 값을 격리하기 위해 비트 마스킹이 사용됩니다. 해당 비트가 설정된 경우에는 버튼이 눌리고(아래쪽), 그렇지 않은 경우에는 놓입니다(위쪽).

다음 예제에서는 동작 1 버튼이 눌렸는지 확인합니다.
```cpp
if (ArcadeStickButtons::Action1 == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is pressed
}
```

다음 예제에서는 동작 1 버튼이 놓였는지 확인합니다.
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
[Windows.Gaming.Input.UINavigationController][]
[Windows.Gaming.Input.IGameController][]

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
