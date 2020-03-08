---
title: 게임용 입력 시스템
description: 입력 디바이스를 효과적으로 사용하기 위한 패턴 및 기술을 알아봅니다.
ms.assetid: CBAD3345-3333-4924-B6D8-705279F52676
ms.date: 11/20/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, 입력
ms.localizationpriority: medium
ms.openlocfilehash: 8235b2c2029b2bb3b9351263a3c908879b4beba9
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853064"
---
# <a name="input-practices-for-games"></a>게임용 입력 시스템

이 페이지는 UWP(유니버설 Windows 플랫폼) 게임에서 입력 디바이스를 효과적으로 사용하기 위한 패턴 및 기술을 설명합니다.

이 페이지에서는 다음에 대해 알아봅니다.

* 플레이어 추적 및 플레이어가 현재 사용 중인 입력 디바이스와 탐색 디바이스의 추적 방법
* 단추 전환(누름에서 놓음 상태로, 놓음에서 누름 상태로의 전환)을 감지하는 방법
* 단일 테스트로 복잡한 단추 배열을 감지하는 방법

## <a name="choosing-an-input-device-class"></a>입력 디바이스 클래스 선택

사용할 수 있는 입력 API에는 [ArcadeStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick), [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) 및 [게임 패드](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad) 등 여러 가지 유형이 있습니다. 게임에 대해 사용할 API를 어떻게 결정하시겠습니까?

어떤 API가 게임에 가장 적절한 입력을 제공할지 선택해야 합니다. 예를 들어 2D 플랫폼 게임을 만드는 경우 단순히 **게임 패드** 클래스를 사용하고 다른 클래스를 통해 사용할 수 있는 추가 기능은 신경 쓰지 않을 수도 있습니다. 이는 게임이 게임 패드만 지원하도록 제한할 수 있으며 추가 코드가 필요 없이 여러 가지 게임 패드에서 작동하는 일관된 인터페이스를 제공하게 될 수 있습니다.

한편 복잡한 플라이트 및 레이싱 시뮬레이션의 경우 모든 [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) 개체를 기준선으로 열거하여 별개의 페달 또는 스로틀이 여전이 단일 플레이어에 의해 사용되는지를 포함하여 열정적인 플레이어가 가지고 있을 수 있는 모든 틈새 장치를 지원하는지 확인할 수 있습니다. 

여기에서 [Gamepad.FromGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.fromgamecontroller)와 같은 입력 클래스의 **FromGameController** 메서드를 사용하여 각 장치에 더 많은 큐레이트 보기가 있는지 확인합니다. 예를 들면 장치가 **게임 패드**인 경우에도 버튼 매핑 UI를 조정하여 이를 반영하고 선택할 수 있는 감지 가능 기본 버튼 매핑을 제공하고자 할 수 있습니다. (이는 **RawGameController**만을 사용하는 경우 플레이어에게 수동으로 게임 패드 입력을 요구하는 것과 반대입니다.) 

또는 각각 [HardwareVendorId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) 및 [HardwareProductId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId)를 사용하여 **RawGameController**의 벤더 ID(VID) 및 제품 ID(PID)를 확인하고 인기 있는 장치에 대해 제안된 버튼 매핑을 제공하는 동시에 플레이어의 수동 매핑을 통해 추후에 제공되는 알 수 없는 장치와 호환되게 할 수 있습니다.

## <a name="keeping-track-of-connected-controllers"></a>지속적으로 연결 컨트롤러 추적

각 컨트롤러 유형에는 연결 컨트롤러(예: [Gamepad.Gamepads](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.Gamepads)) 목록이 포함되어 있지만 자체 컨트롤러 목록을 유지하는 것이 좋습니다. 자세한 내용은 [게임 패드 목록](gamepad-and-vibration.md#the-gamepads-list)을 참조하세요(컨트롤러 유형마다 자체 주제에 대해 비슷한 이름을 가진 섹션이 존재).

한편 플레이어에서 컨트롤러를 분리하거나 새 컨트롤러를 연결하면 어떻게 되나요? 이러한 이벤트를 처리하고 이에 따라 목록을 업데이트해야 합니다. 자세한 내용은 [게임 패드 추가 및 제거](gamepad-and-vibration.md#adding-and-removing-gamepads)를 참조하세요(다시 말하지만 컨트롤러 유형마다 자체 주제에 대해 비슷한 이름을 가진 섹션이 존재).

추가 및 제거된 이벤트가 비동기적으로 발생하기 때문에 컨트롤러 목록을 처리할 때 잘못된 결과를 얻을 수 있습니다. 따라서 컨트롤러 목록에 액세스할 때는 언제든지 한번에 오직 하나의 스레드만 하나의 목록에 액세스할 수 있도록 잠금을 설정해야 합니다. **&lt;ppl.h&gt;** 의 [동시성 런타임](https://docs.microsoft.com/cpp/parallel/concrt/concurrency-runtime), 구체적으로 [critical_section 클래스](https://docs.microsoft.com/cpp/parallel/concrt/reference/critical-section-class)를 통해 이것이 가능합니다.

또 하나 생각할 것은 연결 컨트롤러 목록이 처음에는 비어있다가 채워지는 데 1 ~ 2초가 걸린다는 사실입니다. 따라서 시작 메서드에서 현재 게임 패드를 할당만 하면 값이 **null**이 됩니다!

이 문제를 해결하려면 주 게임 패드를 "새로 고침"하는 메서드가 있어야 합니다(싱글 플레이어 게임의 경우 멀티플레이어 게임을 위해서는 보다 정교한 솔루션이 필요) . 컨트롤러 추가 이벤트 처리기와 컨트롤러 제거 이벤트 처리기 모두에서, 또는 업데이트 메서드에서 이 메서드를 호출해야 합니다.

다음 메서드는 목록의 첫 번째 게임 패드를 반환합니다(목록이 비어 있는 경우 **nullptr**). 컨트롤러에서 언제 어떤 작업을 수행하든 **nullptr**을 확인하는 것을 잊지 마시기 바랍니다. 연결된 컨트롤러가 없을 때 게임 플레이를 차단할 것인지(예: 게임 일시 중지), 아니면 입력을 무시하면서 게임 플레이를 계속할 것인지는 사용자가 결정합니다.

```cpp
#include <ppl.h>

using namespace Platform::Collections;
using namespace Windows::Gaming::Input;
using namespace concurrency;

Vector<Gamepad^>^ m_myGamepads = ref new Vector<Gamepad^>();

Gamepad^ GetFirstGamepad()
{
    Gamepad^ gamepad = nullptr;
    critical_section::scoped_lock{ m_lock };

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(0);
    }

    return gamepad;
}
```

이 모두를 하나로 결합해 게임 패드의 입력을 처리하는 방법이 여기에 예제로 나와 있습니다.

```cpp
#include <algorithm>
#include <ppl.h>

using namespace Platform::Collections;
using namespace Windows::Foundation;
using namespace Windows::Gaming::Input;
using namespace concurrency;

static Vector<Gamepad^>^ m_myGamepads = ref new Vector<Gamepad^>();
static Gamepad^          m_gamepad = nullptr;
static critical_section  m_lock{};

void Start()
{
    // Register for gamepad added and removed events.
    Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(&OnGamepadAdded);
    Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(&OnGamepadRemoved);

    // Add connected gamepads to m_myGamepads.
    for (auto gamepad : Gamepad::Gamepads)
    {
        OnGamepadAdded(nullptr, gamepad);
    }
}

void Update()
{
    // Update the current gamepad if necessary.
    if (m_gamepad == nullptr)
    {
        auto gamepad = GetFirstGamepad();

        if (m_gamepad != gamepad)
        {
            m_gamepad = gamepad;
        }
    }

    if (m_gamepad != nullptr)
    {
        // Gather gamepad reading.
    }
}

// Get the first gamepad in the list.
Gamepad^ GetFirstGamepad()
{
    Gamepad^ gamepad = nullptr;
    critical_section::scoped_lock{ m_lock };

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(0);
    }

    return gamepad;
}

void OnGamepadAdded(Platform::Object^ sender, Gamepad^ args)
{
    // Check if the just-added gamepad is already in m_myGamepads; if it isn't, 
    // add it.
    critical_section::scoped_lock lock{ m_lock };
    auto it = std::find(begin(m_myGamepads), end(m_myGamepads), args);

    if (it == end(m_myGamepads))
    {
        m_myGamepads->Append(args);
    }
}

void OnGamepadRemoved(Platform::Object^ sender, Gamepad^ args)
{
    // Remove the gamepad that was just disconnected from m_myGamepads.
    unsigned int indexRemoved;
    critical_section::scoped_lock lock{ m_lock };

    if (m_myGamepads->IndexOf(args, &indexRemoved))
    {
        if (m_gamepad == m_myGamepads->GetAt(indexRemoved))
        {
            m_gamepad = nullptr;
        }

        m_myGamepads->RemoveAt(indexRemoved);
    }
}
```

## <a name="tracking-users-and-their-devices"></a>사용자 및 사용자의 디바이스 추적

모든 입력 장치는 [사용자](https://docs.microsoft.com/uwp/api/windows.system.user)와 연결되어 있으므로 게임 플레이, 도전 과제, 설정 변경 및 기타 활동에 ID를 연결할 수 있습니다. 사용자는 마음대로 로그인 또는 로그아웃할 수 있으며, 이전 사용자가 로그아웃 한 후에도 시스템에 계속 연결되어 있는 입력 장치에 다른 사용자가 로그인하는 경우도 많습니다. 사용자가 로그인하거나 로그아웃하면 [IGameController.UserChanged](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.UserChanged) 이벤트가 발생합니다. 이 이벤트의 이벤트 처리기를 등록하면 플레이어와 해당 플레이어가 사용 중인 디바이스를 추적할 수 있습니다.

또한 사용자 ID를 통해 입력 장치가 해당 [UI 탐색 컨트롤러](ui-navigation-controller.md)와 연결됩니다.

이러한 이유로 플레이어 입력은 [IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller) 인터페이스에서 상속된 장치 클래스의 [사용자](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.User) 속성으로 추적하고 상호 연결해야 합니다.

이 [UserGamepadPairingUWP(GitHub)](
https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/UserGamepadPairingUWP) 샘플은 사용자 및 사용 중인 장치를 추적하는 방법을 보여 줍니다.

## <a name="detecting-button-transitions"></a>단추 전환 감지

단추를 처음 누르거나 놓은 시점, 즉 단추를 놓은 상태에서 누르거나 누른 상태에서 놓은 단추 상태 전환의 정확한 시점을 알고 싶을 때가 있습니다. 이를 확인하려면 이전 디바이스가 읽은 내용을 기억해 두었다가 현재 디바이스가 읽은 내용과 비교하여 변경된 내용을 확인해야 합니다.

다음 예제는 이전 읽기 내용을 기억하기 위한 기본 접근 방식을 보여 줍니다. 여기에서는 게임 패드로 설명하지만 아케이드 스틱과 레이싱 휠, 기타 입력 장치 유형도 원리가 동일합니다.

```cpp
Gamepad gamepad;
GamepadReading newReading();
GamepadReading oldReading();

// Called at the start of the game.
void Game::Start()
{
    gamepad = Gamepad::Gamepads[0];
}

// Game::Loop represents one iteration of a typical game loop
void Game::Loop()
{
    // move previous newReading into oldReading before getting next newReading
    oldReading = newReading, newReading = gamepad.GetCurrentReading();

    // process device readings using buttonJustPressed/buttonJustReleased (see below)
}
```

먼저 `Game::Loop`은 `newReading`(이전 루프 반복에서 읽은 게임 패드)의 기존 값을 `oldReading`으로 옮긴 다음 `newReading`을 현재 반복에 대해 읽은 새로운 게임 패드로 채웁니다. 이 과정에서 단추 전환 감지에 필요한 정보가 제공됩니다.

다음 예제는 단추 전환을 감지하는 기본 접근 방식을 보여 줍니다.

```cpp
bool ButtonJustPressed(const GamepadButtons selection)
{
    bool newSelectionPressed = (selection == (newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (oldReading.Buttons & selection));

    return newSelectionPressed && !oldSelectionPressed;
}

bool ButtonJustReleased(GamepadButtons selection)
{
    bool newSelectionReleased =
        (GamepadButtons.None == (newReading.Buttons & selection));

    bool oldSelectionReleased =
        (GamepadButtons.None == (oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

이 두 함수는 먼저 `newReading`과 `oldReading`에서 단추 선택의 부울 상태를 얻은 다음 부울 로직을 수행하여 대상 전환 발생 여부를 판단합니다. 이들 함수는 새로운 읽기가 대상 상태(누름 또는 놓음 각각의 상태)를 포함하는 *동시에* 이전 읽기는 대상 상태를 포함하지 않는 경우에만 **true**를 반환합니다. 그렇지 않으면 **false**를 반환합니다.

## <a name="detecting-complex-button-arrangements"></a>복잡한 단추 배열 감지

입력 장치의 각 단추는 누름(아래) 또는 놓음(위) 상태 여부를 나타내는 디지털 읽기를 수행합니다. 효율성을 위해 단추 읽기는 개별 부울 값으로 표시되지 않고 [GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons)와 같은 장치별 열거로 표시되는 비트 필드로 모두 채워집니다. 특정 단추를 읽으려면 비트 마스킹을 사용하여 관심 있는 값을 분리합니다. 해당 비트가 설정된 경우에는 버튼이 눌리고(아래쪽), 그렇지 않은 경우에는 놓입니다(위쪽).

단일 단추의 누름 또는 놓음 상태 판단 방법이 다시 등장합니다. 여기에서는 게임 패드로 설명하지만 아케이드 스틱과 레이싱 휠, 기타 입력 장치 유형도 원리가 동일합니다.

```cpp
GamepadReading reading = gamepad.GetCurrentReading();

// Determines whether gamepad button A is pressed.
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // The A button is pressed.
}

// Determines whether gamepad button A is released.
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // The A button is released (not pressed).
}
```

여기에서 볼 수 있듯 단일 단추의 상태를 판단하는 것은 간단합니다. 그러나 여러 단추의 누름 혹은 놓음 여부, 또는 단추 집합이 특정 방식으로(일부는 누름 상태이고 일부는 아닌 상태로) 배열되어 있는지 여부를 판단하고자 할 때도 있습니다. 여러 단추의 테스트는 단일 단추의 테스트보다 복잡합니다. 특히 단추 상태가 혼합되어 있을 가능성이 있는 경우에는 더욱 복잡합니다. 그러나 단일 단추 테스트와 여러 단추 테스트에 똑같이 적용되는 간단한 공식이 있습니다.

다음 예제에서는 게임 패드 단추 A와 B가 모두 누름 상태인지 여부를 확인합니다.

```cpp
if ((GamepadButtons::A | GamepadButtons::B) == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both pressed.
}
```

다음 예제에서는 게임 패드 단추 A와 B가 모두 놓음 상태인지 여부를 확인합니다.

```cpp
if ((GamepadButtons::None == (reading.Buttons & GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both released (not pressed).
}
```

다음 예제에서는 단추 B가 놓음인 상태에서 게임 패드 단추 A가 누름 상태인지 여부를 확인합니다.

```cpp
if (GamepadButtons::A == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

이 5가지 예제 모두에 공통으로 해당되는 공식이 있다면, 테스트할 단추의 배열은 같음 연산자의 왼쪽에 있는 식으로 지정되며 고려해야 할 단추는 오른쪽에 있는 마스킹 식으로 선택된다는 것입니다.

다음 예제에서는 이전 예제를 다시 작성하여 이 공식을 좀 더 분명하게 보여 줍니다.

```cpp
auto buttonArrangement = GamepadButtons::A;
auto buttonSelection = (reading.Buttons & (GamepadButtons::A | GamepadButtons::B));

if (buttonArrangement == buttonSelection)
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

이 공식을 적용하여 모든 상태 배열에서 단추를 몇 개든 테스트할 수 있습니다.

## <a name="get-the-state-of-the-battery"></a>배터리 상태 가져오기

[IGameControllerBatteryInfo](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo) 인터페이스를 구현하는 게임 컨트롤러에서는 컨트롤러 인스턴스에 대한 [TryGetBatteryReport](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo.TryGetBatteryReport)를 호출하여 컨트롤러에 배터리에 대한 정보를 제공하는 [BatteryReport](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport) 개체를 얻을 수 있습니다. 배터리가 충전되고 있는 속도([ChargeRateInMilliwatts](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.ChargeRateInMilliwatts)), 새 배터리의 예상 에너지 용량([DesignCapacityInMilliwattHours](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.DesignCapacityInMilliwattHours)), 현재 배터리의 완전 충전 시 에너지 용량([FullChargeCapacityInMilliwattHours](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.FullChargeCapacityInMilliwattHours)) 같은 속성들을 얻을 수 있습니다.

상세 배터리 보고를 지원하는 게임 컨트롤러에서는 [배터리 정보 가져오기](../devices-sensors/get-battery-info.md)에 자세히 설명되어 있는 대로 이 속성과 배터리에 대한 자세한 정보를 얻을 수 있습니다. 그러나 대부분의 게임 컨트롤러는 배터리 보고 수준을 지원하지 않고 대신에 저가 하드웨어를 사용합니다. 이러한 컨트롤러에서는 다음 사항을 항상 유념해야 합니다.

* **ChargeRateInMilliwatts** 및 **DesignCapacityInMilliwattHours**는 항상 **NULL**입니다.

* [RemainingCapacityInMilliwattHours](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.RemainingCapacityInMilliwattHours) / **FullChargeCapacityInMilliwattHours**를 계산하여 배터리 비율(%)을 얻을 수 있습니다. 이러한 속성의 값을 무시하고 계산된 비율만 처리해야 합니다.

* 이전의 글머리 기호에서의 비율은 항상 다음 중 하나가 됩니다.

    * 100%(완전 충전)
    * 70%(중간 수준)
    * 40%(낮은 수준)
    * 10%(위험 수준)

코드에서 배터리 잔여 사용 시간의 비율에 따라 어떤 작업을 수행할 경우(예: UI 작성)에는 위의 값들을 준수하는지 확인합니다. 예를 들어 배터리 수준이 10%가 되면 컨트롤러 배터리가 부족하다고 플레이어에게 경고합니다.

## <a name="see-also"></a>참고 항목

* [Windows. User 클래스](https://docs.microsoft.com/uwp/api/windows.system.user)
* [IGameController 인터페이스입니다.](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Windows GamepadButtons 열거형](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons)
