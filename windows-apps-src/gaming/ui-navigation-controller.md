---
author: eliotcowley
title: UI 탐색 컨트롤러
description: Windows.Gaming.Input UI 탐색 컨트롤러 API를 사용하여 UI 탐색을 위한 여러 가지 입력 장치를 검색하고 읽어보세요.
ms.assetid: 5A14926D-8C2E-4DE8-AAFB-BEEB9BFE91A5
ms.author: elcowle
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, ui, 탐색
ms.localizationpriority: medium
ms.openlocfilehash: a0ec2790f6dddf93959a8c826602c0ac622a7d23
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5665009"
---
# <a name="ui-navigation-controller"></a>UI 탐색 컨트롤러

이 페이지에서는 UWP(유니버설 Windows 플랫폼)용 [Windows.Gaming.Input.UINavigationController][uinavigationcontroller] 및 관련 API를 사용하여 UI 탐색 장치용 프로그래밍의 기본 사항을 설명합니다.

이 페이지에서는 다음에 대해 알아봅니다.
* 연결된 UI 탐색 장치 및 사용자 목록을 수집하는 방법
* 탐색 장치가 추가 또는 제거된 사실을 감지하는 방법
* 하나 이상의 UI 탐색 장치에서 입력을 읽는 방법
* 게임 패드 및 아케이드 스틱이 탐색 장치로 동작하는 방법

## <a name="ui-navigation-controller-overview"></a>UI 탐색 컨트롤러 개요

거의 모든 게임에는 프리게임(pregame) 메뉴나 게임 내 대화와 같은 게임 플레이와 별개의 사용자 인터페이스가 있습니다. 플레이어는 어떤 입력 장치를 선택했든 해당 장치를 사용하여 이러한 UI를 탐색할 수 있어야 합니다. 하지만 개발자 입장에서는 각 입력 장치 유형별 지원 기능을 추가하는 것이 부담되고, 게임과 입력 장치 간에 불일치가 발생하여 플레이어에게 혼동을 줄 수도 있습니다. 이러한 이유로 [UINavigationController][] API가 만들어졌습니다.

UI 탐색 컨트롤러는 다양한 _물리적_ 입력 장치에서 지원할 수 있는 일반적인 UI 탐색 명령의 용어 모음집을 제공하기 위해 존재하는 _논리적_ 입력 장치입니다. _UI 탐색 컨트롤러_는 물리적 입력 장치를 다르게 볼 수 있는 방법일 뿐입니다. _탐색 장치_는 탐색 컨트롤러로 보이는 모든 물리적 입력 장치를 가리킵니다. 개발자는 특정 입력 장치가 아닌 탐색 장치를 대상으로 프로그래밍하여 여러 입력 장치를 지원해야 하는 부담을 없애고 기본적으로 일관성을 구현할 수 있습니다.

각 입력 장치 유형이 지원하는 컨트롤의 수와 다양성은 매우 다를 수 있고, 보다 풍부한 탐색 명령 집합을 지원해야 하는 특정 입력 장치도 있기 때문에 탐색 컨트롤러 인터페이스는 명령의 용어 모음집을 가장 일반적이고 필수적인 명령이 포함된 _필수 집합_과 편리하지만 필수적이지는 않은 명령이 포함된 _선택 집합_으로 나눕니다. 모든 탐색 장치는 _필수 집합_의 모든 명령을 지원하며 _선택 집합_의 명령을 전부, 일부 지원하거나 지원하지 않을 수 있습니다.

### <a name="required-set"></a>필수 집합

탐색 장치는 _필수 집합_의 모든 탐색 명령, 즉 방향(위쪽, 아래쪽, 왼쪽, 오른쪽), 보기, 메뉴, 수락 및 취소 명령을 지원해야 합니다.

방향 명령은 단일 UI 요소 간 기본 [XY 포커스 탐색](../design/devices/designing-for-tv.md#xy-focus-navigation-and-interaction)을 위한 것입니다. 보기 및 메뉴 명령은 게임 플레이 정보(주로 일시적, 가끔 형식적)를 표시하고 각각 게임 플레이와 메뉴 컨텍스트 간에 전환하기 위한 것입니다. 수락 및 취소 명령은 각각 긍정(예) 및 부정(아니요) 응답을 위한 것입니다.

다음 표에는 이러한 명령과 올바른 사용법이 예제와 함께 요약되어 있습니다.
| 명령 | 올바른 사용법
| -------:| ---------------
|      위쪽 | XY 포커스 탐색 위쪽
|    아래쪽 | XY 포커스 탐색 아래쪽
|    왼쪽 | XY 포커스 탐색 왼쪽
|   오른쪽 | XY 포커스 탐색 오른쪽
|    보기 | 게임 플레이 정보 표시 _(점수판, 게임 통계, 목표, 세계 또는 지역 지도)_
|    메뉴 | 기본 메뉴/일시 중지 _(설정, 상태, 장비, 인벤토리, 일시 중지)_
|  수락 | 긍정적인 응답 _(수락, 다음, 확인, 시작, 예)_
|  취소 | 부정적인 응답 _(거부, 뒤로, 거절, 중지, 아니요)_


### <a name="optional-set"></a>선택 집합

탐색 장치는 _선택 집합_의 탐색 명령, 즉 페이지 이동(위쪽, 아래쪽, 왼쪽, 오른쪽), 스크롤 이동(위쪽, 아래쪽, 왼쪽, 오른쪽), 상황별(컨텍스트 1-4) 명령을 전부, 일부 지원하거나 지원하지 않을 수도 있습니다.

상황별 명령은 명시적으로 응용 프로그램별 명령과 탐색 바로 가기를 위한 것입니다. 페이지 이동 및 스크롤 이동 명령은 UI 요소의 페이지 또는 그룹 간 빠른 탐색 및 UI 요소 내 세부적인 탐색을 위한 것입니다.

다음 표에는 이러한 명령과 올바른 사용법이 요약되어 있습니다.
|     명령 | 올바른 사용법
| -----------:| ------------
|      PageUp | 위로 이동(위쪽/이전 세로 페이지 또는 그룹으로 이동)
|    PageDown | 아래로 이동(아래쪽/다음 세로 페이지 또는 그룹으로 이동)
|    PageLeft | 왼쪽으로 이동(왼쪽/이전 가로 페이지 또는 그룹으로 이동)
|   PageRight | 오른쪽으로 이동(오른쪽/다음 가로 페이지 또는 그룹으로 이동)
|    ScrollUp | 위로 스크롤(포커스가 있는 UI 요소 또는 스크롤 가능한 그룹 내)
|  ScrollDown | 아래로 스크롤(포커스가 있는 UI 요소 또는 스크롤 가능한 그룹 내)
|  ScrollLeft | 왼쪽으로 스크롤(포커스가 있는 UI 요소 또는 스크롤 가능한 그룹 내)
| ScrollRight | 오른쪽으로 스크롤(포커스가 있는 UI 요소 또는 스크롤 가능한 그룹 내)
|    Context1 | 첫 번째 컨텍스트 동작
|    Context2 | 두 번째 컨텍스트 동작
|    Context3 | 세 번째 컨텍스트 동작
|    Context4 | 네 번째 컨텍스트 동작

> **참고**    게임은 올바른 사용법과 다른 실제 함수를 가진 모든 명령에 자유롭게 반응하지만 예기치 않은 동작은 피해야 합니다. 특히 명령의 올바른 사용법이 필요한 경우 명령의 실제 함수를 변경하지 말고, 명령에 가장 적절한 새 함수를 할당하고, PageUp/PageDown과 같은 대응 명령에 대응 함수를 할당하세요. 마지막으로, 각 입력 유형에서 지원되는 명령과 명령이 매핑되는 컨트롤을 고려하여 모든 장치에서 중요한 명령에 액세스할 수 있도록 합니다.

## <a name="gamepad-arcade-stick-and-racing-wheel-navigation"></a>게임 패드, 아케이드 스틱 및 레이싱 휠 탐색

Windows.Gaming.Input 네임스페이스에서 지원하는 모든 입력 장치는 UI 탐색 장치입니다.

다음 표에는 탐색 명령의 _필수 집합_이 다양한 입력 장치에 매핑되는 방식이 요약되어 있습니다.

| 탐색 명령 | 게임 패드 입력                       | 아케이드 스틱 입력 | 레이싱 휠 입력 |
| ------------------:| ----------------------------------- | ------------------ | ------------------ |
|                 위쪽 | 왼쪽 섬스틱(thumbstick) 위쪽/D 패드 위쪽       | 스틱 위쪽           | D 패드 위쪽           |
|               아래쪽 | 왼쪽 섬스틱(thumbstick) 아래쪽/D 패드 아래쪽   | 스틱 아래쪽         | D 패드 아래쪽         |
|               왼쪽 | 왼쪽 섬스틱(thumbstick) 왼쪽/D 패드 왼쪽   | 스틱 왼쪽         | D 패드 왼쪽         |
|              오른쪽 | 왼쪽 섬스틱(thumbstick) 오른쪽/D 패드 오른쪽 | 스틱 오른쪽        | D 패드 오른쪽        |
|               보기 | 보기 버튼                         | 보기 버튼        | 보기 버튼        |
|               메뉴 | 메뉴 버튼                         | 메뉴 버튼        | 메뉴 버튼        |
|             수락 | A 버튼                            | 동작 1 버튼    | A 버튼           |
|             취소 | B 버튼                            | 동작 2 버튼    | B 버튼           |

다음 표에는 탐색 명령의 _선택 집합_이 다양한 입력 장치에 매핑되는 방식이 요약되어 있습니다.

| 탐색 명령 | 게임 패드 입력          | 아케이드 스틱 입력 | 레이싱 휠 입력    |
| ------------------:| ---------------------- | ------------------ | --------------------- |
|             PageUp | 왼쪽 트리거           | _지원되지 않음_    | _다양_              |
|           PageDown | 오른쪽 트리거          | _지원되지 않음_    | _다양_              |
|           PageLeft | 왼쪽 범퍼            | _지원되지 않음_    | _다양_              |
|          PageRight | 오른쪽 범퍼           | _지원되지 않음_    | _다양_              |
|           ScrollUp | 오른쪽 섬스틱(thumbstick) 위쪽    | _지원되지 않음_    | _다양_              |
|         ScrollDown | 오른쪽 섬스틱(thumbstick) 아래쪽  | _지원되지 않음_    | _다양_              |
|         ScrollLeft | 오른쪽 섬스틱(thumbstick) 왼쪽  | _지원되지 않음_    | _다양_              |
|        ScrollRight | 오른쪽 섬스틱(thumbstick) 오른쪽 | _지원되지 않음_    | _다양_              |
|           Context1 | X 버튼               | _지원되지 않음_    | X 버튼(_일반적_) |
|           Context2 | Y 버튼               | _지원되지 않음_    | Y 버튼(_일반적_) |
|           Context3 | 왼쪽 섬스틱(thumbstick) 누름  | _지원되지 않음_    | _다양_              |
|           Context4 | 오른쪽 섬스틱(thumbstick) 누름 | _지원되지 않음_    | _다양_              |


## <a name="detect-and-track-ui-navigation-controllers"></a>UI 탐색 컨트롤러 검색 및 추적

UI 탐색 컨트롤러는 논리적 입력 장치이지만 물리적 장치를 표현한 것이며 시스템에서 동일한 방식으로 관리합니다. UI 탐색 컨트롤러가 추가되거나 제거되면 시스템에서 연결된 UI 탐색 컨트롤러 및 이벤트 목록을 표시하여 알려주므로 개발자가 생성하거나 초기화할 필요는 없습니다.

### <a name="the-ui-navigation-controllers-list"></a>UI 탐색 컨트롤러 목록

[UINavigationController][] 클래스는 정적 속성 [UINavigationControllers][]를 제공하는데, 이는 현재 연결된 UI 탐색 장치의 읽기 전용 목록입니다. 개발자는 연결된 탐색 장치 중 일부에만 관심이 있기 때문에 `UINavigationControllers` 속성을 통해 액세스하는 대신 자체 컬렉션을 유지 관리하는 것이 좋습니다.

다음 예제에서는 연결된 모든 UI 탐색 컨트롤러를 새 컬렉션에 복사합니다.
```cpp
auto myNavigationControllers = ref new Vector<UINavigationController^>();

for (auto device : UINavigationController::UINavigationControllers)
{
    // This code assumes that you're interested in all navigation controllers.
    myNavigationControllers->Append(device);
}
```

### <a name="adding-and-removing-ui-navigation-controllers"></a>UI 탐색 컨트롤러 추가 및 제거

UI 탐색 컨트롤러가 추가되거나 제거되면 [UINavigationControllerAdded][] 및 [UINavigationControllerRemoved][] 이벤트가 발생합니다. 이러한 이벤트의 이벤트 처리기를 등록하면 현재 연결된 탐색 장치를 추적할 수 있습니다.

다음 예제에서는 추가된 UI 탐색 장치의 추적을 시작합니다.
```cpp
UINavigationController::UINavigationControllerAdded += ref new EventHandler<UINavigationController^>(Platform::Object^, UINavigationController^ args)
{
    // This code assumes that you're interested in all new navigation controllers.
    myNavigationControllers->Append(args);
}
```

다음 예제에서는 제거된 아케이드 스틱의 추적을 중지합니다.
```cpp
UINavigationController::UINavigationControllerRemoved += ref new EventHandler<UINavigationController^>(Platform::Object^, UINavigationController^ args)
{
    unsigned int indexRemoved;

    if(myNavigationControllers->IndexOf(args, &indexRemoved))
    {
        myNavigationControllers->RemoveAt(indexRemoved);
    }
}
```

### <a name="users-and-headsets"></a>사용자 및 헤드셋

각 탐색 장치를 사용자 계정과 연결하여 해당 ID를 입력에 연결할 수 있고, 음성 채팅 또는 탐색 기능을 도와주는 헤드셋을 연결할 수 있습니다. 사용자 및 헤드셋 관련 작업에 대해 자세히 알아보려면 [사용자 및 장치 추적](input-practices-for-games.md#tracking-users-and-their-devices)과 [헤드셋](headset.md)을 참조하세요.

## <a name="reading-the-ui-navigation-controller"></a>UI 탐색 컨트롤러 읽기

관심 있는 UI 탐색 장치를 식별하면 장치의 입력을 수집할 준비가 된 것입니다. 하지만 기존에 사용하던 다른 입력 유형과 달리 탐색 장치는 이벤트 발생을 통해 상태 변경을 알리지 않습니다. 대신, 탐색 장치를 직접 _폴링_하여 현재 상태의 규칙적인 판독값을 가져올 수 있습니다.

### <a name="polling-the-ui-navigation-controller"></a>UI 탐색 컨트롤러 폴링

폴링은 정확한 시점에 탐색 장치의 스냅숏을 캡처합니다. 이러한 입력 수집 방법은 대부분의 게임에 적합합니다. 게임의 논리는 일반적으로 이벤트 기반 방식이 아닌 결정적 루프로 실행되기 때문입니다. 또한 시간을 두고 수집된 여러 단일 입력보다 한 번에 수집된 입력에서 게임 명령을 해석하는 것이 일반적으로 더 간단합니다.

탐색 장치는 [UINavigationController.GetCurrentReading][getcurrentreading]을 호출하여 폴링합니다. 이 함수는 탐색 장치의 상태가 포함된 [UINavigationReading][]을 반환합니다.

```cpp
auto navigationController = myNavigationControllers[0];

UINavigationReading reading = navigationController->GetCurrentReading();
```

### <a name="reading-the-buttons"></a>버튼 읽기

각 UI 탐색 버튼은 눌림(아래쪽) 또는 놓임(위쪽)에 해당하는 부울 판독값을 제공합니다. 버튼 판독값은 효율을 위해 개별 부울 값으로 표현되지 않는 대신 [RequiredUINavigationButtons][] 및 [OptionalUINavigationButtons][] 열거로 표현되는 두 개의 비트 필드 중 하나로 모두 압축됩니다.

_필수 집합_에 속하는 버튼은 [UINavigationReading][] 구조의 `RequiredButtons` 속성에서 읽어들이고, _선택 집합_에 속하는 버튼은 `OptionalButtons` 속성에서 읽어들입니다. 이러한 속성은 비트 필드이므로 해당 버튼의 값을 격리하기 위해 비트 마스킹이 사용됩니다. 해당 비트가 설정된 경우에는 버튼이 눌리고(아래쪽), 그렇지 않은 경우에는 놓입니다(위쪽).

다음 예제에서는 _필수 집합_의 수락 버튼이 눌렸는지 확인합니다.
```cpp
if (RequiredUINavigationButtons::Accept == (reading.RequiredButtons & RequiredUINavigationButtons::Accept))
{
    // Accept is pressed
}
```

다음 예제에서는 _필수 집합_의 수락 버튼이 놓였는지 확인합니다.
```cpp
if (RequiredUINavigationButtons::None == (reading.RequiredButtons & RequiredUINavigationButtons::Accept))
{
    // Accept is released (not pressed)
}
```

_선택 집합_의 버튼을 읽을 때는 `OptionalButtons` 속성 및 `OptionalUINavigationButtons` 열거를 사용하세요.

다음 예제에서는 _선택 집합_의 Context 1 버튼이 눌렸는지 확인합니다.
```cpp
if (OptionalUINavigationButtons::Context1 == (reading.OptionalButtons & OptionalUINavigationButtons::Context1))
{
    // Context 1 is pressed
}
```

버튼이 눌렸다가 놓이거나, 반대로 놓였다가 눌렸을 때 여러 개의 버튼이 눌렸거나 놓였는지 또는 일련의 버튼이 특정 방식으로 정렬되었는지(일부는 눌리고 일부는 놓임) 확인해야 하는 경우가 있습니다. 이러한 상태를 검색하는 방법에 대한 자세한 내용은 [버튼 전환 검색](input-practices-for-games.md#detecting-button-transitions) 및 [복잡한 버튼 정렬 검색](input-practices-for-games.md#detecting-complex-button-arrangements)을 참조하세요.


## <a name="run-the-ui-navigation-controller-sample"></a>UI 탐색 컨트롤러 샘플 실행

[InputInterfacingUWP 샘플 _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/InputInterfacingUWP)에서는 여러 입력 장치가 UI 탐색 컨트롤러로 동작하는 방법을 보여 줍니다.

## <a name="see-also"></a>참고 항목
[Windows.Gaming.Input.Gamepad][]
[Windows.Gaming.Input.ArcadeStick][]
[Windows.Gaming.Input.RacingWheel][]
[Windows.Gaming.Input.IGameController][]


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.Gamepad]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.aspx
[Windows.Gaming.Input.Arcadestick]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.aspx
[Windows.Gaming.Input.Racingwheel]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[uinavigationcontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[uinavigationcontrollers]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrollers.aspx
[uinavigationcontrolleradded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrolleradded.aspx
[uinavigationcontrollerremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrollerremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.getcurrentreading.aspx
[uinavigationreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationreading.aspx
[requireduinavigationbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.requireduinavigationbuttons.aspx
[optionaluinavigationbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.optionaluinavigationbuttons.aspx
