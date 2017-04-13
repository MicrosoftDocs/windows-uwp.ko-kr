---
author: mithom
title: "게임 입력 장치 사용 방법"
description: "입력 장치를 효과적으로 사용하기 위한 패턴 및 기술을 알아봅니다."
ms.assetid: CBAD3345-3333-4924-B6D8-705279F52676
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10 uwp, 게임, 입력"
ms.openlocfilehash: 15d56a27ad914b258bb19b80b3498510d01105cd
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="input-practices-for-games"></a>게임용 입력 시스템

이 페이지는 UWP(유니버설 Windows 플랫폼) 게임에서 입력 장치를 효과적으로 사용하기 위한 패턴 및 기술을 설명합니다.

이 페이지에서는 다음에 대해 알아봅니다.
* 플레이어 추적 및 플레이어가 현재 사용 중인 입력 장치와 탐색 장치의 추적 방법
* 단추 전환(누름에서 놓음 상태로, 놓음에서 누름 상태로의 전환)을 감지하는 방법
* 단일 테스트로 복잡한 단추 배열을 감지하는 방법

## <a name="tracking-users-and-their-devices"></a>사용자 및 사용자의 장치 추적

모든 입력 장치는 [사용자][Windows.System.User]와 연결되어 있으므로 게임 플레이, 도전 과제, 설정 변경 및 기타 활동에 ID를 연결할 수 있습니다. 사용자는 마음대로 로그인 또는 로그아웃할 수 있으며, 이전 사용자가 로그아웃 한 후에도 시스템에 계속 연결되어 있는 입력 장치에 다른 사용자가 로그인하는 경우도 많습니다. 사용자가 로그인하거나 로그아웃하면 [IGameController.UserChanged][] 이벤트가 발생합니다. 이 이벤트의 이벤트 처리기를 등록하면 플레이어와 해당 플레이어가 사용 중인 장치를 추적할 수 있습니다.

또한 사용자 ID를 통해 입력 장치가 해당 UI 탐색 컨트롤러와 연결됩니다.

이러한 이유로 플레이어 입력은 [IGameController][] 인터페이스에서 상속된 장치 클래스의 [사용자][igamecontroller.user] 속성을 사용하여 추적하고 상호 연결시켜야 합니다.

이 [UserGamepadPairingUWP _(github)_](
https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/UserGamepadPairingUWP) 샘플은 사용자 및 사용 중인 장치를 추적하는 방법을 보여 줍니다.

## <a name="detecting-button-transitions"></a>단추 전환 감지

단추를 처음 누르거나 놓은 시점, 즉 단추를 놓은 상태에서 누르거나 누른 상태에서 놓은 단추 상태 전환의 정확한 시점을 알고 싶을 때가 있습니다. 이를 확인하려면 이전 장치가 읽은 내용을 기억해 두었다가 현재 장치가 읽은 내용과 비교하여 변경된 내용을 확인해야 합니다.

다음 예제는 이전 읽기 내용을 기억하기 위한 기본 접근 방식을 보여 줍니다. 여기에서는 게임 패드로 설명하지만 아케이드 스틱과 레이싱 휠, UI 탐색 단추도 원리가 동일합니다.

```cpp
GamepadReading newReading();
GamepadReading oldReading();

// Game::Loop represents one iteration of a typical game loop
void Game::Loop()
{
    // move previous newReading into oldReading before getting next newReading
    oldReading = newReading, newReading = gamepad.CurrentReading();

    // process device readings using buttonJustPressed/buttonJustReleased
}
```

먼저 `Game::Loop`은 `newReading`(이전 루프 반복에서 읽은 게임 패드)의 기존 값을 `oldReading`으로 옮긴 다음 `newReading`을 현재 반복에 대해 읽은 새로운 게임 패드로 채웁니다. 이 과정에서 단추 전환 감지에 필요한 정보가 제공됩니다.

다음 예제는 단추 전환을 감지하는 기본 접근 방식을 보여 줍니다.

```cpp
bool buttonJustPressed(const gamepadButtons selection)
{
    bool newSelectionPressed = (selection == (newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (oldReading.Buttons & selection));

    return newSelectionPressed && !oldSelectionPressed;
}

bool buttonJustReleased(gamepadButtons selection)
{
    bool newSelectionReleased = (gamepadButtons.None == (newReading.Buttons & selection));
    bool oldSelectionReleased = (gamepadButtons.None == (oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

이 두 함수는 먼저 `newReading`과 `oldReading`에서 단추 선택의 부울 상태를 얻은 다음 부울 로직을 수행하여 대상 전환 발생 여부를 판단합니다. 이들 함수는 새로운 읽기가 대상 상태(누름 또는 놓음 각각의 상태)를 포함하는 *동시에* 이전 읽기는 대상 상태를 포함하지 않는 경우에만 _true_를 반환합니다. 그렇지 않으면 _false_를 반환합니다.


## <a name="detecting-complex-button-arrangements"></a>복잡한 단추 배열 감지

입력 장치의 각 단추는 누름(아래) 또는 놓음(위) 상태 여부를 나타내는 디지털 읽기를 수행합니다. 효율성을 위해 단추 읽기는 개별 부울 값으로 표시되지 않고 [GamepadButtons][]와 같은 장치별 열거로 표시되는 비트 필드로 모두 채워집니다. 특정 단추를 읽으려면 비트 마스킹을 사용하여 관심 있는 값을 분리합니다. 해당 비트가 설정되면 단추가 누름(아래) 상태가 되고 그렇지 않으면 놓음(위) 상태가 됩니다.

단일 단추의 누름 또는 놓음 상태 판단 방법이 다시 등장합니다. 여기에서는 게임 패드로 설명하지만 아케이드 스틱과 레이싱 휠, UI 탐색 단추도 원리가 동일합니다.

```cpp
// determines whether gamepad button A is pressed
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}

// determines whether gamepad button A is released
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // button A is released (not pressed)
}
```

여기에서 볼 수 있듯 단일 단추의 상태를 판단하는 것은 간단합니다. 그러나 여러 단추의 누름 혹은 놓음 여부, 또는 단추 집합이 특정 방식으로(일부는 누름 상태이고 일부는 아닌 상태로) 배열되어 있는지 여부를 판단하고자 할 때도 있습니다. 여러 단추의 테스트는 단일 단추의 테스트보다 복잡합니다. 특히 단추 상태가 혼합되어 있을 가능성이 있는 경우에는 더욱 복잡합니다. 그러나 단일 단추 테스트와 여러 단추 테스트에 똑같이 적용되는 간단한 공식이 있습니다.

다음 예제에서는 게임 패드 단추 A와 B가 모두 누름 상태인지 여부를 확인합니다.

```cpp
if ((GamepadButtons::A | GamepadButtons::B) == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // buttons A and B are pressed
}
```

다음 예제에서는 게임 패드 단추 A와 B가 모두 놓음 상태인지 여부를 확인합니다.

```cpp
if ((GamepadButtons::None == (reading.Buttons & GamepadButtons::A | GamepadButtons::B))
{
    // buttons A and B are released (not pressed)
}
```

다음 예제에서는 단추 B가 놓음인 상태에서 게임 패드 단추 A가 누름 상태인지 여부를 확인합니다.

```cpp
if (GamepadButtons::A == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // button A is pressed and button B is released (button B is not pressed)
}
```

이 5가지 예제 모두에 공통으로 해당되는 공식이 있다면, 테스트할 단추의 배열은 같음 연산자의 왼쪽에 있는 식으로 지정되며 고려해야 할 단추는 오른쪽에 있는 마스킹 식으로 선택된다는 것입니다.

다음 예제에서는 이전 예제를 다시 작성하여 이 공식을 좀 더 분명하게 보여 줍니다.

```cpp
auto buttonArrangement = GamepadButtons::A;
auto buttonSelection = (reading.Buttons & (GamepadButtons::A | GamepadButtons::B));

if (buttonArrangement == buttonSelection)
{
    // button A is pressed and button B is released (button B is not pressed)
}
```

이 공식을 적용하여 모든 상태 배열에서 단추를 몇 개든 테스트할 수 있습니다.



[Windows.System.User]: https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx
[igamecontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[igamecontroller.user]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.user.aspx
[igamecontroller.userchanged]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.userchanged.aspx
[gamepadbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadbuttons.aspx
