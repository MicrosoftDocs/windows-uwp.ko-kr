---
title: 게임용 입력 시스템
description: UWP (유니버설 Windows 플랫폼) 게임에서 입력 장치를 효과적으로 사용 하기 위한 패턴 및 기술을 알아봅니다.
ms.assetid: CBAD3345-3333-4924-B6D8-705279F52676
ms.date: 11/20/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 입력
ms.localizationpriority: medium
ms.openlocfilehash: d8ea74f7053cfdd0aeb6ef3dbe032afdb089f420
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173117"
---
# <a name="input-practices-for-games"></a>게임용 입력 시스템

이 항목에서는 UWP (유니버설 Windows 플랫폼) 게임에서 입력 장치를 효과적으로 사용 하기 위한 패턴 및 기술에 대해 설명 합니다.

이 항목을 참조 하 여 다음에 대해 알아봅니다.

* 플레이어를 추적 하는 방법 및 현재 사용 중인 입력 및 탐색 장치
* 단추 전환을 검색 하는 방법 (눌린 상태에서 눌렀다 놓은 상태)
* 단일 테스트를 사용 하 여 복잡 한 단추 정렬을 검색 하는 방법

## <a name="choosing-an-input-device-class"></a>입력 장치 클래스 선택

[ArcadeStick](/uwp/api/windows.gaming.input.arcadestick), [FlightStick](/uwp/api/windows.gaming.input.flightstick)및 [게임 패드](/uwp/api/windows.gaming.input.gamepad)와 같이 다양 한 유형의 입력 api를 사용할 수 있습니다. 게임에 사용할 API를 어떻게 결정 하나요?

게임에 가장 적합 한 입력을 제공 하는 어떤 API를 선택 해야 합니다. 예를 들어 2D 플랫폼 게임을 수행 하는 경우 다른 클래스를 통해 사용할 수 있는 추가 기능을 사용 하지 않고 게임 **패드** 클래스를 사용 하는 것이 좋습니다. 이렇게 하면 gamepads만 지원 하도록 게임을 제한 하 고 추가 코드를 요구 하지 않고 여러 다른 gamepads에서 작동 하는 일관적인 인터페이스를 제공 합니다.

반면에 복잡 한 비행 및 레이스 시뮬레이션의 경우, 단일 플레이어에서 여전히 사용 되는 별도의 페달이 나 스로틀 등의 장치를 포함 하 여 열성적인 플레이어가 가질 수 있는 모든 틈새 장치를 지원 하도록 [RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller) 모든 개체를 기준선으로 열거할 수 있습니다. 

여기에서 [FromGameController](/uwp/api/windows.gaming.input.gamepad.fromgamecontroller)와 같은 입력 클래스의 **FromGameController** 메서드를 사용 하 여 각 장치에 더 많은 큐 레이트 뷰가 있는지 확인할 수 있습니다. 예를 들어 장치가 **게임 패드**이기도 한 경우 단추 매핑 UI를 조정 하 여이를 반영 하 고 적절 한 기본 단추 매핑을 제공 하 여 선택할 수 있습니다. **RawGameController**만 사용 하는 경우 플레이어에서 게임 패드 입력을 수동으로 구성 해야 하는 것과 대조적입니다. 

또는 **RawGameController** (각각 [HardwareVendorId](/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) 및 [HardwareProductId](/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId)를 사용 하 여)의 VID (공급 업체 ID) 및 PID (제품 id)를 살펴보고, 자주 사용 하는 장치에 대해 제안 된 단추 매핑을 제공할 수 있습니다 .이 경우 플레이어의 수동 매핑을 통해 나중에 제공 되는 알 수 없는 장치와 계속 호환 됩니다.

## <a name="keeping-track-of-connected-controllers"></a>연결 된 컨트롤러 추적 유지

각 컨트롤러 유형에는 연결 된 컨트롤러 목록 (예: [Gamepads](/uwp/api/windows.gaming.input.gamepad.Gamepads))이 포함 되어 있지만 컨트롤러의 고유한 목록을 유지 관리 하는 것이 좋습니다. 자세한 내용은 [gamepads 목록을](gamepad-and-vibration.md#the-gamepads-list) 참조 하십시오. 각 컨트롤러 형식에는 해당 항목에 비슷한 이름의 섹션이 있습니다.

그러나 플레이어가 자신의 컨트롤러를 unplugs 새 항목을 연결 하는 경우 어떻게 되나요? 이러한 이벤트를 처리 하 고 목록을 적절 하 게 업데이트 해야 합니다. 자세한 내용은 [Gamepads 추가 및 제거](gamepad-and-vibration.md#adding-and-removing-gamepads) 를 참조 하세요. 자세한 내용은 각 컨트롤러 형식에 유사한 이름의 섹션이 있습니다.

추가 및 제거 된 이벤트는 비동기적으로 발생 하므로 컨트롤러 목록을 처리할 때 잘못 된 결과를 얻을 수 있습니다. 따라서 컨트롤러 목록에 액세스할 때마다 한 번에 하나의 스레드만 해당 컨트롤러에 액세스할 수 있도록 잠금을 설정 해야 합니다. 이 작업은 [동시성 런타임](/cpp/parallel/concrt/concurrency-runtime), 특히 ** &lt; ppl &gt; **의 [critical_section 클래스](/cpp/parallel/concrt/reference/critical-section-class)를 사용 하 여 수행할 수 있습니다.

또 다른 고려 사항은 연결 된 컨트롤러의 목록이 처음에는 비어 있으며 두 번째 또는 두 번째를 사용 하 여 채우는 것입니다. 따라서 start 메서드에서 현재 게임 패드만 할당 하면 **null**이 됩니다.

이를 수정 하려면 기본 게임 패드를 "새로 고치는" (단일 플레이어 게임에서 멀티 플레이 게임에 보다 정교한 솔루션이 필요 함) 메서드가 있어야 합니다. 그런 다음 컨트롤러 추가 및 컨트롤러에서 제거 된 이벤트 처리기 또는 업데이트 메서드에서이 메서드를 호출 해야 합니다.

다음 메서드는 목록에서 첫 번째 게임 패드를 반환 합니다 (목록이 비어 있는 경우 **nullptr** ). 그런 다음 컨트롤러를 사용 하 여 언제 든 지 **nullptr** 를 확인 해야 합니다. 연결 된 컨트롤러가 없을 때 (예: 게임 일시 중지) 플레이를 차단 하 고, 입력을 무시 하는 동안 플레이를 계속 하는 것이 좋습니다.

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

이를 모두 함께 수행 하는 방법에 대 한 예제는 게임 패드의 입력을 처리 하는 방법의 예입니다.

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

## <a name="tracking-users-and-their-devices"></a>사용자 및 장치 추적

모든 입력 장치는 해당 id가 게임, 성과, 설정 변경 내용 및 기타 작업에 연결 될 수 있도록 [사용자](/uwp/api/windows.system.user) 와 연결 됩니다. 사용자는 언제 든 지 로그인 하거나 로그 아웃할 수 있으며, 다른 사용자가 이전 사용자가 로그 아웃 한 후 시스템에 연결 된 상태로 유지 되는 입력 장치에 로그인 하는 것이 일반적입니다. 사용자가 로그인 하거나 로그 아웃할 때 [IGameController](/uwp/api/windows.gaming.input.igamecontroller.UserChanged) 이벤트가 발생 합니다. 이 이벤트에 대 한 이벤트 처리기를 등록 하 여 플레이어와 사용 중인 장치를 추적할 수 있습니다.

사용자 id는 입력 장치가 해당 [UI 탐색 컨트롤러](ui-navigation-controller.md)와 연결 된 방법 이기도 합니다.

이러한 이유로 플레이어 입력을 추적 하 고 장치 클래스 ( [IGameController](/uwp/api/windows.gaming.input.igamecontroller) 인터페이스에서 상속)의 [사용자](/uwp/api/windows.gaming.input.igamecontroller.User) 속성과 상관 관계를 지정 해야 합니다.

[UserGamepadPairingUWP](/samples/microsoft/xbox-atg-samples/usergamepadpairinguwp/) 샘플은 사용자와 사용자가 사용 하는 장치를 추적할 수 있는 방법을 보여 줍니다.

## <a name="detecting-button-transitions"></a>단추 전환 검색

경우에 따라 단추를 처음 눌렀다 놓았을 때 알고 싶을 수 있습니다. 즉, 단추 상태가 릴리즈됨에서 누름으로 또는에서 눌렀다 놓았을 때 정확 하 게 전환 됩니다. 이를 확인 하려면 이전 장치를 읽고이를 비교 하 여 변경 된 내용을 확인 해야 합니다.

다음 예제에서는 이전 읽기를 기억 하는 기본적인 방법을 보여 줍니다. gamepads는 여기에 표시 되지만 해당 원칙은 아케이드 스틱, 레이스 휠 및 기타 입력 장치 유형에 대해 동일 합니다.

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

다른 작업을 수행 하기 전에 `Game::Loop` 의 기존 값 `newReading` (이전 루프 반복에서 읽은 게임 패드)을로 이동한 `oldReading` 다음 `newReading` 현재 반복에 대 한 새로운 게임 패드를 채웁니다. 그러면 단추 전환을 검색 하는 데 필요한 정보가 제공 됩니다.

다음 예제에서는 단추 전환을 검색 하는 기본적인 방법을 보여 줍니다.

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

이러한 두 함수는 먼저 및에서 단추 선택의 부울 상태를 파생 시킨 `newReading` `oldReading` 다음, 부울 논리를 수행 하 여 대상 전환이 발생 했는지 여부를 확인 합니다. 이러한 함수는 새 읽기가 대상 상태 (각각 눌린 상태 또는 릴리즈됨)를 포함 *하 고* 이전 읽기에 대상 상태가 포함 되지 않은 경우에만 **true** 를 반환 합니다. 그렇지 않으면 **false**를 반환 합니다.

## <a name="detecting-complex-button-arrangements"></a>복합 단추 배치 검색

입력 장치의 각 단추는 누름 (다운) 또는 릴리스 (up) 여부를 나타내는 디지털 읽기를 제공 합니다. 효율성을 위해 단추 판독값은 개별 부울 값으로 표시 되지 않습니다. 대신, [GamepadButtons](/uwp/api/windows.gaming.input.gamepadbuttons)와 같은 장치 특정 열거형으로 표현 되는 비트 필드에 모두 압축 됩니다. 특정 단추를 읽으려면 비트 마스킹을 사용 하 여 관심 있는 값을 격리 합니다. 해당 비트가 설정 된 경우 단추가 눌러져 있습니다 (down). 그렇지 않으면 해제 됩니다 (up).

단일 단추가 눌러져 있거나 해제 되도록 결정 되는 방식을 기억 합니다. gamepads는 여기에 표시 되지만 해당 원칙은 아케이드 스틱, 레이스 휠 및 기타 입력 장치 유형에 대해 동일 합니다.

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

여기에서 볼 수 있듯이 단일 단추의 상태를 결정 하는 것은 매우 간단 하지만 때로는 여러 단추를 눌렀는지 또는 눌렀다 놓았을 지 또는 단추 집합이 특정 방법으로 몇 번의 단추를 눌렀는지를 확인 하는 것이 좋습니다 &mdash; . 여러 단추를 테스트 하는 것은 특히 혼합 단추 상태를 사용 하 여 단일 단추를 테스트 하는 것 보다 복잡 &mdash; &mdash; 하지만, 단일 및 여러 단추 테스트에 동일 하 게 적용 되는 이러한 테스트에 대 한 간단한 수식이 있습니다.

다음 예에서는 게임 패드 단추 A와 B가 모두 눌러져 있는지 여부를 확인 합니다.

```cpp
if ((GamepadButtons::A | GamepadButtons::B) == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both pressed.
}
```

다음 예에서는 게임 패드 단추 A와 B가 모두 릴리스 되었는지 여부를 확인 합니다.

```cpp
if ((GamepadButtons::None == (reading.Buttons & GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both released (not pressed).
}
```

다음 예제에서는 단추 B가 해제 된 상태에서 게임 패드 단추 A를 눌렀는지 여부를 결정 합니다.

```cpp
if (GamepadButtons::A == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

이러한 모든 예제에 공통적으로 적용 되는 수식에는 테스트 하는 데 사용할 단추의 배열이 오른쪽의 마스킹 식에 의해 선택 되는 반면 같음 연산자의 왼쪽에 있는 식으로 지정 됩니다.

다음 예제에서는 이전 예제를 다시 작성 하 여이 수식을 더 명확 하 게 보여 줍니다.

```cpp
auto buttonArrangement = GamepadButtons::A;
auto buttonSelection = (reading.Buttons & (GamepadButtons::A | GamepadButtons::B));

if (buttonArrangement == buttonSelection)
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

이 수식을 적용 하 여 모든 상태를 정렬 하 여 원하는 수의 단추를 테스트할 수 있습니다.

## <a name="get-the-state-of-the-battery"></a>배터리 상태 가져오기

[IGameControllerBatteryInfo](/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo) 인터페이스를 구현 하는 게임 컨트롤러의 경우 컨트롤러 인스턴스에서 [TryGetBatteryReport](/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo.TryGetBatteryReport) 를 호출 하 여 컨트롤러의 배터리에 대 한 정보를 제공 하는 [BatteryReport](/uwp/api/windows.devices.power.batteryreport) 개체를 가져올 수 있습니다. 배터리 요금 ([ChargeRateInMilliwatts](/uwp/api/windows.devices.power.batteryreport.ChargeRateInMilliwatts))과 같은 속성, 새 배터리의 예상 에너지 용량 ([DesignCapacityInMilliwattHours](/uwp/api/windows.devices.power.batteryreport.DesignCapacityInMilliwattHours)) 및 현재 배터리의 완전히 충전 된 에너지 용량 ([FullChargeCapacityInMilliwattHours](/uwp/api/windows.devices.power.batteryreport.FullChargeCapacityInMilliwattHours))과 같은 속성을 얻을 수 있습니다.

자세한 배터리 보고를 지 원하는 게임 컨트롤러의 경우 배터리 [정보 가져오기](../devices-sensors/get-battery-info.md)에 설명 된 대로 배터리에 대 한이 정보와 자세한 정보를 얻을 수 있습니다. 그러나 대부분의 게임 컨트롤러는 해당 배터리 보고 수준을 지원 하지 않으며 대신 저렴 한 하드웨어를 사용 합니다. 이러한 컨트롤러의 경우 다음 사항을 염두에 두어야 합니다.

* **ChargeRateInMilliwatts** 및 **DesignCapacityInMilliwattHours** 는 항상 **NULL**입니다.

* [RemainingCapacityInMilliwattHours](/uwp/api/windows.devices.power.batteryreport.RemainingCapacityInMilliwattHours)FullChargeCapacityInMilliwattHours를 계산 하 여 배터리 비율을 얻을 수 있습니다  /  **FullChargeCapacityInMilliwattHours**. 이러한 속성의 값을 무시 하 고 계산 된 비율을 처리 해야 합니다.

* 이전 글머리 기호 지점의 백분율은 항상 다음 중 하나입니다.

    * 100% (전체)
    * 70% (보통)
    * 40% (낮음)
    * 10% (위험)

남아 있는 배터리 수명의 백분율을 기준으로 코드에서 그리기 UI와 같은 일부 작업을 수행 하는 경우에는 위의 값을 준수 하는지 확인 합니다. 예를 들어 컨트롤러의 배터리가 부족할 때 플레이어에 게 경고를 표시 하려면 10%에 도달할 때이를 수행 합니다.

## <a name="see-also"></a>참고 항목

* [ Tem를Windows.Sys합니다. 사용자 클래스](/uwp/api/windows.system.user)
* [IGameController 인터페이스입니다.](/uwp/api/windows.gaming.input.igamecontroller)
* [Windows GamepadButtons 열거형](/uwp/api/windows.gaming.input.gamepadbuttons)
* [UserGamepadPairingUWP 샘플](/samples/microsoft/xbox-atg-samples/usergamepadpairinguwp/)