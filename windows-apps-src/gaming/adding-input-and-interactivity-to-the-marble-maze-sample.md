---
title: Marble Maze 샘플에 입력 및 대화형 작업 추가
description: 입력 장치에서 작업을 할 때 고려해야 할 주요 사항에 대해 알아보세요.
ms.assetid: b946bf62-c0ca-f9ec-1a87-8195b89a5ab4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 입력, 샘플
ms.localizationpriority: medium
ms.openlocfilehash: f078cd721406120105efb35d1519e7fd0b36e74c
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258604"
---
# <a name="adding-input-and-interactivity-to-the-marble-maze-sample"></a>Marble Maze 샘플에 입력 및 대화형 작업 추가




유니버설 Windows 플랫폼(UWP)은 데스크톱 컴퓨터, 노트북, 태블릿 등 다양한 장치에서 실행됩니다. 한 장치에 너무 많은 입력 및 제어 메커니즘이 있을 수 있습니다. 이 문서에서는 입력 장치 작업을 할 때 고려할 몇 가지 주요 사항을 설명하고 Marble Maze가 이러한 사례를 적용하는 방법을 보여 줍니다.

> [!NOTE]
> 이 문서에 해당하는 샘플 코드는 [DirectX Marble Maze 게임 샘플](https://github.com/microsoft/Windows-appsample-marble-maze)에 있습니다.

 
게임에서 입력 작업을 하는 경우에 대해 논의하는 주요 사항은 다음과 같습니다.

-   가능한 경우 게임이 고객의 광범위한 기본 설정과 기능을 수용할 수 있도록 여러 입력 장치를 지원합니다. 게임 컨트롤러 및 센서 사용은 선택 사항이지만 플레이어 환경을 향상시키기 위해 사용하는 것이 좋습니다. 이러한 입력 장치를 더 쉽게 통합할 수 있도록 게임 컨트롤러 및 센서 API를 디자인했습니다.

-   터치를 초기화하려면 포인터가 활성화, 해제 및 이동되는 경우 등의 창 이벤트를 등록해야 합니다. 가속도계를 초기화하려면 응용 프로그램을 초기화할 때 [Windows::Devices::Sensors::Accelerometer](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer) 개체를 만듭니다. Xbox 컨트롤러는 초기화가 필요하지 않습니다.

-   싱글 플레이어 게임의 경우 가능한 모든 Xbox 컨트롤러의 입력을 결합할지 여부를 고려합니다. 이렇게 하면 어떤 입력이 어떤 컨트롤러에서 제공되었는지 추적할 필요가 없습니다. 또는 이 샘플에서와 마찬가지로 가장 최근에 추가된 컨트롤러의 입력만 추적합니다.

-   입력 장치를 처리하기 전에 Windows 이벤트를 처리합니다.

-   Xbox 컨트롤러와 가속도계는 폴링을 지원합니다. 즉, 필요할 때 데이터를 폴링할 수 있습니다. 터치의 경우 입력 처리 코드에서 사용할 수 있는 데이터 구조에 터치 이벤트를 기록합니다.

-   입력 값을 공통 형식으로 정규화할지 여부를 고려합니다. 이렇게 하면 물리학 시뮬레이션 등 게임의 다른 구성 요소에서 입력이 해석되는 방식을 간소화할 수 있으며, 각기 다른 화면 해상도에서 작동하는 게임을 더 쉽게 작성할 수 있습니다.

## <a name="input-devices-supported-by-marble-maze"></a>Marble Maze에서 지원하는 입력 장치


Marble Maze는 Xbox 컨트롤러, 마우스 및 터치를 통한 메뉴 항목 선택과 Xbox 컨트롤러, 마우스, 터치 및 가속도계를 통한 게임 플레이 제어를 지원합니다. Marble Maze는 [Windows::Gaming::Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) API를 사용하여 입력에 대한 컨트롤러를 폴링합니다. 터치를 사용하면 응용 프로그램이 손가락 입력을 추적하고 응답할 수 있습니다. 가속도계는 X, Y, Z 축을 따라 적용되는 힘을 측정하는 센서입니다. Windows 런타임을 사용하면 가속도계 장치의 현재 상태를 폴링하고 Windows 런타임 이벤트 처리 메커니즘을 통해 터치 이벤트를 받을 수 있습니다.

> [!NOTE]
> 이 문서에서는 터치를 사용하여 터치식 및 마우스 입력을 모두 참조하고 포인터를 사용하여 포인터 이벤트를 사용하는 모든 장치를 참조합니다. 터치 및 마우스는 표준 포인터 이벤트를 사용하므로 두 장치를 중 하나를 사용하여 메뉴 항목을 선택하고 게임 플레이를 제어할 수 있습니다.

 

> [!NOTE]
> 패키지 매니페스트는 장치를 회전하여 구슬을 굴릴 때 방향이 변경되지 않도록 게임에서 유일하게 지원되는 회전 방향으로 **가로 방향**을 설정합니다. 패키지 매니페스트를 보려면 Visual Studio에서 **솔루션 탐색기**로 **Package.appxmanifest**를 엽니다.

 

## <a name="initializing-input-devices"></a>입력 장치 초기화


Xbox 컨트롤러는 초기화가 필요하지 않습니다. 터치를 초기화하려면 포인터가 활성화(예: 플레이어가 마우스 단추를 누르거나 화면 터치), 해제 및 이동되는 경우 등의 창 이벤트를 등록해야 합니다. 가속도계를 초기화하려면 응용 프로그램을 초기화할 때 [Windows::Devices::Sensors::Accelerometer](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer) 개체를 만들어야 합니다.

다음 예제에서는 **App::SetWindow** 메서드가 [Windows::UI::Core::CoreWindow::PointerPressed](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerPressed), [Windows::UI::Core::CoreWindow::PointerReleased](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerReleased) 및 [Windows::UI::Core::CoreWindow::PointerMoved](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerMoved) 포인터 이벤트를 등록하는 방법을 보여줍니다. 이러한 이벤트는 응용 프로그램이 초기화되는 동안, 그리고 게임 루프가 시작되기 전에 등록됩니다.

이러한 이벤트는 이벤트 처리기를 호출하는 별도의 스레드에서 처리됩니다.

응용 프로그램을 초기화하는 방법에 대한 자세한 내용은 [Marble Maze 응용 프로그램 구조](marble-maze-application-structure.md)를 참조하세요.

```cpp
window->PointerPressed += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerPressed);

window->PointerReleased += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerReleased);

window->PointerMoved += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerMoved);
```

또한 **MarbleMazeMain** 클래스는 터치 이벤트를 저장할 **std::map** 개체를 만듭니다. 이 Map 개체의 키는 입력 포인터를 고유하게 식별하는 값입니다. 각 키는 모든 터치 지점과 화면 중심 사이의 거리에 매핑됩니다. 나중에 Marble Maze는 이러한 값을 사용하여 미로의 기울기 값을 계산합니다.

```cpp
typedef std::map<int, XMFLOAT2> TouchMap;
TouchMap        m_touches;
```

또한 **MarbleMazeMain** 클래스는 [Accelerometer](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer) 개체를 저장합니다.

```cpp
Windows::Devices::Sensors::Accelerometer^           m_accelerometer;
```

다음 예제와 같이 **Accelerometer** 개체가 **MarbleMazeMain** 생성자에서 초기화됩니다. [Windows::Devices::Sensors::Accelerometer::GetDefault](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer.GetDefault) 메서드는 기본 가속도계의 인스턴스를 반환합니다. 기본 가속도계가 없는 경우, **Accelerometer::GetDefault**는 **nullptr**을 반환합니다.

```cpp
// Returns accelerometer ref if there is one; nullptr otherwise.
m_accelerometer = Windows::Devices::Sensors::Accelerometer::GetDefault();
```

##  <a name="navigating-the-menus"></a>메뉴 탐색

다음과 같이 마우스, 터치 또는 Xbox 컨트롤러를 사용하여 메뉴를 탐색할 수 있습니다.

-   방향 패드를 사용하여 활성 메뉴 항목을 변경합니다.
-   터치, A 단추 또는 메뉴 단추를 사용하여 메뉴 항목을 선택하거나 최고 점수 테이블 등의 현재 메뉴를 닫습니다.
-   메뉴 단추를 사용하여 게임을 일시 중지하거나 다시 시작합니다.
-   마우스로 메뉴 항목을 클릭하여 해당 작업을 선택합니다.

###  <a name="tracking-xbox-controller-input"></a>Xbox 컨트롤러 입력 추적

현재 디바이스에 연결된 게임 패드를 추적하기 위해 **MarbleMazeMain**는 [Windows::Gaming::Input::Gamepad](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad) 개체 모음인 멤버 변수 **m_myGamepads**를 정의합니다. 생성자에서 다음과 같이 초기화됩니다.

```cpp
m_myGamepads = ref new Vector<Gamepad^>();

for (auto gamepad : Gamepad::Gamepads)
{
    m_myGamepads->Append(gamepad);
}
```

또한 **MarbleMazeMain** 생성자는 게임 패드가 추가 또는 제거돌 때 이벤트를 등록합니다.

```cpp
Gamepad::GamepadAdded += 
    ref new EventHandler<Gamepad^>([=](Platform::Object^, Gamepad^ args)
{
    m_myGamepads->Append(args);
    m_currentGamepadNeedsRefresh = true;
});

Gamepad::GamepadRemoved += 
    ref new EventHandler<Gamepad ^>([=](Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;

    if (m_myGamepads->IndexOf(args, &indexRemoved))
    {
        m_myGamepads->RemoveAt(indexRemoved);
        m_currentGamepadNeedsRefresh = true;
    }
});
```

게임 패드를 추가할 때는 **m_myGamepads**에 추가하고, 제거할 때는 **m_myGamepads**에 게임 패드가 있는지 여부를 확인하고 존재할 경우 이를 제거합니다. 두 경우 모두 **m_currentGamepadNeedsRefresh**를 **true**로 설정하여 **m_gamepad** 재할당이 필요하다는 것을 나타냅니다.

마지막으로, **m_gamepad**에 게임 패드를 할당하고 **m_currentGamepadNeedsRefresh**를 **false**로 설정합니다.

```cpp
m_gamepad = GetLastGamepad();
m_currentGamepadNeedsRefresh = false;
```

**업데이트** 메서드에서 **m_gamepad**를 재할당해야 하는지 여부를 확인합니다.

```cpp
if (m_currentGamepadNeedsRefresh)
{
    auto mostRecentGamepad = GetLastGamepad();

    if (m_gamepad != mostRecentGamepad)
    {
        m_gamepad = mostRecentGamepad;
    }

    m_currentGamepadNeedsRefresh = false;
}
```

**m_gamepad** 재할당이 필요하지 않은 경우에는 다음과 같이 정의된 **GetLastGamepad**를 사용하여 가장 최근에 추가된 게임 패드를 할당합니다.

```cpp
Gamepad^ MarbleMaze::MarbleMazeMain::GetLastGamepad()
{
    Gamepad^ gamepad = nullptr;

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(m_myGamepads->Size - 1);
    }

    return gamepad;
}
```

이 방법은 간단하게 **m_myGamepads**에 가장 마지막 게임 패드를 반환합니다.

최대 4개의 Xbox 컨트롤러를 Windows 10 장치에 연결할 수 있습니다. 활성 상태인 컨트롤러를 파악할 필요가 없도록 가장 최근에 추가된 게임 패드만 추적하고 있습니다. 게임이 둘 이상의 플레이어를 지원하는 경우 각 플레이어에 대한 입력을 개별적으로 추적해야 합니다.

**MarbleMazeMain::Update** 메서드는 입력에 대한 게임 패드를 폴링합니다.

```cpp
if (m_gamepad != nullptr)
{
    m_oldReading = m_newReading;
    m_newReading = m_gamepad->GetCurrentReading();
}
```

**m_oldReading**을 통해 마지막 프레임에서 얻은 입력 판독값과 [Gamepad::GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.GetCurrentReading)을 호출하여 얻은 **m_newReading**을 통한 최신 입력 판독값이 추적됩니다. 그리고 게임 패드의 현재 상태에 대한 정보가 포함된 [GamepadReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadreading) 개체가 반환됩니다.

방금 버튼을 눌렀는지 놓았는지 확인하려면 현재 프레임과 마지막 프레임에서의 버튼 판독값을 비교하는  **MarbleMazeMain::ButtonJustPressed** 및 **MarbleMazeMain::ButtonJustReleased**를 정의합니다. 이렇게 하면 단추를 계속 누르고 있을 때가 아니라 처음 누르거나 놓았을 때만 작업을 수행할 수 있습니다.

```cpp
bool MarbleMaze::MarbleMazeMain::ButtonJustPressed(GamepadButtons selection)
{
    bool newSelectionPressed = (selection == (m_newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (m_oldReading.Buttons & selection));
    return newSelectionPressed && !oldSelectionPressed;
}

bool MarbleMaze::MarbleMazeMain::ButtonJustReleased(GamepadButtons selection)
{
    bool newSelectionReleased = 
        (GamepadButtons::None == (m_newReading.Buttons & selection));

    bool oldSelectionReleased = 
        (GamepadButtons::None == (m_oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

[GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons) 판독값은 비트 연산자를 사용해 비교가 됩니다. &mdash;즉, *비트 &* 를 사용하여 버튼을 눌렀는지 여부를 확인합니다. 기존 판독값과 최신 판독값을 비교하여 방금 단추를 누른 것인지 놓은 것인지를 판단합니다.

위의 방법을 사용하여 특정 단추를 눌렀는지 여부를 확인하고 조치가 필요할 경우 해당되는 작업을 수행합니다. 예를 들어 메뉴 단추(**GamepadButtons::Menu**)를 누르면 게임 상태가 활성에서 일시 중지로, 또는 일시 주지에서 활성으로 바뀝니다.

```cpp
if (ButtonJustPressed(GamepadButtons::Menu) || m_pauseKeyPressed)
{
    m_pauseKeyPressed = false;

    if (m_gameState == GameState::InGameActive)
    {
        SetGameState(GameState::InGamePaused);
    }  
    else if (m_gameState == GameState::InGamePaused)
    {
        SetGameState(GameState::InGameActive);
    }
}
```

또한 플레이어가 보기 단추를 누르는지 확인하여 게임을 다시 시작하거나 최고 점수 테이블을 지우는 등 작업이 수행됩니다.

```cpp
if (ButtonJustPressed(GamepadButtons::View) || m_homeKeyPressed)
{
    m_homeKeyPressed = false;

    if (m_gameState == GameState::InGameActive ||
        m_gameState == GameState::InGamePaused ||
        m_gameState == GameState::PreGameCountdown)
    {
        SetGameState(GameState::MainMenu);
        m_inGameStopwatchTimer.SetVisible(false);
        m_preGameCountdownTimer.SetVisible(false);
    }
    else if (m_gameState == GameState::HighScoreDisplay)
    {
        m_highScoreTable.Reset();
    }
}
```

주 메뉴가 활성화된 경우 방향 패드를 위 또는 아래로 누르면 활성 메뉴 항목이 변경됩니다. 사용자가 현재 선택 항목을 선택하면 해당 UI 요소가 선택된 것으로 표시됩니다.

```cpp
// Handle menu navigation.
bool chooseSelection = 
    (ButtonJustPressed(GamepadButtons::A) 
    || ButtonJustPressed(GamepadButtons::Menu));

bool moveUp = ButtonJustPressed(GamepadButtons::DPadUp);
bool moveDown = ButtonJustPressed(GamepadButtons::DPadDown);

switch (m_gameState)
{
case GameState::MainMenu:
    if (chooseSelection)
    {
        m_audio.PlaySoundEffect(MenuSelectedEvent);
        if (m_startGameButton.GetSelected())
        {
            m_startGameButton.SetPressed(true);
        }
        if (m_highScoreButton.GetSelected())
        {
            m_highScoreButton.SetPressed(true);
        }
    }
    if (moveUp || moveDown)
    {
        m_startGameButton.SetSelected(!m_startGameButton.GetSelected());
        m_highScoreButton.SetSelected(!m_startGameButton.GetSelected());
        m_audio.PlaySoundEffect(MenuChangeEvent);
    }
    break;

case GameState::HighScoreDisplay:
    if (chooseSelection || anyPoints)
    {
        SetGameState(GameState::MainMenu);
    }
    break;

case GameState::PostGameResults:
    if (chooseSelection || anyPoints)
    {
        SetGameState(GameState::HighScoreDisplay);
    }
    break;

case GameState::InGamePaused:
    if (m_pausedText.IsPressed())
    {
        m_pausedText.SetPressed(false);
        SetGameState(GameState::InGameActive);
    }
    break;
}
```

### <a name="tracking-touch-and-mouse-input"></a>터치식 및 마우스 입력 추적

터치식 및 마우스 입력의 경우 사용자가 터치하거나 클릭하면 메뉴 항목이 선택됩니다. 다음 예제에서는 **MarbleMazeMain::Update** 메서드가 포인터 입력을 처리하여 메뉴 항목을 선택하는 방법을 보여줍니다. The **m\_pointQueue** member variable tracks the locations where the user touched or clicked on the screen. Marble Maze가 포인터 입력을 수집하는 방법은 이 문서의 뒷부분에 있는 [포인터 입력 처리](#processing-pointer-input) 섹션에서 자세히 설명합니다.

```cpp
// Check whether the user chose a button from the UI. 
bool anyPoints = !m_pointQueue.empty();
while (!m_pointQueue.empty())
{
    UserInterface::GetInstance().HitTest(m_pointQueue.front());
    m_pointQueue.pop();
}
```

**UserInterface::HitTest** 메서드는 제공된 포인트가 UI 요소의 범위 내에 있는지 확인합니다. 이 테스트를 통과하는 UI 요소는 터치된 것으로 표시됩니다. 이 메서드는 **PointInRect** 도우미 함수를 사용하여 제공된 포인트가 각 UI 요소의 범위 내에 있는지 확인합니다.

```cpp
void UserInterface::HitTest(D2D1_POINT_2F point)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if (!(*iter)->IsVisible())
            continue;

        TextButton* textButton = dynamic_cast<TextButton*>(*iter);
        if (textButton != nullptr)
        {
            D2D1_RECT_F bounds = (*iter)->GetBounds();
            textButton->SetPressed(PointInRect(point, bounds));
        }
    }
}
```

### <a name="updating-the-game-state"></a>게임 상태 업데이트

**MarbleMazeMain::Update** 메서드는 컨트롤러 및 터치식 입력을 처리한 후 단추를 누른 경우 게임 상태를 업데이트합니다.

```cpp
// Update the game state if the user chose a menu option. 
if (m_startGameButton.IsPressed())
{
    SetGameState(GameState::PreGameCountdown);
    m_startGameButton.SetPressed(false);
}
if (m_highScoreButton.IsPressed())
{
    SetGameState(GameState::HighScoreDisplay);
    m_highScoreButton.SetPressed(false);
}
```

##  <a name="controlling-game-play"></a>게임 플레이 제어


게임 루프와 **MarbleMazeMain::Update** 메서드는 함께 작동하여 게임 개체의 상태를 업데이트합니다. 게임이 여러 장치에서 입력을 받아들이는 경우 유지 관리하기 쉬운 코드를 작성할 수 있도록 모든 장치의 입력을 하나의 변수 집합에 누적할 수 있습니다. **MarbleMazeMain::Update** 메서드는 모든 장치의 움직임을 누적하는 하나의 변수 집합을 정의합니다.

```cpp
float combinedTiltX = 0.0f;
float combinedTiltY = 0.0f;
```

입력 메커니즘은 입력 장치마다 다를 수 있습니다. 예를 들어 포인터 입력은 Windows 런타임 이벤트 처리 모델을 사용하여 처리됩니다. 반대로, 필요할 때 Xbox 컨트롤러에서 입력 데이터를 폴링합니다. 항상 지정된 장치에 대해 규정된 입력 메커니즘을 따르는 것이 좋습니다. 이 섹션에서는 Marble Maze가 각 장치에서 입력을 읽는 방법, 결합된 입력 값을 업데이트하는 방법, 결합된 입력 값을 사용하여 게임 상태를 업데이트하는 방법 등을 설명합니다.

###  <a name="processing-pointer-input"></a>포인터 입력 처리

포인터 입력 작업을 하는 경우 [Windows::UI::Core::CoreDispatcher::ProcessEvents](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents) 메서드를 호출하여 창 이벤트를 처리합니다. 장면을 업데이트하거나 렌더링하기 전에 게임 루프에서 이 메서드를 호출합니다. Marble Maze는 **App::Run** 메서드에서 이를 호출합니다. 

```cpp
while (!m_windowClosed)
{
    if (m_windowVisible)
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

        m_main->Update();

        if (m_main->Render())
        {
            m_deviceResources->Present();
        }
    }
    else
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
    }
}
```

창이 보이는 경우에는 **ProcessEvents**에 **CoreProcessEventsOption::ProcessAllIfPresent**을 전달하여 대기 중인 모든 이벤트를 처리하고 즉시 반환합니다. 창이 보이지 않는 경우에는 **CoreProcessEventsOption::ProcessOneAndAllPending**를 전달하여 대기 중인 모든 이벤트를 처리하고 다음 새 이벤트를 기다립니다. 이벤트가 처리된 후 Marble Maze는 다음 프레임을 렌더링하고 표시합니다.

Windows 런타임은 발생한 각 이벤트에 대해 등록된 처리기를 호출합니다. **App::SetWindow** 메서드는 이벤트를 등록하고 **MarbleMazeMain** 클래스에 포인터 정보를 전달합니다.

```cpp
void App::OnPointerPressed(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->AddTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}

void App::OnPointerReleased(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->RemoveTouch(args->CurrentPoint->PointerId);
}

void App::OnPointerMoved(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->UpdateTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}
```

**MarbleMazeMain** 클래스는 터치 이벤트를 저장하는 Map 개체를 업데이트하여 포인터 이벤트에 반응합니다. **MarbleMazeMain::AddTouch** 메서드는 포인터를 처음 누를 때(예: 사용자가 터치 사용 장치의 화면을 처음 터치할 때) 호출됩니다. **MarbleMazeMain::UpdateTouch** 메서드는 포인터 위치가 이동할 때 호출됩니다. **MarbleMazeMain::RemoveTouch** 메서드는 포인터를 해제할 때(예: 사용자가 화면 터치를 중지할 때) 호출됩니다.

```cpp
void MarbleMazeMain::AddTouch(int id, Windows::Foundation::Point point)
{
    m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());

    m_pointQueue.push(D2D1::Point2F(point.X, point.Y));
}

void MarbleMazeMain::UpdateTouch(int id, Windows::Foundation::Point point)
{
    if (m_touches.find(id) != m_touches.end())
        m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());
}

void MarbleMazeMain::RemoveTouch(int id)
{
    m_touches.erase(id);
}
```

**PointToTouch** 함수는 원점이 화면 중앙에 오도록 현재 포인터 위치의 좌표를 이동한 다음 약 -1.0에서 +1.0 사이가 되도록 좌표를 조정합니다. 이렇게 하면 여러 입력 방법에서 일관된 방식으로 미로의 기울기를 더 쉽게 계산할 수 있습니다.

```cpp
inline XMFLOAT2 PointToTouch(Windows::Foundation::Point point, Windows::Foundation::Size bounds)
{
    float touchRadius = min(bounds.Width, bounds.Height);
    float dx = (point.X - (bounds.Width / 2.0f)) / touchRadius;
    float dy = ((bounds.Height / 2.0f) - point.Y) / touchRadius;

    return XMFLOAT2(dx, dy);
}
```

**MarbleMazeMain::Update** 메서드는 기울기 계수를 일정한 조정 값만큼 증가시켜 결합된 입력 값을 업데이트합니다. 이 조정 값은 여러 다른 값으로 실험하여 확인되었습니다.

```cpp
// Account for touch input.
for (TouchMap::const_iterator iter = m_touches.cbegin(); 
    iter != m_touches.cend(); 
    ++iter)
{
    combinedTiltX += iter->second.x * m_touchScaleFactor;
    combinedTiltY += iter->second.y * m_touchScaleFactor;
}
```

### <a name="processing-accelerometer-input"></a>가속도계 입력 처리

가속도계 입력을 처리하기 위해 **MarbleMazeMain::Update** 메서드는 [Windows::Devices::Sensors::Accelerometer::GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.devices.sensors.accelerometer.getcurrentreading) 메서드를 호출합니다. 이 메서드는 가속도계 읽기를 나타내는 [Windows::Devices::Sensors::AccelerometerReading](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.AccelerometerReading) 개체를 반환합니다. **Windows::Devices::Sensors::AccelerometerReading::AccelerationX** 및 **Windows::Devices::Sensors::AccelerometerReading::AccelerationY** 속성은 각각 X축과 Y축을 따라 g-force 가속을 유지합니다.

다음 예제에서는 **MarbleMazeMain::Update** 메서드가 가속도계를 폴링하고 결합된 입력 값을 업데이트하는 방법을 보여줍니다. 장치를 기울이면 중력으로 인해 구슬이 더 빨리 이동합니다.

```cpp
// Account for sensors.
if (m_accelerometer != nullptr)
{
    Windows::Devices::Sensors::AccelerometerReading^ reading =
        m_accelerometer->GetCurrentReading();

    if (reading != nullptr)
    {
        combinedTiltX += 
            static_cast<float>(reading->AccelerationX) * m_accelerometerScaleFactor;

        combinedTiltY += 
            static_cast<float>(reading->AccelerationY) * m_accelerometerScaleFactor;
    }
}
```

사용자 컴퓨터에 가속도계가 있는지 확신할 수 없는 경우 가속도계를 폴링하기 전에 항상 유효한 [Accelerometer](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer) 개체가 있는지 확인합니다.

### <a name="processing-xbox-controller-input"></a>Xbox 컨트롤러 입력 처리

**MarbleMazeMain::Update** 메서드에서 **m_newReading**을 사용하여 왼쪽 아날로그 스틱에서 입력을 처리합니다.

```cpp
float leftStickX = static_cast<float>(m_newReading.LeftThumbstickX);
float leftStickY = static_cast<float>(m_newReading.LeftThumbstickY);

auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

if ((oppositeSquared + adjacentSquared) > m_deadzoneSquared)
{
    combinedTiltX += leftStickX * m_controllerScaleFactor;
    combinedTiltY += leftStickY * m_controllerScaleFactor;
}
```

왼쪽된 아날로그 스틱의 입력이 데드존 밖에 있는지 여부를 확인하고, 그럴 경우 **combinedTiltX** 및 **combinedTiltY**에 이를 추가하여(배율 인수 곱하기) 스테이지를 기울입니다.

> [!IMPORTANT]
> Xbox 컨트롤러로 작업하는 경우 항상 데드존을 고려하세요. 데드존은 초기 움직임에 대한 게임 패드 간의 민감도 차이를 가리킵니다. 작은 움직임이 있을 때 읽기가 생성되지 않는 컨트롤러도 있고, 측정 가능한 읽기가 생성되는 컨트롤러도 있습니다. 게임에서 이 차이를 고려하려면 초기 엄지스틱 움직임에 대해 비이동 영역을 만듭니다. 데드존에 대한 자세한 내용은 [섬스틱(thumbstick) 읽기](gamepad-and-vibration.md#reading-the-thumbsticks)를 참조하세요.

 

###  <a name="applying-input-to-the-game-state"></a>게임 상태에 입력 적용

장치는 각기 다른 방법으로 입력 값을 보고합니다. 예를 들어 포인터 입력은 화면 좌표이고 컨트롤러 입력은 완전히 다른 형식일 수 있습니다. 여러 장치의 입력을 하나의 입력 값 집합으로 결합하는 경우의 한 가지 문제는 정규화, 즉 값을 공통 형식으로 변환하는 것입니다. Marble Maze normalizes values by scaling them to the range \[-1.0, 1.0\]. 이 섹션의 앞부분에서 설명한 **PointToTouch** 함수는 약 -1.0에서 +1.0 사이의 정규화된 값으로 화면 좌표를 변환합니다.

> [!TIP]
> 응용 프로그램이 하나의 입력 방법을 사용하는 경우에도 항상 입력 값을 정규화하는 것이 좋습니다. 이렇게 하면 물리학 시뮬레이션 등 게임의 다른 구성 요소에서 입력이 해석되는 방식을 간소화할 수 있으며, 각기 다른 화면 해상도에서 작동하는 게임을 더 쉽게 작성할 수 있습니다.

 

**MarbleMazeMain::Update** 메서드는 입력을 처리한 후, 미로의 기울기가 구슬에 미치는 영향을 나타내는 벡터를 만듭니다. 다음 예제에서는 Marble Maze가 [XMVector3Normalize](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmvector3normalize) 함수를 사용하여 정규화된 중력 벡터를 만드는 방법을 보여 줍니다. **maxTilt** 변수는 미로 기울기 양을 제한하고 미로가 옆으로 서지 않도록 합니다.

```cpp
const float maxTilt = 1.0f / 8.0f;

XMVECTOR gravity = XMVectorSet(
    combinedTiltX * maxTilt, 
    combinedTiltY * maxTilt, 
    1.0f, 
    0.0f);

gravity = XMVector3Normalize(gravity);
```

장면 개체의 업데이트를 완료하기 위해 Marble Maze는 업데이트된 중력 벡터를 물리학 시뮬레이션에 전달하고, 이전 프레임 이후 경과 시간에 대한 물리학 시뮬레이션을 업데이트하고, 구슬의 위치와 방향을 업데이트합니다. 구슬이 미로에서 떨어진 경우 **MarbleMazeMain::Update** 메서드는 구슬이 터치한 마지막 검사점에 다시 구슬을 놓고 물리학 시뮬레이션의 상태를 다시 설정합니다.

```cpp
XMFLOAT3A g;
XMStoreFloat3(&g, gravity);
m_physics.SetGravity(g);

if (m_gameState == GameState::InGameActive)
{
    // Only update physics when gameplay is active.
    m_physics.UpdatePhysicsSimulation(static_cast<float>(m_timer.GetElapsedSeconds()));

    // ...Code omitted for simplicity...

}

// ...Code omitted for simplicity...

// Check whether the marble fell off of the maze. 
const float fadeOutDepth = 0.0f;
const float resetDepth = 80.0f;
if (marblePosition.z >= fadeOutDepth)
{
    m_targetLightStrength = 0.0f;
}
if (marblePosition.z >= resetDepth)
{
    // Reset marble.
    memcpy(&marblePosition, &m_checkpoints[m_currentCheckpoint], sizeof(XMFLOAT3));
    oldMarblePosition = marblePosition;
    m_physics.SetPosition((const XMFLOAT3&)marblePosition);
    m_physics.SetVelocity(XMFLOAT3(0, 0, 0));
    m_lightStrength = 0.0f;
    m_targetLightStrength = 1.0f;

    m_resetCamera = true;
    m_resetMarbleRotation = true;
    m_audio.PlaySoundEffect(FallingEvent);
}
```

이 섹션에서는 물리학 시뮬레이션의 작동 방식에 대해 설명하지 않습니다. 자세한 내용은 Marble Maze 소스의 **Physics.h** 및 **Physics.cpp**를 참조하세요.

## <a name="next-steps"></a>다음 단계


오디오 작업을 할 때 고려할 몇 가지 주요 사항에 대한 자세한 내용은 [Marble Maze 샘플에 오디오 추가](adding-audio-to-the-marble-maze-sample.md)를 참조하세요. 이 문서에서는 Marble Maze가 Microsoft 미디어 파운데이션 및 XAudio2를 사용하여 오디오 리소스를 로드, 믹싱 및 재생하는 방법을 설명합니다.

## <a name="related-topics"></a>관련 항목


* [Adding audio to the Marble Maze sample](adding-audio-to-the-marble-maze-sample.md)
* [Adding visual content to the Marble Maze sample](adding-visual-content-to-the-marble-maze-sample.md)
* [Developing Marble Maze, a UWP game in C++ and DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




