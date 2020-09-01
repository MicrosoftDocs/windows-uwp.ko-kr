---
title: UI 탐색 컨트롤러
description: UI 탐색을 위해 다양 한 종류의 입력 장치를 검색 하 고 읽으려면 Windows 게이밍 UI 탐색 컨트롤러 Api를 사용 합니다.
ms.assetid: 5A14926D-8C2E-4DE8-AAFB-BEEB9BFE91A5
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, ui, 탐색
ms.localizationpriority: medium
ms.openlocfilehash: 7cf5369bd01fbcf95c5af7bddc7055958cc50299
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159087"
---
# <a name="ui-navigation-controller"></a>UI 탐색 컨트롤러

이 페이지에서는 [UINavigationController][UINavigationController] 를 사용 하는 UI 탐색 장치 프로그래밍의 기본 사항과 UWP (유니버설 Windows 플랫폼) 관련 api에 대해 설명 합니다.

이 페이지를 읽으면 다음에 대해 알아봅니다.
* 연결 된 UI 탐색 장치 및 해당 사용자 목록을 수집 하는 방법
* 탐색 장치가 추가 또는 제거 되었는지 검색 하는 방법
* 하나 이상의 UI 탐색 장치에서 입력을 읽는 방법
* Gamepads 및 아케이드 스틱이 탐색 장치로 작동 하는 방식

## <a name="ui-navigation-controller-overview"></a>UI 탐색 컨트롤러 개요

거의 모든 게임에는 단순히 메뉴 또는 게임 내 대화 상자를 사용 하는 경우에도 게임 pregame 별도의 사용자 인터페이스가 하나 이상 있습니다. 플레이어는 선택한 입력 장치를 사용 하 여이 UI를 탐색할 수 있어야 하지만 개발자가 각 종류의 입력 장치에 대 한 특정 지원을 추가 하는 것을 부담, 플레이어를 혼동 하는 게임 및 입력 장치 간에 불일치를 일으킬 수도 있습니다. 이러한 이유로 [UINavigationController][] API를 만들었습니다.

UI 탐색 컨트롤러는 다양 한 _물리적_ 입력 장치에서 지원할 수 있는 일반적인 ui 탐색 명령의 어휘를 제공 하기 위해 존재 하는 _논리적_ 입력 장치입니다. _UI 탐색 컨트롤러_ 는 물리적 입력 장치를 보는 다른 방법입니다. 탐색 _장치_ 를 사용 하 여 탐색 컨트롤러로 표시 되는 모든 물리적 입력 장치를 참조 합니다. 개발자는 특정 입력 장치 대신 탐색 장치에 대해 프로그래밍 하 여 다양 한 입력 장치를 지원 하 고 기본적으로 일관성을 유지 하는 부담을 방지 합니다.

각 종류의 입력 장치에서 지원 되는 다양 한 컨트롤은 서로 다를 수 있으며 특정 입력 장치에서 다양 한 탐색 명령 집합을 지원할 수 있기 때문에, 탐색 컨트롤러 인터페이스는 명령의 어휘를 가장 일반적이 고 필수적인 명령을 포함 하는 _필수 집합이_ 나 편리 하지만 불필요 한 명령이 포함 된 _선택적 집합_ 으로 나눕니다. 모든 탐색 장치는 _필수 집합_ 의 모든 명령을 지원 하며 _선택적 집합_의 모든, 일부 또는 모든 명령을 지원할 수 있습니다.

### <a name="required-set"></a>필수 설정

탐색 장치는 _필요한 집합_의 모든 탐색 명령을 지원 해야 합니다. 이는 방향 (위쪽, 아래쪽, 왼쪽 및 오른쪽), 보기, 메뉴, 수락 및 취소 명령입니다.

방향 명령은 단일 UI 요소 간의 기본 [XY 포커스 탐색](../design/input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction) 을 위한 것입니다. 보기 및 메뉴 명령은 게임 전 정보를 표시 하는 데 사용 되며 (종종 일시적이 고 때때로 모달로) 게임 및 메뉴 컨텍스트 간을 전환 하는 데 사용 됩니다. Accept 및 cancel 명령은 각각 찬성 (예) 및 부정 (no) 응답을 위한 것입니다.

다음 표에서는 예제와 함께 이러한 명령 및 용도를 요약 합니다.
| 명령 | 올바른 사용법
| -------:| ---------------
|      위로 | XY 포커스 탐색
|    아래로 | XY 포커스 탐색
|    왼쪽 | XY 포커스 탐색 왼쪽
|   오른쪽 | XY 포커스 탐색 오른쪽
|    보기 | 게임 플레이 정보 _(스코어 보드, 게임 통계, 목표, 세계 또는 지역 지도)_ 를 표시 합니다.
|    메뉴 | 기본 메뉴/일시 중지  _(설정, 상태, 장비, 인벤토리, 일시 중지)_
|  동의함 | 찬성 response  _(수락, 진행, 확인, 시작, 예)_
|  취소 | 부정 응답     _(거부, 역방향, 거절, 중지, 아니요)_


### <a name="optional-set"></a>선택적 집합

또한 탐색 장치는 _선택적인 집합_에서 탐색 명령을 모두, 일부 또는 모두 지원 하지 않을 수 있습니다. 페이징 (위쪽, 아래쪽, 왼쪽 및 오른쪽), 스크롤 (위쪽, 아래쪽, 왼쪽 및 오른쪽) 및 상황별 (컨텍스트 1-4) 명령이 있습니다.

상황별 명령은 응용 프로그램별 명령과 탐색 바로 가기에 대해 명시적으로 제공 됩니다. 페이징 및 스크롤 명령은 각각 ui 요소 또는 ui 요소 내에서 세부적인 탐색을 위한 빠른 탐색을 위한 것입니다.

다음 표에는 이러한 명령과 올바른 사용법이 요약되어 있습니다.
|     명령 | 올바른 사용법
| -----------:| ------------
|      PageUp | 위쪽/이전 세로 페이지 또는 그룹으로 이동
|    PageDown | 아래쪽/다음 세로 페이지 또는 그룹으로 아래로 이동
|    PageLeft | 왼쪽으로 이동 (왼쪽으로/이전 가로 페이지 또는 그룹으로 이동)
|   PageRight | 오른쪽으로 이동 (오른쪽/next 가로 페이지 또는 그룹으로 이동)
|    ScrollUp | 위로 스크롤 (포커스가 있는 UI 요소 또는 스크롤 가능한 그룹 내)
|  ScrollDown | 아래로 스크롤 (포커스가 있는 UI 요소 내 또는 스크롤 가능한 그룹 내)
|  ScrollLeft | 왼쪽으로 스크롤 (포커스가 있는 UI 요소 내 또는 스크롤 가능한 그룹 내)
| ScrollRight | 오른쪽으로 스크롤 (포커스가 있는 UI 요소 내 또는 스크롤 가능한 그룹 내)
|    컨텍스트 1 | 기본 컨텍스트 작업
|    컨텍스트 2 | 보조 컨텍스트 작업
|    Context3 | 세 번째 컨텍스트 작업
|    Context4 | 네 번째 컨텍스트 작업

> **참고**    게임은 의도 된 용도와 다른 실제 함수를 사용 하 여 모든 명령에 응답 하는 데 사용할 수 있지만 놀라운 동작을 피해 야 합니다. 특히 의도 된 용도를 필요로 하는 경우 명령의 실제 함수를 변경 하지 말고, 가장 적합 한 명령에 novel 함수를 할당 하 고 Pageup 키/Pagedown 키와 같은 대응 하는 명령에 해당 함수를 할당 합니다. 마지막으로, 각 종류의 입력 장치에서 지원 되는 명령과 매핑되는 컨트롤을 고려 하 여 모든 장치에서 중요 한 명령을 액세스할 수 있도록 합니다.

## <a name="gamepad-arcade-stick-and-racing-wheel-navigation"></a>게임 패드, 아케이드 스틱 및 레이스 휠 탐색

Windows. 입력 네임 스페이스에서 지원 되는 모든 입력 장치는 UI 탐색 장치입니다.

다음 표에는 필요한 탐색 명령 _집합이_ 다양 한 입력 장치에 매핑되는 방법이 요약 되어 있습니다.

| 탐색 명령 | 게임 패드 입력                       | 아케이드 스틱 입력 | 레이스 휠 입력 |
| ------------------:| ----------------------------------- | ------------------ | ------------------ |
|                 위로 | 왼쪽 엄지 스틱 위쪽/D-패드 위로       | 스틱 위로           | D-패드 위로           |
|               아래로 | 왼쪽 엄지 스틱 아래쪽/D-패드 아래로   | 스틱 다운         | D-패드 다운         |
|               왼쪽 | 왼쪽 엄지 스틱 왼쪽/D-패드 왼쪽   | 왼쪽 스틱         | D-패드 왼쪽         |
|              오른쪽 | 왼쪽 엄지 스틱 오른쪽/D-패드 오른쪽 | 오른쪽 스틱        | D-패드 오른쪽        |
|               보기 | 보기 단추                         | 보기 단추        | 보기 단추        |
|               메뉴 | 메뉴 버튼                         | 메뉴 버튼        | 메뉴 버튼        |
|             동의함 | 단추                            | 작업 1 단추    | 단추           |
|             취소 | B 단추                            | 작업 2 단추    | B 단추           |

다음 표에는 선택적 탐색 명령의 _집합이_ 다양 한 입력 장치에 매핑되는 방법이 요약 되어 있습니다.

| 탐색 명령 | 게임 패드 입력          | 아케이드 스틱 입력 | 레이스 휠 입력    |
| ------------------:| ---------------------- | ------------------ | --------------------- |
|             PageUp | 왼쪽 트리거           | _지원 되지 않음_    | _잠기기_              |
|           PageDown | 오른쪽 트리거          | _지원 되지 않음_    | _잠기기_              |
|           PageLeft | 왼쪽 범퍼            | _지원 되지 않음_    | _잠기기_              |
|          PageRight | 오른쪽 범퍼           | _지원 되지 않음_    | _잠기기_              |
|           ScrollUp | 오른쪽 엄지 스틱 위쪽    | _지원 되지 않음_    | _잠기기_              |
|         ScrollDown | 오른쪽 엄지 스틱 아래로  | _지원 되지 않음_    | _잠기기_              |
|         ScrollLeft | 오른쪽 엄지 스틱 왼쪽  | _지원 되지 않음_    | _잠기기_              |
|        ScrollRight | 오른쪽 섬스틱(thumbstick) 오른쪽 | _지원 되지 않음_    | _잠기기_              |
|           컨텍스트 1 | X 버튼               | _지원 되지 않음_    | X 단추 (_일반적으로_) |
|           컨텍스트 2 | Y 단추               | _지원 되지 않음_    | Y 단추 (_일반적으로_) |
|           Context3 | 왼쪽 엄지 스틱 누르기  | _지원 되지 않음_    | _잠기기_              |
|           Context4 | 오른쪽 엄지 스틱 누르기 | _지원 되지 않음_    | _잠기기_              |


## <a name="detect-and-track-ui-navigation-controllers"></a>UI 탐색 컨트롤러 검색 및 추적

UI 탐색 컨트롤러는 논리적 입력 장치 이지만 물리적 장치의 표현 이며 동일한 방식으로 시스템에서 관리 됩니다. 이를 만들거나 초기화할 필요가 없습니다. 시스템에서는 UI 탐색 컨트롤러가 추가 되거나 제거 될 때 사용자에 게 알리는 연결 된 UI 탐색 컨트롤러 및 이벤트의 목록을 제공 합니다.

### <a name="the-ui-navigation-controllers-list"></a>UI 탐색 컨트롤러 목록

[UINavigationController][] 클래스는 현재 연결 되어 있는 UI 탐색 장치의 읽기 전용 목록 인 정적 속성인 [UINavigationControllers][]를 제공 합니다. 연결 된 탐색 장치 중 일부에만 관심이 있을 수 있기 때문에 속성을 통해 액세스 하는 대신 고유한 컬렉션을 유지 관리 하는 것이 좋습니다 `UINavigationControllers` .

다음 예제에서는 연결 된 모든 UI 탐색 컨트롤러를 새 컬렉션에 복사 합니다.
```cpp
auto myNavigationControllers = ref new Vector<UINavigationController^>();

for (auto device : UINavigationController::UINavigationControllers)
{
    // This code assumes that you're interested in all navigation controllers.
    myNavigationControllers->Append(device);
}
```

### <a name="adding-and-removing-ui-navigation-controllers"></a>UI 탐색 컨트롤러 추가 및 제거

UI 탐색 컨트롤러를 추가 하거나 제거 하면 [UINavigationControllerAdded][] 및 [UINavigationControllerRemoved][] 이벤트가 발생 합니다. 이러한 이벤트에 대 한 이벤트 처리기를 등록 하 여 현재 연결 된 탐색 장치를 계속 추적할 수 있습니다.

다음 예제에서는 추가 된 UI 탐색 장치 추적을 시작 합니다.
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

각 탐색 장치를 사용자 계정에 연결 하 여 해당 id를 입력에 연결 하 고, 음성 채팅 또는 탐색 기능을 용이 하 게 하는 헤드셋을 연결할 수 있습니다. 사용자 및 헤드셋 사용에 대 한 자세한 내용은 [사용자 및 장치](input-practices-for-games.md#tracking-users-and-their-devices) 및 [헤드셋](headset.md)추적을 참조 하세요.

## <a name="reading-the-ui-navigation-controller"></a>UI 탐색 컨트롤러 읽기

관심이 있는 UI 탐색 장치를 확인 한 후에는 입력을 수집할 준비가 된 것입니다. 그러나 사용할 수 있는 다른 종류의 입력과 달리 탐색 장치는 이벤트를 발생 시켜 상태 변경을 전달 하지 않습니다. 대신, 현재 상태를 _폴링하여_ 정기적으로 사용 합니다.

### <a name="polling-the-ui-navigation-controller"></a>UI 탐색 컨트롤러 폴링

폴링은 정확한 시점에 탐색 장치의 스냅숏을 캡처합니다. 입력 수집에 대 한이 방법은 일반적으로 이벤트 기반이 아니라 결정적 루프에서 실행 되기 때문에 대부분의 게임에 적합 합니다. 또한 일반적으로는 시간에 따라 수집 된 여러 단일 입력에서 가져온 것 보다 한 번에 모두 수집 된 입력에서 게임 명령을 해석 하는 것이 더 간단 합니다.

[UINavigationController. GetCurrentReading][getcurrentreading];를 호출 하 여 탐색 장치를 폴링합니다. 이 함수는 탐색 장치의 상태를 포함 하는 [UINavigationReading][] 를 반환 합니다.

```cpp
auto navigationController = myNavigationControllers[0];

UINavigationReading reading = navigationController->GetCurrentReading();
```

### <a name="reading-the-buttons"></a>단추 읽기

각 UI 탐색 단추는 눌린 (down) 또는 릴리스 (up) 여부에 해당 하는 부울 읽기를 제공 합니다. 효율성을 위해 단추 판독값은 개별 부울 값으로 표시 되지 않습니다. 대신 [RequiredUINavigationButtons][] 및 [OptionalUINavigationButtons][] 열거형이 나타내는 두 비트 필드 중 하나에 압축 됩니다.

_필요한 집합_ 에 속하는 단추는 `RequiredButtons` [UINavigationReading][] 구조의 속성에서 읽습니다. _선택적 집합_ 에 속하는 단추는 속성에서 읽습니다 `OptionalButtons` . 이러한 속성은 비트 필드 이므로 관심이 있는 단추의 값을 격리 하기 위해 비트 마스킹을 사용 됩니다. 해당 비트가 설정 된 경우 단추가 눌러져 있습니다 (down). 그렇지 않으면 해제 됩니다 (up).

다음 예에서는 _필수 집합_ 의 수락 단추를 눌렀는지 여부를 확인 합니다.
```cpp
if (RequiredUINavigationButtons::Accept == (reading.RequiredButtons & RequiredUINavigationButtons::Accept))
{
    // Accept is pressed
}
```

다음 예에서는 _필수 집합_ 의 수락 단추가 해제 되어 있는지 여부를 확인 합니다.
```cpp
if (RequiredUINavigationButtons::None == (reading.RequiredButtons & RequiredUINavigationButtons::Accept))
{
    // Accept is released (not pressed)
}
```

`OptionalButtons` `OptionalUINavigationButtons` _선택적인 집합_에서 단추를 읽을 때 속성 및 열거형을 사용 해야 합니다.

다음 예에서는 _선택적 집합_ 의 컨텍스트 1 단추를 눌렀는지 여부를 확인 합니다.
```cpp
if (OptionalUINavigationButtons::Context1 == (reading.OptionalButtons & OptionalUINavigationButtons::Context1))
{
    // Context 1 is pressed
}
```

경우에 따라 단추를 눌렀다 눌렀다 놓았을 때 눌린 상태로 전환 하는 경우, 여러 단추가 눌러져 있거나 해제 되었는지, 아니면 단추가 특정 방식으로 정렬 되는 경우 (몇 가지 누름)를 결정 해야 할 수도 있습니다. 이러한 조건을 검색 하는 방법에 대 한 자세한 내용은 [단추 전환 검색](input-practices-for-games.md#detecting-button-transitions) 및 [복잡 한 단추 배열 검색](input-practices-for-games.md#detecting-complex-button-arrangements)을 참조 하세요.


## <a name="run-the-ui-navigation-controller-sample"></a>UI 탐색 컨트롤러 샘플 실행

[InputInterfacingUWP 샘플 _(github)_ ](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/InputInterfacingUWP) 에서는 다양 한 입력 장치가 UI 탐색 컨트롤러 처럼 동작 하는 방식을 보여 줍니다.

## <a name="see-also"></a>참고 항목
[Windows. 게이밍][] 
 . [ArcadeStick.][] 
 [RacingWheel.][] 
 [IGameController.][]


[Windows.Gaming.Input]: /uwp/api/Windows.Gaming.Input
[Windows.Gaming.Input.Gamepad]: /uwp/api/Windows.Gaming.Input.Gamepad
[Arcadestick.]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[Racingwheel.]: /uwp/api/Windows.Gaming.Input.RacingWheel
[IGameController.]: /uwp/api/Windows.Gaming.Input.IGameController
[uinavigationcontroller]: /uwp/api/Windows.Gaming.Input.UINavigationController
[uinavigationcontrollers]: /uwp/api/Windows.Gaming.Input.UINavigationController
[uinavigationcontrolleradded]: /uwp/api/Windows.Gaming.Input.UINavigationController
[uinavigationcontrollerremoved]: /uwp/api/Windows.Gaming.Input.UINavigationController
[getcurrentreading]: /uwp/api/Windows.Gaming.Input.UINavigationController
[uinavigationreading]: /uwp/api/Windows.Gaming.Input.UINavigationReading
[requireduinavigationbuttons]: /uwp/api/Windows.Gaming.Input.RequiredUINavigationButtons
[optionaluinavigationbuttons]: /uwp/api/Windows.Gaming.Input.OptionalUINavigationButtons